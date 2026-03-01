Ya se han visto algunas variables. Una variable es un nombre para un fragmento de memoria que almacena datos. Al declarar una variable, es necesario indicar el tipo de variable junto con un nombre. Asignar un valor a una variable se denomina inicializar una variable. Para inicializar una variable, simplemente se escribe el nombre de la variable seguido de un signo igual, seguido del valor deseado. Este ejemplo muestra la declaración e inicialización de una variable en una sola línea:

```Java
String zooName = "The Best Zoo";
```

En las siguientes secciones, se analiza cómo definir correctamente las variables en una o múltiples líneas.

### Identificación de identificadores

Probablemente no sorprenda que Java tenga reglas precisas sobre los nombres de los identificadores. Un **identificador** es el nombre de una variable, método, clase, interfaz o paquete. Afortunadamente, las reglas de los identificadores para variables se aplican a todos los demás tipos que se pueden nombrar libremente.

Solo hay cuatro reglas que recordar para los identificadores legales:

- Los identificadores deben comenzar con una letra, un símbolo de moneda o un símbolo `_`. Los símbolos de moneda incluyen dólar (`$`), yuan (`¥`), euro (`€`), etc.
- Los identificadores pueden incluir números, pero no comenzar con ellos.
- Un guion bajo simple (`_`) no está permitido como identificador.
- No se puede utilizar el mismo nombre que una **palabra reservada** de Java. Una palabra reservada es una palabra especial que Java ha apartado, por lo que no está permitido su uso. Se debe recordar que Java distingue entre mayúsculas y minúsculas (_case sensitive_), por lo que es posible usar versiones de las palabras clave que difieran solo en mayúsculas o minúsculas. Sin embargo, no se recomienda hacerlo.

No hay motivo de preocupación: no será necesario memorizar la lista completa de palabras reservadas. El examen solo preguntará sobre las que se utilizan habitualmente, como `class` y `for`. La Tabla 1.8 enumera todas las palabras reservadas en Java.

**TABLA 1.8** Palabras reservadas

||||||
|---|---|---|---|---|
|abstract|assert|boolean|break|byte|
|case|catch|char|class|const*|
|continue|default|do|double|else|
|enum|extends|final|finally|float|
|for|goto*|if|implements|import|
|instanceof|int|interface|long|native|
|new|package|private|protected|public|
|return|short|static|strictfp|super|
|switch|synchronized|this|throw|throws|
|transient|try|void|volatile|while|

_* Las palabras reservadas `const` y `goto` no se utilizan realmente en Java. Están reservadas para que las personas provenientes de otros lenguajes de programación no las usen por accidente y, en teoría, por si Java desea utilizarlas algún día._

Existen otros nombres que no se pueden usar. Por ejemplo, `true`, `false` y `null` son valores literales, por lo que no pueden ser nombres de variables. Además, existen palabras clave contextuales como `module` en el capítulo 12. Se debe estar preparado para ser evaluado sobre estas reglas.

Los siguientes ejemplos son **legales**:

``` Java
long okidentifier;
float $OK2Identifier;
boolean _alsoOK1d3ntifi3r;
char __SStillOkbutKnotsonice$;
```

Los siguientes ejemplos **no son legales**:

```Java 
int 3DPointClass;    // los identificadores no pueden comenzar con un número
byte hollywood@vine; // @ no es una letra, dígito, $ o _
String *$coffee;     // el primer carácter * no es una letra, $ o _
double public;       // public es una palabra reservada
short _;             // un guion bajo simple no está permitido
```

### camelCase y snake_case

Aunque es posible realizar combinaciones extrañas con los nombres de identificadores, se recomienda no hacerlo. Java posee convenciones para que el código sea legible y consistente. Por ejemplo, `camelCase` tiene la primera letra de cada palabra en mayúscula (excepto la primera, generalmente). Los nombres de métodos y variables se escriben típicamente en `camelCase` con la primera letra en minúscula, como `toUpper()`. Los nombres de clases e interfaces también se escriben en `camelCase`, con la primera letra en mayúscula, como `ArrayList`.

