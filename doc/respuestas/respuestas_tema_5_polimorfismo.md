<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo es la capacidad que tiene una referencia de una superclase para adoptar múltiples formas, apuntando a objetos de distintas subclases y ejecutando comportamientos específicos. En programación orientada a objetos, sirve para escribir código genérico y extensible; permite que un programa trate a diversos objetos de manera uniforme a través de su interfaz común, eliminando la necesidad de usar estructuras condicionales complejas para verificar el tipo de cada objeto.

La sobrescritura de métodos es el mecanismo que permite a una subclase proporcionar una implementación propia de un método que ya existe en su superclase. Para que sea efectiva, el método en la clase hija debe tener exactamente la misma firma (nombre, parámetros y tipo de retorno). Es la herramienta que hace posible el polimorfismo, ya que permite que cada objeto responda al mismo mensaje (llamada al método) de la manera que mejor se adapte a su naturaleza específica.

En términos de ejecución, cuando se invoca un método sobrescrito, Java utiliza la ligadura dinámica. Esto implica que la decisión sobre qué versión del método ejecutar no se toma durante la compilación, sino en tiempo de ejecución, basándose en el tipo real del objeto que recibe la llamada y no en el tipo de la variable que lo referencia.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica, también conocida como enlace tardío (late binding), es el mecanismo por el cual el sistema determina qué implementación específica de un método debe ejecutarse justo en el momento de la llamada, durante la ejecución del programa. En contraposición a la ligadura estática, donde el compilador decide la dirección de la función en tiempo de compilación, la ligadura dinámica posterga esta decisión hasta que el programa está en marcha y se conoce la identidad real del objeto.

La relación con el polimorfismo es de absoluta dependencia: la ligadura dinámica es el motor tecnológico que lo hace posible. Sin ella, una referencia de una superclase siempre ejecutaría los métodos de la clase base, ignorando las especializaciones de las subclases. Gracias al enlace tardío, Java puede examinar el objeto en el heap y ejecutar la versión del método que corresponde a su clase real, permitiendo que una misma línea de código produzca resultados distintos según el objeto que reciba el mensaje.

Respecto a su indicación explícita, la sintaxis varía significativamente según el lenguaje de programación:

En Java: La ligadura dinámica es el comportamiento por defecto para todos los métodos de instancia que no sean final, private o static. El programador no tiene que indicar nada especial; simplemente al sobrescribir un método, Java asume que debe usar enlace tardío. Es un lenguaje diseñado para ser polimórfico "de serie".

En C++: La ligadura dinámica debe indicarse explícitamente mediante la palabra reservada virtual en la declaración del método en la clase base. Si un método no se marca como virtual, C++ aplica ligadura estática por defecto para maximizar la eficiencia (evitando la consulta a la vtable), lo que impide el comportamiento polimórfico a menos que el desarrollador lo solicite expresamente.

En el caso de Python, la situación es distinta debido a su naturaleza de tipado dinámico. En Python, todo es ligadura dinámica por definición. Al ser un lenguaje interpretado donde el tipo de las variables no se declara, el intérprete siempre busca el método en el objeto en tiempo de ejecución (proceso conocido como duck typing). No existen palabras clave para activar este comportamiento ni modificadores para evitarlo, ya que la flexibilidad de ejecución es la base misma del lenguaje.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

Para ilustrar el polimorfismo y la sobrescritura, se presenta un diseño donde la clase base define un comportamiento genérico que las subclases pueden modificar. En este caso, se utiliza la ligadura dinámica de Java para que, al recorrer una colección de tipo general, se ejecute la implementación específica de cada objeto real.

La clase Zapador reemplaza íntegramente la lógica del padre, mientras que el Artillero mantiene el comportamiento original (o podría haberlo extendido). Al invocar el método desde una referencia de tipo Soldado, el entorno de ejecución de Java determina automáticamente a qué versión del método llamar basándose en la instancia almacenada en la memoria heap.

Java
// Clase base
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soldado " + nombre + ": ¡Presente!");
    }
}

// Subclase que sobrescribe por completo el comportamiento
class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        // Sustitución completa del comportamiento original
        System.out.println("Zapador " + nombre + ": ¡Despejando el camino, señor!");
    }
}

// Subclase que hereda el comportamiento base
class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }
}

