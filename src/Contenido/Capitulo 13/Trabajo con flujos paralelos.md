Este capítulo **concluye combinando** lo que aprendiste en el Capítulo 10, "Streams", con los conceptos vistos en este capítulo. Una de las funciones más potentes de la Stream API **es** el soporte de concurrencia integrado. Hasta ahora, todos los streams con los que has trabajado han sido streams seriales. Un **stream serial** **es** un stream en el que los resultados están ordenados, procesándose una sola entrada a la vez.

Un **stream paralelo** **es capaz de procesar** resultados de forma concurrente, usando múltiples hilos. Por ejemplo, puedes usar un stream paralelo y la operación `map()` para operar de forma concurrente sobre los elementos del stream, mejorando enormemente el rendimiento en comparación con procesar un solo elemento a la vez. **Usar** un stream paralelo **puede cambiar** no solo el rendimiento de tu aplicación sino también los resultados esperados. Como verás, algunas operaciones también requieren un manejo especial para poder procesarse de forma paralela.

> **Generación de números aleatorios**
>
> En el Capítulo 4, "APIs Principales", aprendiste a generar números aleatorios para un programa de un solo hilo. Para generar números aleatorios en un programa multihilo, en su lugar **se usa** la clase `ThreadLocalRandom`. Para empezar, **se obtiene** una instancia usando `current()`. Luego **se tienen disponibles** todos los métodos de instancia.
>
> ```Java
> ThreadLocalRandom.current()
>    .ints()
>    .limit(5)
>    .forEach(System.out::println);        // Muestra 5 números enteros aleatorios
> ```
>
> Para trabajar con `ints` **hay** seis métodos disponibles:
>
> - `ints()`: stream infinito de valores `int` cualesquiera
> - `ints(lowestInclusive, highestExclusive)`: stream ilimitado de valores `int` entre los dos parámetros, excluyendo el segundo
> - `ints(numberIntsToInclude)`: stream finito del número solicitado de valores `int`
> - `ints(numberIntsToInclude, lowestInclusive, highestExclusive)`: stream finito del número solicitado de valores `int` entre los dos parámetros, excluyendo el segundo
> - `nextInt(highestExclusive)`: un solo `int` entre 0 y el parámetro, sin incluir el parámetro
> - `nextInt(lowestInclusive, highestExclusive)`: un solo `int` entre el primer parámetro y el segundo parámetro, sin incluir el segundo parámetro
>
> Para los `double`s y los `long`s **se dispone** de métodos similares, como `doubles()`, `nextDouble()`, `longs()`, `nextLong()`, etc.

### Creando streams paralelos

La Stream API **fue diseñada** para que crear streams paralelos sea bastante fácil. Para el examen, **deberías** familiarizarte con dos formas de crear un stream paralelo.

```Java
Collection<Integer> collection = List.of(1, 2);
Stream<Integer> p1 = collection.stream().parallel();
Stream<Integer> p2 = collection.parallelStream();
```

La primera forma de crear un stream paralelo **es a partir de** un stream existente. ¿No es genial? ¡Cualquier stream **puede volverse** paralelo! La segunda forma de crear un stream paralelo **es a partir de** una clase `Collection` de Java. **Usamos** ambos métodos a lo largo de esta sección.

> **Nota:** La interfaz `Stream` incluye un método `isParallel()` que **se puede** usar para determinar si la instancia de un stream soporta procesamiento paralelo. Algunas operaciones sobre streams conservan el atributo paralelo, mientras que otras no.

### Realizando una descomposición paralela

Una **descomposición paralela** **es** el proceso de tomar una tarea, dividirla en piezas más pequeñas que puedan realizarse de forma concurrente y luego volver a ensamblar los resultados. Cuanto más concurrente sea una descomposición, mayor será la mejora de rendimiento del uso de streams paralelos.

**Vamos a probarlo**. Primero, **definamos** una función reutilizable que "hace trabajo" solo esperando cinco segundos.

```Java
private static int doWork(int input) {
   try {
      Thread.sleep(5000);
   } catch (InterruptedException e) {}
   return input;
}
```

