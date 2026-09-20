En el Capítulo 6, "Diseño de Clases", **se mostró** cómo crear, inicializar y extender clases tanto abstractas como concretas. En este capítulo, **se va** más allá de las clases hacia otros tipos disponibles en Java, incluyendo interfaces, `enums`, clases selladas (_sealed classes_) y `records`. Muchas de las mismas reglas básicas que **se aprendieron** en el Capítulo 5, "Métodos", aún aplican, tales como los modificadores de acceso y los miembros `static`, aunque existen reglas adicionales para cada tipo. También **se cubre** el encapsulamiento y cómo proteger adecuadamente los miembros de instancia. Finalmente, **se concluye** este capítulo discutiendo los tipos anidados y la herencia polimórfica.

Para este capítulo, **se debe recordar** que un archivo Java puede tener como máximo un tipo de nivel superior (_top-level type_) `public`, y este debe coincidir con el nombre del archivo. Esto aplica a clases, `enums`, `records`, etc. Además, **se debe recordar** que un tipo de nivel superior solo se puede declarar con acceso `public` o de paquete (_package access_).

> **Anotaciones que se Deben Conocer para el Examen**
> 
> Otro tipo de nivel superior disponible en Java son las anotaciones (_annotations_), las cuales son "etiquetas" de metadatos que se pueden aplicar a clases, tipos, métodos e incluso variables. Además de las anotaciones `@Override` y `@FunctionalInterface`, las cuales **se cubren** en otros capítulos, el examen espera que **se esté al tanto** de las siguientes tres anotaciones:
> - `@Deprecated` permite a otros desarrolladores saber que una característica ya no tiene soporte y puede ser eliminada en futuras versiones. Si el código hace uso de una clase o método `@Deprecated`, puede desencadenar una advertencia del compilador.
> - `@SuppressWarnings` instruye al compilador para que ignore notificar al usuario sobre cualquier advertencia generada dentro de una sección de código.
> - `@SafeVarargs` permite a otros desarrolladores saber que un método no realiza ninguna operación potencialmente insegura en sus parámetros vararg.
> 
> En la práctica, crear anotaciones propias también es una habilidad útil, aunque este conocimiento no es requerido para el examen.

#### Implementando Interfaces

En el Capítulo 6, se aprendió sobre las clases abstractas, específicamente cómo crearlas y extenderlas. Puesto que las clases solo pueden extender una clase, tenían un uso limitado para la herencia. Por otro lado, una clase puede implementar cualquier cantidad de interfaces. Una **interfaz** es un tipo de datos abstracto que declara una lista de métodos abstractos que cualquier clase que implemente la interfaz debe proporcionar.

Con el tiempo, la definición precisa de una interfaz ha cambiado, ya que ahora se soportan tipos de métodos adicionales. En este capítulo, se comienza con una definición básica de una interfaz y se amplía para cubrir todos los miembros soportados.

#### Declarando y Usando una Interfaz

En Java, una interfaz se define con la palabra clave `interface`, análoga a la palabra clave `class` utilizada al definir una clase. Consulte la siguiente imagen para una declaración de interfaz adecuada.

![[Definición de una interfaz.png]]

En la anterior imagen, la declaración de la interfaz incluye un único método abstracto y una variable constante. Las variables de interfaz se denominan constantes porque se asume que son `public`, `static` y `final`. Se inicializan con un valor constante cuando se declaran. Puesto que son `public` y `static`, pueden ser utilizadas fuera de la declaración de la interfaz sin requerir una instancia de la interfaz. La imagen anterior también incluye un método abstracto que, al igual que una variable de interfaz, se asume que es `public`.

> Para abreviar, a menudo se dice "una instancia de una interfaz" en este capítulo para referirse a una instancia de una clase que implementa la interfaz.

¿Qué significa que se asuma que una variable o método sea algo? Un aspecto de la declaración de una interfaz que difiere de una clase abstracta es que contiene **modificadores implícitos**. Un modificador implícito es un modificador que el compilador inserta automáticamente en el código. Por ejemplo, ¡una interfaz siempre se considera abstracta, incluso si no está marcada como tal! En breve se cubren las reglas y ejemplos de los modificadores implícitos con más detalle.

Se comienza con un ejemplo sencillo. Suponga que se tiene una interfaz `WalksOnTwoLegs`, definida de la siguiente manera:

```Java
public abstract interface WalksOnTwoLegs {}
```

Compila porque no se requiere que las interfaces definan ningún método. El modificador `abstract` en este ejemplo es opcional para las interfaces, y el compilador lo insertará si no se proporciona. Ahora, considere los siguientes dos ejemplos:

```Java
final interface WalksOnEightLegs {} // NO COMPILA

public class Biped {
    public static void main(String[] args) {
        var e = new WalksOnTwoLegs(); // NO COMPILA
    }
}
```

La interfaz `WalksOnEightLegs` no compila porque las interfaces no pueden ser marcadas como `final` por la misma razón que las clases abstractas no pueden ser marcadas como `final`. Marcar una interfaz como `final` implicaría que ninguna clase podría implementarla nunca. La clase `Biped` tampoco compila, ya que `WalksOnTwoLegs` es una interfaz y no puede ser instanciada.

¿Cómo se usa una interfaz? Suponga que se tiene una interfaz `Climb`, definida de la siguiente manera:

```Java
public interface Climb {
    Number getSpeed(int age);
}
```

A continuación, se tiene una clase concreta `FieldMouse` que implementa la interfaz `Climb`, como se muestra en la imagen a continuación. 

![[Implementación de una interfaz.png]]

La clase `FieldMouse` declara que implementa la interfaz `Climb` e incluye una versión sobrescrita de `getSpeed()` heredada de la interfaz `Climb`. La anotación `@Override` es opcional. Sirve para hacer saber a otros desarrolladores que el método es heredado.

La firma del método `getSpeed()` coincide exactamente, y el tipo de retorno es covariante, puesto que un `Float` puede ser moldeado (_cast_) implícitamente a un `Number`. El modificador de acceso del método de la interfaz es implícitamente `public` en `Climb`, aunque la clase concreta `FieldMouse` debe declararlo explícitamente.

Como se muestra en imagen, una clase puede implementar múltiples interfaces, cada una separada por una coma (`,`). Si alguna de las interfaces define métodos abstractos, entonces la clase concreta está obligada a sobrescribirlos. En este caso, `FieldMouse` implementa la interfaz `CanBurrow` que se vio en la primera imagen. De esta manera, la clase sobrescribe dos métodos abstractos al mismo tiempo con una sola declaración de método. Se aprenderá más sobre métodos de interfaz duplicados y compatibles en este capítulo.

#### Extendiendo una Interfaz

Al igual que una clase, una interfaz puede extender otra interfaz usando la palabra clave `extends`.

```Java
public interface Nocturnal {}
public interface HasBigEyes extends Nocturnal {}
```

A diferencia de una clase, que solo puede extender una clase, una interfaz puede extender **múltiples** interfaces.

```Java
public interface Nocturnal {
    public int hunt();
}
public interface CanFly {
    public void flap();
}
public interface HasBigEyes extends Nocturnal, CanFly {}
public class Owl implements HasBigEyes {
    public int hunt() { return 5; }
    public void flap() { System.out.println("Flap!"); }
}
```

En este ejemplo, la clase `Owl` implementa la interfaz `HasBigEyes` y debe implementar los métodos `hunt()` y `flap()`. Extender dos interfaces está permitido porque las interfaces no se inicializan como parte de una jerarquía de clases. A diferencia de las clases abstractas, no contienen constructores y no son parte de la inicialización de instancias. Las interfaces simplemente definen un conjunto de reglas y métodos que una clase que las implemente debe seguir.

#### Heredando una Interfaz

Al igual que una clase abstracta, cuando una clase concreta hereda una interfaz, todos los métodos abstractos heredados deben ser implementados. Se ilustra este principio en la siguiente imagen. ¿Cuántos métodos abstractos hereda la clase concreta `Swan`?

![[Herencia de interfaces.png]]

¿Te rindes? La clase concreta `Swan` hereda cuatro métodos abstractos que debe sobrescribir: `getType()`, `canSwoop()`, `fly()` y `swim()`.

Se analizará otro ejemplo que involucra una clase abstracta que implementa una interfaz:

```Java
public interface HasTail {
    public int getTailLength();
}
public interface HasWhiskers {
    public int getNumberOfWhiskers();
}
public abstract class HarborSeal implements HasTail, HasWhiskers {}
public class CommonSeal extends HarborSeal {} // NO COMPILA
```

