Las respuestas a las preguntas de revisión del capítulo se pueden encontrar al final del capitulo.

**1.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
32: Object skips = 10;
33: switch (skips) {
34:     case a when a < 10 -> System.out.print(2);
35:     case b when b >= 10 -> System.out.print(4);
36:     case null -> System.out.print(6);
37:     default -> System.out.print(8);
38: }
```

- [ ]  A. 2
- [ ]  B. 4
- [ ]  C. 6
- [ ]  D. 8
- [ ]  E. Exactamente una línea no compila.
- [ ]  F. Exactamente dos líneas no compilan.
- [ ]  G. Ninguna de las anteriores.

**2.** ¿Cuáles de los siguientes tipos de datos se pueden usar en una expresión `switch`? (Elija todas las opciones que correspondan).

- [ ]  A. `enum`
- [ ]  B. `int`  
- [ ]  C. `Byte`
- [ ]  D. `long`
- [ ]  E. `boolean`
- [ ]  F. `double`

**3.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
3: int temperature = 4;
4: long humidity = -temperature + temperature * 3;
5: if (temperature >= 4)
6:     if (humidity < 6) System.out.println("Too Low");
7:     else System.out.println("Just Right");
8: else System.out.println("Too High");
```

- [ ]  A. `Too Low`
- [ ]  B. `Just Right`
- [ ]  C. `Too High`
- [ ]  D. Se lanza una `NullPointerException` en tiempo de ejecución.
- [ ]  E. El código no compilará debido a la línea 7.
- [ ]  F. El código no compilará debido a la línea 8.

**4.** ¿Cuáles de los siguientes tipos de datos están permitidos en el lado derecho de una expresión `for-each`? (Elija todas las opciones que correspondan).

- [x]  A. `Double[][]`
- [ ]  B. `Object`
- [ ]  C. `Map`
- [x]  D. `List`
- [ ]  E. `String`
- [x]  F. `char[]`
- [ ]  G. `Exception`

**5.** ¿Cuál es la salida de llamar a `printReptile(6)`?

```Java
void printReptile(int category) {
    var type = switch (category) {
        case 1,2 -> "Snake";
        case 3,4 -> "Lizard";
        case 5,6 -> "Turtle";
        case 7,8 -> "Alligator";
    };
    System.out.print(type);
}
```

- [ ]  A. `Snake`
- [ ]  B. `Lizard`
- [ ]  C. `Turtle`
- [ ]  D. `Alligator`
- [ ]  E. `TurtleAlligator`
- [ ]  F. Ninguna de las anteriores

**6.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
List<Integer> myFavoriteNumbers = new ArrayList<>();
myFavoriteNumbers.add(10);
myFavoriteNumbers.add(14);
for (var a : myFavoriteNumbers) {
    System.out.print(a + ", ");
    break;
}
for (int b : myFavoriteNumbers) {
    continue;
    System.out.print(b + ", ");
}
for (Object c : myFavoriteNumbers)
    System.out.print(c + ", ");
