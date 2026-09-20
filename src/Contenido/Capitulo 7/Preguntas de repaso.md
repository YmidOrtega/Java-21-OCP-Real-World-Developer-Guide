Las respuestas a las preguntas de revisión del capítulo se pueden encontrar al final del capitulo.

**1. ¿Cuáles de las siguientes son declaraciones de `record` válidas?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
public record Iguana(int age) {
    private static final int age = 10; }

public final record Gecko() {}

public abstract record Chameleon()  {
    private static String name; }

public record BeardedDragon(boolean fun) {
    @Override public boolean fun() { return false; } }

public record Reptile(long size) {
    public Reptile {
        if(size == 1) throw new IllegalArgumentException();
    } }

public record Newt(double age) extends Reptile {
    public Newt(double age) {
        age = this.age % 2 == 0 ? 5 : 10;
    } }
```

- [ ]  A. `Iguana`
- [ ]  B. `Gecko`
- [ ]  C. `Chameleon`
- [ ]  D. `BeardedDragon`
- [ ]  E. `Reptile`
- [ ]  F. `Newt`
- [ ]  G. Ninguna de las anteriores

**2. ¿Cuáles de las siguientes instrucciones se pueden insertar en la línea en blanco para que el código compile exitosamente?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
interface CanHop {}
public class Frog implements CanHop {
    public static void main(String[] args) {
        ____________ frog = new TurtleFrog();
    }
}

class BrazilianHornedFrog extends Frog {}
class TurtleFrog extends Frog {}
```

- [ ]  A. `Frog`
- [ ]  B. `TurtleFrog`
- [ ]  C. `BrazilianHornedFrog`
- [ ]  D. `CanHop`
- [ ]  E. `var`
- [ ]  F. `Long`
- [ ]  G. Ninguna de las anteriores; el código contiene un error de compilación.

**3. ¿Cuál es el resultado del siguiente programa?**

```Java
11: public class Favorites {
12:     enum Flavors {
13:         VANILLA, CHOCOLATE, STRAWBERRY
14:         public Flavors() {}
15:     }
16:     public static void main(String[] args) {
17:         for(final var e : Flavors.values())
18:             System.out.print((e.ordinal() % 2) + " ");
19:     } }
```

- [ ]  A. `0 1 0`
- [ ]  B. `1 0 1`
- [ ]  C. Exactamente una línea de código no compila.
- [ ]  D. Más de una línea de código no compila.
- [ ]  E. El código compila pero produce una excepción en tiempo de ejecución.
- [ ]  F. Ninguna de las anteriores.

**4. ¿Cuál es la salida del siguiente programa?**

```Java
public sealed class ArmoredAnimal permits Armadillo {
    public ArmoredAnimal(int size) {}
    @Override public String toString() { return "Strong"; }
    public static void main(String[] a) {
        var c = new Armadillo(10, null);
        System.out.println(c);
    }
}
class Armadillo extends ArmoredAnimal {
    @Override public String toString() { return "Cute"; }
    public Armadillo(int size, String name) {
        super(size);
    }
}
```

- [ ]  A. `Strong`
- [ ]  B. `Cute`
- [ ]  C. El programa no compila.
- [ ]  D. El código compila pero produce una excepción en tiempo de ejecución.
- [ ]  E. Ninguna de las anteriores.

**5. ¿Cuál afirmación sobre el siguiente programa es correcta?**

```Java
1:  interface HasExoskeleton {
2:      double size = 2.0f;
3:      abstract int getNumberOfSections();
4:  }
5:  abstract class Insect implements HasExoskeleton {
6:      abstract int getNumberOfLegs();
7:  }
8:  public class Beetle extends Insect {
9:      int getNumberOfLegs() { return 6; }
10:     int getNumberOfSections(int count) { return 1; }
11: }
```

- [ ]  A. Compila sin problema.
- [ ]  B. El código producirá una `ClassCastException` si se llama en tiempo de ejecución.
- [ ]  C. El código no compilará a causa de la línea 2.
- [ ]  D. El código no compilará a causa de la línea 5.
- [ ]  E. El código no compilará a causa de la línea 8.
- [ ]  F. El código no compilará a causa de la línea 10.

