En el Capítulo 1, **se cubrió** el orden de inicialización (_order of initialization_), aunque de una manera muy simplista. El **orden de inicialización** se refiere a cómo se asignan valores a los miembros de una clase. Se les pueden dar valores por defecto, como 0 para un `int`, o requerir valores explícitos, como para las variables `final`. En esta sección, **se profundiza** mucho más en cómo funciona el orden de inicialización y cómo detectar errores en el examen.

### Inicializando Clases

**Se comienza** la discusión sobre el orden de inicialización con la inicialización de clases. Primero, **se inicializa** la clase, lo cual implica invocar todos los miembros `static` en la jerarquía de la clase, comenzando con la superclase más alta y bajando. Esto a veces se conoce como **cargar la clase** (_loading the class_). La Máquina Virtual de Java (JVM) controla cuándo se inicializa la clase, aunque **se puede asumir** que la clase se carga antes de ser usada. La clase puede ser inicializada cuando el programa se inicia por primera vez, cuando se hace referencia a un miembro `static` de la clase, o poco antes de que se cree una instancia de la clase.

Una de las reglas más importantes con la inicialización de clases es que ocurre **como máximo una vez** para cada clase. La clase también podría no cargarse nunca si no se usa en el programa. **Se resume** el orden de inicialización para una clase de la siguiente manera:

**Inicializar la Clase X**

1. Inicializar la superclase de X.
2. Procesar todas las declaraciones de variables `static` en el orden en que aparecen en la clase.
3. Procesar todos los inicializadores `static` en el orden en que aparecen en la clase.

Tomando como base un ejemplo, ¿qué imprime el siguiente programa?

```Java
public class Animal {
    static { System.out.print("A"); }
}
public class Hippo extends Animal {
    public static void main(String[] grass) {
        System.out.print("C");
        new Hippo();
        new Hippo();
        new Hippo();
    }
    static { System.out.print("B"); }
}
```

Imprime `ABC` exactamente una vez. Puesto que el método `main()` está dentro de la clase `Hippo`, la clase se inicializará primero, comenzando con la superclase e imprimiendo `A` y `B`. Después, se ejecuta el método `main()`, imprimiendo `C`. Aunque el método `main()` crea tres instancias, la clase se carga solo una vez.

#### Por qué el Programa Hippo Imprimió C Después de AB

En el ejemplo anterior, la clase `Hippo` se inicializó antes de que se ejecutara el método `main()`. Esto ocurrió porque el método `main()` estaba dentro de la clase que se estaba ejecutando, por lo que tuvo que ser cargado al inicio. ¿Qué pasaría si en su lugar **se llamara** a `Hippo` dentro de otro programa?

En el ejemplo anterior, la clase `Hippo` se inicializó antes de que se ejecutara el método `main()`. Esto ocurrió porque el método `main()` estaba dentro de la clase que se estaba ejecutando, por lo que tuvo que ser cargado al inicio. ¿Qué pasaría si en su lugar **se llamara** a `Hippo` dentro de otro programa?

```Java
public class HippoFriend {
    public static void main(String[] grass) {
        System.out.print("C");
        new Hippo();
    }
}
```

Asumiendo que no se hace referencia a la clase en ningún otro lugar, este programa **probablemente** imprimirá `CAB`, y la clase `Hippo` no se cargará hasta que sea necesaria dentro del método `main()`. **Se dice** "probablemente" porque las reglas de cuándo se cargan las clases están determinadas por la JVM en tiempo de ejecución (_runtime_). Para el examen, solo **se necesita** saber que una clase debe ser inicializada antes de que sea referenciada o usada. Además, la clase que contiene el punto de entrada del programa, también conocido como el método `main()`, se carga antes de que se ejecute el método `main()`.

### Inicializando Campos final

Antes de profundizar en el orden de inicialización para los miembros de instancia, **se necesita** hablar de los campos `final` (variables de instancia) por un minuto. Cuando **se presentaron** las variables de instancia y de clase en el Capítulo 1, **se indicó** que se les asigna un valor por defecto basado en su tipo si no se especifica un valor. Por ejemplo, un `double` se inicializa con `0.0`, mientras que una referencia de objeto se inicializa a `null`. Sin embargo, un valor por defecto solo se aplica a un campo no `final`.