La clase `HarborSeal` compila porque es abstracta y no está obligada a implementar ninguno de los métodos abstractos que hereda. La clase concreta `CommonSeal`, sin embargo, debe sobrescribir todos los métodos abstractos heredados.

> **Mezclando las Palabras Clave class e interface**
> 
> A los creadores del examen les gustan las preguntas que mezclan terminología de clases e interfaces. Aunque una clase puede implementar una interfaz, una clase no puede extender una interfaz. Del mismo modo, mientras que una interfaz puede extender otra interfaz, una interfaz no puede implementar otra interfaz. Los siguientes ejemplos ilustran estos principios:
> ```Java
> public interface CanRun {}
> public class Cheetah extends CanRun {} // NO COMPILA
> public class Hyena {}
> public interface HasFur extends Hyena {} // NO COMPILA
> ```
> 
> Se debe tener cuidado con las preguntas del examen que mezclan declaraciones de clases e interfaces.

#### Heredando Métodos Abstractos Duplicados

Java soporta la herencia de dos métodos abstractos que tienen declaraciones de métodos compatibles.

```Java
public interface Herbivore { public int eatPlants(int plantsLeft); }
public interface Omnivore { public int eatPlants(int foodRemaining); }
public class Bear implements Herbivore, Omnivore {
    public int eatPlants(int plants) {
        System.out.print("Eating plants");
        return plants - 1;
    } 
}
```

Por compatible, se entiende que se puede escribir un método que sobrescriba adecuadamente ambos métodos heredados: por ejemplo, utilizando tipos de retorno covariantes sobre los que se aprendió en el Capítulo 6. Note que los nombres de los parámetros del método no necesitan coincidir, solo el tipo.

El siguiente es un ejemplo de una declaración incompatible:

```Java
public interface Herbivore { public void eatPlants(int plantsLeft); }
public interface Omnivore { public int eatPlants(int foodRemaining); }
public class Tiger implements Herbivore, Omnivore { // NO COMPILA
    // ¡No importa!
}
```

La implementación de `Tiger` no importa en este caso ya que es imposible escribir una versión de `Tiger` que satisfaga ambos métodos abstractos heredados. El código no compila, independientemente de lo que se declare dentro de la clase `Tiger`.

#### Insertando Modificadores Implícitos

Como se ha mencionado anteriormente, un **modificador implícito** es aquel que el compilador inserta automáticamente. Es similar a cuando el compilador inserta un constructor predeterminado sin argumentos si no se define ningún constructor, tal y como se explicó en el capítulo 6. Puedes optar por insertar estos modificadores implícitos tú mismo o dejar que el compilador lo haga por ti.

La siguiente lista incluye los modificadores implícitos para interfaces que se necesitan conocer para el examen:

- Las interfaces son implícitamente `abstract`.
- Las variables de interfaz son implícitamente `public`, `static` y `final`.
- Los métodos de interfaz sin cuerpo son implícitamente `abstract`.
- Los métodos de interfaz sin el modificador `private` son implícitamente `public`.


La última regla se aplica a los métodos de interfaz abstractos, por defecto (`default`) y estáticos, los cuales se cubren en la siguiente sección.

Se analizará un ejemplo. Las siguientes dos definiciones de interfaz son equivalentes, ya que el compilador las convertirá ambas a la segunda declaración:

```Java
public interface Soar {
    int MAX_HEIGHT = 10;
    final static boolean UNDERWATER = true;
    void fly(int speed);
    abstract void takeoff();
    public abstract double dive();
}
```

```Java
public abstract interface Soar {
    public static final int MAX_HEIGHT = 10;
    public final static boolean UNDERWATER = true;
    public abstract void fly(int speed);
    public abstract void takeoff();
    public abstract double dive();
}
```
#### Modificadores en Conflicto

¿Qué sucede si un desarrollador marca un método o variable con un modificador que entra en conflicto con un modificador implícito? Por ejemplo, si un método abstracto es implícitamente `public`, ¿puede ser marcado explícitamente como `protected` o `private`?

```Java
public interface Dance {
    private int count = 4; // NO COMPILA
    protected void step(); // NO COMPILA
}
```

