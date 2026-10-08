Este capítulo trata sobre la creación de aplicaciones que se adapten al cambio. ¿Qué pasa si un usuario ingresa datos inválidos en una página web? ¿Y si nuestra conexión con una base de datos se interrumpe en medio de una venta? Por último, ¿cómo desarrollamos aplicaciones que puedan admitir múltiples idiomas o regiones geográficas?

En este capítulo, analizamos estos problemas y sus soluciones mediante **excepciones, formateo y localización**. Una forma de asegurarte de que tus aplicaciones respondan a los cambios es incorporar la compatibilidad desde el principio. Por ejemplo, ofrecer compatibilidad con la localización no significa que realmente debas admitir idiomas específicos de inmediato. Solo significa que tu aplicación podrá adaptarse más fácilmente en el futuro. Al final de este capítulo, esperamos haberte proporcionado una estructura para diseñar aplicaciones que se adapten mejor a los cambios.

### Comprensión de las excepciones

Un programa puede fallar por prácticamente cualquier motivo. Estas son solo algunas posibilidades:

- El código intenta conectarse a un sitio web, pero la conexión a Internet no funciona.
- Cometiste un error de programación e intentaste acceder a un índice inválido en un array.
- Un método llama a otro con un valor que ese método no admite.

Como puedes ver, algunos de estos son errores de programación. Otros están completamente fuera de tu control. Tu programa no tiene la culpa si se interrumpe la conexión a Internet. Lo que sí puede hacer es lidiar con la situación.

### El rol de las excepciones

Una **excepción** es la forma que tiene Java de decir: Me rindo. No sé qué hacer en este momento. Resuélvelo tú. Cuando escribes un método, puedes manejar la excepción o dejar que sea un problema del código que lo invoca.

A modo de ejemplo, imagina que Java es un niño que visita el zoológico. La **ruta feliz** (_happy path_) es cuando nada sale mal. El niño sigue observando a los animales hasta que el programa termina sin problemas. No pasó nada malo y no hubo excepciones que manejar.

La hermana menor de este niño no vive la ruta feliz. En medio de toda la emoción, tropieza y se cae. Por suerte, no es una caída grave. La niña se levanta y sigue observando más animales. Ha manejado el problema por sí misma. Desafortunadamente, más tarde ese mismo día se cae de nuevo y empieza a llorar. Esta vez, al llorar, ha dado a entender que necesita ayuda. La historia termina bien. Su papá le frota la rodilla y le da un abrazo. Luego regresan a ver más animales y disfrutan el resto del día.

Estos son los dos enfoques que utiliza Java al manejar excepciones. Un método puede **manejar el caso de excepción por sí mismo** o **hacer que sea responsabilidad de quien lo invoca**.

> **Códigos de retorno frente a excepciones**
>
> Las excepciones se utilizan cuando algo sale mal. Sin embargo, la palabra mal es subjetiva. El siguiente código devuelve `-1` en lugar de lanzar una excepción si no se encuentra ninguna coincidencia:
>
> ```Java
> public int indexOf(String[] names, String name) {
>     for (int i = 0; i < names.length; i++) {
>         if (names[i].equals(name)) { return i; }
>     }
>     return -1;
> }
> ```
>
> Aunque es común en ciertas tareas, como las búsquedas, por lo general **se deben evitar los códigos de retorno**. Después de todo, Java ofrece un marco de excepciones, ¡así que deberías usarlo!

### Comprensión de los tipos de excepciones

Una excepción es un **evento que altera el flujo del programa**. Java cuenta con la clase `Throwable` para todos los objetos que representan estos eventos. No todos ellos incluyen la palabra excepción en el nombre de su clase, lo cual puede resultar confuso. La imagen a continuación muestra las subclases principales de `Throwable`.

![[Categorías de excepción.png]]

¡Asegúrate de **memorizar la imagen anterior** para el examen! Repasaremos los detalles en este capítulo, pero es muy probable que aparezca en el examen.

#### Excepciones verificadas (_Checked Exceptions_)

Una **excepción verificada** es aquella que **debe ser declarada o manejada** por el código de la aplicación en el lugar donde se lanza. En Java, todas las excepciones verificadas heredan de `Exception`, pero no de `RuntimeException`. Las excepciones verificadas suelen ser más previsibles; por ejemplo, intentar leer un archivo que no existe.

> Las excepciones verificadas también incluyen cualquier clase que herede de `Throwable` pero no de `Error` o `RuntimeException`, como una clase que extienda directamente a `Throwable`. Para el examen, solo necesitas conocer las excepciones verificadas que extienden a `Exception`.

