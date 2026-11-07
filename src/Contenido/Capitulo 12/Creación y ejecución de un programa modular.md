En esta sección, **se crea**, **se construye** y **se ejecuta** el módulo `zoo.animal.feeding`. **Se eligió** este módulo para comenzar porque todos los demás módulos dependen de él. La siguiente imagen muestra el diseño de este módulo. Además del archivo `module-info.java`, este módulo tiene un paquete con una clase dentro.

En las secciones siguientes, **se crea**, **se compila**, **se ejecuta** y **se empaqueta** el módulo `zoo.animal.feeding`.

![[Contenido de zoo.animal.feeding.jpg]]
### Creación de los archivos

Primero hay una clase muy simple que imprime una línea en un método `main()`. **Se sabe** que no es gran cosa de implementación. Todos esos programadores que **se contrataron** pueden rellenarla con lógica de negocio. En este proyecto, **se centra** en lo que **se necesita** saber para el examen. Así que, **se crea** una clase simple:

```Java
package zoo.animal.feeding;

public class Task {
   public static void main(String... args) {
      System.out.println("All fed!");
   }
}
```

A continuación viene el archivo `module-info.java`. Este es el más simple posible:

```Java
module zoo.animal.feeding {
}
```

**Hay algunas diferencias clave** entre una declaración de módulo (_module declaration_) y una declaración normal de clase Java:

- El archivo `module-info.java` debe estar en el directorio raíz de su módulo. Las clases Java normales deben estar en paquetes.
- La declaración de módulo debe usar la palabra clave `module` en lugar de `class`, `interface` o `enum`.
- El nombre del módulo sigue las reglas de nombres de paquetes. A menudo incluye puntos (`.`) en su nombre. No se permite que los nombres normales de clases y paquetes tengan guiones (`-`). Los nombres de módulo siguen la misma regla.

**Son muchas reglas** para el archivo más simple posible. **Habrá** muchas más reglas cuando **se desarrolle** este archivo más adelante en el capítulo.

El siguiente paso es asegurarse de que los archivos estén en la estructura de directorios correcta. La siguiente imagen muestra la estructura de directorios esperada.

![[Estructura de directorios del módulo zoo.animal.feeding.jpg]]

En particular, `feeding` es el directorio del módulo, y el archivo `module-info.java` está directamente debajo de él. Al igual que con un archivo JAR normal, también está el paquete `zoo.animal.feeding` con una subcarpeta por cada parte del nombre. La clase `Task` está en la subcarpeta adecuada para su paquete.

**También hay que notar** que **se creó** un directorio llamado `mods` en el mismo nivel que el módulo. **Se usa** para almacenar los artefactos del módulo un poco más adelante en el capítulo. Este directorio puede llamarse de cualquier manera, pero `mods` es un nombre común. Si **se sigue** el ejemplo de código en línea, **hay que notar** que el directorio `mods` no está incluido porque está vacío.

### Compilando nuestro primer módulo

Antes de poder ejecutar código modular, **se necesita** compilarlo. Aparte de la opción `--module-path`, este código debería ser familiar del Capítulo 1:

```
javac --module-path mods
   -d feeding
   feeding/zoo/animal/feeding/*.java feeding/module-info.java
```

