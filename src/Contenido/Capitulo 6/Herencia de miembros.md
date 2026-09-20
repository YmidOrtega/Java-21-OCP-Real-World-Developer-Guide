Ahora que **se ha creado** una clase, ¿qué **se puede hacer** con ella? Una de las mayores fortalezas de Java es aprovechar su modelo de herencia para simplificar el código. Por ejemplo, **se asume** que **se tienen** cinco clases, cada una de las cuales extiende de la clase `Animal`. Además, cada clase define un método `eat()` con una implementación idéntica. En este escenario, es mucho mejor definir `eat()` una vez en la clase `Animal` que tener que mantener el mismo método en cinco clases separadas.

Heredar una clase no solo otorga acceso a métodos heredados en la clase padre, sino que también prepara el terreno para colisiones entre métodos definidos tanto en la clase padre como en la subclase. En esta sección, **se repasan** las reglas para la herencia de métodos y cómo Java maneja estos escenarios.

**Se hace referencia** a la capacidad de un objeto de adoptar muchas formas diferentes como **polimorfismo** (_polymorphism_). **Se cubre** esto más a fondo en el próximo capítulo, pero por ahora solo **se necesita** saber que un objeto se puede usar de diversas maneras, en parte según la variable de referencia usada para llamar al objeto.

### Sobrescribiendo un Método

¿Qué sucede si un método con la misma firma se define tanto en la clase padre como en la hija? Por ejemplo, **se podría** querer definir una nueva versión del método y hacer que se comporte de manera diferente para esa subclase. La solución es sobrescribir el método en la clase hija. En Java, la sobrescritura de un método ocurre cuando una subclase declara una nueva implementación para un método heredado con la misma firma y tipo de retorno compatible.

**Se debe recordar** que la firma de un método está compuesta por el nombre del método y los parámetros del método. No incluye el tipo de retorno, los modificadores de acceso, los especificadores opcionales ni ninguna excepción declarada.

Cuando **se sobrescribe** un método, aún **se puede referenciar** a la versión padre del método usando la palabra clave `super`. De esta manera, las palabras clave `this` y `super` **permiten** seleccionar entre la versión actual y la versión padre de un método, respectivamente. **Se ilustra** esto con el siguiente ejemplo:

```Java
public class Marsupial {
    public double getAverageWeight() {
        return 50;
    }
}
public class Kangaroo extends Marsupial {
    public double getAverageWeight() {
        return super.getAverageWeight()+20;
    }
    public static void main(String[] args) {
        System.out.println(new Marsupial().getAverageWeight()); // 50.0
        System.out.println(new Kangaroo().getAverageWeight());  // 70.0
    }
}
```

En este ejemplo, la clase `Kangaroo` sobrescribe el método `getAverageWeight()` pero en el proceso llama a la versión del padre usando la referencia `super`.

> **Llamadas Infinitas en Sobrescritura de Métodos**
> Podría surgir la duda sobre si el uso de `super` en el ejemplo anterior era necesario. Por ejemplo, ¿qué emitiría el siguiente código si **se eliminara** la palabra clave `super`?
> ```Java
> public double getAverageWeight() {
>     return getAverageWeight()+20; // StackOverflowError
> }
> ```
> En este ejemplo, el compilador no llamaría al método del padre `Marsupial`; llamaría al método actual de `Kangaroo`. La aplicación intentará llamarse a sí misma infinitamente y producirá un `StackOverflowError` en tiempo de ejecución.

Para sobrescribir un método, **se debe seguir** una serie de reglas. El compilador realiza las siguientes verificaciones cuando **se sobrescribe** un método:

1. El método en la clase hija debe tener la misma firma que el método en la clase padre.
    
2. El método en la clase hija debe ser al menos tan accesible como el método en la clase padre.
    
3. El método en la clase hija no puede declarar una excepción verificada (_checked exception_) que sea nueva o más amplia que la clase de cualquier excepción declarada en el método de la clase padre.
    
4. Si el método devuelve un valor, debe ser el mismo o un subtipo del método en la clase padre, conocidos como **tipos de retorno covariantes** (_covariant return types_).
    

