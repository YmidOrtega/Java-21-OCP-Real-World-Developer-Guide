Las respuestas a las preguntas de revisión del capítulo se pueden encontrar al final del capitulo.

**1.** ¿Cuál es el resultado de la siguiente clase?

```Java
1:  import java.util.function.*;
2:
3:  public class Panda {
4:      int age;
5:      public static void main(String[] args) {
6:          Panda p1 = new Panda();
7:          p1.age = 1;
8:          check(p1, p -> p.age < 5);
9:      }
10:     private static void check(Panda panda,
11:         Predicate<Panda> pred) {
12:         String result =
13:             pred.test(panda) ? "match" : "not match";
14:         System.out.print(result);
15: } }
```

- [x] A. match
- [ ] B. not match
- [ ] C. Error de compilación en la línea 8
- [ ] D. Error de compilación en las líneas 10 y 11
- [ ] E. Error de compilación en las líneas 12 y 13
- [ ] F. Una excepción en tiempo de ejecución

**2.** ¿Cuál es el resultado del siguiente código?

```Java
1:  interface Climb {
2:      boolean isTooHigh(int height, int limit);
3:  }
4:
5:  public class Climber {
6:      public static void main(String[] args) {
7:          check((h, m) -> h.append(m).isEmpty(), 5);
8:      }
9:      private static void check(Climb climb, int height) {
10:         if (climb.isTooHigh(height, 10))
11:             System.out.println("too high");
12:         else
13:             System.out.println("ok");
14:     }
15: }
```

- [ ] A. ok
- [ ] B. too high
- [ ] C. Error de compilación en la línea 7
- [x] D. Error de compilación en la línea 10
- [ ] E. Error de compilación en una línea diferente
- [ ] F. Se lanza una excepción en tiempo de ejecución

**3.** ¿Cuáles afirmaciones sobre las interfaces funcionales son verdaderas? (_Se deben seleccionar todas las opciones que apliquen_)

- [x] A. Una interfaz funcional puede contener métodos `default` y `private`.
- [ ] B. Una interfaz funcional puede definirse como una clase o una interfaz.
- [x] C. Los métodos abstractos con firmas contenidas en métodos `public` de `java.lang.Object` no cuentan para el conteo de métodos abstractos de una interfaz funcional.
- [ ] D. Una interfaz funcional no puede contener métodos `static` o `private static`.
- [ ] E. Una interfaz funcional debe estar marcada con la anotación `@FunctionalInterface`.

**4.** ¿Cuál lambda puede reemplazar a la clase `MySecret` para devolver el mismo valor? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
interface Secret {
	String magic(double d);
}

class MySecret implements Secret { public String magic(double d) {
	return "Poof";
	} 
}
```

- [ ] A. `(e) -> "Poof"`
- [x] B. `(e) -> {"Poof"}`
- [ ] C. `(e) -> { String e = ""; "Poof" }`
- [x] D. `(e) -> { String e = ""; return "Poof"; }`
- [ ] E. `(e) -> { String e = ""; return "Poof" }`
- [x] F. `(e) -> { String f = ""; return "Poof"; }`

**5.** ¿Cuáles de las siguientes interfaces funcionales contienen un método abstracto que devuelve un valor primitivo? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. `BooleanSupplier`
- [x] B. `CharSupplier`
- [ ] C. `DoubleSupplier`
- [x] D. `FloatSupplier`
- [x] E. `IntSupplier`
- [ ] F. `StringSupplier`

**6.** ¿Cuáles de las siguientes expresiones lambda pueden pasarse a una función de tipo `Predicate<String>`? (_Se deben seleccionar todas las opciones que apliquen_)

- [x] A. `s -> s.isEmpty()`
- [ ] B. `s --> s.isEmpty()`
- [x] C. `(String s) -> s.isEmpty()`
- [ ] D. `(String s) --> s.isEmpty()`
- [x] E. `(StringBuilder s) -> s.isEmpty()`
- [ ] F. `(StringBuilder s) --> s.isEmpty()`

**7.** ¿Cuál de estas afirmaciones es verdadera sobre el siguiente código?

```Java
public void method() {
    x((var x) -> {}, (var x, var y) -> false);
}
public void x(Consumer<String> x, BinaryOperator<Boolean> y) {}
```

- [ ] A. El código no compila por una de las variables llamadas `x`.
- [ ] B. El código no compila por una de las variables llamadas `y`.
- [ ] C. El código no compila por otra razón.
- [ ] D. El código compila y el `x` en cada lambda hace referencia al mismo tipo.
- [x] E. El código compila y el `x` en cada lambda hace referencia a un tipo diferente.

**8.** ¿Cuál de los siguientes es equivalente a este código?

```Java
UnaryOperator<Integer> u = x -> x * x;
```

- [ ] A. `BiFunction<Integer> f = x -> x*x;`
- [x] B. `BiFunction<Integer, Integer> f = x -> x*x;`
- [ ] C. `BinaryOperator<Integer, Integer> f = x -> x*x;`
- [ ] D. `Function<Integer> f = x -> x*x;`
- [x] E. `Function<Integer, Integer> f = x -> x*x;`
- [ ] F. Ninguna de las anteriores

**9.** ¿Cuáles afirmaciones son verdaderas? (_Se deben seleccionar todas las opciones que apliquen_)

- [x] A. La interfaz `Consumer` es buena para imprimir un valor existente.
- [ ] B. La interfaz `Supplier` es buena para imprimir un valor existente.
- [x] C. La interfaz `IntegerSupplier` devuelve un `int`.
- [ ] D. La interfaz `Predicate` devuelve un `int`.
- [ ] E. La interfaz `Function` tiene un método llamado `test()`.
- [x] F. La interfaz `Predicate` tiene un método llamado `test()`.

**10.** ¿Cuáles de los siguientes pueden insertarse sin causar un error de compilación? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
public void remove(List<Character> chars) {
    char end = 'z';
    Predicate<Character> predicate = c -> {
        char start = 'a'; return start <= c && c <= end; };
    // INSERTAR LÍNEA AQUÍ
}
```

