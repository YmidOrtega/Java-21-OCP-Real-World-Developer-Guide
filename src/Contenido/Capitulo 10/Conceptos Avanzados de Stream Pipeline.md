¡Felicitaciones, solo quedan unos pocos temas más! En esta última sección de streams, **se aprende** sobre la relación entre los streams y los datos subyacentes, el encadenamiento de `Optional`, los recopiladores de agrupación y el uso de `Spliterator`. Después de esto, ¡**se debería** ser un profesional con los streams!

#### Vinculando Streams a los Datos Subyacentes

¿Qué **se cree** que produce esto?

```Java
25: var cats = new ArrayList<String>();
26: cats.add("Annie");
27: cats.add("Ripley");
28: var stream = cats.stream();
29: cats.add("KC");
30: System.out.println(stream.count());
```

La respuesta correcta es 3. Las líneas 25–27 **crean** una `List` con dos elementos. La línea 28 **solicita** que **se cree** un stream a partir de esa `List`. **Hay que recordar** que los streams **se evalúan** perezosamente. Esto significa que el stream no **se crea** en la línea 28. **Se crea** un objeto que sabe dónde buscar los datos cuando **se necesiten**. En la línea 29, la `List` **obtiene** un nuevo elemento. En la línea 30, el stream pipeline **ve** tres elementos cuando se ejecuta dando ese conteo.

#### Encadenando *Optional*s

A estas alturas, **se está** familiarizado con los beneficios de **encadenar** operaciones en un stream pipeline. Algunas de las operaciones intermedias para streams están disponibles para `Optional`, como **se muestra** en la Tabla 10.9.

**TABLA 10.9** Métodos avanzados de instancia de `Optional`

| **Método** | **Cuando Optional está vacío** | **Cuando Optional contiene valor** |
|---|---|---|
| `filter(Predicate p)` | Devuelve `Optional` vacío | Devuelve `Optional` que contiene el elemento si coincide con el `Predicate`, de lo contrario `Optional` vacío |
| `flatMap(Function f)` | Devuelve `Optional` vacío | Devuelve `Optional` con `Function` aplicada al elemento. El tipo de retorno de `Function` debe heredar `Optional`. |
| `map(Function f)` | Devuelve `Optional` vacío | Devuelve `Optional` con `Function` aplicada al elemento |

Supongamos que **se da** un `Optional<Integer>` y **se pide** imprimir el valor, pero solo si es un número de tres dígitos. Sin programación funcional, **se podría** escribir lo siguiente:

```Java
private static void threeDigit(Optional<Integer> optional) {
    if (optional.isPresent()) { // if externo
        var num = optional.get();
        var string = "" + num;
        if (string.length() == 3) // if interno
            System.out.println(string);
    }
}
```

Esto funciona, pero contiene sentencias `if` anidadas. Es una complejidad extra. **Se intenta** esto nuevamente con programación funcional:

```Java
private static void threeDigit(Optional<Integer> optional) {
    optional.map(n -> "" + n)         // parte 1
        .filter(s -> s.length() == 3) // parte 2
        .ifPresent(System.out::println); // parte 3
}
```

Esto es mucho más corto y expresivo. Con lambdas, el examen es aficionado a **dividir** una única sentencia e **identificar** las piezas con un comentario. Aquí **se ha hecho** esto para mostrar qué sucede tanto con el enfoque de programación funcional como con el no funcional.

Supongamos que **se da** un `Optional` vacío. El primer enfoque devuelve `false` para la sentencia `if` externo. El segundo enfoque ve un `Optional` vacío y hace que tanto `map()` como `filter()` lo pasen tal cual. Luego `ifPresent()` ve un `Optional` vacío y no llama al parámetro `Consumer`.

El siguiente caso es donde **se da** un `Optional.of(4)`. El primer enfoque devuelve `false` para la sentencia `if` interno. El segundo enfoque **mapea** el número 4 a "4". El `filter()` luego **devuelve** un `Optional` vacío ya que el filtro no coincide, y `ifPresent()` no llama al parámetro `Consumer`.

