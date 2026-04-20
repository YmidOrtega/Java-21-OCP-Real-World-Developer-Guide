**1.** ¿Qué afirmaciones sobre el modificador `final` son correctas? (Elija todas las opciones que correspondan).

- [x]  A. Las variables de instancia y estáticas pueden marcarse como `final`.
- [ ]  B. Una variable es efectivamente final solo si está marcada como `final`.
- [ ]  C. Un objeto que está marcado como `final` no puede ser modificado.
- [x]  D. Las variables locales no pueden declararse con el tipo `var` y el modificador `final`.
- [x]  E. Un primitivo que está marcado como `final` no puede ser modificado.

**2.** ¿Cuáles de las siguientes opciones pueden llenar el espacio en blanco en este código para que compile? (Elija todas las opciones que correspondan).

```Java
public class Ant {
    _________ void method() {}
}
```

- [ ]  A. `default`
- [x]  B. `final`
- [x]  C. `private`
- [ ]  D. `Public`
- [ ]  E. `String`
- [ ]  F. `zzz:`

**3.** ¿Cuáles de los siguientes métodos compilan? (Elija todas las opciones que correspondan).

- [x]  A. `final static void rain() {}`
- [ ]  B. `public final int void snow() {}`
- [ ]  C. `private void int hail() {}`
- [x]  D. `static final void sleet() {}`
- [ ]  E. `void final ice() {}` 
- [ ]  F. `void public slush() {}` 

**4.** ¿Cuáles de las siguientes opciones pueden llenar el espacio en blanco y permitir que el código compile? (Elija todas las opciones que correspondan).

```Java
final ______ song = 6;
```

- [x]  A. `int`
- [x]  B. `Integer` 
- [x]  C. `long` 
- [ ]  D. `Long`
- [x]  E. `double`
- [ ]  F. `Double`

**5.** ¿Cuáles de los siguientes métodos compilan? (Elija todas las opciones que correspondan).

- [x]  A. `public void january() { return; }`
- [ ]  B. `public int february() { return null;}`
- [x]  C. `public void march() {}`
- [x]  D. `public int april() { return 9;}`
- [ ]  E. `public int may() { return 9.0;}`
- [ ]  F. `public int june() { return;}`

**6.** ¿Cuáles de los siguientes métodos compilan? (Elija todas las opciones que correspondan).

- [x]  A. `public void violin(int... nums) {}`
- [x]  B. `public void viola(String values, int... nums) {}`
- [ ]  C. `public void cello(int... nums, String values) {}`
- [ ]  D. `public void bass(String... values, int... nums) {}`
- [ ]  E. `public void flute(String[] values, ...int nums) {}`
- [x]  F. `public void oboe(String[] values, int[] nums) {}`

**7.** Dado el siguiente método, ¿cuáles de las llamadas al método devuelven `2`? (Elija todas las opciones que correspondan).

```Java
public int juggle(boolean b, boolean... b2) {
    return b2.length;
}
```

- [ ]  A. `juggle();`
- [ ]  B. `juggle(true);`
- [ ]  C. `juggle(true, true);`
- [x]  D. `juggle(true, true, true);`
- [ ]  E. `juggle(true, {true, true});`
- [x]  F. `juggle(true, new boolean[2]);`

**8.** ¿Cuál de las siguientes afirmaciones es correcta?

- [ ]  A. El acceso de paquete es más permisivo que el acceso `protected`.
- [ ]  B. Una clase pública que tiene campos privados y métodos de paquete no es visible para clases fuera del paquete.    
- [x]  C. Se pueden usar modificadores de acceso para que solo algunas de las clases en un paquete vean a una clase de paquete particular.   
- [x]  D. Se pueden usar modificadores de acceso para permitir el acceso a todos los métodos y no a ninguna variable de instancia.  
- [x]  E. Se pueden usar modificadores de acceso para restringir el acceso a todas las clases que comienzan con la palabra "Test".

**9.** Dadas las siguientes definiciones de clase, ¿qué líneas en el método `main()` generan un error del compilador? (Elija todas las opciones que correspondan).

```Java
// Classroom.java
package my.school;
public class Classroom {
    private int roomNumber;
    protected static String teacherName;
    static int globalKey = 54321;
    public static int floor = 3;
    Classroom(int r, String t) {
        roomNumber = r;
        teacherName = t; 
    } 
}

// School.java
1: package my.city;
2: import my.school.*;
3: public class School {
4:     public static void main(String[] args) {
5:         System.out.println(Classroom.globalKey);
6:         Classroom room = new Classroom(101, "Mrs. Anderson");
7:         System.out.println(room.roomNumber);
8:         System.out.println(Classroom.floor);
9:         System.out.println(Classroom.teacherName); 
       } 
   }
```

- [ ]  A. Ninguna: el código compila bien.
- [x]  B. Línea 5.
- [x]  C. Línea 6.
- [x]  D. Línea 7.
- [ ]  E. Línea 8.
- [ ]  F. Línea 9.

