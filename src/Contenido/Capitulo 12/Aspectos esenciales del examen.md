**Crear archivos `module-info.java`.** El archivo `module-info.java` **se coloca** en el directorio raíz del módulo. **Hay que** saber codificar las directivas `exports`, `requires`, `provides` y `uses`. Además, **se debe** estar familiarizado con la directiva `opens`.

**Usar operaciones de línea de comandos con módulos.** El comando `java` **puede describir** un módulo, **listar** los módulos disponibles o **mostrar** la resolución de módulos. El comando `jar` **puede describir** un módulo de forma similar a como lo hace el comando `java`. El comando `jdeps` **imprime** detalles sobre un módulo y sus paquetes. El comando `jmod` **ofrece** varios modos para trabajar con archivos JMOD en lugar de archivos JAR. El comando `jlink` **crea** imágenes de Java personalizadas. El comando `jpackage` **crea** un archivo ejecutable.

**Identificar los tres tipos de módulos.** Los módulos nombrados (_named modules_) son JARs que **se han** modularizado. Los módulos sin nombre (_unnamed modules_) no **se han** modularizado. Los módulos automáticos (_automatic modules_) están en medio: están en el module path pero no tienen un archivo `module-info.java`.

**Listar los módulos integrados del JDK.** El módulo `java.base` está disponible para todos los módulos. Hay unos 20 módulos más que proporciona el JDK que comienzan con `java.*` y unos 30 que comienzan con `jdk.*`.

**Explicar la migración de arriba abajo y de abajo arriba.** Una migración de arriba abajo (_top-down_) **coloca** todos los JARs en el module path, convirtiéndolos en módulos automáticos mientras se migra de arriba hacia abajo. Una migración de abajo arriba (_bottom-up_) **deja** todos los JARs en el classpath, convirtiéndolos en módulos sin nombre mientras se migra de abajo hacia arriba.

**Diferenciar las cuatro partes principales de un servicio.** La interfaz del proveedor de servicio (_service provider interface_) **declara** la interfaz que un servicio debe implementar. El localizador de servicios (_service locator_) **busca** el servicio y un consumidor (_consumer_) lo llama. Por último, el proveedor de servicio (_service provider_) **implementa** el servicio.
