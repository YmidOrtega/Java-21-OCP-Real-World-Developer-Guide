En capítulos anteriores, se aprendió cómo escribir fragmentos de código sin pensar demasiado en los métodos que lo contenían. En este capítulo, se exploran los métodos en profundidad, incluyendo modificadores, argumentos, _varargs_, sobrecarga y _autoboxing_. Muchos de estos fundamentos, como los modificadores de acceso y `static`, son aplicables a clases y otros tipos a lo largo del resto del texto. Si se tienen dificultades, ¡podría ser conveniente leer este capítulo dos veces!

### Diseño de métodos

Todo programa interesante de Java que se ha visto ha tenido un método `main()`. También se pueden escribir otros métodos. Por ejemplo, se puede escribir un método básico para tomar una siesta, como se muestra a continuación.

![[Declaración de método.png]]

A esto se le llama **declaración de método**, la cual especifica toda la información necesaria para llamarlo. Hay muchas partes, y se cubrirá cada una con más detalle. Dos de las partes (el nombre del método y la lista de parámetros) se denominan **firma del método** (_method signature_). La firma del método proporciona instrucciones sobre cómo los invocadores pueden hacer referencia a este método. La firma del método no incluye el tipo de retorno ni los modificadores de acceso, los cuales controlan desde dónde se puede referenciar el método.

La Tabla 5.1 es una breve referencia a los elementos de una declaración de método. No hay que preocuparse si parece mucha información; para cuando se termine este capítulo, todo encajará.

**TABLA 5.1** Partes de una declaración de método en el ejemplo `nap()`

|**Elemento**|**Valor en el ejemplo nap()**|**¿Es obligatorio?**|
|---|---|---|
|Modificador de acceso|`public`|No|
|Especificador opcional|`final`|No|
|Tipo de retorno|`void`|**Sí**|
|Nombre del método|`nap`|**Sí**|
|Lista de parámetros|`(int minutes)`|**Sí**, pero los paréntesis pueden estar vacíos|
|Firma del método|`nap(int minutes)`|**Sí**|
|Lista de excepciones|`throws InterruptedException`|No|
|Cuerpo del método|`{ // tomar una siesta }`|**Sí**, excepto para métodos abstractos|

Para llamar a este método, simplemente se utiliza la firma del método y se proporciona un valor `int` entre paréntesis:

```Java
nap(10);
```

Se comenzará echando un vistazo a cada una de estas partes de un método básico.

### Modificadores de acceso (_Access Modifiers_)

Un modificador de acceso determina desde qué clases se puede acceder a un método. Se puede pensar en ello como un guardia de seguridad. Algunas clases son buenos amigos, otras son parientes lejanos y otras son completas extrañas. Los modificadores de acceso ayudan a imponer cuándo se permite que estos componentes se comuniquen entre sí.

Java ofrece cuatro opciones de acceso:

- **`private`**: El modificador privado significa que el método se puede llamar **solo desde dentro de la misma clase**.
- **Acceso de paquete (_Package Access_)**: Con el acceso de paquete, el método se puede llamar **solo desde una clase en el mismo paquete**. Este es engañoso porque no hay una palabra clave. Simplemente se omite el modificador de acceso. Al acceso de paquete a veces se le denomina paquete-privado (_package-private_) o acceso predeterminado (_default access_).
- **`protected`**: El modificador protegido significa que el método se puede llamar **solo desde una clase en el mismo paquete o una subclase**.
- **`public`**: El modificador público significa que el método se puede llamar **desde cualquier lugar**.

Por simplicidad, en este capítulo el enfoque principal está en los modificadores de acceso aplicados a métodos y campos. Las reglas para los modificadores de acceso también son aplicables a clases y otros tipos que se aprenden en el Capítulo 7, «Más allá de las clases», como interfaces, enumeraciones (_enums_) y registros (_records_).

El impacto de los distintos modificadores de acceso se explorará más adelante en este capítulo. Por ahora, solo se debe dominar la identificación de la sintaxis válida de los métodos. A los creadores del examen les gusta intentar engañar poniendo elementos del método en el orden incorrecto o usando valores incorrectos.

Se verán ejemplos de práctica a medida que se revisa cada uno de los elementos del método. Hay que asegurarse de comprender por qué cada una de estas es una declaración de método válida o inválida. Se debe prestar atención a los modificadores de acceso al descubrir qué está mal en los que no compilan:

```Java
public class ParkTrip {
    public void skip1() {}
    default void skip2() {} // NO COMPILA
    void public skip3() {}  // NO COMPILA
    void skip4() {}
}
```

