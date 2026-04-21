<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La herencia se define como un mecanismo fundamental de la programación orientada a objetos que permite crear una nueva clase a partir de una ya existente. Esta relación se articula bajo el concepto "A es-un B", lo que implica que la subclase (clase derivada) es una versión especializada de la superclase (clase base). Al establecer esta jerarquía, se asume que cualquier instancia de la subclase posee la esencia de la clase superior, permitiendo organizar el código de lo general a lo particular.

La primera implicación es la herencia de estado y comportamiento. Esto significa que la subclase adquiere automáticamente los atributos (estado) y los métodos (comportamiento) definidos en la superclase, siempre que su visibilidad lo permita. Aunque los atributos sean privados, estos forman parte del objeto derivado y pueden gestionarse mediante los constructores o métodos de la clase base. En comparación con la programación en C, esto evita la duplicación de estructuras de datos y lógica común en módulos que comparten una base conceptual.

La segunda implicación es la compatibilidad de tipos, un concepto clave para el polimorfismo. Se establece que un objeto de una subclase puede ser tratado legalmente como un objeto de su superclase. Esto permite que una variable de referencia del tipo base pueda apuntar a cualquier objeto de sus tipos derivados. Gracias a esta propiedad, es posible diseñar estructuras de datos o funciones genéricas que operen sobre la clase padre, siendo capaces de manejar automáticamente cualquier especialización futura sin necesidad de modificar el código existente.

Ejemplo de implementación en Java
Java
// Superclase que define la base común
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soldado " + nombre + " reportándose.");
    }
}

// Subclase que añade comportamiento de Artillero
class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre); // Llamada al constructor de la superclase
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

// Subclase que añade comportamiento de Zapador
class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

