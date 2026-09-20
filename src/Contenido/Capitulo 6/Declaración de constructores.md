Como **se aprendió** en el Capítulo 1, un constructor es un método especial que coincide con el nombre de la clase y no tiene un tipo de retorno. Es llamado cuando se crea una nueva instancia de la clase. Para el examen, **se necesitará** conocer una gran cantidad de reglas sobre constructores. En esta sección, **se muestra** cómo crear y llamar constructores.

### Creando un Constructor

**Se empezará** con un constructor sencillo:

```Java
public class Bunny {
    public Bunny() {
        System.out.print("hop");
    }
}
```

El nombre del constructor, `Bunny`, coincide con el nombre de la clase, `Bunny`, y no hay tipo de retorno, ni siquiera `void`. Eso lo convierte en un constructor. ¿**Se puede identificar** por qué estos dos no son constructores válidos para la clase `Bunny`?

```Java
public class Bunny {
    public bunny() {} // NO COMPILA
    public void Bunny() {}
}
```

El primero no coincide con el nombre de la clase porque Java distingue entre mayúsculas y minúsculas (_case-sensitive_). Dado que no coincide, Java sabe que no puede ser un constructor y asume que debe ser un método regular. Sin embargo, le falta el tipo de retorno y no compila. El segundo método es un método perfectamente válido, pero no es un constructor porque tiene un tipo de retorno.

Al igual que los parámetros de los métodos, los parámetros de los constructores pueden ser de cualquier clase, arreglo o tipo primitivo válido, incluyendo genéricos, pero no pueden incluir `var`. Por ejemplo, lo siguiente no compila:

```Java
public class Bonobo {
    public Bonobo(var food) { // NO COMPILA
    }
}
```

Una clase puede tener múltiples constructores, siempre y cuando cada constructor tenga una firma de constructor única. En este caso, eso significa que los parámetros del constructor deben ser distintos. Al igual que los métodos con el mismo nombre pero con diferentes firmas, declarar múltiples constructores con diferentes firmas se conoce como **sobrecarga de constructores** (_constructor overloading_). La siguiente clase `Turtle` tiene cuatro constructores sobrecargados distintos:

```Java
public class Turtle {
    private String name;
    public Turtle() {
        name = "John Doe";
    }
    public Turtle(int age) {}
    public Turtle(long age) {}
    public Turtle(String newName, String... favoriteFoods) {
        name = newName;
    }
}
```

Los constructores se utilizan al crear un nuevo objeto. Este proceso se llama **instanciación** porque crea una nueva instancia de la clase. Un constructor es llamado cuando **se escribe** `new` seguido del nombre de la clase que **se desea** instanciar. Aquí hay un ejemplo:

```Java
new Turtle(15)
```

Cuando Java lee la palabra clave `new`, asigna memoria para el nuevo objeto. Luego busca un constructor con una firma que coincida y lo llama.

### El Constructor por Defecto

Toda clase en Java tiene un constructor, ya sea que **se programe** uno o no. Si **no se incluyen** constructores en la clase, Java creará uno sin parámetros. Este constructor creado por Java se llama **constructor por defecto** (_default constructor_) y se agrega cada vez que se declara una clase sin ningún constructor. A menudo **se hace referencia** a él como el constructor por defecto sin argumentos, para mayor claridad. Aquí hay un ejemplo:

```Java
public class Rabbit {
    public static void main(String[] args) {
        new Rabbit(); // Llama al constructor por defecto
    }
}
```

En la clase `Rabbit`, Java observa que no se programó ningún constructor y crea uno. La clase anterior es equivalente a la siguiente, en la que se proporciona el constructor por defecto y, por lo tanto, no es insertado por el compilador:

```Java
public class Rabbit {
    public Rabbit() {}
    public static void main(String[] args) {
        new Rabbit(); // Llama al constructor definido por el usuario
    }
}
```

