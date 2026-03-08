Aunque las sentencias `while` y `do/while` son bastante poderosas, algunas tareas son tan comunes al escribir software que se crearon tipos especiales de bucles; por ejemplo, iterar sobre una sentencia exactamente 10 veces o iterar sobre una lista de nombres. Se podrían lograr fácilmente estas tareas con varios bucles `while` que se han visto hasta ahora, pero generalmente requieren mucho código repetitivo. ¿No sería genial si existiera una estructura de bucle que pudiera hacer lo mismo en una sola línea de código?

Con eso, se presenta la estructura de control de repetición más conveniente, los bucles `for`. Hay dos tipos de bucles `for`, aunque ambos usan la misma palabra clave `for`. El primero se conoce como el bucle `for` básico, y el segundo a menudo se llama el bucle `for` mejorado. Para mayor claridad, se hará referencia a ellos como el bucle `for` y el bucle `for-each`, respectivamente, a lo largo del proyecto.

#### El bucle `for`

Un bucle `for` básico tiene la misma expresión de condición de terminación que los bucles `while`, así como dos nuevas secciones: un **bloque de inicialización** y una **sentencia de actualización**. La imagen a continuación muestra cómo se disponen estos componentes.

![[La estructura de un bucle for básico.png]]

Aunque la anterior imagen pueda parecer un poco confusa y casi arbitraria al principio, la organización de los componentes y el flujo permiten crear sentencias extremadamente poderosas en una sola línea que de otra manera tomarían múltiples líneas con un bucle `while`. Cada una de las tres secciones está separada por un punto y coma (`;`). Además, las secciones de inicialización y actualización pueden contener múltiples sentencias, separadas por comas (`,`).

Las variables declaradas en el bloque de inicialización de un bucle `for` tienen un **alcance limitado** y son accesibles **solo dentro** del bucle `for`. Se debe tener cuidado con cualquier pregunta de examen en la que una variable se declare dentro del bloque de inicialización de un bucle `for` y luego se lea fuera del bucle. Por ejemplo, este código no compila porque se hace referencia a la variable del bucle `i` fuera del bucle:

```Java
for (int i = 0; i < 10; i++)
    System.out.println("Value is: " + i);
System.out.println(i); // NO COMPILA
```

Alternativamente, las variables declaradas antes del bucle `for` y a las que se les asigna un valor en el bloque de inicialización pueden usarse fuera del bucle `for` porque su alcance precede a la creación del bucle `for`.

```Java
int i;
for (i = 0; i < 10; i++)
    System.out.println("Value is: " + i);
System.out.println(i);
```

Se examinará un ejemplo que imprime los primeros cinco números, comenzando con cero:

```Java
for (int i = 0; i < 5; i++) {
    System.out.print(i + " ");
}
```

La variable local `i` se inicializa primero en 0. La variable `i` solo está en alcance mientras dura el bucle y no está disponible fuera del bucle una vez que este ha finalizado. Al igual que un bucle `while`, la condición booleana se evalúa en **cada** iteración del bucle antes de que el bucle se ejecute. Dado que devuelve `true`, el bucle se ejecuta y genera 0 seguido de un espacio. A continuación, el bucle ejecuta la sección de actualización, que en este caso incrementa el valor de `i` a 1. Luego, el bucle evalúa la expresión booleana por segunda vez, y el proceso se repite varias veces, imprimiendo lo siguiente:

`0 1 2 3 4`

En la quinta iteración del bucle, el valor de `i` llega a 4 y se incrementa en 1 para llegar a 5. En la sexta iteración del bucle, la expresión booleana se evalúa, y dado que `(5 < 5)` devuelve `false`, el bucle termina sin ejecutar el cuerpo del bucle.

### ¿Por qué `i` en los bucles `for`?

Es posible notar que es una práctica común nombrar `i` a la variable de un bucle `for`. Mucho antes de que existiera Java, se comenzó a utilizar `i` como abreviatura de variable de incremento (_increment variable_), y la práctica existe hoy en día, ¡incluso cuando muchos de esos lenguajes de programación ya no lo hacen! Para bucles dobles o triples, donde la `i` ya está en uso, a menudo se utilizan las siguientes letras del alfabeto, `j` y `k`.

### Impresión de elementos en orden inverso

Supóngase que se desea imprimir los mismos primeros cinco números desde cero como se hizo en la sección anterior, pero esta vez en orden inverso. El objetivo entonces es imprimir `4 3 2 1 0`.

¿Cómo se haría eso? Una implementación inicial podría verse de la siguiente manera:

```Java
for (var counter = 5; counter > 0; counter--) {
    System.out.print(counter + " ");
}
```

