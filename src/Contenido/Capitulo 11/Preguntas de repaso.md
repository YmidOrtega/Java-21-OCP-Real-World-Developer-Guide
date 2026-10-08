Las respuestas a las preguntas de repaso del capítulo **se pueden** encontrar en el Apéndice.

**1.** ¿Cuál de los siguientes **puede** insertarse en la línea 8 para que este código compile? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
7: public void whatHappensNext() throws IOException {
8:     // INSERTAR CÓDIGO AQUÍ
9: }
```

- [ ] A. `System.out.println("está bien");`
- [ ] B. `throw new Exception();`
- [ ] C. `throw new IllegalArgumentException();`
- [ ] D. `throw new java.io.IOException();`
- [ ] E. `throw new RuntimeException();`
- [ ] F. Ninguna de las anteriores.

---

**2.** ¿Cuál afirmación sobre la siguiente clase es correcta?

```Java
1:  class Problem extends Exception {
2:      public Problem() {}
3:  }
4:  class YesProblem extends Problem {}
5:  public class MyDatabase {
6:      public static void connectToDatabase() throw Problem {
7:          throws new YesProblem();
8:      }
9:      public static void main(String[] c) throw Exception {
10:         connectToDatabase();
11:     }
12: }
```

- [ ] A. El código compila e imprime un stack trace para `YesProblem` en tiempo de ejecución.
- [ ] B. El código compila e imprime un stack trace para `Problem` en tiempo de ejecución.
- [ ] C. El código no compila porque `Problem` define un constructor.
- [ ] D. El código no compila porque `YesProblem` no define un constructor.
- [ ] E. El código no compila pero lo haría si `Problem` y `YesProblem` **se intercambiaran** en las líneas 6 y 7.
- [ ] F. Ninguna de las anteriores.

---

**3.** ¿Cuáles de los siguientes son tipos comunes a localizar? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. Fechas
- [ ] B. Expresiones lambda
- [ ] C. Nombres de clase
- [ ] D. Moneda
- [ ] E. Números
- [ ] F. Nombres de variables

---

**4.** ¿Cuál es la salida del siguiente fragmento, asumiendo que `a` y `b` son ambos `0`?

```Java
3:  try {
4:      System.out.print(a / b);
5:  } catch (RuntimeException e) {
6:      System.out.print(-1);
7:  } catch (ArithmeticException e) {
8:      System.out.print(0);
9:  } finally {
10:     System.out.print("done");
11: }
```

- [ ] A. `-1`
- [ ] B. `0`
- [ ] C. `done-1`
- [ ] D. `done0`
- [ ] E. El código no compila.
- [ ] F. **Se lanza** una excepción no atrapada.
- [ ] G. Ninguna de las anteriores.

---

**5.** Asumiendo que el local actual usa dólares (`$`) y el siguiente método **es llamado** con un valor `double` de `100_102.2`, ¿cuáles de los siguientes valores **se imprimen**? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
public void print(double t) {
    System.out.print(NumberFormat.getCompactNumberInstance()
        .format(t));
    System.out.print(
        NumberFormat.getCompactNumberInstance(
            Locale.getDefault(), Style.SHORT).format(t));
    System.out.print(NumberFormat.getCurrencyInstance().format(t));
}
```

- [ ] A. `100`
- [ ] B. `$100,000.00`
- [ ] C. `100K`
- [ ] D. `100 thousand`
- [ ] E. `100M`
- [ ] F. `$100,102.20`
- [ ] G. Ninguna de las anteriores.

---

**6.** ¿Cuál es la salida del siguiente código?

```Java
LocalDate date = LocalDate.parse("2025-04-30",
    DateTimeFormatter.ISO_LOCAL_DATE_TIME);
System.out.println(date.getYear() + " "
    + date.getMonth() + " " + date.getDayOfMonth());
```