Si bien estas reglas pueden parecer confusas o arbitrarias al principio, son necesarias para la consistencia. Sin estas reglas en vigor, es posible crear contradicciones dentro del lenguaje Java.

#### Regla #1: Firmas de Métodos

La primera regla para sobrescribir un método se explica por sí misma. Si dos métodos tienen el mismo nombre pero diferentes firmas, los métodos están sobrecargados, no sobrescritos. Los métodos sobrecargados se consideran independientes y no comparten las mismas propiedades polimórficas que los métodos sobrescritos.

> **Se cubrió** la sobrecarga de un método en el Capítulo 5, y es similar a la sobrescritura de un método, ya que ambas implican definir un método usando el mismo nombre. La sobrecarga difiere de la sobrescritura en que los métodos sobrecargados usan una lista de parámetros diferente. Para el examen, es importante que **se comprenda** esta distinción y que los métodos sobrescritos tienen la misma firma y muchas más reglas que los métodos sobrecargados.

#### Regla #2: Modificadores de Acceso

¿Cuál es el propósito de la segunda regla sobre los modificadores de acceso? **Se probará** un ejemplo ilustrativo:

```Java
public class Camel {
    public int getNumberOfHumps() {
        return 1;
    } 
}
public class BactrianCamel extends Camel {
    private int getNumberOfHumps() { // NO COMPILA
        return 2;
    } 
}
```

En este ejemplo, `BactrianCamel` intenta sobrescribir el método `getNumberOfHumps()` definido en la clase padre pero falla porque el modificador de acceso `private` es más restrictivo que el definido en la versión del padre del método. Suponga que a `BactrianCamel` se le permitiera compilar. ¿Qué imprimiría este programa?

```Java
public class Rider {
    public static void main(String[] args) {
        Camel c = new BactrianCamel();
        System.out.print(c.getNumberOfHumps()); // ???
    } 
}
```

La respuesta es que no **se sabe**. El tipo de referencia para el objeto es `Camel`, donde el método se declara `public`, pero el objeto es en realidad una instancia de tipo `BactrianCamel`, donde el método se declara `private`. Java evita este tipo de problemas de ambigüedad limitando la sobrescritura de un método a modificadores de acceso que sean igual de accesibles o más accesibles que la versión en el método heredado.

#### Regla #3: Excepciones Verificadas

La tercera regla establece que la sobrescritura de un método no puede declarar nuevas excepciones verificadas (_checked exceptions_) o excepciones verificadas más amplias que el método heredado. Esto se hace por razones polimórficas similares a la limitación de los modificadores de acceso. En otras palabras, **se podría terminar** con un objeto que es más restrictivo que el tipo de referencia al que se asigna, resultando en una excepción verificada que no es manejada o declarada. Una implicación de esta regla es que los métodos sobrescritos son libres de declarar cualquier número de nuevas excepciones no verificadas (_unchecked exceptions_).

> Si no **se sabe** qué es una excepción verificada o no verificada, no hay de qué preocuparse. **Se cubre** esto en el Capítulo 11, "Excepciones y Localización". Por ahora, solo **se necesita** saber que la regla aplica únicamente a las excepciones verificadas. También es útil saber que tanto `IOException` como `FileNotFoundException` son excepciones verificadas, y que `FileNotFoundException` es una subclase de `IOException`.

**Se analizará** un ejemplo:

```Java
public class Reptile {
    protected void sleep() throws IOException {}
    protected void hide() {}
    protected void exitShell() throws FileNotFoundException {}
}
public class GalapagosTortoise extends Reptile {
    public void sleep() throws FileNotFoundException {}
    public void hide() throws FileNotFoundException {} // NO COMPILA
    public void exitShell() throws IOException {}      // NO COMPILA
}
```

En este ejemplo, **se tienen** tres métodos sobrescritos. Estos métodos sobrescritos usan el modificador más accesible `public`, lo cual está permitido según la segunda regla para métodos sobrescritos. El primer método sobrescrito `sleep()` en `GalapagosTortoise` compila sin problema porque la excepción declarada es más estrecha que la excepción declarada en la clase padre.

