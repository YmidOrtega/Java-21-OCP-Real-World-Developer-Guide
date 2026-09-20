Al diseñar un modelo, a veces **se desea** crear una entidad que no pueda ser instanciada directamente. Por ejemplo, suponga que **se tiene** una clase `Canine` con las subclases `Wolf`, `Fox` y `Coyote`. **Se desea** que otros desarrolladores puedan crear instancias de las subclases, pero quizás no **se desea** que puedan crear una instancia de `Canine`. En otras palabras, **se desea** forzar a que todos los objetos de `Canine` tengan un tipo particular en tiempo de ejecución (_runtime_).

### Introduciendo Clases Abstractas

Entran en juego las **clases abstractas** (_abstract classes_). Una clase abstracta es una clase declarada con el modificador `abstract` que no puede ser instanciada directamente y puede contener métodos abstractos. **Se analizará** un ejemplo basado en el modelo de datos `Canine`:

```Java
public abstract class Canine {}
public class Wolf extends Canine {}
public class Fox extends Canine {}
public class Coyote extends Canine {}
```

En este ejemplo, otros desarrolladores pueden crear instancias de `Wolf`, `Fox` o `Coyote`, pero no de `Canine`. Ciertamente, pueden pasar una referencia de variable como `Canine`, pero el objeto subyacente debe ser una subclase de `Canine` en tiempo de ejecución.

¡Pero hay más! Una clase abstracta puede contener **métodos abstractos** (_abstract methods_). Un método abstracto es un método declarado con el modificador `abstract` que no define un cuerpo. Dicho de otra manera, un método abstracto fuerza a las subclases a sobrescribir (_override_) el método.

¿Por qué **se querría** esto? ¡Por el **polimorfismo**, por supuesto! Al declarar un método como `abstract`, **se puede garantizar** que alguna versión estará disponible en una instancia sin tener que especificar cuál es esa versión en la clase padre abstracta.

```Java
public abstract class Canine {
    public abstract String getSound();
    public void bark() { System.out.println(getSound()); }
}
public class Wolf extends Canine {
    public String getSound() {
        return "Wooooooof!";
    } 
}
public class Fox extends Canine {
    public String getSound() {
        return "Squeak!";
    } 
}
public class Coyote extends Canine {
    public String getSound() {
        return "Roar!";
    }
}
```

Entonces **se puede crear** una instancia de `Fox` y asignarla al tipo padre `Canine`. El método sobrescrito será utilizado en tiempo de ejecución.

```Java
public static void main(String[] p) {
    Canine w = new Fox();
    w.bark(); // Squeak!
}
```

Sencillo hasta ahora. Pero hay algunas reglas que **se deben tener** en cuenta:

- Solo los métodos de instancia pueden ser marcados como `abstract` dentro de una clase, no las variables, constructores o métodos `static`.
- Una clase abstracta puede incluir cero o más métodos abstractos, mientras que una clase no abstracta no puede contener ninguno.
- Una clase no abstracta que extiende a una clase abstracta debe implementar todos los métodos abstractos heredados.
- Sobrescribir un método abstracto sigue las reglas existentes para la sobrescritura de métodos que **se aprendieron** anteriormente en el capítulo.
 

**Se observará** si **se puede identificar** por qué cada una de estas declaraciones de clases no compila:

```Java
public class FennecFox extends Canine {
    public int getSound() {
        return 10;
    } 
}
public class ArcticFox extends Canine {}
public class Direwolf extends Canine {
    public abstract rest();
    public String getSound() {
        return "Roof!";
    } 
}
public class Jackal extends Canine {
    public abstract String name;
    public String getSound() {
        return "Laugh";
    } 
}
```

En primer lugar, la clase `FennecFox` no compila porque es una sobrescritura de método inválida. En particular, los tipos de retorno no son covariantes. La clase `ArcticFox` no compila porque no sobrescribe el método abstracto `getSound()`. La clase `Direwolf` no compila porque no es abstracta pero declara un método abstracto `rest()`. Finalmente, la clase `Jackal` no compila porque las variables no pueden ser marcadas como `abstract`.

Una clase abstracta se utiliza más comúnmente cuando **se desea** que otra clase herede propiedades de una clase particular, pero **se desea** que la subclase complete algunos de los detalles de implementación.

