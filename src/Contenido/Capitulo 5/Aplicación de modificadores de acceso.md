Ya se vio que existen cuatro niveles de acceso: _private_, de paquete, _protected_ y _public_. Se discutirán en orden de más restrictivo a menos restrictivo:

- **`private`**: Solo accesible dentro de la misma clase.
- **Acceso de paquete (_Package access_)**: `private` más otros miembros del mismo paquete. A veces se le denomina paquete-privado (_package-private_) o acceso predeterminado (_default access_).
- **`protected`**: Acceso de paquete más acceso dentro de las subclases.
- **`public`**: `protected` más clases en los otros paquetes.

Se explorará el impacto de estos cuatro niveles de acceso en los miembros de una clase.

### Acceso Privado (_Private Access_)

Se comenzará con el acceso privado, que es el más simple. **Solo el código de la misma clase puede llamar a métodos privados o acceder a campos privados**.

Primero, observe la imagen a continuación que muestra las clases que se utilizarán para explorar el acceso privado y de paquete. Los cuadros grandes son los nombres de los paquetes. Los cuadros más pequeños dentro de ellos son las clases de cada paquete. Se puede consultar esta imagen si se desea ver rápidamente cómo se relacionan las clases.

![[Clases utilizadas para mostrar el acceso privado y el acceso por paquetes.png]]

Este es un código perfectamente legal porque todo está en una sola clase:

```Java
1: package pond.duck;
2: public class FatherDuck {
3:     private String noise = "quack";
4:     private void quack() {
5:         System.out.print(noise); // el acceso private es correcto
6:     }
7: }
```

Hasta aquí, todo bien. `FatherDuck` declara un método privado `quack()` y utiliza la variable de instancia privada `noise` en la línea 5.

Ahora se añade otra clase:

```Java
1: package pond.duck;
2: public class BadDuckling {
3:     public void makeNoise() {
4:         var duck = new FatherDuck();
5:         duck.quack();                 // NO COMPILA
6:         System.out.print(duck.noise); // NO COMPILA
7:     }
8: }
```

`BadDuckling` está intentando acceder a una variable de instancia y a un método que no le corresponde tocar. En la línea 5, intenta acceder a un método privado en otra clase. En la línea 6, intenta acceder a una variable de instancia privada en otra clase. Ambas generan errores del compilador. ¡Patito malo!

Afortunadamente, se sabe que no se permite acceder a miembros privados de otras clases y que se necesita usar un tipo de acceso diferente.

> En el ejemplo anterior, `FatherDuck` y `BadDuckling` están en archivos separados, pero **¿qué pasaría si se declararan en el mismo archivo? Incluso entonces, el código no compilaría** ya que Java impide el acceso fuera de la clase (la excepción es si una fuera una clase anidada de la otra).

### Acceso de Paquete (_Package Access_)

Afortunadamente, `MotherDuck` es más complaciente con lo que pueden hacer sus patitos. Permite que las **clases del mismo paquete** accedan a sus miembros. **Cuando no hay modificador de acceso, Java asume acceso de paquete**.

```Java

package pond.duck;
public class MotherDuck {
    String noise = "quack";
    void quack() {
        System.out.print(noise); // el acceso de paquete es correcto
    }
}
```

`MotherDuck` puede referirse a `noise` y llamar a `quack()`. Después de todo, los miembros de la misma clase ciertamente están en el mismo paquete. La gran diferencia es que `MotherDuck` permite que **otras clases del mismo paquete** accedan a los miembros, mientras que `FatherDuck` no lo hacía (debido a que era `private`). `GoodDuckling` tiene una experiencia mucho mejor que `BadDuckling`:

```Java
package pond.duck;
public class GoodDuckling {
    public void makeNoise() {
        var duck = new MotherDuck();
        duck.quack();                 // el acceso de paquete es correcto
        System.out.print(duck.noise); // el acceso de paquete es correcto
    }
}
```

