Java incluye miles de clases integradas, y existen incontables más creadas por desarrolladores. Con todas esas clases, Java requiere una forma de organizarlas. Esto se gestiona de manera similar a un archivador. Los documentos se colocan en carpetas. Java coloca las clases en paquetes. Estos son agrupamientos lógicos para las clases.

No se colocaría a una persona frente a un archivador con la instrucción de encontrar un documento específico. En su lugar, se indicaría en qué carpeta buscar. Java funciona de la misma manera. Requiere que se le indique en qué paquetes buscar para encontrar el código.

Supóngase que se intenta compilar el siguiente código:

```java
public class NumberPicker {
	public static void main(String[] args) {
		Random r = new Random(); // NO COMPILA
		System.out.println(r.nextInt(10));
	} 
}
```

El compilador de Java emite un error similar a este:

``` Plaintext
error: cannot find symbol
```

Este error podría significar que se cometió un error tipográfico en el nombre de la clase. Tras una doble verificación, se descubre que no es el caso. La otra causa de este error es la omisión de una declaración `import` necesaria. Una declaración es una instrucción, y las declaraciones `import` indican a Java en qué paquetes buscar las clases. Dado que no se indicó a Java dónde buscar `Random`, el sistema desconoce su ubicación.

Al intentar esto nuevamente con la importación, el código compila.

```java
import java.util.Random; // import indica dónde encontrar Random

public class NumberPicker {
	public static void main(String[] args) {
		Random r = new Random(); // COMPILA
		System.out.println(r.nextInt(10));
	} 
}
```

Ahora el código se ejecuta; imprime un número aleatorio entre 0 y 9. Al igual que con los arrays, en Java se comienza a contar desde 0.
### Paquetes

Como se vio en el ejemplo anterior, las clases de Java se agrupan en paquetes. La declaración `import` indica al compilador en qué paquete buscar para encontrar una clase. Esto es similar al funcionamiento del correo postal. Suponga el envío de una carta a 123 Main Street, Apartamento 9. El cartero lleva primero la carta a 123 Main Street. Luego, busca el buzón del apartamento 9. La dirección es comparable al nombre del paquete en Java. El número de apartamento es comparable al nombre de la clase. Del mismo modo que el cartero solo busca números de apartamento en el edificio, Java solo busca nombres de clase en el paquete.

Los nombres de los paquetes también son jerárquicos, al igual que el correo. El servicio postal comienza con el nivel superior, observando primero el país. También se comienza a leer el nombre de un paquete desde el principio. Por ejemplo, si comienza con `java`, significa que proviene del JDK. Si comienza con otro elemento, probablemente indique su origen utilizando el nombre del sitio web al revés. Por ejemplo, `com.wiley.javabook` indica que el código está asociado con el sitio web u organización wiley.com. Después del nombre del sitio web, se puede añadir lo que se desee. Por ejemplo, `com.wiley.java.my.name` también proviene de wiley.com.

Java denomina **paquetes hijos** a los paquetes más detallados. El paquete `com.wiley.javabook` es un paquete hijo de `com.wiley`. Se puede distinguir porque es más largo y, por tanto, más específico.

En el examen se verán nombres de paquetes que no siguen esta convención. No debe causar sorpresa encontrar nombres de paquetes como `a.b.c`. La regla para los nombres de paquetes establece que se componen principalmente de letras o números separados por puntos (.). Técnicamente, se permiten algunos otros caracteres entre los puntos (.). Incluso se pueden utilizar nombres de paquetes de sitios web que no son de propiedad propia si así se desea, como `com.wiley`, aunque quienes lean el código podrían confundirse. Las reglas son las mismas que para los nombres de variables, las cuales se verán más adelante en este capítulo. El examen puede intentar confundir al candidato con nombres de variables no válidos. Afortunadamente, no se intenta engañar mediante nombres de paquetes no válidos.

En las siguientes secciones, se analizarán las importaciones con comodines (_wildcards_), los conflictos de nombres con importaciones, la creación de un paquete propio y el formato de código en el examen.

### Comodines (Wildcards)

A menudo, las clases de un mismo paquete se importan juntas. Se puede utilizar un atajo para importar todas las clases de un paquete.

```java
import java.util.*;// importa java.util.Random entre otras cosas

public class NumberPicker {
    public static void main(String[] args) {
        Random r = new Random();
        System.out.println(r.nextInt(10));
    }
}
```

En este ejemplo, se importó `java.util.Random` y un conjunto de otras clases. El `*` es un **comodín** que coincide con todas las clases del paquete. Todas las clases del paquete `java.util` están disponibles para este programa cuando Java lo compila. La declaración `import` no incluye paquetes hijos, campos o métodos; importa únicamente las clases que se encuentran directamente bajo el paquete.

