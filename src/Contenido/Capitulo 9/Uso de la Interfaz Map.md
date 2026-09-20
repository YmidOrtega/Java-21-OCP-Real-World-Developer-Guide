**Se usa** un `Map` (mapa o diccionario) cuando **se desea** identificar valores mediante una clave. Por ejemplo, cuando usas la lista de contactos en tu teléfono, buscas "George" en lugar de revisar cada número de teléfono uno por uno.

**Se puede** visualizar un `Map` como se muestra en la imagen a continuación. No **se necesita** conocer los nombres de las interfaces específicas que implementan los diferentes mapas, pero sí **se debe** saber que `LinkedHashMap` mantiene el orden de inserción y `TreeMap` mantiene los elementos ordenados por clave.

![[Ejemplo de un Map.png]]

Lo principal que todas las clases `Map` tienen en común es que tienen **claves** (_keys_) y **valores** (_values_). Más allá de eso, cada una ofrece diferente funcionalidad. A continuación, **se analizarán** las implementaciones que **se necesitan** conocer y los métodos disponibles.


> #### Map.of() y Map.copyOf()
>
Al igual que con `List` y `Set`, existe un método de fábrica (_factory method_) para crear un `Map`. **Se pueden** pasar hasta 10 pares de claves y valores.
>
> ```Java
> Map.of("key1", "value1", "key2", "value2");
> ```
>
A diferencia de `List` y `Set`, esto no es lo ideal. Pasar claves y valores es más difícil de leer porque hay que llevar la cuenta de qué parámetro es cuál. Afortunadamente, hay una forma mejor. `Map` también proporciona un método que permite suministrar pares clave/valor.
>
> ```Java
> Map.ofEntries(
>     Map.entry("key1", "value1"),
>     Map.entry("key2", "value2"));
> ```
> Ahora **no se puede** olvidar pasar un valor. Si **se omite** un parámetro, el método `entry()` no compilará. Convenientemente, `Map.copyOf(map)` funciona exactamente igual que los métodos `copyOf()` de las interfaces `List` y `Set`.

### Comparación de Implementaciones de Map

Según la imagen [[Framework de Colecciones de Java.png]], `HashMap`, `LinkedHashMap` y `TreeMap` son las tres clases que implementan la interfaz `Map`.

- Un `HashMap` almacena las claves en una tabla hash. Esto significa que utiliza el método `hashCode()` de las claves para recuperar sus valores de manera más eficiente.
- Al igual que `LinkedHashSet`, el `LinkedHashMap` soporta la iteración sobre los elementos en un orden bien definido. Este es generalmente el orden de inserción, aunque también incluye métodos para añadir o eliminar elementos al principio o al final del mapa.
- Finalmente, un `TreeMap` almacena las claves en una estructura de árbol ordenada. El principal beneficio es que las claves **siempre están ordenadas**. Al igual que con un `TreeSet`, la desventaja es que añadir y comprobar si una clave está presente toma más tiempo a medida que el árbol crece.

### Trabajo con Métodos de Map

Dado que `Map` no extiende de `Collection`, se especifican más métodos en la interfaz `Map`. Como hay tanto claves como valores, **se necesitan** parámetros de tipo genérico para ambos. La clase usa `K` para la clave (_key_) y `V` para el valor (_value_). Los métodos que **se necesitan** conocer para el examen están en la Tabla 9.6. Algunas de las firmas de los métodos están simplificadas para hacerlas más fáciles de entender.

**TABLA 9.6** Métodos de `Map`

