<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En el lenguaje C, al no disponer de un mecanismo de excepciones, el control de errores se basa en la comunicación explícita entre la función y quien la invoca a través de los valores de retorno o de variables globales de estado.

Aquí tienes las dos opciones de diseño más habituales:

Opción 1: Uso de un valor de retorno especial (Sentinela)
Esta técnica consiste en devolver un valor que sea legal para el tipo de dato (en este caso float) pero que sea lógicamente imposible como resultado de la operación. Para una raíz cuadrada, cualquier número negativo sirve como indicador de error.

C
#include <stdio.h>
#include <math.h>

float raiz(float n) {
    if (n < 0) {
        return -1.0; // Valor sentinela para indicar error
    }
    return sqrt(n);
}

int main() {
    float resultado = raiz(-5.0);
    if (resultado < 0) {
        printf("Error: No se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("El resultado es: %f\n", resultado);
    }
    return 0;
}
Opción 2: Paso de un parámetro por referencia para el estado
En este diseño, la función devuelve el resultado numérico normalmente, pero acepta un puntero a una variable (frecuentemente llamada status o err) que modifica para indicar si la operación tuvo éxito o no.

C
#include <stdio.h>
#include <math.h>

float raiz(float n, int *error) {
    if (n < 0) {
        *error = 1; // Indicamos que ha ocurrido un error
        return 0;
    }
    *error = 0; // Operacion exitosa
    return sqrt(n);
}

int main() {
    int hubo_error;
    float resultado = raiz(-5.0, &hubo_error);
    
    if (hubo_error) {
        printf("Error detectado desde fuera de la funcion.\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }
    return 0;
}


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un evento inusual o inesperado que ocurre durante la ejecución de un programa e interrumpe el flujo normal de sus instrucciones. No representa necesariamente un error de programación (como un bug), sino situaciones excepcionales que el sistema debe gestionar, como un archivo que no existe, una conexión de red perdida o un cálculo matemático imposible.

El objetivo principal de un programador al usarlas es separar la lógica principal del programa del manejo de errores. Al implementar una función, el programador lanza una excepción para delegar la responsabilidad de decidir qué hacer ante un problema a quien tenga más contexto para resolverlo. Al llamar a funciones, el objetivo es capturar posibles fallos de forma controlada, evitando que el programa se detenga bruscamente y permitiendo una recuperación elegante o un cierre seguro de los recursos.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

Para implementar este ejemplo en Java, utilizaremos una excepción de la biblioteca estándar, IllegalArgumentException, que es la forma idónea de indicar que un método ha recibido un argumento no válido.

A diferencia de C, no necesitamos valores sentinela ni punteros; simplemente "lanzamos" el error y el flujo del programa salta directamente al bloque de control externo.

Java
public class Calculadora {

    public double raiz(double n) {
        if (n < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo: " + n);
        }
        return Math.sqrt(n);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();

        try {
            double resultado = calc.raiz(-5.0);
            System.out.println("El resultado es: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error detectado en el main: " + e.getMessage());
        }
        
        System.out.println("El programa continúa su ejecución normal...");
    }
}
En este diseño, si la condición n < 0 se cumple, la ejecución del método raiz se detiene inmediatamente y Java busca el bloque catch más cercano en la pila de llamadas. Esto permite que el método main decida cómo informar al usuario o cómo recuperarse del error sin que la lógica del cálculo esté mezclada con la lógica de impresión de mensajes.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

"Lanzar" (throw) una excepción es el acto de detectar un error y crear un objeto que lo describa para enviarlo fuera de la función. En el ejemplo de la raiz, cuando el código ejecuta throw new IllegalArgumentException(...), está notificando formalmente que el flujo normal no puede continuar porque los datos son inválidos.

"Controlar" o "capturar" (catch) es la acción de recibir ese objeto de error y ejecutar un bloque de código específico para gestionarlo. Es el momento en el que el programador decide qué hacer ante el fallo (mostrar un mensaje, reintentar la operación o cerrar archivos), evitando que la aplicación se cierre bruscamente.

"Propagar" una excepción es lo que ocurre cuando una función donde se lanza el error no tiene un bloque catch. La excepción "viaja" hacia arriba en la pila de llamadas (call stack), buscando a la función que llamó a la actual para ver si ella sabe qué hacer. Si la función B llamó a la función A, y en A se lanza una excepción no capturada, esta salta a la función B.

En cuanto a lo que le ocurre a las funciones en la pila de llamadas:

Interrupción inmediata: En el momento en que se lanza o propaga una excepción por una función, esta se interrumpe por completo. Ninguna línea de código que esté debajo del error llegará a ejecutarse jamás dentro de esa llamada.

Finalización abrupta: Las funciones que no controlan la excepción no se reanudan. Una vez que la excepción sale de la función hacia su "padre" en la pila, esa ejecución de la función muere y se limpia de la memoria. Si la excepción llega hasta el método main y nadie la captura allí, el programa finaliza con un error.

En nuestro ejemplo de la Calculadora, si el método raiz lanza la excepción, el control sale de raiz instantáneamente. Si en el main no tuviéramos el try-catch, la excepción se propagaría fuera del main y la máquina virtual de Java detendría el programa. Como sí tenemos el catch, el main captura la excepción, ejecuta el mensaje de error y, en este caso, el main sí continúa con las líneas que estén después del bloque catch.



## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La "propagación natural" de las excepciones ofrece varias ventajas estructurales y de limpieza de código frente al modelo de control de errores manual de C:

En primer lugar, elimina la contaminación del flujo principal. En C, después de cada llamada a una función que pueda fallar, es obligatorio insertar un bloque if para comprobar el valor de retorno o el estado del error. Esto oscurece la lógica de negocio con constantes interrupciones. En Java, el programador escribe el "camino feliz" del código de forma continua, sabiendo que si algo falla, el sistema saltará automáticamente al gestor de errores adecuado sin necesidad de comprobaciones manuales en cada línea.

En segundo lugar, permite una gestión de errores centralizada. En C, si una función a diez niveles de profundidad en la pila de llamadas detecta un error, cada una de las nueve funciones intermedias debe recibir el error, procesarlo y volverlo a retornar hacia arriba para que llegue al origen. Con la propagación natural, la excepción "atraviesa" todas las capas intermedias automáticamente hasta encontrar un bloque catch. Esto evita que las funciones intermedias tengan que incluir lógica de control de errores que no les corresponde.

Finalmente, garantiza la imposibilidad de ignorar un error por descuido. En C, es muy fácil olvidar comprobar el valor de retorno de una función (como un printf o un malloc), lo que puede llevar a que el programa continúe ejecutándose con datos corruptos o en un estado inestable. En un sistema de excepciones, si un error ocurre y nadie lo captura, el programa se detiene de forma segura. Esto fuerza al programador a ser consciente de los fallos potenciales, mejorando drásticamente la robustez del software.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Sí, en los lenguajes de programación orientada a objetos modernos (como Java, C#, Python o C++), las excepciones son objetos. Esto significa que cuando ocurre un error, se instancia una clase específica que representa dicho fallo y se lanza a través de la pila de llamadas.

Esta naturaleza de objeto aporta ventajas significativas en términos de encapsulación:

Agrupación de información: Una excepción no es solo un código de error numérico; como objeto, puede contener atributos que expliquen el contexto del fallo (la hora, el usuario que lo provocó, el recurso que falló o un mensaje detallado). Toda esta información viaja empaquetada y protegida dentro del objeto.

Jerarquía y herencia: Al ser clases, las excepciones se organizan en jerarquías. Esto permite capturar errores de forma selectiva. Puedes capturar una excepción muy específica (como FileNotFoundException) o una más genérica (IOException) que agrupe a varias relacionadas, manteniendo el código de control organizado.

Comportamiento especializado: Las excepciones pueden tener métodos. Por ejemplo, una excepción de red podría tener un método getRetryAfter() que indique cuánto tiempo esperar antes de reintentar, ocultando la lógica compleja de cálculo dentro del propio objeto de error.

Debido a que son clases normales, podemos crear nuestras propias excepciones personalizadas de forma muy sencilla. Basta con crear una nueva clase que herede de una clase de excepción existente (en Java, generalmente de Exception o RuntimeException). Esto es muy recomendable para representar errores específicos de la lógica de tu negocio (como SaldoInsuficienteException o UsuarioNoEncontradoException), lo que hace que el código sea mucho más legible y fácil de mantener que si usaras excepciones genéricas del sistema.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Gemini ha dicho
Cuando una excepción se propaga en un lenguaje como Java, el objeto que llega al manejador (catch) es mucho más rico en información que un simple código de error de C. Esta información es crucial para que el programador pueda diagnosticar qué falló exactamente sin tener que adivinar el estado del programa en el momento del error.

La información esencial que porta cualquier objeto excepción incluye:

El tipo específico de error (Clase): Al ser un objeto, su propia clase ya indica la naturaleza del problema (por ejemplo, no es lo mismo recibir un NullPointerException que un IOException). Esto permite filtrar el error en bloques catch específicos.

El Mensaje de Detalle (String): Todos los objetos excepción permiten almacenar una cadena de texto personalizada. Aquí se suelen incluir datos dinámicos, como el nombre del archivo que no se pudo abrir o el valor numérico que causó el desbordamiento, lo cual es vital para la depuración.

La Traza de la Pila (Stack Trace): Es, quizás, la pieza más valiosa. Es un historial completo de todas las funciones que estaban activas en el momento del error, indicando el archivo y la línea exacta de código donde se originó el problema y el camino que siguió la excepción hasta ser capturada.

La Causa Original (Cause): En sistemas complejos, una excepción puede envolver a otra (encadenamiento). El objeto excepción puede guardar una referencia a la "excepción raíz" que provocó el fallo inicial, permitiendo rastrear errores a través de diferentes capas de la aplicación.

Frente al modelo de C, donde a menudo solo tienes un -1 o un NULL, tener un objeto con toda esta "caja negra" de datos permite que el manejador tome decisiones mucho más inteligentes y genere informes de error precisos.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?


Sí, en Java se pueden tener múltiples bloques catch asociados a un mismo bloque try. Esto permite manejar diferentes tipos de errores de manera específica según su naturaleza.

En cuanto a la ejecución, solo se ejecuta un bloque catch (como máximo). Cuando se lanza una excepción dentro del try, Java recorre los bloques catch de arriba hacia abajo y selecciona el primero que sea compatible con el tipo de excepción lanzada. Una vez que ese bloque termina su ejecución, el programa salta el resto de los bloques catch y continúa con el flujo normal después de la estructura completa.

Es fundamental tener en cuenta el orden de estos bloques: se deben colocar primero las excepciones más específicas y al final las más genéricas. Si pusieras una clase muy general (como Exception) en el primer catch, esta capturaría absolutamente todo, haciendo que los bloques inferiores nunca lleguen a ejecutarse (lo que el compilador de Java detectará como un error de "código inalcanzable").


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Para garantizar que un fragmento de código se ejecute siempre, independientemente de si se lanza una excepción o no, Java proporciona el bloque finally.

Este bloque es fundamental para la liberación de recursos (cerrar archivos, conexiones a bases de datos o sockets) porque el sistema garantiza su ejecución incluso si ocurre una ruptura en el flujo normal. Si se lanza una excepción, el código en finally se ejecuta antes de que la excepción continúe propagándose hacia arriba en la pila de llamadas.

1. Ejemplo con catch (Manejo y limpieza)
Es el escenario más común: intentamos algo, capturamos el error si ocurre y, pase lo que pase, limpiamos.

Java
public void leerArchivo(String ruta) {
    try {
        System.out.println("Abriendo archivo...");
        // Lógica que podría fallar
        if (ruta == null) throw new Exception("Ruta no válida");
    } catch (Exception e) {
        System.out.println("Error capturado: " + e.getMessage());
    } finally {
        System.out.println("Cerrando recursos (esto se ejecuta SIEMPRE).");
    }
}
2. Ejemplo sin catch (Solo limpieza y propagación)
A veces no queremos capturar el error porque no sabemos cómo arreglarlo, pero sí tenemos la responsabilidad de cerrar el recurso antes de que la excepción se propague al llamador.

Java
public void procesoCritico() throws Exception {
    try {
        System.out.println("Iniciando operación crítica...");
        throw new Exception("Fallo catastrófico"); // Se lanza y nadie lo detiene aquí
    } finally {
        System.out.println("Liberando memoria antes de que el error suba al main.");
    }
}
Puntos clave del finally:
Si hay un return dentro del try, el bloque finally se ejecuta antes de que el método devuelva el valor.

Si la excepción no se captura, el finally se ejecuta y, justo después, la excepción sigue su camino hacia la siguiente función en la pila.

Es la única forma segura de evitar "fugas de recursos" (resource leaks).


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, el bloque finally es extremadamente flexible y tiene un comportamiento muy estricto garantizado por la Máquina Virtual de Java (JVM).

1. ¿Puede ir un finally sin un catch?
Sí. La estructura try-finally es perfectamente válida. Se utiliza cuando un método no sabe (o no debe) gestionar el error, pero tiene la responsabilidad de liberar recursos antes de que la excepción se propague al nivel superior.

Java
public void procesar() throws Exception {
    try {
        // Código que puede fallar
    } finally {
        // Se ejecuta SIEMPRE antes de que la excepción salga de este método
        System.out.println("Limpieza realizada.");
    }
}
2. ¿Se ejecuta siempre, ocurra o no una excepción?
Sí. El bloque finally es el "seguro de vida" del programador. Se ejecuta en estos tres escenarios:

Si el código del try termina con éxito.

Si el código del try lanza una excepción que es capturada por un catch.

Si el código del try lanza una excepción que NO es capturada (se propaga).

Nota: Solo hay casos extremos donde no se ejecutaría, como si el sistema operativo mata el proceso, si ocurre un StackOverflowError fatal o si llamas a System.exit(0).

3. ¿Qué ocurre si hay un return en medio del try?
Este es uno de los comportamientos más curiosos de Java: el finally se ejecuta incluso si hay un return.

Cuando el programa encuentra un return dentro de un bloque try, Java hace lo siguiente:

Evalúa el valor que se va a devolver y lo reserva.

Interrumpe el retorno para ejecutar el bloque finally.

Una vez terminado el finally, el método efectivamente devuelve el valor.

Ejemplo ilustrativo:

Java
public int pruebaReturn() {
    try {
        return 10;
    } finally {
        System.out.println("Ejecutando finally tras el return...");
    }
}
Al llamar a este método, verás el mensaje en consola y luego recibirás el número 10. El finally siempre tiene la "última palabra" antes de que el control vuelva al llamador.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Las excepciones controladas (checked) son aquellas que el compilador te obliga a gestionar. Si un método lanza una de estas excepciones, cualquier código que lo llame debe envolver la instrucción en un bloque try-catch o declarar que también la lanza mediante la cláusula throws. Se utilizan para situaciones que son ajenas al control directo del programador, como fallos en la red o archivos que no se encuentran, donde el sistema debe estar preparado para recuperarse.

Las excepciones no controladas (unchecked) heredan de RuntimeException y el compilador no exige su gestión. Representan errores lógicos o de programación que, en teoría, no deberían ocurrir si el código estuviera bien diseñado, como acceder a una posición inexistente de un array o intentar operar con una referencia nula. Su propagación es automática hacia arriba en la pila de llamadas, lo que evita ensuciar el código con declaraciones repetitivas.

El papel de RuntimeException es servir de base para todos esos errores que no requieren una acción correctiva obligatoria por parte del llamador. Permiten que el flujo del programa sea más limpio, delegando la responsabilidad de la estabilidad al programador encargado de validar los datos antes de usarlos. Es la herramienta ideal para señalar que se han violado las precondiciones de un método de forma interna.

A la hora de diseñar, se prefiere una excepción controlada cuando el error es una alternativa previsible del flujo (como una contraseña incorrecta) y se desea forzar al usuario de la clase a manejar ese escenario. Por el contrario, se opta por una no controlada cuando el error indica un mal uso de la API o un estado inválido del objeto que hace que la continuación del programa no tenga sentido sin una corrección del código fuente.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La palabra reservada throws se utiliza en la firma de un método para declarar que dicho método puede lanzar una o varias excepciones específicas durante su ejecución. Su función principal es informar a cualquier otra parte del programa que decida llamar a ese método sobre los riesgos potenciales que asume, trasladando la responsabilidad de la gestión del error hacia arriba en la pila de llamadas. Es una cláusula de "advertencia" que forma parte del contrato público de la función.

Se utiliza como alternativa a capturar una excepción cuando el método actual no tiene la información suficiente o la capacidad lógica para resolver el problema de forma adecuada. En lugar de atrapar el error con un bloque catch y tratar de silenciarlo o arreglarlo localmente, el programador decide que es mejor dejar que la excepción se propague hacia quien invocó la función. De esta manera, el "padre" en la pila de llamadas, que quizás tenga más contexto sobre la interfaz de usuario o el estado global, podrá decidir cómo reaccionar.

En el caso de las excepciones controladas (checked), Java obliga por contrato a que el error sea gestionado de alguna forma para que el código compile. Declarar throws cumple con este requisito legal del lenguaje sin necesidad de escribir lógica de tratamiento de errores en ese punto exacto. Es una herramienta de diseño que permite mantener los métodos intermedios limpios de bloques try-catch que no aportan valor, centralizando la gestión del error en capas superiores de la aplicación.

Por tanto, usar throws permite que una excepción viaje de forma natural hasta encontrar un manejador competente. Si un método que lee un archivo declara throws IOException, no necesita preocuparse por qué hacer si el disco falla; simplemente avisa de que eso puede ocurrir. Quien llame a ese método de lectura será el encargado de decidir si intenta leer otro archivo, muestra un aviso al usuario o cierra la aplicación.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

Para cumplir con este diseño, el método debe declarar FileNotFoundException en su firma. Esto avisa a quien llame a la función de que el error puede ocurrir, permitiendo que la excepción "salte" este método y siga subiendo por la pila de llamadas si el archivo no se encuentra en el sistema.

El uso de finally en este escenario es crítico para la limpieza. Aunque el método no sepa cómo arreglar el error de un archivo inexistente, tiene la responsabilidad de cerrar cualquier otro recurso que haya abierto (como un flujo de datos) antes de que la excepción lo abandone. Java garantiza que el bloque finally se ejecute justo antes de que la excepción se propague al método llamador.

Java
import java.io.*;

public void procesarArchivo(String ruta) throws FileNotFoundException {
    FileReader fr = null;
    try {
        // Intentamos abrir el recurso
        fr = new FileReader(ruta);
        System.out.println("Archivo abierto correctamente.");
        // Lógica de lectura...
    } finally {
        // No hay catch: el error sube, pero antes limpiamos
        System.out.println("Cerrando recursos antes de propagar el posible error...");
        if (fr != null) {
            try {
                fr.close();
            } catch (IOException e) {
                // Error silencioso al cerrar
            }
        }
    }
}
Este patrón asegura que el programa no deje "cabos sueltos" (fugas de memoria o archivos bloqueados). Al no incluir un bloque catch para FileNotFoundException, delegamos la decisión de qué hacer (mostrar un aviso al usuario o usar un archivo por defecto) a la función que invocó a procesarArchivo.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?


Sí, es perfectamente posible incluir excepciones no controladas como RuntimeException o sus hijas (por ejemplo, NullPointerException) en la cláusula throws. Aunque el compilador no te obliga a hacerlo, Java permite esta sintaxis para que el programador pueda documentar de forma explícita qué errores de lógica o de estado podrían surgir al invocar ese método.

En este caso, el método llamador no está obligado a poner un bloque try-catch. Al ser una excepción de tipo unchecked, el compilador ignorará la presencia del throws a efectos de validación, permitiendo que el código compile aunque no se gestione el error. Si ocurre la excepción y no hay un catch, esta se propagará automáticamente hacia arriba por la pila de llamadas hasta detener el programa o encontrar un manejador genérico.

El sentido principal de incluir una RuntimeException en el throws es la documentación y claridad de la API. Al declararla, estás comunicando a otros desarrolladores que, bajo ciertas condiciones (como pasar un argumento inválido), el método fallará de esa forma específica. Esto sirve como una advertencia para que el llamador decida si prefiere validar los datos antes de la llamada o si, por el contrario, quiere capturar la excepción para mostrar un mensaje de error controlado en la interfaz.

En resumen, poner una excepción no controlada en el throws es una invitación a la cautela, no una imposición técnica. Ayuda a que las herramientas de autocompletado de los IDEs y la documentación (Javadoc) muestren ese riesgo, facilitando que el programador que usa tu código entienda mejor el comportamiento del método sin necesidad de leer toda su implementación interna.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomienda usar excepciones controladas (IOException) cuando el error es una contingencia razonable del mundo real, ajena al control del programador, de la cual el sistema debe intentar recuperarse. Si un archivo no existe o la red se cae, el programa no tiene por qué morir; el compilador te obliga a prever ese escenario. En cambio, las no controladas (IllegalArgumentException) se usan para errores de lógica o violaciones del contrato del método. Indican que el programador cometió un fallo (como pasar un null donde no debía) y, por lo general, la solución no es capturar el error, sino corregir el código.

No, esta distinción no existe en todos los lenguajes. De hecho, Java es el único lenguaje de uso masivo que implementa excepciones controladas de forma estricta. Lenguajes modernos y potentes como C#, Python, C++, Kotlin o Swift no obligan al programador a capturar excepciones mediante el sistema de tipos. En estos lenguajes, todas las excepciones se comportan, a efectos prácticos, como las "no controladas" de Java: se pueden lanzar en cualquier momento sin necesidad de declararlas en la firma del método.

En los lenguajes donde solo existe una opción, la más habitual es el modelo de excepciones no controladas (unchecked). La corriente principal del diseño de software ha concluido que obligar al programador a gestionar cada posible error pequeño (como hace Java con las checked) a menudo ensucia el código con bloques try-catch vacíos o delegaciones sistemáticas de throws. Al dejar las excepciones como no controladas, se permite que el error fluya libremente hasta el nivel que realmente tenga la capacidad de gestionarlo, simplificando la estructura de las funciones intermedias.

Este enfoque de "excepción única" (estilo RuntimeException) fomenta un código más limpio y legible, aunque traslada la responsabilidad de la robustez totalmente al desarrollador, quien debe consultar la documentación para saber qué errores podrían surgir. En entornos como Kotlin, que corre sobre la misma máquina virtual que Java, se decidió eliminar las excepciones controladas por completo para ganar agilidad, demostrando que la tendencia actual huye de la rigidez del chequeo en tiempo de compilación


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene mucho sentido y es una práctica común en el desarrollo profesional. Lanzar una excepción dentro de un bloque catch permite transformar un error técnico en uno más comprensible o asegurar que, aunque se realice una acción correctiva intermedia, el sistema siga sabiendo que algo falló.

Lanzar una nueva excepción (Encadenamiento)
Se usa cuando capturas una excepción de bajo nivel (como un error de base de datos) y quieres lanzar una que tenga sentido para la lógica de tu negocio (como un error de registro de usuario). Lo ideal es pasar la excepción original como "causa" para no perder la traza del error inicial.

Java
try {
    // Código de bajo nivel (ej. abrir un socket)
} catch (IOException e) {
    // Lanzamos una nueva excepción más descriptiva
    throw new ServiceUnavailableException("El servidor de pagos no responde", e);
}
Relanzar la misma excepción
Es posible relanzar exactamente el mismo objeto que acabas de capturar usando throw e;. Esto se hace cuando el método necesita realizar una tarea obligatoria tras el error (como escribir en un log o cerrar una transacción manualmente), pero no tiene la capacidad de "arreglar" el problema. Al relanzarla, permites que la excepción siga su camino hacia arriba en la pila de llamadas.

Java
try {
    documento.procesar();
} catch (Exception e) {
    System.out.println("Error procesando el documento: " + e.getMessage());
    // Relanzamos para que el llamador sepa que la operación falló
    throw e; 
}


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

El concepto de "causa" se refiere al encadenamiento de excepciones. Consiste en envolver una excepción original (el motivo técnico real) dentro de una nueva excepción que tiene más sentido para la lógica de negocio. Esto permite que la capa superior reciba una alerta coherente con su función, pero sin perder la información técnica que originó el fallo.

Para implementar esto en Java, la mayoría de las clases de excepción tienen un constructor que acepta un objeto de tipo Throwable llamado cause. Al pasar la excepción capturada a este constructor, queda vinculada permanentemente a la nueva.

Java
// Definimos nuestra propia excepción de alto nivel
class ErrorDeNegocioException extends Exception {
    public ErrorDeNegocioException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public void realizarOperacion() throws ErrorDeNegocioException {
    try {
        // Simulamos un error de bajo nivel (acceso a un archivo inexistente)
        new java.io.FileReader("config.txt");
    } catch (java.io.FileNotFoundException e) {
        // Encapsulamos el error técnico en uno de negocio
        throw new ErrorDeNegocioException("No se pudo cargar la configuración de la app", e);
    }
}
Cuando una excepción con causa sale por pantalla (a través de printStackTrace()), sí se ve. El volcado de la pila mostrará primero la excepción de alto nivel y, justo debajo, aparecerá una sección encabezada por "Caused by:" seguida de la traza de la excepción original. Esto es fundamental para el programador, ya que permite ver la jerarquía completa del error: desde el problema general ("No cargó la configuración") hasta el detalle exacto ("Archivo config.txt no encontrado en línea 15").