El caso final es donde **se da** un `Optional.of(123)`. El primer enfoque devuelve `true` para ambas sentencias `if`. El segundo enfoque **mapea** el número 123 a "123". El `filter()` luego **devuelve** el mismo `Optional`, y `ifPresent()` ahora sí llama al parámetro `Consumer`.

Ahora supongamos que **se quisiera** obtener un `Optional<Integer>` que represente la longitud del `String` contenido en otro `Optional`. Es suficientemente fácil:

```Java
Optional<Integer> result = optional.map(String::length);
```

¿Qué pasa si en cambio **se tiene** un método auxiliar que toma un `String` y hace la lógica de calcular algo? Devolvería `Optional<Integer>`, como este:

```Java
public static Optional<Integer> calculator(String text) {
    // lógica de cálculo aquí
}
```

Usar `map` para llamarlo no funciona:

```Java
Optional<Integer> result = optional
    .map(ChainingOptionals::calculator); // NO COMPILA
```

El problema es que `calculator` devuelve `Optional<Integer>`. El método `map()` **agrega** otro `Optional`, dándonos `Optional<Optional<Integer>>`. Eso no es bueno. La solución es llamar a `flatMap()` en su lugar:

```Java
Optional<Integer> result = optional
    .flatMap(ChainingOptionals::calculator);
```

Este funciona porque `flatMap` **elimina** la capa innecesaria. En otras palabras, **aplana** el resultado. **Encadenar** llamadas a `flatMap()` es útil cuando **se quiere** transformar un tipo de `Optional` a otro.

> **Escenario del Mundo Real: Excepciones Verificadas e Interfaces Funcionales**
>
> **Se podría** haber notado a estas alturas que la mayoría de las interfaces funcionales no declaran excepciones verificadas. Esto es normalmente correcto. Sin embargo, es un problema cuando **se trabaja** con métodos que declaran excepciones verificadas. Supongamos que **se tiene** una clase con un método que lanza una `IOException`.
>
> ```Java
> import java.io.*;
> import java.util.*;
> public class ExceptionCaseStudy {
>     private static List<String> create() throws IOException {
>         throw new IOException();
>     }
> }
> ```
>
> Ahora **se usa** en un stream.
>
> ```Java
> public void good() throws IOException {
>     ExceptionCaseStudy.create().stream().count();
> }
> ```
>
> Nada nuevo aquí. El método `create()` lanza una excepción verificada. El método que llama la maneja o la declara. Ahora, ¿qué pasa con este?
>
> ```Java
> public void bad() throws IOException {
>     Supplier<List<String>> s = ExceptionCaseStudy::create; // NO COMPILA
> }
> ```
>
> El error real del compilador es el siguiente:
>
> ```Plaintext
> unhandled exception type IOException
> ```
>
> El problema es que la interfaz funcional a la que esta referencia de método **se expande** no declara una excepción. La interfaz `Supplier` no permite excepciones verificadas. Hay dos enfoques para **resolver** este problema. Uno es **capturar** la excepción y **convertirla** en una excepción no verificada.
>
> ```Java
> public void ugly() {
>     Supplier<List<String>> s = () -> {
>         try {
>             return ExceptionCaseStudy.create();
>         } catch (IOException e) {
>             throw new RuntimeException(e);
>         }
>     };
> }
> ```
>
> Esto funciona. Pero el código es feo. Uno de los beneficios de la programación funcional es que el código debe ser fácil de leer y conciso. Otra alternativa es **crear** un método envolvente con `try/catch`.
>
> ```Java
> private static List<String> createSafe() {
>     try {
>         return ExceptionCaseStudy.create();
>     } catch (IOException e) {
>         throw new RuntimeException(e);
>     }
> }
> ```
>
> Ahora **se puede** usar el envolvente seguro en el `Supplier` sin problema.
>
> ```Java
> public void wrapped() {
>     Supplier<List<String>> s2 = ExceptionCaseStudy::createSafe;
> }
> ```

---

#### Recopilación de resultados

