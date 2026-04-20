Java es un lenguaje de **"paso por valor"** (_pass-by-value_). Esto significa que **se hace una copia** del valor de la variable en la llamada y el método recibe esa copia en su propio parámetro. Las reasignaciones hechas al parámetro dentro del método **no afectan** a la variable original del invocador (_caller_). Se observará un ejemplo con primitivos:

```Java
2: public static void main(String[] args) {
3:     int num = 4;
4:     newNumber(num); // Se pasa una copia del valor '4'
5:     System.out.print(num); // 4
6: }
7: public static void newNumber(int num) {
8:     num = 8; // Solo modifica la variable local (parámetro) del método
9: }
```

En la línea 3, a `num` se le asigna el valor de 4. En la línea 4, se llama a un método, pasando una copia del 4. En la línea 8, el parámetro `num` (que es una variable local nueva del método) se establece en 8. Aunque este parámetro tiene el mismo nombre que la variable en la línea 3, esto es una coincidencia. El nombre podría ser cualquier cosa. El examen a menudo utilizará el mismo nombre para intentar confundir. La variable local `num` del `main` en la línea 3 nunca cambia porque no se le hacen asignaciones directas.

#### Paso de objetos (referencias)

Ahora que se han visto los primitivos, se intentará un ejemplo con un tipo de referencia. ¿Qué se cree que emite el siguiente código?

```Java
public class Dog {
    public static void main(String[] args) {
        String name = "Webby";
        speak(name); // Se pasa una copia del valor de la referencia
        System.out.print(name);
    }
    public static void speak(String name) {
        name = "Georgette"; // Reasigna la referencia local a un nuevo String
    }
}
```

La respuesta correcta es `Webby`. Al igual que en el ejemplo de primitivos, la asignación de variable (`name = "Georgette"`) es solo al parámetro del método (que es una copia de la referencia original) y no afecta la variable de referencia del invocador. Ambas variables `name` inicialmente apuntaban al mismo objeto en el _pool_ de `String`, pero luego la referencia local en `speak` se redirigió a un objeto diferente, dejando la referencia en `main` intacta.

Note cómo se sigue hablando de **asignaciones** de variables (usar el operador `=`). Esto es distinto a cuando podemos **llamar a métodos que mutan el estado** sobre el objeto al que apuntan los parámetros. Como ejemplo, aquí hay código que llama a un método mutador en el `StringBuilder` pasado al método:

```Java
public class Dog {
    public static void main(String[] args) {
        var name = new StringBuilder("Webby");
        speak(name);
        System.out.print(name); // WebbyGeorgette
    }
    public static void speak(StringBuilder s) {
        s.append("Georgette"); // Muta el objeto apuntado por la referencia 's'
    }
}
```

En este caso, `speak()` llama a un método sobre el parámetro `s`. **No reasigna** `s` a un objeto diferente. En imagen que se muestra a continuación, se puede ver cómo el "paso por valor" se sigue utilizando. La variable `s` (parámetro) recibe una **copia del valor de la referencia** de la variable `name`. Esto significa que tanto `name` como `s` apuntan exactamente al **mismo objeto `StringBuilder`** en el _heap_. En consecuencia, los cambios de estado (mutaciones) realizados al objeto a través de cualquiera de las referencias son visibles para la otra.

![[Copiar una referencia mediante el paso por valor.png]]

A modo de repaso, Java utiliza el "paso por valor" para introducir datos en un método. Asignar un nuevo primitivo o una nueva referencia a un parámetro de método (usando `=`) no cambia la variable del invocador. Sin embargo, llamar a métodos mutadores en una referencia a un objeto (como `append`, `setX`, etc.) **sí afectará** el estado del objeto visto por el invocador, porque ambas referencias apuntan al mismo objeto subyacente.