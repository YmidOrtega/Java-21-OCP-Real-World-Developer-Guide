Los `records` son herramientas increíblemente útiles para crear clases orientadas a datos que eliminan un montón de código repetitivo (_boilerplate_). Antes de adentrarse en los `records`, ayuda tener algo de contexto sobre por qué fueron añadidos al lenguaje, así que **se empieza** con el encapsulamiento.

#### Comprendiendo el Encapsulamiento

Un **POJO** (por sus siglas en inglés, Plain Old Java Object) es una clase utilizada para modelar y pasar datos de un lado a otro, a menudo con pocos o ningún método complejo (de ahí la parte "plain" [simple] de la definición). También se podría haber oído hablar de un **JavaBean**, el cual es un POJO que tiene algunas reglas adicionales aplicadas.

**Se creará** un POJO simple con dos campos:

```Java
public class Crane {
    int numberEggs;
    String name;
    public Crane(int numberEggs, String name) {
        this.numberEggs = numberEggs;
        this.name = name;
    }
}
```

Uh oh, los campos tienen acceso de paquete. ¿Por qué esto es importante? Eso significa que alguien fuera de la clase en el mismo paquete podría cambiar estos valores y crear datos inválidos como este:

```Java
public class Poacher {
    public void badActor() {
        var mother = new Crane(5, "Cathy");
        mother.numberEggs = -100;
    }
}
```

Evidentemente esto no es bueno. ¡No **se desea** que la madre `Crane` tenga un número negativo de huevos! El encapsulamiento al rescate. El encapsulamiento es una forma de proteger a los miembros de la clase restringiendo el acceso a ellos. En Java, se implementa comúnmente declarando todas las variables de instancia `private`. A los llamadores se les requiere usar métodos para recuperar o modificar las variables de instancia.

El encapsulamiento se trata de proteger una clase del uso inesperado. También permite modificar los métodos y el comportamiento de la clase más adelante sin que alguien ya tenga acceso directo a una variable de instancia dentro de la clase. Por ejemplo, **se puede cambiar** el tipo de datos de una variable de instancia pero mantener las mismas firmas de métodos. De esta manera, **se mantiene** el control total sobre el funcionamiento interno de una clase.

**Se echará** un vistazo a la clase `Crane` recién encapsulada (e inmutable):

```Java
1:  public final class Crane {
2:      private final int numberEggs;
3:      private final String name;
4:      public Crane(int numberEggs, String name) {
5:          if (numberEggs >= 0) this.numberEggs = numberEggs; // salvaguarda
6:          else throw new IllegalArgumentException();
7:          this.name = name;
8:      }
9:      public int getNumberEggs() { // getter
10:         return numberEggs;
11:     }
12:     public String getName() { // getter
13:         return name;
14:     }
15: }
```

Note que las variables de instancia ahora son `private` en las líneas 2 y 3. Esto significa que solo el código dentro de la clase puede leer o escribir sus valores. Puesto que **se escribió** la clase, **se sabe** que es mejor no establecer un número negativo de huevos. **Se añadió** un método en las líneas 9–11 para leer el valor, el cual es llamado un método de acceso o **getter**.

Se podría haber notado que **se marcaron** la clase y sus variables de instancia como `final`, y no **se tienen** métodos mutadores, o **setters**, para modificar el valor de las variables de instancia. Eso es porque **se desea** que la clase sea inmutable además de estar bien encapsulada. Como **se vio** en el Capítulo 6, el patrón de objetos inmutables es un patrón de diseño orientado a objetos en el que un objeto no puede ser modificado después de ser creado. En lugar de modificar un objeto inmutable, **se crea** un nuevo objeto que contenga las propiedades del objeto original que **se desean** copiar.

Para repasar, **se debe recordar** que los datos son `private` y los getters/setters son `public`. Ni siquiera **se tiene** que proporcionar getters y setters. Siempre que las variables de instancia sean `private`, todo está bien. Por ejemplo, la siguiente clase está bien encapsulada, aunque no es terriblemente útil ya que no declara ningún método no `private`:

```Java
public class Vet {
    private String name = "Dr Rogers";
    private int yearsExperience = 25;
}
```

**Se deben omitir** los setters para que una clase sea inmutable. Repase el Capítulo 6 para las reglas adicionales sobre la creación de objetos inmutables.

### Aplicando records

