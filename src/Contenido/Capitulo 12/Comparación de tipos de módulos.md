Todos los módulos que **se han usado** hasta ahora en este capítulo **se llaman** módulos nombrados (_named modules_). Hay otros dos tipos de módulo: los módulos automáticos (_automatic modules_) y los módulos sin nombre (_unnamed modules_). En esta sección **se describen** estos tres tipos de módulo. En el examen, **se necesita** ser capaz de compararlos.

### Módulos nombrados

Un módulo nombrado es aquel que contiene un archivo `module-info.java`. Para repasar, este archivo aparece en la raíz del JAR junto con uno o más paquetes. A menos que **se especifique** lo contrario, un módulo es un módulo nombrado. Los módulos nombrados aparecen en el module path en lugar del classpath. Más adelante **se verá** qué sucede si un archivo JAR que contiene un `module-info.java` está en el classpath. Por ahora, basta con saber que no **se considera** un módulo nombrado porque no está en el module path.

Como forma de recordarlo, un módulo nombrado tiene el nombre dentro del archivo `module-info.java` y está en el module path.

> **Nota:** **Hay que recordar**, del Capítulo 7, «Más allá de las clases», **que** la única manera de que las subclases de clases selladas (_sealed classes_) estén en un paquete diferente es que estén en el mismo módulo nombrado.

### Módulos automáticos

Un módulo automático aparece en el module path pero no contiene un archivo `module-info.java`. Simplemente es un archivo JAR regular que **se coloca** en el module path y **se trata** como un módulo.

Como forma de recordarlo, Java determina el nombre del módulo automáticamente. El código que hace referencia a un módulo automático lo trata como si hubiera un archivo `module-info.java` presente. Exporta automáticamente todos los paquetes. También determina el nombre del módulo. ¿Cómo determina el nombre del módulo? Excelente pregunta.

Para responder esto, **hay que** dar un poco de historia sobre los archivos JAR y la adopción de los módulos. Cada archivo JAR contiene una carpeta especial llamada `META-INF` y, dentro de ella, un archivo de texto llamado `MANIFEST.MF`. Puede **crearse** automáticamente cuando **se crea** el JAR o a mano por el autor del JAR. Volviendo a los módulos, muchas librerías de Java no estaban del todo listas para modularizarse cuando **se introdujo** esta característica. A los autores **se les animó** a declarar el nombre que pretendían usar para el módulo agregando una propiedad llamada `Automatic-Module-Name` en su archivo `MANIFEST.MF`.

### Sobre el archivo MANIFEST.MF

Un archivo JAR contiene un archivo de texto especial llamado `META-INF/MANIFEST.MF` que contiene información sobre el JAR. Existe desde bastante antes que los módulos, desde los primeros días de Java y los JARs, para ser exactos. La siguiente imagen muestra cómo encaja el manifiesto dentro de la estructura de directorios de un archivo JAR.

![[El manifiesto en la estructura de directorios de un JAR.jpg]]

El manifiesto contiene información adicional sobre el archivo JAR. Por ejemplo, a menudo contiene la versión de Java usada para compilar el archivo JAR.

> **Nota:** Para programas de línea de comandos, comúnmente **se especifica** la clase con el método `main()`.
>
> Cada línea en el manifiesto es un par clave/valor separado por dos puntos. **Se puede** pensar en el manifiesto como un mapa de nombres de propiedades y valores. El manifiesto predeterminado en Java 21 tiene el siguiente aspecto:
>
> ```
> Manifest-Version: 1.0
> Created-By: 21 (Oracle Corporation)
> ```

Especificar una única propiedad en el manifiesto permitió a los proveedores de librerías facilitar las cosas a las aplicaciones que querían usar su librería en una aplicación modular. **Se puede** pensar en ello como una promesa de que, cuando la librería se convierta en un módulo nombrado, usará el nombre de módulo especificado.

Si el archivo JAR no especifica un nombre de módulo automático, Java aún permitirá usarlo en el module path. En este caso, Java determinará el nombre del módulo. **Se diría** que esto sucede automáticamente, pero el chiste probablemente ya perdió su gracia.

Java determina el nombre del módulo automático basándolo en el nombre del archivo JAR. **Se repasan** las reglas comenzando con un ejemplo. Supongamos que hay un archivo JAR llamado `holiday-calendar-1.0.0.jar`.

Primero Java eliminará la extensión `.jar` del nombre. Luego Java eliminará la versión del final del nombre del archivo JAR. Esto es importante porque **se quiere** que los nombres de módulo sean consistentes. ¡Tener un nombre de módulo automático diferente cada vez que **se actualice** a una nueva versión no sería bueno! Después de todo, esto **obligaría** a cambiar la declaración de módulo de la aplicación modularizada, bonita y limpia, cada vez que **se incorporara** una versión posterior del JAR del calendario de festivos.

