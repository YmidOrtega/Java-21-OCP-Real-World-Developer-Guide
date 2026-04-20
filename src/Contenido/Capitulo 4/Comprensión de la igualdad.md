En el Capítulo 2, se aprendió cómo utilizar `==` para comparar números y que las referencias a objetos apuntan al mismo objeto. Anteriormente en este capítulo, se vio el método `equals()` en `String`. En esta sección, se analiza qué significa que dos objetos sean equivalentes o iguales. También se examina el impacto del _String pool_ (grupo de cadenas) en la igualdad.

### Comparación entre `equals()` y `==`

Considere el siguiente código que utiliza `==` con objetos:

```Java
var one = new StringBuilder();
var two = new StringBuilder();
var three = one.append("a");
System.out.println(one == two);   // false
System.out.println(one == three); // true
```

Dado que este ejemplo no trata con primitivos, se sabe que se debe buscar si las referencias están apuntando al mismo objeto. Las variables `one` y `two` son objetos `StringBuilder` completamente separados, lo que da como resultado dos objetos. Por lo tanto, la primera sentencia de impresión da `false`. La variable `three` es más interesante. ¿Recuerda cómo a los métodos de `StringBuilder` les gusta devolver la referencia actual para permitir el encadenamiento? Esto significa que `one` y `three` apuntan al mismo objeto, y la segunda sentencia de impresión da `true`.

Se vio anteriormente que `equals()` utiliza igualdad lógica en lugar de igualdad de objetos (referencia) para los objetos `String`.

```Java
var x = "Hello World";
var z = " Hello World".trim();
System.out.println(x.equals(z)); // true
```

Esto funciona porque los autores de la clase `String` implementaron un método estándar llamado `equals()` para verificar los valores dentro del `String` en lugar de la referencia de la cadena en sí. Si una clase no tiene un método `equals()`, Java determina si las referencias apuntan al mismo objeto, que es exactamente lo que hace `==`.

En caso de duda, se debe saber que los autores de `StringBuilder` **no** implementaron `equals()`. Si se llama a `equals()` en dos instancias de `StringBuilder`, verificará la igualdad de referencia. En su lugar, se puede llamar a `toString()` en `StringBuilder` para obtener un `String` y verificar la igualdad lógica.

Finalmente, el examen podría intentar engañar con una pregunta como esta. ¿Se puede adivinar por qué el código no compila?

```Java
var name = "a";
var builder = new StringBuilder("a");
System.out.println(name == builder); // NO COMPILA
```

Recuerde que `==` verifica la igualdad de referencia de objetos. El compilador es lo suficientemente inteligente como para saber que dos referencias no pueden apuntar al mismo objeto cuando son de tipos completamente diferentes.

### El _String Pool_ (Grupo de cadenas)

Dado que las cadenas están en todas partes en Java, consumen mucha memoria. En algunas aplicaciones de producción, pueden utilizar una gran cantidad de memoria en todo el programa. Java reconoce que muchas cadenas se repiten en el programa y resuelve este problema reutilizando las más comunes. El _String pool_, también conocido como _intern pool_, es una ubicación en la Máquina Virtual de Java (JVM) que recopila todas estas cadenas.

El _String pool_ contiene valores literales y constantes que aparecen en el programa. Por ejemplo, `"name"` es un literal y, por lo tanto, va al _String pool_. El método `myObject.toString()` devuelve una cadena pero no un literal, por lo que **no** va al _String pool_.

Ahora se visitará el escenario más complejo y confuso: la igualdad de `String`, que en parte es así debido a la forma en que la JVM reutiliza los literales `String`.

```Java
var x = "Hello World";
var y = "Hello World";
System.out.println(x == y); // true
```

Recuerde que un `String` es inmutable y los literales se agrupan en el _pool_. La JVM creó solo un literal en la memoria. Las variables `x` e `y` apuntan a la misma ubicación en la memoria; por lo tanto, la sentencia emite `true`. Se vuelve aún más complicado. Considere este código:

```Java
var x = "Hello World";
var z = " Hello World".trim();
System.out.println(x == z); // false
```

En este ejemplo, no se tienen dos del mismo literal `String`. Aunque resulta que `x` y `z` se evalúan como la misma cadena lógica, una se calcula en **tiempo de ejecución** (`trim()`). Dado que no es la misma en tiempo de compilación, se crea un nuevo objeto `String`. Se probará con otra. ¿Qué se cree que se emite aquí?

```Java
var singleString = "hello world";
var concat = "hello ";
concat += "world";
System.out.println(singleString == concat); // false
```

Esto imprime `false`. Llamar a `+=` es como llamar a un método y da como resultado un nuevo `String`. Incluso se puede forzar la situación creando un nuevo `String` explícitamente:

```Java
var x = "Hello World";
var y = new String("Hello World");
System.out.println(x == y); // false
```

El primero indica que se utilice el _String pool_ normalmente. El segundo indica: "No, JVM, realmente no quiero que uses el _String pool_. Por favor, crea un nuevo objeto aunque sea menos eficiente".

También se puede hacer lo contrario y decirle a Java que use el _String pool_. El método `intern()` utilizará un objeto en el _String pool_ si hay uno presente.

```Java
public String intern()
```

Si el literal aún no está en el _String pool_, Java lo agregará en este momento.

```Java
var name = "Hello World";
var name2 = new String("Hello World").intern();
System.out.println(name == name2); // true
```

Primero se le dice a Java que use el _String pool_ normalmente para `name`. Luego, para `name2`, se le dice a Java que cree un nuevo objeto usando el constructor, pero que lo interne (_intern_) y use el _String pool_ de todos modos. Dado que ambas variables apuntan a la misma referencia en el _String pool_, se puede utilizar el operador `==`.

Pruebe con otro. ¿Qué se cree que imprime esto? Se debe tener cuidado. Es engañoso.

```Java
15: var first = "rat" + 1;
16: var second = "r" + "a" + "t" + "1";
17: var third = "r" + "a" + "t" + new String("1");
18: System.out.println(first == second);
19: System.out.println(first == second.intern());
20: System.out.println(first == third);
21: System.out.println(first == third.intern());
```

En la línea 15, se tiene una **constante en tiempo de compilación** que se coloca automáticamente en el _String pool_ como `"rat1"`. En la línea 16, se tiene una expresión más complicada que también es una constante en tiempo de compilación (ya que concatena puros literales). Por lo tanto, `first` y `second` comparten la misma referencia del _String pool_. Esto hace que las líneas 18 y 19 impriman `true`.

En la línea 17, se tiene un constructor `String`. Esto significa que ya no se tiene una constante en tiempo de compilación, y `third` no apunta a una referencia en el _String pool_. Por lo tanto, la línea 20 imprime `false`. En la línea 21, la llamada a `intern()` busca en el _String pool_. Java nota que `first` apunta al mismo `String` e imprime `true`.

Recuerde que **nunca** se debe usar `intern()` o `==` para comparar objetos `String` en el código de producción. En su lugar, se debe usar el método `equals()`. El único momento en que se debería tener que lidiar con estos es durante el examen de certificación.