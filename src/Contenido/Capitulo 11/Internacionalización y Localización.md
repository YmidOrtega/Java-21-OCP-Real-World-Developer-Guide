Muchas aplicaciones necesitan funcionar en diferentes países y con diferentes idiomas. Por ejemplo, **consideremos** la frase "El zoológico **tiene** un evento especial el 4/1/25 para observar comportamientos animales." ¿Cuándo es el evento? En Estados Unidos, es el 1 de abril. Sin embargo, un lector británico lo **interpretaría** como el 4 de enero. Un lector británico también **podría** preguntarse por qué no escribimos "_behaviours_". Si **se está** creando un sitio web o programa que **se usará** en múltiples países, **se quiere** usar el idioma y el formato correctos.

_Internacionalización_ es el proceso de diseñar el programa para que pueda **adaptarse**. Esto implica colocar cadenas en un archivo de propiedades y asegurarse de que **se usen** los formateadores de datos apropiados. _Localización_ significa soportar múltiples locales o regiones geográficas. **Se puede** pensar en un local como un par de idioma y país. La localización incluye traducir cadenas a diferentes idiomas. También incluye mostrar fechas y números en el formato correcto para ese local.

> **Nota:** Inicialmente, el programa no necesita soportar múltiples locales. La clave es preparar la aplicación para el futuro usando estas técnicas. De esta manera, cuando el producto **sea** exitoso, **se puede** agregar soporte para nuevos idiomas o regiones sin reescribir todo.

En esta sección, **se ve** cómo definir un local y usarlo para formatear fechas, números y cadenas.

## Eligiendo un Locale

Mientras que Oracle define un local como "una región geográfica, política o cultural específica", en el examen solo **se verán** idiomas y países. Oracle ciertamente no va a adentrarse en regiones políticas que no son países. ¡Eso es demasiado controvertido para un examen!

La clase `Locale` está en el paquete `java.util`. El primer `Locale` útil a encontrar es el local actual del usuario. **Se intenta** ejecutar el siguiente código en la computadora:

```Java
Locale locale = Locale.getDefault();
System.out.println(locale);
```

Al ejecutarlo, **imprime** `en_US`. Esto puede ser diferente en otra computadora. Esta salida predeterminada **indica** que la computadora está usando inglés y está en los Estados Unidos.

**Hay que notar** el formato. Primero viene el código de idioma en minúsculas. El idioma siempre **es requerido**. Luego viene un guión bajo seguido del código de país en mayúsculas. El país es opcional. La imagen a continuación muestra los dos formatos para objetos `Locale` que **hay que** recordar.

![[Formatos de configuración regional.jpeg]]

Como práctica, **hay que** asegurarse de entender por qué cada uno de estos identificadores de `Locale` es inválido:

```Java
US     // No puede tener país sin idioma
enUS   // Falta el guión bajo
US_en  // El país y el idioma están invertidos
EN     // El idioma debe ser en minúsculas
```

Las versiones corregidas son `en` y `en_US`.

> **Nota:** No **hay que** memorizar los códigos de idioma o país. El examen **informará** sobre cualquier código que **se use**. Sí **hay que** reconocer formatos válidos e inválidos. **Hay que prestar** atención a las mayúsculas/minúsculas y al guión bajo. Por ejemplo, si **se ve** un local expresado como `es_CO`, **se debe** saber que el idioma es `es` y el país es `CO`, aunque no **se sepa** que representan español y Colombia, respectivamente.

Como desarrollador, a menudo **se necesita** escribir código que **seleccione** un local diferente al predeterminado. Hay tres formas comunes de hacerlo. La primera es usar las constantes incorporadas en la clase `Locale`, disponibles para algunos locales comunes.

```Java
System.out.println(Locale.GERMAN);  // de
System.out.println(Locale.GERMANY); // de_DE
```

El primer ejemplo **selecciona** el idioma alemán, que **se habla** en muchos países, incluyendo Austria (`de_AT`) y Liechtenstein (`de_LI`). El segundo ejemplo **selecciona** tanto el idioma alemán como el país Alemania. Aunque estos ejemplos pueden parecer similares, no son lo mismo. Solo uno incluye un código de país.

La segunda forma de **seleccionar** un `Locale` es usar los métodos de fábrica `Locale.of()`. **Se puede** pasar solo un idioma, o tanto un idioma como un país:

```Java
System.out.println(Locale.of("fr"));       // fr
System.out.println(Locale.of("hi", "IN")); // hi_IN
```

