Además de gestionar hilos, la API de Concurrencia incluye interfaces y clases que ayudan a coordinar el acceso a colecciones compartidas por múltiples tareas. Por colecciones, por supuesto, nos referimos al Framework de Colecciones de Java que se introdujo en el Capítulo 9, "Colecciones y Genéricos". En esta sección, **se muestran** muchas de las clases concurrentes disponibles al usar la API de Concurrencia.

### Comprendiendo los errores de consistencia de memoria

El propósito de las clases de colecciones concurrentes **es resolver** los errores comunes de consistencia de memoria. Un error de consistencia de memoria ocurre cuando dos hilos tienen visiones inconsistentes de los datos que deberían ser los mismos. En términos conceptuales, **se quiere** que las escrituras hechas en un hilo estén disponibles para otro hilo si este accede a la colección concurrente después de que la escritura haya ocurrido.

Cuando dos hilos intentan modificar la misma colección no concurrente, la JVM puede lanzar una `ConcurrentModificationException` en tiempo de ejecución. De hecho, puede ocurrir con un solo hilo. Echa un vistazo al siguiente fragmento de código:

```Java
11: var foodData = new HashMap<String, Integer>();
12: foodData.put("penguin", 1);
13: foodData.put("flamingo", 2);
14: for (String key : foodData.keySet())
15:    foodData.remove(key);
```

Este fragmento lanzará una `ConcurrentModificationException` durante la segunda iteración del bucle, ya que el iterador sobre `keySet()` no **se actualiza** correctamente después de que **se elimina** el primer elemento. Cambiar la primera línea para usar una `ConcurrentHashMap` **evitará** que el código lance una excepción en tiempo de ejecución.

```Java
11: var foodData = new ConcurrentHashMap<String, Integer>();
```

Aunque normalmente no **se modifica** una variable de bucle, este ejemplo resalta el hecho de que la `ConcurrentHashMap` **ordena** el acceso de lectura/escritura de modo que todo el acceso a la clase sea consistente. En este fragmento de código, el iterador creado por `keySet()` **se actualiza** tan pronto como **se elimina** un objeto del `Map`.

Las clases concurrentes **se crearon** para ayudar a evitar problemas comunes en los que múltiples hilos agregan y eliminan objetos de las mismas colecciones. En cualquier instancia dada, todos los hilos deberían tener la misma vista consistente de la estructura de la colección.

### Trabajando con clases concurrentes

**Se recomienda** usar una clase de colección concurrente cada vez que **se tenga** múltiples hilos modificando una colección fuera de un bloque o método `synchronized`, incluso si no **se espera** un problema de concurrencia. Sin las colecciones concurrentes, múltiples hilos accediendo a una colección podrían resultar en una excepción lanzada o, peor aún, ¡datos corruptos!

> **Consejo:** Si la colección es inmutable (y contiene objetos inmutables), las colecciones concurrentes **no son necesarias**. Los objetos inmutables pueden ser accesados por cualquier número de hilos y no requieren sincronización. Por definición, no cambian, así que no hay probabilidad de un error de consistencia de memoria.

La Tabla 13.12 **enumera** las clases concurrentes comunes con las que **se debería** estar familiarizado para el examen.

**TABLA 13.12** Clases de colecciones concurrentes

| **Nombre de la clase** | **Interfaces de la Colección de Java** | **¿Ordenada?** | **¿Bloqueante?** |
|---|---|---|---|
| `ConcurrentHashMap` | `Map`<br>`ConcurrentMap` | No | No |
| `ConcurrentLinkedQueue` | `Queue` | No | No |
| `ConcurrentSkipListMap` | `Map`<br>`SequencedMap`<br>`SortedMap`<br>`NavigableMap`<br>`ConcurrentMap`<br>`ConcurrentNavigableMap` | Sí | No |
| `ConcurrentSkipListSet` | `Set`<br>`SequencedSet`<br>`SortedSet`<br>`NavigableSet` | Sí | No |
| `CopyOnWriteArrayList` | `List`<br>`SequencedCollection` | No | No |
| `CopyOnWriteArraySet` | `Set` | No | No |
| `LinkedBlockingQueue` | `Queue`<br>`BlockingQueue` | No | Sí |