`GoodDuckling` tiene éxito al aprender a hacer `quack()` y hacer ruido copiando a su madre. Note que todas las clases cubiertas hasta ahora están en el mismo paquete, `pond.duck`. Esto permite que el acceso de paquete funcione.

En este mismo estanque, un cisne acaba de dar a luz a un cisne bebé. Un cisne bebé se llama polluelo de cisne (_cygnet_). El polluelo ve a los patitos aprendiendo a graznar y decide aprender de `MotherDuck` también.

```Java
package pond.swan;
import pond.duck.MotherDuck; // importar otro paquete

public class BadCygnet {
    public void makeNoise() {
        var duck = new MotherDuck();
        duck.quack();                 // NO COMPILA
        System.out.print(duck.noise); // NO COMPILA
    }
}
```

¡Oh, no! `MotherDuck` solo permite lecciones a otros patos al restringir el acceso al paquete `pond.duck`. El pobre `BadCygnet` está en el paquete `pond.swan`, por lo que el código no compila. Recuerde que cuando no hay modificador de acceso en un miembro, **solo las clases del mismo paquete** pueden acceder al miembro.

### Acceso Protegido (_Protected Access_)

El acceso protegido permite todo lo que hace el acceso de paquete, y más. El modificador de acceso `protected` añade la capacidad de acceder a miembros desde una **subclase**. La creación de subclases se cubrirá en profundidad en el Capítulo 6. Por ahora, se cubre el uso más simple posible de una subclase. En el siguiente ejemplo, la clase "hija" `ClownFish` es una subclase de la clase "padre" `Fish`, utilizando la palabra clave `extends` para conectarlas:


```Java
public class Fish {}
public class ClownFish extends Fish {}
```

Al extender una clase, la subclase obtiene acceso a todos los miembros `protected` y `public` de la clase padre, **como si hubieran sido declarados en la subclase**. Si las dos clases están en el mismo paquete, entonces la subclase también obtiene acceso a todos los miembros de paquete.

La siguiente imagen muestra las muchas clases que se crean en esta sección. Hay varias clases y paquetes, así que no hay que preocuparse por retenerlos todos en la memoria. Simplemente se puede revisar esta imagen a medida que se avanza.

![[Clases que se utilizan para mostrar el acceso protegido.png]]

Primero, se crea una clase `Bird` y se da acceso `protected` a sus miembros:

```Java
package pond.shore;
public class Bird {
    protected String text = "floating";
    protected void floatInWater() {
        System.out.print(text); // el acceso protected es correcto
    }
}
```

A continuación, se crea una subclase:

```Java
package pond.goose; // Diferente paquete que Bird
import pond.shore.Bird;

public class Gosling extends Bird { // Gosling es una subclase de Bird
    public void swim() {
        floatInWater();         // el acceso protected es correcto
        System.out.print(text); // el acceso protected es correcto
    }
    public static void main(String[] args) {
        new Gosling().swim();
    }
}
```

Esta es una subclase simple. Extiende la clase `Bird`. Extender significa crear una subclase que tiene acceso a cualquier miembro `protected` o `public` de la clase padre. Al ejecutar este programa se imprime `floating` dos veces: una al llamar a `floatInWater()`, y otra por la sentencia `print` en `swim()`. **Dado que `Gosling` es una subclase de `Bird`, puede acceder a estos miembros aunque esté en un paquete diferente**.

Recuerde que `protected` también otorga acceso a todo lo que hace el acceso de paquete. Esto significa que **una clase en el mismo paquete** que `Bird` puede acceder a sus miembros `protected`.

```Java
package pond.shore; // Mismo paquete que Bird
public class BirdWatcher {
    public void watchBird() {
        Bird bird = new Bird();
        bird.floatInWater();         // el acceso protected es correcto
        System.out.print(bird.text); // el acceso protected es correcto
    }
}
```

