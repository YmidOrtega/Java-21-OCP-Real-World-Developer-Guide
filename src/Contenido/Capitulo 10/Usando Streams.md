Un _stream_ en Java es una secuencia de datos. Un _stream pipeline_ consiste en las operaciones que **se ejecutan** sobre un stream para producir un resultado. **Hay que notar** que las personas frecuentemente usan _stream_ y _stream pipeline_ de forma intercambiable para referirse al pipeline del stream. Primero, **se analiza** el flujo de los pipelines de forma conceptual. Después de eso, **se entra** al código.

#### Comprendiendo el Flujo del Pipeline

**Hay que** pensar en un stream pipeline como una línea de ensamblaje en una fábrica. Supongamos que **se está** ejecutando una línea de ensamblaje para hacer letreros para los exhibidores de animales del zoológico. **Se tiene** una serie de trabajos. Es el trabajo de una persona tomar los letreros de una caja. Es el trabajo de una segunda persona pintar el letrero. Es el trabajo de una tercera persona estarcir el nombre del animal en el letrero. Es el trabajo de la última persona colocar el letrero terminado en una caja para llevarlo al exhibidor correspondiente.

**Hay que notar** que la segunda persona no puede hacer nada hasta que un letrero haya sido sacado de la caja por la primera persona. De manera similar, la tercera persona no puede hacer nada hasta que un letrero haya sido pintado, y la última persona no puede hacer nada hasta que esté estarcido.

La línea de ensamblaje para hacer letreros es finita. Una vez que **se procesan** los contenidos de la caja de letreros, **se termina**. Los streams _finitos_ tienen un límite. Otras líneas de ensamblaje esencialmente corren para siempre, como una para la producción de alimentos. Por supuesto, en algún momento **se detienen** cuando la fábrica cierra, pero **hay que pretender** que eso no sucede. O **hay que pensar** en un ciclo de amanecer/atardecer como _infinito_, ya que no termina por un período de tiempo inordinadamente largo.

Otra característica importante de una línea de ensamblaje es que cada persona toca cada elemento para realizar su operación, y luego esa pieza de datos desaparece. No vuelve. La siguiente persona **se ocupa** de ella en ese momento. Esto es diferente de las listas y colas que **se vieron** en el capítulo anterior. Con una lista, **se puede** acceder a cualquier elemento en cualquier momento. Con una cola, **se está** limitado a los elementos a los que **se puede** acceder, pero todos los elementos están ahí. Con los streams, los datos no **se generan** por adelantado — **se crean** cuando **se necesitan**. Este es un ejemplo de _evaluación perezosa_ (_lazy evaluation_), que retrasa la ejecución hasta que **sea** necesaria.

Muchas cosas pueden suceder en las estaciones de la línea de ensamblaje en el camino. En programación funcional, estas **se llaman** _operaciones de stream_ (_stream operations_). Al igual que con la línea de ensamblaje, las operaciones ocurren en un pipeline. Alguien tiene que comenzar y terminar el trabajo, y puede haber cualquier número de estaciones intermedias. Hay tres partes en un stream pipeline, como **se muestra** en la imagen a continuación.

- **Fuente (_Source_):** De dónde proviene el stream.
- **Operaciones intermedias (_Intermediate operations_):** Transforma el stream en otro. Puede haber tan pocas o tantas operaciones intermedias como **se desee**. Dado que los streams usan evaluación perezosa, las operaciones intermedias no **se ejecutan** hasta que **se ejecute** la operación terminal.
- **Operación terminal (_Terminal operation_):** Produce un resultado. Dado que los streams solo **se pueden** usar una vez, el stream ya no es válido después de que una operación terminal **se completa**.
  
![[Stream pipeline.jpeg]]

**Hay que notar** que las operaciones son desconocidas desde afuera. Al ver la línea de ensamblaje desde el exterior, solo **se importa** lo que entra y sale. Lo que sucede en el medio es un detalle de implementación.

**Se necesitará** conocer bien las diferencias entre operaciones intermedias y terminales. **Hay que asegurarse** de poder completar la Tabla 10.2.

**TABLA 10.2** Operaciones intermedias vs. terminales

| **Escenario**                                         | **Operación intermedia** | **Operación terminal** |
| ----------------------------------------------------- | ------------------------ | ---------------------- |
| ¿Parte requerida de un pipeline útil?                 | No                       | Sí                     |
| ¿Puede existir múltiples veces en un pipeline?        | Sí                       | No                     |
| ¿El tipo de retorno es tipo stream?                   | Sí                       | No                     |
| ¿Se ejecuta al llamar al método?                      | No                       | Sí                     |
| ¿El stream sigue siendo válido después de la llamada? | Sí                       | No                     |

Una fábrica típicamente tiene un capataz que supervisa el trabajo. Java actúa como el capataz cuando **se trabaja** con stream pipelines. Este es un rol muy importante, especialmente cuando **se trata** con evaluación perezosa y streams infinitos. **Hay que pensar** en declarar el stream como dar instrucciones al capataz. A medida que el capataz descubre lo que **se necesita** hacer, **establece** las estaciones y les dice a los trabajadores cuáles serán sus funciones. Sin embargo, los trabajadores no comienzan hasta que el capataz les diga que empiecen. El capataz espera hasta que **ve** la operación terminal para poner en marcha el trabajo. El capataz también **observa** el trabajo y **detiene** la línea tan pronto como el trabajo **se completa**.

**Se verán** algunos ejemplos de esto. No **se está** usando código en estos ejemplos porque es realmente importante entender el concepto del stream pipeline antes de comenzar a escribir el código. La imagen muestra un stream pipeline con una operación intermedia.

![[Pasos en la ejecución de un stream pipeline.jpeg]]

**Se observa** lo que sucede desde el punto de vista del capataz. Primero, **ve** que la fuente está tomando letreros de la caja. El capataz **establece** un trabajador en la mesa para desempacar la caja y dice que **espere** una señal para comenzar. Luego, el capataz **ve** la operación intermedia para pintar el letrero. **Establece** un trabajador con pintura y dice que **espere** una señal para comenzar. Finalmente, el capataz **ve** la operación terminal para poner los letreros en un montón. **Establece** un trabajador para hacer esto y **grita** a los tres trabajadores que deben empezar.

