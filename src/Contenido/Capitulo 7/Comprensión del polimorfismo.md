**Se concluye** este capítulo con una discusión sobre el polimorfismo, la propiedad de un objeto de tomar muchas formas diferentes. Para ser más precisos, un objeto de Java puede ser accedido usando lo siguiente:

- Una referencia con el mismo tipo que el objeto.
- Una referencia que es una superclase del objeto.
- Una referencia de una interfaz que el objeto implementa o hereda.

Además, no **se requiere** un moldeado (_cast_) si el objeto está siendo reasignado a un supertipo o interfaz del objeto. ¡Eso es mucho! No hay que preocuparse; tendrá sentido en breve.

**Se ilustrará** esta propiedad del polimorfismo con el siguiente ejemplo:

```Java
public class Primate {
    public boolean hasHair() {
        return true;
    }
}

public interface HasTail {
    public abstract boolean isTailStriped();
}

public class Lemur extends Primate implements HasTail {
    public boolean isTailStriped() {
        return false;
    }
    public int age = 10;
    public static void main(String[] args) {
        Lemur lemur = new Lemur();
        System.out.println(lemur.age);

        HasTail hasTail = lemur;
        System.out.println(hasTail.isTailStriped());

        Primate primate = lemur;
        System.out.println(primate.hasHair());
    } }
```

Este código compila e imprime la siguiente salida:

```Plaintext
10
false
true
```

Lo más importante a notar en este ejemplo es que solo **se crea** un objeto, `Lemur`. El polimorfismo permite que una instancia de `Lemur` sea reasignada o pasada a un método usando uno de sus supertipos, como `Primate` o `HasTail`.

Una vez que el objeto ha sido asignado a un nuevo tipo de referencia, solo los métodos y variables disponibles para ese tipo de referencia son invocables en el objeto sin un moldeado (_cast_) explícito. Por ejemplo, los siguientes fragmentos de código no compilarán:

```Java
HasTail hasTail = new Lemur();
System.out.println(hasTail.age);           // NO COMPILA

Primate primate = new Lemur();
System.out.println(primate.isTailStriped()); // NO COMPILA
```

En este ejemplo, la referencia `hasTail` solo tiene acceso directo a los métodos definidos con la interfaz `HasTail`; por lo tanto, no sabe que la variable `age` es parte del objeto. Del mismo modo, la referencia `primate` solo tiene acceso a los métodos definidos en la clase `Primate`, y no tiene acceso directo al método `isTailStriped()`.

#### Objeto vs. Referencia

En Java, todos los objetos **se acceden** por referencia, por lo que como desarrollador nunca **se tiene** acceso directo al objeto en sí. Conceptualmente, sin embargo, **se debería** considerar el objeto como la entidad que existe en memoria, asignada por la JVM. Independientemente del tipo de la referencia que **se tenga** para el objeto en memoria, el objeto en sí no cambia. Por ejemplo, puesto que todos los objetos heredan `java.lang.Object`, todos pueden ser reasignados a `java.lang.Object`, como **se muestra** en el siguiente ejemplo:

```Java
Lemur lemur = new Lemur();
Object lemurAsObject = lemur;
```

Aunque el objeto `Lemur` ha sido asignado a una referencia con un tipo diferente, el objeto en sí no ha cambiado y todavía existe como un objeto `Lemur` en memoria. Lo que ha cambiado, entonces, es la capacidad de acceder a los métodos dentro de la clase `Lemur` con la referencia `lemurAsObject`. Sin un moldeado explícito de vuelta a `Lemur`, como **se verá** en la siguiente sección, ya no **se tiene** acceso a las propiedades de `Lemur` del objeto.

**Se puede resumir** este principio con las siguientes dos reglas:

1. El tipo del objeto determina qué propiedades existen dentro del objeto en memoria.
2. El tipo de la referencia al objeto determina qué métodos y variables son accesibles al programa Java.

Por lo tanto, cambiar con éxito la referencia de un objeto a un nuevo tipo de referencia puede permitirte acceder a nuevas propiedades del objeto; pero recuerda que esas propiedades ya existían antes de que se produjera el cambio de referencia.

