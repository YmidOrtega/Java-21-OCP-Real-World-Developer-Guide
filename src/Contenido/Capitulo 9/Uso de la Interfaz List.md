Ahora que **se tiene familiaridad** con algunos métodos comunes de la interfaz `Collection`, **se pasará** a analizar interfaces específicas. **Se usa** un `List` cuando **se desea** una colección ordenada que pueda contener entradas duplicadas. Por ejemplo, una lista de nombres puede contener duplicados, ya que dos animales pueden tener el mismo nombre. Los elementos **se pueden** recuperar e insertar en posiciones específicas dentro de la lista basándose en un índice de tipo `int`, de manera muy similar a un arreglo (_array_). Sin embargo, a diferencia de un arreglo, muchas implementaciones de `List` pueden cambiar de tamaño después de ser declaradas.

Las listas **se utilizan** comúnmente porque existen muchas situaciones en la programación donde **se necesita** mantener un registro de una lista de objetos. Por ejemplo, **se podría** hacer una lista de lo que **se quiere** ver en el zoológico: primero, ver a los leones, porque se van a dormir temprano; segundo, ver a los pandas, porque hay una fila larga más tarde en el día; y así sucesivamente.

La imagen a continuación muestra cómo **se puede** visualizar un `List`. Cada elemento del `List` tiene un índice, y los índices comienzan en cero.

![[Ejemplo de una lista.png]]

A veces no **importa** el orden de los elementos en una lista. `List` es como el tipo de dato "por defecto" (_go-to_). Cuando **se hace** una lista de compras antes de ir a la tienda, el orden de la lista suele ser el orden en el que **se pensaron** los artículos. Probablemente no **haya un apego** a ese orden en particular, pero conservarlo no perjudica en nada.

Aunque las clases que implementan la interfaz `List` tienen muchos métodos, solo **se necesitan** conocer los más comunes. Convenientemente, estos métodos son los mismos para todas las implementaciones que podrían aparecer en el examen.

Lo principal que todas las implementaciones de `List` tienen en común es que están **ordenadas** y **permiten duplicados**. Más allá de eso, cada una ofrece diferentes funcionalidades. A continuación **se analizan** las implementaciones que **se deben** conocer y los métodos disponibles.

> **Se debe prestar especial atención** a qué nombres son clases y cuáles son interfaces. El examen puede preguntar cuál es la mejor clase o cuál es la mejor interfaz para un escenario.

### Comparación de Implementaciones de List

Revisando la imagen de la seccion anterior ([[Framework de Colecciones de Java.png]]), **se deben** conocer dos clases que implementan la interfaz `List`: `ArrayList` y `LinkedList`. Un `ArrayList` es como un arreglo redimensionable (_resizable array_). Cuando **se añaden** elementos, el `ArrayList` crece automáticamente. Cuando no **se está seguro** de qué colección usar, **se debe usar** un `ArrayList`.

El principal beneficio de un `ArrayList` es que **se puede** buscar cualquier elemento en **tiempo constante**. Añadir o eliminar un elemento es más lento que acceder a él. Esto hace que un `ArrayList` sea una buena opción cuando **se lee** con mayor frecuencia (o en la misma proporción) de lo que **se escribe** en él.

Un `LinkedList` es especial porque implementa tanto `List` como `Deque`. Tiene todos los métodos de un `List`. También posee métodos adicionales para facilitar la adición o eliminación desde el principio y/o el final de la lista.

Los principales beneficios de un `LinkedList` son que **se puede** acceder, añadir y eliminar desde el principio y el final de la lista en **tiempo constante**. La desventaja (_trade-off_) es que lidiar con un índice arbitrario toma un **tiempo lineal**. Esto hace que un `LinkedList` sea una buena elección cuando **se vaya** a utilizar como un `Deque`.

### Creación de un List con un Factory

Cuando **se crea** un `List` de tipo `ArrayList` o `LinkedList`, **se conoce** el tipo. Existen algunos métodos especiales en los que **se obtiene** un `List` de vuelta pero no **se conoce** el tipo subyacente. Estos métodos permiten crear un `List` que incluye datos en una sola línea usando un **método de fábrica** (_factory method_). Esto es conveniente, especialmente al hacer pruebas (_testing_). Algunos de estos métodos devuelven un objeto **inmutable**. Como **se vio** en el Capítulo 6, "Diseño de clases", un objeto inmutable no **se puede** cambiar o modificar. La Tabla 9.1 resume estos tres métodos para crear una lista.

**TABLA 9.1** Métodos factory para crear un `List`

|**Método**|**Descripción**|**¿Puede añadir elementos?**|**¿Puede reemplazar elementos?**|**¿Puede eliminar elementos?**|
|---|---|---|---|---|
|`Arrays.asList(varargs)`|Devuelve una lista de tamaño fijo respaldada por un arreglo (_backed by an array_)|No|Sí|No|
|`List.of(varargs)`|Devuelve una lista inmutable|No|No|No|
|`List.copyOf(collection)`|Devuelve una lista inmutable con una copia de los valores de la colección original|No|No|No|

