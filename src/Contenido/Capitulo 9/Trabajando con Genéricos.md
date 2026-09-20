**Se concluye** este capítulo con una de las características más útiles, y a veces más confusas, del lenguaje Java: los genéricos. En esta sección, **se presentan** temas más avanzados incluyendo la creación de clases y métodos genéricos.

#### Creando Clases Genéricas

**Se pueden** introducir genéricos en las propias clases. La sintaxis para introducir un genérico **es** declarar un *parámetro de tipo formal* (_formal type parameter_) entre corchetes angulares. Por ejemplo, la siguiente clase llamada `Crate` tiene una variable de tipo genérico declarada después del nombre de la clase:

```Java
public class Crate<T> {
    private T contents;
    public T lookInCrate() {
        return contents;
    }
    public void packCrate(T contents) {
        this.contents = contents;
    }
}
```

El tipo genérico `T` **está disponible** en cualquier lugar dentro de la clase `Crate`. Cuando **se instancia** la clase, **se le dice** al compilador qué debe ser `T` para esa instancia particular.

> **Convenciones de Nomenclatura para Genéricos**
>
> Un parámetro de tipo **puede** ser nombrado como **se quiera**. La convención **es** usar letras mayúsculas simples para que **sea** obvio que no son nombres de clases reales. Las siguientes **son** letras comunes a usar:
>
> - `E` para un elemento
> - `K` para una clave de mapa
> - `V` para un valor de mapa
> - `N` para un número
> - `T` para un tipo de datos genérico
> - `S`, `U`, `V`, y así sucesivamente para múltiples tipos genéricos

Por ejemplo, supongamos que existe una clase `Elephant` y **se está** moviendo el elefante a un nuevo y más grande recinto en el zoológico.

```Java
Elephant elephant = new Elephant();
Crate<Elephant> crateForElephant = new Crate<>();
crateForElephant.packCrate(elephant);
Elephant inNewHome = crateForElephant.lookInCrate();
```

También **se puede** crear una `Crate` para una `Zebra`:

```Java
Crate<Zebra> crateForZebra = new Crate<>();
```

Ahora no **se podría** haber simplemente codificado `Elephant` en la clase `Crate` ya que una `Zebra` no **es** un `Elephant`. Sin embargo, **se podría** haber creado una superclase o interfaz `Animal` y usarla en `Crate`.

Las clases genéricas **se vuelven** útiles cuando las clases usadas como parámetro de tipo pueden no tener nada que ver entre sí. Por ejemplo, **se necesita** enviar un robot de 120 libras a otra ciudad.

```Java
Robot joeBot = new Robot();
Crate<Robot> robotCrate = new Crate<>();
robotCrate.packCrate(joeBot);
// enviar a Houston
Robot atDestination = robotCrate.lookInCrate();
```

Las clases genéricas no **están limitadas** a tener un único parámetro de tipo. Esta clase muestra dos parámetros genéricos.

```Java
public class SizeLimitedCrate<T, U> {
    private T contents;
    private U sizeLimit;
    public SizeLimitedCrate(T contents, U sizeLimit) {
        this.contents = contents;
        this.sizeLimit = sizeLimit;
    } }
```

`T` representa el tipo que **se está** poniendo en la caja. `U` representa la unidad que **se está** usando para medir el tamaño máximo de la caja. Para usar esta clase genérica, **se puede** escribir lo siguiente:

```Java
Elephant elephant = new Elephant();
Integer numPounds = 15_000;
SizeLimitedCrate<Elephant, Integer> c1
    = new SizeLimitedCrate<>(elephant, numPounds);
```

Aquí **se especifica** que el tipo **es** `Elephant`, y la unidad **es** `Integer`.

#### Entendiendo el Borrado de Tipos

Especificar un tipo genérico permite al compilador imponer el uso correcto del tipo genérico. Por ejemplo, especificar el tipo genérico de `Crate` como `Robot` **es** como reemplazar la `T` en la clase `Crate` con `Robot`. Sin embargo, esto **es** solo para tiempo de compilación.

