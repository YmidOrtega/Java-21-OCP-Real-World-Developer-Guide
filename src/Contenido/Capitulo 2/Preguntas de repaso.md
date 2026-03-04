Las respuestas a las preguntas de revisión del capítulo se pueden encontrar al final del capitulo.

**1.** ¿Cuáles de los siguientes operadores de Java se pueden usar con variables booleanas? (Elija todas las opciones que correspondan).

- [ ]  A. `==`
- [ ]  B. `+`
- [ ]  C. `--`
- [ ]  D. `!`
- [ ]  E. `%`
- [ ]  F. `-`
- [ ]  G. Cast con `(boolean)`

**2.** ¿Qué tipo de datos (o tipos) permitirá que el siguiente fragmento de código compile? (Elija todas las opciones que correspondan).

```Java
byte apples = 5;
short oranges = 10;
____ bananas = apples + oranges;
```

- [ ]  A. `int`
- [ ]  B. `long`
- [ ]  C. `boolean`
- [ ]  D. `double`
- [ ]  E. `short`
- [ ]  F. `byte`

**3.** ¿Qué cambio, cuando se aplica de forma independiente, permitiría que el siguiente fragmento de código compile? (Elija todas las opciones que correspondan).

```Java
3: long ear = 10;
4: int hearing = 2 * ear;
```

- [ ]  A. Ningún cambio; compila tal como está.
- [ ]  B. Hacer un cast de `ear` en la línea 4 a `int`.
- [ ]  C. Cambiar el tipo de datos de `ear` en la línea 3 a `short`.
- [ ]  D. Hacer un cast de `2 * ear` en la línea 4 a `int`.
- [ ]  E. Cambiar el tipo de datos de `hearing` en la línea 4 a `short`.
- [ ]  F. Cambiar el tipo de datos de `hearing` en la línea 4 a `long`.

**4.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
3: boolean canine = true, wolf = true;
4: int teeth = 20;
5: canine = (teeth != 10) ^ (wolf=false);
6: System.out.println(canine+", "+teeth+", "+wolf);
```

- [ ]  A. `true, 20, true`
- [ ]  B. `true, 20, false`
- [ ]  C. `false, 10, true`
- [ ]  D. `false, 20, false`
- [ ]  E. El código no compila debido a la línea 5.
- [ ]  F. Ninguna de las anteriores.

**5.** ¿Cuáles de los siguientes operadores están clasificados en orden de precedencia creciente (de menor a mayor) o igual? Asuma que el operador `+` es la suma binaria, no la forma unaria. (Elija todas las opciones que correspondan).

- [ ]  A. `+`, `*`, `%`, `--`
- [ ]  B. `++`, `(int)`, `*`
- [ ]  C. `=`, `==`, `!`
- [ ]  D. `(short)`, `=`, `!`, `*`
- [ ]  E. `*`, `/`, `%`, `+`, `==`
- [ ]  F. `!`, `||`, `&`
- [ ]  G. `^`, `+`, `=`, `+=`

**6.** ¿Cuál es la salida del siguiente programa?

```Java
1: public class CandyCounter {
2:      static long addCandy(double fruit, float vegetables) {
3:          return (int)fruit+vegetables;
4:      }
5:
6:      public static void main(String[] args) {
7:          System.out.print(addCandy(1.4, 2.4f) + ", ");
8:          System.out.print(addCandy(1.9, (float)4) + ", ");
9:          System.out.print(addCandy((long)(int)(short)2, (float)4)); 
        } 
}
```

- [ ]  A. `4, 6, 6.0`
- [ ]  B. `3, 5, 6`
- [ ]  C. `3, 6, 6`
- [ ]  D. `4, 5, 6`
- [ ]  E. El código no compila debido a la línea 9.
- [ ]  F. Ninguna de las anteriores.

**7.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
int ph = 7, vis = 2;
boolean clear = vis > 1 & (vis < 9 || ph < 2);
boolean safe = (vis > 2) && (ph++ > 1);
boolean tasty = 7 <= --ph;
System.out.println(clear + "-" + safe + "-" + tasty);
```

- [ ]  A. `true-true-true`
- [ ]  B. `true-true-false`
- [ ]  C. `true-false-true`
- [ ]  D. `true-false-false`
- [ ]  E. `false-true-true`
- [ ]  F. `false-true-false`
- [ ]  G. `false-false-true`
- [ ]  H. `false-false-false`

