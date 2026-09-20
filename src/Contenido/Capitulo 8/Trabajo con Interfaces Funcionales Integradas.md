**Sería** inconveniente escribir la propia interfaz funcional cada vez que **se quiera** escribir una lambda. Afortunadamente, **se proporciona** un gran número de interfaces funcionales de uso general. **Se cubren** en esta sección.

Las interfaces funcionales principales de la Tabla 8.4 **se proporcionan** en el paquete `java.util.function`. **Se cubren** los genéricos en el próximo capítulo, pero por ahora, solo **se necesita** saber que `<T>` permite que la interfaz tome un objeto de un tipo especificado. Si **se necesita** un segundo parámetro de tipo, **se usa** la siguiente letra, `U`. Si **se necesita** un tipo de retorno distinto, **se elige** `R` para *retorno* (_return_) como el tipo genérico.

**TABLA 8.4** Interfaces funcionales comunes

| **Interfaz funcional** | **Tipo de retorno** | **Nombre del método** | **# de parámetros** |
|---|---|---|---|
| `Supplier<T>` | `T` | `get()` | 0 |
| `Consumer<T>` | `void` | `accept(T)` | 1 (T) |
| `BiConsumer<T, U>` | `void` | `accept(T,U)` | 2 (T, U) |
| `Predicate<T>` | `boolean` | `test(T)` | 1 (T) |
| `BiPredicate<T, U>` | `boolean` | `test(T,U)` | 2 (T, U) |
| `Function<T, R>` | `R` | `apply(T)` | 1 (T) |
| `BiFunction<T, U, R>` | `R` | `apply(T,U)` | 2 (T, U) |
| `UnaryOperator<T>` | `T` | `apply(T)` | 1 (T) |
| `BinaryOperator<T>` | `T` | `apply(T,T)` | 2 (T, T) |

Para el examen, **se necesita** memorizar la Tabla 8.4. **Se dará** mucha práctica en esta sección para ayudar a que sea memorable. La mayor parte del tiempo no **se asigna** la implementación de la interfaz a una variable. El nombre de la interfaz está implícito y **se pasa** directamente al método que lo necesita. **Se presentan** los nombres para que **se pueda** comprender mejor y recordar qué está pasando. En el próximo capítulo, **se asumirá** que esto **se domina** y **se dejará** de crear la variable intermedia.

> **Se aprenderán** algunas interfaces funcionales más adelante en este proyecto. En el próximo capítulo, **se cubre** `Comparator`. En el Capítulo 13, "Concurrencia", **se discuten** `Runnable` y `Callable`. Estas pueden aparecer en el examen cuando **se pida** reconocer interfaces funcionales.

**Se analizará** cómo implementar cada una de estas interfaces. Dado que tanto las lambdas como las referencias a métodos aparecen en todo el examen, **se muestra** una implementación usando ambas donde sea posible. Después de presentar las interfaces, también **se cubren** algunos métodos de conveniencia disponibles en estas interfaces.

#### Implementando `Supplier`

Un `Supplier` **se usa** cuando **se quiere** generar o suministrar valores sin tomar ninguna entrada. La interfaz `Supplier` **se define** de la siguiente manera:

```Java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

**Se puede** crear un objeto `LocalDate` usando el método de fábrica `now()`. Este ejemplo muestra cómo usar un `Supplier` para llamar a esta fábrica:

```Java
Supplier<LocalDate> s1 = LocalDate::now;
Supplier<LocalDate> s2 = () -> LocalDate.now();

LocalDate d1 = s1.get();
LocalDate d2 = s2.get();

System.out.println(d1);  // 2025-02-20
System.out.println(d2);  // 2025-02-20
```

Este ejemplo imprime una fecha dos veces. También **es** una buena oportunidad para revisar las referencias a métodos `static`. La referencia a método `LocalDate::now` **se usa** para crear un `Supplier` que **se asigna** a una variable intermedia `s1`. Un `Supplier` **se usa** con frecuencia al construir nuevos objetos. Por ejemplo, **se pueden** imprimir dos objetos `StringBuilder` vacíos.

```Java
Supplier<StringBuilder> s1 = StringBuilder::new;
Supplier<StringBuilder> s2 = () -> new StringBuilder();

