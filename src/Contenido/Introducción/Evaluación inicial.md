Ahora vas a realizar la prueba de evaluación inicia. No te preocupes si no sabes todas las respuestas; lo importante es intentarlo con calma y dar lo mejor de ti.

Evita usar ayuda externa, como inteligencia artificial o internet. Siéntate en un lugar tranquilo, concéntrate y dedica aproximadamente una hora para completarla. Esta prueba te permitirá conocer tu punto de partida y saber dónde estás.

¡Mucho éxito! Recuerda que, con esfuerzo y dedicación, podrás lograrlo.

### Evaluación

Utiliza la siguiente prueba de evaluación para medir tu nivel actual de habilidad en Java para el examen 1Z0-830. Esta prueba está diseñada para resaltar algunos temas como tus fortalezas y debilidades, de modo que sepas qué capítulos podrías querer leer varias veces. Incluso si te va bien en la prueba de evaluación, deberías leer el proyecto de principio a fin, ya que las preguntas del examen real son bastante desafiantes.

**1. ¿Cuál es el resultado de ejecutar el siguiente fragmento de código?**

```java
41: final int score1 = 8, score2 = 3;
42: Integer myScore = 7;
43: var goal = switch (myScore) {
44:     case score1, score2, 7 -> "good";
45:     case Integer i when i < 10 -> "better";
46:     case Integer i when i >= 10 -> "best";
47:     default -> { yield "unknown"; }
48:     case null -> "nope";
49: };
50: System.out.print(goal);
```

- [ ] A. good
- [ ] B. better
- [ ] C. best
- [ ] D. unknown
- [ ] E. nope
- [ ] F. La línea 44 no compila.
- [ ] G. La línea 45 no compila. 
- [ ] H. La línea 46 no compila. 
- [ ] I. La línea 47 no compila. 
- [ ] J. La línea 48 no compila.

**2. ¿Cuál es la salida del siguiente fragmento de código?**

```java
int moon = 9, star = 2 + 2 * 3;
float sun = star > 10 ? 1 : 3;
double jupiter = (sun + moon) - 1.0f;
int mars = --moon <= 8 ? 2 : 3;
System.out.println(sun + ", " + jupiter + ", " + mars);
```

- [ ] A. 1, 11, 2 
- [ ] B. 3.0, 11.0, 2 
- [ ] C. 1.0, 11.0, 3 
- [ ] D. 3.0, 13.0, 3 
- [ ] E. 3.0f, 12, 2 
- [ ] F. El código no compila porque una de las asignaciones requiere un cast numérico explícito.

**3. ¿Qué APIs existen para crear o trabajar con hilos virtuales (virtual threads)? (Elige todas las que apliquen)** 

- [ ] A. `Executors.newVirtualThread()` 
- [ ] B. `Executors.newVirtualThreadExecutor()` 
- [ ] C. `Executors.newVirtualThreadPerTaskExecutor()` 
- [ ] D. `new VirtualThread()` 
- [ ] E. `Thread.ofVirtual()` 
- [ ] F. `Thread.ofVirtualThread()`

**4. ¿Cuál es la salida de este código?**

```java
20: Predicate<String> empty = String::isEmpty;
21: Predicate<String> notEmpty = empty.negate();
22: var result = Stream.generate(() -> "")
23:     .filter(notEmpty)
24:     .collect(Collectors.groupingBy(k -> k))
25:     .entrySet()
26:     .stream()
27:     .map(Entry::getValue)
28:     .flatMap(Collection::stream)
29:     .collect(Collectors.partitioningBy(notEmpty));
30: System.out.println(result);
```

- [ ] A. Imprime `{}`. 
- [ ] B. Imprime `{false=[], true=[]}`. 
- [ ] C. El código no compila. 
- [ ] D. El código no termina.

**5. ¿Cuál es el resultado del siguiente programa?**

``` java
1: public class MathFunctions {
2:    public static void addToInt(int x, int amountToAdd) {
3:       x = x + amountToAdd;
4:    }
5:    public static void main(String[] args) {
6:       var a = 15;
7:       var b = 10;
8:       MathFunctions.addToInt(a, b);
9:       System.out.println(a); 
      } 
   }
```