La mayoría de las clases de la Tabla 13.12 son solo versiones concurrentes de sus clases homólogas no concurrentes, como `ConcurrentHashMap` frente a `HashMap`, o `ConcurrentLinkedQueue` frente a `LinkedList`. Para el examen, no **se necesitan** conocer métodos concurrentes específicos de cada clase. Solo **se necesitan** conocer los métodos heredados, como `get()` y `set()` para instancias de `List`.

Las clases Skip pueden sonar extrañas, pero son solo versiones "ordenadas" de las colecciones concurrentes asociadas. Cuando veas una clase con Skip en el nombre, simplemente piensa en colecciones concurrentes "ordenadas", y el resto debería seguir de forma natural.

Las clases CopyOnWrite se comportan un poco diferente a las otras estructuras de datos concurrentes que has visto. Estas clases crean una copia de la colección cada vez que **se agrega**, **se elimina** o **se cambia** una referencia en la colección y luego **actualizan** la referencia original de la colección para que apunte a la copia. Estas clases se usan comúnmente para garantizar que un iterador no vea las modificaciones de la colección.

**Veamos** cómo funciona esto con un ejemplo:

```Java
List<Integer> favNumbers = new CopyOnWriteArrayList<>(List.of(4, 3, 42));
for (var n : favNumbers) {
   System.out.print(n + " "); // 4 3 42
   favNumbers.add(n + 1);
}
System.out.println();
System.out.println("Size: " + favNumbers.size()); // Size: 6
```

A pesar de agregar elementos, el iterador no **se modifica**, y el bucle se ejecuta exactamente tres veces. Alternativamente, si hubiéramos usado un objeto `ArrayList` normal, una `ConcurrentModificationException` se habría lanzado en tiempo de ejecución. Las clases CopyOnWrite pueden usar mucha memoria, ya que **se crea** una nueva estructura de colección cada vez que la colección **se modifica**. Por lo tanto, se usan comúnmente en entornos multihilo donde las lecturas son mucho más comunes que las escrituras.

Finalmente, la Tabla 13.12 incluye `LinkedBlockingQueue`, que implementa la interfaz concurrente `BlockingQueue`. Esta clase es igual que un `Queue` normal, excepto que incluye versiones sobrecargadas de `offer()` y `poll()` que reciben un tiempo de espera. Estos métodos esperan (o se bloquean) hasta una cantidad específica de tiempo para completar una operación.

### Obteniendo colecciones sincronizadas

Además de las clases de colecciones concurrentes que **se han** cubierto, la API de Concurrencia también incluye métodos para obtener versiones sincronizadas de objetos de colección no concurrentes existentes. Estos métodos sincronizados **se definen** en la clase `Collections`. Operan sobre la colección ingresada y devuelven una referencia que es del mismo tipo que la colección subyacente. Estos métodos estáticos **se listan** en la Tabla 13.13.

**TABLA 13.13** Métodos de colecciones sincronizadas

| **Método** |
|---|
| `synchronizedCollection(Collection<T> c)` |
| `synchronizedList(List<T> list)` |
| `synchronizedMap(Map<K,V> m)` |
| `synchronizedNavigableMap(NavigableMap<K,V> m)` |
| `synchronizedNavigableSet(NavigableSet<T> s)` |
| `synchronizedSet(Set<T> s)` |
| `synchronizedSortedMap(SortedMap<K,V> m)` |
| `synchronizedSortedSet(SortedSet<T> s)` |

Si estás escribiendo código para crear una colección y esta requiere sincronización, deberías usar las clases definidas en la Tabla 13.12. Por otro lado, si recibes una colección no concurrente y necesitas sincronización, usa los métodos de la Tabla 13.13.