Ninguna de estas declaraciones de miembros de la interfaz compila, ya que el compilador aplicará el modificador `public` a ambas, resultando en un conflicto.

#### Diferencias entre Interfaces y Clases Abstractas

Aunque tanto las clases abstractas como las interfaces se consideran tipos abstractos, solo las interfaces hacen uso de modificadores implícitos. ¿Cómo difieren los métodos `play()` en las siguientes dos definiciones?

```Java
abstract class Husky {
    abstract void play(); // Se requiere el modificador abstract
}
interface Poodle {
    void play(); // El modificador abstract es opcional
}
```

Ambas definiciones de métodos se consideran abstractas. Dicho esto, la clase `Husky` no compilará si el método `play()` no está marcado como `abstract`, mientras que el método en la interfaz `Poodle` compilará con o sin el modificador `abstract`.

¿Qué hay del nivel de acceso del método `play()`? ¿Se puede detectar algo incorrecto en las siguientes definiciones de clases que usan nuestros tipos abstractos?

```Java
public class Webby extends Husky {
    void play() {} // play() se declara con acceso de paquete en Husky
}
public class Georgette implements Poodle {
    void play() {} // NO COMPILA - play() es public en Poodle
}
```

La clase `Webby` compila, pero la clase `Georgette` no. A pesar de que las dos implementaciones de métodos son idénticas, el método en la clase `Georgette` reduce el modificador de acceso en el método de `public` a acceso de paquete.

#### Declarando Métodos de Interfaz Concretos

La Tabla 7.1 enumera los seis tipos de miembros de interfaz que se necesitan conocer para el examen. Ya se han cubierto los métodos abstractos y las constantes, por lo que la atención se centrará en los cuatro métodos concretos restantes en esta sección.

En la Tabla 7.1, el **tipo de pertenencia** (_membership type_) determina cómo se puede acceder a él. Un método con un tipo de pertenencia de **clase** se comparte entre todas las instancias de la interfaz, mientras que un método con un tipo de pertenencia de **instancia** está asociado con una instancia particular de la interfaz.

**TABLA 7.1 Tipos de miembros de interfaz**

|**Tipo de miembro**|**Tipo de pertenencia**|**Modificadores requeridos**|**Modificadores implícitos**|**¿Tiene valor o cuerpo?**|
|---|---|---|---|---|
|Variable constante|Clase|—|`public static final`|Sí|
|Método `abstract`|Instancia|—|`public abstract`|No|
|Método `default`|Instancia|`default`|`public`|Sí|
|Método `static`|Clase|`static`|`public`|Sí|
|Método `private`|Instancia|`private`|—|Sí|
|Método `private static`|Clase|`private static`|—|Sí|

> **¿Qué pasa con los miembros de interfaz `protected` o de paquete?**
> 
> Las interfaces no soportan miembros `protected`, ya que una clase no puede extender una interfaz. Tampoco soportan miembros de acceso de paquete (_package access_), aunque es más probable que sea por razones de sintaxis y compatibilidad con versiones anteriores. Puesto que los métodos de interfaz sin un modificador de acceso se han considerado implícitamente `public`, ¡cambiar este comportamiento a acceso de paquete rompería muchos programas existentes!

#### Escribiendo un Método de Interfaz `default`

El primer tipo de método concreto con el que se debería estar familiarizado para el examen es un **método por defecto** (_default method_). Un método por defecto es un método definido en una interfaz con la palabra clave `default` e incluye el cuerpo de un método. Puede ser opcionalmente sobrescrito por una clase que implemente la interfaz.

Un uso de los métodos por defecto es para la compatibilidad con versiones anteriores. Se puede añadir un nuevo método por defecto a una interfaz sin la necesidad de modificar todas las clases existentes que implementan la interfaz. Las clases más antiguas simplemente usarán la implementación por defecto del método definida en la interfaz. ¡De aquí es de donde proviene el nombre _default method_!

El siguiente es un ejemplo de un método por defecto definido en una interfaz:

```Java
public interface IsColdBlooded {
    boolean hasScales();
    default double getTemperature() {
        return 10.0;
    }
}
```

Este ejemplo define dos métodos de interfaz, uno abstracto y uno por defecto. La siguiente clase `Snake`, la cual implementa `IsColdBlooded`, debe implementar `hasScales()`. Puede depender de la implementación por defecto de `getTemperature()` o sobrescribir el método con su propia versión:

```Java
public class Snake implements IsColdBlooded {
    public boolean hasScales() { // Sobrescritura requerida
        return true;
    }
    public double getTemperature() { // Sobrescritura opcional
        return 12.2;
    } 
}
```

> Note que el modificador de método de interfaz `default` no es lo mismo que la etiqueta `default` usada en un `switch`. De igual forma, aunque al acceso de paquete a veces se le llama _default access_, esa característica se implementa omitiendo un modificador de acceso. ¡Disculpas si esto es confuso! ¡Se coincide en que Java ha abusado de la palabra `default` a lo largo de los años!

Para el examen, se debería estar familiarizado con varias reglas para declarar métodos por defecto.

**Reglas de Definición de Métodos de Interfaz `default`**

1. Un método `default` solo puede ser declarado dentro de una interfaz.
2. Un método `default` debe estar marcado con la palabra clave `default` e incluir el cuerpo de un método.
3. Un método `default` es implícitamente `public`.
4. Un método `default` no puede ser marcado como `abstract`, `final` o `static`.
5. Un método `default` puede ser sobrescrito por una clase que implemente la interfaz.
6. Si una clase hereda dos o más métodos `default` con la misma firma de método, entonces la clase debe sobrescribir el método.

La primera regla debería dar algo de consuelo al saber que solo se verán métodos `default` en las interfaces. Si se ven en una clase o enumeración (`enum`) en el examen, algo anda mal. La segunda regla simplemente denota la sintaxis, ya que los métodos `default` deben usar la palabra clave `default`. Por ejemplo, los siguientes fragmentos de código no compilarán porque mezclan métodos de interfaz concretos y abstractos:

```Java
public interface Carnivore {
    public default void eatMeat(); // NO COMPILA
    public int getRequiredFoodAmount() { // NO COMPILA
        return 13;
    } 
}
```

Las siguientes tres reglas para los métodos `default` se derivan de la relación con los métodos de interfaz abstractos. Al igual que los métodos de interfaz abstractos, los métodos `default` son implícitamente `public`. A diferencia de los métodos abstractos, sin embargo, los métodos de interfaz `default` no pueden ser marcados como `abstract` puesto que proporcionan un cuerpo. Tampoco pueden ser marcados como `final`, porque están diseñados para que puedan ser sobrescritos en las clases que implementan la interfaz, al igual que los métodos abstractos. Finalmente, no pueden ser marcados como `static` puesto que están asociados con la instancia de la clase que implementa la interfaz.

#### Heredando Métodos `default` Duplicados

La última regla para crear un método de interfaz `default` requiere de una explicación. Por ejemplo, ¿qué valor produciría el siguiente código?

```Java
public interface Walk {
    public default int getSpeed() { return 5; }
}
public interface Run {
    public default int getSpeed() { return 10; }
}
public class Cat implements Walk, Run {} // NO COMPILA
```

En este ejemplo, `Cat` hereda los dos métodos `default` para `getSpeed()`, entonces, ¿cuál usa? Puesto que `Walk` y `Run` se consideran hermanos en términos de cómo se usan en la clase `Cat`, no está claro si el código debería generar 5 o 10. En este caso, el compilador se rinde y dice: "¡Demasiado difícil, me rindo!" y falla.

Sin embargo, no todo está perdido. Si la clase que implementa las interfaces sobrescribe el método `default` duplicado, el código compilará sin problema. Al sobrescribir el método en conflicto, se elimina la ambigüedad sobre qué versión del método llamar. Por ejemplo, la siguiente implementación modificada de `Cat` compilará:

```Java
public class Cat implements Walk, Run {
    public int getSpeed() { return 1; }
}
```

#### Llamando a un Método `default`

Un método `default` existe en cualquier objeto que herede la interfaz, no en la interfaz en sí. En otras palabras, se debería tratar como un método heredado que puede ser opcionalmente sobrescrito, más que como un método estático. Considere lo siguiente:

```Java
public interface Dance {
    default int getRhythm() { return 33; }
}
public class Snake implements Dance {
    static void move() {
        var snake = new Snake();
        System.out.print(snake.getRhythm());
        System.out.print(Dance.getRhythm()); // NO COMPILA
    } 
}
```

