Una novedad en Java 21 son las colecciones secuenciadas (_sequenced collections_), que incluyen las tres interfaces de la imagen [[Framework de Colecciones de Java.png]]:

- `SequencedCollection`
- `SequencedSet`
- `SequencedMap`

Una colección secuenciada es una colección en la que el orden de encuentro (_encounter order_) está bien definido. Por orden de encuentro se entiende que todos los elementos pueden leerse de forma repetible. Aunque los elementos de la colección pueden estar ordenados (_sorted_), esto no es un requisito.

### Trabajando con SequencedCollection

**Se comenzará** con el ejemplo más sencillo de una colección secuenciada, una con la que **se ha estado** trabajando a lo largo de este libro. Un `ArrayList` es un `SequencedCollection`, ya que su primer y último elemento están bien definidos, al igual que el orden de todos los elementos intermedios.

La Tabla 9.11 incluye varios métodos disponibles en un `SequencedCollection`.

**TABLA 9.11** Métodos de `SequencedCollection`

|**Método**|**Descripción**|
|---|---|
|`addFirst(E e)`|Añade el elemento como el primer elemento en la colección.|
|`addLast(E e)`|Añade el elemento como el último elemento en la colección.|
|`getFirst()`|Recupera el primer elemento de la colección.|
|`getLast()`|Recupera el último elemento de la colección.|
|`removeFirst()`|Elimina el primer elemento de la colección.|
|`removeLast()`|Elimina el último elemento de la colección.|
|`reversed()`|Devuelve una vista en orden inverso de la colección.|

Para `ArrayList`, debería ser bastante obvio cómo se implementan la mayoría de estos métodos. Por ejemplo, para añadir o recuperar el primer elemento de la lista, **se podría** llamar a `add(0, e)` y `get(0)`, respectivamente. El propósito de la interfaz `SequencedCollection` no es necesariamente añadir nueva funcionalidad, sino facilitar el trabajo con tipos relacionados. Por ejemplo, supongamos que **se tiene** el siguiente método que da la bienvenida al próximo visitante del zoológico:

```Java
public void welcomeNext(SequencedCollection<String> visitors) {
    System.out.println("Welcome to the Zoo! " +
        visitors.getFirst());
    visitors.removeFirst();
}
```

Ahora **se pueden** aplicar varias colecciones secuenciadas a este método:

```Java
var visitArrayList = new ArrayList<String>(List.of("Huey", "Dewey", "Louie"));
var visitLinkedList = new LinkedList<String>(List.of("Moe", "Larry", "Shemp"));
var visitTreeSet = new TreeSet<String>(Set.of("Alvin", "Simon", "Theodore"));

welcomeNext(visitArrayList);  // Welcome to the Zoo! Huey
welcomeNext(visitLinkedList); // Welcome to the Zoo! Moe
welcomeNext(visitTreeSet);    // Welcome to the Zoo! Alvin
```

Las colecciones secuenciadas otorgan la capacidad de trabajar con muchos tipos diferentes que tienen un orden de encuentro bien definido. Usando algunos de los otros métodos de la Tabla 9.11, incluso **se pueden** reorganizar los elementos.

```Java
public void moveToEnd(SequencedCollection<String> visitors) {
    visitors.addLast(visitors.removeFirst());
}
```

¿Qué sucede si **se llama** a este nuevo método en otro grupo de colecciones?

```Java
var visitArrayList = new ArrayList<String>(List.of("Bluey", "Bingo", "Socks"));
var visitLinkedList = new LinkedList<String>(List.of("Garfield", "Odie"));
var visitTreeSet = new TreeSet<String>(Set.of("Tom", "Jerry"));

moveToEnd(visitArrayList);
welcomeNext(visitArrayList);   // Welcome to the Zoo! Bingo

moveToEnd(visitLinkedList);
welcomeNext(visitLinkedList);  // Welcome to the Zoo! Odie

moveToEnd(visitTreeSet);       // java.lang.UnsupportedOperationException
welcomeNext(visitTreeSet);
```

Oh oh, ¿por qué no funcionó el último ejemplo? El hecho de que una clase implemente `SequencedCollection` no significa que soporte todos los métodos de la Tabla 9.11. En este ejemplo, la llamada a `addLast()` falla en tiempo de ejecución porque no **se puede** insertar un elemento al final de una estructura ordenada (_sorted_). Hacerlo podría violar el comparador dentro del `TreeSet`.

> Para el examen, no **se necesita** saber qué colecciones soportan qué métodos de la Tabla 9.11, pero sí **se debe** conocer la diferencia entre una colección secuenciada (_sequenced_) y una colección ordenada (_sorted_).

Un `SequencedSet` es un subtipo de `SequencedCollection`; por lo tanto, hereda todos sus métodos. Solo se aplica a clases `SequencedCollection` que también implementan `Set`, como `LinkedHashSet` y `TreeSet`.

### Trabajando con SequencedMap

Como probablemente **se pueda** adivinar, un `SequencedMap` es un `Map` con un orden de encuentro definido. **Se definen** los métodos comunes en la Tabla 9.12.

**TABLA 9.12** Métodos comunes de `SequencedMap`

|**Método**|**Descripción**|
|---|---|
|`firstEntry()`|Recupera el primer par clave/valor en el mapa.|
|`lastEntry()`|Recupera el último par clave/valor en el mapa.|
|`pollFirstEntry()`|Elimina y recupera el primer par clave/valor en el mapa.|
|`pollLastEntry()`|Elimina y recupera el último par clave/valor en el mapa.|
|`putFirst(K k, V v)`|Añade el par clave/valor como el primer elemento en el mapa.|
|`putLast(K k, V v)`|Añade el par clave/valor como el último elemento en el mapa.|
|`reversed()`|Devuelve una vista en orden inverso del mapa.|

**Se definirá** un método para trabajar con `SequencedMap`:

```Java
public void welcomeNext(SequencedMap<String, String> visitors) {
    System.out.println("Welcome to the Zoo! " +
        visitors.pollFirstEntry());
}
```

¿Qué crees que imprime el siguiente fragmento?

```Java
var visitHashMap = new HashMap<String,String>(
    Map.of("1", "Yakko", "2", "Wakko", "3", "Dot"));
welcomeNext(visitHashMap);
```

¡Pregunta trampa! En realidad no compila. Como **se explicó** anteriormente con `HashSet`, un `HashMap` no tiene un orden, por lo que no puede usarse como un `SequencedMap`. ¿Qué hay de este ejemplo?

```Java
var visitTreeMap = new TreeMap<String,String>(
    Map.of("Pink", "Blossom", "Green", "Buttercup", "Blue", "Bubbles"));
welcomeNext(visitTreeMap);
```

Si adivinaste `Welcome to the Zoo! Blue=Bubbles`, entonces estabas prestando atención cuando **se cubrió** `TreeMap`. Un `TreeMap` ordena los elementos por el orden natural de sus claves, no por el orden en que se añadieron al mapa. Dado que `Blue` es la primera clave en el ordenamiento, es el primer par que se imprime.

> La mayoría de las colecciones con las que **se ha trabajado** a lo largo de este proyecto son secuenciadas. Dos excepciones notables son `HashSet` y `HashMap`.