**Se está** casi terminando de **aprender** sobre streams! El último tema **se basa** en lo que **se ha aprendido** hasta ahora para **agrupar** los resultados. Al principio del capítulo, **se vio** la operación terminal `collect()`. Hay muchos recopiladores predefinidos, incluyendo los que **se muestran** en la Tabla 10.10. Estos recopiladores **están disponibles** a través de métodos `static` en la clase `Collectors`. **Se analizan** los diferentes tipos de recopiladores en las siguientes secciones. **Se omitieron** los tipos genéricos para simplificar.

**TABLA 10.10** Ejemplos de recopiladores de agrupación/partición

| **Recopilador**                                                                                                                                 | **Descripción**                                                                                                                | **Valor de retorno al pasarlo a collect**                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `averagingDouble(ToDoubleFunction f)` `averagingInt(ToIntFunction f)` `averagingLong(ToLongFunction f)`                                         | Calcula el promedio para los tres tipos primitivos principales                                                                 | `Double`                                                                 |
| `counting()`                                                                                                                                    | Cuenta el número de elementos                                                                                                  | `Long`                                                                   |
| `filtering(Predicate p, Collector c)`                                                                                                           | Aplica filtro antes de llamar al recopilador _downstream_                                                                      | `R`                                                                      |
| `groupingBy(Function f)`                                                                                                                        | Crea agrupación de mapa por la función especificada con recopilador _downstream_ opcional y proveedor de tipo de mapa opcional | `Map<K, List<T>>`                                                        |
| `joining(CharSequence cs)`                                                                                                                      | Crea un único `String` usando `cs` como delimitador entre elementos si **se especifica** uno                                   | `String`                                                                 |
| `maxBy(Comparator c)` `minBy(Comparator c)`                                                                                                     | **Encuentra** el elemento más grande/más pequeño                                                                               | `Optional<T>`                                                            |
| `mapping(Function f, Collector dc)`                                                                                                             | **Agrega** otro nivel de recopiladores                                                                                         | `Collector`                                                              |
| `partitioningBy(Predicate p)` `partitioningBy(Predicate p, Collector dc)`                                                                       | Crea agrupación de mapa por predicado especificado con recopilador _downstream_ adicional opcional                             | `Map<Boolean, List<T>>`                                                  |
| `summarizingDouble(ToDoubleFunction f)` `summarizingInt(ToIntFunction f)` `summarizingLong(ToLongFunction f)`                                   | **Calcula** promedio, min, max, etc.                                                                                           | `DoubleSummaryStatistics` `IntSummaryStatistics` `LongSummaryStatistics` |
| `summingDouble(ToDoubleFunction f)` `summingInt(ToIntFunction f)` `summingLong(ToLongFunction f)`                                               | **Calcula** la suma para los tres tipos primitivos principales                                                                 | `Double` `Integer` `Long`                                                |
| `teeing(Collector c1, Collector c2, BiFunction f)`                                                                                              | **Trabaja** con los resultados de dos recopiladores para crear un nuevo tipo                                                   | `R`                                                                      |
| `toList()` `toSet()`                                                                                                                            | Crea un tipo arbitrario de lista o conjunto                                                                                    | `List` `Set`                                                             |
| `toCollection(Supplier s)`                                                                                                                      | Crea una `Collection` del tipo especificado                                                                                    | `Collection`                                                             |
| `toMap(Function k, Function v)` `toMap(Function k, Function v, BinaryOperator m)` `toMap(Function k, Function v, BinaryOperator m, Supplier s)` | Crea mapa usando funciones para **mapear** claves, valores, función de fusión opcional y proveedor de tipo de mapa opcional    | `Map`                                                                    |

> Hay un recopilador más llamado `reducing()`. No **se necesita** conocerlo para el examen. Es una reducción general en caso de que ninguno de los recopiladores anteriores **satisfaga** las necesidades.

#### Usando Recopiladores Básicos

