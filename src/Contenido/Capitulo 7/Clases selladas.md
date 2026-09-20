Un `enum` con muchos constructores, campos y métodos puede empezar a parecerse a una clase completa. ¿Qué pasaría si **se pudiera crear** una clase pero limitar las subclases directas a un conjunto fijo de clases? ¡Entran las clases selladas! Una **clase sellada** (_sealed class_) es una clase que restringe qué otras clases pueden extenderla.

### Declarando una Clase Sellada

**Se comenzará** con un ejemplo sencillo. Una clase sellada declara una lista de clases que pueden extenderla, mientras que las subclases declaran que extienden a la clase sellada. La imagen a continuación declara una clase sellada con dos subclases.

![[Definición de una clase sellada.png]]

La imagen incluye tres palabras clave con las que **se debería estar** familiarizado para el examen.

**Palabras Clave de Clases Selladas**

- `sealed`: Indica que una clase o interfaz solo puede ser extendida/implementada por clases o interfaces nombradas.
- `permits`: Utilizada con la palabra clave `sealed` para listar las clases e interfaces permitidas.
- `non-sealed`: Aplicada a una clase o interfaz que extiende una clase sellada, indicando que puede ser extendida por clases no especificadas.

Bastante fácil hasta ahora, ¿verdad? Es igual de probable que el examen **evalúe** para qué **no se pueden usar** las clases selladas. Por ejemplo, ¿**se puede identificar** por qué cada uno de estos dos conjuntos de declaraciones no compila?

```Java
public class sealed Frog permits GlassFrog {} // NO COMPILA
public final class GlassFrog extends Frog {}

public abstract sealed class Mammal permits Wolf {}
public final class Wolf extends Mammal {}
public final class Tiger extends Mammal {} // NO COMPILA
```

El primer ejemplo no compila porque los modificadores `class` y `sealed` están en el orden equivocado. El modificador debe ir antes del tipo de clase. El segundo ejemplo no compila porque `Tiger` no está listada en la declaración de `Mammal`.

> Las clases selladas se declaran comúnmente con el modificador `abstract`, aunque esto ciertamente no es obligatorio.

Declarar una clase sellada con el modificador `sealed` es la parte fácil. La mayoría de las veces, si **se ve** una pregunta en el examen sobre clases selladas, estarán evaluando el conocimiento de si la subclase extiende la clase sellada correctamente. Hay una serie de reglas importantes que **se necesitan conocer** para el examen, así que **se deben leer** las siguientes secciones cuidadosamente.

### Compilando Clases Selladas

Suponga que **se crea** una clase `Penguin` y se compila en un nuevo paquete sin ningún otro código fuente. Teniendo eso en cuenta, ¿compila lo siguiente?

```Java
// Penguin.java
package zoo;
public sealed class Penguin permits Emperor {}
```

¡No, no lo hace! ¿Por qué? La respuesta es que una clase sellada necesita ser declarada (y compilada) en el mismo paquete que sus subclases directas. Pero, ¿qué pasa con las subclases en sí? Cada una de ellas debe extender la clase sellada. Por ejemplo, las siguientes dos declaraciones no compilan:

```Java
// Penguin.java
package zoo;
public sealed class Penguin permits Emperor {} // NO COMPILA

// Emperor.java
package zoo;
public final class Emperor {}
```

A pesar de que la clase `Emperor` está declarada, no extiende a la clase `Penguin`.

¡Pero hay más! En el Capítulo 12, "Módulos", **se aprenderá** sobre los módulos nombrados (_named modules_), los cuales permiten clases selladas y sus subclases directas en diferentes paquetes, siempre que estén en el mismo módulo nombrado.

### Especificando el Modificador de la Subclase

Mientras que algunos tipos, como las interfaces, tienen un cierto número de modificadores implícitos, las clases selladas no. Toda clase que extienda directamente una clase sellada debe especificar exactamente uno de los siguientes tres modificadores: `final`, `sealed` o `non-sealed`. ¡**Se debe recordar** esta regla para el examen!

#### Creando Subclases `final`

El primer modificador que **se va a analizar** y que se puede aplicar a una subclase directa de una clase sellada es el modificador `final`. Una clase sellada con solo subclases `final` tiene un conjunto fijo de tipos, lo cual es similar a un `enum` con un conjunto fijo de valores.

