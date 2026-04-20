Las respuestas a las preguntas de revisión del capítulo se pueden encontrar al final del capitulo.

**1.** ¿Qué emite el siguiente código?

```Java
1: public class Fish {
2:     public static void main(String[] args) {
3:         int numFish = 4;
4:         String fishType = "tuna";
5:         String anotherFish = numFish + 1;
6:         System.out.println(anotherFish + " " + fishType);
7:         System.out.println(numFish + " " + 1);
8:     } 
   }
```

- [ ]   A. `4 1`
- [ ]   B. `5`
- [ ]   C. `5 tuna`
- [ ]   D. `5tuna`
- [ ]   E. `51tuna`
- [x]   F. El código no compila.

**2.** ¿Cuáles de estas declaraciones de arreglos no son legales? (Elija todas las opciones que correspondan).

- [ ]   A. `int[][] scores = new int[5][];`
- [ ]   B. `Object[][][] cubbies = new Object[3][0][5];`
- [ ]   C. `String beans[] = new beans[6];`
- [ ]   D. `java.util.Date[] dates[] = new java.util.Date[2][];`
- [x]   E. `int[][] types = new int[];`
- [ ]   F. `int[][] java = new int[][];`

**3.** Note que el 12 de marzo de 2028 es el fin de semana en que se adelantan los relojes, y el 5 de noviembre de 2028 es cuando se atrasan para el horario de verano. ¿Cuáles de las siguientes opciones pueden llenar el espacio en blanco sin que el código lance una excepción? (Elija todas las opciones que correspondan).

```Java
var zone = ZoneId.of("US/Eastern");
var date = ______________________;
var time = LocalTime.of(2, 15);
var z = ZonedDateTime.of(date, time, zone);
```

- [x]   A. `LocalDate.of(2028, 3, 12)`
- [ ]   B. `LocalDate.of(2028, 3, 40)`
- [x]   C. `LocalDate.of(2028, 11, 5)`
- [x]   D. `LocalDate.of(2028, 11, 6)`
- [ ]   E. `LocalDate.of(2029, 2, 29)`
- [ ]   F. `LocalDate.of(2028, MonthEnum.MARCH, 12);`

**4.** ¿Cuáles de las siguientes salidas son producidas por este código? (Elija todas las opciones que correspondan).

```Java
3: var s = "Hello";
4: var t = new String(s);
5: if ("Hello".equals(s)) System.out.println("one");
6: if (t == s) System.out.println("two");
7: if (t.intern() == s) System.out.println("three");
8: if ("Hello" == s) System.out.println("four");
9: if ("Hello".intern() == t) System.out.println("five");
```

- [x]   A. `one`
- [ ]   B. `two`
- [x]   C. `three`
- [ ]   D. `four`
- [x]   E. `five`
- [ ]   F. El código no compila.
- [ ]   G. Ninguna de las anteriores.

**5.** ¿Cuál es el resultado del siguiente código?

```Java
7: var sb = new StringBuilder();
8: sb.append("aaa").insert(1, "bb").insert(4, "ccc");
9: System.out.println(sb);
```

- [ ]   A. `abbaaccc`
- [x]   B. `abbaccca`
- [ ]   C. `bbaaaccc`
- [ ]   D. `bbaaccca`
- [ ]   E. Una línea vacía.
- [ ]   F. El código no compila.

**6.** ¿Cuántas de estas líneas contienen un error del compilador?

```Java
23: double one = Math.pow(1, 2);
24: int two = Math.round(1.0);
25: float three = Math.random();
26: var doubles = new double[] {one, two, three};
```

- [x]   A. 0
- [ ]   B. 1
- [ ]   C. 2
- [ ]   D. 3
- [ ]   E. 4

**7.** ¿Cuáles de estas afirmaciones son verdaderas sobre los dos valores? (Elija todas las opciones que correspondan). `2025-08-28T05:00 GMT-04:00` `2025-08-28T09:00 GMT-06:00`

- [x]   A. La primera fecha/hora es anterior.
- [ ]   B. La segunda fecha/hora es anterior.
- [ ]   C. Ambas fechas/horas son las mismas.
- [ ]   D. Las fechas/horas tienen una diferencia de dos horas.
- [x]   E. Las fechas/horas tienen una diferencia de seis horas.
- [ ]   F. Las fechas/horas tienen una diferencia de 10 horas.

**8.** ¿Cuáles de las siguientes opciones devuelven `5` cuando se ejecutan de forma independiente? (Elija todas las opciones que correspondan).

```Java
var string = "12345";
var builder = new StringBuilder("12345");
```

- [x]   A. `builder.charAt(4)`
- [x]   B. `builder.replace(2, 4, "6").charAt(3)`
- [ ]   C. `builder.replace(2, 5, "6").charAt(2)`
- [x]   D. `string.charAt(5)`
- [ ]   E. `string.length`
- [x]   F. `string.replace("123", "1").charAt(2)`  
- [ ]   G. Ninguna de las anteriores.