public class PruebaPolimorfismo {
    public static void main(String[] args) {
        // Creación de un array de la superclase (Polimorfismo de tipos)
        Soldado[] peloton = {
            new Zapador("Vargas"),
            new Artillero("Bravo")
        };

        // Recorrido empleando referencias del supertipo
        for (Soldado s : peloton) {
            // Gracias a la ligadura dinámica, se ejecuta el método del objeto real
            s.saludar();
        }
    }
}
En la ejecución de este código, se observa que aunque el bucle trata a todos como Soldado, la salida por consola muestra mensajes distintos. El primer elemento invoca la versión modificada de Zapador, mientras que el segundo ejecuta la versión por defecto de la clase Soldado. Esta capacidad de respuesta diferenciada ante una misma llamada es la esencia del polimorfismo.

Desde el punto de vista de la gestión de memoria, el array almacena punteros (referencias) a objetos que pueden ser de cualquier tipo dentro de la jerarquía. El programador no necesita preocuparse por realizar casting manual ni comprobaciones de tipo, ya que el sistema de despacho de Java asegura que el flujo de ejecución llegue a la implementación correcta de forma transparente.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Es totalmente posible invocar el método de la clase base desde una sobrescritura para reutilizar su lógica y complementarla con nuevas acciones. Esto evita la duplicación de código y permite que la subclase extienda el comportamiento original en lugar de simplemente reemplazarlo. Para lograr esto, se utiliza la palabra clave super.

Dentro del cuerpo del método sobrescrito en la subclase, la expresión super.nombreDelMetodo() indica a la Máquina Virtual de Java que debe buscar y ejecutar la implementación que reside en la superclase inmediata. A diferencia de lo que ocurre en los constructores, donde la llamada a super() debe ser obligatoriamente la primera línea, en los métodos ordinarios se puede invocar a la versión del padre en cualquier momento del flujo de ejecución, permitiendo realizar acciones antes o después de dicha llamada.

En el caso del Zapador, se puede invocar el saludo estándar de la clase Soldado para que imprima el mensaje genérico y, acto seguido, añadir la frase específica de su especialidad. Esta técnica garantiza que si en el futuro se modifica la forma en que todos los soldados saludan en la clase base, el Zapador incorporará automáticamente esos cambios y mantendrá su coletilla particular.

Ejemplo de extensión de comportamiento con super
Java
class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void saludar() {
        // Se invoca el comportamiento de la clase base
        super.saludar(); 
        
        // Se añade la funcionalidad específica de la subclase
        System.out.println("¡ZAPADOR A SUS ORDENES!");
    }
}
Al utilizar la palabra clave super, se rompe temporalmente la búsqueda de la ligadura dinámica que normalmente ejecutaría el método de la propia clase (lo que causaría una recursión infinita si se llamara a saludar() sin el prefijo). De esta manera, el lenguaje proporciona un mecanismo limpio para construir comportamientos complejos por capas, respetando la jerarquía de herencia y manteniendo la cohesión del código.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Para que la sobrescritura de métodos sea válida en Java, existen reglas estrictas que garantizan la integridad del polimorfismo. Respecto a los parámetros, estos deben ser exactamente iguales en número, orden y tipo a los del método de la superclase; cualquier variación en la lista de argumentos invalidaría la sobrescritura. En cuanto al tipo de retorno, Java permite lo que se denomina "retorno covariante", lo que significa que el método sobrescrito puede devolver el mismo tipo que el original o una subclase de este, permitiendo una mayor especialización del resultado.

La distinción entre sobrescritura (overriding) y sobrecarga (overloading) es fundamental para entender cómo se resuelve el código. La sobrescritura ocurre entre clases distintas vinculadas por herencia, mantiene la firma del método y se resuelve mediante ligadura dinámica en tiempo de ejecución. Por el contrario, la sobrecarga ocurre generalmente dentro de una misma clase, requiere que la lista de parámetros sea diferente y se resuelve mediante ligadura estática en tiempo de compilación; es, en esencia, definir varios métodos con el mismo nombre pero con distintas entradas.

La anotación @Override sirve para comunicar explícitamente al compilador que la intención del programador es sobrescribir un método de la superclase. No es un requisito sintáctico para que el polimorfismo funcione, pero actúa como un mecanismo de seguridad y documentación. Si se utiliza la anotación y el método no coincide exactamente con ninguno de la clase padre (por un error tipográfico o un cambio en los parámetros), el compilador lanzará un error inmediatamente, evitando fallos lógicos que de otro modo pasarían desapercibidos.

