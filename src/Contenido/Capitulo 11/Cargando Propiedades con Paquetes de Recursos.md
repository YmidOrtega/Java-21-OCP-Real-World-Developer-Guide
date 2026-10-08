Hasta ahora, **se han** mantenido todas las cadenas de texto mostradas a los usuarios como parte del programa dentro de las clases que las usan. La localización requiere externalizarlas a otro lugar.

Un _paquete de recursos_ (_resource bundle_) contiene los objetos específicos del local que **serán** usados por un programa. Es como un mapa con claves y valores. El paquete de recursos comúnmente **se almacena** en un archivo de propiedades. Un _archivo de propiedades_ (_properties file_) es un archivo de texto en un formato específico con pares clave/valor.

El programa del zoológico ha sido exitoso. Ahora **se reciben** peticiones para usarlo en tres zoológicos más. Ya **se tiene** soporte para zoológicos de EE.UU. Ahora **se necesita** añadir el Zoo de La Palmyre en Francia, el Greater Vancouver Zoo en el Canadá de habla inglesa, y el Zoo de Granby en el Canadá de habla francesa. Inmediatamente **se comprende** que **se va a** necesitar internacionalizar el programa. Los paquetes de recursos **serán** muy útiles: **permitirán** traducir fácilmente la aplicación a múltiples locales o incluso soportar varios locales a la vez. También **será** fácil añadir más locales en el futuro si zoológicos de aún más países **están** interesados. **Se pensó** en qué locales **se necesitan** soportar y **se llegaron** a estos cuatro:

```Java
Locale us           = Locale.of("en", "US");
Locale france       = Locale.of("fr", "FR");
Locale englishCanada = Locale.of("en", "CA");
Locale frenchCanada  = Locale.of("fr", "CA");
```

En las siguientes secciones, **se crea** un paquete de recursos usando archivos de propiedades. Es conceptualmente similar a un `Map<String, String>`, donde cada línea **representa** un par clave/valor diferente. La clave y el valor **se separan** con un signo igual (`=`) o dos puntos (`:`). Para mantener las cosas simples, **se usa** un signo igual en todo este capítulo. También **se ve** cómo Java determina qué paquete de recursos usar.

## Creando un Paquete de Recursos

**Se va a** actualizar la aplicación para soportar los cuatro locales listados anteriormente. Por suerte, Java no nos obliga a crear cuatro paquetes de recursos diferentes. Si no **se tiene** un paquete de recursos específico del país, Java usará uno específico del idioma. Es un poco más complejo que esto, pero **se empieza** con un ejemplo simple.

Por ahora, **se necesitan** archivos de propiedades en inglés y francés para el paquete de recursos `Zoo`. Primero, **se crean** dos archivos de propiedades:

```
Zoo_en.properties
hello=Hello
open=The zoo is open

Zoo_fr.properties
hello=Bonjour
open=Le zoo est ouvert
```

Los nombres de archivo **coinciden** con el nombre del paquete de recursos, `Zoo`. Luego van seguidos de un guión bajo (`_`), el local objetivo y la extensión `.properties`. **Se puede** escribir el primer programa que usa un paquete de recursos para imprimir esta información:

```Java
10: public static void printWelcomeMessage(Locale locale) {
11:     var rb = ResourceBundle.getBundle("Zoo", locale);
12:     System.out.println(rb.getString("hello")
13:         + ", " + rb.getString("open"));
14: }
15: public static void main(String[] args) {
16:     var us = Locale.of("en", "US");
17:     var france = Locale.of("fr", "FR");
18:     printWelcomeMessage(us);     // Hello, The zoo is open
19:     printWelcomeMessage(france); // Bonjour, Le zoo est ouvert
20: }
```

Las líneas 16 y 17 **crean** los dos locales que **se quieren** probar, pero el método en las líneas 10–14 hace el trabajo real. La línea 11 **llama** a un método de fábrica en `ResourceBundle` para obtener el paquete de recursos correcto. Las líneas 12 y 13 **recuperan** la cadena correcta del paquete de recursos e **imprimen** los resultados.

