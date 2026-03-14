 <!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos



Las cuatro características básicas de la programación orientada a objetos son encapsulación, abstracción, herencia y polimorfismo. Estas propiedades permiten organizar el software en unidades llamadas objetos, que combinan datos y comportamiento. Este enfoque facilita la reutilización de código, la modularidad y el mantenimiento de programas grandes, a diferencia de la programación procedural tradicional donde datos y funciones suelen estar separados.

 La encapsulación consiste en agrupar datos y métodos dentro de una misma estructura llamada clase, controlando además el acceso a esos datos. De esta manera, los atributos internos de un objeto no se modifican directamente desde el exterior, sino a través de métodos definidos para ello. Esto permite proteger el estado interno del objeto y evitar modificaciones incorrectas.

 La herencia permite crear nuevas clases a partir de otras ya existentes. Una clase derivada hereda los atributos y métodos de la clase base, pudiendo reutilizarlos o modificarlos. Este mecanismo facilita la reutilización de código y permite modelar relaciones jerárquicas entre tipos de objetos.

  El polimorfismo permite que un mismo método o interfaz pueda comportarse de diferentes maneras según el objeto que lo utilice. Esto significa que diferentes clases pueden implementar un mismo método con comportamientos distintos. Finalmente, la abstracción consiste en representar únicamente las características esenciales de un objeto, ocultando los detalles internos innecesarios para quien utiliza la clase. De esta forma se simplifica el uso de los objetos y se reduce la complejidad del sistema


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Algunos lenguajes de programación populares que permiten utilizar el paradigma de programación orientada a objetos son Java, C++, Python y C#. Estos lenguajes proporcionan mecanismos para definir clases, crear objetos y aplicar conceptos fundamentales como encapsulación, herencia y polimorfismo. Gracias a estas características, resultan adecuados para desarrollar aplicaciones grandes y estructuradas.

Java es uno de los lenguajes más representativos de la programación orientada a objetos. En este lenguaje prácticamente todo se organiza en clases y objetos, lo que obliga a utilizar este paradigma desde el principio. Es muy utilizado en el desarrollo de aplicaciones empresariales, aplicaciones Android y sistemas distribuidos.

C++ también permite la programación orientada a objetos, aunque originalmente deriva del lenguaje C. A diferencia de Java, C++ permite combinar programación procedural y orientada a objetos. Esto facilita la transición para programadores que ya conocen C, ya que se pueden ir incorporando progresivamente conceptos como clases, herencia y métodos.

Python y C# también son lenguajes ampliamente utilizados que soportan programación orientada a objetos. Python se caracteriza por su sintaxis sencilla y flexible, lo que facilita aprender los conceptos de clases y objetos. Por su parte, C# es un lenguaje desarrollado para la plataforma .NET que incorpora un modelo de orientación a objetos muy completo y estructurado.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

 La programación estructurada es un paradigma de programación basado en la organización del código mediante estructuras de control claras, como secuencias, condicionales y bucles. Su objetivo principal es mejorar la claridad y mantenibilidad de los programas evitando el uso excesivo de saltos incontrolados (por ejemplo, goto). En este enfoque, los programas se dividen en funciones o procedimientos que realizan tareas específicas y que se ejecutan de forma ordenada.

En la programación estructurada, los datos y las funciones suelen estar separados. Las funciones operan sobre datos que normalmente se pasan como parámetros o se almacenan en estructuras. Este modelo es típico de lenguajes como C, donde se definen estructuras (struct) para agrupar datos y funciones independientes para manipularlos. Esto contrasta con la programación orientada a objetos, donde datos y métodos se agrupan dentro de clases.

#include <stdio.h>

struct Punto {
    int x;
    int y;
};

void imprimirPunto(struct Punto p) {
    printf("(%d, %d)\n", p.x, p.y);
}

int main() {
    struct Punto p = {2, 3};
    imprimirPunto(p);
    return 0;
}