System.out.println(s1.get());  // Cadena vacía
System.out.println(s2.get());  // Cadena vacía
```

Esta vez, **se usó** una referencia a constructor para crear el objeto. **Se han estado** usando genéricos para declarar qué tipo de `Supplier` **se está** usando. Esto puede ser un poco largo de leer. ¿**Se puede** descifrar qué hace lo siguiente? Solo **hay que** tomarlo un paso a la vez.

```Java
Supplier<ArrayList<String>> s3 = ArrayList::new;
ArrayList<String> a1 = s3.get();
System.out.println(a1);  // []
```

**Se tiene** un `Supplier` de un cierto tipo. Ese tipo resulta ser `ArrayList<String>`. Luego llamar a `get()` crea una nueva instancia de `ArrayList<String>`, que **es** el tipo genérico del `Supplier`: en otras palabras, un genérico que contiene otro genérico. **Se debe** mirar el código cuidadosamente cuando aparezca este tipo de cosa.

**Se observa** cómo **se llamó** a `get()` en la interfaz funcional. ¿Qué sucedería si **se intentara** imprimir `s3` directamente?

```Java
System.out.println(s3);
```

El código imprime algo como esto:

```
functionalinterface.BuiltIns$$Lambda$1/0x0000000800066840@4909b8da
```

Ese **es** el resultado de llamar a `toString()` en una lambda. La clase de prueba **se llama** `BuiltIns`, y está en un paquete creado llamado `functionalinterface`. Luego viene `$$`, lo que significa que la clase no existe en un archivo de clase en el sistema de archivos. Existe solo en memoria. No **hay que** preocuparse por el resto.

#### Implementando `Consumer` y `BiConsumer`

**Se usa** un `Consumer` cuando **se quiere** hacer algo con un parámetro pero no devolver nada. `BiConsumer` hace lo mismo, excepto que toma dos parámetros. Las interfaces **se definen** de la siguiente manera:

```Java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
    // método default omitido
}

@FunctionalInterface
public interface BiConsumer<T, U> {
    void accept(T t, U u);
    // método default omitido
}
```

> **Se notará** este patrón. *Bi* significa dos. Viene del latín, pero **se puede** recordar a partir de palabras en inglés como *binary* (0 o 1) o *bicycle* (dos ruedas). El método de la interfaz siempre tomará dos entradas cuando **se vea** *Bi*.

Imprimir **es** un uso común de la interfaz `Consumer`.

```Java
Consumer<String> c1 = System.out::println;
Consumer<String> c2 = x -> System.out.println(x);

c1.accept("Annie");  // Annie
c2.accept("Annie");  // Annie
```

`BiConsumer` **se llama** con dos parámetros. No tienen que ser del mismo tipo. Por ejemplo, **se puede** poner una clave y un valor en un mapa usando esta interfaz:

```Java
var map = new HashMap<String, Integer>();
BiConsumer<String, Integer> b1 = map::put;
BiConsumer<String, Integer> b2 = (k, v) -> map.put(k, v);

b1.accept("chicken", 7);
b2.accept("chick", 1);

System.out.println(map);  // {chicken=7, chick=1}
```

La salida **es** `{chicken=7, chick=1}`, lo que muestra que ambas implementaciones de `BiConsumer` **fueron llamadas**. Al declarar `b1`, **se usó** una referencia a método de instancia en un objeto ya que **se quería** llamar a un método en la variable local `map`. El código para instanciar `b1` **es** bastante más corto que el código para `b2`. Probablemente por eso el examen **es** tan aficionado a las referencias a métodos.

Como otro ejemplo, **se usa** el mismo tipo para ambos parámetros genéricos:

```Java
var map = new HashMap<String, String>();
BiConsumer<String, String> b1 = map::put;
BiConsumer<String, String> b2 = (k, v) -> map.put(k, v);

b1.accept("chicken", "Cluck");
b2.accept("chick", "Tweep");

System.out.println(map);  // {chicken=Cluck, chick=Tweep}
```

Esto muestra que un `BiConsumer` **puede** usar el mismo tipo para ambos parámetros genéricos `T` y `U`.

#### Implementando `Predicate` y `BiPredicate`

`Predicate` **se usa** frecuentemente al filtrar o coincidir. Ambas **son** operaciones comunes. Un `BiPredicate` **es** igual que un `Predicate`, excepto que toma dos parámetros en lugar de uno. Las interfaces **se definen** de la siguiente manera:

```Java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
    // métodos default y static omitidos
}

