Las respuestas a las preguntas de revisión del capítulo se pueden encontrar al final del capitulo.

**1.** ¿Cuáles de los siguientes son métodos de punto de entrada legales que se pueden ejecutar desde la línea de comandos? (Elija todas las opciones que correspondan).

- [ ]  A. `private static void main(String[] args)`
- [ ]  B. `public static final main(String[] args)`
- [ ]  C. `public void main(String[] args)`
- [ ]  D. `public static final void main(String[] args)`
- [ ]  E. `public static void main(String[] args)`
- [ ]  F. `public static main(String[] args)`  

**2.** ¿Qué opciones de respuesta representan el orden en el que las siguientes sentencias pueden ensamblarse en un programa que compile exitosamente? (Elija todas las opciones que correspondan). `X: class Rabbit {}` `Y: import java.util.*;` `Z: package animals;`

- [ ]  A. X, Y, Z
- [ ]  B. Y, Z, X    
- [ ]  C. Z, Y, X
- [ ]  D. Y, X
- [ ]  E. Z, X
- [ ]  F. X, Z
- [ ]  G. Ninguna de las anteriores

**3.** ¿Cuáles de las siguientes afirmaciones son verdaderas? (Elija todas las opciones que correspondan).

```Java
public class Bunny {
    public static void main(String[] x) {
        Bunny bun = new Bunny();
    } 
}
```

- [ ]  A. Bunny es una clase.
- [ ]  B. bun es una clase.
- [ ]  C. main es una clase.
- [ ]  D. Bunny es una referencia a un objeto.
- [ ]  E. bun es una referencia a un objeto.
- [ ]  F. main es una referencia a un objeto.
- [ ]  G. El método main() no se ejecuta porque el nombre del parámetro es incorrecto.

**4.** ¿Cuáles de los siguientes son identificadores Java válidos? (Elija todas las opciones que correspondan).

- [ ]  A. `_`
- [ ]  B. `_helloWorld$`
- [ ]  C. `true`
- [ ]  D. `java.lang`
- [ ]  E. `Public`
- [ ]  F. `1980_s`
- [ ]  G. `_Q2_`

**5.** ¿Qué afirmaciones sobre el siguiente programa son correctas? (Elija todas las opciones que correspondan).

```Java
2: public class Bear {
3:      private Bear pandaBear;
4:      private void roar(Bear b) {
5:          System.out.println("Roar!");
6:          pandaBear = b;
7:      }
8:      public static void main(String[] args) {
9:          Bear brownBear = new Bear();
10:         Bear polarBear = new Bear();
11:         brownBear.roar(polarBear);
12:         polarBear = null;
13:         brownBear = null;
14:         System.gc(); 
        } 
}
```

- [ ]  A. El objeto creado en la línea 9 es elegible por primera vez para la recolección de basura después de la línea 13.
- [ ]  B. El objeto creado en la línea 9 es elegible por primera vez para la recolección de basura después de la línea 14.
- [ ]  C. El objeto creado en la línea 10 es elegible por primera vez para la recolección de basura después de la línea 12.
- [ ]  D. El objeto creado en la línea 10 es elegible por primera vez para la recolección de basura después de la línea 13.
- [ ]  E. Se garantiza que la recolección de basura se ejecutará.
- [ ]  F. La recolección de basura podría ejecutarse o no.
- [ ]  G. El código no compila.

**6.** Asumiendo que la siguiente clase compila, ¿cuántas variables definidas en la clase o método están dentro del alcance (_scope_) en la línea marcada como 14?

```Java
1: public class Camel {
2:      { int hairs = 3_000_0; }
3:      long water, air=2;
4:      boolean twoHumps = true;
5:      public void spit(float distance) {
6:          var path = "";
7:          { double teeth = 32 + distance++; }
8:          while(water > 0) {
9:              int age = twoHumps ? 1 : 2;
10:             short i=-1;
11:             for(i=0; i<10; i++) {
12:                 var Private = 2;
13:             }
14:             // ALCANCE (SCOPE)
15:         }
16:     }
17: }
```

- [ ]  A. 2
- [ ]  B. 3
- [ ]  C. 4
- [ ]  D. 5
- [ ]  E. 6
- [ ]  F. 7
- [ ]  G. Ninguna de las anteriores

**7.** ¿Cuáles son verdaderas sobre este código? (Elija todas las opciones que correspondan).

