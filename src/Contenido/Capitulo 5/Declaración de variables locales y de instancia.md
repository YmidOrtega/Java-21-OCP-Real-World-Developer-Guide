Ahora que tenemos métodos, necesitamos hablar un poco sobre las variables que pueden crear o usar. Como recordarás del Capítulo 1, las variables locales son aquellas definidas dentro de un método o bloque, mientras que las variables de instancia son aquellas definidas como miembro de una clase. Echemos un vistazo a un ejemplo:

```Java
public class Lion {
    int hunger = 4;
    public int feedZooAnimals() {
        int snack = 10; // Variable local
        if (snack > 4) {
            long dinnerTime = snack++;
            hunger--;
        }
        return snack;
    }
}
```

En la clase `Lion`, `snack` y `dinnerTime` son **variables locales** accesibles solo dentro de sus respectivos bloques de código, mientras que `hunger` es una **variable de instancia** y se crea en cada objeto de la clase `Lion`.

El objeto o valor devuelto por un método puede estar disponible fuera del método, pero la referencia a la variable `snack` desaparece. Mantén esto en mente mientras lees este capítulo: todas las referencias a variables locales se destruyen después de que se ejecuta el bloque, pero los objetos a los que apuntan aún pueden ser accesibles.

### Modificadores de variables locales

Solo hay un modificador que se puede aplicar a una variable local: `final`. Fácil de recordar, ¿verdad? Al escribir métodos, los desarrolladores pueden querer crear una variable que no cambie durante el transcurso del método. En este fragmento de código, intentar cambiar el valor o el objeto al que hacen referencia estas variables da como resultado un error del compilador:

```Java
public void zooAnimalCheckup(boolean isWeekend) {
    final int rest;
    if (isWeekend) rest = 5; else rest = 20;
    System.out.print(rest);
    
    final var giraffe = new Animal();
    final int[] friends = new int[5];
    
    rest = 10;              // NO COMPILA
    giraffe = new Animal(); // NO COMPILA
    friends = null;         // NO COMPILA
}
```

Como se muestra con la variable `rest`, **no necesitamos asignar un valor cuando se declara una variable `final`**. La regla es solo que se le debe asignar un valor antes de que pueda ser utilizada. Incluso podemos usar `var` y `final` juntos. Contrasta esto con el siguiente ejemplo:

```Java
public void zooAnimalCheckup(boolean isWeekend) {
    final int rest;
    if (isWeekend) rest = 5;
    System.out.print(rest); // NO COMPILA
}
```

Es posible que no se le haya asignado un valor a la variable `rest`, por ejemplo, si `isWeekend` es `false`. Dado que el compilador no permite el uso de variables locales a las que no se les haya asignado un valor, el código no compila.

¿Usar el modificador `final` significa que no podemos modificar los datos? No. El atributo `final` se refiere **solo a la referencia** de la variable; el contenido se puede modificar libremente (asumiendo que el objeto no sea inmutable).

```Java
public void zooAnimalCheckup() {
    final int rest = 5;
    final Animal giraffe = new Animal();
    final int[] friends = new int[5];
    
    giraffe.setName("George"); // Válido
    friends[2] = 2;            // Válido
}
```

La variable `rest` es un primitivo, por lo que es solo un valor que no se puede modificar. Por otro lado, el contenido de las variables `giraffe` y `friends` se puede modificar libremente, siempre y cuando las variables en sí no sean reasignadas a nuevos objetos.

Aunque pueda no parecer obvio, marcar una variable local como `final` es a menudo una buena práctica. Por ejemplo, puedes tener un método complejo en el que se hace referencia a una variable docenas de veces. Sería realmente malo si alguien viniera y reasignara la variable a la mitad del método. ¡Usar el atributo `final` es como enviar un mensaje a otros desarrolladores para que dejen la variable en paz!

### Variables efectivamente finales (_Effectively Final_)

Una variable local **efectivamente final** es aquella que no se modifica después de ser asignada. Esto significa que el valor de una variable no cambia después de establecerse, independientemente de si está marcada explícitamente como `final` o no. Si no estás seguro de si una variable local es efectivamente final, simplemente agrégale la palabra clave `final`. Si el código aún compila, la variable es efectivamente final.