Anteriormente, **se dijo** que una clase abstracta es una que no puede ser instanciada. Esto significa que si **se intenta** instanciarla, el compilador reportará una excepción, como en este ejemplo:

```Java
abstract class Alligator {
    public static void main(String... food) {
        var a = new Alligator(); // NO COMPILA
    }
}
```

Una clase abstracta puede ser inicializada, pero solo como parte de la instanciación de una subclase no abstracta.

### Declarando Métodos Abstractos

Un método abstracto siempre se declara sin un cuerpo. También incluye un punto y coma (`;`) después de la declaración del método. Como **se observó** en el ejemplo anterior, una clase abstracta puede incluir métodos no abstractos, en este caso con el método `bark()`. De hecho, una clase abstracta puede incluir todos los mismos miembros que una clase no abstracta, incluyendo variables, métodos `static` y de instancia, constructores, etc.

Podría ser sorprendente saber que una clase abstracta no está obligada a incluir ningún método abstracto. Por ejemplo, el siguiente código compila a pesar de que no define ningún método abstracto:

```Java
public abstract class Llama {
    public void chew() {}
}
```

Incluso sin métodos abstractos, la clase no puede ser instanciada directamente. Para el examen, **se debe estar atento** a los métodos abstractos declarados fuera de clases abstractas, como el siguiente:

```Java
public class Egret { // NO COMPILA
    public abstract void peck();
}
```

A los creadores del examen les gusta incluir declaraciones de clases inválidas, mezclando clases no abstractas con métodos abstractos.

Al igual que el modificador `final`, el modificador `abstract` puede colocarse antes o después del modificador de acceso en las declaraciones de clases y métodos, como se muestra en esta clase `Tiger`:

```Java
abstract public class Tiger {
    abstract public int claw();
}
```

El modificador `abstract` no puede ser colocado después de la palabra clave `class` en una declaración de clase ni después del tipo de retorno en una declaración de método. Las siguientes declaraciones de `Bear` y `howl()` no compilan por estas razones:

```Java
public class abstract Bear { // NO COMPILA
    public int abstract howl();  // NO COMPILA
}
```

No es posible definir un método abstracto que tenga un cuerpo o una implementación por defecto. Aún **se puede definir** un método por defecto con un cuerpo; simplemente no **se puede marcar** como `abstract`. Siempre y cuando no **se marque** el método como `final`, la subclase tiene la opción de sobrescribir el método heredado.

### Creando una Clase Concreta

Una clase abstracta se vuelve utilizable cuando es extendida por una subclase concreta. Una **clase concreta** (_concrete class_) es una clase no abstracta. La primera subclase concreta que extiende una clase abstracta está obligada a implementar todos los métodos abstractos heredados. Esto incluye implementar cualquier método abstracto heredado de interfaces heredadas, como **se verá** en el próximo capítulo.

Cuando **se vea** una clase concreta extendiendo una clase abstracta en el examen, **se debe verificar** para asegurarse de que implementa todos los métodos abstractos requeridos. ¿**Se puede identificar** por qué la siguiente clase `Walrus` no compila?

```Java
public abstract class Animal {
    public abstract String getName();
}
public class Walrus extends Animal {} // NO COMPILA
```

En este ejemplo, **se observa** que `Animal` está marcada como `abstract` y `Walrus` no, haciendo de `Walrus` una subclase concreta de `Animal`. Dado que `Walrus` es la primera subclase concreta, debe implementar todos los métodos abstractos heredados (`getName()` en este ejemplo). Debido a que no lo hace, el compilador reporta un error con la declaración de `Walrus`.

**Se destaca** la primera subclase concreta por una razón. Una clase abstracta puede extender una clase no abstracta y viceversa. Siempre que una clase concreta esté extendiendo una clase abstracta, debe implementar todos los métodos que se heredan como abstractos. **Se ilustrará** esto con un conjunto de clases heredadas:

```Java
public abstract class Mammal {
    abstract void showHorn();
    abstract void eatLeaf();
}
public abstract class Rhino extends Mammal {
    void showHorn() {} // Heredado de Mammal
}
public class BlackRhino extends Rhino {
    void eatLeaf() {}  // Heredado de Mammal
}
```