```Java
public class KitchenSink {
    private int numForks;
    public static void main(String[] args) {
        int numKnives;
        System.out.print("""
            "# forks = " + numForks +
            " # knives = " + numKnives +
            # cups = 0""");
    }
}
```

- [ ]  A. La salida incluye `# forks = 0`.
- [ ]  B. La salida incluye `# knives = 0`.
- [ ]  C. La salida incluye `# cups = 0`.
- [ ]  D. La salida incluye una línea en blanco.
- [ ]  E. La salida incluye una o más líneas que comienzan con espacios en blanco.
- [ ]  F. El código no compila.

**8.** ¿Cuáles de los siguientes fragmentos de código sobre `var` compilan sin problemas cuando se usan en un método? (Elija todas las opciones que correspondan).

- [ ]  A. `var spring = null;`
- [ ]  B. `var fall = "leaves";`
- [ ]  C. `var evening = 2; evening = null;`
- [ ]  D. `var night = Integer.valueOf(3);`
- [ ]  E. `var day = 1/0;`
- [ ]  F. `var winter = 12, cold;`
- [ ]  G. `var fall = 2, autumn = 2;`
- [ ]  H. `var morning = ""; morning = null;`

**9.** ¿Cuál de las siguientes opciones es correcta?

- [ ]  A. Una variable de instancia de tipo `float` tiene un valor predeterminado de `0`.
- [ ]  B. Una variable de instancia de tipo `char` tiene un valor predeterminado de `null`.
- [ ]  C. Una variable local de tipo `double` tiene un valor predeterminado de `0.0`.    
- [ ]  D. Una variable local de tipo `int` tiene un valor predeterminado de `null`.
- [ ]  E. Una variable de clase de tipo `String` tiene un valor predeterminado de `null`.
- [ ]  F. Una variable de clase de tipo `String` tiene un valor predeterminado de cadena vacía `""`.
- [ ]  G. Ninguna de las anteriores.

**10.** ¿Cuáles de las siguientes expresiones, al insertarse independientemente en la línea en blanco, permiten que el código compile? (Elija todas las opciones que correspondan).

```Java
public void printMagicData() {
    var magic = ___________;
    System.out.println(magic);
}
```

- [ ]  A. `3_1`
- [ ]  B. `1_329_.0`
- [ ]  C. `3_13.0_`
- [ ]  D. `5_291._2`
- [ ]  E. `2_234.0_0`
- [ ]  F. `9___6`
- [ ]  G. `_1_3_5_0`

**11.** Dados los siguientes dos archivos de clase, ¿cuál es el número máximo de importaciones (_imports_) que se pueden eliminar y que el código siga compilando?

```Java
// Water.java
package aquarium;
public class Water { }

// Tank.java
package aquarium;
import java.lang.*;
import java.lang.System;
import aquarium.Water;
import aquarium.*;
public class Tank {
    public void print(Water water) {
        System.out.println(water); 
    } 
}
```

- [ ]  A. 0
- [ ]  B. 1    
- [ ]  C. 2
- [ ]  D. 3
- [ ]  E. 4
- [ ]  F. No compila

**12.** ¿Qué afirmaciones sobre la siguiente clase son correctas? (Elija todas las opciones que correspondan).

```Java
1: public class ClownFish {
2:      int gills = 0, double weight=2;
3:      { int fins = gills; }
4:      void print(int length = 3) {
5:          System.out.println(gills);
6:          System.out.println(weight);
7:          System.out.println(fins);
8:          System.out.println(length);
9:      } 
}
```

- [ ]  A. La línea 2 genera un error de compilación.
- [ ]  B. La línea 3 genera un error de compilación.
- [ ]  C. La línea 4 genera un error de compilación.
- [ ]  D. La línea 7 genera un error de compilación.
- [ ]  E. El código imprime 0.
- [ ]  F. El código imprime 2.0.
- [ ]  G. El código imprime 2.
- [ ]  H. El código imprime 3.

**13.** Dadas las siguientes clases, ¿cuáles de los siguientes fragmentos pueden insertarse independientemente en lugar de `INSERT IMPORTS HERE` y hacer que el código compile? (Elija todas las opciones que correspondan).

```Java
package aquarium;
public class Water {
    boolean salty = false;
}
package aquarium.jellies;
public class Water {
    boolean salty = true;
}
package employee;
INSERT IMPORTS HERE
public class WaterFiller {
    Water water;
}
```

