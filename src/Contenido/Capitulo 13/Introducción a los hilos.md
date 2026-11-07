Este capítulo **comienza revisando** la terminología común asociada con los hilos. Un **hilo** **es** la unidad más pequeña de ejecución que el sistema operativo **puede administrar**. Un **proceso** **es** un grupo de hilos asociados que se ejecutan en el mismo entorno compartido. De ello **se deduce** que un proceso de un solo hilo **es** aquel que contiene exactamente un hilo, mientras que un proceso multihilo contiene uno o más hilos.

Por entorno compartido **se entiende** que los hilos del mismo proceso comparten el mismo espacio de memoria y pueden comunicarse directamente entre sí.

Dentro de una computadora, un sistema operativo (SO, _operating system_) **administra** los hilos del sistema operativo utilizando el hardware subyacente de la CPU. Java **ejecuta** procesos usando hilos de plataforma, que tienen una correspondencia uno a uno con los hilos del sistema operativo, como se muestra en la siguiente imagen.

La imagen **muestra** un solo proceso con tres hilos de plataforma. También **muestra** cómo estos se corresponden con un número arbitrario de *n* CPUs disponibles dentro del sistema.

La Máquina Virtual de Java (JVM) **crea** y **administra** dos tipos diferentes de hilos de plataforma. Un **hilo del sistema** lo crea la JVM y se ejecuta en segundo plano de la aplicación. Por ejemplo, la recolección de basura (_garbage collection_) la administra un hilo del sistema creado por la JVM. Alternativamente, un **hilo definido por el usuario** lo crea el desarrollador de la aplicación.

En este capítulo **se habla** mucho de las tareas y de su relación con los hilos. Una **tarea** **es** una unidad única de trabajo realizada por un hilo. Un hilo **puede completar** múltiples tareas independientes, pero solo una a la vez.

![[Hilos plataforma.jpeg]]

Por memoria compartida, en general **se hace** referencia a las variables estáticas, así como a las variables de instancia y locales pasadas a un hilo. Sí, finalmente **puedes ver** cómo las variables estáticas **pueden ser** útiles para realizar tareas complejas y multihilo. **Recuerda** del Capítulo 5, "Métodos", que los métodos y variables estáticos se definen en un solo objeto de clase. Por ejemplo, si un hilo actualiza el valor de una variable estática, esta información queda inmediatamente disponible para que otros hilos del proceso la lean.

### Comparando con los hilos virtuales

Los hilos de plataforma **suelen ser** ineficientes. **Son** como tener un mayordomo personal que se queda de pie por si necesitas algo. Si necesitas cosas constantemente, ese es un buen uso del tiempo del mayordomo. Para que un hilo de plataforma sea eficiente, **necesitas estar** usando intensamente la CPU.

Por el contrario, cuando vamos a un restaurante, hay un mesero al que se le asignan muchas mesas. Como no necesitamos que alguien se quede de pie mientras la comida se cocina y mientras comemos, este es un uso más eficiente del tiempo del mesero. El equivalente en Java de un solo mesero que atiende múltiples mesas **es** un **hilo portador** (_carrier thread_). Las mesas corresponden a los **hilos virtuales**, que requieren menos recursos que los hilos de plataforma, lo que **convierte** a los hilos virtuales en una buena opción cuando se espera tener que esperar recursos de E/R o de red.

Cada vez que el hilo virtual está listo para ejecutarse, espera a que haya un hilo portador disponible. El hilo virtual no obtiene automáticamente el mismo hilo portador. **Es** como cuando un mesero pasa y te vuelve a llenar el café o el agua. **No te dicen** que ¡solo tu mesero original puede hacerlo! La siguiente imagen **muestra** un hilo virtual ejecutándose en un hilo portador.

**Observa** cómo el otro hilo portador no está ejecutando ningún hilo virtual en ese momento. Los tres hilos virtuales bloqueados no están listos para continuar, así que no necesitan un hilo portador en este momento. No están ligados a hilos específicos del SO, lo que libera a los hilos del SO para que trabajen en otras tareas. (Gracias a Venkat Subramaniam por la analogía del mesero. ¡La extensión del mayordomo es toda nuestra!)

