Ahora que **se han aprendido** las interfaces funcionales, **se usarán** para mostrar diferentes enfoques para las variables. Estas **pueden** aparecer en tres lugares con respecto a las lambdas: la lista de parámetros, las variables locales declaradas dentro del cuerpo de la lambda y las variables referenciadas desde el cuerpo de la lambda. Los tres **son** oportunidades para que el examen intente engañar. **Se explora** cada uno para que **se esté** alerta cuando aparezcan trucos.

#### Listando los Parámetros

Anteriormente en este capítulo, **se aprendió** que especificar el tipo de los parámetros **es** opcional. Además, `var` **se puede** usar en lugar del tipo específico. Eso significa que las tres declaraciones **son** intercambiables:

```Java
Predicate<String> p = x -> true;
Predicate<String> p = (var x) -> true;
Predicate<String> p = (String x) -> true;
```

El examen podría pedir que **se identifique** el tipo del parámetro lambda. En el ejemplo, la respuesta **es** `String`. ¿Cómo **se determinó** eso? Una lambda **infiere** los tipos del contexto circundante. Eso significa que **se hace** lo mismo.

En este caso, la lambda **se está asignando** a un `Predicate` que toma un `String`. Otro lugar donde buscar el tipo **es** en la firma del método. **Se intenta** otro ejemplo. ¿**Se puede** determinar el tipo de `x`?

```Java
public void whatAmI() {
    consume((var x) -> System.out.print(x), 123);
}
public void consume(Consumer<Integer> c, int num) {
    c.accept(num);
}
```

Si se adivinó `Integer`, se tuvo razón. El método `whatAmI()` crea una lambda que **se pasa** al método `consume()`. Dado que el método `consume()` espera un `Integer` como genérico, **se sabe** que ese **es** el tipo inferido de `x`.

Pero espera; hay más. En algunos casos, **se puede** determinar el tipo sin siquiera ver la firma del método. ¿Qué tipo es `x` aquí?

```Java
public void counts(List<Integer> list) {
    list.sort((var x, var y) -> x.compareTo(y));
}
```

La respuesta **es** de nuevo `Integer`. Dado que **se está** ordenando una lista, **se puede** usar el tipo de la lista para determinar el tipo del parámetro lambda.

Dado que los parámetros lambda son iguales que los parámetros de métodos, **se les pueden** añadir modificadores. Específicamente, **se puede** añadir el modificador `final` o una anotación, como **se muestra** en este ejemplo:

```Java
public void counts(List<Integer> list) {
    list.sort((final var x, @Deprecated var y) ->
        x.compareTo(y));
}
```

Aunque esto **tiende** a ser poco común en la vida real, modificadores como estos **han sido conocidos** por aparecer de pasada en el examen.

> **Formatos de Lista de Parámetros**
>
> **Se tienen** tres formatos para especificar los tipos de parámetros dentro de una lambda: sin tipos, con tipos y con `var`. El compilador requiere que todos los parámetros en la lambda usen el mismo formato. ¿**Se puede** ver por qué los siguientes no **son** válidos?
>
> ```Java
> 5: (var x, y) -> "Hello"                   // NO COMPILA
> 6: (var x, Integer y) -> true              // NO COMPILA
> 7: (String x, var y, Integer z) -> true    // NO COMPILA
> 8: (Integer x, y) -> "goodbye"             // NO COMPILA
> ```
>
> La línea 5 necesita eliminar `var` de `x` o añadirlo a `y`. A continuación, las líneas 6 y 7 necesitan usar el tipo o `var` de manera consistente. Finalmente, la línea 8 necesita eliminar `Integer` de `x` o añadir un tipo a `y`.

#### Usando Variables Locales Dentro del Cuerpo de una Lambda

Aunque **es** más común que el cuerpo de una lambda **sea** una única expresión, **es** legal definir un bloque. Ese bloque **puede** tener cualquier cosa que **sea** válida en un bloque Java normal, incluyendo declaraciones de variables locales.

El siguiente código hace exactamente eso. Crea una variable local llamada `c` con alcance en el bloque lambda:

```Java
(a, b) -> { int c = 0; return 5; }
```

Ahora **se intentará** otro. ¿**Se ve** qué está mal aquí?

```Java
(a, b) -> { int a = 0; return 5; }  // NO COMPILA
```

**Se intentó** redeclarar `a`, lo cual no **está** permitido. Java no permite crear una variable local con el mismo nombre que una ya declarada en ese alcance.

Aunque este tipo de error **es** menos probable que aparezca en la vida real, ¡**ha sido conocido** por aparecer en el examen!

