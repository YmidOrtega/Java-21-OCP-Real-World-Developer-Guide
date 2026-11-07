Las respuestas a las preguntas de revisión del capítulo se encuentran en el Apéndice del libro.

**1.** Dado el siguiente fragmento de código, ¿qué opciones **crean** correctamente un stream paralelo? (Se deben seleccionar todas las opciones que apliquen.)

```Java
var c = List.of(19, 66);
var s = ThreadLocalRandom.current().doubles();
var p = ________________;
```

- [ ] A. `new ParallelStream(s)`
- [ ] B. `c.parallel()`
- [ ] C. `s.parallelStream()`
- [ ] D. `c.parallelStream()`
- [ ] E. `new ParallelStream(c)`
- [ ] F. `s.parallel()`

---

**2.** Dado que la suma de los números de 1 (incluido) a 10 (excluido) es 45, ¿cuáles son los resultados posibles de ejecutar el siguiente programa? (Se deben seleccionar todas las opciones que apliquen.)

```Java
1: import java.util.concurrent.locks.*;
2: import java.util.stream.*;
3: public class Bank {
4:    private Lock vault = new ReentrantLock();
5:    private int total = 0;
6:    public void deposit(int value) {
7:       try {
8:          vault.tryLock();
9:          total += value;
10:      } finally { vault.unlock(); }
11:   }
12:   public static void main(String[] unused) {
13:       var bank = new Bank();
14:       IntStream.range(1, 10).parallel()
15:          .forEach(s -> bank.deposit(s));
16:       System.out.println(bank.total);
17:   } }
```

- [ ] A. **Se imprime** 45.
- [ ] B. **Se imprime** un número menor que 45.
- [ ] C. **Se imprime** un número mayor que 45.
- [ ] D. **Se lanza** una excepción.
- [ ] E. Ninguno de los anteriores, ya que el código **no compila**.

---

**3.** ¿Cuáles de las siguientes afirmaciones sobre los métodos `call()` de `Callable` y `run()` de `Runnable` son correctas? (Se deben seleccionar todas las opciones que apliquen.)

- [ ] A. Ambos métodos **devuelven** `void`.
- [ ] B. Ambos **pueden lanzar** excepciones no verificadas (_unchecked_).
- [ ] C. Ambos pueden implementarse con expresiones lambda.
- [ ] D. `Runnable` devuelve un tipo genérico.
- [ ] E. Ambos pueden lanzar excepciones verificadas (_checked_).
- [ ] F. `Callable` devuelve un tipo genérico.

---

**4.** ¿Qué líneas **deben cambiarse** para que el código compile?

```Java
try (ExecutorService service =         // w1
   Executors.newSingleThreadScheduledExecutor()) {
   service.scheduleWithFixedDelay(() -> {
      System.out.println("Open Zoo");
      return null;                     // w2
   }, 0, 1, TimeUnit.MINUTES);
   var result = service.submit(() ->   // w3
      System.out.println("Wake Staff"));
   System.out.println(result.get());
}
```

- [ ] A. Solo la línea w1.
- [ ] B. Solo la línea w2.
- [ ] C. Solo la línea w3.
- [ ] D. Las líneas w1 y w2.
- [ ] E. Las líneas w1 y w2.
- [ ] F. Las líneas w1 y w3.
- [ ] G. Ninguna de las anteriores; el código compila.

---

**5.** ¿Qué afirmación sobre el siguiente código es verdadera?

```Java
var value1 = new AtomicLong(0);
final long[] value2 = {0};
IntStream.iterate(1, i -> 1).limit(100).parallel()
   .forEach(i -> value1.incrementAndGet());
IntStream.iterate(1, i -> 1).limit(100).parallel()
   .forEach(i -> ++value2[0]);
System.out.println(value1 + " " + value2[0]);
```

