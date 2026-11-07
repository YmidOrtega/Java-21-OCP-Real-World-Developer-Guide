Las respuestas a las preguntas de revisión del capítulo se pueden encontrar en el Apéndice del libro.

**1.** ¿Qué afirmación es verdadera sobre el siguiente módulo?

```
|---zoo
   |-- staff
      |-- Vet.java
```

- [ ] A. La estructura de directorios mostrada es un módulo válido.
- [ ] B. La estructura de directorios sería un módulo válido si `module.java` **se agregara** directamente debajo de `zoo/staff`.
- [ ] C. La estructura de directorios sería un módulo válido si `module.java` **se agregara** directamente debajo de `zoo`.
- [ ] D. La estructura de directorios sería un módulo válido si `module-info.java` **se agregara** directamente debajo de `zoo/staff`.
- [ ] E. La estructura de directorios sería un módulo válido si `module-info.java` **se agregara** directamente debajo de `zoo`.
- [ ] F. Ninguno de estos cambios haría que esta estructura de directorios fuera un módulo válido.

---

**2.** Supóngase que el módulo `puppy` depende del módulo `dog` y el módulo `dog` depende del módulo `animal`. Complete el espacio en blanco para que el código del módulo `dog` pueda acceder al paquete `animal.behavior` del módulo `animal`.

```Java
module animal {
    _________ animal.behavior;
}
```

- [ ] A. `export`
- [ ] B. `exports`
- [ ] C. `require`
- [ ] D. `requires`
- [ ] E. `require transitive`
- [ ] F. `requires transitive`
- [ ] G. Ninguna de las anteriores.

---

**3.** Complete los espacios en blanco para que el siguiente comando para ejecutar el programa sea correcto:

```
java
_________ modules
_________ zoo.animal.talks/zoo/animal/talks/Peacocks
```

- [ ] A. `-d` y `-m`
- [ ] B. `-d` y `-p`
- [ ] C. `-m` y `-d`
- [ ] D. `-m` y `-p`
- [ ] E. `-p` y `-d`
- [ ] F. `-p` y `-m`
- [ ] G. Ninguna de las anteriores.

---

**4.** ¿Cuál de los siguientes pares forma un servicio?

- [ ] A. Consumidor y localizador de servicios
- [ ] B. Consumidor e interfaz del proveedor de servicio
- [ ] C. Localizador de servicios y proveedor de servicio
- [ ] D. Localizador de servicios e interfaz del proveedor de servicio
- [ ] E. Proveedor de servicio e interfaz del proveedor de servicio

---