// Clase para ejecutar el ejemplo y demostrar compatibilidad
public class GestionEjercito {
    public static void main(String[] args) {
        // Compatibilidad de tipos: un array de Soldado acepta subtipos
        Soldado[] ejercito = {
            new Artillero("Bravo", 10),
            new Zapador("Sierra", 5),
            new Artillero("Tango", 8)
        };

        // Recorrido polimórfico
        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
En el código expuesto, se observa cómo el array de tipo Soldado almacena instancias de Artillero y Zapador indistintamente. Al recorrer la estructura, se invoca el método saludar() definido en la superclase, el cual es ejecutado por todos los objetos gracias a la herencia de comportamiento, independientemente de sus atributos específicos adicionales.

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Cuando se crea una instancia de una clase derivada (subclase), se ejecutan de manera encadenada los constructores de toda la jerarquía de herencia, desde la clase más general hasta la más específica. En el caso de los soldados concretos como el Artillero, se ejecutan dos constructores: primero el de la superclase (Soldado) y después el de la propia subclase (Artillero). Este orden ascendente-descendente garantiza que el estado heredado esté correctamente inicializado antes de que la subclase intente añadir o modificar su propio estado.

La palabra reservada super dentro de un constructor se utiliza para invocar de forma explícita al constructor de la clase base. Su función principal es pasar los argumentos necesarios desde la subclase hacia la superclase para que esta última pueda realizar su propia inicialización. En Java, el uso de super debe ser siempre la primera instrucción dentro del cuerpo del constructor de la subclase, asegurando así que la base del objeto esté construida antes de ejecutar cualquier lógica adicional.

Respecto a la obligatoriedad de su uso, si la clase base carece de un constructor sin parámetros (o este no es visible debido a modificadores de acceso), es obligatorio llamar a super explícitamente indicando los argumentos correctos. En Java, si el programador no escribe una llamada a super, el compilador intenta insertar automáticamente una llamada al constructor vacío de la clase padre (super()). Si dicho constructor no existe en la superclase por haber definido otros con parámetros, se producirá un error de compilación que fuerza al desarrollador a realizar la llamada manual.

Esta mecánica es análoga a la necesidad en C de inicializar ciertos campos de una estructura antes de operar con ellos, pero automatizada mediante el sistema de tipos de Java. Al forzar la llamada a super, se protege la integridad del objeto, impidiendo que una subclase ignore la lógica de inicialización (como la asignación del nombre en el ejemplo previo) definida por el programador de la superclase.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

En el modelo de memoria de Java, cuando se crea una instancia de una subclase, el objeto resultante en el heap contiene todos los atributos definidos tanto en su propia clase como en todas sus superclases, sin importar los modificadores de acceso aplicados. Esto significa que los atributos privados de la clase base sí forman parte físicamente de la instancia de la subclase. El objeto no se fragmenta en piezas separadas; es un bloque de memoria único y continuo que reserva espacio para la totalidad del estado heredado.

Sin embargo, la presencia física en memoria no implica accesibilidad directa desde el código. Aunque el atributo privado resida dentro del objeto, las reglas de encapsulación de Java impiden que la subclase acceda a él por su nombre. Esto se asemeja a tener una caja fuerte dentro de una habitación: la caja está físicamente allí, pero si es privada, solo quien posea la "llave" (los métodos de la clase que la definió) puede manipular su contenido. La subclase "es" el objeto completo, pero su código solo tiene visibilidad sobre las secciones que han sido declaradas como públicas o protegidas.

En el ejemplo propuesto, cuando se instancia un Artillero, el objeto en memoria reserva espacio para el atributo cohetes y también para el atributo nombre. A pesar de que nombre es parte del Artillero, si se intenta escribir this.nombre dentro de un método de la clase Artillero, el compilador generará un error. La única forma que tiene el Artillero de interactuar con ese espacio de memoria es a través de la interfaz que la clase Soldado haya proporcionado, como el constructor mediante super(nombre) o un método público de acceso.

Esta distinción es fundamental para mantener la integridad del diseño. Al marcar un atributo como privado en la superclase, el programador asegura que, aunque las subclases crezcan y se especialicen, la lógica interna de la base permanece protegida de modificaciones accidentales. En definitiva, la subclase posee el estado privado en memoria, pero no tiene la autorización para manipularlo directamente desde su propio código fuente.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La extensibilidad es la capacidad de un sistema de software para permitir la incorporación de nuevas funcionalidades o tipos de datos con un impacto mínimo o nulo en el código ya existente. En el paradigma de la orientación a objetos, la compatibilidad de tipos es el motor de esta propiedad. Al diseñar algoritmos o estructuras de datos que operan sobre una superclase (el tipo general), se crea un sistema "abierto a la extensión pero cerrado a la modificación", ya que el código base no necesita conocer las implementaciones específicas de las futuras subclases para interactuar con ellas.

En términos prácticos, esto significa que el programador puede añadir una lógica totalmente nueva simplemente creando una nueva clase que herede de la base. Debido a que la jerarquía garantiza que la nueva subclase cumple con el contrato de la superclase (es decir, implementa los mismos métodos públicos), cualquier parte del sistema que ya funcionara con la clase padre aceptará automáticamente a la nueva clase hija. Esto reduce drásticamente el riesgo de introducir errores en el código consolidado, ya que no es necesario editar bucles, condicionales o métodos de gestión para dar cabida al nuevo componente.

Para ilustrar este concepto, se puede añadir un nuevo tipo de soldado, el Medico, cuya función principal es curar, pero que también hereda la identidad de Soldado. Al integrarlo en el sistema previo, se observa que el código encargado de procesar el "Pase de revista" permanece intacto, demostrando una extensibilidad perfecta.

Ejemplo de extensión sin modificación de código
Java
// Nueva especialización añadida posteriormente
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }

    public void curar() {
        System.out.println("Curando heridos en el campo.");
    }
}

