A menudo, una aplicación trabaja con archivos, bases de datos y varios objetos de conexión. Comúnmente, estas fuentes de datos externas **se denominan** _recursos_. En muchos casos, **se abre** una conexión al recurso, ya sea a través de la red o dentro de un sistema de archivos. Luego **se leen** o **escriben** los datos que **se necesitan**. Finalmente, **se cierra** el recurso para indicar que ya **se terminó** con él.

¿Qué sucede si no **se cierra** un recurso cuando **se termina** con él? En resumen, pueden suceder cosas muy malas. Si **se conecta** a una base de datos, **se podrían** usar todas las conexiones disponibles, lo que significaría que nadie **puede** hablar con la base de datos hasta que **se liberen** las conexiones. Aunque comúnmente **se escucha** sobre fugas de memoria que hacen fallar los programas, una _fuga de recursos_ (_resource leak_) es igual de mala y **ocurre** cuando un programa no libera sus conexiones a un recurso, haciendo que el recurso **se vuelva** inaccesible. Esto podría significar que el programa ya no **puede** hablar con la base de datos, o, peor aún, que todos los programas **puedan** dejar de alcanzar la base de datos.

Para el examen, un _recurso_ es típicamente un archivo o base de datos que requiere algún tipo de stream o conexión para leer o escribir datos. En el Capítulo 14, **se crearán** numerosos recursos que **tendrán** que cerrarse cuando **se termine** con ellos.

## Introduciendo Try-with-Resources

**Se observa** un método que abre un archivo, lee los datos y lo cierra:

```Java
4:  public void readFile(String file) {
5:      FileInputStream is = null;
6:      try {
7:          is = new FileInputStream("myfile.txt");
8:          // Leer datos del archivo
9:      } catch (IOException e) {
10:         e.printStackTrace();
11:     } finally {
12:         if(is != null) {
13:             try {
14:                 is.close();
15:             } catch (IOException e2) {
16:                 e2.printStackTrace();
17:             }
18:         }
19:     }
20: }
```

¡Vaya, qué método tan largo! ¿Por qué hay dos bloques `try` y `catch`? Pues bien, las líneas 7 y 14 incluyen llamadas verificadas a `IOException`, y esas deben atraparse en el método o relanzarse por el método. La mitad de las líneas de código de este método solo **están** cerrando un recurso. Y cuantos más recursos **se tengan**, más largo **se vuelve** el código así. Por ejemplo, **se pueden** tener múltiples recursos que necesiten cerrarse en un orden particular. También **no se quiere** que una excepción causada por cerrar un recurso **impida** cerrar otro recurso.

Para resolver esto, Java incluye la sentencia _try-with-resources_ para cerrar automáticamente todos los recursos abiertos en una cláusula `try`. Esta característica también **se conoce** como _gestión automática de recursos_ (_automatic resource management_), porque Java automáticamente **se encarga** del cierre.

**Se observa** el mismo ejemplo usando una sentencia `try-with-resources`:

```Java
4:  public void readFile(String file) {
5:      try (FileInputStream is = new FileInputStream("myfile.txt")) {
6:          // Leer datos del archivo
7:      } catch (IOException e) {
8:          e.printStackTrace();
9:      }
10: }
```

Funcionalmente son similares, pero la nueva versión tiene la mitad de líneas. Más importante aún, al usar una sentencia `try-with-resources`, **se garantiza** que tan pronto como una conexión **sale** del alcance, Java intentará cerrarla dentro del mismo método.

Entre bastidores, el compilador reemplaza un bloque `try-with-resources` con un bloque `try` y un bloque `finally`. A este bloque `finally` "oculto" **se le llama** bloque `finally` _implícito_, ya que **es creado** y **usado** automáticamente por el compilador. Aún **se puede** crear un bloque `finally` definido por el programador cuando **se usa** una sentencia `try-with-resources`; solo **hay que tener** en cuenta que el implícito **se llamará** primero.

