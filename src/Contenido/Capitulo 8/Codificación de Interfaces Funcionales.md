Anteriormente en este capítulo, **se declaró** la interfaz `CheckTrait`, que tiene exactamente un método para que los implementadores escriban. Las lambdas tienen una relación especial con tales interfaces. De hecho, estas interfaces tienen un nombre. Una **interfaz funcional** (_functional interface_) es una interfaz que contiene un único método abstracto. El amigo "Sam" puede ayudar a recordar esto porque oficialmente **se conoce** como la regla del **método abstracto único** (_single abstract method, SAM_).

#### Definiendo una Interfaz Funcional

**Se analizará** un ejemplo de una interfaz funcional y una clase que la implementa:

```Java
@FunctionalInterface
public interface Sprint {
    public void sprint(int speed);
}

public class Tiger implements Sprint {
    public void sprint(int speed) {
        System.out.println("Animal is sprinting fast! " + speed);
    }
}
```

En este ejemplo, la interfaz `Sprint` es una interfaz funcional porque contiene exactamente un método abstracto, y la clase `Tiger` es una clase válida que implementa la interfaz.

> **La Anotación `@FunctionalInterface`**
>
> La anotación `@FunctionalInterface` le indica al compilador que **se tiene** la intención de que el código sea una interfaz funcional. Si la interfaz no sigue las reglas de una interfaz funcional, el compilador dará un error.
>
> ```Java
> @FunctionalInterface  // NO COMPILA
> public interface Dance {
>     void move();
>     void rest();
> }
> ```
>
> Java incluye `@FunctionalInterface` en algunas, pero no en todas, las interfaces funcionales. Esta anotación significa que los autores de la interfaz prometen que será seguro usarla en una lambda en el futuro. Sin embargo, el hecho de que no **se vea** la anotación no significa que no sea una interfaz funcional. **Se debe recordar** que tener exactamente un método abstracto es lo que la hace una interfaz funcional, no la anotación.

**Se considerarán** las siguientes cuatro interfaces. Dada la interfaz funcional `Sprint` anterior, ¿cuáles de las siguientes son interfaces funcionales?

```Java
public interface Dash extends Sprint {}

public interface Skip extends Sprint {
    void skip();
}

public interface Sleep {
    private void snore() {}
    default int getZzz() { return 1; }
}

public interface Climb {
    void reach();
    default void fall() {}
    static int getBackUp() { return 100; }
    private static boolean checkHeight() { return true; }
}
```

Las cuatro son interfaces válidas, pero no todas son interfaces funcionales. La interfaz `Dash` es una interfaz funcional porque extiende la interfaz `Sprint` y hereda el único método abstracto `sprint()`. La interfaz `Skip` no es una interfaz funcional válida porque tiene dos métodos abstractos: el método heredado `sprint()` y el método declarado `skip()`.

La interfaz `Sleep` tampoco es una interfaz funcional válida. Ni `snore()` ni `getZzz()` cumplen con el criterio de un único método abstracto. Aunque los métodos `default` funcionan como métodos abstractos en el sentido de que pueden ser sobrescritos en una clase que implementa la interfaz, son insuficientes para satisfacer el requisito del método abstracto único.

Finalmente, la interfaz `Climb` es una interfaz funcional. A pesar de definir una cantidad de métodos `static`, `private` y `default`, contiene solo un método abstracto: `reach()`.

#### Agregando Métodos de Object

Todas las clases heredan ciertos métodos de `Object`. Para el examen, **se deben** conocer las siguientes firmas de métodos de `Object`:

- `public String toString()`
- `public boolean equals(Object)`
- `public int hashCode()`

**Se menciona** esto ahora porque hay una excepción a la regla del método abstracto único que **se debe** conocer. Si una interfaz funcional incluye un método abstracto con la misma firma que un método `public` encontrado en `Object`, *esos métodos no cuentan para la prueba del método abstracto único*. La motivación detrás de esta regla es que cualquier clase que implemente la interfaz heredará de `Object`, como todas las clases lo hacen, y por lo tanto siempre implementará estos métodos.

> Dado que Java asume que todas las clases extienden de `Object`, tampoco **se puede** declarar un método de interfaz que sea incompatible con `Object`. Por ejemplo, declarar un método abstracto `int toString()` en una interfaz no compilaría ya que la versión de `Object` del método devuelve un `String`.

**Se analizará** un ejemplo. ¿Es la clase `Soar` una interfaz funcional?

```Java
public interface Soar {
    abstract String toString();
}
```

No lo es. Dado que `toString()` es un método `public` implementado en `Object`, no cuenta para la prueba del método abstracto único. Por otro lado, la siguiente implementación de `Dive` es una interfaz funcional:

```Java
public interface Dive {
    String toString();
    public boolean equals(Object o);
    public abstract int hashCode();
    public void dive();
}
```

El método `dive()` es el único método abstracto, mientras que los otros no **se cuentan** ya que son métodos `public` definidos en la clase `Object`.

**Se debe tener cuidado** con ejemplos que se parecen a los métodos de la clase `Object` pero que en realidad no están definidos en ella. ¿**Se ve** por qué el siguiente no es una interfaz funcional válida?

```Java
public interface Hibernate {
    String toString();
    public boolean equals(Hibernate o);
    public abstract int hashCode();
    public void rest();
}
```

A pesar de parecerse mucho a la interfaz `Dive`, la interfaz `Hibernate` usa `equals(Hibernate)` en lugar de `equals(Object)`. Dado que esto no coincide con la firma del método `equals(Object)` definido en la clase `Object`, esta interfaz **se considera** como si contuviera dos métodos abstractos: `equals(Hibernate)` y `rest()`.