Afortunadamente, muchos de estos recopiladores funcionan de la misma manera. **Se observa** un ejemplo:

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
String result = ohMy.collect(Collectors.joining(", "));
System.out.println(result); // lions, tigers, bears
```

**Hay que notar** cómo los recopiladores predefinidos están en la clase `Collectors` en lugar de en la interfaz `Collector`. Esto es un tema común, que **se vio** con `Collection` versus `Collections`. De hecho, **se ve** este patrón nuevamente en el Capítulo 14 cuando **se trabaja** con `Paths` y `Path` y otros tipos relacionados.

**Se pasa** el recopilador predefinido `joining()` al método `collect()`. Todos los elementos del stream **se fusionan** luego en un `String` con el delimitador especificado entre cada elemento. Es importante **pasar** el `Collector` al método `collect`. Un `Collector` no hace nada por sí solo.

**Se intenta** otro. ¿Cuál es la longitud promedio de los tres nombres de animales?

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Double result = ohMy.collect(Collectors.averagingInt(String::length));
System.out.println(result); // 5.333333333333333
```

El patrón es el mismo. **Se pasa** un recopilador a `collect()`, y este realiza el promedio. Esta vez, **se necesitó** **pasar** una función para decirle al recopilador qué promediar. **Se usó** una referencia a método, que devuelve un `int` al ser ejecutada. Con streams primitivos, el resultado de un promedio siempre era un `double`, independientemente del tipo que **se esté** promediando. Para los recopiladores, es un `Double` ya que esos necesitan un `Object`.

A menudo, **se encontrará** interactuando con código que fue escrito sin streams. Esto significa que **esperará** un tipo `Collection` en lugar de un tipo `Stream`. No hay problema. Todavía **se puede** expresar usando un `Stream` y luego **convertirlo** a una `Collection` al final. Por ejemplo:

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
TreeSet<String> result = ohMy
    .filter(s -> s.startsWith("t"))
    .collect(Collectors.toCollection(TreeSet::new));
System.out.println(result); // [tigers]
```

Esta vez **se tienen** las tres partes del stream pipeline. `Stream.of()` es la fuente del stream. La operación intermedia es `filter()`. Finalmente, la operación terminal es `collect()`, que **crea** un `TreeSet`. Si no **se importara** qué implementación de `Set` **se obtiene**, **se podría** haber escrito `Collectors.toSet()` en su lugar.

> **Usando `toList()`**
>
> Uno de los recopiladores más comunes es `Collectors.toList()`, que **convierte** el resultado de nuevo en una `List`. De hecho, es tan común que hay un atajo. Ambos hacen casi lo mismo:
>
> ```Java
> Stream<String> ohMy1 = Stream.of("lions", "tigers", "bears");
> List<String> mutableList = ohMy1.collect(Collectors.toList());
>
> Stream<String> ohMy2 = Stream.of("lions", "tigers", "bears");
> List<String> immutableList = ohMy2.toList();
> ```
>
> ¿Casi? Si bien ambos devuelven un `List<String>`, el contrato es diferente. El `Collectors.toList()` da una lista mutable que **se puede** editar más tarde. El `toList()` más corto no permite cambios. **Se puede** ver la diferencia en las siguientes líneas de código adicionales:
>
> ```Java
> mutableList.add("zebras");    // Sin problemas
> immutableList.add("zebras"); // UnsupportedOperationException
> ```

#### Recopilando en Mapas

El código que usa `Collectors` involucrando mapas puede volverse bastante largo. **Se construirá** poco a poco. **Hay que asegurarse** de **entender** cada ejemplo antes de **pasar** al siguiente. **Se empieza** con un ejemplo directo para **crear** un mapa desde un stream:

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<String, Integer> map = ohMy.collect(
    Collectors.toMap(s -> s, String::length));
System.out.println(map); // {lions=5, bears=5, tigers=6}
```

Al **crear** un mapa, **se necesita** especificar dos funciones. La primera le dice al recopilador cómo **crear** la clave. En el ejemplo, **se usa** el `String` proporcionado como clave. La segunda le dice al recopilador cómo **crear** el valor. En el ejemplo, **se usa** la longitud del `String` como valor.

