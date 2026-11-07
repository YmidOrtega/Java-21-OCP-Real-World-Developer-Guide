La seguridad de hilos (_thread-safety_) **es** la propiedad de un objeto que garantiza una ejecución segura por múltiples hilos al mismo tiempo. Dado que los hilos se ejecutan en un entorno y un espacio de memoria compartidos, ¿cómo **evitamos** que dos hilos interfieran entre sí? **Hay que organizar** el acceso a los datos para no terminar con resultados inválidos o inesperados.

En esta parte del capítulo, **se muestra** cómo usar una variedad de técnicas para proteger datos, incluidas las clases atómicas, los bloques `synchronized`, el framework de bloqueos (_Lock framework_) y las barreras cíclicas (_cyclic barriers_).

### Comprensión de la seguridad de hilos

**Imagina** que nuestro zoológico tiene un programa para contar ovejas; ¡de preferencia uno que no duerma a los trabajadores del zoológico! Cada trabajador del zoológico sale corriendo hacia un campo, agrega una oveja nueva al rebaño, cuenta el número total de ovejas y vuelve a reportarnos los resultados. **Lo siguiente muestra** esto conceptualmente, eligiendo un tamaño de grupo de hilos (_thread pool_) para que todas las tareas **puedan ejecutarse** de forma concurrente:

```Java
 1: import java.util.concurrent.*;
 2: public class SheepManager {
 3:    private int sheepCount = 0;
 4:    private void incrementAndReport() {
 5:       System.out.print((++sheepCount) + " ");
 6:    }
 7:    public static void main(String[] args) {
 8:       try (var service = Executors.newFixedThreadPool(20)) {
 9:          SheepManager manager = new SheepManager();
10:          for (int i = 0; i < 10; i++)
11:             service.submit(() -> manager.incrementAndReport());
12:       } } }
```

¿Qué **produce** este programa? **Podrías pensar** que producirá los números del 1 al 10, en orden, pero eso está lejos de estar garantizado. Podría producirlos en un orden diferente. Peor aún, ¡podría imprimir algunos números dos veces y no imprimir algunos números en absoluto! Las siguientes son salidas posibles de este programa:

```
1 2 3 4 5 6 7 8 9 10
1 9 8 7 3 6 6 2 4 5
1 8 7 3 2 6 5 4 2 9
```

Entonces, ¿qué salió mal? En este ejemplo, **usamos** el operador de preincremento (`++`) para actualizar la variable `sheepCount`. **Ocurre un problema** cuando dos hilos leen el valor «antiguo» antes de que cualquiera de los dos escriba el valor «nuevo» de la variable. Las dos asignaciones se vuelven redundantes; ambas asignan el mismo valor nuevo, con un hilo sobrescribiendo los resultados del otro. La siguiente imagen **demuestra** este problema con dos hilos, suponiendo que `sheepCount` tiene un valor inicial de 1.

**Puedes ver** en la imagen anterior que ambos hilos leen y escriben los mismos valores, lo que hace que una de las dos operaciones `++sheepCount` se pierda. Por lo tanto, el operador de incremento `++` no es seguro para hilos (_thread-safe_). Como **se verá** más adelante en este capítulo, el resultado inesperado de dos o más tareas ejecutándose al mismo tiempo **se conoce** como condición de carrera (_race condition_).

En términos conceptuales, la idea aquí **es** que algunos trabajadores del zoológico podrían correr más rápido de camino al campo pero más lento de regreso y reportar tarde. Otros trabajadores podrían llegar al campo al final pero, de algún modo, ser los primeros en volver para reportar los resultados.

> **Nota:** El modificador `volatile` **se puede** usar en una variable de instancia para garantizar que un hilo no vea ningún valor intermedio mientras se está realizando una operación. Desafortunadamente, agregarlo a la variable `sheepCount` en nuestro ejemplo anterior es insuficiente para la seguridad de hilos, ya que `++sheepCount` es en realidad dos operaciones separadas de lectura y escritura. En la práctica, `volatile` rara vez **se usa**. Solo lo mencionamos porque se sabe que ha aparecido en el examen de vez en cuando.

![[Falta de sincronización entre hilos.jpeg]]

### Protección de datos con clases atómicas