**10.** ¿Cuál es la salida de ejecutar el programa `Chimp`?

```Java
// Rope.java
1: package rope;
2: public class Rope {
3:     public static int LENGTH = 5;
4:     static {
5:         LENGTH = 10;
6:     }
7:     public static void swing() {
8:         System.out.print("swing ");
9:     } 
   }

// Chimp.java
1: import rope.*;
2: import static rope.Rope.*;
3: public class Chimp {
4:     public static void main(String[] args) {
5:         Rope.swing();
6:         new Rope().swing();
7:         System.out.println(LENGTH);
8:     } 
   }
```

- [ ]  A. `swing swing 5`
- [ ]  B. `swing swing 10`
- [x]  C. Error de compilador en la línea 2 de `Chimp`
- [x]  D. Error de compilador en la línea 5 de `Chimp`
- [x]  E. Error de compilador en la línea 6 de `Chimp`
- [ ]  F. Error de compilador en la línea 7 de `Chimp`

**11.** ¿Qué afirmaciones son verdaderas sobre el siguiente código? (Elija todas las opciones que correspondan).

```Java
1:  public class Rope {
2:      public static void swing() {
3:          System.out.print("swing");
4:      }
5:      public void climb() {
6:          System.out.println("climb");
7:      }
8:      public static void play() {
9:          swing();
10:         climb();
11:     }
12:     public static void main(String[] args) {
13:         Rope rope = new Rope();
14:         rope.play();
15:         Rope rope2 = null;
16:         System.out.print("-");
17:         rope2.play();
18:     } 
    }
```

- [ ]  A. El código compila tal cual.
- [x]  B. Hay exactamente un error de compilador en el código.
- [ ]  C. Hay exactamente dos errores de compilador en el código.
- [ ]  D. Si se eliminan las líneas con errores de compilación, la salida es `swing-climb`.
- [ ]  E. Si se eliminan las líneas con errores de compilación, la salida es `swing-swing`.
- [ ]  F. Si se eliminan las líneas con errores de compilación, el código lanza una `NullPointerException`.

**12.** ¿Cuántas variables en el siguiente método son efectivamente finales?

```Java
10: public void feed() {
11:     int monkey = 0;
12:     if(monkey > 0) {
13:         var giraffe = monkey++;
14:         String name;
15:         name = "geoffrey";
16:     }
17:     String name = "milly";
18:     var food = 10;
19:     while(monkey <= 10) {
20:         food = 0;
21:     }
22:     name = null;
23: }
```

- [ ]  A. 1.
- [x]  B. 2.
- [ ]  C. 3.
- [ ]  D. 4.
- [ ]  E. 5.
- [ ]  F. Ninguna de las anteriores. El código no compila.

**13.** ¿Cuál es la salida del siguiente código?

```Java
// RopeSwing.java
import rope.*;
import static rope.Rope.*;
public class RopeSwing {
    private static Rope rope1 = new Rope();
    private static Rope rope2 = new Rope();
    {
        System.out.println(rope1.length);
    }
    public static void main(String[] args) {
        rope1.length = 2;
        rope2.length = 8;
        System.out.println(rope1.length);
    }
}

// Rope.java
package rope;
public class Rope {
    public static int length = 0;
}
```

- [ ]  A. `02`
- [x]  B. `08`
- [ ]  C. `2`
- [ ]  D. `8`
- [ ]  E. El código no compila.
- [ ]  F. Se lanza una excepción.

**14.** ¿Cuántas líneas en el siguiente código tienen errores de compilador?

```Java
1:  public class RopeSwing {
2:      private static final String leftRope;
3:      private static final String rightRope;
4:      private static final String bench;
5:      private static final String name = "name";
6:      static {
7:          leftRope = "left";
8:          rightRope = "right";
9:      }
10:     static {
11:         name = "name";
12:         rightRope = "right";
13:     }
14:     public static void main(String[] args) {
15:         bench = "bench";
16:     }
17: }
```

- [ ]  A. 0
- [ ]  B. 1
- [ ]  C. 2
- [x]  D. 3    
- [ ]  E. 4
- [ ]  F. 5

**15.** ¿Cuáles de las siguientes opciones pueden reemplazar la línea 2 para hacer que este código compile?

```Java
1: import java.util.*;
2: // INSERTE CÓDIGO AQUÍ
3: public class Imports {
4:     public void method(ArrayList<String> list) {
5:         sort(list);
6:     }
7: }
```

- [ ]  A. `import static java.util.Collections;`
- [x]  B. `import static java.util.Collections.*;`
- [x]  C. `import static java.util.Collections.sort(ArrayList<String>);`
- [ ]  D. `static import java.util.Collections;`
- [ ]  E. `static import java.util.Collections.*;`
- [ ]  F. `static import java.util.Collections.sort(ArrayList<String>);`

