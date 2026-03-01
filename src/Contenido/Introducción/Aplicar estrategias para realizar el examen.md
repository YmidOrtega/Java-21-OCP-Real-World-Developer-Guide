### Trayendo tu Propio Papel en Blanco

Actualmente, Oracle no proporciona materiales de escritura en línea (como una pizarra/tablero digital). Están permitiendo usar papel en blanco como material de escritura. Si traes papel, asegúrate de informar a tu supervisor antes de comenzar. El supervisor puede querer ver que el papel está en blanco sosteniéndolo ante la cámara.

### Usando el Material de Escritura

- **Rastreo de estado:** Es útil para rastrear el estado de variables primitivas y de referencia, especialmente en preguntas sobre recolección de basura o bucles complejos. Puedes dibujar diagramas de objetos y referencias.

- **Proceso de eliminación:** Para preguntas con código en las opciones, escribe A B C D E en tu papel y tacha las que elimines (por ejemplo, las que no compilan o tienen sintaxis inválida) .

- **Datos de inicio:** Puedes escribir material una vez que comience el examen (no antes). Por ejemplo, listar interfaces funcionales para usarlas en múltiples preguntas.

Por ejemplo, supongamos que te encuentras con
el siguiente fragmento de código en una pregunta sobre la recolección de basura:

```java
Object o = new Turtle(); 
Mammal m = new Monkey(); 
Animal a = new Rabbit(); 
o = m;
```

En una situación como esta, puede ser útil dibujar un diagrama del estado actual de las referencias de variables.

![[Diagrama.png]]

A medida que cada variable de referencia cambia el objeto al que apunta, borra o tacha la flecha entre ellos y dibuja una nueva hacia un objeto diferente.

El uso del material de escritura para realizar un seguimiento del estado también es útil para preguntas complejas que implican un bucle, especialmente preguntas con bucles incrustados. Por ejemplo, el valor de una variable puede cambiar cinco o más veces durante la ejecución de un bucle. Debe utilizar el material de escritura si se le permite para mejorar su puntuación.

En las preguntas con código en las opciones, es útil utilizar el material de escritura para el proceso de eliminación.

### Entendiendo la **Pregunta**

La mayoría de las preguntas del examen contienen fragmentos de código y te piden que respondas preguntas sobre ellos. Para aquellas preguntas que contienen fragmentos de código, la pregunta número uno que te recomiendo que respondas antes de intentar resolver la pregunta es esta:

_¿Necesito determinar si el código se compila?_

La respuesta a esta pregunta determina tu enfoque para resolverla. Si todas las respuestas a una pregunta son valores impresos, es decir, no hay ninguna opción «No se compila», considera esa pregunta como un regalo. Significa que todas las líneas se compilan y que puedes utilizar la información de esta pregunta para responder a otras.

### Desconfiando de Palabras Fuertes

Ten cuidado con cualquier opción de respuesta que incluya palabras fuertes como "debe" (must), "todo" (all), o "no puede" (cannot). Es raro que una regla no tenga excepciones. Si estás atascado entre dos respuestas y una usa "debe" y la otra "puede", es mejor elegir la palabra más débil.

### Manejo del Tiempo

Debes establecer un límite estricto a los cinco minutos restantes para asegurarte de haber respondido todas y cada una de las preguntas. Si no respondes, pierdes puntos; si adivinas, tienes una oportunidad. Si tienes poco tiempo, responde una pregunta lo mejor que puedas y márcala para volver más tarde. Todas las preguntas tienen el mismo peso, así que no gastes 10 minutos en una sola pregunta difícil.