A continuación, **se observará** un ejemplo de estos tres métodos:

```Java
16: String[] array = new String[] {"a", "b", "c"};
17: List<String> asList = Arrays.asList(array); // [a, b, c]
18: List<String> of = List.of(array); // [a, b, c]
19: List<String> copy = List.copyOf(asList); // [a, b, c]
20:
21: array[0] = "z";
22:
23: System.out.println(asList); // [z, b, c]
24: System.out.println(of); // [a, b, c]
25: System.out.println(copy); // [a, b, c]
26:
27: asList.set(0, "x");
28: System.out.println(Arrays.toString(array)); // [x, b, c]
```

La línea 17 crea un `List` que está respaldado por un arreglo. La línea 21 cambia el arreglo, y la línea 23 refleja ese cambio. Las líneas 27 y 28 muestran la otra dirección, donde cambiar el `List` actualiza el arreglo subyacente. Las líneas 18 y 19 crean, cada una, un `List` inmutable.

Cuando **se ejecutan** de forma independiente, el siguiente código muestra que ambos tipos son inmutables al lanzar una excepción cuando **se intenta** establecer un valor.

```Java
of.set(0, "y"); // UnsupportedOperationException
copy.set(0, "y"); // UnsupportedOperationException
```

De manera similar, cada una de las siguientes líneas lanza una excepción al añadir o eliminar un valor:

```Java
asList.add("z"); // UnsupportedOperationException
of.remove(0); // UnsupportedOperationException
copy.remove(0); // UnsupportedOperationException
```

### Creación de un List con un Constructor

La mayoría de las Colecciones tienen dos constructores que **se necesitan** conocer para el examen. A continuación **se muestran** para `LinkedList`:

```Java
var linked1 = new LinkedList<String>();
var linked2 = new LinkedList<String>(linked1);
```

El primero indica crear un `LinkedList` vacío que contiene todos los valores predeterminados. El segundo le indica a Java que **se desea** hacer una copia de otro `LinkedList`. Es cierto que `linked1` está vacío en este ejemplo, por lo que no es particularmente interesante.

`ArrayList` tiene un constructor adicional que **se debe** conocer. A continuación **se muestran** los tres constructores.

```Java
var list1 = new ArrayList<String>();
var list2 = new ArrayList<String>(list1);
var list3 = new ArrayList<String>(10);
```

Los dos primeros son los constructores comunes que **se necesitan** conocer para todas las Colecciones. El ejemplo final indica crear un `ArrayList` que contenga un número específico de espacios (_slots_), pero nuevamente sin asignar ninguno. **Se puede pensar** en esto como el tamaño del arreglo subyacente.

### Trabajo con Métodos de List

Los métodos en la interfaz `List` son para trabajar con índices. Además de los métodos heredados de `Collection`, también **se deben** conocer los métodos de la Tabla 9.2 para el examen.

**TABLA 9.2** Métodos de `List`

|**Método**|**Descripción**|
|---|---|
|`boolean add(E element)`|Añade el elemento al final (disponible en todas las APIs de `Collection`).|
|`void add(int index, E element)`|Añade el elemento en el índice (_index_) y mueve el resto hacia el final.|
|`E get(int index)`|Devuelve el elemento en el índice.|
|`int indexOf(Object o)`|Devuelve el índice del primer elemento coincidente o `-1` si no **se encuentra**.|
|`int lastIndexOf(Object o)`|Devuelve el índice del último elemento coincidente o `-1` si no **se encuentra**.|
|`E remove(int index)`|Elimina el elemento en el índice y mueve el resto hacia el principio.|
|`default void replaceAll(UnaryOperator<E> op)`|Reemplaza cada elemento en la lista con el resultado del operador.|
|`E set(int index, E e)`|Reemplaza el elemento en el índice y devuelve el original. Lanza `IndexOutOfBoundsException` si el índice es inválido.|
|`default void sort(Comparator<? super E> c)`|Ordena la lista. **Se cubrirá** esto más adelante en el capítulo en la sección "Sorting Data".|

Las siguientes sentencias demuestran la mayoría de estos métodos para trabajar con un `List`:

```Java
3: List<String> list = new ArrayList<>();
4: list.add("SD"); // [SD]
5: list.add(0, "NY"); // [NY,SD]
6: list.set(1, "FL"); // [NY,FL]
7: System.out.println(list.get(0)); // NY
8: list.remove("NY"); // [FL]
9: list.remove(0); // []
10: list.set(0, "?"); // IndexOutOfBoundsException
```

En la línea 3, la lista comienza vacía. La línea 4 añade un elemento al final de la lista. La línea 5 añade un elemento en el índice 0 que desplaza el índice 0 original al índice 1. **Se debe notar** cómo el `ArrayList` es ahora automáticamente más grande en una unidad. La línea 6 reemplaza el elemento en el índice 1 con un nuevo valor.

