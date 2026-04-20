Un programa pequeño puede crear una gran cantidad de objetos `String` muy rápidamente. Por ejemplo, ¿cuántos objetos se cree que crea este fragmento de código?

```Java
10: String alpha = "";
11: for(char current = 'a'; current <= 'z'; current++)
12:     alpha += current;
13: System.out.println(alpha);
```

El `String` vacío en la línea 10 es instanciado, y luego la línea 12 añade una `"a"`. Sin embargo, debido a que el objeto `String` es inmutable, se asigna un **nuevo** objeto `String` a `alpha`, y el objeto `""` se vuelve elegible para la recolección de basura (_garbage collection_). La próxima vez que pasa por el bucle, a `alpha` se le asigna un nuevo objeto `String`, `"ab"`, y el objeto `"a"` se vuelve elegible para la recolección de basura. La siguiente iteración asigna `alpha` a `"abc"`, y el objeto `"ab"` se vuelve elegible para la recolección de basura, y así sucesivamente.

Esta secuencia de eventos continúa, y después de 26 iteraciones a través del bucle, se instancia un total de 27 objetos, la mayoría de los cuales son inmediatamente elegibles para la recolección de basura.

Esto es muy ineficiente. Afortunadamente, Java tiene una solución. La clase `StringBuilder` crea un `String` sin almacenar todos esos valores `String` provisionales. A diferencia de la clase `String`, `StringBuilder` **no** es inmutable.

```Java
15: StringBuilder alpha = new StringBuilder();
16: for(char current = 'a'; current <= 'z'; current++)
17:     alpha.append(current);
18: System.out.println(alpha);
```

En la línea 15, se instancia un nuevo objeto `StringBuilder`. La llamada a `append()` en la línea 17 añade un carácter al objeto `StringBuilder` cada vez que pasa por el bucle `for`, anexando el valor de `current` al final de `alpha`. Este código reutiliza el mismo `StringBuilder` sin crear un `String` provisional cada vez.

En código antiguo, es posible ver referencias a `StringBuffer`. Funciona de la misma manera, excepto que soporta hilos (_threads_), lo cual se aprende en el Capítulo 13, «Concurrencia». `StringBuffer` no está en el examen. Su rendimiento es más lento que el de `StringBuilder`, por lo que simplemente se debe usar `StringBuilder`.

En esta sección, se observa cómo crear un `StringBuilder` y usar sus métodos comunes.

### Mutabilidad y encadenamiento

Es seguro que se notó esto en el ejemplo anterior, pero `StringBuilder` no es inmutable. De hecho, se le dieron 27 valores diferentes en el ejemplo (uno en blanco más la adición de cada letra del alfabeto). Es probable que el examen intente engañar con respecto a que `StringBuilder` es mutable y `String` es inmutable.

El encadenamiento (_chaining_) hace esto aún más interesante. Cuando se encadenaban llamadas a métodos `String`, el resultado era un **nuevo** `String` con la respuesta. El encadenamiento de métodos `StringBuilder` no funciona de esta manera. En su lugar, el `StringBuilder` cambia su propio estado y devuelve una referencia a **sí mismo**. Se observará un ejemplo para aclarar esto:

```Java
4: StringBuilder sb = new StringBuilder("start");
5: sb.append("+middle");                   // sb = "start+middle"
6: StringBuilder same = sb.append("+end"); // sb = "start+middle+end"
```

La línea 5 añade texto al final de `sb`. También devuelve una referencia a `sb`, la cual es ignorada. La línea 6 también añade texto al final de `sb` y devuelve una referencia a `sb`. Esta vez la referencia se almacena en `same`. Esto significa que `sb` y `same` apuntan al mismo objeto e imprimirían el mismo valor.

El examen no siempre facilitará la lectura del código teniendo solo un método por línea. ¿Qué se cree que imprime este ejemplo?

```Java
4: StringBuilder a = new StringBuilder("abc");
5: StringBuilder b = a.append("de");
6: b = b.append("f").append("g");
7: System.out.println("a=" + a);
8: System.out.println("b=" + b);
```

¿Se pensó que ambos imprimen `"abcdefg"`? Correcto. Solo hay **un** objeto `StringBuilder` aquí. Se sabe eso porque `new StringBuilder()` se llama solo una vez. En la línea 5, hay dos variables que se refieren a ese objeto, que tiene un valor de `"abcde"`. En la línea 6, esas dos variables siguen refiriéndose a ese mismo objeto, que ahora tiene un valor de `"abcdefg"`. Casualmente, la asignación de vuelta a `b` no hace absolutamente nada. `b` ya está apuntando a ese `StringBuilder`.