Suponga que se desea utilizar la clase `AtomicInteger` (la cual se estudia en el capítulo 13, «Concurrencia») del paquete `java.util.concurrent.atomic`. ¿Qué importación o importaciones permiten esto?

```java
import java.util.*;
import java.util.concurrent.*;
import java.util.concurrent.atomic.*;
```

Solo la última importación permite reconocer la clase, ya que los paquetes hijos no se incluyen en las dos primeras.

Se podría pensar que incluir tantas clases ralentiza la ejecución del programa, pero no es así. El compilador determina lo que realmente se necesita. El enfoque que se elija es una preferencia personal (o preferencia del equipo, si se trabaja con otros). Listar las clases utilizadas facilita la lectura del código, especialmente para nuevos programadores. El uso del comodín puede acortar la lista de importaciones. Ambos enfoques aparecerán en el examen.

### Importaciones redundantes

Un momento. Se ha estado haciendo referencia a `System` sin una importación cada vez que se imprimió texto, y Java lo encontraba correctamente. Existe un paquete especial en el entorno Java llamado `java.lang`. Este paquete es especial porque se importa automáticamente. Es posible escribir este paquete en una declaración `import`, pero no es obligatorio. En el siguiente código, ¿cuántas importaciones se consideran redundantes?

```java
1: import java.lang.System;
2: import java.lang.*;
3: import java.util.Random;
4: import java.util.*;
5: public class NumberPicker {
6:     public static void main(String[] args) {
7:         Random r = new Random();
8:         System.out.println(r.nextInt(10));
9:     }
10: }
```

La respuesta es que tres de las importaciones son redundantes. Las líneas 1 y 2 son redundantes porque todo el contenido de `java.lang` se importa automáticamente. La línea 4 también es redundante en este ejemplo porque `Random` ya está importado desde `java.util.Random`. Sin embargo, si la línea 3 no estuviera presente, `java.util.*` no sería redundante, ya que cubriría la importación de `Random`.

Otro caso de redundancia implica importar una clase que se encuentra en el mismo paquete que la clase que la importa. Java busca automáticamente otras clases en el paquete actual.

Se examinará un ejemplo más para asegurar la comprensión de los casos límite en las importaciones. En este ejemplo, `Files` y `Paths` se encuentran en el paquete `java.nio.file`. El examen podría utilizar paquetes nunca antes vistos. La pregunta indicará en qué paquete se encuentra la clase si es necesario saberlo para responder.

¿Qué declaraciones de importación permitirían compilar este código?

```java
public class InputImports {
    public void read(Files files) {
        Paths.get("name");
    }
}
```

Existen dos respuestas posibles. La más breve consiste en utilizar un comodín para importar ambas al mismo tiempo.

``` java
import java.nio.file.*;
```

La otra respuesta consiste en importar ambas clases explícitamente.

``` java
import java.nio.file.Files;
import java.nio.file.Paths;
```

Ahora se considerarán algunas importaciones que no funcionan.

```java
import java.nio.*; // INCORRECTO - un comodín solo coincide con nombres de clase, no con "file.Files"
import java.nio.*.*; // INCORRECTO - solo se permite un comodín y debe estar al final
import java.nio.file.Paths.*; // INCORRECTO - no se pueden importar métodos solo nombres de clases
```

### Conflictos de nombres

Una de las razones para utilizar paquetes es evitar que los nombres de las clases deban ser únicos en todo el entorno Java. Esto implica que, en ocasiones, será necesario importar una clase que se encuentre en múltiples ubicaciones. Un ejemplo común de esto es la clase `Date`. Java proporciona implementaciones de `java.util.Date` y `java.sql.Date`. ¿Qué declaración de importación se puede utilizar si se desea la versión `java.util.Date`?

```java
public class Conflicts {
    Date date;
    // algo más de código
}
```

La respuesta debería resultar sencilla a estas alturas. Se puede escribir `import java.util.*;` o `import java.util.Date;`. Los casos complicados surgen cuando existen otras importaciones presentes.

```java
import java.util.*;
import java.sql.*; // provoca que la declaración de Date no compile
```

Cuando el nombre de la clase se encuentra en múltiples paquetes, Java genera un error de compilación. En este ejemplo, la solución es sencilla: eliminar la importación `java.sql.*` que no se necesita. Pero, ¿qué se debe hacer si se requiere una gran cantidad de otras clases del paquete `java.sql`?

```java
import java.util.Date;
import java.sql.*;
```

¡Ahora funciona! Si se importa explícitamente un nombre de clase, este tiene prioridad sobre cualquier comodín presente. Java interpreta: «Se desea explícitamente asumir el uso de la clase `java.util.Date`».

Un ejemplo más. ¿Qué hace Java con los «empates» de prioridad?

``` java
import java.util.Date;
import java.sql.Date;
```

