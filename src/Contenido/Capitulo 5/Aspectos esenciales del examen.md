- Ser capaz de identificar declaraciones de métodos correctas e incorrectas. Ser capaz de ver la firma de un método y saber si es correcta, si contiene elementos inválidos o conflictivos, o si contiene elementos en el orden incorrecto.

- Identificar cuándo un método o campo es accesible. Reconocer cuándo un método o campo es accesible cuando el modificador de acceso es: `private`, paquete (omitido), `protected` o `public`.

- Comprender cómo declarar y usar variables `final`. Las variables locales, de instancia y estáticas pueden declararse como `final`. Ser capaz de comprender cómo declararlas y cómo pueden (o no) ser utilizadas.

- Ser capaz de detectar variables efectivamente finales (_effectively final_). Las variables efectivamente finales son variables locales que no se modifican después de ser asignadas. Dada una variable local, ser capaz de determinar si es efectivamente final.
 
- Reconocer usos válidos e inválidos de las importaciones estáticas (_static imports_). Las importaciones estáticas importan miembros estáticos. Se escriben como `import static`, no `static import`. Hay que asegurarse de que están importando métodos o variables estáticas en lugar de nombres de clases.

- Aplicar _autoboxing_ y _unboxing_. El proceso de convertir automáticamente de un valor primitivo a una clase envolvente se llama _autoboxing_, mientras que el proceso recíproco se llama _unboxing_. Prestar atención a la `NullPointerException` al realizar un _unboxing_ de un `null`.
 
- Indicar la salida del código que involucra métodos. Identificar cuándo llamar a métodos estáticos en lugar de métodos de instancia según si el nombre de la clase o un objeto va antes del método. Reconocer que los métodos de instancia pueden llamar a métodos estáticos y que los métodos estáticos necesitan una instancia del objeto para llamar a un método de instancia.

- Reconocer el método sobrecargado correcto. Las coincidencias exactas se utilizan primero, seguidas de primitivos más amplios (promoción), luego por el _autoboxing_, y por último los _varargs_. Asignar nuevos valores a los parámetros del método no cambia al invocador, pero llamar a métodos sobre ellos sí puede hacerlo.