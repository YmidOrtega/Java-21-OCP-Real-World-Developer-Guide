**1. J.** El código no compila. En las declaraciones `switch` con coincidencia de patrones (pattern matching), el orden importa. En este ejemplo, la línea 47 (el caso `default`) domina a la línea 48 (`case null`), lo que lleva a código inalcanzable en la línea 48, haciendo que la opción J sea la respuesta correcta. Para más información, ver Capítulo 3.

**2. B.** Inicialmente, `moon` se asigna a 9, mientras que `star` se asigna a 8 (la multiplicación tiene precedencia: 2 + (2*3)). Dado que `star` no es mayor que 10, `sun` se asigna a 3, que se promueve a `3.0f`. El valor de `jupiter` es `(3.0f + 9) - 1.0`, que es `11.0`. En la última asignación, `moon` se decrementa de 9 a 8 (`--moon`), por lo que 8 es menor o igual a 8, y `mars` se establece en 2. La salida final es 3.0, 11.0, 2. Opción B es correcta. Ver Capítulo 2.

**3. C, E.** La clase `Executors` tiene un método de fábrica para trabajar con hilos virtuales: `newVirtualThreadPerTaskExecutor()`, lo que hace correcta la opción C. No hay constructor público para hilos virtuales. Hay un método de fábrica `Thread.ofVirtual()`, lo que hace correcta la opción E. Ver Capítulo 13.

**4. D.** El código compila. Sin embargo, la fuente es un `Stream` infinito (`Stream.generate`). La operación `filter` verificará cada elemento a su vez para ver si alguno no está vacío. Como nada pasa el filtro, el código no termina. Opción D es correcta. Ver Capítulo 10.

**5. B.** El código compila exitosamente. El valor de `a` no puede ser cambiado por el método `addToInt()` porque solo se pasa una copia de la variable primitiva `x`. Por lo tanto, `a` no cambia y la salida es 15. Opción B correcta. Ver Capítulo 5.

**6. C.** Java usará `Penguin_en.properties` como el resource bundle coincidente en la línea 7 (ya que no hay coincidencia para francés `fr`, se usa el default `en_US`). La línea 8 encuentra una clave coincidente en `Penguin_en.properties` ("Willy"). La línea 9 no encuentra coincidencia en `Penguin_en.properties`, por lo que tiene que buscar más arriba en la jerarquía en `Penguin.properties`, encontrando "1". Esto hace que la opción C sea la respuesta. Ver Capítulo 11.

**7. D, E.** El array puede usar un inicializador anónimo. Los resultados de `binarySearch` son indefinidos ya que el array no está ordenado, por lo que A y B son incorrectas. La opción D es correcta porque `compare()` devuelve 0 cuando los arrays son de la misma longitud y tienen los mismos elementos. La opción E es correcta porque `mismatch()` devuelve -1 cuando los arrays son equivalentes. Ver Capítulo 4.

**8. C, E, F.** La opción A es incorrecta porque la interfaz debería ser `BiPredicate`. La línea 6 requiere saber que `negate()` es un método de conveniencia en `Predicate`, haciendo correcta la opción E. La línea 7 no toma parámetros y no devuelve nada, lo que la hace un `Runnable` (opción F). La línea 8 toma dos parámetros y devuelve un int, lo que coincide con `Comparator` (opción C). `Comparable` es incorrecto porque solo toma un parámetro. Ver Capítulo 8.

**9. D.** Si este fuera un archivo `module-info.java` válido, debería colocarse en el directorio raíz del módulo (Opción A). Sin embargo, un módulo no puede usar el modificador de acceso `public`. La opción D es correcta porque el archivo proporcionado no compila. Ver Capítulo 12.

**10. F, H.** Un método dentro de una clase abstracta sin modificador de acceso tiene acceso de paquete. La clase implementa el método default de la interfaz que es implícitamente público, lo que significa que la línea 17 reduce la visibilidad, causando un error (Opción F). La línea 23 tampoco compila porque una clase local solo puede acceder a variables locales que sean final o efectivamente final; `length` se modifica en la línea 27, por lo que no es efectivamente final (Opción H). Ver Capítulo 7.

**11. C, E, F.** El método `jump()` tiene acceso de paquete, por lo que solo puede ser accedido desde el mismo paquete. `Tadpole` no está en el mismo paquete que `Frog`, lo que causa errores en las líneas 7 y 10 (Opciones C y F). El método `ribbit()` es `protected`, accesible desde una subclase. La línea 6 está bien, pero la línea 9 no compila (Opción E) porque la referencia de la variable es a `Frog`, lo cual no otorga acceso al método protegido desde otro paquete. Ver Capítulo 5.

**12. C, D, E.** La declaración `mySet` define un límite superior de tipo `RuntimeException`. Esto significa que las clases pueden especificar `RuntimeException` o cualquier subclase. `Exception` es una superclase, por lo que B es incorrecta. El comodín `?` no puede ocurrir en el lado derecho de la asignación (A y F incorrectas). C, D y E compilan. Ver Capítulo 9.

**13. D, E.** La declaración en las líneas 9-11 incluye una `IOException` chequeada no manejada (Opción D). La línea 12 no compila porque `is.readObject()` debe ser casteado a `Bird` y también lanza `ClassNotFoundException` e `IOException` que no están manejadas (Opción E). Si se corrigiera, imprimiría `null` porque el campo `age` es `transient`. Ver Capítulo 14.

