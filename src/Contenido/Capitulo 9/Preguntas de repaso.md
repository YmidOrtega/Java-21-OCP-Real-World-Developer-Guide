Las respuestas a las preguntas de revisión del capítulo se pueden encontrar al final del capitulo.

**1.** Supón que necesitas mostrar una colección de productos para la venta, que puede contener duplicados. Además, tienes una colección de ventas que necesitas rastrear, ordenada por el orden natural del ID de la venta, y necesitas recuperar el texto de cada una. ¿Cuáles dos de las siguientes clases se adaptan mejor a tus necesidades para cada uno de estos escenarios? (Elige dos.)

- [ ] A. `ArrayList`
- [ ] B. `HashMap`
- [ ] C. `HashSet`
- [ ] D. `LinkedList`
- [x] E. `SequencedTreeSet`
- [x] F. `TreeMap`

**2.** ¿Cuáles de las siguientes afirmaciones son verdaderas? (Elige todas las que apliquen.)

```java
12: List<?> q = List.of("mouse", "parrot");
13: var v = List.of("mouse", "parrot");
14:
15: q.removeIf(String::isEmpty);
16: q.removeIf(s -> s.length() == 4);
17: v.removeIf(String::isEmpty);
18: v.removeIf(s -> s.length() == 4);
```

- [ ] A. Este código compila y se ejecuta sin errores.
- [ ] B. Exactamente una de estas líneas contiene un error de compilación.
- [ ] C. Exactamente dos de estas líneas contienen un error de compilación.
- [ ] D. Exactamente tres de estas líneas contienen un error de compilación.
- [ ] E. Exactamente cuatro de estas líneas contienen un error de compilación.
- [ ] F. Si se elimina cualquier línea con errores de compilación, este código se ejecuta sin lanzar una excepción.
- [x] G. Si se elimina cualquier línea con errores de compilación, este código lanza una excepción.

**3.** ¿Cuál es el resultado de las siguientes sentencias?

```java
3:  var greetings = new ArrayDeque<String>();
4:  greetings.offerLast("hello");
5:  greetings.offerLast("hi");
6:  greetings.offerFirst("ola");
7:  greetings.pop();
8:  greetings.peek();
9:  while (greetings.peek() != null)
10:     System.out.print(greetings.pop());
```

- [ ] A. `hello`
- [x] B. `hellohi`
- [ ] C. `hellohiola`
- [ ] D. `hiola`
- [ ] E. El código no compila.
- [ ] F. Se lanza una excepción.

**4.** ¿Cuáles de estas sentencias compilan? (Elige todas las que apliquen.)

- [ ] A. `HashSet<Number> hs = new HashSet<Integer>();`
- [x] B. `HashSet<? super ClassCastException> set = new HashSet<Exception>();`
- [ ] C. `List<> list = new ArrayList<String>();`
- [ ] D. `List<Object> values = new HashSet<Object>();`
- [x] E. `List<Object> objects = new ArrayList<? extends Object>();`
- [x] F. `Map<String, ? extends Number> hm = new HashMap<String, Integer>();`

**5.** ¿Cuál es el resultado del siguiente código?

```java
1:  public record Hello<T>(T t) {
2:      public Hello(T t) { this.t = t; }
3:      private <T> void println(T message) {
4:          System.out.print(t + "-" + message);
5:      }
6:      public static void main(String[] args) {
7:          new Hello<String>("hi").println(1);
8:          new Hello("hola").println(true);
9:      } }
```

- [ ] A. `hi` seguido de una excepción en tiempo de ejecución.
- [x] B. `hi-1hola-true`
- [ ] C. El primer error de compilación está en la línea 1.
- [ ] D. El primer error de compilación está en la línea 3.
- [ ] E. El primer error de compilación está en la línea 8.
- [ ] F. El primer error de compilación está en otra línea.

**6.** ¿Cuál de los siguientes fragmentos puede rellenar el espacio en blanco para imprimir `[7, 5, 3]`? (Elige todos los que apliquen.)

```java
8:  public record Platypus(String name, int beakLength) {
9:      @Override public String toString() {return "" + beakLength;}
10: 
11:     public static void main(String[] args) {
12:         Platypus p1 = new Platypus("Paula", 3);
13:         Platypus p2 = new Platypus("Peter", 5);
14:         Platypus p3 = new Platypus("Peter", 7);
15:     
16:         List<Platypus> list = Arrays.asList(p1, p2, p3);
17:     
18:         Collections.sort(list, Comparator.comparing________________);
19:            
20:         System.out.println(list);
21:     }     
22: }
```

