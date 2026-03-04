A continuación, se pasa a los operadores que toman dos operandos, denominados **operadores binarios**. Los operadores binarios son, con diferencia, los más comunes en el lenguaje Java. Pueden utilizarse para realizar operaciones matemáticas en variables, crear expresiones lógicas y realizar asignaciones básicas de variables.

Los operadores binarios se combinan a menudo en expresiones complejas con otros operadores binarios; por lo tanto, la precedencia de operadores es muy importante al evaluar expresiones que contienen operadores binarios. En esta sección, se comienza con los operadores aritméticos binarios; se ampliará a otros operadores binarios en secciones posteriores.
### Operadores aritméticos

Los operadores aritméticos son aquellos que operan sobre valores numéricos. Se muestran en la Tabla 2.4.

**TABLA 2.4** Operadores aritméticos binarios

|**Operador**|**Ejemplo**|**Descripción**|
|---|---|---|
|**Suma**|`a + b`|Suma dos valores numéricos.|
|**Resta**|`c - d`|Resta dos valores numéricos.|
|**Multiplicación**|`e * f`|Multiplica dos valores numéricos.|
|**División**|`g / h`|Divide un valor numérico por otro.|
|**Módulo**|`i % j`|Devuelve el **resto** después de la división de un valor numérico por otro.|

Se deberían conocer todos excepto el módulo desde las matemáticas básicas. Si se desconoce qué es el módulo, no hay motivo de preocupación; se cubrirá en breve.

Los operadores aritméticos también incluyen los operadores unarios `++` y `--`, los cuales ya se cubrieron. Como es posible que se haya notado en la Tabla 2.1, los operadores multiplicativos (`*`, `/`, `%`) tienen un orden de precedencia mayor que los operadores aditivos (`+`, `-`). Observe la siguiente expresión:

```Java
int price = 2 * 5 + 3 * 4 - 8;
```

Primero, se evalúan `2 * 5` y `3 * 4`, lo que reduce la expresión a esto:

```Java
int price = 10 + 12 - 8;
```

Luego, se evalúan los términos restantes en orden de izquierda a derecha, resultando en que el valor de `price` sea 14. Se debe asegurar la comprensión de por qué el resultado es 14, ya que es probable que aparezca este tipo de pregunta sobre precedencia de operadores en el examen.

Todos los operadores aritméticos pueden aplicarse a cualquier primitivo de Java, con la excepción de `boolean`. Además, solo los operadores de suma `+` y `+=` pueden aplicarse a valores `String`, lo que resulta en la **concatenación** de cadenas. Se aprenderá más sobre estos operadores y cómo se aplican a valores `String` en el Capítulo 4, «APIs principales».

#### Adición de paréntesis

Es posible que se haya notado que se dijo «A menos que se anule con paréntesis» antes de presentar la Tabla 2.1 sobre precedencia de operadores. Esto se debe a que se puede cambiar el orden de operación explícitamente envolviendo con paréntesis las secciones que se desea evaluar primero.

##### Cambio del orden de operación

Se retomará el ejemplo anterior de `price`. El siguiente fragmento de código contiene los mismos valores y operadores, en el mismo orden, pero con dos conjuntos de paréntesis añadidos:

```Java
int price = 2 * ((5 + 3) * 4 - 8);
```

Esta vez se evaluaría el operador de suma `5 + 3`, lo que reduce la expresión a lo siguiente:

```Java
int price = 2 * (8 * 4 - 8);
```

Se puede reducir aún más esta expresión multiplicando los dos primeros valores dentro de los paréntesis.

```Java
int price = 2 * (32 - 8);
```

A continuación, se restan los valores dentro de los paréntesis antes de aplicar los términos fuera de ellos.

```Java
int price = 2 * 24;
```

Finalmente, se multiplicaría el resultado por 2, resultando en un valor de 48 para `price`.

Los paréntesis pueden aparecer en casi cualquier pregunta del examen que involucre valores numéricos, por lo que se debe asegurar la comprensión de cómo cambian el orden de operación al observarlos.

Cuando se encuentre código en la carrera profesional en el que no se esté seguro sobre el orden de operación, se recomienda añadir paréntesis opcionales. Aunque a menudo no son requeridos, pueden mejorar la legibilidad, especialmente como se verá con los operadores ternarios.

