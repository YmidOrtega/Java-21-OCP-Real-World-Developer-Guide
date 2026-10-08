¿Qué **se hace** cuando **se encuentra** una excepción? ¿Cómo **se maneja** o **se recupera** de ella? En esta sección **se cubren** las distintas sentencias de Java que soportan el manejo de excepciones.

## Usando Sentencias *try* y *catch*

Ahora que **se sabe** qué son las excepciones, **se explora** cómo **manejarlas**. Java usa una sentencia `try` para separar la lógica que podría lanzar una excepción de la lógica que **se encarga** de manejarla. La imagen muestra la sintaxis de una _sentencia try_.

![[La sintaxis de una instrucción `try`.jpeg]]

El código del bloque `try` **se ejecuta** normalmente. Si alguna de las sentencias lanza una excepción que puede ser atrapada por el tipo de excepción listado en el bloque `catch`, el bloque `try` deja de ejecutarse y la ejecución pasa a la sentencia `catch`. Si ninguna de las sentencias en el bloque `try` lanza una excepción que pueda atraparse, la _cláusula catch_ no **se ejecuta**.

Probablemente **se haya notado** que las palabras _bloque_ y _cláusula_ **se usan** de forma intercambiable. El examen también **lo hace**, así que **hay que acostumbrarse**. Ambas son correctas: _bloque_ es correcto porque hay llaves presentes, y _cláusula_ es correcta porque es parte de una sentencia `try`.

No hay muchas reglas de sintaxis aquí. Las llaves **son obligatorias** para los bloques `try` y `catch`, mientras que las sentencias `if` y los bucles son especiales y permiten omitirlas. En nuestro ejemplo, la niña pequeña se levanta por sí misma la primera vez que se cae. He aquí cómo **se ve** el ejemplo en código:

```Java
3:  void explore() {
4:      try {
5:          fall();
6:          System.out.println("nunca llega aquí");
7:      } catch (RuntimeException e) {
8:          getUp();
9:      }
10:     seeAnimals();
11: }
12: void fall() { throw new RuntimeException(); }
```

La línea 5 llama al método `fall()`. La línea 12 lanza una excepción. Esto hace que Java salte directamente al bloque `catch`, omitiendo la línea 6. La niña se levanta en la línea 8. Luego la sentencia `try` termina, y la ejecución continúa normalmente con la línea 10.

Ahora **se observan** sentencias `try` no válidas que el examen puede intentar usar para confundir. ¿Ves qué está mal aquí?

```Java
try  // NO COMPILA
    fall();
catch (Exception e)
    System.out.println("levántate");
```

El problema es que faltan las llaves `{}`. Las sentencias `try` son como métodos en que las llaves **son obligatorias** incluso si solo hay una sentencia dentro de los bloques, mientras que las sentencias `if` y los bucles son especiales y permiten omitir las llaves. Y este otro:

```Java
try { // NO COMPILA
    fall();
}
```

Este código no compila porque el bloque `try` no tiene nada después. El punto de una sentencia `try` es que algo **suceda** si **se lanza** una excepción. Sin otra cláusula, la sentencia `try` **está** sola. Como **se verá** próximamente, hay un tipo especial de sentencia `try` que incluye un bloque `finally` implícito, aunque la sintaxis es muy diferente de este ejemplo.

## Encadenando Bloques *catch*

Para el examen **se podrían** dar clases de excepción y **hay que** entender cómo funcionan. Este es el procedimiento: primero, **hay que ser** capaz de reconocer si la excepción es verificada o no verificada; segundo, **hay que** determinar si alguna de las excepciones es subclase de las otras.

```Java
class AnimalsOutForAWalk extends RuntimeException {}
class ExhibitClosed extends RuntimeException {}
class ExhibitClosedForLunch extends ExhibitClosed {}
```

En este ejemplo hay tres excepciones personalizadas, todas no verificadas porque extienden directamente o indirectamente `RuntimeException`. Ahora **se encadenan** ambos tipos de excepciones con dos bloques `catch` y **se manejan** imprimiendo el mensaje apropiado:

```Java
public void visitPorcupine() {
    try {
        seeAnimal();
    } catch (AnimalsOutForAWalk e) { // primer bloque catch
        System.out.print("intentar más tarde");
    } catch (ExhibitClosed e) {      // segundo bloque catch
        System.out.print("hoy no");
    }
}
```