> **Consejo:** A diferencia de la recolección de basura, los recursos no **se cierran** automáticamente cuando **salen** del alcance. Por lo tanto, **se recomienda** cerrar los recursos en el mismo bloque de código que los abre. Al usar una sentencia `try-with-resources` para abrir todos los recursos, esto **ocurre** automáticamente.

## Fundamentos de Try-with-Resources

La imagen a continuación muestra cómo luce una sentencia `try-with-resources`. **Hay que notar** que uno o más recursos **pueden** abrirse en la cláusula `try`. Cuando **se abren** múltiples recursos, **se cierran** en el orden _inverso_ al que fueron creados. También, **hay que notar** que los paréntesis **se usan** para listar esos recursos y los puntos y comas **se usan** para separar las declaraciones. Esto funciona igual que declarar múltiples índices en un bucle `for`.

![[La sintaxis de una instrucción básica try-with-resources.jpeg]]

¿Qué pasó con el bloque `catch` de la imagen? Resulta que un bloque `catch` es _opcional_ con una sentencia `try-with-resources`. Por ejemplo, **se puede** reescribir el ejemplo anterior de `readFile()` para que el método declare la excepción y así hacerlo aún más corto:

```Java
4:  public void readFile(String file) throws IOException {
5:      try (FileInputStream is = new FileInputStream("myfile.txt")) {
6:          // Leer datos del archivo
7:      }
8: }
```

Anteriormente en el capítulo, **se aprendió** que una sentencia `try` debe tener uno o más bloques `catch` o un bloque `finally`. Una sentencia `try-with-resources` difiere de una sentencia `try` en que ninguno de estos **es requerido**, aunque un desarrollador puede añadir ambos. Para el examen, **hay que saber** que el bloque `finally` implícito **se ejecuta** antes que cualquier bloque `finally` definido por el programador.

#### Construyendo Sentencias Try-with-Resources

Solo las clases que implementan la interfaz `AutoCloseable` **pueden** usarse en una sentencia `try-with-resources`. Por ejemplo, lo siguiente no compila porque `String` no implementa la interfaz `AutoCloseable`:

```Java
try (String reptile = "lizard") {}
```

Heredar `AutoCloseable` requiere implementar un método `close()` compatible:

```Java
interface AutoCloseable {
    public void close() throws Exception;
}
```

A partir de los estudios sobre sobrescritura de métodos, esto significa que la versión implementada de `close()` **puede** elegir lanzar `Exception` o una subclase, o no lanzar ninguna excepción.

A lo largo del resto de esta sección **se usa** la siguiente clase de recurso personalizado que simplemente imprime un mensaje cuando **se llama** al método `close()`:

```Java
public class MyFileClass implements AutoCloseable {
    private final int num;
    public MyFileClass(int num) { this.num = num; }
    @Override public void close() {
        System.out.println("Closing: " + num);
    }
}
```

> **Nota:** En el Capítulo 14, **se encontrarán** recursos que implementan `Closeable` en lugar de `AutoCloseable`. Dado que `Closeable` extiende `AutoCloseable`, ambos **son compatibles** con sentencias `try-with-resources`. La única diferencia es que el método `close()` de `Closeable` declara `IOException`, mientras que el de `AutoCloseable` declara `Exception`.

#### Declarando Recursos

Aunque `try-with-resources` soporta la declaración de múltiples variables, cada variable debe **ser declarada** en una sentencia separada. Por ejemplo, lo siguiente no compila:

```Java
try (MyFileClass is = new MyFileClass(1),
    os = new MyFileClass(2)) {
} // NO COMPILA

try (MyFileClass ab = new MyFileClass(1),
    MyFileClass cd = new MyFileClass(2)) {
} // NO COMPILA
```

El primer ejemplo no compila porque **le falta** el tipo de dato y usa una coma (`,`) en lugar de un punto y coma (`;`). El segundo ejemplo tampoco compila porque también usa una coma (`,`) en lugar de un punto y coma (`;`). Cada recurso debe incluir el tipo de dato y **estar** separado por un punto y coma (`;`).

**Se puede** declarar un recurso usando `var` como tipo de dato en una sentencia `try-with-resources`, ya que los recursos son variables locales:

```Java
try (var f = new BufferedInputStream(new FileInputStream("it.txt"))) {
    // Procesar archivo
}
```

Declarar recursos es una situación común donde usar `var` es bastante útil, ya que acorta la ya larga línea de código.

#### Alcance de Try-with-Resources

Los recursos creados en la cláusula `try` **están** en alcance solo dentro del bloque `try`. Esta es otra forma de recordar que el `finally` implícito **se ejecuta** antes que cualquier bloque `catch`/`finally` que **se codifique** por cuenta propia. El cierre implícito ya **se ejecutó**, y el recurso ya no **está** disponible. ¿Por qué las líneas 6 y 8 no compilan en este ejemplo?

```Java
3:  try (Scanner s = new Scanner(System.in)) {
4:      s.nextLine();
5:  } catch(Exception e) {
6:      s.nextInt(); // NO COMPILA
7:  } finally {
8:      s.nextInt(); // NO COMPILA
9:  }
```

El problema es que `Scanner` **ha salido** del alcance al final de la cláusula `try`. Las líneas 6 y 8 no tienen acceso a él. Esta es una característica agradable: no **se puede** usar accidentalmente un objeto que ya **ha sido** cerrado. En una sentencia `try` tradicional, la variable debe declararse antes de la sentencia `try` para que tanto el bloque `try` como el bloque `finally` **puedan** acceder a ella, lo que tiene el desagradable efecto secundario de dejar la variable en alcance durante el resto del método, invitando a llamarla accidentalmente.

#### Siguiendo el Orden de Operaciones

Al trabajar con sentencias `try-with-resources`, **es importante** saber que los recursos **se cierran** en el orden inverso al orden en que **se crean**. Usando la clase personalizada `MyFileClass`, ¿**se puede** determinar qué imprime este método?

```Java
public static void main(String... xyz) {
    try (MyFileClass bookReader = new MyFileClass(1);
            MyFileClass movieReader = new MyFileClass(2)) {
        System.out.println("Try Block");
        throw new RuntimeException();
    } catch (Exception e) {
        System.out.println("Catch Block");
    } finally {
        System.out.println("Finally Block");
    }
}
```

Aunque este ejemplo puede parecer un poco enrevesado en la práctica, preguntas como esta son comunes en el examen. La salida es la siguiente:

```Plaintext
Try Block
Closing: 2
Closing: 1
Catch Block
Finally Block
```

Para el examen, **hay que asegurarse** de entender por qué el método imprime las sentencias en este orden. **Hay que recordar** que los recursos **se cierran** en el orden inverso al que **se declaran**, y el `finally` implícito **se ejecuta** antes del `finally` definido por el programador.

## Aplicando Efectivamente Final

Aunque los recursos frecuentemente **se crean** en la sentencia `try-with-resources`, es posible declararlos con anticipación, siempre que **estén** marcados como `final` o sean efectivamente finales. El Capítulo 5 **se puede** revisar si **se quiere** recordar qué significa efectivamente final.

La sintaxis usa el nombre del recurso en lugar de la declaración del recurso, separados por punto y coma (`;`). **Se prueba** otro ejemplo:

```Java
11: public static void main(String... xyz) {
12:     final var bookReader = new MyFileClass(4);
13:     MyFileClass movieReader = new MyFileClass(5);
14:     try (bookReader;
15:             var tvReader = new MyFileClass(6);
16:             movieReader) {
17:         System.out.println("Try Block");
18:     } finally {
19:         System.out.println("Finally Block");
20:     }
21: }
```

**Se analiza** este una línea a la vez. La línea 12 declara una variable `final` `bookReader`, mientras que la línea 13 declara una variable efectivamente final `movieReader`. Ambos recursos **pueden** usarse en una sentencia `try-with-resources`. **Se sabe** que `movieReader` es efectivamente final porque es una variable local a la que **se le asigna** un valor solo una vez. **Hay que recordar** que la prueba de efectivamente final es que, si **se inserta** la palabra clave `final` cuando **se declara** la variable, el código sigue compilando.

