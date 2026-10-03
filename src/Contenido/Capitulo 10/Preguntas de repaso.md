Las respuestas a las preguntas de repaso del capítulo **se pueden encontrar** en el Apéndice.

**1.** ¿Cuál podría ser la salida del siguiente código?

```Java
var stream = Stream.iterate("", (s) -> s + "1");
System.out.println(stream.limit(2).map(x -> x + "2"));
```

- [ ] A. 12112
- [ ] B. 212
- [ ] C. 212112
- [ ] D. java.util.stream.ReferencePipeline$3@4517d9a3
- [ ] E. El código no compila.
- [ ] F. Se lanza una excepción.
- [ ] G. El código se cuelga.

---

**2.** ¿Cuál podría ser la salida del siguiente código?

```Java
Predicate<String> predicate = s -> s.startsWith("g");
var stream1 = Stream.generate(() -> "growl!");
var stream2 = Stream.generate(() -> "growl!");
var b1 = stream1.anyMatch(predicate);
var b2 = stream2.allMatch(predicate);
System.out.println(b1 + " " + b2);
```

- [ ] A. true false
- [ ] B. true true
- [ ] C. java.util.stream.ReferencePipeline$3@4517d9a3
- [ ] D. El código no compila.
- [ ] E. Se lanza una excepción.
- [ ] F. El código se cuelga.

---

**3.** ¿Cuál podría ser la salida del siguiente código?

```Java
Predicate<String> predicate = s -> s.length()> 3;
var stream = Stream.iterate("-",
    s -> !s.isEmpty(), (s) -> s + s);
var b1 = stream.noneMatch(predicate);
var b2 = stream.anyMatch(predicate);
System.out.println(b1 + " " + b2);
```

- [ ] A. false false
- [ ] B. false true
- [ ] C. java.util.stream.ReferencePipeline$3@4517d9a3
- [ ] D. El código no compila.
- [ ] E. Se lanza una excepción.
- [ ] F. El código se cuelga.

---

**4.** ¿Cuáles son afirmaciones verdaderas sobre las operaciones terminales en un stream que se ejecuta con éxito? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. Como máximo puede existir una operación terminal en un stream pipeline.
- [ ] B. Las operaciones terminales son una parte requerida del stream pipeline para obtener un resultado.
- [ ] C. Las operaciones terminales tienen `Stream` como tipo de retorno.
- [ ] D. El método `peek()` es un ejemplo de operación terminal.
- [ ] E. El `Stream` referenciado puede ser usado después de llamar a una operación terminal.

---

**5.** ¿Cuáles de los siguientes establecen `result` en `8.0`? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A.
  ```Java
  double result = LongStream.of(6L, 8L, 10L)
          .mapToInt(x -> (int) x)
          .collect(Collectors.groupingBy(x -> x))
          .keySet()
          .stream()
          .collect(Collectors.averagingInt(x -> x));
  ```

- [ ] B.
  ```Java
  double result = LongStream.of(6L, 8L, 10L)
          .mapToInt(x -> x)
          .boxed()
          .collect(Collectors.groupingBy(x -> x))
          .keySet()
          .stream()
          .collect(Collectors.averagingInt(x -> x));
  ```

- [ ] C.
  ```Java
  double result = LongStream.of(6L, 8L, 10L)
          .mapToInt(x -> (int) x)
          .boxed()
          .collect(Collectors.groupingBy(x -> x))
          .keySet()
          .stream()
          .collect(Collectors.averagingInt(x -> x));
  ```

- [ ] D.
  ```Java
  double result = LongStream.of(6L, 8L, 10L)
          .mapToInt(x -> (int) x)
          .collect(Collectors.groupingBy(x -> x, Collectors.toSet()))
          .keySet()
          .stream()
          .collect(Collectors.averagingInt(x -> x));
  ```

- [ ] E.
  ```Java
  double result = LongStream.of(6L, 8L, 10L)
          .mapToInt(x -> x)
          .boxed()
          .collect(Collectors.groupingBy(x -> x, Collectors.toSet()))
          .keySet()
          .stream()
          .collect(Collectors.averagingInt(x -> x));
  ```

