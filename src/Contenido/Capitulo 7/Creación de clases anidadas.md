Una **clase anidada** (_nested class_) es una clase que está definida dentro de otra clase. Una clase anidada puede presentarse en cuatro variantes, todas ellas con soporte para variables de instancia y `static` como miembros.

- *Clase interna* (_inner class_): Un tipo no `static` definido al nivel de miembro de una clase.
- *Clase anidada estática* (_static nested class_): Un tipo `static` definido al nivel de miembro de una clase.
- *Clase local* (_local class_): Una clase definida dentro del cuerpo de un método.
- *Clase anónima* (_anonymous class_): Un caso especial de clase local que no tiene nombre.

Existen muchos beneficios al usar clases anidadas. Pueden definir clases auxiliares y restringirlas a la clase que las contiene, mejorando así el encapsulamiento. También facilitan la creación de una clase que solo **se usará** en un lugar. Incluso pueden hacer que el código sea más limpio y fácil de leer.

Sin embargo, cuando **se usan** de forma incorrecta, las clases anidadas pueden hacer el código más difícil de leer. También tienden a acoplar estrechamente la clase externa e interna, pero podría haber casos en los que **se quisiera** usar la clase interna por sí sola. En ese caso, **se debería** mover la clase interna a una clase de nivel superior separada.

Desafortunadamente, el examen evalúa casos extremos donde los programadores normalmente no usarían una clase anidada. Esto tiende a crear código difícil de leer, ¡así que nunca **se debe hacer** esto en la práctica!

> Por convención y a lo largo de este capítulo, a menudo **se usa** el término _nested class_ para hacer referencia a todos los tipos anidados, incluyendo interfaces anidadas, `enums`, `records` y anotaciones. Incluso **se podría** encontrar literatura que los llama a todos _inner classes_. **Se reconoce** que esto puede ser confuso.

#### Declarando una Clase Interna

Una **clase interna** (_inner class_), también llamada **clase interna miembro** (_member inner class_), es un tipo no `static` definido al nivel de miembro de una clase (el mismo nivel que los métodos, variables de instancia y constructores). Puesto que no son tipos de nivel superior, pueden usar cualquiera de los cuatro niveles de acceso, no solo `public` y de paquete.

Las clases internas tienen las siguientes propiedades:

- Pueden ser declaradas `public`, `protected`, de paquete o `private`.
- Pueden extender una clase e implementar interfaces.
- Pueden ser marcadas como `abstract` o `final`.
- Pueden acceder a los miembros de la clase externa, incluyendo los miembros `private`.

La última propiedad es bastante interesante. Significa que la clase interna puede acceder a las variables de la clase externa sin hacer nada especial. ¿Listo para una forma complicada de imprimir "Hi" tres veces?

```Java
1:  public class Home {
2:      private String greeting = "Hi"; // Variable de instancia de la clase externa
3:
4:      protected class Room {          // Declaración de la clase interna
5:          public int repeat = 3;
6:          public void enter() {
7:              for (int i = 0; i < repeat; i++) greet(greeting);
8:          }
9:          private static void greet(String message) {
10:             System.out.println(message);
11:         }
12:     }
13:
14:     public void enterRoom() {       // Método de instancia en la clase externa
15:         var room = new Room();      // Crear la instancia de la clase interna
16:         room.enter();
17:     }
18:     public static void main(String[] args) {
19:         var home = new Home();      // Crear la instancia de la clase externa
20:         home.enterRoom();
21: } }
```

Una declaración de clase interna luce exactamente igual que una declaración de clase independiente, excepto que está ubicada dentro de otra clase. La línea 7 muestra que la clase interna simplemente **hace referencia** a `greeting` como si estuviera disponible en la clase `Room`. Esto funciona porque, de hecho, lo está. Aunque la variable es `private`, **se accede** dentro de esa misma clase.

Puesto que una clase interna no es `static`, debe ser llamada usando una instancia de la clase externa. Eso significa que hay que crear dos objetos. La línea 19 crea el objeto externo `Home`, mientras que la línea 15 crea el objeto interno `Room`. Es importante notar que la línea 15 no requiere una instancia explícita de `Home` porque es un método de instancia dentro de `Home`. Esto funciona porque `enterRoom()` es un método de instancia dentro de la clase `Home`. Tanto `Room` como `enterRoom()` son miembros de `Home`.