Detrás de escena, el compilador reemplaza todas las referencias a `T` en `Crate` con `Object`. En otras palabras, después de que el código **compila**, los genéricos son simplemente tipos `Object`. La clase `Crate` **se ve** así en tiempo de ejecución:

```Java
public class Crate {
        private Object contents;
        public Object lookInCrate() {
            return contents;
        }
        public void packCrate(Object contents) {
            this.contents = contents;
        }
    }
```

Esto significa que solo hay un archivo de clase. No hay diferentes copias para diferentes tipos parametrizados. Este proceso de eliminar la sintaxis de genéricos del código **se denomina** *borrado de tipos* (_type erasure_). El borrado de tipos permite que el código **sea** compatible con versiones anteriores de Java que no contienen genéricos.

El compilador agrega los casts relevantes para que el código funcione con este tipo de clase borrada. Por ejemplo, **se escribe** lo siguiente:

```Java
Robot r = crate.lookInCrate();
```

El compilador lo convierte en lo siguiente:

```Java
Robot r = (Robot) crate.lookInCrate();
```

#### Sobrecargando un Método Genérico

Solo uno de estos dos métodos **está permitido** en una clase porque el borrado de tipos reducirá ambos conjuntos de argumentos a `(List input)`.

```Java
public class LongTailAnimal {
    protected void chew(List<Object> input) {}
    protected void chew(List<Double> input) {} // NO COMPILA
}
```

Por la misma razón, tampoco **se puede** sobrecargar un método genérico desde una clase padre.

```Java
public class LongTailAnimal {
    protected void chew(List<Object> input) {}
}

public class Anteater extends LongTailAnimal {
    protected void chew(List<Double> input) {} // NO COMPILA
}
```

Ambos ejemplos fallan al compilar debido al borrado de tipos. En la forma compilada, el tipo genérico **se elimina** y **aparece** como un método sobrecargado inválido. Ahora, **se mira** otra versión de la misma subclase:

```Java
public class Anteater extends LongTailAnimal {
    protected void chew(List<Object> input) {}
    protected void chew(ArrayList<Double> input) {}
}
```

El primer método `chew()` **compila** porque usa el mismo tipo genérico en el método sobrescrito que el definido en la clase padre. El segundo método `chew()` también **compila**. Sin embargo, **es** un método sobrecargado porque uno de los argumentos del método **es** una `List` y el otro **es** un `ArrayList`. Al trabajar con métodos genéricos, **es** importante considerar el tipo subyacente.

#### Devolviendo Tipos Genéricos

Cuando **se trabaja** con métodos sobrescritos que devuelven genéricos, los valores de retorno deben **ser** covariantes. En términos de genéricos, esto significa que el tipo de retorno de la clase o interfaz declarado en el método sobrescrito debe **ser** un subtipo de la clase definida en la clase padre. El parámetro de tipo genérico debe coincidir exactamente con el tipo de su padre.

Dada la siguiente declaración para la clase `Mammal`, ¿cuál de las dos subclases, `Monkey` y `Goat`, compila?

```Java
public class Mammal {
    public List<CharSequence> play() { … }
    public CharSequence sleep() { … }
}

public class Monkey extends Mammal {
    public ArrayList<CharSequence> play() { … }
}

public class Goat extends Mammal {
    public List<String> play() { … }  // NO COMPILA
    public String sleep() { … }
}
```

La clase `Monkey` **compila** porque `ArrayList` **es** un subtipo de `List`. El método `play()` en la clase `Goat` no compila. Para que los tipos de retorno **sean** covariantes, el parámetro de tipo genérico debe coincidir exactamente. Aunque `String` **es** un subtipo de `CharSequence`, no coincide exactamente con el tipo genérico definido en la clase `Mammal`. Por lo tanto, **se considera** una sobrescritura inválida.

**Hay que notar** que el método `sleep()` en la clase `Goat` sí compila ya que `String` **es** un subtipo de `CharSequence`. Este ejemplo muestra que la covarianza **se aplica** al tipo de retorno, no solo al parámetro de tipo genérico.

