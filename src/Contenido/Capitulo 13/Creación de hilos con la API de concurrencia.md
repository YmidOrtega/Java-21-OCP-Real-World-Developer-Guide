Java **incluye** el paquete `java.util.concurrent`, al que **nos referimos** como la API de Concurrencia, para manejar el complicado trabajo de gestionar hilos por ti. La API de Concurrencia **incluye** la interfaz `ExecutorService`, que define servicios que crean y gestionan hilos.

Primero **se obtiene** una instancia de la interfaz `ExecutorService` y luego **se envían** al servicio las tareas que deben procesarse. El marco de trabajo **incluye** numerosas funciones útiles, como el grupo de hilos (_thread pool_) y la programación de tareas. **Se recomienda** usar este marco de trabajo siempre que necesites crear y ejecutar una tarea independiente, incluso si solo necesitas un solo hilo.

### Introducción al executor de un solo hilo

Ya que `ExecutorService` es una interfaz, ¿cómo **se obtiene** una instancia de ella? La API de Concurrencia **incluye** la clase fábrica `Executors`, que **se puede** usar para crear instancias del objeto `ExecutorService`. **Volvamos** a escribir nuestro ejemplo anterior con las dos instancias `Runnable` para usar un `ExecutorService`.

```Java
try (ExecutorService service =
   Executors.newSingleThreadExecutor()) {
   System.out.println("begin");
   service.execute(printInventory);
   service.execute(printRecords);
   service.execute(printInventory);
   System.out.println("end");
}
```

En este ejemplo, **usamos** el método `newSingleThreadExecutor()` para crear el servicio. A diferencia de nuestro ejemplo anterior, en el que teníamos cuatro hilos (uno de `main()` y tres hilos nuevos), ahora solo tenemos dos hilos (uno de `main()` y un hilo nuevo). Esto **significa** que la salida, aunque sigue siendo impredecible, tendrá menos variación que antes. Por ejemplo, la siguiente **es** una salida posible:

```
begin
Printing zoo inventory
Printing record: 0
Printing record: 1
end
Printing record: 2
Printing zoo inventory
```

**Observa** que el bucle `printRecords` ya no se interrumpe con otras tareas `Runnable` enviadas al `executor` de hilos. Con un `executor` de un solo hilo, **se garantiza** que las tareas se ejecuten de forma secuencial. **Observa** que el texto `end` se muestra mientras las tareas de nuestro `executor` de hilos siguen en ejecución. Esto **se debe** a que el método `main()` sigue siendo un hilo independiente del `ExecutorService`.

### Enviando tareas

**Se pueden** enviar tareas a una instancia de `ExecutorService` de varias maneras. El método `execute()` toma una instancia `Runnable` y completa la tarea de forma asincrónica. Dado que el tipo de retorno del método es `void`, no nos dice nada sobre el resultado de la tarea. **Se considera** un método de "disparar y olvidar" (_fire-and-forget_), ya que, una vez enviado, los resultados no están directamente disponibles para el hilo que lo invoca.

Por suerte, los creadores de Java **agregaron** los métodos `submit()` a la interfaz `ExecutorService`, que, al igual que `execute()`, **se pueden** usar para completar tareas de forma asincrónica. A diferencia de `execute()`, sin embargo, `submit()` devuelve una instancia `Future` que **se puede** usar para determinar si la tarea está completa. También **se puede** usar para devolver un objeto genérico de resultado después de que la tarea se haya completado.

La Tabla 13.3 **muestra** los cinco métodos, incluidos `execute()` y los dos métodos `submit()`, que **debes** conocer para el examen. **No te preocupes** si no has visto `Future` ni `Callable` antes; los discutimos en detalle en la siguiente sección.

**TABLA 13.3** Métodos de `ExecutorService`

