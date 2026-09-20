**Se pasó** mucho tiempo en este capítulo enseñando cómo usar las expresiones lambda, y con buena razón. Los próximos dos capítulos dependen en gran medida de la capacidad de crear y usar expresiones lambda. **Se recomienda** entender bien este capítulo antes de continuar.

Las expresiones lambda, o lambdas, permiten pasar bloques de código. La sintaxis completa **se ve** así:

```Java
(String a, String b) -> { return a.equals(b); }
```

Los tipos de los parámetros **pueden ser** omitidos. Cuando solo **se especifica** un parámetro sin un tipo, los paréntesis también **pueden ser** omitidos. Las llaves, el punto y coma y la sentencia `return` **pueden ser** omitidos para una única sentencia, haciendo que la forma corta **sea** la siguiente:

```Java
a -> a.equals(b)
```

Las lambdas **pueden ser** pasadas a un método que espera una instancia de una interfaz funcional. Una lambda **puede** definir parámetros o variables en el cuerpo siempre que sus nombres **sean** diferentes de las variables locales existentes. El cuerpo de una lambda **puede** usar cualquier variable de instancia o de clase. Adicionalmente, **puede** usar cualquier variable local o parámetro de método que **sean** `final` o efectivamente finales.

Una referencia a método **es** una sintaxis compacta para escribir lambdas que **se refieren** a métodos. Hay cuatro tipos: métodos `static`, métodos de instancia en un objeto particular, métodos de instancia en un parámetro y referencias a constructores.

Una interfaz funcional tiene un único método abstracto. Cualquier interfaz funcional **puede** ser implementada con una expresión lambda. **Se deben** conocer las interfaces funcionales integradas.

**Se deben revisar** las tablas en el capítulo. Aunque hay muchas tablas, algunas comparten patrones comunes, lo que facilita recordarlas. Absolutamente **se debe** memorizar la Tabla 8.4, que **lista** las interfaces funcionales comunes.