@FunctionalInterface
public interface BiPredicate<T, U> {
    boolean test(T t, U u);
    // métodos default omitidos
}
```

**Se puede** usar un `Predicate` para probar una condición.

```Java
Predicate<String> p1 = String::isEmpty;
Predicate<String> p2 = x -> x.isEmpty();

System.out.println(p1.test(""));  // true
System.out.println(p2.test(""));  // true
```

Esto imprime `true` dos veces. Más interesante **es** un `BiPredicate`. Este ejemplo también imprime `true` dos veces:

```Java
BiPredicate<String, String> b1 = String::startsWith;
BiPredicate<String, String> b2 =
    (string, prefix) -> string.startsWith(prefix);

System.out.println(b1.test("chicken", "chick"));  // true
System.out.println(b2.test("chicken", "chick"));  // true
```

La referencia a método incluye tanto la variable de instancia como el parámetro para `startsWith()`. Este **es** un buen ejemplo de cómo las referencias a métodos ahorran bastante escritura. La desventaja **es** que son menos explícitas y realmente **hay que** entender qué está pasando.

#### Implementando `Function` y `BiFunction`

Una `Function` **es** responsable de convertir un parámetro en un valor de un tipo potencialmente diferente y devolverlo. De manera similar, una `BiFunction` **es** responsable de convertir dos parámetros en un valor y devolverlo. Las interfaces **se definen** de la siguiente manera:

```Java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
    // métodos default y static omitidos
}

@FunctionalInterface
public interface BiFunction<T, U, R> {
    R apply(T t, U u);
    // método default omitido
}
```

Por ejemplo, esta función convierte un `String` a la longitud del `String`:

```Java
Function<String, Integer> f1 = String::length;
Function<String, Integer> f2 = x -> x.length();

System.out.println(f1.apply("cluck"));  // 5
System.out.println(f2.apply("cluck"));  // 5
```

Esta función convierte un `String` en un `Integer`. Técnicamente, convierte el `String` en un `int`, que **se autoboxea** en un `Integer`. Los tipos no tienen que ser diferentes. El siguiente combina dos objetos `String` y produce otro `String`:

```Java
BiFunction<String, String, String> b1 = String::concat;
BiFunction<String, String, String> b2 =
    (string, toAdd) -> string.concat(toAdd);

System.out.println(b1.apply("baby ", "chick"));  // baby chick
System.out.println(b2.apply("baby ", "chick"));  // baby chick
```

Los primeros dos tipos en la `BiFunction` son los tipos de entrada. El tercero **es** el tipo de resultado. Para la referencia a método, el primer parámetro **es** la instancia en la que **se llama** a `concat()`, y el segundo **se pasa** a `concat()`.

#### Implementando `UnaryOperator` y `BinaryOperator`

`UnaryOperator` y `BinaryOperator` son casos especiales de una `Function`. Requieren que todos los parámetros de tipo sean del mismo tipo. Un `UnaryOperator` transforma su valor en uno del mismo tipo. Por ejemplo, incrementar en uno **es** una operación unaria. De hecho, `UnaryOperator` extiende `Function`. Un `BinaryOperator` combina dos valores en uno del mismo tipo. Añadir dos números **es** una operación binaria. De manera similar, `BinaryOperator` extiende `BiFunction`. Las interfaces **se definen** de la siguiente manera:

```Java
@FunctionalInterface
public interface UnaryOperator<T> extends Function<T, T> {
    // método static omitido
}

@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T, T, T> {
    // métodos static omitidos
}
```

Esto significa que las firmas de los métodos **se ven** así:

```Java
T apply(T t);         // UnaryOperator
T apply(T t1, T t2);  // BinaryOperator
```

En el Javadoc, **se notará** que estos métodos **se heredan** de la superclase `Function`/`BiFunction`. Las declaraciones genéricas en la subclase son las que fuerzan a que el tipo sea el mismo. Para el ejemplo unario, **se nota** cómo el tipo de retorno **es** del mismo tipo que el parámetro.

```Java
UnaryOperator<String> u1 = String::toUpperCase;
UnaryOperator<String> u2 = x -> x.toUpperCase();

System.out.println(u1.apply("chirp"));  // CHIRP
System.out.println(u2.apply("chirp"));  // CHIRP
```

Esto imprime `CHIRP` dos veces. No **se necesita** especificar el tipo de retorno en los genéricos porque `UnaryOperator` requiere que sea el mismo que el parámetro. Y ahora está el ejemplo binario:

```Java
BinaryOperator<String> b1 = String::concat;
BinaryOperator<String> b2 = (string, toAdd) ->
    string.concat(toAdd);

