Ya **se discutió** el "orden" para las clases `TreeSet` y `TreeMap`. Para los números, el orden es obvio: es el orden numérico. Para los objetos `String`, el orden está definido de acuerdo con el mapeo de caracteres Unicode.

> ¿Recuerdas el concepto _7Up_ del Capítulo 4, "APIs Principales" (_Core APIs_)? Cuando **se trabaja** con un `String`, los números se ordenan antes que las letras, y las letras mayúsculas se ordenan antes que las letras minúsculas.

**Se utiliza** `Collections.sort()` en muchos de estos ejemplos. Devuelve `void` porque el parámetro del método es lo que resulta ordenado.

También **se pueden** ordenar objetos creados por uno mismo. Java proporciona una interfaz llamada `Comparable`. Si tu clase implementa `Comparable`, puede ser usada en estructuras de datos que requieren comparación. De hecho, ya **se han visto** muchas clases `Comparable` en este proyecto, incluyendo `String`, `StringBuilder`, `BigDecimal`, `BigInteger` y las clases envolventes primitivas. También hay una clase llamada `Comparator`, que **se utiliza** para especificar que **se desea** usar un orden diferente al que el objeto mismo proporciona de forma natural.

`Comparable` y `Comparator` son lo suficientemente similares como para ser confusos. Al examen le gusta ver si puede engañarte para que confundas los dos. ¡No te confundas! En esta sección, **se discutirá** `Comparable` primero. Luego, a medida que **se avance** con `Comparator`, **se señalarán** todas las diferencias.

### Creación de una Clase Comparable

La interfaz `Comparable` tiene solo un método. De hecho, esta es toda la interfaz:

```java
public interface Comparable<T> {
    int compareTo(T o);
}
```

El tipo genérico `T` permite implementar este método y especificar el tipo de tu objeto. Esto permite evitar un _cast_ (conversión de tipo) al implementar `compareTo()`. Cualquier objeto puede ser `Comparable`. Por ejemplo, **se tiene** un grupo de patos (`Duck`) y **se quieren** ordenar por nombre. Primero, **se crea** un `record` que hereda `Comparable<Duck>`, y luego **se implementa** el método `compareTo()`.

```java
public record Duck(String name) implements Comparable<Duck> {
    public int compareTo(Duck d) {
        return name.compareTo(d.name); // Ordena ascendentemente por nombre
    }
}
```

A continuación, **se pueden** ordenar los patos de la siguiente manera:

```java
11: var ducks = new ArrayList<Duck>();
12: ducks.add(new Duck("Quack"));
13: ducks.add(new Duck("Puddles"));
14: Collections.sort(ducks); // ordena por nombre
15: System.out.print(ducks); // [Duck[name=Puddles], Duck[name=Quack]]
```

Si no **se implementara** la interfaz `Comparable`, todo lo que **se tendría** es un método llamado `compareTo()`, y la línea 14 no compilaría. También **se podría** implementar `Comparable<Object>` o alguna otra clase para `T`, pero esto no sería tan útil para ordenar un grupo de objetos `Duck`.

Finalmente, la clase `Duck` implementa `compareTo()`. Dado que `Duck` está comparando objetos de tipo `String` y la clase `String` ya tiene un método `compareTo()`, simplemente puede delegar la acción.

> Es posible que hayas notado que aquí **se usa** un `record`. Del Capítulo 7, "Más allá de las Clases", un `record` proporciona una gran cantidad de código repetitivo (_boilerplate_) muy útil, como constructores e implementaciones significativas de `toString()`. Solo **se debe recordar** que tanto los _records_ como las clases regulares pueden implementar `Comparable`.

### Diseño de un método compareTo()

En el ejemplo anterior, **se dependió** del método `compareTo()` incorporado en `String`, pero a menudo **se necesita** crear uno propio. Al escribir un método `compareTo()`, la parte más importante es el valor de retorno. Las siguientes reglas deberían aplicarse al tipo de retorno de tu método `compareTo()`:

- Se devuelve el número `0` cuando el objeto actual es **equivalente** al argumento pasado a `compareTo()`.
- Se devuelve un número **negativo** (menor que 0) cuando el objeto actual es **menor** que el argumento pasado a `compareTo()`.
- Se devuelve un número **positivo** (mayor que 0) cuando el objeto actual es **mayor** que el argumento pasado a `compareTo()`.

**Se verá** una implementación de `compareTo()` que compara números en lugar de objetos `String`:

```java
2: public record ZooDuck(int id, String name) implements Comparable<ZooDuck> {
3:     public int compareTo(ZooDuck d) {
4:         return id - d.id; // Ordena ascendentemente por id
5:     }
6: }
```

La línea 4 muestra una forma de comparar dos valores `int`. **Se podría** haber usado `Integer.compare(id, d.id)`, pero **se quería** mostrar cómo crear uno propio. Asegúrese de poder reconocer ambos enfoques. **Se debe recordar** que `id - d.id` ordena en orden ascendente, y `d.id - id` ordena en orden descendente.

**Se probará** este nuevo método en algo de código:

```java
21: var d1 = new ZooDuck (5, "Daffy");
22: var d2 = new ZooDuck(7, "Donald");
23: System.out.println(d1.compareTo(d2)); // -2
24: System.out.println(d1.compareTo(d1)); // 0
25: System.out.println(d2.compareTo(d1)); // 2
```

La línea 23 compara un `id` menor con uno mayor y, por lo tanto, imprime un número negativo. La línea 24 compara animales con el mismo `id` y, por lo tanto, imprime `0`. La línea 25 compara un `id` mayor con uno menor y, por lo tanto, devuelve un número positivo.

### Casteo (Conversión) del Argumento de compareTo()

Al tratar con código heredado (_legacy_) o código que no usa genéricos, el método `compareTo()` requiere un _cast_ ya que se le pasa un `Object`. **Se puede** lograr esto usando la coincidencia de patrones (_pattern-matching_) que **se vio** en el Capítulo 3.

```java
public record LegacyDuck(String name) implements Comparable {
    public int compareTo(Object obj) {
        if(obj instanceof LegacyDuck d)
            return name.compareTo(d.name);
        throw new UnsupportedOperationException("Not a duck");
    }
}
```

Dado que no **se especifica** un tipo genérico para `Comparable`, Java asume que **se va a trabajar** con un `Object`.

### Comprobación de Valores null

Al trabajar con `Comparable` y `Comparator` en este capítulo, **se tiende** a asumir que los datos tienen valores, pero este no es siempre el caso. Al escribir métodos de comparación propios, **se deben** comprobar los datos antes de compararlos si no han sido validados con anticipación.

```java
public record MissingDuck(String name) implements Comparable<MissingDuck> {
    public int compareTo(MissingDuck quack) {
        if (quack == null)
            throw new IllegalArgumentException("Poorly formed duck!");
        
        if (this.name == null && quack.name == null)
            return 0;
        else if (this.name == null) return -1;
        else if (quack.name == null) return 1;
        else return name.compareTo(quack.name);
    }
}
```

Este método lanza una excepción si se le pasa un objeto `MissingDuck` con valor `null`. ¿Qué hay del ordenamiento interno? Si el nombre de un pato es `null`, se posicionará primero (debido a que retorna `-1`).

### Mantener la Consistencia entre compareTo() y equals()

Si escribes una clase que implementa `Comparable`, introduces nueva lógica de negocio para determinar la igualdad. El método `compareTo()` devuelve `0` si dos objetos son iguales, mientras que tu método `equals()` devuelve `true` si dos objetos son iguales. Se dice que un ordenamiento natural que usa `compareTo()` es **consistente con equals** si, y solo si, `x.equals(y)` es `true` siempre que `x.compareTo(y)` sea igual a `0`.

De manera similar, `x.equals(y)` debe ser `false` siempre que `x.compareTo(y)` no sea `0`. Se recomienda encarecidamente hacer que las clases `Comparable` sean consistentes con `equals` porque no todas las clases de colecciones se comportan de manera predecible si los métodos `compareTo()` y `equals()` no son consistentes.

Por ejemplo, la siguiente clase `Product` define un método `compareTo()` que **no es consistente** con `equals`:

```java
public class Product implements Comparable<Product> {
    private int id;
    private String name;
    
    public int hashCode() { return id; }
    
    public boolean equals(Object obj) {
        if (obj instanceof Product other)
            return this.id == other.id;
        return false;
    }
    
    public int compareTo(Product obj) {
        return this.name.compareTo(obj.name);
    }
}
```

Esta clase comprueba la igualdad utilizando el `id`, pero ordena por el `name` (nombre). Asumiendo que los nombres no son únicos, esto significa que **se podrían** tener muchos pares de elementos en los que `compareTo()` devuelva `0`, pero `equals()` devuelva `false`.

Una forma de solucionar esto es actualizar los métodos para que dependan de los mismos atributos. Si aún **se necesita** ordenar los elementos por nombre, **se puede** usar un `Comparator` definido fuera de la clase, como se muestra en la siguiente sección.

### Comparación de Datos con un Comparator

A veces **se desea** ordenar un objeto que no implementó `Comparable`, o **se quiere** ordenar objetos de diferentes maneras en diferentes momentos. Supongamos que **se añade** la propiedad peso (`weight`) a la clase `Duck`.

```java
public record Duck(String name, int weight) implements Comparable<Duck> {
    public int compareTo(Duck d) {
        return name.compareTo(d.name);
    }
    public String toString() { return name; }
}
```

También **se sobrescribe** `toString()` para que la siguiente salida de datos sea más corta. Ahora **se tiene** lo siguiente:

```java
11: Comparator<Duck> byWeight = new Comparator<>() {
12:     public int compare(Duck d1, Duck d2) {
13:         return d1.weight() - d2.weight();
14:     }
15: };
16: var ducks = new ArrayList<Duck>();
17: ducks.add(new Duck("Quack", 7));
18: ducks.add(new Duck("Puddles", 10));
19: Collections.sort(ducks);
20: System.out.println(ducks); // [Puddles, Quack]
21: Collections.sort(ducks, byWeight);
22: System.out.println(ducks); // [Quack, Puddles]
```

La clase `Duck` en sí misma solo puede definir un método `compareTo()`. En este caso, se eligió `name`. Si **se quiere** ordenar por otra característica, **se tiene** que definir ese orden de clasificación fuera del método `compareTo()` usando una clase separada o una expresión lambda.

Las líneas 11–15 muestran cómo definir un `Comparator` usando una clase anónima. En las líneas 19–22, **se ordena** con el `Comparator` interno (natural) de la clase y luego con el `Comparator` externo (`byWeight`) para ver la diferencia en la salida.

`Comparator` es una **interfaz funcional** ya que solo hay un método abstracto que implementar. Esto significa que **se puede** reescribir el `Comparator` de las líneas 11–15 usando una expresión lambda, como se muestra aquí:

```java
Comparator<Duck> byWeight = (d1, d2) -> d1.weight() - d2.weight();
```

Alternativamente, **se puede** usar una referencia a método y un método auxiliar estático para especificar que **se quiere** ordenar por peso.

```java
Comparator<Duck> byWeight = Comparator.comparing(Duck::weight);
```

En este ejemplo, `Comparator.comparing()` es un método estático de la interfaz que crea un `Comparator` dada una expresión lambda o una referencia a método. Conveniente, ¿no?

> ### ¿Es Comparable una Interfaz Funcional?
>
Se ha dicho que `Comparator` es una interfaz funcional porque tiene un único método abstracto. `Comparable` también es una interfaz funcional ya que también tiene un único método abstracto. Sin embargo, usar una lambda para `Comparable` sería tonto. El propósito de `Comparable` es ser implementado **dentro** del objeto que se está comparando.

### Comparación entre Comparable y Comparator

Hay varias diferencias clave entre `Comparable` y `Comparator`. Se han resumido en la Tabla 9.8.

**TABLA 9.8** Comparación entre `Comparable` y `Comparator`