```Java
public sealed class Antelope permits Gazelle {}
public final class Gazelle extends Antelope {}
public class DamaGazelle extends Gazelle {} // NO COMPILA
```

Al igual que con una clase regular, el modificador `final` evita que la subclase `Gazelle` sea extendida aún más.

#### Creando Subclases `sealed`

A continuación, **se observa** un ejemplo usando el modificador `sealed`:

```Java
public sealed class Fish permits ClownFish {}
public sealed class ClownFish extends Fish permits OrangeClownFish {}
public final class OrangeClownFish extends ClownFish {}
```

El modificador `sealed` aplicado a la subclase `ClownFish` significa que deben estar presentes el mismo tipo de reglas que **se aplicaron** a la clase padre `Fish`. Es decir, `ClownFish` define su propia lista de subclases permitidas. Note en este ejemplo que `OrangeClownFish` es una subclase indirecta de `Fish` pero no está nombrada en la clase `Fish`.

A pesar de permitir subclases indirectas no nombradas en `Fish`, la lista de clases que pueden heredar de `Fish` sigue siendo fija en tiempo de compilación. Si **se tiene** una referencia a un objeto `Fish`, debe ser un `Fish`, `ClownFish` u `OrangeClownFish`.

#### Creando Subclases `non-sealed`

El modificador `non-sealed` se utiliza para abrir una clase padre sellada a subclases potencialmente desconocidas.

```Java
abstract sealed class Mammal permits Feline {}
non-sealed class Feline extends Mammal {}
class Tiger extends Feline {}
```

En este ejemplo, **se puede crear** una subclase indirecta de `Mammal`, llamada `Tiger`, no nombrada en la declaración de `Mammal`. También note que `Tiger` no es `final`, por lo que puede ser extendida por cualquier subclase, como `BengalTiger`.

```Java
class BengalTiger extends Tiger {}
```

A primera vista, esto podría parecer un poco contraintuitivo. Después de todo, **se pudieron crear** subclases de `Mammal` que no fueron declaradas en `Mammal`. Entonces, ¿sigue estando sellada `Mammal`? Sí, pero eso es gracias al polimorfismo. Cualquier instancia de `Tiger` o `BengalTiger` también es una instancia de `Feline`, la cual está nombrada en la declaración de `Mammal`. **Se discutirá** el polimorfismo más hacia el final de este capítulo. Por ahora, solo **se necesita comprender** que `Mammal` está sellada para `Feline` y sus subclases.

> Si aún existe preocupación por abrir demasiado una clase sellada con una subclase `non-sealed`, **se debe recordar** que la persona que escribe la clase sellada puede ver la declaración de todas las subclases directas en tiempo de compilación. Ellos pueden decidir si permitir que se soporte la subclase `non-sealed`.

### Omitiendo la Cláusula permits

Hasta ahora, todos los ejemplos que **se han visto** han requerido una cláusula `permits` al declarar una clase sellada, pero este no es siempre el caso. Suponga que **se tiene** un archivo `Snake.java` con dos clases de nivel superior definidas en su interior:

```Java
// Snake.java
public sealed class Snake permits Cobra {}
final class Cobra extends Snake {}
```

En este caso, la cláusula `permits` es opcional y se puede omitir. Sin embargo, la palabra clave `extends` sigue siendo obligatoria en la subclase:

```Java
// Snake.java
public sealed class Snake {}
final class Cobra extends Snake {}
```

Si estas clases estuvieran en archivos separados, ¡este código no compilaría! Para omitir la cláusula `permits`, las declaraciones deben estar en el mismo archivo.

La cláusula `permits` también se puede omitir si las subclases están anidadas.

```Java
public sealed class Snake {
    final class Cobra extends Snake {}
}
```

**Se cubrirán** las clases anidadas en breve. Por ahora, solo **se necesita saber** que una clase anidada es una clase definida dentro de otra clase y que la regla de omisión también aplica a las clases anidadas. La Tabla 7.3 es una referencia útil para estos casos.

**TABLA 7.3 Uso de la cláusula permits en clases selladas**

|**Ubicación de las subclases directas**|**Cláusula permits**|
|---|---|
|En un archivo diferente al de la clase sellada|Obligatoria|
|En el mismo archivo que la clase sellada|Permitida, pero no obligatoria|
|Anidada dentro de la clase sellada|Permitida, pero no obligatoria|