System.out.println(b1.apply("baby ", "chick"));  // baby chick
System.out.println(b2.apply("baby ", "chick"));  // baby chick
```

**Se nota** que esto hace lo mismo que el ejemplo de `BiFunction`. El código **es** más conciso, lo que muestra la importancia de usar la mejor interfaz funcional. **Es** bueno tener un solo tipo genérico especificado en lugar de tres.

#### Verificando Interfaces Funcionales

**Es** realmente importante conocer el número de parámetros, los tipos, el valor de retorno y el nombre del método para cada una de las interfaces funcionales. Ahora **sería** un buen momento para memorizar la Tabla 8.4 si no **se ha hecho** ya. **Se realizarán** algunos ejemplos para practicar.

¿Qué interfaz funcional **se usaría** en estas tres situaciones?

- Devuelve un `String` sin tomar ningún parámetro
- Devuelve un `Boolean` y toma un `String`
- Devuelve un `Integer` y toma dos `Integers`

¿Listo? **Se debe pensar** en las respuestas antes de continuar. La primera **es** un `Supplier<String>` porque genera un objeto y no toma parámetros. La segunda **es** una `Function<String,Boolean>` porque toma un parámetro y devuelve otro tipo. Es un poco complicado. **Se podría pensar** que **es** un `Predicate<String>`. **Se nota** que un `Predicate` devuelve un primitivo `boolean` y no un objeto `Boolean`.

Finalmente, la tercera **es** un `BinaryOperator<Integer>` o un `BiFunction<Integer,Integer,Integer>`. Dado que `BinaryOperator` **es** un caso especial de `BiFunction`, cualquiera **es** una respuesta correcta. `BinaryOperator<Integer>` **es** la mejor respuesta de las dos ya que **es** más específico.

**Se intenta** este ejercicio de nuevo pero con código. Lo primero que **se hace** es mirar cuántos parámetros toma la lambda y si hay un valor de retorno. ¿Qué interfaz funcional **se usaría** para rellenar los espacios en blanco para estas?

```Java
6:   ___________<List> ex1 = x -> "".equals(x.get(0));
7:   ___________<Long> ex2 = (Long l) -> System.out.println(l);
8:   ___________<String, String> ex3 = (s1, s2) -> false;
```

De nuevo, **se debe pensar** en las respuestas antes de continuar. La línea 6 pasa un parámetro `List` a la lambda y devuelve un `boolean`. Esto **indica** que **es** un `Predicate` o `Function`. Dado que la declaración genérica solo tiene un parámetro, **es** un `Predicate`.

La línea 7 pasa un parámetro `Long` a la lambda y no devuelve nada. Esto **indica** que **es** un `Consumer`. La línea 8 toma dos parámetros y devuelve un `boolean`. Cuando **se ve** un `boolean` devuelto, **se debe pensar** en `Predicate` a menos que los genéricos especifiquen un tipo de retorno `Boolean`. En este caso, hay dos parámetros, por lo que **es** un `BiPredicate`.

¿Le están pareciendo fáciles? Si no, **se debe revisar** la Tabla 8.4 nuevamente. Ahora que **se está** fresco del estudio de la tabla, **se va a** jugar a "identificar el error". Estos **están** destinados a ser complicados.

```Java
6: Function<List<String>> ex1 = x -> x.get(0);  // NO COMPILA
7: UnaryOperator<Long> ex2 = (Long l) -> 3.14;  // NO COMPILA
```

La línea 6 afirma ser una `Function`. Una `Function` necesita especificar dos tipos genéricos: el tipo del parámetro de entrada y el tipo del valor de retorno. El tipo del valor de retorno **falta** en la línea 6, lo que hace que el código no compile. La línea 7 **es** un `UnaryOperator`, que devuelve el mismo tipo que el que **se le pasa**. El ejemplo devuelve un `double` en lugar de un `Long`, lo que hace que el código no compile.

#### Usando Métodos de Conveniencia en Interfaces Funcionales

Por definición, todas las interfaces funcionales tienen un único método abstracto. Esto no significa que solo puedan tener un método. Varias de las interfaces funcionales comunes proporcionan una cantidad de métodos `default` útiles.

La Tabla 8.5 muestra los métodos de conveniencia en las interfaces funcionales integradas que **se necesitan** conocer para el examen. Todos estos facilitan la modificación o combinación de interfaces funcionales del mismo tipo. **Se nota** que la Tabla 8.5 muestra solo las interfaces principales. Las interfaces `BiConsumer`, `BiFunction` y `BiPredicate` tienen métodos similares disponibles.

**TABLA 8.5** Métodos de conveniencia

| **Instancia de interfaz** | **Tipo de retorno del método** | **Nombre del método** | **Parámetros del método** |
|---|---|---|---|
| `Consumer` | `Consumer` | `andThen()` | `Consumer` |
| `Function` | `Function` | `andThen()` | `Function` |
| `Function` | `Function` | `compose()` | `Function` |
| `Predicate` | `Predicate` | `and()` | `Predicate` |
| `Predicate` | `Predicate` | `negate()` | — |
| `Predicate` | `Predicate` | `or()` | `Predicate` |

**Se comienza** con estas dos variables `Predicate`:

```Java
Predicate<String> egg = s -> s.contains("egg");
Predicate<String> brown = s -> s.contains("brown");
```

Ahora **se quiere** un `Predicate` para los huevos marrones y otro para todos los demás colores de huevos. **Se podría** escribir esto a mano, como **se muestra** aquí:

```Java
Predicate<String> brownEggs = s -> s.contains("egg") &&
    s.contains("brown");