// Clase de gestión que demuestra la extensibilidad
public class SistemaMilitar {
    public static void main(String[] args) {
        // El array de Soldado ahora incluye un Medico sin problemas de tipo
        Soldado[] peloton = {
            new Artillero("Bravo", 10),
            new Zapador("Sierra", 5),
            new Medico("Desmond", 3) // Nueva clase integrada
        };

        // ESTE BLOQUE DE CÓDIGO NO CAMBIA:
        // Sigue funcionando perfectamente aunque hayamos añadido un tipo nuevo
        System.out.println("--- Pase de revista (Código Original) ---");
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }
}
Como se aprecia en el ejemplo, la estructura del bucle for que invoca el método saludar() es totalmente agnóstica a la existencia del Medico. Al cumplir el Medico con la relación "es-un" Soldado, el sistema lo procesa como a cualquier otro miembro del pelotón. Esta capacidad permite que los sistemas crezcan en complejidad y funcionalidades manteniendo un núcleo de código estable y fácil de mantener, a diferencia de los enfoques procedimentales donde a menudo sería necesario actualizar múltiples estructuras switch o if-else para manejar un nuevo tipo de dato.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

En Java, el sistema de tipos permite una gran flexibilidad mediante el uso de referencias de la jerarquía de herencia. Es totalmente posible, y de hecho es una práctica estándar, tener una referencia de un supertipo apuntando a un objeto real de un subtipo. Esta capacidad es la base del polimorfismo: una variable de tipo Soldado puede almacenar la dirección de memoria de un objeto Artillero porque, siguiendo la semántica "es-un", un artillero posee todas las características garantizadas por la clase base.

Sin embargo, existe una limitación importante respecto a la visibilidad de los miembros: no se pueden invocar métodos específicos del subtipo utilizando una referencia del supertipo. El compilador de Java solo permite acceder a los métodos y atributos que existen en el tipo declarado de la referencia. Si la variable es de tipo Soldado, el compilador solo garantiza que el objeto tiene el método saludar(); no puede asegurar que tenga el método getCohetes(), ya que la referencia podría estar apuntando a cualquier otro tipo de soldado que no posea dicha funcionalidad.

Para gestionar estas situaciones, se utilizan los conceptos de upcasting y downcasting. El upcasting es la conversión de una referencia de un subtipo a un supertipo (por ejemplo, tratar un Artillero como Soldado); es automático y siempre seguro. El downcasting es la operación inversa: convertir una referencia de un tipo general a uno más específico. Esta última operación no es automática y requiere un "cast" explícito, ya que conlleva el riesgo de que el objeto real no sea del tipo esperado, lo que provocaría un error en tiempo de ejecución.

Para realizar un downcasting seguro, se emplea el operador instanceof. Este operador es un mecanismo de introspección que permite verificar en tiempo de ejecución si un objeto pertenece a una clase determinada o a alguna de sus subclases. Devuelve un valor booleano, lo que permite al programador confirmar la identidad real del objeto antes de forzar la conversión de tipo, evitando así excepciones críticas en el programa.

Ejemplo: Recorrido con comprobación de tipo
Java
public class GestionSuministros {
    public static void main(String[] args) {
        // Array de supertipo con objetos de subtipos (Upcasting automático)
        Soldado[] peloton = {
            new Artillero("Bravo", 12),
            new Zapador("Sierra", 5),
            new Artillero("Tango", 15)
        };

        for (Soldado s : peloton) {
            // El método saludar es común y accesible para la referencia Soldado
            s.saludar();

            // Comprobación de identidad con instanceof
            if (s instanceof Artillero) {
                // Downcasting explícito para acceder a métodos específicos
                Artillero a = (Artillero) s;
                System.out.println("-> Munición disponible: " + a.getCohetes() + " cohetes.");
            }
        }
    }
}
En este ejemplo, el bucle procesa a cada elemento como un Soldado. Al detectar mediante instanceof que una referencia concreta apunta realmente a un Artillero, se realiza el downcasting para "desbloquear" el acceso al método getCohetes(), el cual no sería visible utilizando únicamente la referencia genérica del array.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El nivel de acceso protegido (protected) es un modificador de visibilidad intermedio que flexibiliza la encapsulación en favor de la jerarquía de clases. Mientras que los miembros privados son estrictamente internos a su clase y los públicos son accesibles desde cualquier lugar, lo protegido permite que los atributos o métodos sean visibles para la propia clase, para sus subclases (independientemente del paquete en el que se encuentren) y para otras clases dentro del mismo paquete.

