Los paquetes se pueden agrupar en módulos. En este capítulo **se explica** el propósito de los módulos y cómo construir los propios. También **se muestra** cómo ejecutarlos y cómo descubrir módulos existentes. A continuación, **se cubren** estrategias para migrar una aplicación a que use módulos, ejecutar una aplicación parcialmente modularizada y manejar dependencias. Después, **se pasa** a discutir los servicios y los localizadores de servicios (_service locator_). Finalmente, **se muestra** cómo crear una imagen de tiempo de ejecución.

Al escribir código para el examen, por lo general **se ven** clases pequeñas. Después de todo, ¡las preguntas del examen tienen que caber en una sola pantalla! Cuando **se trabaja** en programas reales, estos son mucho más grandes. Un proyecto real constará de cientos o miles de clases agrupadas en paquetes. Estos paquetes **se agrupan** en archivos Java archive (JAR). Un JAR es un archivo zip con información adicional, y la extensión es `.jar`.

Además del código escrito por el equipo, la mayoría de las aplicaciones también usan código escrito por otros. El código abierto (_open source_) es software cuyo código se proporciona y que suele ser de uso gratuito. Java tiene una vibrante comunidad de software de código abierto (OSS, _open-source software_), y esas bibliotecas también se proporcionan como archivos JAR. Por ejemplo, hay bibliotecas para leer archivos, conectarse a una base de datos y mucho más. 

Algunos proyectos de código abierto incluso dependen de funcionalidad de otros proyectos de código abierto. Por ejemplo, Spring es un framework de uso común, y JUnit es una biblioteca de pruebas de uso común. Para usar cualquiera de los dos, **hay que asegurarse** de tener disponibles en tiempo de ejecución versiones compatibles de todos los JARs relevantes. Esta compleja cadena de dependencias y versiones mínimas frecuentemente **se conoce** en la comunidad como el _infierno de JARs_ (_JAR hell_). El infierno es una excelente manera de describir la carga de la versión incorrecta de una clase o incluso un `ClassNotFoundException` en tiempo de ejecución.

El Java Platform Module System (JPMS) agrupa el código en un nivel superior. El propósito principal de un módulo es proporcionar grupos de paquetes relacionados que ofrezcan a los desarrolladores un conjunto particular de funcionalidad. Es como un archivo JAR, excepto que un desarrollador elige qué paquetes están accesibles fuera del módulo. A continuación **se analiza** qué son los módulos y qué problemas pretenden resolver.

El Java Platform Module System incluye lo siguiente:

- Un formato para archivos JAR de módulos
- La partición del JDK en módulos
- Opciones de línea de comandos adicionales para las herramientas de Java

### Explorando un módulo

En el Capítulo 1, "Bloques de Construcción", **se tuvo** una pequeña aplicación Zoo. Esta tenía solo una clase e imprimía una sola cosa. Ahora **se imagina** que hay todo un equipo de programadores y que **se están** automatizando las operaciones del zoológico. Hay muchas cosas que codificar, incluidas las interacciones con los animales, los visitantes, el sitio web público y las acciones de divulgación.

Un módulo es un grupo de uno o más paquetes más un archivo especial llamado `module-info.java`. El contenido de este archivo es la declaración de módulo. La siguiente imagen muestra solo algunos de los módulos que un zoológico podría necesitar. **Se decidió** centrar el ejemplo en las interacciones con los animales. El zoológico completo podría tener fácilmente una docena de módulos. En ella **se observa** que hay flechas entre muchos de los módulos. Estas representan dependencias, donde un módulo depende del código de otro. El personal necesita alimentar a los animales para conservar sus empleos. La línea de `zoo.staff` a `zoo.animal.feeding` muestra que el primero depende del segundo.

![[Diseño de un sistema modular.jpg]]

Ahora **se profundiza** en uno de estos módulos. La siguiente imagen muestra qué hay dentro del módulo `zoo.animal.talks`. Hay tres paquetes con dos clases cada uno. (Es un zoológico pequeño.) También hay un archivo extraño llamado `module-info.java`. **Se requiere** que este archivo esté dentro de todos los módulos. **Se explica** esto con más detalle más adelante en el capítulo.

![[Dentro de un módulo.jpg]]

### Beneficios de los módulos

Los módulos parecen ser otra capa de cosas que **se necesita** saber para programar. Si bien el uso de módulos es opcional, es importante comprender los problemas que pretenden resolver.

- **Mejor control de acceso:** Además de los niveles de control de acceso cubiertos en el Capítulo 5, "Métodos", **se pueden** tener paquetes que solo sean accesibles para otros paquetes del módulo.
- **Gestión de dependencias más clara:** Dado que los módulos especifican de qué dependen, Java **puede** señalar un JAR faltante al iniciar el programa en lugar de cuando **se accede** a él por primera vez en tiempo de ejecución.
- **Compilaciones personalizadas de Java:** **Se puede** crear un tiempo de ejecución de Java que tenga solo las partes del JDK que el programa necesita en lugar del completo, que supera los 150 MB.
- **Seguridad mejorada:** Dado que **se pueden** omitir partes del JDK en la compilación personalizada, no **hay que preocuparse** por vulnerabilidades descubiertas en una parte que no se usa.
- **Rendimiento mejorado:** Otro beneficio de un paquete de Java más pequeño es un mejor tiempo de inicio y un menor requisito de memoria.
- **Garantía de paquetes únicos:** Dado que los módulos especifican los paquetes expuestos, Java **puede** garantizar que cada paquete provenga de un solo módulo y evitar confusión sobre lo que se está ejecutando.
