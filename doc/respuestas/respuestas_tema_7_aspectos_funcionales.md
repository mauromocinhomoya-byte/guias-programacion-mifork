<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a una función es una variable que almacena la dirección de memoria donde comienza el código de una función, no un dato como un entero o una cadena. Tiene un tipo asociado que describe la firma de la función a la que puede apuntar: los tipos de sus parámetros y de su valor de retorno.

Gracias a esto, se puede decidir en tiempo de ejecución qué función concreta se invoca a través de esa variable, en vez de fijarlo en tiempo de compilación. Es la base para tratar el comportamiento como un dato: guardarlo en una variable, pasarlo como argumento o devolverlo como resultado.

Es, de hecho, el antecedente de las funciones lambda que se verán en este tema, aunque en C la sintaxis es más rígida y no existen closures.

c#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Función que convierte una cadena a mayúsculas
char* convertirMayusculas(char* cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "hola mundo";

    // Puntero local a la función, llamado aMayusculas
    char* (*aMayusculas)(char*) = convertirMayusculas;

    // Invocación a través del puntero
    printf("%s\n", aMayusculas(texto));

    return 0;
}


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima: no necesita nombre propio ni declararse aparte, y puede asignarse a una variable, pasarse como argumento o devolverse como resultado. Cumple el mismo papel que un puntero a función en C, representar comportamiento como un valor.

La diferencia está en la sintaxis, mucho más compacta, y en que puede capturar variables del contexto donde se define (los closures, vistos más adelante).

En JavaScript, sin tipado estático, se define con una "arrow function":

javascriptconst aMayusculas = (cadena) => cadena.toUpperCase();

console.log(aMayusculas("hola mundo"));

En Java, al tener tipado estático, la variable necesita un tipo declarado: una interfaz funcional. Por simplicidad se usa Function<String, String>, que representa una función de String a String:

javaimport java.util.function.Function;

public class Ejemplo {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        System.out.println(aMayusculas.apply("hola mundo"));
    }
}

A diferencia de una llamada normal, la lambda no se invoca como aMayusculas("hola mundo"), sino a través del método apply, el método abstracto de la interfaz Function.


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional estructura el cómputo como evaluación de funciones, evitando el estado mutable y los efectos secundarios. Frente al paradigma imperativo (secuencia de instrucciones que modifican variables) o al orientado a objetos (objetos que combinan estado y comportamiento), pone el foco en combinar funciones puras: su resultado depende solo de sus argumentos, sin modificar nada externo.

Java 8 incorporó lambdas, interfaces funcionales y streams, permitiendo un estilo funcional dentro de un lenguaje que sigue siendo orientado a objetos: clases, encapsulación, herencia y polimorfismo continúan siendo la base. Por eso se le llama "multi-paradigma": no deja de ser orientado a objetos, pero añade herramientas del paradigma funcional que se combinan con las anteriores.

Que las funciones sean "ciudadanos de primera clase" significa que se tratan como cualquier otro valor: se pueden asignar a variables, pasar como parámetros, devolver como resultado o guardar en estructuras de datos. En C esto ya era parcialmente posible con punteros a función; en Java, antes de la versión 8, solo se lograba con clases anónimas que implementaran una interfaz de un único método. Las lambdas son las que finalmente lo permiten de forma directa.


## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis general es (parámetros) -> cuerpo. A la izquierda de -> van los parámetros; a la derecha, el cuerpo, que puede ser una única expresión (su valor se devuelve automáticamente, sin return) o un bloque entre llaves con varias sentencias, donde sí hace falta return explícito.

No es obligatorio indicar el tipo de los parámetros ni el de retorno: el compilador los infiere del contexto, normalmente a partir de la interfaz funcional a la que se asigna la lambda. Si solo hay un parámetro, los paréntesis son opcionales; sin parámetros, hacen falta paréntesis vacíos ().

javaimport java.util.function.Function;
import java.util.function.Supplier;

// Un parámetro, sin paréntesis, cuerpo de una sola expresión
Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

// Un parámetro con tipo explícito y paréntesis
Function<String, String> aMayusculas2 = (String cadena) -> cadena.toUpperCase();

// Cuerpo con bloque y return explícito
Function<String, String> aMayusculasConLog = (cadena) -> {
    System.out.println("Transformando: " + cadena);
    return cadena.toUpperCase();
};

