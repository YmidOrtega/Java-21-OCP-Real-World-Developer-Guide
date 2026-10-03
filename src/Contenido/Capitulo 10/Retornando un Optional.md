A estas alturas, **se debe tener familiaridad** con la sintaxis de las **expresiones _lambda_** y las **referencias a métodos** (_method references_). Ambas se utilizan al implementar **interfaces funcionales**. Si se requiere más práctica, se puede revisar el Capítulo 8, "Lambdas e Interfaces Funcionales", y el Capítulo 9, "Colecciones y Genéricos". En este capítulo, **se añade la programación funcional real**, centrándose en la **API de _Streams_**.

Se debe tener en cuenta que la **API de _Streams_** abordada en este capítulo se utiliza para la programación funcional. En contraste, existen también los _streams_ de `java.io`, los cuales se analizan en el Capítulo 14, "E/S" (_I/O_). A pesar de que ambos utilizan la palabra _stream_, no guardan ninguna semejanza.

En este capítulo, **se introduce la clase `Optional`**. Posteriormente, **se presenta la tubería de _Stream_** (_Stream pipeline_) y se integra todo el contenido. Se recomienda leer este capítulo dos veces antes de realizar las preguntas de repaso para asegurar la comprensión total. La programación funcional suele presentar una curva de aprendizaje pronunciada, pero resulta sumamente gratificante una vez que se domina.

### Retorno de un `Optional`

Suponga que se cursa una clase introductoria de Java y se obtienen calificaciones de 90 y 100 en los dos primeros exámenes. Al calcular el promedio, se suman las puntuaciones y se dividen entre la cantidad total de calificaciones, obteniendo $(90+100)/2$, lo que resulta en $190/2$, es decir, un promedio de 95.

Ahora, suponga que se asiste al primer día de una segunda clase de Java. Al consultar cuál es el promedio en esta clase que acaba de comenzar, al no haberse realizado ningún examen aún, no se dispone de datos para promediar. Decir que el promedio es cero no sería preciso ni verdadero. Simplemente **no existen datos**, por lo que **no se tiene un promedio**.

¿Cómo se expresa esta respuesta de "se desconoce" o "no aplica" en Java? **Se utiliza el tipo `Optional`**. Un `Optional` se crea mediante una **fábrica** (_factory_). Se puede solicitar un `Optional` vacío o pasar un valor para que el `Optional` lo envuelva. Se puede visualizar un `Optional` como una caja que puede contener algo o estar vacía. La Figura mas adelante ilustra ambas opciones.

### Creación de un `Optional`

A continuación, se presenta la implementación del método `average()`:

```Java
10: public static Optional<Double> average(int... scores) {
11:    if (scores.length == 0) return Optional.empty();
12:    int sum = 0;
13:    for (int score: scores) sum += score;
14:    return Optional.of((double) sum / scores.length);
15: }
```

![[Optional.png]]

La línea 11 **retorna un `Optional` vacío** cuando no se puede calcular un promedio. Las líneas 12 y 13 suman las calificaciones. Existe una forma en programación funcional para realizar este cálculo, pero se abordará más adelante en el capítulo. De hecho, todo el método podría escribirse en una sola línea, pero eso no enseñaría cómo funciona `Optional`. La línea 14 **crea un `Optional` para envolver el promedio**. Se observa que se utiliza un método estático para crear un `Optional`. Esto se debe a que `Optional` se basa en el **patrón fábrica** (_factory pattern_) y no expone constructores públicos.

Al invocar el método, se observa el contenido de ambas cajas:

```Java
System.out.println(average(90, 100)); // Optional[95.0]
System.out.println(average());         // Optional.empty
```

Un `Optional` puede recibir un **tipo genérico**, lo que facilita la extracción de valores. Se observa que un `Optional<Double>` contiene un valor mientras que el otro está vacío. Por lo general, se desea **verificar si un valor está presente** y/o extraerlo de la caja. A continuación, se muestra una forma de realizarlo:

```Java
Optional<Double> opt = average(90, 100);
if (opt.isPresent())
    System.out.println(opt.get()); // 95.0
```

Primero se verifica si el `Optional` contiene un valor y luego se imprime. ¿Qué sucedería si no se realizara la verificación y el `Optional` estuviera vacío?

```Java
Optional<Double> opt = average();
System.out.println(opt.get()); // NoSuchElementException
```

Se obtendría una excepción dado que no hay ningún valor dentro del `Optional`:

```Plaintext
java.util.NoSuchElementException: No value present
```

Al crear un `Optional`, es común querer utilizar `empty()` cuando el valor es `null`. Esto se puede realizar con una sentencia `if` o con el **operador ternario** (`? :`) para simplificar el código:


```Java
Optional o = (value == null) ? Optional.empty() : Optional.of(value);
```

Si `value` es `null`, a `o` se le asigna el `Optional` vacío. De lo contrario, se envuelve el valor. Al ser un patrón tan común, Java proporciona un **método de fábrica** para realizar exactamente lo mismo:

```Java
Optional o = Optional.ofNullable(value);
```

Con esto se cubren los métodos estáticos requeridos sobre `Optional`. En la **Tabla 10.1** se resumen la mayoría de los **métodos de instancia** de `Optional` necesarios para el examen. Existen algunos otros que involucran encadenamiento (_chaining_), los cuales se analizarán más adelante.

