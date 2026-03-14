<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.


La encapsulación y la ocultación de información buscan agrupar los datos y los métodos que los manipulan en una sola unidad (la clase), protegiendo el estado interno del objeto frente a interferencias externas. El objetivo fundamental es evitar que el código ajeno a la clase dependa de detalles de implementación que podrían cambiar, o que manipule los datos de forma incorrecta. En C, esto equivale a intentar que nadie toque las variables de una struct directamente, sino que siempre utilicen funciones específicas para ello.

Al aplicar la ocultación, se establece una separación clara entre qué hace un objeto (su interfaz pública) y cómo lo hace (su implementación privada). Esto permite que el programador pueda modificar la estructura interna de una clase —por ejemplo, cambiar un tipo de dato o una fórmula de cálculo— sin que el resto del programa se entere ni deje de funcionar. Se garantiza así la integridad de los datos, ya que el objeto se convierte en el único responsable de validar cualquier cambio en sus propios atributos.

Las ventajas principales de la ocultación de información incluyen:

Robustez: Se evitan estados inconsistentes al impedir que variables externas asignen valores inválidos (como una edad negativa).

Mantenibilidad: Permite cambiar la lógica interna de una clase sin afectar a las partes del sistema que la utilizan.

Seguridad: Protege datos sensibles o críticos de accesos no autorizados o accidentales.

Simplicidad: El usuario de la clase solo necesita conocer los métodos públicos, reduciendo la carga cognitiva al ocultar la complejidad interna.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

 La interfaz pública de una clase es el conjunto de métodos y atributos que son accesibles desde el exterior de la misma. En términos de C, se puede comparar con las declaraciones de funciones en un archivo de cabecera (.h): es el "contrato" o manual de instrucciones que indica al resto del programa qué operaciones se pueden solicitar a un objeto. Esta interfaz define el comportamiento visible, ocultando por completo la lógica interna y la estructura de los datos que permiten realizar dichas tareas.

La relación con la ocultación de información es de complementariedad absoluta: la interfaz pública es la única vía de comunicación autorizada con el objeto. Mientras que la ocultación (mediante el uso de private) mantiene los datos sensibles o complejos bajo llave, la interfaz pública (mediante public) ofrece puntos de entrada controlados. Esto asegura que nadie pueda manipular el estado de un objeto de forma arbitraria, obligando a pasar por filtros o métodos de validación definidos por el programador de la clase.

Un diseño correcto en POO busca que la interfaz pública sea lo más pequeña y estable posible. Al exponer solo lo estrictamente necesario, se reduce el acoplamiento entre las distintas partes del software. Si en el futuro se decide cambiar cómo se almacena un dato internamente (por ejemplo, pasar de un entero a un número de punto flotante), mientras la interfaz pública —los nombres de los métodos y sus parámetros— no varíe, el resto del programa seguirá funcionando sin necesidad de una sola línea de cambio.

Java
public class CuentaBancaria {
    private double saldo; // Ocultación (Información privada)

    // Interfaz pública: el mundo exterior solo ve estos métodos
    public void depositar(double cantidad) {
        if (cantidad > 0) saldo += cantidad;
    }

    public double consultarSaldo() {
        return saldo;
    }
}


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Diseñar la interfaz pública con cuidado es fundamental porque representa el contrato inamovible entre una clase y el resto del sistema. Una vez que otros programadores (o incluso otras partes del propio código) empiezan a utilizar los métodos públicos de una clase, cualquier modificación en sus nombres, parámetros o tipos de retorno provocará errores de compilación en cascada en todo el proyecto. En el desarrollo de software a gran escala, una interfaz mal diseñada actúa como una "fuga" de complejidad que obliga a mantener estructuras deficientes solo para no romper la compatibilidad.

