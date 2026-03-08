Sin embargo, aún queda más por explorar sobre la coincidencia de patrones. En la siguiente sección, se utilizará la coincidencia de patrones con sentencias `switch`, y en el Capítulo 7, se aplicará la coincidencia de patrones a registros (_records_) y clases selladas (_sealed classes_).

Una sentencia `if/else` puede volverse realmente difícil de leer si existen muchas ramas. Observe lo siguiente:

```Java
String getAnimalBad(int type) {
    String animal;
    if (type == 0)
        animal = "Lion";
    else if (type == 1)
        animal = "Elephant";
    else if (type == 2 || type == 3)
        animal = "Alligator";
    else if (type == 4)
        animal = "Crane";
    else
        animal = "Unknown";
    return animal;
}
```

Cada vez que se añade un nuevo animal, ese código se vuelve más largo y difícil de mantener. Afortunadamente, Java incluye `switch` para ayudar a simplificar este código.

### Introducción a `switch`

Un `switch` es una estructura compleja de toma de decisiones en la cual se evalúa un valor único y el flujo se redirige a una o más ramas. En Java, existen dos variantes: una **sentencia `switch`** y una **expresión `switch`**. La diferencia principal entre ambas (¡además de muchas diferencias de sintaxis!) es que una expresión `switch` **debe** devolver un valor, mientras que una sentencia `switch` no.

Se comenzará reescribiendo el método anterior a uno que utilice una sentencia `switch`.

```Java
String getAnimalBetter(int type) {
    String animal;
    switch (type) {
        case 0:
            animal = "Lion";
            break;
        case 1:
            animal = "Elephant";
            break;
        case 2, 3:
            animal = "Alligator";
            break;
        case 4:
            animal = "Crane";
            break;
        default:
            animal = "Unknown";
    }
    return animal;
}
```

Ciertamente, esto es mejor que la versión `if/else` en términos de organización, pero sigue siendo muy extenso. Se asigna `animal` cuatro veces, y ¿qué sucede con todas las sentencias `break`? Se cubrirán todos estos detalles en breve, pero por ahora se intentará utilizar una expresión `switch` en su lugar.

```Java
String getAnimalBest(int type) {
    return switch (type) {
        case 0 -> "Lion";
        case 1 -> "Elephant";
        case 2, 3 -> "Alligator";
        case 4 -> "Crane";
        default -> "Unknown";
    };
}
```

¡Vaya, eso es mucho más corto y fácil de leer! En Java, las expresiones `switch` se introdujeron más recientemente que las sentencias `switch`, resultando en un mayor énfasis en la reducción de código repetitivo (_boilerplate_).

Como tal vez se recuerde del Capítulo 1, el espacio en blanco adicional no importa en Java. Dicho esto, el espacio en blanco puede utilizarse para alinear texto y ayudar a mejorar la legibilidad. Al escribir muchos de los ejemplos en este capítulo, se añadió espacio en blanco adicional para facilitar la lectura de sentencias y expresiones `switch`.

### Estructuración de sentencias y expresiones `switch`

Si bien las sentencias y expresiones `switch` pueden lucir diferentes, comparten muchas reglas comunes que se cubren en esta sección. Posteriormente, se cubrirán los detalles específicos de cada tipo.

#### Definición de un `switch`

En primer lugar, ambos tipos comienzan con una palabra clave `switch` y una variable envuelta en paréntesis.

```Java
String name = "123";
switch (name) { // Sentencia Switch
    case "Sancha": System.out.print(1); break;
    case "Jacob", "Jake": System.out.print(2); break;
    default: System.out.print(999); break;
}

System.out.println(switch (name) { // Expresión Switch
    case "Sancha" -> 1;
    case "Jacob", "Jake" -> 2;
    default -> 999;
});
```

Como se puede observar, ambos tipos de `switch` soportan cero o más cláusulas `case`. Cada cláusula `case` incluye un conjunto de valores coincidentes separados por comas (`,`). Luego es seguida por un separador, que puede ser dos puntos (`:`) o el operador flecha (`->`). Finalmente, cada cláusula define una expresión, o bloque de código con llaves (`{}`), para indicar qué ejecutar cuando hay una coincidencia.

