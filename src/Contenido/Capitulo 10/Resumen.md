Un `Optional<T>` puede estar vacío o **almacenar** un valor. **Se puede** verificar si contiene un valor con `isPresent()` y **obtener** el valor dentro con `get()`. **Se puede** devolver un valor diferente con `orElse(T t)` o **lanzar** una excepción con `orElseThrow()`. Hay incluso tres métodos que toman interfaces funcionales como parámetros: `ifPresent(Consumer c)`, `orElseGet(Supplier s)` y `orElseThrow(Supplier s)`. Hay tres tipos de `Optional` para primitivos: `OptionalDouble`, `OptionalInt` y `OptionalLong`. Estos tienen los métodos `getAsDouble()`, `getAsInt()` y `getAsLong()`, respectivamente.

Un stream pipeline tiene tres partes. La fuente es requerida, y **crea** los datos en el stream. Puede haber cero o más operaciones intermedias, que no **se ejecutan** hasta que **se ejecute** la operación terminal. La primera interfaz de stream que **se cubrió** fue `Stream<T>`, que toma un argumento genérico `T`. La interfaz `Stream<T>` incluye muchas operaciones intermedias útiles incluyendo `filter()`, `map()`, `flatMap()` y `sorted()`. Ejemplos de operaciones terminales incluyen `allMatch()`, `count()` y `forEach()`.

Además de la interfaz `Stream<T>`, hay tres streams primitivos: `DoubleStream`, `IntStream` y `LongStream`. Además de los métodos habituales de `Stream<T>`, `IntStream` y `LongStream` tienen `range()` y `rangeClosed()`. La llamada `range(1, 10)` en `IntStream` y `LongStream` **crea** un stream de los primitivos del 1 al 9. Por contraste, `rangeClosed(1, 10)` **crea** un stream de los primitivos del 1 al 10. Los streams primitivos tienen operaciones matemáticas que incluyen `average()`, `max()` y `sum()`. También tienen `summaryStatistics()` para **obtener** muchas estadísticas en una sola llamada.

**Se puede** usar un `Collector` para **transformar** un stream en una colección tradicional. Incluso **se pueden** agrupar campos para **crear** un mapa complejo en una sola línea. La partición funciona igual que la agrupación, excepto que las claves son siempre `true` y `false`. Un mapa particionado siempre tiene dos claves, incluso si el valor está vacío para la clave. Un recopilador de _teeing_ permite **combinar** los resultados de dos recopiladores.

**Se deben** memorizar las Tablas 10.6 y 10.7. Como mínimo, **hay que** poder **detectar** incompatibilidades, como diferencias de tipo. Finalmente, **hay que recordar** que los streams **se evalúan** perezosamente. Toman lambdas o referencias a métodos como parámetros, que **se ejecutan** más tarde cuando **se ejecuta** el método.

---

**Ver también:** [[Arrays]] | [[Algoritmos]]