- [ ] A. 10 
- [ ] B. 15 
- [ ] C. 25 
- [ ] D. Error de compilación en la línea 3 
- [ ] E. Error de compilación en la línea 8 
- [ ] F. Ninguna de las anteriores

**6. Supongamos que tenemos los siguientes archivos de propiedades y código. ¿Qué valores se imprimen en las líneas 8 y 9, respectivamente?**

_Penguin.properties_ 
`name=Billy` 
`age=1`

_Penguin_de.properties_ 
`name=Chilly` 
`age=4`

_Penguin_en.properties_ 
`name=Willy`

```java
5: Locale fr = Locale.of("fr");
6: Locale.setDefault(Locale.of("en", "US"));
7: var b = ResourceBundle.getBundle("Penguin", fr);
8: System.out.println(b.getString("name"));
9: System.out.println(b.getString("age"));
```

- [ ] A. Billy y 1 
- [ ] B. Billy y null 
- [ ] C. Willy y 1 
- [ ] D. Willy y null 
- [ ] E. Chilly y null 
- [ ] F. El código no compila.

**7. ¿Qué se garantiza que imprimirá el siguiente código? (Elige todas las que apliquen)**

```java
int[] array = {6, 9, 8};
System.out.println("B" + Arrays.binarySearch(array, 9));
System.out.println("C" + Arrays.compare(array, new int[] {6, 9, 8}));
System.out.println("M" + Arrays.mismatch(array, new int[] {6, 9, 8}));
```

- [ ] A. B1 
- [ ] B. B2 
- [ ] C. C-1 
- [ ] D. C0 
- [ ] E. M-1 
- [ ] F. M0 
- [ ] G. El código no compila.

**8. ¿Qué interfaces funcionales completan el siguiente código, asumiendo que la variable `r` existe? (Elige todas las que apliquen)**

```java
6: _____ x = r.negate();
7: _____ y = () -> System.out.println();
8: _____ z = (a, b) -> a - b;
```

- [ ] A. `BinaryPredicate<Integer, Integer>` 
- [ ] B. `Comparable<Integer>` 
- [ ] C. `Comparator<Integer>`
- [ ] D. `Consumer<Integer>` 
- [ ] E. `Predicate<Integer>` 
- [ ] F. `Runnable` 
- [ ] G. `Runnable<Integer>`

**9. Supón que tienes un módulo llamado `com.vet`. ¿Dónde podrías colocar el siguiente archivo `module-info.java` para crear un módulo válido?**

``` java
public module com.vet {
    exports com.vet;
}
```

- [ ] A. Al mismo nivel que la carpeta `com` 
- [ ] B. Al mismo nivel que la carpeta `vet` 
- [ ] C. Dentro de la carpeta `vet` 
- [ ] D. Ninguna de las anteriores

**10. ¿Cuál es la salida del siguiente programa? (Elige todas las que apliquen)**

```java
15: interface HasTail { default int getTailLength() { return 2; } }
16: abstract class Puma implements HasTail {
17:    int getTailLength() { return 4; } 
    }
18: public class Cougar implements HasTail {
19:    public static void main(String[] args) {
20:       var puma = new Puma() {};
21:       System.out.print(puma.getTailLength());
22:    }
23:    public int getTailLength(int length) { return 2; }
24: }
```

- [ ] A. 2 
- [ ] B. 4 
- [ ] C. 10 
- [ ] D. La línea 15 no compila. 
- [ ] E. La línea 16 no compila. 
- [ ] F. La línea 17 no compila. 
- [ ] G. La línea 20 no compila. 
- [ ] H. Las líneas 22-24 no compilan. 
- [ ] I. La línea 27 no compila. 
- [ ] J. Ninguna de las anteriores.

**11. ¿Qué líneas en `Tadpole.java` dan un error de compilación? (Elige todas las que apliquen)**

```java
//Frog.java
1: package animal;
2: public class Frog {
3:    protected void ribbit() { }
4:    void jump() { }
5: }
```

