Hasta ahora, **se ha estado** trabajando con módulos que **se escribieron**. Incluso las clases integradas en el JDK están modularizadas. En esta sección, **se muestra** cómo usar comandos para conocer los módulos.

No **se necesita** conocer la salida de los comandos de esta sección. Sin embargo, **se necesita** conocer la sintaxis de los comandos y lo que hacen. **Se incluye** la salida donde facilita recordar qué es lo que está sucediendo, pero no **hay que** memorizarla (lo que libera más espacio en la cabeza para memorizar opciones de línea de comandos).

### Identificando los módulos integrados

El módulo más importante por conocer es `java.base`. **Contiene** la mayoría de los paquetes que **se han estado** aprendiendo para el examen. De hecho, es tan importante que ni siquiera **hay que** usar la directiva `requires`: **está** disponible para todas las aplicaciones modulares. El archivo `module-info.java` seguirá compilando si **se requiere** explícitamente `java.base`. Sin embargo, es redundante, así que es mejor omitirlo. La Tabla 12.6 **enumera** algunos módulos comunes y lo que contienen.

**TABLA 12.6** Módulos comunes

| **Nombre del módulo** | **Qué contiene**                                                 | **Cobertura en el proyecto**                           |
| --------------------- | ---------------------------------------------------------------- | ------------------------------------------------------ |
| `java.base`           | Colecciones, matemáticas, IO, NIO.2, concurrencia, etc.          | La mayor parte de este proyecto                        |
| `java.desktop`        | Abstract Windows Toolkit (AWT) y Swing                           | No aparece en el examen más allá del nombre del módulo |
| `java.logging`        | Logging                                                          | No aparece en el examen más allá del nombre del módulo |
| `java.sql`            | JDBC                                                             | No aparece en el examen más allá del nombre del módulo |
| `java.xml`            | Lenguaje de marcado extensible (Extensible Markup Language, XML) | No aparece en el examen más allá del nombre del módulo |

Los creadores del examen consideran importante reconocer los nombres de los módulos que proporciona el JDK. Aunque no **hay que** conocer los nombres de memoria, **se debe** poder identificarlos en una lista.

Para el examen, **hay que** saber que los nombres de los módulos comienzan con `java` para las APIs que **es probable** que **se usen**, y con `jdk` para las APIs que son específicas del JDK. La Tabla 12.7 **enumera** todos los módulos que comienzan con `java`.

**TABLA 12.7** Módulos Java con prefijo `java`

| | | |
|---|---|---|
| `java.base` | `java.naming` | `java.smartcardio` |
| `java.compiler` | `java.net.http` | `java.sql` |
| `java.datatransfer` | `java.prefs` | `java.sql.rowset` |
| `java.desktop` | `java.rmi` | `java.transaction.xa` |
| `java.instrument` | `java.scripting` | `java.xml` |
| `java.logging` | `java.se` | `java.xml.crypto` |
| `java.management` | `java.security.jgss` | |
| `java.management.rmi` | `java.security.sasl` | |

La Tabla 12.8 **enumera** todos los módulos que comienzan con `jdk`. **Se recomienda** revisarla justo antes del examen para aumentar las probabilidades de que suenen familiares. **Hay que recordar** que no **hay que** memorizarlos.

**TABLA 12.8** Módulos Java con prefijo `jdk`

| | | |
|---|---|---|
| `jdk.accessibility` | `jdk.jcmd` | `jdk.management.agent` |
| `jdk.attach` | `jdk.jconsole` | `jdk.management.jfr` |
| `jdk.charsets` | `jdk.jdeps` | `jdk.naming.dns` |
| `jdk.compiler` | `jdk.jdi` | `jdk.naming.rmi` |
| `jdk.crypto.cryptoki` | `jdk.jdwp.agent` | `jdk.net` |
| `jdk.crypto.ec` | `jdk.jfr` | `jdk.nio.mapmode` |
| `jdk.dynalink` | `jdk.jlink` | `jdk.sctp` |
| `jdk.editpad` | `jdk.jpackage` | `jdk.security.auth` |
| `jdk.hotspot.agent` | `jdk.jshell` | `jdk.security.jgss` |
| `jdk.httpserver` | `jdk.jsobject` | `jdk.xml.dom` |
| `jdk.incubator.vector` | `jdk.jstatd` | `jdk.zipfs` |
| `jdk.jartool` | `jdk.localedata` | |
| `jdk.javadoc` | `jdk.management` | |

