**Se usa** un `Queue` (cola) cuando los elementos se añaden y eliminan en un orden específico. **Se puede** pensar en un `Queue` como una fila de espera. Por ejemplo, cuando **se quiere** entrar a un estadio y alguien está esperando en la fila, **se hace** fila detrás de esa persona. ¡Y si eres británico, te pones en la _queue_ detrás de esa persona, lo que hace que esto sea muy fácil de recordar! Esta es una cola **FIFO** (primero en entrar, primero en salir; _first-in, first-out_).

Un `Deque` (cola de doble extremo; _double-ended queue_), a menudo pronunciado "deck", extiende `Queue` pero es diferente de una cola regular en que **se pueden** insertar y eliminar elementos tanto desde el frente (_head_ o cabeza) como desde atrás (_tail_ o cola). Piensa en esto: "¡Dr. Woodie Flowers, venga directo al frente! Usted es el único que recibe este trato especial. Todos los demás tendrán que empezar al final de la fila".

**Se puede** visualizar una cola de doble extremo como se muestra en la imagen.

![[Ejemplo de un Deque.png]]

Suponiendo que **se está** usando esto como una cola FIFO. Rover es el primero, lo que significa que fue el primero en llegar. Bella es la última, lo que significa que fue la última en llegar y tiene la espera más larga por delante. Todas las colas tienen requisitos específicos para añadir y eliminar el siguiente elemento. Más allá de eso, cada una ofrece diferente funcionalidad. A continuación **se analizan** las implementaciones que **se necesitan** conocer y los métodos disponibles.

### Comparación de Implementaciones de Deque

Revisando la imagen [[Framework de Colecciones de Java.png]] una vez más (¡ya **se debería** conocer bien a estas alturas!), tanto `LinkedList` como `ArrayDeque` implementan la interfaz `Deque`, la cual hereda de `Queue`. Ya **se vio** `LinkedList` anteriormente en la sección de `List`. El principal beneficio de un `LinkedList` es que implementa tanto la interfaz `List` como `Deque`. La desventaja (_trade-off_) es que no es tan eficiente como una cola "pura". **Se puede** usar la clase `ArrayDeque` si no **se necesitan** los métodos de `List`.

### Trabajo con Métodos de Queue y Deque

La interfaz `Queue` contiene seis métodos, mostrados en la Tabla 9.3. Hay tres funcionalidades, cada una con dos versiones de los métodos: una que lanza una excepción, y otra que usa el tipo de retorno para transmitir la misma información. **Se han resaltado en negrita** aquellos métodos que lanzan una excepción cuando algo sale mal, como intentar leer de un `Queue` vacío.

**TABLA 9.3** Métodos de `Queue`

| **Funcionalidad**                  | **Métodos**                                    |
| ---------------------------------- | ---------------------------------------------- |
| Añadir al final                    | **`boolean add(E e)`**<br>`boolean offer(E e)` |
| Leer desde el frente               | **`E element()`**<br>`E peek()`                |
| Obtener y eliminar desde el frente | **`E remove()`**<br>`E poll()`                 |
A continuación, **se analizará** el siguiente ejemplo sencillo de una cola:

```Java
4: Queue<Integer> queue = new LinkedList<>();
5: queue.add(10);
6: queue.add(4);
7: System.out.println(queue.remove()); // 10
8: System.out.println(queue.peek());   // 4
```

Las líneas 5 y 6 añaden elementos a la cola. La línea 7 le pide al primer elemento que ha estado esperando más tiempo que salga de la cola. La línea 8 comprueba la siguiente entrada en la cola, pero la deja en su lugar.

A continuación, **se pasa** a la interfaz `Deque`. Dado que la interfaz `Deque` soporta colas de doble extremo, hereda todos los métodos de `Queue` y añade más para que quede claro si **se está** trabajando con el frente o el final de la cola. La Tabla 9.4 muestra los métodos cuando **se utiliza** como una cola de doble extremo.

**TABLA 9.4** Métodos de `Deque`

| **Funcionalidad**                  | **Métodos**                                           |
| ---------------------------------- | ----------------------------------------------------- |
| Añadir al frente                   | **`void addFirst(E e)`**<br>`boolean offerFirst(E e)` |
| Añadir al final                    | **`void addLast(E e)`**<br>`boolean offerLast(E e)`   |
| Leer desde el frente               | **`E getFirst()`**<br>`E peekFirst()`                 |
| Leer desde el final                | **`E getLast()`**<br>`E peekLast()`                   |
| Obtener y eliminar desde el frente | **`E removeFirst()`**<br>`E pollFirst()`              |
| Obtener y eliminar desde el final  | **`E removeLast()`**<br>`E pollLast()`                |
**Se probará** un ejemplo que trabaja con ambos extremos de la cola:

```Java
Deque<Integer> deque = new LinkedList<>();
```

Esto es un poco más complicado, así que **se usa** la imagen a continuación para mostrar cómo se ve la cola en cada paso del código.

![[Trabajar con un Deque.png]]

Las líneas 13 y 14 añaden con éxito un elemento al frente y al final de la cola, respectivamente. Algunas colas tienen un tamaño limitado, lo que causaría que el intento de ofrecer (_offering_) un elemento a la cola falle. No **se encontrará** un escenario como ese en el examen. La línea 15 observa el primer elemento de la cola, pero no lo elimina. Las líneas 16 y 17 eliminan los elementos de la cola, uno de cada extremo. Esto da como resultado una cola vacía. Las líneas 18 y 19 intentan observar el primer elemento de la cola, lo que da como resultado `null`.

Además de las colas FIFO, existen las colas **LIFO** (último en entrar, primero en salir; _last-in, first-out_), a las que comúnmente **se les llama pilas** (_stacks_). **Se puede** imaginar una pila de platos. Siempre **se añade** o **se retira** de la parte superior de la pila para evitar un desastre. Afortunadamente, **se pueden** usar las mismas implementaciones de cola de doble extremo. **Se utilizan** diferentes métodos por claridad, como se muestra en la Tabla 9.5.

**TABLA 9.5** Uso de un `Deque` como una pila (_stack_)

| **Funcionalidad**                    | **Métodos**          |
| ------------------------------------ | -------------------- |
| Añadir al frente / parte superior    | **`void push(E e)`** |
| Eliminar del frente / parte superior | **`E pop()`**        |
| Obtener el primer elemento           | `E peek()`           |

**Se probará** otro ejemplo usando el `Deque` como una pila (_stack_):

```Java
Deque<Integer> stack = new ArrayDeque<>();
```

Esta vez, la imagen muestra cómo se ve la pila en cada paso del código.

![[Trabajar con un Srack.png]]

Las líneas 13 y 14 colocan con éxito un elemento en el frente / parte superior de la pila. El código restante también observa el frente.

Al usar un `Deque`, es realmente importante determinar si **se está** usando como una cola FIFO, una pila LIFO o una cola de doble extremo. A modo de repaso:

- Una **cola FIFO** es como una fila de personas. **Se entra** por atrás y **se sale** por delante.
- Una **pila LIFO** es como una pila de platos. **Se pone** el plato encima y **se saca** de encima.
- Una **cola de doble extremo** usa ambos lados.