![[Hilos virtuales.jpeg]]

En la documentación, puedes ver que un hilo portador se menciona como un tipo de hilo de plataforma. Con esto **se pretende** transmitir que se ejecuta en el sistema operativo. Desde el punto de vista del código, los hilos de plataforma y los hilos portadores **son** cosas diferentes.

> **Escenario del mundo real: Plataforma frente a hilos virtuales**
>
> Para ver lo ligeros que son los hilos virtuales, vamos a compararlos con los hilos de plataforma. No te preocupes si no has visto estos métodos antes. Aprenderás sobre ellos más adelante en este capítulo. Supongamos que tenemos una tarea que tarda un segundo en ejecutarse:
>
> ```Java
> public class PlatformVsVirtual {
>    static void waitUp() {
>       try {
>          Thread.sleep(1_000);
>       } catch (InterruptedException e) {
>          throw new RuntimeException(e);
>       }
>    } }
> ```
>
> Ahora intentamos ejecutar un millón de hilos de plataforma. ¿Cuánto tiempo crees que tarda?
>
> ```Java
> public static void main(String[] args) throws
>       InterruptedException {
>    var threads = Stream.generate(() ->
>          Thread.ofPlatform()
>          .unstarted(PlatformVsVirtual::waitUp))
>          .limit(1_000_000)
>          .toList();
>    threads.forEach(Thread::start);
>    for (var t : threads)
>       t.join();
> }
> ```
>
> La respuesta es que puede que ni siquiera se ejecute. Los hilos de plataforma consumen suficientes recursos como para que el programa probablemente falle.
>
> ```
> Exception in thread "main" java.lang.OutOfMemoryError:
> unable to create native thread: possibly out of memory
> or process/resource limits reached
> ```
>
> Cambiar `Thread.ofPlatform()` por `Thread.ofVirtual()` permite que el programa se ejecute. El programa fue bastante rápido cuando lo probamos. ¡Es bastante bueno para un millón de hilos!

### Comprendiendo la concurrencia de hilos

La propiedad de ejecutar múltiples hilos y procesos al mismo tiempo **se conoce** como concurrencia. Un **planificador de hilos** **determina** qué hilos deberían estar ejecutándose en este momento. Por ejemplo, un planificador de hilos **puede emplear** una planificación round-robin en la que cada hilo disponible recibe un número igual de ciclos de CPU con los que ejecutarse, visitando los hilos en un orden circular.

Cuando el tiempo asignado a un hilo se completa pero el hilo no ha terminado de procesar, ocurre un **cambio de contexto**. Un cambio de contexto **es** el proceso de almacenar el estado actual de un hilo y, más tarde, restaurar el estado del hilo para continuar la ejecución. Dado que hay un costo asociado con el cambio de contexto debido al tiempo perdido y a tener que recargar el estado de un hilo, los planificadores de hilos inteligentes hacen lo posible por minimizar el número de cambios de contexto mientras mantienen la aplicación ejecutándose sin problemas.

Por último, un hilo puede interrumpir o suplantar a otro hilo si tiene una prioridad de hilo más alta. Una **prioridad de hilo** **es** un valor numérico asociado con un hilo que el planificador de hilos considera al determinar qué hilos deberían ejecutarse. La prioridad **se puede** establecer desde 1 (`Thread.MIN_PRIORITY`) hasta 10 (`Thread.MAX_PRIORITY`), ya sea antes de que se inicie un hilo o mientras se está ejecutando.

```Java
var thread1 = new Thread(() -> System.out.print("Super
Important"));
thread1.setPriority(Thread.MAX_PRIORITY);
thread1.start();

var thread2 = new Thread(() -> System.out.print("Less
Important"));
thread2.start();
thread2.setPriority(2);
```