La clase `Crane` tenía 15 líneas de largo. **Se puede escribir** eso mucho más concisamente, como se muestra en imagen a continuación. Dejando de lado por un momento la cláusula de salvaguarda en `numberEggs` dentro del constructor, ¡este `record` es equivalente e inmutable!

![[Definición de un record.png]]

¡Guau! ¡Tiene solo una línea de largo! Un `record` es un tipo especial de clase orientada a datos en la que el compilador inserta código repetitivo (_boilerplate_) por uno.

De hecho, el compilador inserta mucho más que las 14 líneas que **se escribieron** antes. Como ventaja, el compilador inserta implementaciones útiles de los métodos de `Object`: `equals()`, `hashCode()` y `toString()`. ¡**Se ha cubierto** mucho en una sola línea de código!

Ahora **se debe imaginar** que **se tuvieran** 10 campos de datos en lugar de 2. Eso es un montón de métodos que **se evita** escribir. ¡Y ni siquiera **se ha hablado** de los constructores! Peor aún, cada vez que alguien cambia un campo, decenas de líneas de código relacionado podrían necesitar ser actualizadas. Por ejemplo, `name` podría usarse en el constructor, en el método `toString()`, en `equals()`, etc. Si **se tiene** una aplicación con cientos de POJOs, un `record` puede ahorrar un tiempo valioso.

Crear una instancia de un `Crane` e imprimir algunos campos es fácil:

```Java
var mommy = new Crane(4, "Cammy");
System.out.println(mommy.numberEggs()); // 4
System.out.println(mommy.name());       // Cammy
```

Algunas cosas deberían destacar aquí. Primero, nunca **se definieron** constructores ni métodos en la declaración de `Crane`. ¿Cómo sabe el compilador qué hacer? Detrás de escena, crea un constructor con los parámetros en el mismo orden en que aparecen en la declaración del `record`. Omitir o cambiar el orden de los tipos provocará errores del compilador:

```Java
var mommy1 = new Crane("Cammy", 4); // NO COMPILA
var mommy2 = new Crane("Cammy");    // NO COMPILA
```

Para cada campo, también crea un método de acceso (_accessor_) con el mismo nombre del campo, más un par de paréntesis. A diferencia de los POJOs o JavaBeans tradicionales, los métodos no tienen el prefijo `get` o `is`. ¡Solo unos cuantos caracteres más que los `records` ahorran escribir! Finalmente, los `records` sobrescriben varios métodos de `Object` por uno.

**Miembros Agregados Automáticamente a los records**

- **Constructor:** Un constructor con los parámetros en el mismo orden que la declaración del `record`.
- **Método de acceso:** Un método de acceso para cada campo.
- **`equals()`:** Un método para comparar dos elementos que devuelve `true` si cada campo es igual en términos de `equals()`.
- **`hashCode()`:** Un método `hashCode()` consistente que utiliza todos los campos.
- **`toString()`:** Una implementación de `toString()` que imprime cada campo del `record` en un formato conveniente y fácil de leer.  

Lo siguiente muestra ejemplos de los nuevos métodos. **Se debe recordar** que el método `println()` llamará automáticamente al método `toString()` sobre cualquier objeto que se le pase.

```Java
var father = new Crane(0, "Craig");
System.out.println(father); // Crane[numberEggs=0, name=Craig]

var copy = new Crane(0, "Craig");
System.out.println(copy); // Crane[numberEggs=0, name=Craig]

System.out.println(father.equals(copy)); // true
System.out.println(father.hashCode() + ", " + copy.hashCode()); // 1007, 1007
```

Esos son los conceptos básicos de los `records`. **Se dice** "básicos" porque hay mucho más que **se puede hacer** con ellos, como **se verá** en las siguientes secciones.

> Dada la declaración de una línea de `Crane`, **se puede imaginar** cuánto código y trabajo se requeriría para escribir una clase equivalente. ¡Fácilmente podría tomar más de 40 líneas! Podría ser un ejercicio divertido intentar escribir todos los métodos que proveen los `records`.

Dato curioso: es legal tener un `record` sin ningún campo. Simplemente se declara con la palabra clave `record` y paréntesis:

```Java
public record Crane() {}
```

Este no es el tipo de cosas que **se usarían** en el propio código, pero podría aparecer en el examen.

#### Declarando Constructores