> Devolver el mismo valor pasado a una lambda es una operación común, por lo que Java proporciona un método para ello. **Se puede** reescribir `s -> s` como `Function.identity()`. No es más corto y puede o no ser más claro, así que **hay que usar** el propio criterio sobre si usarlo.

Ahora **se quiere** hacer lo inverso y mapear la longitud del nombre del animal al nombre mismo. El primer intento incorrecto **se muestra** aquí:

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Integer, String> map = ohMy.collect(Collectors.toMap(
    String::length,
    k -> k)); // MALO
```

**Ejecutar** esto da una excepción similar a la siguiente:

```Plaintext
Exception in thread "main" java.lang.IllegalStateException:
    Duplicate key 5 (attempted merging values lions and bears)
```

¿Qué está mal? Dos de los nombres de los animales tienen la misma longitud. No **se le dijo** a Java qué hacer. ¿Debería el recopilador **elegir** el primero que **encuentra**? ¿El último? ¿**Concatenar** los dos? Dado que el recopilador no tiene idea de qué hacer, **"resuelve"** el problema lanzando una excepción y haciéndolo problema nuestro. ¡Qué considerado! Supongamos que el requisito es **crear** un `String` separado por comas con los nombres de los animales. **Se podría** escribir esto:

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Integer, String> map = ohMy.collect(Collectors.toMap(
    String::length,
    k -> k,
    (s1, s2) -> s1 + "," + s2));
System.out.println(map);             // {5=lions,bears, 6=tigers}
System.out.println(map.getClass()); // class java.util.HashMap
```

Resulta que el `Map` devuelto es un `HashMap`. Este comportamiento no está garantizado. Supongamos que **se quiere** que el código devuelva un `TreeMap` en su lugar. No hay problema. Solo **se agregaría** una referencia de constructor como parámetro:

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
TreeMap<Integer, String> map = ohMy.collect(Collectors.toMap(
    String::length,
    k -> k,
    (s1, s2) -> s1 + "," + s2,
    TreeMap::new));
System.out.println(map);             // {5=lions,bears, 6=tigers}
System.out.println(map.getClass()); // class java.util.TreeMap
```

Esta vez **se obtiene** el tipo que **se especificó**. ¿Está bien hasta aquí? Este código es largo pero no particularmente complicado. ¡**Se prometió** que el código sería largo!

#### Agrupando, Particionando y Mapeando

¡Gran trabajo **llegando** hasta aquí! A los creadores del examen les gusta **preguntar** sobre `groupingBy()` y `partitioningBy()`, por lo que **hay que asegurarse** de **entender** muy bien estas secciones. Ahora supongamos que **se quieren** obtener grupos de nombres por su longitud. **Se puede** hacer eso diciendo que **se quiere** agrupar por longitud.

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Integer, List<String>> map = ohMy.collect(
    Collectors.groupingBy(String::length));
System.out.println(map); // {5=[lions, bears], 6=[tigers]}
```

El recopilador `groupingBy()` le **dice** a `collect()` que debe **agrupar** todos los elementos del stream en un `Map`. La función **determina** las claves en el `Map`. Cada valor en el `Map` es una `List` de todas las entradas que coinciden con esa clave.

> **Hay que notar** que la función que **se llama** en `groupingBy()` no puede devolver `null`. No permite claves `null`.

Supongamos que no **se quiere** una `List` como valor en el mapa y **se prefiere** un `Set` en su lugar. No hay problema. Hay otra firma del método que permite **pasar** un _recopilador downstream_ (_downstream collector_). Este es un segundo recopilador que **hace** algo especial con los valores.

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Integer, Set<String>> map = ohMy.collect(
    Collectors.groupingBy(
        String::length,
        Collectors.toSet()));
System.out.println(map); // {5=[lions, bears], 6=[tigers]}
```

Incluso **se puede** cambiar el tipo de `Map` devuelto a través de otro parámetro.

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
TreeMap<Integer, Set<String>> map = ohMy.collect(
    Collectors.groupingBy(
        String::length,
        TreeMap::new,
        Collectors.toSet()));
System.out.println(map); // {5=[lions, bears], 6=[tigers]}
```