Dado que un paquete de recursos contiene pares clave/valor, incluso **se puede** iterar a través de ellos para listar todos los pares. La clase `ResourceBundle` proporciona un método `keySet()` para obtener un conjunto de todas las claves:

```Java
var us = Locale.of("en", "US");
ResourceBundle rb = ResourceBundle.getBundle("Zoo", us);
rb.keySet().stream()
    .map(k -> k + ": " + rb.getString(k))
    .forEach(System.out::println);
```

Este ejemplo **recorre** todas las claves e **imprime**:

```Java
hello: Hello
open: The zoo is open
```

> **Escenario del mundo real: Cargando Archivos de Paquete de Recursos en Tiempo de Ejecución**
>
> Para el examen, no **hay que** saber dónde **se almacenan** los archivos de propiedades para los paquetes de recursos. Si el examen proporciona un archivo de propiedades, **es seguro** asumir que existe y **se carga** en tiempo de ejecución.
>
> En las propias aplicaciones, sin embargo, los paquetes de recursos **pueden almacenarse** en una variedad de lugares. Aunque **pueden almacenarse** dentro del JAR que los usa, hacerlo no **es recomendado** porque obliga a **reconstruir** el JAR de la aplicación cada vez que algún texto cambia. Uno de los beneficios de usar paquetes de recursos es **desacoplar** el código de la aplicación de los datos de texto específicos del local. Otro enfoque es tener todos los archivos de propiedades en un JAR o carpeta separada y **cargarlos** en el classpath en tiempo de ejecución. De esta manera, **se puede** agregar un nuevo idioma sin cambiar el JAR de la aplicación.

## Eligiendo un Paquete de Recursos

Hay dos métodos para **obtener** un paquete de recursos con los que **hay que** estar familiarizado para el examen:

```Java
ResourceBundle.getBundle("name");
ResourceBundle.getBundle("name", locale);
```

El primero **usa** el local predeterminado. Es el que **se va** a usar probablemente en los programas que **se escriban**. O el examen **dice** qué asumir como local predeterminado, o **usa** el segundo enfoque.

Java **maneja** la lógica de elegir el mejor paquete de recursos disponible para una clave dada. **Intenta** encontrar el valor más específico. La Tabla 11.11 **muestra** lo que Java **recorre** cuando **se le pide** el paquete de recursos `Zoo` con el local `Locale.of("fr", "FR")` cuando el local predeterminado es inglés de EE.UU.

**TABLA 11.11** Eligiendo un paquete de recursos para francés/Francia con local predeterminado inglés/EE.UU.

| **Paso** | **Busca archivo** | **Razón** |
|---|---|---|
| 1 | `Zoo_fr_FR.properties` | Local solicitado |
| 2 | `Zoo_fr.properties` | Idioma solicitado sin país |
| 3 | `Zoo_en_US.properties` | Local predeterminado |
| 4 | `Zoo_en.properties` | Idioma del local predeterminado sin país |
| 5 | `Zoo.properties` | Sin local — paquete predeterminado |
| 6 | Si aún no **se encuentra**, lanzar `MissingResourceException` | No hay local ni paquete predeterminado disponible |

Como otra forma de recordar el orden de la Tabla 11.11, **hay que** aprender estos pasos:

1. **Buscar** el paquete de recursos para el local solicitado, seguido por el del local predeterminado.
2. Para cada local, **verificar** el idioma/país, seguido solo del idioma.
3. **Usar** el paquete de recursos predeterminado si no **se puede** encontrar un local coincidente.

> **Nota:** Java soporta paquetes de recursos de clases Java y archivos de propiedades por igual. Cuando Java **está** buscando un paquete de recursos coincidente, primero **verificará** si hay un archivo de paquete de recursos con el nombre de clase coincidente. Para el examen, solo **hay que** saber cómo trabajar con archivos de propiedades.

