Ahora que **se han creado** módulos exitosamente, **se puede** aprender más sobre la declaración de módulo. En estas secciones, **se analizan** las directivas `exports`, `requires` y `opens`. En la siguiente sección sobre servicios, **se exploran** `provides` y `uses`. **Sería** buen momento para mencionar que estas directivas **pueden** aparecer en cualquier orden en la declaración de módulo.

### Exportando un paquete

Ya **se ha visto** cómo `exports packageName` exporta un paquete a otros módulos. También **es posible** exportar un paquete a un módulo en específico. Supongamos que el zoológico decide que solo el personal debe tener acceso a las charlas. **Se podría** actualizar la declaración de módulo de la siguiente manera:

```Java
module zoo.animal.talks {
   exports zoo.animal.talks.content to zoo.staff;
   exports zoo.animal.talks.media;
   exports zoo.animal.talks.schedule;

     requires zoo.animal.feeding;
     requires zoo.animal.care;
}
```

En el módulo `zoo.staff` nada ha cambiado. Sin embargo, no **se permitiría** que ningún otro módulo acceda a ese paquete.

Probablemente **se haya notado** que ninguno de nuestros demás módulos requiere `zoo.animal.talks` en primer lugar. Sin embargo, no **se sabe** qué otros módulos existirán en el futuro. **Es importante** considerar el uso futuro al diseñar módulos. Dado que **se quiere** que solo ese módulo tenga acceso, solo **se permite** el acceso para dicho módulo.

> **Nota: tipos exportados**
>
> **Se ha estado** hablando de exportar un paquete. ¿Pero qué significa eso exactamente? Todas las clases públicas, interfaces, enums y records **se exportan**. Además, cualquier campo y método público y protegido en esos archivos **es visible**.
>
> Los campos y métodos privados no **son visibles** porque no **son accesibles** fuera de la clase. De manera similar, los campos y métodos de paquete no **son visibles** porque no **son accesibles** fuera del paquete.

La directiva `exports` en esencia nos **da** más niveles de control de acceso. La Tabla 12.3 **enumera** todas las opciones de control de acceso.

**TABLA 12.3** Control de acceso con módulos

| **Nivel** | **Dentro del código del módulo** | **Fuera del módulo** |
|---|---|---|
| `private` | Disponible solo dentro de la clase | Sin acceso |
| `package` | Disponible solo dentro del paquete | Sin acceso |
| `protected` | Disponible solo dentro del paquete o para subclases | Accesible para subclases solo si el paquete está exportado |
| `public` | Disponible para todas las clases | Accesible solo si el paquete está exportado |

### Requiriendo un módulo de forma transitiva

Como **se vio** anteriormente en este capítulo, `requires moduleName` **especifica** que el módulo actual depende de `moduleName`. También existe `requires transitive moduleName`, lo que **significa** que cualquier módulo que requiera este módulo también dependerá de `moduleName`.

Bueno, eso fue bastante de digerir. **Se observa** un ejemplo. La siguiente imagen muestra los módulos con líneas discontinuas para las relaciones redundantes y líneas sólidas para las relaciones especificadas en el `module-info.java`. Esto **muestra** cómo **se verían** las relaciones entre módulos si únicamente **se usaran** dependencias transitivas.

![[Nuestra versión de módulos con dependencia transitiva.jpg]]

Por ejemplo, `zoo.animal.talks` depende de `zoo.animal.care`, que depende de `zoo.animal.feeding`. Eso **significa** que la flecha entre `zoo.animal.talks` y `zoo.animal.feeding` ya no **aparece** en la imagen.

Ahora **se observan** las cuatro declaraciones de módulo. El primer módulo permanece sin cambios. **Se exporta** un paquete a cualquier paquete que use el módulo.

```Java
module zoo.animal.feeding {
   exports zoo.animal.feeding;
}
```

El módulo `zoo.animal.care` es la primera oportunidad de mejorar las cosas. En lugar de obligar a todos los módulos restantes a especificar explícitamente `zoo.animal.feeding`, el código usa `requires transitive`.

```Java
module zoo.animal.care {
   exports zoo.animal.care.medical;
   requires transitive zoo.animal.feeding;
}
```

En el módulo `zoo.animal.talks`, **se hace** un cambio similar y no **se obliga** a otros módulos a especificar `zoo.animal.care`. Tampoco **se necesita** ya especificar `zoo.animal.feeding`, por lo que esa línea **se comenta**.

```Java
module zoo.animal.talks {
	exports zoo.animal.talks.content to zoo.staff;
	exports zoo.animal.talks.media;
	exports zoo.animal.talks.schedule;
	// ya no se necesita requires zoo.animal.feeding;
	// ya no se necesita requires zoo.animal.care;
	requires transitive zoo.animal.care;
}
```