**14. B.** La línea 14 no compila (`var` no permitido en campos de instancia). Líneas 15 y 18 no compilan (`case` y `new` son palabras reservadas). Línea 17 no compila (un solo guion bajo `_` no puede ser un identificador). Línea 19 no compila (`var` no puede ser tipo de retorno). Solo la línea 16 compila (`var` puede ser nombre de método). Opción B es la respuesta. Ver Capítulo 1.

**15. C, F.** La opción C es correcta porque `mismatch()` lanza una excepción si los archivos no existen. La opción F es correcta porque se devuelve el primer índice que difiere. "Hello" vs "Howdy": H(0) igual, e(1) vs o(1) difiere. El índice es 1. Ver Capítulo 14.

**16. F.** La clase `Amphibian` está marcada como `final`, lo que significa que la línea 3 (que intenta extenderla) provoca un error de compilación. Por lo tanto, ninguna opción hace que compile. Opción F correcta. Ver Capítulo 6.

**17. C.** El código compila. Analizando el bucle:

1. Iteración externa i=0. Interna i=1, x=6. x>10 falso. x=10, j=1.
2. Iteración interna (j=1<=2). i=2, x=11. x>10 verdadero -> `break INNER`.
3. Iteración externa (i=2). i=3, x=12. `break INNER`.
4. Iteración externa (i=3). Condición i<3 falsa. Termina. Se imprime 12. Opción C correcta. Ver Capítulo 3.

**18. C.** El bloque de texto tiene el cierre `"""` en una línea separada, lo que agrega una nueva línea al final (descarta D, E, F). Los bloques de texto no comienzan con una nueva línea inicial (descarta A y B). `.indent(1)` agrega un espacio a cada línea y normaliza el final de línea. Opción C es correcta. Ver Capítulo 1.

**19. C.** Solo los módulos nombrados requieren `module-info.java`. Los módulos automáticos siempre exportan todos los paquetes. Opción C correcta. Ver Capítulo 12.

**20. I.** El código compila (G incorrecta). Línea 26 no lanza excepción porque `TreeMap` (t) es modificable; `u` es la vista inmodificable, pero aquí modificamos `t`. `TreeMap` es un `SequencedMap`, pero las operaciones `putFirst` y `putLast` lanzan `UnsupportedOperationException` en `TreeMap` porque romperían el ordenamiento. Por lo tanto, la opción correcta es I (Ninguna de las anteriores, o más bien, que lanza excepción en líneas 27/28, no 26). Ver Capítulo 9.

**21. C, F.** La opción A parece referencia a método pero no es válida. `Predicate` (error en explicación del libro, el código usa `Function<Integer, Boolean>`) toma un parámetro y devuelve boolean. Lambda con un parámetro puede omitir paréntesis (C correcta). El `return` es opcional si es una sola declaración, pero si se usa llaves `{}` es obligatorio el punto y coma (F correcta). Opción B incorrecta (falta `return`). D y E incorrectas por usar `int` en lugar de `Integer` (autoboxing no funciona para inferir tipos en lambdas de esta manera). Ver Capítulo 8.

**22. D.** `s1` y `s2` son iguales (pool de strings). 1º print: true. 2º print: true. `s3` es nuevo objeto, pero `s4 = s3.intern()` devuelve la referencia del pool ("Java"), que es `s1`. 4º print: true. `sb1.toString()` crea nuevo objeto, `==` es false. `.equals()` compara contenido, es true. Total 4 veces true. Opción D. Ver Capítulo 4.

**23. C.** `Reindeer(5)` llama implícitamente a `super()` (constructor sin args de `Deer`), imprimiendo "Deer". Luego imprime "Reindeer". `hasHorns()` es polimórfico, se llama la versión de `Reindeer` (true). Resultado: "DeerReindeer, true". Opción C. Ver Capítulo 6.

**24. B, F.** Llamar a `get()` en un `Optional` vacío lanza excepción (B correcta). `filter(x -> x < 5)` con `Stream.of(5, 10)` elimina todos los elementos, dejando el Optional vacío, por lo que `get()` lanza excepción (F correcta). Ver Capítulo 10.

**25. C, D.** `Music` no compila (variables de instancia no permitidas en record fuera de la cabecera). `Song` no compila (constructor compacto no puede asignar variables de instancia explícitamente). `Dance` compila (clases selladas en mismo archivo pueden omitir `permits` si las subclases están ahí, pero aquí `Ballet` extiende `Dance` explícitamente). `March` compila (`switch` con patrones). `Ballet` no compila (constructor normal en clase, falta nombre del método o es constructor malformado, los records tienen constructor compacto, las clases no). `NewDance` no compila (debe ser `final`, `sealed` o `non-sealed` al extender una clase sellada). Opciones C y D son las que compilan. Ver Capítulo 7.

**26. B, D.** A no compila (resultado double a int). B compila (long a double implícito). C no compila (falta el `:` del operador ternario). D compila (cast explícito). E no compila (`L` con decimal). F no compila (guion bajo junto a punto decimal). Ver Capítulo 2.

**27. F.** El código compila. El ejecutor tiene un solo hilo, pero la barrera (`CyclicBarrier`) espera 3 partes. La primera tarea ocupará el único hilo y esperará en la barrera. Las otras tareas nunca comenzarán porque el hilo está ocupado. El programa se bloquea (deadlock). Opción F correcta. Ver Capítulo 13.

**28. F.** Línea 5: `FileNotFoundException` (L12) es chequeada y no se declara ni captura. Línea 7: `StringBuilder` no es `AutoCloseable`, no se puede usar en try-with-resources. Línea 10: `RuntimeException` es subclase de `Exception`, no se pueden poner juntas en multi-catch (redundante). 3 errores. Opción F correcta. Ver Capítulo 11.