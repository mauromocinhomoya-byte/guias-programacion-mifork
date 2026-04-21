<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

Para ilustrar cómo se gestionaba la universalidad antes de la llegada de la genericidad, se puede analizar el uso de tipos "comodín". En C, esto se logra mediante punteros a void, que representan una dirección de memoria sin un tipo asociado. En Java (antes de J2SE 5.0), se utilizaba la clase Object, que al ser la raíz de toda la jerarquía, puede referenciar cualquier instancia.

En ambos casos, la estructura de datos pierde la capacidad de conocer qué está almacenando exactamente. El programador asume la responsabilidad total de recordar el tipo original y realizar la conversión correspondiente al extraer los datos. Si se comete un error en este proceso, el programa fallará (un error de segmentación en C o una ClassCastException en Java), ya que el compilador no puede verificar la integridad del tipo durante la fase de construcción.

Ejemplo en C (empleando void*)
En C, un array de void* es simplemente un array de direcciones de memoria. Se debe pasar la dirección de la variable que se desea almacenar.

C
#include <stdio.h>

int main() {
    // Array de 2 punteros genéricos
    void* bolsa;

    int edad = 25;
    char* nombre = "Unidad_01";

    // Almacenamos las direcciones
    bolsa = &edad;
    bolsa = nombre;

    // Para recuperar, debemos hacer un cast explícito al tipo correcto
    int valorEdad = *(int*)bolsa;
    printf("Edad: %d, Nombre: %s\n", valorEdad, (char*)bolsa);

    return 0;
}
Ejemplo en Java (empleando Object)
En Java, un array de Object puede guardar cualquier referencia. Al recuperar el elemento, es obligatorio realizar un downcasting manual, ya que el array solo garantiza que lo que devuelve es un "objeto".

Java
public class EstructuraUniversal {
    public static void main(String[] args) {
        // Array capaz de guardar cualquier objeto
        Object[] contenedor = new Object;

        contenedor = "Mensaje de texto"; // String es un Object
        contenedor = 100;               // El int se convierte a Integer (Object)

        // Recuperación con casting manual (Peligroso)
        String texto = (String) contenedor;
        Integer numero = (Integer) contenedor;

        System.out.println("Texto: " + texto + ", Número: " + numero);
    }
}
Este enfoque, aunque funcional, es propenso a errores. Si se intentara convertir contenedor a un Integer, el compilador no daría ningún aviso, pero el programa se detendría abruptamente durante la ejecución. La genericidad moderna elimina este riesgo al permitir que el compilador "vigile" el tipo de dato que entra y sale de la estructura.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es un paradigma que permite escribir algoritmos y estructuras de datos de forma abstracta, sin especificar de antemano el tipo de dato concreto sobre el que operarán. Su objetivo es crear código reutilizable que sea independiente del tipo, pero que mantenga la seguridad de tipos. En lugar de escribir una función para sumar enteros y otra para sumar reales, se escribe una única lógica "genérica" que el lenguaje adapta según se necesite.

Respecto al ejemplo anterior con void* u Object, la respuesta es no en el sentido moderno del término. Aunque ese enfoque permite la "reutilización" para cualquier tipo, se considera más bien programación con tipos universales o borrado manual de tipos. La diferencia técnica es crucial: en la programación genérica real (como los Generics de Java), el compilador conoce y valida el tipo que estás usando, mientras que con Object o void*, el compilador "pierde la vista" de qué hay dentro y la responsabilidad de que el programa no explote recae totalmente en el programador.

Por tanto, mientras que el uso de Object es una solución basada en la herencia para lograr flexibilidad, la programación genérica utiliza parámetros de tipo (como <T>) para lograr esa misma flexibilidad pero con la garantía de que, si intentas meter un Soldado donde esperas una Caja<String>, el código ni siquiera llegará a compilar.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El empleo de void* en C o Object en Java para crear estructuras genéricas traslada la responsabilidad de la validación de tipos del compilador al programador. El principal problema es la ausencia de tipado fuerte durante la fase de compilación: el lenguaje permite insertar cualquier dato en la estructura sin rechistar. Esto crea un escenario de "fe ciega" donde el código compila perfectamente aunque estemos mezclando peras con manzanas, postergando la detección de errores fatales al momento en que el usuario final ejecuta la aplicación.