### Obteniendo detalles con `java`

El comando `java` **tiene** tres opciones relacionadas con módulos. Una **describe** un módulo, otra **lista** los módulos disponibles y la tercera **muestra** la lógica de resolución de módulos.

> **Nota:** También **es posible** agregar módulos, exports y más en la línea de comandos, pero mejor no hacerlo: es confuso y difícil de mantener. **Hay que notar** que estas banderas están disponibles en `java` pero no en todos los comandos.

### Describiendo un módulo

Supongamos que **se tiene** el archivo JAR del módulo `zoo.animal.feeding` y **se quiere** conocer su estructura de módulo. **Se podría** "descomprimir" el JAR y abrir el archivo `module-info.java`. Esto mostraría que el módulo exporta un paquete y no requiere explícitamente ningún módulo.

```Java
module zoo.animal.feeding {
   exports zoo.animal.feeding;
}
```

Sin embargo, hay una forma más fácil. El comando `java` **tiene** una opción para describir un módulo. Los siguientes dos comandos son equivalentes:

```
java -p mods
   -d zoo.animal.feeding

java -p mods
   --describe-module zoo.animal.feeding
```

Cada uno **imprime** información sobre el módulo. Por ejemplo, podría imprimir esto:

```
zoo.animal.feeding
file:///absolutePath/mods/zoo.animal.feeding.jar
exports zoo.animal.feeding
requires java.base mandated
```

La primera línea es el módulo sobre el que **se preguntó**: `zoo.animal.feeding`. La segunda línea **comienza** con información sobre el módulo. En nuestro caso, es la misma sentencia `exports` de paquete que **se tenía** en el archivo de declaración de módulo. En la tercera línea, **se ve** `requires java.base mandated`. Un momento: la declaración de módulo no especifica claramente ningún módulo del que `zoo.animal.feeding` tenga dependencias.

**Hay que recordar** que el módulo `java.base` es especial: **se agrega** automáticamente como dependencia de todos los módulos. Este módulo **tiene** paquetes de uso frecuente como `java.util`. De eso se trata el `mandated`: **se obtiene** `java.base` independientemente de si **se pidió** o no.

En las clases, el paquete `java.lang` **se importa** automáticamente, ya **se escriba** o no. El módulo `java.base` funciona de la misma manera: **está** disponible automáticamente para todos los demás módulos.

### Más sobre la descripción de módulos

Para el examen, solo **se necesita** saber cómo ejecutar `--describe-module`, no interpretar la salida. Sin embargo, **se pueden** encontrar algunas sorpresas al experimentar con esta característica, por lo que **se describen** aquí con un poco más de detalle.

Supongamos que los siguientes son los contenidos del archivo `module-info.java` de `zoo.animal.care`:

```Java
module zoo.animal.care {
   exports zoo.animal.care.medical to zoo.staff;
   requires transitive zoo.animal.feeding;
}
```

Ahora **se tiene** el comando para describir el módulo y su salida.

```
java -p mods -d zoo.animal.care

zoo.animal.care
file:///absolutePath/mods/zoo.animal.care.jar
requires zoo.animal.feeding transitive
requires java.base mandated
qualified exports zoo.animal.care.medical to zoo.staff
contains zoo.animal.care.details
```

La primera línea de la salida es la ruta absoluta del archivo del módulo. Las dos líneas con `requires` también **deberían** ser familiares: la primera está en el `module-info.java`, y la otra **se agrega** a todos los módulos. A continuación viene algo nuevo: el `qualified exports` es el nombre completo del paquete que **se exporta** a un módulo específico. Finalmente, el `contains` **indica** que hay un paquete en el módulo que no se exporta en absoluto. Esto es cierto: nuestro módulo tiene dos paquetes, y uno está disponible solo para el código dentro del módulo.

### Listando los módulos disponibles

Además de describir módulos, **se puede** usar el comando `java` para listar los módulos disponibles. La forma más simple **lista** los módulos que forman parte del JDK.

```
java --list-modules
```

Cuando **se ejecutó**, la salida continuó durante unas 70 líneas y **comienza** así:

```
java.base@21
java.compiler@21
java.datatransfer@21
```

Este es un listado de todos los módulos que vienen con Java y sus números de versión. **Se puede** ver que **se estaba** usando Java 21 al probar este ejemplo.