- [ ] A. `2025 APRIL 2`
- [ ] B. `2025 APRIL 30`
- [ ] C. `2025 MAY 2`
- [ ] D. El código no compila.
- [ ] E. **Se lanza** una excepción en tiempo de ejecución.

---

**7.** ¿Qué **imprime** el siguiente método?

```Java
11: public void tryAgain(String s) {
12:     try (FileReader r = null, p = new FileReader("")) {
13:         System.out.print("X");
14:         throw new IllegalArgumentException();
15:     } catch (Exception s) {
16:         System.out.print("A");
17:         throw new FileNotFoundException();
18:     } finally {
19:         System.out.print("O");
20:     }
21: }
```

- [ ] A. `XAO`
- [ ] B. `XOA`
- [ ] C. Una línea de este método contiene un error del compilador.
- [ ] D. Dos líneas de este método contienen errores del compilador.
- [ ] E. Tres o más líneas de este método contienen errores del compilador.
- [ ] F. El código compila, pero **se lanza** una `NullPointerException` en tiempo de ejecución.
- [ ] G. Ninguna de las anteriores.

---

**8.** Asumiendo que todos los archivos mencionados en las opciones de respuesta existen y definen las mismas claves. ¿Cuál **se usará** para encontrar la clave en la línea 8?

```Java
6: Locale.setDefault(Locale.of("en", "US"));
7: var b = ResourceBundle.getBundle("Dolphins");
8: System.out.println(b.getString("name"));
```

- [ ] A. `Dolphins.properties`
- [ ] B. `Dolphins_US.properties`
- [ ] C. `Dolphins_en.properties`
- [ ] D. `Whales.properties`
- [ ] E. `Whales_en_US.properties`
- [ ] F. El código no compila.

---

**9.** ¿Para qué valor de `pattern` imprimirá lo siguiente `<005.21> <008.49> <1,234.0>`?

```Java
String pattern = "________________";
var message = DoubleStream.of(5.21, 8.49, 1234)
    .mapToObj(v -> new DecimalFormat(pattern).format(v))
    .collect(Collectors.joining("> <"));
System.out.println("<" + message + ">");
```

- [ ] A. `##.#`
- [ ] B. `0,000.0#`
- [ ] C. `#,###.0`
- [ ] D. `#,###,000.0#`
- [ ] E. El código no compila independientemente de lo que **se coloque** en el espacio en blanco.

---

**10.** ¿Qué escenario es el mejor uso de una excepción?

- [ ] A. Un elemento no **se encuentra** al buscar en una lista.
- [ ] B. Un parámetro inesperado **es pasado** a un método.
- [ ] C. La computadora se incendió.
- [ ] D. **Se quiere** recorrer una lista.
- [ ] E. No **se sabe** cómo codificar un método.

---

**11.** ¿Cuáles de las siguientes excepciones deben **ser manejadas** o declaradas en el método donde **se lanzan**? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
class Apple extends RuntimeException {}
class Orange extends Exception {}
class Banana extends Error {}
class Pear extends Apple {}
class Tomato extends Orange {}
class Peach extends Throwable {}
```

- [ ] A. `Apple`
- [ ] B. `Orange`
- [ ] C. `Banana`
- [ ] D. `Pear`
- [ ] E. `Tomato`
- [ ] F. `Peach`

---

**12.** ¿Cuáles de los siguientes cambios, **hechos** de forma independiente, **harían** que este código compile? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
1:  import java.io.*;
2:  public class StuckTurkeyCage implements AutoCloseable {
3:      public void close() throws IOException {
4:          throw new FileNotFoundException("Cage not closed");
5:      }
6:      public static void main(String[] args) {
7:          try (StuckTurkeyCage t = new StuckTurkeyCage()) {
8:              System.out.println("put turkeys in");
9:          }
10:     }
11: }
```

