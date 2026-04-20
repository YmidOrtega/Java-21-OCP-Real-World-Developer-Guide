Hasta ahora, se ha hecho referencia a las clases `String` y `StringBuilder` como una "secuencia de caracteres". Esto es cierto. Se implementan utilizando un array (_matriz o array_) de caracteres. Un array es un área de memoria en el _heap_ (montículo) con espacio para un número designado de elementos. Un `String` se implementa como un array con algunos métodos que se podrían desear usar cuando se trata específicamente con caracteres. Un `StringBuilder` se implementa como un array donde el objeto array es reemplazado por un nuevo objeto array más grande cuando se queda sin espacio para almacenar todos los caracteres.

Una gran diferencia es que un array puede ser de cualquier otro tipo de variable en Java. Si por alguna razón no se deseara usar un `String`, se podría usar un array de primitivos `char` directamente:

```Java
char[] letters;
```

Esto no sería muy conveniente porque se perderían todas las propiedades especiales que proporciona `String`, como escribir `"Java"`. Se debe tener en cuenta que `letters` es una variable de referencia y no un primitivo. El tipo `char` es un primitivo. Pero `char` es lo que va dentro del array y no el tipo del array en sí. El array en sí es de tipo `char[]`. Mentalmente, se pueden leer los corchetes (`[]`) como "array de".

En otras palabras, un array es una lista ordenada. Puede contener duplicados. En esta sección, se examina la creación de un array de primitivos y objetos, la ordenación, la búsqueda y los _varargs_ (argumentos variables).

### Creación de un array de primitivos

La imagen a continuación muestra la forma más común de crear un array. Especifica el tipo del array (`int`) y el tamaño (3). Los corchetes indican que se trata de un array.

![[La estructura básica de un array.png]]

Cuando se utiliza esta forma para instanciar un array, todos los elementos se establecen en el **valor predeterminado** para ese tipo. Como se aprendió en el Capítulo 1, el valor predeterminado de un `int` es 0. Dado que `numbers` es una variable de referencia, apunta al objeto array, como se muestra en la siguiente imagen el valor predeterminado para todos los elementos es 0. Además, los índices comienzan en 0 y cuentan hacia arriba, tal como lo hacían para un `String`.

![[Una matriz vacía.png]]

Otra forma de crear un array es especificar todos los elementos con los que debería comenzar.

```Java
int[] moreNumbers = new int[] {42, 55, 99};
```

En este ejemplo, también se crea un array `int` de tamaño 3. Esta vez, se especifican los valores iniciales de esos tres elementos en lugar de usar los predeterminados. La imagen a continuación muestra cómo se ve este array.

![[Una matriz inicializada.png]]

Java reconoce que esta expresión es redundante. Dado que se está especificando el tipo del array en el lado izquierdo del signo igual, Java ya conoce el tipo. Y dado que se están especificando los valores iniciales, ya conoce el tamaño. Como atajo, Java permite escribir esto:

```Java
int[] moreNumbers = {42, 55, 99};
```

Este enfoque se llama un **array anónimo**. Es anónimo porque no se especifica el tipo y el tamaño en el lado derecho.

Finalmente, se pueden escribir los `[]` antes o después del nombre, y añadir un espacio es opcional. Esto significa que estas cinco sentencias hacen exactamente lo mismo:

```Java
int[] numAnimals;
int [] numAnimals2;
int []numAnimals3;
int numAnimals4[];
int numAnimals5 [];
```

La mayoría de las personas usan la primera forma. Sin embargo, se podría ver cualquiera de estas en el examen, así que hay que acostumbrarse a ver los corchetes en lugares extraños.

#### Múltiples "arrays" en declaraciones

¿Qué tipos de variables de referencia se cree que crea el siguiente código?

```Java
int[] ids, types;
```

La respuesta correcta es dos variables de tipo `int[]`. Esto parece bastante lógico. Después de todo, `int a, b;` creaba dos variables `int`. ¿Qué pasa con este ejemplo?

```Java
int ids[], types;
```