- [ ] F.
  ```Java
  double result = LongStream.of(6L, 8L, 10L)
          .mapToInt(x -> (int) x)
          .boxed()
          .collect(Collectors.groupingBy(x -> x, Collectors.toSet()))
          .keySet()
          .stream()
          .collect(Collectors.averagingInt(x -> x));
  ```

---

**6.** ¿Cuál de los siguientes métodos puede llenar el espacio en blanco para que el código imprima `false`?

```Java
var s = Stream.generate(() -> "meow");
var match = s.__________(String::isEmpty);
System.out.println(match);
```

- [ ] A. Solo `allMatch`
- [ ] B. Solo `anyMatch`
- [ ] C. Solo `noneMatch`
- [ ] D. Tanto `allMatch` como `anyMatch`
- [ ] E. Tanto `allMatch` como `noneMatch`
- [ ] F. Ninguno de los anteriores

---

**7.** Se tiene un método que devuelve una lista ordenada sin cambiar la original. Se quiere reescribirlo. ¿Cuál de los siguientes pares puede llenar los espacios en blanco en `refactored()` para hacer lo mismo con streams?

```Java
private static List<String> sort(List<String> list) {
    var copy = new ArrayList<String>(list);
    Collections.sort(copy, (a, b) -> b.compareTo(a));
    return copy;
}

private static List<String> refactored(List<String> list) {
    return list.stream()
        .______((a, b) -> b.compareTo(a))
        .__________;
}
```

- [ ] A. `compare` y `toList()`
- [ ] B. `compare` y `sort()`
- [ ] C. `compareTo` y `toList()`
- [ ] D. `compareTo` y `sort()`
- [ ] E. `sorted` y `collect()`
- [ ] F. `sorted` y `collect(Collectors.toList())`

---

**8.** ¿Cuáles de las siguientes son verdaderas dada esta declaración? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
var is = IntStream.empty();
```

- [ ] A. `is.average()` devuelve el tipo `int`.
- [ ] B. `is.average()` devuelve el tipo `OptionalInt`.
- [ ] C. `is.findAny()` devuelve el tipo `int`.
- [ ] D. `is.findAny()` devuelve el tipo `OptionalInt`.
- [ ] E. `is.sum()` devuelve el tipo `int`.
- [ ] F. `is.sum()` devuelve el tipo `OptionalInt`.

---

**9.** ¿Cuál de los siguientes se puede agregar después de la línea 6 para que el código se ejecute sin error y no produzca ninguna salida? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
4: var stream = LongStream.of(1, 2, 3);
5: var opt = stream.map(n -> n * 10)
6:     .filter(n -> n < 5).findFirst();
```

- [ ] A. `if (opt.isPresent()) System.out.println(opt.get());`
- [ ] B. `if (opt.isPresent()) System.out.println(opt.getAsLong());`
- [ ] C. `opt.ifPresent(System.out.println);`
- [ ] D. `opt.ifPresent(System.out::println);`
- [ ] E. Ninguno de estos; el código no compila.
- [ ] F. Ninguno de estos; la línea 6 lanza una excepción en tiempo de ejecución.

---

**10.** Dadas las cuatro sentencias (L, M, N, O), selecciona el orden que haría que el código produzca 10 líneas.

```Java
Stream.generate(() -> "1")
    L: .filter(x -> x.length()> 1)
    M: .forEach(System.out::println)
    N: .limit(10)
    O: .peek(System.out::println)
;
```

- [ ] A. L, N
- [ ] B. L, N, O
- [ ] C. L, N, M
- [ ] D. L, N, M, O
- [ ] E. L, O, M
- [ ] F. N, M
- [ ] G. N, O

---

**11.** ¿Qué cambios se necesitan hacer juntos para que este código imprima el string `12345`? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
Stream.iterate(1, x -> x++)
    .limit(5).map(x -> x)
    .collect(Collectors.joining());
