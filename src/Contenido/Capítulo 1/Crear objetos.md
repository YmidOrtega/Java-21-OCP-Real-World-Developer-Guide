Los programas no podrían realizar ninguna tarea útil si no se tuviera la capacidad de crear nuevos objetos. Cabe recordar que un objeto es una instancia de una clase. En las siguientes secciones, se analizan los constructores, los campos de objeto, los inicializadores de instancia y el orden en que se inicializan los valores.

### Invocación de constructores

Para crear una instancia de una clase, solo es necesario escribir `new` antes del nombre de la clase y añadir paréntesis después. He aquí un ejemplo:

``` java
Park p = new Park();
```

Primero se declara el tipo que se creará (`Park`) y se le asigna un nombre a la variable (`p`). Esto proporciona a Java un lugar para almacenar una referencia al objeto. Luego se escribe `new Park()` para crear el objeto propiamente dicho. `Park()` parece un método, dado que va seguido de paréntesis. Se denomina **constructor**, el cual es un tipo especial de método que crea un nuevo objeto.

Ahora es el momento de definir un constructor propio.

``` java
public class Chick {
    public Chick() {
        System.out.println("in constructor");
    }
}
```

Existen dos puntos clave a tener en cuenta sobre el constructor: el nombre del constructor coincide con el nombre de la clase y no tiene tipo de retorno. Es posible encontrar un método como este en el examen:

``` java
public class Chick {
    public void Chick() { } // NO ES UN CONSTRUCTOR
}
```

Cuando se observe un nombre de método que comience con una letra mayúscula y tenga un tipo de retorno, se debe prestar especial atención. No se trata de un constructor, ya que existe un tipo de retorno. Es un método regular que compila, pero no será invocado al escribir `new Chick()`.

El propósito de un constructor es inicializar campos, aunque se puede colocar cualquier código en su interior. Otra forma de inicializar campos es hacerlo directamente en la línea en la que se declaran. Este ejemplo muestra ambos enfoques:

``` java
public class Chicken {
    int numEggs = 12; // inicialización en línea
    String name;
    public Chicken() {
        name = "Duke"; // inicialización en el constructor
    }
}
```

Para la mayoría de las clases, no es necesario codificar un constructor; el compilador proporcionará un constructor predeterminado que «no hace nada». Existen algunos escenarios que sí requieren declarar un constructor. Se aprenderá todo sobre ellos en el capítulo 6.
### Lectura y escritura de campos miembro

Es posible leer y escribir variables de instancia directamente desde el invocador. En este ejemplo, una madre cisne pone huevos:

``` java
public class Swan {
    int numberEggs; // variable de instancia
    public static void main(String[] args) {
        Swan mother = new Swan();
        mother.numberEggs = 1; // establecer variable
        System.out.println(mother.numberEggs); // leer variable
    }
}
```

El «invocador» en este caso es el método `main()`, el cual podría estar en la misma clase o en otra. Esta clase establece `numberEggs` en 1 y luego lee `numberEggs` directamente para su impresión.

Incluso se pueden leer los valores de campos ya inicializados en una línea que inicializa un nuevo campo.

``` java
1: public class Name {
2:      String first = "Theodore";
3:      String last = "Moose";
4:      String full = first + last;
5: }
```

Tanto la línea 2 como la 3 escriben en campos. La línea 4 lee y escribe datos. Se leen los campos `first` y `last`. A continuación, se escribe el campo `full`.

### Ejecución de bloques de inicialización de instancia

Al aprender sobre métodos, se observaron las llaves (`{}`). El código entre las llaves (a veces denominado «dentro de las llaves») se llama bloque de código. Cualquier lugar donde se vean llaves constituye un bloque de código.

A veces, los bloques de código se encuentran dentro de un método. Estos se ejecutan cuando se invoca el método. En otras ocasiones, los bloques de código aparecen fuera de un método. Estos se denominan inicializadores de instancia. En el capítulo 6, se aprenderá a utilizar un inicializador estático.

¿Cuántos bloques se observan en el siguiente ejemplo? ¿Cuántos inicializadores de instancia se distinguen?