Todo lo que se hizo fue mover los corchetes, pero eso cambió el comportamiento. Esta vez se obtiene **una** variable de tipo `int[]` (`ids`) y **una** variable de tipo `int` (`types`).

Java ve esta línea de código y piensa algo como esto: "Quieren dos variables de tipo `int`. La primera se llama `ids[]`. Esta es un `int[]` llamado `ids`. La segunda simplemente se llama `types`. No hay corchetes, así que es un entero regular".

No hace falta decir que no se debería escribir código que luzca así. Sin embargo, es necesario comprenderlo para el examen.

### Creación de un array con variables de referencia

Se puede elegir cualquier tipo de Java para que sea el tipo del array. Esto incluye clases creadas por uno mismo. Se observará un tipo incorporado con `String`:

```Java
String[] bugs = { "cricket", "beetle", "ladybug" };
String[] alias = bugs;
String[] anotherArray = { "cricket", "beetle", "ladybug" };
System.out.println(bugs.equals(alias));        // true
System.out.println(bugs.equals(anotherArray)); // false
System.out.println(bugs.toString());           // [Ljava.lang.String;@160bc7c0
```

Se puede llamar a `equals()` porque un array es un objeto. La primera prueba con `alias` devuelve `true` debido a la igualdad de referencia. ¿Por qué la segunda prueba de igualdad devuelve `false`? El método `equals()` en los arrays **no** observa los elementos del array.

La segunda sentencia de impresión es aún más interesante. ¿Qué diablos es `[Ljava.lang.String;@160bc7c0`? No es necesario saber esto para el examen, pero `[` significa que es un array, `L` indica un tipo de referencia, `java.lang.String` es el tipo, y `160bc7c0` es el código _hash_. Se obtendrán números y letras diferentes cada vez que se ejecute ya que esta es una referencia.

Java proporciona un método que imprime un array de forma agradable:

`Arrays.toString(bugs)` imprimiría `[cricket, beetle, ladybug]`.

Se puede ver el array `bugs` representado en la memoria en la siguiente figura. Hay que asegurarse de comprender esta figura. El array **no** asigna espacio para los objetos `String`. En su lugar, asigna espacio para una **referencia** a donde los objetos están realmente almacenados.

![[Una matriz que apunta a cadenas.png]]

A modo de repaso rápido, ¿a qué se cree que apunta este array?

```Java
public class Names {
    String names[];
}
```

Es una trampa. Fue un repaso del Capítulo 1 y no de la discusión sobre arrays. La respuesta es `null`. El código nunca instanció el array, por lo que es solo una variable de referencia a `null`. Se intentará de nuevo: ¿a qué se cree que apunta este array?

```Java
public class Names {
    String names[] = new String[2];
}
```

Es un array porque tiene corchetes. Es un array de tipo `String` ya que ese es el tipo mencionado en la declaración. Tiene dos elementos porque la longitud es 2. Cada una de esas dos ranuras actualmente es `null` pero tiene el potencial de apuntar a un objeto `String`.

¿Se recuerda la conversión de tipos (_casting_) del capítulo anterior cuando se quería forzar un tipo más grande en un tipo más pequeño? Se puede hacer eso con arrays también:

```Java
3: String[] strings = { "stringValue" };
4: Object[] objects = strings;
5: String[] againStrings = (String[]) objects;
6: againStrings[0] = new StringBuilder(); // NO COMPILA
7: objects[0] = new StringBuilder();      // ¡Cuidado!
```

La línea 3 crea un array de tipo `String`. La línea 4 no requiere un _cast_ porque `Object` es un tipo más amplio que `String`. En la línea 5, se necesita un _cast_ porque se está pasando a un tipo más específico. La línea 6 no compila porque un `String[]` permite solo objetos `String`, y `StringBuilder` no es un `String`.

La línea 7 es donde esto se pone interesante. Desde el punto de vista del compilador, esto está bien. Un objeto `StringBuilder` claramente puede ir en un `Object[]`. El problema es que en realidad no se tiene un `Object[]`. Se tiene un `String[]` referenciado desde una variable `Object[]`. En tiempo de ejecución, el código lanza una `ArrayStoreException`. No es necesario memorizar el nombre de esta excepción, pero sí es necesario saber que esta línea compilará y lanzará una excepción.

