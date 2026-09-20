Las respuestas a las preguntas de revisión del capítulo se pueden encontrar al final del capitulo.

**1. ¿Qué código se puede insertar para que el código imprima 2?**

```Java
public class BirdSeed {
    private int numberBags;
    boolean call;
    public BirdSeed() {
        // LÍNEA 1
        call = false;
        // LÍNEA 2
    }
    public BirdSeed(int numberBags) {
        this.numberBags = numberBags;
    }
    public static void main(String[] args) {
        var seed = new BirdSeed();
        System.out.print(seed.numberBags);
    } 
}
```

- [ ]  A. Reemplazar la línea 1 con `BirdSeed(2);`
- [ ]  B. Reemplazar la línea 2 con `BirdSeed(2);`
- [ ]  C. Reemplazar la línea 1 con `new BirdSeed(2);`
- [ ]  D. Reemplazar la línea 2 con `new BirdSeed(2);`
- [x]  E. Reemplazar la línea 1 con `this(2);`
- [ ]  F. Reemplazar la línea 2 con `this(2);`
- [ ]  G. El código imprime 2 sin ningún cambio.

**2. ¿Qué pares de modificadores se pueden usar juntos en la declaración de un método?** (_Se deben seleccionar todas las opciones que apliquen_).

- [x]  A. `static` y `final`
- [x]  B. `private` y `static`
- [ ]  C. `static` y `abstract`
- [ ]  D. `private` y `abstract`
- [ ]  E. `abstract` y `final`
- [x]  F. `private` y `final`

**3. ¿Cuáles de las siguientes afirmaciones sobre los métodos son verdaderas?** (_Se deben seleccionar todas las opciones que apliquen_).

- [ ]  A. Los métodos sobrecargados (_overloaded_) deben tener la misma firma.
- [x]  B. Los métodos sobrescritos (_overridden_) deben tener la misma firma.
- [x]  C. Los métodos ocultos (_hidden_) deben tener la misma firma.
- [ ]  D. Los métodos sobrecargados (_overloaded_) deben tener el mismo tipo de retorno.
- [x]  E. Los métodos sobrescritos (_overridden_) deben tener el mismo tipo de retorno.
- [x]  F. Los métodos ocultos (_hidden_) deben tener el mismo tipo de retorno.

**4. ¿Cuál es la salida del siguiente programa?**

```Java
1:  class Mammal {
2:      private void sneeze() {}
3:      public Mammal(int age) {
4:          System.out.print("Mammal");
5:      } 
6:  }
7:  public class Platypus extends Mammal {
8:      int sneeze() { return 1; }
9:      public Platypus() {
10:         System.out.print("Platypus");
11:     }
12:     public static void main(String[] args) {
13:         new Mammal(5);
14:     } 
15: }
```

- [ ]  A. Platypus
- [ ]  B. Mammal
- [ ]  C. PlatypusMammal
- [ ]  D. MammalPlatypus
- [ ]  E. El código compilará si **se cambia** la línea 7.
- [x]  F. El código compilará si **se cambia** la línea 9.

**5. ¿Cuál de las siguientes opciones completa el constructor para que este código imprima 50?**

```Java
class Speedster {
    int numSpots;
}
public class Cheetah extends Speedster {
    int numSpots;
    public Cheetah(int numSpots) {
        // INSERTAR CÓDIGO AQUÍ
    }
    public static void main(String[] args) {
        Speedster s = new Cheetah(50);
        System.out.print(s.numSpots);
    }
}
```

- [ ]  A. `numSpots = numSpots;`
- [ ]  B. `numSpots = this.numSpots;`
- [ ]  C. `this.numSpots = numSpots;`
- [ ]  D. `numSpots = super.numSpots;`
- [x]  E. `super.numSpots = numSpots;`
- [ ]  F. El código no compila independientemente del código insertado en el constructor.
- [ ]  G. Ninguna de las anteriores.

