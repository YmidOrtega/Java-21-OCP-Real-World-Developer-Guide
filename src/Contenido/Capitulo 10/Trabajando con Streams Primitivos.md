Hasta ahora, todos los streams que **se han creado** usaron la interfaz `Stream` con un tipo genérico, como `Stream<String>`, `Stream<Integer>`, y así sucesivamente. Para valores numéricos, **se han estado** usando clases envolventes (_wrapper classes_). Esto **se hizo** con la API de `Collections` en el Capítulo 9, por lo que debería **sentirse** natural.

Java en realidad incluye otras interfaces de stream además de `Stream` que **se pueden** usar para trabajar con tipos primitivos seleccionados: `int`, `double` y `long`. **Se observa** por qué es necesario. Supongamos que **se quiere** calcular la suma de números en un stream finito:

```Java
Stream<Integer> stream = Stream.of(1, 2, 3);
System.out.println(stream.reduce(0, (s, n) -> s + n)); // 6
```

No está mal. No fue difícil **escribir** una reducción. **Se comenzó** el acumulador en cero. Luego **se agregó** cada número al total acumulado a medida que **aparecía** en el stream. Hay otra forma de hacer esto, que **se muestra** aquí:

```Java
Stream<Integer> stream = Stream.of(1, 2, 3);
System.out.println(stream.mapToInt(x -> x).sum()); // 6
```

Esta vez, **se convirtió** el `Stream<Integer>` a un `IntStream` y se le **pidió** al `IntStream` que **calculara** la suma. Un `IntStream` tiene muchos de los mismos métodos intermedios y terminales que un `Stream`, pero incluye métodos especializados para trabajar con datos numéricos. Los streams primitivos saben cómo **realizar** ciertas operaciones comunes de forma automática.

Es aún más útil para operaciones que son más trabajo calcular, como el promedio:

```Java
IntStream intStream = IntStream.of(1, 2, 3);
OptionalDouble avg = intStream.average();
System.out.println(avg.getAsDouble()); // 2.0
```

No solo es posible **calcular** el promedio, sino que también es fácil de hacer. Claramente, los streams primitivos son importantes. **Se observa** cómo **crear** y **usar** dichos streams, incluyendo los opcionales y las interfaces funcionales.

#### Creando Streams Primitivos

Hay tres tipos de streams primitivos:

- **`IntStream`:** **Se usa** para los tipos primitivos `int`, `short`, `byte` y `char`.
- **`LongStream`:** **Se usa** para el tipo primitivo `long`.
- **`DoubleStream`:** **Se usa** para los tipos primitivos `double` y `float`.

¿Por qué no cada tipo primitivo tiene su propio stream? Estos tres son los más comunes, por lo que los diseñadores de la API **se quedaron** con ellos.

> Cuando **se ve** la palabra _stream_ en el examen, **hay que prestar** atención a las mayúsculas. Con una _S_ mayúscula o en código, `Stream` es el nombre de la interfaz que contiene un tipo `Object`. Con una _s_ minúscula, un stream es un concepto que podría ser un `Stream`, `DoubleStream`, `IntStream` o `LongStream`.

La Tabla 10.5 muestra algunos de los métodos que son _únicos_ para los streams primitivos. **Hay que notar** que no **se incluyen** métodos en la tabla como `empty()` que **se conocen** de la interfaz `Stream`.

**TABLA 10.5** Métodos comunes de stream primitivo