En el diseño de software, el uso de protected se justifica cuando se desea que las clases derivadas manipulen directamente el estado heredado para optimizar el rendimiento o simplificar la lógica de implementación, sin exponer esos detalles al resto del mundo (usuarios de la clase). Es una forma de decirle al programador de la subclase: "Confío en que sabes cómo gestionar este dato", manteniendo al mismo tiempo una barrera de seguridad contra el acceso externo no autorizado.

Para implementar este nivel de acceso en Java, se antepone la palabra reservada protected a la declaración del atributo o método. Esto altera la regla de visibilidad que se aplicaba con los miembros privados; en el ejemplo del Soldado, cambiar el acceso del nombre a protegido permite que el Zapador lea o modifique esa variable directamente usando la referencia this, eliminando la necesidad de pasar por métodos intermediarios como getters o setters.

Ejemplo de uso de protected
Java
// Superclase con atributo protegido
class Soldado {
    // Al ser protected, es visible para sus hijos
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soldado " + nombre + " presente.");
    }
}

// Subclase que accede directamente al estado de la superclase
class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        if (minas > 0) {
            minas--;
            // Acceso directo a 'nombre' gracias a que es protected
            System.out.println("El zapador " + this.nombre + " ha colocado una mina.");
            System.out.println("Minas restantes: " + minas);
        }
    }
}

public class PruebaProteccion {
    public static void main(String[] args) {
        Zapador z = new Zapador("Vargas", 3);
        z.ponerMina();
    }
}
En este código, se observa cómo el método ponerMina() utiliza el atributo nombre como si hubiera sido declarado dentro de la propia clase Zapador. Esta técnica agiliza la escritura de código en las subclases, aunque debe usarse con precaución para no violar excesivamente el principio de ocultación de información, ya que cualquier cambio en el tipo o nombre del atributo en la superclase obligaría a revisar todas las subclases dependientes.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?
En el diseño de lenguajes orientados a objetos, la existencia de una raíz común en la jerarquía de clases depende de la filosofía del lenguaje. En los lenguajes de jerarquía unificada, existe una clase base universal de la que heredan todas las demás, facilitando la creación de estructuras de datos genéricas. Sin embargo, no todos los lenguajes siguen este modelo; en C++, por ejemplo, no existe una clase raíz obligatoria, permitiendo que existan múltiples jerarquías independientes o "bosques" de clases, lo que prioriza la eficiencia sobre la uniformidad.

En el caso específico de Java, el lenguaje adopta una jerarquía estrictamente unificada. La clase Object, perteneciente al paquete java.lang, se sitúa en la cúspide de todo el sistema. Cualquier clase que se defina en Java, aunque no se especifique explícitamente mediante la palabra extends, hereda automáticamente de Object. Esto garantiza que todos los objetos compartan un conjunto de comportamientos básicos y puedan ser tratados bajo un tipo común cuando la situación lo requiera.

Esta herencia universal en Java tiene implicaciones fundamentales para el manejo de memoria y la interoperabilidad. Al ser Object el ancestro común, es posible crear colecciones o métodos que acepten cualquier tipo de objeto. Además, esta clase base proporciona métodos esenciales que todo objeto debe poseer, como equals() para la comparación, hashCode() para la indexación en estructuras de datos o toString() para obtener una representación textual, asegurando una interfaz mínima coherente en todo el ecosistema de Java.

Para los programadores que provienen de C, este concepto puede resultar novedoso, ya que en Java la compatibilidad de tipos siempre tiene un punto de encuentro final. Mientras que en C una función genérica solía depender de punteros void* sin tipo definido, en Java se utiliza la referencia a Object. Esto permite que incluso tipos de datos dispares como un Soldado, una Exception o un String puedan ser almacenados en un mismo array de tipo Object[], aprovechando la compatibilidad de tipos en su nivel más abstracto.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La herencia múltiple es una característica de algunos lenguajes de programación orientada a objetos que permite que una clase herede atributos y métodos de más de una superclase simultáneamente. En este modelo, una subclase puede tener varios padres directos, lo que facilita la combinación de funcionalidades de distintas jerarquías. Sin embargo, esto introduce el problema de la ambigüedad, conocido comúnmente como el "problema del diamante", que ocurre cuando dos superclases definen un método con el mismo nombre y la subclase no sabe cuál de ellos debe ejecutar.