``` java
1: public class Bird {
2:      public static void main(String[] args) {
3:          { System.out.println("Feathers"); }
4:      }
5:      { System.out.println("Snowy"); }
6: }
```

Existen cuatro bloques de código en este ejemplo: una definición de clase, una declaración de método, un bloque interno y un inicializador de instancia. Contar bloques de código es sencillo: simplemente se cuenta el número de pares de llaves. Si no hay el mismo número de llaves de apertura (`{`) y de cierre (`}`), o si no están definidas en el orden adecuado, el código no compila. Por ejemplo, no se puede utilizar una llave de cierre (`}`) si no existe una llave de apertura (`{`) correspondiente escrita anteriormente en el código. En programación, esto se conoce como el problema de paréntesis balanceados, y a menudo surge en preguntas de entrevistas de trabajo.

Al contar inicializadores de instancia, se debe tener en cuenta que no pueden existir dentro de un método. La línea 5 es un inicializador de instancia, con sus llaves fuera de un método. Por otro lado, la línea 3 no es un inicializador de instancia, ya que se invoca solo cuando se ejecuta el método `main()`. Existe un conjunto adicional de llaves en las líneas 1 y 6 que constituyen la declaración de la clase.
### Seguimiento del orden de inicialización

Al escribir código que inicializa campos en múltiples lugares, es necesario realizar un seguimiento del orden de inicialización. Este es simplemente el orden en el que se invocan los diferentes métodos, constructores o bloques al crear una instancia de la clase. Se añadirán más reglas al orden de inicialización en el capítulo 6. Mientras tanto, es necesario recordar lo siguiente:

- Los campos y los bloques de inicialización de instancia se ejecutan en el orden en que aparecen en el archivo.
- El constructor se ejecuta después de que se hayan ejecutado todos los campos y bloques de inicialización de instancia. 

Se analizará un ejemplo:

``` java
1:  public class Chick {
2:       private String name = "Fluffy";
3:       { System.out.println("setting field"); }
4:       public Chick() {
5:           name = "Tiny";
6:           System.out.println("setting constructor");
7:       }
8:       public static void main(String[] args) {
9:           Chick chick = new Chick();
10:          System.out.println(chick.name); 
11:      } 
12: }
```

La ejecución de este ejemplo imprime lo siguiente:

``` Plaintext
setting field
setting constructor
Tiny
```

Se analizará lo que sucede aquí. Se comienza con el método `main()` porque es donde Java inicia la ejecución. En la línea 9, se invoca al constructor de `Chick`. Java crea un nuevo objeto. Primero, se inicializa `name` con "Fluffy" en la línea 2. A continuación, se ejecuta la instrucción `println()` en el inicializador de instancia en la línea 3. Una vez que se han ejecutado todos los campos e inicializadores de instancia, Java regresa al constructor. La línea 5 cambia el valor de `name` a "Tiny", y la línea 6 imprime otra instrucción. En este punto, el constructor finaliza y la ejecución regresa a la instrucción `println()` en la línea 10.

![[Orden de ejecucion.gif]]

El orden es importante para los campos y bloques de código. No es posible hacer referencia a una variable antes de que haya sido definida.

```java
{ System.out.println(name); } // NO COMPILA
private String name = "Fluffy";
```

Se debe esperar encontrar una pregunta sobre inicialización en el examen. Se probará con un ejemplo más. ¿Qué imprimirá este código?

```java 
public class Egg {
    public Egg() {
        number = 5;
    }
    public static void main(String[] args) {
        Egg egg = new Egg();
        System.out.println(egg.number);
    }
    private int number = 3;
    { number = 4; } 
}
```

Si la respuesta fue 5, es correcta. Los campos y bloques se ejecutan primero en orden, estableciendo `number` en 3 y luego en 4. Luego se ejecuta el constructor, estableciendo `number` en 5. Se verán muchas más reglas y ejemplos que cubren el orden de inicialización en el capítulo 6. Aquí solo se cubren los conceptos básicos para poder seguir el orden de inicialización en programas sencillos.