Supongamos que hay dos letreros en la caja. El Paso 1 es el primer trabajador tomando un letrero de la caja y pasándolo al segundo trabajador. El Paso 2 es el segundo trabajador pintándolo y pasándolo al tercer trabajador. El Paso 3 es el tercer trabajador poniéndolo en el montón. Los Pasos 4–6 son este mismo proceso para el otro letrero. Luego, el capataz **ve** que no quedan letreros y **apaga** toda la empresa.

El capataz es inteligente y puede tomar decisiones sobre cómo realizar mejor el trabajo según lo que **se necesite**. Como ejemplo, **se explora** el stream pipeline en la siguiente imagen.

![[Un stream pipeline con un límite.jpeg]]

El capataz todavía **ve** una fuente de tomar letreros de la caja y **asigna** un trabajador para hacer eso bajo demanda. Todavía **ven** una operación intermedia para pintar y **establecen** otro trabajador con instrucciones para esperar y luego pintar. Luego, **ven** un paso intermedio en el que solo **se necesitan** dos letreros. **Establecen** un trabajador para contar los letreros que pasan y notificar al capataz cuando el trabajador haya visto dos. Finalmente, **establecen** un trabajador para la operación terminal de poner los letreros en un montón.

Esta vez, supongamos que hay 10 letreros en la caja. **Se empieza** como la última vez. El primer letrero **recorre** el pipeline. El segundo letrero también **recorre** el pipeline. Cuando el trabajador a cargo de contar **ve** el segundo letrero, le **dice** al capataz. El capataz permite que el trabajador de la operación terminal termine su tarea y luego **grita**: "¡Paren la línea!" No importa que haya ocho letreros más en la caja. No **se necesitan**, por lo que sería trabajo innecesario pintarlos. ¡Y todos queremos evitar el trabajo innecesario!

De manera similar, el capataz habría detenido la línea después del primer letrero si la operación terminal fuera encontrar el primer letrero que **se crea**.

En las siguientes secciones, **se cubren** las tres partes del pipeline. También **se discuten** tipos especiales de streams para primitivos y cómo imprimir un stream.

#### Creando Fuentes de Streams

En Java, los streams de los que **se ha estado** hablando están representados por la interfaz `Stream<T>`, definida en el paquete `java.util.stream`.

#### Creando Streams Finitos

Para simplificar, **se empieza** con streams finitos. Hay algunas formas de crearlos.

```Java
11: Stream<String> empty = Stream.empty();          // count = 0
12: Stream<Integer> singleElement = Stream.of(1);   // count = 1
13: Stream<Integer> fromArray = Stream.of(1, 2, 3); // count = 3
```

La línea 11 muestra cómo **crear** un stream vacío. La línea 12 muestra cómo **crear** un stream con un solo elemento. La línea 13 muestra cómo **crear** un stream a partir de varargs.

Java también proporciona una forma conveniente de convertir una `Collection` a un stream.

```Java
14: var list = List.of("a", "b", "c");
15: Stream<String> fromList = list.stream();
```

La línea 15 muestra que es una simple llamada a método para crear un stream a partir de una lista. Esto es útil ya que tales conversiones son comunes.

> **Creando un Stream Paralelo**
>
> Es igualmente fácil crear un stream paralelo a partir de una lista.
>
> ```Java
> 24: var list = List.of("a", "b", "c");
> 25: Stream<String> fromListParallel = list.parallelStream();
> ```
>
> Esta es una gran característica porque **se puede** escribir código que use concurrencia antes de siquiera aprender qué es un hilo. Usar streams paralelos es como establecer múltiples mesas de trabajadores que pueden hacer la misma tarea. Pintar sería mucho más rápido si **se pudieran** tener cinco pintores de letreros en lugar de uno. **Hay que tener** en cuenta que algunas tareas no **se pueden** hacer en paralelo, como poner los letreros en el orden en que **se crearon** en el stream. También **hay que ser** consciente de que hay un costo en coordinar el trabajo, por lo que para streams más pequeños podría ser más rápido hacerlo secuencialmente. **Se aprende** mucho más sobre la ejecución de tareas de forma concurrente en el Capítulo 13, "Concurrencia".

#### Creando Streams Infinitos

Hasta ahora, esto no es particularmente impresionante. **Se podría** hacer todo esto con listas. Sin embargo, no **se puede** crear una lista infinita, lo cual hace que los streams sean más poderosos.

```Java
17: Stream<Double> randoms = Stream.generate(Math::random);
18: Stream<Integer> oddNumbers = Stream.iterate(1, n -> n + 2);
```

La línea 17 **genera** un stream de números aleatorios. ¿Cuántos números aleatorios? Los que **se necesiten**. **Hay que recordar** que la fuente no **crea** realmente los valores hasta que **se llame** a una operación terminal. Más adelante en el capítulo, **se aprende** sobre operaciones como `limit()` para convertir el stream infinito en un stream finito y hacerlo seguro de imprimir sin bloquear el programa.

La línea 18 da más control. El método `iterate()` toma un valor semilla o inicial como primer parámetro. Este es el primer elemento que **formará** parte del stream. El otro parámetro es una expresión lambda que **recibe** el valor anterior y **genera** el siguiente valor. Como en el ejemplo de los números aleatorios, continuará produciendo números impares siempre que **se necesiten**.

¿Y si **se quisieran** solo números impares menores a 100? Hay una versión sobrecargada de `iterate()` que ayuda:

```Java
19: Stream<Integer> oddNumberUnder100 = Stream.iterate(
20:     1,              // seed
21:     n -> n < 100,  // Predicate para especificar cuándo terminar
22:     n -> n + 2);   // UnaryOperator para obtener el siguiente valor
```

Este método toma tres parámetros. **Hay que notar** cómo están separados por comas (`,`) al igual que en todos los demás métodos. El examen puede intentar confundir usando punto y coma ya que es similar a un bucle `for`. Adicionalmente, **hay que tener** cuidado de no crear accidentalmente un stream que **correrá** para siempre.

> **Imprimiendo una Referencia a Stream**
>
> Si **se intenta** imprimir un objeto stream, **se obtendrá** algo como lo siguiente:
>
> ```Java
> System.out.print(stream); // java.util.stream.ReferencePipeline$3@4517d9a3
> ```
>
> Esto es diferente de una `Collection`, donde **se ve** el contenido. No **se necesita** saber esto para el examen. **Se menciona** para que no **se sea** sorprendido cuando **se escriba** código de práctica.