Esto es muy flexible. ¿Y si **se quiere** cambiar el tipo de `Map` devuelto pero **dejar** el tipo de los valores como una `List`? No hay un método específico para ello porque es suficientemente fácil de **escribir** con los existentes.

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
TreeMap<Integer, List<String>> map = ohMy.collect(
    Collectors.groupingBy(
        String::length,
        TreeMap::new,
        Collectors.toList()));
System.out.println(map);
```

La partición es un caso especial de agrupación. Con la partición, solo hay dos grupos posibles: verdadero y falso. La _partición_ es como **dividir** una lista en dos partes.

Supongamos que **se está** haciendo un letrero para **colocar** fuera del exhibidor de cada animal. **Se tienen** dos tamaños de letreros. Uno puede acomodar nombres con cinco o menos caracteres. El otro **se necesita** para nombres más largos. **Se puede** particionar la lista según qué letrero **se necesita**.

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Boolean, List<String>> map = ohMy.collect(
    Collectors.partitioningBy(s -> s.length() <= 5));
System.out.println(map); // {false=[tigers], true=[lions, bears]}
```

Aquí **se pasa** un `Predicate` con la lógica que determina a qué grupo pertenece cada nombre de animal. Ahora supongamos que **se averiguó** cómo usar una fuente diferente, y siete caracteres ahora **caben** en el letrero más pequeño. Solo **se cambia** el `Predicate`.

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Boolean, List<String>> map = ohMy.collect(
    Collectors.partitioningBy(s -> s.length() <= 7));
System.out.println(map); // {false=[], true=[lions, tigers, bears]}
```

**Hay que notar** que todavía hay dos claves en el mapa — una para cada valor `boolean`. Resulta que uno de los valores es una lista vacía, pero sigue estando ahí. Al igual que con `groupingBy()`, **se puede** cambiar el tipo de `List` a algo más.

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Boolean, Set<String>> map = ohMy.collect(
    Collectors.partitioningBy(
        s -> s.length() <= 7,
        Collectors.toSet()));
System.out.println(map); // {false=[], true=[lions, tigers, bears]}
```

A diferencia de `groupingBy()`, no **se puede** cambiar el tipo de `Map` que **se devuelve**. Sin embargo, solo hay dos claves en el mapa, ¿entonces realmente importa qué tipo de `Map` se **use**?

En lugar de **usar** el recopilador _downstream_ para especificar el tipo, **se puede** usar cualquiera de los recopiladores que **se han mostrado** ya. Por ejemplo, **se puede** agrupar por la longitud del nombre del animal para ver cuántos de cada longitud **se tienen**.

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Integer, Long> map = ohMy.collect(
    Collectors.groupingBy(
        String::length,
        Collectors.counting()));
System.out.println(map); // {5=2, 6=1}
```

> **Depurando Genéricos Complicados**
>
> Cuando **se trabaja** con `collect()`, a menudo hay muchos niveles de genéricos, haciendo que los errores del compilador sean ilegibles. Aquí hay tres técnicas útiles para **tratar** con esta situación:
>
> - **Comenzar** desde cero con una sentencia simple, y seguir agregándole. Al **hacer** un pequeño cambio a la vez, **se sabrá** qué código introdujo el error.
> - **Extraer** partes de la sentencia en sentencias separadas. Por ejemplo, **intenta** escribir `Collectors.groupingBy(String::length, Collectors.counting());`. Si compila, **se sabe** que el problema está en otro lugar. Si no compila, **se tiene** una sentencia mucho más corta para solucionar problemas.
> - **Usar** comodines genéricos para el tipo de retorno de la sentencia final: por ejemplo, `Map<?, ?>`. Si ese cambio solo permite que el código **compile**, **se sabrá** que el problema está en el tipo de retorno que no es lo que **se espera**.

Finalmente, hay un recopilador `mapping()` que permite **bajar** un nivel y **agregar** otro recopilador. Supongamos que **se quisiera** obtener la primera letra del primer animal alfabéticamente de cada longitud. ¿Por qué? Tal vez para muestreo aleatorio. Los ejemplos en esta parte del examen son bastante artificiales. **Se escribiría** lo siguiente:

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Integer, Optional<Character>> map = ohMy.collect(
    Collectors.groupingBy(
        String::length,
        Collectors.mapping(
            s -> s.charAt(0),
            Collectors.minBy((a, b) -> a - b))));
System.out.println(map); // {5=Optional[b], 6=Optional[t]}
```