Usando el ejemplo de `Lemur`, **se ilustra** esta propiedad en la siguiente imagen.

![[Objeto frente a referencia.png]]

Como **se puede ver** en la imagen, el mismo objeto existe en memoria independientemente de cuál referencia está apuntando a él. Dependiendo del tipo de la referencia, solo **se puede** tener acceso a ciertos métodos. Por ejemplo, la referencia `hasTail` tiene acceso al método `isTailStriped()` pero no tiene acceso a la variable `age` definida en la clase `Lemur`. Como **se aprenderá** en la siguiente sección, es posible recuperar el acceso a la variable `age` moldeando explícitamente la referencia `hasTail` a una referencia de tipo `Lemur`.

> **Escenario del Mundo Real**
>
> **Usando Referencias de Interfaz**
>
> Cuando **se trabaja** con un grupo de objetos que implementan una interfaz común, **se considera** una buena práctica de codificación usar una interfaz como tipo de referencia. Esto es especialmente común con las colecciones que **se aprenderán** en el Capítulo 9. Considere el siguiente método:
>
> ```Java
> public void sortAndPrintZooAnimals(List<String> animals) {
>     Collections.sort(animals);
>     for(String a : animals) System.out.println(a);
> }
> ```
>
> Este método ordena e imprime `animals` en orden alfabético. En ningún momento este clase está interesada en cuál es el objeto subyacente real para `animals`. Podría ser un `ArrayList` u otro tipo. El punto es que el código funciona en cualquiera de estos tipos porque **se usó** el tipo de referencia de la interfaz en lugar de un tipo de clase.

#### Moldeando Objetos

En el ejemplo anterior, **se creó** una única instancia de un objeto `Lemur` y **se accedió** a ella mediante referencias de superclase e interfaz. Una vez que **se cambió** el tipo de referencia, sin embargo, **se perdió** el acceso a miembros más específicos definidos en la subclase que todavía existen dentro del objeto. **Se pueden recuperar** esas referencias moldeando el objeto de vuelta a la subclase específica de la que provino:

```Java
Lemur lemur = new Lemur();

Primate primate = lemur;          // Moldeado implícito a supertipo

Lemur lemur2 = (Lemur)primate;    // Moldeado explícito a subtipo

Lemur lemur3 = primate;           // NO COMPILA (falta el moldeado)
```

En este ejemplo, primero **se crea** un objeto `Lemur` y **se moldea** implícitamente a una referencia `Primate`. Puesto que `Lemur` es un subtipo de `Primate`, esto puede hacerse sin un operador de moldeado. Luego **se moldea** de vuelta a un objeto `Lemur` usando un moldeado explícito, ganando acceso a todos los métodos y campos en la clase `Lemur`. La última línea no compila porque **se requiere** un moldeado explícito. Aunque el objeto está almacenado en memoria como un objeto `Lemur`, **se necesita** un moldeado explícito para asignarlo a `Lemur`.

Moldear objetos es similar a moldear primitivos, como **se vio** en el Capítulo 2. Cuando **se moldean** objetos, **no se necesita** un operador de moldeado si **se moldea** a un supertipo heredado. Esto **se denomina** un **moldeado implícito** (_implicit cast_) y aplica a clases o interfaces que el objeto hereda. Alternativamente, si **se quiere** acceder a un subtipo de la referencia actual, **se necesita** realizar un moldeado explícito con un tipo compatible. Si el objeto subyacente no es compatible con el tipo, entonces **se lanzará** una `ClassCastException` en tiempo de ejecución.

Cuando **se revisa** una pregunta del examen que involucra moldeado y polimorfismo, hay que asegurarse de recordar cuál es la instancia real del objeto. Luego, hay que concentrarse en si el compilador permitirá que el objeto sea referenciado con o sin moldeos explícitos.

**Se resumen** estos conceptos en un conjunto de reglas para memorizar para el examen:

1. Moldear una referencia de un subtipo a un supertipo no requiere un moldeado explícito.
2. Moldear una referencia de un supertipo a un subtipo requiere un moldeado explícito.
3. En tiempo de ejecución, un moldeado inválido de una referencia a un tipo incompatible resulta en que **se lanza** una `ClassCastException`.
4. El compilador no permite moldeos a tipos no relacionados.

