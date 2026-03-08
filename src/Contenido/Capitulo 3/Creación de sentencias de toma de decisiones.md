Al igual que muchos lenguajes de programación, Java se compone principalmente de variables, operadores y sentencias organizadas en un orden lógico. En el capítulo anterior, se cubrió cómo crear y manipular variables.

Sin embargo, escribir software trata de algo más que gestionar variables; trata de crear aplicaciones que puedan tomar decisiones inteligentes. En este capítulo, se presentan las diversas sentencias de toma de decisiones disponibles dentro del lenguaje. Este conocimiento permitirá construir funciones complejas y estructuras de clases que se verán a lo largo de este proyecto.

### Creación de sentencias de toma de decisiones

Los operadores de Java permiten crear muchas expresiones complejas, pero están limitados en la manera en que pueden controlar el flujo del programa. Imagínese que se desea ejecutar un método solo bajo ciertas condiciones que no pueden evaluarse hasta el tiempo de ejecución. Por ejemplo, en días lluviosos, un zoológico debería recordar a los clientes que lleven un paraguas, o en un día de nieve, el zoológico podría necesitar cerrar. El software no cambia, pero el comportamiento del software debería hacerlo, dependiendo de las entradas suministradas en el momento. En esta sección, se discuten las sentencias de toma de decisiones, incluyendo `if` y `else`, junto con la coincidencia de patrones (_pattern matching_).

### Sentencias y bloques

Como tal vez se recuerde del Capítulo 1, «Bloques de construcción», una **sentencia Java** es una unidad completa de ejecución en Java, terminada con un punto y coma (`;`). En este capítulo, se presentan varias sentencias de flujo de control de Java. Las sentencias de flujo de control dividen el flujo de ejecución mediante la toma de decisiones, bucles y ramificaciones, permitiendo que la aplicación ejecute selectivamente segmentos particulares de código.

Estas sentencias pueden aplicarse tanto a expresiones individuales como a un bloque de código Java. Como se describió en el Capítulo 1, un **bloque de código** en Java es un grupo de cero o más sentencias entre llaves equilibradas (`{}`) y puede utilizarse en cualquier lugar donde se permita una sola sentencia. Por ejemplo, los siguientes dos fragmentos son equivalentes; el primero es una sola sentencia y el segundo es un bloque que contiene la misma sentencia:

```Java
// Sentencia única
patrons++;

// Sentencia dentro de un bloque
{
    patrons++;
}
```

Una sentencia o un bloque a menudo sirve como el objetivo de una sentencia de toma de decisiones. Por ejemplo, se puede anteponer la sentencia de toma de decisiones `if` a estos dos ejemplos:

```Java
// Sentencia única
if (ticketsTaken > 1)
    patrons++;

// Sentencia dentro de un bloque
if (ticketsTaken > 1) {
    patrons++;
}
```

Nuevamente, ambos fragmentos de código son equivalentes. Solo recuerde que el objetivo de una sentencia de toma de decisiones puede ser una sola sentencia o un bloque de sentencias. Durante el resto del capítulo, se utilizan ambas formas para preparar mejor al candidato para lo que se verá en el examen.

Si bien los dos ejemplos anteriores son equivalentes, estilísticamente se suele preferir el uso de bloques, incluso si el bloque tiene solo una sentencia. La segunda forma tiene la ventaja de que se pueden insertar rápidamente nuevas líneas de código en el bloque, sin modificar la estructura circundante.

### La sentencia `if`

A menudo, se desea ejecutar un bloque solo bajo ciertas circunstancias. La sentencia `if`, como se muestra en la imagen a continuación, logra esto permitiendo que la aplicación ejecute un bloque de código particular si y solo si una expresión booleana se evalúa como `true` en tiempo de ejecución.

![[La estructura de una instrucción if.png]]

Por ejemplo, imagínese que se tiene una función que utiliza la hora del día, un valor entero de 0 a 23, para mostrar un mensaje al usuario:

```Java
if (hourOfDay < 11)
    System.out.println("Good Morning");
```

Si la hora del día es menor que 11, entonces se mostrará el mensaje.