Hay tres posibilidades cuando **se ejecuta** este código: si `seeAnimal()` no lanza una excepción, no **se imprime** nada. Si el animal **está** de paseo, solo **se ejecuta** el primer bloque `catch`. Si el exhibidor **está** cerrado, solo **se ejecuta** el segundo bloque `catch`. No es posible que ambos bloques `catch` **se ejecuten** cuando **se encadenan** de esta manera.

Existe una regla para el orden de los bloques `catch`: Java los **examina** en el orden en que aparecen. Si es imposible que uno de los bloques `catch` **sea** ejecutado, **se produce** un error del compilador sobre código inalcanzable. Por ejemplo, esto sucede cuando un bloque `catch` de la superclase aparece antes que un bloque `catch` de la subclase. **Hay que recordar** la advertencia de **prestar** atención a cualquier excepción de subclase.

En el ejemplo del puercoespín, el orden de los bloques `catch` **podría invertirse** porque las excepciones no se heredan entre sí. Y sí, **se ha visto** a un puercoespín sacado a paseo con correa.

El siguiente ejemplo muestra tipos de excepción que sí heredan entre sí:

```Java
public void visitMonkeys() {
    try {
        seeAnimal();
    } catch (ExhibitClosedForLunch e) { // Excepción subclase
        System.out.print("intentar más tarde");
    } catch (ExhibitClosed e) {          // Excepción superclase
        System.out.print("hoy no");
    }
}
```

Si **se lanza** la excepción más específica `ExhibitClosedForLunch`, **se ejecuta** el primer bloque `catch`. Si no, Java verifica si **se lanzó** la excepción superclase `ExhibitClosed` y la atrapa. Esta vez, el orden de los bloques `catch` sí importa. El orden inverso no funciona:

```Java
public void visitMonkeys() {
    try {
        seeAnimal();
    } catch (ExhibitClosed e) {
        System.out.print("hoy no");
    } catch (ExhibitClosedForLunch e) { // NO COMPILA
        System.out.print("intentar más tarde");
    }
}
```

Si **se lanza** `ExhibitClosedForLunch`, el bloque `catch` de `ExhibitClosed` **se ejecuta** — lo que significa que no hay forma de que el segundo bloque `catch` llegue a ejecutarse nunca. Java correctamente **informa** un bloque `catch` inalcanzable.

**Hay que intentar** este más. ¿Ves por qué este código no compila?

```Java
public void visitSnakes() {
    try {
    } catch (IllegalArgumentException e) {
    } catch (NumberFormatException e) { // NO COMPILA
    }
}
```

**Hay que recordar** que `NumberFormatException` es una subclase de `IllegalArgumentException`. Dado que siempre **sería** atrapada por el primer bloque `catch`, el segundo bloque `catch` es código inalcanzable y no compila. De igual manera, **hay que saber** que `FileNotFoundException` es subclase de `IOException` y no **puede** usarse de manera similar.

Para revisar múltiples bloques `catch`, **hay que recordar** que a lo sumo un bloque `catch` **se ejecutará**, y será el primer bloque `catch` que pueda manejar la excepción. También, **hay que recordar** que una excepción definida por la sentencia `catch` solo **está** en alcance para ese bloque `catch`. Por ejemplo, el siguiente **produce** un error de compilación porque intenta usar la referencia de objeto de excepción fuera del bloque para el cual fue definida:

```Java
public void visitManatees() {
    try {
    } catch (NumberFormatException e1) {
        System.out.println(e1);
    } catch (IllegalArgumentException e2) {
        System.out.println(e1); // NO COMPILA
    }
}
```

## Aplicando un Bloque Multi-catch

A menudo **se quiere** que el resultado de una excepción lanzada sea el mismo, independientemente de cuál excepción particular **se lanzó**. Por ejemplo, **se observa** el siguiente método:

```Java
public static void main(String args[]) {
    try {
        System.out.println(Integer.parseInt(args[1]));
    } catch (ArrayIndexOutOfBoundsException e) {
        System.out.println("Entrada faltante o no válida");
    } catch (NumberFormatException e) {
        System.out.println("Entrada faltante o no válida");
    }
}
```

**Se nota** que hay la misma sentencia `println()` en dos bloques `catch` diferentes. Esto **se puede manejar** de forma más elegante usando un bloque _multi-catch_. Un bloque _multi-catch_ permite que múltiples tipos de excepción sean atrapados por el mismo bloque `catch`. **Se reescribe** el ejemplo anterior usando un bloque multi-catch:

```Java
public static void main(String[] args) {
    try {
        System.out.println(Integer.parseInt(args[1]));
    } catch (ArrayIndexOutOfBoundsException | NumberFormatException e) {
        System.out.println("Entrada faltante o no válida");
    }
}
```