**Instanciando una Instancia de una Clase Interna**

Existe otra forma de instanciar `Room` que luce extraña al principio. Bueno, quizás no solo al principio. Esta sintaxis **no se usa** con la suficiente frecuencia para acostumbrarse a ella:

```Java
20:     public static void main(String[] args) {
21:         var home = new Home();
22:         Room room = home.new Room(); // Crear la instancia de la clase interna
23:         room.enter();
24:     }
```

**Se examinará** más de cerca las líneas 21 y 22. **Se necesita** una instancia de `Home` para crear un `Room`. No **se puede** simplemente llamar a `new Room()` dentro del método `static main()`, porque Java no sabrá a qué instancia de `Home` está asociada. Java resuelve esto llamando a `new` como si fuera un método en la variable `home`. **Se pueden acortar** las líneas 21–23 en una sola línea:

```Java
21: new Home().new Room().enter(); // ¡A nosotros también nos parece feo!
```

> **Creando Archivos .class para Clases Internas**
>
> Al compilar la clase `Home.java` con la que **se ha estado** trabajando, **se crean** dos archivos de clase. **Se debería** esperar el archivo `Home.class`. Para la clase interna, el compilador crea `Home$Room.class`. No es necesario conocer esta sintaxis, **se menciona** para que no sorprenda ver archivos con `$` en los directorios. Sí **se debe** entender que **se crean** múltiples archivos de clase a partir de un único archivo `.java`.

**Referenciando Miembros de una Clase Interna**

Las clases internas pueden tener los mismos nombres de variables que las clases externas, lo que hace que el ámbito sea un poco complicado. Existe una forma especial de llamar a `this` para indicar a qué variable **se quiere** acceder. Esto es algo que podría verse en el examen, pero idealmente no en el mundo real.

De hecho, **no se está** limitado a tener solo una clase interna. Si bien lo siguiente es común en el examen, por favor nunca **se haga** esto en código propio. Así es como **se anidan** múltiples clases y **se accede** a una variable con el mismo nombre en cada una:

```Java
1:  public class A {
2:      private int x = 10;
3:      class B {
4:          private int x = 20;
5:          class C {
6:              private int x = 30;
7:              public void allTheX() {
8:                  System.out.println(x);        // 30
9:                  System.out.println(this.x);   // 30
10:                 System.out.println(B.this.x); // 20
11:                 System.out.println(A.this.x); // 10
12:             } } }
13:     public static void main(String[] args) {
14:         A a = new A();
15:         A.B b = a.new B();
16:         A.B.C c = b.new C();
17:         c.allTheX();
18: } }
```

Sí, este código también me hace estremecer. Tiene dos clases anidadas. La línea 14 instancia la más externa. La línea 15 usa la sintaxis incómoda para instanciar una `B`. Note que el tipo es `A.B`. **Se podría** haber escrito `B` como el tipo porque está disponible al nivel de miembro de `A`. Java sabe dónde buscarlo. En la línea 16, **se instancia** una `C`. Esta vez, el tipo `A.B.C` es necesario especificarlo. `C` está demasiado anidada para que Java sepa dónde buscar. Luego la línea 17 llama a un método en la variable de instancia `c`.

Las líneas 8 y 9 son el tipo de código al que **se está** acostumbrado. **Se refieren** a la variable de instancia en la clase actual, la declarada en la línea 6. La línea 10 usa `this` de una manera especial. Aún **se quiere** una variable de instancia, pero esta vez **se quiere** la de la clase `B`, que es la variable de la línea 4. La línea 11 hace lo mismo para la clase `A`, obteniendo la variable de la línea 2.