| **Método** | **Stream primitivo** | **Descripción** |
|---|---|---|
| `OptionalDouble average()` | `IntStream` `LongStream` `DoubleStream` | Media aritmética de los elementos |
| `Stream<T> boxed()` | `IntStream` `LongStream` `DoubleStream` | `Stream<T>` donde `T` es la clase envolvente asociada con el valor primitivo |
| `OptionalInt max()` | `IntStream` | Elemento máximo del stream |
| `OptionalLong max()` | `LongStream` | Elemento máximo del stream |
| `OptionalDouble max()` | `DoubleStream` | Elemento máximo del stream |
| `OptionalInt min()` | `IntStream` | Elemento mínimo del stream |
| `OptionalLong min()` | `LongStream` | Elemento mínimo del stream |
| `OptionalDouble min()` | `DoubleStream` | Elemento mínimo del stream |
| `IntStream range(int a, int b)` | `IntStream` | Devuelve stream primitivo de `a` (inclusivo) a `b` (exclusivo) |
| `LongStream range(long a, long b)` | `LongStream` | Devuelve stream primitivo de `a` (inclusivo) a `b` (exclusivo) |
| `IntStream rangeClosed(int a, int b)` | `IntStream` | Devuelve stream primitivo de `a` (inclusivo) a `b` (inclusivo) |
| `LongStream rangeClosed(long a, long b)` | `LongStream` | Devuelve stream primitivo de `a` (inclusivo) a `b` (inclusivo) |
| `int sum()` | `IntStream` | Devuelve la suma de los elementos en el stream |
| `long sum()` | `LongStream` | Devuelve la suma de los elementos en el stream |
| `double sum()` | `DoubleStream` | Devuelve la suma de los elementos en el stream |
| `IntSummaryStatistics summaryStatistics()` | `IntStream` | Devuelve objeto que contiene numerosas estadísticas del stream como promedio, min, max, etc. |
| `LongSummaryStatistics summaryStatistics()` | `LongStream` | Devuelve objeto que contiene numerosas estadísticas del stream como promedio, min, max, etc. |
| `DoubleSummaryStatistics summaryStatistics()` | `DoubleStream` | Devuelve objeto que contiene numerosas estadísticas del stream como promedio, min, max, etc. |

Algunos de los métodos para **crear** un stream primitivo son equivalentes a cómo **se creó** la fuente para un `Stream` regular. **Se puede** crear un stream vacío con esto:

```Java
DoubleStream empty = DoubleStream.empty();
```

Otra forma es usar el método de fábrica `of()` desde un único valor o usando la sobrecarga de varargs.

```Java
DoubleStream oneValue = DoubleStream.of(3.14);
oneValue.forEach(System.out::println);

DoubleStream varargs = DoubleStream.of(1.0, 1.1, 1.2);
varargs.forEach(System.out::println);
```

Este código **produce** lo siguiente:

```Plaintext
3.14
1.0
1.1
1.2
```

También **se pueden** usar los dos métodos para **crear** streams infinitos, al igual que **se hizo** con `Stream`.

```Java
var random = DoubleStream.generate(Math::random);
var fractions = DoubleStream.iterate(.5, d -> d / 2);
random.limit(3).forEach(System.out::println);
fractions.limit(3).forEach(System.out::println);
```

Dado que los streams son infinitos, **se agregó** una operación intermedia de `limit` para que la salida no imprima valores para siempre. El primer stream **llama** a un método `static` en `Math` para obtener un `double` aleatorio. Dado que los números son aleatorios, la salida obviamente **será** diferente. El segundo stream sigue **creando** números más pequeños, **dividiendo** el valor anterior por dos cada vez. La salida de cuando **se ejecutó** este código fue la siguiente:

```Plaintext
0.07890654781186413
0.28564363465842346
0.6311403511266134
0.5
0.25
0.125
```

> No **se necesita** saber esto para el examen, pero la clase `Random` proporciona un método para **obtener** streams primitivos de números aleatorios directamente. ¡Dato curioso! Por ejemplo, `ints()` genera un `IntStream` infinito de primitivos.

Cuando **se trabaja** con primitivos `int` o `long`, es común **contar**. Supongamos que **se quisiera** un stream con los números del 1 al 5. **Se podría** escribir esto usando lo que **se ha explicado** hasta ahora:

```Java
IntStream count = IntStream.iterate(1, n -> n + 1).limit(5);
count.forEach(System.out::print); // 12345
```