- [ ] A. Eliminar `throws IOException` de la declaración en la línea 3.
- [ ] B. Agregar `throws Exception` a la declaración en la línea 6.
- [ ] C. Cambiar la línea 9 a `} catch (Exception e) {}`.
- [ ] D. Cambiar la línea 9 a `} finally {}`.
- [ ] E. El código compila tal como está.
- [ ] F. Ninguna de las anteriores.

---

**13.** ¿Cuáles de las siguientes afirmaciones sobre el manejo de excepciones en Java son verdaderas? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. Una sentencia `try` tradicional sin un bloque `catch` requiere un bloque `finally`.
- [ ] B. Una sentencia `try` tradicional sin un bloque `finally` requiere un bloque `catch`.
- [ ] C. Una sentencia `try` tradicional con solo una sentencia puede omitir las llaves `{}`.
- [ ] D. Una sentencia `try-with-resources` sin un bloque `catch` requiere un bloque `finally`.
- [ ] E. Una sentencia `try-with-resources` sin un bloque `finally` requiere un bloque `catch`.
- [ ] F. Una sentencia `try-with-resources` con solo una sentencia puede omitir las llaves `{}`.

---

**14.** Asumiendo que `-g:vars` **se usa** cuando el código **se compila** para incluir información de depuración, ¿cuál es la salida del siguiente fragmento?

```Java
var huey = (String)null;
Integer dewey = null;
Object louie = null;
if(louie == huey.substring(dewey.intValue())) {
    System.out.println("Quack!");
}
```

- [ ] A. Una `NullPointerException` que no incluye nombres de variables en el stack trace.
- [ ] B. Una `NullPointerException` nombrando a `huey` en el stack trace.
- [ ] C. Una `NullPointerException` nombrando a `dewey` en el stack trace.
- [ ] D. Una `NullPointerException` nombrando a `louie` en el stack trace.
- [ ] E. Una `NullPointerException` nombrando a `huey` y `louie` en el stack trace.
- [ ] F. Una `NullPointerException` nombrando a `huey` y `dewey` en el stack trace.
- [ ] G. Ninguna de las anteriores.

---

**15.** ¿Cuáles de las siguientes opciones, **insertadas** de forma independiente en el espacio en blanco, usan parámetros de locale con el formato correcto? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
import java.util.Locale;
public class ReadMap implements AutoCloseable {
    private Locale locale;
    private boolean closed = false;
    @Override public void close() {
        System.out.println("Folding map");
        locale = null;
        closed = true;
    }
    public void open() {
        this.locale = ____________;
    }
    public void use() {
        // Implementation omitted
    }
}
```

- [ ] A. `Locale.of("xM")`
- [ ] B. `Locale.of("MQ", "ks")`
- [ ] C. `Locale.of("qw")`
- [ ] D. `Locale.of("wp", "VW")`
- [ ] E. `Locale.create("zp")`
- [ ] F. `new Locale.Builder().setLanguage("yw").setRegion("PM")`
- [ ] G. El código no compila independientemente de lo que **se coloque** en el espacio en blanco.

---

**16.** ¿Cuál de las siguientes opciones puede **insertarse** en el espacio en blanco para permitir que el código compile y se ejecute sin lanzar una excepción?

```Java
var f = DateTimeFormatter.ofPattern("hh o'clock");
System.out.println(f.format(___________________.now()));
```

- [ ] A. `ZonedTime`
- [ ] B. `LocalDate`
- [ ] C. `LocalTimestamp`
- [ ] D. `LocalTime`
- [ ] E. El código no compila independientemente de lo que **se coloque** en el espacio en blanco.
- [ ] F. Ninguna de las anteriores.

---

**17.** ¿Cuáles de las siguientes afirmaciones sobre paquetes de recursos son correctas? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. Todas las claves deben **estar** en el mismo paquete de recursos para **ser usadas**.
- [ ] B. Un paquete de recursos **se carga** llamando al constructor `new ResourceBundle()`.
- [ ] C. Los valores del paquete de recursos siempre **se leen** usando la clase `Properties`.
- [ ] D. Cambiar el locale predeterminado dura solo para una ejecución del programa.
- [ ] E. Si **se solicita** un paquete de recursos para un locale específico, el paquete de recursos del locale predeterminado no **se usará**.
- [ ] F. Es posible **usar** un paquete de recursos para un locale sin **especificar** un locale predeterminado.

---

**18.** ¿Cuál es la salida del siguiente código?

```Java
import java.io.*;
public class FamilyCar {
    static class Door implements AutoCloseable {
        public void close() {
            System.out.print("D");
        }
    }
    static class Window implements Closeable {
        public void close() {
            System.out.print("W");
            throw new RuntimeException();
        }
    }
    public static void main(String[] args) {
        var d = new Door();
        try (d; var w = new Window()) {
            System.out.print("T");
        } catch (Exception e) {
            System.out.print("E");
        } finally {
            System.out.print("F");
        }
    }
}
```

- [ ] A. `TWF`
- [ ] B. `TWDF`
- [ ] C. `TWDEF`
- [ ] D. `TWF` seguido de una excepción
- [ ] E. `TWDF` seguido de una excepción
- [ ] F. `TWEF` seguido de una excepción
- [ ] G. El código no compila.

---

**19.** Suponiendo que **tenemos** los siguientes tres archivos de propiedades y código. ¿Qué paquetes **se usan** en las líneas 8 y 9, respectivamente?

```
Dolphins.properties
name=The Dolphin
age=0

