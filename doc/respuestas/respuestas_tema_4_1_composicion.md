<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En C, la composición se basa en anidar estructuras para crear modelos de datos más complejos. En este caso, la estructura Linea no "es" un punto, sino que tiene dos puntos como componentes fundamentales.Para calcular la distancia, utilizaremos la fórmula de la hipotenusa: $\sqrt{(x_2-x_1)^2 + (y_2-y_1)^2}$.C#include <stdio.h>
#include <math.h>

// Estructura básica: El Punto
struct Punto {
    double x;
    double y;
};

// Composición: La Línea "tiene" dos puntos
struct Linea {
    struct Punto inicio;
    struct Punto fin;
};

// Función para calcular distancia entre dos puntos genéricos
double calcularDistancia(struct Punto p1, struct Punto p2) {
    return sqrt(pow(p2.x - p1.x, 2) + pow(p2.y - p1.y, 2));
}

// Función para hallar la longitud de una línea usando la anterior
double longitudLinea(struct Linea l) {
    return calcularDistancia(l.inicio, l.fin);
}

int main() {
    struct Punto pA = {0.0, 0.0};
    struct Punto pB = {3.0, 4.0};
    struct Linea miLinea = {pA, pB};

    printf("La longitud de la linea es: %.2f\n", longitudLinea(miLinea));
    return 0;
}
La composición permite que longitudLinea delegue el cálculo matemático pesado a calcularDistancia. De esta forma, si mañana cambias cómo se calcula la distancia entre puntos, la lógica de la línea se actualizará automáticamente sin modificar su propio código.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

En Java, la composición se potencia gracias a los modificadores de acceso. Para garantizar la inmutabilidad y superar el modelo de C, declaramos los atributos como private y final. Esto asegura que, una vez asignados los valores en el constructor, no puedan ser alterados, protegiendo la integridad del objeto.

La clase Punto encapsula sus coordenadas y la lógica para medir distancias. Al usar el modificador final en los atributos x e y, garantizamos que un punto siempre represente la misma posición en el espacio, eliminando efectos secundarios inesperados.

Java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}
La clase Linea ejemplifica la composición ("tiene-un"). Al recibir dos objetos Punto en su constructor y guardarlos en variables final, la línea se vuelve inmutable: siempre conectará los mismos dos puntos. El método getLongitud simplemente delega el trabajo al método que ya definimos en la clase Punto.

Java
public class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double getLongitud() {
        return inicio.distanciaA(fin);
    }
}
Esta estructura es superior a la de C porque impide que un programador modifique accidentalmente las coordenadas de un punto o los extremos de una línea después de su creación. La ocultación de información asegura que el estado interno del objeto solo sea accesible y gestionable a través de los métodos que nosotros definamos, manteniendo la consistencia de los datos en todo momento.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad en la composición define cuántas instancias de una clase pueden estar asociadas con una instancia de la otra clase. Es una regla de negocio que establece los límites mínimos y máximos de los objetos que participan en la relación, permitiendo entender la estructura del sistema de un vistazo.

En el ejemplo de la Linea y el Punto, la multiplicidad se define de la siguiente manera:

De Linea a Punto (1 a 2): Una instancia de Linea está compuesta exactamente por 2 objetos Punto (el inicio y el fin). No puede tener ni más ni menos para ser una línea válida en este modelo.

De Punto a Linea (1 a 1): En una composición estricta (donde el "todo" es dueño de las "partes"), un objeto Punto específico pertenece a 1 única Linea. Si la línea desaparece, sus puntos mueren con ella.

Esta relación suele representarse en diagramas como 1 → 2, indicando que por cada unidad del objeto contenedor, existen dos del contenido. Al ser inmutables, esta relación queda "sellada" desde el momento de la instanciación en el constructor.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La composición fuerte describe una relación de dependencia total donde las partes no tienen sentido sin el "todo". En este escenario, el objeto contenedor es el dueño absoluto de sus componentes, lo que implica que el ciclo de vida de los objetos está ligado: si el objeto principal es destruido, sus partes componentes se destruyen automáticamente con él, ya que no pueden existir de forma independiente.

La composición débil, por el contrario, representa una unión más flexible donde las partes pueden existir antes de la creación del contenedor y sobrevivir a su destrucción. En este caso, el objeto contenedor simplemente mantiene una referencia a otros objetos independientes; por tanto, el ciclo de vida es independiente, permitiendo que un mismo objeto sea compartido por diferentes contenedores a lo largo del tiempo.

En la terminología técnica de la orientación a objetos, solemos llamar "composición" propiamente a la relación fuerte (el "todo" posee a las partes). Por otro lado, reservamos los términos "agregación" o "asociación" para referirnos a la relación débil, donde existe un vínculo o una colección de objetos, pero sin una propiedad destructiva o exclusiva sobre ellos.