Ambos tipos de `switch` soportan una cláusula `default` opcional. Con expresiones `switch`, a menudo se requiere una cláusula `default`, ya que la expresión debe devolver un valor. Más sobre esto en breve.

Sin más preámbulos, la siguiente imagen muestra la estructura de una sentencia `switch`. Si bien las sentencias `switch` pueden usar el operador flecha, se escriben más comúnmente con dos puntos.

![[Una instrucción switch.png]]

La imagen que se mostrara a continuación muestra la estructura de una expresión `switch`, la cual devuelve un valor que se asigna a una variable. Compare con la anterior imagen y asegúrese de comprender las diferencias entre ambas.

A diferencia de una sentencia `switch`, una expresión `switch` a menudo requiere un punto y coma (`;`) después de ella, como cuando se usa con el operador de asignación (`=`) o una sentencia `return`. Esto tiene más que ver con cómo se usa la expresión `switch` que con la expresión en sí misma.

Una expresión `switch` también requiere un punto y coma (`;`) después de cada expresión `case` que no use un bloque. Por ejemplo, ¿cuántos puntos y coma faltan en lo siguiente?

```Java
var result = switch (bear) {
    case 30 -> "Grizzly"
    default -> "Panda"
}
```

La respuesta es tres. Cada expresión `case` o `default` requiere un punto y coma, así como la asignación misma. Lo siguiente corrige el código:

```Java
var result = switch (bear) {
    case 30 -> "Grizzly";
    default -> "Panda";
};
```

![[Una expresión switch.png]]

**¡Hora del desafío!** Intente determinar cuál de estos compila:

```Java
int food = 5, month = 4, weather = 2, day = 0, time = 4;

String meal = switch food { // #1
    case 1 -> "Dessert"
    default -> "Porridge"
};

switch (month) // #2
    case 4: System.out.print("January");

switch (weather) { // #3
    case 2: System.out.print("Rainy");
    case 5: {
        System.out.print("Sunny");
    }
}

switch (day) { // #4
    case 1: 13: System.out.print("January");
    default System.out.print("July");
}

String description = switch (time) { // #5
    case 10 -> "Morning";
    default -> "Late";
}
```

La primera sentencia no compila porque a la expresión `switch` le faltan paréntesis alrededor de la variable `switch`, así como puntos y coma después de las cláusulas `case` y `default`. La segunda sentencia no compila porque le faltan llaves alrededor del cuerpo del `switch`. La tercera sentencia sí compila. Note que una cláusula `case` puede usar una expresión o un bloque de código con llaves.

La cuarta sentencia no compila por dos razones. La cláusula `case` debería usar una coma (`,`) para separar dos valores, no dos puntos (`:`). También le faltan dos puntos después de la cláusula `default`. La última sentencia no compila porque al operador de asignación (`=`) le falta un punto y coma (`;`) al final.

### Uso del operador flecha con sentencias `switch`

Aunque las sentencias `switch` soportan tanto los dos puntos como los operadores flecha, es probable verlos utilizados con dos puntos más a menudo en la práctica. Esto se debe a que la sintaxis de dos puntos ha existido por mucho más tiempo en Java. Si se utiliza el operador flecha, entonces se debe utilizar para **todas** las cláusulas. Por ejemplo, la siguiente sentencia `switch` no compila:

```Java
switch (type) {
    case 0 : System.out.print("Lion");
    case 1 -> System.out.print("Elephant"); // NO COMPILA (mezcla de : y ->)
}
```

Esto compilaría si ambas cláusulas utilizaran el mismo operador.

### Selección de la variable `switch`

Como se muestra en las imágenes anteriores, un `switch` tiene una variable objetivo que no se evalúa hasta el tiempo de ejecución. La siguiente es una lista de todos los tipos de datos soportados por `switch`:

- `int` e `Integer`
- `byte` y `Byte`
- `short` y `Short`
- `char` y `Character`
- `String`
- Valores `enum`
- Todos los tipos de objeto (cuando se usan con coincidencia de patrones)
- `var` (si el tipo se resuelve en uno de los tipos anteriores)

Note que `boolean`, `long`, `float` y `double` **no** están soportados en sentencias y expresiones `switch`.