¿Qué pasa si **se necesita** declarar un `record` con algunas salvaguardas como **se hizo** antes? En esta sección, **se cubren** dos formas en que **se puede lograr** esto con `records`.

**El Constructor Largo**

Primero, simplemente **se puede declarar** el constructor que el compilador normalmente inserta automáticamente, al cual **se le llama** el constructor largo (_long constructor_).

```Java
public record Crane(int numberEggs, String name) {
    public Crane(int numberEggs, String name) {
        if (numberEggs < 0) throw new IllegalArgumentException();
        this.numberEggs = numberEggs;
        this.name = name;
    }
}
```

El compilador no insertará un constructor si **se define** uno con la misma lista de parámetros en el mismo orden. Puesto que cada campo es `final`, el constructor debe establecer todos los campos. Por ejemplo, este `record` no compila:

```Java
public record Crane(int numberEggs, String name) {
    public Crane(int numberEggs, String name) {} // NO COMPILA
}
```

Si bien poder declarar un constructor es una característica agradable de los `records`, también es problemático. Si **se tienen** 20 campos, **se necesitará** declarar asignaciones para cada uno, introduciendo el código repetitivo (_boilerplate_) que **se buscaba** eliminar. ¡Oh, qué molestia!

**Constructores Compactos**

Afortunadamente, los autores de Java agregaron la capacidad de definir un **constructor compacto** (_compact constructor_) para los `records`. Un constructor compacto es un tipo especial de constructor usado para los `records` para procesar la validación y las transformaciones de manera concisa. No toma parámetros y establece implícitamente todos los campos. La imagen muestra un ejemplo de un constructor compacto.

![[Declaración de un constructor compacto.png]]

¡Genial! Ahora **se pueden verificar** los valores que **se deseen**, y no **se tienen** que listar todos los parámetros del constructor y las asignaciones triviales. Java ejecutará el constructor completo después del constructor compacto. También **se debe recordar** que un constructor compacto se declara sin paréntesis, ya que el examen podría intentar engañar con esto. Como se muestra en la imagen anterior, incluso **se pueden transformar** los parámetros del constructor, como **se discutirá** más a fondo en la siguiente sección.

**Transformando Parámetros**

Los constructores compactos dan la oportunidad de aplicar transformaciones a cualquiera de los valores de entrada. **Se observará** si **se puede descubrir** lo que hace el siguiente constructor compacto:

```Java
public record Crane(int numberEggs, String name) {
    public Crane {
        if (name == null || name.length() < 1)
            throw new IllegalArgumentException();
        name = name.substring(0, 1).toUpperCase()
             + name.substring(1).toLowerCase();
    }
}
```

¿Rendirse? Valida la cadena de texto, luego le da un formato tal que solo la primera letra esté en mayúscula. Como antes, Java llama al constructor completo después del constructor compacto pero con los parámetros del constructor modificados.

Aunque los constructores compactos pueden modificar los parámetros del constructor, no pueden modificar los campos del `record`. Por ejemplo, esto no compila:

```Java
public record Crane(int numberEggs, String name) {
    public Crane {
        this.numberEggs = 10; // NO COMPILA
    }
}
```

Eliminar la referencia `this` permite que el código compile, ya que en su lugar se modifica el parámetro del constructor.

> Aunque en esta sección **se cubrieron** tanto las formas largas como las compactas de los constructores de `records`, se recomienda encarecidamente **ceñirse** a la forma compacta a menos que **se tenga** una buena razón para no hacerlo.

**Constructores Sobrecargados**

También **se pueden crear** constructores sobrecargados (_overloaded constructors_) que tomen una lista de parámetros completamente diferente. Están más estrechamente relacionados con el constructor de forma larga y no utilizan ninguna de las características sintácticas de los constructores compactos.

```Java
public record Crane(int numberEggs, String name) {
    public Crane(String firstName, String lastName) {
        this(0, firstName + " " + lastName);
    }
}
```

La primera línea de un constructor sobrecargado debe ser una llamada explícita a otro constructor a través de `this()`. Si no hay otros constructores, debe llamarse al constructor largo. **Se debe contrastar** esto con lo que **se aprendió** en el Capítulo 6, donde llamar a `super()` o `this()` a menudo era opcional en las declaraciones de constructores. Además, a diferencia de los constructores compactos, solo **se pueden transformar** los datos en la primera línea. Después de la primera línea, todos los campos ya estarán asignados y el objeto será inmutable.

