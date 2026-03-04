El último conjunto de operadores binarios gira en torno a la comparación de valores. Se pueden utilizar para verificar si dos valores son iguales, comprobar si un valor numérico es menor o mayor que otro, y realizar aritmética booleana. Es probable que se hayan utilizado muchos de los operadores de esta sección en la experiencia de desarrollo.

### Operadores de igualdad

Determinar la igualdad en Java puede ser una tarea no trivial, ya que existe una diferencia semántica entre «dos objetos son los mismos» y «dos objetos son equivalentes». Esto se complica aún más por el hecho de que para los primitivos numéricos y booleanos, no existe tal distinción.

La Tabla 2.7 enumera los operadores de igualdad. El operador de **igualdad** (`==`) y el operador de **desigualdad** (`!=`) comparan dos operandos y devuelven un valor booleano determinando si las expresiones o valores son iguales o desiguales, respectivamente.

**TABLA 2.7** Operadores de igualdad

|**Operador**|**Ejemplo**|**Aplicar a primitivos**|**Aplicar a objetos**|
|---|---|---|---|
|**Igualdad**|`a == 10`|Devuelve `true` si los dos valores representan el mismo valor.|Devuelve `true` si los dos valores hacen referencia al **mismo** objeto.|
|**Desigualdad**|`b != 3.14`|Devuelve `true` si los dos valores representan valores diferentes.|Devuelve `true` si los dos valores **no** hacen referencia al mismo objeto.|

El operador de igualdad puede aplicarse a valores numéricos, valores booleanos y objetos (incluyendo `String` y `null`). Al aplicar el operador de igualdad, no se pueden mezclar estos tipos. Cada uno de los siguientes resulta en un error de compilación:

```Java
boolean monkey = true == 3; // NO COMPILA
boolean ape = false != "Grape"; // NO COMPILA
boolean gorilla = 10.2 == "Koko"; // NO COMPILA
```

Se debe prestar mucha atención a los tipos de datos cuando se observe un operador de igualdad en el examen. Como se mencionó en la sección anterior, los creadores del examen también tienen la costumbre de mezclar operadores de asignación y operadores de igualdad.

```Java
boolean bear = false;
boolean polar = (bear = true);
System.out.println(polar); // true
```

A primera vista, podría pensarse que la salida debería ser `false`, y si la expresión fuera `(bear == true)`, entonces se estaría en lo correcto. Sin embargo, en este ejemplo, la expresión está asignando el valor de `true` a `bear`, y como se vio en la sección sobre operadores de asignación, la asignación misma tiene el valor de la asignación. Por lo tanto, a `polar` también se le asigna un valor de `true`, y la salida es `true`.

Para la comparación de objetos, el operador de igualdad se aplica a las referencias a los objetos, no a los objetos a los que apuntan. Dos referencias son iguales si y solo si apuntan al mismo objeto o ambas apuntan a `null`. Obsérvense algunos ejemplos:

```Java
var monday = new File("schedule.txt");
var tuesday = new File("schedule.txt");
var wednesday = tuesday;
System.out.println(monday == tuesday); // false
System.out.println(tuesday == wednesday); // true
```

Aunque todas las variables apuntan a la misma información de archivo, solo dos referencias, `tuesday` y `wednesday`, son iguales en términos de `==` ya que apuntan al mismo objeto.

En algunos lenguajes, comparar `null` con cualquier otro valor es siempre `false`, aunque este no es el caso en Java.

```Java
System.out.print(null == null); // true
```

En el Capítulo 4, se continuará la discusión sobre la igualdad de objetos introduciendo lo que significa que dos objetos diferentes sean equivalentes. También se cubrirá la igualdad de `String` y se mostrará cómo esto puede ser un tema no trivial.

¿Qué es la clase `File`? En este ejemplo, así como durante el examen, pueden presentarse nombres de clases que no resulten familiares, como `File`. Muchas veces se pueden responder preguntas sobre estas clases sin conocer sus detalles específicos. En el ejemplo anterior, se debería ser capaz de responder preguntas que indiquen que `monday` y `tuesday` son dos objetos separados y distintos porque se utiliza la palabra clave `new`, incluso si no se está familiarizado con los tipos de datos de estos objetos.
### Operadores relacionales

Ahora se pasa a los operadores relacionales, los cuales comparan dos expresiones y devuelven un valor booleano. La Tabla 2.8 describe los operadores relacionales que es necesario conocer para el examen.

**TABLA 2.8** Operadores relacionales