| **Nombre del método**                                                    | **Descripción**                                                                                                                                                      |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `void execute(Runnable command)`                                         | Ejecuta la tarea `Runnable` en algún momento del futuro.                                                                                                             |
| `Future<?> submit(Runnable task)`                                        | Ejecuta la tarea `Runnable` en algún momento del futuro y devuelve un `Future` que representa la tarea.                                                              |
| `<T> Future<T> submit(Callable<T> task)`                                 | Ejecuta la tarea `Callable` en algún momento del futuro y devuelve un `Future` que representa los resultados pendientes de la tarea.                                 |
| `<T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks)` | Ejecuta las tareas indicadas y espera a que todas se completen. Devuelve una lista de instancias `Future` en el mismo orden en que estaban en la colección original. |
| `<T> T invokeAny(Collection<? extends Callable<T>> tasks)`               | Ejecuta las tareas indicadas y espera a que al menos una se complete.                                                                                                |

> **Consejo:** Para el examen, **hay que** estar familiarizado tanto con `execute()` como con `submit()`, pero en tu propio código **se recomienda** usar `submit()` en lugar de `execute()` siempre que sea posible, ya que admite `Callable` y, por lo tanto, un valor de retorno opcional.

### Esperando los resultados

¿Cómo sabemos cuándo está completa una tarea enviada a un `ExecutorService`? Como **se mencionó** en la sección anterior, el método `submit()` devuelve una instancia `Future<?>` que **se puede** usar para determinar este resultado.

```Java
Future<?> future = service.submit(() -> System.out.println("Hello"));
```

El tipo `Future` **es** en realidad una interfaz. Para el examen, no **se necesita** saber ninguna de las clases que implementan `Future`, solo que diversos métodos de la API devuelven una instancia `Future`. La Tabla 13.4 **incluye** los métodos más útiles.

**TABLA 13.4** Métodos de `Future`

| **Nombre del método**                           | **Descripción**                                                                                                                                                                                     |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `boolean isDone()`                              | Devuelve `true` si la tarea se completó, lanzó una excepción o fue cancelada.                                                                                                                       |
| `boolean isCancelled()`                         | Devuelve `true` si la tarea fue cancelada antes de completarse normalmente.                                                                                                                         |
| `boolean cancel(boolean mayInterruptIfRunning)` | Intenta cancelar la ejecución de la tarea y devuelve `true` si se canceló correctamente o `false` si no se pudo cancelar o ya está completa.                                                        |
| `V get()`                                       | Recupera el resultado de la tarea, esperando indefinidamente si aún no está disponible.                                                                                                             |
| `V get(long timeout, TimeUnit unit)`            | Recupera el resultado de la tarea, esperando la cantidad de tiempo especificada. Si el resultado no está listo cuando se alcanza el tiempo de espera, se lanzará una `TimeoutException` verificada. |

El siguiente ejemplo usa una instancia `Future` para esperar los resultados:

```Java
import java.util.concurrent.*;
public class CheckResults {
   private static int counter = 0;
   public static void main(String[] unused) throws Exception {
      try (var service = Executors.newSingleThreadExecutor()) {
         Future<?> result = service.submit(() -> {
            for (int i = 0; i < 1_000_000; i++) counter++;
         });
         result.get(10, TimeUnit.SECONDS); // Returns null for Runnable
         System.out.println("Reached!");
      } catch (TimeoutException e) {
         System.out.println("Not reached in time");
      } } }
```

**Observa** que este ejemplo no usa la clase `Thread` directamente. En parte, esta **es** la esencia de la API de Concurrencia: hacer cosas complejas con hilos sin tener que gestionar hilos directamente. Este código también espera como máximo 10 segundos, lanzando una `TimeoutException` en la llamada a `result.get()` si la tarea no está terminada.

¿Cuál es el valor de retorno de esta tarea? Dado que `Future<V>` es una interfaz genérica, el tipo `V` se determina por el tipo de retorno del método `Runnable`. Dado que el tipo de retorno de `Runnable.run()` es `void`, el método `get()` siempre devuelve `null` cuando se trabaja con expresiones `Runnable`.