#### Implementando Interfaces Genéricas

Al igual que una clase, una interfaz **puede** declarar un parámetro de tipo formal. Por ejemplo, la siguiente interfaz `Shippable` usa un tipo genérico como argumento para su método `ship()`:

```Java
public interface Shippable<T> {
    void ship(T t);
}
```

Hay tres formas en que una clase **puede** implementar esta interfaz. La primera **es** especificar el tipo genérico en la clase. La siguiente clase concreta dice que solo trabaja con robots. Esto le permite declarar el método `ship()` con un parámetro `Robot`.

```Java
class ShippableRobotCrate implements Shippable<Robot> {
    public void ship(Robot t) { }
}
```

La siguiente forma **es** crear una clase genérica. La siguiente clase concreta permite al llamador especificar el tipo del genérico:

```Java
class ShippableAbstractCrate<U> implements Shippable<U> {
    public void ship(U t) { }
}
```

La forma final **es** no usar genéricos en absoluto. Esta **es** la forma antigua de escribir código. Genera una advertencia del compilador sobre `Shippable` siendo un *tipo crudo* (_raw type_), pero **compila**. Aquí el método `ship()` tiene un parámetro `Object` ya que el tipo genérico no **está definido**:

```Java
class ShippableCrate implements Shippable {
    public void ship(Object t) { }
}
```

> **Escenario del Mundo Real: Lo que No Se Puede Hacer con Tipos Genéricos**
>
> Hay algunas limitaciones sobre lo que **se puede** hacer con un tipo genérico. La mayoría **se deben** al borrado de tipos.
>
> - **Llamar a un constructor:** Escribir `new T()` no **está permitido** porque en tiempo de ejecución sería `new Object()`.
> - **Crear un array de ese tipo genérico:** Esta **es** la más molesta, pero tiene sentido porque **se estaría** creando un array de valores `Object`.
> - **Llamar `instanceof`:** Esto no **está permitido** porque en tiempo de ejecución `List<Integer>` y `List<String>` **se ven** igual para Java, gracias al borrado de tipos.
> - **Usar un tipo primitivo como parámetro de tipo genérico:** Esto no **es** gran cosa porque **se puede** usar la clase wrapper en su lugar. Si **se quiere** un tipo de `int`, solo **se usa** `Integer`.
> - **Crear una variable `static` como parámetro de tipo genérico:** Esto no **está permitido** porque el tipo **está ligado** a la instancia de la clase.
> - **Capturar una excepción de tipo T:** Incluso si `T` extiende `Exception`, no **puede ser** usada en un bloque catch ya que el tipo preciso no **es** conocido.

#### Escribiendo Métodos Genéricos

Hasta este punto, **se han visto** parámetros de tipo formal declarados a nivel de clase o interfaz. También **es** posible declararlos a nivel de método. Esto a menudo **es** útil para métodos `static` ya que no **son** parte de una instancia que **puede** declarar el tipo. Sin embargo, también **está permitido** en métodos no `static`.

En este ejemplo, ambos métodos usan un parámetro genérico:

```Java
public class Handler {
    public static <T> void prepare(T t) {
        System.out.println("Preparing " + t);
    }
    public static <T> Crate<T> ship(T t) {
        System.out.println("Shipping " + t);
        return new Crate<T>();
    }
}
```

El parámetro del método **es** el tipo genérico `T`. Antes del tipo de retorno, **se declara** el parámetro de tipo formal de `<T>`. En el método `ship()`, **se muestra** cómo **se puede** usar el parámetro genérico en el tipo de retorno, `Crate<T>`, para el método.

A menos que un método esté obteniendo el parámetro de tipo formal genérico de la clase/interfaz, **se especifica** inmediatamente antes del tipo de retorno del método. Esto puede llevar a código que **se ve** interesante:

```Java
2: public class More {
3:     public static <T> void sink(T t) { }
4:     public static <T> T identity(T t) { return t; }
5:     public static T noGood(T t) { return t; } // NO COMPILA
6: }
```