El método `skip1()` es una declaración válida con acceso `public`. El método `skip4()` es una declaración válida con acceso de paquete. El método `skip2()` no compila porque `default` **no es un modificador de acceso válido**. Existe una palabra clave `default`, que se usa en sentencias `switch` e interfaces, pero `default` nunca se usa como un modificador de acceso. El método `skip3()` no compila porque el modificador de acceso se especifica después del tipo de retorno.

### Especificadores opcionales

Existe una serie de especificadores opcionales para métodos, mostrados en la Tabla 5.2. A diferencia de los modificadores de acceso, **se pueden tener múltiples especificadores** en el mismo método (aunque no todas las combinaciones son legales). Cuando esto sucede, se pueden especificar en cualquier orden. Y dado que estos especificadores son opcionales, está permitido no tener ninguno en absoluto. Esto significa que se pueden tener cero o más especificadores en una declaración de método.

Como se puede ver en la Tabla 5.2, cuatro de los modificadores de método se cubren en capítulos posteriores, y los dos últimos ni siquiera están dentro del alcance del examen (y rara vez se usan en la vida real). En este capítulo, el enfoque es introducir estos modificadores. Su uso a menudo requiere muchas más reglas.

**TABLA 5.2** Especificadores opcionales para métodos

|**Modificador**|**Descripción**|**Capítulo cubierto**|
|---|---|---|
|`static`|Indica que el método es miembro del objeto de clase compartido.|Capítulo 5|
|`abstract`|Se utiliza en una clase o interfaz abstracta cuando se excluye el cuerpo del método.|Capítulo 6|
|`final`|Especifica que el método no puede ser sobrescrito en una subclase.|Capítulo 6|
|`default`|Se utiliza en una interfaz para proporcionar una implementación predeterminada de un método.|Capítulo 7|
|`synchronized`|Se utiliza con código multiproceso (_multithreaded_).|Capítulo 13|
|`native`|Se utiliza al interactuar con código escrito en otro lenguaje, como C++.|Fuera de alcance|
|`strictfp`|Se utiliza para hacer portátiles los cálculos de punto flotante.|Fuera de alcance|

Si bien los modificadores de acceso y los especificadores opcionales pueden aparecer en cualquier orden, **todos deben aparecer antes del tipo de retorno**. En la práctica, es común listar primero el modificador de acceso. Como también se aprenderá en los próximos capítulos, algunos especificadores no son compatibles entre sí. Por ejemplo, no se puede declarar un método (o clase) como `final` y `abstract` al mismo tiempo.

Recuerde, los modificadores de acceso y los especificadores opcionales se pueden listar en cualquier orden, pero una vez que se especifica el tipo de retorno, el resto de las partes del método se escriben en un orden específico: **nombre, lista de parámetros, lista de excepciones, cuerpo**.

Nuevamente, por ahora el enfoque es solo la sintaxis. ¿Se logra ver por qué estos compilan o no compilan?

```Java
public class Exercise {
    public void bike1() {}
    public final void bike2() {}
    public static final void bike3() {}
    public final static void bike4() {}
    public modifier void bike5() {} // NO COMPILA
    public void final bike6() {}    // NO COMPILA
    final public void bike7() {}
}
```

El método `bike1()` es una declaración válida sin especificador opcional. Esto está bien; después de todo, es opcional. El método `bike2()` es una declaración válida, con `final` como especificador opcional. Los métodos `bike3()` y `bike4()` son declaraciones válidas con `final` y `static` como especificadores opcionales. El orden de estas dos palabras clave no importa. El método `bike5()` no compila porque `modifier` no es un especificador opcional válido. El método `bike6()` no compila porque el especificador opcional está después del tipo de retorno.

El método `bike7()` sí compila. Java permite que los especificadores opcionales aparezcan antes del modificador de acceso. Este es un caso extraño y no es uno que se necesite saber para el examen. Se menciona para evitar confusiones al practicar.

### Tipo de retorno (_Return Type_)

El siguiente elemento en una declaración de método es el tipo de retorno. **Debe aparecer después de cualquier modificador de acceso o especificador opcional y antes del nombre del método**. El tipo de retorno puede ser un tipo real de Java, como `String` o `int`. Si no hay un tipo de retorno, se utiliza la palabra clave `void`. Este tipo de retorno especial proviene del inglés: _void_ significa "vacío" o "sin contenido".

Recuerde que un método **debe tener un tipo de retorno**. Si no se devuelve ningún valor, se debe utilizar la palabra clave `void`. No se puede omitir el tipo de retorno.