```

- [ ]  A. Compila y se ejecuta sin problemas, pero no produce ninguna salida.
- [ ]  B. `10, 14,`
- [ ]  C. `10, 10, 14,`
- [ ]  D. `10, 10, 14, 10, 14,`
- [ ]  E. Exactamente una línea de código no compila.
- [ ]  F. Exactamente dos líneas de código no compilan.
- [ ]  G. Tres o más líneas de código no compilan.
- [ ]  H. El código contiene un bucle infinito y no termina.

**7.** Suponiendo que `weather` es un arreglo no vacío y bien formado, ¿qué fragmento de código, cuando se inserta de forma independiente en el espacio en blanco en el siguiente código, imprime todos los elementos de `weather`? (Elija todas las opciones que correspondan).

```Java
private void print(int[] weather) {
    for (__________) {
        System.out.println(weather[i]);
    }
}
```

- [ ]  A. `int i=weather.length; i>0; i--` 
- [ ]  B. `int i=0; i<=weather.length-1; ++i`
- [ ]  C. `var w : weather`
- [ ]  D. `int i=weather.length-1; i>=0; i--`
- [ ]  E. `int i=0, int j=3; i<weather.length; i++`
- [ ]  F. `int i=0; ++i<10 && i<weather.length;`

**8.** ¿Cuál es la salida de llamar a `printType(11)`?

```Java
31: void printType(Object o) {
32:     if (o instanceof Integer bat) {
33:         System.out.print("int");
34:     } else if (o instanceof Integer bat && bat < 10) {
35:         System.out.print("small int");
36:     } else if (o instanceof Long bat || bat <= 20) {
37:         System.out.print("long");
38:     } default {
39:         System.out.print("unknown");
40:     }
41: }
```

- [ ]  A. `int`
- [ ]  B. `small int`
- [ ]  C. `long`
- [ ]  D. `unknown`
- [ ]  E. No se imprime nada.
- [ ]  F. El código contiene una línea que no compila.
- [ ]  G. El código contiene dos líneas que no compilan.
- [ ]  H. Ninguna de las anteriores.

**9.** ¿Qué sentencias, cuando se insertan de forma independiente en el siguiente espacio en blanco, harán que el código imprima 2 en tiempo de ejecución? (Elija todas las opciones que correspondan).

```Java
int count = 0;
BUNNY: for (int row = 1; row <=3; row++)
    RABBIT: for (int col = 0; col <3 ; col++) {
        if ((col + row) % 2 == 0)
            _____________;
        count++;
    }
System.out.println(count);
```

- [ ]  A. `break BUNNY`
- [ ]  B. `break RABBIT`
- [ ]  C. `continue BUNNY`
- [ ]  D. `continue RABBIT`
- [ ]  E. `break`
- [ ]  F. `continue`
- [ ]  G. Ninguna de las anteriores, ya que el código contiene un error del compilador

**10.** Dado el siguiente método, ¿cuántas líneas contienen errores de compilación?

```Java
8:  enum DayOfWeek {
9:      SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY;
10:     private DayOfWeek getWeekDay(int day, final int thursday) {
11:         int otherDay = day;
12:         int Sunday = 0;
13:         switch (otherDay) {
14:             default:
15:             case 1: continue;
16:             case thursday: return DayOfWeek.THURSDAY;
17:             case 2,10: break;
18:             case Sunday: return DayOfWeek.SUNDAY;
19:             case DayOfWeek.MONDAY: return DayOfWeek.MONDAY;
20:         }
21:         return DayOfWeek.FRIDAY;
22:     } 
    }
```

- [ ]  A. Ninguna, el código compila y se ejecuta sin problemas.
- [ ]  B. 1.
- [ ]  C. 2.
- [ ]  D. 3.
- [ ]  E. 4.
- [ ]  F. 5.
- [ ]  G. 6.
- [ ]  H. El código compila pero puede producir un error en tiempo de ejecución.

**11.** ¿Cuál es la salida de llamar a `printLocation(Animal.MAMMAL)`?

```Java
10: class Zoo {
11:     enum Animal {BIRD, FISH, MAMMAL}
12:     void printLocation(Animal a) {
13:         long type = switch (a) {
14:             case BIRD -> 1;
15:             case FISH -> 2;
16:             case MAMMAL -> 3;
17:             default -> 4;
18:         };
19:         System.out.print(type);
20:     } 
21: }
```

- [ ] A. 3
- [ ]  B. 4
- [ ]  C. 34
- [ ]  D. El código no compila debido a la línea 13.
- [ ]  E. El código no compila debido a la línea 17.
- [ ]  F. Ninguna de las anteriores.

**12.** ¿Cuál es el resultado del siguiente fragmento de código?

```Java
3: int sing = 8, squawk = 2, notes = 0;
4: while (sing > squawk) {
5:     sing--;
6:     squawk += 2;
7:     notes += sing + squawk;
8: }
9: System.out.println(notes);
```

- [ ]  A. 11
- [ ]  B. 13
- [ ]  C. 23
- [ ]  D. 33
- [ ]  E. 50
- [ ]  F. El código no compilará debido a la línea 7.

**13.** ¿Cuál es la salida de llamar a `printType(Animal.MAMMAL)`?

```Java
11: enum Animal {BIRD, FISH, MAMMAL}
12: void printType(Object type) {
13:     type = switch(type) {
14:         case BIRD -> 1;
15:         case FISH -> 2;
16:         case MAMMAL -> 3;
17:         default -> 4;
18:     };
19:     System.out.print(type);
20: }
```

- [ ]  A. 3
- [ ]  B. 4
- [ ]  C. ... (Opciones incompletas en el original proporcionado)
- [ ]  F. El código no compila por una razón diferente.

**14.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
2: boolean keepGoing = true;
3: int result = 15, meters = 10;
4: do {
5:     meters--;
6:     if (meters==8) keepGoing = false;
7:     result -= 2;
8: } while keepGoing;
9: System.out.println(result);
```