##### Verificación de la sintaxis de paréntesis

Al trabajar con paréntesis, se debe asegurar que sean siempre válidos y estén equilibrados. Considere los siguientes ejemplos:

```Java
long pigeon = 1 + ((3 * 5) / 3; // NO COMPILA
int blueJay = (9 + 2) + 3) / (2 * 4; // NO COMPILA
```

El primer ejemplo no compila porque los paréntesis no están equilibrados. Existe un paréntesis izquierdo sin su correspondiente paréntesis derecho. El segundo ejemplo tiene un número igual de paréntesis izquierdos y derechos, pero no están equilibrados adecuadamente. Al leer de izquierda a derecha, un nuevo paréntesis derecho debe coincidir con un paréntesis izquierdo previo. Del mismo modo, todos los paréntesis izquierdos deben cerrarse con paréntesis derechos antes del final de la expresión.

Se probará con otro ejemplo:

```Java
short robin = 3 + [(4 * 2) + 4]; // NO COMPILA
```

Este ejemplo no compila porque Java, a diferencia de otros lenguajes de programación, no permite el uso de corchetes, `[]`, en lugar de paréntesis. Si se reemplazan los corchetes por paréntesis, el ejemplo anterior compilará perfectamente.

#### Operadores de división y módulo

Como se mencionó anteriormente, el operador de módulo, `%`, puede resultar nuevo. El operador de módulo, a veces llamado operador de resto, es simplemente el resto cuando dos números se dividen. Por ejemplo, 9 dividido por 3 se divide uniformemente y no tiene resto; por lo tanto, el resultado de `9 % 3` es 0. Por otro lado, 11 dividido por 3 no se divide uniformemente; por lo tanto, el resultado de `11 % 3` es 2.

Los siguientes ejemplos ilustran esta distinción:

```Java
System.out.println(9 / 3); // 3
System.out.println(9 % 3); // 0
System.out.println(10 / 3); // 3
System.out.println(10 % 3); // 1
System.out.println(11 / 3); // 3
System.out.println(11 % 3); // 2
System.out.println(12 / 3); // 4
System.out.println(12 % 3); // 0
```

Como se puede observar, los resultados de la división aumentan solo cuando el valor del lado izquierdo pasa de 11 a 12, mientras que el valor del resto del módulo aumenta en 1 cada vez que se incrementa el lado izquierdo hasta que vuelve a cero. Para un divisor `y` dado, la operación de módulo resulta en un valor entre 0 y `(y - 1)` para dividendos positivos, o 0, 1, 2 en este ejemplo.

Se debe asegurar la comprensión de la diferencia entre la división aritmética y el módulo. Para valores enteros, la división resulta en el valor suelo (_floor value_) del entero más cercano que cumple con la operación, mientras que el módulo es el valor del resto.

Si se escucha la frase **valor suelo** (_floor value_), simplemente significa el valor sin nada después del punto decimal. Por ejemplo, el valor suelo es 4 para cada uno de los valores 4.0, 4.5 y 4.9999999. A diferencia del redondeo, que se cubrirá en el Capítulo 4, simplemente se toma el valor antes del punto decimal, independientemente de lo que haya después.

También es posible utilizar el módulo con números negativos. Si el divisor es negativo, entonces el signo negativo se ignora. Sin embargo, los valores negativos sí cambian el comportamiento del módulo cuando se aplican al dividendo. Por ejemplo, si el divisor es 5, entonces el valor del módulo de un número negativo está entre -4 y 0. El signo del resultado **siempre es igual al signo del dividendo**.

- `Negativo % Positivo = Negativo`
- `Positivo % Negativo = Positivo`

Los siguientes ejemplos muestran cómo funciona esto:

```Java
System.out.println(2 % 5); // 2
System.out.println(7 % 5); // 2
System.out.println(2 % -5); // 2
System.out.println(7 % -5); // 2
System.out.println(-2 % 5); // -2
System.out.println(-7 % 5); // -2
System.out.println(-2 % -5); // -2
System.out.println(-7 % -5); // -2
```

La operación de módulo también puede aplicarse a números de punto flotante, aunque eso está fuera del alcance del examen.

Para no tener que hacer la fórmula siempre, usa esta regla:

> **Si el número de la izquierda (dividendo) es MENOR que el de la derecha (divisor), el resultado es siempre el número de la izquierda.**

