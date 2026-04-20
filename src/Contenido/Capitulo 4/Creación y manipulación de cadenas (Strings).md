En el contexto de una interfaz de programación de aplicaciones (API), una **interfaz** se refiere a un grupo de clases o definiciones de interfaz de Java que dan acceso a la funcionalidad.

En este capítulo, se aprenden muchas estructuras de datos principales en Java, junto con las API más comunes para acceder a ellas. Por ejemplo, `String` y `StringBuilder`, junto con sus API asociadas, se utilizan para crear y manipular datos de texto. Luego se cubren los arreglos (_arrays_). Finalmente, se exploran las API de matemáticas y de fecha/hora.

### Creación y manipulación de cadenas (`Strings`)

La clase `String` es una clase tan fundamental que sería muy difícil escribir código sin ella. Después de todo, ni siquiera se puede escribir un método `main()` sin usar la clase `String`. Una cadena es básicamente una secuencia de caracteres; aquí hay un ejemplo:

```Java
String name = "Fluffy";
```

Como se aprendió en el Capítulo 1, «Bloques de construcción», este es un ejemplo de un **tipo de referencia**. También se aprendió que los objetos se crean utilizando la palabra clave `new`. Un momento. Falta algo en el ejemplo anterior: ¡no tiene `new` en él! En Java, estos dos fragmentos crean un `String`:

```Java
String name = "Fluffy";
String name = new String("Fluffy");
```

Ambos proporcionan una variable de referencia llamada `name` que apunta al objeto `String` `"Fluffy"`. Son sutilmente diferentes, como se verá más adelante en este capítulo. Por ahora, solo es necesario recordar que la clase `String` es especial y no necesita ser instanciada con `new`.

Además, los **bloques de texto** (_text blocks_) son otra forma de crear un `String`. A modo de repaso, este bloque de texto es igual a las variables anteriores:

```Java
String name = """
Fluffy""";
```

Dado que un `String` es una secuencia de caracteres, probablemente no sorprenda saber que implementa la interfaz `CharSequence`. Esta interfaz es una forma general de representar varias clases, incluyendo `String` y `StringBuilder`. Se aprenderá más sobre interfaces en el Capítulo 7, «Más allá de las clases».

En esta sección, se examina la concatenación, los métodos comunes y el encadenamiento de métodos (_method chaining_).

### Concatenación

En el Capítulo 2, «Operadores», se aprendió cómo sumar números. `1 + 2` es claramente 3. Pero, ¿qué es `"1" + "2"`? Es `"12"` porque Java combina los dos objetos `String`. Colocar un `String` antes de otro `String` y combinarlos se llama **concatenación de cadenas**. A los creadores del examen les gusta la concatenación de cadenas porque el operador `+` se puede usar de dos maneras dentro de la misma línea de código. No hay muchas reglas que conocer para esto, pero hay que conocerlas bien.

1. Si ambos operandos son numéricos, `+` significa **suma numérica**.
2. Si alguno de los operandos es un `String`, `+` significa **concatenación**.
3. La expresión se evalúa de **izquierda a derecha**. 

Ahora se observarán algunos ejemplos:

```Java
System.out.println(1 + 2);           // 3
System.out.println("a" + "b");       // ab
System.out.println("a" + "b" + 3);   // ab3
System.out.println(1 + 2 + "c");     // 3c
System.out.println("c" + 1 + 2);     // c12
System.out.println("c" + null);      // cnull
```

El primer ejemplo utiliza la primera regla. Ambos operandos son números, por lo que se usa la suma normal. El segundo ejemplo es una simple concatenación de cadenas, descrita en la segunda regla. Las comillas para el `String` se usan solo en el código; no se imprimen.

El tercer ejemplo combina la segunda y la tercera regla. Dado que se comienza por la izquierda, Java calcula a qué se evalúa `"a" + "b"`. Eso ya se sabe: es `"ab"`. Luego Java observa la expresión restante de `"ab" + 3`. La segunda regla indica que se debe concatenar ya que uno de los operandos es un `String`.

En el cuarto ejemplo, se comienza con la tercera regla, que indica considerar `1 + 2`. Ambos operandos son numéricos, por lo que la primera regla indica que la respuesta es 3. Luego se tiene `3 + "c"`, que utiliza la segunda regla para dar `"3c"`. ¿Se nota que las tres reglas se usan en una sola línea?

El quinto ejemplo muestra la importancia de la tercera regla. Primero se tiene `"c" + 1`, que utiliza la segunda regla para dar `"c1"`. Luego se tiene `"c1" + 2`, que utiliza la segunda regla nuevamente para dar `"c12"`.

Finalmente, el último ejemplo muestra cómo `null` se representa como una cadena cuando se concatena o imprime, dando `"cnull"`.

El examen lleva los trucos un paso más allá e intentará engañar con algo como esto:

