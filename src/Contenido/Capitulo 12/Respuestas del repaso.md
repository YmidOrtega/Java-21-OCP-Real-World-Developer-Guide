**1. E.** Los módulos **deben** tener un archivo `module-info.java` en el directorio raíz del módulo. La opción E cumple este requisito.

**2. B.** Las opciones A, C y E son incorrectas porque hacen referencia a directivas que no existen. La directiva `exports` **se usa** para permitir que un paquete sea llamado por código fuera del módulo, haciendo que la opción B sea la respuesta correcta. Hay que notar que las opciones D y F son incorrectas porque `requires` es la palabra clave equivocada.

**3. G.** La opción `-p` o `--module-path` **se usa** para especificar la ubicación de los módulos. La opción `-m` o `--module` **se usa** para especificar el módulo y el nombre de la clase. Sin embargo, para ejecutar un programa **se debe** especificar el nombre del paquete con puntos (`.`) en lugar de barras. Dado que el comando es incorrecto, la opción G es correcta.

**4. D.** Un servicio está formado por la interfaz del proveedor de servicios (_service provider interface_) y la lógica para buscar implementaciones mediante un localizador de servicios (_service locator_). Esto hace que la opción D sea correcta. Hay que tener claro que el proveedor de servicios en sí es la implementación, la cual **no se considera** parte del servicio.

**5. E, F.** Los módulos automáticos están en el _module path_ pero no tienen un archivo `module-info.java`. Los módulos con nombre (_named modules_) están en el _module path_ y sí tienen un `module-info.java`. Los módulos sin nombre (_unnamed modules_) están en el _classpath_. Por lo tanto, las opciones E y F son correctas.

**6. A, F.** Las opciones C y D son incorrectas porque no existe la directiva `use`. Las opciones A y F son correctas porque `opens` es para reflexión y `uses` declara que una API consume un servicio.

**7. A, B, E.** Cualquier información de versión al final del nombre del archivo JAR **se elimina**, haciendo que las opciones A y B sean correctas. Los guiones bajos (`_`) **se convierten** en puntos (`.`), haciendo que las opciones C y D sean incorrectas. Otros caracteres especiales, como el signo de dólar (`$`), también **se convierten** en puntos. Sin embargo, los puntos adyacentes **se fusionan** y los puntos al inicio o al final **se eliminan**. Por lo tanto, la opción E también es correcta.

**8. A, D.** Una dependencia cíclica ocurre cuando un grafo de módulos forma un círculo. La opción A es correcta porque el Java Platform Module System no permite dependencias cíclicas entre módulos. No existe tal restricción para los paquetes, haciendo que la opción B sea incorrecta. Una dependencia cíclica puede involucrar dos o más módulos que se requieren entre sí, haciendo que la opción D sea correcta, mientras que la opción C es incorrecta. Por último, la opción E es incorrecta porque los módulos sin nombre no **se pueden** referenciar desde un módulo automático.

**9. F.** La directiva `provides` toma primero el nombre de la interfaz y en segundo lugar el nombre de la clase que la implementa, y además usa `with`. Solo la opción F cumple estos dos criterios, haciendo que sea la respuesta correcta.

**10. B, C.** Los paquetes dentro de un módulo no **se exportan** por defecto, haciendo que la opción B sea correcta y la opción A incorrecta. Exportar es necesario para que otro código use los paquetes; no es necesario para llamar al método `main()` desde la línea de comandos, haciendo que la opción C sea correcta y la opción D incorrecta. El archivo `module-info.java` tiene el nombre correcto y compila, haciendo que las opciones E y F sean incorrectas.

**11. D, G, H.** Las opciones A, B, E y F son incorrectas porque hacen referencia a directivas que no existen. La directiva `requires transitive` **se usa** para especificar un módulo que será utilizado por el módulo que lo solicita y por cualquier otro módulo que use al módulo solicitante. Por lo tanto, `dog` necesita especificar la relación transitiva, y la opción G es correcta. El módulo `puppy` solo necesita `requires dog` y obtiene las dependencias transitivas, haciendo que la opción D sea correcta. Sin embargo, `requires transitive` hace todo lo que hace `requires` y más, lo que convierte a la opción H en la última respuesta correcta.

**12. A, B, C, F.** La opción D es incorrecta porque es un nombre de paquete en lugar de un nombre de módulo. La opción E es incorrecta porque `java.base` es el nombre del módulo, no `jdk.base`. La opción G es incorrecta porque es inventada. Las opciones A, B, C y F son correctas.

