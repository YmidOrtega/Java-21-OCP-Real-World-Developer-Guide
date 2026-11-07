En esta sección, **se aprende** a crear un servicio. Un servicio está compuesto por una interfaz, cualquier clase que la interfaz referencie, y una forma de buscar las implementaciones de la interfaz. Las implementaciones no forman parte del servicio.

En la sección de servicios **se usa** una aplicación de tours (_tour application_). Tiene cuatro módulos que **se muestran** en la siguiente imagen. En este ejemplo, los módulos `zoo.tours.api` y `zoo.tours.reservations` componen el servicio, ya que consisten en la interfaz y la funcionalidad de búsqueda.

![[Módulos en la aplicación tour.jpg]]

> **Nota:** No **se requiere** tener cuatro módulos separados. **Se hace** para ilustrar los conceptos. Por ejemplo, la interfaz del proveedor de servicio y el localizador de servicios podrían estar en el mismo módulo.

### Declarando la interfaz del proveedor de servicio

Primero, el módulo `zoo.tours.api` define un objeto Java llamado `Souvenir`. Se considera parte del servicio porque será referenciado por la interfaz.

```Java
// Souvenir.java
package zoo.tours.api;

public record Souvenir(String description) { }
```

A continuación, el módulo contiene un tipo de interfaz Java. Esta interfaz **se llama** la interfaz del proveedor de servicio (_service provider interface_) porque especifica qué comportamiento tendrá el servicio. En este caso, es una API simple con tres métodos.

```Java
// Tour.java
package zoo.tours.api;

public interface Tour {
   String name();
   int length();
   Souvenir getSouvenir();
}
```

Los tres métodos usan el modificador `public` implícito. Dado que **se trabaja** con módulos, también **se necesita** crear un archivo `module-info.java` para que la definición del módulo exporte el paquete que contiene la interfaz.

```Java
// module-info.java
module zoo.tours.api {
   exports zoo.tours.api;
}
```

Ahora que **se tienen** ambos archivos, **se puede** compilar y empaquetar este módulo.

```
javac -d serviceProviderInterfaceModule
   serviceProviderInterfaceModule/zoo/tours/api/*.java
   serviceProviderInterfaceModule/module-info.java

jar -cvf mods/zoo.tours.api.jar -C serviceProviderInterfaceModule/ .
```

> **Nota:** La "interfaz" del proveedor de servicio **puede** ser una clase abstracta en lugar de una interfaz real. Dado que **se verá** únicamente como una interfaz en el examen, **se usa** ese término en el proyecto.

Para revisar, el servicio incluye la interfaz del proveedor de servicio y las clases de soporte que referencia. El servicio también incluye la funcionalidad de búsqueda, que **se define** a continuación.

### Creando un localizador de servicios

Para completar el servicio, **se necesita** un localizador de servicios (_service locator_). Un localizador de servicios **puede encontrar** cualquier clase que implemente una interfaz del proveedor de servicio.

Afortunadamente, Java proporciona una clase `ServiceLoader` para ayudar con esta tarea. **Se pasa** el tipo de la interfaz del proveedor de servicio a su método `load()`, y Java devolverá cualquier servicio de implementación que pueda encontrar. La siguiente clase lo muestra en acción:

```Java
// TourFinder.java
package zoo.tours.reservations;

import java.util.*;
import zoo.tours.api.*;

public class TourFinder {

   public static Tour findSingleTour() {
      ServiceLoader<Tour> loader = ServiceLoader.load(Tour.class);
      for (Tour tour : loader)
         return tour;
      return null;
   }
   public static List<Tour> findAllTours() {
      List<Tour> tours = new ArrayList<>();
      ServiceLoader<Tour> loader = ServiceLoader.load(Tour.class);
      for (Tour tour : loader)
         tours.add(tour);
      return tours;
   }
}
```

Como **se puede** ver, **se proporcionaron** dos métodos de búsqueda. El primero es un método de conveniencia para cuando **se espera** que se devuelva exactamente un solo `Tour`. El otro devuelve un `List`, que acomoda cualquier número de proveedores de servicio. En tiempo de ejecución, el localizador de servicios **puede encontrar** muchos proveedores de servicio (o ninguno).