```Java
int three = 3;
String four = "4";
System.out.println(1 + 2 + three + four);
```

Cuando se vea esto, simplemente se debe ir despacio, recordar las tres reglas y asegurarse de verificar los tipos de las variables. En este ejemplo, se comienza con la tercera regla, que indica considerar `1 + 2`. La primera regla da 3. A continuación, se tiene `3 + three`. Dado que `three` es de tipo `int`, todavía se usa la primera regla, dando 6. Luego, se tiene `6 + four`. Dado que `four` es de tipo `String`, se cambia a la segunda regla y se obtiene una respuesta final de `"64"`. Cuando se vean preguntas como esta, solo hay que tomarse el tiempo necesario y verificar los tipos. Ser metódico vale la pena.

Hay una cosa más que saber sobre la concatenación, pero es fácil. En este ejemplo, solo hay que recordar qué hace `+=`. Se debe tener en cuenta que `s += "2"` significa lo mismo que `s = s + "2"`.

```Java
4: var s = "1"; // s actualmente contiene "1"
5: s += "2";    // s actualmente contiene "12"
6: s += 3;      // s actualmente contiene "123"
7: System.out.println(s); // 123
```

En la línea 5, se están "sumando" dos cadenas, lo que significa que se concatenan. La línea 6 intenta engañar añadiendo un número, pero es exactamente como si se hubiera escrito `s = s + 3`. Se sabe que una cadena "más" cualquier otra cosa significa usar concatenación.

Para repasar las reglas una vez más: use suma numérica si hay dos números involucrados, use concatenación de lo contrario, y evalué de izquierda a derecha. ¿Ya se han memorizado estas tres reglas? ¡Hay que asegurarse de hacerlo antes del examen!

### Métodos importantes de `String`

La clase `String` tiene docenas de métodos. Afortunadamente, solo es necesario conocer un puñado para el examen. Los creadores del examen seleccionan la mayoría de los métodos que los desarrolladores utilizan en el mundo real.

Para todos estos métodos, se debe recordar que una cadena es una secuencia de caracteres y que **Java cuenta desde 0** cuando se indexa. La siguiente imagen muestra cómo se indexa cada carácter en la cadena `"animals"`.

![[Indexación para una cadena.png]]

También es necesario saber que un `String` es **inmutable**, o inalterable. Esto significa que llamar a un método en un `String` devolverá un objeto `String` diferente en lugar de cambiar el valor de la referencia. En este capítulo, se utilizan objetos inmutables. En el Capítulo 6, «Diseño de clases», se aprenderá cómo crear objetos inmutables propios.

Se observarán varios métodos de la clase `String`. Muchos de ellos son directos, por lo que no se discutirán extensamente. Es necesario saber cómo usar estos métodos.

#### Determinación de la longitud

El método `length()` devuelve el número de caracteres en el `String`. La firma del método es la siguiente:

```Java
public int length()
```

El siguiente código muestra cómo usar `length()`:

```Java
var name = "animals";
System.out.println(name.length()); // 7
```

Un momento. ¿Imprime 7? ¿No se acaba de decir que Java cuenta desde cero? La diferencia es que el conteo desde cero ocurre solo cuando se utilizan índices o posiciones dentro de una lista. Al determinar el tamaño total o la longitud, Java vuelve a usar el conteo normal.

#### Obtención de un solo carácter

El método `charAt()` permite consultar la cadena para averiguar qué carácter se encuentra en un índice específico. La firma del método es la siguiente:

```Java
public char charAt(int index)
```

El siguiente código muestra cómo usar `charAt()`:

```Java
var name = "animals";
System.out.println(name.charAt(0)); // a
System.out.println(name.charAt(6)); // s
System.out.println(name.charAt(7)); // excepción
```

Dado que los índices comienzan a contar en cero, `charAt(0)` devuelve el "primer" carácter de la secuencia. De manera similar, `charAt(6)` devuelve el "séptimo" carácter de la secuencia. Sin embargo, `charAt(7)` es un problema. Pide el "octavo" carácter de la secuencia, pero solo hay siete caracteres presentes. Cuando algo sale mal y Java no sabe cómo lidiar con ello, lanza una excepción, como se muestra a continuación. Se aprenderá más sobre excepciones en el Capítulo 11, «Excepciones y localización».

```Plaintext
java.lang.StringIndexOutOfBoundsException: String index out of range: 7
```

#### Trabajo con puntos de código (_Code Points_)

En este proyecto y en el examen, a menudo se usa el formato de codificación de datos ASCII. En todo el mundo, algunos caracteres utilizan una codificación más larga llamada Unicode, que tiene un rango más amplio y no cabe en un `char`, como una comilla estilizada (`’`). Un **punto de código** (_code point_) es más grande que un carácter, por lo que se expresa como un número. Las firmas de método relevantes son las siguientes:

```Java
public int codePointAt(int index)
public int codePointBefore(int index)
public int codePointCount(int beginIndex, int endIndex)
```

`codePointAt()` devuelve el valor numérico del punto de código en el índice especificado. El método `codePointBefore()` hace lo mismo, pero observa el valor antes del índice. Finalmente, el método `codePointCount()` devuelve el número de puntos de código entre dos índices.

```Java
var s = "We’re done feeding the animals";
System.out.println(s.charAt(0) + " " + s.codePointAt(0)); // W 87
System.out.println(s.charAt(2) + " " + s.codePointAt(2)); // ’ 8217
System.out.println(s.codePointBefore(3));                 // 8217
System.out.println(s.codePointCount(0,4));                // 4
```

¡No hay de qué preocuparse! No es necesario memorizar los valores ASCII o Unicode. Solo se necesita saber que si se ve `codePointAt()` en el examen, funciona de manera similar a `charAt()` para los caracteres ASCII, devolviendo el valor numérico del carácter en la ubicación.

#### Obtención de una subcadena (_Substring_)

El método `substring()` es similar a `charAt()` excepto que devuelve un grupo de caracteres de la cadena. El primer parámetro es el índice por el cual comenzar para la cadena devuelta. Como de costumbre, este es un índice basado en cero. Hay un segundo parámetro opcional, que es el índice final en el que se desea detener.

Note que se dijo "detener en" en lugar de "incluir". Esto significa que se permite que el parámetro `endIndex` sea **uno más allá** del final de la secuencia si se desea detener al final de la secuencia. Eso sería redundante, sin embargo, ya que se podría omitir el segundo parámetro por completo en ese caso. En el código propio, se desea evitar esta redundancia. No debe sorprender, sin embargo, si el examen la utiliza. Las firmas de los métodos son las siguientes:

```Java
public String substring(int beginIndex)
public String substring(int beginIndex, int endIndex)
```

Resulta útil pensar en los índices de manera un poco diferente para los métodos de subcadena. Imagine que los índices están justo _antes_ del carácter al que apuntarían. La figura a continuación ayuda a visualizar esto. Note cómo la flecha con el 0 apunta antes del carácter que tendría el índice 0. La flecha con el 1 apunta entre los caracteres con índices 0 y 1. Hay siete caracteres en el `String`. Dado que Java utiliza índices basados en cero, esto significa que el último carácter tiene un índice de 6. La flecha con el 7 apunta inmediatamente **después** de este último carácter. Esto ayudará a recordar que `endIndex` no da una excepción de fuera de límites (_out-of-bounds_) cuando es uno más allá del final del `String`.

![[Índices para una subcadena.png]]

El siguiente código muestra cómo usar `substring()`:

```Java
var name = "animals";
System.out.println(name.substring(3)); // mals
System.out.println(name.substring(name.indexOf('m'))); // mals
System.out.println(name.substring(3, 4)); // m
System.out.println(name.substring(3, 7)); // mals
```

El método `substring()` es el método de `String` más engañoso en el examen. El primer ejemplo indica que se tomen los caracteres comenzando con el índice 3 hasta el final, lo que da `"mals"`. El segundo ejemplo hace lo mismo, pero llama a `indexOf()` para obtener el índice en lugar de codificarlo (_hard-coding_). Esta es una práctica común al codificar porque es posible que no se conozca el índice de antemano.

El tercer ejemplo indica que se tomen los caracteres comenzando con el índice 3 hasta, pero **sin incluir**, el carácter en el índice 4. Esta es una forma complicada de decir que se desea un `String` con un solo carácter: el que está en el índice 3. Esto resulta en `"m"`. El ejemplo final indica que se tomen los caracteres comenzando con el índice 3 hasta llegar al índice 7. Dado que el índice 7 es el mismo que el final de la cadena, es equivalente al primer ejemplo.

Se espera que esto no haya sido demasiado confuso. Los siguientes ejemplos son menos obvios:

```Java
System.out.println(name.substring(3, 3)); // cadena vacía
System.out.println(name.substring(3, 2)); // excepción
System.out.println(name.substring(3, 8)); // excepción
```

El primer ejemplo en este conjunto imprime una cadena vacía. La solicitud es para los caracteres comenzando con el índice 3 hasta llegar al índice 3. Dado que se comienza y termina con el mismo índice, no hay caracteres en el medio. El segundo ejemplo en este conjunto lanza una excepción porque los índices no pueden estar al revés. Java sabe perfectamente que nunca llegará al índice 2 si comienza con el índice 3. El tercer ejemplo indica que se continúe hasta el octavo carácter. No hay octava posición, por lo que Java lanza una excepción. Por supuesto, tampoco hay séptimo carácter, pero al menos existe la posición invisible de "fin de cadena".