| **Operador**            | **Ejemplo**           | **Descripción**                                                                                                                            |
| ----------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Menor que**           | `a < 5`               | Devuelve `true` si el valor de la izquierda es estrictamente menor que el valor de la derecha.                                             |
| **Menor o igual que**   | `b <= 6`              | Devuelve `true` si el valor de la izquierda es menor o igual al valor de la derecha.                                                       |
| **Mayor que**           | `c > 9`               | Devuelve `true` si el valor de la izquierda es estrictamente mayor que el valor de la derecha.                                             |
| **Mayor o igual que**   | `3 >= d`              | Devuelve `true` si el valor de la izquierda es mayor o igual al valor de la derecha.                                                       |
| **Comparación de tipo** | `e instanceof String` | Devuelve `true` si la referencia del lado izquierdo es una instancia del tipo del lado derecho (clase, interfaz, record, enum, anotación). |

### Operadores de comparación numérica

Los primeros cuatro operadores relacionales en la Tabla 2.8 se aplican solo a valores numéricos. Si los dos operandos numéricos no son del mismo tipo de datos, el más pequeño se promueve, como se discutió anteriormente.

Observe ejemplos de estos operadores en acción:

```Java
int gibbonNumFeet = 2, wolfNumFeet = 4, ostrichNumFeet = 2;
System.out.println(gibbonNumFeet < wolfNumFeet); // true
System.out.println(gibbonNumFeet <= wolfNumFeet); // true
System.out.println(gibbonNumFeet >= ostrichNumFeet); // true
System.out.println(gibbonNumFeet > ostrichNumFeet); // false
```

Note que el último ejemplo imprime `false`, porque aunque `gibbonNumFeet` y `ostrichNumFeet` tienen el mismo valor, `gibbonNumFeet` no es estrictamente mayor que `ostrichNumFeet`.

### Operador `instanceof`

El operador relacional final que se necesita conocer para el examen es el operador `instanceof`, mostrado en la Tabla 2.8. Es útil para determinar si un objeto arbitrario es miembro de una clase o interfaz particular en tiempo de ejecución.

¿Por qué no se sabría de qué clase o interfaz es un objeto? Como se verá en el Capítulo 6, «Diseño de clases», Java soporta polimorfismo. Por ahora, todo lo que se necesita saber es que los objetos pueden pasarse utilizando una variedad de referencias. Por ejemplo, todas las clases heredan de `java.lang.Object`. Esto significa que cualquier instancia puede asignarse a una referencia `Object`. Por ejemplo, ¿cuántos objetos se crean y utilizan en el siguiente fragmento de código?

```Java
Integer zooTime = Integer.valueOf(9);
Number num = zooTime;
Object obj = zooTime;
```

En este ejemplo, solo se crea un objeto en memoria, pero hay tres referencias diferentes a él porque `Integer` hereda tanto de `Number` como de `Object`. Esto significa que se puede llamar a `instanceof` en cualquiera de estas referencias con tres tipos de datos diferentes, y devolverá `true` para cada uno de ellos.

Donde el polimorfismo entra en juego a menudo es cuando se crea un método que toma un tipo de dato con muchas subclases posibles. Por ejemplo, imagínese que se tiene una función que abre el zoológico e imprime la hora. Como entrada, toma un `Number` como parámetro.

```Java
public void openZoo(Number time) {}
```

Ahora, se desea que la función agregue "O'clock" al final de la salida si el valor es un tipo de número entero, como un `Integer`; de lo contrario, simplemente imprime el valor.

```Java
public void openZoo(Number time) {
    if (time instanceof Integer)
        System.out.print((Integer)time + " O'clock");
    else
        System.out.print(time);
}
```

Ahora se tiene un método que puede manejar inteligentemente tanto `Integer` como otros valores. Un buen ejercicio es agregar comprobaciones para otros tipos de datos numéricos como `Short`, `Long`, `Double`, etc.

Note que se convirtió (_cast_) el valor `Integer` en este ejemplo. Es común usar _casting_ con `instanceof` al trabajar con objetos que pueden ser de varios tipos diferentes, ya que el _casting_ da acceso a campos disponibles solo en las clases más específicas. Se considera una buena práctica de codificación usar el operador `instanceof` antes de convertir (_casting_) un objeto a un tipo más estrecho.

Para el examen, solo es necesario enfocarse en cuándo se usa `instanceof` con clases e interfaces. Aunque puede usarse con otros tipos de alto nivel, como _records_, _enums_ y anotaciones, no es común.

### `instanceof` inválido

Un área en la que el examen podría intentar confundir es el uso de `instanceof` con tipos incompatibles. Por ejemplo, `Number` no puede contener posiblemente un valor `String`, por lo que lo siguiente causa un error de compilación:

```Java
public void openZoo(Number time) {
    if(time instanceof String) // NO COMPILA
        System.out.print(time);
}
```