- [ ]  A. `import aquarium.*;`
- [ ]  B. `import aquarium.Water;`
- [ ]  C. `import aquarium.jellies.*;`
- [ ]  D. `import aquarium.*;`
- [ ]  E. `import aquarium.jellies.Water;`
- [ ]  F. `import aquarium.*;`
- [ ]  G. `import aquarium.jellies.*;`
- [ ]  H. `import aquarium.Water;`
- [ ]  I. `import aquarium.jellies.Water;`
- [ ]  J. Ninguna de las anteriores

**14.** ¿Cuáles de las siguientes afirmaciones sobre el fragmento de código son verdaderas? (Elija todas las opciones que correspondan).

```Java
3: short numPets = 5L;
4: int numGrains = 2.0;
5: String name = "Scruffy";
6: int d = numPets.length();
7: int e = numGrains.length;
8: int f = name.length();
```

- [ ]  A. La línea 3 genera un error de compilación.
- [ ]  B. La línea 4 genera un error de compilación.
- [ ]  C. La línea 5 genera un error de compilación.
- [ ]  D. La línea 6 genera un error de compilación.
- [ ]  E. La línea 7 genera un error de compilación.
- [ ]  F. La línea 8 genera un error de compilación.

**15.** ¿Cuáles de las siguientes afirmaciones sobre la recolección de basura son correctas? (Elija todas las opciones que correspondan).

- [ ]  A. Llamar a `System.gc()` garantiza la liberación de memoria al destruir objetos elegibles para la recolección de basura.
- [ ]  B. La recolección de basura se ejecuta en un horario establecido.
- [ ]  C. La recolección de basura permite a la JVM reclamar memoria para otros objetos.
- [ ] D. La recolección de basura se ejecuta cuando el programa ha utilizado la mitad de la memoria disponible.
- [ ]  E. Un objeto puede ser elegible para la recolección de basura pero nunca ser eliminado del _heap_.
- [ ]  F. Un objeto es elegible para la recolección de basura una vez que no hay referencias accesibles a él en el programa.
- [ ]  G. Marcar una variable como `final` significa que su objeto asociado nunca será recolectado.

**16.** ¿Cuáles son verdaderas sobre este código? (Elija todas las opciones que correspondan).

```Java
var blocky = """
    squirrel \s
    pigeon \
    termite""";
System.out.print(blocky);
```

- [ ]  A. Imprime dos líneas.
- [ ]  B. Imprime tres líneas.
- [ ]  C. Imprime cuatro líneas.
- [ ]  D. Hay una línea con espacio en blanco al final.
- [ ]  E. Hay dos líneas con espacio en blanco al final.
- [ ]  F. Si indentáramos cada línea cinco caracteres, cambiaría la salida.

**17.** ¿Qué líneas imprime el siguiente programa? (Elija todas las opciones que correspondan).

```Java
1: public class WaterBottle {
2:      private String brand;
3:      private boolean empty;
4:      public static float code;
5:      public static void main(String[] args) {
6:          WaterBottle wb = new WaterBottle();
7:          System.out.println("Empty = " + wb.empty);
8:          System.out.println("Brand = " + wb.brand);
9:          System.out.println("Code = " + code);
10:     } 
}
```

- [ ]  A. La línea 8 genera un error de compilación.
- [ ]  B. La línea 9 genera un error de compilación.
- [ ]  C. `Empty =`
- [ ]  D. `Empty = false`
- [ ]  E. `Brand =`
- [ ]  F. `Brand = null`
- [ ]  G. `Code = 0.0`
- [ ]  H. `Code = 0f`

**18.** ¿Cuáles de las siguientes afirmaciones sobre `var` son verdaderas? (Elija todas las opciones que correspondan).

- [ ]  A. `var` se puede utilizar como parámetro de constructor.
- [ ]  B. El tipo de una `var` se conoce en tiempo de compilación.
- [ ]  C. `var` no se puede utilizar como variable de instancia.
- [ ]  D. `var` se puede utilizar en una sentencia de asignación de múltiples variables.
- [ ]  E. El valor de una `var` no puede cambiar en tiempo de ejecución.
- [ ]  F. El tipo de una `var` no puede cambiar en tiempo de ejecución.
- [ ]  G. La palabra `var` es una palabra reservada en Java.

