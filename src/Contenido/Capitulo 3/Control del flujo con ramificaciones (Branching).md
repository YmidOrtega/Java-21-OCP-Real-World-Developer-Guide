Los últimos tipos de estructuras de flujo de control que se cubren en este capítulo son las **sentencias de ramificación**. Hasta ahora, se ha estado lidiando con bucles simples que terminaban solo cuando su expresión booleana se evaluaba como `false`. Ahora se mostrarán otras formas en que los bucles podrían terminar, o ramificarse, y se verá que el camino tomado durante el tiempo de ejecución puede no ser tan directo como en los ejemplos anteriores.

### Bucles anidados

Antes de pasar a las sentencias de ramificación, es necesario introducir el concepto de bucles anidados. Un **bucle anidado** es un bucle que contiene otro bucle, incluyendo bucles `while`, `do/while`, `for` y `for-each`. Por ejemplo, considere el siguiente código que itera sobre un arreglo bidimensional, que es un arreglo que contiene otros arreglos como sus miembros. Se cubrirán los arreglos en detalle en el Capítulo 4, «APIs principales», pero por ahora, asuma que la siguiente es la forma en que se declararía un arreglo de arreglos:

```Java
int[][] myComplexArray = {{5,2,1,3}, {3,9,8,9}, {5,7,12,7}};
for (int[] mySimpleArray : myComplexArray) {
    for (int i = 0; i < mySimpleArray.length; i++) {
        System.out.print(mySimpleArray[i]+"\t");
    }
    System.out.println();
}
```

Note que intencionalmente se mezclan un bucle `for` y un bucle `for-each` en este ejemplo. El bucle externo se ejecutará un total de tres veces. Cada vez que se ejecuta el bucle externo, el bucle interno se ejecuta cuatro veces. Al ejecutar este código, se observa la siguiente salida:

```Plaintext
5 2 1 3
3 9 8 9
5 7 12 7
```

Los bucles anidados pueden incluir `while` y `do/while`, como se muestra en este ejemplo. Intente determinar qué emitirá este código:

```Java
int hungryHippopotamus = 8;
while (hungryHippopotamus > 0) {
    do {
        hungryHippopotamus -= 2;
    } while (hungryHippopotamus > 5);
    hungryHippopotamus--;
    System.out.print(hungryHippopotamus + ", ");
}
```

La primera vez que este bucle se ejecuta, el bucle interno se repite hasta que el valor de `hungryHippopotamus` es 4. El valor se decrementará a 3, y esa será la salida al final de la primera iteración del bucle externo.

En la segunda iteración del bucle externo, el `do/while` interno se ejecutará una vez, aunque `hungryHippopotamus` ya no sea mayor que 5. Como tal vez se recuerde, las sentencias `do/while` siempre ejecutan el cuerpo al menos una vez. Esto reducirá el valor a 1, que será disminuido aún más por el operador de decremento en el bucle externo a 0. Una vez que el valor alcance 0, el bucle externo terminará. El resultado es que el código emitirá lo siguiente:

`3, 0,`

Los ejemplos en el resto de esta sección incluyen muchos bucles anidados. También se encontrarán bucles anidados en el examen, por lo que cuanta más práctica se tenga con ellos, más preparado se estará.

### Adición de etiquetas opcionales (_Labels_)

Una cosa que intencionalmente se omitió cuando se presentaron las sentencias `if`, sentencias `switch` y bucles es que todos pueden tener **etiquetas opcionales**. Una etiqueta es un puntero opcional a la cabecera de una sentencia que permite que el flujo de la aplicación salte hacia ella o salga de ella. Es un identificador único seguido de dos puntos (`:`). Por ejemplo, se pueden añadir etiquetas opcionales a uno de los ejemplos anteriores:

```Java
int[][] myComplexArray = {{5,2,1,3}, {3,9,8,9}, {5,7,12,7}};
OUTER_LOOP: for (int[] mySimpleArray : myComplexArray) {
    INNER_LOOP: for (int i = 0; i < mySimpleArray.length; i++) {
        System.out.print(mySimpleArray[i]+"\t");
    }
    System.out.println();
}
```