### Creación de un `StringBuilder`

Hay tres formas de construir un `StringBuilder`:

```Java
StringBuilder sb1 = new StringBuilder();
StringBuilder sb2 = new StringBuilder("animal");
StringBuilder sb3 = new StringBuilder(10);
```

La primera indica crear un `StringBuilder` que contiene una secuencia vacía de caracteres y asignar `sb1` para que apunte a él. La segunda indica crear un `StringBuilder` que contiene un valor específico y asignar `sb2` para que apunte a él. Los dos primeros ejemplos le dicen a Java que gestione los detalles de implementación. El ejemplo final le dice a Java que se tiene alguna idea de cuán grande será el valor final y que se desea que el `StringBuilder` reserve una cierta capacidad, o número de ranuras (_slots_), para los caracteres.

### Métodos importantes de `StringBuilder`

Al igual que con `String`, no se cubrirán todos y cada uno de los métodos de la clase `StringBuilder`. Estos son los que se podrían ver en el examen.

#### Uso de métodos comunes

Estos cuatro métodos funcionan exactamente igual que en la clase `String`. Hay que asegurarse de poder identificar la salida de este ejemplo:

```Java
var sb = new StringBuilder("animals");
String sub = sb.substring(sb.indexOf("a"), sb.indexOf("al"));
int len = sb.length();
char ch = sb.charAt(6);
System.out.println(sub + " " + len + " " + ch);
```

La respuesta correcta es `anim 7 s`. El método `indexOf()` llama devuelven 0 y 4, respectivamente. El método `substring()` devuelve el `String` comenzando con el índice 0 y terminando justo antes del índice 4.

El método `length()` devuelve 7 porque es el número de caracteres en el `StringBuilder` en lugar de un índice. Finalmente, `charAt()` devuelve el carácter en el índice 6. Aquí, sí se comienza con 0 porque se hace referencia a los índices. Si esto no suena familiar, se debe volver a leer la sección sobre `String`.

Note que `substring()` devuelve un `String` en lugar de un `StringBuilder`. Es por eso que `sb` **no cambia**. El método `substring()` es realmente solo un método que consulta sobre el estado del `StringBuilder`.

#### Adición de valores (_Appending_)

El método `append()` es, con mucho, el método más utilizado en `StringBuilder`. De hecho, se utiliza tan frecuentemente que se comenzó a usar sin hacer comentarios previos. Afortunadamente, este método hace exactamente lo que parece: añade el parámetro al `StringBuilder` y devuelve una referencia al `StringBuilder` actual. Una de las firmas del método es la siguiente:

```Java
public StringBuilder append(String str)
```

Note que se dijo _una_ de las firmas del método. Hay más de 10 firmas de métodos que se ven similares pero toman diferentes tipos de datos como parámetros, como `int`, `char`, etc. Todos esos métodos se proporcionan para que se pueda escribir código como este:

```Java
var sb = new StringBuilder().append(1).append('c');
sb.append("-").append(true);
System.out.println(sb); // 1c-true
```

Buen encadenamiento de métodos, ¿verdad? El método `append()` se llama directamente después del constructor. Al tener todas estas firmas de métodos, simplemente se puede llamar a `append()` sin tener que convertir el parámetro a un `String` primero.

#### Aplicación de puntos de código (_Code Points_)

Los métodos `codePointAt()`, `codePointBefore()` y `codePointCount()` de `String` también están disponibles en `StringBuilder`. Hay un método más que se necesita conocer para los puntos de código que está solo en `StringBuilder`:

```Java
public StringBuilder appendCodePoint(int codePoint)
```

Funciona como el método `append()` en la sección anterior, excepto que toma un entero que representa el valor Unicode, lo convierte en un carácter y lo añade al `StringBuilder`.

```Java
var sb = new StringBuilder()
    .appendCodePoint(87).append(',')
    .append((char)87).append(',')
    .append(87).append(',')
    .appendCodePoint(8217);
System.out.println(sb); // W,W,87,’
```

Como se vio con `String`, también maneja caracteres no ASCII como una comilla estilizada (`’`). Nuevamente, no es necesario conocer los valores numéricos de los caracteres para el examen, pero se debe comprender cómo se genera el texto en este ejemplo.

#### Inserción de datos

El método `insert()` añade caracteres al `StringBuilder` en el índice solicitado y devuelve una referencia al `StringBuilder` actual. Al igual que `append()`, hay muchas firmas de métodos para diferentes tipos. Aquí hay una:

```Java
public StringBuilder insert(int offset, String str)
```

