### Entendiendo el examen

Al fin y al cabo, el examen es una lista de preguntas. Cuanto más sepas sobre la estructura del examen, mejor te irá probablemente. Por ejemplo, saber cuántas preguntas contiene el examen te permite gestionar mejor tu progreso y el tiempo restante. En esta sección, discutimos los detalles del examen, junto con algo de historia de los exámenes de certificación anteriores.

### Eligiendo qué examen tomar

[[Java]] tiene ahora unos 30 años, celebrando su "nacimiento" en 1995. Como con cualquier cosa de esa edad, hay una buena cantidad de historia y variación entre las diferentes versiones de Java. A lo largo de los años, los exámenes de certificación han cambiado para cubrir diferentes temas. El número de exámenes y los nombres de las certificaciones también han cambiado.

Oracle ha simplificado las cosas con el tiempo. Convertirse en un OCP ahora requiere aprobar solo un examen, no dos, y no hay exámenes de actualización (upgrade) para Java 21. Independientemente de las certificaciones previas que poseas, todos toman el mismo examen de Java 21 para convertirse en un Oracle Certified Professional.

Existe otro examen menos popular llamado examen _Java Foundations_. Mi consejo es tomar el examen Java Foundations solo si tu empleador te lo ha pedido específicamente, ya que no está destinado a profesionales que trabajan con Java todos los días.

### Diferencias entre el examen Java 21 y los exámenes Java anteriores

Si estás certificado con una versión anterior de Java, podrías esperar que el examen de Java 21 sea muy parecido a los exámenes que tomaste en el pasado. Si bien hay algunas similitudes, el examen también contiene algunas diferencias importantes que debes conocer:

- Las preguntas son generalmente más largas, tienen más código para leer y contienen muchas más opciones de respuesta (hasta 10).
- Las preguntas a menudo abarcan múltiples objetivos independientes.
- Es más probable que te quedes sin tiempo en este examen.
- Ya no existe una función en el software del examen para hacer clic derecho y tachar las opciones que has eliminado.

En mi experiencia, encontré que este es un examen más desafiante que algunos de los exámenes de Java del pasado, por lo que es importante asignar una cantidad adecuada de tiempo para estudiar para el examen.

### Tomando el examen por Internet 

En el pasado, el examen se ofrecía tanto en un centro de pruebas físico como en Internet a través de un supervisor remoto. Oracle ahora solo ofrece el examen en Internet, lo que significa que necesitarás tomarlo de forma remota. Necesitarás un entorno tranquilo, libre de interrupciones durante la duración del examen.

Antes de tomar el examen, también necesitarás instalar un software que monitorea tu sesión y asegura que la integridad del examen no sea violada. Por esta razón, no recomiendo tomar el examen con una computadora del trabajo, donde la instalación de software puede estar restringida.

### Revisando el formato del examen

En el momento en que se publica este proyecto, los detalles del examen son los siguientes:

- **Límite de tiempo:** 120 minutos.
- **Número de preguntas:** 50.   
- **Puntuación para aprobar:** 68% (34 preguntas).

Oracle tiene tendencia a modificar la duración del examen y la puntuación de aprobación una vez que sale. A Oracle también le gusta "ajustar" los objetivos del examen con el tiempo. No sería una sorpresa que Oracle hiciera cambios menores en los objetivos del examen, el número de preguntas o la puntuación de aprobación después de que este proyecto se publica.

### Considerando los Objetivos del examen

Oracle proporciona una lista de objetivos para guiarte sobre qué estudiar para el examen. Cada objetivo define una lista de subobjetivos que proporcionan detalles adicionales sobre el objetivo. Desafortunadamente, los objetivos no abarcan la cantidad total de material necesario para aprobar el examen.

Como punto de partida, debes revisar la lista de objetivos presentada en esta introducción y marcar los que no te resulten familiares.

#### Alcance de los Objetivos

En exámenes de certificación anteriores, la lista de objetivos del examen tendía a incluir temas específicos, clases y APIs que necesitabas conocer. Por ejemplo, una versión anterior era detallada y describía clases específicas (como `BufferedReader`, `FileWriter`, etc.). La versión más nueva (para Java 21) es mucho más vaga, como "Leer y escribir datos de consola y archivos usando flujos de E/S". Esto da a los redactores del examen mucha más libertad para insertar una nueva característica sin tener que actualizar la lista de objetivos.

#### Objetivos "No Oficiales" (Supuestos del Examen)

Oracle tiene el hábito de agregar asunciones y requisitos adicionales fuera del alcance de los objetivos del examen. Para el 1Z0-830, eso incluye lo siguiente:

##### Supuestos del Examen:

- **Paquetes y declaraciones de importación faltantes:** Si el código de muestra no incluye declaraciones de paquete o importación y la pregunta no se refiere explícitamente a ellas, asume que todo el código de muestra está en el mismo paquete o que las importaciones existen para soportarlo.
- **Sin nombres de ruta de archivo o directorio:** Si una pregunta no indica los nombres de archivo o ubicaciones, asume una de las siguientes opciones (la que permita compilar): Todas las clases están en un archivo, o cada clase está en un archivo separado en un mismo directorio.  
- **Saltos de línea no intencionados:** El código de muestra podría tener saltos de línea no intencionados. Si ves una línea que parece haberse envuelto (por ejemplo, un literal de String entre comillas), asume que es una extensión de la misma línea y no un retorno de carro duro que causaría un fallo de compilación.
- **Fragmentos de código:** Asume que todo el código de soporte necesario existe y que el entorno soporta completamente la correcta compilación y ejecución.  
- **Comentarios descriptivos:** Toma los comentarios descriptivos, como "setters y getters van aquí", al pie de la letra. Asume que el código correcto existe, compila y se ejecuta exitosamente.