A diferencia de la lógica interna o los atributos privados, que pueden refactorizarse libremente sin afectar a nadie, cambiar una interfaz pública es extremadamente difícil y costoso. Si se elimina o renombra un método público que ya está en uso, se invalidan todas las llamadas existentes a ese método, lo que en C equivaldría a cambiar la firma de una función en un archivo .h compartido por cientos de archivos .c. Por ello, la regla de oro en POO es exponer el mínimo de funcionalidad necesaria y mantener esa superficie de contacto lo más estable posible a lo largo del tiempo.

Esta rigidez es la razón por la cual se prefiere ocultar los datos tras métodos específicos. Si se expone un atributo directamente y luego se decide que su valor debe validarse o calcularse de otra forma, el cambio es traumático. Sin embargo, si la interfaz pública consiste en métodos controlados, la implementación interna puede evolucionar con total libertad mientras la "firma" del método permanezca intacta, preservando la estabilidad de todo el ecosistema de software.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones que siempre deben ser verdaderas para que un objeto sea válido. La ocultación de información es clave aquí porque, al hacer los datos privados, se impide que factores externos los modifiquen de forma que rompan esas reglas. En lugar de permitir el acceso directo, la clase ofrece métodos controlados que validan cada cambio antes de aplicarlo.

Esto garantiza que el objeto mantenga su integridad lógica en todo momento. Al centralizar las reglas dentro de la clase, se evita que los errores se propaguen por el resto del sistema, facilitando la depuración y asegurando que los datos siempre sean coherentes con la realidad que intentan modelar.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

Para implementar la ocultación de información en la clase Punto, los atributos deben marcarse como privados, obligando a que cualquier interacción se realice a través de métodos controlados.

Java
public class Punto {
    // Atributos ocultos al exterior
    private double x;
    private double y;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método que calcula la distancia al origen (0,0)
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(Math.pow(this.x, 2) + Math.pow(this.y, 2));
    }

    // Métodos de la interfaz pública para acceder a los datos
    public double getX() { return x; }
    public double getY() { return y; }
    
    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }
}
Interfaz pública de la clase
La interfaz pública de esta clase está compuesta por el constructor Punto(double, double), el método de cálculo calcularDistanciaAOrigen(), y los métodos de acceso getX, getY, setX y setY. Estos son los únicos elementos que otros objetos pueden ver y utilizar.

Significado de public y private
private: Es un modificador de acceso que restringe la visibilidad del elemento únicamente al interior de la propia clase. Nadie desde fuera puede leer o modificar un atributo private directamente.

public: Indica que el elemento es accesible desde cualquier otra clase o parte del programa. Se utiliza para definir los servicios que el objeto ofrece al mundo exterior.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores public y private tienen alcances distintos dependiendo de a qué elemento se apliquen dentro de la estructura del código:A nivel de Clase:Una clase puede ser public, lo que significa que es accesible desde cualquier otra clase en cualquier paquete.Sin embargo, una clase de nivel superior (la que define el archivo) no puede ser private. Solo las clases "anidadas" o internas (una clase dentro de otra) pueden marcarse como privadas para que solo la clase contenedora pueda utilizarlas.A nivel de Miembros (dentro de la clase):Atributos (variables): Es la aplicación más común. private oculta el dato y public lo expone.Métodos: public define la interfaz que otros pueden usar; private define métodos auxiliares que solo sirven para la lógica interna de esa clase.Constructores: Si un constructor es public, cualquier clase puede crear instancias. Si es private, se impide la instanciación externa (útil en patrones de diseño como el Singleton o en clases de utilidades estáticas).En resumen, mientras que los miembros de una clase pueden usar ambos con total libertad, las clases principales tienen restricciones, ya que una clase privada "suelta" en un archivo no tendría sentido práctico al no poder ser accedida por nadie.¿Desea que le explique qué ocurre cuando no se pone ningún modificador (el llamado acceso por defecto) o prefiere continuar con otro tema?


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Sí, en el paradigma de la Programación Orientada a Objetos existen niveles intermedios de visibilidad que permiten un control más granular que el simple binario público/privado. Estos niveles suelen estar vinculados a la organización del código en carpetas (paquetes o módulos) y a la jerarquía de herencia.

