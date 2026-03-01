Se ha aprendido que las variables locales se declaran dentro de un bloque de código. ¿Cuántas variables con alcance en este método se observan?

``` Java
public void eat(int piecesOfCheese) {
    int bitesOfCheese = 1;
}
```

Existen dos variables con alcance local. La variable `bitesOfCheese` se declara dentro del método. La variable `piecesOfCheese` es un parámetro del método. Ninguna de las dos variables puede utilizarse fuera de donde está definida.

### Limitación del alcance

Las variables locales nunca pueden tener un alcance mayor que el método en el que están definidas. Sin embargo, pueden tener un alcance menor. Considérese este ejemplo:

```Java
3: public void eatIfHungry(boolean hungry) {
4:      if (hungry) {
5:          int bitesOfCheese = 1;
6:      } // bitesOfCheese sale del alcance aquí
7:      System.out.println(bitesOfCheese); // NO COMPILA
8: }
```

La variable `hungry` tiene el alcance de todo el método, mientras que la variable `bitesOfCheese` tiene un alcance menor. Solo está disponible para su uso en la declaración `if` porque se declara dentro de ella. Cuando se observa un conjunto de llaves (`{}`) en el código, significa que se ha entrado en un nuevo bloque de código. Cada bloque de código tiene su propio alcance. Cuando hay múltiples bloques, se emparejan desde adentro hacia afuera. En este caso, el bloque de la declaración `if` comienza en la línea 4 y termina en la línea 6. El bloque del método comienza en la línea 3 y termina en la línea 8.

Dado que `bitesOfCheese` se declara en un bloque de declaración `if`, el alcance se limita a ese bloque. Cuando el compilador llega a la línea 7, señala que desconoce qué es `bitesOfCheese` y genera un error.

Recuerde que los bloques pueden contener otros bloques. Estos bloques contenidos más pequeños pueden hacer referencia a variables definidas en los bloques de alcance mayor, pero no viceversa. He aquí un ejemplo:

``` Java
16: public void eatIfHungry(boolean hungry) {
17:     if (hungry) {
18:         int bitesOfCheese = 1;
19:         {
20:             var teenyBit = true;
21:             System.out.println(bitesOfCheese);
22:         }
23:     }
24:     System.out.println(teenyBit); // NO COMPILA
25: }
```

La variable definida en la línea 18 está en alcance hasta que el bloque termina en la línea 23. Su uso en el bloque más pequeño de las líneas 19 a 22 es correcto. La variable definida en la línea 20 sale del alcance en la línea 22. Su uso en la línea 24 no está permitido.

### Rastreo del alcance

El examen intentará confundir al candidato con varias preguntas sobre el alcance. Es probable que aparezca una pregunta que parezca tratar sobre algo complejo y falle al compilar porque una de las variables está fuera de alcance.

Se probará con un ejemplo. No hay que preocuparse si aún no hay familiaridad con las declaraciones `if` o los bucles `while`. No importa lo que haga el código, ya que se está hablando de alcance. Intente determinar en qué línea entra y sale del alcance cada una de las cinco variables locales.

```Java
11: public void eatMore(boolean hungry, int amountOfFood) {
12:     int roomInBelly = 5;
13:     if (hungry) {
14:         var timeToEat = true;
15:         while (amountOfFood > 0) {
16:             int amountEaten = 2;
17:             roomInBelly = roomInBelly - amountEaten;
18:             amountOfFood = amountOfFood - amountEaten;
19:         }
20:     }
21:     System.out.println(amountOfFood);
22: }
```

Este método compila. El primer paso para determinar el alcance es identificar los bloques de código. En este caso, hay tres bloques. Esto se distingue porque hay tres conjuntos de llaves. Comenzando desde el conjunto más interno, se puede ver dónde empieza y termina el bloque del bucle `while`. Se repite este proceso para el bloque de la declaración `if` y el bloque del método. La Tabla 1.9 muestra los números de línea en los que comienza y termina cada bloque.

**TABLA 1.9** Seguimiento del alcance por bloque

|**Línea**|**Primera línea en el bloque**|**Última línea en el bloque**|
|---|---|---|
|**while**|15|19|
|**if**|13|20|
|**Método**|11|22|

Ahora que se conoce la ubicación de los bloques, se puede observar el alcance de cada variable. `hungry` y `amountOfFood` son parámetros del método, por lo que están disponibles para todo el método. Esto significa que su alcance va de la línea 11 a la 22. La variable `roomInBelly` entra en alcance en la línea 12 porque es donde se declara. Permanece dentro del alcance por el resto del método y sale del alcance en la línea 22. La variable `timeToEat` entra en alcance en la línea 14 donde se declara. Sale del alcance en la línea 20 donde termina el bloque `if`. Finalmente, la variable `amountEaten` entra en alcance en la línea 16 donde se declara. Sale del alcance en la línea 19 donde termina el bloque `while`.

Se recomienda practicar mucho esta habilidad. Identificar bloques y el alcance de las variables debe convertirse en algo natural para el examen. La buena noticia es que hay muchos ejemplos de código para practicar. Se puede observar cualquier ejemplo de código sobre cualquier tema en este proyecto y emparejar las llaves.

### Aplicación del alcance a las clases

Todo lo anterior se refería a variables locales. Afortunadamente, la regla para las variables de instancia es más sencilla: están disponibles tan pronto como se definen y duran toda la vida del objeto mismo. La regla para las variables de clase, también conocidas como `static`, es aún más fácil: entran en alcance cuando se declaran, al igual que los otros tipos de variables. Sin embargo, permanecen en alcance durante toda la vida del programa.

Se realizará un ejemplo más para asegurar el dominio del tema. Nuevamente, intente determinar el tipo de las cuatro variables y cuándo entran y salen del alcance.

```Java
1: public class Mouse {
2:      final static int MAX_LENGTH = 5;
3:      int length;
4:      public void grow(int inches) {
5:          if (length < MAX_LENGTH) {
6:              int newSize = length + inches;
7:              length = newSize;
8:          }
9:      }
10: }
```

En esta clase, existe una variable de clase, `MAX_LENGTH`; una variable de instancia, `length`; y dos variables locales, `inches` y `newSize`. La variable `MAX_LENGTH` es una variable de clase porque tiene la palabra clave `static` en su declaración. En este caso, `MAX_LENGTH` entra en alcance en la línea 2 donde se declara. Permanece en alcance hasta que el programa termina.

A continuación, `length` entra en alcance en la línea 3 donde se declara. Permanece en alcance mientras exista este objeto `Mouse`. `inches` entra en alcance donde se declara en la línea 4. Sale del alcance al final del método en la línea 9. `newSize` entra en alcance donde se declara en la línea 6. Dado que se define dentro del bloque de la declaración `if`, sale del alcance cuando ese bloque termina en la línea 8.

### Repaso del alcance

¿Quedó claro? Se revisarán las reglas sobre el alcance.

- **Variables locales:** En alcance desde la declaración hasta el final del bloque.
- **Parámetros de método:** En alcance durante la duración del método.
- **Variables de instancia:** En alcance desde la declaración hasta que el objeto es elegible para la recolección de basura (_garbage collection_).    
- **Variables de clase:** En alcance desde la declaración hasta que el programa termina.

¿No se está seguro de qué es la recolección de basura? Tranquilidad, esa es la siguiente y última sección de este capítulo.