Se recomienda usar @Override siempre por una cuestión de robustez y mantenimiento. Además de detectar errores durante la fase de desarrollo, facilita la lectura del código a otros programadores, indicando claramente qué métodos forman parte de la especialización de la subclase. Sin esta anotación, si un desarrollador modifica la firma de un método en la clase base, las subclases dejarían de sobrescribirlo silenciosamente, convirtiendo esos métodos en simples sobrecargas y rompiendo el comportamiento polimórfico de toda la aplicación.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Efectivamente, el polimorfismo se utiliza en Java desde las etapas más tempranas del aprendizaje, a menudo de forma transparente para el programador. Al sobrescribir métodos como toString() o equals(), se está participando activamente en la jerarquía de herencia universal de Java, donde la clase Object actúa como raíz. Dado que todos los objetos en Java son, en última instancia, de tipo Object, cualquier redefinición de estos métodos es una aplicación directa de la sobrescritura para lograr un comportamiento polimórfico.

Cuando se sobrescribe el método toString(), se está preparando al objeto para que responda de forma específica a una llamada que el sistema realiza mediante ligadura dinámica. Por ejemplo, cuando se utiliza System.out.println(miObjeto), el método println recibe una referencia de tipo Object y llama internamente a toString(). Gracias al polimorfismo, no se ejecuta la versión genérica de la clase Object (que suele imprimir la dirección de memoria), sino la versión especializada que se haya definido en la subclase para mostrar los datos de interés.

Lo mismo sucede con el método equals(Object obj). Al implementarlo, se permite que estructuras de datos genéricas (como las colecciones del framework de Java) comparen objetos basándose en su contenido real y no en su identidad física. El programador de la clase ArrayList, por ejemplo, escribió el código de búsqueda utilizando referencias a Object; es el polimorfismo el que garantiza que, al ejecutar la comparación, se utilice la lógica específica que el desarrollador ha definido en su clase para determinar si dos instancias son equivalentes.

En conclusión, el polimorfismo no es una característica avanzada que se "activa" opcionalmente, sino que es la base sobre la que se construye todo el ecosistema de Java. Cada vez que se personaliza el comportamiento de un objeto frente a los métodos heredados de la clase base, se está confiando en que el sistema será capaz de encontrar la implementación correcta en tiempo de ejecución, demostrando que el polimorfismo es el principio organizativo fundamental del lenguaje.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta es una clase que no representa un objeto completo o concreto, sino un concepto general o una base para una jerarquía. Se utiliza para capturar atributos y comportamientos comunes que deben compartir todas las subclases, pero sin permitir que la clase base sea utilizada para crear objetos por sí misma. En el diseño de sistemas, actúa como un "molde incompleto" o un contrato que define qué deben ser capaces de hacer sus descendientes, asegurando que todos sigan una estructura mínima coherente.

Un método abstracto es una declaración de un método que no tiene implementación (carece de cuerpo). Se define indicando su firma (nombre, parámetros y retorno) seguida de un punto y coma. Su propósito es obligar a las subclases a proporcionar su propia implementación específica. Si una clase contiene al menos un método abstracto, la clase completa debe declararse obligatoriamente como abstracta, ya que el sistema no sabría qué código ejecutar si se intentara llamar a ese método sin cuerpo.

En Java, no es posible crear instancias de una clase abstracta. Si se intenta utilizar el operador new con una clase marcada como abstract, el compilador generará un error. Esta restricción es lógica: al ser una clase incompleta (con métodos sin código), permitir su instanciación dejaría al objeto en un estado inconsistente. Sin embargo, sí se pueden declarar referencias del tipo de la clase abstracta para aprovechar el polimorfismo, siempre que apunten a objetos de subclases concretas que hayan implementado todos los métodos pendientes.

Ejemplo de implementación en Java
Para redefinir la jerarquía, se debe colocar la palabra reservada abstract tanto en la definición de la clase como en la declaración del método que no tiene una implementación genérica.

Java
// Se marca la clase como abstract para evitar su instanciación
abstract class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    // Método concreto: tiene cuerpo y es compartido por todos
    public void saludar() {
        System.out.println("Soldado " + nombre + " presente.");
    }

    // MÉTODO ABSTRACTO: No tiene cuerpo ({...}) y termina en ";"
    // Obliga a las subclases a definir cómo atacan
    public abstract void atacar();
}

class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }

    @Override
    public void atacar() {
        System.out.println(nombre + " dispara proyectiles de artillería.");
    }
}