```Java
public record Crane(int numberEggs, String name) {
    public Crane(int numberEggs, String firstName, String lastName) {
        this(numberEggs + 1, firstName + " " + lastName);
        numberEggs = 10; // SIN EFECTO (se aplica al parámetro, no al campo de instancia)
        this.numberEggs = 20; // NO COMPILA
    }
}
```

Solo el constructor largo, con campos que coincidan con la declaración del `record`, soporta establecer valores de campo con una referencia `this`. Los constructores compactos y sobrecargados no lo hacen.

Como **se vio** en el Capítulo 6, tampoco **se pueden declarar** dos constructores de `record` que se llamen entre sí infinitamente o como un ciclo.

```Java
public record Crane(int numberEggs, String name) {
    public Crane(String name) {
        this(1); // NO COMPILA
    }
    public Crane(int numberEggs) {
        this(""); // NO COMPILA
    }
}
```

#### Comprendiendo la Inmutabilidad de los records

Como **se observó**, los `records` no tienen setters. Cada campo es inherentemente `final` y no puede ser modificado después de haber sido escrito en el constructor. Para "modificar" un `record`, **se tiene** que hacer un nuevo objeto y copiar todos los datos que **se deseen** preservar.

```Java
var cousin = new Crane(3, "Jenny");
var friend = new Crane(cousin.numberEggs(), "Janeice");
```

Así como las interfaces son implícitamente `abstract`, los `records` también son implícitamente `final`. El modificador `final` es opcional pero se asume.

```Java
public final record Crane(int numberEggs, String name) {}
```

Al igual que los `enums`, eso significa que no **se puede extender** ni heredar un `record`.

```Java
public record BlueCrane() extends Crane {} // NO COMPILA
```

También como los `enums`, un `record` puede implementar una interfaz regular o sellada, siempre y cuando implemente todos los métodos abstractos.

```Java
public interface Bird {}
public record Crane(int numberEggs, String name) implements Bird {}
```

Aunque los miembros de instancia de un `record` son `final`, no se exige que los miembros `static` lo sean. Por ejemplo, lo siguiente define un `record` inmutable en el cual se actualiza un valor `static` cada vez que se crea un `record`.

```Java
public record WhoopingCrane(String name, int position) {
    private static int counter = 0;
    public WhoopingCrane(String name) {
        this(name, counter++);
    }
}
```

> Aunque va mucho más allá del alcance de este libro, existen algunas buenas razones para hacer que las clases orientadas a datos sean inmutables. Hacerlo puede conducir a un código menos propenso a errores, ya que se establece un nuevo objeto cada vez que se modifican los datos. También los hace inherentemente seguros para hilos (_thread-safe_) y utilizables en _frameworks_ concurrentes.

#### Usando Coincidencia de Patrones (Pattern Matching) con records

Como novedad en Java 21, los `records` han sido actualizados para soportar la coincidencia de patrones. Inicialmente, se podría pensar que esto en realidad no es algo nuevo. Después de todo, **se podían usar** los `records` con coincidencia de patrones en Java 17. La nueva característica es realmente sobre los miembros del `record`, en lugar del `record` en sí. **Se probará** un ejemplo:

```Java
1:  record Monkey(String name, int age) {}
2: 
3:  public class Zoo {
4:      public static void main(String[] args) {
5: 
6:          Object animal = new Monkey("George", 3);
7: 
8:          if(animal instanceof Monkey(String name, int myAge)) {
9:              System.out.println("Hello " + name);
10:             System.out.println("Your age is " + myAge);
11:         } 
12:     } 
13: }
```

Un momento, ¿qué está pasando en la línea 8? Parece que **se redeclaró** la declaración del `record`. ¡Descuide, no **se hizo**! Lo que sí **se hizo**, sin embargo, es definir un patrón que sea compatible con el `record` `Monkey`. También **se nombraron** dos elementos, `name` y `myAge`. Al igual que la coincidencia de patrones que **se vio** en el Capítulo 3, esto **permite** usarlos como variables locales en las líneas 9 y 10, sin una variable de referencia.

Para el examen, **se deben tener** en cuenta las siguientes reglas al trabajar con coincidencia de patrones y `records`:

- Si se incluye cualquier campo declarado en el `record`, entonces deben incluirse todos los campos.
- El orden de los campos debe ser el mismo que en el `record`.
- Los nombres de los campos no tienen que coincidir.
- En tiempo de compilación, el tipo del campo debe ser compatible con el tipo declarado en el `record`.
- El patrón puede no coincidir en tiempo de ejecución si el `record` soporta elementos de varios tipos.
 
Trabajar con `records` y coincidencia de patrones tiene algunas similitudes con el moldeo (_casting_). Por ejemplo, el compilador no permitirá cosas que sepa que son inválidas. Sin embargo, hay algunas diferencias que **se verán** en breve.

¡Hora del cuestionario! Dado el `record` `Monkey` anterior, ¿cuáles de las siguientes líneas de código no compilan?

```Java
11: if(animal instanceof Monkey myMonkey) {}
12: if(animal instanceof Monkey(String n, int a) myMonkey) {}
13: if(animal instanceof Monkey(String n, long a)) {}
14: if(animal instanceof Monkey(Object n, int a)) {}
```

El primer ejemplo compila, ya que esto es solo la coincidencia de patrones simple que **se vio** en el Capítulo 3. La línea 12 no compila, sin embargo. **Se puede nombrar** el `record` o sus campos, pero no ambos. La línea 13 tampoco compila, ya que no se soporta la promoción numérica. La última línea sí compila, puesto que `String` es compatible con `Object`.

**Coincidencia de records**

Las dos últimas reglas para la coincidencia de `records` ameritan un poco más de discusión. La coincidencia de patrones para `records` incluye coincidir tanto el tipo del `record` como el tipo de cada campo. Dadas las cinco instrucciones de coincidencia de patrones, ¿qué imprime el siguiente código?

```Java
1:  record Fish(Object type) {}
2:  public class Veterinarian {
3:      public static void main(String[] args) {
4:          Fish f1 = new Fish("Nemo");
5:          Fish f2 = new Fish(Integer.valueOf(1));
6: 
7:          if(f1 instanceof Fish(Object t)) {
8:              System.out.print("Match1-");
9:          }
10:         if(f1 instanceof Fish(String t)) {
11:             System.out.print("Match2-");
12:         }
13:         if(f1 instanceof Fish(Integer t)) {
14:             System.out.print("Match3-");
15:         }
16:         if(f2 instanceof Fish(String t)) {
17:             System.out.print("Match4-");
18:         }
19:         if(f2 instanceof Fish(Integer x)) {
20:             System.out.print("Match5");
21:         } 
22:     } 
23: }
```

La primera y la segunda instrucción de coincidencia de patrones coinciden porque `"Nemo"` puede ser moldeado implícitamente a `Object` y `String`, respectivamente. La tercera instrucción compila pero no coincide, ya que `"Nemo"` no puede ser moldeado a `Integer`. Del mismo modo, la cuarta instrucción compila pero no coincide, ya que el valor numérico no puede ser moldeado a `String`. Finalmente, la última instrucción coincide ya que el tipo de ambos es `Integer`. El código compila e imprime lo siguiente en tiempo de ejecución:

```Plaintext
Match1-Match2-Match5
```

¿Qué sucede si **se cambia** la declaración de `Fish` a lo siguiente?

```Java
1:  record Fish(Integer type) {}
```

En primer lugar, ¡la variable `f1` declarada en la línea 4 ya no compilaría! Sin embargo, suponiendo que **se arreglara** la declaración de la variable, las líneas 10 y 16 no compilarían. El compilador es lo suficientemente inteligente como para saber que ninguna instancia de `Fish` es capaz de coincidir un `Integer` con un `String`.

**Anidando Patrones de record**

Si un `record` incluye otros valores `record` como miembros, entonces **se pueden** opcionalmente hacer coincidir los campos dentro del `record`. ¿Listos para ver cómo funciona esto? **Se comenzará** con dos `records`.

```Java
record Bear(String name, List<String> favoriteThings) {}
record Couple(Bear a, Bear b) {}
```

Ahora, suponga que **se define** una instancia de `Couple` dentro de un método.

```Java
var c = new Couple(new Bear("Yogi", List.of("PicnicBaskets")), new Bear("Fozzie", List.of("BadJokes")));
```

¿Cuáles de las siguientes instrucciones de coincidencia de patrones compilan?

