Ahora **se cambia** de enfoque para hablar sobre cómo formatear datos para los usuarios. En esta sección **se trabaja** con números, fechas y horas. Esto es especialmente importante en la siguiente sección cuando **se expande** la personalización a diferentes idiomas y locales. Puede ser útil revisar el Capítulo 4, "APIs Core", si se necesita un repaso sobre cómo crear objetos de fecha/hora.

## Formateando Números

En el Capítulo 4, **se vio** cómo controlar la salida de un número usando el método `String.format()`. Eso es útil para cosas simples, pero a veces se necesita un control más fino. Para eso, **se introduce** la clase abstracta `NumberFormat`, que tiene dos métodos de uso común:

```Java
public final String format(double number)
public final String format(long number)
```

Como `NumberFormat` es una clase abstracta, **se necesita** la clase concreta `DecimalFormat` para usarla. Incluye un constructor que recibe un `String` de patrón:

```Java
public DecimalFormat(String pattern)
```

Los patrones pueden volverse bastante complejos. Pero por suerte, para el examen solo **hay que** conocer dos caracteres de formato, **mostrados** en la Tabla 11.5.

**TABLA 11.5** Símbolos de `DecimalFormat`

| **Símbolo** | **Significado** | **Ejemplos** |
|---|---|---|
| `#` | Omitir la posición si no hay dígito para ella. | `$2.2` |
| `0` | Poner `0` en la posición si no hay dígito para ella. | `$002.20` |

Estos ejemplos ayudan a ilustrar cómo funcionan estos símbolos:

```Java
12: double d = 1234.567;
13: NumberFormat f1 = new DecimalFormat("###,###,###.0");
14: System.out.println(f1.format(d)); // 1,234.6
15:
16: NumberFormat f2 = new DecimalFormat("000,000,000.00000");
17: System.out.println(f2.format(d)); // 000,001,234.56700
18:
19: NumberFormat f3 = new DecimalFormat("Your Balance $#,###,###.##");
20: System.out.println(f3.format(d)); // Your Balance $1,234.57
```

La línea 14 **muestra** los dígitos del número, redondeando al décimo más cercano después del punto decimal. Las posiciones adicionales a la izquierda **se omiten** porque **se usó** `#`. La línea 17 **agrega** ceros iniciales y finales para que la salida tenga la longitud deseada. La línea 20 **muestra** cómo prefijar un carácter no formateador junto con el redondeo, ya que **se imprimen** menos dígitos de los disponibles. **Hay que notar** que las comas **se eliminan** automáticamente si **se usan** entre símbolos `#`.

Como **se verá** en la sección de localización de este capítulo, hay una segunda clase concreta que hereda `NumberFormat` llamada `CompactNumberFormat`, que también **hay que** conocer para el examen.

## Formateando Fechas y Horas

Las clases de fecha y hora soportan muchos métodos para obtener datos de ellas.

```Java
LocalDate date = LocalDate.of(2025, Month.OCTOBER, 20);
System.out.println(date.getDayOfWeek()); // MONDAY
System.out.println(date.getMonth());     // OCTOBER
System.out.println(date.getYear());      // 2025
System.out.println(date.getDayOfYear()); // 293
```

Java proporciona una clase llamada `DateTimeFormatter` para mostrar formatos estándar.

```Java
LocalDate date = LocalDate.of(2025, Month.OCTOBER, 20);
LocalTime time = LocalTime.of(11, 12, 34);
LocalDateTime dt = LocalDateTime.of(date, time);

System.out.println(date.format(DateTimeFormatter.ISO_LOCAL_DATE));
System.out.println(time.format(DateTimeFormatter.ISO_LOCAL_TIME));
System.out.println(dt.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME));
```

El fragmento de código **imprime** lo siguiente:

```Plaintext
2025-10-20
11:12:34
2025-10-20T11:12:34
```

El `DateTimeFormatter` **lanzará** una excepción si **encuentra** un tipo incompatible. Por ejemplo, cada uno de los siguientes **producirá** una excepción en tiempo de ejecución ya que **intenta** formatear una fecha con un valor de hora, y viceversa:

```Java
date.format(DateTimeFormatter.ISO_LOCAL_TIME); // RuntimeException
time.format(DateTimeFormatter.ISO_LOCAL_DATE); // RuntimeException
```

## Personalizando el Formato de Fecha/Hora

Si no **se quiere** usar uno de los formatos predefinidos, `DateTimeFormatter` soporta un formato personalizado usando un `String` de formato de fecha.