Ahora **se intentará** uno difícil. ¿Cuántos errores de sintaxis **se ven** en este método?

```Java
11: public void variables(int a) {
12:     int b = 1;
13:     Predicate<Integer> p1 = a -> {
14:         int b = 0;
15:         int c = 0;
16:         return b == c; }
17: }
```

Hay tres errores de sintaxis. El primero **está** en la línea 13. La variable `a` ya **fue usada** en este alcance como parámetro del método, por lo que no **puede ser** reutilizada. El siguiente error de sintaxis **está** en la línea 14, donde el código intenta redeclarar la variable local `b`. El tercer error de sintaxis **es** bastante sutil y **está** en la línea 16. ¿**Se ve**? **Hay que** mirar muy de cerca.

A la variable `p1` le **falta** un punto y coma al final. Hay un punto y coma antes del `}`, pero ese **está** dentro del bloque. Aunque normalmente no **hay que** buscar puntos y comas que **faltan**, las lambdas **son** complicadas en este espacio, ¡así que **hay que** tener cuidado!

> **Escenario del Mundo Real: Mantén tus Lambdas Cortas**
>
> Tener una lambda con múltiples líneas y una sentencia `return` **es** a menudo una señal de que **se debería** refactorizar y poner ese código en un método. Por ejemplo, el ejemplo anterior **podría ser** reescrito de la siguiente manera:
>
> ```Java
> Predicate<Integer> p1 = a -> returnSame(a);
> ```
>
> Esta forma más simple **puede ser** refactorizada aún más para usar una referencia a método:
>
> ```Java
> Predicate<Integer> p1 = this::returnSame;
> ```
>
> **Se podría** estar preguntando por qué esto **es** tan importante. En el Capítulo 10, las lambdas y las referencias a métodos **se usan** en llamadas a métodos encadenados. Cuanto más corta **sea** la lambda, más fácil **es** leer el código.

#### Referenciando Variables desde el Cuerpo de la Lambda

Los cuerpos de las lambdas **pueden** referenciar algunas variables del código circundante. El siguiente código **es** legal:

```Java
public class Crow {
    private String color;
    public void caw(String name) {
        String volume = "loudly";
        Consumer<String> consumer = s ->
                System.out.println(name + " says "
                        + volume + " that she is " + color);
    }
}
```

Esto muestra que una lambda **puede** acceder a una variable de instancia, un parámetro del método o una variable local bajo ciertas condiciones. Las variables de instancia (y las variables de clase) **siempre están** permitidas.

Lo único que las lambdas no **pueden** acceder **son** variables que no **son** `final` ni efectivamente finales (_effectively final_). Si **se necesita** un repaso sobre efectivamente final, **se debe ver** el Capítulo 5, "Métodos".

**Se vuelve** aún más interesante cuando **se mira** dónde ocurren los errores del compilador cuando las variables no **son** efectivamente finales.

```Java
2:  public class Crow {
3:      private String color;
4:      public void caw(String name) {
5:          String volume = "loudly";
6:          name = "Caty";
7:          color = "black";
8:
9:          Consumer<String> consumer = s ->
10:             System.out.println(name + " says "     // NO COMPILA
11:                     + volume + " that she is " + color);  // NO COMPILA
12:         volume = "softly";
13:     }
14: }
```

En este ejemplo, el parámetro del método `name` no **es** efectivamente final porque **se establece** en la línea 6. Sin embargo, el error del compilador **ocurre** en la línea 10. No **es** un problema asignar un valor a una variable no `final`. Sin embargo, una vez que la lambda intenta usarla, **se tiene** un problema. La variable ya no **es** efectivamente final, por lo que la lambda no **puede** usarla.

La variable `volume` tampoco **es** efectivamente final ya que **se actualiza** en la línea 12. En este caso, el error del compilador **está** en la línea 11. ¡Eso **es** antes de la reasignación! De nuevo, el acto de asignar un valor solo **es** un problema desde el punto de vista de la lambda. Por lo tanto, la lambda **es** la que tiene que generar el error del compilador.

Para revisar, **hay que** asegurarse de que **se ha** memorizado la Tabla 8.8.

**TABLA 8.8** Reglas para acceder a una variable desde el cuerpo de una lambda dentro de un método

| **Tipo de variable** | **Regla** |
|---|---|
| Variable de instancia | Permitida |
| Variable estática | Permitida |
| Variable local | Permitida si es `final` o efectivamente final |
| Parámetro del método | Permitida si es `final` o efectivamente final |
| Parámetro lambda | Permitida |