**Moldeos No Permitidos**

Las primeras tres reglas son solo un repaso de lo que **se ha dicho** hasta ahora. La última regla es un poco más complicada. El examen podría intentar engañar con un moldeado que el compilador sabe que no está permitido (es decir, imposible). En el ejemplo anterior, **se pudo** moldear una referencia de `Primate` a una referencia de `Lemur` porque `Lemur` es una subclase de `Primate` y por lo tanto están relacionadas. Considere este ejemplo en su lugar:

```Java
public class Bird {}

public class Fish {
    public static void main(String[] args) {
        Fish fish = new Fish();
        Bird bird = (Bird)fish; // NO COMPILA
    }
}
```

En este ejemplo, las clases `Fish` y `Bird` no están relacionadas a través de ninguna jerarquía de clases de la que el compilador tenga conocimiento; por lo tanto, el código no compilará. Aunque ambas extienden `Object` implícitamente, **se consideran** tipos no relacionados ya que ninguna puede ser un subtipo de la otra.

**Moldeando Interfaces**

Mientras que el compilador puede hacer cumplir reglas sobre el moldeado a tipos no relacionados para clases, no siempre puede hacer lo mismo para interfaces. Hay que recordar que las interfaces soportan herencia múltiple, lo que limita lo que el compilador puede razonar sobre ellas. Si bien una clase dada puede no implementar una interfaz, es posible que alguna subclase pueda implementar la interfaz. Cuando **se tiene** una referencia a una clase particular, el compilador no sabe qué subtipo específico está sosteniendo.

**Se intentará** un ejemplo. ¿Crees que el siguiente programa compila?

```Java
1: interface Canine {}
2: interface Dog {}
3: class Wolf implements Canine {}
4:
5: public class BadCasts {
6:     public static void main(String[] args) {
7:         Wolf wolfy = new Wolf();
8:         Dog badWolf = (Dog)wolfy;
9:     } }
```

En este programa, un objeto `Wolf` **se crea** y luego **se asigna** a un tipo de referencia `Wolf` en la línea 7. Con interfaces, el compilador tiene capacidad limitada para hacer cumplir muchas reglas porque aunque un tipo de referencia podría no implementar una interfaz, uno de sus subclases podría. Por lo tanto, permite el moldeado inválido al tipo de referencia `Dog` en la línea 8, aunque `Dog` y `Wolf` no estén relacionados. No hay que temer, aunque el código compila, aún lanza una `ClassCastException` en tiempo de ejecución.

Dejando de lado esta limitación, el compilador puede hacer cumplir una regla en cuanto al moldeado de interfaces. El compilador no permite un moldeado de una referencia de interfaz a una referencia de objeto si el tipo de objeto no puede posiblemente implementar la interfaz, como si la clase está marcada como `final`. Por ejemplo, ¿qué pasaría si **se cambia** la línea 3 del código anterior?

```Java
3: final class Wolf implements Canine {}
```

La línea 8 ya no compila. El compilador reconoce que no hay subclases posibles de `Wolf` capaces de implementar la interfaz `Dog`.

#### El Operador `instanceof`

El operador `instanceof` puede **ser usado** para verificar si un objeto pertenece a una clase o interfaz particular y para prevenir una `ClassCastException` en tiempo de ejecución. Como **se vio** en el Capítulo 3, también puede **ser usado** con la coincidencia de patrones (_pattern matching_). Considere el siguiente ejemplo:

```Java
1: class Rodent {}
2:
3: public class Capybara extends Rodent {
4:     public static void main(String[] args) {
5:         Rodent rodent = new Rodent();
6:         var capybara = (Capybara)rodent; // ClassCastException
7:     }
8: }
```

Este programa lanza una excepción en la línea 6. **Se puede** reemplazar la línea 6 con lo siguiente:

```Java
6:      if(rodent instanceof Capybara c) {
7:          // Hacer algo
8:      }
```

