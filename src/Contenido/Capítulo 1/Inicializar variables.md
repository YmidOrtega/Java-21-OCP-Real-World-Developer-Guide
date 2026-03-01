Antes de poder utilizar una variable, esta requiere un valor. Algunos tipos de variables obtienen este valor automáticamente, mientras que otros requieren que el programador lo especifique. En las siguientes secciones, se analizan las diferencias entre los valores predeterminados para variables locales, de instancia y de clase.

### Creación de variables locales

Una **variable local** es una variable definida dentro de un constructor, método o bloque de inicialización. Para simplificar, esta sección se centra principalmente en las variables locales dentro de métodos, aunque las reglas para los otros casos son las mismas.

### Variables locales finales (`final`)

La palabra clave `final` puede aplicarse a variables locales y es equivalente a declarar constantes en otros lenguajes. Considérese este ejemplo:

``` Java
5: final int y = 10;
6: int x = 20;
7: y = x + 10; // NO COMPILA
```

Ambas variables están establecidas, pero `y` utiliza la palabra clave `final`. Por esta razón, la línea 7 provoca un error de compilación, ya que el valor no puede ser modificado.

El modificador `final` también puede aplicarse a referencias de variables locales. El siguiente ejemplo utiliza un objeto array `int[]`, el cual se estudia en el capítulo 4.

```Java
5: final int[] favoriteNumbers = new int[10];
6: favoriteNumbers[0] = 10;
7: favoriteNumbers[1] = 20;
8: favoriteNumbers = null; // NO COMPILA
```

Obsérvese que es posible modificar el contenido, o los datos, en el array. El error de compilación no ocurre hasta la línea 8, cuando se intenta cambiar el valor de la referencia `favoriteNumbers`.

### Variables locales no inicializadas

Las variables locales no tienen un valor predeterminado y deben inicializarse antes de su uso. Además, el compilador reportará un error si se intenta leer un valor no inicializado. Por ejemplo, el siguiente código genera un error de compilación:

```Java
4: public int notValid() {
5:      int y = 10;
6:      int x;
7:      int reply = x + y; // NO COMPILA
8:      return reply;
9: }
```

La variable `y` se inicializa en 10. Por el contrario, `x` no se inicializa antes de ser utilizada en la expresión de la línea 7, y el compilador genera un error. El compilador es lo suficientemente inteligente como para reconocer variables que han sido inicializadas después de su declaración pero antes de ser utilizadas. He aquí un ejemplo:

```Java
public int valid() {
    int y = 10;
    int x; // x se declara aquí
    x = 3; // x se inicializa aquí
    int z; // z se declara aquí pero nunca se inicializa ni se usa
    int reply = x + y;
    return reply;
}
```

En este ejemplo, `x` se declara, inicializa y utiliza en líneas separadas. Además, `z` se declara pero nunca se utiliza, por lo que no se requiere que sea inicializada.

El compilador también es lo suficientemente inteligente para reconocer inicializaciones más complejas. En este ejemplo, existen dos ramas de código.

```Java
public void findAnswer(boolean check) {
    int answer;
    int otherAnswer;
    int onlyOneBranch;
    if (check) {
        onlyOneBranch = 1;
        answer = 1;
    } else {
        answer = 2;
    }
    System.out.println(answer);
    System.out.println(onlyOneBranch); // NO COMPILA
}
```

La variable `answer` se inicializa en ambas ramas de la declaración `if`, por lo que el compilador lo acepta perfectamente. El sistema sabe que, independientemente de si `check` es `true` o `false`, el valor de `answer` se establecerá antes de ser utilizado.

La variable `otherAnswer` no está inicializada pero nunca se utiliza, y el compilador lo acepta igualmente. Recuerde que al compilador solo le preocupa si se intenta utilizar variables locales no inicializadas; no le importan las que nunca se utilizan.

La variable `onlyOneBranch` se inicializa solo si `check` resulta ser `true`. El compilador sabe que existe la posibilidad de que `check` sea `false`, lo que daría lugar a código no inicializado, y genera un error de compilación. Se aprenderá más sobre la declaración `if` en el capítulo 3, «Toma de decisiones».
### Paso de parámetros de constructor y método

Las variables que se pasan a un constructor o método se denominan parámetros de constructor o parámetros de método, respectivamente. Estos parámetros son como variables locales que han sido preinicializadas. Las reglas para inicializar parámetros de constructor y método son las mismas, por lo que el enfoque se centra principalmente en los parámetros de método.