Predicate<String> otherEggs = s -> s.contains("egg") &&
    !s.contains("brown");
```

Esto funciona, pero no **es** ideal. **Es** un poco largo de leer y contiene duplicación. ¿Qué pasaría si **se decide** que la letra *e* debe estar en mayúsculas en *egg*? **Se tendría** que cambiar en tres variables: `egg`, `brownEggs` y `otherEggs`. Una mejor manera de manejar esta situación **es** usar dos de los métodos `default` en `Predicate`.

```Java
Predicate<String> brownEggs = egg.and(brown);
Predicate<String> otherEggs = egg.and(brown.negate());
```

¡Genial! Ahora **se está** reutilizando la lógica en las variables `Predicate` originales para construir dos nuevas. **Es** más corto y más claro cuál **es** la relación entre las variables. También **se puede** cambiar la ortografía de *egg* en un solo lugar, y los otros dos objetos tendrán nueva lógica porque la hacen referencia.

Pasando a `Consumer`, **se observará** el método `andThen()`, que ejecuta dos interfaces funcionales en secuencia.

```Java
Consumer<String> c1 = x -> System.out.print("1: " + x);
Consumer<String> c2 = x -> System.out.print(",2: " + x);

Consumer<String> combined = c1.andThen(c2);
combined.accept("Annie");  // 1: Annie,2: Annie
```

**Se nota** cómo el mismo parámetro **se pasa** a ambas `c1` y `c2`. Esto muestra que las instancias de `Consumer` **se ejecutan** en secuencia y son independientes entre sí. Por el contrario, el método `compose()` en `Function` encadena interfaces funcionales. Sin embargo, **pasa** la salida de una como entrada de la otra.

```Java
Function<Integer, Integer> before = x -> x + 1;
Function<Integer, Integer> after = x -> x * 2;