La programación orientada a objetos amplía esta idea agrupando datos y operaciones dentro de clases y objetos. En lugar de tener funciones externas que manipulan estructuras de datos, los propios objetos contienen métodos que operan sobre su estado interno. Este cambio introduce conceptos como encapsulación, herencia y polimorfismo, que permiten organizar programas complejos de manera más modular y reutilizable.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

En la programación orientada a objetos, un objeto se define fundamentalmente por tres elementos: identidad, estado y comportamiento. Estos elementos permiten representar entidades del mundo real dentro de un programa y describir cómo interactúan entre sí. Un objeto es una instancia concreta de una clase y combina datos con las operaciones que pueden realizarse sobre esos datos.

La identidad es la propiedad que permite distinguir un objeto de otro, incluso cuando ambos contienen los mismos valores en sus datos. Cada objeto ocupa una posición propia en memoria y puede ser referenciado de forma independiente. Esto significa que dos objetos pueden tener el mismo estado pero seguir siendo objetos diferentes dentro del programa.

El estado está formado por los valores que contienen los atributos del objeto en un momento determinado. Estos atributos representan la información que describe al objeto. Por ejemplo, un objeto que represente un coche podría tener atributos como color, velocidad o número de puertas. El estado de un objeto puede cambiar a lo largo de la ejecución del programa.

El comportamiento está definido por los métodos del objeto, es decir, las operaciones que pueden realizarse sobre él. Los métodos permiten modificar el estado del objeto o consultar información sobre él. De esta forma, el comportamiento determina cómo interactúa el objeto con otros objetos dentro del sistema.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es una estructura que define el modelo o plantilla a partir de la cual se crean los objetos en la programación orientada a objetos. En una clase se especifican los atributos (datos) y los métodos (funciones) que tendrán los objetos creados a partir de ella. Puede entenderse como una descripción general de un tipo de entidad, mientras que los objetos representan ejemplos concretos de esa descripción.

Una clase no es lo mismo que un objeto. La clase describe qué características y comportamientos tendrán los objetos, pero no representa una entidad concreta en sí misma. El objeto, en cambio, es una entidad creada a partir de la clase que contiene valores específicos en sus atributos. Por ejemplo, una clase Coche podría definir atributos como color o velocidad, mientras que un objeto concreto podría representar un coche rojo con una velocidad determinada.

Una instancia es precisamente un objeto creado a partir de una clase. El proceso de crear un objeto a partir de una clase se denomina instanciación. Cada instancia tiene su propio estado, aunque todas comparten la misma definición de atributos y métodos establecida en la clase.

No todos los lenguajes que soportan programación orientada a objetos utilizan necesariamente el concepto de clase. Algunos lenguajes emplean modelos alternativos, como la programación basada en prototipos, donde los objetos se crean a partir de otros objetos en lugar de a partir de clases. Sin embargo, muchos lenguajes orientados a objetos populares, como Java, C++ o C#, sí utilizan clases como elemento fundamental para definir objetos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En muchos lenguajes de programación orientados a objetos, los objetos suelen almacenarse en la memoria dinámica, conocida comúnmente como heap. Cuando se crea un objeto durante la ejecución del programa, el sistema reserva espacio en esa zona de memoria para guardar sus atributos y otra información interna. Las variables del programa normalmente almacenan referencias o direcciones que apuntan a ese objeto en memoria. Este funcionamiento es común en lenguajes como Java, donde los objetos se crean mediante new y se accede a ellos a través de referencias.

Sin embargo, no todos los lenguajes gestionan la memoria exactamente de la misma forma. En algunos lenguajes como C++, un objeto puede almacenarse tanto en la pila (stack) como en el heap, dependiendo de cómo se declare. Por ejemplo, si un objeto se declara como una variable local dentro de una función, suele almacenarse en la pila; en cambio, si se crea dinámicamente usando new, se almacena en el heap. Esto implica que el programador puede tener mayor control sobre la gestión de memoria.