En el ejemplo anterior, `check` es un parámetro de método.

```Java
public void findAnswer(boolean check) {}
```

Obsérvese el siguiente método `checkAnswer()` en la misma clase:

```Java
public void checkAnswer() {
    boolean value;
    findAnswer(value); // NO COMPILA
}
```

La llamada a `findAnswer()` no compila porque intenta utilizar una variable que no está inicializada. Si bien quien invoca el método `checkAnswer()` debe preocuparse por la inicialización de la variable, una vez dentro del método `findAnswer()`, se puede asumir que la variable local ha sido inicializada con algún valor.

### Definición de variables de instancia y de clase

Las variables que no son locales se definen como variables de instancia o como variables de clase. Una **variable de instancia**, a menudo llamada campo (_field_), es un valor definido dentro de una instancia específica de un objeto. Suponga que existe una clase `Person` con una variable de instancia `name` de tipo `String`. Cada instancia de la clase tendría su propio valor para `name`, como Elys o Sarah. Dos instancias podrían tener el mismo valor para `name`, pero cambiar el valor de una no modifica la otra.

Por otro lado, una **variable de clase** es aquella definida a nivel de clase y compartida entre todas las instancias de la clase. Incluso puede ser accesible públicamente para clases externas y no requiere una instancia para su uso. En el ejemplo anterior de `Person`, se podría utilizar una variable de clase compartida para representar la lista de personas presentes en el zoológico hoy. Se puede distinguir que una variable es de clase porque lleva la palabra clave `static` antes de ella. Esto se estudia en el capítulo 5. Por ahora, basta con saber que una variable es de clase si tiene la palabra clave `static` en su declaración.

Las variables de instancia y de clase no requieren ser inicializadas. Tan pronto como se declaran estas variables, se les asigna un valor predeterminado. El compilador desconoce qué valor utilizar y, por tanto, busca el valor más simple que pueda asignar al tipo: `null` para un objeto, cero para los tipos numéricos y `false` para un booleano. No es necesario conocer el valor predeterminado para `char`, pero en caso de curiosidad, es `'\u0000'` (NUL).

### Inferencia de tipos con `var`

Existe la opción de utilizar la palabra clave `var` en lugar del tipo al declarar variables locales bajo ciertas condiciones. Para usar esta característica, simplemente se escribe `var` en lugar del tipo primitivo o de referencia. He aquí un ejemplo:

```Java
public class Zoo {
    public void whatTypeAmI() {
        var name = "Hello";
        var size = 7;
    }
}
```

El nombre formal de esta característica es **inferencia de tipos de variables locales**. Se analizará por partes. Primero viene _variable local_. Esto significa exactamente lo que parece. Esta característica solo puede utilizarse para variables locales. El examen podría intentar confundir al candidato con código como este:

```Java
public class VarKeyword {
    var tricky = "Hello"; // NO COMPILA
}
```

¡Un momento! Se acaba de aprender la diferencia entre variables de instancia y locales. La variable `tricky` es una variable de instancia. La inferencia de tipos de variables locales funciona con variables locales y no con variables de instancia.

### Inferencia de tipos de `var`

Ahora que se comprende la parte de la variable local, es momento de pasar a lo que significa la inferencia de tipos. La buena noticia es que esto también significa lo que parece. Al escribir `var`, se instruye al compilador para que determine el tipo. El compilador observa el código en la línea de la declaración y lo utiliza para inferir el tipo. Obsérvese este ejemplo:

```Java
7: public void reassignment() {
8:      var number = 7;
9:      number = 4;
10:     number = "five"; // NO COMPILA
11: }
```

En la línea 8, el compilador determina que se desea una variable `int`. En la línea 9, no hay problema en asignar un `int` diferente a ella. En la línea 10, Java encuentra un problema. Se ha solicitado asignar un `String` a una variable `int`. Esto no está permitido. Es equivalente a escribir lo siguiente:

```Java
int number = "five";
```

Para simplificar al discutir `var`, se asume que una declaración de variable se completa en una sola línea. Es posible insertar un salto de línea entre el nombre de la variable y su valor de inicialización, como en el siguiente ejemplo:

```Java

7: public void breakingDeclaration() {
8:      var silly
9:      = 1;
10: }
```