> **Nota:** La llamada a `ServiceLoader` es relativamente costosa. Si **se está** escribiendo una aplicación real, lo mejor **es almacenar** el resultado en caché.

La definición del módulo exporta el paquete con la clase de búsqueda `TourFinder`. Requiere el paquete de la interfaz del proveedor de servicio. También tiene la directiva `uses` ya que buscará un servicio.

```Java
// module-info.java
module zoo.tours.reservations {
   exports zoo.tours.reservations;
   requires zoo.tours.api;
   uses zoo.tours.api.Tour;
}
```

**Hay que recordar** que tanto `requires` como `uses` son necesarios, uno para la compilación y otro para la búsqueda. Finalmente, **se compila** y **se empaqueta** el módulo.

```
javac -p mods -d serviceLocatorModule
   serviceLocatorModule/zoo/tours/reservations/*.java
   serviceLocatorModule/module-info.java

jar -cvf mods/zoo.tours.reservations.jar -C serviceLocatorModule/ .
```

Ahora que **se tiene** la interfaz y la lógica de búsqueda, **se ha completado** el servicio.

> **Usando `ServiceLoader`**
>
> Hay dos métodos en `ServiceLoader` que **hay que conocer** para el examen. La declaración es la siguiente, sin la implementación completa:
>
> ```Java
> public final class ServiceLoader<S> implements Iterable<S> {
>
>    public static <S> ServiceLoader<S> load(Class<S> service) { … }
>
>    public Stream<Provider<S>> stream() { … }
>
>    // Additional methods
> }
> ```
>
> Como ya **se vio**, llamar a `ServiceLoader.load()` devuelve un objeto sobre el que **se puede** iterar normalmente. Sin embargo, solicitar un `Stream` devuelve un tipo diferente. La razón es que un `Stream` controla cuándo **se evalúan** los elementos. Por lo tanto, un `ServiceLoader` devuelve un stream de objetos `Provider`. **Hay que** llamar a `get()` para recuperar el valor que **se quiere** de cada `Provider`, como en este ejemplo:
>
> ```Java
> ServiceLoader.load(Tour.class)
>   .stream()
>   .map(Provider::get)
>   .mapToInt(Tour::length)
>   .max()
>   .ifPresent(System.out::println);
> ```

### Invocando desde un consumidor

A continuación, **se llama** al localizador de servicios desde un consumidor. Un consumidor (_consumer_, o cliente) se refiere a un módulo que obtiene y usa un servicio. Una vez que el consumidor ha adquirido un servicio a través del localizador de servicios, es capaz de invocar los métodos proporcionados por la interfaz del proveedor de servicio.

```Java
// Tourist.java
package zoo.visitor;

import java.util.*;
import zoo.tours.api.*;
import zoo.tours.reservations.*;

public class Tourist {
   public static void main(String[] args) {
      Tour tour = TourFinder.findSingleTour();
      System.out.println("Single tour: " + tour);

      List<Tour> tours = TourFinder.findAllTours();
      System.out.println("# tours: " + tours.size());
   }
}
```

La definición del módulo no necesita saber nada sobre las implementaciones, ya que el módulo `zoo.tours.reservations` se encarga de la búsqueda.

```Java
// module-info.java
module zoo.visitor {
   requires zoo.tours.api;
   requires zoo.tours.reservations;
}
```

Esta vez, **se puede** ejecutar un programa después de compilar y empaquetar.

```
javac -p mods -d consumerModule
   consumerModule/zoo/visitor/*.java consumerModule/module-info.java

jar -cvf mods/zoo.visitor.jar -C consumerModule/ .

java -p mods -m zoo.visitor/zoo.visitor.Tourist
```

El programa produce la salida siguiente:

```
Single tour: null
# tours: 0
```

Bueno, esto tiene sentido. **Todavía no se ha escrito** una clase que implemente la interfaz.

### Agregando un proveedor de servicio

Un proveedor de servicio (_service provider_) es la implementación de una interfaz del proveedor de servicio. Como **se dijo** anteriormente, en tiempo de ejecución es posible tener múltiples clases o módulos de implementación. Aquí **se usará** solo uno por simplicidad.