En nuestra aplicación `SheepManager` anterior, los mismos valores a veces se imprimían dos veces, con un contador máximo de 9 en lugar de 10. **Como vimos**, el operador de incremento `++` no es seguro para hilos porque la operación no es atómica, llevando a cabo dos tareas, lectura y escritura, que pueden ser interrumpidas por otros hilos.

**Atómico** **es** la propiedad de una operación de llevarse a cabo como una sola unidad de ejecución sin ninguna interferencia de otro hilo. Una versión atómica y segura para hilos del operador de incremento **realizaría** la lectura y la escritura de la variable como una sola operación, sin permitir que ningún otro hilo acceda a la variable durante la operación. La siguiente imagen **muestra** el resultado de hacer atómica la variable `sheepCount`.

En este caso, cualquier hilo que intente acceder a la variable `sheepCount` mientras una operación atómica está en proceso **tendrá que esperar** hasta que la operación atómica sobre la variable se complete. En términos conceptuales, esto **equivale a establecer** una regla para nuestros trabajadores del zoológico de que solo puede haber un empleado en el campo a la vez, aunque no todos **puedan** reportar sus resultados en orden.

Dado que acceder a primitivos y referencias es común en Java, la API de concurrencia **incluye** numerosas clases útiles en el paquete `java.util.concurrent.atomic`. La tabla 13.9 **lista** las clases atómicas con las que **debes** estar familiarizado para el examen. Como con muchas de las clases de la API de concurrencia, estas clases existen para hacerte la vida más fácil.

![[Sincronización de hilos con operaciones atómicas.jpeg]]

**TABLA 13.9** Clases atómicas

| **Nombre de clase** | **Descripción** |
| --- | --- |
| `AtomicBoolean` | Un valor booleano que **se puede** actualizar de forma atómica |
| `AtomicInteger` | Un valor int que **se puede** actualizar de forma atómica |
| `AtomicLong` | Un valor long que **se puede** actualizar de forma atómica |

¿Cómo **usamos** una clase atómica? Cada clase **incluye** numerosos métodos que son equivalentes a muchos de los operadores integrados que usamos en los primitivos, como el operador de asignación (`=`) y los operadores de incremento (`++`). **Describimos** los métodos atómicos comunes que **debes** conocer para el examen en la tabla 13.10. El tipo lo determina la clase.

**TABLA 13.10** Métodos atómicos comunes

| **Nombre de método** | **Descripción** |
| --- | --- |
| `get()` | Recupera el valor actual |
| `set(type newValue)` | Establece el valor dado, equivalente al operador de asignación `=` |
| `getAndSet(type newValue)` | Establece de forma atómica el valor nuevo y devuelve el valor antiguo |
| `incrementAndGet()` | Para las clases numéricas, operación atómica de preincremento equivalente a `++value` |
| `getAndIncrement()` | Para las clases numéricas, operación atómica de postincremento equivalente a `value++` |
| `decrementAndGet()` | Para las clases numéricas, operación atómica de predecremento equivalente a `--value` |
| `getAndDecrement()` | Para las clases numéricas, operación atómica de postdecremento equivalente a `value--` |

En el siguiente ejemplo, **supón** que importamos el paquete atómico y luego actualizamos nuestra clase `SheepManager` con un `AtomicInteger`:

```Java
 3:    private AtomicInteger sheepCount = new AtomicInteger(0);
 4:    private void incrementAndReport() {
 5:       System.out.print(sheepCount.incrementAndGet() + " ");
 6:    }
```

¿Cómo **difiere** esta implementación de nuestros ejemplos anteriores? Cuando ejecutamos esta modificación, **obtenemos** una salida variable, como la siguiente:

```
2 3 1 4 5 6 7 8 9 10
1 4 3 2 5 6 7 8 9 10
1 4 3 5 6 2 7 8 10 9
```

A diferencia de nuestra salida de ejemplo anterior, los números del 1 al 10 siempre **se imprimirán**, aunque el orden todavía no está garantizado. **No te preocupes**; abordaremos ese asunto en breve. Lo clave en esta sección **es** que usar las clases atómicas garantiza que los datos sean consistentes entre los trabajadores y que no se pierdan valores debido a modificaciones concurrentes.

### Mejorando el acceso con bloques `synchronized`