**6. ¿Cuáles afirmaciones sobre el siguiente programa son correctas?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
1: public abstract interface Herbivore {
2:     int amount = 10;
3:     public void eatGrass();
4:     public abstract int chew() { return 13; }
5: }
6:
7: abstract class IsAPlant extends Herbivore {
8:     Object eatGrass(int season) { return null; }
9: }
```

- [ ]  A. Compila y se ejecuta sin problema.
- [ ]  B. El código no compilará a causa de la línea 1.
- [ ]  C. El código no compilará a causa de la línea 2.
- [ ]  D. El código no compilará a causa de la línea 4.
- [ ]  E. El código no compilará a causa de la línea 7.
- [ ]  F. El código no compilará porque la línea 8 contiene una sobrescritura de método inválida.

**7. ¿Cuál es la salida del siguiente programa?**

```Java
1: interface Aquatic {
2:     int getNumOfGills(int p);
3: }
4: public class ClownFish implements Aquatic {
5:     String getNumOfGills() { return "14"; }
6:     int getNumOfGills(int input) { return 15; }
7:     public static void main(String[] args) {
8:         System.out.println(new
9: ClownFish().getNumOfGills(-1));
9: } }
```

- [ ]  A. `14`
- [ ]  B. `15`
- [ ]  C. El código no compilará a causa de la línea 4.
- [ ]  D. El código no compilará a causa de la línea 5.
- [ ]  E. El código no compilará a causa de la línea 6.
- [ ]  F. Ninguna de las anteriores.

**8. Dado lo siguiente, selecciona las instrucciones que se pueden insertar en la línea en blanco para que el código compile e imprima `true` en tiempo de ejecución?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
record Walrus(List<String> diet) {}
record Exhibit(Walrus animal, String location) {}

var e = new Exhibit(new Walrus(List.of("Wally")), "Artic");
System.out.print(e instanceof ____________);
```

- [ ]  A. `Exhibit(Walrus(List<Integer> z), Object a)`
- [ ]  B. `Exhibit(Walrus(List m), Object n)`
- [ ]  C. `Object w && w.animal().diet().size() == 0`
- [ ]  D. `Exhibit(Walrus(var i), var i)`
- [ ]  E. `Exhibit(var p, var q)`
- [ ]  F. `Exhibit(List<?> g, var h)`
- [ ]  G. `Exhibit(var x, CharSequence y)`
- [ ]  H. `Exhibit(Walrus(null), var v)`
- [ ]  I. Ninguna de las anteriores

**9. ¿Cuáles de las siguientes instrucciones se pueden insertar en el espacio en blanco para que el código compile exitosamente?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
abstract class Snake {}
class Cobra extends Snake {}
class GardenSnake extends Cobra {}
public class SnakeHandler {
    private Snake snakey;
    public void setSnake(Snake mySnake) { this.snakey = mySnake; }
    public static void main(String[] args) {
        new SnakeHandler().setSnake(____________);
    } }
```

- [ ]  A. `new Cobra()`
- [ ]  B. `new Snake()`
- [ ]  C. `new Object()`
- [ ]  D. `new String("Snake")`
- [ ]  E. `new GardenSnake()`
- [ ]  F. `null`
- [ ]  G. Ninguna de las anteriores. La clase no compila, independientemente del valor insertado en el espacio en blanco.

**10. ¿Qué tipos se pueden insertar en los espacios en blanco de las líneas marcadas X y Z que permiten que el código compile?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
interface Walk { private static List move() { return null; } }
interface Run extends Walk { public ArrayList move(); }
class Leopard implements Walk {
    public ____________ move() {  // X
        return null;
    }
}
class Panther implements Run {
    public  move(____________) {  // Z
        return null;
    }
}
```