¿Excepciones verificadas? ¿Qué es lo que verificamos? Java tiene una regla llamada **regla de manejo o declaración**. La regla de manejar o declarar significa que todas las excepciones verificadas que podrían lanzarse dentro de un método deben estar envueltas en bloques `try` y `catch` compatibles o declaradas en la firma del método.

Debido a que las excepciones verificadas suelen ser anticipadas, Java impone la regla de que el programador debe hacer algo para demostrar que se tomó en cuenta la excepción. Tal vez se manejó dentro del método. O tal vez el método declara que no puede manejar la excepción y que alguien más debe hacerlo.

Veamos un ejemplo. El siguiente método `fall()` declara que podría lanzar una `IOException`, que es una excepción verificada:

```Java
void fall(int distance) throws IOException {
	if(distance> 10) {
		throw new IOException();
	}
}
```

Fíjate que aquí estás usando dos palabras clave diferentes. La palabra clave `throw` le indica a Java que quieres lanzar una excepción, mientras que la palabra clave `throws` **simplemente declara que el método podría lanzar una excepción**. Aunque también podría no hacerlo.

Ahora que ya sabes cómo declarar una excepción, ¿cómo la manejas? La siguiente versión alternativa del método `fall()` maneja la excepción:

```Java
void fall(int distance) {
	try {
		if(distance> 10) {
			throw new IOException();
		}
	} catch (Exception e) {
		e.printStackTrace();
	}
}
```

Observa que la instrucción `catch` utiliza `Exception`, no `IOException`. Dado que `IOException` es una subclase de `Exception`, el bloque `catch` puede capturarla. Analizaremos los bloques `try` y `catch` con más detalle más adelante en este capítulo.

#### Excepciones no verificadas (_Unchecked Exceptions_)

Una **excepción no verificada** es cualquier excepción que **no necesita ser declarada ni manejada** por el código de la aplicación donde se lanza. A las excepciones no verificadas se les suele llamar excepciones de tiempo de ejecución, aunque en Java, las excepciones no verificadas incluyen cualquier clase que herede de `RuntimeException` o `Error`.

> Está permitido manejar o declarar una excepción no verificada. Dicho esto, **es mejor documentar las excepciones no verificadas** que los usuarios deben conocer en un comentario Javadoc, en lugar de declarar una excepción no verificada.

Una **excepción en tiempo de ejecución** se define como la clase `RuntimeException` y sus subclases. Las excepciones en tiempo de ejecución suelen ser **inesperadas, pero no necesariamente fatales**. Por ejemplo, acceder a un índice de matriz no válido es algo inesperado. Aunque heredan de la clase `Exception`, **no son excepciones verificadas**.

Una excepción no verificada puede ocurrir en casi cualquier línea de código, ya que no es obligatorio manejarla ni declararla. Por ejemplo, se puede lanzar una `NullPointerException` en el cuerpo del siguiente método si la referencia de entrada es nula:

```Java
void fall(String input) {
	System.out.println(input.toLowerCase());
}
```

Trabajamos con objetos en Java con tanta frecuencia que una `NullPointerException` puede ocurrir casi en cualquier lugar. Si tuvieras que declarar excepciones no verificadas en todas partes, ¡cada método estaría lleno de ese desorden! Recuerda que **el código seguirá compilándose aunque declares una excepción no verificada redundante**.

#### `Error` y `Throwable`

`Error` significa que algo salió tan terriblemente mal que tu programa **no debería intentar recuperarse de ello**. Por ejemplo, la unidad de disco desapareció o el programa se quedó sin memoria. Estas son condiciones anormales con las que probablemente no te encontrarás y de las que no es posible recuperarse.

Para el examen, lo único que necesitas saber sobre `Throwable` es que **es la clase padre de todas las excepciones**, incluida la clase `Error`. Si bien puedes manejar excepciones de tipo `Throwable` y `Error`, **no se recomienda hacerlo en el código de tu aplicación**. Cuando nos referimos a excepciones en este capítulo, generalmente nos referimos a cualquier clase que herede de `Throwable`, aunque casi siempre trabajamos con la clase `Exception` o sus subclases.

#### Repaso de los tipos de excepciones

Asegúrate de estudiar detenidamente todo lo que aparece en la Tabla 11.1. Para el examen, recuerda que un `Throwable` es una `Exception` o un `Error`. **No debes capturar un `Throwable` directamente en tu código.**

##### TABLA 11.1 Tipos de excepciones y errores

| **Tipo**                                            | **Cómo reconocerlo**                                   | **¿Es correcto que el programa la capture?** | **¿El programa debe manejarla o declararla obligatoriamente?** |
| --------------------------------------------------- | ------------------------------------------------------ | -------------------------------------------- | -------------------------------------------------------------- |
| **Excepción no verificada** (_Unchecked exception_) | Subclase de `RuntimeException`                         | Sí                                           | No                                                             |
| **Excepción verificada** (_Checked exception_)      | Subclase de `Exception`, pero no de `RuntimeException` | Sí                                           | Sí                                                             |
| **Error**                                           | Subclase de `Error`                                    | No                                           | No                                                             |