Si bien las clases atómicas son excelentes para proteger una sola variable, no son particularmente útiles si necesitas ejecutar una serie de comandos o llamar a un método. Por ejemplo, no podemos usarlas para actualizar dos variables atómicas al mismo tiempo. ¿Cómo **mejoramos** los resultados para que cada trabajador pueda incrementar y reportar los resultados en orden?

La técnica más común **es usar** un monitor para sincronizar el acceso. Un monitor, también llamado bloqueo (_lock_), **es** una estructura que admite la exclusión mutua, que es la propiedad de que como máximo un hilo esté ejecutando un segmento particular de código en un momento dado.

En Java, cualquier objeto `Object` **se puede** usar como monitor, junto con la palabra clave `synchronized`, como se muestra en el siguiente ejemplo:

```Java
var manager = new SheepManager();
synchronized(manager) {
   // El trabajo debe realizarse un hilo cada vez
}
```

Este ejemplo **se conoce** como bloque `synchronized`. Cada hilo que llegue primero comprobará si ya hay hilos ejecutando el bloque. Si el bloqueo no está disponible, el hilo transicionará a un estado BLOQUEADO (_BLOCKED_) hasta que pueda «adquirir el bloqueo». Si el bloqueo está disponible (o el hilo ya posee el bloqueo), el hilo entrará al bloque, impidiendo que todos los demás hilos entren. Una vez que el hilo termina de ejecutar el bloque, liberará el bloqueo, permitiendo que uno de los hilos en espera proceda.

> **Nota:** Para sincronizar el acceso entre múltiples hilos, cada hilo debe tener acceso al mismo objeto `Object`. Si cada hilo se sincroniza en objetos diferentes, el código no es seguro para hilos.

**Volvamos** a nuestro ejemplo `SheepManager` que usaba `++sheepCount` y veamos si podemos mejorar los resultados para que cada trabajador incremente e imprima el contador en orden. **Supongamos** que reemplazamos nuestro bucle `for()` con la siguiente implementación:

```Java
10: for (int i = 0; i < 10; i++) {
11:    synchronized(manager) {
12:       service.submit(() -> manager.incrementAndReport());
13:    }
14: }
```

¿Esta solución arregla el problema? ¡No, no lo hace! ¿Puedes detectar el problema? Hemos sincronizado la creación de los hilos, pero no la ejecución de los hilos. En este ejemplo, los hilos se crearían de uno en uno, pero todos podrían seguir ejecutando y realizando su trabajo simultáneamente, resultando en el mismo tipo de salida que viste antes. ¡Ya dijimos que diagnosticar y resolver problemas de hilos es difícil en la práctica!

Esta **es** una versión corregida de la clase `SheepManager` que ordena a los trabajadores:

```Java
 1: import java.util.concurrent.*;
 2: public class SheepManager {
 3:    private int sheepCount = 0;
 4:    private void incrementAndReport() {
 5:       synchronized(this) {
 6:          System.out.print((++sheepCount) + " ");
 7:       }
 8:    }
 9:    public static void main(String[] args) {
10:       try (var service = Executors.newFixedThreadPool(20)) {
11:          var manager = new SheepManager();
12:          for (int i = 0; i < 10; i++)
13:             service.submit(() -> manager.incrementAndReport());
14:       } } }
```

Cuando este código se ejecuta, **producirá** de forma consistente lo siguiente:

```
1 2 3 4 5 6 7 8 9 10
```

Aunque todos los hilos todavía se crean y se ejecutan al mismo tiempo, cada uno espera en el bloque `synchronized` a que el trabajador incremente y reporte el resultado antes de entrar. De esta manera, cada trabajador del zoológico espera a que el trabajador anterior regrese antes de salir corriendo al campo. Si bien es aleatorio qué trabajador saldrá a continuación, está garantizado que como máximo habrá uno en el campo y que los resultados se reportarán en orden.

**Podríamos habernos** sincronizado en cualquier objeto, siempre que fuera el mismo objeto. Por ejemplo, el siguiente fragmento de código también funcionaría:

```Java
 4:    private final Object herd = new Object();
 5:    private void incrementAndReport() {
 6:       synchronized(herd) {
 7:          System.out.print((++sheepCount) + " ");
 8:       }
 9:    }
```