En Java, existen cuatro niveles de visibilidad, aunque solo tres se definen mediante palabras reservadas:

public: Acceso total desde cualquier clase y paquete.

private: Acceso restringido exclusivamente a la propia clase.

protected: Permite el acceso a la propia clase, a sus subclases (aunque estén en paquetes distintos) y a otras clases dentro del mismo paquete.

Acceso por defecto (Package-private): Se aplica cuando no se escribe ningún modificador. Permite el acceso a cualquier clase dentro del mismo paquete, pero no a clases de paquetes externos, ni siquiera a sus propias subclases.

En otros lenguajes, los conceptos son similares pero los nombres y comportamientos varían. Por ejemplo, en C++ también existen public, private y protected, pero su comportamiento en la herencia es más complejo. En Python, la visibilidad es meramente convencional: no existen restricciones reales, pero se usa un guion bajo (_variable) para indicar que algo es "protegido" y doble guion bajo (__variable) para "privado". En Swift, existen niveles adicionales como internal (por defecto en el módulo) y fileprivate (restringido al archivo fuente).


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

La respuesta correcta es la (a): otras clases.

En Java (y en la mayoría de los lenguajes de POO), la visibilidad private se define a nivel de clase, no a nivel de instancia. Esto significa que un objeto puede acceder a los miembros privados de otro objeto, siempre que ambos pertenezcan a la misma clase. No están ocultos para otras instancias del mismo tipo.

Aquí tienes el ejemplo solicitado en la clase Punto:

Java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAPunto(Punto otro) {
        // Aquí accedemos directamente a otro.x y otro.y
        // a pesar de ser atributos privados.
        double dx = this.x - otro.x; 
        double dy = this.y - otro.y;
        return Math.sqrt(Math.pow(dx, 2) + Math.pow(dy, 2));
    }
}
Explicación:
Dentro del método calcularDistanciaAPunto, el objeto actual (this) está manipulando los atributos otro.x y otro.y. Aunque esos campos son private, el código compila y funciona perfectamente porque el método está escrito dentro de la clase Punto. Java asume que si tú has programado la clase, conoces su estructura interna y puedes manipular cualquier instancia de ella de forma segura. Si intentaras hacer ese mismo acceso (otro.x) desde una clase llamada Main o Calculadora, el compilador daría un error.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?
Los métodos getter y setter son las herramientas fundamentales para aplicar el principio de encapsulación en la programación orientada a objetos. Su función principal es actuar como una capa de seguridad o una aduana entre los datos internos de un objeto, que se declaran como privados, y el resto del programa. El getter tiene la misión de devolver el valor de un atributo para que pueda ser consultado desde el exterior, mientras que el setter se encarga de recibir un nuevo valor y asignarlo al atributo correspondiente.

El uso de estos métodos en lugar de exponer los atributos directamente como públicos aporta beneficios estructurales muy importantes. En primer lugar, permite mantener la integridad de los datos mediante validaciones; por ejemplo, un setter puede contener una estructura condicional que rechace valores que no tengan sentido lógico, como una edad negativa o una fecha inexistente, protegiendo así las invariantes de la clase. En segundo lugar, otorgan flexibilidad para cambiar la implementación interna sin romper el código que utiliza la clase. Si en el futuro decidimos cambiar el tipo de dato de un atributo o la forma en que se almacena, solo tendremos que modificar el código interno del getter y el setter, manteniendo la misma firma del método para el exterior.

Además, estos métodos permiten controlar los permisos de acceso de forma granular. Si una clase solo incluye el getter para un atributo pero omite el setter, ese dato se convierte automáticamente en una propiedad de solo lectura para el resto del sistema. En el desarrollo profesional, esta práctica también facilita enormemente las tareas de depuración, ya que es mucho más sencillo colocar un punto de interrupción en un método setter para detectar qué parte del programa está intentando modificar un valor que rastrear todos los accesos directos a una variable pública dispersos por el código.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, en el contexto de la ingeniería de software y la POO, el término "seguridad" no se refiere habitualmente a la ciberseguridad o a la protección contra ataques externos (hackers), sino a la robustez y fiabilidad interna del código.