- [ ]  A. `Integer` en la línea marcada X
- [ ]  B. `ArrayList` en la línea marcada X
- [ ]  C. `List` en la línea marcada X
- [ ]  D. `List` en la línea marcada Z
- [ ]  E. `ArrayList` en la línea marcada Z
- [ ]  F. Ninguna de las anteriores, ya que la interfaz `Run` no compila.
- [ ]  G. No compila por una razón diferente.

**11. ¿Cuál es el resultado de compilar y ejecutar el siguiente código?**

```Java
1:  public class Movie {
2:      private int butter = 5;
3:      private Movie() {}
4:      protected class Popcorn {
5:          private Popcorn() {}
6:          public static int butter = 10;
7:          public void startMovie() {
8:              System.out.println(butter);
9:          }
10:     }
11:     public static void main(String[] args) {
12:         var movie = new Movie();
13:         Movie.Popcorn in = new Movie().new Popcorn();
14:         in.startMovie();
15:     } }
```

- [ ]  A. La salida es `5`.
- [ ]  B. La salida es `10`.
- [ ]  C. La línea 6 genera un error de compilación.
- [ ]  D. La línea 12 genera un error de compilación.
- [ ]  E. La línea 13 genera un error de compilación.
- [ ]  F. El código compila pero produce una excepción en tiempo de ejecución.

**12. ¿Qué variables o miembros son accesibles desde dentro del método `hiss()`?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
13: public class BoaConstrictor {
14:     private Body body;
15:     BoaConstrictor(Body b) { this.body = b; }
16:     private long tail = 10;
17:     record Body(int stripes) {
18:         private static int counter = 0;
19:         int counter() { return counter; }
20:         Body {
21:             stripes = stripes + counter++;
22:         }
23:         private void hiss() {} } }
```

- [ ]  A. `counter()`
- [ ]  B. `tail`
- [ ]  C. `body`
- [ ]  D. `stripes()`
- [ ]  E. `stripes`
- [ ]  F. `counter`
- [ ]  G. La línea 15 no compila.
- [ ]  H. La línea 17 no compila.
- [ ]  I. Las líneas 20–22 no compilan.

**13. ¿Cuál es el resultado del siguiente programa?**

```Java
public class Weather {
    enum Seasons {
        WINTER, SPRING, SUMMER, FALL
    }

    public static void main(String[] args) {
        Seasons v = null;
        switch (v) {
            case Seasons.SPRING -> System.out.print("s");
            case Seasons.WINTER -> System.out.print("w");
            case Seasons.SUMMER -> System.out.print("m");
            default -> System.out.println("missing data"); }
    } }
```

- [ ]  A. `s`
- [ ]  B. `w`
- [ ]  C. `m`
- [ ]  D. `missing data`
- [ ]  E. Exactamente una línea de código no compila.
- [ ]  F. Más de una línea de código no compila.
- [ ]  G. El código compila pero produce una excepción en tiempo de ejecución.

**14. ¿Cuáles afirmaciones sobre las clases selladas son correctas?** (_Se deben seleccionar todas las opciones que apliquen_).

- [ ]  A. Una interfaz sellada restringe qué subinterfaces pueden extenderla.
- [ ]  B. Una clase sellada no puede ser extendida indirectamente por una clase que no está listada en su cláusula `permits`.
- [ ]  C. Una clase sellada puede ser extendida por una clase `abstract`.
- [ ]  D. Una clase sellada puede ser extendida por una subclase que usa el modificador `nonsealed`.
- [ ]  E. Una interfaz sellada restringe qué subclases pueden implementarla.
- [ ]  F. Una clase sellada no puede contener ninguna subclase anidada.
- [ ]  G. Ninguna de las anteriores.

**15. ¿Qué línea permite que el código imprima `Not scared` en tiempo de ejecución?**

```Java
public class Ghost {
    public static void boo() {
        System.out.println("Not scared");
    }
    protected final class Spirit {
        public void boo() {
            System.out.println("Booo!!!");
        }
    }
    public static void main(String… haunt) {
        var g = new Ghost().new Spirit() {};
        ____________________________;
    } }