// Sin parámetros
Supplier<String> saludo = () -> "Hola";


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.
Un método puede recibir una función como parámetro igual que recibe un String o un int, porque una lambda es en realidad un objeto que implementa una interfaz funcional. Basta con declarar el parámetro con ese tipo (Function<String, String>) e invocarlo con apply dentro del método.

Esta técnica permite personalizar comportamiento sin herencia ni jerarquías de clases: en vez de crear subclases que sobrescriban un método, se pasa directamente la función deseada. Es una alternativa más ligera al patrón "Strategy".

javaimport java.util.function.Function;

public class Ejemplo {

    // Recibe un String y una función transformadora, y aplica la función
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        String resultado = transformar("hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}

En JavaScript no hace falta un método intermedio como apply; la función se invoca directamente:

javascriptfunction transformar(texto, funcion) {
    return funcion(texto);
}

const aMayusculas = (cadena) => cadena.toUpperCase();

console.log(transformar("hola mundo", aMayusculas));

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Una lambda no necesita asignarse antes a una variable: puede definirse directamente como argumento de una llamada. Esto reduce código y muestra en el propio punto de uso qué comportamiento se pasa.

En Java basta escribir la lambda dentro de los paréntesis de la llamada; el compilador infiere el tipo a partir de la firma del método:

javapublic class Ejemplo {

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        // Lambda de inversión de cadena definida directamente en la llamada
        String resultado = transformar("hola mundo", cadena -> new StringBuilder(cadena).reverse().toString());
        System.out.println(resultado);
    }
}

En JavaScript el planteamiento es el mismo:

javascriptfunction transformar(texto, funcion) {
    return funcion(texto);
}

const resultado = transformar("hola mundo", (cadena) => cadena.split("").reverse().join(""));
console.log(resultado);


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un closure es la capacidad de una función lambda de acceder a las variables del entorno donde fue definida, incluso después de que ese entorno haya dejado de existir en el flujo normal del programa. La lambda no solo lleva su código: también lleva una referencia al contexto donde se creó.

En Java, para que una variable local pueda capturarse, debe ser efectivamente final: aunque no lleve final explícito, su valor no puede cambiar tras la primera asignación. Esto es así porque la lambda captura una copia del valor, no la variable en sí.

javaimport java.util.function.Function;

public class Ejemplo {
    public static void main(String[] args) {
        String sufijo = " - procesado";

        // La lambda captura la variable local "sufijo" del contexto donde se define
        Function<String, String> concatenarSufijo = cadena -> cadena + sufijo;

        System.out.println(concatenarSufijo.apply("hola mundo"));
    }
}

Si tras definir la lambda se intentara modificar sufijo, el compilador daría error, precisamente porque deja de ser efectivamente final.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

Un puntero a función en C solo puede apuntar a una función ya existente y con nombre; no permite crear comportamiento anónimo en el punto donde se usa. Una lambda sí permite definirlo directamente, sin declararlo antes por separado.

La diferencia más importante es la ausencia de closures en C: un puntero a función no puede capturar variables locales del contexto, porque no se "crea" nada, solo se apunta a código fijo y compilado. Una lambda en Java sí lleva ese contexto capturado consigo.

Además, un puntero a función en C es un valor de bajo nivel, una dirección de memoria tipada. Una lambda en Java es, en realidad, una instancia de un objeto que implementa una interfaz funcional, por lo que se integra con el resto del sistema de tipos orientado a objetos.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.
Devolver una función desde otra función extiende la idea de closure: una función normal puede construir y devolver una lambda "a medida", con variables ya fijadas en su interior. El resultado es una función especializada que recuerda los valores con los que fue creada.

crearDescuento recibe un porcentaje y devuelve una Function<Double, Double> que resta ese porcentaje a una cantidad. El parámetro porcentaje es capturado por la lambda devuelta, de modo que cada función descuento "recuerda" el suyo propio.

javaimport java.util.function.Function;

public class Ejemplo {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // La lambda captura "porcentaje", el parámetro de crearDescuento
        return cantidad -> cantidad - (cantidad * porcentaje / 100);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento25 = crearDescuento(25);

        System.out.println(descuento10.apply(100.0)); // 90.0
        System.out.println(descuento25.apply(100.0)); // 75.0
    }
}

descuento10 y descuento25 son dos objetos distintos, cada uno con su propia copia capturada de porcentaje, aunque provienen de la misma expresión lambda. Esto no sería posible con un simple puntero a función en C sin recurrir a estructuras adicionales.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una interfaz funcional es una interfaz de Java que declara exactamente un único método abstracto. Ese método define la forma que debe tener cualquier lambda asignada a ella: número y tipo de parámetros, y tipo de retorno.