Si nunca se ha trabajado con _enums_, no hay motivo de pánico. Para este capítulo, solo es necesario saber que una enumeración, o `enum`, es un tipo en Java que representa un conjunto fijo de constantes, como el siguiente:

```Java
enum Season { SPRING, SUMMER, FALL, WINTER }
enum DayOfWeek {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}
```

Si ayuda, piense en un `enum` como un tipo de clase con valores de objeto predefinidos conocidos en tiempo de compilación. Se cubren los _enums_ en detalle en el Capítulo 7, incluyendo cómo pueden definir variables, métodos y constructores. Por ahora, solo es necesario pensar en ellos como una lista de valores.

A partir de **Java 21**, las sentencias y expresiones `switch` ahora soportan la **coincidencia de patrones**, lo que significa que cualquier tipo de objeto ahora puede ser utilizado como una variable `switch`, siempre que se utilice la coincidencia de patrones. Se cubrirá `switch` con coincidencia de patrones en breve.

### Determinación de valores `case` aceptables

No cualquier valor puede ser utilizado en una cláusula `case`. Primero, los valores en cada cláusula `case` deben ser valores **constantes en tiempo de compilación**. Esto significa que solo se pueden usar literales, constantes de enumeración o variables constantes finales.

Por **constante final**, se entiende que la variable debe estar marcada con el modificador `final` e inicializada con un valor literal en la misma expresión en la que se declara. Por ejemplo, no se puede tener un valor de cláusula `case` que requiera ejecutar un método en tiempo de ejecución, incluso si ese método siempre devuelve el mismo valor.

Por estas razones, intente deducir por qué solo la primera y la última cláusula `case` compilan:

```Java
final int getCookies() { return 4; }
void feedAnimals() {
    final int bananas = 1;
    int apples = 2;
    int numberOfAnimals = 3;
    final int cookies = getCookies();
    switch (numberOfAnimals) {
        case bananas:
        case apples: // NO COMPILA
        case getCookies(): // NO COMPILA
        case cookies : // NO COMPILA
        case 3 * 5 :
    } 
}
```

La variable `bananas` está marcada como `final`, y su valor se conoce en tiempo de compilación, por lo que la primera cláusula `case` es válida. La variable `apples` en la segunda cláusula `case` no está marcada como `final`, por lo que no está permitida. Las siguientes dos cláusulas `case`, con valores `getCookies()` y `cookies`, no compilan porque los métodos no se evalúan hasta el tiempo de ejecución, por lo que no pueden usarse como el valor de una cláusula `case`, incluso si uno de los valores se almacena en una variable `final`. La última cláusula `case`, con valor `3 * 5`, sí compila, ya que las expresiones están permitidas como valores `case`, siempre que el valor pueda resolverse en tiempo de compilación.

Recuerde, el tipo de datos para las cláusulas `case` debe coincidir con el tipo de datos de la variable `switch`. Intente detectar por qué lo siguiente no compila:

```Java
String cleanFishTank(int dirty) {
    return switch (dirty) {
        case "Very" -> "1 hour"; // NO COMPILA (tipo incorrecto)
        default -> "45 minute";
    };
}
```

La variable `switch` es de tipo `int`, mientras que la cláusula `case` es de tipo `String`.

### Uso de valores `enum` con `switch`

Cuando la variable `switch` es un tipo `enum`, entonces las cláusulas `case` deben ser los valores del `enum`.

```Java
enum Season { SPRING, SUMMER, FALL, WINTER }
boolean shouldGetACoat(Season s) {
    return switch (s) {
        case SPRING -> false;
        case Season.SUMMER -> false; // NO COMPILA en versiones antiguas, válido en Java 21+
        case FALL -> true;
        case Season.WINTER -> true;
    };
}
```

Para un valor `enum`, se puede especificar solo el valor, como se muestra con `SPRING` y `FALL`. A partir de Java 21, opcionalmente se puede especificar el nombre con el valor, como `Season.SPRING` y `Season.FALL`.

### Trabajo con sentencias `switch`

Al observar el método `getAnimalBetter()` anterior, podría recordarse que había muchas sentencias `break` al final de cada `case`. Una sentencia `break` termina la sentencia `switch` y devuelve el control de flujo al proceso envolvente. En pocas palabras, finaliza la sentencia `switch` inmediatamente.