El constructor por defecto tiene una lista de parámetros vacía y un cuerpo vacío. Está bien que **se escriba** esto manualmente. Sin embargo, dado que no hace nada, Java se encarga de generarlo y ahorrar el tiempo de escritura.

**Se sigue mencionando** "generado". Esto ocurre durante el paso de compilación. Si **se examina** el archivo con la extensión `.java`, el constructor seguirá ausente. Solo hace su aparición en el archivo compilado con la extensión `.class`.

Para el examen, una de las reglas más importantes que **se debe conocer** es que el compilador solo inserta el constructor por defecto cuando **no hay** constructores definidos. ¿Cuál de estas clases **se cree** que tiene un constructor por defecto?

```Java
public class Rabbit1 {}
public class Rabbit2 {
    public Rabbit2() {}
}
public class Rabbit3 {
    public Rabbit3(boolean b) {}
}
public class Rabbit4 {
    private Rabbit4() {}
}
```

Solo `Rabbit1` obtiene un constructor por defecto sin argumentos. No tiene un constructor programado, por lo que Java genera un constructor por defecto sin argumentos. `Rabbit2` y `Rabbit3` ya tienen constructores `public`. `Rabbit4` tiene un constructor `private`. Puesto que estas tres clases tienen un constructor definido, el constructor por defecto sin argumentos no es insertado automáticamente.

Se observará cómo se llaman estos constructores:

```Java
1: public class RabbitsMultiply {
2:      public static void main(String[] args) {
3:          var r1 = new Rabbit1();
4:          var r2 = new Rabbit2();
5:          var r3 = new Rabbit3(true);
6:          var r4 = new Rabbit4(); // NO COMPILA
7:      } 
8: }
```

- **Línea 3** — Llama al constructor por defecto generado.
- **Líneas 4 y 5** — Llaman a los constructores proporcionados por el usuario.
- **Línea 6** — No compila. `Rabbit4` declaró su constructor como `private`, por lo que otras clases no pueden invocarlo.

> Tener **únicamente constructores privados** le indica al compilador que no proporcione un constructor por defecto sin argumentos, e impide que otras clases instancien la clase. Esto es útil cuando una clase solo tiene métodos `static` o cuando se desea control total sobre la creación de instancias.

### Llamando a Constructores Sobrecargados con this()

¿**Se comprenden** los conceptos básicos sobre la creación y referencia de constructores? Bien, porque las cosas están a punto de volverse un poco más complicadas. Puesto que una clase puede contener múltiples constructores sobrecargados, estos constructores en realidad pueden llamarse entre sí. **Se comenzará** con una clase simple que contiene dos constructores sobrecargados:

```Java
public class Hamster {
    private String color;
    private int weight;
    public Hamster(int weight, String color) { // Primer constructor
        this.weight = weight;
        this.color = color;
    }
    public Hamster(int weight) { // Segundo constructor
        this.weight = weight;
        color = "brown";
    }
}
```

Uno de los constructores toma un único parámetro `int`. El otro toma un `int` y un `String`. Estas listas de parámetros son diferentes, por lo que los constructores se sobrecargan exitosamente.

Hay un poco de duplicación, ya que `this.weight` se asigna de la misma manera en ambos constructores. En programación, incluso un poco de duplicación tiende a convertirse en mucha duplicación a medida que **se sigue añadiendo** "solo una cosa más". Por ejemplo, suponga que se tuvieran cinco variables configurándose como `this.weight`, en lugar de solo una. Lo que realmente **se desea** es que el primer constructor llame al segundo constructor con dos parámetros. Entonces, ¿cómo **se puede lograr** que un constructor llame a otro constructor? **Podría existir** la tentación de reescribir el primer constructor de la siguiente manera:

```Java
public Hamster(int weight) { // Segundo constructor
    Hamster(weight, "brown"); // NO COMPILA
}
```

Esto no funcionará. Los constructores solo pueden ser llamados escribiendo `new` antes del nombre del constructor. No son como los métodos normales que simplemente **se pueden llamar**. ¿Qué sucede si **se coloca** `new` antes del nombre del constructor?