Como **se vio** en el Capítulo 5, a las variables `final static` se les debe asignar explícitamente un valor exactamente una vez. Los campos marcados como `final` siguen reglas similares. Se les pueden asignar valores en la línea en la que se declaran o en un inicializador de instancia (_instance initializer_).

```Java
public class MouseHouse {
    private final int volume;
    private final String name = "The Mouse House"; // Asignación en la declaración
    {
        volume = 10; // Asignación en el inicializador de instancia
    }
}
```

Sin embargo, a diferencia de los miembros de clase `static`, los campos de instancia `final` también pueden ser establecidos en un constructor. El constructor es parte del proceso de inicialización, por lo que se le permite asignar variables de instancia `final`. Para el examen, **se necesita** conocer una regla importante: para cuando el constructor se completa, a todas las variables de instancia `final` **se les debe haber asignado** un valor exactamente una vez.

**Se probará** esto en un ejemplo:

```Java
public class MouseHouse {
    private final int volume;
    private final String name;
    public MouseHouse() {
        this.name = "Empty House"; // Asignación en el constructor
    }
    {
        volume = 10; // Asignación en el inicializador de instancia
    }
}
```

A diferencia de las variables `final` locales, a las cuales no se les exige tener un valor a menos que realmente sean usadas, a las variables de instancia `final` se les debe asignar un valor. Si no se les asigna un valor cuando se declaran o en un inicializador de instancia, entonces se les debe asignar un valor en la declaración del constructor. No hacerlo resultará en un error de compilación.

```Java
public class MouseHouse {
    private final int volume;
    private final String type;
    {
        this.volume = 10;
    }
    public MouseHouse(String type) {
        this.type = type;
    }
    public MouseHouse() { // NO COMPILA
        this.volume = 2;  // NO COMPILA
    }
}
```

En este ejemplo, el primer constructor que toma un argumento `String` compila. En términos de asignar valores, cada constructor se revisa individualmente, que es por lo que el segundo constructor no compila. Primero, el constructor falla en establecer un valor para la variable `type`. El compilador detecta que nunca se establece un valor para `type` y reporta un error. Segundo, el constructor establece un valor para la variable `volume`, a pesar de que ya le fue asignado un valor por el inicializador de instancia.

> En el examen, **se debe tener precaución** con cualquier variable de instancia marcada como `final`. **Se debe asegurar** que se les asigne un valor en la línea donde se declaran, en un inicializador de instancia o en un constructor. Deben tener un valor asignado solo una vez, y la falta de asignación de un valor se considera un error de compilación.

¿Qué hay de las variables de instancia `final` cuando un constructor llama a otro constructor en la misma clase? En ese caso, **se debe seguir** el flujo cuidadosamente, asegurándose de que a cada variable de instancia `final` se le asigne un valor exactamente una vez. **Se puede reemplazar** el constructor erróneo anterior por el siguiente que sí compila:

```Java
public MouseHouse() {
    this(null);
}
```

Este constructor no realiza ninguna asignación a ninguna variable de instancia `final`, pero llama al constructor `MouseHouse(String)`, el cual **se observó** que compila sin problema. **Se utiliza** `null` aquí para demostrar que la variable no necesita ser un valor de objeto. **Se puede asignar** un valor `null` a variables de instancia `final` siempre y cuando se establezcan explícitamente.

### Inicializando Instancias

**Se ha cubierto** la inicialización de clases y los campos `final`, así que ahora es el momento de pasar al orden de inicialización para los objetos. **Se advierte** que esto puede ser un poco engorroso al principio, pero no es probable que el examen haga preguntas más complicadas que los ejemplos de esta sección. Sin embargo, **se promete** tomarlo con calma.

Primero, **se debe comenzar** en el constructor de nivel más bajo donde se usa la palabra clave `new`. **Se debe recordar** que la primera línea de cada constructor es una llamada a `this()` o `super()`, y si se omite, el compilador insertará automáticamente una llamada al constructor padre sin argumentos `super()`. Luego, **se debe avanzar** hacia arriba y notar el orden de los constructores. Finalmente, **se debe inicializar** cada clase comenzando con la superclase, procesando los inicializadores de instancia y los constructores dentro de cada clase, en el orden inverso en que cada clase fue instanciada. **Se resume** el orden de inicialización para una instancia de la siguiente manera:

**Inicializar Instancia de X**

1. Inicializar la Clase X si no ha sido inicializada previamente.
2. Inicializar la instancia de la superclase de X.
3. Procesar todas las declaraciones de variables de instancia en el orden en que aparecen en la clase.
4. Procesar todos los inicializadores de instancia en el orden en que aparecen en la clase.
5. Inicializar el constructor, incluyendo cualquier constructor sobrecargado referenciado con `this()`.


**Se probará** un ejemplo sin herencia. **Se comprobará** si **se puede averiguar** qué imprime la siguiente aplicación:

```Java
1: public class ZooTickets {
2:      private String name = "BestZoo";
3:      { System.out.print(name + "-"); }
4:      private static int COUNT = 0;
5:      static { System.out.print(COUNT + "-"); }
6:      static { COUNT += 10; System.out.print(COUNT + "-"); }
7: 
8:      public ZooTickets() {
9:          System.out.print("z-");
10:     }
11: 
12:     public static void main(String... patrons) {
13:         new ZooTickets();
14:     } 
15: }
```

La salida es la siguiente:

`0-10-BestZoo-z-`

Primero, **se tiene que inicializar** la clase. Puesto que no hay una superclase declarada, lo que significa que la superclase es `Object`, **se puede comenzar** con los componentes `static` de `ZooTickets`. En este caso, se ejecutan las líneas 4, 5 y 6, imprimiendo `0-` y `10-`. A continuación, **se inicializa** la instancia creada en la línea 13. Nuevamente, como no se declara una superclase, **se comienza** con los componentes de instancia. Se ejecutan las líneas 2 y 3, lo que imprime `BestZoo-`. Finalmente, **se ejecuta** el constructor en las líneas 8–10, lo que emite `z-`.

A continuación, **se probará** un ejemplo sencillo con herencia:

```Java
class Primate {
    public Primate() {
        System.out.print("Primate-");
    } 
}
class Ape extends Primate {
    public Ape(int fur) {
        System.out.print("Ape1-");
    }
    public Ape() {
        System.out.print("Ape2-");
    } 
}
public class Chimpanzee extends Ape {
    public Chimpanzee() {
        super(2);
        System.out.print("Chimpanzee-");
    }
    public static void main(String[] args) {
        new Chimpanzee();
    } 
}
```

El compilador inserta el comando `super()` como la primera instrucción de los constructores `Primate` y `Ape`. El código se ejecutará llamando primero a los constructores padre y producirá la siguiente salida:

`Primate-Ape1-Chimpanzee-`

Note que solo se llama a uno de los dos constructores de `Ape()`. **Se necesita comenzar** con la llamada a `new Chimpanzee()` para determinar qué constructores se ejecutarán. **Se debe recordar**, los constructores se ejecutan de abajo hacia arriba, pero dado que la primera línea de todo constructor es una llamada a otro constructor, el flujo termina con el constructor padre ejecutándose antes que el constructor hijo.

El siguiente ejemplo es un poco más difícil. ¿Qué **se cree** que sucede aquí?

```Java
1: public class Cuttlefish {
2:      private String name = "swimmy";
3:      { System.out.println(name); }
4:      private static int COUNT = 0;
5:      static { System.out.println(COUNT); }
6:      { COUNT++; System.out.println(COUNT); }
7: 
8:      public Cuttlefish() {
9:          System.out.println("Constructor");
10:     }
11: 
12:     public static void main(String[] args) {
13:         System.out.println("Ready");
14:         new Cuttlefish();
15:     } 
16: }
```

La salida se ve así:

```Plaintext
0
Ready
swimmy
1
Constructor
```