- [x] A. `char start = 'a';`
- [ ] B. `char c = 'x';`
- [x] C. `chars = null;`
- [ ] D. `end = '1';`
- [ ] E. Ninguna de las anteriores

**11.** ¿Cuántas veces **se imprime** `true` con este código?

```Java
import java.util.function.Predicate;
public class Fantasy {
    public static void scary(String animal) {
        var dino = s -> "dino".equals(animal);
        var dragon = s -> "dragon".equals(animal);
        var combined = dino.or(dragon);
        System.out.println(combined.test(animal));
    }
    public static void main(String[] args) {
        scary("dino");
        scary("dragon");
        scary("unicorn");
    }
}
```

- [ ] A. Una.
- [ ] B. Dos.
- [ ] C. Tres.
- [ ] D. El código no compila.
- [x] E. Se lanza una excepción en tiempo de ejecución.

**12.** ¿Qué imprime el siguiente código?

```Java
Function<Integer, Integer> s = a -> a + 4;
Function<Integer, Integer> t = a -> a * 3;
Function<Integer, Integer> c = s.compose(t);
System.out.print(c.apply(1));
```

- [x] A. 7
- [ ] B. 15
- [ ] C. El código no compila por los tipos de datos en las expresiones lambda.
- [ ] D. El código no compila por la llamada a `compose()`.
- [ ] E. El código no compila por otra razón.

**13.** ¿Cuál es verdadero sobre el siguiente código?

```Java
int length = 3;

for (int i = 0; i<3; i++) {
    if (i%2 == 0) {
        Supplier<Integer> supplier = () -> length; // A
        System.out.println(supplier.get());        // B
    } else {
        int j = i;
        Supplier<Integer> supplier = () -> j;      // C
        System.out.println(supplier.get());        // D
    }
}
```

- [ ] A. El primer error de compilación está en la línea A.
- [ ] B. El primer error de compilación está en la línea B.
- [x] C. El primer error de compilación está en la línea C.
- [ ] D. El primer error de compilación está en la línea D.
- [ ] E. El código compila correctamente.

**14.** ¿Cuáles de las siguientes son expresiones lambda válidas? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. `(Wolf w, var c) -> 39`
- [x] B. `(final Camel c) -> {}`
- [ ] C. `(a,b,c) -> {int b = 3; return 2;}`
- [x] D. `(x,y) -> new RuntimeException()`
- [x] E. `(var y) -> return 0;`
- [ ] F. `() -> {float r}`
- [ ] G. `(Cat a, b) -> {}`