```java
//Tadpole.java
1: package other;
2: import animal.*;
3: public class Tadpole extends Frog {
4:    public static void main(String[] args) {
5:       Tadpole t = new Tadpole();
6:       t.ribbit();
7:       t.jump();
8:       Frog f = new Tadpole();
9:       f.ribbit();
10:      f.jump();
11:   }
12: }
```

- [ ] A. Línea 5. 
- [ ] B. Línea 6. 
- [ ] C. Línea 7. 
- [ ] D. Línea 8. 
- [ ] E. Línea 9. 
- [ ] F. Línea 10. 
- [ ] G. Todas las líneas compilan.

**12. ¿Cuál de las siguientes declaraciones puede llenar el espacio en blanco para que el código compile exitosamente? (Elige todas las que apliquen)**

```java
Set<? extends RuntimeException> mySet = new _________________ ();
```

- [ ] A. `HashSet<? extends RuntimeException>` 
- [ ] B. `HashSet<Exception>` 
- [ ] C. `TreeSet<RuntimeException>` 
- [ ] D. `TreeSet<NullPointerException>` 
- [ ] E. `LinkedHashSet<>` 
- [ ] F. `LinkedHashSet<?>` 
- [ ] G. Ninguna de las anteriores

**13. Asume que `birds.ser` existe, es accesible y contiene datos para un objeto `Bird`. ¿Cuál es el resultado de ejecutar el siguiente código? (Elige todas las que apliquen)**

```java
1: import java.io.*;
2: public class Bird {
3:    private String name;
4:    private transient Integer age;
5:    // Getters/setters omitidos
6:    public static void main(String[] args) {
7:       try(var is = new ObjectInputStream(
8:          new BufferedInputStream(
9:             new FileInputStream("birds.ser")))) {
10:         Bird b = is.readObject();
11:         System.out.println(b.age);
12:       }
13:    }
14: }
```

- [ ] A. Compila e imprime 0 en tiempo de ejecución. 
- [ ] B. Compila e imprime null en tiempo de ejecución. 
- [ ] C. Compila e imprime un número en tiempo de ejecución. 
- [ ] D. El código no compilará debido a las líneas 9-11. 
- [ ] E. El código no compilará debido a la línea 10 (nota: el código fuente original tiene un error tipográfico en la pregunta, pero en la opción se refiere a la lectura del objeto). 
- [ ] F. Compila pero lanza una excepción en tiempo de ejecución.

**14. ¿Cuántas de las siguientes declaraciones (líneas 14-19) son miembros de instancia válidos de una clase?**

```java
13: public class Zoo {
14:    var var = 3;
15:    Var case = new Var();
16:    void var() {}
17:    int Var() { var _ = 7; return _; }
18:    String new = "var";
19:    var var() { return null; }
20: }
```

- [ ] A. 0 
- [ ] B. 1 
- [ ] C. 2 
- [ ] D. 3 
- [ ] E. 4 
- [ ] F. 5

**15. ¿Qué es verdadero si el contenido de `path1` comienza con el texto "Howdy"? (Elige dos)**

```java
System.out.println(Files.mismatch(path1, path2));
```

- [ ] A. Si `path2` no existe, el código imprime -1. 
- [ ] B. Si `path2` no existe, el código imprime 0. 
- [ ] C. Si `path2` no existe, el código lanza una excepción. 
- [ ] D. Si el contenido de `path2` comienza con "Hello", el código imprime -1. 
- [ ] E. Si el contenido de `path2` comienza con "Hello", el código imprime 0. 
- [ ] F. Si el contenido de `path2` comienza con "Hello", el código imprime 1.

**16. ¿Cuál de los siguientes es el mejor tipo para insertar en el espacio en blanco para permitir que el programa compile exitosamente?**

```java
1: import java.util.*;
2: final class Amphibian {}
3: abstract class Tadpole extends Amphibian {}
4: public class FindAllTadpoles {
5:    public static void main(String... args) {
6:       var tadpoles = new ArrayList<Tadpole>();
7:       for (var amphibian : tadpoles) {
8:          __________ tadpole = amphibian;
9:       }
10:   }
11: }
```

- [ ] A. `List<Tadpole>` 
- [ ] B. `Boolean` 
- [ ] C. `Amphibian` 
- [ ] D. `Tadpole` 
- [ ] E. `Object` 
- [ ] F. Ninguna de las anteriores