| **Método**                                          | **Descripción**                                                                                                                                                             |
| --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `void clear()`                                      | Elimina todas las claves y valores del mapa.                                                                                                                                |
| `boolean containsKey(Object key)`                   | Devuelve si la clave está en el mapa.                                                                                                                                       |
| `boolean containsValue(Object value)`               | Devuelve si el valor está en el mapa.                                                                                                                                       |
| `Set<Map.Entry<K,V>> entrySet()`                    | Devuelve un `Set` de pares clave/valor.                                                                                                                                     |
| `void forEach(BiConsumer<K, V> action)`             | Recorre cada par clave/valor.                                                                                                                                               |
| `V get(Object key)`                                 | Devuelve el valor mapeado por la clave o `null` si ninguno **está mapeado**.                                                                                                |
| `V getOrDefault(Object key, V defaultValue)`        | Devuelve el valor mapeado por la clave o el valor predeterminado si ninguno **está mapeado**.                                                                               |
| `boolean isEmpty()`                                 | Devuelve si el mapa está vacío.                                                                                                                                             |
| `Set<K> keySet()`                                   | Devuelve el conjunto de todas las claves.                                                                                                                                   |
| `V merge(K key, V value, BiFunction<V, V, V> func)` | Establece el valor si la clave no **está establecida**. Ejecuta la función si la clave **está establecida**, para determinar el nuevo valor. Elimina si el valor es `null`. |
| `V put(K key, V value)`                             | Agrega o reemplaza el par clave/valor. Devuelve el valor anterior o `null`.                                                                                                 |
| `V putIfAbsent(K key, V value)`                     | Agrega el valor si la clave no **está presente** y devuelve `null`. De lo contrario, devuelve el valor existente.                                                           |
| `V remove(Object key)`                              | Elimina y devuelve el valor mapeado a la clave. Devuelve `null` si ninguno.                                                                                                 |
| `V replace(K key, V value)`                         | Reemplaza el valor de la clave dada si la clave **está establecida**. Devuelve el valor original o `null` si ninguno.                                                       |
| `void replaceAll(BiFunction<K, V, V> func)`         | Reemplaza cada valor con los resultados de la función.                                                                                                                      |
| `int size()`                                        | Devuelve el número de entradas (pares clave/valor) en el mapa.                                                                                                              |
| `Collection<V> values()`                            | Devuelve una `Collection` de todos los valores.                                                                                                                             |
Aunque la Tabla 9.6 es una lista bastante larga de métodos, ¡no te preocupes!; muchos de los nombres son intuitivos. Además, muchos existen por conveniencia. Por ejemplo, `containsKey()` puede ser reemplazado por una llamada a `get()` que compruebe si el resultado es `null`. Cuál usar depende de ti.
### Llamada a Métodos Básicos

**Se comenzará** comparando el comportamiento de cada una de las clases `Map`. Considera el siguiente método:

```Java
void addElementsAndPrint(Map<String, String> map) {
    map.put("koala", "bamboo");
    map.put("lion", "meat");
    map.put("giraffe", "leaf");
    String food = map.get("koala"); // bamboo
    for (String key: map.keySet())
        System.out.print(key + ",");
}
```

Aquí **se usa** el método `put()` para añadir pares clave/valor al mapa y `get()` para obtener un valor dada una clave. También **se usa** el método `keySet()` para obtener todas las claves. Luego, **se puede** aplicar este método a cada una de nuestras tres clases `Map`.

```Java
addElementsAndPrint(new HashMap<>());        // koala,giraffe,lion,
addElementsAndPrint(new LinkedHashMap<>());  // koala,lion,giraffe,
addElementsAndPrint(new TreeMap<>());        // giraffe,koala,lion,
```

Al igual que **se vio** con las clases `Set`, `HashMap` imprime los elementos en un orden arbitrario usando el `hashCode()` de la clave. `LinkedHashMap` imprime los elementos en el orden en que fueron insertados. Finalmente, `TreeMap` imprime los elementos basándose en el orden de las claves.

Usando nuestra instancia de `HashMap`, **se pueden** probar algunas comprobaciones booleanas.

```Java
System.out.println(map.containsKey("lion"));    // true
System.out.println(map.containsValue("lion"));  // false
System.out.println(map.size());  // 3
map.clear();
System.out.println(map.size());  // 0
System.out.println(map.isEmpty()); // true
```

Las dos primeras líneas muestran que las claves y los valores se comprueban por separado. **Se puede** ver que hay tres pares clave/valor en el mapa. Luego **se borra** el contenido del mapa y **se comprueba** que hay cero elementos y que está vacío.

¿Ves por qué esto no compila?

```Java
System.out.println(map.contains("lion")); // NO COMPILA
```

No compila porque el método `contains()` pertenece a la interfaz `Collection`, pero no a la interfaz `Map`.

