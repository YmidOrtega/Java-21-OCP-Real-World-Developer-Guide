Cuando la palabra clave `static` se aplica a una variable, método o clase, **pertenece a la clase  en sí, y no a una instancia concreta de la clase** de la clase. En esta sección, se verá que la palabra clave `static` también se puede aplicar a las sentencias `import`.

### Diseño de métodos y variables estáticas

A excepción del método `main()`, hasta ahora se han estado observando métodos de instancia. Los métodos y variables declarados como `static` **no requieren una instancia de la clase**. Se comparten entre todos los usuarios de la clase. Por ejemplo, observe la siguiente clase `Penguin`:

```Java
public class Penguin {
    String name;
    static String nameOfTallestPenguin;
}
```

En esta clase, cada instancia de `Penguin` tiene su propio `name` (nombre) como Willy o Lilly, pero solo hay un pingüino entre todas las instancias que es el más alto (`nameOfTallestPenguin`). Se puede pensar en una variable estática como un miembro del único objeto de clase que existe independientemente de cualquier instancia de esa clase. Considere el siguiente ejemplo:

```Java
public static void main(String[] unused) {
    var p1 = new Penguin();
    p1.name = "Lilly";
    p1.nameOfTallestPenguin = "Lilly";
    
    var p2 = new Penguin();
    p2.name = "Willy";
    p2.nameOfTallestPenguin = "Willy";
    
    System.out.println(p1.name);                 // Lilly
    System.out.println(p1.nameOfTallestPenguin); // Willy
    System.out.println(p2.name);                 // Willy
    System.out.println(p2.nameOfTallestPenguin); // Willy
}
```

Se observa que cada instancia de pingüino se actualiza con su propio nombre único. Sin embargo, el campo `nameOfTallestPenguin` es estático y, por lo tanto, compartido, por lo que cada vez que se actualiza, impacta a todas las instancias de la clase.

Se ha visto un método estático desde el Capítulo 1. El método `main()` es un método estático. Eso significa que se puede llamar utilizando el nombre de la clase:

```Java
public class Koala {
    public static int count = 0; // variable estática
    public static void main(String[] args) { // método estático
        System.out.print(count);
    }
}
```

Aquí la JVM básicamente llama a `Koala.main()` para que el programa comience. Esto también se puede hacer manualmente. Se puede tener un `KoalaTester` que no haga nada más que llamar al método `main()`:

```Java
public class KoalaTester {
    public static void main(String[] args) {
        Koala.main(new String[0]); // llama al método estático
    }
}
```

Es una forma bastante complicada de imprimir 0, ¿verdad? Cuando se ejecuta `KoalaTester`, hace una llamada al método `main()` de `Koala`, que imprime el valor de `count`. El propósito de todos estos ejemplos es mostrar que `main()` se puede llamar igual que cualquier otro método estático.

Además de los métodos `main()`, los métodos estáticos tienen dos propósitos principales:

1. Para métodos de utilidad o ayuda que no requieren ningún estado del objeto. Dado que no hay necesidad de acceder a las variables de instancia, tener métodos estáticos elimina la necesidad de que el invocador instancie un objeto solo para llamar al método.
 
2. Para un estado que es compartido por todas las instancias de una clase, como un contador. Todas las instancias deben compartir el mismo estado. Los métodos que simplemente usan ese estado también deberían ser estáticos.

### Acceso a una variable o método estático

Por lo general, acceder a un miembro estático es fácil.

```Java
public class Snake {
    public static long hiss = 2;
}
```

Simplemente se coloca el nombre de la clase antes del método o variable, y listo. Aquí hay un ejemplo:

```Java
System.out.println(Snake.hiss);
```

Fácil y sencillo. Hay una regla que es más engañosa. **Se puede utilizar una instancia del objeto para llamar a un método estático**. El compilador comprueba el tipo de la referencia y lo utiliza en lugar del objeto, lo cual es un truco sutil de Java. Este código es perfectamente legal:

```Java
5: Snake s = new Snake();
6: System.out.println(s.hiss); // s es una Snake
7: s = null;
8: System.out.println(s.hiss); // s sigue siendo una Snake
```

Aunque cueste creerlo, este código emite `2` dos veces. La línea 6 ve que `s` es de tipo `Snake` y `hiss` es una variable estática, por lo que lee esa variable estática basándose en el tipo de referencia. La línea 8 hace lo mismo. A Java no le importa que `s` resulte ser `null` en tiempo de ejecución. Dado que se está buscando una variable estática, la instancia real no importa.