Cuando decimos que la ocultación de información hace el programa más seguro, nos referimos a que evita que el propio programador, o sus compañeros de equipo, cometan errores accidentales que dejen al objeto en un estado inconsistente o corrupto. Es una seguridad contra el error humano de desarrollo. Al cerrar el acceso directo a los datos, impedimos que una parte del programa modifique una variable de forma imprevista, lo que en sistemas complejos podría provocar fallos en cascada difíciles de detectar.

En C, si una estructura tiene sus campos expuestos, cualquier función en cualquier parte del proyecto puede cambiar un valor por error, y encontrar el origen de ese fallo es como buscar una aguja en un pajar. En Java, al usar private, limitas el "radio de explosión" de un error: si el dato está mal, el problema solo puede estar en los métodos de esa clase. Por tanto, es una seguridad enfocada a la integridad de la lógica y a la estabilidad del sistema a largo plazo, garantizando que el software se comporte exactamente como se diseñó.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?


La diferencia fundamental radica en a quién pertenece la información y cómo se gestiona en la memoria del ordenador.

Un miembro de instancia es aquel que pertenece a cada objeto individual. Si tienes la clase Punto, cada vez que creas un objeto con new, este recibe su propia copia de las variables x e y. Si cambias la x de un punto, la del otro permanece intacta. Es la forma en la que cada objeto mantiene su propia identidad y estado.

Por el contrario, un miembro de clase se define con la palabra reservada static. Este miembro no pertenece a ningún objeto en particular, sino a la clase en su conjunto. Solo existe una única copia en memoria para todo el programa, sin importar si has creado mil objetos o ninguno. Es útil para compartir información común a todas las instancias (como un contador de objetos creados) o para métodos de utilidad que no dependen del estado de un objeto concreto (como las funciones de Math.sqrt).

En cuanto a la ocultación, los miembros de clase se pueden ocultar exactamente igual que los de instancia. Puedes declarar una variable static como private, lo que significará que solo los métodos de esa clase podrán verla o modificarla. De hecho, es una práctica muy recomendada para proteger variables globales de la clase, como configuraciones internas o contadores, evitando que código externo altere el comportamiento compartido de todos los objetos de forma imprevista.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido que un constructor sea privado, aunque a primera vista parezca contradictorio impedir que se creen instancias de una clase desde el exterior. Esta práctica se utiliza principalmente para controlar estrictamente cómo y cuándo se crean los objetos, o incluso para impedir totalmente su creación.

Un uso muy común es en las clases de utilidades, que solo contienen métodos estáticos (como la clase Math en Java). Al hacer el constructor privado, evitas que otros programadores desperdicien memoria creando objetos de una clase que no tiene estado interno y cuyas funciones se pueden usar directamente.