> **Referenciando Subclases Anidadas**
> 
> Aunque hace que el código sea más fácil de leer si **se omite** la cláusula `permits` para las subclases anidadas, **se es libre** de nombrarlas. Sin embargo, la sintaxis podría ser diferente a la que **se espera**.
> ```Java
> public sealed class Snake permits Cobra { // NO COMPILA
>     final class Cobra extends Snake {}
> }
> ```
> Este código no compila porque `Cobra` requiere una referencia al espacio de nombres de `Snake`. Lo siguiente arregla este problema:
> ```Java
> public sealed class Snake permits Snake.Cobra {
>     final class Cobra extends Snake {}
> }
> ```
> Cuando todas las subclases están anidadas, **se recomienda encarecidamente** omitir la cláusula `permits`.

### Sellando Interfaces

Además de las clases, las interfaces también pueden ser selladas. La idea es análoga a la de las clases, y aplican muchas de las mismas reglas. Por ejemplo, la interfaz sellada debe aparecer en el mismo paquete o módulo nombrado que las clases o interfaces que la extienden o implementan directamente.

Una característica distintiva de una interfaz sellada es que la lista `permits` puede aplicar a una clase que implemente la interfaz o a una interfaz que extienda la interfaz.

```Java
// Interfaz sellada
public sealed interface Swims permits Duck, Swan, Floats {}

// Clases con permiso para implementar la interfaz sellada
public final class Duck implements Swims {}
public final class Swan implements Swims {}

// Interfaz con permiso para extender la interfaz sellada
public non-sealed interface Floats extends Swims {}
```

¿Qué modificadores están permitidos para las interfaces que extienden una interfaz sellada? Bueno, **se debe recordar** que las interfaces son implícitamente `abstract` y no pueden ser marcadas como `final`. Por esta razón, las interfaces que extienden una interfaz sellada solo pueden ser marcadas como `sealed` o `non-sealed`. No pueden ser marcadas como `final`.

### Aplicando Coincidencia de Patrones (Pattern Matching) a una Clase Sellada

**Se debe recordar** que, a partir del Capítulo 3, el `switch` ahora soporta coincidencia de patrones (_pattern matching_). ¿Qué pasaría si **se pudiera tratar** una clase sellada como un `enum` en un `switch` aplicando coincidencia de patrones? ¡Bueno, **se puede**! Dada una clase sellada `Fish` con dos subclases directas:

```Java
abstract sealed class Fish permits Trout, Bass {}
final class Trout extends Fish {}
final class Bass extends Fish {}
```

**Se puede definir** una expresión `switch` que no requiera una cláusula `default`:

```Java
public String getType(Fish fish) {
    return switch (fish) {
        case Trout t -> "Trout!";
        case Bass b -> "Bass!";
    };
}
```

Esto solo funciona porque `Fish` es abstracta y sellada, y se manejan todas las subclases posibles. Si **se elimina** el modificador `abstract` en la declaración de `Fish`, entonces la expresión `switch` no compilaría. Como ejercicio para el lector, véase si **se puede descubrir** cuántas maneras diferentes existen de cambiar la expresión `switch` que permitirían que compile de nuevo.

> Al igual que con los `enums`, **se debe asegurar** de que si un `switch` usa una clase sellada con coincidencia de patrones, todos los tipos posibles estén cubiertos o se incluya una cláusula `default`.

### Revisando las Reglas de las Clases Selladas

Cada vez que **se vea** una clase sellada en el examen, **se debe prestar** mucha atención a la declaración y a los modificadores de la subclase.

**Reglas de las Clases Selladas**

- Las clases selladas se declaran con los modificadores `sealed` y `permits`.
- Las clases selladas deben ser declaradas en el mismo paquete o módulo nombrado que sus subclases directas.
- Las subclases directas de las clases selladas deben ser marcadas como `final`, `sealed` o `non-sealed`. Para las interfaces que extienden una interfaz sellada, solo están permitidos los modificadores `sealed` y `non-sealed`.
- La cláusula `permits` es opcional si la clase sellada y sus subclases directas están declaradas dentro del mismo archivo o las subclases están anidadas dentro de la clase sellada.
- Las interfaces pueden ser selladas para limitar las clases que las implementan o las interfaces que las extienden.