```

- [ ]  A. `g.boo()`
- [ ]  B. `g.super.boo()`
- [ ]  C. `new Ghost().boo()`
- [ ]  D. `g.Ghost.boo()`
- [ ]  E. `new Spirit().boo()`
- [ ]  F. Ninguna de las anteriores

**16. El siguiente código aparece en un archivo llamado `Ostrich.java`. ¿Cuál es el resultado de compilar el archivo fuente?**

```Java
1: public class Ostrich {
2:     private int count;
3:     static class OstrichWrangler {
4:         public int stampede() {
5:             return count;
6:         } } }
```

- [ ]  A. El código compila exitosamente, y se genera un archivo de bytecode: `Ostrich.class`.
- [ ]  B. El código compila exitosamente, y se generan dos archivos de bytecode: `Ostrich.class` y `OstrichWrangler.class`.
- [ ]  C. El código compila exitosamente, y se generan dos archivos de bytecode: `Ostrich.class` y `Ostrich$OstrichWrangler.class`.
- [ ]  D. Ocurre un error de compilación en la línea 3.
- [ ]  E. Ocurre un error de compilación en la línea 5.

**17. ¿Cuáles líneas de las siguientes declaraciones de interfaz no compilan?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
1: public interface Omnivore {
2:     int amount = 10;
3:     static boolean gather = true;
4:     static void eatGrass() {}
5:     int findMore() { return 2; }
6:     default float rest() { return 2; }
7:     protected int chew() { return 13; }
8:     private static void eatLeaves() {}
9: }
```

- [ ]  A. Todas las líneas compilan sin problema.
- [ ]  B. Línea 2.
- [ ]  C. Línea 3.
- [ ]  D. Línea 4.
- [ ]  E. Línea 5.
- [ ]  F. Línea 6.
- [ ]  G. Línea 7.
- [ ]  H. Línea 8.

**18. ¿Qué imprime el siguiente programa?**

```Java
public class Deer {
    enum Food {APPLES, BERRIES, GRASS}
    protected class Diet {
        private Food getFavorite() {
            return Food.BERRIES;
        }
    }
    public static void main(String[] seasons) {
        System.out.print(switch(new Diet().getFavorite()) {
            case APPLES -> "a";
            case BERRIES -> "b";
            default -> "c";
        });
    } }
```

- [ ]  A. `a`
- [ ]  B. `b`
- [ ]  C. `c`
- [ ]  D. La declaración de la clase `Diet` no compila.
- [ ]  E. El método `main()` no compila.
- [ ]  F. El código compila pero produce una excepción en tiempo de ejecución.
- [ ]  G. Ninguna de las anteriores.

**19. ¿Cuál de las siguientes imprime el programa `Bear`?**

```Java
public class Bear {
    enum FOOD {
        BERRIES, INSECTS {
            public boolean isHealthy() { return true; }},
        FISH, ROOTS, COOKIES, HONEY;
        public abstract boolean isHealthy();
    }
    public static void main(String[] args) {
        System.out.print(FOOD.INSECTS);
        System.out.print(FOOD.INSECTS.ordinal());
        System.out.print(FOOD.INSECTS.isHealthy());
        System.out.print(FOOD.COOKIES.isHealthy());
    } }
```

- [ ]  A. `insects`
- [ ]  B. `Insects`
- [ ]  C. `0`
- [ ]  D. `1`
- [ ]  E. `false`
- [ ]  F. El código no compila.

**20. ¿Cuál es la salida de este código?**

```Java
13: record Gorilla(int x, Double y) {
14:     Gorilla {}
15:     Gorilla() { this(1,2.0); }
16: }
17: record Family(Gorilla parent1, Gorilla parent2) {}
18:
19: var family = new Family(
20:     new Gorilla(1, null), new Gorilla(0, 1.2));
21: System.out.print(switch (family) {
22:     case Family(var a, var b) -> "1";
23:     case Family(Gorilla c, Gorilla (int d, double e)) ->
24: "2";
25:     case Family(Gorilla (int f, Double g), var h) ->
26: "3";
27:     case Family(Gorilla i, Gorilla (int j, Double k)) ->
28: "4";
29:     case Family(Object m, Object n) -> "5";
30:     case null -> "6";
31:     default -> "7";
32: });
```