### Uso de un array

Ahora que se sabe cómo crear un array, se intentará acceder a uno:

```Java
4: String[] mammals = {"monkey", "chimp", "donkey"};
5: System.out.println(mammals.length); // 3
6: System.out.println(mammals[0]);     // monkey
7: System.out.println(mammals[1]);     // chimp
8: System.out.println(mammals[2]);     // donkey
```

La línea 4 declara e inicializa el array. La línea 5 indica cuántos elementos puede contener el array. El resto del código imprime el array. Note que los elementos se indexan comenzando en 0. Esto debería resultar familiar de `String` y `StringBuilder`, que también comienzan a contar en 0. Esas clases también contaban la longitud como el número de elementos. Note que **no hay paréntesis** después de `length` ya que no es un método (es una propiedad final). ¡Hay que tener cuidado con errores de compilación como el siguiente en el examen!

```Java
4: String[] mammals = {"monkey", "chimp", "donkey"};
5: System.out.println(mammals.length()); // NO COMPILA
```

Para asegurarse de que se comprende cómo funciona `length`, ¿qué se cree que imprime esto?

```Java
4: var birds = new String[6];
5: System.out.println(birds.length);
```

La respuesta es 6. Aunque los seis elementos del array son `null`, todavía hay seis de ellos. El atributo `length` no considera qué hay en el array; considera solo cuántas ranuras se han asignado.

Es muy común usar un bucle al leer o escribir en un array. Este bucle establece cada elemento de `numbers` en un valor cinco veces mayor que el índice actual:

```Java
5: var numbers = new int[10];
6: for (int i = 0; i < numbers.length; i++)
7:     numbers[i] = i + 5;
8: for(int n : numbers)
9:     System.out.println(n);
```

La línea 5 simplemente instancia un array con 10 ranuras. La línea 6 es un bucle `for` que utiliza un patrón extremadamente común. Comienza en el índice 0, que es donde también comienza un array. Sigue avanzando, uno a la vez, hasta que llega al final del array (`< length`). La línea 7 establece el elemento actual de `numbers` en el índice del elemento más 5. Las líneas 8 y 9 imprimen los números en el array, utilizando el bucle `for-each` que se aprendió en el Capítulo 3.

El examen pondrá a prueba si se es observador intentando acceder a elementos que no están en el array. ¿Se puede decir por qué cada uno de estos lanza una `ArrayIndexOutOfBoundsException` para el array de tamaño 10?

```Java
3: var numbers = new int[10];
4: numbers[10] = 3;
5:
6: numbers[numbers.length] = 5;
7:
8: for (int i = 0; i <= numbers.length; i++)
9:     numbers[i] = i + 5;
```

El primero intenta comprobar si se sabe que los índices comienzan en 0. Dado que se tienen 10 elementos en el array, esto significa que solo `numbers[0]` a través de `numbers[9]` son válidos. El segundo ejemplo asume que se es lo suficientemente inteligente como para saber que 10 es inválido y lo disfraza usando la propiedad `length`. Sin embargo, la longitud es siempre uno más que el índice máximo válido. Finalmente, el bucle `for` utiliza incorrectamente `<=` en lugar de `<`, lo cual también es una forma de referirse a ese décimo índice inexistente.

### Ordenación (_Sorting_)

Java facilita la ordenación de un array al proporcionar un método de ordenación (o más bien, un montón de métodos de ordenación). Al igual que `StringBuilder` permitía pasar casi cualquier cosa a `append()`, se puede pasar casi cualquier array a `Arrays.sort()`.

`Arrays` requiere una importación. Para usarlo, se debe tener cualquiera de las siguientes dos sentencias en la clase:

```Java
import java.util.*;      // importa todo el paquete incluyendo Arrays
import java.util.Arrays; // importa solo Arrays
```

Hay una excepción, aunque no aparece a menudo en el examen. Se puede escribir `java.util.Arrays` cada vez que se usa en la clase en lugar de especificarlo como una importación.