Aunque no **era necesario** hacer que la variable `herd` fuera `final`, hacerlo garantiza que no vuelva a asignarse después de que los hilos empiecen a usarla.

### Sincronización de métodos

En el ejemplo anterior, **establecimos** nuestro monitor usando `synchronized(this)` alrededor del cuerpo del método. Java **proporciona** una sintaxis más conveniente para hacerlo. **Podemos agregar** el modificador `synchronized` a cualquier método de instancia para sincronizar automáticamente en el propio objeto. Por ejemplo, las siguientes dos definiciones de método son equivalentes:

```Java
void sing() {
   synchronized(this) {
      System.out.print("La la la!");
   }
}
synchronized void sing() {
   System.out.print("La la la!");
}
```

La primera usa un bloque `synchronized`, mientras que la segunda usa el modificador de método `synchronized`. Cuál uses **depende** completamente de ti.

También **podemos aplicar** el modificador `synchronized` a métodos estáticos. ¿Qué objeto se usa como monitor cuando nos sincronizamos en un método estático? ¡El objeto de la clase, por supuesto! Por ejemplo, los siguientes dos métodos son equivalentes para la sincronización estática dentro de nuestra clase `SheepManager`:

```Java
static void dance() {
   synchronized(SheepManager.class) {
      System.out.print("Time to dance!");
   }
}
static synchronized void dance() {
   System.out.print("Time to dance!");
}
```

Como antes, la primera usa un bloque `synchronized`, con el segundo ejemplo usando el modificador `synchronized`. **Puedes usar** la sincronización estática si necesitas ordenar el acceso de hilos en todas las instancias en lugar de en una sola instancia.

### Comprensión del framework de bloqueos

Un bloque `synchronized` admite solo un conjunto limitado de funcionalidades. Por ejemplo, ¿qué pasa si queremos comprobar si un bloqueo está disponible y, si no lo está, realizar alguna otra tarea? Además, si el bloqueo nunca está disponible y nos sincronizamos en él, podríamos esperar para siempre.

La API de concurrencia **incluye** la interfaz `Lock`, que conceptualmente es similar a usar la palabra clave `synchronized`, pero con muchos más adornos.

#### Aplicando un `ReentrantLock`

La interfaz `Lock` **es** bastante fácil de usar. Cuando necesites proteger un trozo de código del procesamiento multihilo, **crea** una instancia de `Lock` a la que todos los hilos tengan acceso. Entonces, cada hilo llama a `lock()` antes de entrar al código protegido y llama a `unlock()` antes de salir del código protegido.

Para contraste, lo siguiente **muestra** dos implementaciones, una con un bloque `synchronized` y otra con una instancia de `Lock`. Si bien es más larga, la solución con `Lock` tiene varias características que no están disponibles para el bloque `synchronized`:

```Java
// Implementación #1 con un bloque sincronizado
var object = new Object();
synchronized(object) {
   // Código protegido
}

// Implementación #2 con un Lock
var myLock = new ReentrantLock();
try {
   myLock.lock();
   // Código protegido
} finally {
   myLock.unlock();
}
```

Estas dos implementaciones **son conceptualmente equivalentes**. La clase `ReentrantLock` **es** un monitor simple que implementa la interfaz `Lock` y admite la exclusión mutua. En otras palabras, se permite que como máximo un hilo posea un bloqueo en cualquier momento dado.

> **Consejo:** Si bien no **es** absolutamente necesario, es una buena práctica usar un bloque `try/finally` con las instancias de `Lock` para garantizar que cualquier bloqueo adquirido se libere correctamente. Esto puede ayudar a prevenir una fuga de recursos en la práctica.

La clase `ReentrantLock` **garantiza** que, una vez que un hilo ha llamado a `lock()` y ha obtenido el bloqueo, todos los demás hilos que llamen a `lock()` esperarán hasta que el primer hilo llame a `unlock()`. Qué hilo obtiene el bloqueo a continuación depende de los parámetros usados para crear el objeto `Lock`.

Si necesitas controlar el orden en que se ejecutan los hilos, **puedes usar** el constructor de `ReentrantLock` que toma un único parámetro booleano de «imparcialidad» (_fairness_). En la práctica, **debes** habilitar la imparcialidad solo cuando el orden sea absolutamente requerido, ya que podría llevar a una desaceleración significativa.