#### Revisando los Métodos de Creación de Streams

Para revisar, **hay que asegurarse** de conocer todos los métodos en la Tabla 10.3. Estas son las formas de crear una fuente para streams, dada una instancia de `Collection` llamada `coll`.

**TABLA 10.3** Creando una fuente

| **Método** | **¿Finito o infinito?** | **Notas** |
|---|---|---|
| `Stream.empty()` | Finito | Crea un `Stream` con cero elementos. |
| `Stream.of(varargs)` | Finito | Crea un `Stream` con los elementos listados. |
| `coll.stream()` | Finito | Crea un `Stream` a partir de una `Collection`. |
| `coll.parallelStream()` | Finito | Crea un `Stream` a partir de una `Collection` donde el stream puede correr en paralelo. |
| `Stream.generate(supplier)` | Infinito | Crea un `Stream` llamando al `Supplier` para cada elemento bajo demanda. |
| `Stream.iterate(seed, unaryOperator)` | Infinito | Crea un `Stream` usando el seed para el primer elemento y luego llamando al `UnaryOperator` para cada elemento subsecuente bajo demanda. |
| `Stream.iterate(seed, predicate, unaryOperator)` | Finito o infinito | Crea un `Stream` usando el seed para el primer elemento y luego llamando al `UnaryOperator` para cada elemento subsecuente bajo demanda. **Se detiene** si el `Predicate` devuelve `false`. |
#### Uso de operaciones comunes en la terminal

**Se puede** realizar una operación terminal sin ninguna operación intermedia, pero no al revés. Es por eso que **se habla** de las operaciones terminales primero. Las _reducciones_ son un tipo especial de operación terminal donde todos los contenidos del stream **se combinan** en un único primitivo u `Object`. Por ejemplo, **se podría** terminar con un `int` o una `Collection`.

La Tabla 10.4 **resume** esta sección. **Se puede** usar como guía para recordar los puntos más importantes a medida que **se va** a través de cada uno individualmente. **Se explican** de más simples a más complejos en lugar de alfabéticamente.

**TABLA 10.4** Operaciones terminales de stream

| **Método** | **Qué sucede con streams infinitos** | **Valor de retorno** | **Reducción** |
|---|---|---|---|
| `count()` | No termina | `long` | Sí |
| `min()` / `max()` | No termina | `Optional<T>` | Sí |
| `findAny()` / `findFirst()` | Termina | `Optional<T>` | No |
| `allMatch()` / `anyMatch()` / `noneMatch()` | A veces termina | `boolean` | No |
| `forEach()` | No termina | `void` | No |
| `reduce()` | No termina | Varía | Sí |
| `collect()` | No termina | Varía | Sí |

#### Contando

El método `count()` **determina** el número de elementos en un stream finito. Para un stream infinito, nunca termina. ¿Por qué? ¡Cuenta del 1 al infinito y avisa cuando **se haya** terminado! El método `count()` es una reducción porque **revisa** cada elemento del stream y devuelve un único valor. La firma del método es la siguiente:

```Java
public long count()
```

Este ejemplo muestra cómo **llamar** a `count()` en un stream finito:

```Java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
System.out.println(s.count()); // 3
```

#### Encontrando el Mínimo y el Máximo

Los métodos `min()` y `max()` permiten **pasar** un comparador personalizado y **encontrar** el valor más pequeño o más grande en un stream finito según ese orden de clasificación. Al igual que el método `count()`, `min()` y `max()` **se cuelgan** en un stream infinito porque no pueden estar seguros de que un valor más pequeño o más grande no **llegue** más adelante en el stream. Ambos métodos son reducciones porque devuelven un único valor después de revisar todo el stream. Las firmas de los métodos son las siguientes:

```Java
public Optional<T> min(Comparator<? super T> comparator)
public Optional<T> max(Comparator<? super T> comparator)
```

Este ejemplo **encuentra** el animal con el menor número de letras en su nombre:

```Java
Stream<String> s = Stream.of("monkey", "ape", "bonobo");
Optional<String> min = s.min((s1, s2) -> s1.length() - s2.length());
min.ifPresent(System.out::println); // ape
```

**Hay que notar** que el código devuelve un `Optional` en lugar del valor. Esto permite al método especificar que no **se encontró** ningún mínimo o máximo. **Se usa** el método `ifPresent()` de `Optional` y una referencia a método para imprimir el mínimo solo si **se encontró** uno. Como ejemplo de dónde no hay un mínimo, **se observa** un stream vacío:

```Java
Optional<?> minEmpty = Stream.empty().min((s1, s2) -> 0);
System.out.println(minEmpty.isPresent()); // false
```

Dado que el stream está vacío, el comparador nunca **es llamado**, y no hay valor presente en el `Optional`.

> ¿Qué pasa si **se necesitan** tanto los valores `min()` como `max()` del mismo stream? Por ahora, no **se pueden** tener ambos, al menos no usando estos métodos. **Hay que recordar** que un stream solo puede tener una operación terminal. Una vez que una operación terminal **ha sido** ejecutada, el stream no **se puede** usar nuevamente. Como **se verá** más adelante en este capítulo, hay métodos de resumen integrados para algunos streams _numéricos_ que **calcularán** un conjunto de valores.

#### Encontrando un Valor

Los métodos `findAny()` y `findFirst()` **devuelven** un elemento del stream a menos que el stream esté vacío. Si el stream está vacío, **devuelven** un `Optional` vacío. Este es el primer método que **se ha visto** que puede terminar con un stream infinito. Dado que Java genera solo la cantidad de stream que **se necesite**, el stream infinito solo necesita generar un elemento.

Como su nombre lo indica, el método `findAny()` **puede devolver** cualquier elemento del stream. Cuando **se llama** a los streams que **se han visto** hasta ahora, comúnmente **devuelve** el primer elemento, aunque este comportamiento no está garantizado. Como **se verá** en el Capítulo 13, el método `findAny()` es más probable que **devuelva** un elemento aleatorio cuando **se trabaja** con streams paralelos.

Estos métodos son operaciones terminales pero no reducciones. La razón es que a veces devuelven sin procesar todos los elementos. Esto significa que **devuelven** un valor basado en el stream pero no **reducen** el stream entero a un único valor.

Las firmas de los métodos son las siguientes:

```Java
public Optional<T> findAny()
public Optional<T> findFirst()
```