Recuerde que si se muestra un fragmento de código, se puede asumir que las importaciones necesarias están allí. Este sencillo ejemplo ordena tres números:

```Java
int[] numbers = { 6, 9, 1 };
Arrays.sort(numbers);
for (int i = 0; i < numbers.length; i++)
    System.out.print(numbers[i] + " ");
```

El resultado es `1 6 9`, como cabría esperar. Note que se iteró a través de la salida para imprimir los valores en el array. Simplemente imprimir la variable del array directamente habría dado el molesto _hash_ de `[I@2bd9c3e7`. Alternativamente, se podría haber impreso `Arrays.toString(numbers)` en lugar de usar el bucle. Eso habría dado como salida `[1, 6, 9]`.

Pruebe esto de nuevo con tipos `String`:

```Java
String[] strings = { "10", "9", "100" };
Arrays.sort(strings);
for (String s : strings)
    System.out.print(s + " ");
```

Esta vez el resultado podría no ser el esperado. Este código emite `10 100 9`. El problema es que `String` se ordena en **orden alfabético**, y `1` se ordena antes que `9`. (Los números se ordenan antes que las letras, y las mayúsculas se ordenan antes que las minúsculas). En el Capítulo 9, «Colecciones y genéricos», se aprenderá cómo crear órdenes de clasificación personalizados utilizando algo llamado comparador (_comparator_).

Se puede usar la regla mnemotecnia de **7Up**, el refresco, para ayudar a recordar el orden de los caracteres. Los números (`7`) se ordenan primero, seguidos de las mayúsculas (`U`) y luego las minúsculas (`p`).

### Búsqueda

Java también proporciona una forma conveniente de buscar, pero solo si el array **ya está ordenado**. La Tabla 4.3 cubre las reglas para la búsqueda binaria (_binary search_).

**TABLA 4.3** Reglas de búsqueda binaria

|**Escenario**|**Resultado**|
|---|---|
|Elemento objetivo encontrado en un array ordenado|Índice de la coincidencia|
|Elemento objetivo no encontrado en un array ordenado|Valor negativo que muestra uno menos que el negativo del índice donde debería insertarse una coincidencia para preservar el orden ordenado|
|array no ordenado|Una sorpresa; este resultado es indefinido|

Se probarán estas reglas con un ejemplo:

```Java
3: int[] numbers = {2,4,6,8};
4: System.out.println(Arrays.binarySearch(numbers, 2)); // 0
5: System.out.println(Arrays.binarySearch(numbers, 4)); // 1
6: System.out.println(Arrays.binarySearch(numbers, 1)); // -1
7: System.out.println(Arrays.binarySearch(numbers, 3)); // -2
8: System.out.println(Arrays.binarySearch(numbers, 9)); // -5
```

Tome nota del hecho de que la línea 3 es un array **ordenado**. Si no lo fuera, no se podrían aplicar las otras reglas. La línea 4 busca el índice de `2`. La respuesta es el índice 0. La línea 5 busca el índice de `4`, que es 1.

La línea 6 busca el índice de `1`. Aunque `1` no está en la lista, la búsqueda puede determinar que debería insertarse en el elemento 0 para preservar el orden ordenado. Dado que 0 ya significa algo para los índices de array (la primera posición), Java necesita restar 1 para dar la respuesta de `-1`. La línea 7 es similar. Aunque `3` no está en la lista, tendría que insertarse en el elemento 1 para preservar el orden ordenado. Se niega y se resta 1 por consistencia, obteniendo `-1 - 1`, también conocido como `-2`. Finalmente, la línea 8 quiere indicar que `9` debería insertarse en el índice 4. De nuevo se niega y se resta 1, obteniendo `-4 - 1`, también conocido como `-5`.

¿Qué se cree que sucede en este ejemplo?

```Java
5: int[] numbers = new int[] {3,2,1};
6: System.out.println(Arrays.binarySearch(numbers, 2));
7: System.out.println(Arrays.binarySearch(numbers, 3));
```