**5.** Un módulo _____________ está en el classpath mientras que un módulo _____________ está en el module path. (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. automático, nombrado
- [ ] B. automático, sin nombre
- [ ] C. nombrado, automático
- [ ] D. nombrado, sin nombre
- [ ] E. sin nombre, automático
- [ ] F. sin nombre, nombrado
- [ ] G. Ninguna de las anteriores.

---

**6.** ¿Cuáles de las siguientes afirmaciones son verdaderas en un archivo `module-info.java`? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. La directiva `opens` permite el uso de reflexión.
- [ ] B. La directiva `opens` declara que una API es llamada.
- [ ] C. La directiva `use` permite el uso de reflexión.
- [ ] D. La directiva `use` declara que una API es llamada.
- [ ] E. La directiva `uses` permite el uso de reflexión.
- [ ] F. La directiva `uses` declara que una API es llamada.

---

**7.** Si no **se proporciona** un nombre de módulo automático, este **se genera**. ¿Cuáles de los siguientes pares de nombres de archivo JAR y nombre de módulo automático generado son correctos? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. `emily-1.0.0.jar` y `emily`
- [ ] B. `emily-1.0.0-SNAPSHOT.jar` y `emily`
- [ ] C. `emily_the_cat-1.0.0.jar` y `emily_the_cat`
- [ ] D. `emily_the_cat-1.0.0.jar` y `emily-the-cat`
- [ ] E. `emily.$.jar` y `emily`
- [ ] F. `emily.$.jar` y `emily.`
- [ ] G. `emily.$.jar` y `emily..`

---

**8.** ¿Cuáles de las siguientes afirmaciones son verdaderas? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. Los módulos con dependencias cíclicas no compilan.
- [ ] B. Los paquetes con una dependencia cíclica no compilan.
- [ ] C. Una dependencia cíclica siempre involucra exactamente dos módulos.
- [ ] D. Una dependencia cíclica siempre involucra al menos dos sentencias `requires`.
- [ ] E. Un módulo sin nombre puede verse envuelto en una dependencia cíclica con un módulo automático.

---

**9.** Supóngase que **se está creando** un proveedor de servicio que contiene la siguiente clase. ¿Qué línea de código debe incluirse en el archivo `module-info.java`?

```Java
package dragon;
import magic.*;
public class Dragon implements Magic {
   public String getPower() {
     return "breathe fire";
   }
}
```

- [ ] A. `provides dragon.Dragon by magic.Magic;`
- [ ] B. `provides dragon.Dragon using magic.Magic;`
- [ ] C. `provides dragon.Dragon with magic.Magic;`
- [ ] D. `provides magic.Magic by dragon.Dragon;`
- [ ] E. `provides magic.Magic using dragon.Dragon;`
- [ ] F. `provides magic.Magic with dragon.Dragon;`

---

**10.** ¿Cuál de las siguientes afirmaciones es verdadera sobre un módulo que contiene un archivo llamado `module-info.java` con el siguiente contenido? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
module com.food.supplier {}
```

- [ ] A. Todos los paquetes dentro del módulo **se exportan** automáticamente.
- [ ] B. Ningún paquete dentro del módulo **se exporta** automáticamente.
- [ ] C. Un método `main` dentro del módulo puede ejecutarse.
- [ ] D. Un método `main` dentro del módulo no puede ejecutarse ya que la clase no está expuesta.
- [ ] E. El archivo `module-info.java` contiene un error del compilador.
- [ ] F. El nombre del archivo `module-info.java` es incorrecto.

---

**11.** Supóngase que el módulo `puppy` depende del módulo `dog` y el módulo `dog` depende del módulo `animal`. ¿Qué líneas permiten que el módulo `puppy` acceda al paquete `animal.behavior` del módulo `animal`? (_Se deben seleccionar todas las opciones que apliquen_)

```Java
module animal {
   exports animal.behavior;
}
module dog {
    _____________ animal; // line S
}
module puppy {
      _____________ dog;         // line T
}
```

- [ ] A. `require` en la línea S
- [ ] B. `require` en la línea T
- [ ] C. `requires` en la línea S
- [ ] D. `requires` en la línea T
- [ ] E. `require transitive` en la línea S
- [ ] F. `require transitive` en la línea T
- [ ] G. `requires transitive` en la línea S
- [ ] H. `requires transitive` en la línea T

---

**12.** ¿Cuáles de los siguientes módulos **son proporcionados** por el JDK? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. `java.base`
- [ ] B. `java.desktop`
- [ ] C. `java.logging`
- [ ] D. `java.util`
- [ ] E. `jdk.base`
- [ ] F. `jdk.compiler`
- [ ] G. `jdk.xerces`

---

**13.** ¿Cuál de las siguientes opciones compila y es equivalente a este bucle?

```Java
List<Unicorn> all = new ArrayList<>();
for (Unicorn current : ServiceLoader.load(Unicorn.class))
   all.add(current);
```

- [ ] A. `List<Unicorn> all = ServiceLoader.load(Unicorn.class).getStream().toList();`
- [ ] B. `List<Unicorn> all = ServiceLoader.load(Unicorn.class).stream().toList();`
- [ ] C. `List<Unicorn> all = ServiceLoader.load(Unicorn.class).getStream().map(Provider::get).toList();`
- [ ] D. `List<Unicorn> all = ServiceLoader.load(Unicorn.class).stream().map(Provider::get).toList();`
- [ ] E. Ninguna de las anteriores.

---

**14.** ¿Cuál de los siguientes es un comando legal para ejecutar un programa modular en el que `n` es el nombre del módulo y `c` es el nombre completo de la clase?

- [ ] A. `java --module-path x -m n.c`
- [ ] B. `java --module-path x -p n.c`
- [ ] C. `java --module-path x-x -m n/c`
- [ ] D. `java --module-path x -p n/c`
- [ ] E. `java --module-path x-x -m n-c`
- [ ] F. `java --module-path x -p n-c`
- [ ] G. Ninguna de las anteriores.

---

**15.** Para una migración de arriba hacia abajo (_top-down migration_), todos los módulos distintos de los módulos nombrados son módulos _____________ y están en el _____________.

- [ ] A. automático, classpath
- [ ] B. automático, module path
- [ ] C. sin nombre, classpath
- [ ] D. sin nombre, module path
- [ ] E. Ninguna de las anteriores.

---

**16.** Supóngase que **se tienen** módulos separados para la interfaz del proveedor de servicio, el proveedor de servicio, el localizador de servicios y el consumidor. Si **se agrega** un segundo módulo proveedor de servicio, ¿cuántos de los módulos existentes **se deben** recompilar?

- [ ] A. Cero
- [ ] B. Uno
- [ ] C. Dos
- [ ] D. Tres
- [ ] E. Cuatro

---

**17.** Supóngase que **se tiene** un archivo JAR llamado `cat-1.2.3-RC1.jar` y que `Automatic-Module-Name` en el `MANIFEST.MF` está establecido en `dog`. ¿Qué debería incluir en el archivo `module-info.java` un módulo sin nombre que hace referencia a este módulo automático?

- [ ] A. `requires cat;`
- [ ] B. `requires cat.RC;`
- [ ] C. `requires cat-RC;`
- [ ] D. `requires dog;`
- [ ] E. Ninguna de las anteriores.

---

**18.** Dos comandos crean artefactos que incluyen versiones más pequeñas del JDK. ¿Cuáles se usan para crear un archivo `.exe` y un directorio, respectivamente?

- [ ] A. `jimage` y `jlink`
- [ ] B. `jimage` y `jpackage`
- [ ] C. `jlink` y `jimage`
- [ ] D. `jlink` y `jpackage`
- [ ] E. `jpackage` y `jimage`
- [ ] F. `jpackage` y `jlink`

---

**19.** ¿Cuál es una afirmación verdadera sobre el siguiente módulo?

```Java
class dragon {
   exports com.dragon.fire;
   exports com.dragon.scales to castle;
}
```

- [ ] A. Todos los módulos pueden hacer referencia al paquete `com.dragon.fire`.
- [ ] B. Todos los módulos pueden hacer referencia al paquete `com.dragon.scales`.
- [ ] C. Solo el módulo `castle` puede hacer referencia al paquete `com.dragon.fire`.
- [ ] D. Solo el módulo `castle` puede hacer referencia al paquete `com.dragon.scales`.
- [ ] E. Ninguna de las anteriores.

---

**20.** ¿Qué **se esperaría** ver al describir cualquier módulo?

- [ ] A. `requires java.base mandated`
- [ ] B. `requires java.core mandated`
- [ ] C. `requires java.lang mandated`
- [ ] D. `requires mandated java.base`
- [ ] E. `requires mandated java.core`
- [ ] F. `requires mandated java.lang`
- [ ] G. Ninguna de las anteriores.

---

**21.** Supóngase que **se tienen** módulos separados para la interfaz del proveedor de servicio, el proveedor de servicio, el localizador de servicios y el consumidor. ¿Qué módulo(s) debe(n) especificar una directiva `requires` sobre el módulo proveedor de servicio?

- [ ] A. El localizador de servicios
- [ ] B. La interfaz del proveedor de servicio
- [ ] C. El consumidor
- [ ] D. El consumidor y el localizador de servicios
- [ ] E. El consumidor y el proveedor de servicio
- [ ] F. El localizador de servicios y la interfaz del proveedor de servicio
- [ ] G. El consumidor, el localizador de servicios y la interfaz del proveedor de servicio
- [ ] H. Ninguna de las anteriores.

---

**22.** ¿Cuáles de las siguientes afirmaciones son verdaderas? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. Un módulo automático exporta todos los paquetes a los módulos nombrados.
- [ ] B. Un módulo automático exporta solo los paquetes especificados a los módulos nombrados.
- [ ] C. Un módulo automático no exporta paquetes a los módulos nombrados.
- [ ] D. Un módulo sin nombre exporta solo los paquetes nombrados a los módulos nombrados.
- [ ] E. Un módulo sin nombre exporta todos los paquetes a los módulos nombrados.
- [ ] F. Un módulo sin nombre no exporta paquetes a los módulos nombrados.

---

**23.** ¿En qué línea aparece el primer error del compilador?

```Java
1: module snake {
2:    exports com.snake.tail;
3:    exports com.snake.fangs to bird;
4:    requires skin;
5:    requires transitive skin;
6: }
```

- [ ] A. Línea 1.
- [ ] B. Línea 2.
- [ ] C. Línea 3.
- [ ] D. Línea 4.
- [ ] E. Línea 5.
- [ ] F. El código no contiene ningún error del compilador.

---

**24.** ¿Cuál es una afirmación verdadera sobre un paquete en un JAR en el classpath que contiene un archivo `module-info.java`?

- [ ] A. Es posible hacer que el paquete esté disponible para todos los demás módulos en el classpath.
- [ ] B. Es posible hacer que el paquete esté disponible para todos los demás módulos en el module path.
- [ ] C. Es posible hacer que el paquete esté disponible para exactamente un módulo específico más en el classpath.
- [ ] D. Es posible hacer que el paquete esté disponible para exactamente un módulo específico más en el module path.
- [ ] E. Es posible asegurarse de que el paquete no esté disponible para ningún otro módulo en el classpath.

---

**25.** Supóngase que **se tienen** módulos separados para la interfaz del proveedor de servicio, el proveedor de servicio, el localizador de servicios y el consumidor. ¿Cuáles de las siguientes afirmaciones son verdaderas sobre las directivas que **se deben** especificar? (_Se deben seleccionar todas las opciones que apliquen_)

- [ ] A. El consumidor debe usar la directiva `requires`.
- [ ] B. El consumidor debe usar la directiva `uses`.
- [ ] C. El localizador de servicios debe usar la directiva `requires`.
- [ ] D. El localizador de servicios debe usar la directiva `uses`.
- [ ] E. Ninguna de las anteriores.
