Java proporciona una serie de API para trabajar con fechas y horas. También existe una antigua clase `java.util.Date`, pero no está en el examen. Es necesaria una sentencia `import` para trabajar con las clases modernas de fecha y hora. Para usarlas, se debe añadir esta importación al programa:

```Java
import java.time.*; // importa las clases de tiempo
```

### Día (_Day_) vs. Fecha (_Date_)

En inglés americano, la palabra _date_ se usa para representar dos conceptos diferentes. A veces, es la combinación de mes/día/año en que algo sucedió, como el 1 de enero de 2025. A veces, es el día del mes, como en "La fecha (_date_) de hoy es el 6".

Es correcto: las palabras _day_ y _date_ a menudo se usan como sinónimos. Se debe estar alerta a esto en el examen, especialmente si se vive en un lugar donde la gente es más precisa sobre esta distinción.

En las siguientes secciones, se observa cómo crear y manipular fechas y horas, incluyendo zonas horarias y el horario de verano (_daylight saving time_).

### Creación de fechas y horas

En el mundo real, usualmente se habla de fechas y zonas horarias como si la otra persona estuviera ubicada cerca. Por ejemplo, si se dice: "Te llamaré a las 11 el martes por la mañana", se asume que las 11 significa lo mismo para ambas personas. Pero si uno vive en Nueva York y el otro en California, es necesario ser más específicos. California es tres horas más temprano que Nueva York porque los estados están en diferentes zonas horarias. En su lugar, se diría: "Te llamaré a las 11 EST (Hora Estándar del Este) el martes por la mañana".

Al trabajar con fechas y horas, lo primero que hay que hacer es decidir cuánta información se necesita. El examen ofrece cuatro opciones.

- `LocalDate`: Contiene **solo una fecha**, sin hora y sin zona horaria. Un buen ejemplo de `LocalDate` es el cumpleaños de este año. Es el cumpleaños durante todo un día, independientemente de la hora que sea.
- `LocalTime`: Contiene **solo una hora**, sin fecha y sin zona horaria. Un buen ejemplo de `LocalTime` es la medianoche. Es medianoche a la misma hora todos los días.
- `LocalDateTime`: Contiene **tanto una fecha como una hora**, pero sin zona horaria. Un buen ejemplo de `LocalDateTime` es "la campanada de medianoche en la víspera de Año Nuevo".
- `ZonedDateTime`: Contiene **una fecha, hora y zona horaria**. Un buen ejemplo de `ZonedDateTime` es "una llamada de conferencia a las 9 a.m. EST". Si se vive en California, ¡habrá que levantarse muy temprano ya que la llamada es a las 6 a.m. hora local!

Las instancias de fecha y hora se obtienen utilizando un método estático.

```Java
System.out.println(LocalDate.now());
System.out.println(LocalTime.now());
System.out.println(LocalDateTime.now());
System.out.println(ZonedDateTime.now());
```

Cada una de las cuatro clases tiene un método estático llamado `now()`, que da la fecha y hora actuales. La salida dependerá de la fecha/hora en que se ejecute y de dónde se viva. Si se ejecuta en los Estados Unidos el 25 de julio a las 9:13 a.m., la salida se vería de la siguiente manera:

```Plaintext
2025-07-25
09:13:07.768
2025-07-25T09:13:07.768
2025-07-25T09:13:07.769-04:00[America/New_York]
```

La clave es el tipo de información en la salida. La primera línea contiene solo una fecha y no una hora. La segunda contiene solo una hora y no una fecha. La hora muestra horas, minutos, segundos y fracciones de segundo. La tercera contiene tanto una fecha como una hora. La salida usa `T` para separar la fecha y la hora al convertir `LocalDateTime` a un `String`. Finalmente, la cuarta añade el desplazamiento (_offset_) de la zona horaria y la zona horaria. Nueva York está a cuatro zonas horarias del Meridiano de Greenwich (GMT).

El Tiempo Medio de Greenwich (_Greenwich Mean Time_ - GMT) es una zona horaria en Europa que se usa como la zona horaria cero cuando se discuten los desplazamientos. También es posible que se haya oído hablar del Tiempo Universal Coordinado (_Coordinated Universal Time_), que es un estándar de zona horaria. Se abrevia como UTC, como un compromiso entre los nombres en inglés y francés. (No es un error tipográfico. ¡UTC no es en realidad el acrónimo adecuado en ninguno de los dos idiomas!) UTC utiliza la misma zona horaria cero que GMT.

Primero, se intentará averiguar cuán alejados en el tiempo están los siguientes momentos. Note cómo India tiene un desplazamiento de media hora, no una hora completa. Para abordar un problema como este, **se resta** la zona horaria de la hora. Esto da el equivalente GMT de la hora:

```Plaintext
2025-06-20T06:50+05:30[Asia/Kolkata] // GMT 2025-06-20 01:20
2025-06-20T07:50-04:00[US/Eastern]   // GMT 2025-06-20 11:50
```

Recuerde que es necesario sumar al restar un número negativo. Después de convertir a GMT, se puede ver que la hora del Este de EE. UU. ocurre 10 horas y media después de la hora de Calcuta.

El desplazamiento de la zona horaria se puede listar de diferentes maneras: `+02:00`, `GMT+2` y `UTC+2` significan lo mismo. Se podría ver cualquiera de ellos en el examen.

Si hay problemas para recordar esto, se puede intentar memorizar un ejemplo donde las zonas horarias estén a unas pocas zonas de distancia, y recordar la dirección. En los Estados Unidos, la mayoría de la gente sabe que la costa este está tres horas por delante de la costa oeste. Y la mayoría de la gente sabe que Asia está por delante de Europa. Simplemente no se debe cruzar la zona horaria cero en el ejemplo que se elija recordar. El cálculo funciona de la misma manera, pero no es tan buena ayuda para la memoria.

#### Espera, no vivo en los Estados Unidos

El examen reconoce que los examinados viven en todo el mundo y no preguntará sobre los detalles de los formatos de fecha y hora de EE. UU. Dicho esto, los ejemplos utilizan formatos de fecha y hora de EE. UU., al igual que las preguntas del examen. Solo hay que recordar que **el mes va antes de la fecha**. Además, Java tiende a utilizar un reloj de 24 horas, aunque en Estados Unidos se utiliza un reloj de 12 horas con a.m./p.m.

Ahora que se sabe cómo crear la fecha y la hora actuales, se observarán otras fechas y horas específicas. Para empezar, se creará solo una fecha sin hora. Ambos de estos ejemplos crean la misma fecha:

```Java
var date1 = LocalDate.of(2025, Month.JANUARY, 20);
var date2 = LocalDate.of(2025, 1, 20);
```

Ambos pasan el año, el mes y el día. Aunque es bueno usar las constantes de `Month` (para facilitar la lectura del código), se puede pasar el número `int` del mes directamente. Simplemente se usa el número del mes de la misma manera que se haría si se estuviera escribiendo la fecha en la vida real.

Las firmas de los métodos son las siguientes:

```Java
public static LocalDate of(int year, int month, int dayOfMonth)
public static LocalDate of(int year, Month month, int dayOfMonth)
```

Hasta ahora, se ha estado diciendo continuamente que Java cuenta comenzando en 0. Bueno, **los meses son una excepción**. Para los meses en los nuevos métodos de fecha y hora, Java cuenta **comenzando desde 1**, tal como lo hacen los humanos.

Al crear una hora, se puede elegir cuán detallado se quiere ser. Se puede especificar solo la hora y el minuto, o se puede incluir el número de segundos. Incluso se pueden incluir nanosegundos si se desea ser muy preciso. (Un nanosegundo es una milmillonésima de segundo, aunque probablemente no se necesite ser tan específico).

```Java
var time1 = LocalTime.of(6, 15);         // hora y minuto
var time2 = LocalTime.of(6, 15, 30);     // + segundos
var time3 = LocalTime.of(6, 15, 30, 200);// + nanosegundos
```

Estas tres horas son todas diferentes pero están a menos de un minuto entre sí. Las firmas de los métodos son las siguientes:

```Java
public static LocalTime of(int hour, int minute)
public static LocalTime of(int hour, int minute, int second)
public static LocalTime of(int hour, int minute, int second, int nanos)
```

Se pueden combinar fechas y horas en un solo objeto.

```Java
var dateTime1 = LocalDateTime.of(2025, Month.JANUARY, 20, 6, 15, 30);
var dateTime2 = LocalDateTime.of(date1, time2);
```

La primera línea de código muestra cómo se puede especificar toda la información sobre el `LocalDateTime` en la misma línea. La segunda línea de código muestra cómo se pueden crear los objetos `LocalDate` y `LocalTime` por separado primero y luego combinarlos para crear un objeto `LocalDateTime`.

Hay muchas firmas de métodos ya que hay más combinaciones. Las siguientes firmas de métodos utilizan valores enteros:

```Java
public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute)
public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second)
public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanos)
```

Otras toman una referencia `Month`:

```Java
public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute)
public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second)
public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second, int nanos)
```

Finalmente, una toma un `LocalDate` y un `LocalTime` existentes:

```Java
public static LocalDateTime of(LocalDate date, LocalTime time)
```

Para crear un `ZonedDateTime`, primero se necesita obtener la zona horaria deseada. Se utilizará `US/Eastern` en los ejemplos:

```Java
var zone = ZoneId.of("US/Eastern");
var zoned1 = ZonedDateTime.of(2025, 1, 20, 6, 15, 30, 200, zone);
var zoned2 = ZonedDateTime.of(date1, time1, zone);
var zoned3 = ZonedDateTime.of(dateTime1, zone);
```

Se comienza obteniendo el objeto de zona horaria. Luego se utiliza uno de los tres enfoques para crear el `ZonedDateTime`. El primero pasa todos los campos individualmente. No se recomienda este enfoque: hay demasiados números y es difícil de leer. Un mejor enfoque es pasar un objeto `LocalDate` y un objeto `LocalTime`, o un objeto `LocalDateTime`.

Aunque hay otras formas de crear un `ZonedDateTime`, solo es necesario conocer tres para el examen:

```Java
public static ZonedDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanos, ZoneId zone)
public static ZonedDateTime of(LocalDate date, LocalTime time, ZoneId zone)
public static ZonedDateTime of(LocalDateTime dateTime, ZoneId zone)
```

Note que no hay una opción para pasar el enum `Month`. Además, **no se utilizó un constructor** en ninguno de los ejemplos. Las clases de fecha y hora tienen constructores privados junto con métodos estáticos que devuelven instancias. Esto se conoce como el **patrón de fábrica** (_factory pattern_). Los creadores del examen pueden intentar algo como esto:

```Java
var d = new LocalDate(); // NO COMPILA
```

No hay que dejarse engañar. No está permitido construir un objeto de fecha u hora directamente.

Otro truco es qué sucede cuando se pasan números inválidos a `of()`, por ejemplo:

```Java
var d = LocalDate.of(2025, Month.JANUARY, 32); // DateTimeException
```

No es necesario conocer la excepción exacta que se lanza, pero es una clara:

```Plaintext
java.time.DateTimeException: Invalid value for DayOfMonth (valid values 1-28/31): 32
```

### Manipulación de fechas y horas

Añadir datos a una fecha es fácil. Las clases de fecha y hora son **inmutables**. Se debe recordar **asignar los resultados** de estos métodos a una variable de referencia para que no se pierdan.

```Java
12: var date = LocalDate.of(2025, Month.JANUARY, 20);
13: System.out.println(date); // 2025-01-20
14: date = date.plusDays(2);
15: System.out.println(date); // 2025-01-22
16: date = date.plusWeeks(1);
17: System.out.println(date); // 2025-01-29
18: date = date.plusMonths(1);
19: System.out.println(date); // 2025-02-28
20: date = date.plusYears(5);
21: System.out.println(date); // 2030-02-28
```

Este código es agradable porque hace justo lo que parece. Se comienza con el 20 de enero de 2025. En la línea 14, se le añaden dos días y se reasigna a la variable de referencia. En la línea 16, se añade una semana. Este método permite escribir un código más claro que `plusDays(7)`. Ahora la fecha es el 29 de enero de 2025. En la línea 18, se añade un mes. Esto llevaría al 29 de febrero de 2025. Sin embargo, 2025 no es un año bisiesto (2020 y 2024 son años bisiestos). Java es lo suficientemente inteligente como para darse cuenta de que el 29 de febrero de 2025 no existe, y da el 28 de febrero de 2025 en su lugar. Finalmente, la línea 20 añade cinco años.

El 29 de febrero existe solo en un año bisiesto. Los años bisiestos son aquellos que son múltiplos de 4 o 400, pero no otros múltiplos de 100. Por ejemplo, 2000 y 2028 son años bisiestos, pero 2100 no lo es.

También hay métodos agradables y fáciles para retroceder en el tiempo. Esta vez, se trabajará con `LocalDateTime`:

```Java
22: var date = LocalDate.of(2025, Month.JANUARY, 20);
23: var time = LocalTime.of(5, 15);
24: var dateTime = LocalDateTime.of(date, time);
25: System.out.println(dateTime); // 2025-01-20T05:15
26: dateTime = dateTime.minusDays(1);
27: System.out.println(dateTime); // 2025-01-19T05:15
28: dateTime = dateTime.minusHours(10);
29: System.out.println(dateTime); // 2025-01-18T19:15
30: dateTime = dateTime.minusSeconds(30);
31: System.out.println(dateTime); // 2025-01-18T19:14:30
```

La línea 25 imprime la fecha original del 20 de enero de 2025 a las 5:15 a.m. La línea 26 resta un día completo, llevando al 19 de enero de 2025 a las 5:15 a.m. La línea 28 resta 10 horas, mostrando que la fecha cambiará si las horas hacen que se ajuste, y lleva al 18 de enero de 2025 a las 19:15 (7:15 p.m.). Finalmente, la línea 30 resta 30 segundos. Se puede ver que, de repente, el valor visualizado comienza a mostrar los segundos. Java es lo suficientemente inteligente como para ocultar los segundos y nanosegundos cuando no se están usando.