**Veamos** si **se entiende** la Tabla 11.11. ¿Cuál es el número máximo de archivos que Java necesitaría **considerar** para encontrar el paquete de recursos apropiado con el siguiente código?

```Java
Locale.setDefault(Locale.of("hi"));
ResourceBundle rb = ResourceBundle.getBundle("Zoo", Locale.of("en"));
```

La respuesta es tres. **Se listan** aquí:

1. `Zoo_en.properties`
2. `Zoo_hi.properties`
3. `Zoo.properties`

El local solicitado es `en`, así que **se empieza** con ese. Dado que el local `en` no contiene un país, **se pasa** al local predeterminado, `hi`. De nuevo, no hay país, así que **se termina** con el paquete predeterminado.

## Seleccionando Valores del Paquete de Recursos

¿Todo claro? Bien, porque hay un giro. Los pasos que **se han** discutido hasta ahora son para encontrar el paquete de recursos coincidente a usar como base. Java no **está** obligado a obtener todas las claves del mismo paquete de recursos. Puede **obtenerlas** de _cualquier padre del paquete de recursos coincidente_. Un paquete de recursos padre en la jerarquía simplemente **elimina** componentes del nombre hasta **llegar** a la cima. La Tabla 11.12 **muestra** cómo hacerlo.

**TABLA 11.12** Seleccionando propiedades del paquete de recursos

| **Paquete de recursos coincidente** | **Las claves de los archivos de propiedades pueden venir de** |
|---|---|
| `Zoo_fr_FR` | `Zoo_fr_FR.properties`, `Zoo_fr.properties`, `Zoo.properties` |

Una vez que **se ha** seleccionado un paquete de recursos, _solo **se usarán** propiedades a lo largo de una sola jerarquía_. Esto **contrasta** con la Tabla 11.11, en la que **se usa** el paquete de recursos predeterminado `en_US` si no hay otros paquetes de recursos disponibles.

¿Qué significa esto exactamente? **Supongamos** que el local solicitado es `fr_FR` y el predeterminado es `en_US`. La JVM **proporcionará** datos de `en_US` _solo si no hay un paquete de recursos `fr_FR` o `fr` coincidente_. Si **encuentra** un paquete de recursos `fr_FR` o `fr`, entonces solo esos paquetes, junto con el paquete predeterminado, **se usarán**.

**Pongamos** todo esto junto e **imprimamos** algo de información sobre nuestros zoológicos. **Tenemos** varios archivos de propiedades esta vez:

```
Zoo.properties
name=Vancouver Zoo

Zoo_en.properties
hello=Hello
open=is open

Zoo_en_US.properties
name=The Zoo

Zoo_en_CA.properties
visitors=Canada visitors
```

**Supongamos** que **tenemos** un visitante de Québec (que tiene un local predeterminado de canadiense francés) que **ha pedido** al programa que proporcione información en inglés. ¿Qué **imprime** esto?

```Java
10: Locale.setDefault(Locale.of("en", "US"));
11: var locale = Locale.of("en", "CA");
12: ResourceBundle rb = ResourceBundle.getBundle("Zoo", locale);
13:
14: System.out.print(rb.getString("hello"));
15: System.out.print(". ");
16: System.out.print(rb.getString("name"));
17: System.out.print(" ");
18: System.out.print(rb.getString("open"));
19: System.out.print(" ");
20: System.out.print(rb.getString("visitors"));
```

El programa **imprime** lo siguiente:

```Java
Hello. Vancouver Zoo is open Canada visitors
```

El local predeterminado es `en_US`, y el local solicitado es `en_CA`. Primero, Java **recorre** los paquetes de recursos disponibles para **encontrar** una coincidencia. **Encuentra** una de inmediato con `Zoo_en_CA.properties`. Esto significa que el local predeterminado de `en_US` es irrelevante.