```

- [ ] A. Cambiar `Collectors.joining()` a `Collectors.joining(",")`
- [ ] B. Cambiar `map(x -> x)` a `map(x -> "" + x)`
- [ ] C. Cambiar `x -> x++` a `x -> ++x`
- [ ] D. Agregar `.forEach(System.out::print)` después de la llamada a `collect()`
- [ ] E. Envolver toda la línea en una sentencia `System.out.print`
- [ ] F. Ninguno de los anteriores; el código ya imprime `12345`

---

**12.** ¿Cuál es verdadero sobre el siguiente código?

```Java
Set<String> birds = Set.of("oriole", "flamingo");
Stream.concat(birds.stream(), birds.stream(), birds.stream())
    .sorted()      // línea X
    .distinct()
    .findAny()
    .ifPresent(System.out::println);
```

- [ ] A. Se garantiza que imprimirá `flamingo` tal como está y cuando se elimine la línea X.
- [ ] B. Se garantiza que imprimirá `oriole` tal como está y cuando se elimine la línea X.
- [ ] C. Se garantiza que imprimirá `flamingo` tal como está, pero no cuando se elimine la línea X.
- [ ] D. Se garantiza que imprimirá `oriole` tal como está, pero no cuando se elimine la línea X.
- [ ] E. La salida puede variar tal como está.
- [ ] F. El código no compila.
- [ ] G. Lanza una excepción porque se usa la misma lista como fuente para múltiples streams.

---

**13.** ¿Cuál de los siguientes es verdadero?

```Java
List<Integer> x1 = List.of(1, 2, 3);
List<Integer> x2 = List.of(4, 5, 6);
List<Integer> x3 = List.of();
Stream.of(x1, x2, x3).map(x -> x + 1)
    .flatMap(x -> x.stream())
    .forEach(System.out::print);
```

- [ ] A. El código compila e imprime `123456`.
- [ ] B. El código compila e imprime `234567`.
- [ ] C. El código compila pero no imprime nada.
- [ ] D. El código compila pero imprime referencias de stream.
- [ ] E. El código se ejecuta infinitamente.
- [ ] F. El código no compila.
- [ ] G. El código lanza una excepción.

---

**14.** ¿Cuáles de los siguientes son verdaderos? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
4: Stream<Integer> s = Stream.of(1);
5: IntStream is = s.boxed();
6: DoubleStream ds = s.mapToDouble(x -> x);
7: Stream<Integer> s2 = ds.mapToInt(x -> x);
8: s2.forEach(System.out::print);
```

- [ ] A. La línea 4 causa un error de compilador.
- [ ] B. La línea 5 causa un error de compilador.
- [ ] C. La línea 6 causa un error de compilador.
- [ ] D. La línea 7 causa un error de compilador.
- [ ] E. La línea 8 causa un error de compilador.
- [ ] F. El código compila pero lanza una excepción en tiempo de ejecución.
- [ ] G. El código compila e imprime `1`.

---

**15.** Dado el tipo genérico `String`, el recopilador `partitioningBy()` crea un `Map<Boolean, List<String>>` cuando se pasa a `collect()` por defecto. Cuando se pasa un recopilador _downstream_ a `partitioningBy()`, ¿qué tipos de retorno se pueden crear? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. `Map<boolean, List<String>>`
- [ ] B. `Map<Boolean, List<String>>`
- [ ] C. `Map<Boolean, Map<String>>`
- [ ] D. `Map<Boolean, Set<String>>`
- [ ] E. `Map<Long, TreeSet<String>>`
- [ ] F. Ninguno de los anteriores

---