En nuestro ejemplo, si al borrar una Linea sus objetos Punto desaparecen de la memoria, estaríamos ante una composición (fuerte). Si los puntos fueran creados fuera y la línea solo los "tomara prestados" para realizar un cálculo, sobreviviendo estos a la eliminación de la línea, estaríamos ante una agregación (débil).


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase utiliza a otra únicamente de forma temporal —ya sea porque recibe el objeto como parámetro, lo devuelve como resultado de un método, lo instancia como variable local o invoca un método estático de dicha clase— hablamos de dependencia o uso. A diferencia de la composición, aquí no existe una relación de propiedad o pertenencia estructural; la clase no "tiene" a la otra como un atributo permanente, sino que simplemente "la necesita" para realizar una tarea puntual.

En términos de diseño, la dependencia es la relación más débil que puede existir entre dos clases. Mientras que la composición define la estructura y el estado de un objeto (lo que el objeto es o contiene), la dependencia define qué otros objetos necesita para funcionar. Si la clase dependiente desaparece, la clase de la que depende no se ve afectada en absoluto, ya que el vínculo solo existe durante la ejecución de un método específico.

Un ejemplo claro sería un método Impresora.imprimir(Documento doc). La clase Impresora no está compuesta por un Documento (no es dueña de él ni lo guarda en su estado interno), simplemente tiene una dependencia hacia él para poder ejecutar la acción de imprimir. Una vez que el método termina, la relación se desvanece.

Es importante distinguir ambas porque reducir las dependencias innecesarias mejora la modularidad del código. Sin embargo, la dependencia es inevitable en cualquier sistema funcional, ya que los objetos deben colaborar entre sí para resolver problemas complejos, aunque no formen parte de la misma estructura física o lógica.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

Para implementar estas dos variantes en Java, la clave reside en quién crea los objetos y cómo se almacenan las referencias dentro de la clase contenedora.

1. Composición Fuerte (Composición)
En la composición fuerte, la Linea es dueña absoluta de sus puntos. Los crea internamente (o crea copias defensivas) para asegurar que nadie más tenga acceso a ellos. Si la Linea desaparece, sus puntos son candidatos para la recolección de basura porque no existen fuera de ella.

Java
public class LineaFuerte {
    private final Punto inicio;
    private final Punto fin;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        // La propia línea crea sus puntos. Nacen con ella.
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }
}
2. Composición Débil (Agregación)
En la composición débil, la Linea recibe puntos que ya han sido creados fuera. Ella solo guarda una referencia. Si la Linea se destruye, los objetos Punto originales siguen existiendo en el código que los creó, ya que su ciclo de vida es independiente.

Java
public class LineaDebil {
    private final Punto inicio;
    private final Punto fin;

    public LineaDebil(Punto p1, Punto p2) {
        // Recibe puntos externos. Si la línea muere, p1 y p2 siguen vivos fuera.
        this.inicio = p1;
        this.fin = p2;
    }
}
La diferencia fundamental es que en la fuerte, la Linea controla el nacimiento de los puntos, mientras que en la débil, los puntos son "invitados" que pueden pertenecer a varias líneas a la vez o existir por sí solos. En Java, aunque el Garbage Collector maneja la memoria, la relación lógica de propiedad define cuál de los dos patrones estamos usando.

Por último, en la composición fuerte solemos usar tipos primitivos en el constructor para fabricar las partes, mientras que en la débil pasamos los objetos ya instanciados.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, no ves una destrucción explícita porque el lenguaje utiliza un Recolector de Basura (Garbage Collector o GC). A diferencia de lenguajes como C++, donde tendrías que usar un delete manual en el destructor de la clase, Java gestiona la memoria de forma automática basándose en la alcanzabilidad de los objetos.

En la composición fuerte, la única referencia que existe hacia esos objetos Punto está dentro del objeto Linea. Cuando el objeto Linea deja de ser accesible (por ejemplo, porque la variable que lo contenía sale de alcance o se pone a null), sus atributos internos (inicio y fin) también pierden su único camino de acceso. El GC detecta que esos puntos han quedado "huérfanos" y libera su memoria de forma automática.

Por tanto, la "destrucción" en la composición fuerte de Java es implícita y lógica:

Al ser la Linea la única que conoce a esos Punto, la vida de estos queda atada a la de ella.

Si la Linea es marcada para eliminación, sus componentes mueren con ella simplemente porque nadie más tiene su "dirección" de memoria.

Esta es la razón por la que en Java nos enfocamos más en quién crea el objeto y quién guarda la referencia que en quién lo destruye. En la composición fuerte, al haber creado los puntos dentro del constructor de Linea y no haberlos devuelto nunca al exterior, garantizamos que su ciclo de vida sea exactamente el mismo que el del contenedor.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

