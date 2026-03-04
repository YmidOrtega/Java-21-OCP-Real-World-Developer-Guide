Por definición, un **operador unario** es aquel que requiere exactamente un operando, o variable, para funcionar. Como se muestra en la Tabla 2.2, a menudo realizan tareas simples, como incrementar una variable numérica en uno o negar un valor booleano.

**TABLA 2.2** Operadores unarios

|**Operador**|**Ejemplos**|**Descripción**|
|---|---|---|
|**Complemento lógico**|`!a`|Invierte el valor lógico de un booleano.|
|**Complemento a nivel de bits**|`~b`|Invierte todos los ceros y unos en un número.|
|**Más (Plus)**|`+c`|Indica que un número es positivo, aunque se asume que los números son positivos en Java a menos que vayan acompañados de un operador unario negativo.|
|**Negación o menos**|`-d`|Indica que un número literal es negativo o niega una expresión.|
|**Incremento**|`++e`, `f++`|Incrementa un valor en 1.|
|**Decremento**|`--f`, `h--`|Decrementa un valor en 1.|
|**Conversión (Cast)**|`(String) i`|Convierte (_casts_) un valor a un tipo específico.|

Aunque la Tabla 2.2 incluye el operador de conversión (_casting_), se pospone la discusión sobre la conversión hasta la sección «Asignación de valores» más adelante en este capítulo, ya que es donde se utiliza comúnmente.

### Operadores de complemento y negación

El operador de **complemento lógico** (`!`) invierte el valor de una expresión booleana. Por ejemplo, si el valor es `true`, se convertirá a `false`, y viceversa. Para ilustrar esto, compare las salidas de las siguientes sentencias:

```Java
boolean isAnimalAsleep = false;
System.out.print(isAnimalAsleep); // false
isAnimalAsleep = !isAnimalAsleep;
System.out.print(isAnimalAsleep); // true
```

A continuación, el operador de **negación a nivel de bits** (`~`) convierte todos los ceros en unos y viceversa. Se puede calcular el nuevo valor negando el original y restando uno. Por ejemplo:

```Java
int number = 70;
int negated = ~number;
System.out.println(negated); // -71
System.out.println(~negated); // 70
```

El operador `~` siempre equivale matemáticamente a: $~n = -(n + 1)$. 

Luego, el operador de **negación** (`-`) invierte el signo de una expresión numérica, como se muestra en estas sentencias:

```Java
double zooTemperature = 1.21;
System.out.println(zooTemperature); // 1.21
zooTemperature = -zooTemperature;
System.out.println(zooTemperature); // -1.21
zooTemperature = -(-zooTemperature);
System.out.println(zooTemperature); // -1.21
```

Observe que en el ejemplo anterior se utilizaron paréntesis, `()`, para el operador de negación, `-`, para aplicar la negación dos veces. Si en su lugar se hubiera escrito `--`, entonces se habría interpretado como el operador de decremento e impreso `-2.21`. Se verá más sobre ese operador de decremento en breve.

Basándose en la descripción, podría resultar obvio que algunos operadores requieren que la variable o expresión sobre la que actúan sea de un tipo específico. Por ejemplo, no se puede aplicar un operador de negación (`-`) a una expresión booleana, ni se puede aplicar un operador de complemento lógico (`!`) a una expresión numérica. Se debe tener cuidado con las preguntas en el examen que intenten hacer esto, ya que provocan que el código falle al compilar. Por ejemplo, ninguna de las siguientes líneas de código compilará:

```Java
int pelican = !5; // NO COMPILA
boolean penguin = -true; // NO COMPILA
boolean parrot = ~true; // NO COMPILA
boolean peacock = !0; // NO COMPILA
```

La primera sentencia no compilará porque en Java no se puede realizar una inversión lógica de un valor numérico. La segunda y tercera sentencia no compilan porque no se puede negar numéricamente ni complementar un valor booleano; es necesario utilizar el operador inverso lógico. Finalmente, la última sentencia no compila porque no se puede tomar el complemento lógico de un valor numérico, ni se puede asignar un entero a una variable booleana.

Se debe prestar atención a las preguntas del examen que utilizan valores numéricos (como 0 o 1) con expresiones booleanas. A diferencia de otros lenguajes de programación, en Java, 1 y `true` no están relacionados de ninguna manera, así como 0 y `false` tampoco lo están.

### Operadores de incremento y decremento

Los operadores de incremento y decremento, `(++)` y `(--)` respectivamente, pueden aplicarse a variables numéricas y tienen un alto orden de precedencia en comparación con los operadores binarios. En otras palabras, a menudo se aplican primero en una expresión.

Los operadores de incremento y decremento requieren un cuidado especial porque el orden en el que se adjuntan a su variable asociada puede marcar la diferencia en cómo se procesa una expresión. La Tabla 2.3 enumera cada uno de estos operadores.

**TABLA 2.3** Operadores de incremento y decremento

|**Operador**|**Ejemplo**|**Descripción**|
|---|---|---|
|**Pre-incremento**|`++w`|Aumenta el valor en 1 y devuelve el **nuevo** valor.|
|**Pre-decremento**|`--x`|Disminuye el valor en 1 y devuelve el **nuevo** valor.|
|**Post-incremento**|`y++`|Aumenta el valor en 1 y devuelve el valor **original**.|
|**Post-decremento**|`z--`|Disminuye el valor en 1 y devuelve el valor **original**.|

El siguiente fragmento de código ilustra esta distinción:

```Java
int parkAttendance = 0;
System.out.println(parkAttendance); // 0
System.out.println(++parkAttendance); // 1
System.out.println(parkAttendance); // 1
System.out.println(parkAttendance--); // 1
System.out.println(parkAttendance); // 0
```

El primer operador de pre-incremento actualiza el valor de `parkAttendance` y muestra el nuevo valor de 1. El siguiente operador de post-decremento también actualiza el valor de `parkAttendance`, pero muestra el valor antes de que ocurra el decremento.

Para el examen, es crítico conocer la diferencia entre expresiones como `parkAttendance++` y `++parkAttendance`. Los operadores de incremento y decremento aparecerán en múltiples preguntas, y la confusión sobre qué valor se devuelve podría causar la pérdida de muchos puntos en el examen.