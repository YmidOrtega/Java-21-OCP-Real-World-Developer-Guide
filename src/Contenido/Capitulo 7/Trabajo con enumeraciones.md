En programación, es común tener un tipo que solo puede tener un conjunto finito de valores, como los días de la semana, las estaciones del año, los colores primarios, etc. Una enumeración, o `enum` para abreviar, es como un conjunto fijo de constantes. Usar un `enum` es mucho mejor que usar un grupo de constantes porque proporciona comprobación de tipos segura (_type-safe checking_). Con constantes numéricas o de tipo `String`, **se podría pasar** un valor inválido y no darse cuenta hasta el tiempo de ejecución. Con los `enums`, es imposible crear un valor `enum` inválido sin introducir un error de compilación.

Las enumeraciones aparecen siempre que **se tiene** un conjunto de elementos cuyos tipos se conocen en tiempo de compilación. Ejemplos comunes incluyen las direcciones de la brújula, los meses del año, los planetas del sistema solar y las cartas de una baraja (bueno, tal vez no los planetas del sistema solar, dado que a Plutón se le revocó su estatus planetario).

### Creando Enums Simples

Para crear un `enum`, **se declara** un tipo con la palabra clave `enum`, un nombre y una lista de valores, como se muestra en la imagen.

**Se hace referencia** a un `enum` que solo contiene una lista de valores como un `enum` simple. Al trabajar con `enums` simples, el punto y coma (`;`) al final de la lista es opcional. **Se debe mantener** a mano el `enum` `Season`, ya que **se usa** a lo largo de esta sección.

![[Definición de una enumeración sencilla.png]]

> Los valores de un `enum` se consideran constantes y comúnmente se escriben usando `MAYÚSCULAS_SEPARADAS_POR_GUIONES_BAJOS` (_UPPER_SNAKE_CASE_). Por ejemplo, un `enum` que declara una lista de sabores de helado podría incluir valores como `VANILLA`, `ROCKY_ROAD`, `MINT_CHOCOLATE_CHIP`, y así sucesivamente.

Usar un `enum` es súper fácil.

```Java
var s = Season.SUMMER;
System.out.println(Season.SUMMER);      // SUMMER
System.out.println(s == Season.SUMMER); // true
```

Como **se puede observar**, los `enums` imprimen el nombre del `enum` cuando se llama a `toString()`. Pueden ser comparados usando `==` porque son como constantes `static final`. En otras palabras, **se puede usar** `equals()` o `==` para comparar `enums`, puesto que cada valor `enum` se inicializa solo una vez en la Máquina Virtual de Java (JVM).

Una cosa que no **se puede hacer** es extender un `enum`.

```Java
public enum ExtendedSeason extends Season {} // NO COMPILA
```

Los valores en un `enum` son fijos. No **se pueden agregar** más extendiendo el `enum` ni **se puede marcar** un `enum` como `final`. Por otro lado, un `enum` puede implementar una interfaz, lo cual **se cubrirá** en breve.

### Llamando a Métodos Comunes de Enum

Un `enum` proporciona un método `values()` para obtener un arreglo de todos los valores. **Se puede usar** esto como cualquier arreglo normal, incluso en un bucle _for-each_. Además, cada valor `enum` incluye dos métodos, `name()` y `ordinal()`. Lo siguiente muestra los tres métodos:

```Java
for(var season: Season.values()) {
    System.out.println(season.name() + " " + season.ordinal());
}
```

El método `ordinal()` devuelve un valor `int`, el cual denota el orden en el que se declara el valor en el `enum`:

```Plaintext
WINTER 0
SPRING 1
SUMMER 2
FALL 3
```

En general, el código es más fácil de leer si **se ciñe** al valor `enum` legible por humanos, en lugar del valor `ordinal()`. Además, de todos modos **no se puede comparar** directamente un `int` y un valor `enum`, ya que este último es un objeto.

```Java
if (Season.SUMMER == 2) {} // NO COMPILA
```