**16.** ¿Cuál es el resultado de las siguientes sentencias?

```Java
1:  public class Test {
2:      public void print(byte x) {
3:          System.out.print("byte-");
4:      }
5:      public void print(int x) {
6:          System.out.print("int-");
7:      }
8:      public void print(float x) {
9:          System.out.print("float-");
10:     }
11:     public void print(Object x) {
12:         System.out.print("Object-");
13:     }
14:     public static void main(String[] args) {
15:         Test t = new Test();
16:         short s = 123;
17:         t.print(s);
18:         t.print(true);
19:         t.print(6.789);
20:     }
21: }
```

- [ ]  A. `byte-float-Object-`
- [ ]  B. `int-float-Object-`
- [ ]  C. `byte-Object-float-`
- [ ]  D. `int-Object-float-`
- [x]  E. `int-Object-Object-`
- [ ]  F. `byte-Object-Object-`

**17.** ¿Cuál es el resultado del siguiente programa?

```Java
1: public class Squares {
2:     public static long square(int x) {
3:         var y = x * (long) x;
4:         x = -1;
5:         return y;
6:     }
7:     public static void main(String[] args) {
8:         var value = 9;
9:         var result = square(value);
10:        System.out.println(value);
11:    } 
   }
```

- [ ]  A.`-1`
- [ ]  B. `9`
- [x]  C. `81`
- [ ]  D. Error de compilador en la línea 9
- [ ]  E. Error de compilador en una línea diferente

**18.** ¿Cuáles de las siguientes salidas son producidas por el siguiente código? (Elija todas las opciones que correspondan).

```Java
public class StringBuilders {
    public static StringBuilder work(StringBuilder a, StringBuilder b) {
        a = new StringBuilder("a");
        b.append("b");
        return a;
    }
    public static void main(String[] args) {
        var s1 = new StringBuilder("s1");
        var s2 = new StringBuilder("s2");
        var s3 = work(s1, s2);
        System.out.println("s1 = " + s1);
        System.out.println("s2 = " + s2);
        System.out.println("s3 = " + s3);
    }
}
```

- [ ]  A. `s1 = a`
- [x]  B. `s1 = s1`
- [ ]  C. `s2 = s2`
- [x]  D. `s2 = s2b`
- [x]  E. `s3 = a`
- [ ]  F. El código no compila.

**19.** ¿Cuáles de las siguientes opciones compilarán cuando se inserten de forma independiente en el siguiente código? (Elija todas las opciones que correspondan).

```Java
1:  public class Order3 {
2:      final String value1 = "red";
3:      static String value2 = "blue";
4:      String value3 = "yellow";
5:      {
6:          // FRAGMENTO DE CÓDIGO 1
7:      }
8:      static {
9:          // FRAGMENTO DE CÓDIGO 2
10:     } 
    }
```

- [ ]  A. Insertar en la línea 6: `value1 = "green";`
- [ ]  B. Insertar en la línea 6: `value2 = "purple";`
- [x]  C. Insertar en la línea 6: `value3 = "orange";`
- [ ]  D. Insertar en la línea 9: `value1 = "magenta";`
- [x]  E. Insertar en la línea 9: `value2 = "cyan";`
- [ ]  F. Insertar en la línea 9: `value3 = "turquoise";`

**20.** ¿Cuáles de las siguientes afirmaciones son verdaderas sobre el siguiente código? (Elija todas las opciones que correspondan).

```Java
public class Run {
    static void execute() {
        System.out.print("1-");
    }
    static void execute(int num) {
        System.out.print("2-");
    }
    static void execute(Integer num) {
        System.out.print("3-");
    }
    static void execute(Object num) {
        System.out.print("4-");
    }
    static void execute(int... nums) {
        System.out.print("5-");
    }
    public static void main(String[] args) {
        Run.execute(100);
        Run.execute(100L);
    }
}
```

- [x]  A. El código imprime `2-4-`.
- [ ]  B. El código imprime `3-4-`.
- [ ]  C. El código imprime `4-2-`.
- [ ]  D. El código imprime `4-4-`.
- [x]  E. El código imprime `3-4-` si se elimina el método `static void execute(int num)`.
- [ ]  F. El código imprime `4-4-` si se elimina el método `static void execute(int num)`.

**21.** ¿Qué firmas de métodos son sobrecargas válidas de la siguiente firma de método? (Elija todas las opciones que correspondan).

```Java
public void moo(int m, int... n)
```

- [ ]  A. `public void moo(int a, int... b)`
- [x]  B. `public int moo(char ch)`
- [ ]  C. `public void moooo(int... z)`
- [x]  D. `private void moo(int... x)`
- [ ]  E. `public void moooo(int y)`
- [ ]  F. `public void moo(int... c, int d)`
- [ ]  G. `public void moo(int... i, int j...)`