Las líneas 14 y 16 usan la nueva sintaxis para declarar recursos en una sentencia `try-with-resources`, usando solo el nombre de la variable y separando los recursos con punto y coma (`;`). La línea 15 usa la sintaxis normal para declarar un nuevo recurso dentro de la cláusula `try`.

Al ejecutarse, el código imprime lo siguiente:

```Plaintext
Try Block
Closing: 5
Closing: 6
Closing: 4
Finally Block
```

Si en el examen **se encuentra** una sentencia `try-with-resources` con una variable no declarada en la cláusula `try`, **hay que asegurarse** de que sea efectivamente final. Por ejemplo, lo siguiente no compila:

```Java
31: var writer = Files.newBufferedWriter(path);
32: try (writer) {   // NO COMPILA
33:     writer.append("Bienvenido al zoológico!");
34: }
35: writer = null;
```

La variable `writer` **es reasignada** en la línea 35, lo que hace que el compilador no la considere efectivamente final. Al no ser efectivamente final, no **puede** usarse en una sentencia `try-with-resources` en la línea 32.

Otro lugar donde el examen puede intentar confundir es accediendo a un recurso después de que **ha sido** cerrado. **Se considera** lo siguiente:

```Java
41: var writer = Files.newBufferedWriter(path);
42: writer.append("Esta escritura está permitida ¡pero es una idea muy mala!");
43: try (writer) {
44:     writer.append("Bienvenido al zoológico!");
45: }
46: writer.append("¡Esta escritura fallará!"); // IOException
```

Este código compila pero lanza una excepción en la línea 46 con el mensaje `Stream closed`. Si bien **es posible** escribir en el recurso antes de la sentencia `try-with-resources`, no después.

## Comprendiendo las Excepciones Suprimidas

**Se concluye** la discusión de excepciones con probablemente el tema más confuso: las _excepciones suprimidas_ (_suppressed exceptions_). ¿Qué sucede si el método `close()` lanza una excepción? **Se prueba** un ejemplo ilustrativo:

```Java
public class TurkeyCage implements AutoCloseable {
    public void close() {
        System.out.println("Cerrar la puerta");
    }
    public static void main(String[] args) {
        try (var t = new TurkeyCage()) {
            System.out.println("Poniendo los pavos");
        }
    }
}
```

Si la jaula de pavos no **se cierra**, todos los pavos **podrían** escaparse. Claramente, **hay que** manejar dicha condición. Ya **se sabe** que los recursos **se cierran** antes de que **se ejecuten** los bloques `catch` definidos por el programador. Esto significa que **se puede** atrapar la excepción lanzada por `close()` si **se quiere**. Alternativamente, **se puede** permitir que quien invoca **se encargue** de ella.

**Se expande** el ejemplo con la siguiente implementación de `JammedTurkeyCage`:

```Java
1:  public class JammedTurkeyCage implements AutoCloseable {
2:      public void close() throws IllegalStateException {
3:          throw new IllegalStateException("La puerta de la jaula no cierra");
4:      }
5:      public static void main(String[] args) {
6:          try (JammedTurkeyCage t = new JammedTurkeyCage()) {
7:              System.out.println("Poniendo los pavos");
8:          } catch (IllegalStateException e) {
9:              System.out.println("Caught: " + e.getMessage());
10:         }
11:     }
12: }
```

El método `close()` **es llamado** automáticamente por `try-with-resources`. Lanza una excepción, que **es atrapada** por el bloque `catch` e imprime lo siguiente:

```Plaintext
Caught: La puerta de la jaula no cierra
```

Esto parece razonable. ¿Qué sucede si el bloque `try` también lanza una excepción? Cuando **se lanzan** múltiples excepciones, todas excepto la primera **se denominan** _excepciones suprimidas_ (_suppressed exceptions_). La idea es que Java trata la primera excepción como la principal y le añade cualquiera que surja durante el cierre automático.

¿Qué **produce** la siguiente implementación del método `main()`?