Un `enum` proporciona un método `valueOf()` muy util para convertir un `String` a un valor `enum`. Esto es útil cuando **se trabaja** con código más antiguo o se analiza la entrada del usuario. Sin embargo, el `String` que se pasa debe coincidir exactamente con el valor `enum`.

```Java
Season s = Season.valueOf("SUMMER"); // SUMMER
Season t = Season.valueOf("summer"); // IllegalArgumentException
```

La primera instrucción funciona y asigna el valor `enum` adecuado a `s`. Note que esta línea no está creando un valor `enum`, al menos no directamente. Cada valor `enum` se crea una vez cuando el `enum` se carga por primera vez. Una vez que el `enum` ha sido cargado, recupera el único valor `enum` con el nombre coincidente. La segunda instrucción encuentra un problema. No hay ningún valor `enum` con el nombre en minúsculas `summer`. Java se da por vencido y lanza una `IllegalArgumentException`.

### Usando Enums en Declaraciones switch

Como **se vio** en el Capítulo 3, "Toma de decisiones", los `enums` se pueden usar en sentencias y expresiones `switch`. Los `enums` tienen la propiedad única de que no requieren una rama `default` para un `switch` exhaustivo si se manejan todos los valores `enum`.

```Java
String getWeather(Season value) {
    return switch (value) {
        case SUMMER -> "Too hot";
        case Season.WINTER -> "Too cold";
        case SPRING, FALL -> "Just right";
    };
}
```

También se puede agregar una rama `default`, pero no es obligatoria, siempre y cuando se manejen todos los valores. También note que dentro de cada cláusula `case`, el nombre del `enum`, `Season`, ahora es opcional. En versiones anteriores de Java, el nombre del `enum` no estaba permitido.

Aunque cada valor `enum` tiene un valor ordinal asociado, no se puede usar directamente dentro de una cláusula `case`. Por ejemplo, esto no compila:

```Java
String getWeather(Season value) {
    return switch (value) {
        case SUMMER -> "Too hot";
        case 0      -> "Too cold"; // NO COMPILA
        default     -> "Just right";
    };
}
```

### Trabajando con Enums Complejos

Mientras que un `enum` simple está compuesto por solo una lista de valores, **se puede definir** un `enum` complejo con elementos adicionales. **Se asume** que un zoológico quiere realizar un seguimiento de los patrones de tráfico para determinar qué estaciones reciben la mayoría de los visitantes.

```Java
21: interface Visitors { void printVisitors(); }
22: enum SeasonWithVisitors implements Visitors {
23:     WINTER("Low"), SPRING("Medium"), SUMMER("High"), FALL("Medium");
24: 
25:     private final String visitors;
26:     public static final String DESCRIPTION = "Weather enum";
27: 
28:     private SeasonWithVisitors(String visitors) {
29:         System.out.print("constructing,");
30:         this.visitors = visitors;
31:     }
32: 
33:     @Override public void printVisitors() {
34:         System.out.println(visitors);
35:     } 
36: }
```

Hay algunas cosas a notar aquí. En la línea 23, la lista de valores `enum` termina con un punto y coma (`;`). Si bien esto es opcional para un `enum` simple, es requerido si hay algo en el `enum` además de los valores. Las líneas 25–35 son código Java regular. **Se tienen** variables de instancia y estáticas (líneas 25–26), un constructor (líneas 28–31) y un método (líneas 33–35).

Podría haberse notado que en el ejemplo del `enum`, la lista de valores va primero. Esto no fue un accidente. Para `enums` complejos (y `enums` trivialmente simples), la lista de valores **siempre** va primero.

#### Creando Variables de Enum

Una declaración de `enum` puede incluir tanto variables `static` como de instancia. En la implementación de `SeasonWithVisitors` (líneas 25–26), **se marcan** las variables como `final`, de modo que las propiedades del `enum` no puedan ser modificadas.

> Aunque es posible crear un `enum` con variables de instancia que puedan ser modificadas, es una muy mala práctica hacerlo puesto que se comparten dentro de la JVM. Al diseñar valores `enum`, estos deben ser inmutables.

#### Declarando Constructores de Enum