**19.** ¿Cuáles son verdaderas sobre el siguiente código? (Elija todas las opciones que correspondan).

```Java
var num1 = Integer.parseInt("11");
var num2 = Integer.valueOf("B", 16);
System.out.println(Integer.max(num1, num2));
```

- [ ]  A. La salida es 11.
- [ ]  B. La salida es B.
- [ ]  C. El código no compila. 
- [ ]  D. `num1` es un primitivo.
- [ ]  E. `num2` es un primitivo.
- [ ]  F. Se lanza una `NumberFormatException`.

**20.** ¿Qué afirmación sobre la siguiente clase es correcta?

```Java
1: public class PoliceBox {
2:      String color;
3:      long age;
4:      public void PoliceBox() {
5:          color = "blue";
6:          age = 1200;
7:      }
8:      public static void main(String []time) {
9:          var p = new PoliceBox();
10:         var q = new PoliceBox();
11:         p.color = "green";
12:         p.age = 1400;
13:         p = q;
14:         System.out.println("Q1="+q.color);
15:         System.out.println("Q2="+q.age);
16:         System.out.println("P1="+p.color);
17:         System.out.println("P2="+p.age);
18:     } 
}
```

- [ ]  A. Imprime `Q1=blue`.
- [ ]  B. Imprime `Q2=1200`.
- [ ]  C. Imprime `P1=null`.
- [ ]  D. Imprime `P2=1400`.
- [ ]  E. La línea 4 no compila.
- [ ]  F. La línea 12 no compila.
- [ ]  G. La línea 13 no compila.
- [ ]  H. Ninguna de las anteriores.

**21.** ¿Cuál es la salida al ejecutar la siguiente clase?

```Java
1: public class Salmon {
2:      int count;
3:      { System.out.print(count+"-"); }
4:      { count++; }
5:      public Salmon() {
6:          count = 4;
7:          System.out.print(2+"-");
8:      }
9:      public static void main(String[] args) {
10:         System.out.print(7+"-");
11:         var s = new Salmon();
12:         System.out.print(s.count+"-"); 
    } 
}
```

- [ ]  A. `7-0-2-1-`
- [ ]  B. `7-0-1-`
- [ ]  C. `0-7-2-1-`
- [ ]  D. `7-0-2-4-`
- [ ]  E. `0-7-1-`
- [ ]  F. La clase no compila debido a la línea 3.
- [ ]  G. La clase no compila debido a la línea 4.
- [ ]  H. Ninguna de las anteriores.

**22.** Dada la siguiente clase, ¿cuáles de las siguientes líneas de código pueden reemplazar independientemente a `INSERT CODE HERE` para hacer que el código compile? (Elija todas las opciones que correspondan).

```Java
public class Price {
    public void admission() {
        INSERT CODE HERE
        System.out.print(amount);
    } 
}
```

- [ ]  A. `int Amount = 0b11;`
- [ ]  B. `int amount = 9L;`
- [ ]  C. `int amount = 0xE;`
- [ ]  D. `int amount = 1_2.0;`
- [ ]  E. `double amount = 1_0_.0;`
- [ ]  F. `int amount = 0b101;`
- [ ]  G. `double amount = 9_2.1_2;`
- [ ]  H. `double amount = 1_2_.0_0;`

**23.** ¿Qué afirmaciones sobre la siguiente clase son verdaderas? (Elija todas las opciones que correspondan).

```Java
1: public class River {
2:      int Depth = 1;
3:      float temp = 50.0;
4:      public void flow() {
5:          for (int i = 0; i < 1; i++) {
6:              int depth = 2;
7:              depth++;
8:              temp--;
9:          }
10:         System.out.println(depth);
11:         System.out.println(temp); 
        }
12:     public static void main(String... s) {
13:         new River().flow();
14:     } 
}
```

- [ ]  A. La línea 3 genera un error de compilación.
- [ ]  B. La línea 6 genera un error de compilación.
- [ ]  C. La línea 7 genera un error de compilación.
- [ ]  D. La línea 10 genera un error de compilación.
- [ ]  E. El programa imprime 3 en la línea 10.
- [ ]  F. El programa imprime 4 en la línea 10.
- [ ]  G. El programa imprime 50.0 en la línea 11.
- [ ]  H. El programa imprime 49.0 en la línea 11.