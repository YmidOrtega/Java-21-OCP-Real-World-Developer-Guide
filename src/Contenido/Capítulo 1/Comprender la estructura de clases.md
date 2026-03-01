
En los programas Java, las clases son los bloques básicos de construcción. Al definir una clase, se describen todas las partes y características de uno de esos bloques de construcción. En capítulos posteriores, se verán otros bloques de construcción, como `interfaces`, `records` y `enums`.

Para utilizar la mayoría de las clases, hay que crear objetos. Un objeto es una instancia en tiempo de ejecución de una clase en la memoria. A menudo se hace referencia a un objeto como una instancia, ya que representa una única representación de la clase. Todos los distintos objetos de todas las diferentes clases representan el estado del programa. Una referencia es una variable que apunta a un objeto.

En las siguientes secciones, se analizarán los campos, los métodos y los comentarios. También se explorará la relación entre las clases y los archivos.

### Campos y métodos

Una clase Java tiene dos elementos principales: **métodos** (a menudo llamados funciones o procedimientos en otros lenguajes) y **campos** (más conocidos como **variables**). Juntos, se denominan **miembros de la clase**. La variable contiene el estado del programa y el método opera sobre ese estado. Si es importante recordar el cambio, una variable lo almacena. El trabajo de programación consiste en crear y organizar estos elementos de tal manera que el código resultante sea útil y fácil de entender.

La clase Java más sencilla que se puede escribir tiene este aspecto:

```java
1: public class Animal { 
2: }
```

Java denomina **«palabra clave»** a una palabra con un significado especial, la cual es **public** en el fragmento de código anterior. La línea 1 incluye la palabra clave `public`, que permite que otras clases la utilicen. La palabra clave `class` indica que se está definiendo una clase. `Animal` da el nombre de la clase. Es cierto que no es una clase muy interesante, así que se añadirá el primer campo.

```java
1: public class Animal { 
2:     String name; 
3: }
```

En la línea 2, se definió una variable llamada name. También se declaró que el tipo de esa variable es `String`. Una **cadena (String)** es un valor en el que se puede introducir texto, como «esto es una String». String es también una clase proporcionada por Java. A continuación, se pueden añadir métodos.

```java
1: public class Animal { 
2:     String name; 
3:     public String getName() {
4:         return name;
5:     }
6:     public void setName(String newName) {
7:         name = newName;
8:     }
9: }
```

En las líneas 3-5, se definió un **método**. Un método es una **operación** que se puede invocar. Una vez más, se utiliza `public` para indicar que este método se puede invocar desde otras clases. A continuación viene el **tipo de retorno**; en este caso, el método devuelve una **cadena (String)**. En las líneas 6-8 hay otro método. Este tiene un tipo de retorno especial llamado `void`. La palabra clave void significa que no se devuelve **ningún valor**. Este método requiere que se le proporcione información desde el método que lo invoca; esta información se denomina **parámetro**. El método `setName()` tiene un parámetro llamado `newName`, y es de tipo `String`. Esto significa que el invocador debe pasar un parámetro String y no esperar que se devuelva nada.

El **nombre del método** y los **tipos de parámetros** se denominan **firma del método**. ¿Es posible identificar el nombre del método y los parámetros en el siguiente ejemplo?

```java
public int numberVisitors(int month) { 
	return 10; 
}
```

El nombre del método es **numberVisitors**. Hay un parámetro llamado **month**, que es de tipo **int** (un tipo numérico). Por lo tanto, la **firma del método** es `numberVisitors(int)`.

### Comentarios

Otra parte común del código se denomina **comentario**. Dado que los comentarios no son código ejecutable, se pueden colocar en muchos sitios. Los comentarios pueden facilitar la lectura del código. Aunque en los exámenes se intenta que el código sea más difícil de leer, se siguen utilizando comentarios para señalar los números de línea. Se recomienda utilizar comentarios en el código propio.

En Java hay tres tipos de comentarios. El primero es el comentario de una sola línea.