- [ ]  A. `1`
- [ ]  B. `2`
- [ ]  C. `3`
- [ ]  D. `4`
- [ ]  E. `5`
- [ ]  F. `6`
- [ ]  G. `7`
- [ ]  H. Ninguna de las anteriores

**21. Dada la siguiente declaración de `record`, ¿qué línea de código puede llenar el espacio en blanco y permitir que el código compile?**

```Java
public record RabbitFood(int size, String brand, LocalDate
expires) {
    public static int MAX_STORAGE = 100;
    public RabbitFood() {
        ___________________________;
    }
}
```

- [ ]  A. `size = MAX_STORAGE`
- [ ]  B. `this.size = 10`
- [ ]  C. `if(expires.isAfter(LocalDate.now())) throw new RuntimeException()`
- [ ]  D. `if(brand==null) super.brand = "Unknown"`
- [ ]  E. `throw new RuntimeException()`
- [ ]  F. Ninguna de las anteriores

**22. ¿Cuáles de las siguientes pueden insertarse en el método `rest()`?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
public class Lion {
    class Cub {}
    static class Den {}
    static void rest() {
        ________________;
    } }
```

- [ ]  A. `Cub a = Lion.new Cub()`
- [ ]  B. `Lion.Cub b = new Lion().Cub()`
- [ ]  C. `Lion.Cub c = new Lion().new Cub()`
- [ ]  D. `var d = new Den()`
- [ ]  E. `var e = Lion.new Cub()`
- [ ]  F. `Lion.Den f = Lion.new Den()`
- [ ]  G. `Lion.Den g = new Lion.Den()`
- [ ]  H. `var h = new Cub()`

**23. Dado el siguiente programa, ¿qué se puede insertar en la línea en blanco que permitiría que imprimiera `Swim!` en tiempo de ejecución?**

```Java
interface Swim {
    default void perform() { System.out.print("Swim!"); }
}
interface Dance {
    default void perform() { System.out.print("Dance!"); }
}
public class Penguin implements Swim, Dance {
    public void perform() { System.out.print("Smile!"); }
    private void doShow() {
        ___________________;
    }
    public static void main(String[] eggs) {
        new Penguin().doShow();
    } }
```

- [ ]  A. `super.perform()`
- [ ]  B. `Swim.perform()`
- [ ]  C. `super.Swim.perform()`
- [ ]  D. `Swim.super.perform()`
- [ ]  E. El código no compila independientemente de lo que se inserte en el espacio en blanco.
- [ ]  F. El código compila, pero debido al polimorfismo, no es posible producir la salida solicitada sin crear un nuevo objeto.

**24. ¿Cuáles líneas de la siguiente interfaz no compilan?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
1: public interface BigCat {
2:     abstract String getName();
3:     static int hunt() { getName(); return 5; }
4:     default void climb() { rest(); }
5:     private void roar() { getName();  climb(); hunt(); }
6:     private static boolean sneak() { roar(); return true;
7:     }
8:     private int rest() { return 2; };
9: }
```

- [ ]  A. Línea 2
- [ ]  B. Línea 3
- [ ]  C. Línea 4
- [ ]  D. Línea 5
- [ ]  E. Línea 6
- [ ]  F. Línea 7
- [ ]  G. Ninguna de las anteriores

**25. ¿Qué imprime el siguiente programa?**

```Java
1:  public class Zebra {
2:      private int x = 24;
3:      public int hunt() {
4:          String message = "x is ";
5:          abstract class Stripes {
6:              private int x = 0;
7:              public void print() {
8:                  System.out.print(message + Zebra.this.x);
9:              }
10:         }
11:         var s = new Stripes() {};
12:         s.print();
13:         return x;
14:     }
15:     public static void main(String[] args) {
16:         new Zebra().hunt();
17:     } }
```

- [ ]  A. `x is 0`
- [ ]  B. `x is 24`
- [ ]  C. La línea 6 genera un error de compilación.
- [ ]  D. La línea 8 genera un error de compilación.
- [ ]  E. La línea 11 genera un error de compilación.
- [ ]  F. Ninguna de las anteriores.

