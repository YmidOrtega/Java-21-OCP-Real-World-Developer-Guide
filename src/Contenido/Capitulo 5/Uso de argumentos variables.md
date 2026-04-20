Como se mencionó en el Capítulo 4, un método puede usar un parámetro _varargs_ (argumento variable) como si fuera un arreglo (_array_). Crear un método con un parámetro _varargs_ es un poco más complicado. De hecho, llamar a un método de este tipo puede no usar un arreglo en absoluto.

### Creación de métodos con _varargs_

Existen una serie de reglas importantes para crear un método con un parámetro _varargs_.

**Reglas para crear un método con un parámetro _varargs_**

1. Un método puede tener como máximo **un** parámetro _varargs_.
2. Si un método contiene un parámetro _varargs_, debe ser el **último** parámetro en la lista. 

Dadas estas reglas, ¿se puede identificar por qué cada uno de estos compila o no? (Sí, hay mucha práctica en este capítulo. Es necesario ser realmente bueno en la identificación de métodos válidos e inválidos para el examen).

```Java
public class VisitAttractions {
    public void walk1(int... steps) {}
    public void walk2(int start, int... steps) {}
    public void walk3(int... steps, int start) {} // NO COMPILA
    public void walk4(int... start, int... steps) {} // NO COMPILA
}
```

El método `walk1()` es una declaración válida con un parámetro _varargs_. El método `walk2()` es una declaración válida con un parámetro `int` y un parámetro _varargs_. Los métodos `walk3()` y `walk4()` no compilan porque tienen un parámetro _varargs_ en una posición que no es la última.

### Llamada a métodos con _varargs_

Al llamar a un método con un parámetro _varargs_, se tiene una opción. Se puede **pasar un arreglo**, o se pueden **listar los elementos** del arreglo y dejar que Java lo cree internamente. Dado el método anterior `walk1()`, que toma un parámetro _varargs_, se puede llamar de dos maneras:

```Java
// Pasar un arreglo
int[] data = new int[] {1, 2, 3};
walk1(data);

// Pasar una lista de valores
walk1(1, 2, 3);
```

Independientemente de cuál se utilice para llamar al método, el método recibirá un arreglo que contiene los elementos. Esto se puede reforzar con el siguiente ejemplo:

```Java
public void walk1(int... steps) {
    int[] step2 = steps; // No es necesario, pero muestra que steps es de tipo int[]
    System.out.print(step2.length);
}
```

Incluso se pueden omitir los valores _varargs_ en la llamada al método, y Java creará un arreglo de longitud cero.

```Java
walk1();
```

### Acceso a los elementos de un _vararg_

Acceder a un parámetro _varargs_ es exactamente como acceder a un arreglo. Utiliza la indexación de arreglos. Aquí hay un ejemplo:

```Java
16: public static void run(int... steps) {
17:     System.out.print(steps[1]);
18: }
19: public static void main(String[] args) {
20:     run(11, 77); // 77
21: }
```

La línea 20 llama a un método _varargs_ con dos parámetros. Cuando se llama al método, ve un arreglo de tamaño 2. Dado que los índices están basados en cero, se imprime `77` (el segundo elemento).

### Uso de _varargs_ con otros parámetros de método

¡Por fin! Se llega a hacer algo más que identificar si las declaraciones de métodos son válidas. En su lugar, se observarán las llamadas a métodos. ¿Se puede deducir por qué cada llamada a método emite lo que hace? Por ahora, siéntase libre de ignorar el modificador `static` en la declaración del método `walkDog()`; esto se cubrirá más adelante en el capítulo.

```Java
1: public class DogWalker {
2:     public static void walkDog(int start, int... steps) {
3:         System.out.println(steps.length);
4:     }
5:     public static void main(String[] args) {
6:         walkDog(1);                   // 0
7:         walkDog(1, 2);                // 1
8:         walkDog(1, 2, 3);             // 2
9:         walkDog(1, new int[] {4, 5}); // 2
10:    } 
   }
```

- La línea 6 pasa `1` como `start` pero nada más. Esto significa que Java crea un arreglo de longitud 0 para `steps`. 
- La línea 7 pasa `1` como `start` y un valor más. Java convierte este único valor en un arreglo de longitud 1.
- La línea 8 pasa `1` como `start` y dos valores más. Java convierte estos dos valores en un arreglo de longitud 2.
- La línea 9 pasa `1` como `start` y un arreglo de longitud 2 directamente como `steps`.

Se ha visto que Java creará un arreglo vacío si no se pasan parámetros para un _vararg_. Sin embargo, todavía es posible pasar `null` explícitamente. El siguiente fragmento **sí compila**:

```Java
walkDog(1, null); // Desencadena una NullPointerException en walkDog()
```

Dado que `null` no es un `int`, Java lo trata como una referencia de arreglo que resulta ser `null`. Simplemente pasa el objeto arreglo `null` a `walkDog()`. Luego, el método `walkDog()` lanza una excepción porque intenta determinar la longitud (`length`) de un arreglo que es `null`.