> Se debe recordar observar el **tipo de referencia** de una variable cuando se vea un método o variable estática. Los creadores del examen intentarán hacer creer que se lanza una `NullPointerException` porque la variable es `null`. ¡No hay que dejarse engañar!

Una vez más, porque esto es realmente importante: ¿qué emite lo siguiente?

```Java
Snake.hiss = 4;
Snake snake1 = new Snake();
Snake snake2 = new Snake();
snake1.hiss = 6;
snake2.hiss = 5;
System.out.println(Snake.hiss);
```

Se espera que la respuesta sea `5`. Solo hay una variable `hiss` ya que es estática. Se establece en 4, luego en 6 y finalmente termina como 5. Todas las variables `Snake` son solo distracciones.

### Membresía de Clase vs. Instancia

Hay otra forma en que los creadores del examen intentarán engañar con respecto a los miembros estáticos y de instancia. **Un miembro estático no puede llamar a un miembro de instancia sin hacer referencia a una instancia específica de la clase.** Esto no debería ser una sorpresa, ya que `static` no requiere que exista ninguna instancia de la clase.

El siguiente es un error común que cometen los programadores novatos:

```Java
public class MantaRay {
    private String name = "Sammy";
    public static void first() { }
    public static void second() { }
    public void third() { System.out.print(name); }
    public static void main(String args[]) {
        first();
        second();
        third(); // NO COMPILA
    }
}
```

El compilador dará un error sobre hacer una referencia estática a un método de instancia (`third()`). Si se soluciona esto añadiendo `static` a `third()`, se crea un nuevo problema. ¿Se puede averiguar cuál es?

```Java
public static void third() { System.out.print(name); } // NO COMPILA
```

Todo lo que hace esto es mover el problema. Ahora, `third()` (que es estático) se refiere a una variable de instancia `name`. Habría dos formas de solucionar esto. La primera es añadir `static` a la variable `name` también.

```Java
public class MantaRay {
    private static String name = "Sammy";
    // ...
    public static void third() { System.out.print(name); }
    // ...
}
```

La segunda solución habría sido llamar a `third()` como un método de instancia y no usar `static` para el método o la variable, instanciando un objeto primero en `main()`:

```Java
public class MantaRay {
    private String name = "Sammy";
    // ...
    public void third() { System.out.print(name); }
    public static void main(String args[]) {
        // ...
        var ray = new MantaRay();
        ray.third();
    }
}
```

A los creadores del examen les gusta mucho este tema. Un método estático o de instancia puede llamar a un método estático porque los métodos estáticos no requieren que se use un objeto. **Solo un método de instancia puede llamar a otro método de instancia en la misma clase sin usar una variable de referencia**, porque los métodos de instancia sí requieren un objeto (el `this` implícito). Una lógica similar se aplica para variables de instancia y estáticas.

Suponga que se tiene una clase `Giraffe`:

```Java
public class Giraffe {
    public void eat(Giraffe g) {}
    public void drink() {}
    public static void allGiraffeGoHome(Giraffe g) {}
    public static void allGiraffeComeOut() {}
}
```

Hay que asegurarse de comprender la Tabla 5.5.

**TABLA 5.5** Llamadas estáticas vs. de instancia

| **Método**           | **Llama a**           | **¿Legal?** | **Razón**                                                                                |
| -------------------- | --------------------- | ----------- | ---------------------------------------------------------------------------------------- |
| `allGiraffeGoHome()` | `allGiraffeComeOut()` | Sí          | Un método estático llamando a otro estático.                                             |
| `allGiraffeGoHome()` | `drink()`             | **No**      | Un método estático no puede llamar a un método de instancia sin una referencia.          |
| `allGiraffeGoHome()` | `g.eat()`             | Sí          | Un método estático llamando a un método de instancia _a través de una referencia_ (`g`). |
| `eat()`              | `allGiraffeComeOut()` | Sí          | Un método de instancia puede llamar a un método estático.                                |
| `eat()`              | `drink()`             | Sí          | Un método de instancia puede llamar a otro método de instancia.                          |
| `eat()`              | `g.eat()`             | Sí          | Un método de instancia puede llamar a un método de instancia de otra referencia.         |

Se probará un ejemplo más para tener más práctica en el reconocimiento de este escenario. ¿Se entiende por qué las siguientes líneas fallan al compilar?

