Una vez que se ha trabajado con los objetos, es momento de desecharlos. Afortunadamente, la JVM se encarga de ello. Java proporciona un **recolector de basura** (_garbage collector_) para buscar automáticamente objetos que ya no son necesarios.

Cabe recordar que el código propio no es el único proceso que se ejecuta en un programa Java. El código Java existe dentro de una JVM, la cual incluye numerosos procesos independientes del código de la aplicación. Uno de los más importantes es el recolector de basura integrado.

Todos los objetos Java se almacenan en el **heap** (montículo) de la memoria del programa. El _heap_, también conocido como _free store_ (almacén libre), representa un gran conjunto de memoria no utilizada asignada a la aplicación Java. Si el programa continúa instanciando objetos y dejándolos en el _heap_, eventualmente se quedará sin memoria y fallará (_crash_). ¡Oh, no! Afortunadamente, la recolección de basura resuelve este problema. En las siguientes secciones, se analiza la recolección de basura.

### Comprensión de la recolección de basura

La recolección de basura se refiere al proceso de liberar automáticamente memoria en el _heap_ eliminando objetos que ya no son alcanzables en el programa. Existen muchos algoritmos diferentes para la recolección de basura, pero no es necesario conocer ninguno de ellos para el examen.

Como desarrollador, la parte más interesante de la recolección de basura es determinar cuándo se puede recuperar la memoria que pertenece a un objeto. En Java y otros lenguajes, **elegible para la recolección de basura** se refiere al estado de un objeto de no ser ya accesible en un programa y, por lo tanto, susceptible de ser recolectado.

¿Significa esto que un objeto elegible para la recolección de basura será recolectado inmediatamente? **Definitivamente no.** No se tiene control sobre cuándo se descarta realmente el objeto, pero para el examen, será necesario saber en cualquier momento dado qué objetos son elegibles para la recolección.

Se puede pensar en la elegibilidad para la recolección de basura como el envío de un paquete. Se puede tomar un artículo, sellarlo en una caja etiquetada y ponerlo en el buzón. Esto es análogo a hacer que un elemento sea elegible para la recolección. Sin embargo, el momento en que el cartero pasa a recogerlo no está bajo control propio. Por ejemplo, puede ser un día festivo postal o podría haber un evento climático severo. Incluso se puede llamar a la oficina de correos y pedir que vengan a recogerlo de inmediato, pero no hay forma de garantizar cuándo y si esto sucederá realmente. Idealmente, ¡vendrán antes de que el buzón se llene de paquetes!

Java incluye un método integrado para ayudar a soportar la recolección de basura donde se puede **sugerir** que se ejecute la recolección.

``` Java
System.gc();
```

Al igual que la oficina de correos, Java es libre de ignorar la solicitud. No se garantiza que este método realice ninguna acción.

### Rastreo de elegibilidad

¿Cómo sabe la JVM cuándo un objeto es elegible para la recolección de basura? La JVM espera pacientemente y monitorea cada objeto hasta determinar que el código ya no necesita esa memoria. Un objeto permanecerá en el _heap_ hasta que ya no sea **alcanzable**. Un objeto deja de ser alcanzable cuando ocurre una de estas dos situaciones:

- El objeto ya no tiene ninguna referencia apuntando hacia él.
- Todas las referencias al objeto han salido del alcance (_scope_).

Comprender la diferencia entre una referencia y un objeto es fundamental para entender la recolección de basura, el operador `new` y muchas otras facetas del lenguaje Java. Observe este código e intente deducir cuándo cada objeto se vuelve elegible por primera vez para la recolección de basura:

```Java
1: public class Scope {
2:      public static void main(String[] args) {
3:          String one, two;
4:          one = new String("a");
5:          two = new String("b");
6:          one = two;
7:          String three = one;
8:          one = null;
9:      } 
}
```

Cuando se presenta una pregunta sobre recolección de basura en el examen, se recomienda dibujar lo que está sucediendo. Hay mucho que rastrear mentalmente, y es fácil cometer un error simple tratando de retener todo en la memoria. Se intentará realizarlo conjuntamente. En serio. Se sugiere tomar lápiz y papel. Se esperará un momento.

¿Ya tiene el papel y lápiz? Bien, se comienza. En la línea 3, se escribe `one` y `two` (solo las palabras, sin necesidad de cajas o flechas, ya que ningún objeto ha ido al _heap_ todavía). En la línea 4, se tiene el primer objeto. Dibuje una caja con la cadena "a" dentro y una flecha desde la palabra `one` hacia esa caja. La línea 5 es similar. Dibuje otra caja con la cadena "b" dentro esta vez y una flecha desde la palabra `two`. En este punto, el trabajo debería parecerse a la siguiente figura.

![[Recolección de basura 1.png]]

En la línea 6, la variable `one` cambia para apuntar a "b". Se debe borrar o tachar la flecha desde `one` y dibujar una nueva flecha desde `one` hacia "b". En la línea 7, existe una nueva variable, por lo que se escribe la palabra `three` y se dibuja una flecha desde `three` hacia "b". Observe que `three` apunta a lo que `one` apunta en este momento y no a lo que apuntaba al principio. Esta es la razón por la que se dibujan esquemas. Es fácil olvidar detalles como ese. En este punto, el trabajo debería asemejarse a la siguiente figura.

![[Recolección de basura 2.png]]

Finalmente, se tacha la línea entre `one` y "b", dado que la línea 8 establece esta variable en `null`. Ahora bien, el objetivo era determinar cuándo los objetos se volvían elegibles por primera vez para la recolección de basura. En la línea 6, se eliminó la única flecha que apuntaba a "a", haciendo que ese objeto sea elegible para la recolección de basura. "b" mantiene flechas apuntando hacia él hasta que sale del alcance. Esto significa que "b" no sale del alcance hasta el final del método en la línea 9.

### Formato del código en el examen

No todas las preguntas incluirán declaraciones de paquetes e importaciones. No debe preocupar la ausencia de declaraciones de paquetes o importaciones a menos que se pregunte específicamente por ellas. Los siguientes son casos comunes donde no es necesario verificar las importaciones:

- Código que comienza con un nombre de clase.
- Código que comienza con una declaración de método.
- Código que comienza con un fragmento que normalmente estaría dentro de una clase o método.
- Código que tiene números de línea que no comienzan con 1.

Se encontrará código que no tiene un método. Cuando esto suceda, se debe asumir que cualquier código de soporte necesario, como el método `main()` y la definición de la clase, se escribió correctamente. Solo se pregunta si la parte del código mostrada compila al ser insertada en código circundante válido. Finalmente, recuerde que el espacio en blanco adicional no importa en la sintaxis de Java. El examen puede utilizar cantidades variables de espacio en blanco para confundir al candidato.