Más interesante aún, **se puede** usar este comando con código propio. **Se intenta** de nuevo con el directorio que contiene nuestros módulos del zoológico.

```
java -p mods --list-modules
```

¿Cuántas líneas extra **se esperan** en la salida esta vez? Hay ocho más: los que **se crearon** en este capítulo. Dos de las líneas personalizadas se ven así:

```
zoo.animal.care file:///absolutePath/mods/zoo.animal.care.jar
zoo.animal.feeding
file:///absolutePath/mods/zoo.animal.feeding.jar
```

Dado que estos son módulos personalizados, **se obtiene** una ubicación en el sistema de archivos. Si el proyecto tuviera un número de versión de módulo, tendría tanto el número de versión como la ruta del sistema de archivos.

> **Nota:** **Hay que notar** que `--list-modules` termina tan pronto como imprime los módulos observables. No ejecuta el programa.

### Mostrando la resolución de módulos

Si listar los módulos no **da** suficiente salida, también **se puede** usar la opción `--show-module-resolution`. **Se puede** pensar en ella como una forma de depurar módulos. **Expulsa** mucha salida cuando el programa arranca; luego ejecuta el programa.

```
java --show-module-resolution
   -p feeding
   -m zoo.animal.feeding/zoo.animal.feeding.Task
```

Afortunadamente, no **hay que** entender esta salida. Dicho esto, haberla visto hará más fácil recordarla. Aquí hay un fragmento de la salida:

```
root zoo.animal.feeding file:///absolutePath/feeding/
java.base binds java.desktop jrt:/java.desktop
java.base binds jdk.jartool jrt:/jdk.jartool
…
jdk.security.auth requires java.naming jrt:/java.naming
jdk.security.auth requires java.security.jgss
jrt:/java.security.jgss
…
All fed!
```

**Comienza** listando el módulo raíz, que es el que **se está** ejecutando: `zoo.animal.feeding`. Luego **lista** muchas líneas de paquetes incluidos por el módulo obligatorio `java.base`. Después de un rato, **lista** los módulos que tienen dependencias. Finalmente, **imprime** el resultado del programa: `All fed!`.

### Describiendo con `jar`

Al igual que el comando `java`, el comando `jar` puede describir un módulo. Estos comandos son equivalentes:

```
jar -f mods/zoo.animal.feeding.jar -d

jar --file mods/zoo.animal.feeding.jar --describe-module
```

La salida es ligeramente diferente de cuando **se usó** el comando `java` para describir el módulo. Con `jar`, **se imprime** lo siguiente:

```
zoo.animal.feeding
jar:file:///absolutePath/mods/zoo.animal.feeding.jar
/!module-info.class
exports zoo.animal.feeding
requires java.base mandated
```

La versión JAR **incluye** el `module-info.class` en el nombre del archivo, lo que no es una diferencia particularmente significativa en el esquema de las cosas. No **hay que** conocer esta diferencia. **Sí hay que** saber que ambos comandos pueden describir un módulo.

### Conociendo las dependencias con `jdeps`

El comando `jdeps` **proporciona** información sobre las dependencias dentro de un módulo. A diferencia de describir un módulo, **examina** el código además de la declaración de módulo. Esto **indica** qué dependencias se usan realmente, en lugar de simplemente cuáles están declaradas. Afortunadamente, no **se espera** memorizar todas las opciones para el examen.

**Se espera** saber cómo usar `jdeps` con proyectos que aún no han sido modularizados, para ayudar a identificar dependencias y problemas. Primero, **se creará** un archivo JAR a partir de esta clase. Si **se está** siguiendo paso a paso, **se puede** copiar la clase de los ejemplos en línea referenciados al comienzo del capítulo en lugar de escribirla.

```Java
// Animatronic.java
package zoo.dinos;

import java.time.*;
import java.util.*;
import sun.misc.Unsafe;

public class Animatronic {
   private List<String> names;
   private LocalDate visitDate;

    public Animatronic(List<String> names, LocalDate visitDate)
{
       this.names = names;
       this.visitDate = visitDate;
    }
    public void unsafeMethod() {
       Unsafe unsafe = Unsafe.getUnsafe();
    }
}
```

Este ejemplo es tonto. **Usa** varias clases sin relación entre sí. El Zoológico del Bronx realmente tuvo dinosaurios electrónicos en movimiento durante un tiempo, así que al menos la idea de tener dinosaurios en un zoológico no está fuera del reino de lo posible.