> **Las Clases Internas Requieren una Instancia**
>
> **Se debe examinar** lo siguiente y ver si **se puede** descubrir por qué dos de las tres llamadas a constructores no compilan:
> ```Java
> public class Fox {
>     private class Den {}
>     public void goHome() {
>         new Den();
>     }
>     public static void visitFriend() {
>         new Den(); // NO COMPILA
>     }
> }
>
> public class Squirrel {
>     public void visitFox() {
>         new Den(); // NO COMPILA
>     }
> }
> ```
> La primera llamada al constructor compila porque `goHome()` es un método de instancia, y por lo tanto la llamada está asociada con la instancia `this`. La segunda llamada no compila porque **se llama** dentro de un método `static`. Aún **se puede** llamar al constructor, pero hay que darle explícitamente una referencia a una instancia de `Fox`.
>
> La última llamada al constructor no compila por dos razones. Aunque es un método de instancia, no es un método de instancia dentro de la clase `Fox`. Agregar una referencia de `Fox` no resolvería completamente el problema. `Den` es `private` y no es accesible en la clase `Squirrel`.

#### Creando una Clase Anidada `static`

Una **clase anidada estática** (_static nested class_) es un tipo `static` definido al nivel de miembro. A diferencia de una clase interna, una clase anidada `static` puede ser instanciada sin una instancia de la clase que la contiene. La desventaja, sin embargo, es que no puede acceder a las variables de instancia o métodos declarados en la clase externa.

En otras palabras, es como una clase de nivel superior excepto por lo siguiente:

- El anidamiento crea un espacio de nombres porque el nombre de la clase que la contiene debe usarse para referirse a ella.
- Además puede ser marcada como `private` o `protected`.
- La clase que la contiene puede hacer referencia a los campos y métodos de la clase anidada `static`.

**Se observará** un ejemplo:

```Java
1: public class Park {
2:     static class Ride {
3:         private int price = 6;
4:     }
5:     public static void main(String[] args) {
6:         var ride = new Ride();
7:         System.out.println(ride.price);
8: } }
```

La línea 6 instancia la clase anidada. Puesto que la clase es `static`, no **se necesita** una instancia de `Park` para usarla. **Se puede** acceder a las variables de instancia `private`, como se muestra en la línea 7.

> **Los Records Anidados son Implícitamente `static`**
>
> Si **se ve** un `record` anidado, es implícitamente `static`. Esto significa que puede usarse sin una referencia a la clase externa. También significa que no puede acceder a las variables de miembro de la clase externa. **Se puede comparar** esto con dos implementaciones de `Emu`, una que usa un `record` y otra que usa una clase.
>
> ```Java
> 11: class Emu1 {
> 12:     String name = "Emmy";
> 13:     static Feathers createFeathers() {
> 14:         return new Feathers("grey");
> 15:     }
> 16:     record Feathers(String color) {
> 17:         void fly() {
> 18:             System.out.print(name + " is flying"); // NO COMPILA
> 19:         } } }
> 20:
> 21: class Emu2 {
> 22:     String name = "Emmy";
> 23:     static Feathers createFeathers() {
> 24:         return new Feathers("grey"); // NO COMPILA
> 25:     }
> 26:     class Feathers {
> 27:         void fly() {
> 28:             System.out.print(name + "  is flying");
> 29:         } } }
> ```
>
> La línea 14 compila sin problema porque el `record` es implícitamente `static`. La línea 24 no compila, sin embargo, porque la versión de clase de `Feathers` no es `static` y requeriría una instancia de `Emu2` para crearla. Del mismo modo, la variable externa `name` solo es visible para la clase anidada si no es `static`, como **se muestra** por la línea 28 que compila y la línea 18 que no lo hace.

#### Escribiendo una Clase Local

Una **clase local** (_local class_) es una clase anidada definida dentro de un método. Al igual que las variables locales, una declaración de clase local no existe hasta que el método es invocado, y deja de existir cuando el método retorna. Esto significa que solo **se pueden** crear instancias desde dentro del método. Esas instancias aún pueden ser retornadas por el método. Así es como funcionan las variables locales.

> Las clases locales no están limitadas a ser declaradas solo dentro de métodos. Por ejemplo, pueden ser declaradas dentro de constructores e inicializadores. Por simplicidad, **se limita** la discusión a los métodos en este capítulo.

Las clases locales tienen las siguientes propiedades:

- No tienen modificador de acceso.
- Pueden ser declaradas `final` o `abstract`.
- Pueden incluir miembros de instancia y `static`.
- Tienen acceso a todos los campos y métodos de la clase que las contiene (cuando **se definen** en un método de instancia).
- Pueden acceder a variables locales `final` y efectivamente finales (_effectively final_).

> ¿**Se recuerda** cuando **se presentó** "efectivamente final" (_effectively final_) en el Capítulo 5? Bueno, **se dijo** que sería útil más adelante, ¡y ese momento ha llegado! Si **se necesita** repasar `final` y efectivamente final, **se puede** volver al Capítulo 5 ahora. ¡No hay prisa, **se puede** esperar!

¿Listo para un ejemplo? Aquí hay una forma complicada de multiplicar dos números:

```Java
1:  public class PrintNumbers {
2:      private int length = 5;
3:      public void calculate() {
4:          final int width = 20;
5:          class Calculator {
6:              public void multiply() {
7:                  System.out.print(length * width);
8:              }
9:          }
10:         var calculator = new Calculator();
11:         calculator.multiply();
12:     }
13:     public static void main(String[] args) {
14:         var printer = new PrintNumbers();
15:         printer.calculate(); // 100
16:     }
17: }
```

Las líneas 5–9 son la clase local. El ámbito de esa clase termina en la línea 12, donde termina el método. La línea 7 hace referencia a una variable de instancia y a una variable local `final`, por lo que ambas referencias de variables están permitidas desde dentro de la clase local.

Anteriormente, **se afirmó** que las referencias a variables locales están permitidas si son `final` o efectivamente finales. Como ejemplo ilustrativo, considere lo siguiente:

```Java
public void processData() {
    final int length = 5;
    int width = 10;
    int height = 2;
    class VolumeCalculator {
        public int multiply() {
            return length * width * height; // NO COMPILA
        }
    }
    width = 2;
}
```

Las variables `length` y `height` son `final` y efectivamente final, respectivamente, por lo que ninguna de ellas causa un problema de compilación. Por otro lado, la variable `width` **se reasigna** durante el método, por lo que no puede ser efectivamente final. Por esta razón, la declaración de la clase local no compila.

#### Definiendo una Clase Anónima

Una **clase anónima** (_anonymous class_) es una forma especializada de clase local que no tiene nombre. **Se declara** e instancia en una sola instrucción usando la palabra clave `new`, un nombre de tipo con paréntesis y un conjunto de llaves `{}`. Las clases anónimas deben extender una clase existente o implementar una interfaz existente. Son útiles cuando **se tiene** una implementación corta que no **se usará** en ningún otro lugar. Aquí hay un ejemplo:

```Java
1:  public class ZooGiftShop {
2:      abstract class SaleTodayOnly {
3:          abstract int dollarsOff();
4:      }
5:      public int admission(int basePrice) {
6:          SaleTodayOnly sale = new SaleTodayOnly() {
7:              int dollarsOff() { return 3; }
8:          }; // ¡No olvidar el punto y coma!
9:          return basePrice - sale.dollarsOff();
10: } }
```

Las líneas 2–4 definen una clase `abstract`. Las líneas 6–8 definen la clase anónima. Note que esta clase anónima no tiene nombre. El código dice que **se instancie** un nuevo objeto `SaleTodayOnly`. Pero espera: `SaleTodayOnly` es `abstract`. Esto está bien porque **se proporciona** el cuerpo de la clase ahí mismo, anónimamente. En este ejemplo, escribir una clase anónima es equivalente a escribir una clase local con un nombre no especificado que extiende a `SaleTodayOnly` y la usa inmediatamente.

**Se debe prestar** especial atención al punto y coma de la línea 8. **Se está** declarando una variable local en estas líneas. Las declaraciones de variables locales deben terminar con punto y coma, al igual que otras sentencias de Java, incluso si son largas y contienen una clase anónima.

Ahora **se convierte** este mismo ejemplo para implementar una `interface` en lugar de extender una clase `abstract`:

```Java
1:  public class ZooGiftShop {
2:      interface SaleTodayOnly {
3:          int dollarsOff();
4:      }
5:      public int admission(int basePrice) {
6:          SaleTodayOnly sale = new SaleTodayOnly() {
7:              public int dollarsOff() { return 3; }
8:          };
9:          return basePrice - sale.dollarsOff();
10: } }
```

