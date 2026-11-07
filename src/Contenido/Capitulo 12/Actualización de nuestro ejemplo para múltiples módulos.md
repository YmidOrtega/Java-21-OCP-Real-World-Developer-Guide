Ahora que el módulo `zoo.animal.feeding` está sólido, **se puede** empezar a pensar en los otros módulos. Como **se ve** en la siguiente imagen, los otros tres módulos del sistema dependen del módulo `zoo.animal.feeding`.

![[Módulos que dependen de zoo.animal.feeding.jpg]]

### Actualizando el módulo feeding

Dado que los otros módulos **llamarán** al código del paquete `zoo.animal.feeding`, **se necesita** declarar esta intención en la declaración de módulo.

La directiva `exports` **se usa** para indicar que un módulo pretende que esos paquetes **sean** utilizados por código Java de fuera del módulo. Como **se puede** imaginar, sin una directiva `exports`, el módulo solo está disponible para ejecutarse por sí mismo desde la línea de comandos. En el siguiente ejemplo, **se exporta** un paquete:

```Java
module zoo.animal.feeding {
   exports zoo.animal.feeding;
}
```

Recompilar y reempaquetar el módulo actualizará el archivo `module-info.class` dentro de nuestro archivo `zoo.animal.feeding.jar`. Estos son los mismos comandos `javac` y `jar` que **se ejecutaron** anteriormente:

```
javac -p mods
   -d feeding
   feeding/zoo/animal/feeding/*.java feeding/module-info.java
jar -cvf mods/zoo.animal.feeding.jar -C feeding/ .
```

### Creando un módulo care

A continuación, **se crea** el módulo `zoo.animal.care`. Esta vez, **se van a** tener dos paquetes. El paquete `zoo.animal.care.medical` tendrá las clases y los métodos destinados a ser usados por otros módulos. El paquete `zoo.animal.care.details` solo **se usará** por este módulo. No **se exportará** fuera del módulo. **Hay que** pensarlo como la privacidad de los historiales médicos de los animales.

La siguiente imagen muestra los contenidos de este módulo. **Hay que recordar** que todos los módulos deben tener un archivo `module-info.java`.

![[Contenido de zoo.animal.care.jpg]]

El módulo contiene dos paquetes básicos y clases además del archivo `module-info.java`:

```Java
// HippoBirthday.java
package zoo.animal.care.details;
import zoo.animal.feeding.*;
public class HippoBirthday {
   private Task task;
}

// Diet.java
package zoo.animal.care.medical;
public class Diet { }
```

Esta vez el archivo `module-info.java` especifica tres cosas:

```Java
1: module zoo.animal.care {
2:    exports zoo.animal.care.medical;
3:    requires zoo.animal.feeding;
4: }
```

La línea 1 especifica el nombre del módulo. La línea 2 lista el paquete que **se exporta** para que pueda ser usado por otros módulos. Hasta ahora, esto es similar al módulo `zoo.animal.feeding`.

En la línea 3, **se ve** una directiva nueva. La sentencia `requires` especifica que se necesita un módulo. El módulo `zoo.animal.care` depende del módulo `zoo.animal.feeding`.

A continuación, **hay que** determinar la estructura de directorios. **Se crean** dos paquetes. El primero es `zoo.animal.care.details` y contiene una clase llamada `HippoBirthday`. El segundo es `zoo.animal.care.medical`, que contiene una clase llamada `Diet`. **Hay que** intentar dibujar la estructura de directorios en papel o crearla en la computadora. Si **se están** ejecutando estos ejemplos sin usar el código en línea, basta con crear clases sin variables o métodos para todo excepto los archivos `module-info.java`.

**Puede que se haya notado** que los paquetes comienzan con el mismo prefijo que el nombre del módulo. Esto es intencional. **Se puede** pensarlo como si el nombre del módulo "reclamara" el paquete coincidente y todos sus subpaquetes.

Para repasar, ahora **se compila** y **se empaqueta** el módulo:

```
javac -p mods
   -d care
   care/zoo/animal/care/details/*.java
   care/zoo/animal/care/medical/*.java
   care/module-info.java
```