Esto está mucho mejor. No hay código duplicado, la lógica común está toda en un solo lugar y la lógica está exactamente donde **se espera** encontrarla. Si **se quisiera**, todavía **se podría** tener un segundo bloque `catch` para `Exception` en caso de querer manejar otros tipos de excepciones de manera diferente.

La imagen muestra la sintaxis del multi-catch. Es como una cláusula `catch` regular, excepto que **se especifican** dos o más tipos de excepción, separados por una barra vertical (`|`). La barra `|` también **se usa** como el operador "o", lo que hace fácil recordar que **se puede** usar cualquiera de los tipos de excepción. **Hay que notar** que solo hay un nombre de variable en la cláusula `catch`: Java **está diciendo** que la variable llamada `e` puede ser de tipo `Exception1` o `Exception2`.

![[La sintaxis de un bloque multi-catch.jpeg]]

El examen puede intentar confundir con sintaxis no válida. **Hay que recordar** que las excepciones **pueden** listarse en cualquier orden dentro de la cláusula `catch`. Sin embargo, el nombre de la variable debe aparecer solo una vez y al final. ¿Se pueden ver cuáles son válidas o inválidas?

```Java
catch(Exception1 e | Exception2 e | Exception3 e)   // NO COMPILA
catch(Exception1 e1 | Exception2 e2 | Exception3 e3) // NO COMPILA
catch(Exception1 | Exception2 | Exception3 e)        // compila
```

La primera línea es incorrecta porque el nombre de la variable aparece tres veces. El hecho de que sea el mismo nombre de variable no lo hace aceptable. La segunda línea es incorrecta porque el nombre de la variable otra vez aparece tres veces; usar nombres de variables diferentes no lo mejora. La tercera línea sí compila: muestra la sintaxis correcta para especificar tres tipos de excepción.

Java **pretende** que el multi-catch **se use** para excepciones que no están relacionadas, y por eso **previene** especificar tipos redundantes en un multi-catch. ¿Qué está mal aquí?

```Java
try {
    throw new IOException();
} catch (FileNotFoundException | IOException p) {} // NO COMPILA
```

Especificar excepciones relacionadas en el multi-catch es redundante, y el compilador **da** un mensaje así:

```Java
The exception FileNotFoundException is already caught
    by the alternative IOException
```

Dado que `FileNotFoundException` es una subclase de `IOException`, este código no compila. Un bloque multi-catch sigue reglas similares a las de encadenar bloques `catch`, como **se vio** en la sección anterior; por ejemplo, ambos producen errores del compilador cuando encuentran código inalcanzable o excepciones duplicadas. La única diferencia es que, dentro de una sola expresión `catch`, el orden no importa en un bloque multi-catch.

Para volver al ejemplo, el código correcto es simplemente eliminar la referencia redundante a la subclase:

```Java
try {
    throw new IOException();
} catch (IOException e) {}
```

## Añadiendo un Bloque *finally*

La sentencia `try` también permite ejecutar código al final con una _cláusula finally_, independientemente de si **se lanzó** una excepción. La imagen a continuación muestra la sintaxis.

![[La sintaxis de una instrucción `try` con `finally`.jpeg]]

Hay dos caminos a través del código con un `catch` y un `finally`. Si **se lanza** una excepción, el bloque `finally` **se ejecuta** después del bloque `catch`. Si no **se lanza** ninguna excepción, el bloque `finally` **se ejecuta** después de que el bloque `try` **se completa**.

Retomando el ejemplo de la niña, esta vez con `finally`:

```Java
12: void explore() {
13:     try {
14:         seeAnimals();
15:         fall();
16:     } catch (Exception e) {
17:         getHugFromDaddy();
18:     } finally {
19:         seeMoreAnimals();
20:     }
21:     goHome();
22: }
```

La niña **cae** en la línea 15. Si **se levanta** sola, el código pasa al bloque `finally` y ejecuta la línea 19. Luego la sentencia `try` termina y el código **continúa** con la línea 21. Si la niña no **se levanta** sola, **lanza** una excepción. El bloque `catch` **se ejecuta**, y ella recibe un abrazo en la línea 17. Con ese abrazo, **está** lista para ver más animales en la línea 19. Luego la sentencia `try` termina y el código **continúa** con la línea 21. De cualquier manera, el final es el mismo: el bloque `finally` **se ejecuta** y la ejecución **continúa** después de la sentencia `try`.

El examen intentará confundir con cláusulas faltantes o en el orden incorrecto. ¿Por qué los siguientes no compilan o sí compilan?