Dada esta definición, ¿cuáles de las siguientes variables son efectivamente finales?

```Java
11: public String zooFriends() {
12:     String name = "Harry the Hippo";
13:     var size = 10;
14:     boolean wet;
15:     if(size > 100) size++;
16:     name.substring(0);
17:     wet = true;
18:     return name;
19: }
```

Recuerda, una prueba rápida para saber si es efectivamente final es simplemente agregar `final` a la declaración de la variable y ver si aún compila. En este ejemplo, `name` y `wet` son efectivamente finales y se pueden actualizar con el modificador `final`, pero no `size`.

- A la variable `name` se le asigna un valor en la línea 12 y no se reasigna. La línea 16 crea un valor que nunca se usa (recuerda del Capítulo 4 que las cadenas son inmutables).
- La variable `size` no es efectivamente final porque podría ser incrementada en la línea 15 (`size++` modifica la variable).
- La variable `wet` se le asigna un valor solo una vez en la línea 17 y no se modifica posteriormente.

#### Parámetros efectivamente finales

Recuerda del Capítulo 1 que los parámetros de los métodos y constructores son variables locales que han sido pre-inicializadas. En el contexto de las variables locales, se aplican las mismas reglas en torno a `final` y efectivamente `final`. Esto es especialmente importante en el Capítulo 7 y el Capítulo 8, «Lambdas e Interfaces Funcionales», ya que las clases locales y las expresiones lambda declaradas dentro de un método **solo pueden hacer referencia a variables locales que sean finales o efectivamente finales**.

### Modificadores de variables de instancia

Al igual que los métodos, las variables de instancia pueden tener diferentes niveles de acceso, como `private`, de paquete, `protected` y `public`. Recuerda, el acceso de paquete se indica por la falta de modificadores. Cubriremos cada uno de los diferentes modificadores de acceso en breve en este capítulo. Las variables de instancia también pueden usar especificadores opcionales, descritos en la Tabla 5.3.

**TABLA 5.3** Especificadores opcionales para variables de instancia

|**Modificador**|**Descripción**|**Capítulo cubierto**|
|---|---|---|
|`final`|Especifica que la variable de instancia debe inicializarse con cada instancia de la clase exactamente una vez.|Capítulo 5|
|`volatile`|Indica a la JVM que el valor en esta variable puede ser modificado por otros hilos.|Capítulo 13|
|`transient`|Se utiliza para indicar que una variable de instancia no debe ser serializada junto con la clase.|Capítulo 14|

¡Parece que solo necesitamos discutir `final` en este capítulo! Si una variable de instancia está marcada como `final`, entonces **se le debe asignar un valor** cuando se declara o cuando se instancia el objeto. Sin embargo, al igual que una variable `final` local, no se le puede asignar un valor más de una vez. La siguiente clase `PolarBear` demuestra estas propiedades:

```Java
public class PolarBear {
    final int age = 10;
    final int fishEaten;
    final String name;
    
    { fishEaten = 10; } // Bloque inicializador de instancia
    
    public PolarBear() {
        name = "Robert"; // Constructor
    }
}
```

A la variable `age` se le da un valor cuando se declara, mientras que a la variable `fishEaten` se le asigna un valor en un inicializador de instancia. A la variable `name` se le da un valor en el constructor sin argumentos. **No inicializar una variable de instancia `final` (o asignarle un valor más de una vez) conducirá a un error del compilador.** Hablaremos sobre la inicialización de variables `final` con más detalle cuando cubramos los constructores en el próximo capítulo.

En el Capítulo 1, mostramos que las variables de instancia reciben valores predeterminados basados en su tipo cuando no se establecen. Por ejemplo, un `int` recibe un valor predeterminado de 0, mientras que una referencia a un objeto recibe un valor predeterminado de `null`. Sin embargo, el compilador **no** aplica un valor predeterminado a las variables `final`. Una variable `final` de instancia o estática `final` debe recibir un valor cuando se declara o como parte de la inicialización de forma explícita.