Este código **imprime** los números del 1 al 5. Sin embargo, es mucho código para hacer algo tan simple. Java proporciona un método que puede **generar** un rango de números.

```Java
IntStream range = IntStream.range(1, 6);
range.forEach(System.out::print); // 12345
```

Esto está mejor. Si **se quisieran** los números del 1 al 5, ¿por qué **se pasó** del 1 al 6? El primer parámetro del método `range()` es _inclusivo_, lo que significa que **incluye** el número. El segundo parámetro del método `range()` es _exclusivo_, lo que significa que **se detiene** justo antes de ese número. Sin embargo, todavía podría **ser** más claro. **Se quieren** los números del 1 al 5 inclusive. Afortunadamente, hay otro método, `rangeClosed()`, que es inclusivo en ambos parámetros.

```Java
IntStream rangeClosed = IntStream.rangeClosed(1, 5);
rangeClosed.forEach(System.out::print); // 12345
```

Aún mejor. Esta vez **se expresó** que **se quiere** un rango cerrado o un rango inclusivo. Este método **coincide** mejor con cómo **se expresa** un rango de números en inglés simple.

#### Mapeando Streams

Otra forma de **crear** un stream primitivo es **mapeando** desde otro tipo de stream. La Tabla 10.6 muestra que hay un método para **mapear** entre cualquier tipo de stream.

**TABLA 10.6** Métodos de mapeo entre tipos de streams

| **Stream fuente** | **Para crear `Stream`** | **Para crear `DoubleStream`** | **Para crear `IntStream`** | **Para crear `LongStream`** |
|---|---|---|---|---|
| `Stream<T>` | `map()` | `mapToDouble()` | `mapToInt()` | `mapToLong()` |
| `DoubleStream` | `mapToObj()` | `map()` | `mapToInt()` | `mapToLong()` |
| `IntStream` | `mapToObj()` | `mapToDouble()` | `map()` | `mapToLong()` |
| `LongStream` | `mapToObj()` | `mapToDouble()` | `mapToInt()` | `map()` |

Obviamente, tienen que ser tipos compatibles para que esto funcione. Java requiere que **se proporcione** una función de mapeo como parámetro, por ejemplo:

```Java
Stream<String> objStream = Stream.of("penguin", "fish");
IntStream intStream = objStream.mapToInt(s -> s.length());
```

Esta función **toma** un `Object`, que es un `String` en este caso. La función **devuelve** un `int`. Los mapeos de funciones son intuitivos aquí. **Toman** el tipo de stream fuente y **devuelven** el tipo objetivo. En este ejemplo, el tipo de función real es `ToIntFunction`. La Tabla 10.7 muestra los nombres de las funciones de mapeo. Como **se puede** ver, hacen lo que **se esperaría**.

**TABLA 10.7** Parámetros de función al mapear entre tipos de streams

| **Stream fuente** | **Para crear `Stream`** | **Para crear `DoubleStream`** | **Para crear `IntStream`** | **Para crear `LongStream`** |
| ----------------- | ----------------------- | ----------------------------- | -------------------------- | --------------------------- |
| `Stream<T>`       | `Function<T,R>`         | `ToDoubleFunction<T>`         | `ToIntFunction<T>`         | `ToLongFunction<T>`         |
| `DoubleStream`    | `DoubleFunction<R>`     | `DoubleUnaryOperator`         | `DoubleToIntFunction`      | `DoubleToLongFunction`      |
| `IntStream`       | `IntFunction<R>`        | `IntToDoubleFunction`         | `IntUnaryOperator`         | `IntToLongFunction`         |
| `LongStream`      | `LongFunction<R>`       | `LongToDoubleFunction`        | `LongToIntFunction`        | `LongUnaryOperator`         |

**Se deben** memorizar las Tablas 10.6 y 10.7. No es tan difícil como puede parecer. Hay patrones en los nombres si **se recuerdan** algunas reglas. Para la Tabla 10.6, **mapear** al mismo tipo que **se empezó** simplemente **se llama** `map()`. Cuando **se devuelve** un stream de objetos, el método es `mapToObj()`. Más allá de eso, es el nombre del tipo primitivo en el nombre del método de mapeo.

