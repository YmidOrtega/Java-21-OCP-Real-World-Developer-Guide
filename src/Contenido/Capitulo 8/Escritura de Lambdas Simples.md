En este capítulo, **se comienza** introduciendo las lambdas, una nueva sintaxis. Las lambdas permiten especificar código que **se ejecutará** más adelante en el programa.

A continuación, **se introduce** el concepto de interfaces funcionales, mostrando cómo escribir las propias e identificar si una interfaz es una interfaz funcional. Después de eso, **se introduce** otra nueva pieza de sintaxis: las referencias a métodos (_method references_). Estas son como una forma abreviada de las lambdas.

Luego **se presentan** las interfaces funcionales que **se necesitan** conocer para el examen. Finalmente, **se enfatiza** cómo las variables encajan en las lambdas.

Las lambdas, las referencias a métodos y las interfaces funcionales **se utilizan** con bastante frecuencia en el Capítulo 9, "Colecciones y Genéricos", y en el Capítulo 10, "Streams".

#### Escritura de Lambdas Simples

Java es un lenguaje orientado a objetos en su esencia. **Se han visto** muchos objetos hasta ahora. La **programación funcional** (_Functional programming_) es una forma de escribir código de manera más declarativa. Se especifica qué se quiere hacer en lugar de tratar con el estado de los objetos. Se enfoca más en las expresiones que en los bucles.

La programación funcional usa expresiones lambda para escribir código. Una **expresión lambda** (_lambda expression_) es un bloque de código que **se pasa** alrededor. **Se puede** pensar en una expresión lambda como un método sin nombre que existe dentro de una clase anónima, como los que **se vieron** en el Capítulo 7, "Más allá de las Clases". Tiene parámetros y un cuerpo igual que los métodos completos, pero no tiene un nombre como un método real. A las expresiones lambda frecuentemente **se les llama** simplemente **lambdas** (_lambdas_). También **se les conoce** como **closures** si Java no es el primer lenguaje de programación utilizado. Si se ha tenido una mala experiencia con los _closures_ en el pasado, no hay que preocuparse. Son mucho más simples en Java.

Las lambdas permiten escribir código poderoso en Java. En esta sección, **se cubre** un ejemplo de por qué las lambdas son útiles y la sintaxis de las lambdas.

#### Analizando un Ejemplo de Lambda

El objetivo es imprimir todos los animales de una lista según algún criterio. **Se muestra** cómo hacerlo sin lambdas para ilustrar por qué son útiles. **Se comienza** con el _record_ `Animal`.

```Java
public record Animal(String species, boolean canHop, boolean canSwim) { }
```

El _record_ `Animal` tiene tres campos. Supongamos que **se tiene** una lista de animales y **se quiere** procesar los datos basándose en un atributo particular. Por ejemplo, **se quieren** imprimir todos los animales que pueden saltar. **Se puede** definir una interfaz para generalizar este concepto y soportar una gran variedad de verificaciones.

```Java
public interface CheckTrait {
    boolean test(Animal a);
}
```

Lo primero que **se quiere** verificar es si el `Animal` puede saltar. **Se proporciona** una clase que implementa la interfaz.

```Java
public class CheckIfHopper implements CheckTrait {
    public boolean test(Animal a) {
        return a.canHop();
    }
}
```

Esta clase puede parecer simple, y lo es. Esta es parte del problema que las lambdas resuelven. Ahora **se tiene** todo lo necesario para escribir el código y averiguar si un `Animal` puede saltar.

```Java
1:  import java.util.*;
2:  public class TraditionalSearch {
3:      public static void main(String[] args) {
4:
5:          // lista de animales
6:          var animals = new ArrayList<Animal>();
7:          animals.add(new Animal("fish", false, true));
8:          animals.add(new Animal("kangaroo", true, false));
9:          animals.add(new Animal("rabbit", true, false));
10:         animals.add(new Animal("turtle", false, true));
11:
12:         // pasar la clase que hace la verificación
13:         print(animals, new CheckIfHopper());
14:     }
15:     private static void print(List<Animal> animals,
            CheckTrait checker) {
16:         for (Animal animal : animals) {
17:
18:             // Verificación general
19:             if (checker.test(animal))
20:                 System.out.print(animal + " ");
21:         }
22:         System.out.println();
23:     }
24: }
```