- [ ] A. Imprime `100 100`.
- [ ] B. Imprime `100 99`.
- [ ] C. La salida **no puede determinarse** de antemano.
- [ ] D. El código no compila.
- [ ] E. Compila pero lanza una excepción en tiempo de ejecución.
- [ ] F. Compila pero entra en un bucle infinito en tiempo de ejecución.
- [ ] G. Ninguna de las anteriores.

---

**6.** ¿Cuáles de las siguientes afirmaciones sobre el siguiente código son correctas? (Se deben seleccionar todas las opciones que apliquen.)

```Java
var data = List.of(2, 5, 1, 9, 8);
data.stream().parallel()
   .mapToInt(s -> s)
   .peek(System.out::print)
   .forEachOrdered(System.out::print);
```

- [ ] A. El método `peek()` **imprimirá** las entradas en orden ascendente: 12589.
- [ ] B. El método `peek()` **imprimirá** las entradas en el orden original: 25198.
- [ ] C. El método `peek()` imprimirá las entradas en un orden que no puede determinarse de antemano.
- [ ] D. El método `forEachOrdered()` imprimirá las entradas en orden ascendente: 12589.
- [ ] E. El método `forEachOrdered()` imprimirá las entradas en el orden original: 25198.
- [ ] F. El método `forEachOrdered()` imprimirá las entradas en un orden que no puede determinarse de antemano.
- [ ] G. El código no compila.

---

**7.** Complete los espacios en blanco: __________ **ocurre** cuando dos o más hilos **quedan** bloqueados para siempre pero ambos parecen activos. _______ **ocurre** cuando dos o más hilos **intentan** completar una tarea relacionada al mismo tiempo, con datos inválidos o inesperados.

- [ ] A. Bloqueo vivo, interbloqueo
- [ ] B. Interbloqueo, inanición
- [ ] C. Condiciones de carrera, interbloqueo
- [ ] D. Bloqueo vivo, condiciones de carrera
- [ ] E. Inanición, condiciones de carrera
- [ ] F. Interbloqueo, bloqueo vivo

---

**8.** Suponiendo que a esta clase **se accede** desde un solo hilo a la vez, ¿cuál es el resultado de llamar al método `countIceCreamFlavors()`?

```Java
import java.util.stream.LongStream;
public class Flavors {
   private static int counter;
   public static void countIceCreamFlavors() {
      counter = 0;
      Runnable task = () -> counter++;
      LongStream.range(0, 500)
         .forEach(m -> Thread.ofPlatform()
           .priority(1)
           .unstarted(task)
           .run());
      System.out.println(counter);
   } }
```

- [ ] A. El método imprime de forma constante un número menor que 500.
- [ ] B. El método imprime de forma constante 500.
- [ ] C. El método compila e imprime un valor, pero ese valor no puede determinarse de antemano.
- [ ] D. El método no compila.
- [ ] E. El método compila pero lanza una excepción en tiempo de ejecución.
- [ ] F. Ninguna de las anteriores.

---

**9.** ¿Cuáles de estas afirmaciones sobre `ExecutorService` son verdaderas? (Se deben seleccionar todas las opciones que apliquen.)

- [ ] A. Si **se envía** una tarea cuando no hay hilos disponibles, el executor descarta la tarea sin completarla.
- [ ] B. Si **se envía** una tarea cuando no hay hilos disponibles, el executor agrega la tarea a una cola interna y **se completa** cuando hay un hilo disponible.
- [ ] C. Si **se envía** una tarea cuando no hay hilos disponibles, el hilo que envía la tarea espera en la llamada `submit` hasta que haya un hilo disponible antes de continuar.
- [ ] D. Los hilos de plataforma pueden agruparse usando `ExecutorService`, pero no los hilos virtuales.
- [ ] E. Los hilos virtuales pueden agruparse usando `ExecutorService`, pero no los hilos de plataforma.
- [ ] F. Tanto los hilos de plataforma como los virtuales pueden agruparse usando `ExecutorService`.

---

**10.** ¿Cuál es el resultado de ejecutar el siguiente fragmento de código?