Este ejemplo **encuentra** un animal:

```Java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
Stream<String> infinite = Stream.generate(() -> "chimp");

s.findAny().ifPresent(System.out::println);        // monkey (usualmente)
infinite.findAny().ifPresent(System.out::println); // chimp
```

Encontrar cualquier coincidencia es más útil de lo que parece. A veces solo **se quiere** obtener una muestra de los resultados y obtener un elemento representativo, pero no **se quiere** desperdiciar el procesamiento generándolos todos.

#### Coincidencias

Los métodos `allMatch()`, `anyMatch()` y `noneMatch()` **buscan** en un stream y **devuelven** información sobre cómo el stream **corresponde** al predicado. Estos pueden o no terminar para streams infinitos. Depende de los datos. Al igual que los métodos de búsqueda, no son reducciones porque no necesariamente **revisan** todos los elementos.

Las firmas de los métodos son las siguientes:

```Java
public boolean anyMatch(Predicate <? super T> predicate)
public boolean allMatch(Predicate <? super T> predicate)
public boolean noneMatch(Predicate <? super T> predicate)
```

Este ejemplo **verifica** si los nombres de los animales comienzan con letras:

```Java
var list = List.of("monkey", "2", "chimp");
Stream<String> infinite = Stream.generate(() -> "chimp");
Predicate<String> pred = x -> Character.isLetter(x.charAt(0));

System.out.println(list.stream().anyMatch(pred));  // true
System.out.println(list.stream().allMatch(pred));  // false
System.out.println(list.stream().noneMatch(pred)); // false
System.out.println(infinite.anyMatch(pred));       // true
```

Esto muestra que **se puede** reutilizar el mismo predicado, pero **se necesita** un stream diferente cada vez. El método `anyMatch()` **devuelve** `true` porque dos de los tres elementos coinciden. El método `allMatch()` **devuelve** `false` porque uno no coincide. El método `noneMatch()` también **devuelve** `false` porque al menos uno coincide. Llamar a `anyMatch()` en el stream infinito está bien porque **se encuentra** una coincidencia de inmediato y la llamada termina. Sin embargo, **hay que considerar** qué sucede si **se intenta** llamar a `allMatch()`:

```Java
Stream<String> infinite = Stream.generate(() -> "chimp");
Predicate<String> pred = x -> Character.isLetter(x.charAt(0));
System.out.println(infinite.allMatch(pred)); // Nunca termina
```

Debido a que `allMatch()` necesita verificar cada elemento, **correrá** hasta que se elimine el programa.

> **Hay que recordar** que `allMatch()`, `anyMatch()` y `noneMatch()` **devuelven** un `boolean`. Por contraste, los métodos de búsqueda **devuelven** un `Optional` porque **devuelven** un elemento del stream.

#### Iterando

Al igual que en el Java Collections Framework, es común **iterar** sobre los elementos de un stream. Como **se esperaría**, llamar a `forEach()` en un stream infinito no termina. Dado que no hay valor de retorno, no es una reducción.

Antes de usarlo, **hay que considerar** si otro enfoque sería mejor. Los desarrolladores que aprendieron a escribir bucles primero tienden a usarlos para todo. Por ejemplo, un bucle con una sentencia `if` podría **escribirse** con un filtro. **Se aprenderá** sobre los filtros en la sección de operaciones intermedias.

La firma del método es la siguiente:

```Java
public void forEach(Consumer<? super T> action)
```

**Hay que notar** que esta es la única operación terminal con un tipo de retorno de `void`. Si **se quiere** que algo suceda, **hay que hacer** que suceda en el `Consumer`. Aquí hay una forma de imprimir los elementos del stream (hay otras formas, que **se cubren** más adelante en el capítulo):

```Java
Stream<String> s = Stream.of("Monkey", "Gorilla", "Bonobo");
s.forEach(System.out::print); // MonkeyGorillaBonobo
```

> **Hay que recordar** que **se puede** llamar a `forEach()` directamente en una `Collection` o en un `Stream`. No **hay que confundirse** en el examen cuando **se vean** ambos enfoques.

**Hay que notar** que no **se puede** usar un bucle `for` tradicional en un stream.

```Java
Stream<Integer> s = Stream.of(1);
for (Integer i : s) {} // NO COMPILA
```

Aunque `forEach()` suena como un bucle, en realidad es un operador terminal para streams. Los streams no **se pueden** usar como fuente en un bucle for-each porque no implementan la interfaz `Iterable`.

#### Reduciendo

Los métodos `reduce()` **combinan** un stream en un único objeto. Son (obviamente) reducciones, lo que significa que **procesan** todos los elementos. Las tres firmas de métodos son las siguientes:

```Java
public T reduce(T identity, BinaryOperator<T> accumulator)

public Optional<T> reduce(BinaryOperator<T> accumulator)

public <U> U reduce(U identity,
    BiFunction<U,? super T,U> accumulator,
    BinaryOperator<U> combiner)
```

**Se toman** una a la vez. La forma más común de hacer una reducción es comenzar con un valor inicial y seguir fusionándolo con el siguiente valor. **Hay que pensar** en cómo **se concatenaría** un arreglo de objetos `String` en un único `String` sin programación funcional. Podría verse así:

```Java
var array = new String[] { "w", "o", "l", "f" };
var result = "";
for (var s: array) result = result + s;
System.out.println(result); // wolf
```

La _identidad_ es el valor inicial de la reducción, en este caso un `String` vacío. El _acumulador_ **combina** el resultado actual con el valor actual en el stream. Con lambdas, **se puede** hacer lo mismo con un stream y una reducción:

```Java
Stream<String> stream = Stream.of("w", "o", "l", "f");
String word = stream.reduce("", (s, c) -> s + c);
System.out.println(word); // wolf
```

**Hay que notar** que todavía **se tiene** el `String` vacío como identidad. También **se concatenan** los objetos `String` para obtener el siguiente valor. Incluso **se puede** reescribir esto con una referencia a método:

```Java
Stream<String> stream = Stream.of("w", "o", "l", "f");
String word = stream.reduce("", String::concat);
System.out.println(word); // wolf
```

**Se intenta** otro. ¿**Se puede** escribir una reducción para multiplicar todos los objetos `Integer` en un stream? **Hay que intentarlo**. La solución **se muestra** aquí:

```Java
Stream<Integer> stream = Stream.of(3, 5, 6);
System.out.println(stream.reduce(1, (a, b) -> a*b)); // 90
```

**Se establece** la identidad en `1` y el acumulador en multiplicación. En muchos casos, la identidad no es realmente necesaria, por lo que Java permite omitirla. Cuando no **se especifica** una identidad, **se devuelve** un `Optional` porque podría no haber ningún dato. Hay tres opciones para lo que hay en el `Optional`:

- Si el stream está vacío, **se devuelve** un `Optional` vacío.
- Si el stream tiene un elemento, **se devuelve**.
- Si el stream tiene múltiples elementos, **se aplica** el acumulador para combinarlos.

Lo siguiente **ilustra** cada uno de estos escenarios:

```Java
BinaryOperator<Integer> op = (a, b) -> a * b;
Stream<Integer> empty = Stream.empty();
Stream<Integer> oneElement = Stream.of(3);
Stream<Integer> threeElements = Stream.of(3, 5, 6);

empty.reduce(op).ifPresent(System.out::println);          // sin salida
oneElement.reduce(op).ifPresent(System.out::println);     // 3
threeElements.reduce(op).ifPresent(System.out::println);  // 90
```

¿Por qué hay dos métodos similares? ¿Por qué no siempre requerir la identidad? Java podría haber hecho eso. Sin embargo, a veces es bueno **diferenciar** el caso donde el stream está vacío en lugar del caso donde hay un valor que coincide con la identidad que **se devuelve** del cálculo. La firma que devuelve un `Optional` permite **diferenciar** estos casos. Por ejemplo, **se podría** devolver `Optional.empty()` cuando el stream está vacío y `Optional.of(3)` cuando hay un valor.

La tercera firma del método **se usa** cuando **se trata** con diferentes tipos. Permite a Java **crear** reducciones intermedias y luego **combinarlas** al final. **Se observa** un ejemplo que cuenta el número de caracteres en cada `String`:

```Java
Stream<String> stream = Stream.of("w", "o", "l", "f!");
int length = stream.reduce(0, (i, s) -> i+s.length(), (a, b) -> a+b);
System.out.println(length); // 5
```

El primer parámetro (`0`) es el valor del _inicializador_. Si **se tuviera** un stream vacío, esta sería la respuesta. El segundo parámetro es el _acumulador_. A diferencia de los acumuladores que **se vieron** anteriormente, este **maneja** tipos de datos mixtos. En este ejemplo, el primer argumento, `i`, es un `Integer`, mientras que el segundo argumento, `s`, es un `String`. **Agrega** la longitud del `String` actual al total acumulado. El tercer parámetro **se llama** el _combinador_, que **combina** cualquier total intermedio. En este caso, `a` y `b` son ambos valores `Integer`.

La operación `reduce()` de tres argumentos es útil cuando **se trabaja** con streams paralelos porque permite al stream **descomponerse** y **reensamblarse** por hilos separados. Por ejemplo, si **se necesitara** contar la longitud de cuatro `Strings` de 100 caracteres, los dos primeros valores y los dos últimos valores **podrían** calcularse de forma independiente. El resultado intermedio (200 + 200) luego **se combinaría** en el valor final.

#### Recopilando

El método `collect()` es un tipo especial de reducción llamado _reducción mutable_. Es más eficiente que una reducción regular porque **se usa** el mismo objeto mutable mientras **se acumula**. Los objetos mutables comunes incluyen `StringBuilder` y `ArrayList`. Este es un método muy útil, ya que permite **sacar** datos de streams y **convertirlos** a otra forma. Las firmas de los métodos son las siguientes:

```Java
public <R> R collect(Supplier<R> supplier,
    BiConsumer<R, ? super T> accumulator,
    BiConsumer<R, R> combiner)

public <R,A> R collect(Collector<? super T, A,R> collector)
```

**Se empieza** con la primera firma, que **se usa** cuando **se quiere** codificar específicamente cómo debe funcionar la recopilación. El ejemplo de `wolf` del metodo `reduce` **se puede** adaptar para usar `collect()`:

```Java
Stream<String> stream = Stream.of("w", "o", "l", "f");

StringBuilder word = stream.collect(
    StringBuilder::new,
    StringBuilder::append,
    StringBuilder::append);

System.out.println(word); // wolf
```

El primer parámetro es el _proveedor_ (_supplier_), que **crea** el objeto que **almacenará** los resultados a medida que **se recopilen** los datos. **Hay que recordar** que un `Supplier` no toma ningún parámetro y devuelve un valor. En este caso, **construye** un nuevo `StringBuilder`.

El segundo parámetro es el _acumulador_ (_accumulator_), que es un `BiConsumer` que toma dos parámetros y no devuelve nada. Es responsable de **agregar** un elemento más a la colección de datos. En este ejemplo, **agrega** el siguiente `String` al `StringBuilder`.

El parámetro final es el _combinador_ (_combiner_), que es otro `BiConsumer`. Es responsable de **tomar** dos colecciones de datos y **fusionarlas**. Esto es útil cuando **se está** procesando en paralelo. Se **forman** dos colecciones más pequeñas y luego **se fusionan** en una. Esto funcionaría con `StringBuilder` solo si no nos **importara** el orden de las letras. En este caso, el acumulador y el combinador tienen una lógica similar.

**Se observa** ahora un ejemplo donde la lógica es diferente en el acumulador y el combinador:

```Java
Stream<String> stream = Stream.of("w", "o", "l", "f");

TreeSet<String> set = stream.collect(
    TreeSet::new,
    TreeSet::add,
    TreeSet::addAll);

System.out.println(set); // [f, l, o, w]
```

El recopilador tiene tres partes como antes. El proveedor **crea** un `TreeSet` vacío. El acumulador **agrega** un único `String` del `Stream` al `TreeSet`. El combinador **agrega** todos los elementos de un `TreeSet` a otro en caso de que las operaciones **hayan sido** realizadas en paralelo y deban **fusionarse**.

**Se empezó** con la firma larga porque así es como **se implementa** un recopilador propio. Es importante **saber** cómo hacer esto para el examen y **entender** cómo funcionan los recopiladores. En la práctica, muchos recopiladores comunes aparecen una y otra vez. En lugar de que los desarrolladores sigan reimplementando los mismos, Java proporciona una clase con recopiladores comunes llamada `Collectors`. Este enfoque también hace que el código sea más fácil de leer porque es más expresivo. Por ejemplo, **se podría** reescribir el ejemplo anterior de la siguiente manera:

```Java
Stream<String> stream = Stream.of("w", "o", "l", "f");
TreeSet<String> set = stream.collect(Collectors.toCollection(TreeSet::new));
System.out.println(set); // [f, l, o, w]
```

Si no **se necesitara** que el conjunto esté ordenado, **se podría** hacer el código aún más corto:

```Java
Stream<String> stream = Stream.of("w", "o", "l", "f");
Set<String> set = stream.collect(Collectors.toSet());
System.out.println(set); // [f, w, l, o]
```

**Se podrían** obtener resultados diferentes para este último ya que `toSet()` no garantiza qué implementación de `Set` **se obtendrá**. Lo más probable es que sea un `HashSet`, pero no **se debería** esperar o depender de eso.

> El examen espera que **se conozcan** los recopiladores predefinidos comunes además de **ser** capaz de escribir los propios pasando un proveedor, acumulador y combinador.

Más adelante en este capítulo, **se muestran** muchos `Collectors` que **se usan** para agrupar datos. Es un tema grande, por lo que es mejor **dominar** cómo funcionan los streams antes de **agregar** demasiados `Collectors` a la mezcla.

A diferencia de una operación terminal, una operación intermedia **produce** un stream como resultado. Una operación intermedia también **puede** tratar con un stream infinito simplemente devolviendo otro stream infinito. Dado que los elementos **se producen** solo cuando **se necesitan**, funciona perfectamente. El trabajador de la línea de ensamblaje no necesita preocuparse por cuántos elementos más **vienen** y en cambio **puede** centrarse en el elemento actual.

#### Filtrando

El método `filter()` **devuelve** un `Stream` con los elementos que coinciden con una expresión dada. Aquí está la firma del método:

```Java
public Stream<T> filter(Predicate<? super T> predicate)
```

Esta operación es fácil de recordar y poderosa porque **se puede** pasar cualquier `Predicate`. Por ejemplo, esto **retiene** todos los elementos que comienzan con la letra _m_:

```Java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
s.filter(x -> x.startsWith("m"))
    .forEach(System.out::print); // monkey
```

#### Eliminando Duplicados

El método `distinct()` **devuelve** un stream con los valores duplicados eliminados. Los duplicados no necesitan ser adyacentes para **ser** eliminados. Como **se podría** imaginar, Java llama a `equals()` para determinar si los objetos son equivalentes. La firma del método es la siguiente:

```Java
public Stream<T> distinct()
```

Aquí hay un ejemplo:

```Java
Stream<String> s = Stream.of("duck", "duck", "duck", "goose");
s.distinct()
    .forEach(System.out::print); // duckgoose
```

#### Restringiendo por Posición

Los métodos `limit()` y `skip()` pueden **hacer** un `Stream` más pequeño. El método `limit()` también podría **hacer** un stream finito a partir de un stream infinito. Las firmas de los métodos **se muestran** aquí:

```Java
public Stream<T> limit(long maxSize)
public Stream<T> skip(long n)
```

El siguiente código **crea** un stream infinito de números contando desde 1. La operación `skip()` **devuelve** un stream infinito comenzando con los números que cuentan desde 6, ya que **omite** los primeros cinco elementos. La operación `limit()` **toma** los primeros dos de esos. Ahora **se tiene** un stream finito con dos elementos, que **se pueden** imprimir con el método `forEach()`:

```Java
Stream<Integer> s = Stream.iterate(1, n -> n + 1);
s.skip(5)
    .limit(2)
    .forEach(System.out::print); // 67
```

#### Mapeando

El método `map()` **crea** una asignación de uno a uno de los elementos del stream a los elementos del siguiente paso en el stream. La firma del método es la siguiente:

```Java
public <R> Stream<R> map(Function<? super T, ? extends R> mapper)
```

Esta parece más complicada que las otras que **se han visto**. Usa la expresión lambda para **determinar** el tipo pasado a esa función y el que **se devuelve**.

> El método `map()` en streams es para **transformar** datos. No **hay que confundirlo** con la interfaz `Map`, que **mapea** claves a valores.

Como ejemplo, este código **convierte** una lista de objetos `String` a una lista de objetos `Integer` que representan sus longitudes:

```Java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
s.map(String::length)
    .forEach(System.out::print); // 676
```

**Hay que recordar** que `String::length` es la abreviatura de la lambda `x -> x.length()`, que claramente muestra que es una función que convierte un `String` en un `Integer`.

#### Usando *flatMap*

El método `flatMap()` **toma** cada elemento del stream y **hace** que cualquier elemento que contenga **sea** un elemento de nivel superior en un único stream. Esto es útil cuando **se quiere** eliminar elementos vacíos de un stream o **combinar** un stream de listas. **Se muestra** la firma del método para ser coherentes con los otros métodos para que no **se piense** que **se está** ocultando algo. No **se espera** poder leer esto:

```Java
public <R> Stream<R> flatMap(
    Function<? super T, ? extends Stream<? extends R>> mapper)
```

Esto básicamente dice que **devuelve** un `Stream` del tipo que la función contiene en un nivel más bajo. No **hay que preocuparse** por la firma. Es un dolor de cabeza.

Lo que **se debería** entender es el ejemplo. Esto **pone** a todos los animales en el mismo nivel y **elimina** la lista vacía.

```Java
List<String> zero = List.of();
var one = List.of("Bonobo");
var two = List.of("Mama Gorilla", "Baby Gorilla");
Stream<List<String>> animals = Stream.of(zero, one, two);

animals.flatMap(m -> m.stream())
    .forEach(System.out::println);
```

Aquí está la salida:

```Plaintext
Bonobo
Mama Gorilla
Baby Gorilla
```

Como **se puede** ver, **eliminó** la lista vacía completamente y **cambió** todos los elementos de cada lista para **estar** en el nivel superior del stream.

> **Concatenando Streams**
>
> Aunque `flatMap()` es bueno para el caso general, hay una manera más conveniente de **concatenar** dos streams:
>
> ```Java
> var one = Stream.of("Bonobo");
> var two = Stream.of("Mama Gorilla", "Baby Gorilla");
>
> Stream.concat(one, two)
>     .forEach(System.out::println);
> ```
>
> Esto **produce** las mismas tres líneas que el ejemplo anterior. Los dos streams **se concatenan**, y **se llama** a la operación terminal, `forEach()`.