**Podemos suponer** que en una aplicación real este trabajo podría implicar llamar a una base de datos o leer un archivo. Ahora **usemos** este método con un stream serial.

```Java
10: long start = System.currentTimeMillis();
11: List.of(1, 2, 3, 4, 5)
12:    .stream()
13:    .map(w -> doWork(w))
14:    .forEach(s -> System.out.print(s + " "));
15:
16: System.out.println();
17: var timeTaken = (System.currentTimeMillis()-start)/1000;
18: System.out.println("Time: " + timeTaken + " seconds");
```

¿Qué crees que producirá este código cuando se ejecute como parte de un método `main()`? **Veamos**:

```
1 2 3 4 5
Time: 25 seconds
```

Como podrías esperar, los resultados están ordenados y son predecibles porque estamos usando un stream serial. Además, **se tardó** alrededor de 25 segundos en procesar los cinco resultados, uno a la vez. ¿Qué pasa si reemplazamos la línea 12 por una que use `parallelStream()`? La siguiente **es** una salida de ejemplo:

```
3 2 1 5 4
Time: 5 seconds
```

Como puedes ver, los resultados ya no están ordenados ni son predecibles. Las operaciones `map()` y `forEach()` en un stream paralelo **son equivalentes a enviar** múltiples expresiones lambda `Runnable` a un `executor` con grupo de hilos (_thread pool_) y luego esperar los resultados.

¿Y en cuanto al tiempo requerido? En este caso, nuestro sistema tenía suficientes CPUs para que todas las tareas se ejecutaran de forma concurrente. Si ejecutaras este mismo código en una computadora con menos procesadores, podría producir 10 segundos, 15 segundos o algún otro valor. Lo importante **es** que hemos escrito nuestro código para aprovechar el procesamiento paralelo cuando esté disponible, así que nuestro trabajo está hecho.

> **Ordenación de los resultados**
>
> Si tu operación de stream necesita garantizar el orden y no estás seguro de si es serial o paralelo, **puedes** reemplazar la línea 14 por una que use `forEachOrdered()`:
>
> ```Java
> 14:    .forEachOrdered(s -> System.out.print(s + " "));
> ```
>
> Esto **produce** los resultados en el orden en que están definidos en el stream:
>
> ```
> 1 2 3 4 5
> Time: 5 seconds
> ```
>
> Si bien hemos perdido algunas de las ganancias de rendimiento de usar un stream paralelo, nuestra operación `map()` todavía puede aprovechar el stream paralelo.

### Procesando reducciones paralelas

Además de mejorar potencialmente el rendimiento y modificar el orden de las operaciones, el uso de streams paralelos **puede influir** en la forma en que escribes tu aplicación. Una **reducción paralela** **es** una operación de reducción aplicada a un stream paralelo. Los resultados de las reducciones paralelas pueden diferir de lo que esperas al trabajar con streams seriales.

#### Realizando tareas basadas en el orden

Ya que con los streams paralelos el orden no está garantizado, métodos como `findAny()` en streams paralelos **pueden resultar** en comportamiento inesperado. **Considera** el siguiente ejemplo:

```Java
System.out.print(List.of(1, 2, 3, 4, 5, 6)
   .parallelStream()
   .findAny()
   .get());
```

La JVM **asigna** un número de hilos y **retorna** el valor del primero en producir un resultado, el cual podría ser 4, 2, y así sucesivamente. Si bien no hay garantía de que ni el stream serial ni el paralelo retornen el primer valor, el stream serial a menudo lo hace. Con un stream paralelo, los resultados probablemente sean más aleatorios.

¿Y las operaciones que consideran el orden, como `findFirst()`, `limit()` y `skip()`? El orden todavía se conserva, pero el rendimiento puede verse afectado en un stream paralelo como resultado de que una tarea de procesamiento paralelo se ve forzada a coordinar todos sus hilos de una manera similar a la sincronización.