**8.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
4: int pig = (short)4;
5: pig = pig++;
6: long goat = (int)2;
7: goat -= 1.0;
8: System.out.print(pig + " - " + goat);
```

- [ ]  A. `4 - 1`
- [ ]  B. `4 - 2`
- [ ]  C. `5 - 1`
- [ ]  D. `5 - 2`
- [ ]  E. El código no compila debido a la línea 7.
- [ ]  F. Ninguna de las anteriores. 

**9.** ¿Cuáles son las salidas únicas del siguiente fragmento de código? (Elija todas las opciones que correspondan).

```Java
int a = 2, b = 4, c = 2;
System.out.println(a > 2 ? --c : b++);
System.out.println(b = (a != c ? a : b++));
System.out.println(a > b ? b < c ? b : 2 : 1);
```

- [ ]  A. 1
- [ ]  B. 2
- [ ]  C. 3
- [ ]  D. 4
- [ ]  E. 5
- [ ]  F. 6
- [ ]  G. El código no compila.

**10.** ¿Cuál no es una salida del siguiente fragmento de código?

```Java
short height = 1, weight = 3;
short zebra = (byte) weight * (byte) height;
double ox = 1 + height * 2 + weight;
long giraffe = 1 + 9 % height + 1;
System.out.println(zebra);
System.out.println(ox);
System.out.println(giraffe);
```

- [ ]  A. 2
- [ ]  B. 3
- [ ]  C. 6
- [ ]  D. 6.0
- [ ]  E. El código no compila.

**11.** ¿Cuál es la salida del siguiente código?

```Java
11: int sample1 = (2 * 4) % 3;
12: int sample2 = 3 * 2 % -3;
13: int sample3 = 5 * (1 % 2);
14: System.out.println(sample1 + ", " + sample2 + ", " + sample3);
```

- [ ]  A. `0, 0, 5`
- [ ]  B. `1, 2, 10`
- [ ]  C. `2, 1, 5`
- [ ]  D. `2, 0, 5`
- [ ]  E. `3, 1, 10`
- [ ]  F. `3, 2, 6`
- [ ]  G. El código no compila.

**12.** El operador _________ aumenta un valor y devuelve el valor original, mientras que el operador _______ disminuye un valor y devuelve el nuevo valor.

- [ ]  A. post-incremento, post-incremento
- [ ]  B. pre-decremento, post-decremento
- [ ]  C. post-incremento, post-decremento
- [ ]  D. post-incremento, pre-decremento
- [ ]  E. pre-incremento, pre-decremento 
- [ ]  F. pre-incremento, post-decremento  

**13.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
boolean sunny = true, raining = false, sunday = true;
boolean goingToTheStore = sunny & raining ^ sunday;
boolean goingToTheZoo = sunday && !raining;
boolean stayingHome = !(goingToTheStore && goingToTheZoo);
System.out.println(goingToTheStore + "-" + goingToTheZoo + "-" + stayingHome);
```

- [ ]  A. `true-false-false`
- [ ]  B. `false-true-false`
- [ ]  C. `true-true-true`
- [ ]  D. `false-true-true`
- [ ]  E. `false-false-false`
- [x]  F. `true-true-false`
- [ ]  G. Ninguna de las anteriores

**14.** ¿Cuáles de las siguientes afirmaciones son correctas? (Elija todas las opciones que correspondan).

- [ ]  A. El valor de retorno de una expresión de operación de asignación puede ser `void`. 
- [ ]  B. El operador de desigualdad (`!=`) se puede usar para comparar objetos.
- [ ]  C. El operador de igualdad (`==`) se puede usar para comparar un valor booleano con un valor numérico.
- [ ]  D. Durante el tiempo de ejecución, los operadores `&` y `|` pueden hacer que solo se evalúe el lado izquierdo de la expresión.
- [ ]  E. El valor de retorno de una expresión de operación de asignación es el valor de la variable recién asignada.
- [ ]  F. En Java, `0` y `false` se pueden usar indistintamente.
- [ ]  G. El operador de complemento lógico (`!`) no se puede usar para invertir valores numéricos.

**15.** ¿Qué operador toma tres operandos o valores?

- [ ]  A. `=`
- [ ]  B. `&&`
- [ ]  C. `*=` 
- [ ]  D. `? :` 
- [ ]  E. `&` 
- [ ]  F. `++`
- [ ]  G. `/`

**16.** ¿Cuántas líneas del siguiente código contienen errores de compilación?

```Java
int note = 1 * 2 + (long)3;
short melody = (byte)(double)(note *= 2);
double song = melody;
float symphony = (float)((song == 1_000f) ? song * 2L : song);
```

- [ ]  A. 0
- [ ]  B. 1
- [ ]  C. 2 
- [ ]  D. 3 
- [ ]  E. 4 

**17.** Dado el siguiente fragmento de código, ¿cuáles son los valores de las variables después de su ejecución? (Elija todas las opciones que correspondan).

```Java
int ticketsTaken = 1;
int ticketsSold = 3;
ticketsSold += 1 + ticketsTaken++;
ticketsTaken *= 2;
ticketsSold += (long)1;
```

- [ ]  A. `ticketsSold` es 8.
- [ ]  B. `ticketsTaken` es 2.
- [ ]  C. `ticketsSold` es 6.
- [ ]  D. `ticketsTaken` es 6.
- [ ]  E. `ticketsSold` es 7.
- [ ]  F. `ticketsTaken` es 4.
- [ ]  G. El código no compila.

**18.** ¿Cuál de los siguientes se puede utilizar para cambiar el orden de operación en una expresión?

- [ ]  A. `[ ]`
- [ ]  B. `< >`
- [ ]  C. `( )`
- [ ]  D. `\ /`
- [ ]  E. `{ }`
- [ ]  F. `" "`

**19.** ¿Cuál es el resultado de ejecutar el siguiente fragmento de código? (Elija todas las opciones que correspondan).

```Java
3: int start = 7;
4: int end = 4;
5: end += ++start;
6: start = (byte)(Byte.MAX_VALUE + 1);
```

- [ ]  A. `start` es 0.
- [ ]  B. `start` es -128.
- [ ]  C. `start` es 127.
- [ ]  D. `end` es 8.
- [ ]  E. `end` es 11.
- [ ]  F. `end` es 12.
- [ ]  G. El código no compila.
- [ ]  H. El código compila pero lanza una excepción en tiempo de ejecución.

**20.** ¿Cuáles de las siguientes afirmaciones sobre operadores unarios son verdaderas? (Elija todas las opciones que correspondan).

- [ ]  A. Los operadores unarios siempre se ejecutan antes que cualquier operador binario o ternario numérico circundante.
- [ ]  B. El operador `-` se puede usar para invertir un valor booleano.
- [ ]  C. El operador de pre-incremento (`++`) devuelve el valor de la variable antes de que se aplique el incremento.
- [ ]  D. El operador de post-decremento (`--`) devuelve el valor de la variable antes de que se aplique el decremento.
- [ ]  E. El operador `!` no se puede usar en valores numéricos.
- [ ]  F. Ninguna de las anteriores.