```Java
SequencedCollection<Integer> lions = new ArrayList<>
   (List.of(1, 2, 3));
SequencedCollection<Integer> tigers = new
   CopyOnWriteArrayList<>(lions);
Set<Integer> bears = new ConcurrentSkipListSet<>();
bears.addAll(lions);
for (Integer item: tigers) tigers.add(4); // x1
for (Integer item: bears) bears.add(5);   // x2
System.out.println(lions.size() + " " + tigers.size()
   + " " + bears.size());
```

- [ ] A. Imprime `3 6 4`.
- [ ] B. Imprime `6 6 6`.
- [ ] C. Imprime `6 3 4`.
- [ ] D. El código no compila.
- [ ] E. Compila pero lanza una excepción en tiempo de ejecución en la línea x1.
- [ ] F. Compila pero lanza una excepción en tiempo de ejecución en la línea x2.
- [ ] G. Compila pero entra en un bucle infinito en tiempo de ejecución.

---

**11.** ¿Qué afirmación sobre el siguiente código es verdadera?

```Java
Integer i1 = List.of(1, 2, 3, 4,
   5).stream().findAny().get();
synchronized(i1) { // y1
   Integer i2 = List.of(6, 7, 8, 9, 10)
      .parallelStream()
      .sorted()
      .findAny().get(); // y2
   System.out.println(i1 + " " + i2);
}
```

- [ ] A. El primer valor impreso es siempre 1.
- [ ] B. El segundo valor impreso es siempre 6.
- [ ] C. El código no compilará a causa de la línea y1.
- [ ] D. El código no compilará a causa de la línea y2.
- [ ] E. El código compila pero lanza una excepción en tiempo de ejecución.
- [ ] F. La salida no puede determinarse de antemano.
- [ ] G. Compila pero espera para siempre en tiempo de ejecución.

---

**12.** Suponiendo que cada llamada a `takeNap()` tarda cinco segundos en ejecutarse sin lanzar una excepción, ¿cuál es el resultado esperado de ejecutar el siguiente fragmento de código? (Se deben seleccionar todas las opciones que apliquen.)

```Java
public void shutdown() throws InterruptedException {
   var service = Executors.newFixedThreadPool(4);
   try {
      service.execute(() -> takeNap());
      service.execute(() -> takeNap());
      service.execute(() -> takeNap());
   } finally {
      service.shutdown();
   }
   service.awaitTermination(2, TimeUnit.SECONDS);
   System.out.println("DONE!");
}
public void refactored() {
   try (var service = Executors.newFixedThreadPool(4)) {
      service.execute(() -> takeNap());
      service.execute(() -> takeNap());
      service.execute(() -> takeNap());
   }
   System.out.println("DONE!");
}
```

- [ ] A. `shutdown()` pausará aproximadamente 2 segundos y luego imprimirá `DONE!`.
- [ ] B. `shutdown()` pausará aproximadamente 5 segundos y luego imprimirá `DONE!`.
- [ ] C. `shutdown()` pausará aproximadamente 15 segundos y luego imprimirá `DONE!`.
- [ ] D. `refactored()` pausará aproximadamente 2 segundos y luego imprimirá `DONE!`.
- [ ] E. `refactored()` pausará aproximadamente 5 segundos y luego imprimirá `DONE!`.
- [ ] F. `refactored()` pausará aproximadamente 15 segundos y luego imprimirá `DONE!`.
- [ ] G. Uno de los métodos devuelve el resultado inmediatamente.
- [ ] H. Uno de los métodos lanza una excepción.

---

**13.** ¿Qué afirmación sobre el siguiente código es verdadera?

```Java
System.out.print(List.of("duck","flamingo","pelican")
   .parallelStream().parallel()   // q1
   .reduce(0,
      (c1, c2) -> c1.length() + c2.length(), // q2
      (s1, s2) -> s1 + s2));      // q3
```