La línea 3 muestra el parámetro de tipo formal inmediatamente antes del tipo de retorno de `void`. La línea 4 muestra el tipo de retorno siendo el parámetro de tipo formal. **Se ve** extraño, pero **es** correcto. La línea 5 omite el parámetro de tipo formal y por lo tanto no compila.

> **Escenario del Mundo Real: Sintaxis Opcional para Invocar un Método Genérico**
>
> **Se puede** llamar a un método genérico normalmente, y el compilador intentará averiguar cuál **se quiere**. Alternativamente, **se puede** especificar el tipo explícitamente para que **sea** obvio cuál es el tipo.
>
> ```Java
> Box.<String>ship("package");
> Box.<String[]>ship(args);
> ```
>
> Depende de uno mismo si esto hace las cosas más claras. Al menos **se debe** estar consciente de que esta sintaxis existe.

#### Creando un Record Genérico

Los genéricos también **pueden ser** usados con _records_. Este _record_ toma un único parámetro de tipo genérico:

```Java
public record CrateRecord<T>(T contents) {
    @Override
    public T contents() {
        if (contents == null)
            throw new IllegalStateException("missing contents");
        return contents;
    }
}
```

Funciona de la misma manera que las clases. ¡**Se puede** crear un _record_ del robot!

```Java
Robot robot = new Robot();
CrateRecord<Robot> record = new CrateRecord<>(robot);
```

Esto **es** conveniente. ¡Ahora **se tiene** un _record_ inmutable y genérico!

#### Acotando Tipos Genéricos

Ahora, **se podría** haber notado que los genéricos no parecen particularmente útiles ya que **son** tratados como `Objects` y, por lo tanto, no tienen muchos métodos disponibles. Los comodines acotados (_bounded wildcards_) resuelven esto restringiendo qué tipos **pueden ser** usados en un genérico. Un *tipo de parámetro acotado* (_bounded parameter type_) **es** un tipo genérico que especifica un límite para el genérico. **Hay que tener en cuenta** que esta **es** la sección más difícil del capítulo.

Un *tipo genérico comodín* (_wildcard generic type_) **es** un tipo genérico desconocido representado con un signo de interrogación (`?`). **Se pueden** usar comodines genéricos de tres maneras, como **se muestra** en la Tabla 9.15.

**TABLA 9.15** Tipos de límites

| **Tipo de límite** | **Sintaxis** | **Ejemplo** |
|---|---|---|
| Comodín no acotado | `?` | `List<?> a = new ArrayList<String>();` |
| Comodín con límite superior | `? extends type` | `List<? extends Exception> a = new ArrayList<RuntimeException>();` |
| Comodín con límite inferior | `? super type` | `List<? super Exception> a = new ArrayList<Object>();` |

#### Creando Comodines No Acotados

Un comodín no acotado **representa** cualquier tipo de dato. **Se usa** `?` cuando **se quiere** especificar que cualquier tipo **está bien**. Supongamos que **se quiere** escribir un método que busca en una lista de cualquier tipo.

```Java
public static void printList(List<Object> list) {
    for (Object x: list)
        System.out.println(x);
}
public static void main(String[] args) {
    List<String> keywords = new ArrayList<>();
    keywords.add("java");
    printList(keywords); // NO COMPILA
}
```

Un momento. ¿Qué está mal? Un `String` **es** una subclase de un `Object`. Esto **es** verdad. Sin embargo, `List<String>` no **puede ser** asignado a `List<Object>`. Java intenta protegernos de nosotros mismos con este. Imagínense si **se pudiera** escribir código como este:

```Java
4: List<Integer> numbers = new ArrayList<>();
5: numbers.add(Integer.valueOf(42));
6: List<Object> objects = numbers; // NO COMPILA
7: objects.add("forty two");
8: System.out.println(numbers.get(1));
```