> **Ordenación de un stream paralelo**
>
> **Es posible ordenar** un stream paralelo, si bien los resultados podrían no ser los que esperas. Lo siguiente **imprime** los números del 1 al 99 en un orden estocástico, es decir, aleatorio:
>
> ```Java
> IntStream.range(1,100).parallel().sorted().forEach(System.out::println);
> ```
>
> Después de la llamada a `sorted()`, el stream todavía **se considera** paralelo, lo que hace que el método `forEach()` imprima los valores en un orden estocástico. Recuerda usar un método con orden, como `forEachOrdered()`, si necesitas garantizar el orden en un stream paralelo.

Por otro lado, los resultados de las operaciones con orden en un stream paralelo serán consistentes con los de un stream serial. Por ejemplo, llamar a `skip(5).limit(2).findFirst()` **retornará** el mismo resultado en streams seriales y paralelos con orden.

> **Escenario del mundo real: Creación de streams sin orden**
>
> Todos los streams con los que has estado trabajando **se consideran** con orden de forma predeterminada. **Es posible** crear un stream sin orden a partir de un stream con orden, similar a como creas un stream paralelo a partir de un stream serial.
>
> ```Java
> List.of(1, 2, 3, 4, 5, 6).stream().unordered();
> ```
>
> Este método no reordena los elementos; solo le indica a la JVM que, si **se aplica** una operación de stream basada en el orden, el orden puede ignorarse. Por ejemplo, llamar a `skip(5)` en un stream sin orden omitirá 5 elementos cualesquiera, no necesariamente los primeros 5 que **se requieren** en un stream con orden. Para los streams seriales, usar una versión sin orden no tiene ningún efecto. Pero en los streams paralelos, los resultados pueden mejorar enormemente el rendimiento.
>
> ```Java
> List.of(1, 2, 3, 4, 5, 6).stream().unordered().parallel();
> ```
>
> Aunque los streams sin orden no aparecerán en el examen, si estás desarrollando aplicaciones con streams paralelos, **deberías** saber cuándo aplicar un stream sin orden para mejorar el rendimiento.

#### Combinando resultados con `reduce()`

Como aprendiste en el Capítulo 10, la operación de stream `reduce()` **combina** un stream en un solo objeto. **Recuerda** que el primer parámetro del método `reduce()` se llama la identidad, el segundo parámetro se llama el acumulador y el tercer parámetro se llama el combinador. La siguiente **es** la firma del método:

```Java
<U> U reduce(U identity,
   BiFunction<U,? super T,U> accumulator,
   BinaryOperator<U> combiner)
```

**Podemos concatenar** una lista de valores `char` usando el método `reduce()`, como se muestra en el siguiente ejemplo:

```Java
System.out.println(List.of('w', 'o', 'l', 'f')
   .parallelStream()
   .reduce("",
      (s1, c) -> s1 + c,
      (s2, s3) -> s2 + s3)); // wolf
```

> **Nota:** El nombre de las variables en este ejemplo de stream no es accidental. Usamos `c` por `char`, mientras que `s1`, `s2` y `s3` son valores `String`.

En los streams paralelos, el método `reduce()` **funciona aplicando** la reducción a pares de elementos dentro del stream para crear valores intermedios y luego combinando esos valores intermedios para producir un resultado final. Dicho de otro modo, en un stream serial, `wolf` se construye un carácter a la vez. En un stream paralelo, **se crean** los valores intermedios `wo` y `lf` y luego **se combinan**.

Con los streams paralelos, ahora **hay que preocuparse** por el orden. ¿Y si los elementos de un `String` **se combinan** en el orden incorrecto y producen `wlfo` o `flwo`? La Stream API **previene** este problema mientras todavía permite que los streams se procesen de forma paralela, siempre que sigas una regla simple: **asegúrate** de que el acumulador y el combinador produzcan el mismo resultado sin importar el orden en que se llamen.

> **Nota:** Si bien esto no está en el alcance del examen, el acumulador y el combinador deben ser asociativos, no interferentes y sin estado. ¡No entres en pánico; no necesitas saber términos matemáticos avanzados para el examen!

Si bien los requisitos de los argumentos de entrada del método `reduce()` **se cumplen** tanto para streams seriales como paralelos, **es posible** que no hayas notado ningún problema en los streams seriales porque el resultado siempre estaba ordenado. Con los streams paralelos, sin embargo, el orden ya no está garantizado, y cualquier argumento que viole estas reglas tiene muchas más probabilidades de producir efectos secundarios o resultados impredecibles.