La recolección de basura es un mecanismo automático de gestión de memoria utilizado por algunos lenguajes de programación. Su objetivo es liberar automáticamente la memoria ocupada por objetos que ya no se utilizan en el programa. Cuando el sistema detecta que un objeto ya no tiene referencias activas, el recolector de basura puede eliminarlo y recuperar el espacio de memoria que ocupaba.

Este mecanismo es característico de lenguajes como Java o Python. Gracias a la recolección de basura, el programador no necesita liberar manualmente la memoria de los objetos, lo que reduce errores comunes como fugas de memoria o accesos a memoria liberada. No obstante, también implica que el control sobre cuándo se libera exactamente la memoria es menor que en lenguajes donde la gestión es manual.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

 La sobrecarga de métodos es una técnica utilizada en la programación orientada a objetos que permite definir varios métodos con el mismo nombre dentro de una misma clase, siempre que se diferencien en sus parámetros. Esta diferencia puede estar en el número de parámetros, en el tipo de los parámetros o en el orden en que aparecen. Gracias a esto, un mismo nombre de método puede utilizarse para realizar operaciones similares pero con distintos tipos de datos.

Este mecanismo facilita que el código sea más claro y fácil de usar, ya que el programador puede emplear un único nombre de método para representar una misma idea general. El compilador se encarga de decidir cuál de las versiones del método debe ejecutarse según los argumentos que se pasen en la llamada. De esta forma, se mantiene una interfaz simple mientras se ofrecen varias implementaciones.

Un ejemplo sencillo en Java sería el siguiente:

class Calculadora {

    int sumar(int a, int b) {
        return a + b;
    }

    double sumar(double a, double b) {
        return a + b;
    }

    int sumar(int a, int b, int c) {
        return a + b + c;
    }
}

En este ejemplo existen tres métodos llamados sumar, pero cada uno recibe parámetros diferentes. Cuando el programa llama al método sumar, el compilador determina automáticamente cuál debe utilizar según los tipos y la cantidad de argumentos proporcionados. Este mecanismo permite reutilizar nombres de métodos sin provocar ambigüedad en el programa.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

En Java, una clase permite definir un tipo de objeto especificando sus atributos y los métodos que operan sobre ellos. En este caso se puede definir una clase llamada Punto que represente una posición en un plano mediante dos atributos: x e y. Estos atributos almacenan las coordenadas del punto y, al tener visibilidad por defecto, pueden ser accedidos desde otras clases del mismo paquete.

El método calculaDistanciaAOrigen se encarga de calcular la distancia entre el punto representado por el objeto y el origen del plano (0,0). Para ello se utiliza la fórmula de la distancia euclidiana, que consiste en calcular la raíz cuadrada de la suma de los cuadrados de las coordenadas. Este método no necesita parámetros porque utiliza directamente los valores de x e y almacenados en el objeto.

class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

A continuación se muestra un ejemplo sencillo de uso. En este caso se crea una instancia de la clase Punto, se asignan valores a sus atributos y se llama al método para obtener la distancia al origen. Esto ilustra cómo se crean objetos y cómo se invocan sus métodos en Java.

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia);
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada y el modificador static
En Java, el punto de entrada es el método public static void main(String[] args). Al igual que la función main en C, este es el lugar donde comienza la ejecución. La diferencia fundamental es que en Java debe estar dentro de una clase, ya que no existe código "suelto". Se declara como static para que la Máquina Virtual de Java (JVM) pueda invocarlo sin necesidad de crear previamente un objeto de esa clase, resolviendo así el dilema de cómo empezar a ejecutar el programa.

El uso de static no se limita al método inicial; se aplica a cualquier atributo o método que deba pertenecer a la clase en sí y no a sus instancias. En C, esto equivale conceptualmente a una variable global o una función de utilidad que no depende de una estructura específica. Los elementos estáticos residen en una zona de memoria compartida por todos los objetos, lo que resulta ideal para contadores globales o funciones matemáticas que no requieren mantener un estado interno.