Aunque las sentencias `break` son opcionales, tienden a usarse frecuentemente en sentencias `switch`. Sin sentencias `break`, el código continúa ejecutando la siguiente rama que encuentra, en orden.

¿Qué se piensa que imprime lo siguiente cuando se llama a `printSeasonForMonth(2)`?

```Java
void printSeasonForMonth(int month) {
    switch (month) {
        case 1, 2, 3: System.out.print("Winter-");
        case 4, 5, 6: System.out.print("Spring-");
        default: System.out.print("Unknown-");
        case 7, 8, 9: System.out.print("Summer-");
        case 10, 11, 12: System.out.print("Fall-");
    } 
}
```

¡Imprime todo! `Winter-Spring-Unknown-Summer-Fall-`

Coincide con la primera cláusula `case` y ejecuta **todas** las ramas en el orden en que se encuentran, incluyendo la cláusula `default`. Recuerde que al trabajar con sentencias `switch`, ¡el orden de las ramas es importante! Se puede arreglar esto para imprimir solo `Winter-` con la misma entrada, añadiendo sentencias `break`.

```Java
void printSeasonForMonth(int month) {
    switch (month) {
        case 1, 2, 3: System.out.print("Winter-"); break;
        case 4, 5, 6: System.out.print("Spring-"); break;
        default: System.out.print("Unknown-"); break;
        case 7, 8, 9: System.out.print("Summer-"); break;
        case 10, 11, 12: System.out.print("Fall-"); break;
    } 
}
```

La última cláusula `case` en realidad no requiere un `break`, ya que la sentencia `switch` ha terminado, pero se añade por consistencia.

Los creadores del examen son aficionados a los ejemplos de `switch` a los que les faltan sentencias `break`. Cuando se detecte una sentencia `switch` en el examen, siempre se debe considerar si se pueden visitar múltiples ramas en una sola ejecución.

Contraste esto con una expresión `switch` que coincide solo con una rama única en tiempo de ejecución y, por lo tanto, no requiere sentencias `break`.

```Java
void printSeasonForMonth(int month) {
    String value = switch (month) {
        case 1, 2, 3 -> "Winter-";
        case 4, 5, 6 -> "Spring-";
        default -> "Unknown-";
        case 7, 8, 9 -> "Summer-";
        case 10, 11, 12 -> "Fall-";
    };
    System.out.print(value);
}
```

### ¿Cuándo una expresión `switch` no es una expresión `switch`?

Como se indicó anteriormente, una expresión `switch` siempre devuelve un valor, independientemente de la sintaxis utilizada. ¿Qué sucede con lo siguiente?

```Java
void printWeather(int rain) {
    switch (rain) {
        case 0 -> System.out.print("Dry");
        case 1 -> System.out.print("Wet");
        case 2 -> System.out.print("Storm");
    }
}
```

Dado que el tipo de retorno de `System.out.print()` es `void`, esta sentencia no devuelve un valor. En realidad, esta es una sentencia `switch` que utiliza la sintaxis del operador flecha. Dado que no devuelve un valor, no es una expresión `switch`. ¡Es un poco confuso, se sabe!

### Trabajo con expresiones `switch`

¡Felicitaciones, ahora se es un experto en sentencias `switch`! Desafortunadamente, las expresiones `switch` tienen muchas más características y reglas que aún se deben cubrir. Se cubren (una a la vez) en esta sección.

#### Retorno de tipos de datos consistentes

Al igual que los valores `case` deben usar un tipo de datos consistente, la expresión `switch` debe devolver un valor consistente. En pocas palabras, cuando se asigna un valor como resultado de una expresión `switch`, una rama no puede devolver un valor con un tipo no relacionado.

```Java
int measurement = 10;
int size = switch (measurement) {
    case 5 -> Integer.valueOf(1);
    case 10 -> (short)2;
    default -> 3;
    case 20 -> "4"; // NO COMPILA
    case 40 -> 5L; // NO COMPILA
    case 50 -> null; // NO COMPILA
};
```

