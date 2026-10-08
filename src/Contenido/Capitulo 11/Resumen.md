Este capítulo cubrió una amplia variedad de temas centrados en **construir** aplicaciones que **respondan** bien al cambio. **Se comenzó** la discusión con el manejo de excepciones. Las excepciones **pueden dividirse** en dos categorías: verificadas y no verificadas. En Java, las excepciones verificadas **heredan** `Exception` pero no `RuntimeException` y deben **ser manejadas** o declaradas. Las excepciones no verificadas **heredan** `RuntimeException` o `Error` y no necesitan **ser manejadas** ni declaradas. **Se considera** una mala práctica **atrapar** un `Error`.

**Se pueden** crear excepciones verificadas o no verificadas propias **extendiendo** `Exception` o `RuntimeException`, respectivamente. También **se pueden** definir constructores y mensajes personalizados para las excepciones, que **aparecerán** en los stack traces.

La gestión automática de recursos **puede habilitarse** usando una sentencia try-with-resources para asegurarse de que los recursos **se cierren** apropiadamente. Los recursos **se cierran** al finalizar el bloque `try`, en el orden inverso al que **se declararon**. Una excepción suprimida ocurre cuando **se lanza** más de una excepción, a menudo como parte de un bloque `finally` o una operación `close()` de try-with-resources.

Java incluye una serie de clases integradas para **formatear** números y fechas. **Se revisó** cómo **crear** formateadores personalizados para cada uno. **Hay que** ser capaz de leer estos formatos personalizados cuando **se encuentren** en el examen.

La localización implica **crear** programas que **se adapten** al cambio. **Se puede** crear una clase `Locale` con un código de idioma en minúsculas requerido y un código de país en mayúsculas opcional. Por ejemplo, `en` y `en_US` son locales para inglés e inglés de EE.UU., respectivamente. **Hay que** saber cómo **formatear** valores de número y fecha/hora basándose en el local, incluyendo la nueva clase `CompactNumberFormat`.

Un `ResourceBundle` permite **especificar** pares clave/valor en un archivo de propiedades. Java **recorre** los paquetes de recursos candidatos del más específico al más general para **encontrar** una coincidencia. Si no **se encuentran** coincidencias para el local solicitado, Java **cambia** al local predeterminado y luego finalmente al paquete de recursos predeterminado. Una vez que **se encuentra** un paquete de recursos coincidente, Java busca solo en la jerarquía de ese paquete de recursos para **seleccionar** valores.

Al **aplicar** los principios aprendidos en este capítulo a los proyectos propios, **se pueden construir** aplicaciones que **duren** más tiempo, con soporte integrado para cualquier evento inesperado que pueda surgir.

---

**Ver también:** [[Comprendiendo las Excepciones]] | [[Reconociendo las Clases de Excepción]] | [[Manejando Excepciones]] | [[Formateando Valores]] | [[Internacionalización y Localización]] | [[Contenido/Capitulo 11/Aspectos esenciales del examen]]