Otro escenario fundamental es la implementación de patrones de diseño como el Singleton, donde se garantiza que solo exista una única instancia de una clase en todo el programa. En este caso, el constructor privado impide que cualquiera use new, y la propia clase se encarga de crear la única instancia permitida y entregarla a través de un método público. También es útil cuando quieres obligar al usuario a utilizar "métodos de factoría" para crear objetos, permitiendo así realizar validaciones complejas o reciclar objetos existentes antes de devolver una nueva instancia.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Los miembros de clase en Java se indican utilizando la palabra reservada static. Al añadir este modificador, el atributo o método deja de pertenecer a una instancia concreta para pasar a ser propiedad de la clase, existiendo una única copia para todos los objetos.Para cumplir con lo solicitado, añadiremos dos atributos estáticos y privados que almacenen los valores máximos encontrados, junto con sus respectivos métodos de acceso. Cada vez que se asigne un valor a x o y (ya sea en el constructor o en los setters), la clase comparará el nuevo valor con el máximo registrado.Javapublic class Punto {
    // Miembros de instancia
    private double x;
    private double y;

    // Miembros de clase (estáticos)
    private static double xMaximo = Double.NEGATIVE_INFINITY;
    private static double yMaximo = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        setX(x);
        setY(y);
    }

    public void setX(double x) {
        this.x = x;
        if (x > xMaximo) {
            xMaximo = x;
        }
    }

    public void setY(double y) {
        this.y = y;
        if (y > yMaximo) {
            yMaximo = y;
        }
    }

    // Getters para los miembros de clase
    public static double getXMaximo() {
        return xMaximo;
    }

    public static double getYMaximo() {
        return yMaximo;
    }

    public double getX() { return x; }
    public double getY() { return y; }
}
En este diseño, xMaximo e yMaximo son compartidos por todos los objetos Punto. Si creas un punto en $(10, 5)$ y otro en $(3, 20)$, al llamar a Punto.getXMaximo() obtendrás $10$ y al llamar a Punto.getYMaximo() obtendrás $20$, independientemente de desde qué objeto (o incluso sin usar objetos) realices la consulta.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
Sí, he usado static. Es imprescindible en los métodos factoría porque el método debe poder invocarse directamente a través de la clase (por ejemplo, Punto.crearPuntoRedondeado(...)) para generar el objeto, antes de que exista cualquier instancia de la clase disponible.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

Para cambiar la estructura interna sin romper el código que ya utiliza la clase, sustituimos los atributos individuales por el array, pero mantenemos los métodos de la interfaz pública con las mismas firmas y tipos de datos que tenían anteriormente.

Java
public class Punto {
    // Cambio en la implementación interna: ahora es un array
    private double[] coordenadas = new double[2];

    private static double xMaximo = Double.NEGATIVE_INFINITY;
    private static double yMaximo = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        setX(x);
        setY(y);
    }

    // La interfaz pública se mantiene idéntica para el usuario externo
    public void setX(double x) {
        this.coordenadas[0] = x;
        if (x > xMaximo) xMaximo = x;
    }

    public void setY(double y) {
        this.coordenadas[1] = y;
        if (y > yMaximo) yMaximo = y;
    }

    public double getX() {
        return this.coordenadas[0];
    }

    public double getY() {
        return this.coordenadas[1];
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(Math.pow(coordenadas[0], 2) + Math.pow(coordenadas[1], 2));
    }

    public static double getXMaximo() {
        return xMaximo;
    }

    public static double getYMaximo() {
        return yMaximo;
    }
}
Como se puede observar, aunque hayamos modificado radicalmente cómo se guardan los datos (de variables sueltas a un array), el "contrato" externo de la clase no ha cambiado. Un programador que use Punto ni siquiera notará que hemos hecho este cambio, lo que demuestra la potencia de la ocultación de información.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

No, no es mejor declararlo público. Aunque a corto plazo parezca que un atributo público y un par de métodos getter/setter logran lo mismo, la diferencia radica en el control y la flexibilidad futura.

La convención más habitual y recomendada en POO (específicamente en Java a través del estándar JavaBeans) es que todos los atributos de instancia deben ser privados. El acceso a estos datos debe realizarse siempre a través de métodos públicos si es necesario.

Relación con las invariantes de clase
La razón principal de esta convención es precisamente la protección de las invariantes de clase.

Atributo público: Si declaras un atributo como público, entregas el control total al exterior. No puedes evitar que alguien asigne un valor que rompa la lógica del objeto (por ejemplo, poner un radio negativo en una clase Circulo). No hay ninguna "aduana" que verifique el dato.

Setter público: Al usar un método set, conservas el poder de interceptar el valor. Si en el futuro decides que ese atributo debe cumplir una nueva regla o restricción, puedes añadir un if dentro del setter para validar el dato antes de guardarlo.

Ventajas adicionales de esta práctica
Evolución del código: Como vimos en el ejemplo del array en la clase Punto, puedes cambiar radicalmente cómo se almacena el dato internamente sin que el resto del programa tenga que cambiar ni una sola línea de código, ya que la interfaz (el método) sigue siendo la misma.

