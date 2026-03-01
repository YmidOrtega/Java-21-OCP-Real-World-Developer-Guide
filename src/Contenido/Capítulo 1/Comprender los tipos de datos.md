Las aplicaciones Java contienen dos tipos de datos: tipos primitivos y tipos de referencia. En esta sección, **se analizan** las diferencias entre un tipo primitivo y un tipo de referencia.

### Uso de tipos primitivos

Java cuenta con ocho tipos de datos integrados, denominados tipos primitivos de Java. Estos ocho tipos de datos representan los bloques de construcción de los objetos Java, ya que todos los objetos Java son simplemente una colección compleja de estos tipos de datos primitivos. Dicho esto, un primitivo no es un objeto en Java, ni representa un objeto. Un primitivo es simplemente un valor único en memoria, como un número o un carácter.

#### Los tipos primitivos

El examen asume que **se tiene un buen conocimiento** de los ocho tipos de datos primitivos, sus tamaños relativos y qué se puede almacenar en ellos. La Tabla 1.5 muestra los tipos primitivos de Java junto con su tamaño en bits y el rango de valores que cada uno almacena.

**TABLA 1.5** Tipos primitivos

|**Palabra clave**|**Tipo**|**Valor mín**|**Valor máx**|**Valor predeterminado**|**Ejemplo**|
|---|---|---|---|---|---|
|**boolean**|verdadero o falso|n/a|n/a|`false`|`true`|
|**byte**|valor entero de 8 bits|-128|127|`0`|`123`|
|**short**|valor entero de 16 bits|-32,768|32,767|`0`|`123`|
|**int**|valor entero de 32 bits|-2,147,483,648|2,147,483,647|`0`|`123`|
|**long**|valor entero de 64 bits|$-2^{63}$|$2^{63}-1$|`0L`|`123L`|
|**float**|valor de punto flotante de 32 bits|n/a|n/a|`0.0f`|`123.45f`|
|**double**|valor de punto flotante de 64 bits|n/a|n/a|`0.0`|`123.456`|
|**char**|valor Unicode de 16 bits|0|65,535|`\u0000`|`'a'`|
La Tabla 1.5 contiene mucha información. A continuación, se examinan algunos puntos clave:

- Los tipos `byte`, `short`, `int` y `long` se utilizan para valores enteros sin puntos decimales.
- Cada tipo numérico utiliza el doble de bits que el tipo similar más pequeño. Por ejemplo, `short` utiliza el doble de bits que `byte`.
- Todos los tipos numéricos tienen signo (_signed_) y reservan uno de sus bits para cubrir un rango negativo. Por ejemplo, en lugar de que `byte` cubra de 0 a 255 (o incluso de 1 a 256), en realidad cubre de -128 a 127.
- Un `float` requiere la letra `f` o `F` a continuación del número para que Java sepa que es un `float`. Sin una `f` o `F`, Java interpreta un valor decimal como un `double`.
- Un `long` requiere la letra `l` o `L` a continuación del número para que Java sepa que es un `long`. Sin una `l` o `L`, Java interpreta un número sin punto decimal como un `int` en la mayoría de los escenarios.

No se preguntará sobre los tamaños exactos de estos tipos, aunque se debe tener una idea general del tamaño de los tipos más pequeños como `byte` y `short`. Una pregunta común entre los nuevos desarrolladores de Java es: ¿cuál es el tamaño en bits de `boolean`? La respuesta es que no está especificado y depende de la JVM donde se esté ejecutando el código.

#### Escritura de literales

Hay algunos aspectos más que se deben conocer sobre los primitivos numéricos. Cuando un número está presente en el código, se denomina **literal**. Por defecto, Java asume que se está definiendo un valor `int` con un literal numérico. En el siguiente ejemplo, el número listado es mayor de lo que cabe en un `int`. Cabe recordar que no se espera memorizar el valor máximo para un `int`. El examen lo incluirá en la pregunta si surge el tema.

```java
long max = 3123456789; // NO COMPILA
```

Java señala que el número está fuera de rango. Y lo está, para un `int`. Sin embargo, no se trata de un `int`. La solución consiste en añadir el carácter `L` al número.

```Java
long max = 3123456789L; // Ahora Java sabe que es un long
```

Alternativamente, se podría añadir una `l` minúscula al número. Pero se recomienda utilizar la `L` mayúscula. La `l` minúscula se parece al número 1.

Otra forma de especificar números es cambiando la «base». Al aprender a contar, se estudiaron los dígitos 0-9. Este sistema de numeración se llama base 10, ya que hay 10 valores posibles para cada dígito. También se conoce como sistema decimal. Java permite especificar dígitos en varios otros formatos:

- **Octal** (dígitos 0-7), que utiliza el número 0 como prefijo; por ejemplo, `017`.
- **Hexadecimal** (dígitos 0-9 y letras A-F/a-f), que utiliza `0x` o `0X` como prefijo; por ejemplo, `0xFF`, `0xff`, `0XFf`. El sistema hexadecimal no distingue entre mayúsculas y minúsculas, por lo que todos estos ejemplos significan el mismo valor.
- **Binario** (dígitos 0-1), que utiliza el número 0 seguido de `b` o `B` como prefijo; por ejemplo, `0b10`, `0B10`.

Se debe asegurar la capacidad de reconocer valores literales válidos que puedan asignarse a números.

#### Literales y el carácter de guion bajo

Lo último que se necesita saber sobre los literales numéricos es que se pueden incluir guiones bajos en los números para facilitar su lectura.

```Java
int million1 = 1000000;
int million2 = 1_000_000;
```

Se prefiere la lectura del segundo porque los ceros no se mezclan. Se pueden añadir guiones bajos en cualquier lugar excepto al principio de un literal, al final de un literal, justo antes de un punto decimal o justo después de un punto decimal. Incluso se pueden colocar múltiples caracteres de guion bajo uno junto al otro, aunque no se recomienda.

Se examinarán algunos ejemplos:

```Java
double notAtStart = _1000.00; // NO COMPILA
double notAtEnd = 1000.00_; // NO COMPILA
double notByDecimal = 1000_.00; // NO COMPILA
double annoyingButLegal = 1_00_0.0_0; // Feo, pero compila
double reallyUgly = 1__2; // También compila
```

### Uso de tipos de referencia

Un tipo de referencia hace referencia a un objeto (una instancia de una clase). A diferencia de los tipos primitivos, que almacenan sus valores en la memoria donde se asigna la variable, las referencias no contienen el valor del objeto al que hacen referencia. En su lugar, una referencia «apunta» a un objeto almacenando la dirección de memoria donde se ubica el objeto, un concepto conocido como puntero. A diferencia de otros lenguajes, Java no permite conocer cuál es la dirección de memoria física. Solo se puede utilizar la referencia para referirse al objeto.

Se examinarán algunos ejemplos que declaran e inicializan tipos de referencia. Supóngase que se declara una referencia de tipo `String`.

```Java
String greeting;
```

La variable `greeting` es una referencia que solo puede apuntar a un objeto `String`. Se asigna un valor a una referencia de una de las dos formas siguientes:

- Una referencia se puede asignar a otro objeto del mismo tipo o de un tipo compatible.
- Una referencia se puede asignar a un nuevo objeto utilizando la palabra clave `new`.

Por ejemplo, la siguiente declaración asigna esta referencia a un nuevo objeto:

```Java
greeting = new String("How are you?");
```

La referencia `greeting` apunta a un nuevo objeto `String`, «How are you?». El objeto `String` no tiene nombre y solo se puede acceder a él a través de una referencia correspondiente.

#### Objetos vs. referencias

No se debe confundir una referencia con el objeto al que hace referencia; son dos entidades diferentes. La referencia es una variable que posee un nombre y puede utilizarse para acceder al contenido de un objeto. Una referencia puede asignarse a otra referencia, pasarse a un método o devolverse desde un método. Todas las referencias tienen el mismo tamaño, independientemente de cuál sea su tipo.

Un objeto reside en el _heap_ (montículo) y no tiene nombre. Por lo tanto, no existe forma de acceder a un objeto excepto a través de una referencia. Los objetos se presentan en diversas formas y tamaños, y consumen cantidades variables de memoria. Un objeto no puede asignarse a otro objeto, y tampoco puede pasarse a un método ni devolverse desde él. Es el objeto el que es recolectado por el recolector de basura (_garbage collected_), no su referencia.

![[Objeto vs referencia.jpg]]
### Diferenciación entre primitivos y tipos de referencia

Existen algunas diferencias importantes que se deben conocer entre los primitivos y los tipos de referencia. En primer lugar, obsérvese que todos los nombres de tipos primitivos se escriben en minúsculas. Todas las clases incluidas en Java comienzan con mayúscula. Aunque no es obligatorio, es una práctica estándar, y se recomienda seguir esta convención también para las clases que se creen.

A continuación, los tipos de referencia pueden utilizarse para invocar métodos, asumiendo que la referencia no sea `null`. Los primitivos no tienen métodos declarados. En este ejemplo, se puede invocar un método sobre `reference` dado que es de un tipo de referencia. Se puede distinguir que `length` es un método porque lleva `()` después. Intente comprender por qué el siguiente fragmento no compila:

```Java
4: String reference = "hello";
5: int len = reference.length();
6: int bad = len.length(); // NO COMPILA
```

La línea 6 carece de sentido. No existen métodos en `len` porque es un primitivo `int`. Los primitivos no poseen métodos. Recuérdese que un `String` no es un primitivo, por lo que se pueden invocar métodos como `length()` en una referencia `String`, tal como se hizo en la línea 5.

Finalmente, a los tipos de referencia se les puede asignar `null`, lo que significa que actualmente no refieren a un objeto. Los tipos primitivos generarán un error de compilación si se intenta asignarles `null`. En este ejemplo, `value` no puede apuntar a `null` porque es de tipo `int`:

``` Java
int value = null; // NO COMPILA
String name = null;
```

Pero, ¿qué sucede si se desconoce el valor de un `int` y se desea asignarle `null`? En ese caso, se debe utilizar una clase envoltorio (_wrapper class_) numérica, como `Integer`, en lugar de `int`.

### Creación de clases envoltorio

Cada tipo primitivo posee una clase envoltorio, que es un tipo de objeto que corresponde al primitivo. La Tabla 1.6 enumera todas las clases envoltorio junto con la forma de crearlas.

**TABLA 1.6** Clases envoltorio (_Wrapper classes_)

|**Tipo primitivo**|**Clase envoltorio**|**¿Hereda de Number?**|**Ejemplo de creación**|
|---|---|---|---|
|**boolean**|Boolean|No|`Boolean.valueOf(true)`|
|**byte**|Byte|Sí|`Byte.valueOf((byte) 1)`|
|**short**|Short|Sí|`Short.valueOf((short) 1)`|
|**int**|Integer|Sí|`Integer.valueOf(1)`|
|**long**|Long|Sí|`Long.valueOf(1)`|
|**float**|Float|Sí|`Float.valueOf((float) 1.0)`|
|**double**|Double|Sí|`Double.valueOf(1.0)`|
|**char**|Character|No|`Character.valueOf('c')`|
Todas las clases numéricas de la Tabla 1.6 extienden la clase `Number`, lo que implica que todas incluyen algunos métodos auxiliares útiles: `byteValue()`, `shortValue()`, `intValue()`, `longValue()`, `floatValue()` y `doubleValue()`. Las clases envoltorio `Boolean` y `Character` incluyen `booleanValue()` y `charValue()`, respectivamente.

Como es de esperar, estos métodos devuelven el valor primitivo de una instancia de envoltura, en el tipo solicitado.

```Java
Double apple = Double.valueOf("200.99");
System.out.println(apple.byteValue()); // -56
System.out.println(apple.intValue()); // 200
System.out.println(apple.doubleValue()); // 200.99
```

Estos métodos auxiliares intentan convertir los valores de la mejor manera posible, pero pueden resultar en una pérdida de precisión. En el primer ejemplo, el valor 200 no existe en el tipo `byte`, por lo que se desborda (_wraps around_) a -56. En el segundo ejemplo, el valor se trunca, lo que significa que se eliminan todos los números después del punto decimal. En el capítulo 5, se aplicarán el _autoboxing_ y el _unboxing_ para mostrar la facilidad con la que Java permite trabajar con valores primitivos y de envoltorio.

Algunas de las clases envoltorio contienen métodos auxiliares adicionales para trabajar con números. No es necesario memorizar estos métodos; se puede asumir que cualquiera que se presente en el examen es válido. Por ejemplo, `Integer` dispone de los siguientes:

- `max(int num1, int num2)`: devuelve el mayor de los dos números.
- `min(int num1, int num2)`: devuelve el menor de los dos números.
- `sum(int num1, int num2)`: suma los dos números.
Aquí tienes la traducción y adaptación al **modo impersonal / pasivo**:

### Definición de bloques de texto

Anteriormente se observó un `String` simple con el valor "hello". ¿Qué sucede si se desea tener un `String` con algo más complicado? Por ejemplo, se analizará cómo crear un `String` con este valor:

```Plaintext
"Guia de estudio de Java"
by Ymidツ
```

Construir esto como un `String` requiere dos elementos que aún no se han aprendido. La sintaxis `\"` permite indicar que se desea una `"` en lugar de finalizar el `String`, y `\n` indica que se desea una nueva línea. Ambos se denominan caracteres de escape porque la barra invertida proporciona un significado especial. Con estas dos nuevas habilidades, se puede escribir lo siguiente:

```Java
String eyeTest = "\"Guia de estudio de Java\"\n by Ymidツ";
```

Si bien esto funciona, resulta difícil de leer. Afortunadamente, Java dispone de bloques de texto, también conocidos como cadenas multilínea. Véase la siguiente imagen para el equivalente en bloque de texto.

![[Bloque de texto.png]]

Un bloque de texto comienza y termina con tres comillas dobles (`"""`), y el contenido no necesita ser escapado. Esto resulta mucho más fácil de leer. Observe que el tipo sigue siendo `String`. Esto significa que cualquier método de `String` que ya se conozca o se aprenda en el capítulo 4 se aplica también a los bloques de texto. También implica que se utiliza un bloque de texto con cualquier método que acepte un `String`. Por ejemplo:

```Java
public String label(String title, String author) {
    return """
        Project:
        """ + title + " by " + author;
}
public void prepare() {
    String labelled = label("""
        Guia de estudio de Java
        Para Java 21
        2026 Edición""", "Ymidツ");
    System.out.println(labelled);
}
```

Es posible que se hayan notado las palabras **espacio en blanco incidental** y **esencial** en la figura. ¿En qué consisten? El espacio en blanco esencial forma parte del `String` y es importante. El espacio en blanco incidental simplemente está ahí para facilitar la lectura del código. Se puede reformatear el código y cambiar la cantidad de espacio en blanco incidental sin ningún impacto en el valor del `String`.

Imagine una línea vertical trazada en el carácter no espaciado más a la izquierda de tu bloque de texto. Todo lo que esté a su izquierda es espacio en blanco incidental, y todo lo que esté a su derecha es espacio en blanco esencial. Se probará con un ejemplo. ¿Cuántas líneas produce esta salida y cuántos caracteres de espacio en blanco incidental y esencial comienzan cada línea?

```Java
14: String pyramid = """
15:   *
16:  * *
17: * * *
18: """;
19: System.out.print(pyramid);
```

Existen cuatro líneas de salida. Las líneas 15 a 17 tienen asteriscos. La línea 18 es una línea sin caracteres. Las tres comillas de cierre habrían tenido que estar en la línea 17 si no se deseara esa línea en blanco. Aquí no hay caracteres de espacio en blanco incidentales. Las `"""` de cierre en la línea 18 son los caracteres más a la izquierda, por lo que la línea imaginaria se traza en la posición más a la izquierda. La línea 15 tiene dos caracteres de espacio en blanco esenciales para comenzar la línea, y la línea 16 tiene uno. Ese espacio en blanco rellena la línea trazada para coincidir con la línea 18.

La Tabla 1.7 muestra algunas secuencias de formato especiales y compara su funcionamiento en un `String` regular y en un bloque de texto. 

**TABLA 1.7** Formato de bloques de texto

|**Formato**|**Significado en String regular**|**Significado en bloque de texto**|
|---|---|---|
|`\"`|`"`|`"`|
|`\"""`|n/a – No válido|`"""`|
|`\"\"\"`|`"""`|`"""`|
|Espacio (al final de línea)|Espacio|Ignorado|
|`\s`|Dos espacios (`\s` es un espacio y preserva el espacio inicial en la línea)|Dos espacios|
|`\` (al final de línea)|n/a – No válido|Omite la nueva línea en esa línea|

Se probarán algunos ejemplos. Primero, ¿se observa por qué esto no compila?

```Java
String block = """doe"""; // NO COMPILA
```

Los bloques de texto requieren un salto de línea después de las `"""` de apertura, lo que hace que este sea inválido. Ahora se probará uno válido. ¿Cuántas líneas se cree que hay en este bloque de texto?

```Java
String block = """
doe \
deer""";
```

Solo una. La salida es `doe deer` dado que la `\` indica a Java que no añada una nueva línea antes de `deer`. Se intentará determinar el número de líneas en otro bloque de texto.

```Java
String block = """
doe \n
deer
""";
```

Esta vez hay cuatro líneas. Dado que el bloque de texto tiene las `"""` de cierre en una línea separada, se tienen tres líneas correspondientes al bloque de texto más el `\n` explícito. Se probará uno más. ¿Qué se cree que imprime esto?

```Java
String block = """
"doe\"\"\"
\"deer\"""
""";
System.out.println("*"+ block + "*");
```

La respuesta es:

```Plaintext
* "doe"""
"deer"""
*
```

Todos los `\"` escapan las comillas `"`. Existe un espacio de espacio en blanco esencial en las líneas `doe` y `deer`. Todo el resto del espacio en blanco inicial es espacio en blanco incidental.