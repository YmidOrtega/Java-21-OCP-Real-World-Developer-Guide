Antes de entrar en materia, se abordará un poco de terminología. Un operador de Java es un símbolo especial que puede aplicarse a un conjunto de variables, valores o literales —denominados **operandos**— y que devuelve un resultado. El término operando, que se utiliza a lo largo de este capítulo, se refiere al valor o variable al que se aplica el operador. La siguiente imagen muestra la anatomía de una operación en Java.

La salida de la operación se denomina simplemente **resultado**. En realidad, la siguiente imagen contiene una segunda operación, en la que se utiliza el operador de asignación (`=`) para almacenar el resultado en la variable `c`.

![[Operacion Matematica.png]]

Es seguro que se han utilizado los operadores de suma (`+`) y resta (`-`) desde la infancia. Java soporta muchos otros operadores que es necesario conocer para el examen. Si bien muchos deberían ser un repaso, algunos (como los operadores de asignación compuesta) pueden resultar nuevos.

### Tipos de operadores

Java soporta tres tipos de operadores: **unarios**, **binarios** y **ternarios**. Estos tipos de operadores pueden aplicarse a uno, dos o tres operandos, respectivamente. Para el examen, es necesario conocer un subconjunto específico de operadores Java, cómo aplicarlos y el orden en el que deben aplicarse.

Los operadores de Java no se evalúan necesariamente en orden de izquierda a derecha. En el siguiente ejemplo, la segunda expresión se evalúa realmente de derecha a izquierda, dados los operadores específicos involucrados:

```Java
int cookies = 4;
double reward = 3 + 2 * --cookies;
System.out.print("Zoo animal receives: " + reward + " reward points");
```

En este ejemplo, primero se decrementa `cookies` a 3, luego se multiplica el valor resultante por 2, y finalmente se suma 3. El valor se promueve automáticamente de 9 a 9.0 y se asigna a `reward`. Los valores finales de `reward` y `cookies` son 9.0 y 3, respectivamente, imprimiéndose lo siguiente:

```Plaintext
Zoo animal receives: 9.0 reward points
```

Si no se siguió esa evaluación, no hay motivo de preocupación. Al finalizar este capítulo, la resolución de problemas como este debería ser algo natural.

### Precedencia de operadores

Al leer un libro o un periódico, algunos idiomas escritos se evalúan de izquierda a derecha, mientras que otros se evalúan de derecha a izquierda. En matemáticas, ciertos operadores pueden anular a otros operadores y ser evaluados primero. Determinar qué operadores se evalúan y en qué orden se conoce como **precedencia de operadores**. De esta manera, Java sigue más de cerca las reglas matemáticas. Considere la siguiente expresión:

```Java
var perimeter = 2 * height + 2 * length;
```

Se aplicarán algunos paréntesis opcionales para demostrar cómo el compilador evalúa esta sentencia:

``` Java
var perimeter = ((2 * height) + (2 * length));
```

El operador de multiplicación (`*`) tiene una precedencia mayor que el operador de suma (`+`), por lo que `height` y `length` se multiplican por 2 antes de sumarse. El operador de asignación (`=`) tiene el orden de precedencia más bajo, por lo que la asignación a la variable `perimeter` se realiza al final.

A menos que se anule con paréntesis, los operadores de Java siguen el orden de operación listado en la Tabla 2.1, en orden decreciente de precedencia. Si dos operadores tienen el mismo nivel de precedencia, entonces Java garantiza la evaluación de izquierda a derecha para la mayoría de los operadores, excepto los marcados en la tabla.

Se recomienda mantener la Tabla 2.1 a mano a lo largo de este capítulo. Para el examen, es necesario memorizar el orden de precedencia de esta tabla. Observe que no se evaluarán algunos operadores, como los operadores de desplazamiento (_shift_), aunque se recomienda estar al tanto de su existencia.

**TABLA 2.1** Orden de precedencia de operadores

| **Operador**                             | **Símbolos y ejemplos**                                                | **Evaluación**      |
| ---------------------------------------- | ---------------------------------------------------------------------- | ------------------- |
| **Operadores post-unarios**              | `expresion++`, `expresion--`                                           | Izquierda a derecha |
| **Operadores pre-unarios**               | `++expresion`, `--expresion`                                           | Derecha a izquierda |
| **Otros operadores unarios**             | `-`, `!`, `~`, `+`, `(type)`                                           | Derecha a izquierda |
| **Cast (Conversión)**                    | `(Tipo)referencia`                                                     | Derecha a izquierda |
| **Multiplicación/división/módulo**       | `*`, `/`, `%`                                                          | Izquierda a derecha |
| **Suma/resta**                           | `+`, `-`                                                               | Izquierda a derecha |
| **Operadores de desplazamiento (Shift)** | `<<`, `>>`, `>>>`                                                      | Izquierda a derecha |
| **Operadores relacionales**              | `<`, `>`, `<=`, `>=`, `instanceof`                                     | Izquierda a derecha |
| **Igualdad/desigualdad**                 | `==`, `!=`                                                             | Izquierda a derecha |
| **AND lógico**                           | `&`                                                                    | Izquierda a derecha |
| **OR exclusivo lógico (XOR)**            | `^`                                                                    | Izquierda a derecha |
| **OR inclusivo lógico**                  | \|                                                                     | Izquierda a derecha |
| **AND condicional**                      | `&&`                                                                   | Izquierda a derecha |
| **OR condicional**                       | \|\|                                                                   | Izquierda a derecha |
| **Operadores ternarios**                 | `expresion booleana ? expresion1 : expresion2`                         | Derecha a izquierda |
| **Operadores de asignación**             | `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `^=`,`\|=`,`<<=`,`>>=`,`>>>=` | Derecha a izquierda |
| **Operador flecha (Arrow)**              | `->`                                                                   | Derecha a izquierda |
El operador flecha (`->`), a veces denominado función flecha u operador lambda, es un operador binario que representa una relación entre dos operandos. Aunque no se cubrirá el operador flecha en este capítulo, se verá utilizado en expresiones `switch` en el Capítulo 3, «Toma de decisiones», y en expresiones lambda a partir del Capítulo 8, «Lambdas e interfaces funcionales».