No se declara una superclase, por lo que **se pueden omitir** los pasos relacionados con la herencia. Primero **se procesan** las variables `static` y los inicializadores `static` (líneas 4 y 5, con la línea 5 imprimiendo `0`). Ahora que los inicializadores `static` están fuera del camino, el método `main()` se puede ejecutar, lo que imprime `Ready`. Luego **se crea** una instancia declarada en la línea 14. Las líneas 2, 3 y 6 se procesan, con la línea 3 imprimiendo `swimmy` y la línea 6 imprimiendo `1`. Finalmente, se ejecuta el constructor en las líneas 8–10, que imprime `Constructor`.

¿Listos para un ejemplo más difícil, del tipo que **se podría ver** en el examen? ¿Qué imprime lo siguiente?

```Java
1: class GiraffeFamily {
2:      static { System.out.print("A"); }
3:      { System.out.print("B"); }
4: 
5:      public GiraffeFamily(String name) {
6:          this(1);
7:          System.out.print("C");
8:      }
9: 
10:     public GiraffeFamily() {
11:         System.out.print("D");
12:     }
13: 
14:     public GiraffeFamily(int stripes) {
15:         System.out.print("E");
16:     }
17: }
18: public class Okapi extends GiraffeFamily {
19:     static { System.out.print("F"); }
20: 
21:     public Okapi(int stripes) {
22:         super("sugar");
23:         System.out.print("G");
24:     }
25:     { System.out.print("H"); }
26: 
27:     public static void main(String[] grass) {
28:         new Okapi(1);
29:         System.out.println();
30:         new Okapi(2);
31:     }
32: }
```

El programa imprime lo siguiente:

```Plaintext
AFBECHG
BECHG
```

**Se analizará** paso a paso. **Se comienza** inicializando la clase `Okapi`. Puesto que tiene una superclase `GiraffeFamily`, **se inicializa** esa primero, imprimiendo `A` en la línea 2. A continuación, **se inicializa** la clase `Okapi`, imprimiendo `F` en la línea 19.

Después de que las clases se inicializan, **se ejecuta** el método `main()` en la línea 27. La primera línea del método `main()` crea un nuevo objeto `Okapi`, desencadenando el proceso de inicialización de instancia. Según la segunda regla, la instancia de la superclase de `GiraffeFamily` se inicializa primero. Según la cuarta regla, se llama al inicializador de instancia en la superclase `GiraffeFamily`, y se imprime `B` en la línea 3. Según la quinta regla, **se inicializan** los constructores. En este caso, esto implica llamar al constructor en la línea 5, que a su vez llama al constructor sobrecargado en la línea 14. El resultado es que se imprime `EC`, ya que los cuerpos de los constructores se desenrollan en el orden inverso al que fueron llamados.

El proceso entonces continúa con la inicialización de la instancia `Okapi` en sí. Según las reglas cuarta y quinta, se imprime `H` en la línea 25, y `G` en la línea 23, respectivamente. El proceso es mucho más simple cuando no **se tiene** que llamar a ningún constructor sobrecargado. La línea 29 luego inserta un salto de línea en la salida. Finalmente, la línea 30 inicializa un nuevo objeto `Okapi`. El orden y la inicialización son los mismos que en la línea 28, sin la inicialización de la clase, por lo que se vuelve a imprimir `BECHG`. Note que la `D` nunca se imprime, ya que solo se llaman dos de los tres constructores en la superclase `GiraffeFamily`.

Este ejemplo es engañoso por varias razones. Hay múltiples constructores sobrecargados, muchos inicializadores y una ruta de constructores compleja a la que **se le debe hacer seguimiento**. Por suerte, preguntas como esta son poco comunes en el examen. Si **se ve** una, simplemente **se debe anotar** lo que está pasando a medida que **se lee** el código.

**Se concluye** esta sección listando reglas importantes que **se deben conocer** para el examen:

- Una clase es inicializada **como máximo una vez** por la JVM antes de ser referenciada o usada.
- A todas las variables `static final` se les debe asignar un valor exactamente una vez, ya sea cuando se declaran o en un inicializador `static`.
- A todos los campos `final` se les debe asignar un valor exactamente una vez, ya sea cuando se declaran, en un inicializador de instancia, o en un constructor.
- A las variables `static` y de instancia no `final` definidas sin un valor se les asigna un valor por defecto basado en su tipo.
- El orden de inicialización es el siguiente: declaraciones de variables, luego inicializadores, y finalmente constructores.