- [ ] A. Compila y se ejecuta sin problemas, imprimiendo la longitud total de todas las cadenas del stream.
- [ ] B. El código no compilará a causa de la línea q1.
- [ ] C. El código no compilará a causa de la línea q2.
- [ ] D. El código no compilará a causa de la línea q3.
- [ ] E. Compila pero lanza una excepción en tiempo de ejecución.
- [ ] F. Ninguna de las anteriores.

---

**14.** ¿Cuáles de las siguientes afirmaciones sobre el siguiente fragmento de código son verdaderas? (Se deben seleccionar todas las opciones que apliquen.)

```Java
Object o1 = new Object();
Object o2 = new Object();
try (var service = Executors.newFixedThreadPool(2)) {
   var f1 = service.submit(() -> {
      synchronized (o1) {
         synchronized (o2) { System.out.print("Tortoise");
         }
      }
   });
   var f2 = service.submit(() -> {
      synchronized (o2) {
         synchronized (o1) { System.out.print("Hare"); }
      }
   });
   f1.get();
   f2.get();
}
```

- [ ] A. El código siempre imprimirá `Tortoise` seguido de `Hare`.
- [ ] B. El código siempre imprimirá `Hare` seguido de `Tortoise`.
- [ ] C. Si el código imprime algo, el orden no puede determinarse.
- [ ] D. El código no compila.
- [ ] E. El código compila pero puede producir un interbloqueo en tiempo de ejecución.
- [ ] F. El código compila pero puede producir un bloqueo vivo en tiempo de ejecución.
- [ ] G. Compila pero lanza una excepción en tiempo de ejecución.

---

**15.** ¿Qué afirmación sobre el siguiente fragmento de código es correcta?

```Java
2: var cats = Stream.of("leopard", "lynx", "ocelot",
   "puma")
3:    .parallel();
4: var bears =
      Stream.of("panda","grizzly","polar").parallel();
5: var data = Stream.of(cats,bears).flatMap(s -> s)
6:    .collect(Collectors.groupingByConcurrent(
7:       s -> !s.startsWith("p")));
8: System.out.println(data.get(false).size()
9:    + " " + data.get(true).size());
```

- [ ] A. Imprime `3 4`.
- [ ] B. Imprime `4 3`.
- [ ] C. El código no compilará a causa de la línea 6.
- [ ] D. El código no compilará a causa de la línea 7.
- [ ] E. El código no compilará a causa de la línea 8.
- [ ] F. Compila pero lanza una excepción en tiempo de ejecución.

---

**16.** ¿Qué APIs existen para crear o trabajar con hilos de plataforma? (Se deben seleccionar todas las opciones que apliquen.)

- [ ] A. `Executors.newCachedThreadPool()`
- [ ] B. `Executors.newPlatformThreadPool()`
- [ ] C. `Executors.newPlatformThreadPerTaskExecutor()`
- [ ] D. `new Thread()`
- [ ] E. `Thread.ofPlatform()`
- [ ] F. `Thread.ofPlatformThread()`

---

**17.** ¿Qué afirmación sobre los métodos de `ReentrantLock` es correcta?

- [ ] A. El método `lock()` intentará adquirir un bloqueo sin esperar indefinidamente por él.
- [ ] B. El método `testLock()` intentará adquirir un bloqueo sin esperar indefinidamente por él.
- [ ] C. El método `attemptLock()` intentará adquirir un bloqueo sin esperar indefinidamente por él.
- [ ] D. Por omisión, un `ReentrantLock` cede de forma justa a cada hilo en el orden en que se solicitó.
- [ ] E. Llamar una sola vez al método `unlock()` liberará un recurso para que otros hilos puedan obtener el bloqueo.
- [ ] F. Ninguna de las anteriores.

---

**18.** ¿Cuáles de las siguientes expresiones lambda son expresiones `Callable` válidas? (Se deben seleccionar todas las opciones que apliquen.)

- [ ] A. `a -> {return 10;}`
- [ ] B. `() -> {String s = "";}`
- [ ] C. `() -> 5`
- [ ] D. `() -> {return null}`
- [ ] E. `() -> "The" + "Zoo"`
- [ ] F. `(int count) -> count+1`
- [ ] G. `() -> {System.out.print("Giraffe"); return 10;}`

