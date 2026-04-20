Ahora que se está familiarizado con las reglas para declarar y usar métodos, es momento de observar la creación de métodos con el mismo nombre en la misma clase. La **sobrecarga de métodos** (_method overloading_) ocurre cuando los métodos en la misma clase tienen el **mismo nombre pero diferentes firmas de método**, lo que significa que utilizan diferentes listas de parámetros. (La sobrecarga difiere de la sobrescritura —_overriding_—, que se aprenderá en el Capítulo 6).

Se ha estado mostrando cómo llamar a métodos sobrecargados durante un tiempo. `System.out.println()` y los métodos `append()` de `StringBuilder` proporcionan muchas versiones sobrecargadas, de modo que se les puede pasar casi cualquier cosa sin tener que pensar en ello. En ambos ejemplos, el único cambio fue el tipo del parámetro. La sobrecarga también permite **diferentes cantidades** de parámetros.

**Todo excepto el nombre del método puede variar** para los métodos sobrecargados. Esto significa que puede haber diferentes modificadores de acceso, especificadores opcionales (como `static`), tipos de retorno y listas de excepciones.

Lo siguiente muestra cinco versiones sobrecargadas del método `fly()`:

```Java
public class Falcon {
    public void fly(int numMiles) {}
    public void fly(short numFeet) {}
    public boolean fly() { return false; }
    void fly(int numMiles, short numFeet) {}
    public void fly(short numFeet, int numMiles) throws Exception {}
}
```

Como se puede ver, se puede sobrecargar cambiando cualquier cosa en la lista de parámetros. Se puede tener un tipo diferente, más tipos, o los mismos tipos en un orden diferente. También se debe notar que el tipo de retorno, el modificador de acceso y la lista de excepciones son **irrelevantes** para la sobrecarga. Solo importan el nombre del método y la lista de parámetros.

Ahora se observará un ejemplo que **no** es una sobrecarga válida:

```Java
public class Eagle {
    public void fly(int numMiles) {}
    public int fly(int numMiles) { return 1; } // NO COMPILA
}
```

Este método no compila porque difiere del original solo por el tipo de retorno. Las firmas de los métodos son las mismas, por lo que son métodos duplicados en lo que respecta a Java.

¿Qué pasa con estos? ¿Por qué no compilan?

```Java
public class Hawk {
    public void fly(int numMiles) {}
    public static void fly(int numMiles) {} // NO COMPILA
    public void fly(int numKilometers) {}   // NO COMPILA
}
```

Una vez más, las firmas de estos tres métodos son las mismas (`fly(int)`). No se pueden declarar métodos en la misma clase donde la única diferencia es que uno es un método de instancia y el otro es un método estático. Tampoco se pueden tener dos métodos que tengan listas de parámetros con los mismos tipos de variables y en el mismo orden. Como se mencionó anteriormente, los **nombres** de los parámetros en la lista no importan al determinar la firma del método.

Llamar a métodos sobrecargados es fácil. Simplemente se escribe el código y Java llama al correcto. Por ejemplo, observe estos dos métodos:

```Java
public class Dove {
    public void fly(int numMiles) {
        System.out.println("int");
    }
    public void fly(short numFeet) {
        System.out.println("short");
    }
}
```

La llamada `fly((short) 1)` imprime `short`. Busca tipos coincidentes y llama al método apropiado. Por supuesto, puede ser más complicado que esto. Ahora que se conocen los conceptos básicos de la sobrecarga, se observarán algunos escenarios más complejos que se pueden encontrar en el examen.

### Tipos de referencia

Java elige la versión más específica de un método que pueda encontrar. ¿Qué se cree que emite este código?

```Java
public class Pelican {
    public void fly(String s) {
        System.out.print("string");
    }
    public void fly(Object o) {
        System.out.print("object");
    }
    public static void main(String[] args) {
        var p = new Pelican();
        p.fly("test");
        System.out.print("-");
        p.fly(56);
    }
}
```

La respuesta es `string-object`. La primera llamada pasa un `String` y encuentra una coincidencia directa. No hay razón para usar la versión de `Object` cuando hay una lista de parámetros `String` adecuada esperando ser llamada. La segunda llamada busca una lista de parámetros `int`. Cuando no la encuentra, hace _autoboxing_ a `Integer`. Como aún no encuentra una coincidencia exacta para `Integer`, pasa a la de `Object`.

Se intentará con otro. ¿Qué imprime esto?

```Java
import java.time.*;
import java.util.*;

public class Parrot {
    public static void print(List<Integer> i) {
        System.out.print("I");
    }
    public static void print(CharSequence c) {
        System.out.print("C");
    }
    public static void print(Object o) {
        System.out.print("O");
    }
    public static void main(String[] args){
        print("abc");
        print(Arrays.asList(3));
        print(LocalDate.of(2019, Month.JULY, 4));
    }
}
```

La respuesta es `CIO`. ¡El código está listo para una promoción! La primera llamada a `print()` pasa un `String`. Como se aprendió en el Capítulo 4, `String` y `StringBuilder` implementan la interfaz `CharSequence`. También se aprendió que `Arrays.asList()` se puede usar para crear un objeto `List<Integer>`, lo que explica la segunda salida (`I`). La llamada final a `print()` pasa un `LocalDate`. Es posible que esta no sea una clase conocida por todos, pero está bien. Claramente no es una secuencia de caracteres (`CharSequence`) ni una lista (`List`). Eso significa que se utiliza la firma del método de `Object` (la más general).