```Java
1:  public class Gorilla {
2:      public static int count;
3:      public static void addGorilla() { count++; }
4:      public void babyGorilla() { count++; }
5:      public void announceBabies() {
6:          addGorilla();
7:          babyGorilla();
8:      }
9:      public static void announceBabiesToEveryone() {
10:         addGorilla();
11:         babyGorilla(); // NO COMPILA
12:     }
13:     public int total;
14:     public static double average
15:         = total / count; // NO COMPILA
16: }
```

Las líneas 3 y 4 están bien porque tanto los métodos estáticos como los de instancia pueden referirse a una variable estática (`count`). Las líneas 5–8 están bien porque un método de instancia puede llamar a un método estático. La línea 11 no compila porque **un método estático no puede llamar a un método de instancia**. De manera similar, la línea 15 no compila porque **una variable estática está intentando usar una variable de instancia** (`total`).

Un uso común de las variables estáticas es contar el número de instancias:

```Java
public class Counter {
    private static int count;
    public Counter() { count++; }
    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
        Counter c3 = new Counter();
        System.out.println(count); // 3
    }
}
```

Cada vez que se llama al constructor, se incrementa `count` en uno. Este ejemplo se basa en el hecho de que las variables estáticas (y de instancia) se inicializan automáticamente al valor predeterminado para ese tipo, que es 0 para `int`.

### Modificadores de variables estáticas

Refiriéndose nuevamente a la Tabla 5.3 (vista anteriormente), las variables estáticas pueden declararse con los mismos modificadores que las variables de instancia, como `final`, `transient` y `volatile`.

Mientras que algunas variables estáticas están destinadas a cambiar a medida que se ejecuta el programa, como en el ejemplo de `count`, otras están destinadas a no cambiar nunca. Este tipo de variable estática se conoce como **constante**. Utiliza el modificador `final` para asegurar que la variable nunca cambie.

Las constantes usan el modificador `static final` y una convención de nomenclatura diferente a la de otras variables. Usan todas las letras mayúsculas con guiones bajos entre "palabras". Aquí hay un ejemplo:

```Java
public class ZooPen {
    private static final int NUM_BUCKETS = 45;
    public static void main(String[] args) {
        NUM_BUCKETS = 5; // NO COMPILA
    }
}
```

El compilador se asegurará de que no se intente actualizar accidentalmente una variable `final`. Esto puede ponerse interesante. ¿Se cree que lo siguiente compila?

```Java
import java.util.*;
public class ZooInventoryManager {
    private static final String[] treats = new String[10];
    public static void main(String[] args) {
        treats[0] = "popcorn"; // SÍ COMPILA
    }
}
```

En realidad sí compila, ya que `treats` es una variable de referencia. **Se permite modificar el contenido del objeto o arreglo referenciado**. Todo lo que el compilador puede hacer es verificar que no se intente reasignar `treats` para apuntar a un arreglo diferente.

Las reglas para las variables `static final` son similares a las variables `final` de instancia, excepto que no utilizan constructores (¡no existe un "constructor estático"!) y utilizan **inicializadores estáticos** en lugar de inicializadores de instancia.

```Java
public class Panda {
    final static String name = "Ronda";
    static final int bamboo;
    static final double height; // NO COMPILA
    static { bamboo = 5; }
}
```

A la variable `name` se le asigna un valor cuando se declara, mientras que a la variable `bamboo` se le asigna un valor en un inicializador estático. A la variable `height` no se le asigna un valor en ninguna parte de la definición de la clase, por lo que esa línea no compila. Recuerde que las variables `final` deben inicializarse con un valor.

### Inicializadores estáticos (_Static Initializers_)

En el Capítulo 1, se cubrieron los inicializadores de instancia que parecían métodos sin nombre, solo código dentro de llaves. Ahora se introducen los inicializadores estáticos, que se ven similares. Simplemente se añade la palabra clave `static` para especificar que **deben ejecutarse cuando la clase se carga por primera vez**. Aquí hay un ejemplo:

```Java
private static final int NUM_SECONDS_PER_MINUTE;
private static final int NUM_MINUTES_PER_HOUR;
private static final int NUM_SECONDS_PER_HOUR;
static {
    NUM_SECONDS_PER_MINUTE = 60;
    NUM_MINUTES_PER_HOUR = 60;
}
static {
    NUM_SECONDS_PER_HOUR = NUM_SECONDS_PER_MINUTE * NUM_MINUTES_PER_HOUR;
}
```