Dolphins_en.properties
name=Dolly
age=4

Dolphins_fr.properties
name=Dolly
```

```Java
5:  var fr = Locale.of("fr");
6:  Locale.setDefault(Locale.of("en", "US"));
7:  var b = ResourceBundle.getBundle("Dolphins", fr);
8:  b.getString("name");
9:  b.getString("age");
```

- [ ] A. `Dolphins.properties` y `Dolphins.properties`
- [ ] B. `Dolphins.properties` y `Dolphins_en.properties`
- [ ] C. `Dolphins_en.properties` y `Dolphins_en.properties`
- [ ] D. `Dolphins_fr.properties` y `Dolphins.properties`
- [ ] E. `Dolphins_fr.properties` y `Dolphins_en.properties`
- [ ] F. El código no compila.
- [ ] G. Ninguna de las anteriores.

---

**20.** ¿Qué **imprime** el siguiente programa?

```Java
1:  public class DriveBus {
2:      public void go() {
3:          System.out.print("A");
4:          try {
5:              stop();
6:          } catch (ArithmeticException e) {
7:              System.out.print("B");
8:          } finally {
9:              System.out.print("C");
10:         }
11:         System.out.print("D");
12:     }
13:     public void stop() {
14:         System.out.print("E");
15:         Object x = null;
16:         x.toString();
17:         System.out.print("F");
18:     }
19:     public static void main(String n[]) {
20:         new DriveBus().go();
21:     }
22: }
```

- [ ] A. `AE`
- [ ] B. `AEBCD`
- [ ] C. `AEC`
- [ ] D. `AECD`
- [ ] E. `AE` seguido de un stack trace
- [ ] F. `AEBCD` seguido de un stack trace
- [ ] G. `AEC` seguido de un stack trace
- [ ] H. Un stack trace sin otra salida

---

**21.** ¿Qué cambio permite que el siguiente programa compile?

```Java
1:  public class AhChoo {
2:      static class SneezeException extends Exception {}
3:      static class SniffleException extends SneezeException {}
4:      public static void main(String[] args) {
5:          try {
6:              throw new SneezeException();
7:          } catch (SneezeException | SniffleException e) {
8:          } finally {}
9:      }
10: }
```

- [ ] A. Agregar `throws SneezeException` a la declaración en la línea 4.
- [ ] B. Agregar `throws Throwable` a la declaración en la línea 4.
- [ ] C. Cambiar la línea 7 a `} catch (SneezeException e) {`.
- [ ] D. Cambiar la línea 7 a `} catch (SniffleException e) {`.
- [ ] E. Eliminar la línea 7.
- [ ] F. El código compila correctamente tal como está.
- [ ] G. Ninguna de las anteriores.

---

**22.** ¿Cuál es la salida del siguiente código?

```Java
try {
    LocalDateTime book = LocalDateTime.of(2025, 4, 5, 12, 30, 20);
    System.out.print(book.format(DateTimeFormatter.ofPattern("m")));
    System.out.print(book.format(DateTimeFormatter.ofPattern("z")));
    System.out.print(DateTimeFormatter.ofPattern("y").format(book));
} catch (Throwable e) {}
```

- [ ] A. `4`
- [ ] B. `30`
- [ ] C. `402`
- [ ] D. `3002`
- [ ] E. `3002025`
- [ ] F. `402025`
- [ ] G. Ninguna de las anteriores.

---

**23.** Complete el espacio en blanco: Una clase que implementa _________________ puede **usarse** en una sentencia `try-with-resources`. (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. `AutoCloseable`
- [ ] B. `Resource`
- [ ] C. `Exception`
- [ ] D. `AutomaticResource`
- [ ] E. `Closeable`
- [ ] F. `RuntimeException`
- [ ] G. `Serializable`

---

**24.** ¿Cuál es la salida del siguiente programa?

```Java
public class SnowStorm {
    static class WalkToSchool implements AutoCloseable {
        public void close() {
            throw new RuntimeException("flurry");
        }
    }
    public static void main(String[] args) {
        WalkToSchool walk1 = new WalkToSchool();
        try (walk1; WalkToSchool walk2 = new WalkToSchool()) {
            throw new RuntimeException("blizzard");
        } catch(Exception e) {
            System.out.println(e.getMessage()
                + " " + e.getSuppressed().length);
        }
        walk1 = null;
    }
}
```

- [ ] A. `blizzard 0`
- [ ] B. `blizzard 1`
- [ ] C. `blizzard 2`
- [ ] D. `flurry 0`
- [ ] E. `flurry 1`
- [ ] F. `flurry 2`
- [ ] G. Ninguna de las anteriores.

---

**25.** Asumiendo que la moneda de EE.UU. está en dólares ($) y la moneda alemana está en euros (€), ¿cuál es la salida del siguiente programa?

```Java
import java.text.NumberFormat;
import java.util.Locale;
import java.util.Locale.Category;
public record Wallet(double money) {
    private String openWallet() {
        Locale.setDefault(Category.DISPLAY,
            new Locale.Builder().setRegion("us").build());
        Locale.setDefault(Category.FORMAT,
            new Locale.Builder().setLanguage("en").build());
        return
            NumberFormat.getCurrencyInstance(Locale.GERMANY)
                .format(money);
    }
    public void printBalance() {
        System.out.println(openWallet());
    }
    public static void main(String... unused) {
        new Wallet(2.4).printBalance();
    }
}
```

- [ ] A. `2,40 €`
- [ ] B. `$2.40`
- [ ] C. `2.4`
- [ ] D. El código no compila.
- [ ] E. Ninguna de las anteriores.

---

**26.** ¿Qué líneas pueden **llenar** el espacio en blanco para que el siguiente código compile? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
void rollOut() throws ClassCastException {}
public void transform(String c) {
    try {
        rollOut();
    } catch (IllegalArgumentException
        |________________________) {
    }
}
```

- [ ] A. `IOException a`
- [ ] B. `Error b`
- [ ] C. `NullPointerException c`
- [ ] D. `RuntimeException d`
- [ ] E. `NumberFormatException e`
- [ ] F. `ClassCastException f`
- [ ] G. Ninguna de las anteriores. El código contiene un error del compilador independientemente de lo que **se inserte** en el espacio en blanco.

---

**Ver también:** [[Contenido/Capitulo 11/Aspectos esenciales del examen]] | [[Contenido/Capitulo 11/Resumen]]