Es común que los métodos de fecha y hora se encadenen. Por ejemplo, sin las sentencias de impresión, el ejemplo anterior podría reescribirse de la siguiente manera:

```Java
var date = LocalDate.of(2025, Month.JANUARY, 20);
var time = LocalTime.of(5, 15);
var dateTime = LocalDateTime.of(date, time)
    .minusDays(1).minusHours(10).minusSeconds(30);
```

Cuando se tienen muchas manipulaciones que hacer, este encadenamiento resulta útil. Hay dos formas en que los creadores del examen pueden intentar engañar. ¿Qué se cree que imprime esto?

```Java
var date = LocalDate.of(2025, Month.JANUARY, 20);
date.plusDays(10);
System.out.println(date);
```

Imprime `2025-01-20`. Añadir 10 días fue inútil porque el programa **ignoró el resultado**. Siempre que se vean tipos inmutables, se debe prestar atención para asegurarse de que el valor de retorno de una llamada de método no se ignore. El examen también puede poner a prueba si se recuerda qué incluye cada uno de los objetos de fecha y hora. ¿Se ve qué está mal aquí?

```Java
var date = LocalDate.of(2025, Month.JANUARY, 20);
date = date.plusMinutes(1); // NO COMPILA
```

`LocalDate` no contiene la hora. Esto significa que **no se pueden añadir minutos a él**. Esto puede ser complicado en una secuencia encadenada de operaciones de suma/resta, así que hay que asegurarse de saber qué métodos de la Tabla 4.6 se pueden llamar en qué tipos.

**TABLA 4.6** Métodos en `LocalDate`, `LocalTime`, `LocalDateTime` y `ZonedDateTime`

|**Método**|**¿Se puede llamar en LocalDate?**|**¿Se puede llamar en LocalTime?**|**¿Se puede llamar en LocalDateTime o ZonedDateTime?**|
|---|---|---|---|
|`plusYears()`, `minusYears()`, `withYear()`, `withDayOfYear()`|Sí|No|Sí|
|`plusMonths()`, `minusMonths()`, `withMonth()`|Sí|No|Sí|
|`plusWeeks()`, `minusWeeks()`|Sí|No|Sí|
|`plusDays()`, `minusDays()`, `withDayOfMonth()`|Sí|No|Sí|
|`plusHours()`, `minusHours()`, `withHour()`|No|Sí|Sí|
|`plusMinutes()`, `minusMinutes()`, `withMinute()`|No|Sí|Sí|
|`plusSeconds()`, `minusSeconds()`, `withSecond()`|No|Sí|Sí|
|`plusNanos()`, `minusNanos()`, `withNano()`|No|Sí|Sí|

La Tabla 4.6 también incluye métodos que se pueden usar para crear una copia de un objeto con campos específicos alterados al valor especificado (`with...()`). Por ejemplo:

```Java
var date = LocalDate.of(2025, Month.FEBRUARY, 20); // 2025-02-20
var differentDay = date.withDayOfMonth(15);        // 2025-02-15
var differentMonth = date.withDayOfYear(3);        // 2025-01-03
var allChanged = date.withYear(2026)
                     .withMonth(4)
                     .withDayOfMonth(10);          // 2026-04-10
```

Finalmente, hay métodos para convertir de un tipo a otro (`at...()`). Por ejemplo:

```Java
var date = LocalDate.of(2025, Month.MARCH, 3);
var withTime = date.atTime(5, 30);     // 2025-03-03T05:30
var start = date.atStartOfDay();       // 2025-03-03T00:00
```

Los métodos `at...()` combinan la variable de instancia y el parámetro en un nuevo objeto. Se enumeran en la Tabla 4.7.

**TABLA 4.7** Métodos de conversión en `LocalDate`, `LocalTime`, `LocalDateTime`

|**LocalDate a LocalDateTime**|**LocalTime a LocalDateTime**|**LocalDateTime a ZonedDateTime**|
|---|---|---|
|`atStartOfDay()`|`atDate(LocalDate date)`|`atZone(ZoneId zoneId)`|
|`atTime(int hour, int minute)`|||
|`atTime(int hour, int minute, int second)`|||
|`atTime(int hour, int minute, int second, int nanos)`|||
|`atTime(LocalTime time)`|
### Trabajo con períodos (_Periods_)

Ahora se sabe lo suficiente para hacer algo divertido con las fechas. El zoológico realiza actividades de enriquecimiento animal para darles algo agradable que hacer. El cuidador principal ha decidido cambiar los juguetes cada mes. Este sistema continuará durante tres meses para ver cómo funciona.