Otro estilo se denomina `snake_case`. Simplemente utiliza un guion bajo (`_`) para separar las palabras. Java utiliza generalmente `snake_case` en mayúsculas para constantes y valores de enumeración (_enum_), como `NUMBER_FLAGS`.

El examen no siempre seguirá estas convenciones para hacer que las preguntas sobre identificadores sean más complicadas. Por el contrario, las preguntas sobre otros temas generalmente sí siguen las convenciones estándar. Se recomienda seguir estas convenciones en el entorno laboral.

### Declaración de múltiples variables

También se pueden declarar e inicializar múltiples variables en la misma declaración. ¿Cuántas variables se considera que se declaran e inicializan en el siguiente ejemplo?

```Java
void sandFence() {
    String s1, s2;
    String s3 = "yes", s4 = "no";
}
```

Se declararon cuatro variables `String`: `s1`, `s2`, `s3` y `s4`. Se pueden declarar muchas variables en la misma declaración siempre que sean todas del mismo tipo. También se pueden inicializar cualquiera o todos esos valores en línea. En el ejemplo anterior, existen dos variables inicializadas: `s3` y `s4`. Las otras dos variables permanecen declaradas pero aún no inicializadas.

Aquí es donde la situación se vuelve complicada. Se debe prestar atención a los detalles complejos. El examen intentará confundir al candidato. Nuevamente, ¿cuántas variables se considera que se declaran e inicializan en el siguiente código?

```Java
void paintFence() {
    int i1, i2, i3 = 0;
}
```

Como cabría esperar, se declararon tres variables: `i1`, `i2` e `i3`. Sin embargo, solo uno de esos valores fue inicializado: `i3`. Los otros dos permanecen declarados pero aún no inicializados. Ese es el truco. Cada fragmento separado por una coma es una pequeña declaración propia. La inicialización de `i3` solo aplica a `i3`. No tiene relación alguna con `i1` o `i2` a pesar de estar en la misma sentencia. Como se verá en la siguiente sección, no es posible utilizar realmente `i1` o `i2` hasta que hayan sido inicializadas.

Otra forma en que el examen podría intentar confundirlo, mostrando un código como esta línea:

```Java
int num, String value; // NO COMPILA
```

Este código no compila porque intenta declarar múltiples variables de diferentes tipos en la misma sentencia. El atajo para declarar múltiples variables en la misma sentencia es legal solo cuando comparten un tipo.

Para asegurar la comprensión de esto, inténtese determinar cuáles de las siguientes son declaraciones legales:

``` Java
4: boolean b1, b2;
5: String s1 = "1", s2;
6: double d1, double d2;
7: int i1; int i2;
8: int i3; i4;
```

Las líneas 4 y 5 son legales. Cada una declara dos variables. La línea 4 no inicializa ninguna variable, y la línea 5 inicializa solo una. La línea 7 también es legal. Aunque `int` aparece dos veces, cada uno está en una sentencia separada. Un punto y coma (`;`) separa las sentencias en Java. Sucede que hay dos sentencias completamente diferentes en la misma línea.

La línea 6 no es legal. Java no permite declarar dos tipos diferentes en la misma sentencia. Un momento. Las variables `d1` y `d2` son del mismo tipo. Ambas son de tipo `double`. Aunque eso es cierto, sigue sin estar permitido. Si se desea declarar múltiples variables en la misma sentencia, deben compartir la misma declaración de tipo y no repetirla.

La línea 8 no es legal. Nuevamente, hay dos sentencias completamente diferentes en la misma línea. La segunda en la línea 8 no es una validación válida porque omite el tipo. Cuando se observe un punto y coma colocado de forma extraña en el examen, imagínese que el código está en líneas separadas y analícese si el código compila de esa manera. En este caso, las últimas dos líneas de código podrían reescribirse de la siguiente manera:

```Java
int i1;
int i2;
int i3;
i4;
```

Al observar la última línea por sí sola, se puede ver fácilmente que la declaración es inválida. Y sí, el examen realmente aglomera múltiples sentencias en la misma línea, en parte para intentar confundir y en parte para que quepa más código en la pantalla. En el mundo real, se recomienda limitarse a una declaración por sentencia y línea. El equipo de trabajo agradecerá el código legible.