La expresión `switch` se está asignando a una variable `int`, por lo que todos los valores deben ser consistentes con `int`. La primera cláusula `case` compila sin problemas, ya que el valor `Integer` se desempaqueta (_unboxed_) a `int`. Se cubrirá el _autoboxing_ y _unboxing_ en el Capítulo 5, «Métodos». La segunda y tercera cláusulas `case` están bien, ya que pueden almacenarse como un `int`. Las últimas tres expresiones `case` no compilan porque cada una devuelve un tipo que es incompatible con `int`.

#### Agotamiento de las ramas `switch`

A diferencia de una sentencia `switch`, una expresión `switch` debe devolver un valor. ¿Por qué? Se intentará un ejemplo ilustrativo.

```Java
void identifyType(String type) {
    Integer reptile = switch (type) { // NO COMPILA
        case "Snake" -> 1;
        case "Turtle" -> 2;
    };
}
```

¿Cuál es el valor de `reptile` si `type` no es igual a "Snake" o "Turtle"? ¿Lanza una excepción? ¿Devuelve `null` o -1? La respuesta es «Ninguna de las anteriores». Java decidió que este comportamiento no sería soportado y provoca un error de compilación si la expresión `switch` no es **exhaustiva**.

Se dice que un `switch` es exhaustivo si cubre todos los valores posibles. Todas las expresiones `switch` deben ser exhaustivas, lo que significa que deben manejar todos los valores posibles. Como se verá en breve, hay ocasiones en las que una sentencia `switch` debe ser exhaustiva también. Hay tres formas de escribir un `switch` exhaustivo:

1. Añadir una cláusula `default`.
2. Si el `switch` toma un `enum`, añadir una cláusula `case` para cada valor posible del `enum`.
3. Cubrir todos los tipos posibles de la variable `switch` con coincidencia de patrones.

En la práctica, la primera solución es la que se utiliza con más frecuencia. Se puede intentar escribir cláusulas `case` para todos los valores `int` posibles, ¡pero se promete que no funciona! Incluso tipos más pequeños como `byte` no son permitidos por el compilador sin un `default`, a pesar de haber solo 256 valores posibles.

La segunda solución se aplica solo a expresiones `switch` que toman un `enum`. Por ejemplo, considere lo siguiente:

```Java
enum Season { SPRING, SUMMER, FALL, WINTER }
String getWeatherMissingOne(Season value) {
    return switch (value) { // NO COMPILA
        case WINTER -> "Cold";
        case SPRING -> "Rainy";
        case SUMMER -> "Hot";
    };
}
```

Este código no compila porque `FALL` no está cubierto. La solución es añadir un `case` para `FALL` o añadir una cláusula `default` (o ambas).

```Java
String getWeatherCoveredAll(Season value) {
    return switch (value) {
        case WINTER -> "Cold";
        case SPRING -> "Rainy";
        case SUMMER -> "Hot";
        case FALL -> "Warm";
        default -> throw new RuntimeException("Unsupported Season");
    };
}
```

Dado que todos los valores posibles de `Season` están cubiertos, la rama `default` es opcional.

Al escribir expresiones `switch`, puede ser una buena idea añadir una rama `default`, incluso si se cubren todos los valores posibles. Esto significa que si alguien modifica el `enum` con un nuevo valor, el código seguirá compilando.

La tercera solución para escribir un `switch` exhaustivo requiere alguna explicación. Se cubrirá esto en una próxima sección, ya que solo se aplica a la coincidencia de patrones.

### Uso de la sentencia `yield`

Hasta ahora, los ejemplos de expresiones `switch` mostrados utilizaban expresiones `case`. ¡Es hora de expandir el conocimiento a bloques `case`! Una expresión `switch` soporta tanto expresiones `case` como bloques `case`, denotándose estos últimos con llaves (`{}`). La siguiente imagen muestra ejemplos de ambos.

![[Una expresión switch con un bloque case y una instrucción yield.png]]

En la sección anterior, se dijo que una expresión `switch` debe devolver un valor. Pero, ¿cómo se devuelve un valor desde un bloque `case`? Se podría usar una sentencia `return`, pero eso finalizaría el método, no solo la expresión `switch`.

Aquí entra la sentencia `yield`, mostrada en la anterior imagen. Permite que la cláusula `case` devuelva un valor. Por ejemplo, lo siguiente utiliza una mezcla de expresiones `case` y bloques `case`:

```Java
int fish = 5;
int length = 12;
var name = switch (fish) {
    case 1 -> "Goldfish";
    case 2 -> { yield "Trout"; }
    case 3 -> {
        if (length > 10) yield "Blobfish";
        else yield "Green";
    }
    case 4 -> {
        throw new RuntimeException("Unsupported value");
    }
    default -> "Swordfish";
};
```

Piense en la palabra clave `yield` como una sentencia `return` dentro de una expresión `switch`. Debido a que una expresión `switch` debe devolver un valor, a menudo se requiere un `yield` dentro de un bloque `case`. La única «excepción» a esta regla es si el código lanza una excepción, como se muestra en el ejemplo anterior.

### Atención a los puntos y coma en expresiones `switch`

Al escribir una **expresión** `case` (con `->`), se requiere un punto y coma, pero al escribir un **bloque** `case` (con `{}`), está prohibido después del bloque.

```Java
int fish = 1;
var name = switch (fish) {
    case 1 -> "Goldfish" // NO COMPILA (falta punto y coma)
    case 2 -> { yield "Trout"; }; // NO COMPILA (sobra punto y coma)
    default -> "Shark";
} // NO COMPILA (falta punto y coma)
```

Un poco confuso, ¿verdad? Es solo una de esas cosas que hay que entrenarse para detectar en el examen.

### Uso de coincidencia de patrones con `switch`

Una de las características nuevas más importantes de **Java 21** es que la coincidencia de patrones se ha extendido a `switch`. Hay una serie de reglas que cubrir, así que se comenzará con lo básico. Para usar la coincidencia de patrones con un `switch`, primero se comienza con una variable de referencia a objeto. Se permite cualquier tipo de referencia a objeto, siempre que el `switch` haga uso de la coincidencia de patrones. A continuación, en cada cláusula `case`, se define un tipo y una variable de coincidencia de patrones.

```Java
void printDetails(Number height) {
    String message = switch (height) {
        case Integer i -> "Rounded: " + i;
        case Double d -> "Precise: " + d;
        case Number n -> "Unknown: " + n;
    };
    System.out.print(message);
}
```

En este ejemplo, se emiten valores diferentes dependiendo del tipo de la variable `switch`. Se aplican las mismas reglas sobre variables locales y alcance de flujo (_flow scoping_) que se aprendieron anteriormente con la coincidencia de patrones. Por ejemplo, la variable de coincidencia de patrones existe solo dentro de la rama `case` para la cual está definida. Esto permite reutilizar el mismo nombre para dos ramas `case`.

La figura que se mostrara a continuación muestra la estructura de la coincidencia de patrones con una expresión `switch`. También puede usarse con una sentencia `switch` que no devuelva un valor.

Fácil hasta ahora, ¿verdad? De la figura que se mostrara, podría haberse notado que incluye una **cláusula de guardia** (_guard clause_), que es una cláusula condicional opcional que se puede agregar a una rama `case`. Esto es similar a lo que se vio en la imagen [[Coincidencia de patrones con if.png]] con una sentencia `if` de coincidencia de patrones. La única diferencia es que con `switch`, la palabra clave `when` es requerida entre la variable y la expresión.

Se probará con un ejemplo. Suponga que el zoológico tiene diferentes entrenadores que pueden manejar animales de diferentes tamaños dependiendo del tipo de medida.

```Java
String getTrainer(Number height) {
    return switch (height) {
        case Integer i when i > 10 -> "Joseph";
        case Integer i -> "Daniel";
        case Double num when num <= 15.5 -> "Peter";
        case Double num -> "Kelly";
        case Number num -> "Ralph";
    };
}
```

En este ejemplo, Joseph trabaja con el animal si la altura es un `Integer` mayor que 10. Daniel es seleccionado para todos los demás valores `Integer`. De manera similar, Peter maneja todas las medidas `Double` menores o iguales a 15.5, mientras que Kelly maneja los valores `Double` restantes. Finalmente, Ralph maneja todos los animales que no cumplen con uno de los requisitos anteriores, como si se usara `Short`.

![[Coincidencia de patrones con switch.png]]

Una ventaja de las guardias es que ahora `switch` puede hacer algo que nunca había hecho antes: puede manejar **rangos**. Anteriormente, si se quería soportar un rango de valores con un `switch`, se tenían que listar todos los valores `case` posibles. Con la cláusula `when`, se pueden soportar coincidencias de rango. ¡Bastante conveniente!