```Java
public Hamster(int weight) { // Segundo constructor
    new Hamster(weight, "brown"); // Compila, pero crea un objeto extra
}
```

Este intento sí compila. Sin embargo, no hace lo que **se busca**. Cuando se llama a este constructor, crea un nuevo objeto con el peso y color predeterminados. Luego construye un objeto diferente con el peso y color deseados. De esta manera, **se termina** con dos objetos, uno de los cuales se descarta después de ser creado. Eso no es lo que **se desea**. **Se quiere** configurar el peso y color en el objeto que **se está intentando** instanciar en primer lugar.

Java proporciona una solución: `this()` (sí, la misma palabra clave que **se usó** para referirse a los miembros de instancia, pero con paréntesis). Cuando **se usa** `this()` con paréntesis, Java llama a otro constructor en la misma instancia de la clase.

```Java
public Hamster(int weight) { // Segundo constructor
    this(weight, "brown");
}
```

¡Éxito! Ahora Java llama al constructor que toma dos parámetros, con el peso y color configurados como se esperaba.

> **this vs. this()**
> A pesar de usar la misma palabra clave, `this` y `this()` son muy diferentes. El primero, `this`, se refiere a una instancia de la clase, mientras que el segundo, `this()`, se refiere a una llamada a un constructor dentro de la clase. El examen puede intentar confundir usando ambos juntos, así que **se debe asegurar** saber cuál usar y por qué.

**Regla especial:** Si se decide llamar a `this()`, **debe ser la primera sentencia del constructor**. Como consecuencia, solo puede haber **una llamada a `this()`** en cualquier constructor:

```Java
3: public Hamster(int weight) {
4:      System.out.println("chew");
5:      // Establece el peso y color por defecto
6:      this(weight, "brown"); // NO COMPILA
7: }
```

A pesar de que una instrucción `print` en la línea 4 no cambia ninguna variable, sigue siendo una instrucción Java y no se permite que se inserte antes de la llamada a `this()`. El comentario en la línea 5 está bien. Los comentarios no se consideran instrucciones y se permiten en cualquier lugar.

Hay una última regla para los constructores sobrecargados de la que **se debe estar** al tanto. Considere la siguiente definición de la clase `Gopher`:

```Java
public class Gopher {
    public Gopher(int dugHoles) {
        this(5); // NO COMPILA
    }
}
```

El compilador es capaz de detectar que este constructor se está llamando a sí mismo infinitamente. A esto se le suele llamar un ciclo y es similar a los bucles infinitos que **se discutieron** en el Capítulo 3, "Tomando Decisiones". Puesto que el código nunca puede terminar, el compilador se detiene y reporta esto como un error.

Del mismo modo, esto también no compila.

```Java
public class Gopher {
    public Gopher() {
        this(5); // NO COMPILA
    }
    public Gopher(int dugHoles) {
        this(); // NO COMPILA
    }
}
```

En este ejemplo, los constructores se llaman entre sí, y el proceso continúa infinitamente. Puesto que el compilador puede detectar esto, reporta un error.

Aquí **se resumen** las reglas que **se deben conocer** sobre los constructores que **se cubrieron** en esta sección. ¡**Se deben estudiar** bien!

- Una clase puede contener muchos constructores sobrecargados, siempre que la firma de cada uno sea distinta.
- El compilador inserta un constructor por defecto sin argumentos si no se declaran constructores.
- Si un constructor llama a `this()`, entonces debe ser la primera línea del constructor.    
- Java no permite llamadas cíclicas a constructores.

### Llamando a Constructores Padre con super()

¡Felicidades, **se está** en camino de convertirse en un experto en el uso de constructores! Sin embargo, hay un conjunto más de reglas que **se deben cubrir**, referentes a llamar constructores en la clase padre. Después de todo, ¿cómo se inicializan los miembros de instancia de la clase padre?

