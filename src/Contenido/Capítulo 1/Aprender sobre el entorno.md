
El entorno Java consta de diversas tecnologías. En las siguientes secciones, se repasan los términos y acrónimos clave que es necesario conocer y luego se discute qué software se necesita para estudiar para el examen.

### Componentes principales de Java

El Kit de Desarrollo de Java (**JDK**) contiene el software mínimo necesario para realizar desarrollo en Java. Los comandos clave incluyen los siguientes:

- **javac**: Convierte archivos fuente `.java` en _bytecode_ `.class`.
- **java**: Ejecuta el programa.
- **jar**: Empaqueta archivos juntos.
- **javadoc**: Genera documentación.

El programa `javac` genera instrucciones en un formato especial llamado _bytecode_ que el comando `java` puede ejecutar. Luego, `java` lanza la Máquina Virtual de Java (**JVM**) antes de ejecutar el código. La JVM sabe cómo ejecutar el _bytecode_ en la máquina real en la que se encuentra. Se puede pensar en la JVM como una caja mágica especial en la máquina que sabe cómo ejecutar el archivo `.class` dentro del sistema operativo y hardware particulares.

### ¿Adónde fue el JRE?

En Java 8 y versiones anteriores, se podía descargar un Entorno de Ejecución de Java (**JRE**) en lugar del JDK completo. El JRE era un subconjunto del JDK que se utilizaba para ejecutar un programa pero no podía compilar uno. Ahora, se puede utilizar el JDK completo al ejecutar un programa Java. Alternativamente, los desarrolladores pueden suministrar un ejecutable que contenga las piezas requeridas que habrían estado en el JRE.

Es posible que se haya notado que se dijo que el JDK contiene el software mínimo necesario. Muchos desarrolladores utilizan un entorno de desarrollo integrado (**IDE**) para facilitar la escritura y ejecución de código. Si bien no se recomienda utilizar uno mientras se estudia para el examen, es bueno saber que existen. Los IDEs de Java comunes incluyen Eclipse, IntelliJ IDEA y Visual Studio Code.

### Descarga de un JDK

Cada seis meses, Oracle lanza una nueva versión de Java. Java 21 salió en septiembre de 2023. Esto significa que Java 21 no será la última versión cuando se descargue el JDK para estudiar para el examen. Sin embargo, se debe seguir utilizando Java 21 para estudiar, dado que este es un examen de Java 21. Las reglas y el comportamiento pueden cambiar con versiones posteriores de Java. ¡No se desearía responder incorrectamente una pregunta por haber estudiado con una versión diferente de Java!

Se puede descargar el JDK de Oracle en el sitio web de Oracle, utilizando la misma cuenta que se usa para registrarse en el examen. Hay muchos JDK disponibles, siendo el más popular, además del JDK de Oracle, **OpenJDK**.

Muchas versiones de Java incluyen características preliminares (_preview features_) que están desactivadas por defecto pero que se pueden habilitar. Las características preliminares no están en el examen. Para evitar confusión sobre cuándo se agregó una característica al lenguaje, se dirá «se introdujo oficialmente en» para denotar cuándo salió de la fase preliminar.

### Verificación de la versión de Java

Antes de continuar, se ruega aprovechar esta oportunidad para asegurar que se tiene la versión correcta de Java en la ruta (_path_).

``` Bash
javac -version
java -version
```

Ambos comandos deberían incluir el número de versión 21.