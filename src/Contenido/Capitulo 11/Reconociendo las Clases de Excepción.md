Para el examen, **hay que reconocer** tres grupos de clases de excepción: `RuntimeException`, excepciones verificadas y `Error`. A continuación **se revisan** los ejemplos más comunes de cada tipo. Para el examen, **hay que ser capaz de reconocer** a qué tipo de excepción pertenece cada una y si **es lanzada** por la Máquina Virtual de Java (JVM) o por el programador. Para algunas excepciones, también **hay que saber** cuáles **están heredadas** unas de otras.

### Clases de `RuntimeException`

`RuntimeException` y sus subclases son excepciones no verificadas que no **tienen** que **ser manejadas** ni declaradas. Pueden **ser lanzadas** por el programador o por la JVM. En la Tabla 11.2 **se listan** las clases de excepción no verificada más comunes.

**TABLA 11.2** Excepciones no verificadas

| **Excepción no verificada**      | **Descripción**                                                                                                                                                 |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ArithmeticException`            | Se lanza cuando el código intenta dividir entre cero.                                                                                                           |
| `ArrayIndexOutOfBoundsException` | Se lanza cuando el código usa un índice ilegal para acceder a un array.                                                                                         |
| `ClassCastException`             | Se lanza cuando **se intenta** convertir (_cast_) un objeto a una clase de la cual no es instancia.                                                             |
| `NullPointerException`           | Se lanza cuando **existe** una referencia nula donde **se requiere** un objeto.                                                                                 |
| `IllegalArgumentException`       | Lanzada por el programador para indicar que **se le ha pasado** a un método un argumento ilegal o inapropiado.                                                  |
| `NumberFormatException`          | Subclase de `IllegalArgumentException`. Se lanza cuando **se intenta** convertir un `String` a un tipo numérico pero el `String` no tiene el formato apropiado. |

#### `ArithmeticException`

Dividir un `int` entre cero da como resultado un valor indefinido. Cuando esto ocurre, la JVM lanzará una `ArithmeticException`.

```Java
int answer = 11 / 0;
```

Ejecutar este código produce la siguiente salida:

```Plaintext
Exception in thread "main" java.lang.ArithmeticException: / by zero
```

Java no escribe la palabra dividir. No importa, porque sabemos que `/` es el operador de división y Java nos está intentando decir que **se produjo** una división por cero.

El `thread "main"` nos indica que el código **fue llamado** directa o indirectamente desde un programa con un método `main`. Para el examen, esta es toda la salida que **se verá**. Después **viene** el nombre de la excepción, seguido de la información adicional (si la hay) que acompaña a la excepción.

#### `ArrayIndexOutOfBoundsException`

Ya **se sabe** que los índices de array comienzan en 0 y llegan hasta uno menos que la longitud del array, lo que significa que este código lanzará una `ArrayIndexOutOfBoundsException`:

```Java
int[] countsOfMoose = new int[3];
System.out.println(countsOfMoose[-1]);
```

Esto es un problema porque no existen los índices de array negativos. Ejecutar este código produce la siguiente salida:

```Plaintext
Exception in thread "main"
java.lang.ArrayIndexOutOfBoundsException: Index -1 out of bounds for length 3
```

#### `ClassCastException`

Java intenta proteger al programador de las conversiones imposibles. Este código no compila porque `Integer` no es una subclase de `String`:

```Java
String type = "moose";
Integer number = (Integer) type; // NO COMPILA
```

El código más complicado frustra los intentos de protección de Java. Cuando la conversión falla en tiempo de ejecución, Java lanzará una `ClassCastException`.

```Java
String type = "moose";
Object obj = type;
Integer number = (Integer) obj; // ClassCastException
```

El compilador ve una conversión de `Object` a `Integer`. Esto podría estar bien. El compilador no se da cuenta de que hay un `String` dentro de ese `Object`. Cuando el código se ejecuta, produce la siguiente salida:

```Plaintext
Exception in thread "main" java.lang.ClassCastException:
java.base/java.lang.String cannot be cast to
java.base/java.lang.Integer
```

Java nos indica ambos tipos involucrados en el problema, haciendo evidente qué está mal.

#### `NullPointerException`

Las variables de instancia y los métodos **deben ser llamados** en una referencia no nula. Si la referencia es `null`, la JVM lanzará una `NullPointerException`.

```Java
public class Frog {
    static String name;
    public void hop() {
        System.out.print(name.toLowerCase() + " is hopping");
    }
    public static void main(String[] args) {
        new Frog().hop();
    }
}
```

**Se recuerda** del Capítulo 5, "Métodos", que las variables estáticas **se inicializan** como `null`. Ejecutar este código produce la siguiente salida:

```Plaintext
Exception in thread "main" java.lang.NullPointerException:
Cannot invoke "String.toLowerCase()" because "Frog.name" is null
```

¿**Se nota** algo especial en esta salida? Java incluye una característica llamada _NullPointerExceptions_ útiles (_Helpful NullPointerExceptions_), en la que la JVM nos indica la referencia de objeto que provocó la `NullPointerException`.

En las variables de instancia y estáticas, la JVM nos indicará el nombre de la variable en un formato agradable y fácil de leer, como acabamos de ver. En las variables locales (incluyendo los parámetros de método), no es tan amigable. **Se prueba** un ejemplo:

```Java
public class Frog {
    public void hop(String name) {
        System.out.print(name.toLowerCase() + " is hopping");
    }
    public static void main(String[] args) {
        new Frog().hop(null);
    }
}
```

Este programa imprime lo siguiente:

```Plaintext
Exception in thread "main" java.lang.NullPointerException:
Cannot invoke "String.toLowerCase()" because "<parameter1>" is null
```

¿Y qué es eso? En los parámetros de método se imprime `<parameterX>`, mientras que en las variables locales se imprime `<localX>`, donde X es el orden en que la variable aparece en el método. La razón de esto es que el nombre de la variable se pierde cuando el código **se compila**.

¿No es muy útil, verdad? **No hay que preocuparse**, ¡hay una solución! Si la clase **se compila** con el argumento `-g:vars`, el código imprimirá el nombre de la variable en tiempo de ejecución:

```zsh
javac -g:vars Frog.java
java Frog
```

Este es un argumento de depuración, destinado a proporcionar información adicional en el caso de que el código **se esté** comportando de manera inesperada. Este código ahora imprime lo siguiente en tiempo de ejecución:

```Plaintext
Exception in thread "main" java.lang.NullPointerException:
Cannot invoke "String.toLowerCase()" because "name" is null
```

**Hay que recordar** que este argumento aplica solo a variables locales y parámetros de método, y **tiene** que usarse cuando el código **se compila**.

Si **se está** usando un IDE como Eclipse o IntelliJ, a menudo tienen el parámetro `-g:vars` habilitado por defecto.

#### `IllegalArgumentException`

`IllegalArgumentException` es una forma en que el programa **se protege** a sí mismo. **Se quiere** decirle a quien invoca que algo está mal, preferiblemente de una manera obvia que quien invoca no **pueda** ignorar, para que el programador corrija el problema. Ver el código terminar con una excepción es un gran recordatorio de que algo está mal. **Se considera** este ejemplo cuando **se llama** con `setNumberEggs(-2)`:

```Java
public void setNumberEggs(int numberEggs) {
    if (numberEggs < 0)
        throw new IllegalArgumentException("# eggs must not be negative");
    this.numberEggs = numberEggs;
}
```

El programa lanza una excepción cuando no **está** feliz con los valores de los parámetros. La salida se ve así:

```Plaintext
Exception in thread "main" java.lang.IllegalArgumentException:
# eggs must not be negative
```

Claramente, este es un problema que **tiene** que corregirse si el programador quiere que el programa haga algo útil.

#### `NumberFormatException`

Java proporciona métodos para convertir cadenas en números. Cuando **se les pasa** un valor no válido, lanzan una `NumberFormatException`. La idea es similar a `IllegalArgumentException`. Dado que este es un problema común, Java le da una clase separada. De hecho, `NumberFormatException` es una subclase de `IllegalArgumentException`. Aquí hay un ejemplo de intentar convertir algo no numérico en un `int`:

```Java
Integer.parseInt("abc");
```

La salida se ve así:

```Plaintext
Exception in thread "main" java.lang.NumberFormatException:
For input string: "abc"
```

Para el examen, **hay que saber** que `NumberFormatException` es una subclase de `IllegalArgumentException`. **Se cubre** más sobre por qué esto es importante más adelante en el capítulo.

### Clases de Excepciones Verificadas

Las excepciones verificadas tienen `Exception` en su jerarquía pero no `RuntimeException`. **Deben ser manejadas** o declaradas. En la Tabla 11.3 **se listan** las excepciones verificadas más comunes.

**TABLA 11.3** Excepciones verificadas

| **Excepción verificada** | **Descripción** |
|---|---|
| `FileNotFoundException` | Subclase de `IOException`. Lanzada programáticamente cuando el código intenta referenciar un archivo que no existe. |
| `IOException` | Lanzada programáticamente cuando hay un problema al leer o escribir un archivo. |
| `NotSerializableException` | Subclase de `IOException`. Lanzada programáticamente al intentar serializar o deserializar una clase no serializable. |
| `ParseException` | Indica un problema al analizar (_parse_) la entrada. |

Para el examen, **hay que saber** que todas estas son excepciones verificadas que **deben ser manejadas** o declaradas. También **hay que saber** que `FileNotFoundException` y `NotSerializableException` son subclases de `IOException`. `ParseException` **se verá** más adelante en este capítulo y las otras tres clases en el Capítulo 14, "E/S" (_I/O_).

### Clases de `Error`

Los errores son excepciones no verificadas que extienden la clase `Error`. **Son lanzados** por la JVM y no **deben ser manejados** ni declarados. Los errores son raros, pero es posible ver los que **se listan** en la Tabla 11.4.

**TABLA 11.4** Errores

| **Error** | **Descripción** |
|---|---|
| `ExceptionInInitializerError` | Se lanza cuando un inicializador estático lanza una excepción y no la maneja. |
| `StackOverflowError` | Se lanza cuando un método se llama a sí mismo demasiadas veces (llamado recursión infinita porque el método típicamente se llama a sí mismo sin fin). |
| `NoClassDefFoundError` | Se lanza cuando la clase que el código usa está disponible en tiempo de compilación pero no en tiempo de ejecución. |

Para el examen, **basta con saber** que estos errores no **están verificados** y que el código a menudo **no puede** recuperarse de ellos.

---

**Ver también:** [[Comprendiendo las Excepciones]] | [[Manejando Excepciones]] | [[Gestión Automática de Recursos]] | [[Contenido/Capitulo 11/Resumen]]