Se repasará esto una vez más ya que `substring()` es muy engañoso. El método devuelve la cadena comenzando desde el índice solicitado. Si se solicita un índice final, se detiene justo **antes** de ese índice. De lo contrario, va hasta el final de la cadena.

#### Búsqueda de un índice

El método `indexOf()` observa los caracteres en la cadena y encuentra el primer índice que coincide con el valor deseado. El método `indexOf` puede trabajar con un carácter individual o con un `String` completo como entrada. También puede comenzar y terminar la búsqueda desde posiciones específicas. Note que el índice de inicio es inclusivo y el índice de finalización es exclusivo. Recuerde que se puede pasar un `char` a un tipo de parámetro `int`. En el examen, solo se verá un `char` pasado a los parámetros llamados `ch`. Las firmas de los métodos son las siguientes:

```Java
public int indexOf(int ch)
public int indexOf(int ch, int fromIndex)
public int indexOf(int ch, int fromIndex, int endIndex)
public int indexOf(String str)
public int indexOf(String str, int fromIndex)
public int indexOf(String str, int fromIndex, int endIndex)
```

El siguiente código muestra cómo usar `indexOf()`:

```Java
10: var name = "animals";
11: System.out.println(name.indexOf('a')); // 0
12: System.out.println(name.indexOf("al")); // 4
13: System.out.println(name.indexOf('a', 4)); // 4
14: System.out.println(name.indexOf("al", 5)); // -1
15: System.out.println(name.indexOf('a', 2, 4)); // -1
16: System.out.println(name.indexOf("al", 2, 6)); // 4
```

Dado que los índices comienzan con 0, la primera `'a'` coincide en esa posición. Por lo tanto, la línea 11 emite 0. En la línea 12, Java busca una cadena más específica, por lo que coincide más tarde. En la línea 13, Java ni siquiera debería observar los caracteres hasta llegar al índice 4. La línea 14 no encuentra nada porque comienza a buscar después de que ocurrió la coincidencia. A diferencia de `charAt()`, el método `indexOf()` **no** lanza una excepción si no puede encontrar una coincidencia, en su lugar devuelve `-1`. Debido a que los índices comienzan con 0, el invocador sabe que `-1` no podría ser un índice válido. Esto lo convierte en un valor común para que un método indique al invocador que no se encontró ninguna coincidencia.

La línea 15 busca una coincidencia comenzando en el índice 2 y antes del índice 4. Esto significa los índices 2 o 3. Dado que ninguno de esos coincide, el método devuelve `-1`. Finalmente, la línea 16 busca una coincidencia comenzando en el índice 2 ya que los índices de inicio son inclusivos. Termina antes en el índice 6 ya que el índice final es exclusivo. Esto significa los índices 2, 3, 4 y 5. Los caracteres en el índice 4 y 5 coinciden con el objetivo. El primero de ellos es el 4, que es el que se devuelve.

#### Ajuste de mayúsculas y minúsculas (_Case_)

Uf. Después de ese ejercicio mental, ¡es agradable tener métodos que actúan exactamente como suenan! Estos métodos facilitan la conversión de los datos. Las firmas de los métodos son las siguientes:

```Java
public String toLowerCase()
public String toUpperCase()
```

El siguiente código muestra cómo usar estos métodos:

```Java
var name = "animals";
System.out.println(name.toUpperCase()); // ANIMALS
System.out.println("Abc123".toLowerCase()); // abc123
```

Estos métodos hacen lo que dicen. El método `toUpperCase()` convierte cualquier carácter en minúscula a mayúscula en la cadena devuelta. El método `toLowerCase()` convierte cualquier carácter en mayúscula a minúscula en la cadena devuelta. Estos métodos no alteran ningún carácter que no sea una letra. Además, recuerde que las cadenas son inmutables, por lo que la cadena original permanece igual.

#### Comprobación de igualdad

El método `equals()` comprueba si dos objetos `String` contienen exactamente los mismos caracteres en el mismo orden. El método `equalsIgnoreCase()` comprueba si dos objetos `String` contienen los mismos caracteres, con la excepción de que **ignora** si son mayúsculas o minúsculas. Las firmas de los métodos son las siguientes:

```Java
public boolean equals(Object obj)
public boolean equalsIgnoreCase(String str)
```

Es posible notar que `equals()` toma un `Object` en lugar de un `String`. Esto se debe a que el método es el mismo para todos los objetos. Si se pasa algo que no es un `String`, simplemente devolverá `false`. Por el contrario, el método `equalsIgnoreCase()` se aplica solo a objetos `String`, por lo que puede tomar el tipo más específico como parámetro.

En Java, los valores `String` distinguen entre mayúsculas y minúsculas (_case-sensitive_). Eso significa que `"abc"` y `"ABC"` se consideran valores diferentes. Teniendo esto en cuenta, el siguiente código muestra cómo usar estos métodos:

```Java
System.out.println("abc".equals("ABC")); // false
System.out.println("ABC".equals("ABC")); // true
System.out.println("ABC".equals(6));     // false
System.out.println("abc".equalsIgnoreCase("ABC")); // true
```

Este ejemplo debería ser bastante intuitivo. En el primer ejemplo, los valores no son exactamente iguales. En el segundo, son exactamente iguales. El tercer ejemplo muestra qué sucede si se pasa un tipo diferente. En el último ejemplo, los valores difieren solo por las mayúsculas, pero está bien porque se llamó al método que ignora las diferencias de mayúsculas y minúsculas.

### Sobrescritura de `toString()`, `equals(Object)` y `hashCode()`

Saber cómo sobrescribir adecuadamente `toString()`, `equals(Object)` y `hashCode()` era parte de los exámenes de certificación de Java en el pasado. Como desarrollador profesional de Java, sigue siendo importante conocer al menos las reglas básicas para sobrescribir cada uno de estos métodos.

- **`toString()`:** El método `toString()` se llama cuando se intenta imprimir un objeto o concatenarlo con un `String`. Comúnmente se sobrescribe con una versión que imprime una descripción única de la instancia utilizando los campos de la misma.
- **`equals(Object)`:** El método `equals(Object)` se utiliza para comparar objetos, donde la implementación predeterminada solo utiliza el operador `==`. Se debe sobrescribir el método `equals(Object)` siempre que se desee comparar elementos de manera conveniente para verificar su igualdad, especialmente si esto requiere la comprobación de numerosos campos.
- **`hashCode()`:** Siempre que se sobrescribe `equals(Object)`, **se debe** sobrescribir `hashCode()` para ser consistente. Esto significa que para dos objetos cualesquiera, si `a.equals(b)` es verdadero, entonces `a.hashCode() == b.hashCode()` también debe ser verdadero. Si no son consistentes, esto podría provocar datos no válidos y efectos secundarios en colecciones basadas en _hash_ como `HashMap` y `HashSet`.

Todos estos métodos proporcionan una implementación predeterminada en `Object`, pero si se desea hacer un uso inteligente de ellos, deben ser sobrescritos.

### Búsqueda de subcadenas

A menudo, es necesario buscar en una cadena más grande para determinar si contiene una subcadena. Los métodos `startsWith()` y `endsWith()` comprueban si el valor proporcionado coincide con parte del `String`. También existe un método `startsWith()` sobrecargado que especifica en qué parte del `String` comenzar a buscar. El método `contains()` no es tan particular; busca coincidencias en cualquier parte del `String`. Las firmas de los métodos son las siguientes:

```Java
public boolean startsWith(String prefix)
public boolean startsWith(String prefix, int fromIndex)
public boolean endsWith(String suffix)
public boolean contains(CharSequence charSeq)
```

El siguiente código muestra cómo usar estos métodos:

```Java
System.out.println("abc".startsWith("a"));    // true
System.out.println("abc".startsWith("A"));    // false
System.out.println("abc".startsWith("b", 1)); // true
System.out.println("abc".startsWith("b", 2)); // false
System.out.println("abc".endsWith("c"));      // true
System.out.println("abc".endsWith("a"));      // false
System.out.println("abc".contains("b"));      // true
System.out.println("abc".contains("B"));      // false
```

Nuevamente, no hay sorpresas aquí. Java realiza una comprobación que distingue entre mayúsculas y minúsculas (_case-sensitive_) sobre los valores proporcionados. Note que el método `contains()` es un método de conveniencia para no tener que escribir `str.indexOf(otherString) != -1`.

### Reemplazo de valores

El método `replace()` realiza una simple búsqueda y reemplazo en la cadena. Hay una versión que toma parámetros `char` y otra que toma parámetros `CharSequence`. Las firmas de los métodos son las siguientes:

```Java
public String replace(char oldChar, char newChar)
public String replace(CharSequence target, CharSequence replacement)
```

El siguiente código muestra cómo usar estos métodos:

```Java
System.out.println("abcabc".replace('a', 'A')); // AbcAbc
System.out.println("abcabc".replace("a", "A")); // AbcAbc
```

El primer ejemplo utiliza la primera firma de método, pasando parámetros `char`. El segundo ejemplo utiliza la segunda firma de método, pasando parámetros `String` (que implementan `CharSequence`).

### Eliminación de espacios en blanco (_Whitespace_)

Estos métodos eliminan espacios en blanco del principio y/o final de un `String`. Los métodos `strip()` y `trim()` eliminan los espacios en blanco del principio y del final de un `String`. Para el examen, los espacios en blanco consisten en espacios, junto con los caracteres `\t` (tabulación) y `\n` (nueva línea). Otros caracteres, como `\r` (retorno de carro), también se incluyen en lo que se recorta. El método `strip()` hace todo lo que hace `trim()`, pero además **soporta Unicode**.