El primero es el idioma francés, y el segundo es hindi en India. Nuevamente, no **hay que** memorizar los códigos. Java permitirá crear un `Locale` con un idioma o país no válido, como `xx_XX`. Sin embargo, no **coincidirá** con el `Locale` que **se quiere** usar, y el programa no **se comportará** como **se espera**.

Hay una tercera forma de crear un `Locale` que es más flexible. El patrón de diseño _builder_ permite **establecer** todas las propiedades que **se deseen** y luego **construir** el `Locale` al final. Esto significa que **se pueden** especificar las propiedades en cualquier orden. Los siguientes dos valores de `Locale` representan `en_US`:

```Java
Locale l1 = new Locale.Builder()
    .setLanguage("en")
    .setRegion("US")
    .build();

Locale l2 = new Locale.Builder()
    .setRegion("US")
    .setLanguage("en")
    .build();
```

> **Nota:** Hay en realidad una cuarta forma de crear una instancia de `Locale`, usando un constructor de `Locale`, como `new Locale("en")` o `new Locale("en", "US")`. Estos constructores ahora están obsoletos, así que **hay que** usar una de las tres técnicas anteriores en su lugar.

Al probar un programa, puede que **se necesite** usar un `Locale` diferente al predeterminado de la computadora.

```Java
System.out.println(Locale.getDefault()); // en_US
Locale locale = Locale.of("fr");
Locale.setDefault(locale);
System.out.println(Locale.getDefault()); // fr
```

**Hay que** intentarlo y no preocuparse — el `Locale` cambia solo para ese programa Java. No **cambia** ninguna configuración en la computadora. Ni siquiera **cambia** ejecuciones futuras del mismo programa.

> **Nota:** El examen puede usar `setDefault()` porque no puede hacer suposiciones sobre dónde **se está** ubicado. En la práctica, raramente **se escribe** código para cambiar el local predeterminado de un usuario.

## Localizando Números

Puede sorprender que el formateo o parseo de valores de moneda y número **pueda** cambiar dependiendo del local. Por ejemplo, en Estados Unidos, el signo de dólar **se antepone** al valor junto con un punto decimal para valores menores a un dólar, como `$2.15`. En Alemania, sin embargo, el símbolo del euro **se añade** al valor junto con una coma para valores menores a un euro, como `2,15 €`.

Afortunadamente, el paquete `java.text` incluye clases para solucionar esto. Las siguientes secciones cubren cómo formatear números, moneda y fechas basándose en el local.

El primer paso para formatear o parsear datos es el mismo: **obtener** una instancia de un `NumberFormat`. La Tabla 11.8 muestra los métodos de fábrica disponibles.

**TABLA 11.8** Métodos de fábrica para obtener un `NumberFormat`

| **Descripción** | **Usando Locale predeterminado y uno especificado** |
|---|---|
| Formateador de propósito general | `NumberFormat.getInstance()` / `NumberFormat.getInstance(Locale locale)` |
| Igual que `getInstance` | `NumberFormat.getNumberInstance()` / `NumberFormat.getNumberInstance(Locale locale)` |
| Para formatear cantidades monetarias | `NumberFormat.getCurrencyInstance()` / `NumberFormat.getCurrencyInstance(Locale locale)` |
| Para formatear porcentajes | `NumberFormat.getPercentInstance()` / `NumberFormat.getPercentInstance(Locale locale)` |
| Redondea valores decimales antes de mostrar | `NumberFormat.getIntegerInstance()` / `NumberFormat.getIntegerInstance(Locale locale)` |
| Devuelve formateador de número compacto | `NumberFormat.getCompactNumberInstance()` / `NumberFormat.getCompactNumberInstance(Locale locale, NumberFormat.Style formatStyle)` |

Una vez que **se tiene** la instancia de `NumberFormat`, **se puede** llamar a `format()` para convertir un número a un `String`, o usar `parse()` para convertir un `String` en un número.

> **Consejo:** Las clases de formato no son thread-safe. No **hay que** almacenarlas en variables de instancia o `static`. **Se aprende** más sobre seguridad de hilos en el Capítulo 13, "Concurrencia".

#### Formateando Números

Al formatear datos, **se convierten** de un objeto estructurado o valor primitivo a un `String`. El método `NumberFormat.format()` formatea el número dado basándose en el local asociado con el objeto `NumberFormat`.

**Volvamos** al zoológico por un momento. Para material de marketing, **se quiere** compartir el número promedio mensual de visitantes al Zoológico de San Diego. Lo siguiente **muestra** cómo **se imprime** el mismo número en tres locales diferentes:

```Java
int attendeesPerYear = 3_200_000;
int attendeesPerMonth = attendeesPerYear / 12;

var us = NumberFormat.getInstance(Locale.US);
System.out.println(us.format(attendeesPerMonth)); // 266,666

var gr = NumberFormat.getInstance(Locale.GERMANY);
System.out.println(gr.format(attendeesPerMonth)); // 266.666

var ca = NumberFormat.getInstance(Locale.CANADA_FRENCH);
System.out.println(ca.format(attendeesPerMonth)); // 266 666
```

Esto **muestra** cómo los visitantes de EE.UU., Alemania y Canadá francófono pueden ver la misma información en el formato de número al que **están** acostumbrados. En la práctica, simplemente **se llamaría** a `NumberFormat.getInstance()` y **se dependería** del local predeterminado del usuario para formatear la salida.

El formateo de moneda funciona de la misma manera.

```Java
double price = 48;
var myLocale = NumberFormat.getCurrencyInstance();
System.out.println(myLocale.format(price));
```

Cuando **se ejecuta** con el local predeterminado de `en_US` para Estados Unidos, este código **produce** `$48.00`. Por otro lado, cuando **se ejecuta** con el local predeterminado de `en_GB` para Gran Bretaña, **produce** `£48.00`.

> **Nota:** En el mundo real, **hay que** usar `int` o `BigDecimal` para dinero y no `double`. Hacer matemáticas con cantidades con `double` es peligroso porque los valores **se almacenan** como números de punto flotante. ¡El jefe no **apreciará** que **se pierdan** centavos o fracciones de centavos durante las transacciones!

Finalmente, el examen puede tener ejemplos que muestran el formateo de porcentajes:

```Java
double successRate = 0.802;
var us = NumberFormat.getPercentInstance(Locale.US);
System.out.println(us.format(successRate)); // 80%

var gr = NumberFormat.getPercentInstance(Locale.GERMANY);
System.out.println(gr.format(successRate)); // 80 %
```

No hay mucha diferencia, lo sabemos, pero al menos **hay que** ser conscientes de que la capacidad de imprimir un porcentaje es específica del local para el examen.

#### Parseando Números

Al parsear datos, **se convierten** de un `String` a un objeto estructurado o valor primitivo. El método `NumberFormat.parse()` **logra** esto y **toma** el local en consideración.

Por ejemplo, si el local es inglés/Estados Unidos (`en_US`) y el número contiene comas, las comas **se tratan** como símbolos de formateo. Si el local **se relaciona** con un país o idioma que usa comas como separador decimal, la coma **se trata** como un punto decimal.

**Veamos** un ejemplo. El siguiente código **parsea** un precio de boleto con descuento con diferentes locales. El método `parse()` **lanza** una `ParseException` verificada, así que **hay que** manejarla o declararla en el código propio.

```Java
String s = "40.45";

var en = NumberFormat.getInstance(Locale.US);
System.out.println(en.parse(s)); // 40.45

var fr = NumberFormat.getInstance(Locale.FRANCE);
System.out.println(fr.parse(s)); // 40
```

En Estados Unidos, un punto (`.`) es parte de un número, y el número **se parsea** como **se esperaría**. Francia no usa un punto decimal para separar números. Java lo **parsea** como un carácter de formateo y **deja** de analizar el resto del número. La lección es **asegurarse** de parsear usando el local correcto.

El método `parse()` también **se usa** para parsear moneda. Por ejemplo, **se puede** leer el ingreso mensual del zoológico por venta de boletos:

```Java
String income = "$92,807.99";
var cf = NumberFormat.getCurrencyInstance();
double value = (Double) cf.parse(income);
System.out.println(value); // 92807.99
```

La cadena de moneda `"$92,807.99"` contiene un signo de dólar y una coma. El método `parse` **elimina** los caracteres y **convierte** el valor en un número. El valor de retorno de `parse` es un objeto `Number`. `Number` es la clase padre de todas las clases envolventes de `java.lang`, así que el valor de retorno **puede** convertirse a su tipo de dato apropiado. El `Number` **se convierte** a `Double` y luego **se desempaqueta** automáticamente en un `double`.

#### Formateando con *CompactNumberFormat*

La segunda clase que hereda `NumberFormat` que **hay que** conocer para el examen es `CompactNumberFormat`. Si no **se ha** visto antes, no hay que preocuparse; **se cubre** en esta sección.

`CompactNumberFormat` es similar a `DecimalFormat`, pero **está** diseñado para **ser usado** en lugares donde el espacio de impresión puede ser limitado. Es inflexible en el sentido de que **elige** un formato, y específico del local en que la salida puede cambiar dependiendo de la ubicación.