Además de asegurarse siempre de liberar un bloqueo, también **tienes que** asegurarte de que solo liberes un bloqueo que posees. Si intentas liberar un bloqueo que no tienes, recibirás una excepción en tiempo de ejecución:

```Java
var lock = new ReentrantLock();
lock.unlock(); // IllegalMonitorStateException
```

La interfaz `Lock` incluye cuatro métodos que **debes** conocer para el examen, listados en la tabla 13.11.

**TABLA 13.11** Métodos de `Lock`

| **Nombre de método** | **Descripción** |
| --- | --- |
| `void lock()` | Solicita el bloqueo y se bloquea hasta que el bloqueo es adquirido. |
| `void unlock()` | Libera el bloqueo. |
| `boolean tryLock()` | Solicita el bloqueo y regresa inmediatamente. Devuelve un booleano que indica si el bloqueo fue adquirido exitosamente. |
| `boolean tryLock(long timeout, TimeUnit unit)` | Solicita el bloqueo y se bloquea durante el tiempo especificado o hasta que el bloqueo es adquirido. Devuelve un booleano que indica si el bloqueo fue adquirido exitosamente. |

#### Intentando adquirir un bloqueo

Si bien la clase `ReentrantLock` te permite esperar por un bloqueo, hasta ahora sufre del mismo problema que un bloque `synchronized`. Un hilo podría terminar esperando para siempre para obtener un bloqueo. Por suerte, la tabla 13.11 **incluye** dos métodos adicionales que hacen que la interfaz `Lock` sea mucho más segura de usar que un bloque `synchronized`.

Por conveniencia, **usamos** el siguiente método `printHello()` para el código de esta sección:

```Java
static void printHello(Lock myLock) {
   try {
      myLock.lock();
      System.out.println("Hello");
   } finally {
      myLock.unlock();
   } }
```

El método `tryLock()` **intentará** adquirir un bloqueo y **regresará** inmediatamente con un resultado booleano que indica si el bloqueo fue obtenido. A diferencia del método `lock()`, no espera si otro hilo ya posee el bloqueo. Regresa inmediatamente, independientemente de si el bloqueo está disponible.

Lo siguiente **es** una implementación de muestra usando el método `tryLock()`:

```Java
var myLock = new ReentrantLock();
Thread.ofPlatform().start(() -> printHello(myLock));
if (myLock.tryLock()) {
   try {
      System.out.println("Lock obtained, entering protected code");
   } finally {
      myLock.unlock();
   }
} else {
   System.out.println("Unable to acquire lock, doing something else");
}
```

Cuando ejecutes este código, **podría producir** ya sea el mensaje del `if` o el del `else`, dependiendo del orden de ejecución. Sin embargo, siempre imprimirá `Hello`, ya que la llamada a `lock()` en `printHello()` esperará indefinidamente a que el bloqueo se vuelva disponible. Un ejercicio divertido **es insertar** algunos retardos `Thread.sleep()` en este fragmento para animar a que se muestre un mensaje particular.

Al igual que `lock()`, el método `tryLock()` **debería usarse** con un bloque `try/finally`. Por suerte, solo **es necesario** liberar el bloqueo si fue adquirido exitosamente. Por esta razón, es común usar la salida de `tryLock()` en una sentencia `if`, de modo que `unlock()` solo se llame cuando el bloqueo es obtenido.

> **Nota:** **Es imperativo** que tu programa siempre compruebe el valor de retorno del método `tryLock()`. Le indica a tu programa si es seguro proceder con la operación y si el bloqueo necesita liberarse más adelante.

La interfaz `Lock` **incluye** una versión sobrecargada de `tryLock(long, TimeUnit)` que actúa como un híbrido de `lock()` y `tryLock()`. Como los otros dos métodos, si un bloqueo está disponible, regresará inmediatamente con él. Sin embargo, si un bloqueo no está disponible, esperará hasta el límite de tiempo especificado por el bloqueo.

#### Adquiriendo el mismo bloqueo dos veces

La clase `ReentrantLock` **mantiene** un contador del número de veces que un bloqueo ha sido concedido exitosamente a un hilo. Para liberar el bloqueo y que otros hilos lo usen, `unlock()` debe llamarse el mismo número de veces que el bloqueo fue concedido. El siguiente fragmento de código contiene un error. ¿Puedes detectarlo?