Si el compilador puede determinar que una variable no puede ser posiblemente convertida (_cast_) a una clase específica, reporta un error.

### `null` y el operador `instanceof`

¿Qué sucede si se llama a `instanceof` en una variable `null`? Para el examen, se debe saber que llamar a `instanceof` en el literal `null` o en una referencia `null` siempre devuelve `false`.

```Java
System.out.print(null instanceof Object); // false
String noObjectHere = null;
System.out.print(noObjectHere instanceof String); // false
```

Los ejemplos anteriores imprimen ambos `false`. Casi no importa qué haya en el lado derecho de la expresión. Se dice «casi» porque hay excepciones. Este ejemplo no compila, ya que `null` se usa en el lado derecho del operador `instanceof`:

```Java
System.out.print(null instanceof null); // NO COMPILA
```

Aunque pueda parecer que se ha aprendido todo sobre el operador `instanceof`, ¡hay mucho más por venir! En el Capítulo 3, se introduce el coincidencia de patrones (_pattern matching_) con el operador `instanceof`, y en el Capítulo 7, «Más allá de las clases», se aplica a patrones de registros (_record patterns_) y también se ve cómo impacta en el polimorfismo.
### Operadores lógicos

Si se ha estudiado ciencias de la computación, es posible que ya se hayan encontrado los operadores lógicos con anterioridad. Si no, no hay motivo de pánico: se cubrirán en detalle en esta sección.

Los operadores lógicos, `&`, `|` y `^`, pueden aplicarse tanto a tipos de datos numéricos como booleanos; se enumeran en la Tabla 2.9. Cuando se aplican a tipos de datos booleanos, se denominan **operadores lógicos**. Alternativamente, cuando se aplican a tipos de datos numéricos, se denominan **operadores a nivel de bits** (_bitwise_), ya que realizan comparaciones bit a bit de los bits que componen el número.

**TABLA 2.9** Operadores lógicos

| **Operador**                  | **Ejemplo** | **Descripción**                                                     |
| ----------------------------- | ----------- | ------------------------------------------------------------------- |
| **AND lógico**                | `a & b`     | El valor es `true` solo si **ambos** valores son verdaderos.        |
| **OR inclusivo lógico**       | `c`\|`d`    | El valor es `true` si al menos uno de los valores es verdadero.     |
| **OR exclusivo lógico (XOR)** | `e ^ f`     | El valor es `true` solo si **uno** es verdadero y el otro es falso. |

Se recomienda familiarizarse con las tablas de `true` de la siguiente figura, donde se asume que `x` e `y` son tipos de datos booleanos.

![[Las tablas de true lógica.png]]

Aquí hay algunos consejos para ayudar a recordar esta tabla:

- **AND** es verdadero solo si ambos operandos son verdaderos.
- **OR inclusivo** es falso solo si ambos operandos son falsos.
- **OR exclusivo (XOR)** es verdadero solo si los operandos son diferentes.

Se examinarán algunos ejemplos:

```Java
boolean eyesClosed = true;
boolean breathingSlowly = true;
boolean resting = eyesClosed | breathingSlowly;
boolean asleep = eyesClosed & breathingSlowly;
boolean awake = eyesClosed ^ breathingSlowly;
System.out.println(resting); // true
System.out.println(asleep); // true
System.out.println(awake); // false
```

Se deberían probar estos ejemplos por cuenta propia, cambiando los valores de `eyesClosed` y `breathingSlowly` y estudiando los resultados.

### Operadores a nivel de bits (_Bitwise Operators_)

Los bits son los 0 y 1 que se verían si se observara un número en binario. Por ejemplo, el número 2 se representa como `10` en binario. La Tabla 2.10 incluye tres operaciones a nivel de bits que comparan los ceros y unos de un número, y devuelven un nuevo número basado en estas comparaciones. Para el examen, no es necesario realizar muchas conversiones a nivel de bits, pero sí es necesario conocer algunos conceptos básicos.

**TABLA 2.10** Operadores a nivel de bits

| **Operador**                           | **Ejemplo** | **Descripción**                                                                                                                                                        |
| -------------------------------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **AND a nivel de bits**                | `a & b`     | Compara los bits de dos números, devolviendo un número que tiene un 1 en cada dígito en el que **ambos** operandos tienen un 1, y 0 en caso contrario.                 |
| **OR a nivel de bits**                 | `c`\|`d`    | Compara los bits de dos números y devuelve un número que tiene un 1 en cada **dígito** en el que **cualquiera** de los operandos tiene un 1, y un 0 en caso contrario. |
| **OR exclusivo a nivel de bits (XOR)** | `e ^ f`     | Compara los bits de dos números, devolviendo un número que tiene un 0 en cada dígito que coincidió, y 1 en caso contrario.                                             |

