
Un programa Java comienza su ejecución con su método `main()`. En esta sección, **se aprenderá** a crear uno, pasar un parámetro y ejecutar un programa. Al método `main()` **se le suele denominar** punto de entrada del programa, ya que es el punto de inicio que la JVM busca al comenzar a ejecutar un nuevo programa.

### Creación de un método main()

El método `main()` permite que la JVM invoque el código. La clase más sencilla posible con un método `main()` tiene el siguiente aspecto:

```java
1: public class Zoo { 
2:     public static void main(String[] args) { 
3:         System.out.println("Hello World"); 
4:     } 
5: }
```

Este código imprime Hello World. Para compilar y ejecutar este código, debe escribirse en un archivo llamado `Zoo.java` y ejecutarse lo siguiente:

```zsh
javac Zoo.java 
java Zoo
```

Si se imprime Hello World, la operación ha sido exitosa. Si se obtienen mensajes de error, se debe verificar que el JDK de Java 21 esté instalado, que se haya añadido al PATH y que no se hayan cometido errores tipográficos en el ejemplo. Si surge alguno de estos problemas y no se sabe cómo proceder, se puede preguntar a alguna IA.

Para compilar código Java con el comando `javac`, el archivo debe tener la extensión `.java`. El nombre del archivo debe coincidir con el nombre de la clase pública. El resultado es un archivo de bytecode con el mismo nombre pero con una extensión de archivo `.class`. Cabe recordar que el bytecode consiste en instrucciones que la JVM sabe ejecutar. Obsérvese que se debe omitir la extensión `.class` para ejecutar `Zoo.class`.

Las reglas sobre el contenido de un archivo Java y su orden son más detalladas de lo que se ha explicado hasta ahora (hay más información sobre este tema más adelante en el capítulo). Para simplificar las cosas por el momento, se sigue este subconjunto de reglas:

- Cada archivo puede contener solo una clase pública.
- El nombre del archivo debe coincidir con el nombre de la clase, incluyendo mayúsculas y minúsculas, y tener una extensión `.java`.
- Si la clase Java es un punto de entrada para el programa, debe contener un método `main()` válido.

Primero, se revisarán las palabras de la firma del método `main()`, una por una. La palabra clave `public` es lo que se denomina un modificador de acceso. Declara el nivel de exposición de este método a posibles invocadores en el programa. Naturalmente, `public` significa acceso total desde cualquier lugar del programa. Se aprenderá más sobre los modificadores de acceso en el capítulo 5.

La palabra clave `static` vincula un método a su clase para que pueda ser invocado utilizando solo el nombre de la clase, como, por ejemplo, `Zoo.main()`. Java no necesita crear un objeto para invocar el método `main()`, lo cual es positivo, ¡ya que aún no se ha aprendido a crear objetos! De hecho, la JVM realiza esto, más o menos, al cargar el nombre de la clase que se le proporciona. Si un método `main()` no tiene las palabras clave correctas, se producirá un error al intentar ejecutarlo. El modificador `static` se verá de nuevo en el capítulo 6, «Diseño de clases».

La palabra clave `void` representa el tipo de retorno. Un método que no devuelve datos retorna el control al invocador silenciosamente. En general, se considera una buena práctica utilizar `void` para métodos que cambian el estado de un objeto. En ese sentido, el método `main()` cambia el estado del programa de iniciado a finalizado. Los tipos de retorno también se exploran en el capítulo 5. (¿Hay entusiasmo por el capítulo 5 ya?)

Finalmente, se llega a la lista de parámetros del método `main()`, representada como un array de objetos `java.lang.String`. Se puede utilizar cualquier nombre de variable válido junto con cualquiera de estos tres formatos:

```java
String[] args 
String options[] 
String… friends
```

El compilador acepta cualquiera de estas opciones. El nombre de variable `args` es común porque sugiere que esta lista contiene valores que fueron leídos (argumentos) al iniciarse la JVM. Los caracteres `[]` son corchetes y representan un array. Un array es una lista de tamaño fijo de elementos, todos del mismo tipo. Los caracteres `...` se denominan _varargs_ (listas de argumentos variables). En este capítulo se estudia la clase `String`. Los arrays se tratan en el capítulo 4, «Core APIs», y los _varargs_ en el capítulo 5.

#### Modificadores opcionales en los métodos main()

Si bien la mayoría de los modificadores, como `public` y `static`, son obligatorios para los métodos `main()`, se permiten algunos modificadores opcionales.

```java
public final static void main(final String[] args) {}
```

En este ejemplo, ambos modificadores `final` son opcionales, y el método `main()` constituye un punto de entrada válido con o sin ellos. El significado de los métodos y parámetros `final` se aborda en el capítulo 6.

### Paso de parámetros a un programa Java

A continuación, se verá cómo enviar datos al método `main()` del programa. Primero, se modifica el programa `Zoo` para imprimir los dos primeros argumentos recibidos.

```java
public class Zoo {
    public static void main(String[] args) {
	    System.out.println(args[0]);
        System.out.println(args[1]);
	}
}
```

El código `args[0]` accede al primer elemento del array. Así es: los índices de los arrays comienzan en 0 en Java. Para ejecutarlo, se debe escribir lo siguiente:

```zsh
javac Zoo.java 
java Zoo Bronx Zoo
```

La salida es la que cabría esperar:

```Plaintext
Bronx 
Zoo
```

El programa identifica correctamente las dos primeras «palabras» como los argumentos. Los espacios se utilizan para separar los argumentos. Si se desean espacios dentro de un argumento, es necesario utilizar comillas como en este ejemplo:

``` zsh
javac Zoo.java
java Zoo "San Diego" Zoo
```

Ahora aparece un espacio en la salida.

```Plaintext 
San Diego
Zoo
```

Finalmente, ¿qué sucede si no se suministran suficientes argumentos?

``` zsh
javac Zoo.java
java Zoo Zoo
```

La lectura de `args[0]` se realiza correctamente y se imprime `Zoo`. A continuación, se produce un error en Java. ¡No existe un segundo argumento! ¿Qué hacer ante esto? Java imprime una excepción indicando que desconoce qué hacer con este argumento en la posición 1. (Las excepciones se estudian en el capítulo 11, «Excepciones y localización»).

``` Plaintext
Zoo
Exception in thread "main"
java.lang.ArrayIndexOutOfBoundsException:
    Index 1 out of bounds for length 1
	at Zoo.main(Zoo.java:4)
```

A modo de repaso, el JDK contiene un compilador. Los archivos de clase Java se ejecutan en la JVM y, por lo tanto, funcionan en cualquier máquina con Java, en lugar de limitarse a la máquina o sistema operativo en el que se compilaron.

#### Código fuente de un solo archivo

Si resulta tedioso escribir `javac` y `java` cada vez que se desea probar un ejemplo de código, existe un atajo. En su lugar, se puede ejecutar lo siguiente:

```bash
java Zoo.java Bronx Zoo
```

Aquí existe una diferencia clave. Al compilar primero, se omitía la extensión del archivo al ejecutar `java`. Al omitir el paso de compilación explícita, se incluye dicha extensión. Esta característica se denomina ejecución de programas de código fuente de un solo archivo y resulta útil para pruebas o programas pequeños. El nombre indica ingeniosamente que su diseño está pensado para cuando el programa consta de un solo archivo. No olvide escribir su propio código y probar lo repasado en esta sección.