Puede tener además métodos default y métodos estáticos, ya que estos no cuentan como abstractos. Es habitual, aunque no obligatorio, marcarla con @FunctionalInterface, para que el compilador verifique que solo tiene un método abstracto.

Conecta con lo ya visto en interfaces tradicionales: del mismo modo que una interfaz normal define un contrato que varias clases implementan de formas distintas, una interfaz funcional define un contrato de un único método que distintas lambdas "implementan" sin necesidad de una clase explícita.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Crear una interfaz funcional a mano es como declarar cualquier interfaz, con la restricción de tener un único método abstracto. Transformador debe declarar un método que reciba un String y devuelva otro String, la misma forma que ha tenido aMayusculas en ejemplos anteriores.

java@FunctionalInterface
public interface Transformador {
    String transformar(String cadena);
}

Una vez definida, se usa igual que Function<String, String>: como tipo de variable o como tipo de parámetro de un método como transformar.

javapublic class Ejemplo {

    public static String transformar(String texto, Transformador funcion) {
        return funcion.transformar(texto);
    }

    public static void main(String[] args) {
        Transformador aMayusculas = cadena -> cadena.toUpperCase();

        System.out.println(transformar("hola mundo", aMayusculas));
    }
}

El nombre del método abstracto no tiene por qué coincidir con apply; es la firma, no el nombre, lo que define la compatibilidad con una lambda.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

Transformador tal como está definida solo sirve para String a String. Aplicando genericidad, como en las clases genéricas ya conocidas, se puede parametrizar con dos tipos: uno para la entrada y otro para la salida.

java@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}

Un ejemplo de uso sería redondear un Double a Integer:

javapublic class Ejemplo {

    public static <T, R> R transformar(T valor, Transformador<T, R> funcion) {
        return funcion.transformar(valor);
    }

    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = valor -> (int) Math.round(valor);

        System.out.println(transformar(3.7, redondear)); // 4
    }
}


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.
Transformador<T, R> es, en esencia, idéntica a Function<T, R>, ya incluida en java.util.function. Dado que representar "una función de un tipo a otro" es una necesidad muy común, Java trae de serie un conjunto de interfaces funcionales genéricas, evitando definir las propias salvo que se necesite un nombre más descriptivo.

javaFunction<T, R>      // Recibe un T y devuelve un R. Método: R apply(T t)
Supplier<T>         // No recibe nada y devuelve un T. Método: T get()
Consumer<T>         // Recibe un T y no devuelve nada. Método: void accept(T t)
BiConsumer<T, U>    // Recibe un T y un U y no devuelve nada. Método: void accept(T t, U u)
Predicate<T>        // Recibe un T y devuelve un boolean. Método: boolean test(T t)
BiFunction<T, U, R> // Recibe un T y un U y devuelve un R. Método: R apply(T t, U u)
UnaryOperator<T>    // Recibe un T y devuelve un T (caso particular de Function<T, T>)
BinaryOperator<T>   // Recibe dos T y devuelve un T

Cada una cubre un patrón habitual: producir sin entrada (Supplier), consumir sin producir salida (Consumer), comprobar una condición (Predicate) o transformar un valor (Function).


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

forEach, disponible en Iterable (y por tanto en List), recorre todos los elementos de una colección aplicando una acción a cada uno, sin escribir explícitamente un bucle for. Se le pasa una función con "qué hacer con cada elemento" y es el propio método quien recorre la colección.

Su firma espera un Consumer<T>, ya que su propósito es actuar sobre cada elemento sin devolver ningún resultado. Encaja bien con el caso de mostrar un mensaje si el entero es positivo.

javaimport java.util.List;

public class Ejemplo {
    public static void main(String[] args) {
        List<Integer> numeros = List.of(-3, 5, 0, 8, -1);

        numeros.forEach(numero -> {
            if (numero > 0) {
                System.out.println(numero + " es positivo");
            }
        });
    }
}

Frente al for tradicional, este estilo expresa la intención sin detallar el mecanismo de recorrido.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

La firma real es void forEach(Consumer<? super T> action), no Consumer<T>. Con ? super T se permite pasar un Consumer de T o de cualquier superclase de T, ya que un Consumer de una superclase también sabe procesar objetos T.

PECS ("Producer Extends, Consumer Super") ayuda a decidir entre ? extends y ? super con comodines. Si la estructura "produce" valores de tipo T (se leen de ella), se usa ? extends T. Si "consume" valores de tipo T (se le pasan), se usa ? super T.