En el caso de Java, los diseñadores del lenguaje decidieron no incluir herencia múltiple de clases para evitar la complejidad y los errores derivados de dichas ambigüedades. En Java, una clase solo puede extender (extends) a una única superclase. Esta restricción garantiza una jerarquía clara y lineal, simplificando significativamente el mantenimiento del código y la implementación de la Máquina Virtual de Java (JVM) en comparación con lenguajes que sí la permiten, como C++.

A pesar de esta limitación, Java ofrece una alternativa para lograr un efecto similar de forma segura: las interfaces. Una clase en Java puede implementar múltiples interfaces simultáneamente. Las interfaces permiten que una clase adquiera comportamientos de diferentes fuentes (por ejemplo, que un Soldado sea a la vez Entrenable y Transportable) sin los riesgos de la herencia múltiple de estado, ya que las interfaces definen principalmente contratos de comportamiento y no estructuras de datos complejas.

Para un programador con experiencia en C, esta distinción es clave. Mientras que en C++ se recurre a la herencia múltiple para dotar a una clase de diversas capacidades, en Java se fomenta el uso de la composición o el uso de múltiples interfaces. Este enfoque promueve un diseño más desacoplado y evita los conflictos de nombres que suelen aparecer en sistemas grandes donde las jerarquías de clases se vuelven demasiado profundas o interconectadas.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En Java, las excepciones son objetos que forman parte de una jerarquía de clases específica. Para crear una excepción personalizada no controlada (unchecked exception), la nueva clase debe heredar de RuntimeException. A diferencia de las excepciones controladas, estas no obligan al programador a declarar una cláusula throws ni a usar bloques try-catch, lo que resulta ideal para errores de lógica o situaciones donde el programa no puede recuperarse razonablemente.

La composición dentro de una excepción permite adjuntar información contextual valiosa para el depurado. Al incluir un objeto de tipo Usuario dentro de UsuarioNoEncontradoException, se facilita que el código encargado de gestionar el error identifique exactamente qué entidad causó el conflicto. Esta técnica es más robusta que simplemente pasar una cadena de texto, ya que permite acceder a todos los métodos y atributos del objeto que falló en el momento de la captura.

Finalmente, es una buena práctica permitir la propagación de la causa. Al sobrecargar los constructores para aceptar un objeto de tipo Throwable, se mantiene la "cadena de excepciones". Esto permite saber si nuestra excepción personalizada fue provocada por un error previo (como una desconexión de base de datos o un error de entrada/salida), facilitando el rastreo del origen real del problema a través de la traza de la pila.

Ejemplo de Excepción Personalizada
Java
// Clase auxiliar para el ejemplo
class Usuario {
    private String login;
    public Usuario(String login) { this.login = login; }
    public String getLogin() { return login; }
}

// Excepción personalizada no controlada (Runtime)
class UsuarioNoEncontradoException extends RuntimeException {
    // Composición: la excepción contiene el objeto que dio error
    private final Usuario usuario;

    // Constructor básico
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Constructor sobrecargado que permite incluir la causa (Throwable)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}

// Ejemplo de uso
public class GestionUsuarios {
    public static void main(String[] args) {
        Usuario u = new Usuario("soldado_ryan");
        
        // Lanzamiento de la excepción ante un error lógico
        throw new UsuarioNoEncontradoException("No existe el usuario en el sistema", u);
    }
}
En este código, se observa cómo la clase hereda de RuntimeException para cumplir con el requisito de ser no controlada. Gracias a la sobrecarga de constructores, se puede instanciar la excepción simplemente con el mensaje y el objeto, o añadir un tercer parámetro para vincularla a un error original, cumpliendo así con los estándares de robustez en Java.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