Note que en la línea 5, el array **no** está ordenado en orden ascendente (está invertido). Esto significa que la salida **no estará definida**. Al probar este ejemplo, la línea 6 dio correctamente 1 como salida. Sin embargo, la línea 7 dio una respuesta incorrecta. Los creadores del examen no esperarán que se sepa qué valores incorrectos salen. Tan pronto como se vea que el array no está ordenado, se debe buscar una opción de respuesta sobre un resultado impredecible.

En el examen, es necesario saber qué devuelve una búsqueda binaria en varios escenarios. Curiosamente, no es necesario saber por qué la palabra "binaria" está en el nombre. Por si hay curiosidad, una búsqueda binaria divide el array en dos partes iguales (Recuerde, 2 es binario) y determina en qué mitad está el objetivo. Repite este proceso hasta que solo queda un elemento.

### Comparación

Java también proporciona métodos para comparar dos arrays para determinar cuál es "menor". Primero se cubren los métodos `equals()` y `compare()`, y luego se pasa a `mismatch()`. Estos métodos están sobrecargados para aceptar una variedad de parámetros.

#### Uso de `equals()`

Mientras que `==` compara referencias de objetos, `Arrays` incluye versiones sobrecargadas de `equals()` que permiten verificar si los arrays tienen el mismo tamaño y contienen los mismos elementos, en el mismo orden. Por ejemplo:

```Java
System.out.println(new int[] {1} == new int[] {1}); // false
System.out.println(Arrays.equals(new int[] {1}, new int[] {1})); // true
System.out.println(Arrays.equals(new int[] {1}, new int[] {2})); // false
System.out.println(Arrays.equals(new int[] {1}, new int[] {1, 2})); // false
```

Al comparar elementos, utiliza `==` para valores primitivos y `equals()` para valores de objetos.

#### Uso de `compare()`

Hay un montón de reglas que se necesitan conocer antes de llamar a `compare()`. Afortunadamente, estas son las mismas reglas que se necesitan conocer en el Capítulo 9 al escribir un `Comparator`.

Primero se necesita aprender qué significa el valor de retorno. No es necesario conocer los valores de retorno exactos, pero sí es necesario saber lo siguiente:

- Un número **negativo** significa que el primer array es menor que el segundo.
- Un **cero** significa que los arrays son iguales.
- Un número **positivo** significa que el primer array es mayor que el segundo.

Aquí hay un ejemplo:

```Java
System.out.println(Arrays.compare(new int[] {1}, new int[] {2}));
```

Este código imprime un número negativo. Debería ser bastante intuitivo que 1 es menor que 2, lo que hace que el primer array sea menor.

Ahora que se sabe cómo comparar un solo valor, se observará cómo comparar arrays de diferentes longitudes:

1. Si ambos arrays tienen la **misma longitud y tienen los mismos valores** en cada lugar en el mismo orden, se devuelve **cero**.
2. Si todos los elementos son iguales pero el **segundo array tiene elementos adicionales** al final, se devuelve un número **negativo** (el primero es más corto/menor).
3. Si todos los elementos son iguales, pero el **primer array tiene elementos adicionales** al final, se devuelve un número **positivo**.
4. Si el **primer elemento que difiere es menor en el primer array**, se devuelve un número **negativo**.
5. Si el **primer elemento que difiere es mayor en el primer array**, se devuelve un número **positivo**.

Finalmente, ¿qué significa menor? Aquí hay algunas reglas más que se aplican aquí y a `compareTo()`, que se ve en el Capítulo 8, «Lambdas e Interfaces Funcionales»:

- `null` es menor que cualquier otro valor.
- Para números, se aplica el orden numérico normal.
- Para cadenas, una es menor si es un prefijo de la otra (es más corta).
- Para cadenas/caracteres, los números son menores que las letras.
- Para cadenas/caracteres, las mayúsculas son menores que las minúsculas.

La Tabla 4.4 muestra ejemplos de estas reglas en acción.

**TABLA 4.4** Ejemplos de `Arrays.compare()`