|**Diferencia**|**Comparable**|**Comparator**|
|---|---|---|
|**Nombre del paquete**|`java.lang`|`java.util`|
|**¿La interfaz debe ser implementada por la clase a comparar?**|Sí|No|
|**Nombre del método en la interfaz**|`compareTo()`|`compare()`|
|**Número de parámetros**|1|2|
|**¿Común declararlo usando una lambda?**|No|Sí|

Memoriza esta tabla en serio. El examen intentará engañarte mezclando los dos para ver si puedes notar el error. ¿Ves por qué el siguiente bloque no compila?

```java
var byWeight = new Comparator<Duck>() { // NO COMPILA
    public int compareTo(Duck d1, Duck d2) {
        return d1.getWeight() - d2.getWeight();
    }
};
```

El nombre del método es incorrecto. Un `Comparator` debe implementar un método llamado `compare()`, no `compareTo()`. Presta especial atención a los nombres de los métodos y al número de parámetros cuando veas `Comparator` y `Comparable` en las preguntas.

### Comparación de Múltiples Campos

Al escribir un `Comparator` que compara múltiples variables de instancia, el código se vuelve un poco desordenado. Supongamos que **se tiene** un `record` de `Squirrel` (Ardilla), como se muestra aquí:

```java
public record Squirrel(int weight, String species) {}
```

**Se quiere** escribir un `Comparator` para ordenar por nombre de especie. Si dos ardillas son de la misma especie, **se quiere** ordenar primero la que pesa menos. **Se podría** hacer esto con un código que se vea así:

```java
public class MultiFieldComparator implements Comparator<Squirrel> {
    public int compare(Squirrel s1, Squirrel s2) {
        int result = s1.species().compareTo(s2.species());
        if (result != 0) return result;
        else return s1.weight() - s2.weight();
    } 
}
```

Esto funciona asumiendo que ningún nombre de especie es `null`. Comprueba un campo. Si no coinciden, **se ha terminado** de ordenar. Si coinciden, busca el siguiente campo. Sin embargo, esto no es fácil de leer. También es fácil equivocarse. Cambiar `!=` por `==` rompe la ordenación por completo.

Alternativamente, **se pueden** usar referencias a métodos y construir el `Comparator`. Este código representa la lógica para la misma comparación:

```java
Comparator<Squirrel> c = Comparator.comparing(Squirrel::species)
                                   .thenComparingInt(Squirrel::weight);
```

Esta vez, **se encadenan** los métodos. Primero, **se crea** un `Comparator` ascendente por especie. Luego, si hay un empate, **se ordena** por peso. También **se puede** ordenar en orden descendente. Algunos métodos de `Comparator`, como `thenComparingInt()`, son métodos por defecto (_default methods_).

Supongamos que **se quiere** ordenar en orden descendente por especie:

```java
var c = Comparator.comparing(Squirrel::species).reversed();
```

La Tabla 9.9 muestra los métodos estáticos auxiliares que **se deben** conocer para construir un `Comparator`. Se han omitido los tipos de parámetros para mantener el enfoque en los métodos. Usan muchas de las interfaces funcionales que **se estudiaron** en el capítulo anterior.

**TABLA 9.9** Métodos estáticos auxiliares para construir un `Comparator`

|**Método**|**Descripción**|
|---|---|
|`comparing(function)`|Compara por los resultados de una función que devuelve cualquier `Object` (o un primitivo _autoboxed_ en `Object`).|
|`comparingDouble(function)`|Compara por los resultados de una función que devuelve `double`.|
|`comparingInt(function)`|Compara por los resultados de una función que devuelve `int`.|
|`comparingLong(function)`|Compara por los resultados de una función que devuelve `long`.|
|`naturalOrder()`|Ordena usando el orden especificado por la implementación `Comparable` en el objeto mismo.|
|`reverseOrder()`|Ordena usando el reverso del orden especificado por la implementación `Comparable` en el objeto mismo.|

La Tabla 9.10 muestra los métodos por defecto (_default methods_) que **se pueden** encadenar a un `Comparator` para especificar aún más su comportamiento.

**TABLA 9.10** Métodos auxiliares por defecto para construir un `Comparator`