**Consideremos** el siguiente código de muestra que **aplica** un `CompactNumberFormat` a un grupo de locales, usando un `static import` para `Style` (un enum con valor `SHORT` o `LONG`):

```Java
var formatters = Stream.of(
    NumberFormat.getCompactNumberInstance(),
    NumberFormat.getCompactNumberInstance(Locale.getDefault(), Style.SHORT),
    NumberFormat.getCompactNumberInstance(Locale.getDefault(), Style.LONG),

    NumberFormat.getCompactNumberInstance(Locale.GERMAN, Style.SHORT),
    NumberFormat.getCompactNumberInstance(Locale.GERMAN, Style.LONG),

    NumberFormat.getNumberInstance());

formatters.map(s -> s.format(7_123_456)).forEach(System.out::println);
```

Lo siguiente **es impreso** por este código cuando **se ejecuta** en el local `en_US` (saltos de línea agregados para legibilidad):

```Plaintext
7M
7M
7 million
7 Mio.
7 Millionen
7,123,456
```

**Hay que notar** que las dos primeras líneas son iguales. Si no **se especifica** un estilo, `SHORT` **se usa** por defecto. A continuación, **hay que notar** que los valores excepto el último (que no usa un formateador de número compacto) **se truncan**. ¡Por eso **se llama** formateador de número compacto! También **hay que notar** que el formato corto usa etiquetas comunes para valores grandes, como `K` para miles. Por último, la salida puede **diferir** al ejecutar esto, ya que **se ejecutó** en un local `en_US`.

Usando los mismos formateadores, **hagamos** otro ejemplo:

```Java
formatters.map(s -> s.format(314_900_000)).forEach(System.out::println);
```

Esto **imprime** lo siguiente cuando **se ejecuta** en el local `en_US`:

```Plaintext
315M
315M
315 million
315 Mio.
315 Millionen
314,900,000
```

**Hay que notar** que el tercer dígito **se redondea** automáticamente hacia arriba en las entradas que usan `CompactNumberFormat`. Las siguientes **son** las reglas para `CompactNumberFormat`:

- Primero **determina** el rango más alto para el número, como miles (`K`), millones (`M`), miles de millones (`B`) o trillones (`T`).
- Luego **devuelve** hasta los primeros tres dígitos de ese rango, redondeando el último dígito según sea necesario.
- Finalmente, **imprime** un identificador. Si **se usa** `SHORT`, **se devuelve** un símbolo. Si **se usa** `LONG`, **se devuelve** un espacio seguido de una palabra.

Para el examen, **hay que** asegurarse de entender la diferencia entre los formatos `SHORT` y `LONG` y los símbolos comunes como `M` para millón.

> **Nota:** Si bien está fuera del alcance del examen, algunas instancias de `CompactNumberFormat` mostrarán más de tres dígitos si el valor es mayor que el rango soportado. Por ejemplo, usar `Long.MAX_VALUE` mostrará siete dígitos (`9223372T`) en el ejemplo anterior, ya que un trillón (`10^12`) es el rango más alto que la instancia usará.

`CompactNumberFormat` también **puede usarse** para parsear, aunque no siempre de la manera **esperada**. **Consideremos** este ejemplo:

```Java
20: var locale = Locale.of("en", "US");
21: var compact = NumberFormat.getCompactNumberInstance(
22:     locale, Style.SHORT);
23: System.out.println(compact.format(1_000_000));  // 1M
24: System.out.println(compact.parse("1M"));        // 1000000
25: System.out.println(compact.parse("1000000"));   // 1000000
26: System.out.println(compact.parse("1,000,000")); // 1
27: System.out.println(compact.parse("$1000000"));  // ParseException
```

Las líneas 20-23 **deberían** parecer familiares. **Imprimen** un millón usando el formato corto de `1M`. Las líneas 24 y 25 **muestran** que el formato es flexible al tomar el formato original o el abreviado. La línea 26 puede **sorprender** ya que Java **se detiene** en la puntuación inicial y solo **imprime** `1`. Por contraste, la línea 27 **va** demasiado lejos. Java no sabe qué hacer con el `$` y **lanza** una `ParseException`.

## Localizando Fechas

Al igual que los números, los formatos de fecha **pueden** variar según el local. La Tabla 11.9 **muestra** los métodos usados para obtener una instancia de un `DateTimeFormatter` usando el local predeterminado.

**TABLA 11.9** Métodos de fábrica para obtener un `DateTimeFormatter`