Lo más interesante aquí es cuán poco ha cambiado. Las líneas 2–4 declaran una `interface` en lugar de una clase `abstract`. La línea 7 es `public` en lugar de usar el acceso predeterminado, ya que las interfaces requieren que los métodos `abstract` sean `public`. Y eso es todo. ¡La clase anónima es la misma ya sea que **se implemente** una interfaz o **se extienda** una clase! Java determina automáticamente cuál **se quiere**. Solo hay que recordar que en este segundo ejemplo, **se crea** una instancia de una clase en la línea 6, no de una interfaz.

¿Pero qué pasa si **se quiere** implementar una `interface` y extender una clase al mismo tiempo? No **se puede** hacerlo con una clase anónima a menos que la clase a extender sea `java.lang.Object`. La clase `Object` no cuenta en la regla. Hay que recordar que una clase anónima es solo una clase local sin nombre. **Se puede** escribir una clase local y darle un nombre si **se tiene** este problema. Luego **se puede** extender una clase e implementar tantas interfaces como **se desee**.

Incluso **se pueden** definir clases anónimas fuera del cuerpo de un método. Lo siguiente puede parecer que **se instancia** una interfaz como variable de instancia, pero el `{}` después del nombre de la interfaz indica que se trata de una clase anónima que implementa la interfaz:

```Java
public class Gorilla {
    interface Climb {}
    Climb climbing = new Climb() {};
}
```

> **Escenario del Mundo Real**
>
> **Clases Anónimas y Expresiones Lambda**
>
> Antes de Java 8, las clases anónimas **se usaban** frecuentemente para tareas asíncronas y manejadores de eventos. Por ejemplo, lo siguiente muestra una clase anónima usada como manejador de eventos en una aplicación JavaFX:
>
> ```Java
> var redButton = new Button();
> redButton.setOnAction(new EventHandler<ActionEvent>() {
>     public void handle(ActionEvent e) {
>         System.out.println("Red button pressed!");
>     }
> });
> ```
>
> Desde la introducción de las expresiones lambda, las clases anónimas a menudo son reemplazadas por implementaciones mucho más cortas:
>
> ```Java
> Button redButton = new Button();
> redButton.setOnAction(e -> System.out.println("Red button pressed!"));
> ```
>
> Las expresiones lambda **se cubren** en detalle en el siguiente capítulo.

#### Revisando las Clases Anidadas

Para el examen, **se debe asegurar** de conocer la información de la Tabla 7.4 sobre qué reglas de sintaxis están permitidas en Java.

**TABLA 7.4 Modificadores en clases anidadas**

|**Modificadores permitidos**|**Clase interna**|**Clase anidada `static`**|**Clase local**|**Clase anónima**|
|---|---|---|---|---|
|Modificadores de acceso|Todos|Todos|Ninguno|Ninguno|
|`abstract`|Sí|Sí|Sí|No|
|`final`|Sí|Sí|Sí|No|

También **se debe conocer** la información de la Tabla 7.5 sobre los tipos de acceso. Por ejemplo, el examen podría intentar engañar con una clase `static` que accede a una variable de instancia de la clase externa sin una referencia a la clase externa.

**TABLA 7.5 Reglas de acceso de clases anidadas**

||**Clase interna**|**Clase anidada `static`**|**Clase local**|**Clase anónima**|
|---|---|---|---|---|
|¿Puede incluir miembros de instancia y `static`?|Sí|Sí|Sí|Sí|
|¿Puede extender una clase o implementar cualquier número de interfaces?|Sí|Sí|Sí|No: debe tener exactamente una superclase o una interfaz|
|¿Puede acceder a los miembros de instancia de la clase que la contiene?|Sí|No|Sí (si **se declara** en un método de instancia)|Sí (si **se declara** en un método de instancia)|
|¿Puede acceder a las variables locales del método que la contiene?|N/A|N/A|Sí (si son `final` o efectivamente finales)|Sí (si son `final` o efectivamente finales)|