El método sobrescrito `hide()` no compila porque declara una nueva excepción verificada que no está presente en la declaración del padre. El método `exitShell()` sobrescrito tampoco compila, ya que `IOException` es una excepción verificada más amplia que `FileNotFoundException`. **Se revisarán** estas clases de excepción, incluyendo la memorización de cuáles son subclases de cuáles, en el Capítulo 11.

#### Regla #4: Tipos de Retorno Covariantes

La cuarta y última regla en torno a la sobrescritura de un método es probablemente la más complicada, ya que requiere conocer las relaciones entre los tipos de retorno. El método que sobrescribe debe usar un tipo de retorno que sea covariante con el tipo de retorno del método heredado.

**Se analizará** un ejemplo con fines ilustrativos:

```Java
public class Rhino {
    protected CharSequence getName() {
        return "rhino";
    }
    protected String getColor() {
        return "grey, black, or white";
    } 
}
public class JavanRhino extends Rhino {
    public String getName() {
        return "javan rhino";
    }
    public CharSequence getColor() { // NO COMPILA
        return "grey";
    } 
}
```

La subclase `JavanRhino` intenta sobrescribir dos métodos de `Rhino`: `getName()` y `getColor()`. Ambos métodos sobrescritos tienen el mismo nombre y firma que los métodos heredados. Los métodos sobrescritos también tienen un modificador de acceso más amplio, `public`, que los métodos heredados. **Se debe recordar** que un modificador de acceso más amplio es aceptable en un método sobrescrito.

A partir del Capítulo 4, "Core APIs", **se aprendió** que `String` implementa la interfaz `CharSequence`, haciendo de `String` un subtipo de `CharSequence`. Por lo tanto, el tipo de retorno de `getName()` en `JavanRhino` es covariante con el tipo de retorno de `getName()` en `Rhino`.

Por otro lado, el método sobrescrito `getColor()` no compila porque `CharSequence` no es un subtipo de `String`. Para decirlo de otra manera, todos los valores `String` son valores `CharSequence`, pero no todos los valores `CharSequence` son valores `String`. Por ejemplo, un `StringBuilder` es un `CharSequence` pero no un `String`. Para el examen, **se necesita** saber si el tipo de retorno del método que sobrescribe es el mismo o un subtipo del tipo de retorno del método heredado.

> Una prueba sencilla para la covarianza es la siguiente: dado un tipo de retorno heredado A y un tipo de retorno que sobrescribe B, ¿**se puede asignar** una instancia de B a una variable de referencia para A sin hacer un _cast_? Si es así, entonces son covariantes. Esta regla se aplica tanto a los tipos primitivos como a los tipos de objeto. Si uno de los tipos de retorno es `void`, entonces ambos deben ser `void`, ya que nada es covariante con `void` excepto él mismo.

Eso es todo lo que **se necesita** saber sobre la sobrescritura de métodos para este capítulo. En el Capítulo 9, "Colecciones y Genéricos", **se revisará** la sobrescritura de métodos que involucran genéricos. ¡Siempre hay más por aprender!

### Escenario del Mundo Real

> **Marcando Métodos con la Anotación @Override**
> Una anotación es una etiqueta de metadatos que proporciona información adicional sobre el código. **Se puede usar** la anotación `@Override` para indicarle al compilador que **se está intentando** sobrescribir un método.
> ```Java
> public class Fish {
>     public void swim() {};
> }
> public class Shark extends Fish {
>     @Override
>     public void swim() {};
> }
> ```
> Cuando el método se sobrescribe correctamente, agregar la anotación no impacta el código. Por otro lado, cuando el método se sobrescribe incorrectamente, esta anotación puede prevenir que **se cometa** un error. Lo siguiente no compila debido a la presencia de la anotación `@Override`:
> ```Java
> public class Fish {
>     public void swim() {};
> }
> public class Shark extends Fish {
>     @Override
>     public void swim(int speed) {}; // NO COMPILA
> }
> ```
> El compilador ve que **se está intentando** sobrescribir un método y busca una versión heredada de `swim()` que tome un valor `int`. Dado que el compilador no encuentra una, reporta un error. Aunque conocer temas avanzados (como la forma de crear anotaciones) no es requerido para el examen, saber cómo usarlas adecuadamente sí lo es.