> **Nota:** Cuando **se escriben** comandos en la línea de comandos, **deben escribirse** todos en una sola línea. **Se usan** saltos de línea en el proyecto para que los comandos sean más fáciles de leer y estudiar. Si **se quiere** usar múltiples líneas en el símbolo del sistema, el enfoque varía según el sistema operativo. Linux usa una barra invertida (`\`) como salto de línea.

Para repasar, la opción `-d` especifica el directorio donde colocar los archivos de clase. El final del comando es una lista de los archivos `.java` a compilar. **Se pueden** listar los archivos individualmente o usar un comodín para todos los archivos `.java` de un subdirectorio. La parte nueva es `--module-path`. Esta opción indica la ubicación de cualquier archivo de módulo personalizado. En este ejemplo, `--module-path` podría haberse omitido ya que no hay dependencias. **Se puede** pensar en `--module-path` como el reemplazo de la opción `classpath` cuando **se trabaja** en un programa modular.

> #### **¿Y qué pasa con el classpath?**
>
> La opción `classpath` tiene tres formas posibles: `-cp`, `--class-path` y `-classpath`. Estas opciones todavía **se pueden** usar. De hecho, es común hacerlo cuando **se escriben** programas no modulares.

Al igual que con `classpath`, **se puede** usar una abreviatura en el comando. La sintaxis `--module-path` y `-p` son equivalentes. Eso significa que **se podrían** haber escrito muchos otros comandos en lugar del anterior. Los siguientes cuatro comandos muestran alternativas si **se elige** la opción `-p`:

```
javac -p mods -d feeding
   feeding/zoo/animal/feeding/*.java feeding/*.java

javac -p mods -d feeding
   feeding/zoo/animal/feeding/*.java feeding/module-info.java

javac -p mods -d feeding
   feeding/zoo/animal/feeding/Task.java feeding/module-info.java

javac -p mods -d feeding
   feeding/zoo/animal/feeding/Task.java feeding/*.java
```

Aunque **se puede** usar la forma que **se prefiera**, **hay que asegurarse** de poder reconocer todas las formas válidas para el examen. La Tabla 12.1 lista las opciones que **se necesita** conocer bien al compilar módulos. Hay muchas más opciones que **se pueden** pasar al comando `javac`, pero estas son las que **se espera** que **se evalúen**.

**TABLA 12.1** Opciones que se necesita conocer para usar módulos con `javac`

| **Uso** | **Abreviatura** | **Forma larga** |
|---|---|---|
| Directorio para archivos de clase | `-d <dir>` | n/a |
| Module path | `-p <path>` | `--module-path <path>` |

> **Escenario del mundo real: Construyendo módulos**
>
> Incluso sin módulos, es raro ejecutar manualmente los comandos `javac` y `java` en un proyecto real. Se vuelven largos y complicados muy rápidamente. La mayoría de los desarrolladores usa una herramienta de compilación como Maven o Gradle. Estas herramientas de compilación sugieren directorios donde colocar los archivos de clase, como `target/classes`.
>
> Es probable que la única ocasión en que **se necesite** conocer la sintaxis de estos comandos sea cuando **se rinda** el examen. Los conceptos en sí mismos son útiles, independientemente de esto.
>
> **Hay que memorizar** la sintaxis de los comandos de módulo. **Se evaluará** sobre ello en el examen. **Se incluyen** muchas preguntas de práctica sobre la sintaxis para reforzarla.

### Ejecutando nuestro primer módulo

Antes de empaquetar el módulo, **se debe** asegurar que funcione ejecutándolo. Para eso, **se necesita** aprender la sintaxis completa. Supongamos que hay un módulo llamado `book.module`. Dentro de ese módulo hay un paquete llamado `com.sybex`, que tiene una clase llamada `OCP` con un método `main()`. La siguiente imagen muestra la sintaxis para ejecutar un módulo.

![[Ejecución de un módulo con java.jpeg]]

**Hay que prestar** especial atención a la parte `book.module/com.sybex.OCP`. Es importante recordar que **se especifica** el nombre del módulo, seguido de una barra (`/`), y luego del nombre completo de la clase (_fully qualified class name_).

Ahora que **se vio** la sintaxis, **se puede** escribir el comando para ejecutar la clase `Task` en el paquete `zoo.animal.feeding`. En el ejemplo siguiente, el nombre del paquete y el nombre del módulo son los mismos. Es común que el nombre del módulo coincida con el nombre completo del paquete o con el principio de este.

```
java --module-path feeding
   --module zoo.animal.feeding/zoo.animal.feeding.Task
```

Como **ya se vio** que `--module-path` usa la forma corta de `-p`, **no sorprenderá** saber que también hay una forma corta de `--module`. La opción corta es `-m`. Eso significa que el siguiente comando es equivalente:

```
java -p feeding
   -m zoo.animal.feeding/zoo.animal.feeding.Task
```

En estos ejemplos, **se usó** `feeding` como module path porque ahí es donde **se compiló** el código. Esto cambiará una vez que **se empaquete** el módulo y **se ejecute** este.

La Tabla 12.2 lista las opciones que **se necesita** conocer para el comando `java`.

**TABLA 12.2** Opciones que se necesita conocer para usar módulos con `java`

| **Uso**           | **Abreviatura** | **Forma larga**        |
| ----------------- | --------------- | ---------------------- |
| Nombre del módulo | `-m <name>`     | `--module <name>`      |
| Module path       | `-p <path>`     | `--module-path <path>` |

### Empaquetando nuestro primer módulo

Un módulo no **sirve** de mucho si solo **se puede** ejecutar en la carpeta donde fue creado. El siguiente paso es empaquetarlo. **Hay que** asegurarse de crear un directorio `mods` antes de ejecutar este comando:

```
jar -cvf mods/zoo.animal.feeding.jar -C feeding/.
```

Aquí no hay nada específico de módulos. **Se empaqueta** todo lo que está debajo del directorio `feeding` y **se almacena** en un archivo JAR llamado `zoo.animal.feeding.jar` dentro de la carpeta `mods`. Esto representa cómo verá el JAR del módulo otro código que quiera usarlo.

Ahora **se ejecuta** el programa de nuevo, pero esta vez usando el directorio `mods` en lugar de las clases sueltas:

```
java -p mods
   -m zoo.animal.feeding/zoo.animal.feeding.Task
```

**Se puede notar** que este comando es idéntico al de la sección anterior excepto por el directorio. En el ejemplo anterior era `feeding`. En este es el `module path` de `mods`. Dado que se usa el `module path`, **se está** ejecutando un JAR de módulo.