Otro inconveniente crítico es la necesidad de realizar conversiones explícitas o castin al recuperar los datos. Como la estructura solo devuelve un puntero genérico o una referencia a la clase raíz, el desarrollador debe forzar el tipo manualmente. Si el programador asume erróneamente que un elemento es un Soldado cuando en realidad era un String, se producirá un fallo catastrófico (violación de segmento en C o ClassCastException en Java). Estos errores son especialmente difíciles de depurar en sistemas grandes, ya que el fallo ocurre en el punto de extracción y no en el punto donde se insertó el dato incorrecto.

Finalmente, este enfoque degrada la legibilidad y el mantenimiento del código. Al observar la firma de un método o la declaración de un array, no hay ninguna pista sobre qué se supone que debe contener. Esto obliga a documentar exhaustivamente el código o a confiar en la memoria del equipo de desarrollo, lo cual es propenso a fallos. La falta de semántica en los tipos hace que las herramientas de autocompletado y los análisis estáticos de los IDE sean mucho menos eficaces, aumentando la probabilidad de introducir "bugs" lógicos que podrían haberse evitado con una simple comprobación de tipos genérica.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son marcadores de posición o "variables de tipo" que se utilizan en la declaración de clases, interfaces o métodos genéricos. En lugar de especificar un tipo de dato concreto (como int o Soldado), se emplea un identificador —por convención, una sola letra mayúscula como <T> para Type, <E> para Element o <K, V> para Key/Value—. Estos parámetros actúan como una promesa: indican que, aunque ahora no sepamos el tipo exacto, este será definido y validado en el momento en que el programador cree una instancia de la clase o llame al método.

La función principal de estos parámetros es establecer una relación de dependencia lógica entre diferentes partes del código sin comprometerse con una clase específica. Por ejemplo, en una clase genérica, se puede usar el parámetro <T> para definir que el atributo que se guarda, el argumento que recibe el método set y el valor que devuelve el método get deben ser siempre del mismo tipo. Esto permite que el compilador "sustituya" mentalmente la T por el tipo real proporcionado por el usuario, realizando las comprobaciones de seguridad pertinentes.

A diferencia del uso de Object, donde la identidad del dato se pierde en una jerarquía universal, los parámetros de tipo preservan la información del tipo original a lo largo de toda la estructura. Esto elimina la necesidad de realizar casting manual, ya que el compilador garantiza que, si se declaró una Caja<Soldado>, el método obtener() devolverá siempre un Soldado. Es, en esencia, una forma de metaprogramación donde se escribe una plantilla de código que el compilador ajusta con precisión quirúrgica para cada tipo de dato solicitado.

Este mecanismo permite que el código sea extremadamente flexible pero, a la vez, estrictamente tipado. Un parámetro de tipo asegura que cualquier error de inconsistencia (intentar tratar un número como un texto) sea detectado en tiempo de compilación. Para un programador acostumbrado a la libertad peligrosa de C, esto supone un cambio de paradigma: el compilador deja de ser un simple traductor de código para convertirse en un asistente de seguridad que valida la coherencia lógica de las estructuras de datos.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

Tanto en Java como en C++, el uso de herramientas genéricas permite crear contenedores dinámicos que garantizan que todos sus elementos pertenecen a un tipo específico. Esto elimina la necesidad de realizar conversiones manuales y permite que el compilador detecte errores si se intenta insertar un tipo de dato incorrecto. En ambos lenguajes, la sintaxis utiliza los corchetes angulares < > para especificar el tipo de dato que el contenedor manejará.

En Java, se utiliza la clase ArrayList<T> del paquete java.util. Al declarar ArrayList<String>, se le indica al compilador que esta lista solo aceptará cadenas de texto. Una ventaja clave es que, al recorrerla con un bucle "for-each", no es necesario realizar ningún casting; el lenguaje extrae los elementos directamente como objetos de tipo String, permitiendo el acceso inmediato a todos sus métodos.