Al verificar los tipos de retorno, también se debe observar dentro del cuerpo del método. Los métodos con un tipo de retorno distinto de `void` **están obligados a tener una sentencia `return`** dentro del cuerpo del método. Esta sentencia `return` debe incluir el primitivo u objeto a ser devuelto. A los métodos que tienen un tipo de retorno `void` se les permite tener una sentencia `return` sin valor devuelto o omitir la sentencia `return` por completo. Se puede pensar en una sentencia `return` en un método `void` como el método diciendo: "¡Terminé!" y saliendo antes de tiempo, tal como lo siguiente:

```Java
public void swim(int distance) {
    if(distance <= 0) {
        // Salir temprano, ¡no hay nada que hacer!
        return;
    }
    System.out.print("Fish is swimming " + distance + " meters");
}
```

¿Listo para algunos ejemplos? ¿Se puede explicar por qué estos métodos compilan o no?

```Java
public class Hike {
    public void hike1() {}
    public void hike2() { return; }
    public String hike3() { return ""; }
    public String hike4() {}              // NO COMPILA
    public hike5() {}                     // NO COMPILA
    public String int hike6() { }         // NO COMPILA
    String hike7(int a) {                 // NO COMPILA
        if (1 < 2) return "orange";
    }
}
```

Dado que el tipo de retorno del método `hike1()` es `void`, la sentencia `return` es opcional. El método `hike2()` muestra la sentencia `return` opcional que correctamente no devuelve nada. El método `hike3()` es una declaración válida con un tipo de retorno `String` y una sentencia `return` que devuelve un `String`. El método `hike4()` no compila porque falta la sentencia `return`. El método `hike5()` no compila porque falta el tipo de retorno. El método `hike6()` no compila porque intenta utilizar dos tipos de retorno. Solo se puede tener un tipo de retorno.

El método `hike7()` es un poco engañoso. Hay una sentencia `return`, pero no siempre se ejecuta. Aunque 1 siempre es menor que 2, el compilador no evaluará completamente la sentencia `if` y requerirá una sentencia `return` en caso de que esta condición sea falsa. ¿Qué pasa con esta versión modificada?

```Java
String hike8(int a) {
    if (1 < 2) return "orange";
    return "apple"; // ADVERTENCIA DEL COMPILADOR
}
```

El código compila, aunque el compilador producirá una advertencia sobre código inalcanzable (_unreachable code_). Esto significa que el compilador fue lo suficientemente inteligente como para darse cuenta de que se escribió código al que es imposible llegar.

Al devolver un valor, este debe ser asignable al tipo de retorno. ¿Se puede detectar qué está mal con dos de estos ejemplos?

```Java
public class Measurement {
    int getHeight1() {
        int temp = 9;
        return temp;
    }
    int getHeight2() {
        int temp = 9L; // NO COMPILA
        return temp;
    }
    int getHeight3() {
        long temp = 9L;
        return temp;   // NO COMPILA
    }
}
```

El método `getHeight2()` no compila porque no se puede asignar un `long` a un `int`. El método `getHeight3()` no compila porque no se puede devolver un valor `long` cuando el tipo de retorno es `int`. Si esto no resulta claro, se debería volver al Capítulo 2, «Operadores», y releer las secciones sobre tipos numéricos y _casting_.

### Nombre del método

Los nombres de los métodos siguen las mismas reglas que se practicaron con los nombres de las variables en el Capítulo 1, «Bloques de construcción». A modo de repaso, un identificador solo puede contener letras, números, símbolos de moneda o `_`. Además, no se permite que el primer carácter sea un número, y no se permiten palabras reservadas. Finalmente, no se permite el uso de un solo carácter de guion bajo (`_`).

Por convención, los métodos comienzan con una letra minúscula, pero no es obligatorio. Ya que esto es un repaso del Capítulo 1, se puede pasar directamente a practicar con algunos ejemplos:

```Java
public class BeachTrip {
    public void jog1() {}
    public void 2jog() {}       // NO COMPILA
    public jog3 void() {}       // NO COMPILA
    public void Jog_$() {}
    public _() {}               // NO COMPILA
    public void() {}            // NO COMPILA
}
```

El método `jog1()` es una declaración válida con un nombre tradicional. El método `2jog()` no compila porque no se permite que los identificadores comiencen con números. El método `jog3()` no compila porque el nombre del método está antes del tipo de retorno. El método `Jog_$()` es una declaración válida. Si bien ciertamente no es una buena práctica comenzar el nombre de un método con una letra mayúscula y terminar con puntuación, es legal. El método `_` no está permitido ya que consiste en un solo guion bajo. La línea final de código no compila porque falta el nombre del método.

### Lista de parámetros

