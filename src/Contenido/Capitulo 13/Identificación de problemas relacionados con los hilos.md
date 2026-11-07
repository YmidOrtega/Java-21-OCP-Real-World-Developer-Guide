Ahora que ya sabes cómo escribir código seguro para hilos (_thread-safe_), hablemos de qué se considera un problema relacionado con los hilos. Un problema relacionado con los hilos puede ocurrir en aplicaciones multihilo cuando dos o más hilos interactúan de una manera inesperada e indeseable. Por ejemplo, dos hilos **pueden** bloquearse mutuamente el acceso a un segmento particular de código.

La API de Concurrencia **se creó** para ayudar a eliminar los problemas potenciales relacionados con los hilos comunes a todos los desarrolladores. Como has visto, la API de Concurrencia **crea** hilos y **gestiona** por ti interacciones complejas de hilos, a menudo en solo unas pocas líneas de código.

Si bien la API de Concurrencia reduce el potencial de problemas relacionados con los hilos, no los elimina. En la práctica, **encontrar e identificar** problemas relacionados con los hilos dentro de una aplicación **es** a menudo una de las tareas más difíciles que un desarrollador puede emprender.

### Comprendiendo la vivacidad

Como has visto en este capítulo, muchas operaciones de hilos **se pueden** realizar de forma independiente, pero algunas **requieren** coordinación. Por ejemplo, **sincronizar** en un método **requiere** que todos los hilos que llaman al método esperen a que otros hilos terminen antes de continuar. También **viste antes** en el capítulo que los hilos de un `CyclicBarrier` esperarán cada uno a que se alcance el límite de la barrera antes de continuar.

¿Qué le sucede a la aplicación mientras todos estos hilos están esperando? En muchos casos, la espera es efímera, y el usuario apenas tiene idea de que haya ocurrido algún retraso. En otros casos, sin embargo, la espera **puede** ser extremadamente larga, quizá infinita.

La **vivacidad** (_liveness_) **es** la capacidad de una aplicación para poder ejecutarse de manera oportuna. Los problemas de vivacidad **son** aquellos en los que la aplicación se vuelve irresponsiva o se encuentra en algún tipo de estado "atorado". Más precisamente, los problemas de vivacidad **son** a menudo el resultado de un hilo que entra para siempre en un estado `BLOCKED` o `WAITING`, o que entra y sale repetidamente de estos estados. Para el examen, hay tres tipos de problemas de vivacidad con los que **se debería** estar familiarizado: interbloqueo (_deadlock_), inanición (_starvation_) y bloqueo vivo (_livelock_).

#### Interbloqueo (_deadlock_)

El **interbloqueo** (_deadlock_) **ocurre** cuando dos o más hilos quedan bloqueados para siempre, cada uno esperando al otro. **Podemos** ilustrar este principio con el siguiente ejemplo. Imagina que nuestro zoológico tiene dos zorros: Foxy y Tails. A Foxy le gusta comer primero y luego beber agua, mientras que a Tails le gusta beber agua primero y luego comer. Además, a ninguno de los dos animales le gusta compartir, y **terminarán** su comida solo si **tienen** acceso exclusivo tanto a la comida como al agua.

El cuidador del zoológico coloca la comida de un lado del recinto y el agua del otro. Aunque nuestros zorros son rápidos, todavía les toma 100 milisegundos correr de un lado del recinto al otro. ¿Qué sucede si Foxy obtiene la comida primero y Tails obtiene el agua primero? La siguiente aplicación **modela** este comportamiento:

```Java
class Food {}
class Water {}
public record Fox(String name) {
	public void eatAndDrink(Food food, Water water) {
		synchronized(food) {
			System.out.println(name() + " Got Food!");
	        move();
		    synchronized(water) {
			    System.out.println(name() + " Got Water!");
			} 
		} 
	}
	public void drinkAndEat(Food food, Water water) {
		synchronized(water) {
			System.out.println(name() + " Got Water!");
	        move();
            synchronized(food) {
               System.out.println(name() + " Got Food!");
            }
        } 
    }
    public void move() {
	    try { Thread.sleep(100); } catch (InterruptedException e) {}
	}
	public static void main(String[] args) {
		// Crear participantes y recursos
        var foxy = new Fox("Foxy");
        var tails = new Fox("Tails");
        var food = new Food();
        var water = new Water();
        // Datos de proceso
        try (var service = Executors.newScheduledThreadPool(10)) {
	        service.submit(() -> foxy.eatAndDrink(food,water));
            service.submit(() -> tails.drinkAndEat(food,water));
        }
    }
}
```

En este ejemplo, Foxy obtiene la comida y luego se mueve al otro lado del recinto para obtener el agua. Desafortunadamente, Tails ya bebió el agua y está esperando a que la comida esté disponible. El resultado es que nuestro programa **imprime** lo siguiente, y **se cuelga** indefinidamente:

```
Foxy Got Food!
Tails Got Water!
```

Este ejemplo **se considera** un interbloqueo porque ambos participantes **quedan** bloqueados permanentemente, esperando recursos que nunca estarán disponibles.