- [ ] A. `(Platypus::beakLength)`
- [ ] B. `(Platypus::beakLength).reversed()`
- [ ] C. `(Platypus::name).thenComparing(Platypus::beakLength)`
- [ ] D. `(Platypus::name).thenComparing(Comparator.comparing(Platypus::beakLength).reversed())`
- [ ] E. `(Platypus::name).thenComparingNumber(Platypus::beakLength).reversed()`
- [ ] F. `(Platypus::name).thenComparingInt(Platypus::beakLength).reversed()`

**7.** ¿Cuáles de las siguientes firmas de método son sobrescrituras (overrides) válidas del método `hairy()` en la clase `Alpaca`? (Elige todas las que apliquen.)

```java
import java.util.*;

public class Alpaca {
    public List<String> hairy(List<String> list) { return null;
}
}
```

- [ ] A. `public List<String> hairy(List<CharSequence> list) { return null; }`
- [ ] B. `public List<String> hairy(List<String> list) { return null; }`
- [ ] C. `public List<String> hairy(List<Integer> list) { return null; }`
- [ ] D. `public List<CharSequence> hairy(List<String> list) { return null; }`
- [ ] E. `public Object hairy(List<String> list) { return null; }`
- [ ] F. `public ArrayList<String> hairy(List<String> list) { return null; }`

**8.** ¿Cuál de las siguientes opciones rellena el espacio en blanco, permitiendo que el código compile y se ejecute sin problemas?

```java
11: SequencedCollection<String> animals = new _____________<> ();
12: animals.addFirst("lions");
13: animals.addLast("tigers");
14: for(var a : animals)
15:     System.out.println(a);
16: System.out.println(animals.get(0));
```

- [ ] A. `HashSet`
- [ ] B. `LinkedList`
- [ ] C. `TreeSetMap`
- [ ] D. `HashMap`
- [ ] E. Ninguna de las anteriores

**9.** ¿Cuál es el resultado del siguiente programa?

```java
3:  public class MyComparator implements Comparator<String> {
4:      public int compare(String a, String b) {
5:          return b.toLowerCase().compareTo(a.toLowerCase());
6:      }
7:      public static void main(String[] args) {
8:          String[] values = { "123", "Abb", "aab" };
9:          Arrays.sort(values, new MyComparator());
10:         for (var s: values)
11:             System.out.print(s + " ");
12:     }
13: }
```

- [ ] A. `Abb aab 123`
- [ ] B. `aab Abb 123`
- [ ] C. `123 Abb aab`
- [ ] D. `123 aab Abb`
- [ ] E. El código no compila.
- [ ] F. Se lanza una excepción en tiempo de ejecución.

**10.** ¿Cuáles de estas sentencias pueden rellenar el espacio en blanco para que la clase `Helper` compile con éxito? (Elige todas las que apliquen.)

```java
2:  public class Helper {
3:      public static <U extends Exception>
4:          void printException(U u) {
5:
6:          System.out.println(u.getMessage());
7:      }
8:      public static void main(String[] args) {
9:          Helper.__________________________________;
10:     } }
```

- [ ] A. `printException(new FileNotFoundException("A"))`
- [ ] B. `printException(new Exception("B"))`
- [ ] C. `<Throwable>printException(new Exception("C"))`
- [ ] D. `<NullPointerException>printException(new NullPointerException ("D"))`
- [ ] E. `printException(new Throwable("E"))`

**11.** ¿Cuál de las siguientes opciones compilará al rellenar el espacio en blanco? (Elige todas las que apliquen.)

```java
var list = List.of(1, 2, 3);
var set = Set.of(1, 2, 3);
var map = Map.of(1, 2, 3, 4);

____________.forEach(System.out::println);
```

- [ ] A. `list`
- [ ] B. `set`
- [ ] C. `map`
- [ ] D. `map.keys()`
- [ ] E. `map.keySet()`
- [ ] F. `map.values()`
- [ ] G. `map.valueSet()`

**12.** ¿Cuáles de estas sentencias pueden rellenar el espacio en blanco para que la clase `Wildcard` compile con éxito? (Elige todas las que apliquen.)

```java
3:  public class Wildcard {
4:      public void showSize(List<?> list) {
5:          System.out.println(list.size());
6:      }
7:      public static void main(String[] args) {
8:          Wildcard card = new Wildcard();
9:          _____________________________________;
10:         card.showSize(list);
11:     } }
```