Depuración: Es mucho más fácil encontrar un error si puedes poner un punto de interrupción en un único método set que rastrear cada vez que una variable pública es modificada desde cualquier punto del programa.

Propiedades calculadas: Un getter puede devolver un valor que ni siquiera existe como atributo físico (por ejemplo, un método getNombreCompleto() que concatena nombre y apellido), manteniendo la consistencia de la interfaz.

En resumen: los atributos se ocultan para que el objeto sea el único responsable de mantener su estado válido, utilizando la interfaz pública como el único canal autorizado de modificación.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable cuando su estado (sus datos) no puede ser modificado una vez que el objeto ha sido creado. Para lograr esto, todos sus atributos suelen declararse como private y final, no se proporcionan métodos "setter" y el estado inicial se fija exclusivamente a través del constructor. Si se necesita un cambio, en lugar de modificar el objeto existente, se debe crear una instancia nueva con los nuevos valores.

Un método modificador (también llamado mutador) es cualquier método que altera el estado interno de un objeto, es decir, que cambia el valor de uno o más de sus atributos.

Respecto a tu pregunta sobre los "setters", un método modificador no es siempre un setter. Aunque los setters son los modificadores más comunes (como setX(10)), existen otros métodos que modifican el estado sin seguir esa nomenclatura. Por ejemplo, en una clase CuentaBancaria, un método retirarEfectivo(double cantidad) es un método modificador porque altera el saldo, pero no es un simple setter ya que incluye lógica de negocio y validaciones.

Las ventajas de que una clase sea inmutable son considerables:

Robustez y seguridad: Al no poder cambiar, es imposible que el objeto entre en un estado inconsistente después de su creación.

Programación multihilo (Thread-safety): Varios hilos de ejecución pueden compartir el mismo objeto sin riesgo de que uno lo modifique mientras otro lo lee, eliminando la necesidad de mecanismos complejos de sincronización.

Simplicidad: Son más fáciles de entender, usar y probar, ya que su valor es constante durante todo su ciclo de vida.

Uso como claves: Son candidatos ideales para ser usados como claves en estructuras de datos (como diccionarios o mapas), ya que su valor (y por tanto su código hash) nunca cambiará.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, no es recomendable incluirlos por defecto. Aunque muchas herramientas de desarrollo permiten generarlos automáticamente, hacerlo de forma sistemática es una práctica que suele debilitar el diseño del software y comprometer la integridad de los objetos.

La principal razón para evitar los setters innecesarios es que rompen la encapsulación al exponer las interioridades del objeto, convirtiéndolo en un simple contenedor de datos en lugar de una entidad autónoma. Al permitir que cualquier parte del programa modifique un atributo, pierdes el control sobre las invariantes de clase, ya que resulta mucho más difícil garantizar que el estado interno del objeto sea coherente en todo momento.

Además, incluir setters por convención impide crear clases inmutables, las cuales son mucho más seguras en entornos de programación multihilo y más fáciles de depurar. Un objeto que no puede cambiar después de su creación elimina de raíz una gran cantidad de errores lógicos. La tendencia moderna en el diseño de software es favorecer la inmutabilidad y solo añadir métodos modificadores cuando existe una necesidad real y justificada por la lógica de negocio.

La convención más sólida consiste en declarar todos los atributos como privados y no proporcionar setters a menos que el estado del objeto deba cambiar obligatoriamente durante su ciclo de vida. En muchos casos, es preferible sustituir un setter genérico por un método con un nombre que describa una acción específica, lo que aporta mucha más claridad sobre el comportamiento del sistema y protege mejor la lógica interna.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable. Esto significa que una vez que se crea un objeto de este tipo en la memoria, su contenido no puede ser modificado. Cualquier operación que aparentemente "cambie" el texto de la cadena en realidad está creando un objeto nuevo totalmente distinto con el nuevo contenido.