### Redeclarando Métodos private

¿Qué sucede si **se intenta** sobrescribir un método `private`? En Java, **no se pueden sobrescribir** los métodos `private` puesto que no se heredan. El hecho de que una clase hija no tenga acceso al método del padre no significa que la clase hija no pueda definir su propia versión del método. Simplemente significa que, estrictamente hablando, el nuevo método no es una versión sobrescrita del método de la clase padre.

Java **permite** redeclarar un nuevo método en la clase hija con la misma o con una firma modificada con respecto al método en la clase padre. Este método en la clase hija es un método separado e independiente, no relacionado con el método de la versión del padre, por lo que no se invoca ninguna de las reglas para sobrescribir métodos. Por ejemplo, estas dos declaraciones compilan:

```Java
public class Beetle {
    private String getSize() {
        return "Undefined";
    } 
}
public class RhinocerosBeetle extends Beetle {
    private int getSize() {
        return 5;
    } 
}
```

Note que el tipo de retorno difiere en el método hijo, pasando de `String` a `int`. En este ejemplo, el método `getSize()` en la clase padre no es heredado, por lo que el método en la clase hija es un nuevo método y no una sobrescritura del método en la clase padre.

¿Qué pasaría si el método `getSize()` se declarara `public` en `Beetle`? En este caso, el método en `RhinocerosBeetle` sería una sobrescritura inválida. El modificador de acceso en `RhinocerosBeetle` es más restrictivo y los tipos de retorno no son covariantes.

### Ocultando Métodos estáticos

Un método `static` no puede ser sobrescrito porque los objetos de clase no heredan entre sí de la misma forma que los objetos de instancia. Por otro lado, sí pueden ser ocultados. Un método oculto ocurre cuando una clase hija define un método `static` con el mismo nombre y firma que un método `static` heredado definido en una clase padre. El ocultamiento de métodos (_method hiding_) es similar pero no exactamente lo mismo que la sobrescritura de métodos. Las cuatro reglas anteriores para sobrescribir un método deben seguirse cuando un método es ocultado. Además, se añade una nueva quinta regla para ocultar un método:

5. El método definido en la clase hija debe estar marcado como `static` si está marcado como `static` en una clase padre.

En pocas palabras, es **ocultamiento de métodos** si ambos métodos están marcados como `static` y **sobrescritura de métodos** si no están marcados como `static`. Si uno está marcado como `static` y el otro no, la clase no compilará.

**Se revisarán** algunos ejemplos de la nueva regla:

```Java
public class Bear {
    public static void eat() {
        System.out.println("Bear is eating");
    } 
}
public class Panda extends Bear {
    public static void eat() {
        System.out.println("Panda is chewing");
    }
    public static void main(String[] args) {
        eat();
    } 
}
```

En este ejemplo, el código compila y se ejecuta. El método `eat()` en la clase `Panda` oculta al método `eat()` en la clase `Bear`, imprimiendo `"Panda is chewing"` en tiempo de ejecución. Puesto que ambos están marcados como `static`, esto no se considera un método sobrescrito. Dicho esto, aún hay cierta herencia en curso. Si **se elimina** la declaración `eat()` en la clase `Panda`, entonces el programa imprime `"Bear is eating"` en su lugar.

**Se analizará** si **se puede descubrir** por qué cada una de las declaraciones de métodos en la clase `SunBear` no compila:

```Java
public class Bear {
    public static void sneeze() {
        System.out.println("Bear is sneezing");
    }
    public void hibernate() {
        System.out.println("Bear is hibernating");
    }
    public static void laugh() {
        System.out.println("Bear is laughing");
    }
}
public class SunBear extends Bear {
    public void sneeze() { // NO COMPILA
        System.out.println("Sun Bear sneezes quietly");
    }
    public static void hibernate() { // NO COMPILA
        System.out.println("Sun Bear is going to sleep");
    }
    protected static void laugh() { // NO COMPILA
        System.out.println("Sun Bear is laughing");
    }
}
```