En la línea 4, el compilador promete que solo **aparecerán** objetos `Integer` en `numbers`. Si la línea 6 compilara, la línea 7 rompería esa promesa poniendo un `String` allí ya que `numbers` y `objects` **son** referencias al mismo objeto.

Volviendo a imprimir una lista, no **se puede** asignar un `List<String>` a un `List<Object>`. Está bien; no **se necesita** un `List<Object>`. Lo que realmente **se necesita** es una `List` de "cualquier cosa". Eso es lo que `List<?>` **es**. El siguiente código hace lo que **se espera**:

```Java
public static void printList(List<?> list) {
    for (Object x: list)
        System.out.println(x);
}
public static void main(String[] args) {
    List<String> keywords = new ArrayList<>();
    keywords.add("java");
    printList(keywords);
}
```

El método `printList()` toma cualquier tipo de lista como parámetro. La variable `keywords` **es** de tipo `List<String>`. ¡Hay coincidencia! `List<String>` **es** una lista de cualquier cosa. "Cualquier cosa" resulta ser un `String` aquí.

Finalmente, **se analiza** el impacto de `var`. ¿Cree que estas dos sentencias **son** equivalentes?

```Java
List<?> x1 = new ArrayList<>();
var x2 = new ArrayList<>();
```

No lo **son**. Hay dos diferencias clave. Primero, `x1` **es** de tipo `List`, mientras que `x2` **es** de tipo `ArrayList`. Adicionalmente, solo **se puede** asignar `x2` a un `List<Object>`. Estas dos variables tienen una cosa en común. Ambas devuelven tipo `Object` al llamar al método `get()`.

#### Creando Comodines con Límite Superior

**Se intenta** escribir un método que suma el total de una lista de números.

```Java
ArrayList<Number> list = new ArrayList<Integer>(); // NO COMPILA
```

En cambio, **se necesita** usar un comodín:

```Java
List<? extends Number> list = new ArrayList<Integer>();
```

El comodín con límite superior dice que cualquier clase que extiende `Number` o el propio `Number` **puede ser** usada como el parámetro de tipo formal:

```Java
public static long total(List<? extends Number> list) {
    long count = 0;
    for (Number number: list)
        count += number.longValue();
    return count;
}
```

Algo interesante sucede cuando **se trabaja** con límites superiores o comodines no acotados. La lista **se vuelve** lógicamente inmutable y por lo tanto no **puede ser** modificada. Técnicamente, **se pueden** eliminar elementos de la lista, pero el examen no **preguntará** sobre esto.

```Java
2: static class Sparrow extends Bird { }
3: static class Bird { }
4:
5: public static void main(String[] args) {
6:     List<? extends Bird> birds = new ArrayList<Bird>();
7:     birds.add(new Sparrow()); // NO COMPILA
8:     birds.add(new Bird());    // NO COMPILA
9: }
```

El problema **viene** del hecho de que Java no sabe qué tipo **es** `List<? extends Bird>` realmente. Podría **ser** `List<Bird>` o `List<Sparrow>` o algún otro tipo genérico que ni siquiera **se ha escrito** aún. La línea 7 no compila porque no **se puede** agregar un `Sparrow` a `List<? extends Bird>`, y la línea 8 no compila porque no **se puede** agregar un `Bird` a `List<? extends Sparrow>`. Desde el punto de vista de Java, ambos escenarios **son** igualmente posibles, por lo que ninguno **está permitido**.

**Hay que notar** que **se usó** la palabra clave `extends` en lugar de `implements`. Los límites superiores **son** como las clases anónimas en que usan `extends` independientemente de si **se está** trabajando con una clase o una interfaz.

#### Creando Comodines con Límite Inferior

**Se intenta** escribir un método que agrega la cadena `"quack"` a dos listas.

```Java
List<String> strings = new ArrayList<>();
strings.add("tweet");

List<Object> objects = new ArrayList<Object>(strings);
addSound(strings);
addSound(objects);
```

El problema **es** que **se quiere** pasar un `List<String>` y un `List<Object>` al mismo método. Primero, **hay que** asegurarse de entender por qué los primeros tres ejemplos en la Tabla 9.16 *no* resuelven este problema.