### Lanzar una excepción

**Cualquier código Java puede lanzar una excepción**; esto incluye el código que tú escribas. Algunas excepciones vienen incluidas en Java. Es posible que te encuentres con una excepción inventada para el examen. No hay problema. La pregunta dejará claro que se trata de una excepción al hacer que el nombre de la clase termine en `Exception`. Por ejemplo, `MyMadeUpException` es claramente una excepción.

> En Java es una práctica común que las clases de excepción terminen con la palabra `Exception`, pero no es obligatorio. ¡Sin embargo, **debes seguir esta convención** al crear tus propias clases de excepción!

En el examen, verás **dos tipos de código que provocan una excepción**. El primero es código que está mal. Aquí hay un ejemplo:

```Java
String[] animals = new String[0];
System.out.println(animals[0]); // ArrayIndexOutOfBoundsException
```

Este código lanza una `ArrayIndexOutOfBoundsException` ya que el array no tiene elementos. Eso significa que las preguntas sobre excepciones pueden estar ocultas en preguntas que parecen tratar sobre otro tema.

> En el examen, algunas preguntas incluyen una opción relacionada con que el código no se compile o con que lance una excepción. Presta especial atención al código que llama a un método con una referencia nula o que hace referencia a un índice inválido de una matriz o una lista. Si detectas esto, sabrás que la respuesta correcta es que **el código lanza una excepción en tiempo de ejecución**.

La segunda forma en que el código puede provocar una excepción es **solicitarle explícitamente a Java que lance una**. Java te permite escribir sentencias como estas:

```Java
throw new Exception();
throw new Exception("¡Ay! Me caí.");
throw new RuntimeException();
throw new RuntimeException("¡Ay! Me caí.");
```

La palabra clave `throw` le indica a Java que quieres que otra parte del código se encargue de la excepción. Esto es lo mismo que la niña pequeña que llora llamando a su papá. Alguien más debe resolver qué hacer con la excepción.

> **`throw` vs. `throws`**
>
> Cada vez que veas `throw` o `throws` en el examen, asegúrate de que se esté utilizando la opción correcta. La palabra clave `throw` **se usa como una instrucción dentro de un bloque de código** para lanzar una nueva excepción o relanzar una excepción existente, mientras que la palabra clave `throws` **se usa únicamente al final de la declaración de un método** para indicar qué excepciones admite.

Al crear una excepción, por lo general puedes pasar un parámetro de tipo `String` con un mensaje, o puedes no pasar ningún parámetro y usar los valores predeterminados. Decimos por lo general porque se trata de una convención. Alguien ha declarado un constructor que toma un `String`. También se podría crear una clase de excepción que no tenga un constructor que acepte un mensaje.

Además, debes saber que **una excepción (`Exception`) es un objeto (`Object`)**. Esto significa que puedes almacenarla en una referencia de objeto, y esto es válido:

```Java
var e = new RuntimeException();
throw e;
```

El código instancia una excepción en una línea y luego la lanza en la siguiente. La excepción puede provenir de cualquier lugar, incluso ser pasada a un método. **Siempre que sea una excepción válida, se puede lanzar.**

El examen también podría intentar engañarte. ¿Ves por qué este código no se compila?

```Java
throw RuntimeException(); // NO SE COMPILA
```

Si tu respuesta es que falta una palabra clave, tienes toda la razón. **La excepción nunca se instancia con la palabra clave `new`.**

Echemos un vistazo a otro punto en el que el examen podría intentar engañarte. ¿Puedes ver por qué lo siguiente no se compila?

```Java
3: try {
4:    throw new RuntimeException();
5:    throw new ArrayIndexOutOfBoundsException(); // NO SE COMPILA
6: } catch (Exception e) {}
```

Dado que la línea 4 lanza una excepción, nunca se puede llegar a la línea 5 durante el tiempo de ejecución. El compilador lo reconoce y reporta un **error de código inaccesible**.

### Llamadas a métodos que lanzan excepciones

Cuando llamas a un método que lanza una excepción, **las reglas son las mismas que para manejar una excepción dentro del método**. ¿Entiendes por qué lo siguiente no se compila?

```Java
class NoMoreCarrotsException extends Exception {}
public class Bunny {
	private void eatCarrot() throws NoMoreCarrotsException {}
	public void hopAround() {
		eatCarrot(); // NO SE COMPILA
	}
}
```