En este ejemplo, la clase `BlackRhino` es la primera subclase concreta, mientras que las clases `Mammal` y `Rhino` son abstractas. La clase `BlackRhino` hereda el método `eatLeaf()` como abstracto y, por lo tanto, está obligada a proporcionar una implementación, lo cual hace.

¿Qué ocurre con el método `showHorn()`? Puesto que la clase padre, `Rhino`, proporciona una implementación de `showHorn()`, el método es heredado en `BlackRhino` como un método no abstracto. Por esta razón, a la clase `BlackRhino` se le permite, pero no se le exige, sobrescribir el método `showHorn()`. Las tres clases en este ejemplo están correctamente definidas y compilan.

¿Qué pasaría si **se cambiara** la declaración de `Rhino` para eliminar el modificador `abstract`?

```Java
public class Rhino extends Mammal { // NO COMPILA
    void showHorn() {}
}
```

Al cambiar `Rhino` a una clase concreta, se convierte en la primera clase no abstracta en extender la clase abstracta `Mammal`. Por lo tanto, debe proporcionar una implementación tanto para el método `showHorn()` como para `eatLeaf()`. Dado que solo proporciona uno de estos métodos, la declaración modificada de `Rhino` no compila.

**Se probará** un ejemplo más. La siguiente clase concreta `Lion` hereda dos métodos abstractos, `getName()` y `roar()`:

```Java
public abstract class Animal {
    abstract String getName();
}
public abstract class BigCat extends Animal {
    protected abstract void roar();
}
public class Lion extends BigCat {
    public String getName() {
        return "Lion";
    }
    public void roar() {
        System.out.println("The Lion lets out a loud ROAR!");
    }
}
```

En este código de muestra, `BigCat` extiende `Animal` pero está marcada como `abstract`; por lo tanto, no está obligada a proporcionar una implementación para el método `getName()`. La clase `Lion` no está marcada como `abstract`, y como la primera subclase concreta, debe implementar todos los métodos abstractos heredados no definidos en una clase padre. Las tres clases compilan con éxito.

### Creando Constructores en Clases Abstractas

Aunque las clases abstractas no pueden ser instanciadas, aún son inicializadas a través de constructores por sus subclases. Por ejemplo, considere el siguiente programa:

```Java
abstract class Mammal {
    abstract CharSequence chew();
    public Mammal() {
        System.out.println(chew()); // ¿Compila esta línea?
    }
}
public class Platypus extends Mammal {
    String chew() { return "yummy!"; }
    public static void main(String[] args) {
        new Platypus();
    }
}
```

Utilizando las reglas de constructores que **se aprendieron** anteriormente en este capítulo, el compilador inserta un constructor por defecto sin argumentos en la clase `Platypus`, el cual primero llama a `super()` en la clase `Mammal`. El constructor de `Mammal` solo es llamado cuando la clase abstracta está siendo inicializada a través de una subclase; por lo tanto, hay una implementación de `chew()` en el momento en que se llama al constructor. Este código compila e imprime `yummy!` en tiempo de ejecución.

Para el examen, **se debe recordar** que las clases abstractas se inicializan con constructores de la misma manera que las clases no abstractas. Por ejemplo, si una clase abstracta no proporciona un constructor, el compilador insertará automáticamente un constructor por defecto sin argumentos.

La diferencia principal entre un constructor en una clase abstracta y una clase no abstracta es que un constructor en una clase abstracta solo puede ser llamado cuando está siendo inicializado por una subclase no abstracta. Esto tiene sentido, ya que las clases abstractas no pueden ser instanciadas.

### Detectando Declaraciones Inválidas

**Se concluye** la discusión sobre clases abstractas con una revisión de problemas potenciales que **es más probable encontrar** en el examen que en la vida real. A los redactores del examen les gustan las preguntas con métodos marcados como `abstract` para los cuales también se define una implementación. Por ejemplo, ¿**se puede identificar** por qué cada uno de los siguientes métodos no compila?

```Java
public abstract class Turtle {
    public abstract long eat() // NO COMPILA
    public abstract void swim() {}; // NO COMPILA
    public abstract int getAge() { // NO COMPILA
        return 10;
    }
    public abstract void sleep; // NO COMPILA
    public void goInShell(); // NO COMPILA
}
```