Aplicado a transformar: la función recibida consume el tipo de entrada y produce el de salida. Siguiendo PECS, podría declararse como Transformador<? super T, ? extends R>, admitiendo transformadores algo más flexibles sin perder seguridad de tipos.

javapublic static <T, R> R transformar(T valor, Transformador<? super T, ? extends R> funcion) {
    return funcion.transformar(valor);
}

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Una referencia a método es una forma abreviada de escribir una lambda que se limita a invocar un método ya existente. En vez de persona -> persona.saludar(), se usa directamente la referencia con el operador ::.

Cualquier método público puede referenciarse así, sea de instancia (sobre un objeto concreto) o estático de la clase.

En JavaScript no hay sintaxis especial, pero se logra un efecto similar con bind:

javascriptclass Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const persona = new Persona("Ana");
const saludar = persona.saludar.bind(persona);
saludar();

En Java, la referencia a método de instancia sobre un objeto concreto se escribe instancia::metodo:

javapublic class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

javaimport java.util.function.Supplier;

public class Ejemplo {
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");

        // Referencia al método de instancia "saludar" de un objeto concreto
        Runnable saludar = persona::saludar;

        saludar.run();
    }
}


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

Java distingue cuatro tipos de referencias a método, según a qué apunten. Todas son formas abreviadas de una lambda que delega en un método existente, y deben ser compatibles con la firma de la interfaz funcional a la que se asignan.

Los cuatro tipos son: método estático de una clase, método de instancia de un objeto concreto, método de instancia sin objeto fijado (el objeto se recibe como primer parámetro) y referencia a constructor.

javaimport java.util.function.BiFunction;
import java.util.function.Function;
import java.util.function.Supplier;

public class Ejemplo {
    public static void main(String[] args) {

        // 1. Referencia a método estático: ClassName::metodoEstatico
        Function<String, Integer> aEntero = Integer::parseInt;
        System.out.println(aEntero.apply("42"));

        // 2. Referencia a método de instancia de un objeto concreto: instancia::metodo
        String saludo = "hola mundo";
        Supplier<String> aMayusculas = saludo::toUpperCase;
        System.out.println(aMayusculas.get());

        // 3. Referencia a método de instancia sobre cualquier instancia: ClassName::metodo
        BiFunction<String, String, Boolean> comparar = String::equalsIgnoreCase;
        System.out.println(comparar.apply("Hola", "hola"));

        // 4. Referencia a constructor: ClassName::new
        Function<String, Persona> crearPersona = Persona::new;
        Persona persona = crearPersona.apply("Ana");
    }
}

En el tercer caso, el objeto no se fija de antemano: al llamar comparar.apply("Hola", "hola"), el primer argumento pasa a ser el objeto sobre el que se invoca equalsIgnoreCase, y el segundo es el parámetro real del método.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.
Ordenar objetos requiere indicar cómo comparar dos elementos, ya que Java no sabe por defecto si una Persona es mayor o menor que otra. Collections.sort acepta un Comparator<T>, otra interfaz funcional con un único método compare(T o1, T o2), que devuelve negativo, cero o positivo.

En la primera versión, la comparación se escribe a mano: primero se comparan las edades y, si son iguales, se recurre al nombre con compareTo.

javapublic class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

javaimport java.util.*;

public class Ejemplo {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>(List.of(
            new Persona("Carlos", 30),
            new Persona("Ana", 25),
            new Persona("Beatriz", 30)
        ));

        // Versión 1: comparación manual con lambda
        Collections.sort(personas, (p1, p2) -> {
            if (p1.getEdad() != p2.getEdad()) {
                return p1.getEdad() - p2.getEdad();
            }
            return p1.getNombre().compareTo(p2.getNombre());
        });

        System.out.println(personas);
    }
}

La segunda versión usa la clase Comparator, que combina criterios sin escribir la lógica a mano. comparingInt crea un comparador a partir de la edad, y thenComparing añade el nombre como desempate.

javaimport java.util.*;

public class Ejemplo {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>(List.of(
            new Persona("Carlos", 30),
            new Persona("Ana", 25),
            new Persona("Beatriz", 30)
        ));

        // Versión 2: usando Comparator y referencias a método
        Collections.sort(personas,
            Comparator.comparingInt(Persona::getEdad)
                      .thenComparing(Persona::getNombre));

        System.out.println(personas);
    }
}

Esta versión es más declarativa: describe qué criterio de ordenación se quiere, en lugar de detallar cómo compararlo.