```Java
var f = DateTimeFormatter.ofPattern("MMMM dd, yyyy 'at' hh:mm");
System.out.println(dt.format(f)); // October 20, 2025 at 11:12
```

**Hay que** desglosar esto un poco. Java **asigna** a cada letra o símbolo una parte específica de fecha/hora. Por ejemplo, `M` **se usa** para el mes, mientras que `y` **se usa** para el año. ¡Y el caso importa! Usar `m` en lugar de `M` significa que **devolverá** el minuto de la hora, no el mes del año.

¿Qué hay del número de símbolos? El número a menudo **dicta** el formato de la parte de fecha/hora. Usar `M` solo **produce** el número mínimo de caracteres para un mes, como `1` para enero, mientras que `MM` siempre **produce** dos dígitos, como `01`. Además, `MMM` **imprime** la abreviatura de tres letras, como `Jul` para julio, mientras que `MMMM` **imprime** el nombre completo del mes.

> **Nota:** Es posible, aunque poco probable, encontrar preguntas en el examen que usen `SimpleDateFormat` en lugar del más útil `DateTimeFormatter`. Si **se ve** en el examen usado con un objeto `java.util.Date` más antiguo, **hay que saber** que los formatos personalizados que **es probable** que aparezcan en el examen **serán** compatibles con ambos.

#### Aprendiendo los Símbolos Estándar de Fecha/Hora

Para el examen, **hay que** estar suficientemente familiarizado con los distintos símbolos para poder mirar un `String` de fecha/hora y tener una buena idea de cuál será la salida. La Tabla 11.6 incluye los símbolos con los que **hay que** estar familiarizado para el examen.

**TABLA 11.6** Símbolos comunes de fecha/hora

| **Símbolo** | **Significado** | **Ejemplos** |
|---|---|---|
| `y` | Año | `25, 2025` |
| `M` | Mes | `1, 01, Jan, January` |
| `d` | Día | `5, 05` |
| `H` | Hora (24h) | `15` |
| `h` | Hora (12h) | `9, 09` |
| `m` | Minuto | `45` |
| `s` | Segundo | `52` |
| `a` | a.m./p.m. | `AM, PM` |
| `z` | Nombre de zona horaria | `Eastern Standard Time, EST` |
| `Z` | Desplazamiento de zona horaria | `-0400` |

> **Consejo:** Puede ser difícil recordar las diferencias entre letras mayúsculas y minúsculas de la Tabla 11.6. Como consejo, **hay que recordar** que `H` es de 24 horas y `h` es de 12 horas porque la mayúscula es más grande. Del mismo modo, `M` es el mes y `m` es el minuto porque `M` es más grande.

**Hay que** intentar algunos ejemplos. ¿Qué **imprime** lo siguiente?

```Java
var dt = LocalDateTime.of(2025, Month.OCTOBER, 20, 6, 15, 30);

var formatter1 = DateTimeFormatter.ofPattern("MM/dd/yyyy hh:mm:ss");
System.out.println(dt.format(formatter1)); // 10/20/2025 06:15:30

var formatter2 = DateTimeFormatter.ofPattern("MM_yyyy_-_dd");
System.out.println(dt.format(formatter2)); // 10_2025_-_20

var formatter3 = DateTimeFormatter.ofPattern("hh:mm:z");
System.out.println(dt.format(formatter3)); // DateTimeException
```

El primer ejemplo **imprime** la fecha, con el mes antes del día, seguido de la hora. El segundo ejemplo **imprime** la fecha en un formato extraño con caracteres adicionales que simplemente **se muestran** como parte de la salida.

El tercer ejemplo **lanza** una excepción en tiempo de ejecución porque el `LocalDateTime` subyacente no tiene una zona horaria especificada. Si **se usara** `ZonedDateTime`, el código **se completaría** con éxito e **imprimiría** algo como `06:15 EDT`, dependiendo de la zona horaria.

Como **se vio** en el ejemplo anterior, **hay que** asegurarse de que el `String` de formato sea compatible con el tipo de fecha/hora subyacente. La Tabla 11.7 **muestra** qué símbolos **se pueden** usar con cada objeto de fecha/hora.

**TABLA 11.7** Símbolos de fecha/hora soportados

| **Símbolo** | **LocalDate** | **LocalTime** | **LocalDateTime** | **ZonedDateTime** |
|---|---|---|---|---|
| `y` | ✓ | | ✓ | ✓ |
| `M` | ✓ | | ✓ | ✓ |
| `d` | ✓ | | ✓ | ✓ |
| `h` | | ✓ | ✓ | ✓ |
| `m` | | ✓ | ✓ | ✓ |
| `s` | | ✓ | ✓ | ✓ |
| `a` | | ✓ | ✓ | ✓ |
| `z` | | | | ✓ |
| `Z` | | | | ✓ |