**TABLA 9.16** Por qué **se necesita** un límite inferior

| `static void addSound(list) { list.add("quack"); }` | **¿El método compila?** | **¿Puede pasar un `List<String>`?** | **¿Puede pasar un `List<Object>`?** |
|---|---|---|---|
| `List<?>` | No | Sí | Sí |
| `List<? extends Object>` | No | Sí | Sí |
| `List<Object>` | Sí | No (con genéricos, debe coincidir exactamente) | Sí |
| `List<? super String>` | Sí | Sí | Sí |

Para resolver este problema, **se necesita** usar un límite inferior.

```Java
public static void addSound(List<? super String> list) {
    list.add("quack");
}
```

Con un límite inferior, **se le está diciendo** a Java que la lista **será** una lista de objetos `String` o una lista de algunos objetos que **son** una superclase de `String`. De cualquier manera, **es** seguro agregar un `String` a esa lista.

> **Entendiendo los Supertipos Genéricos**
>
> Cuando **se tienen** subclases y superclases, los límites inferiores **pueden ponerse** complicados.
>
> ```Java
> 3: List<? super IOException> exceptions = new ArrayList<Exception>();
> 4: exceptions.add(new Exception()); // NO COMPILA
> 5: exceptions.add(new IOException());
> 6: exceptions.add(new FileNotFoundException());
> ```
>
> La línea 3 referencia una `List` que podría **ser** `List<IOException>` o `List<Exception>` o `List<Object>`. La línea 4 no compila porque podríamos tener un `List<IOException>`, y un objeto `Exception` no cabría allí.
>
> La línea 5 **está bien**. `IOException` **puede ser** agregado a cualquiera de esos tres tipos. La línea 6 también **está bien**. `FileNotFoundException` también **puede ser** agregado a cualquiera de esos tres tipos. Esto **es** complicado porque `FileNotFoundException` **es** una subclase de `IOException`, y la palabra clave dice `super`. Java dice, "Bueno, `FileNotFoundException` también resulta ser un `IOException`, así que todo está bien."

### Integrándolo Todo

En este punto, **se conoce** todo lo necesario para responder con éxito las preguntas del examen sobre genéricos. Es posible combinar estos conceptos para escribir código bastante confuso, algo que le encanta hacer al examen.

Esta sección va a ser difícil de leer. Contiene las preguntas más complejas que te podrían hacer sobre genéricos. Las preguntas reales del examen probablemente serán más fáciles de leer que estas. El objetivo es enfrentar los casos realmente difíciles aquí para estar preparado para el examen. En otras palabras, ¡no entres en pánico! Tómalo con calma y lee el código un par de veces. Lo lograrás.

#### Combinación de Declaraciones Genéricas

**Se probará** con un ejemplo. Primero, **se declaran** tres clases que usará el ejemplo:

```Java
class A {}
class B extends A {}
class C extends B {}
```

¿Listo? Intenta averiguar por qué estas líneas compilan o no compilan, y qué hace cada una.

```Java
6: List<?> list1 = new ArrayList<A>();
7: List<? extends A> list2 = new ArrayList<A>();
8: List<? super A> list3 = new ArrayList<A>();
```

- La **línea 6** crea un `ArrayList` que puede almacenar instancias de la clase `A`. Se asigna a una variable con un comodín sin límite (_unbounded wildcard_). Cualquier tipo genérico puede ser referenciado desde un comodín sin límite, por lo que esto está bien.
- La **línea 7** intenta almacenar una lista en una declaración de variable con un comodín con límite superior (_upper-bounded wildcard_). Esto está bien. **Se puede** tener `ArrayList<A>`, `ArrayList<B>` o `ArrayList<C>` almacenado en esa referencia.
- La **línea 8** también está bien. Esta vez **se tiene** un comodín con límite inferior (_lower-bounded wildcard_). El tipo más bajo que se puede referenciar es `A`. Dado que ese es el tipo que **se tiene**, el código compila.