La línea 7 usa el método `get()` para imprimir el elemento en un índice específico. La línea 8 elimina el elemento que coincide con `NY`. Finalmente, la línea 9 elimina el elemento en el índice 0, y la lista queda vacía nuevamente.

La línea 10 lanza una excepción `IndexOutOfBoundsException` porque no hay elementos en el `List`. Como no hay elementos para reemplazar, ni siquiera el índice 0 está permitido. Si la línea 10 **se moviera** hacia arriba entre las líneas 4 y 5, la llamada tendría éxito.

La salida sería la misma si **se probaran** estos ejemplos con `LinkedList`. Aunque el código sería menos eficiente, no **se notaría** hasta tener listas muy grandes.

Ahora **se observará** el método `replaceAll()`. Éste usa un `UnaryOperator` que toma un parámetro y devuelve un valor del mismo tipo.

```Java
var numbers = Arrays.asList(1, 2, 3);
numbers.replaceAll(x -> x*2);
System.out.println(numbers); // [2, 4, 6]
```

Esta lambda duplica el valor de cada elemento en la lista. El método `replaceAll()` llama a la lambda en cada elemento de la lista y reemplaza el valor en ese índice.

> ### Métodos remove() Sobrecargados
>
Hasta ahora **se han visto** dos métodos `remove()` sobrecargados (_overloaded_). El que proviene de `Collection` elimina un objeto que coincide con el parámetro. Por el contrario, el de `List` elimina un elemento en un índice especificado.
>
Esto se vuelve complicado cuando **se tiene** un tipo `Integer`. ¿Qué **se espera** que imprima lo siguiente?
>```Java
31: var list = new LinkedList<Integer>();
32: list.add(3);
33: list.add(2);
34: list.add(1);
35: list.remove(2);
36: list.remove(Integer.valueOf(2));
37: System.out.println(list);
>```
>La respuesta correcta es `[3]`. **Se observará** cómo **se llegó** allí. Al final de la línea 34, **se tiene** `[3, 2, 1]`. La línea 35 pasa un primitivo, lo que significa que **se está** solicitando la eliminación del elemento en el índice 2. Esto deja la lista como `[3, 2]`. Luego, la línea 36 pasa un objeto `Integer`, lo que significa que **se está** eliminando el valor 2. Eso lleva a `[3]`.
>
El método `remove()` que toma un elemento devolverá `false` si no **se encuentra** el elemento. **Se debe contrastar** esto con el método `remove()` que toma un `int`, el cual lanza una excepción si el elemento no **se encuentra**:
>
>```Java
var list = new LinkedList<Integer>();
list.remove(Integer.valueOf(100)); // Returns false
list.remove(100); // IndexOutOfBoundsException
>```

### Búsqueda en un List

Según la Tabla 9.2, la interfaz `List` incluye dos métodos para buscar elementos, `indexOf()` y `lastIndexOf()`. Funcionan de manera similar a los métodos del mismo nombre en la clase `String`:

```Java
var list = List.of("peacock", "chicken", "peacock", "turkey");
System.out.println(list.indexOf("peacock")); // 0
System.out.println(list.lastIndexOf("peacock")); // 2
System.out.println(list.indexOf("penguin")); // -1
```

Más adelante en este capítulo, **se mostrará** cómo realizar una búsqueda más eficiente ordenando primero la lista y luego utilizando el método `Collections.binarySearch()`.

### Conversión de un List a un Arreglo (Array)

Puesto que un arreglo **se puede** pasar como un `vararg`, la Tabla 9.1 cubrió cómo convertir un arreglo a un `List`. También **se debe** saber cómo hacer lo inverso. **Se comenzará** por convertir un `List` en un arreglo.

```Java
13: List<String> list = new ArrayList<>();
14: list.add("hawk");
15: list.add("robin");
16: Object[] objectArray = list.toArray();
17: String[] stringArray = list.toArray(new String[0]);
18: list.clear();
19: System.out.println(objectArray.length); // 2
20: System.out.println(stringArray.length); // 2
```

La línea 16 muestra que un `List` sabe cómo convertirse a sí mismo en un arreglo. El único problema es que, por defecto, lo hace a un arreglo de la clase `Object`. Esto no suele ser lo que **se desea**. La línea 17 especifica el tipo del arreglo y hace lo que **se requiere**. La ventaja de especificar un tamaño de 0 para el parámetro es que Java creará un nuevo arreglo del tamaño adecuado para el valor de retorno. Si **se prefiere**, **se puede** sugerir un arreglo más grande en su lugar. Si el `List` cabe en ese arreglo, será devuelto. De lo contrario, **se creará** un nuevo arreglo.

Además, **se debe notar** que la línea 18 vacía el `List` original. Esto no afecta a ninguno de los arreglos. El arreglo es un objeto recién creado sin ninguna relación con el `List` original. Es simplemente una copia.