Java es lo suficientemente inteligente como para detectar que este código no es válido. Se ha indicado explícitamente que se desea que el valor predeterminado sean tanto las implementaciones de `java.util.Date` como las de `java.sql.Date`. Dado que no pueden existir dos valores predeterminados, el compilador indica que las importaciones son ambiguas.

#### Si realmente es necesario utilizar dos clases con el mismo nombre

En ocasiones, realmente se desea utilizar `Date` de dos paquetes diferentes. Cuando esto sucede, se puede elegir uno para la declaración `import` y utilizar el nombre de clase totalmente calificado del otro. O bien, se pueden omitir ambas declaraciones `import` y utilizar siempre el nombre de clase totalmente calificado.

```java
public class Conflicts {
    java.util.Date date;
    java.sql.Date sqlDate;
}
```

### Creación de un nuevo paquete

Hasta ahora, todo el código escrito en este capítulo ha estado en el **paquete predeterminado** (_default package_). Se trata de un paquete especial sin nombre que solo debe utilizarse para código desechable o temporal. Se puede distinguir que el código está en el paquete predeterminado porque no hay nombre de paquete. En el examen, se verá el uso frecuente del paquete predeterminado para ahorrar espacio en los listados de código. En la práctica, siempre se deben nombrar los paquetes para evitar conflictos de nombres y permitir la reutilización del código por parte de terceros.

Ahora es el momento de crear un nuevo paquete. La estructura de directorios en el ordenador está relacionada con el nombre del paquete. En esta sección, basta con leer. La compilación y ejecución del código se abordarán en la siguiente sección.

Supóngase que existen estas dos clases en el directorio `C:\temp`:

```java
package packagea;

public class ClassA {}
```

```java
package packageb;

import packagea.ClassA;

public class ClassB {
	public static void main(String[] args) {
		ClassA a;
		System.out.println("Got it");
	}
}
```

Al ejecutar un programa Java, el sistema sabe dónde buscar esos nombres de paquetes. En este caso, la ejecución desde `C:\temp` funciona porque tanto `packagea` como `packageb` se encuentran bajo este directorio.

### Compilación y ejecución de código con paquetes

Se aprenderá Java con mayor facilidad utilizando la línea de comandos para compilar y probar los ejemplos. Una vez que se conozca bien la sintaxis de Java, se puede cambiar a un IDE. Sin embargo, para el examen, el objetivo es conocer los detalles del lenguaje y evitar que el IDE los oculte.

Se debe seguir este ejemplo para asegurar el conocimiento sobre el uso de la línea de comandos. Si surgen problemas al seguir este procedimiento, se puede preguntar a alguna IA. Se recomienda describir qué se ha intentado y qué indicaba el error.

El primer paso consiste en crear los dos archivos de la sección anterior. La Tabla 1.1 muestra los nombres de archivo totalmente calificados esperados y el comando para acceder al directorio para los siguientes pasos.

**TABLA 1.1** Procedimiento de configuración por sistema operativo

| **Paso**                   | **Windows**                    | **Mac/Linux**               |
| -------------------------- | ------------------------------ | --------------------------- |
| 1. Crear la primera clase. | `C:\temp\packagea\ClassA.java` | `/tmp/packagea/ClassA.java` |
| 2. Crear la segunda clase. | `C:\temp\packageb\ClassB.java` | `/tmp/packageb/ClassB.java` |
| 3. Ir al directorio.       | `cd C:\temp`                   | `cd /tmp`                   |

Ahora es el momento de compilar el código. Afortunadamente, este proceso es el mismo independientemente del sistema operativo. Para compilar, se debe escribir el siguiente comando:

```zsh
javac packagea/ClassA.java packageb/ClassB.java
```

Si este comando no funciona, se obtendrá un mensaje de error. Se deben revisar los archivos cuidadosamente en busca de errores tipográficos, comparándolos con los archivos proporcionados. Si el comando funciona, se crearán dos nuevos archivos: `packagea/ClassA.class` y `packageb/ClassB.class`.

Ahora que el código se ha compilado, se puede ejecutar escribiendo el siguiente comando:

```zsh
java packageb.ClassB
```

Si funciona, se verá impreso _Got it_. Es posible que se haya notado que se escribió `ClassB` en lugar de `ClassB.class`. Como se mencionó anteriormente, no se incluye la extensión al ejecutar un programa.

La figura a continuación muestra dónde se crearon los archivos `.class` en la estructura de directorios.

![[Paquete 1.1.png]]

### Compilación en otro directorio

Por defecto, el comando `javac` coloca las clases compiladas en el mismo directorio que el código fuente. También proporciona una opción para colocar los archivos de clase en un directorio diferente. La opción `-d` especifica este directorio de destino.