En este ejemplo, `sneeze()` está marcado como `static` en la clase padre pero no en la clase hija. El compilador detecta que **se está intentando** sobrescribir usando un método de instancia. Sin embargo, `sneeze()` es un método `static` que debería ser ocultado, lo que causa que el compilador genere un error. El segundo método, `hibernate()`, no compila por la razón opuesta. El método está marcado como `static` en la clase hija pero no en la clase padre.

Finalmente, el método `laugh()` no compila. Aunque ambas versiones del método están marcadas como `static`, la versión en `SunBear` tiene un modificador de acceso más restrictivo que la que hereda, y rompe la segunda regla para la sobrescritura de métodos. **Se debe recordar**, las cuatro reglas para sobrescribir métodos se deben seguir al ocultar métodos `static`.

### Ocultando Variables

Como **se observó** con la sobrescritura de métodos, existen muchas reglas cuando dos métodos tienen la misma firma y están definidos tanto en la clase padre como en la hija. Afortunadamente, las reglas para variables con el mismo nombre en las clases padre e hija son mucho más simples. De hecho, Java no permite que las variables sean sobrescritas. Sin embargo, las variables pueden ser ocultadas.

Una variable oculta ocurre cuando una clase hija define una variable con el mismo nombre que una variable heredada definida en la clase padre. Esto crea dos copias distintas de la variable dentro de una instancia de la clase hija: una instancia definida en la clase padre y una definida en la clase hija.

Al igual que al ocultar un método `static`, no **se puede sobrescribir** una variable; solo **se puede ocultar**. **Se analizará** una variable oculta. ¿Qué **se cree** que imprime la siguiente aplicación?

```Java
class Carnivore {
    protected boolean hasFur = false;
}
public class Meerkat extends Carnivore {
    protected boolean hasFur = true;
    public static void main(String[] args) {
        Meerkat m = new Meerkat();
        Carnivore c = m;
        System.out.println(m.hasFur); // true
        System.out.println(c.hasFur); // false
    }
}
```

¿**Causa confusión** la salida? Ambas clases definen una variable `hasFur`, pero con diferentes valores. A pesar de que solo se crea un objeto por el método `main()`, ambas variables existen independientemente una de la otra. La salida cambia dependiendo de la variable de referencia utilizada.

Si no **se entendió** el último ejemplo, no hay problema. **Se cubre** el polimorfismo con más detalle en el próximo capítulo. Por ahora, solo **se necesita** saber que sobrescribir un método reemplaza el método del padre en todas las variables de referencia (distintas a `super`), mientras que ocultar un método o variable reemplaza el miembro solo si se usa un tipo de referencia de la hija.

### Escribiendo Métodos final

**Se concluye** la discusión sobre la herencia de métodos con una regla que en cierta medida se explica por sí misma: los métodos `final` no pueden ser sobrescritos. Al marcar un método como `final`, **se prohíbe** a una clase hija reemplazar este método. Esta regla está vigente tanto cuando **se sobrescribe** un método como cuando **se oculta** un método. En otras palabras, no **se puede ocultar** un método `static` en una clase hija si está marcado como `final` en la clase padre.

**Se analizará** un ejemplo:

```Java
public class Bird {
    public final boolean hasFeathers() {
        return true;
    }
    public final static void flyAway() {}
}
public class Penguin extends Bird {
    public final boolean hasFeathers() { // NO COMPILA
        return false;
    }
    public final static void flyAway() {} // NO COMPILA
}
```

En este ejemplo, el método de instancia `hasFeathers()` está marcado como `final` en la clase padre `Bird`, por lo que la clase hija `Penguin` no puede sobrescribir el método padre, resultando en un error de compilación. El método `static` `flyAway()` también está marcado como `final`, por lo que no puede ser ocultado en la subclase. En este ejemplo, el hecho de si el método hijo usa o no la palabra clave `final` es irrelevante; el código no compilará de ninguna manera.

Esta regla aplica solo a los métodos heredados. Por ejemplo, si los dos métodos estuvieran marcados como `private` en la clase padre `Bird`, entonces la clase `Penguin`, tal como se definió, compilaría. En ese caso, los métodos `private` serían redeclarados, no sobrescritos ni ocultados.