Cuando concatenas dos cadenas utilizando el operador +, Java no añade caracteres al final del objeto original. En su lugar, el sistema reserva un nuevo espacio de memoria, copia el contenido de la primera cadena, luego el de la segunda y genera una tercera cadena resultante. Si realizas esta operación repetidamente, el programa termina creando y descartando multitud de objetos intermedios, lo que consume memoria y tiempo de procesamiento de forma innecesaria.

Si necesitas construir paso a paso una cadena muy larga o realizar concatenaciones dentro de un bucle, lo más eficiente es utilizar la clase StringBuilder. A diferencia de String, esta clase está diseñada para ser mutable, lo que significa que permite modificar su contenido interno (añadir, insertar o borrar caracteres) sin necesidad de crear nuevos objetos en cada paso. Funciona mediante un búfer interno que crece dinámicamente, lo que hace que la operación sea órdenes de magnitud más rápida y ligera para el sistema.

Al finalizar la construcción con StringBuilder, simplemente se llama al método toString() para obtener la cadena definitiva. Existe también una alternativa llamada StringBuffer, que funciona de manera idéntica pero es segura para entornos multihilo (thread-safe), aunque en la mayoría de los casos de uso general se prefiere StringBuilder por ser ligeramente más veloz al no requerir sincronización.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En programación orientada a objetos, existen dos formas de comparar objetos: por identidad y por contenido. La comparación por identidad utiliza el operador == y verifica si ambas referencias apuntan a la misma dirección de memoria, es decir, si son exactamente el mismo objeto. Por el contrario, la comparación por contenido verifica si los datos almacenados dentro de dos objetos distintos son equivalentes, aunque residan en lugares diferentes de la memoria.

El método equals en Java es la herramienta diseñada específicamente para realizar la comparación por contenido. Por defecto, la implementación de equals que heredan todas las clases (proveniente de la clase base Object) se comporta exactamente igual que el operador ==, comparando únicamente las direcciones de memoria. Por esta razón, cuando creamos nuestras propias clases, debemos "sobrescribir" este método si queremos que sea capaz de comparar los atributos internos de los objetos.

Para comparar dos cadenas de texto (String) en Java, nunca se debe utilizar el operador ==, ya que este solo nos diría si son el mismo objeto en memoria. Lo correcto es utilizar siempre el método equals (o equalsIgnoreCase si queremos ignorar mayúsculas y minúsculas), el cual recorre carácter a carácter ambas cadenas para determinar si representan el mismo texto. Utilizar el operador de igualdad con cadenas es uno de los errores más comunes en Java, ya que puede dar resultados positivos de forma impredecible debido a optimizaciones internas del lenguaje como el String Pool.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper (o clases envoltorio) son clases que encapsulan un tipo de dato primitivo dentro de un objeto. En Java, por ejemplo, cada tipo primitivo tiene su equivalente: int tiene a Integer, double a Double o boolean a Boolean. Su función principal es permitir que los valores primitivos se comporten como objetos, lo cual es necesario para interactuar con ciertas partes de la API de Java que solo aceptan objetos, como las colecciones.Este proceso es automático en Java gracias a dos mecanismos llamados Autoboxing y Unboxing. El autoboxing ocurre cuando el compilador convierte automáticamente un tipo primitivo en su clase wrapper correspondiente (por ejemplo, al asignar un int a una variable Integer), mientras que el unboxing es el proceso inverso. Esto facilita la escritura de código, ya que permite mezclar primitivos y objetos envoltorios sin necesidad de realizar conversiones manuales constantes.Las ventajas de usar estas clases son variadas. Primero, permiten que los datos primitivos se utilicen en estructuras de datos genéricas, como un ArrayList. Segundo, las clases wrapper proporcionan métodos de utilidad estáticos muy útiles, como Integer.parseInt() para convertir texto a números. Tercero, permiten el uso de valores null, lo que puede ser útil en bases de datos para representar la ausencia de un valor, algo que un tipo primitivo (que siempre tiene un valor por defecto como $0$) no puede hacer.No todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers. En lenguajes de "objetos puros" como Smalltalk o Ruby, todo es un objeto desde el principio, incluidos los números. Otros lenguajes, como Python, tratan los números como objetos de forma nativa. Java y C++ mantienen los tipos primitivos por una cuestión de rendimiento, ya que procesar un dato primitivo es mucho más rápido y consume menos memoria que gestionar un objeto completo en el montón (heap).


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un tipo de dato enumerado (Enum) es un tipo de dato especial que permite definir una variable que solo puede tomar un conjunto de valores constantes y predefinidos. Se utiliza para representar categorías fijas que no cambian durante la ejecución del programa, como los días de la semana, los puntos cardinales o los estados de un pedido, proporcionando una forma mucho más legible y segura de manejar estas opciones que el uso de simples números o cadenas de texto.