Ahora supóngase que también se desea incrementar algún valor, `morningGreetingCount`, cada vez que se imprime el saludo. Se podría escribir la sentencia `if` dos veces, pero afortunadamente Java ofrece un enfoque más natural utilizando un bloque:

```Java
if (hourOfDay < 11) {
    System.out.println("Good Morning");
    morningGreetingCount++;
}
```

### Atención a la indentación y las llaves

Un área donde los creadores del examen intentarán confundir al candidato es con las sentencias `if` sin llaves (`{}`). Por ejemplo, obsérvese esta forma ligeramente modificada del ejemplo anterior:

```Java
if (hourOfDay < 11)
    System.out.println("Good Morning");
    morningGreetingCount++;
```

Basándose en la indentación, podría pensarse que la variable `morningGreetingCount` solo se incrementará si `hourOfDay` es menor que 11, pero eso no es lo que hace este código. Ejecutará la sentencia de impresión solo si se cumple la condición, pero **siempre** ejecutará la operación de incremento.

Recuerde que en Java, a diferencia de otros lenguajes de programación, las tabulaciones son solo espacios en blanco y no se evalúan como parte de la ejecución. Cuando se observe una sentencia de flujo de control en una pregunta, se debe asegurar el rastreo de las llaves de apertura y cierre del bloque, ignorando cualquier indentación que se pueda encontrar.

### La sentencia `else`

Se expandirá un poco el ejemplo. ¿Qué sucede si se desea mostrar un mensaje diferente si son las 11 a.m. o más tarde? ¿Se puede hacer utilizando solo las herramientas que se tienen? ¡Por supuesto que se puede!

```Java
if (hourOfDay < 11) {
    System.out.println("Good Morning");
}
if (hourOfDay >= 11) {
    System.out.println("Good Afternoon");
}
```

Sin embargo, esto parece un poco redundante, ya que se está realizando una evaluación sobre `hourOfDay` dos veces. Afortunadamente, Java ofrece un enfoque más útil en forma de una sentencia `else`, como se muestra en la siguiente imagen.

![[La estructura de una instrucción else.png]]

Se retoma este ejemplo:

```Java
if (hourOfDay < 11) {
    System.out.println("Good Morning");
} else System.out.println("Good Afternoon");
```

Ahora el código se bifurca realmente entre una de las dos opciones posibles, ocurriendo la evaluación booleana una sola vez. El operador `else` acepta una sentencia o un bloque de sentencias, de la misma manera que la sentencia `if`.

De manera similar, se pueden añadir sentencias `if` adicionales a un bloque `else` para llegar a un ejemplo más refinado:

```Java
if (hourOfDay < 11) {
    System.out.println("Good Morning");
} else if (hourOfDay < 15) {
    System.out.println("Good Afternoon");
} else {
    System.out.println("Good Evening");
}
```

En este ejemplo, el proceso de Java continuará la ejecución hasta encontrar una sentencia `if` que se evalúe como `true`. Si ninguna de las dos primeras expresiones es verdadera, ejecutará el código del bloque `else` final.

### Verificación de que la sentencia `if` se evalúe como una expresión booleana

Otra forma común en la que el examen puede intentar confundir al candidato es proporcionando código donde la expresión booleana dentro de la sentencia `if` no sea realmente una expresión booleana. Por ejemplo, observe las siguientes líneas de código:

```Java
int hourOfDay = 1;
if (hourOfDay) { // NO COMPILA
    …
}
```

Esta sentencia puede ser válida en otros lenguajes de programación y _scripting_, pero no en Java, donde 0 y 1 no se consideran valores booleanos.

### Acortamiento del código con coincidencia de patrones (_Pattern Matching_)

La coincidencia de patrones es una técnica para controlar el flujo del programa que solo ejecuta una sección de código que cumple con ciertos criterios. En esta sección, se realiza la coincidencia de patrones con la sentencia `if`, junto con el operador `instanceof`, para mejorar el control del programa.

Si la coincidencia de patrones resulta nueva, se debe tener cuidado de no confundirla con la clase `Pattern` de Java o las expresiones regulares (_regex_). Si bien la coincidencia de patrones puede incluir el uso de expresiones regulares para el filtrado, son conceptos no relacionados.

