En el Capítulo 1, "Bloques de Construcción", **se introdujo** la definición básica de una clase en Java. En el Capítulo 5, "Métodos", **se profundizó** en los métodos y modificadores y **se mostró** cómo **se pueden usar** para construir clases más estructuradas. En este capítulo, **se lleva** todo un paso más allá y **se muestra** cómo la estructura de clases y la **herencia** es una de las características más poderosas en el lenguaje Java.

En su núcleo, un diseño adecuado de clases en Java se trata de la **reutilización de código**, el aumento de la funcionalidad y la estandarización. Por ejemplo, al crear una nueva clase que extiende una clase existente, **se puede obtener** acceso a una gran cantidad de primitivos, objetos y métodos heredados, lo que aumenta la reutilización de código.

Este capítulo es la culminación de algunos de los temas más importantes en Java, incluyendo la herencia, el diseño de clases, los constructores, el orden de inicialización, la sobrescritura de métodos (_overriding_), las clases abstractas y los objetos inmutables. **Se debe leer** este capítulo con atención y **asegurarse** de comprender bien todos los temas. Este capítulo forma la base del Capítulo 7, "Más allá de las clases", en el cual **se amplía** la discusión sobre los tipos para incluir otros tipos de nivel superior y anidados.

### Comprendiendo la Herencia

Al crear una nueva clase en Java, **se puede definir** la clase para que herede de una clase existente. La **herencia** es el proceso mediante el cual una subclase incluye automáticamente ciertos miembros de la clase, como primitivos, objetos o métodos, definidos en la clase padre.

Para fines ilustrativos, **se hace referencia** a cualquier clase que hereda de otra clase como **subclase** o **clase hija**, ya que se considera descendiente de esa clase. Alternativamente, **se hace referencia** a la clase de la que hereda la hija como **superclase** o **clase padre**, ya que se considera un ancestro de la clase.

Al trabajar con otros tipos, como las interfaces, **se tiende a usar** los términos generales **subtipo** y **supertipo**. **Se observará** más sobre esto en el próximo capítulo.

### Declarando una Subclase

**Se comenzará** con la declaración de una clase y su subclase.La imagen a continuación muestra un ejemplo de una superclase, `Mammal`, y una subclase `Rhinoceros`.

![[Declaraciones de subclases y superclases.png]]

**Se indica** que una clase es una subclase al declararla con la palabra clave `extends`. No **se necesita** declarar nada en la superclase aparte de asegurarse de que no esté marcada como `final`. **Se detallará** más sobre esto en breve.

Un aspecto clave de la herencia es que es **transitiva**. Dadas tres clases [X, Y, Z], si X extiende de Y, e Y extiende de Z, entonces X se considera una subclase o descendiente de Z. Del mismo modo, Z es una superclase o ancestro de X. A veces **se utiliza** el término **subclase directa** o descendiente directo para indicar que la clase extiende directamente a la clase padre. Por ejemplo, X es un descendiente directo solo de la clase Y, no de Z.

En el capítulo anterior, **se aprendió** que hay cuatro niveles de acceso: `public`, `protected`, de paquete (_package-private_) y `private`. Cuando una clase hereda de una clase padre, todos los miembros `public` y `protected` están automáticamente disponibles como parte de la clase hija. Si ambas clases están en el mismo paquete, entonces los miembros de paquete están disponibles para la clase hija. Por último, pero no menos importante, los miembros `private` están restringidos a la clase en la que se definen y nunca están disponibles a través de la herencia. Esto no significa que la clase padre no pueda tener miembros `private` que puedan contener datos o modificar un objeto; simplemente significa que la subclase no tiene acceso directo a ellos.

**Se analizará** un ejemplo sencillo:

```Java
public class BigCat {
    protected double size;
}
public class Jaguar extends BigCat {
    public Jaguar() {
        size = 10.2;
    }
    public void printDetails() {
        System.out.print(size);
    }
}
public class Spider {
    public void printDetails() {
        System.out.println(size); // NO COMPILA
    }
}
```

`Jaguar` es una subclase o hija de `BigCat`, lo que hace que `BigCat` sea una superclase o padre de `Jaguar`. En la clase `Jaguar`, `size` es accesible porque está marcado como `protected`. A través de la herencia, la subclase `Jaguar` puede leer o escribir `size` como si fuera su propio miembro. Esto **se contrasta** con la clase `Spider`, que no tiene acceso a `size` ya que no es heredado.

### Modificadores de Clase

Al igual que los métodos y las variables, la declaración de una clase puede tener varios modificadores. La Tabla 6.1 enumera los modificadores que **se deben conocer** para el examen.

**TABLA 6.1 Modificadores de clase**