Cuando static se combina con final, se crean constantes de clase. Mientras que en C se utilizaría #define o const, en Java se emplea static final para asegurar que un valor sea compartido por todas las instancias y que, además, sea inmutable. Esta combinación es la forma estándar de definir parámetros fijos, como configuraciones del sistema o valores matemáticos, garantizando que no puedan ser alterados durante la ejecución del programa.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para poner en marcha un programa en Java desde la terminal, primero se utiliza el comando javac NombreClase.java, el cual invoca al compilador. Si no existen errores de sintaxis, este proceso genera un archivo con extensión .class. Posteriormente, para iniciar la ejecución, se emplea el comando java NombreClase (sin la extensión), lo que ordena a la plataforma que cargue el programa en la memoria y ejecute el método main.

A diferencia de C, donde el compilador genera un archivo binario ejecutable vinculado directamente al hardware (.exe o un binario de Linux), Java utiliza un modelo híbrido. Se dice que Java es compilado e interpretado: el código fuente no se traduce a lenguaje máquina real, sino a un lenguaje intermedio llamado byte-code. Este formato es universal y no depende del procesador o del sistema operativo, lo que permite que el mismo archivo .class funcione en cualquier entorno que tenga instalada la infraestructura de Java.

El byte-code se almacena en los ficheros .class y es el "idioma" que entiende la Máquina Virtual de Java (JVM). La JVM es un software que actúa como una capa de abstracción o un ordenador ficticio sobre el sistema operativo real. Su función es leer el byte-code y traducirlo a instrucciones que el procesador físico pueda entender en tiempo real. Esto elimina la necesidad de recompilar el código para diferentes plataformas, un concepto que en Java se resume con el lema: "escribe una vez, ejecútalo en cualquier lugar".


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

En Java, el operador new es el encargado de solicitar dinámicamente memoria en el "heap" (montículo) para albergar un objeto, de forma similar a como funciona malloc en C. Sin embargo, a diferencia de C, new no solo reserva el espacio físico, sino que también invoca inmediatamente al constructor de la clase para inicializar dicho espacio. El resultado de esta operación es una referencia (un puntero gestionado) que permite manipular el objeto recién creado.

Un constructor es un método especial que define cómo debe configurarse un objeto al nacer. Se identifica porque tiene el mismo nombre que la clase y carece de tipo de retorno. Su función es asegurar que los atributos del objeto no contengan valores residuales o nulos, estableciendo un estado inicial coherente. Si un programador de C ve el objeto como una estructura, el constructor sería la función de inicialización que garantiza que todos los campos de esa estructura tengan valores válidos desde el primer segundo.

A continuación, se presenta el ejemplo solicitado para una clase Empleado. En este caso, el constructor recibe los datos necesarios y los asigna a las variables internas de la instancia:

Java
public class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor de la clase Empleado
    public Empleado(String dniInicial, String nombreInicial, String apellidosInicial) {
        dni = dniInicial;
        nombre = nombreInicial;
        apellidos = apellidosInicial;
    }
}


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia this es una variable especial que un objeto utiliza para referirse a sí mismo desde dentro de sus propios métodos. En términos de C, se puede conceptualizar como un puntero constante que siempre apunta a la dirección de memoria de la estructura (objeto) que está ejecutando el código en ese momento. Su función principal es resolver ambigüedades cuando el nombre de un parámetro en un método es idéntico al nombre de un atributo de la clase.

Este concepto no recibe el mismo nombre en todos los lenguajes de programación, aunque la lógica sea idéntica. Mientras que en Java, C++ y JavaScript se utiliza la palabra reservada this, en lenguajes como Python o Swift se emplea el término self. Independientemente del nombre, su propósito sigue siendo el mismo: diferenciar las variables locales o parámetros de los datos que pertenecen permanentemente a la instancia del objeto.

En la clase Punto, el uso de this es fundamental cuando el constructor recibe parámetros llamados exactamente igual que los atributos (x e y). Sin el uso de this, el compilador daría prioridad a la variable local (el parámetro) y el atributo de la clase nunca recibiría el valor. Al escribir this.x, se le indica explícitamente a Java que se desea acceder a la variable de la instancia y no al argumento del método.