**13. D.** No existe un método `getStream()` en un `ServiceLoader`, haciendo que las opciones A y C sean incorrectas. La opción B no compila porque el método `stream()` devuelve una lista de interfaces `Provider` que **se deben** convertir a la interfaz `Unicorn` que nos interesa. Por lo tanto, la opción D es correcta.

**14. C.** La opción `-p` es la forma abreviada de `--module-path`. Dado que la misma opción no **se puede** especificar dos veces, las opciones B, D y F son incorrectas. El nombre del módulo y el nombre de la clase **se separan** con una barra, haciendo que la opción C sea la respuesta. Hay que notar que `x-x` es válido porque el _module path_ es el nombre de una carpeta, por lo que **se permiten** guiones.

**15. B.** Una estrategia de migración de arriba hacia abajo (_top-down_) coloca primero todos los JAR en el _module path_. Luego migra el módulo de nivel superior para que sea un módulo con nombre, dejando los demás como módulos automáticos. La opción B es correcta porque cumple ambas características.

**16. A.** Dado que es un módulo nuevo, **hay que** compilarlo. Sin embargo, ninguno de los módulos existentes necesita recompilarse, haciendo que la opción A sea correcta. El localizador de servicios verá el nuevo proveedor de servicios simplemente por tener ese nuevo proveedor en el _module path_.

**17. E.** ¡Pregunta trampa! Un módulo sin nombre no usa un archivo `module-info.java`. Por lo tanto, la opción E es correcta. Un módulo sin nombre puede acceder a un módulo automático. El módulo sin nombre simplemente trataría al módulo automático como un JAR normal, sin involucrar el archivo `module-info.java`.

**18. F.** El comando `jpackage` crea aplicaciones autocontenidas, como un `.exe`. El comando `jlink` crea un directorio con un _runtime_ de Java más pequeño que contiene solo lo necesario. El comando `jimage` **se usa** para inspeccionar un archivo de imagen de Java. Por lo tanto, la opción F es correcta.

**19. E.** Aquí hay una trampa. Una definición de módulo usa la palabra clave `module` en lugar de `class`. Dado que el código no compila, la opción E es correcta. Si el código compilara, las opciones A y D serían correctas.

**20. A.** Al ejecutar `java` con la opción `-d`, **se listan** todos los módulos requeridos. Además, **se lista** el módulo `java.base`, ya que **se incluye** automáticamente. La línea termina con `mandated`, haciendo que la opción A sea correcta. `java.lang` es una trampa, ya que es un paquete que **se importa** por defecto en una clase, no un módulo.

**21. H.** Esta pregunta es engañosa. El localizador de servicios debe tener una directiva `uses`, pero esta va sobre la interfaz del proveedor de servicios. Ningún módulo necesita especificar `requires` sobre el proveedor de servicios, ya que este es la implementación. Dado que ninguna de las opciones es correcta, la opción H es la respuesta.

**22. A, F.** Un módulo automático exporta todos los paquetes, haciendo que la opción A sea correcta. Un módulo sin nombre no está disponible para ningún módulo del _module path_. Por lo tanto, no exporta ningún paquete, y la opción F es correcta.

**23. E.** El nombre del módulo es válido, al igual que las sentencias `exports`. Las líneas 4 y 5 son engañosas porque cada una es válida por separado. Sin embargo, no **se permite** usar el mismo nombre de módulo en dos sentencias `requires`. La segunda falla al compilar en la línea 5, haciendo que la opción E sea la respuesta.

**24. A.** Dado que el JAR está en el _classpath_, **se trata** como un módulo sin nombre normal, aunque tenga un archivo `module-info.java` dentro. Hay que recordar, de la migración de arriba hacia abajo, que los módulos del _module path_ no pueden hacer referencia al _classpath_, haciendo que las opciones B y D sean incorrectas. El _classpath_ no tiene ningún mecanismo para restringir paquetes, haciendo que la opción A sea correcta y las opciones C y E incorrectas.

**25. A, C, D.** Las opciones A y C son correctas porque tanto el consumidor como el localizador de servicios dependen de la interfaz del proveedor de servicios. Además, la opción D es correcta porque el localizador de servicios debe especificar que usa (`uses`) la interfaz del proveedor de servicios para poder buscarlo.