```Java
var myLock = new ReentrantLock();
if (myLock.tryLock()) {
   try {
      myLock.lock();
      System.out.println("Lock obtained, entering protected code");
   } finally {
      myLock.unlock();
   } 
}
```

El hilo obtiene el bloqueo dos veces pero lo libera solo una. **Puedes verificar** esto generando un nuevo hilo después de que este código se ejecute e intente obtener un bloqueo. Lo siguiente imprime `false`:

```Java
Thread.ofPlatform().start(() ->
System.out.print(myLock.tryLock()));
```

¡**Es crítico** que liberes un bloqueo el mismo número de veces que fue adquirido! Para las llamadas con `tryLock()`, solo **tienes que** llamar a `unlock()` si el método devolvió `true`.

#### Repaso del framework de bloqueos

Para repasar, la clase `ReentrantLock` **admite** las mismas características que un bloque `synchronized`, mientras agrega varias mejoras:

- Capacidad de solicitar un bloqueo sin bloquearse.
- Capacidad de solicitar un bloqueo bloqueándose durante una cantidad de tiempo especificada.
- Un bloqueo **se puede** crear con una propiedad de imparcialidad, en la que el bloqueo se concede a los hilos en el orden en que fue solicitado.

> **Introducción a `ReentrantReadWriteLock`**
>
> Cuando **se trabaja** con datos compartidos, leer datos es con mucha más frecuencia que escribir datos. Por ejemplo, un solo operador del zoológico podría estar actualizando el menú del almuerzo que miles de visitantes leen desde su teléfono.
>
> Por esta razón, `ReentrantReadWriteLock` es una clase realmente útil. Incluye bloqueos separados para leer y escribir datos. En tiempo de ejecución, como máximo un hilo puede poseer el bloqueo de escritura a la vez, pero muchos hilos pueden poseer el bloqueo de lectura. Tener bloqueos separados puede ayudarte a maximizar el acceso concurrente.
>
> ```Java
> var lock = new ReentrantReadWriteLock();
> lock.writeLock().lock();
> lock.readLock().lock();
> System.out.println(lock.isWriteLocked());              // true
> System.out.println(lock.getReadLockCount());           // 1
>
> lock.writeLock().unlock();
> System.out.println(lock.isWriteLocked());              // false
> System.out.println(lock.getReadLockCount());           // 1
>
> lock.readLock().unlock();
> System.out.println(lock.getReadLockCount());           // 0
> ```
>
> En el examen, es probable que veas su uso dentro de una sola clase. **Necesitas saber** que puedes obtener fácilmente un bloqueo de lectura después de adquirir un bloqueo de escritura, porque leer es un subconjunto de escribir. Sin embargo, si intentas obtener el bloqueo de lectura primero, el código se quedará colgado, ya que no **se puede** actualizar a un bloqueo de escritura.
>
> ```Java
> var lock = new ReentrantReadWriteLock();
> lock.readLock().lock();
> lock.writeLock().lock(); // Wait forever
> ```

### Orquestación de tareas con `CyclicBarrier`

**Comenzamos** el tema de la seguridad de hilos discutiendo la protección de variables individuales y luego **pasamos** a bloques de código y bloqueos. **Completamos** nuestra discusión sobre la seguridad de hilos mostrando cómo orquestar tareas complejas con muchos pasos.

Nuestros trabajadores del zoológico están de vuelta, y esta vez están limpiando corrales. **Imagina** un corral de leones que necesita ser vaciado, limpiado y luego rellenado con los leones. Para completar la tarea, hemos asignado a cuatro trabajadores del zoológico. Obviamente, no queremos empezar a limpiar la jaula mientras un león está merodeando dentro, ¡para no terminar perdiendo a un trabajador del zoológico! Además, no queremos dejar que los leones regresen al corral mientras todavía se está limpiando.

**Podríamos dejar** que todo el trabajo lo complete un solo trabajador, pero esto sería lento e ignoraría el hecho de que tenemos tres trabajadores del zoológico listos para ayudar. Una mejor solución sería que los cuatro empleados del zoológico trabajen de forma concurrente, pausando entre el final de un conjunto de tareas y el inicio del siguiente.