El método `Future.get()` **puede tomar** un valor opcional y el tipo enumerado `java.util.concurrent.TimeUnit`. La Tabla 13.5 **lista** la lista completa de valores de `TimeUnit`, ya que numerosos métodos de la API de Concurrencia usan este enumerado.

**TABLA 13.5** Valores de `TimeUnit`

| **Nombre del enumerado** | **Descripción** |
| --- | --- |
| `TimeUnit.NANOSECONDS` | Tiempo en milmillonésimas de segundo (1/1,000,000,000) |
| `TimeUnit.MICROSECONDS` | Tiempo en millonésimas de segundo (1/1,000,000) |
| `TimeUnit.MILLISECONDS` | Tiempo en milésimas de segundo (1/1,000) |
| `TimeUnit.SECONDS` | Tiempo en segundos |
| `TimeUnit.MINUTES` | Tiempo en minutos |
| `TimeUnit.HOURS` | Tiempo en horas |
| `TimeUnit.DAYS` | Tiempo en días |

> **Consulta periódica con `sleep`**
>
> La consulta periódica (_polling_) **es** el proceso de verificar datos de forma intermitente en algún intervalo fijo. **Puedes encontrar** código que usa `Thread.sleep()` dentro de un bucle con este propósito en código antiguo. ¡Es mucho mejor usar un `Future` y dejar que Java se encargue de la verificación por ti!

### Analizando `Callable`

La interfaz funcional `java.util.concurrent.Callable` **es** similar a `Runnable`, excepto que su método `call()` devuelve un valor y puede lanzar una excepción verificada. La siguiente **es** la definición de la interfaz `Callable`:

```Java
@FunctionalInterface public interface Callable<V> {
   V call() throws Exception;
}
```

Cuando se pasa un `Callable<V>` a un `ExecutorService` mediante `submit()`, se devuelve un objeto `Future<V>`. Una vez que la tarea esté completa, llamar a `get()` en el `Future<V>` devolverá el resultado de tipo `V`. Esto te **permite averiguar** mucha información sobre los resultados de la tarea.

> **Nota:** El método `submit()` de `ExecutorService` toma un `Runnable` y devuelve un objeto `Future<?>`. Este objeto **se puede** usar para comprobar si el hilo ha terminado. Pero, dado que las tareas `Runnable` tienen un tipo de retorno `void`, llamar a `get()` en un `Future` así devolverá siempre `null` cuando la tarea se complete correctamente.

La interfaz `Callable` a menudo **es preferible** a `Runnable`, ya que permite recuperar fácilmente más detalles de la tarea después de que se haya completado. Dicho esto, **usamos** ambas interfaces, ya que son intercambiables en situaciones en las que la lambda no lanza una excepción y no hay tipo de retorno.

Por suerte, `ExecutorService` **incluye** una versión sobrecargada del método `submit()` que toma un objeto `Callable` y devuelve una instancia genérica `Future<T>`.

**Veamos** un ejemplo que usa `Callable`:

```Java
try (var service = Executors.newSingleThreadExecutor()) {
   Future<Integer> result = service.submit(() -> 30 + 11);
   System.out.println(result.get());   // 41
}
```

Esta implementación **es más fácil de codificar y de entender** que si hubiéramos usado un `Runnable`, algún objeto compartido y una llamada a `interrupt()` o una espera con tiempo. En esencia, ese **es** el espíritu de la API de Concurrencia: darte las herramientas para escribir código multihilo que sea seguro para hilos (_thread-safe_), con buen rendimiento y fácil de seguir.

### Deteniendo un executor de hilos

**Puede que hayas notado** que el `ExecutorService` se ha declarado en un bloque try-with-resources, como aprendiste en el Capítulo 11, "Excepciones y Localización". Un `executor` de hilos crea un hilo no demonio con la primera tarea que se ejecuta, así que olvidar hacer esto hará que tu aplicación nunca termine. ¿No nos crees? ¡**Intenta ejecutar** lo siguiente!