Este ejemplo de agregación (composición débil) muestra cómo un Departamento gestiona referencias a objetos Profesor que existen independientemente. La complejidad reside en mantener la invariante: el director debe ser siempre un profesor que pertenezca a la lista del departamento.

Utilizamos un array primitivo y un contador de control para gestionar la lista internamente, ocultando estos detalles al exterior para no romper la encapsulación.

Java
public class Departamento {
    private String nombre;
    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El departamento debe tener un director desde el inicio.");
        }
        this.nombre = nombre;
        this.profesores = new Profesor[50];
        this.numProfesores = 0;
        
        // El director debe estar en la lista para cumplir la invariante
        this.añadirProfesor(directorInicial);
        this.director = directorInicial;
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("No se puede añadir un profesor nulo.");
        if (numProfesores >= 50) throw new IllegalStateException("Departamento completo (máx 50).");
        
        profesores[numProfesores] = p;
        numProfesores++;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición de profesor inválida.");
        }
        
        // Invariante: No se puede eliminar al profesor que actualmente es director
        if (profesores[posicion] == director) {
            throw new IllegalStateException("No se puede eliminar al profesor porque es el actual director.");
        }

        // Desplazamiento del array para cerrar el hueco
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) throw new IllegalArgumentException("El director no puede ser nulo.");
        
        // Invariante: El nuevo director debe pertenecer ya al departamento
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }
        
        if (!encontrado) {
            throw new IllegalArgumentException("El nuevo director debe ser un profesor del departamento.");
        }
        this.director = nuevoDirector;
    }

    public int getCantidadProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        return profesores[posicion];
    }

    public Profesor getDirector() {
        return director;
    }
}
En este diseño, si el objeto Departamento se destruye, los objetos Profesor permanecen vivos en el sistema (por ejemplo, en una lista general de la Universidad), cumpliendo con la definición de composición débil. La lógica de control asegura que el estado interno sea siempre coherente con las reglas de negocio.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

El uso de List (generalmente ArrayList) simplifica drásticamente la gestión de colecciones en Java, ya que se encarga de la redistribución de memoria y el desplazamiento de elementos de forma automática.

Implementación con List
Java
import java.util.ArrayList;
import java.util.List;
import java.util.Collections;

public class Departamento {
    private String nombre;
    private final List<Profesor> profesores; // Agregación (débil)
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Director requerido.");
        
        this.nombre = nombre;
        this.profesores = new ArrayList<>();
        this.añadirProfesor(directorInicial);
        this.director = directorInicial;
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor nulo.");
        // Ya no comprobamos el límite de 50; ArrayList crece solo
        profesores.add(p);
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        if (profesores.get(posicion) == director) {
            throw new IllegalStateException("No se puede eliminar al director actual.");
        }
        profesores.remove(posicion); // El desplazamiento es automático
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento.");
        }
        this.director = nuevoDirector;
    }

    public int getCantidadProfesores() { return profesores.size(); }

    public Profesor getProfesor(int posicion) { return profesores.get(posicion); }
}
¿Qué nos hemos ahorrado?
Al usar List, eliminamos la gestión manual del almacenamiento:

El contador numProfesores: La lista ya sabe cuántos elementos tiene con size().

El desplazamiento de elementos: En el array, al borrar en la posición 2, debíamos mover manualmente los elementos 3, 4... hacia la izquierda. List.remove(i) lo hace solo.

El límite de capacidad: Ya no necesitamos verificar si llegamos a 50 ni gestionar el desbordamiento.

El peligro de devolver la lista interna
Si creáramos un método getProfesores() que hiciera return this.profesores;, estaríamos rompiendo la encapsulación y permitiendo que se viole la invariante de clase.

El problema: Un programador externo podría llamar a departamento.getProfesores().clear() o remove() desde fuera. Como es la misma referencia a la lista interna, podría borrar al director sin pasar por nuestras validaciones, dejando al departamento en un estado ilegal.

La solución: Vistas no modificables o Copias defensivas
Para resolverlo, debemos devolver una versión de la lista que no permita alteraciones. La forma más elegante es usar Collections.unmodifiableList:

Java
public List<Profesor> getProfesores() {
    // Devuelve una vista que lanza excepción si alguien intenta hacer .add() o .remove()
    return Collections.unmodifiableList(profesores);
}


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

La composición recursiva ocurre cuando una clase tiene un atributo de su propio tipo, permitiendo crear estructuras jerárquicas o en cadena de profundidad arbitraria. Al igual que una excepción puede contener otra excepción como su "causa", un objeto puede contener otro similar para representar relaciones de procedencia o estructura.