La coincidencia de patrones es una herramienta útil para reducir el código repetitivo (_boilerplate_) en la aplicación. El código repetitivo es aquel que tiende a duplicarse en una sección de código una y otra vez de manera similar.

Para entender por qué se añadió esta característica, considere el siguiente código que toma una instancia de `Number` y la compara con el valor 5. Si no se han visto `Number` o `Integer`, solo es necesario saber por ahora que `Integer` hereda de `Number`.

```Java
void compareIntegers(Number number) {
    if (number instanceof Integer) {
        Integer data = (Integer)number;
        System.out.print(data.compareTo(5));
    }
}
```

El _cast_ (conversión explícita) es necesario dado que el método `compareTo()` está definido en `Integer`, pero no en `Number`.

El código que primero verifica si una variable es de un tipo particular y luego inmediatamente la convierte (_casts_) a ese tipo es extremadamente común en el mundo Java. Es tan común que los autores de Java decidieron implementar una sintaxis más corta para ello:

```Java
void compareIntegers(Number number) {
    if (number instanceof Integer data) {
        System.out.print(data.compareTo(5));
    }
}
```

La variable `data` en este ejemplo se denomina **variable de patrón**. Note que este código también evita cualquier `ClassCastException` potencial porque la operación de conversión se ejecuta solo si el operador `instanceof` devuelve `true`.

La siguiente imagen muestra la anatomía de la coincidencia de patrones utilizando el operador `instanceof` y sentencias `if`. Añadir una variable después del tipo es lo que instruye al compilador para tratarlo como coincidencia de patrones. También se muestra una cláusula condicional opcional, que es una característica útil que se cubrirá a continuación.

![[Coincidencia de patrones con if.png]]

### Reasignación de variables de patrón

Aunque es posible, es una mala práctica reasignar una variable de patrón, ya que hacerlo puede llevar a ambigüedad sobre qué está y qué no está en el alcance (_scope_).

```Java
if (number instanceof Integer data) {
    data = 10;
}
```

La reasignación puede prevenirse con un modificador `final`, pero es mejor no reasignar la variable en absoluto.

```Java
if (number instanceof final Integer data) {
    data = 10; // NO COMPILA
}
```

### Variables de patrón y expresiones

La coincidencia de patrones soporta una cláusula condicional opcional, declarada como una expresión booleana. Esto puede utilizarse para filtrar datos, como en el siguiente ejemplo:

```Java
void printIntegersGreaterThan5(Number number) {
    if (number instanceof Integer data && data.compareTo(5) > 0)
        System.out.print(data);
}
```

Se pueden aplicar varios filtros, o patrones, para que la sentencia `if` se ejecute solo en circunstancias específicas. Note que se está utilizando la variable de patrón en una expresión en la misma línea en la que se declara.

### Coincidencia de patrones con `null`

Como se vio en el Capítulo 2, «Operadores», el operador `instanceof` siempre evalúa las referencias `null` como `false`. Lo mismo se aplica para la coincidencia de patrones.

```Java
String noObjectHere = null;
if(noObjectHere instanceof String)
    System.out.println("No se imprime");
if(noObjectHere instanceof String s)
    System.out.println("Tampoco se imprime");
if(noObjectHere instanceof String s && s.length() > -1)
    System.out.println("No, este tampoco");
```

Como se muestra en el último ejemplo, esto también ayuda a evitar cualquier `NullPointerException` potencial, ya que el operador condicional (`&&`) hace que la llamada a `s.length()` se omita.

### Tipos soportados

El tipo de la variable de patrón debe ser un tipo compatible, lo que incluye el mismo tipo, un subtipo o un supertipo de la variable de referencia. Si la variable de referencia no se refiere a una clase o tipo `final`, entonces también puede incluir una interfaz no relacionada por razones que se detallarán en el Capítulo 7, «Más allá de las clases». Considere los siguientes dos ejemplos, en los cuales `Integer` es un subtipo de `Number`:

```Java
11: Number bearHeight = Integer.valueOf(123);
12:
13: if (bearHeight instanceof Integer i) {}
14: if (bearHeight instanceof Number n) {}
15: if (bearHeight instanceof String s) {} // NO COMPILA
16. if (bearHeight instanceof Object o) {}
```

El primer ejemplo utiliza un subtipo, mientras que el segundo ejemplo utiliza el mismo tipo que la variable de referencia `bearHeight`. En la línea 15, el compilador reconoce que un `Number` no puede convertirse a un tipo no relacionado `String` y lanza un error. La línea 16 está permitida pero no es particularmente útil, ya que cada `Object` excepto `null` devolverá `true`.

Cuando se introdujo por primera vez la coincidencia de patrones en Java, el tipo tenía que ser un subtipo estricto de `Number` (y no `Number` en sí mismo). Por esta razón, las líneas 14 y 16 no compilaban. A partir de **Java 21**, esta regla se eliminó para permitir el uso de los mismos tipos o tipos más amplios.

### Alcance de flujo (_Flow Scoping_)

El compilador aplica el **alcance de flujo** cuando trabaja con coincidencia de patrones. El alcance de flujo significa que la variable solo está en alcance cuando el compilador puede determinar definitivamente su tipo. El alcance de flujo es diferente a cualquier otro tipo de alcance, ya que no es estrictamente jerárquico. Es determinado por el compilador basándose en la ramificación y el flujo del programa.

Dada esta información, ¿se puede apreciar por qué lo siguiente no compila?

```Java
void printIntegersOrNumbersGreaterThan5(Number number) {
    if (number instanceof Integer data || data.compareTo(5) > 0)
        System.out.print(data);
}
```

El punto clave a notar es que se utilizó OR (`||`) no AND (`&&`) en la sentencia condicional. Si la entrada no hereda de `Integer`, la variable `data` queda indefinida. Dado que el compilador no puede garantizar que `data` sea una instancia de `Integer`, el código no compila.

¿Qué sucede con este ejemplo?

```Java
void printIntegerTwice(Number number) {
    if (number instanceof Integer data)
        System.out.print(data.intValue());
    System.out.print(data.intValue()); // NO COMPILA
}
```

Dado que la entrada podría no haber heredado de `Integer`, `data` ya no está en alcance después de la sentencia `if`. Entonces, podría pensarse que la variable de patrón solo está en alcance dentro del bloque de la sentencia `if`, ¿verdad? Bueno, ¡no exactamente! Considere el siguiente ejemplo que sí compila:

```Java
void printOnlyIntegers(Number number) {
    if (!(number instanceof Integer data))
        return;
    System.out.print(data.intValue());
}
```

Podría sorprender saber que este código compila. ¿Qué está pasando aquí? El método retorna si la entrada no hereda de `Integer`. Esto significa que cuando se alcanza la última línea del método, la entrada **debe** heredar de `Integer`, y por lo tanto `data` permanece en alcance incluso después de que termina la sentencia `if`.

Comprender por qué este ejemplo compila y el anterior no, es la clave para entender el alcance de flujo.

Se debe asegurar la comprensión de la forma en que funciona el alcance de flujo. En particular, es posible utilizar una variable de patrón fuera de la sentencia `if`, pero solo cuando el compilador puede determinar definitivamente su tipo.

### Alcance de flujo y ramas `else`

Si el último ejemplo de código resulta confuso, no hay de qué preocuparse: es común. Otra forma de pensarlo es reescribir la lógica a algo equivalente que use una sentencia `else`:

```Java
void printOnlyIntegers(Number number) {
    if (!(number instanceof Integer data))
        return;
    else
        System.out.print(data.intValue());
}
```

Ahora se puede ir un paso más allá e invertir las ramas `if` y `else` invirtiendo la expresión booleana:

```Java
void printOnlyIntegers(Number number) {
    if (number instanceof Integer data)
        System.out.print(data.intValue());
    else
        return;
}
```

El nuevo código es equivalente al original y demuestra mejor cómo el compilador fue capaz de determinar que `data` estaba en alcance solo cuando `number` es un `Integer`.