- [ ]  A. 7
- [ ]  B. 9
- [ ]  C. 10
- [ ]  D. 11
- [ ]  E. 15
- [ ]  F. El código no compilará debido a la línea 6.
- [ ]  G. El código no compila por una razón diferente.

**15.** ¿Qué afirmaciones sobre el siguiente fragmento de código son correctas? (Elija todas las opciones que correspondan).

```Java
for (var penguin : new int[2])
    System.out.println(penguin);
var ostrich = new Character[3];
for (var emu : ostrich)
    System.out.println(emu);
List<Integer> parrots = new ArrayList<Integer>();
for (var macaw : parrots)
    System.out.println(macaw);
```

- [ ]  A. El tipo de datos de `penguin` es `Integer`.
- [ ]  B. El tipo de datos de `penguin` es `int`.
- [ ]  C. El tipo de datos de `emu` es indefinido.
- [ ]  D. El tipo de datos de `emu` es `Character`.
- [ ]  E. El tipo de datos de `macaw` es `List`.
- [ ]  F. El tipo de datos de `macaw` es `Integer`.
- [ ]  G. Ninguna de las anteriores, ya que el código no compila.

**16.** ¿Cuál es el resultado del siguiente fragmento de código?

```Java
final char a = 'A', e = 'E';
char grade = 'B';
switch (grade) {
    default:
    case a:
    case 'B': 'C': System.out.print("great ");
    case 'D': System.out.print("good "); break;
    case e:
    case 'F': System.out.print("not good ");
}
```

- [ ]  A. `great`
- [ ]  B. `great good`
- [ ]  C. `good`
- [ ]  D. `not good`
- [ ]  E. El código no compila porque el tipo de datos de una o más cláusulas `case` no coincide con el tipo de datos de la variable `switch`.
- [ ]  F. Ninguna de las anteriores.

**17.** Dado el siguiente arreglo, ¿qué fragmentos de código imprimen los elementos en orden inverso al de su declaración? (Elija todas las opciones que correspondan).

```Java
char[] wolf = {'W', 'e', 'b', 'b', 'y'};
```

- [ ]  A.

```Java
int q = wolf.length;
for ( ; ; ) {
    System.out.print(wolf[--q]);
    if (q==0) break;
}
```

- [ ]  B.

```Java
for (int m=wolf.length-1; m>=0; --m)
    System.out.print(wolf[m]);
```

- [ ]  C.

```Java
for (int z=0; z<wolf.length; z++)
    System.out.print(wolf[wolf.length-z]);
```

- [ ]  D.

```Java
for (int x=wolf.length-1, j=0; x>=0 && j==0; x--)
    System.out.print(wolf[x]);
```

- [ ]  E.

```Java
final int r = wolf.length;
for (int w = r-1; r>-1; w = r-1)
    System.out.print(wolf[w]);
```

- [ ]  F.

```Java
for (int i=wolf.length; i>0; --i)
    System.out.print(wolf[i]);
```

- [ ]  G. Ninguna de las anteriores

**18.** ¿Qué números distintos se imprimen cuando se ejecuta el siguiente método? (Elija todas las opciones que correspondan).

```Java
private void countAttendees() {
    int participants = 4, animals = 2, performers = -1;
    while ((participants = participants + 1) < 10) {}
    do {} while (animals++ <= 1);
    for ( ; performers < 2; performers += 2) {}
    System.out.println(participants);
    System.out.println(animals);
    System.out.println(performers);
}
```