Ahora el fragmento de código no lanza una excepción en tiempo de ejecución y realiza el moldeado solo si el operador `instanceof` tiene éxito.

Al igual que el compilador no permite moldear un objeto a tipos no relacionados, tampoco permite usar `instanceof` con tipos no relacionados. **Se puede demostrar** esto con las clases no relacionadas `Bird` y `Fish`:

```Java
public class Bird {}

public class Fish {
    public static void main(String[] args) {
        Fish fish = new Fish();
        if (fish instanceof Bird b) { // NO COMPILA
            // Hacer algo
        } } }
```

#### Polimorfismo y Sobrescritura de Métodos

En Java, el polimorfismo establece que cuando **se sobrescribe** un método, **se reemplazan** todas las llamadas al mismo, incluso las definidas en la clase padre. Como ejemplo, ¿qué produce el siguiente fragmento de código?

```Java
class Penguin {
    public int getHeight() { return 3; }
    public void printInfo() {
        System.out.print(this.getHeight());
    }
}

public class EmperorPenguin extends Penguin {
    public int getHeight() { return 8; }
    public static void main(String []fish) {
        new EmperorPenguin().printInfo();
    } }
```

Si **se dijo** 8, entonces **se está en** buen camino para entender el polimorfismo. En este ejemplo, el objeto que **se opera** en memoria es un `EmperorPenguin`. El método `getHeight()` está sobrescrito en la subclase, lo que significa que todas las llamadas al mismo se reemplazan en tiempo de ejecución. A pesar de que `printInfo()` está definido en la clase `Penguin`, llamar a `getHeight()` en el objeto llama al método asociado con el objeto preciso en memoria, no al tipo de referencia actual donde **se llama**. Incluso usando la referencia `this`, que es opcional en este ejemplo, no llama a la versión padre porque el método ha sido reemplazado.

*La capacidad del polimorfismo de reemplazar métodos en tiempo de ejecución a través de la sobrescritura es una de las propiedades más importantes de Java.* Permite crear modelos de herencia complejos con subclases que tienen su propia implementación personalizada de métodos sobrescritos. También significa que la clase padre no necesita ser actualizada para usar el método personalizado o sobrescrito. Si el método está sobrescrito correctamente, la versión sobrescrita **se usará** en todos los lugares donde **se llame**.

Hay que recordar que **se puede** elegir limitar el comportamiento polimórfico marcando los métodos como `final`, lo cual impide que sean sobrescritos por una subclase.

> **Escenario del Mundo Real**
>
> **Llamando a la Versión Padre de un Método Sobrescrito**
>
> El hecho de que un método esté sobrescrito no significa que el método padre sea completamente inaccesible. **Se puede** usar la referencia `super` que **se aprendió** en el Capítulo 6 para acceder a él. ¿Cómo **se puede** modificar el ejemplo anterior para imprimir 3 en lugar de 8? **Se podría** intentar llamar a `super.getHeight()` en la clase padre `Penguin`:
>
> ```Java
> class Penguin {
>     public int getHeight() { return 3; }
>     public void printInfo() {
>         System.out.print(super.getHeight()); // NO COMPILA
>     }
> }
> ```
>
> Desafortunadamente, esto no compila, ya que `super` se refiere a la superclase de `Penguin`; en este caso, `Object`. La solución es sobrescribir `printInfo()` en la clase hija `EmperorPenguin` y usar `super` allí.
>
> ```Java
> public class EmperorPenguin extends Penguin {
>     public int getHeight() { return 8; }
>     public void printInfo() {
>         System.out.print(super.getHeight());
>     }
>     public static void main(String []fish) {
>         new EmperorPenguin().printInfo(); // 3
>     }
> }
> ```

#### Sobrescritura vs. Ocultación de Miembros

Si bien la sobrescritura de métodos reemplaza el método en todos los lugares donde **se llama**, la ocultación de métodos `static` y variables no lo hace. Hablando estrictamente, la ocultación de miembros no es una forma de polimorfismo, ya que los métodos y variables mantienen sus propiedades individuales. A diferencia de la sobrescritura de métodos, la ocultación de miembros es muy sensible al tipo de referencia y la ubicación donde el miembro está siendo utilizado.