En C++, se emplea la clase std::vector<T> de la biblioteca estándar (<vector>). A diferencia de Java, C++ genera código específico para cada tipo (plantillas), lo que resulta en una eficiencia máxima de ejecución. Al usar un iterador o un bucle basado en rangos con el tipo std::string, se tiene la seguridad de que cada posición del vector contiene una instancia real de dicho tipo, gestionada de forma contigua en memoria.

Ejemplo en Java (Generics)
Java
import java.util.ArrayList;

public class EjemploGenerics {
    public static void main(String[] args) {
        // Instanciación de una lista que solo admite String
        ArrayList<String> lista = new ArrayList<>();

        lista.add("Alfa");
        lista.add("Bravo");
        lista.add("Charlie");

        // Recorrido seguro: cada elemento se recibe como String
        for (String s : lista) {
            // Se puede usar s.length() directamente sin casting
            System.out.println("Elemento: " + s + " (Longitud: " + s.length() + ")");
        }
    }
}
Ejemplo en C++ (Templates)
C++
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Instanciación de un vector dinámico de Strings
    std::vector<std::string> vectorStrings;

    vectorStrings.push_back("Delta");
    vectorStrings.push_back("Echo");
    vectorStrings.push_back("Foxtrot");

    // Recorrido seguro usando un bucle de rango
    for (const std::string& s : vectorStrings) {
        // El tipo de 's' es estrictamente std::string
        std::cout << "Elemento: " << s << " (Tamaño: " << s.size() << ")" << std::endl;
    }

    return 0;
}
En ambos fragmentos de código, la seguridad de tipos es absoluta. Si se intentara añadir un número entero a estas listas, el compilador detendría el proceso de construcción del programa, algo que no ocurriría si se estuvieran utilizando los métodos antiguos de void* o Object. Esta validación temprana es el beneficio más tangible de la programación genérica en entornos profesionales.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Cuando un compilador encuentra la instanciación de una clase con parámetros de tipo, su comportamiento varía drásticamente dependiendo del lenguaje. En ambos casos, el objetivo inicial es el mismo: verificar que los tipos utilizados cumplen con las restricciones definidas y asegurar que no existan inconsistencias. Sin embargo, una vez superada la fase de validación, la forma en que el código se transforma para ser ejecutado por la máquina sigue filosofías totalmente opuestas.

En C++, se produce lo que se conoce como instanciación de plantillas. Por cada tipo de dato diferente con el que se use una plantilla, el compilador genera una copia física y específica de todo el código de esa clase en el binario. Si se crea un vector<int> y un vector<double>, el ejecutable final contendrá dos versiones separadas de la lógica del vector. Esto permite una optimización máxima, ya que cada versión trabaja con el tamaño exacto y las instrucciones específicas de su tipo, pero a cambio aumenta el tamaño del archivo ejecutable.

En Java, el proceso es el denominado Type Erasure (borrado de tipos). El compilador utiliza la información de los genéricos para validar la seguridad durante el desarrollo, pero la elimina por completo antes de generar el bytecode. Toda referencia a los parámetros de tipo (como T) se sustituye por su límite superior (habitualmente Object) y se insertan conversiones automáticas (casts) invisibles para el programador. Esto significa que, a diferencia de C++, en tiempo de ejecución solo existe una única copia del código para todas las versiones de la clase genérica.

La razón de esta diferencia es histórica. Java introdujo los genéricos en su versión 5.0 y necesitaba que el código nuevo fuera compatible con las máquinas virtuales y librerías antiguas que no los soportaban; el borrado de tipos permitió que el bytecode fuera idéntico al de las versiones previas. C++, al no depender de una máquina virtual y priorizar el rendimiento, optó por la especialización del código, lo que otorga a las plantillas una potencia técnica superior (como el uso de tipos primitivos o constantes en los parámetros) que los genéricos de Java no pueden alcanzar debido a su naturaleza borrada.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

Para implementar una estructura que maneje dos tipos de datos potencialmente distintos, se define una clase con dos parámetros de tipo, convencionalmente denominados <T, U>. Esta aproximación es extremadamente útil en situaciones donde un método necesita devolver más de un valor, superando la limitación de Java de tener un único valor de retorno por función. Al utilizar parámetros de tipo, se mantiene la relación lógica entre ambos valores sin necesidad de crear clases específicas para cada combinación posible de datos.