Este ejemplo es válido y compila, pero se considera que la declaración e inicialización de `silly` ocurren en la misma línea.

### Ejemplos con `var`

Se analizarán más escenarios para evitar confusiones en el examen sobre este tema. ¿Se considera que el siguiente código compila?

``` Java
3: public void doesThisCompile(boolean check) {
4:      var question;
5:      question = 1;
6:      var answer;
7:      if (check) {
8:          answer = 2;
9:      } else {
10:         answer = 3;
11:     }
12:     System.out.println(answer);
13: }
```

El código no compila. Se debe recordar que, para la inferencia de tipos de variables locales, el compilador observa únicamente la línea con la declaración. Dado que a `question` y `answer` no se les asignan valores en las líneas donde se definen, el compilador no sabe cómo interpretarlas. Por esta razón, las líneas 4 y 6 no compilan.

Esto podría parecer extraño, ya que ambas ramas del `if/else` asignan un valor. Lamentablemente, no está en la misma línea que la declaración, por lo que no cuenta para `var`. Contraste este comportamiento con lo visto anteriormente al discutir la ramificación y la inicialización de una variable local en el método `findAnswer()`.

Ahora se sabe que el valor inicial utilizado para determinar el tipo debe ser parte de la misma declaración. ¿Es posible deducir por qué estas dos declaraciones no compilan?

``` Java
4: public void twoTypes() {
5:      int a, var b = 3; // NO COMPILA
6:      var a, b = 3; // NO COMPILA
7:      var n = null; // NO COMPILA
8: }
```

La línea 5 no funcionaría incluso si se reemplazara `var` con un tipo real. Todos los tipos declarados en una sola línea deben ser del mismo tipo y compartir la misma declaración. Tampoco se podría escribir `int a, int b = 3;`. La línea 6 muestra que no se puede utilizar `var` para definir dos variables en la misma línea.

La línea 7 es una sola línea. Se le pide al compilador que infiera el tipo de `null`. Esto podría ser cualquier tipo de referencia. La única elección que podría hacer el compilador es `Object`. Sin embargo, casi con certeza no es lo que pretendía el autor del código. Los diseñadores de Java decidieron que sería mejor no permitir `var` para `null` que tener que adivinar la intención.

Se probará con otro ejemplo. ¿Se observa por qué esto no compila?

```Java
public int addition(var a, var b) { // NO COMPILA
    return a + b;
}
```

En este ejemplo, `a` y `b` son parámetros de método. Estos no son variables locales. Se debe estar atento al uso de `var` con parámetros de constructor, parámetros de método o variables de instancia. El uso de `var` en uno de estos lugares es un truco común de examen para verificar la atención del candidato. ¡Recuerde que `var` se utiliza solo para la inferencia de tipos de variables locales!

Existe una última regla que se debe conocer: `var` no es una palabra reservada y se permite su uso como identificador. Se considera un **nombre de tipo reservado**. Un nombre de tipo reservado significa que no se puede utilizar para definir un tipo, como una clase, interfaz o enumeración. ¿Se considera esto legal?

``` Java
package var;
public class Var {
    public void var() {
        var var = "var";
    }
    public void Var() {
        Var var = new Var();
    }
}
```

Aunque parezca increíble, este código compila. Java distingue entre mayúsculas y minúsculas, por lo que `Var` no introduce conflictos como nombre de clase. Nombrar una variable local `var` es legal. ¡Se ruega no escribir código similar a este en el trabajo! Pero comprender por qué funciona ayudará a la preparación para cualquier pregunta complicada que los creadores del examen puedan plantear.

### `var` en el mundo real

La palabra clave `var` es excelente para los autores de exámenes porque facilita la escritura de código confuso. Al trabajar en un proyecto real, se busca que el código sea fácil de leer.

Una vez que se comienza a tener código similar al siguiente, es momento de considerar el uso de `var`:

```Java
PileOfPapersToFileInFilingCabinet pileOfPapersToFile =
    new PileOfPapersToFileInFilingCabinet();
```

Se puede apreciar cómo acortar esto representaría una mejora sin perder información:

```Java
var pileOfPapersToFile = new PileOfPapersToFileInFilingCabinet();
```

Si alguna vez surge la duda sobre si es apropiado utilizar `var`, se recomienda buscar sobre Inferencia de tipos de variables locales.