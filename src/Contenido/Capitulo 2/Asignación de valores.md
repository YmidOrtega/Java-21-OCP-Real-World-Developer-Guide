A menudo se pasan por alto los errores de compilación derivados de los operadores de asignación en el examen, en parte debido a lo sutiles que pueden ser estos errores. Para tener éxito con los operadores de asignación, se debe comprender con fluidez cómo el compilador maneja la promoción numérica y cuándo se requiere la conversión explícita (_casting_). Ser capaz de detectar estos problemas es crítico para aprobar el examen, ya que los operadores de asignación aparecen en casi todas las preguntas con fragmentos de código.

### Operador de asignación

Un **operador de asignación** es un operador binario que modifica, o asigna, la variable del lado izquierdo del operador con el resultado del valor del lado derecho de la ecuación. A diferencia de la mayoría de los otros operadores de Java, el operador de asignación se evalúa de **derecha a izquierda**.

El operador de asignación más simple es la asignación (`=`), que ya se ha visto:

```Java
int herd = 1;
```

Esta sentencia asigna a la variable `herd` el valor de 1.

Java promoverá automáticamente de tipos de datos más pequeños a más grandes, como se observó en la sección anterior sobre operadores aritméticos, pero lanzará una excepción de compilación si detecta que se está intentando convertir de tipos de datos más grandes a más pequeños sin _casting_. La Tabla 2.5 enumera el primer operador de asignación que es necesario conocer para el examen. Se presentarán operadores de asignación adicionales más adelante en esta sección.

**TABLA 2.5** Operador de asignación simple

|**Operador**|**Ejemplo**|**Descripción**|
|---|---|---|
|**Asignación**|`int a = 50;`|Asigna el valor de la derecha a la variable de la izquierda.|
### Conversión de valores (_Casting_)

Podría parecer sencillo hasta ahora. Sin embargo, no se puede hablar realmente del operador de asignación en detalle hasta haber cubierto el _casting_. El **casting** (conversión explícita) es una operación unaria donde un tipo de dato se interpreta explícitamente como otro tipo de dato. El _casting_ es opcional e innecesario cuando se convierte a un tipo de dato más grande o de ensanchamiento, pero es **requerido** cuando se convierte a un tipo de dato más pequeño o de estrechamiento. Sin _casting_, el compilador generará un error al intentar colocar un tipo de dato más grande dentro de uno más pequeño.

El _casting_ se realiza colocando el tipo de dato, encerrado entre paréntesis, a la izquierda del valor que se desea convertir. He aquí algunos ejemplos de _casting_:

```Java
int fur = (int)5;
int hair = (short) 2;
String type = (String) "Bird";
short tail = (short)(4 + 10);
long feathers = 10(long); // NO COMPILA
```

Los espacios entre el _cast_ y el valor son opcionales. Como se muestra en el penúltimo ejemplo, es común que el lado derecho también esté entre paréntesis. Dado que el _casting_ es una operación unaria, solo se aplicaría al 4 si no se encerrara `4 + 10` entre paréntesis. El último ejemplo no compila porque el tipo está en el lado incorrecto del valor.

Por un lado, es conveniente que el compilador convierta automáticamente tipos de datos más pequeños a más grandes. Por otro lado, esto genera excelentes preguntas de examen cuando hacen lo contrario para ver si se está prestando atención. Intente deducir por qué ninguna de las siguientes líneas de código compila:

```Java
float egg = 2.0 / 9; // NO COMPILA
int tadpole = (int)5 * 2L; // NO COMPILA
short frog = 3 - 2.0; // NO COMPILA
```

Todos estos ejemplos implican colocar un valor más grande en un tipo de dato más pequeño. No hay motivo de preocupación si aún no se comprende del todo; se cubrirán más ejemplos como estos en breve.

En este capítulo, el _casting_ se refiere principalmente a la conversión de tipos de datos numéricos en otros tipos de datos. Como se verá en capítulos posteriores, el _casting_ también puede aplicarse a objetos y referencias. En esos casos, sin embargo, no se realiza ninguna conversión de datos. En pocas palabras, convertir un valor numérico puede cambiar el tipo de dato, mientras que convertir un objeto solo cambia la referencia al objeto, no el objeto en sí.

### Revisión de asignaciones primitivas

Intente determinar por qué cada una de las siguientes líneas no compila:

```Java
int fish = 1.0; // NO COMPILA
short bird = 1921222; // NO COMPILA
int mammal = 9f; // NO COMPILA
long reptile = 192_301_398_193_810_323; // NO COMPILA
```

La primera sentencia no compila porque se está intentando asignar un `double` 1.0 a un valor entero. Aunque el valor es un entero matemático, al añadir `.0`, se instruye al compilador para que lo trate como un `double`.