**17. ¿Cuál es el resultado de compilar y ejecutar el siguiente programa?**

```java
1: public class FeedingSchedule {
2:    public static void main(String[] args) {
3:       var x = 5;
4:       var j = 0;
5:       OUTER: for (var i = 0; i < 3;)
6:          INNER: do {
7:             i++;
8:             x++;
9:             if (x > 10) break INNER;
10:            x += 4;
11:            j++;
12:         } while (j <= 2);
13:      System.out.println(x);
14:   }
15: }
```

- [ ] A. 10 
- [ ] B. 11 
- [ ] C. 12 
- [ ] D. 17 
- [ ] E. El código no compilará debido a la línea 5. 
- [ ] F. El código no compilará debido a la línea 6.

**18. Cuando se imprime, ¿qué String da el mismo valor que este bloque de texto?**

```java
var pooh = """
   "Oh, bother." -Pooh
   """.indent(1);
System.out.print(pooh);
```

- [ ] A. `\n\"Oh, bother.\" -Pooh\n` 
- [ ] B. `\n \"Oh, bother.\" -Pooh\n` 
- [ ] C. `\"Oh, bother.\" -Pooh\n` 
- [ ] D. `\n\"Oh, bother.\" -Pooh` 
- [ ] E. `\n \"Oh, bother.\" -Pooh` 
- [ ] F. `\"Oh, bother.\" -Pooh` 
- [ ] G. Ninguna de las anteriores

**19. Un módulo _______ siempre contiene un archivo `module-info.java`, mientras que un módulo _______ siempre exporta todos sus paquetes a otros módulos.** 

- [ ] A. automático, nombrado 
- [ ] B. automático, sin nombre (unnamed) 
- [ ] C. nombrado, automático 
- [ ] D. nombrado, sin nombre (unnamed) 
- [ ] E. sin nombre, automático 
- [ ] F. sin nombre, nombrado 
- [ ] G. Ninguna de las anteriores

**20. ¿Cuál es el resultado del siguiente código?**

```java
22: SequencedMap<Character, Integer> t = new TreeMap<>();
23: t.put('k', 1);
24: t.put('m', 2);
25: var u = Collections.unmodifiableMap(t);
26: t.put('m', 3);
27: t.putFirst('m', 4);
28: t.putLast('x', 5);
29: t.replaceAll((k, v) -> v + v);
30: u.entrySet()
31:  .stream()
32:  .map(e -> e.getValue())
33:  .forEach(System.out::print);
```

- [ ] A. 145 
- [ ] B. 125 
- [ ] C. 135 
- [ ] D. 2468 
- [ ] E. 2810 
- [ ] F. 2410 
- [ ] G. El código no compila. 
- [ ] H. El código compila pero la línea 26 lanza una excepción en tiempo de ejecución. 
- [ ] I. Ninguna de las anteriores.

**21. ¿Cuáles de las siguientes líneas pueden llenar el espacio en blanco para imprimir `true`? (Elige todas las que apliquen)**

``` java
10: public static void main(String[] args) {
11:    System.out.println(test(
12:       ____________
13:    ));
14: }
15: private static boolean test(Function<Integer, Boolean> b) {
16:    return b.apply(5);
17: }
```

- [ ] A. `i::equals(5)` 
- [ ] B. `i -> {i == 5;}` 
- [ ] C. `(i) -> i == 5` 
- [ ] D. `(int i) -> i == 5` 
- [ ] E. `(int i) -> {return i == 5}` 
- [ ] F. `(i) -> {return i == 5;}`

**22. ¿Cuántas veces se imprime la palabra `true`?**

```java
var s1 = "Java";
var s2 = "Java";
var s3 = s1.indent(1).strip();
var s4 = s3.intern();
var sb1 = new StringBuilder();
sb1.append("Ja").append("va");
System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
System.out.println(s1 == s3);
System.out.println(s1 == s4);
System.out.println(sb1.toString() == s1);
System.out.println(sb1.toString().equals(s1));
```