En la clase Par, cada atributo se declara con uno de los parámetros de tipo definidos en la cabecera. El constructor se encarga de inicializar estas referencias y los métodos de acceso (getters) devuelven los tipos exactos especificados en la instanciación. Este diseño garantiza que, si se crea un par de Double y String, el compilador sabrá exactamente qué tipo de dato devuelve cada método, eliminando la ambigüedad y la necesidad de conversiones manuales.

Implementación de la clase Par y ejemplo de uso
Java
// Definición de la clase con dos parámetros de tipo independientes
public class Par<T, U> {
    private final T primero;
    private final U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}

// Clase de utilidad para cálculos estadísticos
class Estadistica {
    // El método devuelve un Par que agrupa dos resultados de tipo Double
    public static Par<Double, Double> calcularMetricas(double[] datos) {
        double suma = 0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;

        double sumaCuadrados = 0;
        for (double d : datos) sumaCuadrados += Math.pow(d - media, 2);
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        return new Par<>(media, desviacion);
    }
}

// Ejemplo de ejecución
public class Main {
    public static void main(String[] args) {
        double[] valores = {10.0, 12.0, 23.0, 8.0, 15.0};
        
        // Se recibe el resultado con seguridad de tipos
        Par<Double, Double> resultado = Estadistica.calcularMetricas(valores);
        
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}
En el ejemplo propuesto, se observa cómo la clase Par permite "empaquetar" la media y la desviación típica de forma compacta. Aunque en este caso ambos tipos son Double, la estructura es lo suficientemente flexible como para que el mismo objeto Par pudiera utilizarse para emparejar, por ejemplo, un Integer (código de error) y un String (mensaje de error). El uso de final en los atributos de Par es una práctica recomendada en programación genérica para asegurar que el par sea inmutable una vez creado.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Los métodos genéricos permiten aplicar la genericidad de forma puntual sin necesidad de que toda la clase sea genérica. Para declararlos, se sitúa el parámetro de tipo (como <T>) antes del tipo de retorno en la firma del método. Esta técnica es especialmente útil en métodos de utilidad o librerías donde se desea procesar datos manteniendo su identidad específica, permitiendo que el compilador infiera el tipo de dato basándose en los argumentos pasados durante la llamada.

Cuando se utiliza un parámetro de tipo en lugar de la clase Object, el compilador establece un vínculo contractual entre los argumentos y el valor de retorno. En el caso del método seleccionaUno, al usar <T>, se garantiza que el objeto devuelto sea exactamente del mismo tipo que los que entraron. Esto permite que el programador asigne el resultado directamente a una variable del tipo concreto sin necesidad de realizar un downcasting manual, lo cual limpia el código y elimina el riesgo de excepciones en tiempo de ejecución.

Además, el uso de parámetros de tipo permite forzar la homogeneidad de los argumentos. Si se define el método con Object, se podrían pasar un Soldado y un String simultáneamente, ya que ambos son objetos. Sin embargo, al usar un único parámetro <T> para ambos parámetros, el compilador obligará a que ambos pertenezcan a la misma jerarquía de tipos. Si se intenta mezclar tipos incompatibles, el error se detectará inmediatamente antes de la ejecución, asegurando la coherencia lógica del programa.

Ejemplo: Método Genérico vs. Uso de Object
Java
import java.util.Random;

public class Utilidades {
    private static final Random azar = new Random();

    // VERSIÓN GENÉRICA: Segura y sin casting
    public static <T> T seleccionaUno(T a, T b) {
        return azar.nextBoolean() ? a : b;
    }