Aunque este fragmento genera cinco valores distintos y se asemeja al primer ejemplo de bucle `for`, no emite los mismos cinco valores. En su lugar, esta es la salida:

`5 4 3 2 1`

¡Un momento, eso no es lo que se buscaba! Se quería `4 3 2 1 0`. Comienza con 5, porque ese es el primer valor que se le asigna. Se corregirá eso comenzando con 4 en su lugar:

```Java
for (var counter = 4; counter > 0; counter--) {
    System.out.print(counter + " ");
}
```

¿Qué imprime esto ahora? Imprime lo siguiente:

`4 3 2 1`

¡Casi! El problema es que termina en 1, no en 0, porque se le indicó que saliera tan pronto como el valor no fuera estrictamente mayor que 0. Si se desea imprimir los mismos valores del 0 al 4 que en el primer ejemplo, es necesario actualizar la condición de terminación, de esta manera:

```Java
for (var counter = 4; counter >= 0; counter--) {
    System.out.print(counter + " ");
}
```

¡Finalmente! Ahora se tiene un código que imprime `4 3 2 1 0` y coincide con el reverso del ejemplo de bucle `for` de la sección anterior. Se podría haber utilizado `counter > -1` como condición de terminación del bucle en este ejemplo, aunque `counter >= 0` tiende a ser más legible.

Para el examen, será necesario saber cómo leer bucles `for` hacia adelante y hacia atrás. Cuando se observe un bucle `for` en el examen, se debe prestar mucha atención a la variable del bucle y a las operaciones si se utiliza el operador de decremento, `--`. Mientras que incrementar desde 0 en un bucle `for` a menudo es directo, decrementar tiende a ser menos intuitivo. De hecho, si se ve un bucle `for` con un operador de decremento en el examen, se debe asumir que se está intentando evaluar el conocimiento de las operaciones de bucle.

### Trabajo con bucles `for`

Aunque la mayoría de los bucles `for` que probablemente se encontrarán en la experiencia de desarrollo profesional estarán bien definidos y serán similares a los ejemplos anteriores, existe una serie de variaciones y casos límite que podrían verse en el examen. Se debe estar familiarizado con los siguientes cinco ejemplos; es probable que se vean variaciones de estos en la prueba.

#### 1. Creación de un bucle infinito

```Java
for ( ; ; )
	System.out.println("Hello World");
```

Aunque este bucle `for` pueda parecer que no compila, de hecho compilará y se ejecutará sin problemas. En realidad es un bucle infinito que imprimirá la misma sentencia repetidamente. Este ejemplo refuerza el hecho de que los componentes del bucle `for` son opcionales. Note que los puntos y coma que separan las tres secciones son **obligatorios**, ya que un `for()` sin ningún punto y coma no compilará.

#### 2. Adición de múltiples términos a la sentencia `for`

```Java
int x = 0;
for (long y = 0, z = 4; x < 5 && y < 10; x++, y++) {
    System.out.print(y + " "); 
}
System.out.print(x + " ");
```

Este código demuestra tres variaciones del bucle `for` que quizás no se hayan visto. Primero, se puede declarar una variable, como `x` en este ejemplo, antes de que comience el bucle y utilizarla después de que se complete. Segundo, el bloque de inicialización, la expresión booleana y las sentencias de actualización pueden incluir variables adicionales que pueden o no referenciarse entre sí. Por ejemplo, `z` se define en el bloque de inicialización y nunca se utiliza. Finalmente, la sentencia de actualización puede modificar múltiples variables. Este código imprimirá lo siguiente cuando se ejecute:

`0 1 2 3 4 5`

#### 3. Redeclaración de una variable en el bloque de inicialización

```Java
int x = 0;
for (int x = 4; x < 5; x++) // NO COMPILA
    System.out.print(x + " ");
```

Este ejemplo parece similar al anterior, pero no compila debido al bloque de inicialización. La diferencia es que la declaración de `x` se repite en el bloque de inicialización después de haber sido declarada antes del bucle, resultando en que el compilador se detenga debido a una **declaración de variable duplicada**. Se puede corregir este bucle eliminando la declaración de `x` del bucle `for` de la siguiente manera:

```Java
int x = 0;
for ( ; x < 5; x++)
    System.out.print(x + " ");
```

#### 4. Uso de tipos de datos incompatibles en el bloque de inicialización

```Java
int x = 0;
for (long y = 0, int z = 4; x < 5; x++) // NO COMPILA
    System.out.print(y + " ");
```