No **se va** a decir que este código es fácil de leer. **Se dirá** que es lo más complicado que **se necesita** entender para el examen. Comparándolo con el ejemplo anterior, **se puede** ver que **se reemplazó** `counting()` con `mapping()`. Resulta que `mapping()` toma dos parámetros: la función para el valor y cómo agrupar más.

**Se podrían** ver recopiladores usados con una importación `static` para hacer el código más corto. El examen incluso podría usar `var` para el tipo de retorno y menos indentación de lo que **se usó**. Esto significa que **se podría** ver algo como esto:

```Java
var ohMy = Stream.of("lions", "tigers", "bears");
var map = ohMy.collect(groupingBy(String::length,
    mapping(s -> s.charAt(0), minBy((a, b) -> a - b))));
System.out.println(map); // {5=Optional[b], 6=Optional[t]}
```

El código hace lo mismo que en el ejemplo anterior. Esto es importante **reconocer** los nombres de los recopiladores porque puede que no **se tenga** el nombre de la clase `Collectors` para llamar la atención.

#### Recopiladores Teeing

Supongamos que **se quieren** devolver dos cosas. Como **se ha aprendido**, esto es problemático con los streams porque solo **se obtiene** un pase. Las estadísticas de resumen son buenas cuando **se quieren** esas operaciones. Afortunadamente, **se puede** usar `teeing()` para **devolver** múltiples valores propios.

Primero, **se define** el tipo de retorno. **Se usa** un _record_ aquí:

```Java
record Separations(String spaceSeparated, String commaSeparated) {}
```

Ahora **se escribe** el stream. A medida que **se lee**, **hay que prestar** atención al número de `Collectors`:

```Java
var list = List.of("x", "y", "z");
Separations result = list.stream()
    .collect(Collectors.teeing(
            Collectors.joining(" "),
            Collectors.joining(","),
            (s, c) -> new Separations(s, c)));
System.out.println(result);
```

Cuando **se ejecuta**, el código **imprime** lo siguiente:

```Plaintext
Separations[spaceSeparated=x y z, commaSeparated=x,y,z]
```

Hay tres `Collectors` en este código. Dos de ellos son para `joining()` y **producen** los valores que **se quieren** devolver. El tercero es `teeing()`, que **combina** los resultados en el único objeto que **se quiere** devolver. De esta manera, Java está contento porque solo **se devuelve** un objeto, y **se está** contento porque no **se tiene** que **recorrer** el stream dos veces.

#### Usando un *Spliterator*

Supongamos que **se compra** una bolsa de comida para que dos niños puedan alimentar a los animales en el zoológico de mascotas. Para **evitar** discusiones, **se ha venido** preparado con una bolsa extra vacía. **Se saca** aproximadamente la mitad de la comida de la bolsa principal y **se pone** en la bolsa que **se trajo** de casa. La bolsa original todavía existe con la otra mitad de la comida.

Un `Spliterator` proporciona este nivel de control sobre el procesamiento. **Comienza** con una `Collection` o un stream — esa es la bolsa de comida. **Se llama** a `trySplit()` para **sacar** algo de comida de la bolsa. El resto de la comida **se queda** en el objeto `Spliterator` original.

Las características de un `Spliterator` dependen de la fuente de datos subyacente. Una fuente de datos de `Collection` es un `Spliterator` básico. Por contraste, cuando **se usa** una fuente de datos de `Stream`, el `Spliterator` puede **ser** paralelo o incluso infinito. El `Stream` en sí **se ejecuta** perezosamente en lugar de cuando **se crea** el `Spliterator`.