Aunque la lista de parámetros es obligatoria, **no tiene que contener ningún parámetro**. Esto significa que simplemente se puede tener un par de paréntesis vacíos después del nombre del método, de la siguiente manera:

```Java
public class Sleep {
    void nap() {}
}
```

Si se tienen múltiples parámetros, se separan con una coma. Hay un par de reglas más para la lista de parámetros que se verán cuando se cubran los _varargs_ en breve. Por ahora, se practicará observando declaraciones de métodos con parámetros "regulares":

```Java
public class PhysicalEducation {
    public void run1() {}
    public void run2 {}               // NO COMPILA
    public void run3(int a) {}
    public void run4(int a; int b) {} // NO COMPILA
    public void run5(int a, int b) {}
}
```

El método `run1()` es una declaración válida sin ningún parámetro. El método `run2()` no compila porque faltan los paréntesis alrededor de la lista de parámetros. El método `run3()` es una declaración válida con un parámetro. El método `run4()` no compila porque los parámetros están separados por un punto y coma en lugar de una coma. Los puntos y coma son para separar sentencias, no para listas de parámetros. El método `run5()` es una declaración válida con dos parámetros.

### Firma del método (_Method Signature_)

Una firma de método, compuesta por el **nombre del método y la lista de parámetros**, es lo que Java utiliza para determinar de manera única exactamente qué método se está intentando llamar.

Una vez que determina qué método se intenta llamar, entonces determina si la llamada está permitida. Por ejemplo, intentar acceder a un método privado fuera de la clase o asignar el valor de retorno de un método `void` a una variable `int` da como resultado errores del compilador. Sin embargo, ninguno de estos errores del compilador está relacionado con la firma del método.

Es importante notar que los **nombres** de los parámetros en la firma del método no se utilizan como parte de una firma de método. La lista de parámetros se refiere a los **tipos** de parámetros y su **orden**. Por ejemplo, los dos métodos siguientes tienen exactamente la misma firma:

```Java
// NO COMPILA
public class Trip {
    public void visitZoo(String name, int waitTime) {}
    public void visitZoo(String attraction, int rainFall) {}
}
```

A pesar de tener diferentes nombres de parámetros, estos dos métodos tienen la misma firma (`visitZoo(String, int)`) y no pueden ser declarados dentro de la misma clase. Sin embargo, cambiar el orden de los tipos de parámetros sí permite que el método compile:

```Java
public class Trip {
    public void visitZoo(String name, int waitTime) {}
    public void visitZoo(int rainFall, String attraction) {}
}
```

Estas reglas se cubrirán con más detalle al llegar a la sobrecarga de métodos más adelante en este capítulo.

### Lista de excepciones

En Java, el código puede indicar que algo salió mal lanzando una excepción. Esto se cubre en el Capítulo 11, «Excepciones y localización». Por ahora, solo se necesita saber que es opcional y en qué parte de la declaración del método se coloca si está presente. Por ejemplo, `InterruptedException` es un tipo de `Exception`. Se pueden listar tantos tipos de excepciones como se desee en esta cláusula, separados por comas. Aquí hay un ejemplo:

```Java
public class ZooMonorail {
    public void zeroExceptions() {}
    public void oneException() throws IllegalArgumentException {}
    public void twoExceptions() throws IllegalArgumentException, InterruptedException {}
}
```

Si bien la lista de excepciones es opcional, puede ser requerida por el compilador, dependiendo de lo que aparezca dentro del cuerpo del método. Se aprenderá más sobre esto, así como sobre cómo los métodos que los llaman pueden verse obligados a manejar estas declaraciones de excepciones, en el Capítulo 11.

### Cuerpo del método

La parte final de una declaración de método es el cuerpo del método. Un cuerpo de método es simplemente un bloque de código. Tiene llaves que contienen cero o más sentencias de Java. A estas alturas ya se han dedicado varios capítulos a observar sentencias de Java, por lo que debería resultar fácil averiguar por qué las siguientes compilan o no:

```Java
public class Bird {
    public void fly1() {}
    public void fly2()                        // NO COMPILA
    public void fly3(int a) { int name = 5; }
}
```

El método `fly1()` es una declaración válida con un cuerpo de método vacío. El método `fly2()` no compila porque le faltan las llaves alrededor del cuerpo del método vacío. Los métodos están obligados a tener un cuerpo a menos que se declaren como `abstract`. Los métodos abstractos se cubrirán en el Capítulo 6, «Diseño de clases». El método `fly3()` es una declaración válida con una sentencia en el cuerpo del método.

Con esto se han superado los aspectos básicos para identificar declaraciones de métodos correctas e incorrectas. Ahora se puede profundizar en más detalles.