---

**19.** ¿Cuál es el resultado de ejecutar la siguiente aplicación? (Se deben seleccionar todas las opciones que apliquen.)

```Java
import java.util.concurrent.*;
import java.util.stream.*;
public class PrintConstants {
   public static void main(String[] args) {
      var s = Executors.newVirtualThreadPerTaskExecutor();
      DoubleStream.of(3.14159, 2.71828)   // b1
         .forEach(c -> s.submit(          // b2
            () -> System.out.println(10 * c))); // b3
      s.execute(() -> System.out.println("Printed"));
   } }
```

- [ ] A. Compila e imprime los dos números seguidos de `Printed`.
- [ ] B. El código no compilará a causa de la línea b1.
- [ ] C. El código no compilará a causa de la línea b2.
- [ ] D. El código no compilará a causa de la línea b3.
- [ ] E. Compila, pero la salida no puede determinarse de antemano.
- [ ] F. Compila pero lanza una excepción en tiempo de ejecución.
- [ ] G. Compila pero espera para siempre en tiempo de ejecución.

---

**20.** ¿Cuál es el resultado de ejecutar el siguiente programa?

```Java
import java.util.*;
import java.util.concurrent.*;
import java.util.stream.*;
public class PrintCounter {
   static int count = 0;
   public static void main(String[] args) throws
      InterruptedException,
      ExecutionException {
      try (var service =
         Executors.newSingleThreadExecutor()) {
         var r = new ArrayList<Future<?>>();
         IntStream.iterate(0,i -> i + 1).limit(5).forEach(
            i -> r.add(service.execute(() -> {count++;}))
            // n1
         );
         for (Future<?> result : r) {
            System.out.print(result.get() + " "); // n2
         }
      }
   } }
```

- [ ] A. Imprime `0 1 2 3 4`.
- [ ] B. Imprime `1 2 3 4 5`.
- [ ] C. Imprime `null null null null null`.
- [ ] D. Se bloquea indefinidamente en tiempo de ejecución.
- [ ] E. La salida no puede determinarse.
- [ ] F. El código no compilará a causa de la línea n1.
- [ ] G. El código no compilará a causa de la línea n2.

---

**21.** Dado el siguiente fragmento de código y las líneas en blanco p1 y p2, ¿qué valores **garantizan** que 1 **se imprima** en tiempo de ejecución? (Se deben seleccionar todas las opciones que apliquen.)

```Java
var data = List.of(List.of(1, 2),
   List.of(3, 4),
   List.of(5, 6));
data._______________ // p1
   .flatMap(s -> s.stream())
   .________________ // p2
   .ifPresent(System.out::print);
```

- [ ] A. `stream()` en la línea p1, `findFirst()` en la línea p2.
- [ ] B. `stream()` en la línea p1, `findAny()` en la línea p2.
- [ ] C. `parallelStream()` en la línea p1, `findAny()` en la línea p2.
- [ ] D. `parallelStream()` en la línea p1, `findFirst()` en la línea p2.
- [ ] E. El código no compila independientemente de lo que **se inserte** en los espacios en blanco.
- [ ] F. Ninguna de las anteriores.

---

**22.** Suponiendo que un minuto es tiempo suficiente para que las tareas enviadas al executor del servicio **se completen**, ¿cuál es el resultado de ejecutar `countSheep()`?

```Java
import java.util.concurrent.*;
import java.util.concurrent.atomic.*;
public class BedTime {
   private AtomicInteger s1 = new AtomicInteger(0); // w1
   private int s2 = 0;
   private void countSheep() throws InterruptedException {
      try (var service =
         Executors.newSingleThreadExecutor()) { // w2
         for (int i = 0; i < 100; i++)
            service.execute(() -> {
               s1.getAndIncrement(); s2++; }); // w3
         Thread.sleep(60_000);
         System.out.println(s1 + " " + s2);
      }
   }
   public static void main(String... nap)
      throws InterruptedException {
      new BedTime().countSheep();
   } }
```