Las etiquetas siguen las mismas reglas de formato que los identificadores. Para mayor legibilidad, se muestran en `snake_case`, con letras mayúsculas y guiones bajos entre palabras. Cuando se trata de un solo bucle, las etiquetas no añaden ningún valor, pero como se aprende en la siguiente sección, son extremadamente útiles en estructuras anidadas.

Aunque este tema no está en el examen, es posible añadir etiquetas opcionales a sentencias de control y de bloque. Por ejemplo, lo siguiente está permitido por el compilador, aunque es extremadamente inusual:

```Java
int frog = 15;
BAD_IDEA: if (frog > 10)
EVEN_WORSE_IDEA: {
    frog++;
}
```

### La sentencia `break`

Como se vio al trabajar con sentencias `switch`, una sentencia `break` transfiere el flujo de control **fuera** de la sentencia envolvente. Lo mismo ocurre con una sentencia `break` que aparece dentro de un bucle `while`, `do/while` o `for`, ya que terminará el bucle prematuramente, como se muestra a continuación.

![[La estructura de una instrucción break.png]]

Note que en la imagen la sentencia `break` puede tomar un parámetro de etiqueta opcional. Sin un parámetro de etiqueta, la sentencia `break` terminará el bucle interno más cercano que esté actualmente en proceso de ejecución. El parámetro de etiqueta opcional permite salir de un bucle externo de nivel superior. En el siguiente ejemplo, se busca la primera posición de índice de arreglo `(x, y)` de un número dentro de un arreglo bidimensional no ordenado:

```Java
10: public class FindInMatrix {
11:     public static void main(String[] args) {
12:         int[][] list = {{1,13}, {5,2}, {2,2}};
13:         int searchValue = 2;
14:         int positionX = -1;
15:         int positionY = -1;
16:
17:         PARENT_LOOP: for (int i = 0; i < list.length; i++) {
18:             for (int j = 0; j < list[i].length; j++) {
19:                 if (list[i][j] == searchValue) {
20:                     positionX = i;
21:                     positionY = j;
22:                     break PARENT_LOOP;
23:                 }
24:             }
25:         }
26:         if (positionX == -1 || positionY == -1) {
27:             System.out.print("Value "+searchValue+" not found");
28:         } else {
29:             System.out.print("Value "+searchValue+" found at: " +
30:                 "("+positionX+","+positionY+")");
31:         }
32:     } 
33: }
```

Cuando se ejecuta, este código emitirá lo siguiente:

`Value 2 found at: (1,1)`

En particular, observe la sentencia `break PARENT_LOOP`. Esta sentencia saldrá de **toda** la estructura del bucle tan pronto como se encuentre el primer valor coincidente. Ahora, imagínese qué sucedería si se reemplazara el cuerpo del bucle interno con lo siguiente:

```Java
19: if (list[i][j]==searchValue) {
20:     positionX = i;
21:     positionY = j;
22:     break;
23: }
```

¿Cómo cambiaría esto el flujo y cambiaría la salida? En lugar de salir cuando se encuentra el primer valor coincidente, el programa ahora **solo saldría del bucle interno** cuando se cumpliera la condición. En otras palabras, la estructura encontraría el _primer_ valor coincidente del _último_ bucle interno que contuviera el valor, resultando en la siguiente salida:

`Value 2 found at: (2,0)`

Finalmente, ¿qué pasaría si se eliminara el `break` por completo?

```Java
19: if (list[i][j]==searchValue) {
20:     positionX = i;
21:     positionY = j;
22:
23: }
```

En este caso, el código buscaría el **último** valor en toda la estructura que tuviera el valor coincidente. La salida se vería así:

`Value 2 found at: (2,1)`

