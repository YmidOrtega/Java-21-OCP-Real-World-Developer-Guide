Las **referencias a métodos** (_method references_) son otra forma de hacer el código más fácil de leer, como simplemente mencionar el nombre del método. Al igual que las lambdas, lleva tiempo acostumbrarse a la nueva sintaxis. En esta sección, **se muestra** la sintaxis junto con los cuatro tipos de referencias a métodos. También **se mezclan** lambdas con referencias a métodos.

Supongamos que **se está** programando un `Duckling` que intenta aprender a graznar. Primero **se tiene** una interfaz funcional:

```Java
public interface LearnToSpeak {
    void speak(String sound);
}
```

A continuación, **se descubre** que el `Duckling` tiene suerte. Hay una clase auxiliar con la que el patito puede trabajar. **Se han omitido** los detalles de enseñar al patito a graznar y **se ha dejado** la parte que llama a la interfaz funcional:

```Java
public class DuckHelper {
    public static void teacher(String name, LearnToSpeak learner) {
        // Ejercitar la paciencia (omitido)
        learner.speak(name);
    }
}
```

Finalmente, **es hora** de ponerlo todo junto y conocer a nuestro pequeño `Duckling`. Este código implementa la interfaz funcional usando una lambda:

```Java
public class Duckling {
    public static void makeSound(String sound) {
        LearnToSpeak learner = s -> System.out.println(s);
        DuckHelper.teacher(sound, learner);
    }
}
```

No está mal. Hay un poco de redundancia, sin embargo. La lambda declara un parámetro llamado `s`. Sin embargo, no hace nada más que pasar ese parámetro a otro método. Una referencia a método permite eliminar esa redundancia y en su lugar escribir esto:

```Java
LearnToSpeak learner = System.out::println;
```

El operador `::` le dice a Java que llame al método `println()` más tarde. Llevará un poco de tiempo acostumbrarse a la sintaxis. Una vez que **se domine**, **se puede** encontrar que el código es más corto y menos distractor sin tener que escribir tantas lambdas.

> **Se recuerda** que `::` es como una lambda, y **se usa** para la ejecución diferida con una interfaz funcional. Incluso **se puede** imaginar la referencia a método como una lambda si ayuda.

Una referencia a método y una lambda **se comportan** de la misma manera en tiempo de ejecución. **Se puede** pretender que el compilador convierte las referencias a métodos en lambdas automáticamente.

Hay cuatro formatos para las referencias a métodos.

- Métodos `static`
- Métodos de instancia en un objeto particular
- Métodos de instancia en un parámetro que **se determinará** en tiempo de ejecución
- Constructores

**Se analizará** brevemente cada uno de ellos. En cada ejemplo, **se muestra** la referencia a método y su lambda equivalente. Por ahora, **se crea** una interfaz funcional separada para cada ejemplo. En la siguiente sección, **se presentan** las interfaces funcionales integradas para que no haya que seguir escribiendo las propias.

#### Llamando a Métodos `static`

Para este primer ejemplo, **se usa** una interfaz funcional que convierte un `double` a un `long`:

```Java
interface Converter {
    long round(double num);
}
```

**Se puede** implementar esta interfaz con el método `round()` en `Math`. Aquí **se asigna** una referencia a método y una lambda a esta interfaz funcional:

```Java
14: Converter methodRef = Math::round;
15: Converter lambda = x -> Math.round(x);
16:
17: System.out.println(methodRef.round(100.1));  // 100
```

En la línea 14, **se referencia** un método con un parámetro, y Java sabe que es como una lambda con un parámetro. Además, Java sabe que debe pasar ese parámetro al método.

Un momento. **Se puede saber** que el método `round()` está sobrecargado: puede tomar un `double` o un `float`. ¿Cómo sabe Java que **se quiere** llamar la versión con un `double`? Tanto con las lambdas como con las referencias a métodos, Java infiere información a partir del *contexto*. En este caso, **se dijo** que **se estaba** declarando un `Converter`, que tiene un método que toma un parámetro `double`. Java busca un método que coincida con esa descripción. Si no puede encontrarlo o encuentra múltiples coincidencias, el compilador reportará un error. Este último **se llama** a veces un error de tipo *ambiguo* (_ambiguous_).

#### Llamando a Métodos de Instancia en un Objeto Particular

Para este ejemplo, la interfaz funcional verifica si un `String` comienza con un valor especificado.

```Java
interface StringStart {
    boolean beginningCheck(String prefix);
}
```

Convenientemente, la clase `String` tiene un método `startsWith()` que toma un parámetro y devuelve un `boolean`. **Se analizará** cómo usar las referencias a métodos con este código:

```Java
18: var str = "Zoo";
19: StringStart methodRef = str::startsWith;
20: StringStart lambda = s -> str.startsWith(s);
21:
22: System.out.println(methodRef.beginningCheck("A"));  // false
```

La línea 19 muestra que **se quiere** llamar a `str.startsWith()` y pasar un único parámetro que **se suministrará** en tiempo de ejecución. Esta sería una buena manera de filtrar los datos en una lista.

Una referencia a método no tiene que tomar ningún parámetro. En este ejemplo, **se crea** una interfaz funcional con un método que no toma ningún parámetro pero devuelve un valor:

```Java
interface StringChecker {
    boolean check();
}
```

**Se implementa** comprobando si el `String` está vacío.

```Java
18: var str = "";
19: StringChecker methodRef = str::isEmpty;
20: StringChecker lambda = () -> str.isEmpty();
21:
22: System.out.print(methodRef.check());  // true
```

Dado que el método en `String` es un método de instancia, **se llama** a la referencia a método en una instancia de la clase `String`.

