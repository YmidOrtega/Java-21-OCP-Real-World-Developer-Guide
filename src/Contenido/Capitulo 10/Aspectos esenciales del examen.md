**Escribir código que use `Optional`.** Crear un `Optional` se usa `Optional.empty()` u `Optional.of()`. Para recuperarlo, se suelen usar  `isPresent()` y `get()`. Como alternativa, existen los métodos funcionales  `ifPresent()` y `orElseGet()`.

**Reconocer qué operaciones hacen que un stream pipeline se ejecute.** Las operaciones intermedias no **se ejecutan** hasta que **se encuentre** la operación terminal. Si no hay operación terminal en el pipeline, **se devuelve** un `Stream` pero no **se ejecuta**. Ejemplos de operaciones terminales incluyen `collect()`, `forEach()`, `min()` y `reduce()`.

**Determinar qué operaciones terminales son reducciones.** Las reducciones usan todos los elementos del stream para **determinar** el resultado. Las reducciones que **se necesitan** conocer son `collect()`, `count()`, `max()`, `min()` y `reduce()`. Una reducción mutable **recolecta** en el mismo objeto a medida que avanza. El método `collect()` es una reducción mutable.

**Escribir código para operaciones intermedias comunes.** El método `filter()` devuelve un `Stream<T>` **filtrando** en un `Predicate<T>`. El método `map()` devuelve un `Stream`, **transformando** cada elemento de tipo `T` a otro tipo `R` a través de una `Function <T,R>`. El método `flatMap()` **aplana** streams anidados en un único nivel y **elimina** streams vacíos.

**Comparar streams primitivos con `Stream<T>`.** Los streams primitivos son útiles para **realizar** operaciones comunes en tipos numéricos, incluyendo estadísticas como `average()`, `sum()` y así sucesivamente. Hay tres interfaces de stream primitivo: `DoubleStream`, `IntStream` y `LongStream`. También hay tres clases `Optional` primitivas: `OptionalDouble`, `OptionalInt` y `OptionalLong`. Además de `BooleanSupplier`, todas involucran los primitivos `double`, `int` o `long`.

**Convertir tipos de stream primitivo a otros tipos de stream primitivo.** Normalmente, cuando **se mapea**, simplemente **se llama** al método `map()`. Cuando **se cambia** la interfaz usada para el stream, **se necesita** un método diferente. Para convertir a `Stream`, **se usa** `mapToObj()`. Para convertir a `DoubleStream`, **se usa** `mapToDouble()`. Para convertir a `IntStream`, **se usa** `mapToInt()`. Para convertir a `LongStream`, **se usa** `mapToLong()`.

**Usar `peek()` para inspeccionar el stream.** El método `peek()` es una operación intermedia que a menudo **se usa** para propósitos de depuración. **Ejecuta** una lambda o referencia a método sobre la entrada y **pasa** esa misma entrada a través del pipeline al siguiente operador. Es útil para **imprimir** lo que **pasa** a través de un determinado punto en un stream.

**Buscar en un stream.** Los métodos `findFirst()` y `findAny()` **devuelven** un único elemento de un stream en un `Optional`. Los métodos `anyMatch()`, `allMatch()` y `noneMatch()` **devuelven** un `boolean`. **Hay que tener** cuidado, porque estos tres **se pueden** colgar si **se llaman** en un stream infinito con algunos datos. Todos estos métodos son operaciones terminales.

**Ordenar un stream.** El método `sorted()` es una operación intermedia que **ordena** un stream. Hay dos versiones: la firma con cero parámetros que **ordena** usando el orden de clasificación natural, y la firma con un parámetro que **ordena** usando ese `Comparator` como orden de clasificación.

**Comparar `groupingBy()` y `partitioningBy()`.** El método `groupingBy()` **se usa** en una operación terminal que **crea** un `Map`. Las claves y los tipos de retorno **son determinados** por los parámetros que **se pasen**. Los valores en el `Map` son una `Collection` para todas las entradas que **mapean** a esa clave. El método `partitioningBy()` también **devuelve** un `Map`. Esta vez, las claves son `true` y `false`. Los valores son nuevamente una `Collection` de coincidencias. Si no hay coincidencias para ese `boolean`, la `Collection` está vacía.

---

**Ver también:** [[Arrays]] | [[Algoritmos]]