- [ ] A. `List<?> list = new HashSet<String>()`
- [ ] B. `ArrayList<? super Date> list = new ArrayList<Date>()`
- [ ] C. `List<?> list = new ArrayList<?>()`
- [ ] D. `List<Exception> list = new LinkedList<java.io.IOException>()`
- [ ] E. `ArrayList<? extends Number> list = new ArrayList<Integer>()`
- [ ] F. Ninguna de las anteriores

**13.** ¿Cuál es el resultado del siguiente programa?

```java
3:  public record Sorted(int num, String text)
4:      implements Comparable<Sorted>, Comparator<Sorted> {
5:
6:      public String toString() { return "" + num; }
7:      public int compareTo(Sorted s) {
8:          return text.compareTo(s.text);
9:      }
10:     public int compare(Sorted s1, Sorted s2) {
11:         return s1.num - s2.num;
12:     }
13:     public static void main(String[] args) {
14:         var s1 = new Sorted(88, "a");
15:         var s2 = new Sorted(55, "b");
16:         SequencedSet<Sorted> t1 = new TreeSet<Sorted>();
17:         t1.add(s1); t1.add(s2);
18:         var t2 = new TreeSet<Sorted>(s1);
19:         t2.add(s1); t2.add(s2);
20:         System.out.println(t1 + " " + t2);
21:     } }
```

- [ ] A. `[55, 88] [55, 88]`
- [ ] B. `[55, 88] [88, 55]`
- [ ] C. `[88, 55] [55, 88]`
- [ ] D. `[88, 55] [88, 55]`
- [ ] E. El código no compila.
- [ ] F. Se lanza una excepción en tiempo de ejecución.

**14.** ¿Cuál es el resultado del siguiente código?

```java
Comparator<Integer> c1 = (o1, o2) -> o2 - o1;
Comparator<Integer> c2 = Comparator.naturalOrder();
Comparator<Integer> c3 = Comparator.reverseOrder();

var list = Arrays.asList(5, 4, 7, 2);
Collections.sort(list, ____________);
Collections.reverse(list);
Collections.reverse(list);
System.out.println(Collections.binarySearch(list, 2));
```

- [ ] A. Uno o más comparadores pueden rellenar el espacio en blanco de modo que el código imprima `0`.
- [ ] B. Uno o más comparadores pueden rellenar el espacio en blanco de modo que el código imprima `1`.
- [ ] C. Uno o más comparadores pueden rellenar el espacio en blanco de modo que el código imprima `2`.
- [ ] D. El resultado no está definido independientemente de qué comparador se utilice.
- [ ] E. Se lanza una excepción en tiempo de ejecución independientemente de qué comparador se utilice.
- [ ] F. El código no compila.

**15.** ¿Cuáles de las siguientes líneas se pueden insertar para hacer que el código compile? (Elige todas las que apliquen.)

```java
class W {}
class X extends W {}
class Y extends X {}
class Z<Y> {
    // INSERTAR CÓDIGO AQUÍ
}
```

- [ ] A. `W w1 = new W();`
- [ ] B. `W w2 = new X();`
- [ ] C. `W w3 = new Y();`
- [ ] D. `Y y1 = new W();`
- [ ] E. `Y y2 = new X();`
- [ ] F. `Y y3 = new Y();`

**16.** ¿Qué opciones son verdaderas sobre el siguiente código? (Elige todas las que apliquen.)

```java
____________ q = new LinkedList<>();
var u = Collections.unmodifiableCollection(q);
q.add(10);
q.add(12);
q.remove(1);
System.out.print(u);
```

- [ ] A. Si rellenamos el espacio en blanco con `List<Integer>`, la salida es `[10]`.
- [ ] B. Si rellenamos el espacio en blanco con `Queue<Integer>`, la salida es `[10]`.
- [ ] C. Si rellenamos el espacio en blanco con `var`, la salida es `[10]`.
- [ ] D. Uno o más de los escenarios no compila.
- [ ] E. Uno o más de los escenarios lanza una excepción en tiempo de ejecución.

**17.** ¿Cuál es el resultado del siguiente código?

```java
4: Map m = new HashMap();
5: m.put(123, "456");
6: m.put("abc", "def");
7: System.out.println(m.contains("123"));
```

- [ ] A. `false`
- [ ] B. `true`
- [ ] C. Error de compilación en la línea 4.
- [ ] D. Error de compilación en la línea 5.
- [ ] E. Error de compilación en la línea 7.
- [ ] F. Se lanza una excepción en tiempo de ejecución.