|**Método**|**Descripción**|
|---|---|
|`reversed()`|Invierte el orden del `Comparator` encadenado.|
|`thenComparing(function)`|Si el `Comparator` anterior devuelve 0, usa esta función comparadora que devuelve un `Object` o puede ser _autoboxed_ en uno. De lo contrario, devuelve el resultado del `Comparator` anterior.|
|`thenComparingDouble(function)`|Igual, pero con una función que devuelve `double`.|
|`thenComparingInt(function)`|Igual, pero con una función que devuelve `int`.|
|`thenComparingLong(function)`|Igual, pero con una función que devuelve `long`.|

> Probablemente ya hayas notado que a menudo **se ignoran** los valores `null` al comprobar la igualdad y comparar objetos. Esto funciona bien para el examen. Sin embargo, en el mundo real, las cosas no son tan ordenadas. Tendrás que decidir cómo manejar de forma segura los valores `null` o evitar que existan en tu objeto.

### Ordenación y Búsqueda (_Sorting and Searching_)

Ahora que has aprendido todo sobre `Comparable` y `Comparator`, finalmente **se puede** hacer algo útil con ellos, como ordenar. El método `Collections.sort()` usa el método `compareTo()` para ordenar. Espera que los objetos a ser ordenados sean de tipo `Comparable`.

```java
2: public class SortRabbits {
3:     static record Rabbit(int id) {}
4:     public static void main(String[] args) {
5:         List<Rabbit> rabbits = new ArrayList<>();
6:         rabbits.add(new Rabbit(3));
7:         rabbits.add(new Rabbit(1));
8:         Collections.sort(rabbits); // NO COMPILA
9:     }
10: }
```

Java sabe que el `record Rabbit` no es `Comparable`. Sabe que la ordenación fallará, por lo que ni siquiera permite que el código compile. **Se puede** arreglar esto pasando un `Comparator` a `sort()`. **Se debe recordar** que un `Comparator` es útil cuando **se quiere** especificar el orden de clasificación sin usar un método `compareTo()`.

```java
8:  Comparator<Rabbit> c = (r1, r2) -> r1.id - r2.id;
9:  Collections.sort(rabbits, c);
10: System.out.println(rabbits); // [Rabbit[id=1], Rabbit[id=3]]
```

Supongamos que **se quiere** ordenar los conejos en orden descendente. **Se podría** cambiar el `Comparator` a `r2.id - r1.id`. Alternativamente, **se podría** invertir el contenido de la lista después de ordenarla:

```java
8:  Comparator<Rabbit> c = (r1, r2) -> r1.id - r2.id;
9:  Collections.sort(rabbits, c);
10: Collections.reverse(rabbits);
11: System.out.println(rabbits); // [Rabbit[id=3], Rabbit[id=1]]
```

Los métodos `sort()` y `binarySearch()` permiten pasar un objeto `Comparator` cuando no **se quiere** usar el orden natural.

> ### Repaso de binarySearch()
>
El método `binarySearch()` requiere que la `List` esté **previamente ordenada**.
>
>```java
11: List<Integer> list = Arrays.asList(6,9,1,8);
12: Collections.sort(list); // [1, 6, 8, 9]
13: System.out.println(Collections.binarySearch(list, 6)); // 1
14: System.out.println(Collections.binarySearch(list, 3)); // -2
>```
>
La línea 12 ordena la lista para que **se pueda** llamar a la búsqueda binaria correctamente. La línea 13 imprime el índice en el que se encuentra una coincidencia (el 6 está en el índice 1). La línea 14 imprime **un número menor que el índice negado** de donde necesitaría ser insertado el valor solicitado. El número 3 necesitaría ser insertado en el índice 1 (después del número 1 pero antes del número 6). Negar eso nos da `-1`, y restar `1` nos da `-2`.

Hay un truco al trabajar con `binarySearch()`. ¿Qué crees que da como salida lo siguiente?

```java
3: var names = Arrays.asList("Fluffy", "Hoppy");
4: Comparator<String> c = Comparator.reverseOrder();
5: var index = Collections.binarySearch(names, "Hoppy", c);
6: System.out.println(index);
```

