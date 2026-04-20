No debería sorprender que las computadoras sean buenas computando números. Java viene con una poderosa clase `Math` con muchos métodos para facilitar la vida. Aquí solo se cubren algunos métodos comunes que tienen más probabilidades de aparecer en el examen. Cuando se realicen proyectos propios, se debe consultar el Javadoc de `Math` para ver qué otros métodos pueden ser de ayuda. Adicionalmente, en esta sección se cubren las clases `BigInteger` y `BigDecimal`.

Se debe prestar especial atención a los tipos de retorno en las preguntas de matemáticas. ¡Son una excelente oportunidad para los trucos!

### Encontrar el mínimo y el máximo

Los métodos `Math.min()` y `Math.max()` comparan dos valores y devuelven uno de ellos.

Las firmas de los métodos para `Math.min()` son las siguientes:

```Java
public static double min(double a, double b)
public static float min(float a, float b)
public static int min(int a, int b)
public static long min(long a, long b)
```

Hay cuatro métodos sobrecargados, por lo que siempre se tiene una API disponible con el mismo tipo. Cada método devuelve el menor de `a` o `b`. El método `max()` funciona de la misma manera, excepto que devuelve el valor mayor.

Lo siguiente muestra cómo usar estos métodos:

```Java
int first = Math.max(3, 7);   // 7
int second = Math.min(7, -9); // -9
```

La primera línea devuelve 7 porque es mayor. La segunda línea devuelve -9 porque es menor. Recuerde de la escuela que los valores negativos son menores que los positivos.

### Redondeo de números

El método `Math.round()` se deshace de la parte decimal del valor, eligiendo el siguiente número mayor si es apropiado. Si la parte fraccionaria es `.5` o superior, se redondea hacia arriba.

Las firmas de los métodos para `Math.round()` son las siguientes:

```Java
public static long round(double num)
public static int round(float num)
```

Hay dos métodos sobrecargados para asegurar que haya suficiente espacio para almacenar un `double` redondeado si es necesario. Lo siguiente muestra cómo usar este método:

```Java
long low = Math.round(123.45);        // 123
long high = Math.round(123.50);       // 124
int fromFloat = Math.round(123.45f);  // 123
```

La primera línea devuelve 123 porque `.45` es menor que un medio. La segunda línea devuelve 124 porque la parte fraccionaria es apenas un medio. La línea final muestra que un `float` explícito activa la firma del método que devuelve un `int`.

### Determinación del techo (_Ceil_) y el piso (_Floor_)

El método `Math.ceil()` toma un valor `double`. Si es un número entero, devuelve el mismo valor. Si tiene algún valor fraccionario, redondea hacia arriba al siguiente número entero. Por el contrario, el método `Math.floor()` descarta cualquier valor después del decimal.

Las firmas de los métodos son las siguientes:

```Java
public static double ceil(double num)
public static double floor(double num)
```

Lo siguiente muestra cómo usar estos métodos:

```Java
double c = Math.ceil(3.14);  // 4.0
double f = Math.floor(3.14); // 3.0
```

La primera línea devuelve 4.0 porque cuatro es el entero apenas mayor. La segunda línea devuelve 3.0 porque es el entero apenas menor.

### Cálculo de exponentes

El método `Math.pow()` maneja exponentes. Como se podrá recordar de la clase de matemáticas de la escuela primaria, $3^2$ significa tres al cuadrado. Esto es $3 \times 3$ o 9. También se permiten exponentes fraccionarios. Dieciséis a la potencia de 0.5 significa la raíz cuadrada de 16, que es 4. (No hay de qué preocuparse, no habrá que hacer raíces cuadradas en el examen).

La firma del método es la siguiente:

```Java
public static double pow(double number, double exponent)
```

Lo siguiente muestra cómo usar este método:

```Java
double squared = Math.pow(5, 2); // 25.0
```

Note que el resultado es 25.0 en lugar de 25 ya que es un `double`. Una vez más, no hay de qué preocuparse; el examen no pedirá hacer matemáticas complicadas.

### Generación de números aleatorios

El método `Math.random()` devuelve un valor mayor o igual a 0 y menor que 1. La firma del método es la siguiente:

```Java
public static double random()
```

Lo siguiente muestra cómo usar este método:

```Java
double num = Math.random();
```

Dado que es un número aleatorio, no se puede saber el resultado de antemano. Sin embargo, se pueden descartar ciertos números. Por ejemplo, no puede ser negativo porque eso es menor que 0. No puede ser 1.0 porque eso no es menor que 1.

Aunque no está en el examen, es común usar la clase `Random` para generar números pseudoaleatorios. Permite generar números de diferentes tipos.

### Uso de `BigInteger` y `BigDecimal`

Todas las API de `Math` en la sección anterior utilizaban tipos primitivos de Java. Sin embargo, estos no siempre son lo suficientemente precisos. Especialmente cuando se trata de dinero o números grandes. Afortunadamente, Java tiene clases incorporadas llamadas `BigInteger` y `BigDecimal` que pueden manejar valores que no caben en los tipos numéricos primitivos. Al igual que `String`, estas clases son inmutables, por lo que se encadenan métodos para realizar múltiples operaciones.

Si bien existen constructores, se recomienda utilizar el método `valueOf()` donde sea posible. Note que se puede pasar un `long` a cualquiera de los dos tipos, pero un `double` solo a `BigDecimal`.

```Java
var bigInt = BigInteger.valueOf(5_000L);
var bigDecimal = BigDecimal.valueOf(5_000L);
bigDecimal = BigDecimal.valueOf(5_000.00);
```

Ambas clases proporcionan constantes para los valores más comunes, como `BigInteger.ZERO` y `BigDecimal.ONE`.

Hay métodos para realizar operaciones matemáticas con estos tipos, tales como:

```Java
var bigInt = BigInteger.valueOf(199)
    .add(BigInteger.valueOf(1))
    .divide(BigInteger.TEN)
    .max(BigInteger.valueOf(6));
System.out.println(bigInt); // 20
```

Este ejemplo comienza sumando 199 y 1, lo que da 200. Luego divide por 10, resultando en 20. Finalmente, `max()` ve que 20 es mayor que 6 y se obtiene el resultado.

### Escenario del mundo real

#### Cuándo usar `BigInteger` y `BigDecimal`

En el mundo real, se usaría `BigInteger` para manejar valores enteros que no caben dentro de un `int` o un `long`, como en el siguiente ejemplo.

```Java
System.out.println(new BigInteger("12345123451234512345"));
System.out.println(12345123451234512345L); // NO COMPILA
```

Asimismo, `BigDecimal` es para valores más grandes que no caben dentro de `float` o `double`. A menudo se utiliza para valores pequeños que involucran dinero. ¿Por qué? Bueno, a veces los números de punto flotante se almacenan en la memoria de formas inesperadas. Considere el siguiente ejemplo.

```Java
double amountInCents1 = 64.1 * 100;
System.out.println(amountInCents1); // 6409.999999999999
```

La diferencia entre el valor esperado y el valor real se conoce como el **error de punto flotante**. A menudo, estos errores no cambian significativamente el resultado de una operación, pero ¡sería malo hacerlo cuando se trabaja con dinero! Se puede arreglar esto usando `BigDecimal` en su lugar.

```Java
BigDecimal amountInCents2 = BigDecimal.valueOf(64.1)
    .multiply(BigDecimal.valueOf(100));
System.out.println(amountInCents2); // 6410.0
```