| **Primer array**     | **Segundo array**     | **Resultado**   | **Razón**                                                          |
| -------------------- | --------------------- | --------------- | ------------------------------------------------------------------ |
| `new int[] {1, 2}`   | `new int[] {1}`       | Número positivo | El primer elemento es el mismo, pero el primer array es más largo. |
| `new int[] {1, 2}`   | `new int[] {1, 2}`    | Cero            | Coincidencia exacta.                                               |
| `new String[] {"a"}` | `new String[] {"aa"}` | Número negativo | El primer elemento es una subcadena del segundo.                   |
| `new String[] {"a"}` | `new String[] {"A"}`  | Número positivo | Las mayúsculas son menores que las minúsculas.                     |
| `new String[] {"a"}` | `new String[] {null}` | Número positivo | `null` es menor que una letra.                                     |

Finalmente, este código no compila porque los tipos son diferentes. Al comparar dos arrays, deben ser del mismo tipo de array.

```Java
System.out.println(Arrays.compare(new int[] {1}, new String[] {"a"})); // NO COMPILA
```

#### Uso de `mismatch()`

Ahora que se está familiarizado con `compare()`, es hora de aprender sobre `mismatch()`. Si los arrays son iguales, `mismatch()` devuelve `-1`. De lo contrario, devuelve el **primer índice donde difieren**. ¿Se puede deducir qué imprimen estos?

```Java
System.out.println(Arrays.mismatch(new int[] {1}, new int[] {1}));
System.out.println(Arrays.mismatch(new String[] {"a"}, new String[] {"A"}));
System.out.println(Arrays.mismatch(new int[] {1, 2}, new int[] {1}));
```

En el primer ejemplo, los arrays son iguales, por lo que el resultado es `-1`. En el segundo ejemplo, las entradas en el elemento 0 no son iguales, por lo que el resultado es `0`. En el tercer ejemplo, las entradas en el elemento 0 son iguales, por lo que se sigue buscando. El elemento en el índice 1 no es igual. O, más específicamente, un array tiene un elemento en el índice 1 y el otro no. Por lo tanto, el resultado es `1`.

Para asegurarse de que se comprenden los métodos `compare()` y `mismatch()`, se debe estudiar la Tabla 4.5. Si no se comprende por qué están allí todos los valores, se ruega volver atrás y estudiar esta sección de nuevo.

**TABLA 4.5** Igualdad vs. comparación vs. desajuste

|**Método**|**Cuando los arrays contienen los mismos datos**|**Cuando los arrays son diferentes**|
|---|---|---|
|`Arrays.equals()`|`true`|`false`|
|`Arrays.compare()`|`0`|Número positivo o negativo|
|`Arrays.mismatch()`|`-1`|Índice cero o positivo|
### Uso de métodos con `varargs`

Cuando se crea un array por cuenta propia, tiene el aspecto que se ha visto hasta ahora. Sin embargo, cuando se pasa un array a un método, puede tener otro aspecto. Aquí hay tres ejemplos con un método `main()`:

```Java
public static void main(String[] args)
public static void main(String args[])
public static void main(String... args) // varargs
```

El tercer ejemplo utiliza una sintaxis llamada `varargs` (argumentos variables), que se vio en el Capítulo 1. Se aprenderá cómo llamar a un método usando `varargs` en el Capítulo 5, «Métodos». Por ahora, todo lo que se necesita saber es que se puede utilizar una variable definida usando `varargs` **como si fuera un array normal**. Por ejemplo, `args.length` y `args[0]` son legales.

### Trabajo con arrays de arrays

Los arrays son objetos y, por supuesto, los componentes de los arrays pueden ser objetos. No toma mucho tiempo, al juntar esos dos hechos, preguntarse si los arrays pueden contener otros arrays, y por supuesto, pueden.

#### Creación de un array de arrays

Múltiples separadores de arrays son todo lo que se necesita para declarar arrays de arrays. Si bien no son realmente multidimensionales, ayuda pensar en ellos como tales. Se pueden identificar por el tipo o el nombre de la variable en la declaración, tal como antes:

```Java
int[][] vars1;              // array 2D
int vars2 [][];             // array 2D
int[] vars3[];              // array 2D
int[] vars4 [], space [][]; // arrays 2D y 3D
```

Los dos primeros ejemplos no son sorprendentes y declaran un array bidimensional (2D). El tercer ejemplo también declara un array 2D. No hay una buena razón para usar este estilo más que para confundir a los lectores con el código. El ejemplo final declara dos arrays en la misma línea. Sumando los corchetes, se observa que `vars4` es un array 2D y `space` es un array 3D. Nuevamente, no hay razón para usar este estilo más que para confundir a los lectores del código. Sin embargo, a los creadores del examen les gusta intentar confundir. Afortunadamente, ¡se está al tanto y no se permitirá que esto suceda!

Se puede especificar el tamaño del array y el array que contiene en la declaración si se desea:

```Java
String [][] rectangle = new String[3][2];
```

El resultado de esta sentencia es un array `rectangle` con tres elementos, cada uno de los cuales se refiere a un array de dos elementos. Se puede pensar en el rango direccionable como `[0][0]` hasta `[2][1]`, pero no se debe pensar en ello como una estructura de direcciones como `[0,0]` o `[2,1]`.

Ahora suponga que se establece uno de estos valores:

```Java
rectangle[0][1] = "set";
```

Se puede visualizar el resultado como se muestra a continuación. Este array está escasamente poblado porque tiene muchos valores `null`. Se puede ver que `rectangle` todavía apunta a un array de tres elementos y que se tienen tres arrays de dos elementos. También se puede seguir el rastro desde la referencia hasta el único valor que apunta a un `String`. Se comienza en el índice 0 en el array superior. Luego se va al índice 1 en el siguiente array.

![[Una array de arrays con poca densidad.png]]

Aunque ese array resulta ser de forma rectangular, un array no necesita serlo. Considere este:

```Java
int[][] differentSizes = {{1, 4}, {3}, {9,8,7}};
```

Todavía se comienza con un array de tres elementos. Sin embargo, esta vez los elementos en el siguiente nivel son todos de diferentes tamaños. Uno es de longitud 2, el siguiente de longitud 1 y el último de longitud 3. Vea la imagen siguiente. Esta vez el array es de primitivos, por lo que se muestran como si estuvieran en el propio array.

![[Una array asimétrica de arrays.png]]

Otra forma de crear un array asimétrico es inicializar solo la primera dimensión de un array y definir el tamaño de cada componente del array en una sentencia separada.

```Java
int [][] args = new int[2][];
args[0] = new int[5];
args[1] = new int[3];
```

Esta técnica revela lo que realmente se obtiene con Java: **arrays de arrays** que, manejados adecuadamente, podrían parecerse a una matriz.

#### Uso de un array de arrays

La operación más común en un array de arrays es iterar a través de él. Este ejemplo imprime un array 2D:

```Java
var twoD = new int[3][2];
for(int i = 0; i < twoD.length; i++) {
    for(int j = 0; j < twoD[i].length; j++)
        System.out.print(twoD[i][j] + " "); // imprime el elemento
    System.out.println();                   // tiempo de una nueva fila
}
```

Se tienen dos bucles aquí. El primero utiliza el índice `i` y recorre el array de nivel superior para `twoD`. El segundo utiliza una variable de bucle diferente, `j`. Es importante que estos sean nombres de variables diferentes para que los bucles no se mezclen. El bucle interno observa cuántos elementos hay en el array de segundo nivel (`twoD[i].length`). El bucle interno imprime el elemento y deja un espacio para facilitar la lectura. Cuando el bucle interno se completa, el bucle externo va a una nueva línea y repite el proceso para el siguiente elemento.

Todo este ejercicio sería más fácil de leer con el bucle `for` mejorado (_for-each_).

```Java
for(int[] inner : twoD) {
    for(int num : inner)
        System.out.print(num + " ");
    System.out.println();
}
```

Se admitirá que no son menos líneas, pero cada línea es menos compleja y no hay variables de bucle o condiciones de terminación que confundir.