```Java
if(c instanceof Couple(Bear a, Bear b)) {
    System.out.print(a.name() + " " + b.name());
}

if(c instanceof Couple(Bear(String firstName, List<String> f), Bear b)) {
    System.out.print(firstName + " " + b.name());
}

if(c instanceof Couple(Bear(String name, List<String> f1), Bear(String name, List<String> f2))) {
    System.out.print(name + " " + name);
}
```

La primera instrucción de coincidencia de patrones compila y usa `Couple` sin expandir los `records` `Bear` anidados. El segundo ejemplo expande el primer `record` `Bear`, haciendo que `firstName` y `b` sean variables locales dentro de la instrucción de coincidencia de patrones. La tercera instrucción de coincidencia de patrones no compila. Aunque **se pueden expandir** ambos `records`, **se les debe** dar nombres distintos. Sin embargo, esto **se puede arreglar** expandiendo los tipos anidados para que tengan nombres únicos.

```Java
if(c instanceof Couple(Bear(String name1, List<String> f1), Bear(String name2, List<String> f2))) {
    System.out.print(name1 + " " + name2);
}
```

**Coincidencia de Records con var y Genéricos**

También **se puede usar** `var` en un `record` de coincidencia de patrones. **Se aplicará** esto a los ejemplos anteriores.

```Java
var c = new Couple(new Bear("Yogi", List.of("PicnicBaskets")),
                   new Bear("Fozzie", List.of("BadJokes")));

if (c instanceof Couple(var a, var b)) {
    System.out.print(a.name() + " " + b.name());
}

if (c instanceof Couple(Bear(var firstName, List<String> f), var b)) {
    System.out.print(firstName + " " + b.name());
}
```

Como **se puede observar**, **se puede reemplazar** cualquier tipo de referencia de un elemento con `var`. Cuando se usa `var` para uno de los elementos del `record`, el compilador asume que el tipo es la coincidencia exacta para el tipo en el `record`.

Los genéricos de coincidencia de patrones dentro de los `records` siguen reglas similares a las de la sobrecarga de métodos genéricos. No hay que preocuparse si no se ha visto la sobrecarga de genéricos antes, **se cubrirá** en el Capítulo 9, "Colecciones y Genéricos". No obstante, **se probarán** algunos ejemplos para ver los tipos de cosas que el examen podría plantear. Cada uno de los siguientes compila sin problema:

```Java
if(c instanceof Couple(Bear(var n, Object f), var b)) {}
if(c instanceof Couple(Bear(var n, List f), var b)) {}
if(c instanceof Couple(Bear(var n, List<?> f), var b)) {}
if(c instanceof Couple(Bear(var n, List<? extends Object> f), var b)) {}
if(c instanceof Couple(Bear(var n, ArrayList<String> f), var b)) {}
```

Sin embargo, hay límites. Por ejemplo, los siguientes dos ejemplos no compilan:

```Java
if(c instanceof Couple(Bear(var n, List<> f), var b)) {}
if(c instanceof Couple(Bear(var n, List<Object> f), var b)) {}
```

El primer ejemplo no compila porque el operador diamante (`<>`) no puede ser usado para la coincidencia de patrones (ni para la sobrecarga de genéricos). El segundo ejemplo no compila porque `List<Object>` no es compatible con `List<String>`. Esto tampoco compilaría si estos tipos fueran aplicados a los parámetros de métodos heredados, debido a la eliminación de tipos (_type erasure_). Nuevamente, **se cubrirán** los genéricos y la eliminación de tipos con mucho más detalle en el Capítulo 9.

En estos ejemplos, **se debe recordar** que `f` es el tipo del patrón, no el `List<String>` original. Dado esto, ¿**se puede deducir** por qué este código no compila?

```Java
if(c instanceof Couple(Bear(var n, List f), var b) && f.getFirst().toLowerCase().contains("p")) { // NO COMPILA
    System.out.print("Yummy");
}
```

El tipo de referencia de `f` es `List`, no `List<String>`, por lo tanto, `f.getFirst()` devuelve una referencia a `Object`, no una referencia a `String`. Dado que `toLowerCase()` no está definido en `Object`, el código no compila. Para que compile **se tendría** que moldear explícitamente a un `String` o usar un tipo de coincidencia de patrones diferente.

**Aplicando records de Coincidencia de Patrones a Switch**