**Veamos** un ejemplo que usa un acumulador problemático. En particular, el orden importa al restar números; por lo tanto, el siguiente código puede producir valores diferentes dependiendo de si usas un stream serial o paralelo. **Podemos omitir** un parámetro combinador en estos ejemplos, ya que el acumulador puede usarse cuando los tipos de datos intermedios son los mismos.

```Java
System.out.println(List.of(1, 2, 3, 4, 5, 6)
   .parallelStream()
   .reduce(0, (a, b) -> (a - b))); // ACUMULADOR PROBLEMÁTICO
```

**Puede producir** -21, 3 o algún otro valor.

**Puedes ver** otros problemas si usamos un parámetro identidad que no es realmente un valor identidad. Por ejemplo, ¿qué esperas que produzca el siguiente código?

```Java
System.out.println(List.of("w","o","l","f")
   .parallelStream()
   .reduce("X", String::concat)); // XwXoXlXf
```

En un stream serial, imprime `Xwolf`, pero en un stream paralelo, el resultado es `XwXoXlXf`. Como parte del proceso paralelo, la identidad **se aplica** a múltiples elementos del stream, resultando en datos muy inesperados.

> **Selección de un método `reduce()`**
>
> Si bien las versiones de uno y dos argumentos de `reduce()` soportan procesamiento paralelo, **se recomienda** que uses la versión de tres argumentos de `reduce()` cuando trabajes con streams paralelos. Proporcionar un método combinador explícito permite a la JVM particionar las operaciones en el stream de manera más eficiente.

#### Combinando resultados con `collect()`

Al igual que `reduce()`, la Stream API **incluye** una versión de tres argumentos de `collect()` que toma operadores acumulador y combinador junto con un operador proveedor en lugar de una identidad.

```Java
<R> R collect(Supplier<R> supplier,
   BiConsumer<R, ? super T> accumulator,
   BiConsumer<R, R> combiner)
```

También, al igual que con `reduce()`, las operaciones acumulador y combinador deben poder procesar resultados en cualquier orden. De esta manera, la versión de tres argumentos de `collect()` **puede realizarse** como una reducción paralela, como se muestra en el siguiente ejemplo:

```Java
Stream<String> stream = Stream.of("w", "o", "l",
   "f").parallel();
SortedSet<String> set =
   stream.collect(ConcurrentSkipListSet::new,
      Set::add,
      Set::addAll);
System.out.println(set); // [f, l, o, w]
```

**Recuerda** que los elementos en un `ConcurrentSkipListSet` se ordenan de acuerdo con su orden natural. **Deberías** usar una colección concurrente para combinar los resultados, asegurándote de que los resultados de los hilos concurrentes no provoquen una `ConcurrentModificationException`.

**Realizar** reducciones paralelas con un colector **requiere** consideraciones adicionales. Por ejemplo, si la colección en la que estás insertando es un conjunto de datos con orden, como un `List`, los elementos en la colección resultante deben estar en el mismo orden, sin importar si usas un stream serial o paralelo. Esto puede reducir el rendimiento, sin embargo, ya que algunas operaciones no pueden completarse de forma paralela.

#### Realizando una reducción paralela sobre un colector

Si bien cubrimos la interfaz `Collector` en el Capítulo 10, no entra en detalle sobre sus propiedades. Cada instancia de `Collector` **define** un método `characteristics()` que retorna un conjunto de atributos `Collector.Characteristics`. Al usar un `Collector` para realizar una reducción paralela, una serie de propiedades deben cumplirse. De lo contrario, la operación `collect()` se ejecutará de forma de un solo hilo.

### Requisitos para la reducción paralela con `collect()`

- El stream es paralelo.
- El parámetro de la operación `collect()` tiene la característica `Characteristics.CONCURRENT`.
- O bien el stream es sin orden o el colector tiene la característica `Characteristics.UNORDERED`.

Por ejemplo, si bien `Collectors.toSet()` tiene la característica `UNORDERED`, no tiene la característica `CONCURRENT`. Por lo tanto, lo siguiente no es una reducción paralela incluso con un stream paralelo:

```Java
parallelStream.collect(Collectors.toSet()); // No se trata de una reducción paralela
```

La clase `Collectors` **incluye** dos conjuntos de métodos estáticos para obtener colectores, `toConcurrentMap()` y `groupingByConcurrent()`, ambos `UNORDERED` y `CONCURRENT`. Estos métodos producen instancias de `Collector` capaces de realizar reducciones paralelas de manera eficiente. Al igual que sus contrapartes no concurrentes, hay versiones sobrecargadas que toman argumentos adicionales.

**Aquí hay** una reescritura de un ejemplo del Capítulo 10 para usar un stream paralelo y una reducción paralela:

```Java
Stream<String> ohMy = Stream.of("lions", "tigers", "bears").parallel();
ConcurrentMap<Integer, String> map = ohMy
   .collect(Collectors.toConcurrentMap(String::length,
      k -> k,
      (s1, s2) -> s1 + "," + s2));
System.out.println(map); // {5=lions,bears, 6=tigers}
System.out.println(map.getClass()); // java.util.concurrent.ConcurrentHashMap
```

**Usamos** una referencia `ConcurrentMap`, si bien la clase real retornada probablemente es `ConcurrentHashMap`. La clase en particular no está garantizada; simplemente será una clase que implemente la interfaz `ConcurrentMap`.

Finalmente, **podemos reescribir** nuestro ejemplo de `groupingBy()` del Capítulo 10 para usar un stream paralelo y una reducción paralela.

```Java
var ohMy = Stream.of("lions", "tigers", "bears").parallel();
ConcurrentMap<Integer, List<String>> map = ohMy.collect(
   Collectors.groupingByConcurrent(String::length));
System.out.println(map); // {5=[lions, bears], 6=[tigers]}
```

Como antes, el objeto retornado **puede asignarse** a una referencia `ConcurrentMap`.

> **Escenario del mundo real: Evitación de streams con estado**
>
> Pueden aparecer efectos secundarios en streams paralelos si tus expresiones lambda son con estado. Una expresión lambda **con estado** es aquella cuyo resultado depende de cualquier estado que pueda cambiar durante la ejecución de un pipeline. Por ejemplo, el siguiente método que filtra los números pares es con estado:
>
> ```Java
> public List<Integer> addValues(IntStream source) {
>    var data = Collections.synchronizedList(new ArrayList<Integer>());
>    source.filter(s -> s % 2 == 0)
>       .forEach(i -> { data.add(i); }); // CON ESTADO: ¡NO HAGAS ESTO!
>    return data;
> }
> ```
>
> Supongamos que este método se ejecuta con un stream serial:
>
> ```Java
> var list = addValues(IntStream.range(1, 11));
> System.out.print(list); // [2, 4, 6, 8, 10]
> ```
>
> Genial, los resultados están en el mismo orden en que fueron ingresados. Pero, ¿qué pasa si alguien más pasa un stream paralelo?
>
> ```Java
> var list = addValues(IntStream.range(1, 11).parallel());
> System.out.print(list); // [6, 8, 10, 2, 4]
> ```
>
> Oh, no: ¡nuestros resultados ya no coinciden con nuestro orden de entrada! El problema es que nuestra expresión lambda es con estado y modifica una lista que está fuera de nuestro stream. Podemos arreglar esta solución reescribiendo nuestra operación de stream para que sea sin estado:
>
> ```Java
> public List<Integer> addValuesBetter(IntStream source) {
>    return source.filter(s -> s % 2 == 0)
>       .boxed()
>       .collect(Collectors.toList());
> }
> ```
>
> Este método procesa el stream y luego recopila todos los resultados en una nueva lista. Produce el mismo resultado ordenado tanto en streams seriales como paralelos. **Se recomienda encarecidamente** que evites las operaciones con estado cuando uses streams paralelos para eliminar cualquier efecto secundario potencial en los datos. De hecho, **deberían evitarse** en los streams seriales, ya que hacerlo limita la capacidad del código de aprovechar la paralelización en el futuro.