Ahora **se puede** compilar este archivo. **Se habrá notado** que no hay archivo `module-info.java`. Esto es porque no **se está** creando un módulo: **se está** investigando qué dependencias **se necesitarán** cuando **se modularice** este JAR.

```
javac zoo/dinos/*.java
```

La compilación funciona, pero **da** algunas advertencias sobre que `Unsafe` es una API interna. **No hay que** preocuparse por eso por ahora; **se** comenta enseguida. (Quizá los dinosaurios se extinguieron porque hicieron algo inseguro.)

A continuación, **se crea** un archivo JAR.

```
jar -cvf zoo.dino.jar .
```

**Se puede** ejecutar el comando `jdeps` contra este JAR para conocer sus dependencias. Primero, **se ejecuta** el comando sin ninguna opción. En las dos primeras líneas, el comando **imprime** los módulos que **habría que** agregar con una directiva `requires` para migrar al sistema de módulos. También **imprime** una tabla que muestra qué paquetes se usan y a qué módulos corresponden.

```
jdeps zoo.dino.jar

zoo.dino.jar -> java.base
zoo.dino.jar -> jdk.unsupported
   zoo.dinos    -> java.lang             java.base
   zoo.dinos    -> java.time             java.base
   zoo.dinos    -> java.util             java.base
   zoo.dinos    -> sun.misc              JDK internal API
(jdk.unsupported)
```

**Hay que notar** que `java.base` **está** siempre incluido. También **indica** qué módulos contienen los paquetes que usa el JAR. Si **se ejecuta** en modo resumen, solo **se ve** la primera parte, donde `jdeps` **lista** los módulos. Hay dos formatos para la bandera de resumen:

```
jdeps -s zoo.dino.jar
jdeps -summary zoo.dino.jar

zoo.dino.jar -> java.base
zoo.dino.jar -> jdk.unsupported
```

Para un proyecto real, la lista de dependencias podría incluir docenas o incluso cientos de paquetes. Es útil ver el resumen de solo los módulos. Este enfoque también facilita ver si `jdk.unsupported` está en la lista.

También hay una opción `--module-path` que **se puede** usar si **se quieren** buscar módulos fuera del JDK. A diferencia de otros comandos, no hay forma corta para esta opción en `jdeps`.

> **Nota:** **Se habrá notado** que `jdk.unsupported` no está en la lista de módulos que **se vio** en la Tabla 12.8. Es especial porque contiene bibliotecas internas que a los desarrolladores de versiones anteriores de Java **se les disuadía** de usar, aunque muchas personas ignoraron esa advertencia. **No se debería** hacer referencia a él, ya que podría desaparecer en futuras versiones de Java.

### Usando la bandera `--jdk-internals`

El comando `jdeps` **tiene** una opción para proporcionar detalles sobre estas APIs no soportadas. La salida se ve algo así:

```
jdeps --jdk-internals zoo.dino.jar

zoo.dino.jar -> jdk.unsupported
   zoo.dinos.Animatronic -> sun.misc.Unsafe
      JDK internal API (jdk.unsupported)

Warning: <omitted warning>

JDK Internal API          Suggested Replacement
________________          _____________________
sun.misc.Unsafe           See http://openjdk.java.net/jeps/260
```

La opción `--jdk-internals` **lista** cualquier clase que **se esté** usando y que llame a una API interna, junto con cuál es esa API. Al final, **proporciona** una tabla que sugiere qué **se debería** hacer al respecto. Si **se escribió** el código que llama a la API interna, este mensaje es útil. Si no, el mensaje sería útil para el equipo que escribió el código. Por otro lado, **puede ser necesario** actualizar o reemplazar ese archivo JAR por completo con uno que corrija el problema. **Hay que notar** que `-jdkinternals` es equivalente a `--jdk-internals`.