Todos los inicializadores estáticos se ejecutan cuando la clase se utiliza por primera vez, **en el orden en que están definidos**. Las sentencias en ellos se ejecutan y asignan cualquier variable estática según sea necesario. Hay algo interesante sobre este ejemplo. Se acaba de decir que las variables `final` no pueden reasignarse. La clave aquí es que el inicializador estático es la **primera asignación**. Y dado que ocurre por adelantado, es válido.

Se intentará con otro ejemplo para asegurar que se comprende la distinción:

```Java
14: private static int one;
15: private static final int two;
16: private static final int three = 3;
17: private static final int four;        // NO COMPILA
18: static {
19:     one = 1;
20:     two = 2;
21:     three = 3;                      // NO COMPILA
22:     two = 4;                        // NO COMPILA
23: }
```

La línea 14 declara una variable estática que no es `final`. Puede ser asignada tantas veces como se desee. La línea 15 declara una variable `final` sin inicializarla. Esto significa que se puede inicializar **exactamente una vez** en un bloque estático. La línea 22 no compila porque es el segundo intento de asignar `two`. La línea 16 declara una variable `final` y la inicializa al mismo tiempo. No está permitido asignarla de nuevo, por lo que la línea 21 no compila. La línea 17 declara una variable `final` que nunca se inicializa. El compilador da un error porque sabe que los bloques estáticos son el único lugar donde la variable podría inicializarse. Como al programador se le olvidó, esto es claramente un error.

### Importaciones estáticas (_Static Imports_)

En el Capítulo 1, se vio que se puede importar una clase específica o todas las clases de un paquete utilizando `import`.

```Java
import java.util.ArrayList;
import java.util.*;
```

Las importaciones regulares son para importar clases, mientras que las **importaciones estáticas** son para importar miembros estáticos de las clases, como variables y métodos.

Al igual que las importaciones regulares, se puede utilizar un comodín (`*`) o importar un miembro específico. La idea es que no se tenga que especificar de dónde proviene cada método o variable estática cada vez que se usa. Un ejemplo de cuándo brillan las importaciones estáticas es cuando se hace referencia a muchas constantes en otra clase.

```Java
import java.util.List;
import static java.util.Arrays.asList; // importación estática

public class ZooParking {
    public static void main(String[] args) {
        List<String> list = asList("one", "two"); // Sin el prefijo Arrays.
    }
}
```

En este ejemplo, se está importando específicamente el método `asList`. Esto significa que cada vez que se haga referencia a `asList` en la clase, llamará a `Arrays.asList()`.

Un caso interesante es qué sucedería si se creara un método `asList` en la propia clase `ZooParking`. Java le daría preferencia sobre el importado, y se utilizaría el método codificado localmente.

El examen intentará engañar con el mal uso de las importaciones estáticas. Este ejemplo muestra casi todo lo que se puede hacer mal. ¿Se puede deducir qué está mal con cada uno?

```Java
1: import static java.util.Arrays;        // NO COMPILA
2: import static java.util.Arrays.asList;
3: static import java.util.Arrays.*;      // NO COMPILA
4: public class BadZooParking {
5:     public static void main(String[] args) {
6:         Arrays.asList("one");          // NO COMPILA
7:     }
8: }
```

- La línea 1 intenta usar una importación estática para importar una clase. Recuerde que las importaciones estáticas son **solo** para importar miembros estáticos (métodos o variables).
- La línea 3 intenta verificar si se está prestando atención al orden de las palabras clave. La sintaxis es `import static` y no viceversa.
- La línea 6 es engañosa. El método `asList` se importó en la línea 2. Sin embargo, la clase `Arrays` **no se ha importado en ninguna parte** mediante una importación regular. Esto hace que sea válido escribir `asList("one")` pero no `Arrays.asList("one")`.

Solo hay un escenario más con las importaciones estáticas. En el Capítulo 1, se aprendió que importar dos clases con el mismo nombre da un error del compilador. Esto también es cierto para las importaciones estáticas. El compilador se quejará si se intenta realizar explícitamente una importación estática de dos métodos con el mismo nombre o dos variables estáticas con el mismo nombre desde diferentes clases. Aquí hay un ejemplo:

```Java
import static zoo.A.TYPE;
import static zoo.B.TYPE; // NO COMPILA
```

Afortunadamente, cuando esto sucede, simplemente se puede referir a los miembros estáticos a través del nombre de su clase en el código (`A.TYPE` y `B.TYPE`), asegurando que las clases estén importadas, en lugar de intentar usar una importación estática.