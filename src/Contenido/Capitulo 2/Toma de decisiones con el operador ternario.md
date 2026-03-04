El último operador con el que se debe estar familiarizado para el examen es el **operador condicional** `(? :)`, también conocido como **operador ternario**. Es notable porque es el único operador que toma tres operandos. El operador ternario tiene la siguiente forma:

```Java
expresionBooleana ? expresion1 : expresion2
```

El primer operando debe ser una expresión booleana, y el segundo y tercer operando pueden ser cualquier expresión que devuelva un valor. La operación ternaria es realmente una forma condensada de una declaración combinada `if` y `else` que devuelve un valor. Se cubren las declaraciones `if/else` con mucho más detalle en el Capítulo 3, por lo que por ahora solo se utilizarán ejemplos simples.

Por ejemplo, considere el siguiente fragmento de código que calcula la cantidad de comida para un búho:

```Java
int owl = 5;
int food;
if(owl < 2) {
    food = 3;
} else {
    food = 4;
}
System.out.println(food); // 4
```

Compare el fragmento de código anterior con el siguiente fragmento de código con operador ternario:

```Java
int owl = 5;
int food = owl < 2 ? 3 : 4;
System.out.println(food); // 4
```

Estos dos fragmentos de código son equivalentes. Note que a menudo es útil para la legibilidad agregar paréntesis alrededor de las expresiones en operaciones ternarias, aunque hacerlo ciertamente no es obligatorio. Sin embargo, es especialmente útil cuando se utilizan múltiples operadores ternarios juntos. Considere las siguientes dos expresiones equivalentes:

```Java
int food1 = owl < 4 ? owl > 2 ? 3 : 4 : 5;
int food2 = (owl < 4 ? ((owl > 2) ? 3 : 4) : 5);
```

Si bien son equivalentes, se encuentra la segunda declaración mucho más legible. Dicho esto, es posible que el examen utilice múltiples operadores ternarios en una sola línea.

Para el examen, se debe saber que no existe el requisito de que la segunda y tercera expresiones en operaciones ternarias tengan los mismos tipos de datos, aunque esto sí entra en juego cuando se combinan con el operador de asignación.

Compare las dos declaraciones que siguen a la declaración de la variable:

```Java
int stripes = 7;
System.out.print((stripes > 5) ? 21 : "Zebra");
int animal = (stripes < 9) ? 3 : "Horse"; // NO COMPILA
```

Ambas expresiones evalúan valores booleanos similares y devuelven un `int` y un `String`, aunque solo la primera compilará. A `System.out.print()` no le importa que las expresiones sean de tipos completamente diferentes, porque puede convertir ambas a valores `Object` y llamar a `toString()` en ellas. Por otro lado, el compilador sí sabe que `"Horse"` es del tipo de datos incorrecto y no puede asignarse a un `int`; por lo tanto, no permite que el código sea compilado.

### Expresión ternaria y efectos secundarios no realizados

Como se vio con los operadores condicionales, una expresión ternaria puede contener un **efecto secundario no realizado**, ya que solo una de las expresiones en el lado derecho se evaluará en tiempo de ejecución. Se ilustrará este principio con el siguiente ejemplo:

```Java
int sheep = 1;
int zzz = 1;
int sleep = zzz < 10 ? sheep++ : zzz++;
System.out.print(sheep + "," + zzz); // 2,1
```

Note que, dado que la expresión booleana del lado izquierdo fue verdadera, solo se incrementó `sheep`. Contraste el ejemplo anterior con la siguiente modificación:

```Java
int sheep = 1;
int zzz = 1;
int sleep = sheep >= 10 ? sheep++ : zzz++;
System.out.print(sheep + "," + zzz); // 1,2
```

Ahora que la expresión booleana del lado izquierdo se evalúa como falsa, solo se incrementa `zzz`. De esta manera, se observa cómo las expresiones en un operador ternario pueden no aplicarse si la expresión particular no se utiliza.

Para el examen, se debe tener cuidado con cualquier pregunta que incluya una expresión ternaria en la que se modifique una variable en una de las expresiones del lado derecho.