> **Escenario del mundo real: Acerca de `sun.misc.Unsafe`**
>
> Antes del Sistema de Plataforma de Módulos de Java, las clases tenían que ser públicas si **se quería** que fueran usadas fuera del paquete. Era razonable usar la clase en el código del JDK, ya que es código de bajo nivel que ya está estrechamente acoplado al JDK. Dado que **se necesitaba** en varios paquetes, la clase se hizo pública. Sun incluso la nombró `Unsafe`, pensando que eso evitaría que alguien la usara fuera del JDK.
>
> Sin embargo, los desarrolladores son astutos y usaron la clase ya que estaba disponible. Varias bibliotecas de código abierto de amplio uso comenzaron a usar `Unsafe`. Aunque es bastante improbable que **se esté** usando esta clase directamente en un proyecto, es probable que **se use** una biblioteca de código abierto que la esté utilizando.
>
> El comando `jdeps` permite examinar esos JARs para ver si **se tendrán** problemas cuando Oracle finalmente impida el uso de esta clase. Si **se encuentran** usos, **se puede** ver si hay una versión posterior del JAR a la cual se pueda actualizar.

### Usando archivos de módulo con `jmod`

El siguiente comando que **se necesita** conocer para el examen es `jmod`. **Se podría** pensar que un archivo JMOD es un archivo de módulo de Java. No exactamente. Oracle recomienda usar archivos JAR para la mayoría de los módulos. Los archivos JMOD se recomiendan solo cuando hay bibliotecas nativas o algo que no puede ir dentro de un archivo JAR. Esto difícilmente le afectará en el mundo real.

Lo más importante que **hay que recordar** es que `jmod` es solo para trabajar con los archivos JMOD. Convenientemente, no **hay que** memorizar la sintaxis de `jmod`. La Tabla 12.9 **enumera** los modos comunes.

**TABLA 12.9** Modos de uso de `jmod`

| **Operación** | **Descripción** |
|---|---|
| `create` | Crea un archivo JMOD. |
| `extract` | Extrae todos los archivos del JMOD. Funciona como descomprimir un ZIP. |
| `describe` | Muestra detalles del módulo, como `requires`. |
| `list` | Lista todos los archivos del archivo JMOD. |
| `hash` | Imprime o registra hashes. |

### Creando imágenes de tiempo de ejecución de Java con `jlink`

Uno de los beneficios de los módulos es la posibilidad de **suministrar** solo las partes de Java que **se necesitan**. El ejemplo del zoológico del inicio del capítulo no tiene muchas dependencias. Si el usuario aún no tiene Java o está en un dispositivo sin mucha memoria, descargar un JDK de más de 150 MB es mucho pedir. **Se ve** cuán grande necesita ser realmente el paquete. Este comando crea nuestra distribución más pequeña, conocida como imagen de tiempo de ejecución (_runtime image_):

```
jlink --module-path mods --add-modules zoo.animal.talks --output zooApp
```

Primero **se especifica** dónde encontrar los módulos personalizados con `-p` o `--module-path`. Luego **se especifican** los nombres de los módulos con `--add-modules`. Esto incluirá las dependencias que **requiere** siempre que **se puedan** encontrar. Finalmente, **se especifica** el nombre de la carpeta de nuestro JDK más pequeño con `--output`.

Nuestra imagen de tiempo de ejecución **se almacena** en el directorio de salida, que contiene los directorios `bin`, `conf`, `include`, `legal`, `lib` y `man` junto con un archivo `release`. Estos **deberían** resultar familiares, ya que **se encuentran** también en el JDK completo.

Cuando **se ejecuta** este comando y **se comprime** en un archivo zip el directorio `zooApp`, este solo pesa 15 MB. Esto es un orden de magnitud más pequeño que el JDK completo. ¿De dónde provino este ahorro de espacio? Hay muchos módulos en el JDK que no **se necesitan**. Además, herramientas de desarrollo como `javac` no **necesitan** estar en una imagen de tiempo de ejecución.

Hay muchos más elementos para personalizar este proceso que no **se necesita** conocer para el examen. Por ejemplo, **se puede** omitir generar la documentación de ayuda y ahorrar aún más espacio.

> **Nota:** **Hay que recordar** que una imagen de tiempo de ejecución de Java, como las que crea `jlink`, es una carpeta que contiene múltiples carpetas, bibliotecas y ejecutables. No **está** empaquetada como un solo "archivo imagen". Como **se verá** en la siguiente sección, **se puede** usar `jpackage` para crear un paquete ejecutable único.

### Creando aplicaciones Java autocontenidas con `jpackage`

A diferencia de `jlink`, que solo puede crear una imagen de tiempo de ejecución, el comando `jpackage` puede crear una imagen de aplicación (_application image_). Una imagen de aplicación es un archivo ejecutable único capaz de **ejecutar** la aplicación en una plataforma específica. Por ejemplo, **se obtiene** un archivo `.exe` para Windows o un archivo `.dmg` para Mac.