No es necesario saber sobre Unicode para el examen. Pero si se desea probar la diferencia, uno de los caracteres de espacio en blanco Unicode es el siguiente:

```Java
char ch = '\u2000';
```

Adicionalmente, el método `stripLeading()` elimina los espacios en blanco del principio del `String` y los deja al final. El método `stripTrailing()` hace lo contrario. Elimina los espacios en blanco del final del `String` y los deja al principio. Las firmas de los métodos son las siguientes:

```Java
public String strip()
public String stripLeading()
public String stripTrailing()
public String trim()
```

El siguiente código muestra cómo usar estos métodos:

```Java
System.out.println("abc".strip());             // abc
System.out.println("\t   a b c\n".strip());    // a b c

String text = " abc\t ";
System.out.println(text.trim().length());          // 3
System.out.println(text.strip().length());         // 3
System.out.println(text.stripLeading().length());  // 5
System.out.println(text.stripTrailing().length()); // 4
```

Primero, recuerde que `\t` es un solo carácter. La barra invertida escapa la 't' para representar una tabulación. El primer ejemplo imprime la cadena original porque no hay caracteres de espacio en blanco ni al principio ni al final. El segundo ejemplo se deshace de la tabulación inicial, los espacios posteriores y la nueva línea final. Deja los espacios que están en el medio de la cadena.

Los ejemplos restantes solo imprimen la cantidad de caracteres que quedan. Se puede observar que `trim()` y `strip()` dejan los mismos tres caracteres `"abc"` porque eliminan los espacios en blanco tanto del principio como del final. El método `stripLeading()` solo elimina el carácter de espacio en blanco del principio del `String`. Deja la tabulación y el espacio al final. El método `stripTrailing()` elimina estos dos caracteres al final, pero deja el carácter al principio del `String`.

### Trabajo con indentación

Ahora que Java soporta bloques de texto (_text blocks_), es útil tener métodos que manejen la indentación. Ambos métodos son un poco engañosos, así que ¡lea con atención!

```Java
public String indent(int numberSpaces)
public String stripIndent()
```

El método `indent()` añade la cantidad especificada de espacios en blanco al principio de cada línea si se pasa un número positivo. Si se pasa un número negativo, intenta eliminar esa cantidad de caracteres de espacio en blanco del principio de la línea. Si se pasa cero, la indentación no cambiará.

Esto parece bastante sencillo. Sin embargo, `indent()` también **normaliza** los caracteres de espacio en blanco. ¿Qué significa normalizar los espacios en blanco? Primero, se añade un salto de línea al final de la cadena si aún no está allí. Segundo, cualquier salto de línea se convierte al formato `\n`. Independientemente de si el sistema operativo utiliza `\r\n` (Windows) o `\n` (Mac/Unix), Java estandarizará en `\n`.

El método `stripIndent()` es útil cuando un `String` se construyó con concatenación en lugar de usar un bloque de texto. Se deshace de todos los espacios en blanco incidentales (_incidental whitespace_). Esto significa que todas las líneas que no están en blanco se desplazan hacia la izquierda de modo que se elimina la misma cantidad de caracteres de espacio en blanco de cada línea, y el primer carácter que permanece no está en blanco. Al igual que `indent()`, `\r\n` se convierte en `\n`. Sin embargo, el método `stripIndent()` **no** añade un salto de línea final si falta.

Bueno, esas fueron muchas reglas. La Tabla 4.1 proporciona una referencia para facilitar su memorización.

**TABLA 4.1** Reglas para `indent()` y `stripIndent()`

|**Método**|**Cambio de indentación**|**Normaliza saltos de línea**|**Añade salto de línea al final si falta**|
|---|---|---|---|
|`indent(n)` donde $n > 0$|Añade $n$ espacios al principio de cada línea|Sí|Sí|
|`indent(n)` donde $n == 0$|Sin cambios|Sí|Sí|
|`indent(n)` donde $n < 0$|Elimina hasta $n$ espacios de cada línea (se elimina la misma cantidad de cada línea no en blanco)|Sí|Sí|
|`stripIndent()`|Elimina todos los espacios en blanco incidentales iniciales|Sí|**No**|

El siguiente código muestra cómo usar estos métodos. No hay de qué preocuparse si los resultados no son los esperados; se explicará cada uno.

```Java
10: var block = """
11: a
12:  b
13: c""";
14: var concat = " a\n"
15:            + "   b\n"
16:            + " c";
17: System.out.println(block.length());               // 6
18: System.out.println(concat.length());              // 9
19: System.out.println(block.indent(1).length());     // 10
20: System.out.println(concat.indent(-1).length());   // 7
21: System.out.println(concat.indent(-4).length());   // 6
22: System.out.println(concat.stripIndent().length());// 6
```

