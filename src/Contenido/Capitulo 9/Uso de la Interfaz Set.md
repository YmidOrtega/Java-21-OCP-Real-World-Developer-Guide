**Se usa** un `Set` cuando no **se desea** permitir entradas duplicadas. Por ejemplo, **se podría** querer mantener un registro de los animales únicos que **se quieren** ver en el zoológico. No **hay preocupación** por el orden en el que **se ven** estos animales, pero no hay tiempo para verlos más de una vez. Simplemente **se quiere** asegurar de ver los que son importantes y eliminarlos del conjunto de animales pendientes por ver una vez que ya se han visitado.

La imagen a continuación muestra cómo **se puede** visualizar un `Set`. Lo principal que todas las implementaciones de `Set` tienen en común es que **no permiten duplicados**. A continuación, **se analizará** cada implementación que **se necesita** conocer para el examen y cómo escribir código usando `Set`.

![[Ejemplo de un Set.png]]

### Comparación de Implementaciones de Set

Revisando la imagen nuevamente, **se deben** conocer tres clases que implementan la interfaz `Set`: `HashSet`, `LinkedHashSet` y `TreeSet`. Un `HashSet` almacena sus elementos en una tabla hash (_hash table_), lo que significa que las claves son un hash y los valores son un `Object`. Esto significa que el `HashSet` utiliza el método `hashCode()` de los objetos para recuperarlos de manera más eficiente. **Se debe recordar** que un `hashCode()` válido no significa que cada objeto obtendrá un valor único, pero el método a menudo **se escribe** para que los valores hash se distribuyan en un rango amplio con el fin de reducir las colisiones.

Un `LinkedHashSet` es básicamente un `HashSet` con un `LinkedList` imaginario que atraviesa sus elementos. Esto permite iterar sobre el conjunto en un orden de encuentro bien definido, que a menudo es el **orden en el que se insertaron** los elementos. Dicho esto, `LinkedHashSet` también incluye métodos para añadir o eliminar elementos del principio o del final del conjunto, permitiendo cambiar el orden según sea necesario.

Finalmente, un `TreeSet` almacena sus elementos en una estructura de árbol ordenada. El principal beneficio es que el conjunto **siempre está ordenado**. La desventaja (_trade-off_) es que añadir o eliminar un elemento podría tomar más tiempo que con un `HashSet`, especialmente a medida que el árbol crece.

La imagen a continuacion muestra cómo **se puede** visualizar el almacenamiento de estas tres clases. En la realidad, `HashSet` es más complicado, pero esta simplificación es suficiente para el propósito del examen.

![[Ejemplos de Sets.png]]

> Para el examen, no **se necesita** saber cómo crear internamente una clase de tabla hash o de árbol (la implementación puede ser compleja). ¡Uf! ¡Solo **se necesita** saber cómo usarlas!

### Trabajo con Métodos de Set

Al igual que con un `List`, **se puede** crear un `Set` inmutable en una sola línea o hacer una copia de uno existente.

```Java
Set<Character> letters = Set.of('c', 'a', 't');
Set<Character> copy = Set.copyOf(letters);
```

¡Esos son los únicos métodos adicionales que **se necesitan** conocer de la interfaz `Set` para el examen! Sí **se debe** saber cómo se comportan los conjuntos con respecto a los métodos tradicionales de `Collection`. También **se deben** conocer las diferencias entre los tipos de conjuntos. **Se comenzará** con `HashSet`.

```Java
3: Set<Integer> set = new HashSet<>();
4: boolean b1 = set.add(66); // true
5: boolean b2 = set.add(10); // true
6: boolean b3 = set.add(66); // false
7: boolean b4 = set.add(8);  // true
8: for (Integer value: set)
9:    System.out.print(value + ","); // 66,8,10,
```

Los métodos `add()` deberían ser fáciles de entender. Devuelven `true` a menos que el `Integer` ya esté en el conjunto. La línea 6 devuelve `false` porque ya **se tiene** un 66 en el conjunto, y un `Set` debe preservar la unicidad. La línea 8 imprime los elementos del conjunto en un **orden arbitrario**. En este caso, resulta no ser el orden natural (numérico) ni el orden en el que **se añadieron** los elementos.

**Se debe recordar** que el método `equals()` **se usa** para determinar la igualdad. El método `hashCode()` **se usa** para saber en qué "cubeta" (_bucket_) buscar, de modo que Java no tenga que revisar todo el conjunto para averiguar si un objeto está allí. En el mejor de los casos es que los códigos hash sean únicos y Java tenga que llamar a `equals()` solo en un objeto. El peor de los casos es que todas las implementaciones devuelvan el mismo `hashCode()` y Java tenga que llamar a `equals()` en cada elemento del conjunto de todos modos.

**Se reemplazará** la línea 3 con un `LinkedHashSet` para ver cómo cambia la salida.

```Java
3: Set<Integer> set = new LinkedHashSet<>();
```

Esta vez, el código imprime los elementos en el orden en que fueron insertados:

`66,10,8,`

Finalmente, **se puede** usar un `TreeSet` en la línea 3.

```Java
3: Set<Integer> set = new TreeSet<>();
```

Los elementos ahora se imprimen en su orden natural (ascendente):

`8,10,66,`

Los tipos de clase envolventes (_wrapper types_) de los números implementan la interfaz `Comparable` en Java, la cual **se utiliza** para el ordenamiento. Más adelante en el capítulo, **se aprenderá** cómo crear objetos `Comparable` propios.