```Java
5:  public static void main(String[] args) {
6:      try (JammedTurkeyCage t = new JammedTurkeyCage()) {
7:          throw new IllegalStateException("Los pavos se escaparon");
8:      } catch (IllegalStateException e) {
9:          System.out.println("Caught: " + e.getMessage());
10:         for (Throwable t: e.getSuppressed())
11:             System.out.println("Suppressed: " + t.getMessage());
12:     }
13: }
```

La línea 7 lanza la excepción principal. En ese punto, la cláusula `try` termina y Java automáticamente llama al método `close()`. La línea 3 de `JammedTurkeyCage` lanza una `IllegalStateException`, que **se agrega** como excepción suprimida. Luego la línea 8 atrapa la excepción principal. La línea 9 imprime el mensaje de la excepción principal. Las líneas 10 y 11 iteran por las excepciones suprimidas y las imprimen. El programa imprime lo siguiente:

```Plaintext
Caught: Los pavos se escaparon
Suppressed: La puerta de la jaula no cierra
```

**Hay que tener** en cuenta que el bloque `catch` busca coincidencias con la excepción principal. ¿Qué imprime este código?

```Java
5:  public static void main(String[] args) {
6:      try (JammedTurkeyCage t = new JammedTurkeyCage()) {
7:          throw new RuntimeException("Los pavos se escaparon");
8:      } catch (IllegalStateException e) {
9:          System.out.println("caught: " + e.getMessage());
10:     }
11: }
```

La línea 7 otra vez lanza la excepción principal. Java llama al método `close()` y añade una excepción suprimida. La línea 8 atraparía la `IllegalStateException`. Sin embargo, no **se tiene** una de esas: la excepción principal es una `RuntimeException`. Dado que esto no coincide con la cláusula `catch`, la excepción **se lanza** a quien invoca. Eventualmente, el método `main()` produciría algo como lo siguiente:

```Plaintext
Exception in thread "main" java.lang.RuntimeException: Los pavos se escaparon
    at JammedTurkeyCage.main(JammedTurkeyCage.java:7)
Suppressed: java.lang.IllegalStateException: La puerta de la jaula no cierra
    at JammedTurkeyCage.close(JammedTurkeyCage.java:3)
    at JammedTurkeyCage.main(JammedTurkeyCage.java:8)
```

Java **recuerda** las excepciones suprimidas que van con una excepción principal aunque no **se manejen** en el código.

> **Nota:** Si más de dos recursos lanzan una excepción, el primero en lanzarse **se convierte** en la excepción principal, y el resto **se agrupan** como excepciones suprimidas. Y dado que los recursos **se cierran** en el orden inverso al que **se declaran**, la excepción principal **estará** en el último recurso declarado que lance una excepción.

**Hay que tener** en cuenta que las excepciones suprimidas solo aplican a excepciones lanzadas en la cláusula `try`. El siguiente ejemplo **no** lanza una excepción suprimida:

```Java
5:  public static void main(String[] args) {
6:      try (JammedTurkeyCage t = new JammedTurkeyCage()) {
7:          throw new IllegalStateException("Los pavos se escaparon");
8:      } finally {
9:          throw new RuntimeException("y no los pudimos encontrar");
10:     }
11: }
```

La línea 7 lanza una excepción. Luego Java trata de cerrar el recurso y le añade una excepción suprimida. Ahora hay un problema: el bloque `finally` **se ejecuta** después de todo esto. Dado que la línea 9 también lanza una excepción, la excepción anterior de la línea 7 **se pierde**, con el código imprimiendo lo siguiente:

```Plaintext
Exception in thread "main" java.lang.RuntimeException:
    y no los pudimos encontrar
    at JammedTurkeyCage.main(JammedTurkeyCage.java:9)
```

Esto siempre ha sido y sigue siendo una mala práctica de programación. ¡No **se quieren** perder excepciones! Aunque está fuera del alcance del examen, la razón tiene que ver con la compatibilidad hacia atrás: este comportamiento existía antes de que **se añadiera** la gestión automática de recursos.

---

**Ver también:** [[Manejando Excepciones]] | [[Comprendiendo las Excepciones]] | [[Reconociendo las Clases de Excepción]]