#### Inanición (_starvation_)

La **inanición** (_starvation_) **ocurre** cuando a un solo hilo **se le deniega** perpetuamente el acceso a un recurso compartido o a un bloqueo (_lock_). El hilo sigue activo, pero **no puede completar** su trabajo como resultado de que otros hilos toman constantemente el recurso al que él está intentando acceder.

En nuestro ejemplo de los zorros, imagina que tenemos una manada de zorros muy hambrientos y muy competitivos en nuestro recinto. Cada vez que Foxy se levanta para ir a buscar comida, uno de los otros zorros la ve y se apresura a comer antes que ella. Foxy es libre de deambular por el recinto, echarse una siesta y aullar por un cuidador del zoológico, pero nunca puede obtener acceso a la comida. En este ejemplo, Foxy experimenta inanición en sentido literal y figurado. ¡Qué bueno que esto es solo un ejemplo teórico!

#### Bloqueo vivo (_livelock_)

El **bloqueo vivo** (_livelock_) **ocurre** cuando dos o más hilos están conceptualmente bloqueados para siempre, aunque cada uno sigue activo e intenta completar su tarea. El bloqueo vivo **es** un caso especial de inanición de recursos en el que dos o más hilos intentan activamente adquirir un conjunto de bloqueos (_locks_), no pueden hacerlo y reinician parte del proceso.

El bloqueo vivo **es** a menudo el resultado de dos hilos que intentan resolver un interbloqueo. **Regresemos** a nuestro ejemplo de los zorros: imagina que Foxy y Tails tienen sus recursos de comida y agua, respectivamente. Cada uno **se da cuenta** de que no puede terminar su comida en este estado, así que ambos **sueltan** su comida y su agua, **corren** al lado opuesto del recinto y **toman** el otro recurso. ¡Ahora Foxy tiene el agua, Tails tiene la comida, y ninguno de los dos puede terminar su comida!

Si Foxy y Tails continúan este proceso para siempre, **se le** llama bloqueo vivo. Tanto Foxy como Tails están activos, corriendo de un lado a otro de su área, pero ninguno puede terminar su comida. Foxy y Tails están ejecutando una forma fallida de recuperación de interbloqueo. Cada zorro **nota** que están potencialmente entrando en un estado de interbloqueo y **responde** liberando todos sus recursos bloqueados. Desafortunadamente, el proceso de bloquear y desbloquear es cíclico, y los dos zorros están conceptualmente interbloqueados.

En la práctica, el bloqueo vivo **es** a menudo un problema difícil de detectar. Los hilos en estado de bloqueo vivo **parecen** activos y capaces de responder a peticiones, incluso cuando están atascados en un ciclo interminable.

### Gestionando las condiciones de carrera

Una **condición de carrera** (_race condition_) **es** un resultado indeseable que ocurre cuando dos tareas que deberían completarse secuencialmente **se completan** al mismo tiempo. **Nos encontramos** con ejemplos de condiciones de carrera antes en el capítulo cuando **presentamos** la sincronización.

Si bien la imagen muestra un ejemplo clásico basado en hilos de una condición de carrera, ahora **proporcionamos** un ejemplo más ilustrativo. Imagina que dos visitantes del zoológico, Olivia y Sophia, se están registrando para obtener una cuenta en el nuevo sitio web para visitantes del zoológico. Ambas quieren usar el mismo nombre de usuario, ZooFan, y cada una envía una petición para crear la cuenta al mismo tiempo, como se muestra en la imagen.

¿Qué resultado **devuelve** el servidor web cuando ambos usuarios intentan crear una cuenta con el mismo nombre de usuario en la imagen?

![[Condición de carrera en la creación de usuarios.jpg]]

### Posibles resultados de esta condición de carrera

- Ambos usuarios pueden crear cuentas con el nombre de usuario ZooFan.
- Ningún usuario puede crear una cuenta con el nombre de usuario ZooFan, y se devuelve un mensaje de error a ambos usuarios.
- Un usuario puede crear una cuenta con el nombre de usuario ZooFan, mientras que el otro usuario recibe un mensaje de error.

El primer resultado **es** realmente malo, ya que **lleva** a los usuarios a intentar iniciar sesión con el mismo nombre de usuario. ¿De quién **ven** los datos cuando inician sesión? El segundo resultado **obliga** a ambos usuarios a intentarlo de nuevo, lo cual es frustrante pero al menos no **lleva** a datos corruptos o incorrectos.

El tercer resultado **se considera** a menudo la mejor solución. Al igual que en la segunda situación, **se preserva** la integridad de los datos; pero a diferencia de la segunda situación, al menos un usuario **puede avanzar** con la primera petición, evitando escenarios adicionales de condición de carrera.

Para el examen, **hay que** entender que las condiciones de carrera **llevan** a datos inválidos si no **se manejan** correctamente. Incluso la solución en la que ambos participantes no pueden continuar **es preferible** a una en la que se permiten datos inválidos entrar al sistema.