**9.** ¿Cuáles de las siguientes afirmaciones son verdaderas sobre los arreglos? (Elija todas las opciones que correspondan).

- [x]   A. El primer elemento es el índice 0.
- [ ]   B. El primer elemento es el índice 1.
- [x]   C. Los arreglos tienen tamaño fijo.
- [ ]   D. Los arreglos son inmutables.
- [ ]   E. Llamar a `equals()` en dos arreglos diferentes que contienen los mismos valores primitivos siempre devuelve `true`.
- [x]   F. Llamar a `equals()` en dos arreglos diferentes que contienen los mismos valores primitivos siempre devuelve `false`.
- [ ]   G. Llamar a `equals()` en dos arreglos diferentes que contienen los mismos valores primitivos puede devolver `true` o `false`.

**10.** ¿Cuántas de estas líneas contienen un error del compilador?

```Java
23: int one = Math.min(5, 3);
24: long two = Math.round(5.5);
25: double three = Math.floor(6.6);
26: var doubles = new double[] {one, two, three};
```

- [x]   A. 0
- [ ]   B. 1
- [ ]   C. 2
- [ ]   D. 3
- [ ]   E. 4

**11.** ¿Cuál es la salida del siguiente código?

```Java
var date = LocalDate.of(2025, 4, 3);
date.plusDays(2);
date.plusHours(3);
System.out.println(date.getYear() + " " + date.getMonth() + " " + date.getDayOfMonth());
```

- [ ]   A. `2025 MARCH 4`
- [ ]   B. `2025 MARCH 6`
- [x]   C. `2025 APRIL 3`
- [ ]   D. `2025 APRIL 5`
- [ ]   E. El código no compila.
- [ ]   F. Se lanza una excepción en tiempo de ejecución.

**12.** ¿Qué es emitido por el siguiente código ignorando cualquier salto de línea en la salida? (Elija todas las opciones que correspondan).

```Java
var numbers = "012345678".indent(1);
numbers = numbers.stripLeading();
System.out.println(numbers.substring(1, 3));
System.out.println(numbers.substring(7, 7));
System.out.println(numbers.substring(7));
```

- [x]   A. `12`
- [ ]   B. `123`
- [ ]   C. `7`
- [x]   D. `78`
- [ ]   E. Una línea en blanco.
- [ ]   F. Se lanza una excepción.

**13.** ¿Cuál es el resultado del siguiente código?

```Java
public class Lion {
    public void roar(String roar1, StringBuilder roar2) {
        roar1.concat("!!!");
        roar2.append("!!!");
    }
    public static void main(String[] args) {
        var roar1 = "roar";
        var roar2 = new StringBuilder("roar");
        new Lion().roar(roar1, roar2);
        System.out.println(roar1 + " " + roar2);
    } 
}
```

- [ ]   A. `roar roar`
- [x]   B. `roar roar!!!`
- [ ]   C. `roar!!! Roar`
- [ ]   D. `roar!!! Roar!!!`
- [ ]   E. Se lanza una excepción.
- [ ]   F. El código no compila.

**14.** Dado lo siguiente, ¿cuál puede llenar correctamente el espacio en blanco permitiendo que el código compile? (Elija todas las opciones que correspondan).

```Java
var date = LocalDate.now();
var time = LocalTime.now();
var dateTime = date.______(time);
var zoneId = ZoneId.systemDefault();
var zonedDateTime = ZonedDateTime.of(dateTime, zoneId);
Instant instant = ___________________________;
```

- [ ]   A. `asTime()`
- [x]   B. `atTime()`
- [x]   C. `withTime()`
- [ ]   D. `dateTime.toInstant()`
- [ ]   E. `new Instant()`
- [ ]   F. `zonedDateTime.toInstant()`

**15.** ¿Cuál es la salida de lo siguiente? (Elija todas las opciones que correspondan).

```Java
var arr = new String[] { "PIG", "pig", "123"};
Arrays.sort(arr);
System.out.println(Arrays.toString(arr));
System.out.println(Arrays.binarySearch(arr, "Pippa"));
```

- [ ]   A. `[pig, PIG, 123]`
- [ ]   B. `[PIG, pig, 123]`
- [x]   C. `[123, PIG, pig]`
- [ ]   D. `[123, pig, PIG]`
- [x]   E. `-3`
- [ ]   F. `-2`
- [ ]   G. Los resultados de `binarySearch()` no están definidos en este ejemplo.

**16.** ¿Cuáles de estas afirmaciones son verdaderas? (Elija todas las opciones que correspondan).

```Java
var letters = new StringBuilder("abcdefg");
```

- [x]   A. `letters.substring(1, 2)` devuelve un `String` de un solo carácter.
- [x]   B. `letters.substring(2, 2)` devuelve un `String` de un solo carácter.
- [ ]   C. `letters.substring(6, 5)` devuelve un `String` de un solo carácter.
- [x]   D. `letters.substring(6, 6)` devuelve un `String` de un solo carácter.
- [ ]   E. `letters.substring(1, 2)` lanza una excepción.
- [ ]   F. `letters.substring(2, 2)` lanza una excepción.
- [x]   G. `letters.substring(6, 5)` lanza una excepción.
- [ ]   H. `letters.substring(6, 6)` lanza una excepción.