¿Respondiste correctamente? **Se intentará** con unas cuantas más:

```Java
9:  List<? extends B> list4 = new ArrayList<A>(); // NO COMPILA
10: List<? super B> list5 = new ArrayList<A>();
11: List<?> list6 = new ArrayList<? extends A>(); // NO COMPILA
```

- La **línea 9** tiene un comodín con límite superior que permite que se referencie `ArrayList<B>` o `ArrayList<C>`. Dado que es un `ArrayList<A>` lo que se intenta referenciar, el código no compila.
- La **línea 10** tiene un comodín con límite inferior, lo cual permite una referencia a `ArrayList<B>`, `ArrayList<A>` o `ArrayList<Object>`.
- Finalmente, la **línea 11** permite una referencia a cualquier tipo genérico ya que es un comodín sin límite. El problema es que **se necesita** saber cuál será ese tipo al instanciar el `ArrayList` (no se puede usar el comodín `?` con `new`). Tampoco sería útil de todos modos, porque no **se pueden** añadir elementos a ese `ArrayList`. 
#### Paso de Argumentos Genéricos

Ahora pasemos a los métodos. La misma pregunta: intenta averiguar por qué no compilan o qué hacen. **Se presentarán** los métodos uno por uno porque hay más cosas que considerar.

```Java
<T> T first(List<? extends T> list) {
    return list.get(0);
}
```

El primer método, `first()`, es un uso perfectamente normal de los genéricos. Utiliza un parámetro de tipo específico del método, `T`. Toma un parámetro de tipo `List<T>`, o alguna subclase de `T`, y devuelve un único objeto de ese tipo `T`. Por ejemplo, **se podría** llamar con un parámetro `List<String>` y hacer que devuelva un `String`. O **se podría** llamar con un parámetro `List<Number>` y hacer que devuelva un `Number`. O bueno... ya se entiende la idea.

Teniendo eso en cuenta, debería ser fácil ver qué está mal con el siguiente:

```Java
<T> <? extends T> second(List<? extends T> list) { // NO COMPILA
    return list.get(0);
}
```

El siguiente método, `second()`, no compila porque el tipo de retorno no es realmente un tipo válido. Como desarrollador que escribe el método, **se debe** saber qué tipo se supone que devuelve. No se permite especificar un comodín (`?`) como el tipo de retorno de un método.

Ahora ten cuidado, ¡este siguiente es un truco especialmente engañoso!

```Java
<B extends A> B third(List<B> list) {
    return new B(); // NO COMPILA
}
```

Este método, `third()`, no compila. `<B A extends>` dice que **se quiere** usar `B` como un parámetro de tipo solo para este método y que necesita extender la clase `A`. Coincidentemente, `B` también es el nombre de una clase existente. Bueno, no es una coincidencia, es un truco malintencionado. Dentro del alcance del método, `B` representa un parámetro de tipo genérico (que podría ser la clase `A`, `B` o `C`, ya que todas extienden de `A`). Dado que `B` ya no se refiere directamente a la clase concreta `B` dentro del método, no **se puede** instanciar (`new B()` no es válido para un parámetro de tipo genérico).

Después de eso, sería agradable ver algo más directo:

```Java
void fourth(List<? super B> list) {}
```

Finalmente **se obtiene** un método, `fourth()`, que es un uso normal de genéricos. **Se puede** pasar un tipo `List<B>`, `List<A>` o `List<Object>`.

Por último, ¿puedes averiguar por qué este ejemplo no compila?

```Java
<X> void fifth(List<X super B> list) { } // NO COMPILA
```

Este último método, `fifth()`, no compila porque intenta mezclar un parámetro de tipo específico del método con un comodín. Un comodín obligatoriamente debe incluir un signo de interrogación `?`.

¡Uf! Has superado la sección de genéricos. Es el tema más difícil de este capítulo (¡por eso **se cubrió** al final!). Recuerda que no hay problema si **se necesita** repasar este material un par de veces para comprenderlo por completo.