**Observa** que la prioridad del hilo **es** solo una sugerencia y la JVM no garantiza el orden en que los hilos se ejecutarán. Por ejemplo, si hay suficientes recursos de hilo disponibles para todos los procesos, entonces todos los hilos se ejecutarán al mismo tiempo.

### Creando un hilo

Una de las formas más comunes de definir una tarea para un hilo **es usando** la interfaz `Runnable`, que no toma argumentos y no devuelve datos.

```Java
@FunctionalInterface public interface Runnable {
   void run();
}
```

Con esto, **es fácil crear e iniciar** un hilo. De hecho, **puedes hacerlo** en una sola línea de código usando la clase `Thread`:

```Java
Thread.ofPlatform().start(() -> System.out.print("Hello"));
System.out.print("World");
```

La primera línea **crea** un nuevo objeto generador de hilos de plataforma con el método `ofPlatform()`. Luego **inicia** el hilo con el método `start()`, pasando también una tarea `Runnable`, también conocida como el trabajo por hacer, como una expresión lambda. ¿Este código imprime HelloWorld o WorldHello? La respuesta **es** que no lo sabemos. Dependiendo de la prioridad/planificador del hilo, cualquiera de los dos es posible. **Recuerda** que el orden de ejecución de los hilos muchas veces no está garantizado. El examen **presenta comúnmente** preguntas en las que múltiples tareas se inician al mismo tiempo y debes determinar el resultado.

Hay varias formas de crear un hilo, como se muestra en la Tabla 13.1. **Presta atención** a cuáles son hilos de plataforma y cuáles son hilos virtuales.

**TABLA 13.1** Creando e iniciando un `Thread`

| **Código**                                                                         | **Tipo**   | **Descripción** |
| ---------------------------------------------------------------------------------- | ---------- | --------------- |
| `var builder = Thread.ofPlatform();`<br>`Thread thread = builder.start(runnable);` | Plataforma | Fábrica         |
| `var builder = Thread.ofVirtual();`<br>`Thread thread = builder.start(runnable);`  | Virtual    | Fábrica         |
| `Thread thread = new Thread(runnable);`<br>`thread.start();`                       | Plataforma | Constructor     |

Antes de Java 21, era aconsejable crear un hilo con la llamada al constructor `new Thread()`, en la que la tarea se define cuando se crea el hilo. A partir de Java 21, el método de fábrica **es preferible** ya que **queda más claro** qué tipo de hilo estás obteniendo. El método de fábrica **crea** un generador (_builder_), que te permite llamar a otros métodos para establecer atributos como el nombre.

Para los hilos de plataforma, **puedes establecer** una prioridad a través del objeto generador llamando a `priority()`. La prioridad de los hilos virtuales **es** siempre 5 (`Thread.NORM_PRIORITY`) y no se puede cambiar. **Llamar** a `setPriority()` en el `Thread` virtual recién creado no tiene ningún efecto.

> **Aplazando la tarea**
>
> **Observa** que `Thread.ofPlatform()` y `Thread.ofVirtual()` crean un generador que usas para crear un `Thread`. **Pasas** un `Runnable` usando el método `start()` o `unstarted()` dependiendo de si quieres que el hilo se ejecute ahora o más adelante. **Echa un vistazo** al siguiente fragmento de código, que inicia el hilo después de haberse establecido la tarea:
>
> ```Java
> Thread t = Thread.ofVirtual().unstarted(task);
> // Do some other stuff
> t.start();
> ```

Veamos un ejemplo más complejo. ¿Cuál es la salida de esto?

```Java
12: public static void main(String[] args)
13:       throws InterruptedException {
14:    Runnable printInventory =
15:       () -> System.out.println("Printing zoo inventory");
16:    Runnable printRecords = () -> {
17:       for (int i = 0; i < 3; i++)
18:          System.out.println("Printing record: " + i);
19:    };
20:    System.out.println("begin");
21:    var platformThread = Thread.ofPlatform()
22:       .priority(10)
23:       .start(printInventory);
24:    var virtualThread = Thread.ofVirtual()
25:       .start(printRecords);
26:    var constructorThread = new Thread(printInventory);
27:    constructorThread.start();
28:    System.out.println("end");
29:    platformThread.join();
30:    virtualThread.join();
31:    constructorThread.join();
32: }
```