```java
// comment until end of line
```

Un comentario de una sola línea comienza con dos barras inclinadas. El compilador ignora todo lo que se escriba después de eso en la misma línea. A continuación, viene el comentario de varias líneas.

```java
/* Multiple 
 * line comment 
 */
```

Un comentario de varias líneas (también conocido como comentario multilínea) abarca desde el símbolo `/*` hasta el símbolo `*/`. A menudo, para facilitar la lectura, se escribe un asterisco (`*`) al principio de cada línea de un comentario multilínea, aunque no es obligatorio. Por último, este es un comentario Javadoc:

```java
/**
 * Javadoc multiple-line comment 
 * @author Ymidツ
 */
```

Este comentario es similar a un comentario multilínea, excepto que comienza con `/**`. Esta sintaxis especial indica a la herramienta Javadoc que preste atención al comentario. Los comentarios Javadoc tienen una estructura específica que esta herramienta sabe leer. Probablemente no se verá un comentario Javadoc en el examen. Solo se debe recordar que existe para que, cuando se comiencen a escribir programas para ser utilizados por otros, sea posible documentarse al respecto en línea.

A modo de ejercicio, ¿se puede identificar a qué tipo de comentario pertenece cada una de las seis palabras siguientes? ¿Se trata de comentarios de una sola línea o multilínea?

```java
/*
 * // anteater 
 */
```

```java
// bear
```

```java
// // cat
```

```java
// /* dog */
```

```java
/* elephant */
```

```java
/*
 * /* ferret */ 
 */
```

¿Se ha observado con atención? Algunos son complicados. Aunque técnicamente los comentarios no entran en el examen, es bueno practicar mirando el código detenidamente.

Bien, pasando a las respuestas. El comentario que contiene «anteater» es un comentario de varias líneas. Todo lo que hay entre `/*` y `*/` forma parte de un comentario de varias líneas, ¡incluso si incluye un comentario de una sola línea dentro! El comentario que contiene «bear» es un comentario básico de una sola línea. Los comentarios que contienen «cat» y «dog» también son comentarios de una sola línea. Todo lo que hay desde // hasta el final de la línea forma parte del comentario, aunque sea de otro tipo. El comentario que contiene «elephant» es un comentario multilínea básico, aunque solo ocupe una línea.

La línea con «ferret» es interesante porque no se compila. Todo lo que hay desde el primer `/*` hasta el primer `*/` forma parte del comentario, por lo que el compilador ve algo así:

```java
/* */ */
```

Existe un problema. Hay un `*/` extra. Esa sintaxis no es válida, hecho que el compilador comunicará como error.

### Clases y archivos fuente

Por lo general, cada clase Java se define en su propio archivo `.java`. En este capítulo, el único tipo de nivel superior es una clase. Un tipo de nivel superior es una estructura de datos que se puede definir de forma independiente dentro de un archivo fuente. A lo largo del proyecto, se trabajará principalmente con clases como tipo de nivel superior, pero en el capítulo 7, «Más allá de las clases», se abordarán otros tipos de nivel superior, así como tipos anidados.

Una clase de nivel superior suele ser public, lo que significa que cualquier código puede llamarla. Curiosamente, Java no exige que el tipo sea public. Por ejemplo, esta clase es perfectamente válida:

```java
1: class Animal { 
2:     String name; 
3: }
```

Incluso se pueden incluir dos tipos en el mismo archivo. Al hacerlo, solo uno de los tipos de nivel superior del archivo puede ser público. Esto significa que un archivo que contenga lo siguiente también es válido:

```java
1: public class Animal { 
2:     private String name; 
3: }
4: class Animal2 {}
```

Si se incluye un tipo public, este debe coincidir con el nombre del archivo. La declaración `public class Animal2` no compilaría en un archivo llamado `Animal.java`. En el capítulo 5, «Métodos», se analizan las opciones de acceso disponibles además de `public`.