**18.** ¿Cuál es el resultado del siguiente código? (Elige todas las que apliquen.)

```java
48: var map = Map.of(1,2, 3, 6);
49: var list = List.copyOf(map.entrySet());
50:
51: List<Integer> one = List.of(8, 16, 2);
52: var copy = List.copyOf(one);
53: var copyOfCopy = List.copyOf(copy);
54: var thirdCopy = new ArrayList<>(copyOfCopy);
55:
56: list.replaceAll(x -> x * 2);
57: one.replaceAll(x -> x * 2);
58: thirdCopy.replaceAll(x -> x * 2);
59:
60: System.out.println(thirdCopy);
```

- [ ] A. Una línea no compila.
- [ ] B. Dos líneas no compilan.
- [ ] C. Tres líneas no compilan.
- [ ] D. El código compila pero lanza una excepción en tiempo de ejecución.
- [ ] E. Si se elimina cualquier línea con errores de compilación, el código lanza una excepción en tiempo de ejecución.
- [ ] F. Si se elimina cualquier línea con errores de compilación, el código imprime `[16, 32, 4]`.
- [ ] G. El código compila e imprime `[16, 32, 4]` sin ningún cambio.

**19.** ¿Qué cambio de código se necesita para hacer que el método compile, asumiendo que no existe ninguna clase llamada `T`?

```java
public static T identity(T t) {
    return t;
}
```

- [ ] A. Añadir `<T>` después de la palabra clave `public`.
- [ ] B. Añadir `<T>` después de la palabra clave `static`.
- [ ] C. Añadir `<T>` después de `T`.
- [ ] D. Añadir `<?>` después de la palabra clave `public`.
- [ ] E. Añadir `<?>` después de la palabra clave `static`.
- [ ] F. No se requiere ningún cambio. El código ya compila.

**20.** Asumiendo que las claves se imprimen en orden, ¿cuál es el resultado de lo siguiente?

```java
var map = new HashMap<Integer, Integer>();
map.put(1, 10);
map.put(2, 20);
map.put(3, null);
map.merge(1, 3, (a,b) -> a + b);
map.merge(3, 3, (a,b) -> a + b);
System.out.println(map);
```

- [ ] A. `{1=10, 2=20}`
- [ ] B. `{1=10, 2=20, 3=null}`
- [ ] C. `{1=10, 2=20, 3=3}`
- [ ] D. `{1=13, 2=20}`
- [ ] E. `{1=13, 2=20, 3=null}`
- [ ] F. `{1=13, 2=20, 3=3}`
- [ ] G. El código no compila.
- [ ] H. Se lanza una excepción.

**21.** ¿Cuáles de las siguientes afirmaciones son verdaderas? (Elige todas las que apliquen.)

- [ ] A. `Comparable` está en el paquete `java.util`.
- [ ] B. `Comparator` está en el paquete `java.util`.
- [ ] C. `compare()` está en la interfaz `Comparable`.
- [ ] D. `compare()` está en la interfaz `Comparator`.
- [ ] E. `compare()` toma un parámetro de método.
- [ ] F. `compare()` toma dos parámetros de método.

**22.** ¿Cuál es la salida del siguiente fragmento de código?

```java
21: SequencedMap<Integer, String> cats = new TreeMap<>();
22: cats.put(3, "Snowball");
23: cats.put(2, "Sugar");
24: cats.put(1, "Minnie Mouse");
25: cats.pollFirstEntry();
26: var id = cats.lastEntry().getKey();
27: cats.pollFirstEntry();
28: System.out.print(cats.firstEntry().getValue());
```

- [ ] A. `Minnie Mouse`
- [ ] B. `Snowball`
- [ ] C. `Sugar`
- [ ] D. El código no compila.
- [ ] E. El código compila, pero se lanza una excepción en tiempo de ejecución.

**23.** ¿Cuál es la salida del siguiente fragmento de código?

```java
var fishes = new TreeSet<String>();
fishes.add("Koi");
fishes.addFirst("clown");
fishes.add("carp");
for(var fish : fishes)
    System.out.print(fish + ", ");
```

- [ ] A. `carp, clown, Koi,`
- [ ] B. `carp, Koi, clown,`
- [ ] C. `clown, carp, Koi,`
- [ ] D. `clown, Koi, carp,`
- [ ] E. `Koi, carp, clown,`
- [ ] F. `Koi, clown, carp,`
- [ ] G. El código no compila.
- [ ] H. El código compila pero lanza una excepción en tiempo de ejecución.