**16.** ¿Cuáles de las siguientes afirmaciones son verdaderas sobre este código? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
20: Predicate<String> empty = String::isEmpty;
21: Predicate<String> notEmpty = empty.negate();
22:
23: var result = Stream.generate(() -> "")
24:     .limit(10)
25:     .filter(notEmpty)
26:     .collect(Collectors.groupingBy(k -> k))
27:     .entrySet()
28:     .stream()
29:     .map(Entry::getValue)
30:     .flatMap(Collection::stream)
31:     .collect(Collectors.partitioningBy(notEmpty));
32: System.out.println(result);
```

- [ ] A. Produce `{}`.
- [ ] B. Produce `{false=[], true=[]}`.
- [ ] C. Si se cambia la línea 31 de `partitioningBy(notEmpty)` a `groupingBy(n -> n)`, produciría `{}`.
- [ ] D. Si se cambia la línea 31 de `partitioningBy(notEmpty)` a `groupingBy(n -> n)`, produciría `{false=[], true=[]}`.
- [ ] E. El código no compila.
- [ ] F. El código compila pero no termina en tiempo de ejecución.

---

**17.** ¿Cuál es el resultado del siguiente código?

```Java
var s = DoubleStream.of(1.2, 2.4);
s.peek(System.out::println).filter(x -> x> 2).count();
```

- [ ] A. 1
- [ ] B. 2
- [ ] C. 2.4
- [ ] D. 1.2 y 2.4
- [ ] E. No hay salida.
- [ ] F. El código no compila.
- [ ] G. Se lanza una excepción.

---

**18.** ¿Cuál es la salida del siguiente código?

```Java
11: public class Paging {
12:     record Sesame(String name, boolean human) {
13:         @Override public String toString() {
14:             return name();
15:         }
16:     }
17:     record Page(List<Sesame> list, long count)  {}
18:
19:     public static void main(String[] args) {
20:         var monsters = Stream.of(new Sesame("Elmo", false));
21:         var people = Stream.of(new Sesame("Abby", true));
22:         printPage(monsters, people);
23:     }
24:
25:     private static void printPage(Stream<Sesame> monsters,
26:             Stream<Sesame> people) {
27:         Page page = Stream.concat(monsters, people)
28:             .collect(Collectors.teeing(
29:                 Collectors.filtering(s -> s.name().startsWith("E"),
30:                     Collectors.toList()),
31:                 Collectors.counting(),
32:                 (l, c) -> new Page(l, c)));
33:         System.out.println(page);
34:     } }
```

- [ ] A. `Page[list=[Abby], count=1]`
- [ ] B. `Page[list=[Abby], count=2]`
- [ ] C. `Page[list=[Elmo], count=1]`
- [ ] D. `Page[list=[Elmo], count=2]`
- [ ] E. El código no compila debido a `Stream.concat()`.
- [ ] F. El código no compila debido a `Collectors.teeing()`.
- [ ] G. El código no compila por otra razón.

---

**19.** ¿Cuál es la forma más simple de reescribir este código?

```Java
List<Integer> x = IntStream.range(1, 6)
    .mapToObj(i -> i)
    .collect(Collectors.toList());
x.forEach(System.out::println);
```

- [ ] A. `IntStream.range(1, 6);`
- [ ] B. `IntStream.range(1, 6) .forEach(System.out::println);`
- [ ] C. `IntStream.range(1, 6) .mapToObj(i -> i) .forEach(System.out::println);`
- [ ] D. Ninguno de los anteriores es equivalente.
- [ ] E. El código proporcionado no compila.

---

**20.** ¿Cuáles de los siguientes lanzan una excepción cuando un `Optional` está vacío? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. `opt.orElse("");`
- [ ] B. `opt.orElseGet(() -> "");`
- [ ] C. `opt.orElseThrow();`
- [ ] D. `opt.orElseThrow(() -> throw new Exception());`
- [ ] E. `opt.orElseThrow(RuntimeException::new);`
- [ ] F. `opt.get();`
- [ ] G. `opt.get("");`

---

**21.** ¿Cuál es la salida del siguiente código?

```Java
var spliterator = Stream.generate(() -> "x")
    .spliterator();

spliterator.tryAdvance(System.out::print);
var split = spliterator.trySplit();
split.tryAdvance(System.out::print);
```

- [ ] A. x
- [ ] B. xx
- [ ] C. Una larga lista de x's.
- [ ] D. No hay salida.
- [ ] E. El código no compila.
- [ ] F. El código compila pero no termina en tiempo de ejecución.

---

**Ver también:** [[Arrays]] | [[Algoritmos]]