La segunda sentencia no compila porque el valor literal `1921222` está fuera del rango de `short`, y el compilador detecta esto. La tercera sentencia no compila porque la `f` añadida al final del número instruye al compilador para que trate el número como un valor de punto flotante, pero la asignación es a un `int`. Finalmente, la última sentencia no compila porque Java interpreta el literal como un `int` y nota que el valor es mayor de lo que `int` permite. El literal necesitaría un sufijo `L` o `l` para ser considerado un `long`.

### Aplicación del _Casting_

Es posible corregir tres de los ejemplos anteriores convirtiendo (_casting_) los resultados a un tipo de dato más pequeño. Recuerde que la conversión de primitivos es requerida siempre que se pase de un tipo de dato numérico más grande a uno más pequeño, o al convertir de un número de punto flotante a un valor integral.

```Java
int fish = (int)1.0;
short bird = (short)1921222; // Almacenado como 20678
int mammal = (int)9f;
```

¿Qué sucede al aplicar _casting_ a un ejemplo anterior?

```Java
long reptile = (long)192301398193810323; // NO COMPILA
```

Esto sigue sin compilar porque el valor se interpreta primero como un `int` por el compilador y está fuera de rango. Lo siguiente corrige este código sin requerir _casting_:

```Java
long reptile = 192301398193810323L;
```

Se retomará un ejemplo similar de la sección «Promoción numérica» anterior en este capítulo.

```Java
short mouse = 10;
short hamster = 3;
short capybara = mouse * hamster; // NO COMPILA
```

Basándose en todo lo aprendido hasta ahora sobre la promoción numérica y el _casting_, ¿se comprende por qué la última línea de esta sentencia no compila? Como se recordará, los valores `short` se promueven automáticamente a `int` al aplicar cualquier operador aritmético, resultando en un valor de tipo `int`. Intentar asignar un valor `int` a una variable `short` resulta en un error de compilación, ya que Java interpreta que se está intentando convertir implícitamente de un tipo de dato más grande a uno más pequeño.

Es posible corregir esta expresión mediante _casting_, ya que hay ocasiones en las que se desea anular el comportamiento predeterminado del compilador. En este ejemplo, se sabe que el resultado de `10 * 3` es 30, el cual cabe fácilmente en una variable `short`, por lo que se puede aplicar _casting_ para convertir el resultado nuevamente a `short`:

```Java
short mouse = 10;
short hamster = 3;
short capybara = (short)(mouse * hamster);
```

Al convertir un valor más grande a un tipo de dato más pequeño, se instruye al compilador para que ignore su comportamiento predeterminado. En otras palabras, se le indica al compilador que se han tomado medidas adicionales para prevenir el desbordamiento o subdesbordamiento. También es posible que, en una aplicación y escenario particular, el desbordamiento resulte en valores aceptables.

Por último, el _casting_ puede aparecer en cualquier lugar de una expresión, no solo en la asignación. Por ejemplo, observe una forma modificada del ejemplo anterior:

```Java
short mouse = 10;
short hamster = 3;
short capybara = (short)mouse * hamster; // NO COMPILA
```

Entonces, ¿qué sucede en la última línea? Recuerde que se mencionó que el _casting_ es una operación unaria. Esto significa que el _cast_ en la última línea se aplica a `mouse`, y solo a `mouse`. Una vez completado el _cast_, ambos operandos se promueven a `int` dado que se utilizan con el operador de multiplicación binaria (`*`), haciendo que el resultado sea un `int` y provocando un error de compilación.

¿Qué sucedería si se cambiara la última línea a lo siguiente?

```Java
short capybara = 1 + (short)(mouse * hamster); // NO COMPILA
```

En este ejemplo, el _casting_ se realiza con éxito, pero el valor resultante se promueve automáticamente a `int` porque se utiliza con el operador aritmético binario (`+`).
### Desbordamiento y subdesbordamiento (_Overflow and Underflow_)

Se observa que las expresiones en el ejemplo anterior ahora compilan, aunque esto conlleva un costo. El segundo valor, `1,921,222`, es demasiado grande para almacenarse como `short`, por lo que ocurre un **desbordamiento numérico** (_numeric overflow_), convirtiéndose en `20,678`.

El desbordamiento ocurre cuando un número es tan grande que ya no cabe dentro del tipo de dato, por lo que el sistema «da la vuelta» (_wraps around_) al valor negativo más bajo y cuenta hacia arriba desde allí, de manera similar a cómo funciona la aritmética modular. También existe un **subdesbordamiento** o desbordamiento negativo (_underflow_) análogo, cuando el número es demasiado bajo para caber en el tipo de dato, como al intentar almacenar -200 en un campo `byte`.

Esto está fuera del alcance del examen, pero es algo con lo que se debe tener cuidado en el código propio. Por ejemplo, la siguiente sentencia imprime un número negativo:

```Java
System.out.print(2147483647 + 1); // -2147483648
```

Dado que `2147483647` es el valor máximo de `int`, agregar cualquier valor estrictamente positivo hará que se desborde al número negativo más pequeño.

### Conversión de valores vs a variables

Al revisar la tercera regla de promoción numérica, el compilador no requiere _casting_ cuando se trabaja con valores literales que caben en el tipo de dato. Considere estos ejemplos:

```Java
byte hat = 1;
byte gloves = 7 * 10;
short scarf = 5;
short boots = 2 + 1;
```

Todas estas sentencias compilan sin problemas. Por otro lado, ninguna de las siguientes sentencias compila:

```Java
short boots = 2 + hat; // NO COMPILA
byte gloves = 7 * 100; // NO COMPILA
```

La primera sentencia no compila porque `hat` es una variable, no un valor literal, y ambos operandos se promueven automáticamente a `int`. Cuando se trabaja con valores literales, el compilador tiene suficiente información para determinar la intención del escritor. Sin embargo, cuando se trabaja con variables, existe ambigüedad sobre cómo proceder, por lo que el compilador reporta un error. La segunda expresión no compila porque `700` provoca un desbordamiento para `byte`, el cual tiene un valor máximo de 127.

### Operadores de asignación compuesta

Además del operador de asignación simple (`=`), Java soporta numerosos operadores de asignación compuesta. Para el examen, se debe estar familiarizado con los operadores compuestos de la Tabla 2.6.

**TABLA 2.6** Operadores de asignación compuesta

|**Operador**|**Ejemplo**|**Descripción**|
|---|---|---|
|**Asignación de suma**|`a += 5`|Suma el valor de la derecha a la variable de la izquierda y asigna la suma a la variable.|
|**Asignación de resta**|`b -= 0.2`|Resta el valor de la derecha de la variable de la izquierda y asigna la diferencia a la variable.|
|**Asignación de multiplicación**|`c *= 100`|Multiplica el valor de la derecha con la variable de la izquierda y asigna el producto a la variable.|
|**Asignación de división**|`d /= 4`|Divide la variable de la izquierda por el valor de la derecha y asigna el cociente a la variable.|

Los operadores compuestos son, en realidad, formas extendidas del operador de asignación simple, con una operación aritmética o lógica incorporada que aplica los lados izquierdo y derecho de la sentencia y almacena el valor resultante en la variable del lado izquierdo. Por ejemplo, las siguientes dos sentencias después de la declaración de `camel` y `giraffe` son equivalentes cuando se ejecutan independientemente:

```Java
int camel = 2, giraffe = 3;
camel = camel * giraffe; // Operador de asignación simple
camel *= giraffe;        // Operador de asignación compuesta
```

El lado izquierdo del operador compuesto solo puede aplicarse a una variable que ya esté definida y no puede utilizarse para declarar una nueva variable. En este ejemplo, si `camel` no estuviera ya definida, la expresión `camel *= giraffe` no compilaría.

Los operadores compuestos son útiles para algo más que una simple abreviatura: también pueden evitar la necesidad de convertir (_cast_) explícitamente un valor. Por ejemplo, considere lo siguiente. ¿Es posible deducir por qué la última línea no compila?

```Java
long goat = 10;
int sheep = 5;
sheep = sheep * goat; // NO COMPILA
```

De la sección anterior, se debería poder detectar el problema en la última línea. Se está intentando asignar un valor `long` a una variable `int`. Esta última línea podría corregirse con un _cast_ explícito a `(int)`, pero existe una mejor manera utilizando el operador de asignación compuesta:

```Java
long goat = 10;
int sheep = 5;
sheep *= goat;
```

El operador compuesto primero convertirá `sheep` a `long`, aplicará la multiplicación de dos valores `long`, y luego convertirá (_cast_) el resultado a `int`. A diferencia del ejemplo anterior, en el cual el compilador reportó un error, el compilador convertirá automáticamente el valor resultante al tipo de dato del valor en el lado izquierdo del operador compuesto.

### Valor de retorno de los operadores de asignación

Un último aspecto que se debe conocer sobre los operadores de asignación es que el resultado de una asignación es una expresión en sí misma igual al valor de la asignación. Por ejemplo, el siguiente fragmento de código es perfectamente válido, aunque parezca un poco extraño:

```Java
long wolf = 5;
long coyote = (wolf = 3);
System.out.println(wolf);   // 3
System.out.println(coyote); // 3
```

La clave aquí es que `(wolf = 3)` hace dos cosas. Primero, establece el valor de la variable `wolf` en 3. Segundo, devuelve el valor de la asignación, que también es 3.

Los creadores del examen son aficionados a insertar el operador de asignación (`=`) en medio de una expresión y utilizar el valor de la asignación como parte de una expresión más compleja. Por ejemplo, no debe sorprender si aparece una sentencia `if` en el examen similar a la siguiente:

```Java
boolean healthy = false;
if(healthy = true)
    System.out.print("Good!");
```

Si bien esto puede parecer una prueba de si `healthy` es verdadero, en realidad se está asignando a `healthy` un valor de `true`. El resultado de la asignación es el valor de la asignación, que es `true`, resultando en que este fragmento imprima `Good!`. Esto se cubrirá con más detalle en la próxima sección de "Operadores de igualdad".