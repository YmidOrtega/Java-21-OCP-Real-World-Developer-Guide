**Se concluye** esta parte del capítulo repasando las reglas que se aplican a los distintos tipos de colecciones, así como con un resumen general de todos los tipos cubiertos en este capítulo.

#### Uso de Vistas Envolventes No Modificables

Una vista no modificable (_unmodifiable view_) es un objeto envolvente (_wrapper_) alrededor de una colección que no puede ser modificado a través de la vista misma. Aunque el objeto vista no se puede modificar, los datos subyacentes aún pueden ser modificados.

Hay cuatro métodos con los que **se debe** estar familiarizado para el examen que crean vistas no modificables de una colección:

```Java
Collection<String> coll = Collections.unmodifiableCollection(List.of("brown"));
List<String> list       = Collections.unmodifiableList(List.of("orange"));
Set<String> set         = Collections.unmodifiableSet(Set.of("green"));
Map<String,Integer> map = Collections.unmodifiableMap(Map.of("red", 1));
```

A continuación, **se considera** algo de código que los utiliza:

```Java
10: Map<String, Integer> map = new TreeMap<>();
11: map.put("blue", 41);
12: map.put("red", 90);
13: List<String> list = Arrays.asList("green", "yellow");
14: Set<String> set = new HashSet<>(list);
15:
16: Map<String, Integer> mapView = Collections.unmodifiableMap(map);
17: 
18: Collection<String> collView = Collections.unmodifiableCollection(list);
19: List<String> listView       = Collections.unmodifiableList(list);
20: Set<String> setView         = Collections.unmodifiableSet(set);
```

Como es de esperar, intentar modificar una vista no modificable lanza una excepción. Cuando se ejecutan de forma independiente, cada una de las siguientes líneas compila, pero lanza una `UnsupportedOperationException` en tiempo de ejecución:

```Java
collView.add("pink");
setView.remove("green");
mapView.put("blue", 42);
```

Sin embargo, dado que es una vista, nada impide que **se cambien** los valores originales. Por ejemplo:

```Java
24: System.out.println(mapView);  // {blue=41, red=90}
25: System.out.println(collView); // [green, yellow]
26: System.out.println(listView); // [green, yellow]
27: System.out.println(setView);  // [green, yellow]
28:
29: map.put("blue", 105);
30: list.set(1, "purple");
31:
32: System.out.println(mapView);  // {blue=105, red=90}
33: System.out.println(collView); // [green, purple]
34: System.out.println(listView); // [green, purple]
35: System.out.println(setView);  // [green, yellow]
```

En la línea 29, note que el valor de `blue` cambia a `105` en el `TreeMap` original y se muestra como cambiado en `mapView` en la línea 32. La variable `list` creada en la línea 13 hace referencia a un arreglo subyacente (_backed array_) de tamaño fijo. Esto significa que tanto `collView` como `listView` representan una vista de una `List` que hace referencia a un arreglo subyacente. Dado que el valor se establece mediante `set()` en la línea 30, la colección mantiene el mismo tamaño y el cambio se muestra correctamente en las vistas.

Sin embargo, `setView` no ha cambiado de valor. El constructor en la línea 14 crea un nuevo conjunto que está desconectado de la estructura de datos original. Esto significa que la línea 30 no tiene ningún efecto sobre `set`.

¿Qué sucede si **se intentan** añadir elementos a estas colecciones?

```Java
36: set.add("orange");
37: System.out.println(setView);  // [green, yellow, orange]
38:
39: list.add("orange");           // UnsupportedOperationException
```

La línea 36 modifica con éxito el `HashSet` subyacente, con los cambios reflejados en la vista en la línea 37. La línea 39 lanza una excepción en tiempo de ejecución. **Se debe recordar** que la lista fue creada con `Arrays.asList()`. Como **se vio** al principio del capítulo, **se pueden** reemplazar elementos en dichos objetos, pero no **se pueden** añadir ni eliminar elementos. Para el examen, **se debe recordar** comprobar el tipo del objeto subyacente para determinar si los elementos se pueden añadir, eliminar o modificar.

#### Comparación de los Tipos de Colecciones

Asegure de poder completar la Tabla 9.13 para comparar los cuatro tipos de colecciones de memoria.

**TABLA 9.13** Tipos del Java Collections Framework

|**Tipo**|**¿Puede contener elementos duplicados?**|**¿Elementos siempre ordenados?**|**¿Tiene claves y valores?**|**¿Debe añadir/eliminar en un orden específico?**|
|---|---|---|---|---|
|`List`|Sí|Sí (por índice)|No|No|
|`Queue`|Sí|Sí (recuperados en un orden definido)|No|Sí|
|`Set`|No|No|No|No|
|`Map`|Sí (para los valores)|No|Sí|No|

Adicionalmente, asegúrate de poder completar la Tabla 9.14 para describir los tipos en el examen.

**TABLA 9.14** Clases de colecciones

|**Tipo**|**Interfaces del Java Collections Framework**|**¿Mantiene el orden (Ordered)?**|**¿Ordenado lógicamente (Sorted)?**|**¿Llama a hashCode?**|**¿Llama a compareTo?**|
|---|---|---|---|---|---|
|`ArrayDeque`|`Deque`, `SequencedCollection`|Sí|No|No|No|
|`ArrayList`|`List`, `SequencedCollection`|Sí|No|No|No|
|`HashMap`|`Map`|No|No|Sí|No|
|`HashSet`|`Set`|No|No|Sí|No|
|`LinkedList`|`Deque`, `List`, `SequencedCollection`|Sí|No|No|No|
|`LinkedHashSet`|`Set`, `SequencedSet`|Sí|No|No|No|
|`LinkedHashMap`|`Map`, `SequencedMap`|Sí|No|No|No|
|`TreeMap`|`Map`, `SequencedMap`|Sí|Sí|No|Sí|
|`TreeSet`|`Set`, `SequencedCollection`, `SequencedSet`|Sí|Sí|No|Sí|

El examen espera que **se sepa** que las estructuras de datos que implican ordenamiento lógico (_sorting_) requieren proceder con cuidado al usar `null`. Para los conjuntos ordenados (_sorted sets_), esto significa que no se permite `null`; y para los mapas ordenados (_sorted maps_), esto significa que no se permiten claves con valor `null`.

Finalmente, el examen asume que **se será** capaz de elegir el tipo de colección correcto dada la descripción de un problema. **Se recomienda** identificar primero sobre qué tipo de colección está preguntando la pregunta. **Se debe** averiguar si **se está** buscando una lista, un mapa, una cola o un conjunto. Esto permite eliminar un buen número de respuestas. Luego, **se puede** determinar cuál de las opciones restantes es la mejor respuesta.

> ### Escenario del Mundo Real: Colecciones Más Antiguas
> 
> Hay algunas colecciones que ya no están en el examen, pero que **se podrían** encontrar al trabajar con código antiguo. Las tres fueron estructuras de datos tempranas de Java que **se podían** usar de forma segura con hilos (_threads_).
> 
> - `Vector`: Implementa `List`
>     
> - `Hashtable`: Implementa `Map`
>     
> - `Stack`: Implementa `List`
>     
> 
> Estas clases rara vez se usan hoy en día, ya que existen alternativas concurrentes mucho mejores que **se cubrirán** en el Capítulo 13.
