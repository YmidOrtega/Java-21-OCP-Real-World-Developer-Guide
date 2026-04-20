- Se debe ser capaz de determinar la salida de código usando `String`. Conocer las reglas para concatenar con `String` y cómo usar métodos comunes de `String`.

- Saber que un `String` es inmutable. Prestar especial atención al hecho de que los índices están basados en cero y que el método `substring()` obtiene la cadena hasta justo **antes** del índice del segundo parámetro.

- Se debe ser capaz de determinar la salida de código usando `StringBuilder`. Saber que un `StringBuilder` es mutable y cómo usar los métodos comunes de `StringBuilder`. Saber que `substring()` **no** cambia el valor de un `StringBuilder`, mientras que `append()`, `delete()` e `insert()` **sí** lo cambian. También hay que tener en cuenta que la mayoría de los métodos de `StringBuilder` devuelven una referencia a la instancia actual de `StringBuilder`.

- Comprender la diferencia entre `==` y `equals()`. `==` verifica la igualdad de objetos (referencia). `equals()` depende de la implementación del objeto en el que se está llamando. Para la clase `String`, `equals()` verifica los caracteres dentro de ella.

- Se debe ser capaz de determinar la salida de código usando arreglos (_arrays_). Saber cómo declarar e instanciar arreglos. Ser capaz de acceder a cada elemento y saber cuándo un índice está fuera de los límites (_out of bounds_). Reconocer salidas correctas e incorrectas al buscar y ordenar.

- Identificar los tipos de retorno de los métodos de `Math`. Dependiendo del primitivo pasado, los métodos de `Math` pueden devolver diferentes resultados primitivos.

- Reconocer usos inválidos de fechas y horas. `LocalDate` no contiene campos de tiempo, y `LocalTime` no contiene campos de fecha. Hay que estar atento a operaciones que se realizan en el tipo incorrecto. También hay que prestar atención a sumar o restar tiempo e ignorar el resultado. Se debe estar cómodo con las matemáticas de fechas, incluyendo las zonas horarias y el horario de verano.