**6. ¿Cuáles de las siguientes declaran clases inmutables?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
public final class Moose {
    private final int antlers;
}
```

```Java
public class Caribou {
    private int antlers = 10;
}
```

```Java
public class Reindeer {
    private final int antlers = 5;
}
```

```Java
public final class Elk {}
```

```Java
public final class Deer {
    private final Object o = new Object();
}
```

- [x]  A. `Moose`
- [ ]  B. `Caribou`
- [ ]  C. `Reindeer`
- [x]  D. `Elk`
- [ ]  E. `Deer`
- [ ]  F. Ninguna de las anteriores

**7. ¿Cuál es la salida del siguiente código?**

```Java
1:  class Arthropod {
2:      protected void printName(long input) {
3:          System.out.print("Arthropod");
4:      }
5:      void printName(int input) {
6:          System.out.print("Spooky");
7:      } 
8:  }
9:  public class Spider extends Arthropod {
10:     protected void printName(int input) {
11:         System.out.print("Spider");
12:     }
13:     public static void main(String[] args) {
14:         Arthropod a = new Spider();
15:         a.printName((short)4);
16:         a.printName(4);
17:         a.printName(5L);
18:     }
19: }
```

- [ ]  A. SpiderSpiderArthropod
- [ ]  B. SpiderSpiderSpider
- [ ]  C. SpiderSpookyArthropod
- [ ]  D. SpookySpiderArthropod
- [ ]  E. El código no compilará debido a la línea 5.
- [ ]  F. El código no compilará debido a la línea 9.
- [x]  G. Ninguna de las anteriores.

**8. ¿Cuál es el resultado del siguiente código?**

```Java
1:  abstract class Bird {
2:      private final void fly() { System.out.println("Bird"); }
3:      protected Bird() { System.out.print("Wow-"); }
4:  }
5:  public class Pelican extends Bird {
6:      public Pelican() { System.out.print("Oh-"); }
7:      protected void fly() { System.out.println("Pelican"); }
8:      public static void main(String[] args) {
9:          var chirp = new Pelican();
10:         chirp.fly();
11:     } 
12: }
```

- [ ]  A. Oh-Bird
- [ ]  B. Oh-Pelican
- [ ]  C. Wow-Oh-Bird
- [x]  D. Wow-Oh-Pelican
- [ ]  E. El código contiene un error de compilación.
- [ ]  F. Ninguna de las anteriores.

**9. ¿Cuáles de las siguientes afirmaciones sobre los métodos sobrescritos (_overridden_) son verdaderas?** (_Se deben seleccionar todas las opciones que apliquen_).

- [x]  A. Un método sobrescrito debe contener parámetros de método que sean iguales o covariantes con los parámetros de método en el método heredado.
- [x]  B. Un método sobrescrito puede declarar una nueva excepción, siempre que no sea verificada (_checked_).
- [x]  C. Un método sobrescrito debe ser más accesible que el método en la clase padre.
- [ ]  D. Un método sobrescrito puede declarar una excepción verificada (_checked exception_) más amplia que el método en la clase padre.
- [x]  E. Si un método heredado devuelve `void`, entonces la versión sobrescrita del método debe devolver `void`.
- [ ]  F. Ninguna de las anteriores.

**10. ¿Cuáles de los siguientes pares, al insertarse en los espacios en blanco, permiten que el código compile?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
1:  public class Howler {
2:      public Howler(long shadow) {
3:          ____________;
4:      }
5:      private Howler(int moon) {
6:          super();
7:      }
8:  }
9:  class Wolf extends Howler {
10:     protected Wolf(String stars) {
11:         super(2L);
12:     }
13:     public Wolf() {
14:         _____________;
15:     }
16: }
```

- [x]  A. `this(3)` en la línea 3, `this("")` en la línea 14.
- [ ]  B. `this()` en la línea 3, `super(1)` en la línea 14.
- [x]  C. `this((short)1)` en la línea 3, `this(null)` en la línea 14.
- [ ]  D. `super()` en la línea 3, `super()` en la línea 14.
- [ ]  E. `this(2L)` en la línea 3, `super((short)2)` en la línea 14.
- [x]  F. `this(5)` en la línea 3, `super(null)` en la línea 14.
- [ ]  G. Eliminar las líneas 3 y 14.

**11. ¿Cuál es el resultado de lo siguiente?**

```Java
1:  public class PolarBear {
2:      StringBuilder value = new StringBuilder("t");
3:      { value.append("a"); }
4:      { value.append("c"); }
5:      private PolarBear() {
6:          value.append("b");
7:      }
8:      public PolarBear(String s) {
9:          this();
10:         value.append(s);
11:     }
12:     public PolarBear(CharSequence p) {
13:         value.append(p);
14:     }
15:     public static void main(String[] args) {
16:         Object bear = new PolarBear();
17:         bear = new PolarBear("f");
18:         System.out.println(((PolarBear)bear).value);
19:     } 
20: }
```

- [ ]  A. tacb
- [ ]  B. tacf
- [x]  C. tacbf
- [ ]  D. tcafb
- [ ]  E. taftacb
- [ ]  F. El código no compila.
- [ ]  G. Se lanza una excepción.