- [ ] A. Una vez. 
- [ ] B. Dos veces. 
- [ ] C. Tres veces. 
- [ ] D. Cuatro veces. 
- [ ] E. Cinco veces. 
- [ ] F. El código no compila.

**23. ¿Cuál es la salida del siguiente programa?**

``` java
1: class Deer {
2:    public Deer() { System.out.print("Deer"); }
3:    public Deer(int age) { System.out.print("DeerAge"); }
4:    protected boolean hasHorns() { return false; }
5: }
6: public class Reindeer extends Deer {
7:    public Reindeer(int age) { System.out.print("Reindeer"); }
8:    public boolean hasHorns() { return true; }
9:    public static void main(String[] args) {
10:      Deer deer = new Reindeer(5);
11:      System.out.println("," + deer.hasHorns());
12:   }
13: }
```

- [ ] A. ReindeerDeer, false 
- [ ] B. DeerAgeReindeer, true 
- [ ] C. DeerReindeer, true 
- [ ] D. DeerReindeer, false 
- [ ] E. ReindeerDeer, true 
- [ ] F. DeerAgeReindeer, false 
- [ ] G. El código no compilará debido a la línea 4. 
- [ ] H. El código no compilará debido a la línea 11.

**24. ¿Cuáles de las siguientes afirmaciones son verdaderas? (Elige todas las que apliquen)**

```java
private static void magic(Stream<Integer> s) {
    Optional o = s
        .filter(x -> x < 5)
        .limit(3)
        .max((x, y) -> x - y);
    System.out.println(o.get());
}
```

- [ ] A. `magic(Stream.empty());` se ejecuta infinitamente. 
- [ ] B. `magic(Stream.empty());` lanza una excepción. 
- [ ] C. `magic(Stream.iterate(1, x -> x++));` se ejecuta infinitamente. 
- [ ] D. `magic(Stream.iterate(1, x -> x++));` lanza una excepción. 
- [ ] E. `magic(Stream.of(5, 10));` se ejecuta infinitamente. 
- [ ] F. `magic(Stream.of(5, 10));` lanza una excepción. 
- [ ] G. El método no compila.

**25. Asumiendo que las siguientes declaraciones son tipos de nivel superior declarados en el mismo archivo, ¿cuáles compilan exitosamente? (Elige todas las que apliquen)**

```java
record Music(List<String> tempo) {}
final int score = 10;
record Song(String lyrics, Music m) {
    Song {
        this.lyrics = lyrics + "Never gonna give you up";
    }
}
sealed class Dance {}
record March() {
    int roll(Song s) {
        return switch(s) {
            case null -> 2;
            case Song(var q, Music(List d)) -> 1;
            default -> 3;
        };
    }
}
sealed class Ballet extends Dance permits NewDance {
    Ballet {}
}
var d = LocalDate.of(2025, Month.OCTOBER, 20);
if (d.isAfter(LocalDate.now()))
    System.out.print("say goodbye");
abstract class NewDance extends Ballet {}
```

- [ ] A. Music 
- [ ] B. Song 
- [ ] C. Dance 
- [ ] D. March 
- [ ] E. Ballet 
- [ ] F. NewDance 
- [ ] G. Ninguna de ellas compila.

**26. ¿Cuál de las siguientes expresiones compila sin error? (Elige todas las que apliquen)** 

- [ ] A. `int monday = 3 + 2.0;` 
- [ ] B. `double tuesday = 5_6L;` 
- [ ] C. `boolean wednesday = 1 > 2 ? !true;` (Nota: Error de sintaxis en el origen, falta el valor "else") 
- [ ] D. `short thursday = (short)Integer.MAX_VALUE;` 
- [ ] E. `long friday = 8.0L;` 
- [ ] F. `var saturday = 2_.0;` 
- [ ] G. Ninguna de las anteriores

**27. ¿Cuál es el resultado de ejecutar la siguiente aplicación?**

```java
final var cb = new CyclicBarrier(3,
    () -> System.out.println("Clean!")); // u1
try (var service = Executors.newSingleThreadExecutor()) {
    IntStream.generate(() -> 1)
        .limit(12)
        .parallel()
        .forEach(i -> service.submit(() -> cb.await())); // u2
}
```