Para coordinar estas tareas, **podemos usar** la clase `CyclicBarrier`:

```Java
import java.util.concurrent.*;
public class LionPenManager {
   private void removeLions() { System.out.println("Removing lions");    }
   private void cleanPen()    { System.out.println("Cleaning the pen"); }
   private void addLions()    { System.out.println("Adding lions");      }
   public void performTask() {
      removeLions();
      cleanPen();
      addLions();
   }
   public static void main(String[] args) {
      try (var service = Executors.newFixedThreadPool(4)) {
         var manager = new LionPenManager();
         for (int i = 0; i < 4; i++)
             service.submit(() -> manager.performTask());
      } } }
```

La siguiente **es** una salida de muestra basada en esta implementación:

```
Removing lions
Removing lions
Cleaning the pen
Adding lions
Removing lions
Cleaning the pen
Adding lions
Removing lions
Cleaning the pen
Adding lions
Cleaning the pen
Adding lions
```

Aunque los resultados están ordenados dentro de un solo hilo, la salida es totalmente aleatoria entre los múltiples trabajadores. **Vemos** que algunos leones todavía se están removiendo mientras la jaula se está limpiando, y otros leones se agregan antes de que el proceso de limpieza termine. ¡Esperemos que ninguno de los trabajadores del zoológico sea comido!

**Podemos mejorar** estos resultados usando la clase `CyclicBarrier`. La clase `CyclicBarrier` recibe en sus constructores un valor límite, que indica el número de hilos por los que esperar. A medida que cada hilo termina, llama al método `await()` sobre la barrera cíclica. Una vez que el número especificado de hilos ha llamado cada uno a `await()`, la barrera se libera y todos los hilos pueden continuar.

```Java
import java.util.concurrent.*;
public class LionPenManager {
   private void removeLions() { System.out.println("Removing lions");    }
   private void cleanPen()    { System.out.println("Cleaning the pen"); }
   private void addLions()    { System.out.println("Adding lions");      }
   public void performTask(CyclicBarrier c1, CyclicBarrier c2) {
      try {
         removeLions();
         c1.await();
         cleanPen();
         c2.await();
         addLions();
      } catch (InterruptedException | BrokenBarrierException e) {
         // Gestiona aquí las excepciones controladas
      }
   }
   public static void main(String[] args) {
      try (var service = Executors.newFixedThreadPool(4)) {
         var manager = new LionPenManager();
         var c1 = new CyclicBarrier(4);
         var c2 = new CyclicBarrier(4,
            () -> System.out.println("*** Pen Cleaned!"));
         for (int i = 0; i < 4; i++)
            service.submit(() -> manager.performTask(c1, c2));
      } } }
```

La siguiente **es** una salida de muestra basada en esta implementación revisada de nuestra clase `LionPenManager`:

```
Removing lions
Removing lions
Removing lions
Removing lions
Cleaning the pen
Cleaning the pen
Cleaning the pen
Cleaning the pen
*** Pen Cleaned!
Adding lions
Adding lions
Adding lions
Adding lions
```

Como puedes ver, todos los resultados ahora están organizados. La remoción de los leones ocurre en un paso, al igual que la limpieza del corral y la reinserción de los leones. En este ejemplo, **usamos** dos constructores diferentes para nuestros objetos `CyclicBarrier`, el último de los cuales ejecuta una instancia de `Runnable` al completarse. La clase `CyclicBarrier` nos permite realizar tareas complejas multihilo mientras todos los hilos se detienen y esperan en barreras lógicas. Esta solución **es superior** a una solución de un solo hilo, ya que las tareas individuales, como remover a los leones, pueden completarse en paralelo por los cuatro trabajadores del zoológico.

> **Reutilización de `CyclicBarrier`**
>
> Después de que se alcanza el límite de un `CyclicBarrier` (también conocido como que la barrera fue rota), todos los hilos son liberados y el número de hilos esperando en el `CyclicBarrier` vuelve a cero. En este punto, el `CyclicBarrier` puede usarse de nuevo para un nuevo conjunto de hilos en espera. Por ejemplo, si nuestro límite de `CyclicBarrier` es 5 y tenemos 15 hilos que llaman a `await()`, el `CyclicBarrier` será activado un total de tres veces.