```Java
25: try {  // NO COMPILA
26:     fall();
27: } finally {
28:     System.out.println("todo mejor");
29: } catch (Exception e) {
30:     System.out.println("levántate");
31: }
32:
33: try {  // NO COMPILA
34:     fall();
35: }
36:
37: try {
38:     fall();
39: } finally {
40:     System.out.println("todo mejor");
41: }
```

El primer ejemplo (líneas 25–31) no compila porque los bloques `catch` y `finally` **están** en el orden incorrecto. El segundo ejemplo (líneas 33–35) no compila porque debe haber un bloque `catch` o `finally`. El tercer ejemplo (líneas 37–41) está perfectamente bien. El bloque `catch` no **es requerido** si `finally` **está** presente.

La mayoría de los ejemplos con `finally` que **se encuentren** en el examen parecerán forzados. Por ejemplo, **se pregunta** qué imprime el siguiente código:

```Java
public static void main(String[] unused) {
    StringBuilder sb = new StringBuilder();
    try {
        sb.append("t");
    } catch (Exception e) {
        sb.append("c");
    } finally {
        sb.append("f");
    }
    sb.append("a");
    System.out.print(sb.toString());
}
```

La respuesta es `tfa`. El bloque `try` **se ejecuta**. Dado que no **se lanza** ninguna excepción, Java pasa directamente al bloque `finally`. Luego **se ejecuta** el código después de la sentencia `try`. Es un ejemplo tonto, pero **se puede** esperar ver ejemplos como este en el examen.

Existe una regla adicional que **hay que** conocer sobre los bloques `finally`. Si **se entra** en una sentencia `try` con un bloque `finally`, el bloque `finally` **se ejecutará** siempre, independientemente de si el código **se completa** exitosamente. **Se observa** el siguiente método `goHome()`. Asumiendo que **se lance** o no una excepción en la línea 14, ¿cuáles son los valores que este método podría imprimir? ¿Y cuál sería el valor de retorno en cada caso?

```Java
12: int goHome() {
13:     try {
14:         // Optionally throw an exception here
15:         System.out.print("1");
16:         return -1;
17:     } catch (Exception e) {
18:         System.out.print("2");
19:         return -2;
20:     } finally {
21:         System.out.print("3");
22:         return -3;
23:     }
24: }
```

Si no **se lanza** una excepción en la línea 14, la línea 15 **se ejecuta**, imprimiendo `1`. Antes de que el método retorne, sin embargo, el bloque `finally` **se ejecuta**, imprimiendo `3`. Si **se lanza** una excepción, las líneas 15 y 16 **se omiten** y las líneas 17–19 **se ejecutan**, imprimiendo `2`, seguido del `3` del bloque `finally`. Aunque el primer valor impreso **puede** variar, el método siempre imprime `3` al final, ya que está en el bloque `finally`.

¿Cuál es el valor de retorno del método `goHome()`? En este caso, siempre es `-3`. Dado que el bloque `finally` **se ejecuta** justo antes de que el método **se complete**, interrumpe la sentencia `return` de dentro de los bloques `try` y `catch`.

Para el examen, **hay que recordar** que un bloque `finally` siempre **será** ejecutado. Dicho esto, puede que no **se complete** exitosamente. ¿Qué sucedería si `info` fuera `null` en la línea 32?

```Java
31: } finally {
32:     info.printDetails();
33:     System.out.println("Saliendo");
34:     return "zoo";
35: }
```

Si `info` fuera `null`, el bloque `finally` **se ejecutaría**, pero **se detendría** en la línea 32 y lanzaría una `NullPointerException`. Las líneas 33 y 34 no **se ejecutarían**. En este ejemplo, **se ve** que aunque un bloque `finally` siempre **se ejecuta**, puede que no **se complete**.

> **System.exit()**
>
> Hay una excepción a la regla "el bloque `finally` siempre **se ejecutará**": Java define un método que **se llama** como `System.exit()`. **Toma** un parámetro entero que representa el código de estado que **se devuelve**.
>
> ```Java
> try {
>     System.exit(0);
> } finally {
>     System.out.println("Nunca llega aquí"); // No se imprime
> }
> ```
>
> `System.exit()` le dice a Java: "Para. Termina el programa ahora mismo. No pases por la salida. No recojas los $200." Cuando **se llama** a `System.exit()` en el bloque `try` o `catch`, el bloque `finally` no **se ejecuta**.

---

**Ver también:** [[Gestión Automática de Recursos]] | [[Comprendiendo las Excepciones]] | [[Reconociendo las Clases de Excepción]]