**Se compilan** ambos paquetes y el archivo `module-info.java`. En el mundo real, **se usará** una herramienta de compilación en lugar de hacer esto manualmente. Para el examen, simplemente **se listan** todos los paquetes y/o archivos que **se quieren** compilar.

Ahora que **se tiene** código compilado, es momento de crear el JAR del módulo:

```
jar -cvf mods/zoo.animal.care.jar -C care/ .
```

### Creando el módulo talks

Hasta ahora, **se ha usado** en un módulo solo una sentencia `exports` y una `requires`. Ahora **se aprenderá** cómo manejar la exportación de múltiples paquetes o la directiva `requires` con múltiples módulos. En la siguiente imagen, **se observa** que el módulo `zoo.animal.talks` depende de dos módulos: `zoo.animal.feeding` y `zoo.animal.care`. Esto significa que debe haber dos sentencias `requires` en el archivo `module-info.java`.

![[Dependencias de zoo.animal.talks.jpg]]

La siguiente imagen muestra los contenidos de este módulo. **Se van a** exportar los tres paquetes de este módulo.

![[Contenido de zoo.animal.talks.jpg]]

Primero, **se examina** el archivo `module-info.java` de `zoo.animal.talks`:

```Java
1: module zoo.animal.talks {
2:    exports zoo.animal.talks.content;
3:    exports zoo.animal.talks.media;
4:    exports zoo.animal.talks.schedule;
5:
6:    requires zoo.animal.feeding;
7:    requires zoo.animal.care;
8: }
```

La línea 1 muestra el nombre del módulo. Las líneas 2–4 permiten que otros módulos referencien los tres paquetes. Las líneas 6 y 7 especifican los dos módulos de los que este módulo depende.

Luego están las seis clases, como **se muestra** aquí:

```Java
// ElephantScript.java
package zoo.animal.talks.content;
public class ElephantScript { }

// SeaLionScript.java
package zoo.animal.talks.content;
public class SeaLionScript { }

// Announcement.java
package zoo.animal.talks.media;
public class Announcement {
   public static void main(String[] args) {
      System.out.println("We will be having talks");
   }
}

// Signage.java
package zoo.animal.talks.media;
public class Signage { }

// Weekday.java
package zoo.animal.talks.schedule;
public class Weekday { }

// Weekend.java
package zoo.animal.talks.schedule;
public class Weekend {}
```

Si **se está** siguiendo el ejemplo en la computadora, **hay que** crear estas clases en los paquetes. Los siguientes son los comandos para compilar y construir el módulo:

```
javac -p mods
   -d talks
   talks/zoo/animal/talks/content/*.java
   talks/zoo/animal/talks/media/*.java
   talks/zoo/animal/talks/schedule/*.java talks/module-info.java

jar -cvf mods/zoo.animal.talks.jar -C talks/ .
```

### Creando el módulo staff

El módulo final es `zoo.staff`. La siguiente imagen muestra que solo hay un paquete dentro. No **se expondrá** este paquete fuera del módulo.

![[Contenido de zoo.staff.jpg]]

Basándose en la imagen de dependencias de `zoo.staff`, ¿**se sabe** qué debería ir en el archivo `module-info.java`?

Hay tres flechas en la imagen apuntando desde `zoo.staff` hacia otros módulos. Estas representan los tres módulos que se requieren. Dado que no **se expondrá** ningún paquete desde `zoo.staff`, no hay sentencias `exports`. Esto nos da lo siguiente:

```Java
module zoo.staff {
   requires zoo.animal.feeding;
   requires zoo.animal.care;
   requires zoo.animal.talks;
}
```

La siguiente imagen muestra las dependencias de `zoo.staff`.

![[Dependencias de zoo.staff.jpg]]

En este módulo, **se tiene** una única clase en el archivo Jobs.java:

```Java
package zoo.staff;
public class Jobs { }
```

Para quienes **siguen** el ejemplo en la computadora, **hay que** crear una clase en el paquete. Los siguientes son los comandos para compilar y construir el módulo:

```
javac -p mods
   -d staff
   staff/zoo/staff/*.java staff/module-info.java

jar -cvf mods/zoo.staff.jar -C staff/.
```