Tanto `jlink` como `jpackage` proporcionan las partes de Java que **se necesitan** para **ejecutar**. Y para ambos, no **se necesita** que ya haya un JDK instalado en el sistema donde **se quiera** ejecutar la aplicación.

Además de los formatos de salida, una gran diferencia entre `jlink` y `jpackage` es que `jpackage` puede empaquetar tanto aplicaciones modulares como no modulares, mientras que `jlink` está limitado a las aplicaciones modulares.

Un comando para crear una aplicación modular para el módulo `zoo.animal.feeding` de este capítulo es:

```
jpackage --name feedingTask --module-path mods
   --module zoo.animal.feeding/zoo.animal.feeding.Task
```

En Mac, esto crea el archivo `feedingTask-1.0.dmg`. **Se revisan** las opciones. Primero, `--name` o `-n` **dan** el nombre de la aplicación en el nombre de archivo creado. **Se puede** pasar `--app-version` si **se quiere** una versión distinta de `1.0`, la predeterminada. Los parámetros `--module-path` (o `-p`) y `--module` (o `-m`) **deberían** resultar familiares de la ejecución de una aplicación modular. **Se puede** incluso llamar a `--add-modules` como en `jlink` para **agregar** módulos adicionales que no **están** en el archivo `module-info`.

No **se necesita** saber esto para el examen, pero **se puede** incluso ejecutar `jlink` para crear una imagen de tiempo de ejecución y **usarla** como entrada para crear una imagen de aplicación con `jpackage`.

> **Usando jpackage con aplicaciones no modulares**
>
> **Se puede** ejecutar el comando `jpackage` sobre un JAR no modular. Por ejemplo:
>
> ```
> jpackage --name myApp --input myDir --main-class
> com.wiley.java.Cat
>    --main-jar myApp.jar
> ```
>
> Al igual que en el ejemplo modular, `--name` o `-n` **se usan** para el nombre de la aplicación en el nombre de archivo creado. Cualquiera de `--input` o `-i` especifica el directorio donde **están** los archivos que **se van** a empaquetar. La opción `--main-class` especifica el nombre calificado completo de la clase que **se va** a ejecutar, y la opción `--main-jar` especifica dónde encontrar esa clase.

### Revisando las opciones de línea de comandos

Esta sección presenta varias tablas que cubren lo que **se necesita** saber sobre el uso de opciones de línea de comandos para el examen.

La Tabla 12.10 muestra las operaciones de línea de comandos que **se debe** esperar encontrar en el examen. Hay muchas más opciones en la documentación. Por ejemplo, existe una opción `--module` en `javac` que limita la compilación a ese módulo. Afortunadamente, no **se necesita** conocerlas para el examen.

**TABLA 12.10** Comparando operaciones de línea de comandos

| **Descripción** | **Sintaxis** |
|---|---|
| Compilar código no modular | `javac -cp classpath -d directory classesToCompile`<br>`javac --class-path classpath -d directory classesToCompile`<br>`javac -classpath classpath -d directory classesToCompile` |
| Ejecutar código no modular | `java -cp classpath package.className`<br>`java -classpath classpath package.className`<br>`java --class-path classpath package.className` |
| Compilar módulo | `javac -p moduleFolderName -d directory classesToCompileIncludingModuleInfo`<br>`javac --module-path moduleFolderName -d directory classesToCompileIncludingModuleInfo` |
| Ejecutar módulo | `java -p moduleFolderName -m moduleName/package.className`<br>`java --module-path moduleFolderName --module moduleName/package.className` |
| Describir módulo | `java -p moduleFolderName -d moduleName`<br>`java --module-path moduleFolderName --describe-module moduleName`<br>`jar --file jarName --describe-module`<br>`jar -f jarName -d` |
| Listar módulos disponibles | `java --module-path moduleFolderName --list-modules`<br>`java -p moduleFolderName --list-modules`<br>`java --list-modules` |
| Ver dependencias | `jdeps -summary --module-path moduleFolderName jarName`<br>`jdeps -s --module-path moduleFolderName jarName`<br>`jdeps --jdk-internals jarName`<br>`jdeps -jdkinternals jarName` |
| Mostrar la resolución de módulos | `java --show-module-resolution -p moduleFolderName -m moduleName`<br>`java --show-module-resolution --module-path moduleFolderName --module moduleName` |
| Crear JAR de tiempo de ejecución | `jlink -p moduleFolderName --add-modules moduleName --output zooApp`<br>`jlink --module-path moduleFolderName --add-modules moduleName --output zooApp` |
| Crear una aplicación Java autocontenida | `jpackage -n name -p moduleFolderName -m moduleName/package.className`<br>`jpackage --name name --module-path moduleFolderName --module moduleName/package.className`<br>`jpackage -n myApp -i myDir --main-class package.className --main-jar appJar.jar`<br>`jpackage --name myApp --input myDir --main-class package.className --main-jar appJar.jar` |