**12. ¿Cuántas líneas del siguiente programa contienen un error de compilación?**

```Java
1:  public class Rodent {
2:      public Rodent(Integer x) {}
3:      protected static Integer chew() throws Exception {
4:          System.out.println("Rodent is chewing");
5:          return 1;
6:      }
7:  }
8:  class Beaver extends Rodent {
9:      public Number chew() throws RuntimeException {
10:         System.out.println("Beaver is chewing on wood");
11:         return 2;
12:     } 
13: }
```

- [ ]  A. Ninguna
- [ ]  B. 1
- [x]  C. 2
- [ ]  D. 3
- [ ]  E. 4
- [ ]  F. 5

**13. ¿Cuáles de estas clases compilan e incluirán un constructor por defecto (_default constructor_) creado por el compilador?** (_Se deben seleccionar todas las opciones que apliquen_).

- [x]  A.

```Java
public class Bird {}
```

- [x]  B.

```Java
public class Bird {
    public bird() {}
}
```

- [x]  C.

```Java
public class Bird {
    public bird(String name) {}
}
```

- [ ]  D.

```Java
public class Bird {
    public Bird() {}
}
```

- [ ]  E.

```Java
public class Bird {
    Bird(String name) {}
}
```

- [ ]  F.

```Java
public class Bird {
    private Bird(int age) {}
}
```

- [ ]  G.

```Java
public class Bird {
    public Bird bird() { return null; }
}
```

**14. ¿Cuáles de las siguientes afirmaciones sobre la herencia son correctas?** (_Se deben seleccionar todas las opciones que apliquen_).

- [ ]  A. Una clase puede extender directamente cualquier cantidad de clases.
- [x]  B. Una clase puede implementar cualquier cantidad de interfaces.
- [ ]  C. Todas las variables heredan de `  .lang.Object`.
- [x]  D. Si la clase A es extendida por B, entonces B es una superclase de A.
- [ ]  E. Si la clase C implementa la interfaz D, entonces C es un subtipo de D.
- [x]  F. La herencia múltiple es la propiedad de una clase de tener múltiples superclases directas.

**15. ¿Qué afirmación sobre el siguiente programa es correcta?**

```Java
1:  abstract class Nocturnal {
2:      boolean isBlind();
3:  }
4:  public class Owl extends Nocturnal {
5:      public boolean isBlind() { return false; }
6:      public static void main(String[] args) {
7:          var nocturnal = (Nocturnal)new Owl();
8:          System.out.println(nocturnal.isBlind());
9:      } 
10: }
```

- [ ]  A. Compila e imprime `true`.
- [ ]  B. Compila e imprime `false`.
- [ ]  C. El código no compilará debido a la línea 2.
- [ ]  D. El código no compilará debido a la línea 5.
- [x]  E. El código no compilará debido a la línea 7.
- [ ]  F. El código no compilará debido a la línea 8.
- [ ]  G. Ninguna de las anteriores.

**16. ¿Cuál es el resultado de lo siguiente?**

```Java
1:  class Arachnid {
2:      static StringBuilder sb = new StringBuilder();
3:      { sb.append("c"); }
4:      static
5:      { sb.append("u"); }
6:      { sb.append("r"); }
7:  }
8:  public class Scorpion extends Arachnid {
9:      static
10:     { sb.append("q"); }
11:     { sb.append("m"); }
12:     public static void main(String[] args) {
13:         System.out.print(Scorpion.sb + " ");
14:         System.out.print(Scorpion.sb + " ");
15:         new Arachnid();
16:         new Scorpion();
17:         System.out.print(Scorpion.sb);
18:     } 
19: }
```

- [ ]  A. qu qu qumrcrc
- [ ]  B. u u ucrcrm
- [ ]  C. uq uq uqmcrcr
- [x]  D. uq uq uqcrcrm
- [ ]  E. qu qu qumcrcr
- [ ]  F. qu qu qucrcrm
- [ ]  G. El código no compila.

**17. ¿Cuáles de las siguientes afirmaciones son verdaderas?** (_Se deben seleccionar todas las opciones que apliquen_).

- [ ]  A. `this()` puede ser llamado desde cualquier lugar en un constructor.
- [ ]  B. `this()` puede ser llamado desde cualquier lugar en un método de instancia.
- [x]  C. `this.variableName` puede ser llamado desde cualquier método de instancia en la clase.
- [ ]  D. `this.variableName` puede ser llamado desde cualquier método `static` en la clase.
- [x]  E. **Se puede llamar** al constructor por defecto escrito por el compilador usando `this()`.
- [ ]  F. **Se puede acceder** a un constructor `private` con el método `main()` en la misma clase.