- [ ] A. Imprime "clean!" al menos una vez. 
- [ ] B. Imprime "clean!" exactamente cuatro veces. 
- [ ] C. El código no compilará debido a la línea u1. 
- [ ] D. El código no compilará debido a la línea u2. 
- [ ] E. Compila pero lanza una excepción en tiempo de ejecución. 
- [ ] F. Compila pero espera para siempre en tiempo de ejecución.

**28. ¿Qué afirmación sobre el siguiente método es verdadera?**

```java
5: public static void main(String... unused) {
6:    System.out.print("a");
7:    try (StringBuilder reader = new StringBuilder()) {
8:       System.out.print("b");
9:       throw new IllegalArgumentException();
10:   } catch (Exception e | RuntimeException e) {
11:      System.out.print("c");
12:      throw new FileNotFoundException();
13:   } finally {
14:      System.out.print("d");
15:   }
16: }
```

- [ ] A. Compila e imprime abc. 
- [ ] B. Compila e imprime abd. 
- [ ] C. Compila e imprime abcd. 
- [ ] D. Una línea contiene un error de compilación. 
- [ ] E. Dos líneas contienen un error de compilación. 
- [ ] F. Tres líneas contienen un error de compilación. 
- [ ] G. Compila pero imprime una excepción en tiempo de ejecución.


### **Respuestas a la Prueba de Evaluación**

**1. J.** El código no compila. En las declaraciones `switch` con coincidencia de patrones (pattern matching), el orden importa. En este ejemplo, la línea 47 (el caso `default`) domina a la línea 48 (`case null`), lo que lleva a código inalcanzable en la línea 48, haciendo que la opción J sea la respuesta correcta. Para más información, ver Capítulo 3.

**2. B.** Inicialmente, `moon` se asigna a 9, mientras que `star` se asigna a 8 (la multiplicación tiene precedencia: 2 + (2*3)). Dado que `star` no es mayor que 10, `sun` se asigna a 3, que se promueve a `3.0f`. El valor de `jupiter` es `(3.0f + 9) - 1.0`, que es `11.0`. En la última asignación, `moon` se decrementa de 9 a 8 (`--moon`), por lo que 8 es menor o igual a 8, y `mars` se establece en 2. La salida final es 3.0, 11.0, 2. Opción B es correcta. Ver Capítulo 2.

**3. C, E.** La clase `Executors` tiene un método de fábrica para trabajar con hilos virtuales: `newVirtualThreadPerTaskExecutor()`, lo que hace correcta la opción C. No hay constructor público para hilos virtuales. Hay un método de fábrica `Thread.ofVirtual()`, lo que hace correcta la opción E. Ver Capítulo 13.

**4. D.** El código compila. Sin embargo, la fuente es un `Stream` infinito (`Stream.generate`). La operación `filter` verificará cada elemento a su vez para ver si alguno no está vacío. Como nada pasa el filtro, el código no termina. Opción D es correcta. Ver Capítulo 10.

**5. B.** El código compila exitosamente. El valor de `a` no puede ser cambiado por el método `addToInt()` porque solo se pasa una copia de la variable primitiva `x`. Por lo tanto, `a` no cambia y la salida es 15. Opción B correcta. Ver Capítulo 5.

**6. C.** Java usará `Penguin_en.properties` como el resource bundle coincidente en la línea 7 (ya que no hay coincidencia para francés `fr`, se usa el default `en_US`). La línea 8 encuentra una clave coincidente en `Penguin_en.properties` ("Willy"). La línea 9 no encuentra coincidencia en `Penguin_en.properties`, por lo que tiene que buscar más arriba en la jerarquía en `Penguin.properties`, encontrando "1". Esto hace que la opción C sea la respuesta. Ver Capítulo 11.

**7. D, E.** El array puede usar un inicializador anónimo. Los resultados de `binarySearch` son indefinidos ya que el array no está ordenado, por lo que A y B son incorrectas. La opción D es correcta porque `compare()` devuelve 0 cuando los arrays son de la misma longitud y tienen los mismos elementos. La opción E es correcta porque `mismatch()` devuelve -1 cuando los arrays son equivalentes. Ver Capítulo 4.