```Java
public static void main(String[] args) {
    var start = LocalDate.of(2025, Month.JANUARY, 1);
    var end = LocalDate.of(2025, Month.MARCH, 30);
    performAnimalEnrichment(start, end);
}
private static void performAnimalEnrichment(LocalDate start, LocalDate end) {
    var upTo = start;
    while (upTo.isBefore(end)) { // comprueba si todavía es antes del final
        System.out.println("give new toy: " + upTo);
        upTo = upTo.plusMonths(1); // añade un mes
    } 
}
```

Este código funciona bien. Añade un mes a la fecha hasta que llega a la fecha final. El problema es que este método no se puede reutilizar. El cuidador del zoológico quiere probar diferentes horarios para ver cuál funciona mejor.

Afortunadamente, Java tiene una clase `Period` que se puede pasar. Este código hace lo mismo que el ejemplo anterior:

```Java
public static void main(String[] args) {
    var start = LocalDate.of(2025, Month.JANUARY, 1);
    var end = LocalDate.of(2025, Month.MARCH, 30);
    var period = Period.ofMonths(1); // crea un período
    performAnimalEnrichment(start, end, period);
}
private static void performAnimalEnrichment(LocalDate start, LocalDate end, Period period) { // utiliza el período genérico
    var upTo = start;
    while (upTo.isBefore(end)) {
        System.out.println("give new toy: " + upTo);
        upTo = upTo.plus(period); // añade el período
    } 
}
```

El método puede añadir un período de tiempo arbitrario que se pase. Esto permite reutilizar el mismo método para diferentes períodos de tiempo a medida que el cuidador del zoológico cambia de opinión.

Un `Period` puede ser positivo (hacia adelante en el tiempo) o negativo (hacia atrás en el tiempo). Hay cinco formas de crear una clase `Period`:

```Java
var annually = Period.ofYears(1);          // cada 1 año
var quarterly = Period.ofMonths(3);        // cada 3 meses
var everyThreeWeeks = Period.ofWeeks(-3);  // yendo hacia atrás
var everyOtherDay = Period.ofDays(2);      // cada 2 días
var everyYearAndAWeek = Period.of(1, 0, 7);// cada año más 1 semana
```

Hay un detalle importante. **No se pueden encadenar métodos** al crear un `Period`. El siguiente código parece equivalente al ejemplo `everyYearAndAWeek`, pero no lo es. Solo se utiliza el último método porque los métodos son métodos estáticos.

```Java
var wrong = Period.ofYears(1).ofWeeks(1); // cada semana
```

Este código engañoso es realmente como escribir lo siguiente:

```Java
var wrong = Period.ofYears(1);
wrong = Period.ofWeeks(1);
```

¡Esto claramente no es lo que se pretendía! Es por eso que el método `of()` permite pasar el número de años, meses y días. Todos están incluidos en el mismo período. Se obtendrá una advertencia del compilador sobre esto. Las advertencias del compilador indican que algo está mal o es sospechoso sin fallar la compilación.

El método `of()` toma **solo años, meses y días**. La capacidad de usar otro método de fábrica para pasar semanas es simplemente una conveniencia. Como se podría imaginar, el período real se almacena en términos de años, meses y días. Cuando se imprime el valor, Java muestra cualquier parte que no sea cero utilizando el formato que se muestra en la siguiente imagen.

![[Formato de fecha.png]]

Como se puede ver, la `P` siempre comienza el `String` para mostrar que es una medida de período. Luego vienen el número de años, el número de meses y el número de días. Si alguno de estos es cero, **se omiten**.

¿Se puede deducir qué emite esto?

```Java
System.out.println(Period.ofMonths(3));
```

La salida es `P3M`. Recuerde que Java omite cualquier medida que sea cero.

También se puede crear un período obteniendo la cantidad de tiempo entre dos objetos `LocalDate`:

```Java
var xmas = LocalDate.of(2025, Month.DECEMBER, 25);
var newYears = LocalDate.of(2026, Month.JANUARY, 1);
System.out.println(Period.between(xmas, newYears)); // P7D
System.out.println(Period.between(newYears, xmas)); // P-7D
```

Note cómo **el orden importa**. La primera vez `Period.between()` devuelve un período que representa siete días, pero la segunda vez devuelve un período de siete días negativos.

Lo último que se debe saber sobre `Period` es con qué objetos se puede utilizar. Se observará algo de código:

```Java
3: var date = LocalDate.of(2025, 3, 20);
4: var time = LocalTime.of(6, 15);
5: var dateTime = LocalDateTime.of(date, time);
6: var period = Period.ofMonths(-1);
7: System.out.println(date.plus(period));     // 2025-02-20
8: System.out.println(dateTime.plus(period)); // 2025-02-20T06:15
9: System.out.println(time.plus(period));     // Excepción
```

Las líneas 7 y 8 funcionan como se esperaba. Restan un mes del 20 de marzo de 2025, dando el 20 de febrero de 2025. El primero tiene solo la fecha, y el segundo tiene tanto la fecha como la hora.

La línea 9 intenta añadir un mes a un objeto que **solo tiene una hora**. Esto no funcionará. Java lanza una `UnsupportedTemporalTypeException` y se queja de que se intentó usar una unidad no soportada: meses (_Months_).

Como se puede ver, hay que prestar atención al tipo de objetos de fecha y hora en cada lugar donde se vean.

> `LocalDate` y `LocalDateTime` tienen un método para convertirse en valores `long`, equivalentes al número de milisegundos que han pasado desde el 1 de enero de 1970, conocido como la época (_epoch_). ¿Qué tiene de especial esta fecha? Eso es lo que Unix empezó a usar para los estándares de fecha, así que Java lo reutilizó.

### Trabajo con duraciones (_Durations_)

Probablemente ya se haya notado que un `Period` es un día o más de tiempo. También existe `Duration`, que está destinado a unidades de tiempo más pequeñas. Para `Duration`, se puede especificar el número de días, horas, minutos, segundos o nanosegundos. Y sí, se podrían pasar 365 días para hacer un año, pero realmente no se debería: para eso está `Period`.

Convenientemente, `Duration` funciona aproximadamente de la misma manera que `Period`, excepto que **se utiliza con objetos que tienen hora**. `Duration` se emite comenzando con `PT`, que se puede pensar como un período de tiempo (_period of time_). Un `Duration` se almacena en horas, minutos y segundos. El número de segundos incluye fracciones de segundo.

Se puede crear un `Duration` utilizando un número de diferentes granularidades:

```Java
var daily = Duration.ofDays(1);          // PT24H
var hourly = Duration.ofHours(1);        // PT1H
var everyMinute = Duration.ofMinutes(1); // PT1M
var everyTenSeconds = Duration.ofSeconds(10); // PT10S
var everyMilli = Duration.ofMillis(1);   // PT0.001S
var everyNano = Duration.ofNanos(1);     // PT0.000000001S
```

`Duration` no tiene un método de fábrica que tome múltiples unidades como lo hace `Period`. Si se desea que algo suceda cada hora y media, se especifican 90 minutos.

`Duration` incluye otro método de fábrica más genérico. Toma un número y un `TemporalUnit`. La idea es, por ejemplo, algo así como "5 segundos". Sin embargo, `TemporalUnit` es una interfaz. Por el momento, solo hay una implementación llamada `ChronoUnit`.

El ejemplo anterior se podría reescribir así:

```Java
var daily = Duration.of(1, ChronoUnit.DAYS);
var hourly = Duration.of(1, ChronoUnit.HOURS);
var everyMinute = Duration.of(1, ChronoUnit.MINUTES);
var everyTenSeconds = Duration.of(10, ChronoUnit.SECONDS);
var everyMilli = Duration.of(1, ChronoUnit.MILLIS);
var everyNano = Duration.of(1, ChronoUnit.NANOS);
```

`ChronoUnit` también incluye algunas unidades convenientes, como `ChronoUnit.HALF_DAYS` para representar 12 horas.

El uso de un `Duration` funciona de la misma manera que el uso de un `Period`. Por ejemplo:

```Java
7:  var date = LocalDate.of(2025, 1, 20);
8:  var time = LocalTime.of(6, 15);
9:  var dateTime = LocalDateTime.of(date, time);
10: var duration = Duration.ofHours(6);
11: System.out.println(dateTime.plus(duration)); // 2025-01-20T12:15
12: System.out.println(time.plus(duration));     // 12:15
13: System.out.println(date.plus(duration));     // UnsupportedTemporalTypeException
```

La línea 11 muestra que se pueden añadir horas a un `LocalDateTime`, ya que contiene una hora. La línea 12 también funciona, ya que todo lo que se tiene es una hora. La línea 13 falla porque no se pueden añadir horas a un objeto que no contiene una hora.

Se intentará de nuevo, pero esta vez se añadirán 23 horas.

```Java
7:  var date = LocalDate.of(2025, 1, 20);
8:  var time = LocalTime.of(6, 15);
9:  var dateTime = LocalDateTime.of(date, time);
10: var duration = Duration.ofHours(23);
11: System.out.println(dateTime.plus(duration)); // 2025-01-21T05:15
12: System.out.println(time.plus(duration));     // 05:15
13: System.out.println(date.plus(duration));     // UnsupportedTemporalTypeException
```