**18. ¿Qué afirmaciones sobre las siguientes clases son correctas?** (_Se deben seleccionar todas las opciones que apliquen_).

```Java
1:  public class Mammal {
2:      private void eat() {}
3:      protected static void drink() {}
4:      public Integer dance(String p) { return null; }
5:  }
6:  class Primate extends Mammal {
7:      public void eat(String p) {}
8:  }
9:  class Monkey extends Primate {
10:     public static void drink() throws RuntimeException {}
11:     public Number dance(CharSequence p) { return null; }
12:     public int eat(String p) {}
13: }
```

- [ ]  A. El método `eat()` en `Mammal` es sobrescrito (_overridden_) correctamente en la línea 7.
- [ ]  B. El método `eat()` en `Mammal` es sobrecargado (_overloaded_) correctamente en la línea 7.
- [ ]  C. El método `drink()` en `Mammal` es sobrescrito (_overridden_) correctamente en la línea 10.
- [x]  D. El método `drink()` en `Mammal` es ocultado (_hidden_) correctamente en la línea 10.
- [ ]  E. El método `dance()` en `Mammal` es sobrescrito (_overridden_) correctamente en la línea 11.
- [ ]  F. El método `dance()` en `Mammal` es sobrecargado (_overloaded_) correctamente en la línea 11.
- [ ]  G. El método `eat()` en `Primate` es ocultado (_hidden_) correctamente en la línea 12.
- [ ]  H. El método `eat()` en `Primate` es sobrecargado (_overloaded_) correctamente en la línea 12.

**19. ¿Cuál es la salida del siguiente código?**

```Java
1:  class Reptile {
2:      {System.out.print("A");}
3:      public Reptile(int hatch) {}
4:      void layEggs() {
5:          System.out.print("Reptile");
6:      } 
7:  }
8:  public class Lizard extends Reptile {
9:      static {System.out.print("B");}
10:     public Lizard(int hatch) {}
11:     public final void layEggs() {
12:         System.out.print("Lizard");
13:     }
14:     public static void main(String[] args) {
15:         var reptile = new Lizard(1);
16:         reptile.layEggs();
17:     } 
18: }
```

- [ ]  A. AALizard
- [ ]  B. BALizard
- [ ]  C. BLizardA
- [ ]  D. ALizard
- [ ]  E. El código no compilará debido a la línea 3.
- [x]  F. Ninguna de las anteriores.

**20. ¿Qué afirmación sobre el siguiente programa es correcta?**

```Java
1:  class Bird {
2:      int feathers = 0;
3:      Bird(int x) { this.feathers = x; }
4:      Bird fly() {
5:          return new Bird(1);
6:      } 
7:  }
8:  class Parrot extends Bird {
9:      protected Parrot(int y) { super(y); }
10:     protected Parrot fly() {
11:         return new Parrot(2);
12:     } 
13: }
14: public class Macaw extends Parrot {
15:     public Macaw(int z) { super(z); }
16:     public Macaw fly() {
17:         return new Macaw(3);
18:     }
19:     public static void main(String... sing) {
20:         Bird p = new Macaw(4);
21:         System.out.print(((Parrot)p.fly()).feathers);
22:     } 
23: }
```

- [ ]  A. Una línea contiene un error del compilador.
- [ ]  B. Dos líneas contienen errores del compilador.
- [x]  C. Tres líneas contienen errores del compilador.
- [ ]  D. El código compila pero lanza una `ClassCastException` en tiempo de ejecución.
- [ ]  E. El programa compila e imprime 3.
- [ ]  F. El programa compila e imprime 0.

**21. ¿Cuáles de las siguientes son propiedades de las clases inmutables?** (_Se deben seleccionar todas las opciones que apliquen_).

- [ ]  A. La clase puede contener métodos _setter_, siempre que estén marcados como `final`.
- [x]  B. La clase no debe poder ser extendida fuera de la declaración de la clase.
- [ ]  C. La clase no puede contener ninguna variable de instancia.
- [ ]  D. La clase debe estar marcada como `static`.
- [ ]  E. La clase no puede contener ninguna variable `static`.
- [x]  F. La clase solo puede contener constructores `private`.
- [ ]  G. Los datos para variables de instancia mutables pueden ser leídos, siempre que no puedan ser modificados por el llamador (_caller_).