- [ ]  A. 6
- [ ]  B. 3
- [ ]  C. 4
- [ ]  D. 5
- [ ]  E. 10
- [ ]  F. 9
- [ ]  G. El código no compila.
- [ ]  H. Ninguna de las anteriores.

**19.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
2: double iguana = 0;
3: do {
4:     int snake = 1;
5:     System.out.print(snake++ + " ");
6:     iguana--;
7: } while (snake <= 5);
8: System.out.println(iguana);
```

- [ ]  A. `1 2 3 4 -4.0`
- [ ]  B. `1 2 3 4 -5.0`
- [ ]  C. `1 2 3 4 5 -4.0`
- [ ]  D. `0 1 2 3 4 5 -5.0`
- [ ]  E. El código no compila.
- [ ]  F. El código compila pero produce un bucle infinito en tiempo de ejecución.
- [ ]  G. Ninguna de las anteriores.

**20.** ¿Qué sentencias, cuando se insertan en los siguientes espacios en blanco, permiten que el código compile y se ejecute sin entrar en un bucle infinito? (Elija todas las opciones que correspondan).

```Java
4: int height = 1;
5: L1: while (height++ <10) {
6:     long humidity = 12;
7:     L2: do {
8:         if (humidity-- % 12 == 0) _____________;
9:         int temperature = 30;
10:        L3: for ( ; ; ) {
11:            temperature++;
12:            if (temperature>50) _____________;
13:        }
14:    } while (humidity > 4);
15: }
```

- [ ]  A. `break L2` en la línea 8; `continue L2` en la línea 12
- [ ]  B. `continue` en la línea 8; `continue` en la línea 12
- [ ]  C. `break L3` en la línea 8; `break L1` en la línea 12
- [ ]  D. `continue L2` en la línea 8; `continue L3` en la línea 12
- [ ]  E. `continue L2` en la línea 8; `continue L2` en la línea 12
- [ ]  F. Ninguna de las anteriores, ya que el código contiene un error del compilador

**21.** ¿Cuál es el número mínimo de líneas que necesitan ser corregidas antes de que el siguiente método compile?

```Java
21: void findZookeeper(Integer id) {
22:     System.out.print(switch (id) {
23:         case 10 -> {"Jane";}
24:         case 20 -> {yield "Lisa";};
25:         case 30 -> "Kelly";
26:         case 30 -> "Sarah";
27:         default -> "Unassigned";
28:     });
29: }
```

- [ ]  A. Cero
- [ ]  B. Una
- [ ]  C. Dos
- [ ]  D. Tres
- [ ]  E. Cuatro
- [ ]  F. Cinco

**22.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
2: var tailFeathers = 3;
3: final var one = 1;
4: switch (tailFeathers) {
5:     case one: System.out.print(3 + " ");
6:     default: case 3: System.out.print(5 + " ");
7: }
8: while (tailFeathers > 1) {
9:     System.out.print(--tailFeathers + " "); 
   }
```

- [ ]  A. `3`
- [ ]  B. `5 1`
- [ ]  C. `5 2`
- [ ]  D. `3 5 1`
- [ ]  E. `5 2 1`  
- [ ]  F. El código no compilará debido a las líneas 3–5.
- [ ]  G. El código no compilará debido a la línea 6.

**23.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
15: int penguin = 50, turtle = 75;
16: boolean older = penguin >= turtle;
17: if (older = true) System.out.println("Success");
18: else System.out.println("Failure");
19: else if (penguin != 50) System.out.println("Other");
```

- [ ]  A. `Success`
- [ ]  B. `Failure`
- [ ]  C. `Other`
- [ ]  D. El código no compilará debido a la línea 17.
- [ ]  E. El código compila pero lanza una excepción en tiempo de ejecución.
- [ ]  F. Ninguna de las anteriores.  

**24.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
22: String zooStatus = "Closed";
23: int visitors = switch (zooStatus) {
24:     case String s when s.equals("Open") -> 10;
25:     case Object s when s != null && !s.equals("") -> 20;
26:     case null -> {yield 30;}
27:     default -> 40;
28: };
29: System.out.print(visitors);
```

- [ ]  A. 10
- [ ]  B. 20
- [ ]  C. 30
- [ ]  D. 40
- [ ]  E. Exactamente una línea no compila.
- [ ]  F. Exactamente dos líneas no compilan. 
- [ ]  G. Tres o más líneas no compilan.