La Tabla 12.11 muestra las opciones para `javac`, la Tabla 12.12 muestra las opciones para `java`, la Tabla 12.13 muestra las opciones para `jar` y la Tabla 12.14 muestra las opciones para `jdeps`. Finalmente, la Tabla 12.15 muestra las opciones para `jlink` y la Tabla 12.16 muestra las opciones para `jpackage`.

**TABLA 12.11** Opciones que se necesita conocer para el examen: `javac`

| **Opción** | **Descripción** |
|---|---|
| `-cp <classpath>`<br>`-classpath <classpath>`<br>`--class-path <classpath>` | Ubicación de los JARs en un programa no modular |
| `-d <dir>` | Directorio en el que **se colocan** los archivos de clase generados |
| `-p <path>`<br>`--module-path <path>` | Ubicación de los JARs en un programa modular |

**TABLA 12.12** Opciones que se necesita conocer para el examen: `java`

| **Opción** | **Descripción** |
|---|---|
| `-cp <classpath>`<br>`-classpath <classpath>`<br>`--class-path <classpath>` | Ubicación de los JARs en un programa no modular |
| `-p <path>`<br>`--module-path <path>` | Ubicación de los JARs en un programa modular |
| `-m <name>`<br>`--module <name>` | Nombre del módulo que **se va** a ejecutar |
| `-d`<br>`--describe-module` | Describe los detalles de un módulo |
| `--list-modules` | Lista los módulos observables sin ejecutar el programa |
| `--show-module-resolution` | Muestra los módulos al ejecutar el programa |

**TABLA 12.13** Opciones que se necesita conocer para el examen: `jar`

| **Opción** | **Descripción** |
|---|---|
| `-c`<br>`--create` | Crea un archivo JAR nuevo |
| `-v`<br>`--verbose` | Imprime detalles al trabajar con archivos JAR |
| `-f`<br>`--file` | Nombre del archivo JAR |
| `-C <dir>` | Directorio que contiene los archivos que **se usarán** para crear el JAR |
| `-d`<br>`--describe-module` | Describe los detalles de un módulo |

**TABLA 12.14** Opciones que se necesita conocer para el examen: `jdeps`

| **Opción** | **Descripción** |
|---|---|
| `--module-path <path>` | Ubicación de los JARs en un programa modular |
| `-s`<br>`-summary` | Resume la salida |
| `--jdk-internals`<br>`-jdkinternals` | Lista los usos de las API internas |

**TABLA 12.15** Opciones que se necesita conocer para el examen: `jlink`

| **Opción** | **Descripción** |
|---|---|
| `-p <path>`<br>`--module-path <path>` | Ubicación de los JARs en un programa modular |
| `--add-modules <name>` | Lista de módulos que **se van** a empaquetar |
| `--output` | Nombre del directorio de salida |

**TABLA 12.16** Opciones que se necesita conocer para el examen: `jpackage`

| **Opción** | **Descripción** |
|---|---|
| `-n <name>`<br>`--name <name>` | Prefijo del archivo de salida. **Se combinará** con un número de versión y un sufijo. |
| `-p <path>`<br>`--module-path <path>` | Ubicación de los JARs en un programa modular. |
| `--add-modules <name>` | Lista de módulos que **se van** a empaquetar. |
| `--app-version` | Número de versión opcional para el nombre del archivo de salida. **Será** `1.0` por defecto si no **se especifica**. |
| `--i <dir>`<br>`--input <dir>` | Directorio de entrada que contiene los archivos que **se van** a empaquetar para una aplicación no modular. |
| `--main-class` | Nombre calificado completo de la clase que **se va** a ejecutar en un JAR no modular. |
| `--main-jar` | JAR no modular que contiene la clase que **se va** a ejecutar. |