class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }

    @Override
    public void atacar() {
        System.out.println(nombre + " coloca una carga explosiva.");
    }
}
En este esquema, el método atacar se coloca como abstract en la clase Soldado porque no existe una forma "genérica" de atacar; cada especialista lo hace de manera radicalmente distinta. Al hacerlo así, se garantiza que cualquier objeto que sea tratado como un Soldado en el código (por ejemplo, en un array) tendrá garantizada la existencia del método atacar, permitiendo que el polimorfismo invoque la acción correcta para cada combatiente.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave final en Java actúa como un modificador de restricción que impide la modificación o extensión de un elemento. Cuando se aplica a una clase, significa que dicha clase no puede tener subclases; es decir, se prohíbe la herencia sobre ella. Cuando se aplica a un método, significa que este no puede ser sobrescrito por ninguna subclase, blindando su implementación original para que permanezca inalterable en toda la jerarquía.

La relación de final con el polimorfismo es de exclusión. Dado que el polimorfismo dinámico depende de la capacidad de las subclases para proporcionar implementaciones específicas mediante la sobrescritura, marcar un método como final desactiva el enlace tardío para ese método en particular. El compilador, al saber que el método no puede cambiar, utiliza ligadura estática, lo que puede aportar una ligera mejora en el rendimiento al evitar la consulta en la tabla de métodos virtuales (vtable).

El uso de final suele responder a razones de seguridad y diseño. Al impedir que otros programadores modifiquen el comportamiento de un método o extiendan una clase, se garantiza que el contrato y la lógica interna se mantengan intactos, evitando que una subclase malintencionada o errónea altere el funcionamiento esperado de componentes críticos del sistema. Es una forma de decir que la implementación actual es definitiva y completa.

En la API estándar de Java, el ejemplo más emblemático de una clase final es java.lang.String. Los diseñadores del lenguaje decidieron que las cadenas de texto fueran inmutables y que nadie pudiera heredar de ellas. Esto es fundamental para la seguridad (evita que se pase un objeto que parezca un String pero que cambie su valor internamente) y para la optimización del "String Pool" en la memoria de la Máquina Virtual de Java. Otras clases como Integer, Double y el resto de las clases envolventes (wrappers) también son final.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las interfaces en Java son estructuras de programación que definen un contrato de comportamiento. Se pueden visualizar como una colección de declaraciones de métodos sin cuerpo que especifican qué debe hacer una clase, pero no cómo debe hacerlo. Para un programador de C, una interfaz es comparable a un archivo de cabecera que solo contiene prototipos de funciones puras, estableciendo un estándar que cualquier clase puede decidir cumplir mediante la palabra clave implements.

Aunque guardan similitudes con las clases abstractas, existen diferencias conceptuales y técnicas fundamentales. Mientras que una clase abstracta puede tener estado (atributos de instancia) y métodos con lógica interna (métodos concretos), una interfaz tradicionalmente solo contiene constantes y métodos abstractos. Además, la clase abstracta se utiliza para definir una identidad común dentro de una jerarquía ("es un"), mientras que la interfaz se emplea para definir capacidades o roles transversales ("es capaz de") que pueden compartir clases totalmente distintas.

Una de las ventajas más potentes de las interfaces es que una clase puede implementar múltiples interfaces de forma simultánea. Esta característica permite a Java mitigar la ausencia de herencia múltiple de clases, evitando los conflictos estructurales que esta conlleva. Así, una clase Soldado puede implementar las interfaces Transportable, Combatiente y Sanitario, adquiriendo las obligaciones de cada una sin estar ligada a una única línea sucesoria rígida.

El uso de interfaces es esencial para el polimorfismo a gran escala. Al programar basándose en interfaces en lugar de clases concretas, se logra un desacoplamiento máximo: el sistema solo necesita saber que un objeto cumple con el contrato de la interfaz (por ejemplo, que tiene un método atacar()), sin importarle a qué familia de clases pertenece realmente. Esto facilita la creación de sistemas modulares donde las piezas se pueden intercambiar con total libertad siempre que respeten la interfaz establecida.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

Este diseño permite observar cómo el polimorfismo facilita la creación de estructuras genéricas (como una Linea) que operan sobre conceptos abstractos, mientras que las implementaciones específicas (2D o 3D) gestionan los detalles técnicos y las restricciones de tipos mediante la inspección de objetos en tiempo de ejecución.La clase Punto actúa como la base conceptual. Al declarar calcularDistanciaA como abstracto, se garantiza que cualquier sistema que maneje puntos pueda solicitar una distancia, delegando la fórmula matemática exacta a las subclases. El uso de instanceof y "downcasting" (conversión explícita hacia abajo en la jerarquía) es aquí una medida de seguridad necesaria: dado que el método recibe un Punto genérico, se debe verificar que el objeto recibido sea del mismo subtipo para poder acceder a sus coordenadas específicas ($z$ en el caso de 3D) y evitar errores de cálculo o excepciones de casting.Implementación del modelo en JavaJavaabstract class Punto {
    // Método abstracto que define el contrato
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;

    public Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        // Verificación de tipo (instanceof) y Downcasting
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Se requiere otro Punto2D");
        }
        Punto2D p2 = (Punto2D) otro; 
        return Math.sqrt(Math.pow(p2.x - this.x, 2) + Math.pow(p2.y - this.y, 2));
    }
}