Al igual que el tercer ejemplo, este código no compilará, aunque esta vez por una razón diferente. Las variables en el bloque de inicialización deben ser **todas del mismo tipo**. En el ejemplo de múltiples términos, `y` y `z` eran ambos `long`, por lo que el código compiló sin problemas; pero en este ejemplo, tienen tipos diferentes, por lo que el código no compilará.

#### 5. Uso de variables de bucle fuera del bucle

```Java
for (long y = 0, x = 4; x < 5 && y < 10; x++, y++)
    System.out.print(y + " ");
System.out.print(x); // NO COMPILA
```

Esto ya se cubrió al comienzo de esta sección, pero es tan importante para aprobar el examen que se discute nuevamente aquí. Si se nota, `x` está definida en el bloque de inicialización del bucle y luego se utiliza después de que el bucle termina. Dado que `x` fue declarada con alcance (_scoped_) **solo para el bucle**, usarla fuera del bucle causará un error del compilador.

### Modificación de variables de bucle

Como regla general, se considera una mala práctica de codificación modificar las variables del bucle, ya que puede conducir a un resultado inesperado, como en los siguientes ejemplos:

```Java
for (int i = 0; i < 10; i++) // Bucle infinito
    i = 0;
for (int j = 1; j < 10; j++) // Itera 5 veces
    j++;
```

También tiende a hacer que el código sea difícil de seguir para otras personas.

### El bucle `for-each`

El bucle `for-each` es una estructura especializada diseñada para iterar sobre arreglos y varias clases del marco de colecciones (_Collections Framework_), como se presenta en la siguiente imagen.

![[La estructura de un bucle «for-each» mejorado.png]]

La declaración del bucle `for-each` está compuesta por una sección de inicialización y un objeto sobre el cual iterar. El lado derecho del bucle `for-each` **debe** ser uno de los siguientes:

- Un arreglo incorporado de Java.
- Un objeto cuyo tipo implemente `java.lang.Iterable`.

Se cubrirá qué significa _implementar_ en el Capítulo 7, pero por ahora solo es necesario saber que el lado derecho debe ser un arreglo o colección de elementos, como un `List` o un `Set`. Para el examen, se debe saber que esto no incluye todas las clases o interfaces del marco de colecciones, sino solo aquellas que implementan o extienden la interfaz `Collection`. Por ejemplo, `Map` **no** está soportado en un bucle `for-each`, aunque `Map` sí incluye métodos que devuelven instancias de `Collection`.

El lado izquierdo del bucle `for-each` debe incluir una declaración para una instancia de una variable cuyo tipo sea compatible con el tipo del arreglo o colección en el lado derecho de la sentencia. En cada iteración del bucle, a la variable nombrada en el lado izquierdo de la sentencia se le asigna un nuevo valor del arreglo o colección en el lado derecho de la sentencia.

Compare estos dos métodos que imprimen los valores de un arreglo, uno utilizando un bucle `for` tradicional y el otro utilizando un bucle `for-each`:

```Java
void printNames(String[] names) {
    for (int counter = 0; counter < names.length; counter++)
        System.out.println(names[counter]);
}

void printNames(String[] names) {
    for (var name : names)
        System.out.println(name);
}
```

El bucle `for-each` es mucho más corto, ¿verdad? Ya no se tiene una variable de bucle contador que se necesite crear, incrementar y monitorear. Al igual que usar un bucle `for` en lugar de un bucle `while`, los bucles `for-each` están destinados a reducir el código repetitivo, haciendo que el código sea más fácil de leer/escribir, y liberando tiempo para enfocarse en las partes del código que realmente importan.

También se puede utilizar un bucle `for-each` en un `List`, ya que implementa `Iterable`.

```Java
void printNames(List<String> names) {
    for (var name : names)
        System.out.println(name);
}
```

Se cubrirán los genéricos en detalle en el Capítulo 9, «Colecciones y genéricos». Para este capítulo, solo es necesario saber que, en cada iteración, un bucle `for-each` asigna una variable con el mismo tipo que el argumento genérico. En este caso, `name` es de tipo `String`.

Hasta aquí todo bien. ¿Qué sucede con los siguientes ejemplos?

```Java
String birds = "Jay";
for (String bird : birds) // NO COMPILA
    System.out.print(bird + " ");

String[] sloths = new String[3];
for (int sloth : sloths) // NO COMPILA
    System.out.print(sloth + " ");
```

El primer bucle `for-each` no compila porque el `String` `birds` no puede utilizarse en el lado derecho de la sentencia. Aunque un `String` puede representar una lista de caracteres, en realidad tiene que ser un arreglo o implementar `Iterable`. El segundo ejemplo no compila porque el tipo de variable de bucle en el lado izquierdo de la sentencia es `int` y no coincide con el tipo esperado de `String`.