**15.** ¿Qué expresión lambda, cuando **se introduce** en la línea en blanco del siguiente código, hace que el programa imprima `hahaha`? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
import java.util.function.Predicate;
public class Hyena {
    private int age = 1;
    public static void main(String[] args) {
        var p = new Hyena();
        double height = 10;
        int age = 1;
        testLaugh(p,    ________________);
        age = 2;
    }
    static void testLaugh(Hyena panda, Predicate<Hyena> joke) {
        var r = joke.test(panda) ? "hahaha" : "silence";
        System.out.print(r);
    }
}
```

- [ ] A. `var -> p.age <= 10`
- [ ] B. `shenzi -> age==1`
- [x] C. `p -> true`
- [ ] D. `age==1`
- [ ] E. `shenzi -> age==2`
- [ ] F. `h -> h.age < 5`
- [ ] G. Ninguna de las anteriores, ya que el código no compila

**16.** ¿Cuál de los siguientes puede insertarse sin causar un error de compilación?

```Java
public void remove(List<Character> chars) {
    char end = 'z';

    // INSERTAR LÍNEA AQUÍ

    Predicate<Character> predicate = c -> {
        char start = 'a'; return start <= c && c <= end; };
}
```

- [ ] A. `char start = 'a';`
- [ ] B. `char c = 'x';`
- [x] C. `chars = null;`
- [ ] D. `end = '1';`
- [ ] E. Ninguna de las anteriores

**17.** ¿Cuál es el resultado de ejecutar la siguiente clase?

```Java
1:  import java.util.function.*;
2:
3:  public class Panda {
4:      int age;
5:      public static void main(String[] args) {
6:          Panda p1 = new Panda();
7:          p1.age = 1;
8:          check(p1, p -> {p.age < 5});
9:      }
10:     private static void check(Panda panda,
11:         Predicate<Panda> pred) {
12:         String result = pred.test(panda)
13:             ? "match" : "not match";
14:         System.out.print(result);
15: } }
```

- [x] A. match
- [ ] B. not match
- [ ] C. Error de compilación en la línea 8
- [ ] D. Error de compilación en la línea 10
- [ ] E. Error de compilación en la línea 12
- [ ] F. Se lanza una excepción en tiempo de ejecución

**18.** ¿Qué interfaces funcionales completan el siguiente código? Para la línea 7, asuma que `m` y `n` son instancias de interfaces funcionales que existen y tienen el mismo tipo que `y`. (_Seleccione tres_)

```Java
6:   ______________ x = String::new;
7:   ______________ y = m.andThen(n);
8:   ______________ z = a -> a + a;
```

- [ ] A. `BinaryConsumer<String, String>`
- [x] B. `BiConsumer<String, String>`
- [ ] C. `BinaryFunction<String, String>`
- [x] D. `BiFunction<String, String>`
- [ ] E. `Predicate<String>`
- [ ] F. `Supplier<String>`
- [ ] G. `UnaryOperator<String>`
- [x] H. `UnaryOperator<String, String>`

**19.** ¿Cuál de los siguientes compila e imprime todo el conjunto?

```Java
Set<?> set = Set.of("lion", "tiger", "bear");
var s = Set.copyOf(set);
Consumer<Object> consumer = ________________;
s.forEach(consumer);
```

- [ ] A. `() -> System.out.println(s)`
- [ ] B. `s -> System.out.println(s)`
- [ ] C. `(s) -> System.out.println(s)`
- [ ] D. `System.out.println(s)`
- [ ] E. `System::out::println`
- [x] F. `System.out::println`

**20.** ¿Qué lambda puede reemplazar la llamada `new Sloth()` en el método `main()` y producir la misma salida en tiempo de ejecución?

```Java
import java.util.List;
interface Yawn {
    String yawn(double d, List<Integer> time);
}
class Sloth implements Yawn {
    public String yawn(double zzz, List<Integer> time) {
        return "Sleep: " + zzz;
    } }
public class Vet {
    public static String takeNap(Yawn y) {
        return y.yawn(10, null);
    }
    public static void main(String… unused) {
        System.out.print(takeNap(new Sloth()));
    } }
```

- [ ] A. `(z,f) -> { String x = ""; return "Sleep: " + x }`
- [x] B. `(t,s) -> { String t = ""; return "Sleep: " + t; }`
- [ ] C. `(w,q) -> {"Sleep: " + w}`
- [ ] D. `(e,u) -> { String g = ""; "Sleep: " + e }`
- [ ] E. `(a,b) -> "Sleep: " + (double)(b==null ? a : a)`
- [ ] F. `(r,k) -> { String g = ""; return "Sleep:"; }`

**21.** ¿Cuáles de las siguientes son interfaces funcionales válidas? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
public interface Transport {
    public int go();
    public boolean equals(Object o);
}

public abstract class Car {
    public abstract Object swim(double speed, int duration);
}

public interface Locomotive extends Train {
    public int getSpeed();
}

public interface Train extends Transport {}

abstract interface Spaceship extends Transport {
    default int blastOff();
}

public interface Boat {
    int hashCode();
    int hashCode(String input);
}
```

- [x] A. `Boat`
- [ ] B. `Car`
- [ ] C. `Locomotive`
- [ ] D. `Spaceship`
- [x] E. `Transport`
- [x] F. `Train`
- [ ] G. Ninguna de las anteriores