Dado que `Bird` y `BirdWatcher` están en el mismo paquete, `BirdWatcher` puede acceder a los miembros de la variable `bird`. La definición de `protected` permite el acceso a subclases **y** a clases en el mismo paquete. Este ejemplo utiliza la parte de la definición correspondiente al "mismo paquete".

Ahora se intentará lo mismo desde un paquete diferente:

```Java
package pond.inland; // Diferente paquete que Bird
import pond.shore.Bird;

public class BirdWatcherFromAfar { // No es una subclase de Bird
    public void watchBird() {
        Bird bird = new Bird();
        bird.floatInWater();         // NO COMPILA
        System.out.print(bird.text); // NO COMPILA
    }
}
```

`BirdWatcherFromAfar` no está en el mismo paquete que `Bird`, y no hereda de `Bird`. Esto significa que **no se le permite acceder** a los miembros `protected` de `Bird`.

¿Entendido? **Solo las subclases y las clases en el mismo paquete tienen permitido acceder a miembros `protected`**.

Hay una "trampa" (_gotcha_) con el acceso protegido. Considere esta clase:

```Java
1:  package pond.swan; // Diferente paquete que Bird
2:  import pond.shore.Bird;
3:  public class Swan extends Bird { // Swan es una subclase de Bird
4:      public void swim() {
5:          floatInWater();         // el acceso protected es correcto
6:          System.out.print(text); // el acceso protected es correcto
7:      }
8:      public void helpOtherSwanSwim() {
9:          Swan other = new Swan();
10:         other.floatInWater();         // acceso de subclase a superclase
11:         System.out.print(other.text); // acceso de subclase a superclase
12:     }
13:     public void helpOtherBirdSwim() {
14:         Bird other = new Bird();
15:         other.floatInWater();         // NO COMPILA
16:         System.out.print(other.text); // NO COMPILA
17:     }
18: }
```

Esto es interesante. `Swan` no está en el mismo paquete que `Bird` pero lo extiende, lo que implica que tiene acceso a los miembros `protected` de `Bird` ya que es una subclase. Y lo tiene. Las líneas 5 y 6 se refieren a miembros `protected` heredándolos.

Las líneas 10 y 11 también utilizan con éxito los miembros `protected` de `Bird`. Esto está permitido porque **estas líneas se refieren a un objeto `Swan`**. `Swan` hereda de `Bird`, por lo que esto es correcto. Es una especie de verificación en dos fases. Se permite que la clase `Swan` use miembros `protected` de `Bird`, y **se está haciendo referencia a un objeto `Swan`**. Por supuesto, es un objeto `Swan` creado en la línea 9 en lugar del propio objeto heredado directamente (`this`), pero sigue siendo un objeto `Swan`.

Las líneas 15 y 16 no compilan. ¡Un momento! ¡Son casi exactamente iguales a las líneas 10 y 11! Hay una diferencia clave. Esta vez **se usa una referencia de tipo `Bird`** en lugar del tipo de la subclase. Se crea en la línea 14. `Bird` está en un paquete diferente, y en este uso específico a través de esa referencia, el código no está accediendo a un miembro de un `Swan`, por lo que no se permite el uso de miembros `protected`. ¿Cómo es eso? Se acaba de decir repetidamente que `Swan` hereda de `Bird`. Y lo hace. Sin embargo, la referencia de la variable no es un `Swan`. El código simplemente resulta estar dentro de la clase `Swan`.

Es normal confundirse. Este es posiblemente uno de los puntos más confusos del examen. Viéndolo de otra manera, las reglas `protected` se aplican bajo dos escenarios:

1. **Un miembro se usa sin referirse a una variable** (o usando `this` implícito). Este es el caso en las líneas 5 y 6. En este caso, se está aprovechando la herencia directamente, y se permite el acceso `protected`.
2. **Un miembro se usa a través de una variable**. Este es el caso en las líneas 10, 11, 15 y 16. En este caso, **las reglas para el tipo de referencia de la variable son lo que importa**. Si el tipo de la variable es la propia subclase (o una subclase de esta), se permite el acceso `protected`.