### Aplicación de tipos aceptables

Una de las reglas más simples al trabajar con `switch` y coincidencia de patrones es que el tipo no puede ser no relacionado. Debe ser el mismo tipo que la variable `switch` o un subtipo.

```Java
Number fish = 10;
String name = switch (fish) {
    case Integer freshWater -> "Bass";
    case Number saltWater -> "ClownFish";
    case String s -> "Shark"; // NO COMPILA (Tipo no relacionado)
};
```

El compilador es lo suficientemente inteligente para saber que un `Number` no puede ser convertido (_cast_) como un `String`, resultando en que este código no compile. ¡Esto tampoco estaba permitido en la sección anterior de coincidencia de patrones usando `instanceof`!

### Ordenamiento de ramas `switch`

Como se mencionó anteriormente en el capítulo, el orden de las cláusulas `case` y `default` para la sentencia `switch` importa, porque se puede llegar a más de una rama durante la ejecución. Para expresiones `switch` que no usan coincidencia de patrones, el orden no es importante, ya que solo se puede llegar a una rama.

Bueno, al trabajar con coincidencia de patrones, ¡el orden importa independientemente del tipo de `switch`! Por ejemplo, considérese esta nueva versión de `printDetails()` en la cual se ha cambiado el orden:

```Java
void printDetails(Number height) {
    String message = switch (height) {
        case Number n -> "Unknown: " + n;
        case Integer i -> "Rounded: " + i; // NO COMPILA (Dominado)
        case Double d -> "Precise: " + d;  // NO COMPILA (Dominado)
    };
    System.out.print(message);
}
```

El código ya no compila ya que la segunda y tercera cláusulas `case` se consideran **dominadas** por la sentencia `case Number` precedente. Dicho de otra manera, es imposible que cualquier proceso llegue a estas dos cláusulas `case`. Esto también se conoce como **código inalcanzable** (_unreachable code_), lo cual se cubre más adelante en este capítulo. En la mayoría de los casos, cuando el compilador detecta código inalcanzable, resulta en un error de compilación.

El orden de las ramas también es importante si se usa una cláusula `when`. Por ejemplo, ¿qué pasaría si se reordenaran las dos primeras ramas del método `getTrainer()`?

```Java
String getTrainer(Number animal) {
    return switch (animal) {
        case Integer i -> "Daniel";
        case Integer i when i > 10 -> "Joseph"; // NO COMPILA (Dominado)
        …
    };
}
```

En el caso de que `animal` sea un `Integer`, Daniel siempre será seleccionado. ¡Pobre Joseph! Del mismo modo, el compilador no permite que este código compile.

### Sentencias `switch` exhaustivas

Hasta ahora, solo se requería que las **expresiones** `switch` fueran exhaustivas. Cuando se usa coincidencia de patrones, las **sentencias** `switch` deben ser exhaustivas también.

Como antes, se puede abordar esto con una cláusula `default`, ya que esto manejará todos los valores que no coincidan con una cláusula `case`. Sin embargo, hay otra opción que se aplica solo cuando se trabaja con coincidencia de patrones.

Es posible que se haya notado que no se utilizaron cláusulas `default` en ninguno de los ejemplos anteriores de coincidencia de patrones. Eso es porque se definió la última cláusula `case` con un tipo de variable de coincidencia de patrones que es el mismo que el tipo de referencia de la variable `switch`.

Eso puede sonar complicado, pero es más simple de lo que parece. Por ejemplo, si el tipo de referencia de variable de la expresión `switch` es tipo `Integer` o `String`, entonces solo es necesario asegurarse de que la última cláusula `case` sea de tipo `Integer` o `String`, respectivamente.

Se probará con un ejemplo ilustrativo. ¿Cuál se espera que sea la salida de lo siguiente?

```Java
Number zooPatrons = Integer.valueOf(1_000);
switch (zooPatrons) {
    case Integer count -> System.out.print("Welcome: " + count);
}
```