class Punto3D extends Punto {
    double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Se requiere otro Punto3D");
        }
        Punto3D p3 = (Punto3D) otro;
        return Math.sqrt(Math.pow(p3.x - this.x, 2) + 
                         Math.pow(p3.y - this.y, 2) + 
                         Math.pow(p3.z - this.z, 2));
    }
}

class Linea {
    private Punto p1, p2;

    // La línea acepta cualquier tipo de Punto (Polimorfismo)
    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double obtenerLongitud() {
        // Se invoca el método abstracto; la ligadura dinámica 
        // decidirá qué fórmula usar según el objeto real.
        return p1.calcularDistanciaA(p2);
    }
}
La clase Linea ejemplifica la potencia del polimorfismo de manera excepcional. El programador que diseña Linea no necesita escribir lógica diferente para dos o tres dimensiones, ni usar bloques if para preguntar el tipo de punto. Simplemente invoca p1.calcularDistanciaA(p2). Gracias a la ligadura dinámica, si la línea se construyó con Punto3D, se ejecutará automáticamente la lógica tridimensional.Este enfoque promueve un código altamente desacoplado. Si en el futuro se decidiera añadir puntos en cuatro dimensiones (Punto4D), la clase Linea seguiría funcionando perfectamente sin cambiar una sola línea de su código, siempre que el nuevo tipo de punto implemente correctamente su contrato de distancia. Esto demuestra cómo el polimorfismo permite que las capas superiores de una aplicación sean inmunes a los cambios en los detalles de implementación de las capas inferiores.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces en Java es el mecanismo por el cual una interfaz puede heredar las definiciones de métodos de una o más interfaces existentes. A diferencia de la herencia de clases, donde se utiliza la palabra clave implements para que una clase adquiera un comportamiento, entre interfaces se emplea la palabra clave extends. Esto permite crear jerarquías de contratos, donde una interfaz hija representa una especialización o una ampliación de las capacidades definidas en la interfaz padre.

En Java, sí existe la herencia múltiple de interfaces. Aunque el lenguaje prohíbe que una clase herede de varios padres para evitar ambigüedades estructurales, una interfaz puede extender simultáneamente múltiples interfaces. Esto es posible porque las interfaces definen comportamientos abstractos sin implementaciones de estado (atributos de instancia); si dos interfaces padres definen el mismo método, la interfaz hija simplemente hereda una única firma que deberá ser implementada por la clase concreta que finalmente asuma el contrato.

Este diseño es fundamental para mantener la cohesión en sistemas complejos. Permite definir interfaces granulares (pequeñas y específicas) y luego combinarlas en interfaces más robustas según sea necesario. De este modo, una clase puede decidir si solo necesita implementar la funcionalidad básica o si, por el contrario, desea cumplir con un contrato extendido que abarque múltiples responsabilidades heredadas.

Ejemplo de Herencia de Interfaces
En este ejemplo, se observa cómo la interfaz FicheroEscribible amplía el contrato base de Fichero, añadiendo capacidades de modificación sin perder la obligación de lectura.

Java
// Interfaz base: define solo lectura
interface Fichero {
    String leerContenido();
}

// Herencia de interfaces: FicheroEscribible "es un" Fichero con más capacidades
// Aquí se podría poner "extends Fichero, OtraInterfaz" si fuera necesario
interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

// Clase concreta que debe implementar TODOS los métodos de la jerarquía
class FicheroTexto implements FicheroEscribible {
    private String datos = "";

    @Override
    public String leerContenido() {
        return "Contenido: " + datos;
    }

    @Override
    public void escribirContenido(String contenido) {
        this.datos = contenido;
    }

    @Override
    public void eliminar() {
        this.datos = null;
        System.out.println("Fichero vaciado.");
    }
}
El uso de la herencia de interfaces facilita el polimorfismo en diferentes niveles. Se podría tener un método que acepte cualquier Fichero (solo para lectura) y otro que acepte específicamente un FicheroEscribible. Un objeto de la clase FicheroTexto sería compatible con ambos métodos, permitiendo que el sistema trate a los objetos con el nivel de restricción o capacidad adecuado según el contexto de la operación.