Después de la línea 12, el paquete de recursos **se selecciona**, y Java solo **considerará** los archivos que sean parte de este paquete de recursos, a saber `Zoo_en_CA.properties`, `Zoo_en.properties` y `Zoo.properties`, en este orden.

La línea 14 no **encuentra** una coincidencia para `hello` en `Zoo_en_CA.properties`, por lo que **sube** en la jerarquía a `Zoo_en.properties`. La línea 16 no **encuentra** una coincidencia para `name` en ninguno de los dos primeros archivos, por lo que **tiene** que **subir** hasta la cima a `Zoo.properties`. La línea 18 **tiene** la misma experiencia que la línea 14, usando `Zoo_en.properties`. Finalmente, la línea 20 **tiene** un trabajo más fácil y **encuentra** una clave coincidente en `Zoo_en_CA.properties`.

En este ejemplo, **se usaron** solo tres archivos de propiedades. Incluso cuando la propiedad no **se encontró** en los paquetes de recursos `en_CA` o `en`, el programa prefirió usar `Zoo.properties` (el paquete de recursos predeterminado) en lugar de `Zoo_en_US.properties` (el local predeterminado).

Si una propiedad no **se encuentra** en ningún paquete de recursos, **se lanza** una excepción:

```Java
System.out.print(rb.getString("close")); // MissingResourceException
```

## Formateando Mensajes

A menudo solo **se quiere** producir los datos de texto de un paquete de recursos, pero a veces **se quiere** formatear esos datos con parámetros. En programas reales, es común **sustituir** variables en medio de una cadena del paquete de recursos. La convención es usar un número dentro de llaves como `{0}`, `{1}`, etc. El número **indica** el orden en que **se pasarán** los parámetros. Aunque los paquetes de recursos no **soportan** esto directamente, la clase `MessageFormat` sí lo hace.

Por ejemplo, **supongamos** que **tenemos** esta propiedad definida:

```
helloGreeting=Hello, {0} and {1}
```

En Java, **se puede** leer el valor normalmente. Después, **se puede** ejecutar a través de la clase `MessageFormat` para sustituir los parámetros. El segundo parámetro de `format()` es un vararg, lo que **permite** especificar cualquier número de valores de entrada.

**Supongamos** que **tenemos** un paquete de recursos `rb`:

```Java
String greeting = rb.getString("helloGreeting");
System.out.print(MessageFormat.format(greeting, "Tammy", "Henry"));
```

Esto **imprimirá** lo siguiente:

```Java
Hello, Tammy and Henry
```

## Usando la Clase *Properties*

Al trabajar con la clase `ResourceBundle`, también **se puede** encontrar la clase `Properties`. **Funciona** como la clase `HashMap` que **se aprendió** en el Capítulo 9, "Colecciones y Genéricos", excepto que **usa** valores `String` para las claves y los valores:

```Java
import java.util.Properties;
public class ZooOptions {
    public static void main(String[] args) {
        var props = new Properties();
        props.setProperty("name", "Our zoo");
        props.setProperty("open", "10am");
    }
}
```

La clase `Properties` es comúnmente usada para **manejar** valores que pueden no existir:

```Java
System.out.println(props.getProperty("camel"));        // null
System.out.println(props.getProperty("camel", "Bob")); // Bob
```

Si **se pasara** una clave que realmente existiera, ambas sentencias la **imprimirían**. Esto comúnmente **se conoce** como proporcionar un valor predeterminado, o de respaldo, para una clave faltante.

La clase `Properties` también incluye un método `get()`, pero solo `getProperty()` permite un valor predeterminado. Por ejemplo, la siguiente llamada es inválida ya que `get()` toma solo un parámetro:

```Java
props.get("open");                                // 10am
props.get("open", "The zoo will be open soon");   // NO COMPILA
```

---

**Ver también:** [[Internacionalización y Localización]] | [[Formateando Valores]] | [[Contenido/Capitulo 11/Resumen]]