**22. ¿Qué imprime el siguiente programa?**

```Java
1:  class Person {
2:      static String name;
3:      void setName(String q) { name = q; } 
4:  }
5:  public class Child extends Person {
6:      static String name;
7:      void setName(String w) { name = w; }
8:      public static void main(String[] p) {
9:          final Child m = new Child();
10:         final Person t = m;
11:         m.name = "Elysia";
12:         t.name = "Sophia";
13:         m.setName("Webby");
14:         t.setName("Olivia");
15:         System.out.println(m.name + " " + t.name);
16:     } 
17: }
```

- [ ]  A. Elysia Sophia
- [x]  B. Webby Olivia
- [ ]  C. Olivia Olivia
- [ ]  D. Olivia Sophia
- [ ]  E. El código no compila.
- [ ]  F. Ninguna de las anteriores.

**23. ¿Cuál es la salida del siguiente programa?**

```Java
1:  class Canine {
2:      public Canine(boolean t) { logger.append("a"); }
3:      public Canine() { logger.append("q"); }
4: 
5:      private StringBuilder logger = new StringBuilder();
6:      protected void print(String v) { logger.append(v); }
7:      protected String view() { return logger.toString(); }
8:  }
9: 
10: class Fox extends Canine {
11:     public Fox(long x) { print("p"); }
12:     public Fox(String name) {
13:         this(2);
14:         print("z");
15:     }
16: }
17: 
18: public class Fennec extends Fox {
19:     public Fennec(int e) {
20:         super("tails");
21:         print("j");
22:     }
23:     public Fennec(short f) {
24:         super("eevee");
25:         print("m");
26:     }
27: 
28:     public static void main(String... unused) {
29:         System.out.println(new Fennec(1).view());
30:     } 
31: }
```

- [ ]  A. qpz
- [ ]  B. qpzj
- [ ]  C. jzpa
- [ ]  D. apj
- [ ]  E. apjm
- [x]  F. El código no compila.
- [ ]  G. Ninguna de las anteriores.

**24. ¿Qué imprime el siguiente programa?**

```Java
1:  class Antelope {
2:      public Antelope(int p) {
3:          System.out.print("4");
4:      }
5:      { System.out.print("2"); }
6:      static { System.out.print("1"); }
7:  }
8:  public class Gazelle extends Antelope {
9:      public Gazelle(int p) {
10:         super(6);
11:         System.out.print("3");
12:     }
13:     public static void main(String hopping[]) {
14:         new Gazelle(0);
15:     }
16:     static { System.out.print("8"); }
17:     { System.out.print("9"); }
18: }
```

- [ ]  A. 182640
- [ ]  B. 182943
- [x]  C. 182493
- [ ]  D. 421389
- [ ]  E. El código no compila.
- [ ]  F. La salida no puede ser determinada hasta el tiempo de ejecución (_runtime_).

**25. ¿Cuáles de las siguientes afirmaciones son verdaderas acerca de una clase concreta?** (_Se deben seleccionar todas las opciones que apliquen_).

- [ ]  A. Una clase concreta puede ser declarada como `abstract`.
- [x]  B. Una clase concreta debe implementar todos los métodos abstractos heredados.
- [x]  C. Una clase concreta puede ser marcada como `final`.
- [ ]  D. Una clase concreta debe ser inmutable.
- [ ]  E. Un método concreto que implementa un método abstracto debe coincidir exactamente con la declaración de método del método abstracto.

**26. ¿Cuál es la salida del siguiente código?**

```Java
4:  public abstract class Whale {
5:      public abstract void dive();
6:      public static void main(String[] args) {
7:          Whale whale = new Orca();
8:          whale.dive(3);
9:      }
10: }
11: class Orca extends Whale {
12:     static public int MAX = 3;
13:     public void dive() {
14:         System.out.println("Orca diving");
15:     }
16:     public void dive(int... depth) {
17:         System.out.println("Orca diving deeper "+MAX);
18:     } 
19: }
```

- [ ]  A. Orca diving
- [ ]  B. Orca diving deeper 3
- [ ]  C. El código no compilará debido a la línea 4.
- [ ]  D. El código no compilará debido a la línea 8.
- [ ]  E. El código no compilará debido a la línea 11.
- [ ]  F. El código no compilará debido a la línea 12.
- [ ]  G. El código no compilará debido a la línea 17.
- [x]  H. Ninguna de las anteriores.