Las líneas 10-16 crean cadenas similares utilizando un bloque de texto y un `String` regular, respectivamente. Se dice "similares" porque `concat` tiene un carácter de espacio en blanco al principio de cada línea, mientras que `block` no.

La línea 17 cuenta los seis caracteres en `block`, que son las tres letras, el espacio en blanco antes de la 'b', y el `\n` después de la 'a' y la 'b'. La línea 18 cuenta los nueve caracteres en `concat`, que son las tres letras, un espacio en blanco antes de la 'a', dos espacios en blanco antes de la 'b', un espacio en blanco antes de la 'c', y el `\n` después de la 'a' y la 'b'. Si no se entiende qué caracteres se están contando, solo se volverá más confuso.

En la línea 19, se le pide a Java que añada un solo espacio en blanco a cada una de las tres líneas en `block`. Sin embargo, la salida indica que se añadieron 4 caracteres en lugar de 3, ya que la longitud pasó de 6 a 10. Este carácter adicional misterioso es gracias a la normalización de la terminación de línea. Dado que el bloque de texto no tiene un salto de línea al final, ¡`indent()` añade uno!

En la línea 20, se elimina un carácter de espacio en blanco de cada una de las tres líneas de `concat`. Esto da una longitud de siete. Se comenzó con nueve, se eliminaron tres caracteres y se añadió una nueva línea normalizada final (que antes no estaba al final de la 'c').

En la línea 21, se le pide a Java que elimine cuatro caracteres de espacio en blanco de las mismas tres líneas. Dado que no hay cuatro caracteres de espacio en blanco, Java hace lo mejor que puede. Se elimina el espacio único que hay antes de 'a»' y 'c'. Se eliminan los dos espacios
que hay antes de 'b'.  La longitud de seis debería tener sentido aquí; se eliminó un carácter más aquí que en la línea 20.

Finalmente, la línea 22 utiliza el método `stripIndent()`. Todas las líneas tienen al menos un carácter de espacio en blanco. Dado que no todas tienen dos caracteres de espacio en blanco (el mínimo común es 1), el método se deshace de solo un carácter por línea. Dado que `stripIndent()` no añade una nueva línea al final, la longitud es seis, que es tres menos que los nueve originales.

Si se llama a `indent()` con un número negativo y se intenta eliminar más caracteres de espacio en blanco de los que están presentes al principio de la línea, Java eliminará todos los que pueda encontrar.
### Comprobación de cadenas vacías o en blanco

Java proporciona métodos de conveniencia para determinar si un `String` tiene una longitud de cero o si contiene solo caracteres de espacio en blanco. Las firmas de los métodos son las siguientes:

```Java
public boolean isEmpty()
public boolean isBlank()
```

El siguiente código muestra cómo usar estos métodos:

```Java
System.out.println(" ".isEmpty()); // false
System.out.println("".isEmpty());  // true
System.out.println(" ".isBlank()); // true
System.out.println("".isBlank());  // true
```

La primera línea imprime `false` porque el `String` no está vacío; tiene un espacio en blanco en él. La segunda línea imprime `true` porque, esta vez, no hay caracteres en el `String`. Las dos últimas líneas imprimen `true` porque no hay caracteres distintos de los espacios en blanco presentes.

### Formateo de valores

Existen métodos para formatear valores `String` utilizando banderas de formato (_formatting flags_). Dos de los métodos toman la cadena de formato como parámetro, y el otro usa una instancia para ese valor. Un método toma un `Locale`, que se aprenderá en el Capítulo 11.

Los parámetros del método se utilizan para construir un `String` formateado en una sola llamada de método, en lugar de a través de muchas operaciones de formato y concatenación. Devuelven una referencia a la instancia sobre la que se invocan, de modo que las operaciones se pueden encadenar. Las firmas de los métodos son las siguientes:

```Java
public static String format(String format, Object... args)
public static String format(Locale loc, String format, Object... args)
public String formatted(Object... args)
```

El siguiente código muestra cómo usar estos métodos:

```Java
var name = "Kate";
var orderId = 5;

// Todos imprimen: Hello Kate, order 5 is ready
System.out.println("Hello " + name + ", order " + orderId + " is ready");
System.out.println(String.format("Hello %s, order %d is ready", name, orderId));
System.out.println("Hello %s, order %d is ready".formatted(name, orderId));
```

En las operaciones `format()` y `formatted()`, los parámetros se insertan y formatean a través de símbolos en el orden en que se proporcionan en el _vararg_ (argumentos variables). La Tabla 4.2 enumera los que se deben conocer para el examen.

**TABLA 4.2** Símbolos de formateo comunes