Este es uno de los dilemas más importantes en el diseño de software. Aunque la herencia permite reutilizar código, su propósito principal es la clasificación jerárquica bajo la premisa de que una clase "es-un" tipo de la otra. Emplearla solo por conveniencia técnica, cuando no existe una relación lógica de identidad, suele derivar en diseños rígidos y frágiles que complican el mantenimiento a largo plazo.

La principal razón para evitar la herencia como primera opción es el fuerte acoplamiento que genera. En Java, una subclase depende íntimamente de la implementación interna de su superclase (lo que se conoce como el problema de la "clase base frágil"). Si se modifica la clase padre para adaptar su comportamiento a una nueva necesidad, se corre el riesgo de alterar o romper involuntariamente todas las subclases que heredaron ese código, a menudo sin que el compilador detecte el error lógico.

Por el contrario, la composición (la relación "tiene-un") ofrece una flexibilidad mucho mayor. Al incluir una instancia de otra clase como un atributo privado, se delega la funcionalidad sin exponer la estructura interna de la clase proveedora. Esto permite cambiar el comportamiento en tiempo de ejecución (cambiando el objeto compuesto) y mantiene las clases aisladas entre sí. Si solo se desea reutilizar una lógica específica (por ejemplo, un sistema de disparo para un soldado), es mejor que el soldado "tenga" un arma a que el soldado "herede" de la clase arma.

En la ingeniería de software moderna se promueve el principio de "favorecer la composición sobre la herencia". La herencia debe reservarse para casos donde la sustitución polimórfica sea necesaria y la relación jerárquica sea natural y estable. Si el único objetivo es evitar escribir el mismo código dos veces, la composición suele ser la herramienta más segura y profesional, ya que permite construir sistemas modulares donde las piezas se combinan en lugar de encadenarse.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

La máxima de "favorecer la composición frente a la herencia" es uno de los pilares del diseño de software robusto. Aunque la herencia es una herramienta poderosa, su uso indebido para simplemente "ahorrar código" suele crear sistemas rígidos. La razón fundamental es que la herencia establece una relación de acoplamiento fuerte, mientras que la composición permite un acoplamiento débil, lo que facilita enormemente el mantenimiento y la evolución del programa.

El principal problema de la herencia es que rompe la encapsulación (fenómeno conocido como el problema de la clase base frágil). Una subclase depende de los detalles de implementación de su superclase; si la clase padre cambia su comportamiento interno en versiones futuras, puede romper involuntariamente la lógica de todas sus subclases. En cambio, en la composición, la comunicación entre objetos se realiza exclusivamente a través de sus interfaces públicas, lo que permite cambiar la lógica interna de un componente sin que el objeto que lo contiene sufra ningún impacto.

Además, la composición ofrece una flexibilidad dinámica que la herencia no puede igualar. En Java, la herencia es estática: una vez que un Artillero hereda de Soldado, no puede dejar de serlo ni cambiar su comportamiento base en tiempo de ejecución. Con la composición, si un Soldado posee un objeto HabilidadDisparo, este objeto podría sustituirse por otro (por ejemplo, DisparoRafaga o DisparoPreciso) mientras el programa se está ejecutando, permitiendo que el objeto cambie de comportamiento según el contexto.

En conclusión, la herencia debe reservarse estrictamente para relaciones de identidad claras ("es-un") donde el polimorfismo sea imprescindible. Para la reutilización de funcionalidades, la composición es la opción preferible, ya que permite construir sistemas modulares donde las piezas son intercambiables, fáciles de probar de forma aislada y mucho menos propensas a errores en cascada ante cambios en la arquitectura.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

La afirmación de que la herencia rompe la encapsulación se refiere a que la seguridad y el aislamiento que ofrece el principio de ocultación de información se ven comprometidos cuando una subclase depende de los detalles internos de su superclase. En el desarrollo con Java, esto se conoce técnicamente como el problema de la clase base frágil, donde la frontera que separa la interfaz pública de la implementación interna se vuelve borrosa entre padre e hijo.