Ejemplo de Persona Inmutable
En este diseño, la inmutabilidad es clave. Al ser los atributos final, una vez que se define quién es la madre de una persona, esa relación no puede alterarse, lo que garantiza la integridad histórica del árbol genealógico.

Java
public class Persona {
    private final String nombre;
    private final Persona madre; // Composición recursiva

    // Constructor para la "Eva primordial" (sin madre conocida)
    public Persona(String nombre) {
        this.nombre = nombre;
        this.madre = null;
    }

    // Constructor estándar
    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }

    @Override
    public String toString() {
        return nombre + (madre != null ? " (hijo/a de " + madre.getNombre() + ")" : " (ancestro inicial)");
    }
}
Ejemplo de uso en el Main
Aquí vemos cómo se construye la cadena desde el eslabón más antiguo hacia el más reciente.

Java
public static void main(String[] args) {
    Persona abuela = new Persona("Carmen");
    Persona madre = new Persona("Elena", abuela);
    Persona nieto = new Persona("Lucas", madre);

    System.out.println(nieto);
    System.out.println("La abuela de " + nieto.getNombre() + " es " + 
                       nieto.getMadre().getMadre().getNombre());
}
Otros ejemplos clásicos de composiciones recursivas
Este patrón es fundamental en informática para representar estructuras que pueden crecer indefinidamente:

Sistemas de Archivos: Una Carpeta que contiene una lista de objetos Elemento, donde un Elemento puede ser a su vez otra Carpeta.

Estructuras de Datos: Un Nodo de una lista enlazada que tiene un atributo siguiente, el cual es también un Nodo.

Organigramas: Un Empleado que tiene un atributo jefe, siendo este último también un Empleado.

Interfaces Gráficas: Un Contenedor (como un panel) que puede albergar otros Contenedores dentro de él.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Una relación bidireccional es aquella en la que ambos objetos implicados tienen una referencia al otro. En nuestro caso, el Departamento conoce a sus Profesores, pero ahora cada Profesor también sabrá a qué Departamento pertenece.

Esto es muy útil en la práctica (por ejemplo, para preguntarle a un profesor: "¿En qué departamento trabajas?"), pero introduce un reto crítico: la sincronización. Si mueves a un profesor al Departamento B, debes asegurarte de que el Departamento A lo borre de su lista y el B lo añada, y que el profesor actualice su referencia interna.

Implementación en Java
Para que esto funcione correctamente sin romper la encapsulación, uno de los dos lados (normalmente el "contenedor", el Departamento) debe ser el responsable de mantener la coherencia.

1. Clase Profesor
El profesor ahora tiene un atributo para su departamento, pero ojo: no dejamos que cualquiera lo cambie alegremente (el setter suele ser de acceso limitado o manejado por el departamento).

Java
public class Profesor {
    private String nombre;
    private Departamento departamento; // Referencia inversa

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    // El profesor puede consultar su departamento
    public Departamento getDepartamento() {
        return departamento;
    }

    // Este método lo usará el Departamento para "avisar" al profesor
    // Se suele poner con visibilidad de paquete (sin public) o protected
    void setDepartamento(Departamento depto) {
        this.departamento = depto;
    }
}
2. Clase Departamento (La responsable)
El departamento debe gestionar ambos lados de la relación cada vez que alguien entra o sale.

Java
public void añadirProfesor(Profesor p) {
    if (p == null) throw new IllegalArgumentException();
    
    // 1. Si el profesor ya estaba en otro depto, habría que gestionarlo (opcional según lógica)
    
    // 2. Lo añadimos a nuestra lista
    profesores.add(p);
    
    // 3. ¡Sincronización! Le decimos al profesor que ahora somos su departamento
    p.setDepartamento(this);
}

public void eliminarProfesor(int posicion) {
    // Validaciones de director e índices...
    
    Profesor p = profesores.get(posicion);
    
    // 1. Quitamos la referencia en el profesor
    p.setDepartamento(null);
    
    // 2. Lo borramos de nuestra lista
    profesores.remove(posicion);
}
Consideraciones importantes
Cuidado con la recursividad infinita: Si el método añadirProfesor llama a p.setDepartamento(this) y ese método a su vez llamara de nuevo a depto.añadirProfesor(this), el programa entraría en un bucle infinito y lanzaría un StackOverflowError. Por eso, solo uno de los dos debe llevar la voz cantante.

Serialización: Si intentas convertir estos objetos a JSON o XML, las relaciones bidireccionales suelen causar problemas (el serializador intentará escribir el departamento, que tiene profesores, que tienen el departamento...), por lo que hay que usar anotaciones para ignorar uno de los dos lados.

Complejidad: Solo añade bidireccionalidad si realmente la necesitas. Si siempre vas a acceder a los profesores a través del departamento, es mejor mantener la relación unidireccional para mantener el código simple.