**26. ¿Cuál es la salida del siguiente programa?**

```Java
20: public enum Animals {
21:     MAMMAL(List.of(2,4)),
22:     INVERTEBRATE(List.of(2, 4, 6, 8, 100)),
23:     BIRD(null) {
24:         public int stand() {
25:             return legs.get(0) + 4;
26:         }
27:     };
28:     List<Integer> legs;
29:     Animals(List<Integer> legs) {
30:         this.legs = legs;
31:     }
32:     public int stand() { return legs.get(0); }
33:     public static void main(String[] a) {
34:         Animals.BIRD.legs = List.of(-1);
35:         System.out.println(Animals.BIRD.stand());
36:     } }
```

- [ ]  A. `null`
- [ ]  B. `-1`
- [ ]  C. `3`
- [ ]  D. `4`
- [ ]  E. Error de compilación en la línea 23.
- [ ]  F. Error de compilación en la línea 24.
- [ ]  G. Error de compilación en la línea 34.
- [ ]  H. El código compila pero produce una `NullPointerException` en tiempo de ejecución.
- [ ]  I. Ninguna de las anteriores.

**27. Asumiendo que un `record` está definido con al menos un campo, ¿qué componentes inserta siempre el compilador, cada uno de los cuales puede ser sobrescrito o redeclarado?** (_Se deben seleccionar todas las opciones que apliquen_).

- [ ]  A. Un constructor sin argumentos
- [ ]  B. Un método de acceso para cada campo
- [ ]  C. El método `toString()`
- [ ]  D. El método `equals()`
- [ ]  E. Un método mutador para cada campo
- [ ]  F. Un método de ordenamiento para cada campo
- [ ]  G. El método `hashCode()`

**28. ¿Cuáles de las siguientes clases e interfaces no compilan?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
public abstract class Camel { void travel(); }

public interface EatsGrass { private abstract int chew(); }

public abstract class Elephant {
    abstract private class SleepsAlot {
        abstract int sleep();
    } }

public class Eagle { abstract soar(); }

public interface Spider { default void crawl() {} }
```

- [ ]  A. `Camel`
- [ ]  B. `EatsGrass`
- [ ]  C. `Elephant`
- [ ]  D. `Eagle`
- [ ]  E. `Spider`

**29. ¿Cuántas líneas del siguiente programa contienen un error de compilación?**

```Java
1:  class Primate {
2:      protected int age = 2;
3:      { age = 1; }
4:      public Primate() {
5:          this().age = 3;
6:      }
7:  }
8:  public class Orangutan {
9:      protected int age = 4;
10:     { age = 5; }
11:     public Orangutan() {
12:         this().age = 6;
13:     }
14:     public static void main(String[] bananas) {
15:         final Primate x = (Primate)new Orangutan();
16:         System.out.println(x.age);
17:     }
18: }
```

- [ ]  A. Ninguna, y el programa imprime `1` en tiempo de ejecución.
- [ ]  B. Ninguna, y el programa imprime `3` en tiempo de ejecución.
- [ ]  C. Ninguna, pero provoca una `ClassCastException` en tiempo de ejecución.
- [ ]  D. `1`
- [ ]  E. `2`
- [ ]  F. `3`
- [ ]  G. `4`

**30. Asumiendo que las siguientes clases están declaradas como tipos de nivel superior en el mismo archivo, ¿qué clases contienen errores de compilación?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
sealed class Bird {
    public final class Flamingo extends Bird {}
}

sealed class Monkey {}

class EmperorTamarin extends Monkey {}

non-sealed class Mandrill extends Monkey {}

sealed class Friendly extends Mandrill permits Silly {}

final class Silly {}
```

- [ ]  A. `Bird`
- [ ]  B. `Monkey`
- [ ]  C. `EmperorTamarin`
- [ ]  D. `Mandrill`
- [ ]  E. `Friendly`
- [ ]  F. `Silly`
- [ ]  G. Todas las clases compilan sin problema.