Esta vez se ve que Java avanza más allá del final del día. La línea 11 pasa al día siguiente ya que se pasa la medianoche. La línea 12 no tiene un día, por lo que la hora simplemente da la vuelta, tal como en un reloj real.

#### `ChronoUnit` para diferencias

`ChronoUnit` es una excelente manera de determinar qué tan separados están dos valores `Temporal`. `Temporal` incluye `LocalDate`, `LocalTime`, etc. `ChronoUnit` está en el paquete `java.time.temporal`.

```Java
var one = LocalTime.of(5, 15);
var two = LocalTime.of(6, 55);
var date = LocalDate.of(2025, 1, 20);

System.out.println(ChronoUnit.HOURS.between(one, two));   // 1
System.out.println(ChronoUnit.MINUTES.between(one, two)); // 100
System.out.println(ChronoUnit.MINUTES.between(one, date));// DateTimeException
```

La primera sentencia de impresión muestra que `between` **trunca** en lugar de redondear. La segunda muestra lo fácil que es contar en diferentes unidades. Simplemente se cambia el tipo `ChronoUnit`. La última recuerda que Java lanzará una excepción si se confunde lo que se puede hacer en objetos de fecha versus hora.

Alternativamente, se puede truncar cualquier objeto con un elemento de tiempo. Por ejemplo:

```Java
LocalTime time = LocalTime.of(3,12,45);
System.out.println(time); // 03:12:45
LocalTime truncated = time.truncatedTo(ChronoUnit.MINUTES);
System.out.println(truncated); // 03:12
```

Este ejemplo pone a cero cualquier campo más pequeño que los minutos. En este caso, se deshace de los segundos.

#### `Period` vs. `Duration`

Recuerde que `Period` y `Duration` no son equivalentes. Este ejemplo muestra un `Period` y un `Duration` de la misma longitud (lógica):

```Java
var date = LocalDate.of(2025, 5, 25);
var period = Period.ofDays(1);
var days = Duration.ofDays(1);

System.out.println(date.plus(period)); // 2025-05-26
System.out.println(date.plus(days));   // Unsupported unit: Seconds
```

Dado que se está trabajando con un `LocalDate`, es **obligatorio usar `Period`**. `Duration` tiene unidades de tiempo incorporadas (incluso si no se ven), y están destinadas solo a objetos que manejan tiempo (horas, minutos, etc.). Hay que asegurarse de poder completar la Tabla 4.8 para identificar qué objetos pueden usar `Period` y `Duration`.

**TABLA 4.8** Dónde usar `Duration` y `Period`

|**Clase**|**¿Se puede usar con Period?**|**¿Se puede usar con Duration?**|
|---|---|---|
|`LocalDate`|Sí|No|
|`LocalDateTime`|Sí|Sí|
|`LocalTime`|No|Sí|
|`ZonedDateTime`|Sí|Sí|

### Trabajo con instantes (_Instants_)

La clase `Instant` representa un momento específico en el tiempo en la zona horaria GMT. Supóngase que se desea ejecutar un temporizador.

```Java
var now = Instant.now();
// Hacer algo que consuma tiempo
var later = Instant.now();
var duration = Duration.between(now, later);
System.out.println(duration.toMillis()); // Devuelve el número de milisegundos
```

En este caso, el "algo que consuma tiempo" duró un poco más de un segundo, y el programa imprimió 1025.

Si se tiene un `ZonedDateTime`, se puede convertir en un `Instant`:

```Java
var date = LocalDate.of(2025, 5, 25);
var time = LocalTime.of(11, 55, 00);
var zone = ZoneId.of("US/Eastern");
var zonedDateTime = ZonedDateTime.of(date, time, zone);
var instant = zonedDateTime.toInstant(); // 2025-05-25T15:55:00Z

System.out.println(zonedDateTime); // 2025-05-25T11:55-04:00[US/Eastern]
System.out.println(instant);       // 2025-05-25T15:55:00Z
```

Las dos últimas líneas representan el mismo momento en el tiempo. El `ZonedDateTime` incluye una zona horaria. El `Instant` se deshace de la zona horaria (local) y lo convierte en un Instante de tiempo en GMT (indicado por la 'Z' al final, que significa "Zulú" o GMT/UTC).

No se puede convertir un `LocalDateTime` a un `Instant`. Recuerde que un `Instant` es un punto exacto en el tiempo. Un `LocalDateTime` **no** contiene una zona horaria, y por lo tanto no es universalmente reconocido en todo el mundo como el mismo momento en el tiempo.

### Contabilización del horario de verano (_Daylight Saving Time_)