La respuesta resulta ser `-1`. Antes de que entres en pánico, no **se necesita** saber calcular mentalmente que la respuesta es -1. Sí **se necesita** saber que la respuesta **no está definida**. La línea 3 crea una lista, `[Fluffy, Hoppy]`. Resulta que esta lista está ordenada de manera natural (orden ascendente). La línea 4 crea un `Comparator` que invierte el orden natural. La línea 5 solicita una búsqueda binaria pasando un comparador de orden descendente. Dado que la lista no está ordenada en ese formato descendente, no **se cumple** la precondición para hacer una búsqueda.

Aunque el resultado de llamar a `binarySearch()` en una lista ordenada incorrectamente es indefinido, a veces **se puede** tener suerte. Por ejemplo, la búsqueda comienza en el medio de una lista impar. Si da la casualidad de que **se pide** justo el elemento del medio, el índice devuelto será el que **se espera** (pero no hay que depender de esto).

Anteriormente en el capítulo, **se habló** de colecciones que requieren que las clases implementen `Comparable`. A diferencia de la ordenación de listas, estas colecciones no comprueban que se haya implementado `Comparable` en tiempo de compilación.

Volviendo a nuestro `Rabbit` que no implementa `Comparable`, **se intenta** añadirlo a un `TreeSet`:

```java
2: public class UseTreeSet {
3:     record Rabbit(int id) {}
4:     public static void main(String[] args) {
5:         Set<Duck> ducks = new TreeSet<>();
6:         ducks.add(new Duck("Puddles"));
7: 
8:         Set<Rabbit> rabbits = new TreeSet<>();
9:         rabbits.add(new Rabbit(1)); // ClassCastException
10:    } 
11: }
```

La línea 6 está bien. `Duck` sí implementa `Comparable`. `TreeSet` es capaz de ordenarlo en la posición adecuada dentro del conjunto. La línea 9 es el problema. Cuando `TreeSet` intenta ordenarlo, Java descubre el hecho de que `Rabbit` no implementa `Comparable`. Java lanza una excepción que se ve así en tiempo de ejecución:

`Exception in thread "main" java.lang.ClassCastException: class UseTreeSet$Rabbit cannot be cast to class java.lang.Comparable`

Puede parecer extraño que esta excepción se lance cuando se añade el primer objeto al conjunto. Después de todo, no hay nada con qué compararlo todavía. Java funciona de esta manera por coherencia interna.

Al igual que con la búsqueda y la ordenación de listas, **se puede** indicar a las colecciones que requieren ordenación que **se desea** usar un `Comparator` específico. Por ejemplo:

```java
8: Set<Rabbit> rabbits = new TreeSet<>((r1, r2) -> r1.id - r2.id);
9: rabbits.add(new Rabbit(1));
```

Ahora Java sabe que **se quiere** ordenar por `id`, y todo está bien. Un `Comparator` es un objeto de mucha ayuda. Permite separar el orden de clasificación del objeto mismo que se va a ordenar. Nota que la línea 9 en ambos ejemplos anteriores es idéntica. Es la declaración inicial del `TreeSet` la que ha cambiado.

### Ordenación de una Lista

Aunque **se puede** llamar a `Collections.sort(list)`, también **se puede** invocar el ordenamiento directamente sobre el propio objeto de la lista.

```java
3: List<String> bunnies = new ArrayList<>();
4: bunnies.add("long ear");
5: bunnies.add("floppy");
6: bunnies.add("hoppy");
7: System.out.println(bunnies); // [long ear, floppy, hoppy]
8: bunnies.sort((b1, b2) -> b1.compareTo(b2));
9: System.out.println(bunnies); // [floppy, hoppy, long ear]
```

En la línea 8, **se ordena** la lista alfabéticamente. El método `sort()` toma un `Comparator` que proporciona el orden de clasificación. **Se debe recordar** que `Comparator` (a través de su método interno `compare`) toma dos parámetros y devuelve un `int`. Si **se necesita** un repaso de lo que significa ese valor de retorno de una operación de comparación, revisa la sección de `Comparator` en este capítulo o la sección "Comparación" en el Capítulo 4. ¡Comprenderlo bien es vital para tu estudio!