    // VERSIÓN CON OBJECT: Insegura y requiere casting
    public static Object seleccionaUnoLegacy(Object a, Object b) {
        return azar.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String s1 = "Opcion A";
        String s2 = "Opcion B";

        // (i) Evitar downcasting:
        // Con genéricos, el retorno ya es String
        String elegido1 = seleccionaUno(s1, s2); 
        
        // Con Object, el compilador obliga al casting manual
        String elegido2 = (String) seleccionaUnoLegacy(s1, s2);

        // (ii) Forzar el mismo tipo:
        // seleccionaUno(s1, 100); // ERROR DE COMPILACIÓN: Tipos incompatibles
        seleccionaUnoLegacy(s1, 100); // COMPILA, pero es lógicamente inconsistente
    }
}
Como se observa en el código, el método genérico actúa como una "plantilla inteligente" que se adapta al tipo String en tiempo de compilación. Mientras que la versión basada en Object es ciega a la naturaleza de los datos, la versión genérica protege la integridad del sistema al asegurar que la salida sea consistente con la entrada y que el usuario no tenga que realizar conversiones peligrosas para recuperar la información original.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

En Java, es posible limitar el rango de tipos que un parámetro genérico puede aceptar mediante lo que se conoce como Bounded Type Parameters (parámetros de tipo acotados). Para restringir un tipo <T> de modo que solo acepte números, se utiliza la palabra clave extends seguida de la clase base deseada, en este caso Number. Esto garantiza que cualquier objeto tratado bajo ese parámetro de tipo posea los métodos definidos en la clase Number (como doubleValue() o intValue()), permitiendo realizar operaciones matemáticas sobre tipos que originalmente eran desconocidos.

Al emplear la clase Number de forma directa sin genericidad, se crea una estructura flexible pero menos precisa. En este escenario, las coordenadas pueden ser cualquier combinación de subclases de Number (un Integer para X y un Double para Y), y el compilador siempre tratará el retorno como el tipo base Number. Esto obliga al programador a realizar conversiones manuales si necesita recuperar el tipo específico original, perdiendo la ventaja del tipado fuerte en las capas superiores de la aplicación.

Por el contrario, al utilizar genericidad acotada (<T extends Number>), se refuerza el chequeo de tipos de manera que el Punto queda vinculado a un tipo numérico concreto. Si se instancia un Punto<Integer>, el compilador garantizará que tanto x como y sean enteros, y los métodos getX() y getY() devolverán Integer directamente. Esta solución es superior porque mantiene la homogeneidad de las coordenadas y elimina la ambigüedad sobre la precisión numérica que se está utilizando en los cálculos.

Solución 1: Uso directo de Number (Sin Generics)
Java
class PuntoNumber {
    private Number x, y;