```Java
public class MissingClose {
   public static void main(String[] args) {
      var service = Executors.newSingleThreadExecutor();
      service.submit(() -> System.out.println("Never stops"));
   } 
}
```

Este código se ejecuta pero nunca termina, porque el `ExecutorService` nunca se apaga ni se cierra. La solución **es usar** siempre un `ExecutorService` con un bloque try-with-resources. El bloque try-with-resources **llama automáticamente** a `close()`, que apaga el servicio executor para que no se acepten más tareas. Luego espera a que todas completen su ejecución.

**Usar** un `ExecutorService` en un try-with-resources **se encarga de garantizar** que las tareas se completen. Sin embargo, el método `close()` solo se agregó a esta API en Java 19. En código más antiguo, verías una llamada explícita a `shutdown()`. O tal vez quieras decirle a las tareas que terminen con `shutdownNow()`. En estos escenarios, no usarías el try-with-resources. La Tabla 13.6 **describe** el comportamiento de estas opciones.

**TABLA 13.6** Estados de un executor de hilos

| **Situación**   | **Descripción**                                                               | **¿Se aceptan más tareas?** | **`isShutdown()`** | **`isTerminated()`**                                                       |
| --------------- | ----------------------------------------------------------------------------- | --------------------------- | ------------------ | -------------------------------------------------------------------------- |
| **Activa**      | Acepta tareas                                                                 | Sí                          | `false`            | `false`                                                                    |
| `shutdown()`    | Ejecuta las tareas pendientes hasta que se completen, pero no acepta más      | No                          | `true`             | `false` mientras las tareas se ejecutan; `true` cuando las tareas terminan |
| `close()`       | Llama a `shutdown()` y luego espera la terminación de las tareas en ejecución | No                          | `true`             | `true`                                                                     |
| `shutdownNow()` | Detiene las tareas en ejecución y cancela las tareas pendientes               | No                          | `true`             | `false` mientras las tareas se ejecutan; `true` cuando las tareas terminan |

La imagen que se **muestra** en la siguiente sesión, muestra cómo `ExecutorService` separa los estados de apagado y terminado.

Cuando se está apagando y las tareas se están ejecutando, `isShutdown()` **puede devolver** `true`, mientras que `isTerminated()` puede devolver `false`. Una vez que las tareas terminan, ambos métodos devuelven `true`.

### Programando tareas

A menudo en Java, **necesitamos programar** una tarea para que ocurra en algún momento del futuro. Incluso podríamos necesitar programar la tarea para que ocurra repetidamente, en algún intervalo establecido. Por ejemplo, imagina que queremos revisar el suministro de comida para los animales del zoológico una vez por hora y rellenarlo según sea necesario.

![[Ciclo de vida del ejecutor de hilos.jpeg]]

`ScheduledExecutorService`, que **es** una subinterfaz de `ExecutorService`, **se puede** usar justamente para una tarea como esta.

Al igual que con `ExecutorService`, **obtenemos** una instancia de `ScheduledExecutorService` usando un método fábrica de la clase `Executors`, como **se muestra** en el siguiente fragmento:

```Java
ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();
```

**Consulta** la Tabla 13.7 para nuestro resumen de los métodos de `ScheduledExecutorService`. Cada uno de estos métodos devuelve un objeto `ScheduledFuture`.

**TABLA 13.7** Métodos de `ScheduledExecutorService`

| **Nombre del método** | **Descripción** |
| --- | --- |
| `schedule(Callable<V> callable, long delay, TimeUnit unit)` | Crea y ejecuta la tarea `Callable` después del retardo indicado. |
| `schedule(Runnable command, long delay, TimeUnit unit)` | Crea y ejecuta la tarea `Runnable` después del retardo indicado. |
| `scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit)` | Crea y ejecuta la tarea `Runnable` después del retardo inicial indicado, creando una tarea nueva cada vez que pasa el valor de periodo. |
| `scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit)` | Crea y ejecuta la tarea `Runnable` después del retardo inicial indicado y posteriormente con el retardo indicado entre la terminación de una ejecución y el comienzo de la siguiente. |