La línea 6 muestra cómo **se configura** un `ArrayList` con un tipo específico de `Animal`. El método `print()` en la línea 15 es muy general; puede verificar cualquier rasgo. Esto es un buen diseño. No necesita saber qué **se está** buscando específicamente para imprimir una lista de animales.

¿Qué sucede si **se quieren** imprimir los `Animals` que nadan? **Se necesitaría** escribir otra clase, `CheckIfSwims`. Es cierto que son solo unas pocas líneas, pero es un nuevo archivo completo. Luego **se necesita** añadir una nueva línea bajo la línea 13 que instancie esa clase. Eso son dos cosas solo para hacer otra verificación.

¿Por qué no **se puede** especificar la lógica que **se desea** directamente aquí? Resulta que sí se puede, con expresiones lambda. **Se podría** repetir toda la clase aquí y hacer que se encuentre la única línea que cambió. En cambio, solo **se muestra** que el método `print()` puede mantenerse sin cambios. Reemplazando la línea 13 con la siguiente, que usa una lambda:

```Java
13:         print(animals, a -> a.canHop());
```

No hay que preocuparse de que la sintaxis parezca un poco extraña. **Se acostumbrará** a ella, y **se describe** en la siguiente sección. También **se explican** las partes que parecen magia.

Por ahora, solo hay que enfocarse en lo fácil que es de leer. **Se le está diciendo** a Java que solo nos importa si un `Animal` puede saltar.

No **se necesita** mucha imaginación para agregar lógica que obtenga los `Animals` que pueden nadar. Solo **se necesita** añadir una línea de código, sin necesidad de una clase adicional para hacer algo simple. Aquí está esa otra línea:

```Java
13:         print(animals, a -> a.canSwim());
```

¿Y los `Animals` que no pueden nadar?

```Java
13:         print(animals, a -> !a.canSwim());
```

El punto es que es muy fácil escribir código que usa lambdas una vez que **se dominan** los conceptos básicos. Este código usa un concepto llamado **ejecución diferida** (_deferred execution_), lo que significa que el código **se especifica** ahora pero **se ejecutará** más tarde. En este caso, "más tarde" está dentro del cuerpo del método `print()`, en lugar de cuando **se pasa** al método.

#### Aprendiendo la Sintaxis de las Lambdas

Una de las expresiones lambda más simples que **se puede** escribir es la que **se acaba de ver**.

```Java
a -> a.canHop()
```

Las lambdas funcionan con interfaces que tienen exactamente un método abstracto. En este caso, Java mira la interfaz `CheckTrait`, que tiene un método. La lambda en el ejemplo sugiere que Java debería llamar a un método con un parámetro `Animal` que devuelve un valor `boolean` que es el resultado de `a.canHop()`. **Se sabe** todo esto porque **se escribió** el código. Pero ¿cómo lo sabe Java?

Java se basa en el **contexto** (_context_) para determinar qué significan las expresiones lambda. El contexto **se refiere** a dónde y cómo **se interpreta** la lambda. Por ejemplo, si **se ve** a alguien en la fila para entrar al zoológico con la cartera abierta, es razonable asumir que quiere comprar boletos. Alternativamente, si está en la fila de la cafetería con la cartera abierta, probablemente tenga hambre.

Refiriéndose al ejemplo anterior, **se pasó** la lambda como el segundo parámetro del método `print()`.

```Java
print(animals, a -> a.canHop());
```

El método `print()` espera un `CheckTrait` como segundo parámetro.

```Java
private static void print(List<Animal> animals, CheckTrait checker) { … }
```

Dado que **se está** pasando una lambda, Java intenta mapear la lambda a la declaración del método abstracto en la interfaz `CheckTrait`.

```Java
boolean test(Animal a);
```

Dado que el método de esa interfaz toma un `Animal`, el parámetro de la lambda debe ser un `Animal`. Y dado que el método de esa interfaz devuelve un `boolean`, **se sabe** que la lambda devuelve un `boolean`.

La sintaxis de las lambdas es complicada porque muchas partes son opcionales. Estas dos líneas hacen exactamente lo mismo:

```Java
a -> a.canHop()
(Animal a) -> { return a.canHop(); }
```

**Se analizará** qué está sucediendo aquí. El primer ejemplo, mostrado en la imagen a continuación, tiene tres partes.