**La primera instrucción de todo constructor es una llamada a un constructor padre usando `super()` o a otro constructor en la clase usando `this()`.** **Se debe leer** la oración anterior dos veces para asegurarse de recordarla. ¡Es realmente importante!

Para simplificar en esta sección, a menudo **se hace referencia** a `super()` y `this()` para aludir a cualquier llamada a un constructor padre o sobrecargado, incluso aquellos que toman argumentos.

**Se echará** un vistazo a la clase `Animal` y su subclase `Zebra` y **se verá** cómo sus constructores pueden escribirse correctamente para llamarse entre sí:

```Java
public class Animal {
    private int age;
    public Animal(int age) {
        super(); // Se refiere al constructor en java.lang.Object
        this.age = age;
    }
}
public class Zebra extends Animal {
    public Zebra(int age) {
        super(age); // Se refiere al constructor en Animal
    }
    public Zebra() {
        this(4); // Se refiere al constructor en Zebra con un argumento int
    }
}
```

En la clase `Animal`, la primera instrucción del constructor es una llamada al constructor padre definido en `java.lang.Object`, el cual no toma argumentos. En la segunda clase, `Zebra`, la primera instrucción del primer constructor es una llamada al constructor de `Animal`, que toma un solo argumento. La clase `Zebra` también incluye un segundo constructor sin argumentos que no llama a `super()` sino que llama al otro constructor dentro de la clase `Zebra` utilizando `this(4)`.

> **super vs. super()**
> 
> Al igual que `this` y `this()`, `super` y `super()` no están relacionados en Java. El primero, `super`, se usa para referenciar miembros de la clase padre, mientras que el segundo, `super()`, llama a un constructor padre. Siempre que **se vea** la palabra clave `super` en el examen, **se debe asegurar** de que se esté utilizando correctamente.

Al igual que llamar a `this()`, llamar a `super()` solo se puede utilizar como la primera instrucción del constructor. Por ejemplo, las siguientes dos definiciones de clase no compilarán:

```Java
public class Zoo {
    public Zoo() {
        System.out.println("Zoo created");
        super(); // NO COMPILA
    }
}
public class Zoo {
    public Zoo() {
        super();
        System.out.println("Zoo created");
        super(); // NO COMPILA
    }
}
```

La primera clase no compilará porque la llamada al constructor padre debe ser la primera instrucción del constructor. En el segundo fragmento de código, `super()` es la primera instrucción del constructor, pero también se utiliza como la tercera instrucción. Puesto que `super()` solo puede ser llamado una vez como la primera instrucción del constructor, el código no compilará.

Si la clase padre tiene más de un constructor, la clase hija puede usar cualquier constructor padre válido y accesible en su definición, como se muestra en el siguiente ejemplo:

```Java
public class Animal {
    private int age;
    private String name;
    public Animal(int age, String name) {
        super();
        this.age = age;
        this.name = name;
    }
    public Animal(int age) {
        super();
        this.age = age;
        this.name = null;
    }
}
public class Gorilla extends Animal {
    public Gorilla(int age) {
        super(age, "Gorilla"); // Llama al primer constructor de Animal
    }
    public Gorilla() {
        super(5); // Llama al segundo constructor de Animal
    }
}
```

En este ejemplo, el primer constructor de la hija toma un argumento, `age`, y llama al constructor del padre, que toma dos argumentos, `age` y `name`. El segundo constructor de la hija no toma argumentos, y llama al constructor del padre, que toma un argumento, `age`. En este ejemplo, note que los constructores de la hija no están obligados a llamar a los constructores del padre que coincidan. Cualquier constructor del padre válido es aceptable siempre que sea accesible y se proporcionen los parámetros de entrada adecuados al constructor del padre.

### Entendiendo las Mejoras del Compilador

Un segundo: **se dijo** que la primera línea de todo constructor es una llamada a `this()` o `super()`, pero **se han estado** creando clases y constructores a lo largo de este proyecto, y rara vez **se ha hecho** alguna de las dos cosas. ¿Cómo compilaron estas clases?