Si se está siguiendo el ejercicio, se deben eliminar los archivos `ClassA.class` y `ClassB.class` que se crearon en la sección anterior. ¿Dónde se creará el archivo `ClassA.class` con este comando?

```zsh
javac -d classes packagea/ClassA.java packageb/ClassB.java
```

La respuesta correcta es en `classes/packagea/ClassA.class`. La estructura de paquetes se conserva bajo el directorio de destino indicado. La Figura a continuación muestra esta nueva estructura.

![[Paquete 1.2.png]]

Para ejecutar el programa, se especifica el _classpath_ para que Java sepa dónde encontrar las clases. Existen tres opciones que se pueden utilizar. Las tres realizan la misma función:

```zsh
java -cp classes packageb.ClassB
java -classpath classes packageb.ClassB
java --class-path classes packageb.ClassB
```

Obsérvese que la última opción requiere dos guiones (`--`), mientras que las dos primeras requieren un solo guion (`-`). Si se utiliza el número incorrecto de guiones, el programa no se ejecutará.

Las Tablas 1.2 y 1.3 revisan las opciones que es necesario conocer para el examen. ¡Existen muchas otras opciones disponibles! Además, en el capítulo 12, «Módulos», se aprenden opciones adicionales específicas para módulos.

**TABLA 1.2** Opciones importantes de `javac`

| **Opción**                                                                     | **Descripción**                                                |
| ------------------------------------------------------------------------------ | -------------------------------------------------------------- |
| `-cp <classplath>`<br>`-classpath <classplath>`<br>`--class-path <classplath>` | Ubicación de las clases necesarias para compilar el programa.  |
| `-d <dir>`                                                                     | Directorio donde se colocarán los archivos de clase generados. |

**TABLA 1.3** Opciones importantes de `java`

| **Opción**                                                                     | **Descripción**                                               |
| ------------------------------------------------------------------------------ | ------------------------------------------------------------- |
| `-cp <classplath>`<br>`-classpath <classplath>`<br>`--class-path <classplath>` | Ubicación de las clases necesarias para ejecutar el programa. |

### Ordenamiento de los elementos en una clase

Una vez vistas las partes más comunes de una clase, se examinará el orden correcto para escribirlas en un archivo. Los comentarios pueden ir en cualquier parte del código. Más allá de eso, es necesario memorizar las reglas de la Tabla 1.4.

**TABLA 1.4** Orden para declarar una clase

|**Elemento**|**Ejemplo**|**¿Requerido?**|**¿Dónde se ubica?**|
|---|---|---|---|
|Declaración de paquete|`package abc;`|No|Primera línea del archivo (excluyendo comentarios o líneas en blanco).|
|Declaraciones de importación|`import java.util.*;`|No|Inmediatamente después del paquete (si está presente).|
|Declaración de tipo de nivel superior|`public class C`|Sí|Inmediatamente después de las importaciones (si las hay).|
|Declaraciones de campos|`int value;`|No|Cualquier elemento de nivel superior dentro de una clase.|
|Declaraciones de métodos|`void method()`|No|Cualquier elemento de nivel superior dentro de una clase.|

Se analizarán algunos ejemplos para ayudar a recordar esto. El primer ejemplo contiene uno de cada elemento.

```java
package structure; // el paquete debe ser lo primero que no sea comentario

import java.util.*; // import debe ir después del paquete

public class Meerkat { // luego viene la clase
    double weight; // campos y métodos pueden ir en cualquier orden
    public double getWeight() {
        return weight; 
    }
    double height; // otro campo - no necesitan estar juntos
}
```

Hasta ahora, todo bien. Este es un patrón común con el que se debe estar familiarizado. ¿Qué sucede con el siguiente caso?

```java
/* header */
package structure;

// class Meerkat
public class Meerkat { }
```

Sigue siendo válido. Se pueden colocar comentarios en cualquier lugar, las líneas en blanco se ignoran y las importaciones son opcionales. En el siguiente ejemplo, existe un problema:

```java
import java.util.*;

package structure; // NO COMPILA

String name; // NO COMPILA

public class Meerkat { } // NO COMPILA
```

Aquí hay dos problemas. Uno es que las declaraciones de paquete e importación están invertidas. Aunque ambas son opcionales, el paquete debe ir antes de la importación si está presente. El otro problema es que un campo intenta declararse fuera de una clase. Esto no está permitido. Los campos y métodos deben estar dentro de una clase.

¿Queda claro? Se puede pensar en el acrónimo **PIC** (_picture_): **p**aquete, **i**mportación y **c**lase. Los campos y métodos son más fáciles de recordar porque simplemente tienen que estar dentro de una clase.

Ahora se sabe cómo crear y organizar una clase. En capítulos posteriores se muestra cómo crear clases con operaciones más potentes.