- Un único parámetro especificado con el nombre `a`
- El operador de flecha (`->`) para separar el parámetro y el cuerpo
- Un cuerpo que llama a un único método y devuelve el resultado de ese método

![[Sintaxis lambda que omite partes opcionales.png]]

El segundo ejemplo muestra la forma más detallada de una lambda que devuelve un `boolean`.

- Un único parámetro especificado con el nombre `a` y declarando que el tipo es `Animal`
- El operador de flecha (`->`) para separar el parámetro y el cuerpo
- Un cuerpo que tiene una o más líneas de código, incluyendo un punto y coma (`;`) y una sentencia `return`

!![[Sintaxis de lambda incluidas las partes opcionales.png]]

Los paréntesis alrededor de los parámetros lambda **se pueden** omitir solo si hay un único parámetro y su tipo no **se declara** explícitamente. Java hace esto porque los desarrolladores comúnmente usan las expresiones lambda de esta manera y pueden escribir lo menos posible.

No debería ser una novedad que **se puedan** omitir las llaves cuando solo hay una única sentencia. Esto ya **se hizo** con las sentencias `if` y los bucles. Java permite omitir una sentencia `return` y el punto y coma (`;`) cuando no **se usan** llaves. Este atajo especial no funciona cuando **se tienen** dos o más sentencias. Al menos esto es consistente con el uso de `{}` para crear bloques de código en otros lugares.

La sintaxis de las 2 imagenes **se pueden** combinar. Por ejemplo, los siguientes ejemplos son válidos:

```Java
a -> { return a.canHop(); }
(Animal a) -> a.canHop()
```

> **Dato curioso:** `s -> {}` es una lambda válida. Si no hay código en el lado derecho de la expresión, no **se necesita** el punto y coma ni la sentencia `return`.

La Tabla 8.1 muestra ejemplos de lambdas válidas que devuelven un `boolean`.

**TABLA 8.1** Lambdas válidas que devuelven un `boolean`

| **Lambda** | **# de parámetros** |
|---|---|
| `() -> true` | 0 |
| `x -> x.startsWith("test")` | 1 |
| `(String x) -> x.startsWith("test")` | 1 |
| `(x, y) -> { return x.startsWith("test"); }` | 2 |
| `(String x, String y) -> x.startsWith("test")` | 2 |

La primera fila no toma parámetros y siempre devuelve el valor `boolean` `true`. La segunda fila toma un parámetro y llama a un método en él, devolviendo el resultado. La tercera fila hace lo mismo, excepto que define explícitamente el tipo de la variable. Las últimas dos filas toman dos parámetros e ignoran uno de ellos: no existe una regla que diga que se deben usar todos los parámetros definidos.

Ahora **se asegurará** de poder identificar la sintaxis inválida para cada fila en la Tabla 8.2, donde se supone que cada lambda debe devolver un `boolean`. **Se debe** asegurar de entender qué está mal con estas.

**TABLA 8.2** Lambdas inválidas que deberían devolver un `boolean`

| **Lambda inválida** | **Razón** |
|---|---|
| `x, y -> x.startsWith("fish")` | Faltan paréntesis en el lado izquierdo |
| `x -> { x.startsWith("camel"); }` | Falta `return` en el lado derecho |
| `x -> { return x.startsWith("giraffe") }` | Falta punto y coma dentro de las llaves |
| `String x -> x.endsWith("eagle")` | Faltan paréntesis en el lado izquierdo |

**Se debe recordar** que los paréntesis son opcionales *solo* cuando hay un parámetro y no tiene un tipo declarado. Esos son los conceptos básicos para escribir una lambda. Al final del capítulo, **se cubren** reglas adicionales sobre el uso de variables en una lambda.

> **Asignando Lambdas a `var`**
>
> ¿Por qué cree que esta línea de código no compila?
>
> ```Java
> var invalid = (Animal a) -> a.canHop();  // NO COMPILA
> ```
>
> **Se recuerda** cuando **se habló** de que Java infiere información sobre la lambda a partir del contexto. Pues bien, `var` también asume el tipo basándose en el contexto. ¡No hay suficiente contexto aquí! Ni la lambda ni `var` tienen suficiente información para determinar qué tipo de interfaz funcional debería usarse.