**8. C, E, F.** La opción A es incorrecta porque la interfaz debería ser `BiPredicate`. La línea 6 requiere saber que `negate()` es un método de conveniencia en `Predicate`, haciendo correcta la opción E. La línea 7 no toma parámetros y no devuelve nada, lo que la hace un `Runnable` (opción F). La línea 8 toma dos parámetros y devuelve un int, lo que coincide con `Comparator` (opción C). `Comparable` es incorrecto porque solo toma un parámetro. Ver Capítulo 8.

**9. D.** Si este fuera un archivo `module-info.java` válido, debería colocarse en el directorio raíz del módulo (Opción A). Sin embargo, un módulo no puede usar el modificador de acceso `public`. La opción D es correcta porque el archivo proporcionado no compila. Ver Capítulo 12.

**10. F, H.** Un método dentro de una clase abstracta sin modificador de acceso tiene acceso de paquete. La clase implementa el método default de la interfaz que es implícitamente público, lo que significa que la línea 17 reduce la visibilidad, causando un error (Opción F). La línea 23 tampoco compila porque una clase local solo puede acceder a variables locales que sean final o efectivamente final; `length` se modifica en la línea 27, por lo que no es efectivamente final (Opción H). Ver Capítulo 7.

**11. C, E, F.** El método `jump()` tiene acceso de paquete, por lo que solo puede ser accedido desde el mismo paquete. `Tadpole` no está en el mismo paquete que `Frog`, lo que causa errores en las líneas 7 y 10 (Opciones C y F). El método `ribbit()` es `protected`, accesible desde una subclase. La línea 6 está bien, pero la línea 9 no compila (Opción E) porque la referencia de la variable es a `Frog`, lo cual no otorga acceso al método protegido desde otro paquete. Ver Capítulo 5.

**12. C, D, E.** La declaración `mySet` define un límite superior de tipo `RuntimeException`. Esto significa que las clases pueden especificar `RuntimeException` o cualquier subclase. `Exception` es una superclase, por lo que B es incorrecta. El comodín `?` no puede ocurrir en el lado derecho de la asignación (A y F incorrectas). C, D y E compilan. Ver Capítulo 9.

**13. D, E.** La declaración en las líneas 9-11 incluye una `IOException` chequeada no manejada (Opción D). La línea 12 no compila porque `is.readObject()` debe ser casteado a `Bird` y también lanza `ClassNotFoundException` e `IOException` que no están manejadas (Opción E). Si se corrigiera, imprimiría `null` porque el campo `age` es `transient`. Ver Capítulo 14.

**14. B.** La línea 14 no compila (`var` no permitido en campos de instancia). Líneas 15 y 18 no compilan (`case` y `new` son palabras reservadas). Línea 17 no compila (un solo guion bajo `_` no puede ser un identificador). Línea 19 no compila (`var` no puede ser tipo de retorno). Solo la línea 16 compila (`var` puede ser nombre de método). Opción B es la respuesta. Ver Capítulo 1.

**15. C, F.** La opción C es correcta porque `mismatch()` lanza una excepción si los archivos no existen. La opción F es correcta porque se devuelve el primer índice que difiere. "Hello" vs "Howdy": H(0) igual, e(1) vs o(1) difiere. El índice es 1. Ver Capítulo 14.

**16. F.** La clase `Amphibian` está marcada como `final`, lo que significa que la línea 3 (que intenta extenderla) provoca un error de compilación. Por lo tanto, ninguna opción hace que compile. Opción F correcta. Ver Capítulo 6.

**17. C.** El código compila. Analizando el bucle:

1. Iteración externa i=0. Interna i=1, x=6. x>10 falso. x=10, j=1.
2. Iteración interna (j=1<=2). i=2, x=11. x>10 verdadero -> `break INNER`.
3. Iteración externa (i=2). i=3, x=12. `break INNER`.
4. Iteración externa (i=3). Condición i<3 falsa. Termina. Se imprime 12. Opción C correcta. Ver Capítulo 3.

**18. C.** El bloque de texto tiene el cierre `"""` en una línea separada, lo que agrega una nueva línea al final (descarta D, E, F). Los bloques de texto no comienzan con una nueva línea inicial (descarta A y B). `.indent(1)` agrega un espacio a cada línea y normaliza el final de línea. Opción C es correcta. Ver Capítulo 1.