El problema es que `NoMoreCarrotsException` es una excepción verificada. **Las excepciones verificadas deben manejarse o declararse.** El código se compilaría si cambiaras el método `hopAround()` por cualquiera de estas opciones:

```Java
// Opción 1
public void hopAround() throws NoMoreCarrotsException {
	eatCarrot();
}
// Opción 2
public void hopAround() {
	try {
		eatCarrot();
	} catch (NoMoreCarrotsException e) {
		System.out.print("conejo triste");
	}
}
```

Quizás hayas notado que `eatCarrot()` no lanzó una excepción; simplemente declaró que podría hacerlo. Esto es suficiente para que el compilador exija que quien llama al método maneje o declare la excepción.

El compilador sigue buscando código inaccesible. **Declarar una excepción no utilizada no se considera código inaccesible.** Le da al método la opción de cambiar la implementación para lanzar esa excepción en el futuro. ¿Ves cuál es el problema aquí?

```Java
public class Bunny {
	private void eatCarrot() {}
	public void bad() {
		try {
			eatCarrot();
		} catch (NoMoreCarrotsException e) { // NO SE COMPILA
			System.out.print("conejo triste");
		}
	}
}
```

Java sabe que `eatCarrot()` no puede lanzar una excepción verificada, lo que significa que no hay forma de que se llegue al bloque `catch` en `bad()`.

> Cuando veas una excepción verificada declarada dentro de un bloque `catch` en el examen, asegúrate de que el código en el bloque `try` asociado sea capaz de lanzar la excepción o una subclase de la misma. De lo contrario, **el código es inalcanzable y no se compila**. Recuerda que esta regla no se aplica a las excepciones no verificadas ni a las excepciones declaradas en la firma de un método.

### Sobrescritura de métodos con excepciones

Cuando presentamos la sobrescritura de métodos en el Capítulo 6, Diseño de clases, incluimos una regla relacionada con las excepciones. Un método sobrescrito **no puede declarar ninguna excepción verificada nueva o más amplia** que la del método del que hereda. Por ejemplo, este código no está permitido:

```Java
class CanNotHopException extends Exception {}
class Hopper {
	public void hop() {}
}
public class Bunny extends Hopper {
	public void hop() throws CanNotHopException {} // NO SE COMPILA
}
```

Java sabe que `hop()` no puede lanzar ninguna excepción verificada porque el método `hop()` de la superclase `Hopper` no declara ninguna. Imagina qué pasaría si las versiones del método en las subclases pudieran agregar excepciones verificadas: podrías escribir código que llamara al método `hop()` de `Hopper` sin manejar ninguna excepción. Entonces, si se utilizara `Bunny` en su lugar, el código no sabría cómo manejar ni declarar `CanNotHopException`.

Un método sobrescrito en una subclase **puede declarar menos excepciones que la superclase o la interfaz**. Esto es válido porque quienes lo llaman ya las están manejando.

```Java
class Hopper {
	public void hop() throws CanNotHopException {}
}
public class Bunny extends Hopper {
	public void hop() {} // Esto está bien
}
```

Un método sobrescrito que no declare una de las excepciones lanzadas por el método padre es similar a que el método declare que lanza una excepción que en realidad nunca lanza. **Esto es perfectamente válido.** Del mismo modo, una clase puede declarar una subclase de un tipo de excepción. La idea es la misma: la superclase o interfaz ya se ha encargado de un tipo más amplio.

### Imprimir una excepción

Hay **tres formas de imprimir una excepción**. Puedes dejar que Java la imprima, imprimir solo el mensaje o imprimir de dónde proviene el rastreo de la pila. Este ejemplo muestra los tres enfoques:

```Java
5:  public static void main(String[] args) {
6:     try {
7:        hop();
8:     } catch (Exception e) {
9:        System.out.println(e + "\n");
10:       System.out.println(e.getMessage() + "\n");
11:       e.printStackTrace();
12:    }
13: }
14: private static void hop() {
15:    throw new RuntimeException("no puede saltar");
16: }
```

Este código imprime lo siguiente:

```Plaintext
java.lang.RuntimeException: no puede saltar

no puede saltar

java.lang.RuntimeException: no puede saltar
    at Handling.hop(Handling.java:15)
    at Handling.main(Handling.java:7)
```

La primera línea muestra lo que Java imprime por defecto: el tipo de excepción y el mensaje. La segunda línea muestra solo el mensaje. El resto muestra un rastro de pila. **El rastro de pila suele ser lo más útil** porque muestra la jerarquía de llamadas a métodos que se realizaron para llegar a la línea que lanzó la excepción.

**Ver también:** [[Reconociendo las Clases de Excepción]] | [[Manejando Excepciones]]