La respuesta **es** que el orden se desconoce hasta el tiempo de ejecución. Lo siguiente **es** solo una salida posible:

```
begin
Printing record: 0
Printing zoo inventory
end
Printing record: 1
Printing zoo inventory
Printing record: 2
```

Esta muestra **usa** un total de cuatro hilos: el hilo de usuario `main()` y tres hilos adicionales creados en las líneas 21–25. Cada hilo creado en estas líneas se ejecuta como una tarea **asíncrona**. Por asíncrono, **queremos decir** que el hilo que ejecuta el método `main()` no espera los resultados de cada hilo recién creado antes de continuar. Lo opuesto de este comportamiento **es** una tarea **síncrona** en la que el programa espera (o se bloquea) en la línea 20 a que el hilo termine de ejecutarse antes de pasar a la siguiente línea. Hasta ahora, la gran mayoría de las llamadas a métodos usadas en este libro han sido síncronas.

**Recuerda** que la llamada a `priority()` es una sugerencia, así que no puedes asumir que influye en el orden de los hilos.

Si bien el orden de ejecución de los hilos **es indeterminado** una vez que los hilos se han iniciado, el orden dentro de un solo hilo sigue siendo lineal. En particular, el bucle `for()` todavía está ordenado. También, begin siempre aparece antes que end.

Los métodos `join()` en las líneas 29–31 **le indican** al método `main()` que no termine antes de que los tres hilos se hayan completado. El método `join()` **lanza** una `InterruptedException` si falla, lo cual el método `main()` declara.

> **Llamando a `run()` en lugar de `start()`**
>
> En el examen, **ten cuidado** con el código que intenta iniciar un hilo llamando a `run()` en lugar de `start()`. **Llamar** a `run()` en un `Thread` o en un `Runnable` no inicia un nuevo hilo. Si bien el siguiente fragmento de código se compilará, se ejecuta de forma síncrona en lugar de iniciar un hilo:
>
> ```Java
> new Thread(printInventory).run();
> ```

### Trabajando con hilos demonio

Un **hilo demonio** (_daemon thread_) **es** aquel que no evitará que la JVM salga cuando el programa termine. Una aplicación de Java termina cuando los únicos hilos que se están ejecutando son hilos demonio. Por ejemplo, si la recolección de basura es el único hilo que queda ejecutándose, la JVM se cerrará automáticamente.

**Veamos** un ejemplo. ¿Qué crees que imprime esto?

```Java
1: public class Zoo {
2:    public static void pause() {                  // Define la tarea del hilo
3:       try {
4:          Thread.sleep(10_000);                  // Espera de 10 segundos
5:       } catch (InterruptedException e) {}
6:       System.out.println("Thread finished!");
7:    }
8:
9:    public static void main(String[] unused) {
10:       var job = Thread.ofPlatform().start(Zoo::pause);
11:       System.out.println("Main method finished!");
12:    } }
```

El programa **imprimirá** dos instrucciones con aproximadamente 10 segundos de diferencia:

```
Main method finished!
< 10 second wait >
Thread finished!
```

Eso **es correcto**. Aunque el método `main()` ya terminó, la JVM esperará a que el hilo de usuario termine antes de finalizar el programa. ¿Qué pasa si cambiamos `job` para que sea un hilo demonio agregando esto a la línea 11?

```Java
10: var job = Thread.ofPlatform().daemon(true).start(Zoo::pause);
```

El programa **imprimirá** la primera instrucción y terminará sin llegar nunca a imprimir la segunda línea.

```
Main method finished!
```

> Los hilos virtuales siempre son demonios. Los hilos de plataforma son por defecto no demonios, pero esto se puede cambiar.

### Administrando el ciclo de vida de un hilo