Puede que no sea una sorpresa que **se pueda usar** `switch` con coincidencia de patrones y `records`. Las reglas son las mismas que **ya se aprendieron**, simplemente **se están combinando** las reglas de coincidencia de patrones para `switch` que **se aprendieron** en el Capítulo 3 con lo que **se cubrió** en este capítulo.

Suponga que **se tiene** un `record` `Snake` como el siguiente:

```Java
record Snake(Object data) {}
```

A continuación, **se construirá** un método que opera sobre una instancia de `Snake`.

```Java
long showData(Snake snake) {
    return switch(snake) {
        case Snake(Long hiss)      -> hiss + 1;
        case Snake(Integer nagina) -> nagina + 10;
        case Snake(Number crowley) -> crowley.intValue() + 100;
        case Snake(Object kaa)     -> -1;
    };
}
```

Como **se podrá recordar** del Capítulo 3, no se requiere una cláusula `default` si todos los tipos están cubiertos en la expresión de coincidencia de patrones. Dado este código, a ver si **se puede seguir** la salida generada por cada uno de estos ejemplos:

```Java
System.out.println(showData(new Snake(1)));    // 11
System.out.println(showData(new Snake(2L)));   // 3
System.out.println(showData(new Snake(3.0)));  // 103
```

**Se debe recordar**, el tipo es importante para cualquier cláusula `when` asociada. Por ejemplo, lo siguiente no compila puesto que `kaa` es de tipo `Object`, el cual no tiene un método `doubleValue()`:

```Java
long showData(Snake snake) {
    return switch(snake) {
        case Snake(Object kaa) when kaa.doubleValue() > 0 -> -1; // NO COMPILA
        default -> 1_000;
    };
}
```

¡Felicidades, **se ha aprendido** todo lo que **se necesita saber** sobre la coincidencia de patrones con `records` para el examen! ¡Puesto que es una nueva característica, hay que esperar ver al menos una pregunta sobre ello!

#### Personalizando records

Dado que los `records` están orientados a datos, **se ha enfocado** en las características de los `records` que es probable que **se utilicen**. Los `records` en realidad soportan muchas de las mismas características que una clase. Aquí están algunos de los miembros que los `records` pueden incluir y con los que **se debería estar** familiarizado para el examen:

- Constructores sobrecargados y compactos.
- Métodos de instancia, incluyendo la sobrescritura de cualquier método proporcionado (métodos de acceso, `equals()`, `hashCode()`, `toString()`). 
- Clases anidadas, interfaces, anotaciones, `enums` y `records`.

Como ejemplo ilustrativo, lo siguiente sobrescribe dos métodos de instancia utilizando la anotación opcional `@Override`:

```Java
public record Crane(int numberEggs, String name) {
    @Override public int numberEggs() { return 10; }
    @Override public String toString() { return name; }
}
```

Aunque **se pueden añadir** métodos, campos `static` y otros tipos de datos, no **se pueden añadir** campos de instancia fuera de la declaración del `record`, incluso si son `private` y `final`. ¡Hacerlo anula el propósito de usar un `record` y podría romper la inmutabilidad!

```Java
public record Crane(int numberEggs, String name) {
    private static int TYPE = 10;
    public int size;                     // NO COMPILA
    private final boolean friendly = true; // NO COMPILA
}
```

Los `records` tampoco soportan inicializadores de instancia. Toda la inicialización de los campos de un `record` debe ocurrir en un constructor. Sin embargo, sí soportan inicializadores `static`.

```Java
public record Crane(int numberEggs, String name) {
    static { System.out.print("Hello Bird!"); }
    { System.out.print("Goodbye Bird!"); } // NO COMPILA
    { this.name = "Big"; }                 // NO COMPILA
}
```

En este ejemplo, el primer inicializador compila porque es `static`, mientras que el segundo y el tercero no lo hacen porque son inicializadores de instancia.

> Si bien es una característica útil que los `records` soporten muchos de los mismos miembros que una clase, **se debe intentar** mantenerlos simples. Al igual que los POJOs y JavaBeans de los que nacieron, cuanto más complicados se vuelven, menos utilizables son.

Esta es la segunda vez que **se mencionan** los tipos anidados, la primera fue con las clases selladas y ahora con los `records`. ¡Que no haya preocupación; **se cubrirán** pronto!
