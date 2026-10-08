## Aspectos Esenciales del Examen

**Comprender los distintos tipos de excepciones.** Todas las excepciones son subclases de `java.lang.Throwable`. Las subclases de `java.lang.Error` **nunca deben ser atrapadas**. Solo las subclases de `java.lang.Exception` **deben manejarse** en el código de la aplicación.

**Diferenciar entre excepciones verificadas y no verificadas.** Las excepciones no verificadas no necesitan **ser atrapadas** ni manejadas y son subclases de `java.lang.RuntimeException` o `java.lang.Error`. Todas las demás subclases de `java.lang.Exception` son excepciones verificadas y deben **ser manejadas** o declaradas.

**Comprender el flujo de una sentencia try.** Una sentencia `try` debe tener un bloque `catch` o un bloque `finally`. Múltiples bloques `catch` **pueden encadenarse**, siempre que no aparezca un tipo de excepción de superclase en un bloque `catch` anterior al de su subclase. Una expresión multi-catch **puede usarse** para **manejar** múltiples excepciones en el mismo bloque `catch`, siempre que una excepción no sea subclase de otra. El bloque `finally` **se ejecuta** al final independientemente de si **se lanza** una excepción.

**Ser capaz de seguir el orden de una sentencia try-with-resources.** Una sentencia try-with-resources es un tipo especial de bloque `try` en el que uno o más recursos **se declaran** y **se cierran** automáticamente en el orden inverso al que **se declararon**. **Puede usarse** con o sin un bloque `catch` o `finally`, con el bloque `finally` implícito siempre **ejecutándose** primero.

**Ser capaz de escribir métodos que declaren excepciones.** **Hay que** comprender la diferencia entre las palabras clave `throw` y `throws` y cómo **declarar** métodos con excepciones. **Hay que** saber cómo **sobreescribir** correctamente un método que declara excepciones.

**Identificar cadenas de locale válidas.** **Hay que** saber que el código de idioma es en minúsculas y obligatorio, mientras que el código de país es en mayúsculas y opcional. **Hay que** ser capaz de **seleccionar** un local usando una constante incorporada, un método de fábrica o una clase builder.

**Formatear fechas, números y mensajes.** **Hay que** ser capaz de **formatear** fechas, números y mensajes en varios formatos `String`, y saber cómo el local **influencia** estos formatos. **Hay que** saber cómo los distintos formateadores de número (moneda, porcentaje, compacto) **difieren**. **Hay que** ser capaz de **escribir** un formateador de fecha o número personalizado usando símbolos, incluyendo saber cómo **escapar** valores literales.

**Determinar qué paquete de recursos usará Java para buscar una clave.** **Hay que** ser capaz de **crear** paquetes de recursos para un conjunto de locales usando archivos de propiedades. **Hay que** conocer el orden de búsqueda que Java usa para **seleccionar** un paquete de recursos y cómo **se consideran** el local predeterminado y el paquete de recursos predeterminado. Una vez que **se encuentra** un paquete de recursos, **hay que** reconocer la jerarquía usada para **seleccionar** valores.

---

**Ver también:** [[Contenido/Capitulo 11/Resumen]] | [[Comprendiendo las Excepciones]] | [[Reconociendo las Clases de Excepción]] | [[Manejando Excepciones]] | [[Formateando Valores]] | [[Internacionalización y Localización]]