Se debe prestar atención al desplazamiento (_offset_) en estos ejemplos. Es el índice donde se desea insertar el parámetro solicitado.

```Java
3: var sb = new StringBuilder("animals");
4: sb.insert(7, "-"); // sb = animals-
5: sb.insert(0, "-"); // sb = -animals-
6: sb.insert(4, "-"); // sb = -ani-mals-
7: System.out.println(sb);
```

La línea 4 indica insertar un guion en el índice 7, que resulta ser el final de la secuencia de caracteres. La línea 5 indica insertar un guion en el índice 0, que resulta ser el principio mismo. Finalmente, la línea 6 indica insertar un guion **justo antes** del índice 4. Los creadores del examen intentarán hacer tropezar con esto. A medida que se añaden y eliminan caracteres, **sus índices cambian**. Cuando se vea una pregunta que trate con tales operaciones, se debe dibujar lo que está sucediendo utilizando los materiales de escritura disponibles para no confundirse.

#### Eliminación de contenido

El método `delete()` es el opuesto del método `insert()`. Elimina caracteres de la secuencia y devuelve una referencia al `StringBuilder` actual. El método `deleteCharAt()` es conveniente cuando se desea eliminar solo un carácter. Las firmas de los métodos son las siguientes:

```Java
public StringBuilder delete(int startIndex, int endIndex)
public StringBuilder deleteCharAt(int index)
```

El siguiente código muestra cómo usar estos métodos:

```Java
var sb = new StringBuilder("abcdef");
sb.delete(1, 3);      // sb = adef
sb.deleteCharAt(5);   // excepción
```

Primero, se eliminan los caracteres comenzando con el índice 1 y terminando justo antes del índice 3. Esto da `adef`. A continuación, se pide a Java que elimine el carácter en la posición 5. Sin embargo, el valor restante tiene solo cuatro caracteres de longitud, por lo que lanza una `StringIndexOutOfBoundsException`.

El método `delete()` es más flexible que algunos otros cuando se trata de índices de arreglos. Si se especifica un segundo parámetro que está más allá del final del `StringBuilder`, Java simplemente asumirá que se refería al final. Eso significa que este código es legal:

```Java
var sb = new StringBuilder("abcdef");
sb.delete(1, 100);    // sb = a
```

#### Reemplazo de porciones

El método `replace()` funciona de manera diferente para `StringBuilder` que para `String`. La firma del método es la siguiente:

```Java
public StringBuilder replace(int startIndex, int endIndex, String newString)
```

El siguiente código muestra cómo usar este método:

```Java
var builder = new StringBuilder("pigeon dirty");
builder.replace(3, 6, "sty");
System.out.println(builder); // pigsty dirty
```

Primero, Java elimina los caracteres comenzando con el índice 3 y terminando justo antes del índice 6. Esto da `"pig dirty"`. Luego Java inserta el valor `"sty"` en esa posición.

En este ejemplo, el número de caracteres eliminados e insertados es el mismo. Sin embargo, no hay razón por la que tengan que serlo. ¿Qué se cree que hace esto?

```Java
var builder = new StringBuilder("pigeon dirty");
builder.replace(3, 100, "");
System.out.println(builder);
```

Imprime `"pig"`. Recuerde, el método primero está haciendo una eliminación lógica. El método `replace()` permite especificar un segundo parámetro que está más allá del final del `StringBuilder`. Eso significa que solo quedan los primeros tres caracteres.

#### Inversión (_Reversing_)

Después de todo eso, es hora de un método agradable y fácil. El método `reverse()` hace justo lo que parece: invierte los caracteres en las secuencias y devuelve una referencia al `StringBuilder` actual. La firma del método es la siguiente:


```Java
public StringBuilder reverse()
```

El siguiente código muestra cómo usar este método:

```Java
var sb = new StringBuilder("ABC");
sb.reverse();
System.out.println(sb);
```

Como se esperaba, esto imprime `CBA`. Este método no es tan interesante. Quizás a los creadores del examen les guste incluirlo para animar a escribir el valor en lugar de confiar en la memoria para los índices.

### Uso de `toString()`

La clase `Object` contiene un método `toString()` del que muchas clases ofrecen implementaciones personalizadas. La clase `StringBuilder` es una de ellas.

El siguiente código muestra cómo utilizar este método:

```Java
var sb = new StringBuilder(«ABC»);
String s = sb.toString();
```

A menudo, StringBuilder se utiliza internamente por motivos de rendimiento, pero el resultado final debe ser un String. Por ejemplo, tal vez sea necesario pasarlo a otro método que espera un `String`.