**Se observará** un ejemplo:

```Java
class Penguin {
    public static int getHeight() { return 3; }
    public void printInfo() {
        System.out.println(this.getHeight());
    } }

public class CrestedPenguin extends Penguin {
    public static int getHeight() { return 8; }
    public static void main(String… fish) {
        new CrestedPenguin().printInfo();
    } }
```

El ejemplo `CrestedPenguin` es casi idéntico al ejemplo anterior de `EmperorPenguin`, aunque como probablemente ya **se habrá** adivinado, imprime 3 en lugar de 8. El método `getHeight()` es `static` y por lo tanto está oculto, no sobrescrito. El resultado es que llamar a `getHeight()` en `CrestedPenguin` devuelve un valor diferente que llamarlo en `Penguin`, incluso si el objeto subyacente es el mismo. Contrasta esto con la sobrescritura de un método, donde devuelve el mismo valor para un objeto independientemente de en qué clase **se llame**.

¿Qué pasa con el hecho de que **se usó** `this` para acceder a un método `static` en `this.getHeight()`? Como **se discutió** en el Capítulo 5, aunque está permitido usar una referencia de instancia para acceder a una variable o método `static`, hacerlo a menudo **se desaconseja**. El compilador advertirá cuando **se acceda** a miembros `static` de una forma no `static`. En este caso, la referencia `this` no tuvo ningún impacto en la salida del programa.

Además de la ubicación, el tipo de referencia también puede determinar el valor que **se obtiene** cuando **se trabaja** con miembros ocultos. ¿Listo? **Se intentará** un ejemplo más complejo:

```Java
class Marsupial {
    protected int age = 2;
    public static boolean isBiped() {
        return false;
    } }

public class Kangaroo extends Marsupial {
    protected int age = 6;
    public static boolean isBiped() {
        return true;
    }

    public static void main(String[] args) {
        Kangaroo joey = new Kangaroo();
        Marsupial moey = joey;
        System.out.println(joey.isBiped());
        System.out.println(moey.isBiped());
        System.out.println(joey.age);
        System.out.println(moey.age);
    } }
```

El programa imprime lo siguiente:

```Plaintext
true
false
6
2
```

En este ejemplo, ¡solo **se crea** *un objeto* (de tipo `Kangaroo`) y **se almacena** en memoria! Puesto que los métodos `static` solo pueden estar ocultos, no sobrescritos, Java usa el tipo de referencia para determinar qué versión de `isBiped()` debería llamarse, lo que resulta en que `joey.isBiped()` imprime `true` y `moey.isBiped()` imprime `false`.

Del mismo modo, la variable `age` está oculta, no sobrescrita, por lo que el tipo de referencia **se usa** para determinar qué valor imprimir. Esto resulta en que `joey.age` devuelva 6 y `moey.age` devuelva 2.

Para el examen, **se debe asegurar** de entender estos ejemplos, ya que muestran cómo los métodos ocultos y sobrescritos son fundamentalmente diferentes. En la práctica, la sobrescritura de métodos es la piedra angular del polimorfismo y una característica extremadamente poderosa.

> **Escenario del Mundo Real**
>
> **No Ocultar Miembros en la Práctica**
>
> Aunque Java permite ocultar variables y métodos `static`, **se considera** una práctica de codificación extremadamente mala. Como **se vio** en el ejemplo anterior, el valor de la variable o método puede cambiar dependiendo de qué referencia **se usa**, haciendo que el código sea muy confuso, difícil de seguir y desafiante para que otros lo mantengan. Esto **se agrava** aún más cuando **se empieza** a modificar el valor de la variable tanto en los métodos padre como hijo, ya que puede no quedar claro cuál variable **se está** actualizando.
>
> Cuando **se define** una nueva variable o método `static` en una clase hija, **se considera** una buena práctica de codificación seleccionar un nombre que no esté ya siendo usado por un miembro heredado. Redeclarar métodos y variables `private` **se considera** menos problemático, sin embargo, porque la clase hija no tiene acceso a la variable de la clase padre para empezar.