**Hay que** asegurarse de saber qué símbolos son compatibles con qué tipos de fecha/hora. Por ejemplo, intentar formatear un mes para un `LocalTime` o una hora para un `LocalDate` resultará en una excepción en tiempo de ejecución.

#### Seleccionando un Método *format()*

Las clases de fecha/hora contienen un método `format()` que **tomará** un formateador, mientras que las clases de formateador contienen un método `format()` que **tomará** un valor de fecha/hora. El resultado es que cualquiera de las siguientes opciones es aceptable:

```Java
var dateTime = LocalDateTime.of(2025, Month.OCTOBER, 20, 6, 15, 30);
var formatter = DateTimeFormatter.ofPattern("MM/dd/yyyy hh:mm:ss");

System.out.println(dateTime.format(formatter)); // 10/20/2025 06:15:30
System.out.println(formatter.format(dateTime)); // 10/20/2025 06:15:30
```

Estas sentencias **imprimen** el mismo valor en tiempo de ejecución. La sintaxis que **se use** depende del programador.

#### Agregando Valores de Texto Personalizados

¿Qué pasa si **se quiere** que el formato incluya algunos valores de texto personalizados? Si simplemente **se escriben** como parte del `String` de formato, el formateador **interpretará** cada carácter como un símbolo de fecha/hora. En el mejor caso, **mostrará** datos extraños basados en los símbolos adicionales que **se ingresen**. En el peor caso, **lanzará** una excepción porque los caracteres contienen símbolos no válidos. ¡Ninguno es deseable!

Una forma de abordar esto sería dividir el formateador en múltiples formateadores más pequeños y luego concatenar los resultados.

```Java
var dt = LocalDateTime.of(2025, Month.OCTOBER, 20, 6, 15, 30);

var f1 = DateTimeFormatter.ofPattern("MMMM dd, yyyy ");
var f2 = DateTimeFormatter.ofPattern(" hh:mm");
System.out.println(dt.format(f1) + "at" + dt.format(f2));
```

Esto **imprime** `October 20, 2025 at 06:15` en tiempo de ejecución.

Aunque esto funciona, podría volverse difícil si hay muchos valores de texto y símbolos de fecha intermezclados. Afortunadamente, Java incluye una solución mucho más simple. **Se puede** _escapar_ el texto rodeándolo con un par de comillas simples (`'`). Escapar el texto le **indica** al formateador que ignore los valores dentro de las comillas simples y simplemente los **inserte** como parte del valor final.

```Java
var f = DateTimeFormatter.ofPattern("MMMM dd, yyyy 'at' hh:mm");
System.out.println(dt.format(f)); // October 20, 2025 at 06:15
```

Pero ¿qué pasa si también **se necesita** mostrar una comilla simple en la salida? ¡Bienvenido a la diversión de escapar caracteres! Java lo soporta poniendo dos comillas simples una al lado de la otra.

**Se concluye** la discusión sobre el formato de fechas con algunos ejemplos de formatos y su salida que dependen de valores de texto:

```Java
var g1 = DateTimeFormatter.ofPattern("MMMM dd', Party''s at' hh:mm");
System.out.println(dt.format(g1)); // October 20, Party's at 06:15

var g2 = DateTimeFormatter.ofPattern("'System format, hh:mm:' hh:mm");
System.out.println(dt.format(g2)); // System format, hh:mm: 06:15

var g3 = DateTimeFormatter.ofPattern("'NEW! 'yyyy', yay!'");
System.out.println(dt.format(g3)); // NEW! 2025, yay!
```

Si no **se escapan** los valores de texto con comillas simples, **se lanzará** una excepción en tiempo de ejecución si el texto no **puede** interpretarse como un símbolo de fecha/hora.

```Java
DateTimeFormatter.ofPattern("The time is hh:mm"); // IllegalArgumentException
```

Esta línea **lanza** una excepción ya que `T` es un símbolo desconocido. El examen también puede **presentar** una secuencia de escape incompleta.

```Java
DateTimeFormatter.ofPattern("'Time is: hh:mm: "); // IllegalArgumentException
```

No terminar una secuencia de escape **disparará** una excepción en tiempo de ejecución.

---

**Ver también:** [[Internacionalización y Localización]] | [[Manejando Excepciones]]