Primero, se tienen el operador **AND a nivel de bits** (`&`) y el operador **OR a nivel de bits** (`|`). Estos operadores devuelven 1 cuando ambos o cualquiera de los dígitos binarios correspondientes son 1, respectivamente. En primer lugar, se debe saber que se devuelve el número original si ambos operandos son iguales.

```Java
int number = 70;
System.out.println(number); // 70
System.out.println(number & number); // 70
System.out.println(number | number); // 70
```

También es útil entender las operaciones entre un número y su **complemento (NOT)**:

1. Con el operador **AND (`&`)**, el resultado siempre es **0** (porque no comparten bits activos).
2. Con el operador **OR (`|`)**, el resultado es **-1** (porque todos los bits se activan, lo que equivale a -1 en representación de complemento a dos)."

```Java
int negated = ~number;
System.out.println(negated); // -71
System.out.println(number & negated); // 0
System.out.println(number | negated); // -1
```

Finalmente, se tiene el operador **OR exclusivo binario** (`^`). Funciona como la versión booleana, excepto con 0 como falso y 1 como verdadero. Sin embargo, comprueba en cada posición del número. Se debe saber que devuelve 0 si ambos números son iguales (todos los bits son 0s), y -1 (todos los bits son 1s) para un valor con su negación.

```Java
System.out.println(number ^ number); // 0
System.out.println(number ^ negated); // -1
```

### Operadores condicionales

A continuación, se presentan los operadores condicionales, `&&` y `||`, en la Tabla 2.11.

**TABLA 2.11** Operadores condicionales

| **Operador**        | **Ejemplo** | **Descripción**                                                                                                                              |
| ------------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **AND Condicional** | `a && b`    | El valor es `true` solo si ambos valores son verdaderos. Si el lado izquierdo es `false`, entonces el lado derecho **no se evaluará**.       |
| **OR Condicional**  | `c`\|\|`d`  | El valor es `true` si al menos uno de los valores es verdadero. Si el lado izquierdo es `true`, entonces el lado derecho **no se evaluará**. |

Los operadores condicionales, a menudo llamados **operadores de cortocircuito** (_short-circuit operators_), son casi idénticos a los operadores lógicos `&` y `|`, excepto que el lado derecho de la expresión puede no evaluarse nunca si el resultado final puede determinarse por el lado izquierdo de la expresión. Por ejemplo, considere el siguiente fragmento:

```Java
int hour = 10;
boolean zooOpen = true || (hour < 4);
System.out.println(zooOpen); // true
```

Refiriéndose a las tablas de verdad, el valor `zooOpen` puede ser `false` solo si ambos lados de la expresión son falsos. Dado que se sabe que el lado izquierdo es `true`, no hay necesidad de evaluar el lado derecho, ya que ningún valor de `hour` hará que este código imprima `false`. En otras palabras, `hour` podría haber sido -10 u 892; la salida habría sido la misma. ¡Intente con diferentes valores para `hour`!

### Cómo evitar una `NullPointerException`

Un ejemplo más común de dónde se utilizan los operadores condicionales es la comprobación de objetos nulos antes de realizar una operación. En el siguiente ejemplo, si `duck` es `null`, el programa lanzará una `NullPointerException` en tiempo de ejecución:

```Java
if(duck != null & duck.getAge() < 5) { // Podría lanzar una NullPointerException
    // Hacer algo
}
```

El problema es que el operador lógico AND (`&`) evalúa **ambos** lados de la expresión. Se podría agregar una segunda declaración `if`, pero esto podría volverse difícil de manejar si hay muchas variables para verificar. una solución fácil de leer es usar el operador AND condicional (`&&`):

```Java
if(duck != null && duck.getAge() < 5) {
    // Hacer algo
}
```

En este ejemplo, si `duck` es `null`, el condicional evita que se lance una `NullPointerException`, ya que la evaluación de `duck.getAge() < 5` nunca se alcanza.

### Verificación de efectos secundarios no realizados

Se debe tener cuidado con el comportamiento de cortocircuito en el examen, ya que se sabe que las preguntas alteran una variable en el lado derecho de la expresión que puede no alcanzarse nunca. Esto se conoce como un **efecto secundario no realizado**. Por ejemplo, ¿cuál es la salida del siguiente código?

```Java
int rabbit = 6;
boolean bunny = (rabbit >= 6) || (++rabbit <= 7);
System.out.println(rabbit);
```

Debido a que `rabbit >= 6` es `true`, el operador de incremento en el lado derecho de la expresión nunca se evalúa, por lo que la salida es 6.