- `1 % 2 = 1`
- `3 % 5 = 3`
- `10 % 100 = 10`

### Promoción numérica

Ahora que se comprenden los fundamentos de los operadores aritméticos, es vital hablar sobre la promoción numérica primitiva, ya que Java puede realizar acciones que parezcan inusuales al principio. Como se mostró en el Capítulo 1, «Bloques de construcción», cada tipo numérico primitivo tiene una longitud de bits. No es necesario conocer el tamaño exacto de estos tipos para el examen, sin embargo, se debe saber cuáles son más grandes que otros. Por ejemplo, se debe saber que un `long` ocupa más espacio que un `int`, que a su vez ocupa más espacio que un `short`, y así sucesivamente.

Se deben memorizar ciertas reglas que Java seguirá al aplicar operadores a los tipos de datos.

#### Reglas de promoción numérica

1. Si dos valores tienen diferentes tipos de datos, Java promoverá automáticamente uno de los valores al **mayor** de los dos tipos de datos.
2. Si uno de los valores es integral y el otro es de punto flotante, Java promoverá automáticamente el valor integral al tipo de datos del valor de punto flotante.
3. Los tipos de datos más pequeños, a saber, `byte`, `short` y `char`, se promueven primero a `int` cada vez que se usan con un operador aritmético binario de Java con una variable (a diferencia de un valor literal), incluso si ninguno de los operandos es `int`.
4. Después de que haya ocurrido toda la promoción y los operandos tengan el mismo tipo de datos, el valor resultante tendrá el mismo tipo de datos que sus operandos promovidos.

Las dos últimas reglas son con las que la mayoría de las personas tiene problemas y las que probablemente causen errores en el examen. Para la tercera regla, note que los operadores unarios de incremento/decremento están excluidos. Por ejemplo, aplicar `++` a un valor `short` resulta en un valor `short`.

Se abordarán algunos ejemplos con fines ilustrativos.

**¿Cuál es el tipo de datos de `x * y`?**

```Java
int x = 1;
long y = 33;
var z = x * y;
```

En este caso, se sigue la primera regla. Dado que uno de los valores es `int` y el otro es `long`, y dado que `long` es mayor que `int`, el valor `int` de `x` se promueve primero a `long`. El resultado `z` es entonces un valor `long`.

**¿Cuál es el tipo de datos de `x + y`?**

```Java
double x = 39.21;
float y = 2.1;
var z = x + y;
```

¡Esta es en realidad una pregunta con trampa, ya que la segunda línea no compila! Como se recordará del Capítulo 1, se asume que los literales de punto flotante son `double` a menos que tengan el sufijo `f`, como en `2.1f`. Si el valor de `y` se estableciera correctamente en `2.1f`, entonces la promoción sería similar al ejemplo anterior, con ambos operandos promovidos a `double`, y el resultado `z` sería un valor `double`.

**¿Cuál es el tipo de datos de `x * y`?**

```Java
short x = 10;
short y = 3;
var z = x * y;
```

En la última línea, se debe aplicar la tercera regla: `x` e `y` serán promovidos a `int` antes de la operación de multiplicación binaria, resultando en una salida de tipo `int`. Si se intentara asignar el valor a una variable `short` `z` sin conversión (_casting_), el código no compilaría. Se debe prestar mucha atención al hecho de que la salida resultante **no** es un `short`.

**¿Cuál es el tipo de datos de `w * x / y`?**

```Java
short w = 14;
float x = 13;
double y = 30;
var z = w * x / y;
```

En este caso, se deben aplicar todas las reglas. Primero, `w` se promoverá automáticamente a `int` únicamente porque es un `short` y se está utilizando en una operación binaria aritmética. El valor promovido de `w` luego se promoverá automáticamente a `float` para que pueda multiplicarse con `x`. El resultado de `w * x` luego se promoverá automáticamente a `double` para que pueda dividirse por `y`, resultando en un valor `double`.

Al trabajar con operadores aritméticos en Java, siempre se debe estar al tanto del tipo de datos de las variables, los valores intermedios y los valores resultantes. Se debe aplicar la precedencia de operadores y paréntesis y trabajar hacia afuera, promoviendo los tipos de datos a lo largo del camino. En la siguiente sección, se discutirán las complejidades de asignar estos valores a variables de un tipo particular.