Todos los constructores de `enum` son implícitamente `private`, siendo el modificador opcional. Esto es razonable puesto que no **se puede extender** un `enum` y los constructores solo pueden ser llamados dentro del propio `enum`. De hecho, un constructor de `enum` no compilará si contiene un modificador `public` o `protected`.

```Java
28:     public SeasonWithVisitors(String visitors) { // NO COMPILA
```

¿Qué hay de todos los paréntesis en la línea 23 del `enum` `SeasonWithVisitors`? Esas son llamadas al constructor, pero sin la palabra clave `new` que se usa normalmente para los objetos. La primera vez que **se solicita** cualquiera de los valores `enum`, Java construye todos los valores `enum`. Después de eso, Java simplemente devuelve los valores `enum` ya construidos.

Dada esta explicación, **se puede observar** por qué este fragmento de código llama a cada constructor solo una vez:

```Java
System.out.print("begin,");
var firstCall = SeasonWithVisitors.SUMMER; // Imprime 4 veces
System.out.print("middle,");
var secondCall = SeasonWithVisitors.SUMMER; // No imprime nada
System.out.print("end");
```

Este programa imprime lo siguiente:

```Plaintext
begin,constructing,constructing,constructing,constructing,middle,end
```

Si el `enum` `SeasonWithVisitors` se usó antes en el programa (y, por lo tanto, se inicializó antes), entonces la línea que declara la variable `firstCall` no imprimiría nada.

#### Escribiendo Métodos de Enum

Al igual que una clase, un `enum` puede contener métodos `static` y de instancia. Un `enum` puede incluso implementar una interfaz como **se vio** en las líneas 21–22 del `enum` `SeasonWithVisitors`. **Se incluye** la anotación `@Override` en la línea 33 para dejar claro que es un método heredado.

¿Cómo **se llama** a un método de instancia de `enum`? Eso también es fácil: simplemente **se usa** el valor `enum` seguido por la llamada al método.

```Java
SeasonWithVisitors.SUMMER.printVisitors();
```

A veces **se desea** definir diferentes métodos para cada `enum`. Por ejemplo, el zoológico tiene diferentes horarios de temporada. Hace frío y oscurece temprano en invierno. **Se podría** hacer un seguimiento de los horarios a través de variables de instancia, o **se puede permitir** que cada valor `enum` gestione los horarios por sí mismo.

```Java
public enum SeasonWithTimes {
    WINTER {
        public String getHours() { return "10am-3pm"; }
    },
    SPRING {
        public String getHours() { return "9am-5pm"; }
    },
    SUMMER {
        public String getHours() { return "9am-7pm"; }
    },
    FALL {
        public String getHours() { return "9am-5pm"; }
    };
    public abstract String getHours();
}
```

¿Qué está pasando aquí? Parece que **se creó** una clase abstracta y un grupo de diminutas subclases. En cierto modo, así es. El `enum` en sí tiene un método abstracto. Esto significa que absolutamente todos y cada uno de los valores `enum` están obligados a implementar este método. Si **se olvida** implementar el método para uno de los valores, **se obtiene** un error de compilador:

```Plaintext
The enum constant WINTER must implement the abstract method getHours()
```

Pero, ¿qué sucede si no **se desea** que absolutamente todos y cada uno de los valores `enum` tengan un método? No hay problema. **Se puede crear** una implementación para todos los valores y sobrescribirla solo para los casos especiales.

```Java
public enum SeasonWithTimes {
    WINTER {
        public String getHours() { return "10am-3pm"; }
    },
    SUMMER {
        public String getHours() { return "9am-7pm"; }
    },
    SPRING, FALL;
    public String getHours() { return "9am-5pm"; }
}
```

Esto se ve mejor. Solo **se codifican** los casos especiales y **se permite** que los demás usen la implementación proporcionada por el `enum`.

> El hecho de que un `enum` pueda tener muchos métodos no significa que deba tenerlos. **Se debe intentar** mantener los `enums` simples. Si un `enum` ocupa más de una pantalla o dos, probablemente sea demasiado largo. Cuando los `enums` se vuelven demasiado largos o demasiado complejos, son difíciles de leer.