El primer método, `eat()`, no compila porque está marcado como `abstract` pero no termina con un punto y coma (`;`). Los dos métodos siguientes, `swim()` y `getAge()`, no compilan porque están marcados como `abstract`, pero proporcionan un bloque de implementación encerrado entre llaves (`{}`). Para el examen, **se debe recordar** que la declaración de un método abstracto debe terminar en un punto y coma sin llaves. El siguiente método, `sleep`, no compila porque le faltan los paréntesis, `()`, para los argumentos del método. El último método, `goInShell()`, no compila porque no está marcado como `abstract` y, por lo tanto, debe proporcionar un cuerpo encerrado entre llaves.

**Se debe asegurar** de comprender por qué cada uno de los métodos anteriores no compila y que **se pueden detectar** errores como estos en el examen. Si **se encuentra** una pregunta en el examen en la que una clase o método está marcado como `abstract`, **se debe asegurar** de que la clase esté implementada adecuadamente antes de intentar resolver el problema.

### Modificadores abstract y final

¿Qué sucedería si **se marcara** una clase o método como `abstract` y `final` a la vez? Si **se marca** algo como `abstract`, **se tiene la intención** de que alguien más lo extienda o implemente. Pero si **se marca** algo como `final`, **se está previniendo** que cualquiera lo extienda o implemente. Estos conceptos están en conflicto directo entre sí.

Debido a esta incompatibilidad, Java no permite que una clase o método sea marcado como `abstract` y `final` al mismo tiempo. Por ejemplo, el siguiente fragmento de código no compilará:

```Java
public abstract final class Tortoise { // NO COMPILA
    public abstract final void walk(); // NO COMPILA
}
```

En este ejemplo, ni la declaración de la clase ni la del método compilarán porque están marcadas tanto `abstract` como `final`. El examen no tiende a usar modificadores `final` en clases o métodos a menudo, así que si **se ven**, **se debe asegurar** de que no se usen con el modificador `abstract`.

### Modificadores abstract y private

Un método no puede ser marcado como `abstract` y `private` a la vez. Esta regla tiene sentido si **se piensa** en ello. ¿Cómo **se definiría** una subclase que implemente un método requerido si el método no es heredado por la subclase? La respuesta es que no **se puede**, razón por la cual el compilador se quejará si **se intenta** hacer lo siguiente:

```Java
public abstract class Whale {
    private abstract void sing(); // NO COMPILA
}
public class HumpbackWhale extends Whale {
    private void sing() {
        System.out.println("Humpback whale is singing");
    } 
}
```

En este ejemplo, el método abstracto `sing()` definido en la clase padre `Whale` no es visible para la subclase `HumpbackWhale`. Aunque `HumpbackWhale` proporciona una implementación, no se considera una sobrescritura del método abstracto puesto que el método abstracto no es heredado. El compilador reconoce esto en la clase padre y reporta un error tan pronto como `private` y `abstract` se aplican al mismo método.

Si bien no es posible declarar un método como `abstract` y `private`, es posible (aunque redundante) declarar un método como `final` y `private`.

Si **se cambiara** el modificador de acceso de `private` a `protected` en la clase padre `Whale`, ¿compilaría el código?

```Java
public abstract class Whale {
    protected abstract void sing();
}
public class HumpbackWhale extends Whale {
    private void sing() { // NO COMPILA
        System.out.println("Humpback whale is singing");
    }
}
```

En este ejemplo modificado, el código seguirá sin compilar, pero por una razón completamente diferente. Si **se recuerdan** las reglas para sobrescribir un método, la subclase no puede reducir la visibilidad del método padre, `sing()`. Debido a que el método se declara `protected` en la clase padre, debe estar marcado como `protected` o `public` en la clase hija. Incluso con métodos abstractos, se deben seguir las reglas para la sobrescritura de métodos.

### Modificadores abstract y static

Como **se discutió** anteriormente en el capítulo, un método `static` solo puede ser ocultado, no sobrescrito. Se define como perteneciente a la clase, no a una instancia de la clase. Si un método `static` no puede ser sobrescrito, entonces se deduce que tampoco puede ser marcado como `abstract` puesto que nunca podrá ser implementado. Por ejemplo, la siguiente clase no compila:

```Java
abstract class Hippopotamus {
    abstract static void swim(); // NO COMPILA
}
```

Para el examen, **se debe asegurar** de saber qué modificadores pueden y no pueden usarse entre sí, especialmente para las clases abstractas y las interfaces.