**Implementar** un `Spliterator` propio puede **volverse** complicado y convenientemente no está en el examen. Sí **se necesita** saber cómo **trabajar** con algunos de los métodos comunes declarados en esta interfaz. Los métodos simplificados que **se necesitan** conocer están en la Tabla 10.11.

**TABLA 10.11** Métodos de `Spliterator`

| **Método** | **Descripción** |
|---|---|
| `Spliterator<T> trySplit()` | Devuelve `Spliterator` que contiene idealmente la mitad de los datos, que **se eliminan** del `Spliterator` actual. Este método **se puede** llamar múltiples veces y eventualmente devolverá `null` cuando los datos ya no **puedan** dividirse. |
| `void forEachRemaining(Consumer<T> c)` | **Procesa** los elementos restantes en el `Spliterator`. |
| `boolean tryAdvance(Consumer<T> c)` | **Procesa** un único elemento del `Spliterator` si quedan algunos. **Devuelve** si el elemento **fue procesado**. |

**Se observa** ahora un ejemplo donde **se divide** la bolsa en tres:

```Java
12: var stream = List.of("bird-", "bunny-", "cat-", "dog-", "fish-", "lamb-",
13:     "mouse-");
14: Spliterator<String> originalBagOfFood = stream.spliterator();
15: Spliterator<String> emmasBag = originalBagOfFood.trySplit();
16: emmasBag.forEachRemaining(System.out::print); // bird-bunny-cat-
17:
18: Spliterator<String> jillsBag = originalBagOfFood.trySplit();
19: jillsBag.tryAdvance(System.out::print);       // dog-
20: jillsBag.forEachRemaining(System.out::print); // fish-
21:
22: originalBagOfFood.forEachRemaining(System.out::print); // lamb-mouse-
```

En las líneas 12 y 13, **se define** una `List`. Las líneas 14 y 15 **crean** dos referencias de `Spliterator`. El primero es la bolsa original, que contiene los siete elementos. El segundo es la división de la bolsa original, **poniendo** aproximadamente la mitad de los elementos al principio en la bolsa de Emma. Luego **se imprimen** los tres contenidos de la bolsa de Emma en la línea 16.

La bolsa original de comida ahora contiene cuatro elementos. **Se crea** un nuevo `Spliterator` en la línea 18 y **se ponen** los dos primeros elementos en la bolsa de Jill. **Se usa** `tryAdvance()` en la línea 19 para **mostrar** un único elemento, y luego la línea 20 **imprime** todos los elementos restantes (¡solo queda uno!).

**Se comenzó** con siete elementos, **se eliminaron** tres, y luego **se eliminaron** dos más. Esto nos deja con dos elementos en la bolsa original **creada** en la línea 14. Estos dos elementos **se muestran** en la línea 22.

**Se intenta** ahora un ejemplo con un `Stream`. Esta es una forma complicada de imprimir 123:

```Java
var originalBag = Stream.iterate(1, n -> ++n)
    .spliterator();

Spliterator<Integer> newBag = originalBag.trySplit();

newBag.tryAdvance(System.out::print); // 1
newBag.tryAdvance(System.out::print); // 2
newBag.tryAdvance(System.out::print); // 3
```

**Se podría** haber notado que este es un stream infinito. ¡No hay problema! El `Spliterator` **reconoce** que el stream es infinito y no intenta **darle** la mitad. En cambio, `newBag` contiene una gran cantidad de elementos. **Se obtienen** los primeros tres ya que **se llama** a `tryAdvance()` tres veces. Sería una mala idea llamar a `forEachRemaining()` en un stream infinito.

**Hay que notar** que un `Spliterator` puede tener una serie de características como `CONCURRENT`, `ORDERED`, `SIZED` y `SORTED`. Solo **se verá** un `Spliterator` directo en el examen. Por ejemplo, el stream infinito no era `SIZED`.

---

**Ver también:** [[Arrays]] | [[Algoritmos]]