### Primitivos

Los primitivos funcionan de manera similar a las variables de referencia. Java intenta encontrar el método sobrecargado coincidente más específico. ¿Qué se cree que sucede aquí?

```Java
public class Ostrich {
    public void fly(int i) {
        System.out.print("int");
    }
    public void fly(long l) {
        System.out.print("long");
    }
    public static void main(String[] args) {
        var p = new Ostrich();
        p.fly(123);
        System.out.print("-");
        p.fly(123L);
    }
}
```

La respuesta es `int-long`. La primera llamada pasa un `int` y ve una coincidencia exacta. La segunda llamada pasa un `long` y también ve una coincidencia exacta. Si se comenta el método sobrecargado con la lista de parámetros `int`, la salida se convierte en `long-long`. Java no tiene problema en llamar a un primitivo más grande (promoción). Sin embargo, no lo hará a menos que no se encuentre una coincidencia mejor.

### _Autoboxing_

Como se vio antes, el _autoboxing_ se aplica a las llamadas a métodos, pero ¿qué pasa si se tiene una versión primitiva y una versión envolvente (`Integer`)?

```Java
public class Kiwi {
    public void fly(int numMiles) {}
    public void fly(Integer numMiles) {}
}
```

Estas sobrecargas de métodos son válidas. Java intenta utilizar la lista de parámetros más específica que pueda encontrar. Esto es cierto tanto para el _autoboxing_ como para otros tipos de coincidencia que se discuten en esta sección.

Esto significa que llamar a `fly(3)` llamará al primer método. Cuando la versión del primitivo `int` no está presente, Java hará _autoboxing_. Sin embargo, cuando se proporciona la versión del primitivo `int`, no hay razón para que Java haga el trabajo extra del _autoboxing_.

### Arreglos (_Arrays_)

A diferencia del ejemplo anterior, este código **no** resulta en _autoboxing_:

```Java
public static void walk(int[] ints) {}
public static void walk(Integer[] integers) {}
```

Los arreglos han existido desde el comienzo de Java. Especifican sus tipos reales (no hay autoboxing directo entre `int[]` e `Integer[]`). ¿Qué pasa con los tipos genéricos, como `List<Integer>`? Ese tema se cubre en el Capítulo 9.

### _Varargs_

¿A qué método se cree que se llama si se pasa un `int[]`?

```Java
public class Toucan {
    public void fly(int[] lengths) {}
    public void fly(int... lengths) {} // NO COMPILA
}
```

¡Pregunta trampa! Recuerde que Java trata los _varargs_ como si fueran un arreglo. Esto significa que **la firma del método es la misma para ambos métodos**. Dado que no está permitido sobrecargar métodos con la misma lista de parámetros, este código no compila. Aunque el código no se ve igual, se compila a la misma lista de parámetros.

Ahora que se ha explicado que los dos métodos son similares (en firma), es hora de mencionar cómo son diferentes en su uso. No debería ser una sorpresa que se pueda llamar a cualquiera de los métodos pasando un arreglo:

```Java
fly(new int[] { 1, 2, 3 }); // Permitido llamar a cualquier método fly()
```

Sin embargo, **solo se puede llamar a la versión _varargs_ con parámetros independientes**:

```Java
fly(1, 2, 3); // Permitido llamar SOLO al método fly() usando varargs
```

Obviamente, esto significa que no compilan exactamente igual en todos los contextos de llamada. Sin embargo, la lista de parámetros (la firma) sí es la misma, y eso es lo que se necesita saber con respecto a la sobrecarga para el examen.

### Juntándolo todo

Hasta ahora, todas las reglas sobre cuándo se llama a un método sobrecargado deberían ser lógicas. Java llama al método **más específico** que puede. Cuando algunos de los tipos interactúan, las reglas de Java se centran en la compatibilidad hacia atrás. Hace mucho tiempo, el _autoboxing_ y los _varargs_ no existían. Dado que el código antiguo aún necesita funcionar, esto significa que el _autoboxing_ y los _varargs_ **vienen al final** cuando Java observa los métodos sobrecargados. ¿Listo para el orden oficial? La Tabla 5.6 lo expone.

**TABLA 5.6** El orden que usa Java para elegir el método sobrecargado correcto

|**Regla**|**Ejemplo de lo que se elegirá para glide(1,2)**|
|---|---|
|**1. Coincidencia exacta por tipo**|`String glide(int i, int j)`|
|**2. Tipo primitivo más grande** (Promoción)|`String glide(long i, long j)`|
|**3. Tipo _Autoboxed_**|`String glide(Integer i, Integer j)`|
|**4. _Varargs_**|`String glide(int... nums)`|

Se hará una prueba práctica utilizando las reglas de la Tabla 5.6. ¿Qué se cree que emite esto?

```Java
public class Glider {
    public static String glide(String s) {
        return "1";
    }
    public static String glide(String... s) {
        return "2";
    }
    public static String glide(Object o) {
        return "3";
    }
    public static String glide(String s, String t) {
        return "4";
    }
    public static void main(String[] args) {
        System.out.print(glide("a"));
        System.out.print(glide("a", "b"));
        System.out.print(glide("a", "b", "c"));
    }
}
```

Imprime `142`. La primera llamada coincide con la firma que toma un solo `String` porque esa es la coincidencia más específica. La segunda llamada coincide con la firma que toma dos parámetros `String` ya que es una coincidencia exacta. No es hasta la tercera llamada que se utiliza la versión de _varargs_, ya que no hay mejores coincidencias.