**25.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
6: String instrument = "violin";
7: final String CELLO = "cello";
8: String viola = "viola";
9: int p = -1;
10: switch (instrument) {
11:     case "bass" : break;
12:     case CELLO : p++;
13:     default: p++;
14:     case "VIOLIN": p++;
15:     case "viola" : ++p; break;
16: }
17: System.out.print(p);
```

- [ ]  A. -1
- [ ]  B. 0
- [ ]  C. 1
- [ ]  D. 2  
- [ ]  E. 3
- [ ]  F. El código no compila.

**26.** ¿Cuál es la salida del siguiente fragmento de código?

```Java
9: int w = 0, r = 1;
10: String name = "";
11: while (w < 2) {
12:     name += "A";
13:     do {
14:         name += "B";
15:         if (name.length() > 0) name += "C";
16:         else break;
17:     } while (r <= 1);
18:     r++; w++; 
    }
19: System.out.println(name);
```

- [ ]  A. `ABC`
- [ ]  B. `ABCABC`
- [ ]  C. `ABCABCABC`
- [ ]  D. La línea 15 contiene un error de compilación.
- [ ]  E. La línea 18 contiene un error de compilación.
- [ ]  F. El código compila pero nunca termina en tiempo de ejecución.
- [ ]  G. El código compila pero lanza una `NullPointerException` en tiempo de ejecución.

**27.** ¿Qué imprime el siguiente fragmento de código?

```Java
23: byte amphibian = 2;
24: String name = "Salamander";
25: String color = switch (amphibian) {
26:     case 1 -> { yield "Red"; }
27:     case 2 -> { if (name.equals("Frog")) yield "Green";
28:         yield "Blue"; }
29:     case 3 -> { yield "Purple"; }
30:     default -> throw new RuntimeException();
31: };
32: System.out.print(color);
```

- [ ]  A. `Red`
- [ ]  B. `Green`
- [ ]  C. `Purple`
- [ ]  D. `Blue`
- [ ]  E. El código no compila.
- [ ]  F. Se lanza una excepción en tiempo de ejecución.

**28.** ¿Cuál es la salida de llamar a `getFish("goldie")`?

```Java
40: void getFish(Object fish) {
41:     if (!(fish instanceof String guppy))
42:         System.out.print("Eat!");
43:     else if (!(fish instanceof String guppy)) {
44:         throw new RuntimeException();
45:     }
46:     System.out.print("Swim!");
47: }
```

- [ ]  A. `Eat!`
- [ ]  B. `Swim!`
- [ ]  C. `Eat!` seguido de una excepción
- [ ]  D. `Eat!Swim!`
- [ ]  E. Se imprime una excepción
- [ ]  F. Ninguna de las anteriores

**29.** ¿Cuál es el resultado del siguiente código?

```Java
1: public class PrintIntegers {
2:     public static void main(String[] args) {
3:         int y = -2;
4:         do System.out.print(++y + " ");
5:         while (y <= 5);
6:     } 
   }
```

- [ ]  A. `-2 -1 0 1 2 3 4 5`
- [ ]  B. `-2 -1 0 1 2 3 4`
- [ ]  C. `-1 0 1 2 3 4 5 6`
- [ ]  D. `-1 0 1 2 3 4 5`
- [ ]  E. El código no compilará debido a la línea 5.
- [ ]  F. El código contiene un bucle infinito y no termina.

**30.** ¿Cuál es el número mínimo de líneas que necesitarían cambiarse o eliminarse para que el siguiente código compile y devuelva un valor cuando se llama con `dance(10)`?

```Java
41: double dance(Object speed) {
42:     return switch (speed) {
43:         case 5 -> {yield 4};
44:         case 10 -> 8;
45:         case 15,20 -> 12;
46:         default -> 20;
47:         case null -> 16;
48:     }
49: }
```

- [ ]  A. Cero, el código compila y se ejecuta sin problemas
- [ ]  B. Una
- [ ]  C. Dos
- [ ]  D. Tres
- [ ]  E. Cuatro
- [ ]  F. Cinco
- [ ]  G. Seis