En las siguientes secciones, **se muestran** métodos de `Map` con los que podrías no estar tan familiarizado.

### Iteración a través de un Map

Ya **se vio** el método `forEach()` anteriormente en el capítulo. Note que funciona de manera un poco diferente en un `Map`. Esta vez, la expresión lambda usada por el método `forEach()` tiene dos parámetros: la clave y el valor. A continuación **se muestra** un ejemplo:

```Java
Map<Integer, Character> map = new HashMap<>();
map.put(1, 'a');
map.put(2, 'b');
map.put(3, 'c');
map.forEach((k, v) -> System.out.println(v));
```

La lambda tiene tanto la clave como el valor como parámetros. En este caso imprime el valor, pero podría hacer cualquier cosa con la clave y/o el valor. Curiosamente, dado que no nos importa la clave, este código en particular podría haberse escrito con el método `values()` y una referencia a método en su lugar.

```Java
map.values().forEach(System.out::println);
```

Otra forma de iterar sobre los datos en un mapa es usando `entrySet()`, que devuelve un conjunto (`Set`) de objetos `Map.Entry<K, V>`. Si este tipo te parece un poco extraño, ¡no te preocupes! Es solo una forma elegante de almacenar el par clave/valor en un objeto. Proporciona métodos para recuperar la clave y el valor de cada par.

```Java
map.entrySet().forEach(e ->
    System.out.println(e.getKey() + " " + e.getValue()));
```

En este caso, cada elemento `e` es de tipo `Map.Entry<Integer, Character>`.

### Obtención Segura de Valores

El método `get()` devuelve `null` si la clave solicitada no está en el mapa. A veces prefieres que se devuelva un valor diferente. Afortunadamente, el método `getOrDefault()` facilita esto. **Se compararán** los dos métodos.

```Java
3: Map<Character, String> map = new HashMap<>();
4: map.put('x', "spot");
5: System.out.println("X marks the " + map.get('x'));
6: System.out.println("X marks the " + map.getOrDefault('x', ""));
7: System.out.println("Y marks the " + map.get('y'));
8: System.out.println("Y marks the " + map.getOrDefault('y', ""));
```

Este código imprime lo siguiente:

```
X marks the spot
X marks the spot
Y marks the null
Y marks the
```

Como **se puede** ver, las líneas 5 y 6 tienen la misma salida porque `get()` y `getOrDefault()` se comportan de la misma manera cuando la clave está presente. Devuelven el valor mapeado por esa clave. Las líneas 7 y 8 dan una salida diferente, mostrando que `get()` devuelve `null` cuando la clave no está presente. Por el contrario, `getOrDefault()` devuelve la cadena vacía que **se pasó** como parámetro.

### Reemplazo de Valores

Estos métodos son similares a la versión de `List`, excepto que hay una clave involucrada:

```Java
21: Map<Integer, Integer> map = new HashMap<>();
22: map.put(1, 2);
23: map.put(2, 4);
24: Integer original = map.replace(2, 10); // 4
25: System.out.println(map);    // {1=2, 2=10}
26: map.replaceAll((k, v) -> k + v);
27: System.out.println(map);    // {1=3, 2=12}
```

La línea 24 reemplaza el valor para la clave 2 y devuelve el valor original. La línea 26 llama a una función y establece el valor de cada elemento del mapa como el resultado de esa función. En este caso, **se sumaron** la clave y el valor.

Nota que `replace()` y `replaceAll()` **no modifican** el `Map` si este no contiene la clave. Contrasta esto con `put()`, que siempre intentará establecer un valor.

### Colocación si está Ausente (`putIfAbsent`)

El método `putIfAbsent()` establece un valor en el mapa, pero lo omite si el valor ya está establecido con un valor que no sea nulo.

```Java
Map<String, String> favorites = new HashMap<>();
favorites.put("Jenny", "Bus Tour");
favorites.put("Tom", null);
favorites.putIfAbsent("Jenny", "Tram");
favorites.putIfAbsent("Sam", "Tram");
favorites.putIfAbsent("Tom", "Tram");
System.out.println(favorites); // {Tom=Tram, Jenny=Bus Tour, Sam=Tram}
```