Se intentará esto de nuevo para asegurar la comprensión. ¿Se puede averiguar por qué estos ejemplos no compilan?

```Java
package pond.goose;
import pond.shore.Bird;

public class Goose extends Bird {
    public void helpGooseSwim() {
        Goose other = new Goose();
        other.floatInWater();
        System.out.print(other.text);
    }
    public void helpOtherGooseSwim() {
        Bird other = new Goose();
        other.floatInWater();         // NO COMPILA
        System.out.print(other.text); // NO COMPILA
    }
}
```

El primer método está bien. De hecho, es equivalente al ejemplo de `Swan`. `Goose` extiende `Bird`. Dado que se está en la subclase `Goose` y se hace referencia a una variable de tipo `Goose`, puede acceder a los miembros `protected`. El segundo método es un problema. Aunque el objeto resulta ser un `Goose` en tiempo de ejecución, **se almacena en una referencia de tipo `Bird`**. No se permite referirse a miembros `protected` de la clase `Bird` a través de una referencia `Bird` desde fuera de su paquete, a pesar de que el código resida dentro de una subclase.

¿Qué pasa con este?

```Java
package pond.duck;
import pond.goose.Goose;

public class GooseWatcher {
    public void watch() {
        Goose goose = new Goose();
        goose.floatInWater(); // NO COMPILA
    }
}
```

Este código no compila porque no se está en la clase `Goose` ni en el paquete `pond.shore`. El método `floatInWater()` está declarado en `Bird`. `GooseWatcher` no está en el mismo paquete que `Bird`, ni extiende `Bird`. `Goose` sí extiende `Bird`, pero eso solo permite que la clase `Goose` se refiera a `floatInWater()`, no los usuarios de `Goose`.

### Acceso Público (_Public Access_)

El acceso protegido fue un concepto difícil. Afortunadamente, el último tipo de modificador de acceso es fácil: `public` significa que **cualquiera puede acceder al miembro desde cualquier lugar**.

> El sistema de módulos de Java (_module system_) redefine "cualquier lugar", y se hace posible restringir el acceso al código público fuera de un módulo. Esto se cubre con más detalle en el Capítulo 12, «Módulos». Cuando se den ejemplos de código, se puede asumir que están en el mismo módulo a menos que se indique explícitamente lo contrario.

Se creará una clase que tenga miembros públicos:

```Java
package pond.duck;
public class DuckTeacher {
    public String name = "helpful";
    public void swim() {
        System.out.print(name); // el acceso public es correcto
    }
}
```

`DuckTeacher` permite el acceso a cualquier clase que lo desee. Ahora se puede probar:

```Java
package pond.goose;
import pond.duck.DuckTeacher;

public class LostDuckling {
    public void swim() {
        var teacher = new DuckTeacher();
        teacher.swim();                             // permitido
        System.out.print("Thanks " + teacher.name); // permitido
    }
}
```

`LostDuckling` es capaz de referirse a `swim()` y `name` en `DuckTeacher` porque son públicos. La historia tiene un final feliz.

### Revisión de modificadores de acceso

Hay que asegurarse de saber por qué todo en la Tabla 5.4 es cierto.

**TABLA 5.4** Un método en ______ puede acceder a un miembro ______.

|**... puede acceder a un miembro:**|**private**|**paquete**|**protected**|**public**|
|---|---|---|---|---|
|**la misma clase**|Sí|Sí|Sí|Sí|
|**otra clase en el mismo paquete**|No|Sí|Sí|Sí|
|**una subclase en un paquete diferente**|No|No|Sí|Sí|
|**una clase no relacionada en un paquete diferente**|No|No|No|Sí|