¡No compila! A pesar de que el objeto `zooPatrons` es realmente de tipo `Integer`, la variable de referencia del `switch` es de tipo `Number`. Hay algunas formas en que se puede arreglar esto. Primero, se puede cambiar el tipo de referencia de `zooPatrons` para que sea `Integer`, lo que resulta en que todos los valores posibles de `Integer` estén cubiertos.

```Java
Integer zooPatrons = Integer.valueOf(1_000);
switch (zooPatrons) {
    case Integer count -> System.out.print("Welcome: " + count);
}
```

Alternativamente, también se puede añadir una cláusula `case` al final para `Number`.

```Java
Number zooPatrons = Integer.valueOf(1_000);
switch (zooPatrons) {
    case Integer count -> System.out.print("Welcome: " + count);
    case Number count -> System.out.print("Too many people at the zoo!");
}
```

¡También hay una tercera opción! No se olvide, siempre se puede añadir una cláusula `default` a un `switch` que cubra todo.

Eso lleva a una pregunta interesante: ¿qué sucede si se combinan diferentes soluciones?

```Java
Number zooPatrons = Integer.valueOf(1_000);
switch (zooPatrons) {
    case Integer count -> System.out.print("Welcome: " + count);
    case Number count -> System.out.print("Too many people at the zoo!");
    default -> System.out.print("The zoo is closed"); // Inalcanzable
}
```

En este caso, el código no compila, independientemente de cómo se ordenen las ramas. El compilador es lo suficientemente inteligente para darse cuenta de que las dos últimas sentencias son redundantes, ya que una siempre domina a la otra.

### Manejo de un caso `null`

¿Qué sucede si la variable `switch` es `null` en tiempo de ejecución? Se puede intentar usar una cláusula `default`, pero el resultado podría sorprender.

```Java
String fish = null;
System.out.print(switch (fish) {
    case "ClownFish" -> "Hello!";
    case "BlueTang" -> "Hello again!";
    default -> "Goodbye";
});
```

Este código compila (técnicamente es exhaustivo) pero lanza una `NullPointerException`. Una «solución rápida» sería agregar una sentencia `if/else` alrededor del `switch`, pero eso añadiría mucho código repetitivo extra.

Nuevo en **Java 21**, `switch` ahora soporta la cláusula `case null` al trabajar con tipos de objetos, permitiendo reescribir el ejemplo anterior como sigue:

```Java
String fish = null;
System.out.print(switch (fish) {
    case "ClownFish" -> "Hello!";
    case "BlueTang" -> "Hello again!";
    case null -> "What type of fish are you?";
    default -> "Goodbye";
});
```

Eso es mucho menos código repetitivo, ahora que no se tiene que manejar `null` por separado.

Dado que usar `case null` implica coincidencia de patrones, el orden de las ramas importa siempre que se use `case null`. Si bien `case null` puede aparecer casi en cualquier lugar en `switch`, **no** puede usarse después de una sentencia `default`. Por ejemplo, solo la primera de las siguientes dos sentencias `switch` compila.

```Java
// Compila
System.out.print(switch (fish) {
    case String s when "ClownFish".equals(s) -> "Hello!";
    case null -> "No good";
    case String s when "BlueTang".equals(s) -> "Hello again!";
    default -> "Goodbye";
});

// NO COMPILA
System.out.print(switch (fish) {
    case String s when "ClownFish".equals(s) -> "Hello!";
    case String s when "BlueTang".equals(s) -> "Hello again!";
    default -> "Goodbye";
    case null -> "No good"; // NO COMPILA (después de default)
});
```

En el segundo ejemplo, la cláusula `default` domina a la cláusula `case null`. Para el examen, asegúrese de poder identificar dónde pueden usarse `default` y `case null` dentro de un `switch`.

### `Case null` se considera coincidencia de patrones

¿Alguna suposición de por qué el siguiente fragmento de código no compila?

```Java
String fish = null;
switch (fish) { // NO COMPILA
    case "ClownFish": System.out.print("Hello!");
    case "BlueTang": System.out.print("Hello again!");
    case null: System.out.print("What type of fish are you?");
}
```

Siempre que se use `case null` dentro de un `switch`, entonces se considera que la sentencia `switch` usa coincidencia de patrones. Como se debería recordar de la sección anterior, eso significa que la sentencia `switch` **debe ser exhaustiva**. Añadir una rama `default` permite que el código compile.