Algunos países observan el horario de verano. Aquí es donde los relojes se ajustan una hora dos veces al año para hacer un mejor uso de la luz solar. No todos los países participan, y los que lo hacen utilizan diferentes fines de semana para el cambio. Solo es necesario trabajar con el horario de verano de EE. UU. en el examen, y eso es lo que se describe aquí.

La pregunta del examen informará si una fecha/hora mencionada cae en un fin de semana en que los relojes están programados para ser cambiados. Si no se menciona en una pregunta, se puede asumir que es un fin de semana normal. El acto de mover el reloj hacia adelante o hacia atrás ocurre a las 2:00 a.m., lo que cae muy temprano en la mañana del domingo.

La imagen a continuación muestra qué sucede con los relojes. Cuando se cambian los relojes en marzo, el tiempo salta hacia adelante desde la 1:59 a.m. a las 3:00 a.m. Cuando se cambian los relojes en noviembre, el tiempo retrocede, y se experimenta la hora desde la 1:00 a.m. hasta la 1:59 a.m. dos veces. Los niños aprenden esto como "Saltar hacia adelante en la primavera y caer hacia atrás en el otoño" (_Spring forward in the spring, and fall back in the fall_).

![[Cómo funciona el horario de verano.png]]

Por ejemplo, el 9 de marzo de 2025, se adelantan los relojes una hora y se salta de las 2:00 a.m. a las 3:00 a.m. Esto significa que **no hay 2:30 a.m. ese día**. Si se quisiera saber la hora una hora después de la 1:30, serían las 3:30.

```Java
var date = LocalDate.of(2025, Month.MARCH, 9);
var time = LocalTime.of(1, 30);
var zone = ZoneId.of("US/Eastern");
var dateTime = ZonedDateTime.of(date, time, zone);

System.out.println(dateTime);             // 2025-03-09T01:30-05:00[US/Eastern]
System.out.println(dateTime.getHour());   // 1
System.out.println(dateTime.getOffset()); // -05:00

dateTime = dateTime.plusHours(1);

System.out.println(dateTime);             // 2025-03-09T03:30-04:00[US/Eastern]
System.out.println(dateTime.getHour());   // 3
System.out.println(dateTime.getOffset()); // -04:00
```

Note que dos cosas cambian en este ejemplo. La hora salta de 1:30 a 3:30. El desplazamiento UTC también cambia (de -05:00 a -04:00). ¿Se recuerda cuando se calculó la hora GMT restando la zona horaria de la hora? Se puede ver que se pasó de las 6:30 GMT (1:30 menos -5:00) a las 7:30 GMT (3:30 menos -4:00). Esto muestra que el tiempo realmente cambió una hora desde el punto de vista del GMT. Se imprimieron los campos de hora y desplazamiento por separado para dar énfasis.

De manera similar, en noviembre, una hora después de la 1:30 a.m. inicial es **también** la 1:30 a.m., porque a las 2:00 a.m. se repite la hora. Esta vez, se intentará calcular el tiempo GMT por cuenta propia para las tres horas para confirmar que realmente solo se mueve una hora a la vez.

```Java
var date = LocalDate.of(2025, Month.NOVEMBER, 2);
var time = LocalTime.of(1, 30);
var zone = ZoneId.of("US/Eastern");
var dateTime = ZonedDateTime.of(date, time, zone);

System.out.println(dateTime); // 2025-11-02T01:30-04:00[US/Eastern]

dateTime = dateTime.plusHours(1);
System.out.println(dateTime); // 2025-11-02T01:30-05:00[US/Eastern]

dateTime = dateTime.plusHours(1);
System.out.println(dateTime); // 2025-11-02T02:30-05:00[US/Eastern]
```

¿Se logró comprender? Se pasó de 5:30 GMT a 6:30 GMT y a 7:30 GMT.

Finalmente, intentar crear una hora que no existe simplemente avanza (_rolls forward_):

```Java
var date = LocalDate.of(2025, Month.MARCH, 9);
var time = LocalTime.of(2, 30); // Esta hora no existe debido al salto
var zone = ZoneId.of("US/Eastern");
var dateTime = ZonedDateTime.of(date, time, zone);

System.out.println(dateTime); // 2025-03-09T03:30-04:00[US/Eastern]
```

Java es lo suficientemente inteligente como para saber que no hay 2:30 a.m. esa noche y cambia al desplazamiento GMT apropiado, ajustando la hora a las 3:30 a.m.

Sí, es molesto que Oracle espere que se sepa esto incluso si no se está en los Estados Unidos, o para el caso, en una parte de los Estados Unidos que no sigue el horario de verano. Los creadores del examen están en los Estados Unidos, y decidieron que todos deben saber cómo funcionan las zonas horarias de EE. UU.