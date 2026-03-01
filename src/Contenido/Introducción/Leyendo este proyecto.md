### Cómo está organizado este proyecto

Este proyecto está dividido en 14 capítulos. Organices los  capítulos orgánicamente para que cada uno se base en el material de los anteriores.

- **Capítulo 1: Bloques de Construcción.** Describe lo básico de Java, variables, primitivos, y recolección de basura .
- **Capítulo 2: Operadores.** Explica operaciones con variables, casting y precedencia.
- **Capítulo 3: Tomando Decisiones.** Cubre constructos lógicos, `if/else`, `switch`, coincidencia de patrones (pattern matching) y bucles.
- **Capítulo 4: APIs Centrales.** Trabaja con `String`, `StringBuilder`, arrays y fechas.
- **Capítulo 5: Métodos.** Explica cómo diseñar métodos y modificadores de acceso.
- **Capítulo 6: Diseño de Clases.** Cubre constructores, herencia, inicialización, clases abstractas y sobrecarga.
- **Capítulo 7: Más allá de las Clases.** Introduce interfaces, enums, clases selladas (sealed classes), records y clases anidadas.
- **Capítulo 8: Lambdas e Interfaces Funcionales.** Muestra cómo usar lambdas y referencias a métodos.
- **Capítulo 9: Colecciones y Genéricos.** Cubre genéricos con comodines y el marco de Colecciones.
- **Capítulo 10: Streams.** Explica las tuberías de stream y la clase `Optional`.
- **Capítulo 11: Excepciones y Localización.** Demuestra tipos de excepciones y formateo para soportar múltiples idiomas.
- **Capítulo 12: Módulos.** Detalla los beneficios de los módulos y cómo migrar aplicaciones .
- **Capítulo 13: Concurrencia.** Introduce hilos virtuales y de plataforma, seguridad de hilos (thread-safety) y streams paralelos.
- **Capítulo 14: E/S (I/O).** Gestionar archivos y directorios usando I/O y NIO.2, serialización e interacción con el usuario .

Al final de cada capítulo encontrarás: **Resumen**, **Esenciales del Examen** y **Preguntas de Repaso** . Debes responder estas preguntas y verificar tus respuestas; si no respondes correctamente al menos el 80%, regresa y repasa el capítulo.

**ADVERTENCIA:** Las preguntas de repaso y práctica en este proyecto no se derivan de las preguntas reales del examen, ¡así que no las memorices!. Aprender el tema subyacente es lo que te ayuda a aprobar.

Para sacar el máximo partido a este proyecto, debe leer cada capítulo de principio a fin antes de pasar a los elementos que se encuentran al final del capítulo. Son muy útiles para comprobar y reforzar su comprensión. Incluso si ya está
familiarizado con un tema, debe leer el capítulo por encima. Hay una serie de sutilezas en Java con las que es fácil no encontrarse, incluso después de trabajar con Java durante años. Por ejemplo, lo siguiente se compila: 

```java
var $num = (Integer)null;
```

Incluso un desarrollador Java con experiencia podría sorprenderse con esto. El examen requiere que conozcas este tipo de sutilezas.