#### TABLA 10.1 Métodos de instancia comunes de `Optional`

|**Método**|**Cuando el Optional está vacío**|**Cuando el Optional contiene un valor**|
|---|---|---|
|`get()`|Lanza una excepción|Retorna el valor|
|`ifPresent(Consumer c)`|No hace nada|Invoca al `Consumer` con el valor|
|`isPresent()`|Retorna `false`|Retorna `true`|
|`orElse(T other)`|Retorna el parámetro `other`|Retorna el valor|
|`orElseGet(Supplier s)`|Retorna el resultado de invocar al `Supplier`|Retorna el valor|
|`orElseThrow()`|Lanza `NoSuchElementException`|Retorna el valor|
|`orElseThrow(Supplier s)`|Lanza la excepción creada al invocar al `Supplier`|Retorna el valor|

Previamente se han mostrado `get()` e `isPresent()`. Los demás métodos permiten escribir código utilizando `Optional` en una sola línea sin necesidad de recurrir al operador ternario, lo que mejora la legibilidad. En lugar de utilizar una sentencia `if`, como se hizo al verificar el promedio anteriormente, se puede especificar un `Consumer` que se ejecutará únicamente cuando exista un valor dentro del `Optional`. De lo contrario, el método simplemente omite la ejecución del `Consumer`.

```Java
Optional<Double> opt = average(90, 100);
opt.ifPresent(System.out::println);
```

El uso de `ifPresent()` expresa con mayor claridad la intención del código: ejecutar una acción si el valor está presente. Puede interpretarse como una sentencia `if` sin bloque `else`.

### Manejo de un `Optional` vacío

Los métodos restantes permiten especificar la acción a realizar cuando no hay un valor presente. Existen varias opciones; las dos primeras permiten definir un valor de retorno, ya sea directamente o mediante un `Supplier`.

```Java
30: Optional<Double> opt = average();
31: System.out.println(opt.orElse(Double.NaN));
32: System.out.println(opt.orElseGet(() -> Math.random()));
```

La salida generada es similar a la siguiente:

```Plaintext
NaN
0.49775932295380165
```

La línea 31 muestra que se puede retornar un valor o variable específica (en este caso, el valor "no es un número" o `NaN`). La línea 32 ilustra el uso de un `Supplier` para generar en tiempo de ejecución el valor a retornar.

Alternativamente, se puede hacer que el código lance una excepción si el `Optional` está vacío:

```Java
30: Optional<Double> opt = average();
31: System.out.println(opt.orElseThrow());
```

Esto imprime una salida similar a la siguiente:

```Plaintext
Exception in thread "main" java.util.NoSuchElementException:
No value present
at java.base/java.util.Optional.orElseThrow(Optional.java:382)
```

Si no se especifica un `Supplier` para la excepción, Java lanzará un `NoSuchElementException`. Alternativamente, se puede definir el lanzamiento de una **excepción personalizada** si el `Optional` se encuentra vacío. Se debe recordar que la traza de la pila (_stack trace_) puede parecer inusual debido a que las expresiones _lambda_ son generadas dinámicamente en lugar de ser clases con nombre.

```Java
30: Optional<Double> opt = average();
31: System.out.println(opt.orElseThrow(
32:     () -> new IllegalStateException()));
```

Esto genera una salida similar a la siguiente:

```Plaintext
Exception in thread "main" java.lang.IllegalStateException
at optionals.Methods.lambda$orElse$1(Methods.java:31)
at java.base/java.util.Optional.orElseThrow(Optional.java:408)
```

La línea 32 muestra el uso de un `Supplier` para instanciar la excepción que debe ser lanzada. Observe que no se escribe `throw new IllegalStateException()`. El método `orElseThrow()` se encarga internamente de realizar el lanzamiento de la excepción durante la ejecución.

Los dos métodos que reciben un `Supplier` poseen nombres diferentes. A continuación, se analiza por qué el siguiente código no compila:

```Java
System.out.println(opt.orElseGet(
    () -> new IllegalStateException())); // NO COMPILA
```

La variable `opt` es de tipo `Optional<Double>`, lo que implica que el `Supplier` debe retornar un valor de tipo `Double`. Dado que este `Supplier` retorna una excepción, los tipos no coinciden.

El último ejemplo con `Optional` es directo. Se analiza el comportamiento del siguiente bloque:

```Java
Optional<Double> opt = average(90, 100);
System.out.println(opt.orElse(Double.NaN));
System.out.println(opt.orElseGet(() -> Math.random()));
System.out.println(opt.orElseThrow());
```

Se imprime `95.0` tres veces. Dado que el valor sí existe, no se ejecuta la lógica de contingencia ("_or else_").

> ### ¿Es `Optional` lo mismo que `null`?
>
Una alternativa al uso de `Optional` consiste en retornar `null`. Sin embargo, esta aproximación presenta varias deficiencias. Una de ellas es la ausencia de una forma clara de expresar que `null` representa un estado o valor especial. En cambio, retornar un `Optional` declara explícitamente en la API la posibilidad de que no exista un valor.
>
Otra ventaja de `Optional` es la capacidad de emplear un estilo de **programación funcional** mediante `ifPresent()` y otros métodos, evitando sentencias `if`. Por último, se observa hacia el final del capítulo que es posible realizar **encadenamiento de llamadas** (_method chaining_) con `Optional`.