Finalmente, en el módulo `zoo.staff`, **se pueden** eliminar dos sentencias `requires`.

```Java
module zoo.staff {
   // ya no se necesita requires zoo.animal.feeding;
   // ya no se necesita requires zoo.animal.care;
   requires zoo.animal.talks;
}
```

Cuantos más módulos **haya**, mayores **serán** los beneficios de la construcción `requires transitive`. También **es** más conveniente para quien llama. Si **se estuviera** tratando de trabajar con este zoológico, **se podría** simplemente requerir `zoo.staff` y **tener** las dependencias restantes inferidas automáticamente.

#### Efectos de requires transitive

Dadas las nuevas declaraciones de módulo, y usando la imagen, ¿cuál es el efecto de aplicar el modificador `transitive` a la sentencia `requires` en nuestro módulo `zoo.animal.care`? Aplicar el modificador `transitive` tiene los siguientes efectos:

- El módulo `zoo.animal.talks` **puede** declarar opcionalmente que requiere el módulo `zoo.animal.feeding`, pero no **es obligatorio**.
- El módulo `zoo.animal.care` no **puede** compilarse ni ejecutarse sin acceso al módulo `zoo.animal.feeding`.
- El módulo `zoo.animal.talks` no **puede** compilarse ni ejecutarse sin acceso al módulo `zoo.animal.care`.

Estas reglas **se mantienen** incluso si los módulos `zoo.animal.care` y `zoo.animal.talks` no **referencian** explícitamente ningún paquete del módulo `zoo.animal.feeding`. Por otro lado, sin el modificador `transitive` en nuestra declaración de módulo de `zoo.animal.care`, los demás módulos **tendrían** que usar explícitamente `requires` para referenciar cualquier paquete del módulo `zoo.animal.feeding`.

### Sentencias `requires` duplicadas

Un lugar en el que el examen **podría** intentar confundir es mezclando `requires` y `requires transitive`. ¿**Se puede** pensar en una razón por la que este código no compile?

```Java
module bad.module {
   requires zoo.animal.talks;
   requires transitive zoo.animal.talks;
}
```

Java no **permite** repetir el mismo módulo en una cláusula `requires`. Es redundante y muy probablemente un error de codificación. **Hay que tener** en cuenta que `requires transitive` es como `requires` más algún comportamiento adicional.

### Abriendo un paquete

Java **permite** a quienes llaman inspeccionar y llamar código en tiempo de ejecución con una técnica llamada reflexión (_reflection_). Este es un enfoque poderoso que **permite** llamar código que puede que no **esté** disponible en tiempo de compilación. ¡Incluso **puede** usarse para subvertir el control de acceso! No **hay que preocuparse**: no **se necesita** saber cómo escribir código que use reflexión para el examen.

La directiva `opens` **se usa** para habilitar la reflexión de un paquete dentro de un módulo. Solo **hay que** ser consciente de que la directiva `opens` existe, en lugar de entenderla en detalle, para el examen.

Dado que la reflexión **puede** ser peligrosa, el sistema de módulos **requiere** que los desarrolladores permitan explícitamente la reflexión en la declaración de módulo si **quieren** que los módulos que llaman **tengan** permitido usarla. Lo siguiente **muestra** cómo habilitar la reflexión para dos paquetes en el módulo `zoo.animal.talks`:

```Java
module zoo.animal.talks {
   opens zoo.animal.talks.schedule;
   opens zoo.animal.talks.media to zoo.staff;
}
```

El primer ejemplo **permite** que cualquier módulo que use este **use** la reflexión. El segundo ejemplo **otorga** ese privilegio solo al módulo `zoo.staff`. Hay dos directivas más que **se necesitan** conocer para el examen —`provides` y `uses`—, las cuales **se cubren** en la siguiente sección.

> **Escenario del mundo real: Apertura de un módulo completo**
>
> En el ejemplo anterior, **se abrieron** dos paquetes en el módulo `zoo.animal.talks`, pero supongamos que en su lugar **se quisiera** abrir todos los paquetes para la reflexión. No hay problema. **Se puede** usar el modificador `open`, en lugar de la directiva `opens` (**hay que notar** la diferencia de la _s_):
>
> ```Java
> open module `zoo.animal.talks` {
> }
> ```
>
> Con este modificador de módulo, Java **sabe** que **se quiere** que todos los paquetes del módulo estén abiertos. ¿Qué **sucede** si **se aplican** ambos juntos?
>
> ```Java
> open module `zoo.animal.talks` {
>    opens `zoo.animal.talks.schedule`;           // NO COMPILA
> }
> ```
>
> Esto no compila porque a un módulo que usa el modificador `open` no **se le permite** usar la directiva `opens`. ¡Después de todo, los paquetes ya están abiertos!
