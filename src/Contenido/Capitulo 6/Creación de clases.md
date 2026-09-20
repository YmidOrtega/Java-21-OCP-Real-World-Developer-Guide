Ahora que **se ha establecido** cómo funciona la herencia en Java, **se puede utilizar** para definir y crear relaciones de clases complejas. En esta sección, **se repasan** los conceptos básicos para crear y trabajar con clases.

### Extendiendo una Clase

**Se crearán** dos archivos en el mismo paquete, `Animal.java` y `Lion.java`.

```Java
// Animal.java
public class Animal {
    private int age;
    protected String name;
    public int getAge() {
        return age;
    }
    public void setAge(int newAge) {
        age = newAge;
    }
}
// Lion.java
public class Lion extends Animal {
    protected void setProperties(int age, String n) {
        setAge(age);
        name = n;
    }
    public void roar() {
        System.out.print(name + ", age " + getAge() + ", says: Roar!");
    }
    public static void main(String[] args) {
        var lion = new Lion();
        lion.setProperties(3, "kion");
        lion.roar();
    }
}
```

¡**Se sabe** que hay mucho sucediendo aquí! La variable `age` existe en la clase padre `Animal` y no es directamente accesible en la clase hija `Lion`. Es accesible indirectamente a través del método `setAge()`. La variable `name` es `protected`, por lo que se hereda en la clase `Lion` y es directamente accesible. **Se crea** la instancia de `Lion` en el método `main()` y **se usa** `setProperties()` para establecer las variables de instancia. Finalmente, **se llama** al método `roar()`, el cual imprime lo siguiente:

`kion, age 3, says: Roar!`

**Se analizarán** los miembros de la clase `Lion`. La variable de instancia `age` está marcada como `private` y no es directamente accesible desde la subclase `Lion`. Por lo tanto, lo siguiente no compilaría:

```Java
public class Lion extends Animal {
    public void roar() {
        System.out.print("Lion's age: " + age); // NO COMPILA
    }
}
```

**Se debe recordar**, al trabajar con subclases, los miembros `private` nunca se heredan, y los miembros de paquete (_package-private_) solo se heredan si las dos clases están en el mismo paquete. Si **se necesita** un repaso sobre los modificadores de acceso, puede ser útil volver a leer el Capítulo 5.

### Aplicando Modificadores de Acceso a Clases

Al igual que con las variables y los métodos, **se pueden aplicar** modificadores de acceso a las clases. Como **se podría recordar** del Capítulo 1, una **clase de nivel superior** (_top-level class_) es aquella que no está definida dentro de otra clase. También **se debe recordar** que un archivo `.java` puede tener como máximo una clase de nivel superior `public`.

Aunque solo **se puede tener** una clase de nivel superior `public`, **se pueden tener** tantas clases (en cualquier orden) con acceso de paquete (_package access_) como **se desee**. De hecho, ¡ni siquiera **se necesita** declarar una clase `public`! Lo siguiente declara tres clases, cada una con acceso de paquete:

```Java
// Bear.java
class Bird {}
class Bear {}
class Fish {}
```

Sin embargo, intentar declarar una clase de nivel superior como `protected` o `private` provocará un error de compilación.

```Java
// ClownFish.java
protected class ClownFish{} // NO COMPILA
// BlueTang.java
private class BlueTang {} // NO COMPILA
```

¿Significa eso que una clase nunca puede ser declarada `protected` o `private`? No exactamente. En el Capítulo 7, **se presentan** los tipos anidados y **se muestra** que cuando **se define** una clase dentro de otra, esta puede usar cualquier modificador de acceso.

### Accediendo a la Referencia this

¿Qué sucede cuando un parámetro de método tiene el mismo nombre que una variable de instancia existente? **Se observará** un ejemplo. ¿Qué **se cree** que imprime el siguiente programa?

```Java
public class Flamingo {
    private String color = null;
    public void setColor(String color) {
        color = color;
    }
    public static void main(String... unused) {
        var f = new Flamingo();
        f.setColor("PINK");
        System.out.print(f.color);
    }
}
```

Si **se respondió** `null`, entonces **se estaría** en lo correcto. Java utiliza el alcance (_scope_) más granular, por lo que cuando observa `color = color`, asume que **se está asignando** el valor del parámetro del método a sí mismo (no a la variable de instancia). La asignación se completa exitosamente dentro del método, pero el valor de la variable de instancia `color` nunca se modifica y es `null` cuando se imprime en el método `main()`.

La solución cuando **se tiene** una variable local con el mismo nombre que una variable de instancia es usar la referencia o palabra clave `this`. La **referencia `this`** hace alusión a la instancia actual de la clase y **se puede usar** para acceder a cualquier miembro de la clase, incluyendo los miembros heredados. **Se puede utilizar** en cualquier método de instancia, constructor o bloque inicializador de instancia. No **se puede usar** cuando no hay una instancia implícita de la clase, como en un método `static` o un bloque inicializador `static`. **Se aplica** `this` a la implementación del método anterior de la siguiente manera:

```Java
public void setColor(String color) {
    this.color = color; // Establece la variable de instancia con el parámetro del método
}
```

El código corregido ahora imprimirá `PINK` como se esperaba. En muchos casos, la referencia `this` es opcional. Si Java encuentra una variable o método que no puede localizar, verificará la jerarquía de clases para ver si está disponible.

Ahora **se analizarán** algunos ejemplos que no son comunes pero que **se podrían ver** en el examen:

```Java
1: public class Duck {
2: 
3:      private String color;
4:      private int height;
5:      private int length;
6: 
7:      public void setData(int length, int theHeight) {
8:          length = this.length; // Al revés -- ¡incorrecto!
9:          height = theHeight;   // Bien, porque es un nombre diferente
10:         this.color = "white"; // Bien, pero la referencia this. no es necesaria
11:     }
12: 
13:     public static void main(String[] args) {
14:         Duck b = new Duck();
15:         b.setData(1,2);
16:         System.out.print(b.length + " " + b.height + " " + b.color);
17:     } 
18: }
```

Este código compila e imprime lo siguiente:

`0 2 white`

Sin embargo, esto podría no ser lo que **se esperaba**. La línea 8 es incorrecta, y **se debe tener cuidado** con esto en el examen. La variable de instancia `length` comienza con un valor de 0. Ese 0 se asigna al parámetro del método `length`. La variable de instancia se mantiene en 0. La línea 9 es más directa. El parámetro `theHeight` y la variable de instancia `height` tienen nombres diferentes. Dado que no hay colisión de nombres, `this` no es requerido. Finalmente, la línea 10 muestra que se permite que una asignación de variable use la referencia `this` incluso cuando no hay duplicación de nombres de variables.

### Llamando a la Referencia super

En Java, una variable o método puede definirse tanto en una **clase padre** como en una **clase hija**. Esto significa que la instancia del objeto contiene en realidad **dos copias de la misma variable** con el mismo nombre subyacente. Cuando esto ocurre, ¿cómo se referencia la versión de la **clase padre** en lugar de la clase actual?

Se analizará el siguiente ejemplo:

```Java
// Reptile.java
1: public class Reptile {
2:      protected int speed = 10;
3: }

// Crocodile.java
1: public class Crocodile extends Reptile {
2:      protected int speed = 20;
3:      public int getSpeed() {
4:          return speed;
5:      }
6:      public static void main(String[] data) {
7:          var croc = new Crocodile();
8:          System.out.println(croc.getSpeed()); // 20
9:      } 
10: }
```

Uno de los aspectos más importantes a recordar sobre este código es que una instancia de `Crocodile` almacena **dos valores separados para** `speed`: uno a nivel de `Reptile` y otro a nivel de `Crocodile`. En la línea 4, Java primero verifica si existe una variable local o parámetro de método llamado `speed`. Como no existe, luego verifica `this.speed`; y como sí existe, el programa imprime `20`.

Declarar una variable con el mismo nombre que una variable heredada se denomina **ocultamiento de variable** (_variable hiding_) y se analiza más adelante en este capítulo.

¿Pero qué ocurre si se desea que el programa imprima el valor de la clase `Reptile`? Dentro de la clase `Crocodile`, se puede acceder al valor padre de `speed` utilizando la referencia o palabra clave `super`. La referencia `super` es similar a la referencia `this`, con la diferencia de que **excluye cualquier miembro encontrado en la clase actual**. En otras palabras, el miembro debe ser accesible mediante herencia.

```Java
3:      public int getSpeed() {
4:          return super.speed; // Hace que el programa ahora imprima 10
5:      }
```

**Se comprobará** si **se ha captado** el uso de `this` y `super`. ¿Cuál es la salida del siguiente programa?

```Java
1: class Insect {
2:      protected int numberOfLegs = 4;
3:      String label = "buggy";
4: }
5: 
6: public class Beetle extends Insect {
7:      protected int numberOfLegs = 6;
8:      short age = 3;
9:      public void printData() {
10:         System.out.println(this.label);
11:         System.out.println(super.label);
12:         System.out.println(this.age);
13:         System.out.println(super.age);
14:         System.out.println(numberOfLegs);
15:     }
16:     public static void main(String []n) {
17:         new Beetle().printData();
18:     }
19: }
```

Esto era una pregunta trampa — **¡este código no compila!** Se revisará cada línea del método `printData()`:

- **Líneas 10 y 11** — La variable `label` está definida en la clase padre, por lo que es accesible mediante las referencias `this` y `super`. Ambas líneas compilarían e imprimirían `buggy`.
- **Línea 12** — La variable `age` está definida **únicamente en la clase actual**, lo que la hace accesible mediante `this`. Esta línea compila e imprimiría `3`.
- **Línea 13** — `age` **no es accesible mediante `super`**, ya que `super` solo incluye miembros heredados. **Esta línea no compila.**

> **Regla clave:** Mientras que `this` incluye miembros actuales _e_ heredados, **`super` solo incluye miembros heredados**.

Si la línea 13 estuviera comentada, ¿qué imprimiría `numberOfLegs`? Aunque ambas variables `numberOfLegs` son accesibles en `Beetle`, **Java resuelve nombres desde el ámbito más estrecho hacia afuera**. Por esta razón, se utiliza el valor de `numberOfLegs` de la clase `Beetle`, imprimiendo **`6`**.

En este ejemplo, `this.numberOfLegs` y `super.numberOfLegs` hacen referencia a **variables distintas con valores diferentes**.

Dado que `this` ya incluye los miembros heredados, **`super` se utiliza principalmente cuando existe un conflicto de nombres por herencia** — es decir, cuando un método o variable de la clase actual coincide con uno de una clase padre. Esto surge con frecuencia en la **sobreescritura de métodos** (_method overriding_) y el **ocultamiento de variables** (_variable hiding_), temas que se abordarán más adelante en este capítulo.

Se recomienda asegurarse de comprender bien el último ejemplo antes de continuar, ya que `this` y `super` se utilizan con frecuencia en las secciones siguientes.