En el caso específico de Java, un tipo de dato enumerado es una clase. Aunque se utiliza la palabra reservada enum para definirlos, internamente el compilador los trata como clases especiales que heredan de la clase base java.lang.Enum. Esto significa que, a diferencia de otros lenguajes donde los enumerados son simples listas de enteros, en Java pueden tener sus propios atributos, constructores y métodos, lo que les otorga una potencia y versatilidad extraordinarias.

En términos de encapsulación, los enumerados en Java ofrecen ventajas críticas al garantizar la integridad del sistema. Al ser tratados como clases, permiten ocultar datos internos y exponer solo la funcionalidad necesaria, asegurando que nadie pueda crear nuevas instancias del enumerado fuera de las constantes definidas originalmente. Esto evita que el programa reciba valores inesperados o "mágicos", ya que el compilador verifica en tiempo de desarrollo que solo se utilicen las opciones permitidas, centralizando toda la lógica relacionada con esas constantes en un solo lugar.

Además, los enumerados facilitan el mantenimiento del código al permitir añadir lógica de negocio directamente dentro de cada constante. Por ejemplo, un enumerado que represente tipos de moneda puede encapsular internamente su símbolo y su tasa de cambio, ofreciendo métodos públicos para realizar conversiones sin exponer cómo se realizan los cálculos o dónde se guardan los valores. Esto mantiene el principio de ocultación de información y reduce drásticamente la posibilidad de errores por manipulación indebida de datos globales.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

public enum Mes {
    ENERO(31, 1), 
    FEBRERO(28, 2), 
    MARZO(31, 3), 
    ABRIL(30, 4), 
    MAYO(31, 5), 
    JUNIO(30, 6), 
    JULIO(31, 7), 
    AGOSTO(31, 8), 
    SEPTIEMBRE(30, 9), 
    OCTUBRE(31, 10), 
    NOVIEMBRE(30, 11), 
    DICIEMBRE(31, 12);

    private final int dias;
    private final int numero;

    // El constructor de un enum es siempre privado por defecto
    Mes(int dias, int numero) {
        this.dias = dias;
        this.numero = numero;
    }

    public int getDias() {
        return dias;
    }

    public int getNumero() {
        return numero;
    }
}


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

Para completar la clase Mes, añadiremos los métodos solicitados utilizando una lógica que cruce el número del mes con el valor del booleano para el hemisferio.

Consideraremos los meses "clásicos" de cada estación para simplificar la lógica (por ejemplo, Primavera en el Norte: marzo, abril, mayo, junio):

Java
public boolean esDePrimavera(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return (numero >= 3 && numero <= 6);
    } else {
        return (numero >= 9 && numero <= 12);
    }
}

public boolean esDeVerano(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return (numero >= 6 && numero <= 9);
    } else {
        return (numero == 12 || numero <= 3);
    }
}

public boolean esDeOtoño(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return (numero >= 9 && numero <= 12);
    } else {
        return (numero >= 3 && numero <= 6);
    }
}

public boolean esDeInvierno(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return (numero == 12 || numero <= 3);
    } else {
        return (numero >= 6 && numero <= 9);
    }
}
Esta implementación aprovecha el atributo numero que ya habíamos definido en el constructor del enumerado. Al ser métodos dentro del propio enum, tienen acceso directo a dicho atributo privado para realizar la comparación.