|**Símbolo**|**Descripción**|
|---|---|
|`%s`|Se aplica a cualquier tipo, comúnmente valores `String`|
|`%d`|Se aplica a valores enteros como `int` y `long`|
|`%f`|Se aplica a valores de punto flotante como `float` y `double`|
|`%n`|Inserta un salto de línea utilizando el separador de línea dependiente del sistema|

El siguiente ejemplo utiliza los cuatro símbolos de la Tabla 4.2:

```Java
var name = "James";
var score = 90.25;
var total = 100;

System.out.println("%s:%n  Score: %f out of %d".formatted(name, score, total));
```

Esto imprime lo siguiente:

```Plaintext
James:
  Score: 90.250000 out of 100
```

Mezclar tipos de datos puede causar excepciones en tiempo de ejecución. Por ejemplo, lo siguiente lanza una excepción porque se utiliza un número de punto flotante cuando se espera un valor entero (`%d`):

```Java
var str = "Food: %d tons".formatted(2.0); // IllegalFormatConversionException
```

### Uso de `format()` con banderas (_Flags_)

Además de soportar símbolos, Java también soporta **banderas opcionales** (_flags_) entre el `%` y el carácter del símbolo. En el ejemplo anterior, el número de punto flotante se imprimió como `90.250000`. Por defecto, `%f` muestra exactamente seis dígitos después del decimal. Si se desea mostrar solo un dígito después del decimal, se puede utilizar `%.1f` en lugar de `%f`. El método `format()` se basa en el **redondeo** en lugar del truncamiento al acortar números. Por ejemplo, `90.250000` se mostrará como `90.3` (no `90.2`) cuando se pase a `format()` con `%.1f`.

El método `format()` también admite dos características adicionales. Puedes especificar la longitud total de la salida utilizando un número antes del símbolo decimal. Por defecto, el método rellenará el espacio vacío con espacios en blanco. También puedes rellenar el espacio vacío con ceros colocando un solo cero antes del símbolo decimal. 

Los siguientes ejemplos utilizan corchetes, `[]`, para mostrar el inicio/fin del valor formateado:

```Java
var pi = 3.14159265359;
System.out.format("[%f]", pi);       // [3.141593]
System.out.format("[%12.8f]", pi);   // [  3.14159265]
System.out.format("[%012f]", pi);    // [00003.141593]
System.out.format("[%12.2f]", pi);   // [        3.14]
System.out.format("[%.3f]", pi);     // [3.142]
```

El método `format()` soporta muchos otros símbolos y banderas. No es necesario conocer ninguno de ellos para el examen más allá de lo que ya se ha discutido.

### Encadenamiento de métodos (_Method Chaining_)

Es momento de juntar todo lo aprendido. Es común llamar a múltiples métodos, como se muestra a continuación:

```Java
var start = "AniMaL ";
var trimmed = start.trim();               // "AniMaL"
var lowercase = trimmed.toLowerCase();    // "animal"
var result = lowercase.replace('a', 'A'); // "AnimAl"
System.out.println(result);
```

Esta es simplemente una serie de métodos de `String`. Cada vez que se llama a uno, el valor devuelto se coloca en una nueva variable. A lo largo del camino hay cuatro valores `String`, y se emite `AnimAl`.

Sin embargo, en el examen, existe la tendencia de agrupar tanto código como sea posible en un espacio pequeño. Se verá código utilizando una técnica llamada **encadenamiento de métodos** (_method chaining_). Aquí hay un ejemplo:

```Java
String result = "AniMaL ".trim().toLowerCase().replace('a', 'A');
System.out.println(result);
```

Este código es equivalente al ejemplo anterior. También crea cuatro objetos `String` y emite `AnimAl`. Para leer código que utiliza el encadenamiento de métodos, se debe comenzar por la izquierda y evaluar el primer método. Luego, se llama al siguiente método sobre el valor devuelto por el primer método. Se debe continuar así hasta llegar al punto y coma.

¿Cuál se cree que es el resultado de este código?

```Java
5: String a = "abc";
6: String b = a.toUpperCase();
7: b = b.replace("B", "2").replace('C', '3');
8: System.out.println("a=" + a);
9: System.out.println("b=" + b);
```

En la línea 5, se hace que `a` apunte a `"abc"` y nunca más se hace que `a` apunte a otra cosa. Dado que nada del código en las líneas 6 y 7 cambia a `a`, el valor permanece como `"abc"` (debido a la inmutabilidad de los `String`).

Sin embargo, `b` es un poco más engañoso. La línea 6 hace que `b` apunte a `"ABC"`, lo cual es directo. En la línea 7, se tiene un encadenamiento de métodos. Primero, se llama a `"ABC".replace("B", "2")`. Esto devuelve `"A2C"`. A continuación, se llama a `"A2C".replace('C', '3')`. Esto devuelve `"A23"`. Finalmente, `b` cambia para apuntar a este `String` devuelto. Cuando se ejecuta la línea 9, `b` es `"A23"`.