En una relación de composición, un objeto solo conoce los métodos públicos del objeto que contiene. Sin embargo, en la herencia, la subclase suele estar diseñada con un conocimiento profundo de cómo funciona su superclase para poder extenderla o sobrescribir sus métodos. Si el programador de la superclase decide cambiar un detalle de implementación (por ejemplo, cambiar el orden en que se llaman internamente dos métodos o modificar un atributo protected), puede provocar fallos en cadena en todas las subclases, a pesar de que la interfaz pública parezca no haber cambiado.

Para ilustrarlo con el ejemplo de los soldados, supongamos que la clase Soldado tiene un método saludar() que internamente llama a un método obtenerRango(). Si el programador del Artillero sobrescribe saludar() basándose en esa lógica interna, y más tarde el creador de Soldado modifica la forma en que se gestionan los rangos o elimina esa llamada interna por eficiencia, el Artillero dejará de funcionar correctamente. La subclase ha quedado "expuesta" a las tripas de la clase base, perdiendo esa protección de caja negra que define a la encapsulación.

En resumen, la encapsulación busca que los cambios internos de una clase no afecten a quienes la usan. La herencia, al crear una unión tan íntima y dependiente del código heredado, hace que cualquier evolución en la jerarquía sea arriesgada. Por esta razón, se dice que la composición preserva mejor la encapsulación, ya que los objetos interactúan como entidades totalmente independientes que solo se conocen a través de sus contratos públicos.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

Esta comparativa permite visualizar cómo una misma necesidad de modelado se resuelve mediante dos filosofías de diseño distintas en Java. En ambos casos el objetivo es gestionar el nombre y el DNI, pero la relación entre las clases cambia radicalmente.

En la primera alternativa (Herencia), se define una relación de identidad. Se afirma que un Estudiante es una Persona. Esto implica que el estudiante hereda automáticamente todo el estado y comportamiento de la clase base, integrándolo en su propia naturaleza. Es una solución jerárquica y rígida, pero muy potente para aplicar polimorfismo (por ejemplo, tratar a todos como objetos de tipo Persona).

En la segunda alternativa (Composición), se define una relación de posesión. Se afirma que un Estudiante tiene DatosPersonales. Aquí, la información personal no es la base del objeto, sino un componente externo que el estudiante incorpora. Esta opción es más flexible y favorece el desacoplamiento, ya que la lógica de los datos personales está encapsulada en su propia clase y el estudiante simplemente delega en ella cuando necesita consultar el DNI o el nombre.

Alternativa A: Modelado mediante Herencia
Java
// Superclase que define la identidad común
class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

// Subclase que "es una" Persona
class Estudiante extends Persona {
    private String universidad;

    public Estudiante(String dni, String nombre, String universidad) {
        super(dni, nombre); // Inicializa la parte de Persona
        this.universidad = universidad;
    }
}

class Trabajador extends Persona {
    private double salario;

    public Trabajador(String dni, String nombre, double salario) {
        super(dni, nombre);
        this.salario = salario;
    }
}
Alternativa B: Modelado mediante Composición
Java
// Clase componente que agrupa los datos
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

// Clase que "tiene" DatosPersonales
class Estudiante {
    private DatosPersonales datos; // Composición
    private String universidad;

    public Estudiante(DatosPersonales datos, String universidad) {
        this.datos = datos; // Se recibe la instancia ya creada
        this.universidad = universidad;
    }

    public String getNombre() { return datos.getNombre(); }
}

class Trabajador {
    private DatosPersonales datos; // Composición
    private double salario;

    public Trabajador(DatosPersonales datos, double salario) {
        this.datos = datos;
        this.salario = salario;
    }
    
    public String getNombre() { return datos.getNombre(); }
}
En la alternativa de composición, se observa que para obtener el nombre desde un Trabajador, este debe pedírselo internamente a su objeto datos. Aunque requiere un poco más de código para "delegar" las llamadas, permite que los DatosPersonales puedan existir o cambiar de forma independiente a la lógica laboral o académica de las clases que los contienen.