Java
public class Punto {
    int x; // Atributo de la clase
    int y; // Atributo de la clase

    public Punto(int x, int y) {
        this.x = x; // 'this.x' es el atributo, 'x' es el parámetro
        this.y = y; // 'this.y' es el atributo, 'y' es el parámetro
    }
}


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

Para calcular la distancia entre dos puntos en un plano cartesiano, se aplica la fórmula de la distancia euclídea, la cual se basa en el Teorema de Pitágoras. En Java, el método distanciaA utiliza la referencia this para acceder a las coordenadas del objeto actual y las compara con las coordenadas del objeto Punto que se recibe como argumento. Este enfoque permite que un objeto interactúe con otro de su misma clase para obtener un resultado numérico.Desde la perspectiva de un programador de C, este proceso es equivalente a pasar una estructura por referencia a una función. La diferencia radica en que, al ser un método de la clase, se tiene acceso directo a los atributos del parámetro (como otroPunto.x) y a los del propio objeto (this.x). Para realizar el cálculo matemático de la raíz cuadrada y la potencia, se utiliza la clase de utilidad Math, que provee funciones estáticas similares a las de la librería math.h en C.A continuación, se muestra la implementación del método dentro de la clase Punto. Se observa cómo se calcula la diferencia entre las coordenadas $x$ e $y$, se elevan al cuadrado y se obtiene la raíz de su suma:Javapublic double distanciaA(Punto otroPunto) {
    // Cálculo de la diferencia entre componentes
    int dx = this.x - otroPunto.x;
    int dy = this.y - otroPunto.y;

    // Aplicación de la fórmula: raíz cuadrada de (dx^2 + dy^2)
    return Math.sqrt(Math.pow(dx, 2) + Math.pow(dy, 2));
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

 En Java, el paso de parámetros siempre se realiza por valor, aunque su comportamiento varía según el tipo de dato. Cuando se pasa un objeto como un Punto, lo que se copia es la referencia (la dirección de memoria) al objeto original. Esto significa que tanto la variable fuera del método como el parámetro dentro de él apuntan al mismo objeto en el "heap". Por tanto, si se modifican los atributos del punto dentro del método, los cambios sí afectan al objeto original fuera de él, de forma idéntica a como funcionaría pasar un puntero a una estructura en C.

Sin embargo, si dentro del método se intenta reasignar el parámetro completo (por ejemplo, haciendo punto = new Punto()), esa acción no afectará a la variable original. En ese caso, solo se estaría cambiando hacia dónde apunta la copia local de la referencia, pero la variable externa seguiría apuntando al objeto inicial. Es una distinción sutil pero vital: se puede modificar el contenido del objeto, pero no se puede "desconectar" la variable original de su objeto desde dentro del método.

Por el contrario, cuando se recibe un tipo primitivo como un int, el comportamiento es distinto. En este caso, Java crea una copia física del valor numérico en la pila (stack) de la función. Cualquier modificación que se realice sobre ese entero dentro del método es totalmente local y no tiene ningún efecto sobre la variable original fuera de la función. Este comportamiento es exactamente igual al paso por valor de un int estándar en C o C++.

Java
public void modificar(Punto p, int n) {
    p.x = 500; // Afecta al objeto original (como un puntero en C)
    n = 500;   // NO afecta a la variable original (es una copia local)
}a


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java


En Java, el método toString() es una función especial que devuelve una representación en forma de cadena de texto (String) de un objeto. Por defecto, todas las clases en Java heredan este método de una clase base llamada Object. Si no se sobrescribe de forma personalizada, el comportamiento estándar muestra el nombre de la clase seguido de un código hexadecimal que representa la dirección de memoria (el hashcode), lo cual no suele ser útil para un programador que viene de C y espera ver los datos internos.

Este concepto de "autorrepresentación textual" es común en la mayoría de los lenguajes modernos. En Python existe el método __str__, en C# se utiliza exactamente ToString() y en JavaScript se emplea toString(). En C estándar, no existe un equivalente automático; el programador debe escribir manualmente una función (como imprimir_punto) y pasarle la estructura para mostrar sus campos por consola mediante printf.

La gran ventaja en Java es que este método se invoca automáticamente cuando el objeto se utiliza en un contexto de cadena, como al imprimir con System.out.println(objeto). Al sobrescribirlo, se puede definir un formato legible que facilite la depuración del código. En la clase Punto, se suele implementar para que devuelva las coordenadas en el formato matemático tradicional entre paréntesis.

Java
@Override
public String toString() {
    // Devuelve una cadena con el formato: (x, y)
    return "(" + this.x + ", " + this.y + ")";
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Desde una perspectiva técnica, es correcto considerar que una clase es una evolución directa de la struct de C. En C, una estructura es un contenedor pasivo que solo agrupa datos (variables) bajo un mismo nombre, pero carece de inteligencia propia. La clase toma esa base de agrupación de datos y añade la capacidad de integrar funciones (métodos) que operan directamente sobre ellos, unificando el "qué es" con el "qué hace".

Al struct de C le faltan principalmente tres componentes para convertirse en una clase. El primero es el encapsulamiento, que permite proteger los datos internos mediante modificadores de acceso (private, public); en C, cualquier parte del programa puede alterar los campos de una estructura si tiene acceso a ella. El segundo es la herencia, la capacidad de que una estructura derive de otra compartiendo su base. El tercero es la asociación de métodos, ya que en C las funciones son entes externos que reciben la estructura como argumento, mientras que en Java los métodos pertenecen a la propia definición de la clase.

Para que las variables de una struct se comporten como instancias (objetos), necesitarían un ciclo de vida gestionado por constructores. En C, al declarar una estructura, el programador debe recordar inicializar cada campo manualmente para evitar datos basura. En el paradigma de objetos, la instancia nace mediante un constructor que garantiza un estado inicial válido, y su interacción se realiza a través de mensajes (llamadas a métodos) en lugar de manipulación directa de sus registros de memoria.

En resumen, la struct es un agrupamiento de datos estático, mientras que la clase es un ente dinámico que combina datos, comportamiento y mecanismos de seguridad. Esta transición permite pasar de un código donde los datos están expuestos y las funciones dispersas, a un sistema de objetos autónomos que protegen su propia información y saben cómo manipularse a sí mismos.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?



Para emular una clase de Java en C, se debe definir una struct que contenga únicamente los datos (los atributos x e y). Dado que en C las estructuras no pueden albergar funciones en su interior, el comportamiento se implementa mediante funciones externas que reciben como primer argumento un puntero a la estructura. De esta manera, se simula la agrupación de estado y comportamiento, aunque técnicamente sigan siendo entidades separadas en el código fuente.

En esta emulación, la "magia" de la referencia this desaparece para mostrar su verdadera naturaleza: un simple puntero. En Java, this es un parámetro implícito que el compilador añade automáticamente a todos los métodos. En C, ese parámetro debe hacerse explícito, pasando manualmente la dirección de memoria de la estructura (por ejemplo, Punto *self o Punto *this). Así, lo que en Java se escribe como this.x, en C se traduce como self->x.

El cálculo de la distancia al origen en C requeriría, por tanto, una función que tome ese puntero para acceder a las coordenadas de la instancia específica. Al llamar a la función, el programador de C debe pasar la dirección de la estructura usando el operador &, mientras que en Java esa asociación ocurre de forma transparente al usar la notación de punto. Esta diferencia ilustra cómo la POO es, en esencia, una "capa de azúcar sintáctico" sobre la gestión de punteros y estructuras.

C
#include <math.h>

typedef struct {
    int x;
    int y;
} Punto;

// Emulación del método: el 'this' de Java es el 'self' de C
double distanciaAlOrigen(Punto *self) {
    return sqrt(pow(self->x, 2) + pow(self->y, 2));
}

// Uso en el main:
// Punto p = {3, 4};
// double d = distanciaAlOrigen(&p);