En la práctica, estos métodos **se encuentran** entre los más convenientes de la API de Concurrencia, ya que realizan tareas relativamente complejas con una sola línea de código. Los parámetros `delay` y `period` dependen del argumento `TimeUnit` para determinar el formato del valor, como segundos o milisegundos.

Los dos primeros métodos `schedule()` de la Tabla 13.7 toman un `Callable` o un `Runnable`, respectivamente; realizan la tarea después de cierto retardo; y devuelven una instancia `ScheduledFuture`. La interfaz `ScheduledFuture` **es** idéntica a la interfaz `Future`, excepto que incluye un método `getDelay()` que devuelve el retardo restante. El siguiente ejemplo usa el método `schedule()` con tareas `Callable` y `Runnable`:

```Java
try (var service =
   Executors.newSingleThreadScheduledExecutor()) {
   Runnable task1 = () -> System.out.println("Hello Zoo");
   Callable<String> task2 = () -> "Monkey";
   ScheduledFuture<?> r1 = service.schedule(task1, 10,
      TimeUnit.SECONDS);
   ScheduledFuture<?> r2 = service.schedule(task2,
      8, TimeUnit.MINUTES);
}
```

La primera tarea se programa a 10 segundos en el futuro, mientras que la segunda tarea se programa a 8 minutos en el futuro.

> **Nota:** Aunque estas tareas están programadas para el futuro, la ejecución real puede retrasarse. Por ejemplo, puede que no haya hilos disponibles para realizar las tareas, en cuyo caso simplemente esperarán en la cola. Además, si el `ScheduledExecutorService` se ha apagado por completo cuando se alcanza la hora de ejecución de la tarea programada, estas tareas serán descartadas.

Cada uno de los métodos de `ScheduledExecutorService` **es importante** y tiene aplicaciones en el mundo real. Por ejemplo, **puedes usar** el comando `schedule()` para revisar el estado de la limpieza de la jaula de un león. Luego puede enviar notificaciones si no ha terminado, o incluso llamar a `schedule()` para revisar de nuevo más tarde.

Los dos últimos métodos de la Tabla 13.7 **pueden ser** un poco confusos si no los has visto antes. Conceptualmente, **son similares**, ya que ambos realizan la misma tarea repetidamente después de un retardo inicial. La diferencia está relacionada con el momento del proceso y con cuándo comienza la siguiente tarea.

El método `scheduleAtFixedRate()` **crea** una tarea nueva y la envía al executor cada periodo, independientemente de si la tarea anterior terminó o no. El siguiente ejemplo ejecuta una tarea `Runnable` cada minuto, después de un retardo inicial de cinco minutos:

```Java
service.scheduleAtFixedRate(task1, 5, 1, TimeUnit.MINUTES);
```

El método `scheduleAtFixedRate()` **es útil** para tareas que necesitan ejecutarse en intervalos específicos, como revisar la salud de los animales una vez al día. Aunque examinar un animal lleve dos horas el lunes, eso no significa que el examen del martes deba empezar más tarde en el día.

> **Nota:** Pueden pasar cosas malas con `scheduleAtFixedRate()` si cada tarea tarda consistentemente más en ejecutarse que el intervalo de ejecución. Imagina que tu jefe pasara por tu escritorio cada minuto y dejara una hoja de papel. Ahora imagina que te tardaras cinco minutos en leer cada hoja de papel. En poco tiempo, te estarías ahogando en pilas de papel. Así se siente un executor. Con el tiempo suficiente, el programa enviaría más tareas al servicio executor de las que cabrían en la memoria, haciendo que el programa se bloquee.

Por otro lado, el método `scheduleWithFixedDelay()` **crea** una tarea nueva solo después de que la tarea anterior haya terminado. Por ejemplo, si una tarea se ejecuta a las 12:00 y tarda cinco minutos en terminar, con un periodo entre ejecuciones de dos minutos, la siguiente tarea empezará a las 12:07.