| **Descripción** | **Usando Locale predeterminado** |
|---|---|
| Para formatear fechas | `DateTimeFormatter.ofLocalizedDate(FormatStyle dateStyle)` |
| Para formatear horas | `DateTimeFormatter.ofLocalizedTime(FormatStyle timeStyle)` |
| Para formatear fechas y horas | `DateTimeFormatter.ofLocalizedDateTime(FormatStyle dateStyle, FormatStyle timeStyle)` / `DateTimeFormatter.ofLocalizedDateTime(FormatStyle dateTimeStyle)` |

Cada método en la tabla **toma** un parámetro `FormatStyle` (o dos) con valores posibles `SHORT`, `MEDIUM`, `LONG` y `FULL`. Para el examen, no **se requiere** conocer el formato de cada uno de estos estilos.

Si **se necesita** un formateador para un local específico, basta con agregar `withLocale(locale)` a la llamada del método.

**Pongamos** todo junto. **Echemos** un vistazo al siguiente fragmento de código:

```Java
public static void print(DateTimeFormatter dtf,
        LocalDateTime dateTime, Locale locale) {
    System.out.println(dtf.format(dateTime) + " --- "
        + dtf.withLocale(locale).format(dateTime));
}

public static void main(String[] args) {
    Locale.setDefault(Locale.of("en", "US"));
    var italy = Locale.of("it", "IT");
    var dt = LocalDateTime.of(2025, Month.OCTOBER, 20, 15, 12, 34);

    // 10/20/25 --- 20/10/25
    print(DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT), dt, italy);

    // 3:12 PM --- 15:12
    print(DateTimeFormatter.ofLocalizedTime(FormatStyle.SHORT), dt, italy);

    // 10/20/25, 3:12 PM --- 20/10/25, 15:12
    print(DateTimeFormatter.ofLocalizedDateTime(
        FormatStyle.SHORT, FormatStyle.SHORT), dt, italy);
}
```

Primero **se establece** `en_US` como el local predeterminado, con `it_IT` como el local solicitado. Luego **se produce** cada valor usando los dos locales. Como **se puede** ver, **aplicar** un local tiene un gran impacto en los formateadores de fecha y hora integrados.

## Especificando una Categoría de Locale

Cuando **se llama** a `Locale.setDefault()` con un local, varias opciones de visualización y formateo **se seleccionan** internamente. Si **se requiere** un control más fino del local predeterminado, Java **subdivide** las opciones de formateo subyacentes en categorías distintas con el enum `Locale.Category`.

El enum `Locale.Category` es un elemento anidado en `Locale` que soporta locales distintos para **mostrar** y formatear datos. Para el examen, **hay que** estar familiarizado con los dos valores enum de la Tabla 11.10.

**TABLA 11.10** Valores de `Locale.Category`

| **Valor** | **Descripción** |
|---|---|
| `DISPLAY` | Categoría usada para mostrar datos sobre el local |
| `FORMAT` | Categoría usada para formatear fechas, números o monedas |

Cuando **se llama** a `Locale.setDefault()` con un local, `DISPLAY` y `FORMAT` **se establecen** juntos. **Veamos** un ejemplo:

```Java
public static void printCurrency(Locale locale, double money) {
    System.out.println(
        NumberFormat.getCurrencyInstance().format(money)
        + ", " + locale.getDisplayLanguage());
}

public static void main(String[] args) {
    var spain = Locale.of("es", "ES");
    var money = 1.23;

    // Imprimir con local predeterminado
    Locale.setDefault(Locale.of("en", "US"));
    printCurrency(spain, money); // $1.23, Spanish

    // Imprimir con visualización del local seleccionado
    Locale.setDefault(Category.DISPLAY, spain);
    printCurrency(spain, money); // $1.23, español

    // Imprimir con formato del local seleccionado
    Locale.setDefault(Category.FORMAT, spain);
    printCurrency(spain, money); // 1,23 €, español
}
```

El código **imprime** los mismos datos tres veces. Primero **imprime** la variable de dinero y el valor de idioma de `spain` usando el local `en_US`. Luego **lo imprime** usando la categoría `DISPLAY` de `es_ES`, mientras que la categoría `FORMAT` permanece `en_US`. Finalmente, **imprime** los datos usando ambas categorías **establecidas** a `es_ES`.

Para el examen, no **hay que** memorizar las diversas opciones de visualización y formateo para cada categoría. Solo **hay que** saber que **se pueden** establecer partes del local de forma independiente. También **hay que** saber que llamar a `Locale.setDefault(Locale.US)` después del fragmento de código anterior **cambiará** ambas categorías de local a `en_US`.

---

**Ver también:** [[Cargando Propiedades con Paquetes de Recursos]] | [[Formateando Valores]] | [[Manejando Excepciones]]
