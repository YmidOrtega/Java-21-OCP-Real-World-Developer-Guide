En este capítulo, **se presentan** las clases e interfaces del **Java Collections Framework** que **se necesitan** conocer para el examen. Las colecciones seguras para hilos (_thread-safe_) **se analizan** en el Capítulo 13, "Concurrency".

Como **se puede recordar** del Capítulo 8, "Lambdas and Functional Interfaces", **se cubrieron** las lambdas, las referencias a métodos y las interfaces funcionales integradas. Muchas de estas **se utilizan** a lo largo de este capítulo.

A continuación, **se detallan** aspectos sobre `Comparator`, `Comparable` y la ordenación (_sorting_). También **se introducen** las nuevas interfaces de colecciones secuenciadas (_sequenced collections_). Finalmente, **se explica** cómo crear clases y métodos propios que usen **tipos genéricos** (_generics_) para que la misma clase **se pueda** utilizar con múltiples tipos.

### Uso de APIs Comunes de Colecciones

Una **colección** es un grupo de objetos contenidos en un único objeto. El **Java Collections Framework** es un conjunto de clases en `java.util` para almacenar colecciones. Existen cuatro interfaces principales en el Java Collections Framework:

- `List`: Una lista es una colección ordenada de elementos que permite entradas duplicadas. A los elementos de una lista **se puede** acceder mediante un índice de tipo `int`.
- `Set`: Un conjunto es una colección que **no permite** entradas duplicadas.
- `Queue`: Una cola es una colección que ordena sus elementos en un orden específico para su procesamiento. Un `Deque` es una subinterfaz de `Queue` que permite el acceso por ambos extremos.
- `Map`: Un mapa es una colección que asocia claves con valores, sin permitir claves duplicadas. Los elementos en un mapa son pares **clave/valor** (_key/value pairs_).

La imagen muestra la interfaz `Collection`, sus subinterfaces y algunas clases que implementan las interfaces que **se deben** conocer para el examen. Las interfaces **se muestran** en rectángulos, con las clases en cajas redondeadas.

**Se debe notar** que `Map` no implementa la interfaz `Collection`. **Se considera** parte del Java Collections Framework aunque técnicamente no sea un `Collection`. Sin embargo, es una colección (note la minúscula), en el sentido de que contiene un grupo de objetos. La razón por la que los mapas **se tratan** de forma diferente es que necesitan métodos distintos al ser pares clave/valor.

![[Framework de Colecciones de Java.png]]

> ¿**Se observa** algo nuevo en la imagen? ¡Las **colecciones secuenciadas** son una novedad absoluta en Java 21! Más adelante en este capítulo **se analizarán** `SequencedSet`, `SequencedCollection` y `SequencedMap`.

En esta sección, **se discuten** los métodos comunes que la **Collections API** proporciona a las clases que la implementan. Muchos de estos métodos son **métodos de conveniencia** (_convenience methods_) que **se podrían** implementar de otras formas, pero facilitan la escritura y lectura del código. Por eso son convenientes.

En esta sección, **se utilizan** `ArrayList` y `HashSet` como clases de implementación, pero **se pueden** aplicar a cualquier clase que herede de la interfaz `Collection`. Las propiedades específicas de cada clase `Collection` **se cubren** en la siguiente sección.

### Comprensión de los Tipos Genéricos (Generics)

En el capítulo anterior, **se mostraron** numerosas interfaces funcionales que usan _generics_. Pero, ¿qué son los _generics_? En Java, los _generics_ son simplemente una forma de referirse a un **tipo parametrizado** (_parameterized type_). Por ejemplo, un `List<Integer>` es una lista de números, mientras que un `Set<String>` es un conjunto de cadenas.

Sin _generics_, **se tendría** que escribir mucho código como el siguiente:

```Java
List numbers = new ArrayList(List.of(1,2,3));
Integer element = (Integer)numbers.get(0); // Se requiere casteo para compilar
numbers.add("Welcome to the zoo!");        // Se permiten tipos no relacionados
```

Con los _generics_ **se puede** mejorar. El siguiente cambio no solo elimina el _casteo_ (_cast_) requerido del código anterior, sino que también ayuda a prevenir que objetos no relacionados **se agreguen** a la colección:

```Java
List<Integer> numbers = new ArrayList<Integer>(List.of(1,2,3));
Integer element = numbers.get(0);          // No se requiere casteo
numbers.add("Welcome to the zoo!");        // NO COMPILA
```

Obtener un error del compilador es bueno. **Se sabrá** de inmediato que algo está mal en lugar de esperar descubrirlo más tarde. Los _generics_ son convenientes porque el código para `List`, `Set` y otras colecciones no cambia según el tipo genérico. Incluso **se puede** usar una clase propia como el tipo, como por ejemplo, `List<Visitor>`.

**Se usarán** _generics_ a lo largo de este capítulo, e incluso **se mostrará** cómo definir clases genéricas propias hacia el final de este capítulo.

### Abreviación del Código con Generics

En la sección anterior, **se vieron** _generics_ que declaran el tipo tanto en el lado izquierdo como en el derecho, de la siguiente manera:

```Java
List<Integer> list = new ArrayList<Integer>();
```

Incluso **se pueden** tener _generics_ que contengan otros _generics_, como este:

```Java
Map<Long,List<Integer>> mapOfLists = new HashMap<Long,List<Integer>>();
```

¡Esa es una gran cantidad de código duplicado para escribir! En esta sección, **se ofrecen** dos formas de acortar este código.

#### Aplicación del Operador Diamante (Diamond Operator)

El **operador diamante** (`<>`) es una notación abreviada que permite omitir el tipo genérico del lado derecho de una declaración cuando el tipo **se puede** inferir. **Se le llama** operador diamante porque `<>` parece un diamante. **Se deben comparar** las declaraciones anteriores con estas nuevas versiones mucho más cortas:

```Java
List<Integer> list = new ArrayList<>();
Map<Long,List<Integer>> mapOfLists = new HashMap<>();
```

Para el compilador, tanto estas declaraciones como las anteriores son equivalentes. **Se debe tener en cuenta** que el operador diamante no **se puede** usar como tipo en la declaración de una variable. Solo **se puede** usar en el lado derecho de una operación de asignación. Por ejemplo, ninguna de las siguientes opciones compila:

```Java
List<> list = new ArrayList<Integer>();// NO COMPILA
class InvalidUse {
void use(List<> data) {}
}// NO COMPILA
```

#### Aplicación de var

Como **se ha visto** anteriormente en este proyecto, también **se puede** usar `var` para acortar expresiones con _generics_.

```Java
var list = new ArrayList<Integer>();
var mapOfLists = new HashMap<Long,List<Integer>>();
```

**Se debe observar** cómo el tipo genérico vuelve a estar en el lado derecho. Eso se debe a que `var` infiere el tipo a partir del lado derecho de la declaración, mientras que el operador diamante lo infiere del lado izquierdo.

El estilo que **se utilice**, `var` u operador diamante, es opcional. ¡Simplemente no **se debe** especificar el tipo genérico en ambos lados del `=`, porque eso es redundante!

> #### Uso de Ambos Abreviadores
> 
¿Qué sucede si **se usan** tanto `var` como el operador diamante?
> 
> ```Java
var map = new HashMap<>();
> ```
> 
> Aunque no lo parezca, ¡esto compila! Si **se intenta** que ambos infieran, no hay suficiente información y **se obtiene** `Object` como el tipo genérico. Esto es equivalente a lo siguiente:
> 
> ```Java
HashMap<Object, Object> map = new HashMap<Object, Object>();
> ```

### Agregando Datos

El método `add()` inserta un nuevo elemento en la `Collection` y devuelve si la operación fue exitosa. La firma del método es la siguiente:

```Java
public boolean add(E element)
```

**Se debe recordar** que el Collections Framework usa _generics_. **Se verá** aparecer la letra `E` con frecuencia. Significa el tipo genérico que **se utilizó** para crear la colección. Para algunos tipos de `Collection`, `add()` siempre devuelve `true`. Para otros tipos, existe una lógica que determina si la llamada a `add()` fue exitosa. A continuación, **se muestra** cómo usar este método:

```Java
3: Collection<String> list = new ArrayList<>();
4: System.out.println(list.add("Sparrow")); // true
5: System.out.println(list.add("Sparrow")); // true
6:
7: Collection<String> set = new HashSet<>();
8: System.out.println(set.add("Sparrow")); // true
9: System.out.println(set.add("Sparrow")); // false
```

Un `List` permite duplicados, haciendo que el valor de retorno sea `true` cada vez. Un `Set` no permite duplicados. En la línea 9, **se intentó** agregar un duplicado por lo que Java devuelve `false` desde el método `add()`.

### Eliminando Datos

El método `remove()` elimina un único valor coincidente en la `Collection` y devuelve si la operación fue exitosa. La firma del método es la siguiente:

```Java
public boolean remove(Object object)
```

Esta vez, el valor de retorno `boolean` indica si **se eliminó** una coincidencia. A continuación, **se muestra** cómo usar este método:

```Java
3: Collection<String> birds = new ArrayList<>();
4: birds.add("hawk"); // [hawk]
5: birds.add("hawk"); // [hawk, hawk]
6: System.out.println(birds.remove("cardinal")); // false
7: System.out.println(birds.remove("hawk")); // true
8: System.out.println(birds); // [hawk]
```

La línea 6 intenta eliminar un elemento que no está en `birds`. Devuelve `false` porque no **se encuentra** dicho elemento. La línea 7 intenta eliminar un elemento que sí está en `birds`, por lo que devuelve `true`. **Se debe notar** que solo elimina una coincidencia.

### Conteo de Elementos

Los métodos `isEmpty()` y `size()` verifican cuántos elementos hay en la `Collection`. Las firmas de los métodos son las siguientes:

```Java
public boolean isEmpty()
public int size()
```

A continuación, **se muestra** cómo usar estos métodos:

```Java
Collection<String> birds = new ArrayList<>();
System.out.println(birds.isEmpty()); // true
System.out.println(birds.size()); // 0
birds.add("hawk"); // [hawk]
birds.add("hawk"); // [hawk, hawk]
System.out.println(birds.isEmpty()); // false
System.out.println(birds.size()); // 2
```

Al principio, `birds` tiene un tamaño de 0 y está vacía. Tiene una capacidad que es mayor que 0. Después de que **se añaden** elementos, el tamaño se vuelve positivo y ya no está vacía.

### Limpieza de la Colección

El método `clear()` proporciona una manera fácil de descartar todos los elementos de la `Collection`. La firma del método es la siguiente:

```Java
public void clear()
```

A continuación, **se muestra** cómo usar este método:

```Java
Collection<String> birds = new ArrayList<>();
birds.add("hawk"); // [hawk]
birds.add("hawk"); // [hawk, hawk]
System.out.println(birds.isEmpty()); // false
System.out.println(birds.size()); // 2
birds.clear(); // []
System.out.println(birds.isEmpty()); // true
System.out.println(birds.size()); // 0
```

Después de llamar a `clear()`, `birds` vuelve a ser un `ArrayList` vacío de tamaño 0.

### Verificación de Contenidos

El método `contains()` verifica si un valor determinado está en la `Collection`. La firma del método es la siguiente:

```Java
public boolean contains(Object object)
```

A continuación, **se muestra** cómo usar este método:

```Java
Collection<String> birds = new ArrayList<>();
birds.add("hawk"); // [hawk]
System.out.println(birds.contains("hawk")); // true
System.out.println(birds.contains("robin")); // false
```

El método `contains()` llama a `equals()` en los elementos del `ArrayList` para ver si hay alguna coincidencia.

### Eliminación con Condiciones

El método `removeIf()` elimina todos los elementos que cumplen con una condición. **Se puede** especificar qué **se debe** eliminar usando un bloque de código o incluso una referencia de método.

La firma del método se ve de la siguiente manera. (En la sección "Trabajando con Generics", más adelante en este capítulo, **se explica** qué significa el `? super`).

```Java
public boolean removeIf(Predicate<? super E> filter)
```

Este método usa un `Predicate`, el cual toma un parámetro y devuelve un `boolean`. A continuación **se analizará** un ejemplo:

```Java
4: Collection<String> list = new ArrayList<>();
5: list.add("Magician");
6: list.add("Assistant");
7: System.out.println(list); // [Magician, Assistant]
8: list.removeIf(s -> s.startsWith("A"));
9: System.out.println(list); // [Magician]
```