La respuesta es que el compilador de Java inserta automáticamente una llamada al constructor sin argumentos `super()` si no **se llama** explícitamente a `this()` o `super()` en la primera línea de un constructor. Por ejemplo, las siguientes tres definiciones de clase y constructor son equivalentes, porque el compilador las convertirá todas automáticamente al último ejemplo:

```Java
public class Donkey {}

public class Donkey {
    public Donkey() {}
}

public class Donkey {
    public Donkey() {
        super();
    }
}
```

**Se debe asegurar** de comprender las diferencias entre estas tres definiciones de la clase `Donkey` y por qué Java las convertirá todas automáticamente a la última definición. Mientras **se lee** la siguiente sección, **se debe tener** en cuenta el proceso que realiza el compilador de Java.

### Consejos y Trucos sobre el Constructor por Defecto

Hasta ahora **se han presentado** muchas reglas, y **se podría** haber notado algo. suponga que **se tiene** una clase que no incluye un constructor sin argumentos. ¿Qué sucede si **se define** una subclase sin constructores, o una subclase con un constructor que no incluye una referencia a `super()`?

```Java
public class Mammal {
    public Mammal(int age) {}
}
public class Seal extends Mammal {} // NO COMPILA
public class Elephant extends Mammal {
    public Elephant() {} // NO COMPILA
}
```

La respuesta es que ninguna subclase compila. Puesto que `Mammal` define un constructor, el compilador no inserta un constructor sin argumentos. Sin embargo, el compilador insertará un constructor por defecto sin argumentos en `Seal`, pero será una implementación simple que solo llama a un constructor por defecto de un padre inexistente.

```Java
public class Seal extends Mammal {
    public Seal() {
        super(); // NO COMPILA
    }
}
```

Del mismo modo, `Elephant` no compilará por razones similares. El compilador no ve una llamada a `super()` o `this()` como la primera línea del constructor, por lo que inserta una llamada a un `super()` inexistente sin argumentos automáticamente.

```Java
public class Elephant extends Mammal {
    public Elephant() {
        super(); // NO COMPILA
    }
}
```

En estos casos, el compilador no ayudará, y **se debe crear** al menos un constructor en la clase hija que llame explícitamente a un constructor del padre a través del comando `super()`.

```Java
public class Seal extends Mammal {
    public Seal() {
        super(6); // Llamada explícita al constructor del padre
    }
}
public class Elephant extends Mammal {
    public Elephant() {
        super(4); // Llamada explícita al constructor del padre
    }
}
```

Las subclases pueden incluir constructores sin argumentos incluso si sus clases padres no lo hacen. Por ejemplo, lo siguiente compila porque `Elephant` incluye un constructor sin argumentos:

```Java
public class AfricanElephant extends Elephant {}
```

Es mucha información para asimilar, ¡**se sabe**! Para el examen, **se debe poder** identificar de inmediato por qué clases como las primeras implementaciones de `Seal` y `Elephant` no compilaron.


> **super() Siempre se Refiere al Padre Más Directo** 
> Una clase puede tener múltiples ancestros a través de la herencia. En el ejemplo anterior, `AfricanElephant` es una subclase de `Elephant`, que a su vez es una subclase de `Mammal`. Sin embargo, para los constructores, `super()` siempre se refiere al padre más directo. En este ejemplo, llamar a `super()` dentro de la clase `AfricanElephant` siempre se refiere a la clase `Elephant` y nunca a la clase `Mammal`.

Se concluye esta sección agregando dos reglas de constructores a las habilidades adquiridas.

- Si un constructor llama a `super()` o `this()`, entonces debe ser la primera línea del constructor.
- Si el constructor no contiene una referencia a `this()` o `super()`, entonces el compilador inserta automáticamente `super()` sin argumentos como la primera línea del constructor.

Felicidades, **se ha aprendido** todo lo que **se puede enseñar** sobre la declaración de constructores. A continuación, **se avanza** a la inicialización y **se discute** cómo usar los constructores.