Como **se puede** ver, el valor de Jenny no se actualizó porque ya había uno presente. Sam no estaba en absoluto, así que fue añadido. Tom estaba presente como clave pero tenía un valor `null`. Por lo tanto, también fue actualizado.

### Fusión de Datos (`merge`)

El método `merge()` añade la lógica de qué elegir. Supongamos que **se desea** elegir la atracción (_ride_) con el nombre más largo. **Se puede** escribir código para expresar esto pasando una función de mapeo al método `merge()`.

```Java
11: BiFunction<String, String, String> mapper = (v1, v2)
12:     -> v1.length()> v2.length() ? v1: v2;
13:
14: Map<String, String> favorites = new HashMap<>();
15: favorites.put("Jenny", "Bus Tour");
16: favorites.put("Tom", "Tram");
17:
18: String jenny = favorites.merge("Jenny", "Skyride", mapper);
19: String tom = favorites.merge("Tom", "Skyride", mapper);
20:
21: System.out.println(favorites); // {Tom=Skyride, Jenny=Bus Tour}
22: System.out.println(jenny);     // Bus Tour
23: System.out.println(tom);       // Skyride
```

El código en las líneas 11 y 12 toma dos parámetros y devuelve un valor. Nuestra implementación devuelve el que tiene el nombre más largo. La línea 18 llama a esta función de mapeo, y ve que "Bus Tour" es más largo que "Skyride", así que deja el valor como "Bus Tour". La línea 19 llama a esta función de mapeo nuevamente. Esta vez, "Tram" es más corto que "Skyride", así que el mapa se actualiza. La línea 21 imprime el nuevo contenido del mapa. Las líneas 22 y 23 muestran que el resultado es devuelto desde `merge()`.

El método `merge()` también tiene lógica para lo que sucede si hay valores `null` o claves faltantes involucradas. En este caso, **no llama a la `BiFunction` en absoluto**, y simplemente usa el nuevo valor.

```Java
BiFunction<String, String, String> mapper =
    (v1, v2) -> v1.length()> v2.length() ? v1 : v2;
Map<String, String> favorites = new HashMap<>();
favorites.put("Sam", null);
favorites.merge("Tom", "Skyride", mapper);
favorites.merge("Sam", "Skyride", mapper);
System.out.println(favorites);   // {Tom=Skyride, Sam=Skyride}
```

Note que la función de mapeo no es llamada. Si lo fuera, **se tendría** una `NullPointerException`. La función de mapeo se usa **solo** cuando hay dos valores reales entre los cuales decidir.

Lo último que **se debe** saber sobre `merge()` es qué sucede cuando la función de mapeo es llamada y devuelve `null`. **La clave es eliminada del mapa** cuando esto sucede.

```Java
BiFunction<String, String, String> mapper = (v1, v2) -> null;
Map<String, String> favorites = new HashMap<>();
favorites.put("Jenny", "Bus Tour");
favorites.put("Tom", "Bus Tour");

favorites.merge("Jenny", "Skyride", mapper);
favorites.merge("Sam", "Skyride", mapper);
System.out.println(favorites);   // {Tom=Bus Tour, Sam=Skyride}
```

A Tom no se le aplicó ningún cambio ya que no hubo una llamada a `merge()` para esa clave. Sam fue añadido ya que esa clave no estaba en la lista original. Jenny fue eliminada porque la función de mapeo devolvió `null`.

La Tabla 9.7 muestra todos estos escenarios como referencia.

**TABLA 9.7** Comportamiento del método `merge()`

| **Si la clave solicitada ________**   | **Y la función de mapeo devuelve ________** | **Entonces:**                                                                                  |
| ------------------------------------- | ------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **Tiene un valor `null` en el mapa**  | N/A (la función de mapeo no es llamada)     | Actualiza el valor de la clave en el mapa con el parámetro `value`.                            |
| **Tiene un valor no nulo en el mapa** | `null`                                      | Elimina la clave del mapa.                                                                     |
| **Tiene un valor no nulo en el mapa** | Un valor no nulo                            | Establece el valor con el resultado de la función de mapeo.                                    |
| **No está en el mapa**                | N/A (la función de mapeo no es llamada)     | Añade la clave con el parámetro `value` al mapa directamente sin llamar a la función de mapeo. |