- [ ] A. El método imprime de forma constante `100 99`.
- [ ] B. El método imprime de forma constante `100 100`.
- [ ] C. La salida **no puede determinarse** de antemano.
- [ ] D. El código no compilará a causa de la línea w1.
- [ ] E. El código no compilará a causa de la línea w2.
- [ ] F. El código no compilará a causa de la línea w3.
- [ ] G. Compila pero lanza una excepción en tiempo de ejecución.

---

**23.** ¿Cuál es el resultado de ejecutar la siguiente aplicación?

```Java
import java.util.concurrent.*;
import java.util.stream.*;
public class StockRoomTracker {
   public static void await(CyclicBarrier cb) { // j1
      try { cb.await(); } catch (Exception e) {}
   }
   public static void main(String[] args) {
      var cb = new CyclicBarrier(10,
         () -> System.out.println("Stock Room Full!")); // j2
      IntStream.iterate(1, i -> 1).limit(9).parallel()
         .forEach(i -> await(cb)); // j3
   } }
```

- [ ] A. Imprime `Stock Room Full!`.
- [ ] B. El código no compilará a causa de la línea j1.
- [ ] C. El código no compilará a causa de la línea j2.
- [ ] D. El código no compilará a causa de la línea j3.
- [ ] E. Compila pero lanza una excepción en tiempo de ejecución.
- [ ] F. Compila pero espera para siempre en tiempo de ejecución.

---

**24.** ¿Cuáles de las siguientes afirmaciones sobre la siguiente definición de clase son verdaderas? (Se deben seleccionar todas las opciones que apliquen.)

```Java
public final class TicketManager {
   private int tickets;
   private static TicketManager instance;
   private TicketManager() {}
   static synchronized TicketManager getInstance()
   {      // k1
      if (instance==null) instance = new TicketManager();
      // k2
      return instance;
   }
   public int getTicketCount() { return tickets; }
   public void addTickets(int value) {tickets +=
      value;} // k3
   public void sellTickets(int value) {
      synchronized (this)
      {                               // k4
         tickets -= value;
      } } }
```

- [ ] A. Compila sin problemas.
- [ ] B. El código no compilará a causa de la línea k2.
- [ ] C. El código no compilará a causa de la línea k3.
- [ ] D. Los bloqueos adquiridos en k1 y k4 recaen sobre el mismo objeto.
- [ ] E. La clase protege correctamente los datos `tickets` frente a condiciones de carrera.
- [ ] F. Como mucho **se creará** una instancia de `TicketManager` en una aplicación que use esta clase.

---

**25.** Suponiendo que **se proporciona** una implementación del método `performCount()` antes del tiempo de ejecución, ¿cuáles de los siguientes son resultados posibles de ejecutar la siguiente aplicación? (Se deben seleccionar todas las opciones que apliquen.)

```Java
import java.util.*;
import java.util.concurrent.*;
public class CountZooAnimals {
   public static void performCount(int animal) {
       // IMPLEMENTATION OMITTED
   }
   public static void printResults(Future<?> f) {
       try {
          System.out.println(f.get(1, TimeUnit.DAYS)); // o1
       } catch (Exception e) {
          System.out.println("Exception!");
       }
   }
   public static void main(String[] args) throws Exception
   {
       final var r = new ArrayList<Future<?>>();
       try (var s = Executors.newSingleThreadExecutor()) {
          for (int i = 0; i < 10; i++) {
             final int animal = i;
             r.add(s.submit(() -> performCount(animal))); // o2
          }
          r.forEach(f -> printResults(f));
       }
   } }
```

- [ ] A. Imprime un número 10 veces.
- [ ] B. Imprime un valor booleano 10 veces.
- [ ] C. Imprime un valor null 10 veces.
- [ ] D. Imprime `Exception!` 10 veces.
- [ ] E. El código no compilará a causa de la línea o1.
- [ ] F. El código no compilará a causa de la línea o2.