Eliminar la versión y la extensión nos deja con `holiday-calendar`. Esto deja un problema. Los guiones (`-`) no están permitidos en los nombres de módulo. Java resuelve este problema convirtiendo cualquier carácter especial del nombre a puntos (`.`). Como resultado, el nombre del módulo es `holiday.calendar`. Cualquier carácter que no sea letra o número **se considera** un carácter especial en este reemplazo. Finalmente, cualquier punto adyacente o punto al principio/final **se elimina**.

Dado que son varias reglas, **se revisa** el algoritmo en una lista para determinar el nombre de un módulo automático:

- Si el `MANIFEST.MF` especifica un `Automatic-Module-Name`, **se usa** ese.
- De lo contrario, **se procede** con las reglas restantes.
- **Se elimina** la extensión del archivo del nombre del JAR.
- **Se elimina** cualquier información de versión del final del nombre. Una versión son dígitos y puntos con posible información extra al final: por ejemplo, `-1.0.0` o `-1.0-RC`.
- **Se reemplazan** los caracteres restantes que no sean letras o números por puntos.
- **Se reemplaza** cualquier secuencia de puntos por un solo punto.
- **Se elimina** el punto si es el primer o el último carácter del resultado.

La Tabla 12.17 muestra cómo **aplicar** estas reglas a dos ejemplos en los que el manifiesto no especifica un nombre de módulo automático.

**TABLA 12.17** Practicando con nombres de módulos automáticos

| **Descripción** | **Ejemplo 1** | **Ejemplo 2** |
|---|---|---|
| 1. Nombre inicial del JAR | `commons2-x-1.0.0-SNAPSHOT.jar` | `mod_$-1.0.jar` |
| 2. Eliminar la extensión del archivo | `commons2-x-1.0.0-SNAPSHOT` | `mod_$-1.0` |
| 3. Eliminar la información de versión | `commons2-x` | `mod_$` |
| 4. Reemplazar caracteres especiales | `commons2.x` | `mod..` |
| 5. Reemplazar la secuencia de puntos | `commons2.x` | `mod.` |
| 6. Eliminar los puntos al principio/final (el resultado es el nombre del módulo automático) | `commons2.x` | `mod` |

Aunque el algoritmo para crear nombres de módulos automáticos hace lo mejor que puede, no siempre puede producir un buen nombre. Por ejemplo, `1.2.0-calendar-1.2.2-good-1.jar` no es un nombre adecuado. Afortunadamente, tales nombres son raros y están fuera del alcance del examen.

### Módulos sin nombre

Un módulo sin nombre aparece en el classpath. Al igual que un módulo automático, es un archivo JAR regular. A diferencia de un módulo automático, está en el classpath en lugar del module path. Esto significa que un módulo sin nombre **se trata** como código antiguo y como un ciudadano de segunda clase respecto de los módulos.

Un módulo sin nombre normalmente no contiene un archivo `module-info.java`. Si resulta que contiene uno, ese archivo **se ignorará** ya que está en el classpath. Los módulos sin nombre no exportan ningún paquete a los módulos nombrados. El módulo sin nombre puede leer de cualquier JAR que esté en el classpath o en el module path.

**Se puede** pensar en un módulo sin nombre como código que funciona de la manera en que Java funcionaba antes de los módulos. Sí, **se sabe** que es confuso que algo que no es realmente un módulo tenga la palabra módulo en su nombre.

### Revisando los tipos de módulo

**Se pueden** esperar preguntas en el examen comparando los tres tipos de módulo. **Hay que estudiar** la Tabla 12.18 minuciosamente y estar preparado para responder preguntas sobre estos elementos en cualquier combinación. Un punto clave para recordar es que el código en el classpath puede acceder al module path. Por contraste, el código en el module path no puede leer del classpath.

**TABLA 12.18** Propiedades de los tipos de módulo

| **Propiedad** | **Módulo nombrado (_named_)** | **Módulo automático (_automatic_)** | **Módulo sin nombre (_unnamed_)** |
|---|---|---|---|
| ¿Contiene un módulo ______ un archivo `module-info.java`? | Sí | No | Se ignora si está presente |
| ¿Qué paquetes exporta un módulo ______ a los módulos nombrados? | Los del archivo `module-info.java` | Todos los paquetes | Ningún paquete |
| ¿Qué paquetes exporta un módulo ______ a los módulos automáticos? | Los del archivo `module-info.java` | Todos los paquetes | Todos los paquetes |
| ¿Puede leerse un módulo ______ desde otros módulos en el module path? | Sí | Sí | No |
| ¿Puede leerse un módulo ______ desde otros JARs en el classpath? | Sí | Sí | Sí |