Después de que se ha creado un objeto `Thread`, este se encuentra en uno de seis estados, que se muestran en la siguiente imagen. **Puedes consultar** el estado de un hilo llamando a `getState()` en el objeto del hilo.

Cada hilo se inicializa con un estado `NEW`. Tan pronto como se llama a `start()`, el hilo pasa a un estado `RUNNABLE`. ¿Eso significa que realmente se está ejecutando? No exactamente: **puede que se esté** ejecutando, o puede que no. El estado `RUNNABLE` solo significa que el hilo es capaz de ejecutarse. Una vez que el trabajo del hilo se completa o se lanza una excepción no capturada, el estado del hilo se convierte en `TERMINATED` y no se realiza más trabajo.

![[Estados de un hilo.jpeg]]

Mientras está en un estado `RUNNABLE`, el hilo puede pasar a uno de tres estados donde pausa su trabajo: `BLOCKED`, `WAITING` o `TIMED_WAITING`. En estos estados, un hilo no está usando ningún recurso de CPU (además de llevar la cuenta de un temporizador en el caso de `TIMED_WAITING`). La imagen anterior **incluye** transiciones comunes entre estados de hilos, pero hay otras posibilidades. Por ejemplo, un hilo en estado `WAITING` podría ser activado por `notifyAll()`.

Una vez que un hilo entra en un estado de espera, otro hilo **puede llamar** a `interrupt()` en el hilo, causando que vuelva a un estado `RUNNABLE`. La mayoría de los métodos que causan que un hilo espere también declaran `InterruptedException`.

```Java
var thread = Thread.ofPlatform().start(() -> {
   try {
      Thread.sleep(1000);
   } catch (InterruptedException e) {
      System.out.println("Interrupted!");
   }});
thread.interrupt();
```

Este código **imprime** Interrupted! y luego **reanuda** la ejecución. ¿Qué pasa si se llama a `interrupt()` en un hilo que ya está en el estado `RUNNABLE`? En este caso, no se lanzará ninguna excepción. El hilo **puede consultar** periódicamente `Thread.interrupted()` para determinar si se ha enviado una interrupción recientemente.

```Java
var thread = Thread.ofPlatform().start(() -> {
   while(true) {
      if(Thread.interrupted())
         System.out.println("Someone interrupted us!");
      }});
thread.interrupt();
```

En este capítulo **se cubren** algunas (pero no todas) de las transiciones de la imagen anterior. Si se llama en un hilo una operación que no es compatible, como interrumpir un hilo suspendido, se lanzará una `IllegalThreadStateException`. Algunos métodos relacionados con hilos, como `wait()` y `notify()`, están fuera del alcance del examen y, francamente, son difíciles de usar bien. **Deberías** evitarlos y usar la API de Concurrencia tanto como sea posible. **Se requiere** una gran cantidad de habilidad (¡y algo de suerte!) para usar estos métodos correctamente.

### Repasando los conceptos de hilos

Esta sección **concluye** con la lista de terminología que deberías conocer en la Tabla 13.2. **Presta mucha atención** a los diferentes tipos de hilos, ya que los usaremos a lo largo de todo el capítulo.

**TABLA 13.2** Terminología de hilos en Java

| **Término** | **Descripción** |
|---|---|
| Hilo portador | Hilo del sistema que ejecuta los hilos virtuales cuando no están bloqueados |
| Hilo demonio | Hilo que no evitará que la JVM salga cuando el programa termine |
| Hilo de plataforma | Hilo que es administrado por el sistema operativo |
| Proceso | Grupo de hilos asociados que se ejecutan en el mismo entorno compartido |
| Hilo del sistema | Hilo creado por la JVM que se ejecuta en segundo plano de la aplicación, como el recolector de basura |
| Tarea | Unidad única de trabajo realizada por un hilo |
| Hilo | Unidad más pequeña de ejecución que puede ser administrada |
| Hilo definido por el usuario | Hilo creado por el desarrollador de la aplicación para llevar a cabo una tarea específica |
| Hilo virtual | Hilo ligero que se mapea a un hilo portador cuando se necesita ejecutarlo |