**17.** ¿Cuál es el resultado del siguiente código? (Elija todas las opciones que correspondan).

```Java
13: String s1 = """
14:  purr""";
15: String s2 = "";
16:
17: s1.toUpperCase();
18: s1.trim();
19: s1.substring(1, 3);
20: s1 += "two";
21:
22: s2 += 2;
23: s2 += 'c';
24: s2 += false;
25:
26: if ( s2 == "2cfalse") System.out.println("==");
27: if ( s2.equals("2cfalse")) System.out.println("equals");
28: System.out.println(s1.length());
```

- [ ]   A. 2
- [ ]   B. 4
- [x]   C. 7
- [ ]   D. 10
- [ ]   E. `==`
- [x]   F. `equals`
- [ ]   G. Se lanza una excepción.
- [ ]   H. El código no compila.

**18.** ¿Cuáles de las siguientes opciones llenan el espacio en blanco para imprimir un número entero positivo? (Elija todas las opciones que correspondan).

```Java
String[] s1 = { "Camel", "Peacock", "Llama"};
String[] s2 = { "Camel", "Llama", "Peacock"};
String[] s3 = { "Camel"};
String[] s4 = { "Camel", null};
System.out.println(Arrays.____________________________);
```

- [ ]   A. `compare(s1, s2)`
- [x]   B. `mismatch(s1, s2)`
- [x]   C. `compare(s3, s4)`
- [ ]   D. `mismatch (s3, s4)`
- [x]   E. `compare(s4, s4)`
- [ ]   F. `mismatch (s4, s4)`

**19.** Note que el 12 de marzo de 2028 es el fin de semana en que los relojes se adelantan para el horario de verano. ¿Cuál es la salida de lo siguiente? (Elija todas las opciones que correspondan).

```Java
var date = LocalDate.of(2028, Month.MARCH, 12);
var time = LocalTime.of(1, 30);
var zone = ZoneId.of("US/Eastern");
var dateTime1 = ZonedDateTime.of(date, time, zone);
var dateTime2 = dateTime1.plus(1, ChronoUnit.HOURS);

long diff = ChronoUnit.HOURS.between(dateTime1, dateTime2);
int hour = dateTime2.getHour();
boolean offset = dateTime1.getOffset() == dateTime2.getOffset();

System.out.println("diff = " + diff);
System.out.println("hour = " + hour);
System.out.println("offset = " + offset);
```

- [x]   A. `diff = 1`
- [ ]   B. `diff = 2`
- [ ]   C. `hour = 2`
- [x]   D. `hour = 3`
- [ ]   E. `offset = true`
- [ ]   F. El código no compila.
- [ ]   G. Se lanza una excepción en tiempo de ejecución.

**20.** ¿Cuáles de las siguientes opciones pueden llenar el espacio en blanco para imprimir `avaJ`? (Elija todas las opciones que correspondan).

```Java
3: var puzzle = new StringBuilder("Java");
4: puzzle._________________________;
5: System.out.println(puzzle);
```

- [x]   A. `reverse()`
- [ ]   B. `append("vaJ$").substring(0, 4)`
- [x]   C. `append("vaJ$").delete(0, 3).deleteCharAt(puzzle.length() - 1)`
- [ ]   D. `append("vaJ$").delete(0, 3).deleteCharAt(puzzle.length())`
- [ ]   E. Ninguna de las anteriores.

**21.** ¿Cuál es la salida del siguiente código?

```Java
var date = LocalDate.of(2025, Month.APRIL, 30);
date.plusDays(2);
date.plusYears(3);
System.out.println(date.getYear() + " " + date.getMonth() + " " + date.getDayOfMonth());
```

- [x]   A. `2025 APRIL 30`
- [ ]   B. `2025 MAY 2`
- [ ]   C. `2028 APRIL 2`
- [ ]   D. `2028 APRIL 30`
- [ ]   E. `2028 MAY 2`
- [ ]   F. El código no compila.
- [ ]   G. Se lanza una excepción en tiempo de ejecución.

**22.** ¿Cuál es la salida de lo siguiente?

```Java
var result = LocalDate.of(2025, Month.OCTOBER, 31)
    .plusYears(1)
    .plusMonths(-5)
    .plusMonths(1)
    .withYear(2026)
    .atTime(LocalTime.of(13, 4));
System.out.println(result);
```

- [ ]   A. `2025-06-30T13:04`
- [ ]   B. `2026-04-304`
- [ ]   C. `2026-04-30T13:04`
- [ ]   D. `2026-06-30T`
- [x]   E. `2026-06-30T13:04`
- [ ]   F. El código no compila.
- [ ]   G. Se lanza una excepción en tiempo de ejecución.