Para la Tabla 10.7, **se puede** comenzar pensando en los tipos de origen y destino. Cuando el tipo objetivo es un objeto, **se elimina** el `To` del nombre. Cuando el mapeo es al mismo tipo con el que **se comenzó**, **se usa** un operador unario en lugar de una función para los streams primitivos.

> **Usando *flatMap()***
>
> **Se puede** usar este enfoque en streams primitivos también. Funciona de la misma manera que en un `Stream` regular, excepto que el nombre del método es diferente. Aquí hay un ejemplo:
>
> ```Java
> var integerList = new ArrayList<Integer>();
> IntStream ints = integerList.stream()
>     .flatMapToInt(x -> IntStream.of(x));
> DoubleStream doubles = integerList.stream()
>     .flatMapToDouble(x -> DoubleStream.of(x));
> LongStream longs = integerList.stream()
>     .flatMapToLong(x -> LongStream.of(x));
> ```

Adicionalmente, **se puede** crear un `Stream` a partir de un stream primitivo. Estos métodos muestran dos formas de lograr esto:

```Java
private static Stream<Integer> mapping(IntStream stream) {
    return stream.mapToObj(x -> x);
}

private static Stream<Integer> boxing(IntStream stream) {
    return stream.boxed();
}
```

El primero usa el método `mapToObj()` que **se vio** anteriormente. El segundo es más sucinto. No requiere una función de mapeo porque todo lo que hace es **autoboxear** cada primitivo al objeto envolvente correspondiente. El método `boxed()` existe en los tres tipos de streams primitivos.

#### Usando *Optional* con Streams Primitivos

Anteriormente en el capítulo, **se escribió** un método para calcular el promedio de un arreglo `int[]` y **se prometió** una mejor manera más adelante. Ahora que **se conocen** los streams primitivos, **se puede** calcular el promedio en una línea.

```Java
var stream = IntStream.rangeClosed(1,10);
OptionalDouble optional = stream.average();
```

El tipo de retorno no es el `Optional` al que **se ha acostumbrado**. Es un nuevo tipo llamado `OptionalDouble`. ¿Por qué **se tiene** un tipo separado? ¿Por qué no simplemente usar `Optional<Double>`? La diferencia es que `OptionalDouble` es para un primitivo y `Optional<Double>` es para la clase envolvente `Double`. Trabajar con la clase opcional primitiva **se parece** a la propia clase `Optional`.

```Java
optional.ifPresent(System.out::println);            // 5.5
System.out.println(optional.getAsDouble());         // 5.5
System.out.println(optional.orElseGet(() -> Double.NaN)); // 5.5
```

La única diferencia notable es que **se llama** a `getAsDouble()` en lugar de `get()`. Esto deja claro que **se está** trabajando con un primitivo. También, `orElseGet()` toma un `DoubleSupplier` en lugar de un `Supplier`.

Al igual que con los streams primitivos, hay tres clases específicas de tipo para primitivos. La Tabla 10.8 muestra las pequeñas diferencias entre los tres. Probablemente no **se sorprenderá** de que **se deba** memorizar esta tabla también. Esto es realmente fácil de recordar ya que el nombre del primitivo es el único cambio. Como **se debería** recordar de la sección de operaciones terminales, varios métodos de stream **devuelven** un opcional como `min()` o `findAny()`. Cada uno de estos **devuelve** el tipo opcional correspondiente. Las implementaciones de stream primitivo también **agregan** dos nuevos métodos que **se necesitan** conocer. El método `sum()` no devuelve un opcional. Si **se intenta** sumar un stream vacío, simplemente **se obtiene** cero. El método `average()` siempre **devuelve** un `OptionalDouble` ya que el promedio puede tener potencialmente datos fraccionarios para cualquier tipo.

**TABLA 10.8** Tipos opcionales para primitivos

