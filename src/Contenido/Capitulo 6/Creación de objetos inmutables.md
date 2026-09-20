Como **se recordará** del Capítulo 4, un objeto inmutable es uno que no puede cambiar de estado después de ser creado. El patrón de objetos inmutables es un patrón de diseño orientado a objetos en el que un objeto no puede ser modificado después de su creación.

Los objetos inmutables son útiles al escribir código seguro porque no **se tiene** que preocupar de que los valores cambien. También simplifican el código al tratar con la concurrencia, ya que los objetos inmutables pueden ser compartidos fácilmente entre múltiples hilos (_threads_).

### Declarando una Clase Inmutable

Aunque hay una variedad de técnicas para escribir una clase inmutable, **se debe estar familiarizado** con una estrategia común para hacer que una clase sea inmutable:

1. Marcar la clase como `final` o hacer que todos los constructores sean `private`.
2. Marcar todas las variables de instancia como `private` y `final`.
3. No definir ningún método _setter_.
4. No permitir que se modifiquen los objetos mutables referenciados.
5. Usar un constructor para establecer todas las propiedades del objeto, haciendo una copia si es necesario.

La primera regla impide que alguien cree una subclase mutable. La segunda y tercera reglas aseguran que los llamadores (_callers_) no realicen cambios en las variables de instancia y son los sellos distintivos de un buen encapsulamiento, un tema que **se discute** junto con los _records_ en el Capítulo 7.

La cuarta regla para crear objetos inmutables es sutil. Básicamente, significa que no **se debería exponer** un método de acceso (o _getter_) para campos de instancia mutables. ¿**Se puede identificar** por qué lo siguiente crea un objeto mutable?

```Java
import java.util.*;
public final class Animal { // Declaración de objeto no inmutable
    private final ArrayList<String> favoriteFoods;
    public Animal() {
        this.favoriteFoods = new ArrayList<String>();
        this.favoriteFoods.add("Apples");
    }
    public List<String> getFavoriteFoods() {
        return favoriteFoods;
    } 
}
```

**Se siguieron** cuidadosamente las primeras tres reglas, pero desafortunadamente, un llamador malicioso aún podría modificar los datos.

```Java
var zebra = new Animal();
System.out.println(zebra.getFavoriteFoods()); // [Apples]
zebra.getFavoriteFoods().clear();
zebra.getFavoriteFoods().add("Chocolate Chip Cookies");
System.out.println(zebra.getFavoriteFoods()); // [Chocolate Chip Cookies]
```

¡Oh no! ¡Las cebras no deberían comer galletas con chispas de chocolate! ¡No es un objeto inmutable si **se puede cambiar** su contenido! Si no **se tiene** un _getter_ para el objeto `favoriteFoods`, ¿cómo acceden a él los llamadores? Simple: usando métodos delegados o envoltorios (_wrapper methods_) para leer los datos.

```Java
import java.util.*;
public final class Animal { // Declaración de un objeto inmutable
    private final List<String> favoriteFoods;
    public Animal() {
        this.favoriteFoods = new ArrayList<String>();
        this.favoriteFoods.add("Apples");
    }
    public int getFavoriteFoodsCount() {
        return favoriteFoods.size();
    }
    public String getFavoriteFoodsItem(int index) {
        return favoriteFoods.get(index);
    } 
}
```

En esta versión mejorada, los datos siguen estando disponibles. Sin embargo, es un verdadero objeto inmutable porque la variable mutable no puede ser modificada por el llamador.

> **Copia en Métodos de Acceso (Copy on Read)**
> 
> Además de delegar el acceso a cualquier objeto mutable `private`, otro enfoque es hacer una copia del objeto mutable cada vez que se solicita.
> ```Java
> public ArrayList<String> getFavoriteFoods() {
>     return new ArrayList<String>(this.favoriteFoods);
> }
> ```
> Por supuesto, los cambios en la copia no se verán reflejados en el original, pero al menos el original está protegido de cambios externos. Esta puede ser una operación costosa si es invocada frecuentemente por el llamador.

### Realizando una Copia Defensiva

Entonces, ¿de qué se trata esta quinta y última regla para crear objetos inmutables? En el diseño de una clase, **se asume** que **se desea** una regla donde los datos para `favoriteFoods` sean proporcionados por el llamador y que siempre contengan al menos un elemento. Esta regla a menudo se llama **invariante**; es cierta en cualquier momento que **se tenga** una instancia del objeto.

```Java
import java.util.*;
public final class Animal { // Declaración de objeto no inmutable
    private final ArrayList<String> favoriteFoods;
    public Animal(ArrayList<String> favoriteFoods) {
        if (favoriteFoods == null || favoriteFoods.size() == 0)
            throw new RuntimeException("favoriteFoods is required");
        this.favoriteFoods = favoriteFoods;
    }
    public int getFavoriteFoodsCount() {
        return favoriteFoods.size();
    }
    public String getFavoriteFoodsItem(int index) {
        return favoriteFoods.get(index);
    } 
}
```

Para asegurar que `favoriteFoods` sea proporcionado, **se valida** en el constructor y se lanza una excepción si no se provee. Entonces, ¿es esto inmutable? ¡No del todo! Un llamador malicioso podría ser astuto y mantener su propia referencia secreta al objeto `favoriteFoods`, la cual pueden modificar directamente.

```Java
var favorites = new ArrayList<String>();
favorites.add("Apples");
var zebra = new Animal(favorites); // El llamador aún tiene acceso a favorites
System.out.println(zebra.getFavoriteFoodsItem(0)); // [Apples]
favorites.clear();
favorites.add("Chocolate Chip Cookies");
System.out.println(zebra.getFavoriteFoodsItem(0)); // [Chocolate Chip Cookies]
```

¡Vaya! Parece que `Animal` ya no es inmutable, puesto que su contenido puede cambiar después de ser creado. La solución es hacer una copia del objeto de la lista que contenga los mismos elementos.

```Java
public Animal(List<String> favoriteFoods) {
    if (favoriteFoods == null || favoriteFoods.size() == 0)
        throw new RuntimeException("favoriteFoods is required");
    this.favoriteFoods = new ArrayList<String>(favoriteFoods);
}
```

La operación de copia se llama **copia defensiva** (_defensive copy_) porque la copia se realiza por si acaso otro código hace algo inesperado. Es la misma idea que la conducción defensiva: prevenir un problema antes de que exista. Con este enfoque, la clase `Animal` vuelve a ser inmutable.