**19. C.** Solo los módulos nombrados requieren `module-info.java`. Los módulos automáticos siempre exportan todos los paquetes. Opción C correcta. Ver Capítulo 12.

**20. I.** El código compila (G incorrecta). Línea 26 no lanza excepción porque `TreeMap` (t) es modificable; `u` es la vista inmodificable, pero aquí modificamos `t`. `TreeMap` es un `SequencedMap`, pero las operaciones `putFirst` y `putLast` lanzan `UnsupportedOperationException` en `TreeMap` porque romperían el ordenamiento. Por lo tanto, la opción correcta es I (Ninguna de las anteriores, o más bien, que lanza excepción en líneas 27/28, no 26). Ver Capítulo 9.

**21. C, F.** La opción A parece referencia a método pero no es válida. `Predicate` (error en explicación del libro, el código usa `Function<Integer, Boolean>`) toma un parámetro y devuelve boolean. Lambda con un parámetro puede omitir paréntesis (C correcta). El `return` es opcional si es una sola declaración, pero si se usa llaves `{}` es obligatorio el punto y coma (F correcta). Opción B incorrecta (falta `return`). D y E incorrectas por usar `int` en lugar de `Integer` (autoboxing no funciona para inferir tipos en lambdas de esta manera). Ver Capítulo 8.

**22. D.** `s1` y `s2` son iguales (pool de strings). 1º print: true. 2º print: true. `s3` es nuevo objeto, pero `s4 = s3.intern()` devuelve la referencia del pool ("Java"), que es `s1`. 4º print: true. `sb1.toString()` crea nuevo objeto, `==` es false. `.equals()` compara contenido, es true. Total 4 veces true. Opción D. Ver Capítulo 4.

**23. C.** `Reindeer(5)` llama implícitamente a `super()` (constructor sin args de `Deer`), imprimiendo "Deer". Luego imprime "Reindeer". `hasHorns()` es polimórfico, se llama la versión de `Reindeer` (true). Resultado: "DeerReindeer, true". Opción C. Ver Capítulo 6.

**24. B, F.** Llamar a `get()` en un `Optional` vacío lanza excepción (B correcta). `filter(x -> x < 5)` con `Stream.of(5, 10)` elimina todos los elementos, dejando el Optional vacío, por lo que `get()` lanza excepción (F correcta). Ver Capítulo 10.

**25. C, D.** `Music` no compila (variables de instancia no permitidas en record fuera de la cabecera). `Song` no compila (constructor compacto no puede asignar variables de instancia explícitamente). `Dance` compila (clases selladas en mismo archivo pueden omitir `permits` si las subclases están ahí, pero aquí `Ballet` extiende `Dance` explícitamente). `March` compila (`switch` con patrones). `Ballet` no compila (constructor normal en clase, falta nombre del método o es constructor malformado, los records tienen constructor compacto, las clases no). `NewDance` no compila (debe ser `final`, `sealed` o `non-sealed` al extender una clase sellada). Opciones C y D son las que compilan. Ver Capítulo 7.

**26. B, D.** A no compila (resultado double a int). B compila (long a double implícito). C no compila (falta el `:` del operador ternario). D compila (cast explícito). E no compila (`L` con decimal). F no compila (guion bajo junto a punto decimal). Ver Capítulo 2.

**27. F.** El código compila. El ejecutor tiene un solo hilo, pero la barrera (`CyclicBarrier`) espera 3 partes. La primera tarea ocupará el único hilo y esperará en la barrera. Las otras tareas nunca comenzarán porque el hilo está ocupado. El programa se bloquea (deadlock). Opción F correcta. Ver Capítulo 13.

**28. F.** Línea 5: `FileNotFoundException` (L12) es chequeada y no se declara ni captura. Línea 7: `StringBuilder` no es `AutoCloseable`, no se puede usar en try-with-resources. Línea 10: `RuntimeException` es subclase de `Exception`, no se pueden poner juntas en multi-catch (redundante). 3 errores. Opción F correcta. Ver Capítulo 11.