#### Ordenando

El método `sorted()` **devuelve** un stream con los elementos ordenados. Al igual que **ordenar** arreglos, Java usa el orden natural a menos que **se especifique** un comparador. Las firmas de los métodos **son** las siguientes:

```Java
public Stream<T> sorted()
public Stream<T> sorted(Comparator<? super T> comparator)
```

Llamar a la primera firma **usa** el orden de clasificación predeterminado.

```Java
Stream<String> s = Stream.of("brown-", "bear-");
s.sorted()
    .forEach(System.out::print); // bear-brown-
```

Opcionalmente **se puede** usar una implementación de `Comparator` a través de un método o una lambda. En este ejemplo, **se está** usando un método:

```Java
Stream<String> s = Stream.of("brown bear-", "grizzly-");
s.sorted(Comparator.reverseOrder())
    .forEach(System.out::print); // grizzly-brown bear-
```

Aquí **se pasa** un `Comparator` para especificar que **se quiere** ordenar en el orden natural inverso. ¿Listo para uno complicado? ¿**Se ve** por qué esto no compila?

```Java
Stream<String> s = Stream.of("brown bear-", "grizzly-");
s.sorted(Comparator::reverseOrder);  // NO COMPILA
```

**Hay que** observar la segunda firma del método `sorted()` nuevamente. Toma un `Comparator`, que es una interfaz funcional que toma dos parámetros y devuelve un `int`. Sin embargo, `Comparator::reverseOrder` no hace eso. Debido a que `reverseOrder()` no toma argumentos y devuelve un valor, es equivalente a `() -> Comparator.reverseOrder()`, que en realidad es un `Supplier<Comparator>`. Esto no es compatible con `sorted()`. Esto **se menciona** para recordar que realmente **se necesita** conocer bien las referencias a métodos.

#### Inspeccionando con peek

El método `peek()` es nuestra operación intermedia final. Es útil para depuración porque permite **realizar** una operación de stream sin **cambiar** el stream. La firma del método es la siguiente:

```Java
public Stream<T> peek(Consumer<? super T> action)
```

**Se podría** notar que la operación intermedia `peek()` toma el mismo argumento que la operación terminal `forEach()`. **Hay que pensar** en `peek()` como una versión intermedia de `forEach()` que **devuelve** el stream original.

El uso más común de `peek()` es **mostrar** el contenido del stream a medida que **pasa**. Supongamos que **se cometió** un error tipográfico y **se contaron** osos que comienzan con la letra _g_ en lugar de _b_. **Se pregunta** por qué el conteo es 1 en lugar de 2. **Se puede** agregar un método `peek()` para averiguar por qué.

```Java
var stream = Stream.of("black bear", "brown bear", "grizzly");
long count = stream.filter(s -> s.startsWith("g"))
    .peek(System.out::println).count();  // grizzly
System.out.println(count);              // 1
```

En el Capítulo 9, **se vio** que `peek()` solo mira el primer elemento cuando **se trabaja** con una `Queue`. En un stream, `peek()` **mira** cada elemento que **pasa** por esa parte del stream pipeline. Es como tener un trabajador que toma notas sobre cómo está progresando un paso particular del proceso.

> **Peligro: Cambiando Estado**
>
> En general, es una mala práctica tener efectos secundarios en un stream pipeline. Por ejemplo, es mejor usar un recopilador para **crear** una nueva lista que **cambiar** los elementos de una existente. De manera similar, si **se está** intentando **hacer un seguimiento** de algo, es mejor tener el stream **devolviendo** un conteo que **incrementar** un contador de variable de instancia. Sin embargo, en el examen, **se pueden** ver efectos secundarios para hacer el código más conciso como el siguiente:
>
> ```Java
> private static int count = 20;
> public void incrementCountBadly() {
>     Stream.iterate(0, n -> n + 1)
>         .limit(10)
>         .forEach(p -> count++);
> }
> ```
>
> De manera similar, `peek()` está destinado a **realizar** una operación sin **cambiar** el resultado. Aquí hay un stream pipeline directo que no usa `peek()`:
>
> ```Java
>     var numbers = new ArrayList<>();
>     var letters = new ArrayList<>();
>     numbers.add(1);
>     letters.add('a');
>
>     Stream<List<?>> stream = Stream.of(numbers, letters);
>     stream.map(List::size).forEach(System.out::print); // 11
> ```
>
> Ahora **se agrega** una llamada a `peek()` y **se nota** que Java no **impide** escribir código `peek()` malo:
>
> ```Java
>     Stream<List<?>> bad = Stream.of(numbers, letters);
>     bad.peek(x -> x.remove(0))
>         .map(List::size)
>         .forEach(System.out::print); // 00
> ```
>
> Este ejemplo es malo porque `peek()` está **modificando** la estructura de datos que **se usa** en el stream, lo cual hace que el resultado del stream pipeline sea diferente a si el `peek()` no estuviera presente.

Los streams permiten **usar** encadenamiento y **expresar** lo que **se quiere** lograr en lugar de cómo hacerlo. Digamos que **se quisiera** obtener los dos primeros nombres de los amigos alfabéticamente que tengan cuatro caracteres de longitud. Sin streams, **se tendría** que escribir algo como lo siguiente:

```Java
var list = List.of("Toby", "Anna", "Leroy", "Alex");
List<String> filtered = new ArrayList<>();
for (String name: list)
    if (name.length() == 4) filtered.add(name);
Collections.sort(filtered);
var iter = filtered.iterator();
if (iter.hasNext()) System.out.println(iter.next());
if (iter.hasNext()) System.out.println(iter.next());
```

Esto funciona. Requiere algo de lectura y reflexión para **entender** qué está sucediendo. El problema que **se está** intentando resolver **se pierde** en la implementación. También está muy enfocado en el _cómo_ en lugar del _qué_. Con streams, el código equivalente es el siguiente:

```Java
var list = List.of("Toby", "Anna", "Leroy", "Alex");
list.stream().filter(n -> n.length() == 4).sorted()
    .limit(2).forEach(System.out::println);
```

Antes de decir que es más difícil de leer, **se puede** dar formato.

```Java
var list = List.of("Toby", "Anna", "Leroy", "Alex");
list.stream()
    .filter(n -> n.length() == 4)
    .sorted()
    .limit(2)
    .forEach(System.out::println);
```