|                                      | **`OptionalDouble`** | **`OptionalInt`** | **`OptionalLong`** |
| ------------------------------------ | -------------------- | ----------------- | ------------------ |
| Obtener como primitivo               | `getAsDouble()`      | `getAsInt()`      | `getAsLong()`      |
| Tipo de parámetro de `orElseGet()`   | `DoubleSupplier`     | `IntSupplier`     | `LongSupplier`     |
| Tipo de retorno de `max()` y `min()` | `OptionalDouble`     | `OptionalInt`     | `OptionalLong`     |
| Tipo de retorno de `sum()`           | `double`             | `int`             | `long`             |
| Tipo de retorno de `average()`       | `OptionalDouble`     | `OptionalDouble`  | `OptionalDouble`   |

**Se intenta** un ejemplo para asegurarse de **entender** esto:

```Java
5: LongStream longs = LongStream.of(5, 10);
6: long sum = longs.sum();
7: System.out.println(sum);    // 15
8: DoubleStream doubles = DoubleStream.generate(() -> Math.PI);
9: OptionalDouble min = doubles.min(); // corre infinitamente
```

La línea 5 **crea** un stream de primitivos `long` con dos elementos. La línea 6 muestra que no **se usa** un opcional para la suma. La línea 8 **crea** un stream infinito de primitivos `double`. La línea 9 está ahí para recordar que una pregunta sobre código que se ejecuta infinitamente también puede aparecer con streams primitivos.

#### Estadísticas de Resumen

**Se ha aprendido** lo suficiente para poder obtener el valor máximo de un stream de primitivos `int`. Si el stream está vacío, **se quiere** lanzar una excepción.

```Java
private static int max(IntStream ints) {
    OptionalInt optional = ints.max();
    return optional.orElseThrow(RuntimeException::new);
}
```

Esto debería ser de conocimiento común a estas alturas. **Se obtiene** un `OptionalInt` porque **se tiene** un `IntStream`. Si el opcional contiene un valor, **se devuelve**. De lo contrario, **se lanza** una nueva `RuntimeException`.

Ahora **se quiere** cambiar el método para tomar un `IntStream` y **devolver** un rango. El rango es el valor mínimo **restado** del valor máximo. Uh-oh. Tanto `min()` como `max()` son operaciones terminales, lo que significa que **consumen** el stream cuando **se ejecutan**. No **se pueden** ejecutar dos operaciones terminales contra el mismo stream. Afortunadamente, este es un problema común, y los streams primitivos **lo resuelven** con estadísticas de resumen. _Statistic_ (_estadística_) es simplemente una palabra elegante para un número que **fue calculado** a partir de datos.

```Java
private static int range(IntStream ints) {
    IntSummaryStatistics stats = ints.summaryStatistics();
    if (stats.getCount() == 0) throw new RuntimeException();
    return stats.getMax() - stats.getMin();
}
```

Aquí **se le pidió** a Java que **realizara** muchos cálculos sobre el stream. Las estadísticas de resumen incluyen lo siguiente:

- **`getCount()`:** **Devuelve** un `long` que representa el número de valores.
- **`getAverage()`:** **Devuelve** un `double` que representa el promedio. Si el stream está vacío, **devuelve** `0.0`.
- **`getSum()`:** **Devuelve** la suma como un `double` para `DoubleSummaryStatistics`, y como `long` para `IntSummaryStatistics` y `LongSummaryStatistics`.
- **`getMin()`:** **Devuelve** el número más pequeño (mínimo) como `double`, `int` o `long`, dependiendo del tipo del stream. Si el stream está vacío, **devuelve** el valor numérico más grande basado en el tipo.
- **`getMax()`:** **Devuelve** el número más grande (máximo) como `double`, `int` o `long` dependiendo del tipo del stream. Si el stream está vacío, **devuelve** el valor numérico más pequeño basado en el tipo.

---

**Ver también:** [[Arrays]] | [[Algoritmos]]