Nuestro proveedor de servicio es el paquete `zoo.tours.agency` porque **se ha externalizado** la operación de los tours a un tercero.

```Java
// TourImpl.java
package zoo.tours.agency;

import zoo.tours.api.*;

public class TourImpl implements Tour {
   public String name() {
      return "Behind the Scenes";
   }
   public int length() {
      return 120;
   }
   public Souvenir getSouvenir() {
      return new Souvenir("stuffed animal");
   }
}
```

Nuevamente, **se necesita** un archivo `module-info.java` para crear un módulo.

```Java
// module-info.java
module zoo.tours.agency {
   requires zoo.tours.api;
   provides zoo.tours.api.Tour with zoo.tours.agency.TourImpl;
}
```

La declaración de módulo requiere el módulo que contiene la interfaz como dependencia. No **se exporta** el paquete que implementa la interfaz porque no **se quiere** que los llamadores se refieran a él directamente. En su lugar, **se usa** la directiva `provides`. Esto permite especificar que se proporciona una implementación de la interfaz con una clase de implementación específica. La sintaxis es la siguiente:

```Java
provides interfaceName with className;
```

> **Nota:** No **se ha exportado** el paquete que contiene la implementación. En su lugar, **se ha hecho** la implementación disponible para un proveedor de servicio usando la interfaz.

Finalmente, **se compila** y **se empaqueta**.

```
javac -p mods -d serviceProviderModule
   serviceProviderModule/zoo/tours/agency/*.java
   serviceProviderModule/module-info.java

jar -cvf mods/zoo.tours.agency.jar -C serviceProviderModule/ .
```

Ahora viene la parte genial. **Se puede** ejecutar el programa Java nuevamente.

```
java -p mods -m zoo.visitor/zoo.visitor.Tourist
```

Esta vez, **se ve** una salida como la siguiente:

```
Single tour: zoo.tours.agency.TourImpl@1936f0f5
# tours: 1
```

**Hay que notar** cómo no **se volvió a compilar** el paquete `zoo.tours.reservations` ni el paquete `zoo.visitor`. El localizador de servicios **fue capaz de observar** que ahora había una implementación de proveedor de servicio disponible y **la encontró**. Esto es útil cuando **se tiene** funcionalidad que cambia independientemente del resto de la base de código. Por ejemplo, **se podrían** tener reportes personalizados o _logging_.

> **Nota:** En el desarrollo de software, el concepto de separar distintos componentes en piezas independientes se conoce como acoplamiento laxo (_loose coupling_). Una ventaja del código con acoplamiento laxo es que puede intercambiarse o reemplazarse fácilmente con cambios mínimos (o nulos) en el código que lo usa. Confiar en una estructura con acoplamiento laxo permite que los módulos de servicio sean fácilmente extensibles en tiempo de ejecución.

### Revisando directivas y servicios

La Tabla 12.4 resume lo que **se ha cubierto** en la sección sobre servicios. **Se recomienda** aprender muy bien qué se necesita cuando cada artefacto está en un módulo separado. Eso es lo más probable que **se vea** en el examen y **asegurará** que **se entiendan** los conceptos. La Tabla 12.5 lista todas las directivas que **hay que conocer** para el examen.

**TABLA 12.4** Revisión de servicios

| **Artefacto**                      | **¿Parte del servicio?** | **Directivas requeridas**     |
| ---------------------------------- | ------------------------ | ----------------------------- |
| Interfaz del proveedor de servicio | Sí                       | `exports`                     |
| Proveedor de servicio              | No                       | `requires`, `provides`        |
| Localizador de servicios           | Sí                       | `exports`, `requires`, `uses` |
| Consumidor                         | No                       | `requires`                    |

**TABLA 12.5** Revisión de directivas

| **Directiva** | **Descripción** |
|---|---|
| `exports package;`<br>`exports package to module;` | Hace el paquete disponible fuera del módulo |
| `requires module;`<br>`requires transitive module;` | Especifica otro módulo como dependencia |
| `opens package;`<br>`opens package to module;` | Permite que el paquete se use con reflexión |
| `provides serviceInterface with implName;` | Hace el servicio disponible |
| `uses serviceInterface;` | Referencia el servicio |
