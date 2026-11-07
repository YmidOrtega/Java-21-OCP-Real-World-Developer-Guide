Muchas aplicaciones no fueron diseñadas para usar el Sistema de Módulos de la Plataforma Java porque fueron escritas antes de que este fuera creado o eligieron no usarlo. Idealmente, al menos fueron diseñadas con proyectos en lugar de como una gran bola de barro. Esta sección ofrece una visión general de las estrategias para migrar una aplicación existente para que use módulos. **Se cubren** el ordenamiento de los módulos, la migración de abajo hacia arriba, la migración de arriba hacia abajo y cómo dividir un proyecto existente.

> **Escenario del mundo real: Migrando aplicaciones en el trabajo**
>
> El examen existe en un universo de mentira donde no hay dependencias de código abierto y las aplicaciones son muy pequeñas. Estos escenarios hacen que aprender y discutir la migración sea mucho más fácil. En el mundo real, las aplicaciones tienen librerías que no se han actualizado en 10 o más años, grafos de dependencias complejos y todo tipo de sorpresas.
>
> **Hay que notar** que **se pueden** usar todas las características de Java 21 sin convertir la aplicación a módulos (¡excepto las características de este capítulo de módulos, por supuesto!). Por favor, **hay que asegurarse** de tener un motivo para la migración y no pensar que es obligatoria.
>
> Este capítulo hace un gran trabajo enseñando lo que **hay que** saber para el examen. Sin embargo, no prepara adecuadamente para convertir aplicaciones reales para que usen módulos. Si **se encuentra** en esa situación, **vale la pena considerar** leer *The Java Module System* de Nicolai Parlog (Manning Publications, 2019).

### Determinando el orden

Antes de poder migrar la aplicación para que use módulos, **hay que** saber cómo están estructurados los paquetes y las librerías de la aplicación existente. Supongamos una aplicación simple con tres archivos JAR, como **se muestra** en la siguiente imagen. Las dependencias entre proyectos forman un grafo. Ambas representaciones en la imagen son equivalentes. Las flechas muestran las dependencias apuntando desde el proyecto que necesitará la dependencia hacia el que la pone disponible. En el lenguaje de los módulos, la flecha irá de `requires` a `exports`.

![[Determinación del orden.jpg]]

El lado derecho del diagrama facilita identificar la parte superior e inferior a las que se refieren las migraciones de arriba hacia abajo y de abajo hacia arriba. Los proyectos que no tienen ninguna dependencia están en la parte inferior. Los proyectos que sí tienen dependencias están en la parte superior.

En este ejemplo, solo hay un orden de arriba hacia abajo que honra todas las dependencias. La siguiente imagen muestra que el orden no siempre es único. Dado que dos de los proyectos no tienen una flecha entre ellos, se permite cualquier orden al decidir el orden de migración.

![[Determinación del orden cuando no es único.jpg]]

### Explorando una estrategia de migración de abajo hacia arriba (_bottom-up_)

El enfoque más fácil para la migración es una migración de abajo hacia arriba (_bottom-up_). Este enfoque funciona mejor cuando **se tiene** la posibilidad de convertir cualquier archivo JAR que aún no sean módulos. Para una migración de abajo hacia arriba, **se siguen** estos pasos:

1. **Se elige** el proyecto de nivel más bajo que aún no ha sido migrado. (¿**Se recuerda** la manera en que **se ordenaron** por dependencias en la sección anterior?)
2. **Se agrega** un archivo `module-info.java` a ese proyecto. **Hay que** asegurarse de agregar cualquier `exports` necesario para exponer cualquier paquete usado por los JARs de nivel superior. También **hay que** agregar una directiva `requires` para cualquier módulo del que este módulo dependa.
3. **Se mueve** este módulo nombrado recién migrado del classpath al module path.
4. **Hay que** asegurarse de que cualquier proyecto que aún no haya sido migrado permanezca como módulo sin nombre en el classpath.
5. **Se repite** con el proyecto de siguiente nivel más bajo hasta terminar.

La siguiente imagen muestra este procedimiento aplicado para migrar tres proyectos. **Hay que notar** que cada proyecto **se convierte** en un módulo por turnos.

![[Migración de abajo hacia arriba.jpg]]

Con una migración de abajo hacia arriba, **se están poniendo** los proyectos de nivel inferior en buena forma. Esto facilita migrar los proyectos de nivel superior al final. También fomenta el cuidado con lo que **se expone**.

Durante la migración, **se tiene** una mezcla de módulos nombrados y módulos sin nombre. Los módulos nombrados son los de nivel inferior que han sido migrados. Están en el module path y no se les permite acceder a ningún módulo sin nombre.

Los módulos sin nombre están en el classpath. Pueden acceder a archivos JAR tanto en el classpath como en el module path.

### Explorando una estrategia de migración de arriba hacia abajo (_top-down_)

Una estrategia de migración de arriba hacia abajo (_top-down_) es más útil cuando no **se tiene** el control de todos los archivos JAR que usa la aplicación. Por ejemplo, supongamos que otro equipo es dueño de un proyecto. Simplemente están demasiado ocupados para migrar. No se querría que esta situación frenara toda la migración.

Para una migración de arriba hacia abajo, **se siguen** estos pasos:

1. **Se colocan** todos los proyectos en el module path.
2. **Se elige** el proyecto de nivel más alto que aún no ha sido migrado.
3. **Se agrega** un archivo `module-info.java` a ese proyecto para convertir el módulo automático en un módulo nombrado. Nuevamente, **hay que recordar** agregar cualquier directiva `exports` o `requires`. **Se puede** usar el nombre del módulo automático de otros módulos al escribir la directiva `requires`, ya que la mayoría de los proyectos en el module path todavía no tienen nombres.
4. **Se repite** con el proyecto de siguiente nivel más alto hasta terminar.

La siguiente imagen muestra este procedimiento aplicado para migrar tres proyectos. **Hay que notar** que cada proyecto **se convierte** en un módulo por turnos.

![[Migración de arriba hacia abajo.jpg]]

Con una migración de arriba hacia abajo, **se concede** que todas las dependencias de nivel inferior no están listas, pero que **se quiere** convertir la propia aplicación en un módulo.

Durante la migración, **se tiene** una mezcla de módulos nombrados y módulos automáticos. Los módulos nombrados son los de nivel superior que han sido migrados. Están en el module path y tienen acceso a los módulos automáticos. Los módulos automáticos también están en el module path.

La Tabla 12.19 repasa lo que **hay que** saber sobre las dos estrategias principales de migración. **Hay que** asegurarse de conocerla bien.

**TABLA 12.19** Comparación de estrategias de migración

| **Categoría** | **De abajo hacia arriba** | **De arriba hacia abajo** |
|---|---|---|
| Proyecto que depende de todos los demás | Módulo sin nombre en el classpath | Módulo nombrado en el module path |
| Proyecto que no tiene dependencias | Módulo nombrado en el module path | Módulo automático en el module path |

### Dividiendo un proyecto grande en módulos

Para el examen, **hay que** entender el proceso básico de dividir un proyecto grande en módulos. Por supuesto, no **se dará** un proyecto grande. Después de todo, hay solo tanto espacio para hacer una pregunta. Afortunadamente, el proceso es el mismo para un proyecto pequeño.

Supongamos que **se empieza** con una aplicación que tiene varios paquetes. El primer paso es romperlos en agrupaciones lógicas y dibujar las dependencias entre ellos. La siguiente imagen muestra la descomposición de un sistema imaginario. **Hay que notar** que hay siete paquetes tanto en el lado izquierdo como en el derecho. Hay menos módulos porque algunos paquetes comparten un módulo.

![[Primer intento de descomposición.jpg]]

Hay un problema con esta descomposición. ¿**Se ve** cuál es? El Sistema de Módulos de la Plataforma Java no permite dependencias cíclicas. Una dependencia cíclica (_cyclic dependency_), o dependencia circular, es cuando dos cosas dependen directa o indirectamente la una de la otra. Si el módulo `zoo.tickets.delivery` requiere el módulo `zoo.tickets.discount`, no se permite que `zoo.tickets.discount` requiera el módulo `zoo.tickets.delivery`.

Ahora que **se sabe** que la descomposición de la imagen no funcionará, ¿qué **se puede** hacer al respecto? Una técnica común es introducir otro módulo. Ese módulo contiene el código que los otros dos módulos comparten. La siguiente imagen muestra los nuevos módulos sin ninguna dependencia cíclica. **Hay que notar** el nuevo módulo `zoo.tickets.etech`. **Se crearon** nuevos paquetes para poner en ese módulo. Esto permite a los desarrolladores poner el código común ahí y romper la dependencia. ¡No más dependencias cíclicas!

![[Eliminación de las dependencias cíclicas.jpg]]

### Fallando al compilar con una dependencia cíclica

**Es extremadamente importante entender** que Java no permitirá compilar módulos que tengan dependencias circulares. En esta sección, **se examina** un ejemplo que lleva a ese error del compilador.

**Se considera** el módulo `zoo.butterfly` descrito aquí:

```Java
// Butterfly.java
package zoo.butterfly;
public class Butterfly {
  private Caterpillar caterpillar;
}

// module-info.java
module zoo.butterfly {
   exports zoo.butterfly;
   requires zoo.caterpillar;
}
```

No **se puede** compilar esto todavía, ya que **se necesita** construir `zoo.caterpillar` primero. Después de todo, nuestra mariposa lo requiere. Ahora **se examina** `zoo.caterpillar`:

```Java
// Caterpillar.java
package zoo.caterpillar;
public class Caterpillar {
   Butterfly emergeCocoon() {
      // logic omitted
   }
}

// module-info.java
module zoo.caterpillar {
   exports zoo.caterpillar;
   requires zoo.butterfly;
}
```

No **se puede** compilar esto todavía, ya que **se necesita** construir `zoo.butterfly` primero. ¡Uh-oh! Ahora hay un punto muerto. Ninguno de los dos módulos puede ser compilado. Este es nuestro problema de dependencia circular en acción.

Esta es una de las ventajas del sistema de módulos. **Impide** escribir código que tenga una dependencia cíclica. ¡Ese código ni siquiera compila!

**Se podría** preguntar qué sucede cuando están involucrados tres módulos. Supongamos que el módulo `ballA` requiere el módulo `ballB` y `ballB` requiere el módulo `ballC`. ¿Puede el módulo `ballC` requerir el módulo `ballA`? No. Esto crearía una dependencia cíclica. ¿No **se cree**? **Hay que intentar** dibujarla. **Se puede** seguir el lápiz alrededor del círculo de `ballA` a `ballB` a `ballC` a `ballA` a... bueno, ya **se** entiende la idea. ¡Simplemente hay demasiadas pelotas en el aire!

> **Nota:** Java **permite** que haya una dependencia cíclica entre paquetes dentro de un módulo. **Hace cumplir** que no haya una dependencia cíclica entre módulos.
