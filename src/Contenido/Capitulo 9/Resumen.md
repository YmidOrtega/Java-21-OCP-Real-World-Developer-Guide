El Java Collections Framework incluye cuatro tipos principales de estructuras de datos: listas, conjuntos, colas y mapas. La interfaz `Collection` **es** la interfaz padre de `List`, `Set` y `Queue`. Adicionalmente, `Deque` extiende `Queue`. La interfaz `Map` no extiende `Collection`. **Se necesita** reconocer lo siguiente:

- **`List`:** Una colección ordenada de elementos que permite entradas duplicadas.
  - **`ArrayList`:** Lista estándar redimensionable.
  - **`LinkedList`:** Puede agregar/eliminar fácilmente desde el principio o el final.
- **`Set`:** No permite duplicados.
  - **`HashSet`:** Usa `hashCode()` para encontrar elementos no ordenados.
  - **`LinkedHashSet`:** Orden de encuentro bien definido.
  - **`TreeSet`:** Ordenado. No permite valores `null`.
- **`Queue`/`Deque`:** Ordena elementos para su procesamiento.
  - **`ArrayDeque`:** Cola de doble extremo.
  - **`LinkedList`:** Cola de doble extremo y lista.
- **`Map`:** Mapea claves únicas a valores.
  - **`HashMap`:** Usa `hashCode()` para encontrar claves.
  - **`LinkedHashMap`:** Orden de encuentro bien definido.
  - **`TreeMap`:** Mapa ordenado. No permite claves `null`.

Java 21 ahora incluye colecciones secuenciadas, para tipos con un orden de encuentro definido.

- **`SequencedCollection`:** `ArrayDeque`, `ArrayList`, `LinkedList`, `LinkedHashSet` y `TreeSet`.
- **`SequencedSet`:** `LinkedHashSet` y `TreeSet`.
- **`SequencedMap`:** `LinkedHashMap` y `TreeMap`.

La interfaz `Comparable` declara el método `compareTo()`. Este método devuelve un número negativo si el objeto **es** más pequeño que su argumento, `0` si los dos objetos **son** iguales, y un número positivo en caso contrario. El método `compareTo()` **se declara** en el objeto que **está siendo** comparado y toma un parámetro. La interfaz `Comparator` define el método `compare()`. **Se devuelve** un número negativo si el primer argumento **es** más pequeño, cero si **son** iguales, y un número positivo en caso contrario. El método `compare()` **puede ser** declarado en cualquier código, y toma dos parámetros. Un `Comparator` a menudo **se implementa** usando una lambda.

Los genéricos **son** parámetros de tipo para el código. Para crear una clase con un parámetro genérico, **se agrega** `<T>` después del nombre de la clase. **Se puede** usar cualquier nombre que **se quiera** para el parámetro de tipo. Las letras mayúsculas simples **son** opciones comunes. Los genéricos permiten especificar comodines. `<?>` **es** un comodín no acotado que significa cualquier tipo. `<? extends Object>` **es** un límite superior que significa cualquier tipo que **es** `Object` o lo extiende. `<? extends MyInterface>` significa cualquier tipo que implementa `MyInterface`. `<? super Number>` **es** un límite inferior que significa cualquier tipo que **es** `Number` o una superclase. Un error del compilador **resulta** del código que intenta agregar un elemento en una lista con un comodín no acotado o con límite superior.