Se puede ver a partir de este ejemplo que usar una etiqueta en una sentencia `break` en un bucle anidado, o no usar la sentencia `break` en absoluto, puede hacer que la estructura del bucle se comporte de manera muy diferente.

### La sentencia `continue`

Ahora se ampliará la discusión sobre el control avanzado de bucles con la sentencia `continue`, una sentencia que hace que el flujo finalice la ejecución de la iteración **actual** del bucle, como se muestra a continuación.

![[La estructura de una instrucción continue.png]]

Es posible notar que la sintaxis de la sentencia `continue` refleja la de la sentencia `break`. De hecho, las sentencias son idénticas en cómo se utilizan, pero con resultados diferentes. Mientras que la sentencia `break` transfiere el control a la sentencia envolvente, la sentencia `continue` transfiere el control a la **expresión booleana** que determina si el bucle debería continuar. En otras palabras, **finaliza la iteración actual** del bucle. Además, al igual que la sentencia `break`, la sentencia `continue` se aplica al bucle interno más cercano en ejecución, utilizando sentencias de etiqueta opcionales para anular este comportamiento.

Se examinará un ejemplo. Imagínese que se tiene un cuidador de zoológico que debe limpiar el primer leopardo en cada uno de cuatro establos, pero saltar el establo 'b' por completo.

```Java
1: public class CleaningSchedule {
2:     public static void main(String[] args) {
3:         CLEANING: for (char stables = 'a'; stables<='d'; stables++) {
4:             for (int leopard = 1; leopard <= 3; leopard++) {
5:                 if (stables=='b' || leopard==2) {
6:                     continue CLEANING;
7:                 }
8:                 System.out.println("Cleaning: "+stables+","+leopard);
9:             } 
10:         } 
11:     } 
12: }
```

Con la estructura tal como está definida, el bucle devolverá el control al bucle padre en cualquier momento en que el primer valor sea 'b' o el segundo valor sea 2. En la primera, tercera y cuarta ejecuciones del bucle externo, el bucle interno imprime una sentencia exactamente una vez y luego sale en el siguiente bucle interno cuando `leopard` es 2. En la segunda ejecución del bucle externo, el bucle interno sale inmediatamente sin imprimir nada ya que se encuentra 'b' de inmediato. Se imprime lo siguiente:

```Plaintext
Cleaning: a,1
Cleaning: c,1
Cleaning: d,1
```

Ahora, imagínese que se elimina la etiqueta `CLEANING` en la sentencia `continue` para que el control se devuelva al bucle interno en lugar del externo. La línea 6 se convierte en lo siguiente:

```Java
6: continue;
```

Esto corresponde a que el cuidador del zoológico limpie a todos los leopardos excepto a los etiquetados con 2 o en el establo 'b'. La salida es entonces la siguiente:

```Plaintext
Cleaning: a,1
Cleaning: a,3
Cleaning: c,1
Cleaning: c,3
Cleaning: d,1
Cleaning: d,3
```

Finalmente, si se elimina la sentencia `continue` y la sentencia `if` asociada por completo eliminando las líneas 5-7, se llega a una estructura que emite todos los valores, como esto:

```Plaintext
Cleaning: a,1
Cleaning: a,2
Cleaning: a,3
...
Cleaning: d,3
```

### La sentencia `return`

Dado que este proyecto no debería ser la primera incursión en la programación, se espera que se hayan encontrado métodos que contengan sentencias `return`. Independientemente, se cubrirá cómo diseñar y crear métodos que las utilicen en detalle en el Capítulo 5.

Por ahora, sin embargo, se debe estar familiarizado con la idea de que la creación de métodos y el uso de sentencias `return` pueden usarse como una **alternativa** al uso de etiquetas y sentencias `break`. Por ejemplo, observe esta reescritura de la clase `FindInMatrix` anterior:

```Java
public class FindInMatrixUsingReturn {
    private static int[] searchForValue(int[][] list, int v) {
        for (int i = 0; i < list.length; i++) {
            for (int j = 0; j < list[i].length; j++) {
                if (list[i][j] == v) {
                    return new int[] {i, j};
                }
            }
        }
        return null;
    }

    public static void main(String[] args) {
        int[][] list = { { 1, 13 }, { 5, 2 }, { 2, 2 } };
        int searchValue = 2;
        int[] results = searchForValue(list, searchValue);
        if (results == null) {
            System.out.print("Value " + searchValue + " not found");
        } else {
            System.out.print("Value " + searchValue + " found at: " +
                "(" + results[0] + "," + results[1] + ")");
        }
    }
}
```

Esta clase es funcionalmente la misma que la primera clase `FindInMatrix` que se vio anteriormente utilizando `break`. Si se necesita un control más detallado del bucle con múltiples sentencias `break` y `continue`, la primera clase es probablemente mejor. Dicho esto, se encuentra que el código sin etiquetas y sentencias `break` es mucho más fácil de leer y depurar. Además, hacer que la lógica de búsqueda sea una función independiente hace que el código sea más reutilizable y el método `main()` llamador mucho más fácil de leer.

Para el examen, será necesario conocer ambas formas. Solo recuerde que las sentencias `return` pueden usarse para salir de los bucles rápidamente y pueden conducir a un código más legible en la práctica, especialmente cuando se usan con bucles anidados.

### Código inalcanzable (_Unreachable Code_)

Una faceta de `break`, `continue` y `return` que se debe tener en cuenta es que cualquier código colocado inmediatamente después de ellos en el mismo bloque se considera inalcanzable y no compilará. Por ejemplo, el siguiente fragmento de código no compila:

```Java
int checkDate = 0;
while (checkDate < 10) {
    checkDate++;
    if (checkDate > 100) {
        break;
        checkDate++; // NO COMPILA
    }
}
```

Aunque no sea lógicamente posible que la sentencia `if` se evalúe como verdadera en esta muestra de código, el compilador nota que hay sentencias inmediatamente posteriores al `break` y fallará al compilar con «código inalcanzable» (_unreachable code_) como razón. Lo mismo ocurre con las sentencias `continue` y `return`, como se muestra en los siguientes dos ejemplos:

```Java
int minute = 1;
WATCH: while (minute > 2) {
    if (minute++ > 2) {
        continue WATCH;
        System.out.print(minute); // NO COMPILA
    }
}

int hour = 2;
switch (hour) {
    case 1: return; hour++; // NO COMPILA
    case 2:
}
```

Una cosa que se debe recordar es que no importa si el bucle o la estructura de decisión visita realmente la línea de código. Por ejemplo, el bucle podría ejecutarse cero o infinitas veces en tiempo de ejecución. Independientemente de la ejecución, el compilador reportará un error si encuentra algún código que considere inalcanzable, en este caso cualquier sentencia inmediatamente posterior a una sentencia `break`, `continue` o `return`.

### Revisión de ramificaciones (_Branching_)

Se concluye esta sección con la Tabla 3.1, que ayudará a recordar cuándo se permiten etiquetas y otras diversas sentencias en Java. Con fines ilustrativos, los ejemplos utilizaron estas sentencias en bucles anidados, aunque también pueden usarse dentro de bucles simples.

**TABLA 3.1** Características de sentencias de control soportadas

||**Etiquetas (Labels)**|**break**|**continue**|**yield**|**when**|
|---|---|---|---|---|---|
|`while`|Sí|Sí|Sí|No|No|
|`do/while`|Sí|Sí|Sí|No|No|
|`for`|Sí|Sí|Sí|No|No|
|`switch`|Sí|Sí|No|Sí|Sí|

Algunas de las preguntas que más tiempo consumen en el examen podrían involucrar bucles anidados con muchas ramificaciones. A menos que se pueda detectar un error del compilador de inmediato, se podría considerar saltar estas preguntas y volver a ellas al final. Recuerde, ¡todas las preguntas en el examen tienen el mismo peso!