Function<Integer, Integer> combined = after.compose(before);
System.out.println(combined.apply(3));  // 8
```

Esta vez, `before` **se ejecuta** primero, convirtiendo 3 en 4. Luego **se ejecuta** `after`, duplicando 4 a 8. Todos los métodos en esta sección son útiles para simplificar el código al trabajar con interfaces funcionales.

#### Aprendiendo las Interfaces Funcionales para Primitivos

La mayoría de las interfaces funcionales **son** para los tipos `double`, `int` y `long`. Hay una excepción, que **es** `BooleanSupplier`. **Se cubre** antes de presentar las interfaces funcionales para `double`, `int` y `long`.

##### Interfaces Funcionales para `boolean`

`BooleanSupplier` **es** un tipo separado. Tiene un método para implementar.

```Java
@FunctionalInterface
public interface BooleanSupplier {
    boolean getAsBoolean();
}
```

Funciona igual que **se espera** de las interfaces funcionales. Aquí hay un ejemplo:

```Java
12: BooleanSupplier b1 = () -> true;
13: BooleanSupplier b2 = () -> Math.random()> .5;
14: System.out.println(b1.getAsBoolean());  // true
15: System.out.println(b2.getAsBoolean());  // false
```

Las líneas 12 y 13 crean cada una un `BooleanSupplier`, que **es** la única interfaz funcional para `boolean`. La línea 14 imprime `true`, ya que **es** el resultado de `b1`. La línea 15 imprime `true` o `false`, dependiendo del valor aleatorio generado.

##### Interfaces Funcionales para `double`, `int` y `long`

La mayoría de las interfaces funcionales **son** para `double`, `int` y `long`. La Tabla 8.6 muestra el equivalente de la Tabla 8.4 para estos primitivos.

**TABLA 8.6** Interfaces funcionales comunes para primitivos

| **Interfaces funcionales** | **Tipo de retorno** | **Método abstracto único** | **# de parámetros** |
|---|---|---|---|
| `DoubleSupplier` `IntSupplier` `LongSupplier` | `double` `int` `long` | `getAsDouble` `getAsInt` `getAsLong` | 0 |
| `DoubleConsumer` `IntConsumer` `LongConsumer` | `void` | `accept` | 1 (double) 1 (int) 1 (long) |
| `DoublePredicate` `IntPredicate` `LongPredicate` | `boolean` | `test` | 1 (double) 1 (int) 1 (long) |
| `DoubleFunction<R>` `IntFunction<R>` `LongFunction<R>` | `R` | `apply` | 1 (double) 1 (int) 1 (long) |
| `DoubleUnaryOperator` `IntUnaryOperator` `LongUnaryOperator` | `double` `int` `long` | `applyAsDouble` `applyAsInt` `applyAsLong` | 1 (double) 1 (int) 1 (long) |
| `DoubleBinaryOperator` `IntBinaryOperator` `LongBinaryOperator` | `double` `int` `long` | `applyAsDouble` `applyAsInt` `applyAsLong` | 2 (double, double) 2 (int, int) 2 (long, long) |

Hay algunas diferencias que **se deben notar** entre la Tabla 8.4 y la Tabla 8.6.

- Los genéricos **desaparecen** de algunas de las interfaces, y en su lugar el nombre del tipo **indica** qué tipo primitivo está involucrado. En otros casos, como `IntFunction`, solo **se necesita** el genérico del tipo de retorno porque **se está** convirtiendo un primitivo `int` en un objeto.
- El método abstracto único **a menudo se renombra** cuando **se devuelve** un tipo primitivo.

Además de los equivalentes de la Tabla 8.4, algunas interfaces **son** específicas de los primitivos. La Tabla 8.7 **lista** estas.

**TABLA 8.7** Interfaces funcionales específicas para primitivos

| **Interfaces funcionales** | **Tipo de retorno** | **Método abstracto único** | **# de parámetros** |
|---|---|---|---|
| `ToDoubleFunction<T>` `ToIntFunction<T>` `ToLongFunction<T>` | `double` `int` `long` | `applyAsDouble` `applyAsInt` `applyAsLong` | 1 (T) |
| `ToDoubleBiFunction<T, U>` `ToIntBiFunction<T, U>` `ToLongBiFunction<T, U>` | `double` `int` `long` | `applyAsDouble` `applyAsInt` `applyAsLong` | 2 (T, U) |
| `DoubleToIntFunction` `DoubleToLongFunction` `IntToDoubleFunction` `IntToLongFunction` `LongToDoubleFunction` `LongToIntFunction` | `int` `long` `double` `long` `double` `int` | `applyAsInt` `applyAsLong` `applyAsDouble` `applyAsLong` `applyAsDouble` `applyAsInt` | 1 (double) 1 (double) 1 (int) 1 (int) 1 (long) 1 (long) |
| `ObjDoubleConsumer<T>` `ObjIntConsumer<T>` `ObjLongConsumer<T>` | `void` | `accept` | 2 (T, double) 2 (T, int) 2 (T, long) |

¿Qué interfaz funcional **se usaría** para rellenar el espacio en blanco para que el siguiente código compile?

```Java
var d = 1.0;
____________ f1 = x -> 1;
f1.applyAsInt(d);
```

Cuando **se ve** una pregunta como esta, **hay que** buscar pistas. **Se puede ver** que la interfaz funcional en cuestión toma un parámetro `double` y devuelve un `int`. También **se puede ver** que tiene un único método abstracto llamado `applyAsInt`. Las interfaces funcionales `DoubleToIntFunction` y `ToIntFunction` cumplen con los tres criterios.