Aunque todas las referencias a métodos **se pueden** convertir en lambdas, lo contrario no siempre es cierto. Por ejemplo, **se considera** este código:

```Java
var str = "";
StringChecker lambda = () -> str.startsWith("Zoo");
```

¿Cómo **se podría** escribir esto como una referencia a método? **Se podría** intentar una de las siguientes:

```Java
StringChecker methodReference = str::startsWith;            // NO COMPILA
StringChecker methodReference = str::startsWith("Zoo");     // NO COMPILA
```

¡Ninguna de estas funciona! Aunque **se puede** pasar el `str` como parte de la referencia a método, no hay forma de pasar el parámetro `"Zoo"` con ella. Por lo tanto, no es posible escribir esta lambda como una referencia a método.

#### Llamando a Métodos de Instancia en un Parámetro

Esta vez, **se va a** llamar al mismo método de instancia que no toma ningún parámetro. El truco es que **se hará** sin conocer la instancia de antemano. **Se necesita** una interfaz funcional diferente esta vez ya que necesita saber sobre el `String`.

```Java
interface StringParameterChecker {
    boolean check(String text);
}
```

**Se puede** implementar esta interfaz funcional de la siguiente manera:

```Java
23: StringParameterChecker methodRef = String::isEmpty;
24: StringParameterChecker lambda = s -> s.isEmpty();
25:
26: System.out.println(methodRef.check("Zoo"));  // false
```

La línea 23 dice que el método que **se quiere** llamar **se declara** en `String`. Parece un método `static`, pero no lo es. En cambio, Java sabe que `isEmpty()` es un método de instancia que no toma ningún parámetro. Java usa el parámetro suministrado en tiempo de ejecución como la instancia en la que **se llama** el método.

**Se comparan** las líneas 23 y 24 con las líneas 19 y 20 del ejemplo de instancia anterior. Se parecen, aunque una referencia una variable local llamada `str`, mientras que la otra solo referencia los parámetros de la interfaz funcional.

Incluso **se pueden** combinar los dos tipos de referencias a métodos de instancia. De nuevo, **se necesita** una nueva interfaz funcional que tome dos parámetros.

```Java
interface StringTwoParameterChecker {
    boolean check(String text, String prefix);
}
```

**Se debe prestar atención** al orden de los parámetros al leer la implementación.

```Java
26: StringTwoParameterChecker methodRef = String::startsWith;
27: StringTwoParameterChecker lambda = (s, p) -> s.startsWith(p);
28:
29: System.out.println(methodRef.check("Zoo", "A"));  // false
```

Dado que la interfaz funcional toma dos parámetros, Java tiene que determinar qué representan. El primero siempre será la instancia del objeto para los métodos de instancia. Cualquier otro parámetro será un parámetro del método.

**Se debe recordar** que la línea 26 puede parecer un método `static`, pero en realidad es una referencia a método que declara que la instancia del objeto **se especificará** más tarde. La línea 27 muestra parte del poder de una referencia a método. **Se pudo** reemplazar dos parámetros lambda esta vez.

#### Llamando a Constructores

Una **referencia a constructor** (_constructor reference_) es un tipo especial de referencia a método que usa `new` en lugar de un método e instancia un objeto. Para este ejemplo, la interfaz funcional no tomará ningún parámetro pero devolverá un `String`.

```Java
interface EmptyStringCreator {
    String create();
}
```

Para llamar esto, **se usa** `new` como si fuera un nombre de método.

```Java
30: EmptyStringCreator methodRef = String::new;
31: EmptyStringCreator lambda = () -> new String();
32:
33: var myString = methodRef.create();
34: System.out.println(myString.equals("Snake"));  // false
```

**Se expande** como las referencias a métodos que **se han visto** hasta ahora. En el ejemplo anterior, la lambda no tiene ningún parámetro.

Las referencias a métodos pueden ser complicadas. Esta vez **se crea** una interfaz funcional que toma un parámetro y devuelve un resultado:

```Java
interface StringCopier {
    String copy(String value);
}
```

En la implementación, **se nota** que la línea 32 en el siguiente ejemplo tiene la misma referencia a método que la línea 30 en el ejemplo anterior:

```Java
32: StringCopier methodRef = String::new;
33: StringCopier lambda = x -> new String(x);
34:
35: var myString = methodRef.copy("Zebra");
36: System.out.println(myString.equals("Zebra"));  // true
```

Esto significa que no siempre **se puede** determinar qué método **puede ser** llamado al mirar la referencia a método. En cambio, **se tiene** que mirar el contexto para ver qué parámetros **se usan** y si hay un tipo de retorno. En este ejemplo, Java ve que **se está** pasando un parámetro `String` y llama al constructor de `String` que toma dicho parámetro.

#### Revisando las Referencias a Métodos

Leer las referencias a métodos es útil para comprender el código. La Tabla 8.3 muestra los cuatro tipos de referencias a métodos. Si esta tabla no tiene sentido, **se debe releer** la sección anterior. Puede tomar algunos intentos antes de que las referencias a métodos comiencen a encajar.

**TABLA 8.3** Referencias a métodos

| **Tipo** | **Antes de los dos puntos** | **Después de los dos puntos** | **Ejemplo** |
|---|---|---|---|
| Métodos `static` | Nombre de clase | Nombre del método | `Math::random` |
| Métodos de instancia en un objeto particular | Nombre de variable de instancia | Nombre del método | `str::startsWith` |
| Métodos de instancia en un parámetro | Nombre de clase | Nombre del método | `String::isEmpty` |
| Constructor | Nombre de clase | `new` | `String::new` |