La diferencia es que **se expresa** lo que está pasando. **Se importan** objetos `String` de longitud 4. Luego **se quieren** ordenados. Luego **se quieren** los dos primeros. Luego **se quieren** imprimir. **Se mapea** mejor al problema que **se está** intentando resolver, y es más simple.

Una vez que **se empieza** a usar streams en el código, **se puede** encontrar usándolos en muchos lugares. Tener código más corto, más breve y más claro es definitivamente algo bueno.

En este ejemplo, **se ven** las tres partes del pipeline. La imagen a continuación muestra cómo cada operación intermedia en el pipeline **alimenta** a la siguiente.

![[Stream pipeline con múltiples operaciones intermedias.jpeg]]

**Hay que recordar** que el capataz de la línea de ensamblaje está **determinando** cómo implementar mejor el stream pipeline. **Establecen** todas las mesas con instrucciones para esperar antes de comenzar. Le **dicen** al trabajador de `limit()` que les informe cuando hayan pasado dos elementos. Le **dicen** al trabajador de `sorted()` que deben simplemente **recolectar** todos los elementos a medida que **llegan** y **ordenarlos** todos de una vez. Después de ordenarlos, deben **comenzar** a pasarlos al trabajador de `limit()` uno a la vez. Los datos **fluyen** de la siguiente manera:

1. El método `stream()` **envía** Toby a `filter()`. El método `filter()` **ve** que la longitud es buena y **envía** Toby a `sorted()`. El método `sorted()` no **puede** ordenar aún porque **necesita** todos los datos, por lo que **retiene** Toby.
2. El método `stream()` **envía** Anna a `filter()`. El método `filter()` **ve** que la longitud es buena y **envía** Anna a `sorted()`. El método `sorted()` no **puede** ordenar aún porque **necesita** todos los datos, por lo que **retiene** Anna.
3. El método `stream()` **envía** Leroy a `filter()`. El método `filter()` **ve** que la longitud no coincide, y **saca** a Leroy del procesamiento de la línea de ensamblaje.
4. El método `stream()` **envía** Alex a `filter()`. El método `filter()` **ve** que la longitud es buena y **envía** Alex a `sorted()`. El método `sorted()` no **puede** ordenar aún porque **necesita** todos los datos, pero **retiene** Alex. Resulta que `sorted()` sí tiene todos los datos requeridos, pero aún no lo sabe.
5. El capataz le informa a `sorted()` que es hora de **ordenar**, y **se produce** la ordenación.
6. El método `sorted()` **envía** Alex a `limit()`. El método `limit()` **recuerda** que ha visto un elemento y **envía** Alex a `forEach()`, **imprimiendo** Alex.
7. El método `sorted()` **envía** Anna a `limit()`. El método `limit()` **recuerda** que ha visto dos elementos y **envía** Anna a `forEach()`, **imprimiendo** Anna.
8. El método `limit()` ahora ha visto todos los elementos que **se necesitan** y le **dice** al capataz. El capataz **detiene** la línea, y no se produce más procesamiento en el pipeline.

¿Tiene sentido? **Se intentan** algunos ejemplos más para asegurarse de **entender** bien esto. ¿Qué **se cree** que hace lo siguiente?

```Java
Stream.generate(() -> "Elsa")
    .filter(n -> n.length() == 4)
    .sorted()
    .limit(2)
    .forEach(System.out::println);
```

**Se cuelga** hasta que **se elimine** el programa, o **lanza** una excepción después de quedarse sin memoria. El capataz ha instruido a `sorted()` para que espere hasta que todo esté presente para **ordenar**. Eso nunca sucede porque hay un stream infinito. ¿Y este ejemplo?

```Java
Stream.generate(() -> "Elsa")
    .filter(n -> n.length() == 4)
    .limit(2)
    .sorted()
    .forEach(System.out::println);
```

Este imprime Elsa dos veces. El filtro permite pasar los elementos, y `limit()` detiene las operaciones anteriores después de dos elementos. Ahora `sorted()` puede **ordenar** porque **se tiene** una lista finita. Finalmente, ¿qué **se cree** que hace esto?

```Java
Stream.generate(() -> "Olaf Lazisson")
    .filter(n -> n.length() == 4)
    .limit(2)
    .sorted()
    .forEach(System.out::println);
```

Este también **se cuelga** hasta que **se elimine** el programa. El filtro no permite pasar nada, por lo que `limit()` nunca ve dos elementos. Esto significa que **se tiene** que seguir esperando y esperar que aparezcan.

**Se pueden** incluso **encadenar** dos pipelines juntos. **Se intenta** identificar las dos fuentes y dos operaciones terminales en este código:

```Java
30: long count =  Stream.of("goldfish", "finch")
31:     .filter(s -> s.length()> 5)
32:     .collect(Collectors.toList())
33:     .stream()
34:     .count();
35: System.out.println(count);  // 1
```

Las líneas 30–32 son un pipeline, y las líneas 33 y 34 son otro. Para el primer pipeline, la línea 30 es la fuente, y la línea 32 es la operación terminal. Para el segundo pipeline, la línea 33 es la fuente, y la línea 34 es la operación terminal. ¡Ahora esa es una forma complicada de mostrar el número 1!

> En el examen, **se podrían** ver pipelines largos o complejos como opciones de respuesta. Si esto sucede, **hay que enfocarse** en las diferencias entre las respuestas. Esas serán las pistas para la respuesta correcta. Este enfoque también **ahorrará** tiempo al no tener que **estudiar** todo el pipeline en cada opción.

Cuando **se vean** pipelines encadenados, **hay que notar** dónde están la fuente y las operaciones terminales. Esto ayudará a **hacer un seguimiento** de lo que está pasando. Incluso **se puede** reescribir el código en la cabeza para tener una variable en el medio para que no sea tan largo y complicado. El ejemplo anterior **se puede** escribir de la siguiente manera:

```Java
List<String> helper =  Stream.of("goldfish", "finch")
    .filter(s -> s.length()> 5)
    .collect(Collectors.toList());
long count = helper.stream()
    .count();
System.out.println(count);
```

El estilo que **se use** es decisión propia. Sin embargo, **se necesita** poder leer ambos estilos antes de **tomar** el examen.

---

**Ver también:** [[Arrays]] | [[Algoritmos]]