La línea 8 muestra cómo eliminar todos los valores `String` que comienzan con la letra A. Esto permite hacer desaparecer al asistente (_Assistant_). **Se probará** un ejemplo con una referencia de método (_method reference_):

```Java
11: Collection<String> set = new HashSet<>();
12: set.add("Wand");
13: set.add("");
14: set.removeIf(String::isEmpty); // s -> s.isEmpty()
15: System.out.println(set); // [Wand]
```

En la línea 14, **se elimina** cualquier objeto `String` vacío del conjunto (`set`). El comentario en esa línea muestra el equivalente lambda de la referencia de método. La línea 15 muestra que el método `removeIf()` eliminó exitosamente un elemento de la lista (`list`).

### Iteración en una Colección

Existe un método `forEach()` que **se puede** llamar sobre una `Collection` en lugar de escribir un bucle. Éste utiliza un `Consumer` que toma un solo parámetro y no devuelve nada. La firma del método es la siguiente:

```Java
public void forEach(Consumer<? super T> action)
```

A los gatos les gusta explorar, así que **se imprimirán** dos de ellos usando tanto referencias a métodos como lambdas:

```Java
Collection<String> cats = List.of("Annie", "Ripley");
cats.forEach(System.out::println);
cats.forEach(c -> System.out.println(c));
```

Los gatos han descubierto cómo imprimir sus nombres. ¡Ahora hay más tiempo disponible para jugar!

> #### Otros Enfoques de Iteración
> 
Existen otras formas de iterar a través de una `Collection`. Por ejemplo, en el Capítulo 3, "Toma de decisiones", **se vio** cómo recorrer una lista usando un bucle `for` mejorado (_enhanced for loop_).
> 
> ```Java
for (String element: coll)
System.out.println(element);
> ```
> Es posible que **se observe** el uso de un enfoque más antiguo.
> ```Java
Iterator<String> iter = coll.iterator();
while(iter.hasNext()) {
String name = iter.next();
System.out.println(name);
}
> ```
> **Se debe prestar atención** a la diferencia entre estas técnicas. El método `hasNext()` verifica si existe un valor siguiente. En otras palabras, indica si `next()` se ejecutará sin lanzar una excepción. El método `next()` en realidad mueve el `Iterator` al siguiente elemento.

### Determinación de Igualdad

Existe una implementación personalizada de `equals()` de forma que **se pueden** comparar dos instancias de `Collection` para verificar el tipo y el contenido. La implementación variará. Por ejemplo, `ArrayList` verifica el orden, mientras que `HashSet` no lo hace.

```Java
boolean equals(Object object)
```

A continuación **se muestra** un ejemplo:

```Java
23: var list1 = List.of(1, 2);
24: var list2 = List.of(2, 1);
25: var set1 = Set.of(1, 2);
26: var set2 = Set.of(2, 1);
27:
28: System.out.println(list1.equals(list2)); // false
29: System.out.println(set1.equals(set2)); // true
30: System.out.println(list1.equals(set1)); // false
```

La línea 28 imprime `false` porque los elementos están en un orden diferente, y a un `List` le importa el orden. Por el contrario, la línea 29 imprime `true` porque un `Set` no es sensible al orden. Finalmente, la línea 30 imprime `false` porque los tipos son diferentes.

> ### Desempaquetado (Unboxing) de nulls
> 
> Java protege contra muchos problemas con Colecciones. Sin embargo, todavía es posible generar un `NullPointerException`.
> ```Java
3: var heights = new ArrayList<Integer>();
4: heights.add(null);
5: int h = heights.get(0); // NullPointerException
> ```
> En la línea 4, **se agrega** un `null` a la lista. Esto es válido porque una referencia `null` **se puede** asignar a cualquier variable de referencia. En la línea 5, **se intenta** desempaquetar (_unbox_) ese `null` a un tipo primitivo `int`. Esto es un problema. Java intenta obtener el valor `int` de `null`. Dado que llamar a cualquier método sobre `null` genera un `NullPointerException`, eso es exactamente lo que **se obtiene**. **Se debe tener cuidado** cuando **se observe** un `null` en relación con el autoempaquetado (_autoboxing_).
