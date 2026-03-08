Una práctica común al escribir software es realizar la misma tarea un cierto número de veces. Se podrían utilizar las estructuras de decisión presentadas hasta ahora para lograr esto, pero eso resultaría en una cadena bastante larga de sentencias `if` o `else`, especialmente si se tiene que ejecutar lo mismo 100 veces o más.

¡Aquí entran los bucles! Un **bucle** es una estructura de control repetitiva que puede ejecutar una sentencia o bloque de código múltiples veces en sucesión. Al utilizar variables a las que se les pueden asignar nuevos valores, cada repetición de la sentencia puede ser diferente. El siguiente bucle se ejecuta exactamente 10 veces:

```Java
int counter = 0;
while (counter < 10) {
    double price = counter * 10;
    System.out.println(price);
    counter++;
}
```

Si no se sigue este código, no hay motivo de pánico; se cubrirá en breve. En esta sección, se discutirá el bucle `while` y sus dos formas. En la siguiente sección, se pasará a los bucles `for`, los cuales tienen sus raíces en los bucles `while`.

#### La sentencia `while`

La estructura de control repetitiva más simple en Java es la sentencia `while`, descrita en la imagen a continuación. Como todas las estructuras de control de repetición, tiene una **condición de terminación**, implementada como una expresión booleana. Un bucle continuará mientras esta expresión se evalúe como `true`.

![[Estructura de una instrucción while.png]]

Como se muestra en la imagen anterior, un bucle `while` es similar a una sentencia `if` en el sentido de que está compuesto por una expresión booleana y una sentencia, o un bloque de sentencias. Durante la ejecución, la expresión booleana se evalúa **antes** de cada iteración del bucle y sale si la evaluación devuelve `false`.

Se observa cómo se puede usar un bucle para modelar a un ratón comiendo una comida:

```Java
int roomInBelly = 5;
void eatCheese(int bitesOfCheese) {
    while (bitesOfCheese > 0 && roomInBelly > 0) {
        bitesOfCheese--;
        roomInBelly--;
    }
    System.out.println(bitesOfCheese + " pieces of cheese left");
}
```

Este método toma una cantidad de comida (en este caso, queso) y continúa hasta que el ratón no tiene espacio en su vientre o no queda comida por comer. Con cada iteración del bucle, el ratón «come» un bocado de comida y pierde un lugar en su vientre. Al usar una sentencia booleana compuesta, se asegura que el bucle `while` pueda terminar por cualquiera de las condiciones.

Una cosa que se debe recordar es que un bucle `while` puede terminar después de su primera evaluación de la expresión booleana. Por ejemplo, ¿cuántas veces se imprime `"Not full!"` en el siguiente ejemplo?

```Java
int full = 5;
while (full < 5) {
    System.out.println("Not full!");
    full++;
}
```

¿La respuesta? ¡Cero! En la primera iteración del bucle, se alcanza la condición y el bucle sale. Esta es la razón por la que los bucles `while` se usan a menudo en lugares donde se esperan **cero o más** ejecuciones del bucle. En pocas palabras, el cuerpo del bucle puede no ejecutarse en absoluto.

#### La sentencia `do/while`

La segunda forma que puede tomar un bucle `while` se llama bucle `do/while`, el cual, al igual que un bucle `while`, es una estructura de control de repetición con una condición de terminación y una sentencia, o un bloque de sentencias, como se muestra a continuación.

![[La estructura de una instrucción do while.png]]

A diferencia de un bucle `while`, sin embargo, un bucle `do/while` **garantiza** que la sentencia o bloque se ejecutará al menos **una vez**. Por ejemplo, ¿Cuál es la salida de las siguientes sentencias?

```Java
int lizard = 0;
do {
    lizard++;
} while (false);
System.out.println(lizard);
```

Java ejecutará primero el bloque de sentencias y luego comprobará la condición del bucle. Aunque el bucle sale de inmediato, el bloque de sentencias se ejecuta una vez y el programa imprime 1.

### Bucles infinitos

Lo más importante que se debe tener en cuenta al usar cualquier estructura de control de repetición es asegurarse de que siempre **terminen**. El no terminar un bucle puede llevar a numerosos problemas en la práctica, incluyendo excepciones de desbordamiento, fugas de memoria, rendimiento lento e incluso datos incorrectos. Se examina un ejemplo:

```Java
int pen = 2;
int pigs = 5;
while (pen < 10)
    pigs++;
```

Es posible notar un problema evidente con esta sentencia: nunca terminará. La variable `pen` nunca se modifica, por lo que la expresión `(pen < 10)` siempre se evaluará como `true`. El resultado es que el bucle nunca terminará, creando lo que comúnmente se conoce como un **bucle infinito**. Un bucle infinito es un bucle cuya condición de terminación nunca se alcanza durante el tiempo de ejecución.

Siempre que se escriba un bucle, se debe examinar para determinar si la condición de terminación se cumple eventualmente bajo alguna condición. Por ejemplo, un bucle en el que ninguna variable cambia entre dos ejecuciones sugiere que la condición de terminación puede no cumplirse. Las variables del bucle siempre deben moverse en una dirección particular.

En otras palabras, se debe asegurar que la condición del bucle, o las variables de las que depende la condición, estén cambiando entre ejecuciones. Luego, se debe asegurar que la condición de terminación se alcance eventualmente en todas las circunstancias. Como se aprende en la última sección de este capítulo, un bucle también puede salir bajo otras condiciones, como una sentencia `break` o `return`.