|**Modificador**|**Descripción**|**Capítulo cubierto**|
|---|---|---|
|`final`|La clase no puede ser extendida.|Capítulo 6|
|`abstract`|La clase es abstracta, puede contener métodos abstractos y requiere una subclase concreta para instanciarse.|Capítulo 6|
|`sealed`|La clase solo puede ser extendida por una lista específica de clases.|Capítulo 7|
|`non-sealed`|Una subclase de una clase _sealed_ permite subclases potencialmente no identificadas.|Capítulo 7|
|`static`|Se utiliza para clases anidadas estáticas definidas dentro de una clase.|Capítulo 7|

**Se cubren** las clases abstractas más adelante en este capítulo. En el próximo capítulo, **se analizan** las clases _sealed_ y _non-sealed_, así como las clases anidadas estáticas.

Por ahora, **se hablará** sobre marcar una clase como `final`. El modificador `final` evita que una clase sea extendida en lo absoluto. Por ejemplo, lo siguiente no compila:

```Java
public class Mammal {}
public final class Rhinoceros extends Mammal {}
public class Clara extends Rhinoceros {}
 // NO COMPILA
```

En el examen, **se debe prestar atención** a cualquier clase marcada como `final`. Si **se observa** que otra clase la extiende, **se sabe** de inmediato que el código no compila.

### Herencia Simple vs. Múltiple

Java soporta la **herencia simple**, mediante la cual una clase puede heredar de solo una clase padre directa. Java también soporta **múltiples niveles de herencia**, mediante los cuales una clase puede extender a otra clase, la cual a su vez extiende a otra clase. **Se puede tener** cualquier cantidad de niveles de herencia, permitiendo que cada descendiente obtenga acceso a los miembros de sus ancestros.

Para comprender realmente la herencia simple, puede ser útil contrastarla con la **herencia múltiple**, mediante la cual una clase puede tener varios padres directos. Por diseño, Java no soporta la herencia múltiple en el lenguaje porque esta puede conducir a modelos de datos complejos y a menudo difíciles de mantener. Java sí permite una excepción a la regla de herencia simple, la cual **se verá** en el Capítulo 7: una clase puede implementar múltiples interfaces.

La imagen a continuación ilustra los diversos tipos de modelos de herencia. Los elementos de la izquierda se consideran herencia simple porque cada hija tiene exactamente un padre. **Se puede notar** que la herencia simple no excluye que los padres tengan múltiples hijas. El lado derecho muestra elementos que tienen herencia múltiple. Como **se puede observar**, un objeto `Perro` tiene múltiples designaciones parentales.

![[Tipos de herencia.png]]

Parte de lo que hace que la herencia múltiple sea complicada es determinar de qué padre heredar valores en caso de un conflicto. Por ejemplo, si **se tiene** un objeto o método definido en todos los padres, ¿cuál hereda la hija? No existe un orden natural para los padres en este ejemplo, y es por eso que Java evita estos problemas al no permitir la herencia múltiple en absoluto.

### Heredando de Object

A lo largo de la discusión sobre Java en este proyecto, **se ha mencionado** la palabra objeto numerosas veces, y con buena razón. En Java, todas las clases heredan de una única clase: `java.lang.Object`, u `Object` para abreviar. Además, `Object` es la única clase que no tiene una clase padre.

**Cabe preguntarse**: "Ninguna de las clases que **se han escrito** hasta ahora extiende de `Object`, entonces, ¿cómo es que todas las clases heredan de ella?". La respuesta es que el compilador ha estado insertando código automáticamente en cualquier clase que **se escriba** y que no extienda de una clase específica. Por ejemplo, las dos siguientes son equivalentes:

```Java
public class Zoo { }
public class Zoo extends java.lang.Object { }
```

La clave es que cuando Java detecta que **se define** una clase que no extiende a otra, el compilador agrega automáticamente la sintaxis `extends java.lang.Object` a la definición de la clase. El resultado es que cada clase obtiene acceso a cualquier método accesible en la clase `Object`. Por ejemplo, los métodos `toString()` y `equals()` están disponibles en `Object`; por lo tanto, son accesibles en todas las clases. Sin embargo, si no son sobrescritos en una subclase, es posible que no sean particularmente útiles. **Se aborda** la sobrescritura de métodos más adelante en este capítulo.

Por otro lado, cuando **se define** una nueva clase que extiende una clase existente, Java no extiende automáticamente la clase `Object`. Puesto que todas las clases heredan de `Object`, extender una clase existente significa que la hija ya hereda de `Object` por definición. Si **se observa** la estructura de herencia de cualquier clase, siempre terminará con `Object` en la cima del árbol, como se muestra a continuación.

![[Herencia de objetos en Java.png]]

Los tipos primitivos como `int` y `boolean` no heredan de `Object`, ya que no son clases. Como **se aprendió** en el Capítulo 5, mediante la conversión automática (_autoboxing_) pueden ser asignados o pasados como una instancia de una clase envoltorio (_wrapper class_) asociada, la cual sí hereda de `Object`.