```Java
service.scheduleWithFixedDelay(task1, 0, 2, TimeUnit.MINUTES);
```

El método `scheduleWithFixedDelay()` **es útil** para procesos que quieras que ocurran repetidamente pero cuyo momento específico no importa. Por ejemplo, imagina que tenemos un empleado de la cafetería del zoológico que reabastece periódicamente la barra de ensaladas a lo largo del día. El proceso puede llevar 20 minutos o más, ya que requiere que el empleado transporte una gran cantidad de artículos desde la trastienda. Una vez que el empleado ha llenado la barra de ensaladas con comida fresca, no necesita revisar en un momento específico, solo después de que haya pasado el tiempo suficiente para que vuelva a quedarse con pocas existencias.

### Aumentando la concurrencia con grupos de hilos

Todos nuestros ejemplos hasta ahora **han sido** con un `executor` de un solo hilo, que, aunque interesantes, no eran particularmente útiles. Después de todo, el nombre de este capítulo **es** "Concurrencia", ¡y no puedes hacer mucha de eso con un `executor` de un solo hilo!

Por suerte, la clase `Executors` **incluye** una variedad de métodos que actúan sobre un grupo de hilos (_thread pool_). Un grupo de hilos **es** un grupo de hilos reutilizables preinstanciados que están disponibles para realizar un conjunto arbitrario de tareas. La Tabla 13.8 **incluye** nuestros dos métodos de `executor` de un solo hilo anteriores, junto con los nuevos que debes conocer para el examen.

**TABLA 13.8** Métodos fábrica de `Executors`

| **Nombre del método** | **Descripción** |
| --- | --- |
| `ExecutorService newSingleThreadExecutor()` | Crea un executor de un solo hilo que usa un solo hilo de plataforma trabajador que opera a partir de una cola ilimitada. Los resultados se procesan secuencialmente en el orden en que se enviaron. |
| `ScheduledExecutorService newSingleThreadScheduledExecutor()` | Crea un executor de un solo hilo para hilos de plataforma que puede programar comandos para ejecutarse después de un retardo dado o ejecutarse periódicamente. |
| `ExecutorService newCachedThreadPool()` | Crea un grupo de hilos de plataforma que crea hilos nuevos según sea necesario pero reutiliza hilos construidos previamente cuando están disponibles. |
| `ExecutorService newFixedThreadPool(int)` | Crea un grupo de hilos de plataforma que reutiliza un número fijo de hilos que operan a partir de una cola compartida ilimitada. |
| `ScheduledExecutorService newScheduledThreadPool(int)` | Crea un grupo de hilos de plataforma que puede programar comandos para ejecutarse después de un retardo dado o ejecutarse periódicamente. |
| `ExecutorService newVirtualThreadPerTaskExecutor()` | Crea un grupo de hilos que crea un hilo virtual nuevo para cada tarea. |

Como **se muestra** en la Tabla 13.8, estos métodos devuelven los mismos tipos de instancia, `ExecutorService` y `ScheduledExecutorService`, que usamos antes en este capítulo. En otras palabras, ¡todos nuestros ejemplos anteriores **son compatibles** con estos nuevos `executors` con grupo de hilos!

La diferencia entre un `executor` de un solo hilo y uno con grupo de hilos **es** lo que sucede cuando una tarea ya se está ejecutando. Mientras que un `executor` de un solo hilo esperará a que el hilo esté disponible antes de ejecutar la siguiente tarea, un executor con grupo de hilos puede ejecutar la siguiente tarea de forma concurrente. Si el grupo se queda sin hilos disponibles, la tarea **será encolada** por el `executor` de hilos y esperará a completarse.

Todas las filas de la Tabla 13.8 excepto la última son para hilos de plataforma. Los hilos virtuales tienen su propio método fábrica `newVirtualThreadPerTaskExecutor`. No **se agrupan** ya que son muy ligeros. Esto permite que se use un hilo virtual nuevo para cada tarea.