    public PuntoNumber(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(PuntoNumber otro) {
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
Solución 2: Uso de Generics Acotados (extends Number)
Java
class PuntoGen<T extends Number> {
    private T x, y;

    public PuntoGen(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(PuntoGen<T> otro) {
        // Se puede llamar a doubleValue() porque T es al menos un Number
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
Respecto al Type Erasure, cuando se establece un límite como <T extends Number>, el compilador no sustituye T por Object, sino por el tipo de la cota superior, que en este caso es Number. Tras la compilación, en el bytecode final, todos los atributos y parámetros que eran de tipo T pasan a ser de tipo Number. La diferencia radica en que el compilador ha verificado previamente que los tipos eran consistentes y ha insertado los casts necesarios para que, al llamar a getX(), el valor se convierta automáticamente al tipo específico (como Integer) que el usuario solicitó originalmente.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

Al analizar ambas soluciones, se observa que la principal diferencia radica en el nivel de restricción y precisión que el compilador puede ejercer sobre la estructura interna de la clase. Mientras que la versión sin genéricos prioriza la flexibilidad absoluta a costa de la homogeneidad, la versión con genéricos impone una coherencia de tipos que protege la integridad de los datos almacenados.

En cuanto a la posibilidad de mezclar tipos (una coordenada Integer y otra Double), la solución sin genéricos sí lo permite. Al declarar los atributos simplemente como Number, el objeto puede alojar cualquier combinación de subclases de números simultáneamente. Por el contrario, la solución con genéricos lo impide si se utiliza un único parámetro de tipo <T>. Debido a que tanto x como y están definidos como T, el compilador obliga a que ambos compartan exactamente el mismo tipo concreto al instanciar la clase (por ejemplo, ambos deben ser Double).

Respecto al tipo de retorno de los métodos de acceso, existen implicaciones importantes para el programador que consume la clase:

En la solución sin genéricos: El método getX() devuelve siempre una referencia de tipo Number. Si el usuario necesita realizar una operación específica de enteros (como un desplazamiento de bits) o de reales, se verá obligado a realizar una comprobación con instanceof y un downcasting manual al tipo deseado, con el riesgo de error que esto conlleva.

En la solución con genéricos: El método getX() devuelve el tipo exacto T. Si se ha instanciado un PuntoGen<Integer>, el método devolverá un Integer de forma nativa. Gracias a la información de tipo preservada por el compilador antes del type erasure, no es necesario realizar ninguna conversión manual, permitiendo un uso directo y seguro del valor recuperado.

En conclusión, el refuerzo de tipos con genéricos no solo sirve para "limpiar" el código de conversiones, sino para definir reglas de negocio más estrictas. En el caso de un punto matemático, lo habitual es que todas sus dimensiones compartan la misma precisión numérica; la genericidad acotada permite que el lenguaje valide esta regla automáticamente, evitando que el sistema termine operando accidentalmente con tipos heterogéneos que podrían derivar en errores de redondeo o pérdida de precisión inesperados.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

Para lograr que la interfaz obligue a las subclases a aceptar únicamente puntos de su propio tipo, se debe recurrir a la genericidad recursiva (o F-bounded polymorphism). En este diseño, la interfaz se declara con un parámetro de tipo que representa a la propia clase que la implementa. De este modo, el contrato del método distanciaA deja de ser una referencia universal a Punto y pasa a ser una referencia al tipo específico definido por la implementación concreta.

Al aplicar este cambio, se elimina la necesidad de utilizar instanceof y el posterior downcasting. El compilador de Java, basándose en la información del parámetro de tipo, garantiza que el objeto recibido en el método es del tipo correcto. Si se intenta pasar un Punto3D a un método de Punto2D, el código no compilará, trasladando la responsabilidad de la validación de la ejecución al tiempo de compilación y ganando en seguridad y claridad.

Implementación con Generics Recursivos
Java
// La interfaz recibe un parámetro T que debe ser un subtipo de Punto
public interface Punto<T extends Punto<T>> {
    public double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x; this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        // No hace falta instanceof ni casting: p ya es Punto2D
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + 
                         Math.pow(y - p.y, 2) + 
                         Math.pow(z - p.z, 2));
    }
}
Este enfoque redefine la relación jerárquica: Punto2D no solo implementa la interfaz, sino que se identifica a sí mismo como el único interlocutor válido para sus operaciones de distancia. Esto resuelve el problema de la mezcla de tipos de forma elegante y nativa. La firma del método distanciaA(Punto2D p) es ahora estrictamente tipada, lo que permite acceder directamente a los atributos privados o protegidos de p (como p.x o p.y) sin pasos intermedios, simplificando drásticamente la lógica interna del método.

Desde el punto de vista del diseño de software, esta técnica es una de las más potentes de Java para definir operaciones binarias (operaciones que requieren dos objetos del mismo tipo). Se asegura que el comportamiento polimórfico sea coherente: un punto siempre "sabe" compararse o medirse con otros puntos de su misma naturaleza dimensional, impidiendo errores lógicos donde se intenten calcular distancias imposibles entre espacios de diferentes dimensiones.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

La relación de subtipado en Arrays y GenéricosA diferencia de lo que dicta la intuición basada en la herencia simple, List<String> no es subtipo de List<Object>. En Java, los tipos genéricos son invariantes. Esto significa que, aunque String herede de Object, no existe ninguna relación jerárquica entre sus listas. Si el compilador permitiera asignar una List<String> a una referencia de List<Object>, se podría insertar un Integer en la lista de cadenas, rompiendo la integridad de los tipos que la genericidad intenta proteger.Por el contrario, los arrays sí mantienen la relación de herencia: String[] es subtipo de Object[]. Se dice que los arrays son covariantes. Esta decisión de diseño se tomó en las primeras versiones de Java (antes de que existieran los genéricos) para permitir que métodos generales pudieran procesar cualquier array de objetos. Sin embargo, esta flexibilidad introduce una debilidad en la seguridad del código que los genéricos logran evitar.El problema de los Arrays en tiempo de ejecuciónLa covarianza de los arrays puede provocar la excepción ArrayStoreException. Este error ocurre porque el array "recuerda" internamente el tipo de elementos que puede contener. Si se asigna un array de strings a una referencia de objetos (Object[] array = new String;) y se intenta insertar un número mediante esa referencia (array = 10;), el código compilará sin errores, pero el programa fallará al ejecutarse al detectar que se intenta meter un entero en un almacén de cadenas.Este problema no existe con los genéricos porque el error se detecta en tiempo de compilación. Al ser invariantes, el compilador prohíbe directamente la asignación inicial, obligando al programador a corregir la lógica antes de que el programa llegue a ejecutarse. Es una transición desde la detección de errores en ejecución (típica de punteros mal gestionados en C) hacia una validación estática mucho más robusta.Invarianza, Covarianza y ContravarianzaA partir de estos comportamientos, se pueden definir formalmente las tres variantes de relación entre un tipo compuesto $F$ y sus parámetros $A$ y $B$:Invariante: Se dice que $F<T>$ es invariante si no existe relación entre $F<A>$ y $F<B>$, independientemente de la relación entre $A$ y $B$. Es el comportamiento por defecto de los genéricos en Java; una Caja<String> no es una Caja<Object>.Covariante: Se dice que $F<T>$ es covariante si la relación de subtipo se preserva. Si $A$ es subtipo de $B$, entonces $F<A>$ es subtipo de $F<B>$. Los arrays en Java son el ejemplo clásico de esta propiedad.Contravariante: Es la relación inversa. Se dice que $F<T>$ es contravariante si, siendo $A$ subtipo de $B$, entonces $F<B>$ es subtipo de $F<A>$. En Java, esto se logra mediante el uso de comodines con la palabra clave super (como List<? super String>), permitiendo que una referencia acepte tipos de niveles superiores en la jerarquía.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

El uso de comodines o wildcards (?) es el mecanismo que emplea Java para flexibilizar la invarianza de los genéricos. Un wildcard representa un tipo desconocido; permite que una referencia genérica sea más permisiva, aceptando un rango de tipos en lugar de uno solo, permitiendo así recuperar la covarianza o contravarianza de forma segura y bajo el control del programador.

Covarianza con <? extends T>
La expresión List<? extends T> representa una relación de covarianza. Indica que la lista puede ser de tipo T o de cualquier subtipo de T. Este comodín se utiliza principalmente para lectura (productores). Al asegurar que cualquier elemento de la lista es, como mínimo, un T, se pueden extraer objetos de forma segura tratándolos como T. Sin embargo, prohíbe la escritura (añadir elementos), ya que el compilador no puede garantizar si la lista real es, por ejemplo, de un subtipo específico incompatible con lo que se intenta insertar.

Ejemplo (i): Sumar una lista de números
En este caso, se acepta cualquier lista cuyo contenido sea Number o sus hijos (Integer, Double, etc.). Se puede leer con seguridad porque todos "son" un Number.

Java
public static double sumarLista(List<? extends Number> lista) {
    double suma = 0.0;
    for (Number n : lista) {
        suma += n.doubleValue(); // Lectura segura como Number
    }
    return suma;
}
Contravarianza con <? super T>
La expresión List<? super T> representa una relación de contravarianza. Indica que la lista puede ser de tipo T o de cualquier superclase de T (subiendo en la jerarquía hasta Object). Este comodín se utiliza principalmente para escritura (consumidores). El compilador permite añadir elementos de tipo T (o sus hijos) porque tiene la certeza de que, sea cual sea el tipo de la lista original, esta es capaz de albergar un objeto de tipo T. La lectura, en cambio, es limitada, ya que solo se garantiza que lo que se extraiga sea un Object.

Ejemplo (ii): Añadir números enteros a una lista
Aquí el método puede recibir una List<Integer>, una List<Number> o una List<Object>, ya que todas son capaces de almacenar valores de tipo Integer.

Java
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(10); // Escritura segura de Integer
    lista.add(20);
    // Se podrían añadir subtipos de Integer si existieran
}
La regla PECS
Para recordar cuándo usar cada uno, en la comunidad de Java se utiliza el acrónimo PECS: Producer Extends, Consumer Super. Si el método obtiene datos de la estructura (la estructura "produce"), se usa extends. Si el método introduce datos en la estructura (la estructura "consume"), se usa super. Esta distinción evita los problemas de seguridad en tiempo de ejecución que sufren los arrays, delegando la validación al sistema de tipos del compilador.