##### Expectativas del Examen

- Entender los conceptos básicos de la API de Java Logging.
- Usar Anotaciones como `@Override`, `@FunctionalInterface`, `@Deprecated`, `@SuppressWarnings`, y `@SafeVarargs`.
- Usar genéricos, incluyendo comodines (wildcards).

Puedes asumir que cosas externas como importaciones, estructura de archivos y saltos de línea no están siendo evaluadas, a menos que la pregunta lo pregunte específicamente.

### Eligiendo la(s) respuesta(s) correcta(s)

El examen consiste enteramente en preguntas de opción múltiple. Si una pregunta tiene más de una respuesta, la pregunta indica específicamente cuántas respuestas correctas hay. Este proyecto no hace eso; decidí que "Elige todas las que apliquen" para hacer las preguntas más difíciles. La idea es darte más práctica para que puedas detectar la respuesta correcta más fácilmente en el examen real.

### Leyendo el código del examen

Muchas de las preguntas en el examen son fragmentos de código en lugar de clases completas. Ahorrar espacio al no incluir importaciones y/o definiciones de clase deja lugar para mucho otro código. Solo debes enfocarte en las declaraciones de importación cuando la pregunta pregunte específicamente sobre ellas. Por ejemplo, es habitual encontrar clases en el examen con declaraciones de importación y partes omitidas, como esta: 

```java
public class Zoo implements Serializable {
	String name;
	// Se omiten los getter/setter/constructores.
}
```

En este caso, se puede suponer que `java.io.Serializable` está importado y  que existen métodos como `getName()` y `setName()`, así como los constructores relacionados. Por ejemplo, esperaríamos que este código se compilara:

```java
var name = new Zoo(«Java Zoo»).getName();
```
### Encontrando material fuera del alcance

Cuando tomes el examen, puedes ver algunas preguntas que parecen estar fuera del alcance. ¡No entres en pánico!. A menudo, estas preguntas no requieren saber nada sobre el tema para responderlas. Por ejemplo, después de leer este proyecto, debería ser capaz de detectar que lo siguiente no se compila, incluso si nunca ha oído hablar de la clase `java.util.logging.Logger`.

```java
final Logger myLogger = Logger.getAnonymousLogger(); 
myLogger = Logger.getLogger(String.class.getName());
```

Las clases y métodos utilizados en esta pregunta no están dentro del alcance del examen, pero la razón por la que no se compila sí lo está. En concreto, debes saber que no se puede reasignar una variable marcada como final.

¿Lo ves? No da tanto miedo, ¿verdad? Espera encontrar al menos algunas estructuras en el examen con las que no estés familiarizado. Si no forman parte del material de preparación del examen, no es necesario que las entiendas para responder a la pregunta.

### Revisando los tipos de preguntas

Ser consciente de estas categorías de preguntas puede ayudarte a obtener una puntuación más alta:

1. **Problemas de Palabras:** Un pequeño porcentaje no implica leer código. Son rápidas de resolver (ej. cuáles afirmaciones son verdaderas sobre E/S).
2. **Preguntas de Código con Información Extra:** Imagina que la pregunta dice "XMLParseException es una excepción chequeada". Está bien si no sabes qué es XML; la pregunta es un regalo porque te da la información necesaria sobre el manejo de excepciones .
3. **Preguntas de Código con Preguntas Integradas:** Para responder algunas preguntas del examen, es posible que tengas que responder dos o tres subpreguntas. Por ejemplo, la pregunta puede contener dos líneas en blanco y pedirte que elijas las dos respuestas que completan cada espacio en blanco. En algunos casos, las dos opciones de respuesta no están relacionadas, lo que significa que en realidad estás respondiendo varias preguntas, ¡no solo una! Estas preguntas son más difíciles y requieren más tiempo porque contienen varias preguntas, a menudo independientes, que hay que responder. Desafortunadamente, el examen no otorga crédito parcial, así que ten cuidado al responder preguntas como estas.
4. **Preguntas de Código con Opciones Largas:** Las más difíciles tienen múltiples líneas de código en cada opción (hasta 30 líneas). Recomiendo revisar las respuestas e identificar las partes que son diferentes para eliminar opciones más rápido.
5. **Preguntas de Código con Conceptos Inventados o Incorrectos:** El examen puede usar un término que no tiene sentido (como decir falsamente que un `record` extiende otro `record`) o una palabra clave que no existe en Java, como `struct`. Debes leer cuidadosamente y reconocer la terminología inválida.
6. **Preguntas que Realmente Están Fuera del Alcance:** Al introducir nuevas preguntas, Oracle las incluye inicialmente como preguntas sin puntuar. Esto permite a los creadores del examen ver cómo se desenvuelven los examinados reales sin que ello afecte a su puntuación. Seguirá recibiendo el número de preguntas que figura en el examen. Sin embargo, es posible que algunas de ellas no cuenten. Estas preguntas sin puntuar pueden contener material fuera de alcance o incluso errores. No se marcarán como sin puntuar, por lo que deberá esforzarse al máximo para responderlas. Siga el consejo anterior y asuma que todo lo que no haya visto antes es correcto. ¡Así estará cubierto si la pregunta cuenta!