La primera llamada a `getRhythm()` compila porque se llama en una instancia de la clase `Snake`. La segunda no compila porque no es un método `static` y requiere una instancia de `Dance`.

En la sección anterior, se mostró cómo la clase `Cat` podía sobrescribir un par de métodos `default` en conflicto, pero ¿qué pasaría si la clase `Cat` quisiera acceder a la versión "oculta" de `getSpeed()` en `Walk` o `Run`? ¿Sigue siendo accesible? Sí, pero requiere una sintaxis especial.

```Java
public class Cat implements Walk, Run {
    public int getSpeed() {
        return 1;
    }
    public int getWalkSpeed() {
        return Walk.super.getSpeed();
    } 
}
```

Esta es un área donde un método `default` `getSpeed()` exhibe propiedades tanto de un método de instancia como estático. Se usa el nombre de la interfaz para indicar qué método se desea llamar, pero se usa la palabra clave `super` para mostrar que se está siguiendo la herencia de instancias, no la herencia de clases. Note que llamar a `Walk.this.getSpeed()` no habría funcionado. Es un poco confuso, se sabe, pero se necesita estar familiarizado con esta sintaxis para el examen.

#### Declarando Métodos de Interfaz `static`

Las interfaces también pueden incluir métodos `static`. Estos métodos se definen explícitamente con la palabra clave `static` y, en su mayor parte, se comportan igual que los métodos `static` definidos en las clases.

**Reglas de Definición de Métodos de Interfaz `static`**

1. Un método `static` debe estar marcado con la palabra clave `static` e incluir el cuerpo de un método.
2. Un método `static` sin un modificador de acceso es implícitamente `public`.
3. Un método `static` no puede ser marcado como `abstract` o `final`.
4. Un método `static` no se hereda y no se puede acceder a él en una clase que implementa la interfaz sin una referencia al nombre de la interfaz.

Estas reglas deberían derivarse de lo que se conoce hasta ahora sobre las clases, las interfaces y los métodos `static`. Por ejemplo, tampoco se pueden declarar métodos `static` sin cuerpo en las clases. Al igual que los métodos de interfaz `default` y abstractos, los métodos de interfaz `static` son implícitamente `public` si se declaran sin un modificador de acceso. Como se verá en breve, se puede usar el modificador de acceso `private` con métodos `static`.

Se analizará un método de interfaz `static`:

```Java
public interface Hop {
    static int getJumpHeight() {
        return 8;
    } 
}
```

Dado que el método se define sin un modificador de acceso, el compilador insertará automáticamente el modificador de acceso `public`. El método `getJumpHeight()` funciona igual que un método `static` definido en una clase. En otras palabras, se puede acceder a él sin una instancia de una clase.

```Java
public class Skip implements Hop {
    public int skip() {
        return Hop.getJumpHeight();
    } 
}
```

La última regla sobre la herencia podría ser un poco confusa, así que se observará un ejemplo. El siguiente es un ejemplo de una clase `Bunny` que implementa `Hop` y no compila:

```Java
public class Bunny implements Hop {
    public void printDetails() {
        System.out.println(getJumpHeight()); // NO COMPILA
    } 
}
```

Sin una referencia explícita al nombre de la interfaz, el código no compilará, incluso si `Bunny` implementa `Hop`. Esto se puede arreglar fácilmente usando el nombre de la interfaz:

```Java
public class Bunny implements Hop {
    public void printDetails() {
        System.out.println(Hop.getJumpHeight());
    } 
}
```

Note que no se tiene el mismo problema que cuando se heredaron dos métodos de interfaz `default` con la misma firma. Java "resolvió" el problema de herencia múltiple de los métodos de interfaz `static` ¡no permitiendo que sean heredados!

#### Reutilizando Código con Métodos de Interfaz `private`

Los dos últimos tipos de métodos concretos que se pueden agregar a las interfaces son los métodos de interfaz `private` y `private static`. Debido a que ambos tipos de métodos son privados, solo pueden ser utilizados en la declaración de la interfaz en la que están declarados. Por esta razón, se agregaron principalmente para reducir la duplicación de código. Por ejemplo, Considere el siguiente fragmento de código:

```Java
public interface Schedule {
    default void wakeUp() { checkTime(7); }
    private void haveBreakfast() { checkTime(9); }
    static void workOut() { checkTime(18); }
    private static void checkTime(int hour) {
        if (hour > 17) {
            System.out.println("You're late!");
        } else {
            System.out.println("You have "+(17-hour)+" hours left "
            + "to make the appointment");
        }
    }
}
```

Se podría escribir esta interfaz sin usar un método `private` copiando el contenido del método `checkTime()` en los lugares donde se usa. ¡Es mucho más corto y fácil de leer si no se hace! Dado que los autores de Java fueron lo suficientemente amables de agregar esta característica para comodidad, ¡bien se podría usar!

También se podría haber declarado `checkTime()` como `public` en el ejemplo anterior, pero esto expondría el método para su uso fuera de la interfaz. Un principio importante del encapsulamiento es no exponer el funcionamiento interno de una clase o interfaz cuando no es requerido. Se cubre el encapsulamiento más adelante en este capítulo.

La diferencia entre un método privado no estático y uno estático es análoga a la diferencia entre un método de instancia y un método estático declarado dentro de una clase. En particular, todo se trata de a qué métodos se puede llamar desde cada uno.

**Reglas de Definición de Métodos de Interfaz `private`**

1. Un método de interfaz `private` debe estar marcado con el modificador `private` e incluir el cuerpo de un método.
2. Un método de interfaz `private static` puede ser llamado por cualquier método dentro de la definición de la interfaz.
3. Un método de interfaz `private` solo puede ser llamado por los métodos `default` y otros métodos privados no estáticos dentro de la definición de la interfaz.

Otra forma de pensarlo es que un método de interfaz `private` solo es accesible a los métodos no estáticos definidos dentro de la interfaz. Un método de interfaz `private static`, por otro lado, puede ser accedido por cualquier método en la interfaz. Para ambos tipos de métodos privados, una clase que hereda la interfaz no puede invocarlos directamente.

#### Repasando los Miembros de Interfaz

Se concluye la discusión sobre los miembros de interfaz con la Tabla 7.2, la cual muestra las reglas de acceso para los miembros dentro y fuera de una interfaz.

**TABLA 7.2 Acceso a miembros de interfaz**

||**¿Accesible desde métodos default y privados dentro de la interfaz?**|**¿Accesible desde métodos static dentro de la interfaz?**|**¿Accesible en clases que heredan la interfaz?**|**¿Accesible sin una instancia de la interfaz?**|
|---|---|---|---|---|
|Variable constante|Sí|Sí|Sí|Sí|
|Método `abstract`|Sí|No|Sí|No|
|Método `default`|Sí|No|Sí|No|
|Método `static`|Sí (nombre de interfaz requerido)|Sí|Sí (nombre de interfaz requerido)|Sí|
|Método `private`|Sí|No|No|No|
|Método `private static`|Sí|Sí|No|No|

Si bien la Tabla 7.2 puede parecer mucha información para recordar, aquí hay algunos consejos rápidos para el examen:

- Tratar los métodos `abstract`, `default` y privados no estáticos como pertenecientes a una instancia de la interfaz.
- Tratar los métodos y variables `static` como pertenecientes al objeto de clase de la interfaz.
- Todos los tipos de métodos de interfaz privados solo son accesibles dentro de la declaración de la interfaz.

Usando estas reglas, ¿cuáles de los siguientes métodos no compilan?

```Java
public interface ZooTrainTour {
    abstract int getTrainName();
    private static void ride() {}
    default void playHorn() { getTrainName(); ride(); }
    public static void slowDown() { playHorn(); }
    static void speedUp() { ride(); }
}
```

El método `ride()` es privado y estático, por lo que puede ser accedido por cualquier método `default` o estático dentro de la declaración de la interfaz. El método `getTrainName()` es abstracto, por lo que puede ser accedido por un método `default` asociado a la instancia. Sin embargo, el método `slowDown()` es estático y no puede llamar a un método `default` o privado, como `playHorn()`, sin un objeto de referencia explícito. Por lo tanto, el método `slowDown()` no compila.

¡Felicidades! Se acaba de aprender mucho sobre las interfaces, probablemente más de lo que se creía posible. Ahora se debe tomar un respiro profundo. ¿Listo? El siguiente tipo que se va a cubrir son los enumeradores (`enums`).
