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

### Respuesta
A falta de excepciones en C, lo habitual es señalizar errores sin imprimir dentro de la función, de modo que sea el código llamador quien informe al usuario. Una primera opción clara consiste en devolver un código de estado (0 = éxito, distinto de 0 = error) y escribir el resultado en un parámetro de salida. Esta aproximación separa datos de control de errores, evita ambigüedades con valores de retorno “sentinela” y funciona igual de bien con cualquier tipo de resultado.

#include <math.h>
#include <stdio.h>

int raiz(double x, double *out) {
    if (x < 0.0) {
        return 1; // error: argumento negativo
    }
    *out = sqrt(x);
    return 0; // éxito
}

int main(void) {
    double x = -9.0, r;
    int st = raiz(x, &r);
    if (st != 0) {
        printf("Error: no se puede calcular la raíz de un número negativo (x=%.2f)\n", x);
    } else {
        printf("raiz(%.2f) = %.4f\n", x, r);
    }
    return 0;
}

Una segunda opción habitual en C es usar errno (de <errno.h>) junto con un valor de retorno que permita detectar el fallo, por ejemplo NAN para funciones de punto flotante. La función establece errno a un código apropiado (p. ej., EDOM para dominio matemático) y el llamador decide cómo comunicar el error al usuario. Es importante limpiar errno antes de la llamada o comprobarlo solo si se detecta la condición de error en el valor devuelto.

#include <math.h>
#include <errno.h>
#include <stdio.h>
#include <fenv.h>

double raiz(double x) {
    if (x < 0.0) {
        errno = EDOM;   // dominio inválido
        return NAN;     // señaliza error en punto flotante
    }
    errno = 0;          // opcional: dejar claro que no hay error
    return sqrt(x);
}

int main(void) {
    double x = -4.0;
    errno = 0;          // limpiar antes de llamar
    double r = raiz(x);
    if (isnan(r) && errno == EDOM) {
        printf("Error: argumento fuera de dominio para la raíz (x=%.2f)\n", x);
    } else {
        printf("raiz(%.2f) = %.4f\n", x, r);
    }
    return 0;
}

Ambas soluciones cumplen con el requisito de informar al usuario desde fuera de raiz. La primera ofrece un contrato claro y general (especialmente útil cuando no hay un “sentinela” inequívoco), mientras que la segunda se integra bien con el estilo de las bibliotecas C para funciones matemáticas y permite encadenar funciones que ya usan errno. En contextos de C “estilo sistema” suele preferirse el código de estado + parámetro de salida por su claridad y portabilidad; errno es cómodo cuando ya se trabaja con operaciones matemáticas o de sistema que lo establecen.

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una excepción es un mecanismo que permite interrumpir el flujo normal de un programa cuando ocurre una situación anómala o inesperada. En lugar de devolver códigos de error como en C, el lenguaje proporciona una forma estructurada de “lanzar” un evento que representa el problema, de modo que otro punto del programa pueda “capturarlo” y decidir cómo manejarlo. 

Esto separa claramente el código que realiza la operación del código encargado de gestionar los errores, lo cual hace que la lógica principal sea más limpia y fácil de leer.

El objetivo al usarlas al implementar funciones es poder notificar fallos de forma clara sin mezclar la lógica de cálculo con comprobaciones repetitivas de error. En vez de devolver valores especiales o estados ambiguos, la función lanza una excepción cuando algo impide completar su tarea correctamente. De este modo, la función sigue centrada en lo que debe hacer, y el tratamiento del error se delega a otro nivel.

Cuando un programador llama a funciones que pueden fallar, las excepciones permiten capturar esos errores y decidir qué acción realizar: informar al usuario, intentar una alternativa, registrar el problema o finalizar el programa de forma controlada. Este enfoque obliga a pensar de forma explícita en las situaciones excepcionales, a diferencia de C, donde ignorar un código de error puede pasar desapercibido y provocar fallos posteriores más difíciles de depurar.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
En Java, lo natural es señalizar el error lanzando una excepción cuando el argumento no cumple el contrato (p. ej., número negativo para una raíz cuadrada real). Así se mantiene la lógica de cálculo limpia dentro de Calculadora.raiz y se delega el tratamiento del problema al código llamador. Para argumentos inválidos se suele usar IllegalArgumentException (excepción no comprobada) cuando el fallo es responsabilidad del llamador, en línea con la idea de “precondición incumplida”.

Desde fuera de la función (en main) se controla el error con un bloque try/catch. De este modo, el llamador decide qué hacer: informar al usuario, registrar el error o proponer una alternativa. La estructura try/catch evita mezclar el flujo normal con comprobaciones de “código de error” tras cada llamada, algo habitual en C, y que en Java se reemplaza por un manejo más declarativo y centralizado.
En el ejemplo siguiente, Calculadora.raiz(double) calcula Math.sqrt(x) si x >= 0; en caso contrario lanza IllegalArgumentException. 

En main se muestran dos llamadas: una válida y otra inválida; la segunda queda capturada en el catch, donde se informa al usuario sin que la función raiz imprima nada por sí misma.

public class Calculadora {

    /**
     * Devuelve la raíz cuadrada de x si x >= 0.
     * Lanza IllegalArgumentException si x es negativo.
     */
    public static double raiz(double x) {
        if (x < 0.0) {
            throw new IllegalArgumentException(
                "Argumento fuera de dominio: no existe raíz real de un número negativo (x=" + x + ")"
            );
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        // Caso correcto
        double a = 9.0;
        double r1 = Calculadora.raiz(a);
        System.out.printf("raiz(%.2f) = %.4f%n", a, r1);

        // Caso con error controlado desde fuera
        double b = -4.0;
        try {
            double r2 = Calculadora.raiz(b);
            System.out.printf("raiz(%.2f) = %.4f%n", b, r2);
        } catch (IllegalArgumentException ex) {
            // Tratamiento del error en el llamador (informar al usuario, registrar, etc.)
            System.out.println("Error: " + ex.getMessage());
        }

        // El programa continúa de forma controlada
        System.out.println("Fin del programa.");
    }
}

Nota: si se prefiriese forzar al llamador a tratar el error de manera explícita, se podría definir un tipo de excepción comprobada (extends Exception) y declararla con throws en la firma del método. Sin embargo, para precondiciones incumplidas como argumentos inválidos, en Java se prefiere una unchecked como IllegalArgumentException, por ser un error de uso de la API.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
En Java, “lanzar” una excepción significa interrumpir el flujo normal del método mediante la instrucción throw, indicando que ha ocurrido una situación anómala que impide continuar. Cuando un método lanza una excepción, no devuelve un valor ni sigue ejecutando las instrucciones restantes: el control salta inmediatamente hacia fuera del método, buscando un bloque catch adecuado en algún punto de la pila de llamadas. En el ejemplo de la raíz cuadrada, el método raiz lanza IllegalArgumentException si recibe un argumento negativo, señalando con claridad que el contrato del método ha sido violado.

Por otro lado, “capturar” o “controlar” una excepción consiste en envolver la llamada potencialmente problemática dentro de un bloque try seguido de uno o más catch. Cuando la excepción lanzada coincide con el tipo del catch, el flujo del programa entra en ese bloque y ejecuta el código de manejo: informar, registrar, recuperar o cualquier acción pertinente. En el caso de la raíz cuadrada, el método main sería el encargado de capturar la excepción para informar al usuario, manteniendo así la separación entre la lógica de cálculo y la gestión del error.

La “propagación” de una excepción ocurre cuando un método no la controla. En ese caso, la excepción sube automáticamente al método llamador. Este proceso continúa hacia arriba en la pila hasta encontrar un manejador o, si ninguno existe, terminar el programa con un error no capturado. Durante la propagación, cada método intermedio no se reanuda; una vez que la excepción sale de un método, este finaliza abruptamente sin volver al punto donde ocurrió el fallo. 

Por eso se dice que las funciones “colapsan” hacia atrás en la pila, liberando sus contextos de ejecución sin reanudar su flujo.
A continuación se muestra el ejemplo adaptado al caso:

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
La propagación natural de excepciones permite que un error viaje automáticamente hacia atrás por la pila de llamadas, saltándose todas las funciones intermedias hasta encontrar un bloque catch adecuado. Esto evita que cada función deba comprobar manualmente códigos de retorno, como ocurre en C. 
Gracias a ello, el código que implementa la lógica principal permanece más limpio y centrado en su propósito, porque no se ve obligado a incluir comprobaciones repetitivas ni a reenviar errores hacia arriba en cada nivel.

Otra ventaja importante es que la propagación automática reduce la posibilidad de ignorar accidentalmente un error. En C, es frecuente que una función devuelva un código de fallo y que el llamador, por descuido, no lo compruebe. Esto puede llevar a fallos posteriores más difíciles de rastrear. En cambio, en Java, si una excepción no se captura, sigue subiendo hasta forzar su manejo o provocar un error claro en tiempo de ejecución, lo cual facilita la depuración y hace más explícita la existencia del problema.

Además, este mecanismo permite concentrar el manejo de errores en un único punto, cuando resulta apropiado, en lugar de repartirlo por todas las funciones intermedias. Una función puede limitarse a realizar su trabajo y, si algo falla, lanzar una excepción sin preocuparse por el contexto superior. 
Esto genera una separación más nítida entre lógica normal y gestión de errores, algo que contribuye a un diseño más modular, claro y mantenible.

Por último, esta propagación automática encaja bien con estructuras como try/catch/finally, lo que garantiza que ciertos recursos se liberen siempre, incluso cuando ocurre un error. En C, este patrón debe implementarse manualmente, lo que requiere disciplina y puede dar lugar a fugas de recursos si no se gestiona con cuidado.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
En la mayoría de lenguajes orientados a objetos, incluyendo Java, las excepciones son objetos. Esto significa que no solo representan que ha ocurrido un error, sino que además encapsulan información útil dentro de atributos y métodos. Por ejemplo, una excepción puede almacenar un mensaje descriptivo, la causa original del fallo o el rastro de la pila. Al tratarse de objetos, pueden manipularse, transmitirse y clasificarse igual que cualquier otra instancia de una jerarquía de clases.

Desde el punto de vista de la encapsulación, este enfoque permite agrupar toda la información relevante sobre el error en un solo lugar. En lugar de devolver múltiples valores o códigos dispersos, se encapsula el estado del fallo dentro del propio objeto excepción. Esto hace más claro y modular el diseño: las funciones lanzan una excepción que describe el problema y el código que la captura decide cómo procesarla, sin necesidad de acceder a datos externos o estructuras paralelas.

Otra ventaja es que, gracias a que las excepciones forman una jerarquía de clases, el manejo se puede hacer tanto de forma general (capturando una superclase) como de forma específica (capturando un tipo concreto). Esto facilita diseñar sistemas extensibles y coherentes, donde cada tipo de error está representado por una clase distinta, permitiendo un tratamiento más preciso y estructurado.

Por supuesto, esta organización permite crear excepciones personalizadas. En Java basta con definir una clase que extienda Exception o RuntimeException, según se quiera una excepción comprobada o no comprobada. Estas excepciones a medida permiten describir fallos propios de una aplicación o dominio concreto, mejorando la claridad del código y la posibilidad de que el llamador actúe de forma adecuada ante cada situación.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Un objeto excepción en Java siempre lleva consigo información esencial que resulta muy útil cuando finalmente llega a un manejador (catch). La más importante es el mensaje descriptivo (String message), que indica la causa del problema de forma humana y clara. Este mensaje, que en C habría que gestionar manualmente a través de códigos de error o cadenas externas, queda aquí perfectamente encapsulado dentro del propio objeto, lo que facilita identificar rápidamente la naturaleza del fallo.

Además del mensaje, toda excepción incluye automáticamente el rastro de la pila (stack trace). Este rastro muestra la secuencia exacta de llamadas que condujo al error, es decir, cómo se llegó desde el main hasta el punto donde la excepción fue lanzada. 

En C, obtener esta información es difícil y requiere mecanismos externos o bibliotecas adicionales; en Java se obtiene de manera automática, estructurada y con gran valor diagnóstico para depurar fallos complejos.
Otra pieza de información encapsulada es la “causa” (Throwable cause) cuando una excepción se lanza a partir de otra. 

Esto permite encadenar errores, preservando el contexto completo a través de las capas del programa. Con ello, cuando un manejador recibe la excepción, puede entender no solo el error final, sino también el origen profundo del problema y cómo se propagó, algo que en C suele perderse salvo que se implemente a mano.

En conjunto, mensaje, causa y stack trace proporcionan una visión muy rica y estructurada del fallo, completamente integrada en el propio objeto excepción. Este nivel de detalle supera con creces lo que se obtiene en C con simples códigos de error, y demuestra una de las grandes ventajas de la orientación a objetos aplicada al manejo de errores.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
Sí, en Java se pueden tener varios bloques catch asociados a un mismo try. Cada catch especifica el tipo de excepción que sabe manejar, lo que permite responder de forma diferenciada según la causa del fallo. También existe la forma multi-catch (catch (A | B e)), útil cuando varias excepciones se tratan exactamente igual; en ese caso, la variable e es efectivamente final y no puede reasignarse.

Solo se ejecuta un único catch por excepción lanzada: el primero cuya firma sea compatible con el tipo real de la excepción en ejecución. Por ello, es crucial ordenar los catch de más específico a más general; de lo contrario, se produciría un error de compilación por “código inalcanzable” si, por ejemplo, se coloca catch (Exception e) antes de catch (IOException e).
Un esquema típico sería:

try {
    // código que puede lanzar distintas excepciones
} catch (NumberFormatException e) {
    // manejo específico 1
} catch (IllegalArgumentException e) {
    // manejo específico 2
} catch (IOException | SQLException e) {
    // manejo común para varios tipos (multi-catch)
} catch (Exception e) {
    // manejo general (último recurso)
}

Este patrón asegura que se capture la excepción más adecuada sin bloquear catch más específicos, manteniendo un manejo de errores claro y coherente.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
El bloque finally garantiza la ejecución de un código de limpieza siempre, ocurra o no una excepción, y aunque la excepción se propague. Esto resulta clave para cerrar ficheros, liberar memoria o desbloquear recursos, evitando fugas que en C suelen gestionarse a mano con patrones como “goto cleanup”. En Java, finally se ejecuta tras el try y (si existe) tras el catch correspondiente, incluso si dentro del try se hace return o se relanza la excepción. La única excepción práctica son situaciones anómalas como un System.exit() o el apagado abrupto de la JVM.

Cuando se desea manejar la excepción y, en todo caso, liberar recursos, se emplea try { ... } catch (...) { ... } finally { ... }. El catch procesa el error (p. ej., registrar o informar) y el finally cierra/desbloquea. Si no interesa capturar la excepción en ese nivel y se quiere permitir que se propague, puede usarse try { ... } finally { ... } sin catch: el finally hace la limpieza y, acto seguido, la excepción continúa su camino hacia arriba.

Ejemplo con catch y finally: manejar el error y cerrar el recurso

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class DemoCatchFinally {
    public static void main(String[] args) {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("datos.txt"));
            String linea = br.readLine();
            System.out.println("Primera línea: " + linea);
            // ... más lógica que puede lanzar IOException ...
        } catch (IOException e) {
            // Manejo local del error (informar, registrar, alternativa, etc.)
            System.err.println("Error de E/S: " + e.getMessage());
        } finally {
            // Se ejecuta SIEMPRE: cerrar el recurso si está abierto
            if (br != null) {
                try { br.close(); } 
                catch (IOException cierreErr) {
                    System.err.println("Error al cerrar: " + cierreErr.getMessage());
                }
            }
        }
        System.out.println("El programa continúa.");
    }
}

Ejemplo con finally sin catch: limpieza obligatoria y propagación de la excepción

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class DemoTryFinally {
    // Se declara que este método puede lanzar IOException (excepción comprobada)
    public static String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta));
            return br.readLine(); // podría lanzar IOException
        } finally {
            // Se ejecuta SIEMPRE, incluso si arriba se lanzó IOException o se hizo return
            if (br != null) {
                try { br.close(); } 
                catch (IOException cierreErr) {
                    // Aquí normalmente se registra; no se suele suprimir la excepción original
                    System.err.println("Error al cerrar: " + cierreErr.getMessage());
                }
            }
        }
    }

    public static void main(String[] args) throws IOException {
        // Si ocurre un error, se propagará hasta aquí (o más arriba) después de ejecutar finally
        String linea = leerPrimeraLinea("datos.txt");
        System.out.println("Primera línea: " + linea);
    }
}

Como mejora idiomática en Java moderno, cuando el recurso es AutoCloseable, se recomienda try-with-resources para que el cierre sea automático y seguro, reduciendo la necesidad de finally para esta tarea.
Aun así, el bloque finally sigue siendo la herramienta general para asegurar acciones finales que deben ocurrir pase lo que pase, tanto si se captura la excepción como si se deja que se propague naturalmente.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Sí, en Java el bloque finally puede ir sin catch. Es perfectamente válido escribir un try seguido únicamente de finally. Esto se usa cuando no se desea manejar la excepción en ese nivel, pero sí se necesita garantizar una acción final, como cerrar un recurso o liberar memoria lógica, antes de permitir que la excepción continúe propagándose hacia arriba en la pila de llamadas.

El bloque finally se ejecuta siempre, tanto si ocurre una excepción como si no ocurre. Tras completar el código del try (o tras ejecutar un catch, si existe), el flujo pasa inevitablemente por el finally antes de abandonar la estructura. La única forma de impedir esto sería una terminación abrupta del proceso, como System.exit(), un apagado de la JVM o un error grave en tiempo de ejecución que impida continuar.

En el caso de que dentro del try exista un return, el finally también se ejecuta siempre. El flujo real es: se evalúa el return, la JVM prepara el valor de retorno, pero antes de abandonar el método, se ejecuta el código del finally. Solo cuando éste termina, el método devuelve efectivamente el valor. Por eso se recomienda evitar escribir return dentro de finally, ya que puede ocultar errores o reemplazar el valor que se tenía previsto devolver.

En resumen, finally garantiza la ejecución de código crítico para limpieza o cierre de recursos en prácticamente cualquier circunstancia. Esta fiabilidad hace que se convierta en una herramienta fundamental para asegurar que un método deja el sistema en un estado coherente, ocurra o no una excepción durante su ejecución.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
En Java, las excepciones controladas (checked) son aquellas que el compilador obliga a capturar con try–catch o a declarar con throws. Representan fallos esperables en la ejecución normal, generalmente debidos a condiciones externas: problemas de E/S, red, bases de datos, recursos no disponibles… Son parte explícita del contrato del método. 

En cambio, las excepciones no controladas (unchecked) son aquellas que heredan de RuntimeException. No requieren captura ni declaración, porque normalmente representan errores de programación, como incumplir una precondición o acceder fuera de rango. La jerarquía de RuntimeException sirve de base para todos esos fallos lógicos que el lenguaje considera que no deberían manejarse necesariamente en tiempo de ejecución.

En términos de diseño, las excepciones controladas se utilizan cuando el llamador puede razonablemente intentar recuperarse del fallo. Por ejemplo, si un fichero no existe, el llamador podría preguntar por una ruta alternativa o informar al usuario. Las no controladas, por el contrario, se usan cuando el fallo indica un bug o un uso incorrecto del método, para el cual no tiene sentido obligar al programador a capturar la excepción: si un método recibe un argumento negativo cuando no debería, la respuesta adecuada no es capturar la excepción sino corregir el código que la causó. Por eso, muchos lenguajes de programación solo ofrecen excepciones no controladas.

A continuación, se muestran dos listas para ilustrar ambos tipos, junto con situaciones típicas donde se prefiere cada grupo. Son todas excepciones que podríamos usar en código propio:

✔ Ejemplos de excepciones controladas típicas
Estas necesitan try–catch o throws:

IOException – error de lectura o escritura.
FileNotFoundException – fichero ausente.
SQLException – error en operaciones con bases de datos.
ClassNotFoundException – cuando falta una clase a cargar dinámicamente.

Situaciones donde se prefiere una excepción controlada:

Cuando una operación depende de recursos externos (ficheros, red) que pueden fallar legítimamente.
Cuando el llamador puede escoger recuperarse (reintentar, pedir nueva ruta, etc.).
Cuando se quiere forzar que el desarrollador decida qué hacer ante el error.
Cuando la API quiere dejar claro que la operación no es totalmente fiable y hay que tratar la posibilidad de fallo.


✔ Ejemplos de excepciones no controladas típicas (subclases de RuntimeException)
No obligan a capturar ni declarar:

IllegalArgumentException – argumento no válido.
NullPointerException – referencia nula usada indebidamente.
IndexOutOfBoundsException – índice incorrecto en un array o lista.
ArithmeticException – divisiones entre cero u otros errores aritméticos.

Situaciones donde se prefiere una excepción no controlada:

Cuando se incumplen precondiciones del método (argumentos inválidos).
Cuando ocurre un error de programación que debe corregirse en el código, no manejarse.
Cuando el coste de obligar al manejo sería contraproducente (API más ruidosa sin beneficio real).
Cuando se quiere permitir que la excepción suba sin restricciones hasta un manejador global.


En conjunto, las excepciones controladas ayudan a gestionar fallos esperables y externos, mientras que las no controladas expresan errores lógicos que deben corregirse en el código. La presencia de ambas categorías en Java permite diseñar APIs más claras y con una separación adecuada entre errores recuperables y errores de programación.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
La palabra clave throws se utiliza en la firma de un método para indicar que dicho método puede lanzar una o varias excepciones comprobadas (checked exceptions). 

Es una forma de documentar y declarar explícitamente que la operación no garantiza poder completarse siempre, obligando al código llamador a hacerse responsable de ese posible fallo. En otras palabras, throws forma parte del contrato público del método: se especifica no solo lo que hace, sino también qué errores excepcionales pueden ocurrir durante su ejecución.

Este mecanismo se usa para que una función no tenga que capturar internamente una excepción comprobada si no desea o no puede gestionarla en ese nivel. En vez de envolver el código en un bloque try-catch, el método delega la responsabilidad al llamador, quien decidirá si capturar, registrar, notificar o volver a propagar el error. 

Esta delegación resulta útil cuando el método no conoce el contexto adecuado para actuar ante el fallo (por ejemplo, qué hacer si no se encuentra un fichero), pero sí necesita informar de que algo excepcional ha sucedido.
Por eso throws es una alternativa a capturar una excepción controlada: en lugar de gestionarla localmente, se anuncia que esa excepción puede propagarse hacia arriba sin ser atrapada dentro del método. 

Con ello, el método queda más limpio y centrado en su lógica principal, mientras que el llamador decide la política adecuada de manejo de errores. Esta idea se opone a las excepciones no comprobadas, que no requieren ni throws ni catch, porque en ese caso el lenguaje no obliga a tratarlas explícitamente.

En conjunto, throws complementa el sistema de excepciones controladas de Java asegurando que, cuando se produce un fallo previsible y relevante, ningún nivel del programa pueda ignorarlo sin ser consciente de ello. Esto mejora la robustez del software y la claridad del diseño, al forzar a cada método de la cadena de llamadas a decidir si captura, propaga o traduce la excepción a otra más significativa.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
Una forma de propagar la excepción en lugar de manejarla localmente consiste en declararla en la firma del método con throws. De este modo, el método que abre el fichero no captura IOException (por ejemplo, cuando el fichero no existe), sino que asegura la limpieza en un finally y deja que el llamador decida qué hacer con el error. Es un patrón útil cuando el método carece de contexto para tomar decisiones (p. ej., reintentar, pedir otra ruta, etc.).

En el siguiente ejemplo, leerPrimeraLinea declara throws IOException, usa try con finally (sin catch) para cerrar siempre el recurso, y permite que la excepción se propague hacia arriba. En main se muestra cómo el llamador puede optar por capturarla o también declararla y delegarla aún más.

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class LectorArchivos {

    /**
     * Abre un fichero y devuelve su primera línea.
     * No maneja la excepción si el fichero no existe u ocurre un error de E/S:
     * declara throws para que se propague hacia arriba.
     */
    public static String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta)); // puede lanzar FileNotFoundException (subtipo de IOException)
            return br.readLine();                           // puede lanzar IOException
        } finally {
            // Se ejecuta SIEMPRE: asegura el cierre aunque haya return o excepción
            if (br != null) {
                try {
                    br.close();
                } catch (IOException cierreErr) {
                    // Aquí normalmente se registraría; no se suprime la excepción original
                    // para no ocultar el fallo principal.
                }
            }
        }
    }

    public static void main(String[] args) {
        try {
            String linea = leerPrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            // Manejo en el nivel llamador (informar, registrar, decidir política, etc.)
            System.err.println("No fue posible leer el fichero: " + e.getMessage());
        }
    }
}

Como alternativa moderna y más concisa, cuando el recurso implementa AutoCloseable, puede emplearse try-with-resources para el cierre automático; sin embargo, la pregunta pide explícitamente “acuérdate del finally”, por lo que se muestra el patrón clásico try { ... } finally { ... } que garantiza la liberación de recursos al margen de si la excepción se captura o se deja propagar.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Sí, es posible poner excepciones no controladas (unchecked) como RuntimeException en la cláusula throws, aunque no es necesario. En Java, las excepciones no controladas no requieren ni try‑catch ni throws; por diseño, pueden propagarse libremente sin que el compilador obligue a tratarlas. Por eso, incluirlas en throws es opcional y suele tener únicamente un propósito documental: indicar explícitamente que el método podría lanzar esa excepción, aunque el compilador no lo exija.

El método llamador no está obligado a poner un try‑catch en estos casos, porque las RuntimeException no generan error de compilación si no se capturan. De hecho, muchas veces no se capturan porque reflejan fallos de programación (p. ej., NullPointerException, IndexOutOfBoundsException, IllegalArgumentException). Capturarlas indiscriminadamente puede ocultar errores y dificultar la depuración, por lo que solo se hace en situaciones muy concretas donde tenga sentido recuperar el programa tras un fallo lógico controlable.

El sentido de declarar en throws una excepción no controlada es, por tanto, documentar intencionalmente el contrato del método: se deja claro a quien lo use que, en caso de incumplir ciertas precondiciones o de fallos internos, podría lanzarse una excepción concreta de tipo RuntimeException. Esto ayuda a la legibilidad del API, sin forzar al usuario a escribir try‑catch. En esencia, se trata de una herramienta de comunicación más que de una obligación técnica.

En conclusión, aunque es posible incluir excepciones no controladas en throws, y puede tener valor explicativo, no implica que el llamador deba tratarlas. Su uso tiene sentido cuando se quiere dejar constancia explícita del comportamiento excepcional del método, manteniendo coherencia y claridad en el diseño de la interfaz sin imponer carga adicional al código que lo invoca.
Proporcione sus comentarios sobre BizChat

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta
Las excepciones controladas (checked), como IOException, se recomiendan cuando el error forma parte del funcionamiento normal y previsible del programa. Son fallos que no indican un bug, sino una condición externa que puede ocurrir legítimamente: un fichero que no existe, un recurso de red no disponible, falta de permisos, etc. 
En estos casos, el lenguaje obliga a que el programador decida entre capturar la excepción o declararla con throws, lo cual garantiza que no pueda ignorarse accidentalmente. Este enfoque ayuda a hacer explícito en la API que la operación depende de factores externos que pueden fallar y que el llamador debe tomar una decisión informada al respecto.

Por el contrario, las excepciones no controladas (unchecked), como IllegalArgumentException o NullPointerException, se usan para situaciones que representan errores de programación o violación de precondiciones. Si un método recibe un argumento inválido, no suele ser lógico que intente recuperarse; lo que se quiere es notificar un mal uso de la API. Tampoco tendría sentido obligar al llamador a capturar este tipo de errores, ya que normalmente no es posible corregirlos dinámicamente. Por eso las excepciones no controladas no requieren try-catch ni throws y permiten un diseño más fluido en casos donde la responsabilidad del error recae claramente en el código cliente.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. De hecho, Java es uno de los pocos lenguajes populares que incluye excepciones comprobadas como parte central de su modelo. Otros lenguajes como C++, C#, Python, JavaScript o Rust solo tienen un sistema de excepciones (o errores) no controladas. Esto se debe a que muchos desarrolladores consideran que obligar al manejo explícito puede generar código ruidoso y dificultar la evolución de APIs grandes. Por ello, en lenguajes que solo ofrecen una opción, lo habitual es un modelo como el de las unchecked exceptions, en el que las excepciones se usan cuando se produce un error, pero sin imponer restricciones de captura por parte del compilador.

En resumen, las excepciones controladas se reservan para fallos externos y esperables en tiempo de ejecución, mientras que las no controladas se aplican a errores de programación o precondiciones incumplidas. Y aunque Java ofrece ambas categorías, la mayoría de lenguajes optan por un único modelo donde las excepciones son similares a las no controladas de Java, lo que deja la decisión de manejarlas o no completamente en manos del programador.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta
Sí, tiene sentido lanzar excepciones dentro de un catch cuando se desea traducir una excepción de bajo nivel a otra más significativa para la capa actual (manteniendo la causa original), o cuando se quiere enriquecer el contexto con un mensaje más claro. 

Esta práctica se conoce como exception translation y ayuda a preservar la encapsulación de la API: se evita filtrar tipos de error internos (p. ej., de la base de datos o del sistema de ficheros) hacia capas superiores, que solo deberían conocer excepciones del dominio de la aplicación.

También es posible relanzar la misma excepción capturada (rethrow). En Java, basta con throw e;. Esto es útil cuando el bloque catch realiza tareas auxiliares (registro, métricas, telemetría, actualización de estado) pero no puede resolver el problema en ese nivel. Al relanzarla, se mantiene la propagación natural hacia un manejador más adecuado, conservando el stack trace original (no se “resetea” al re-lanzar). Se recomienda este enfoque cuando se quiere conservar el tipo exacto de la excepción y su rastro, y no tiene sentido cambiarlo por otro.

A continuación se muestran dos ejemplos: (1) traducir una excepción técnica a una de dominio con causa encadenada; (2) relanzar la misma excepción tras registrar y asegurar limpieza. Ambos patrones son idiomáticos en Java y complementan el uso de finally o try-with-resources para garantizar cierre de recursos.

1) Lanzar una nueva excepción dentro del catch (traducción y contexto)

class PedidoException extends Exception {
    public PedidoException(String message, Throwable cause) {
        super(message, cause);
    }
}

public class ServicioPedidos {
    public void crearPedido(String clienteId) throws PedidoException {
        try {
            // Código que puede lanzar SQLException (capa de persistencia)
            insertarEnBD(clienteId);
        } catch (java.sql.SQLException e) {
            // Se traduce a una excepción de dominio, preservando la causa
            throw new PedidoException(
                "No fue posible crear el pedido para el cliente " + clienteId + ".",
                e
            );
        }
    }

    private void insertarEnBD(String clienteId) throws java.sql.SQLException {
        // ... inserción en base de datos ...
    }
}

2) Relanzar la misma excepción capturada (registro/limpieza y propagación)

import java.io.*;

public class LectorSeguro {
    public String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta));
            return br.readLine();
        } catch (IOException e) {
            // Acción auxiliar: registrar y relanzar SIN cambiar el tipo
            System.err.println("Fallo leyendo '" + ruta + "': " + e.getMessage());
            throw e; // rethrow: conserva el stack trace original
        } finally {
            if (br != null) {
                try { br.close(); } catch (IOException cierre) {
                    // Se registra pero no se oculta el error principal
                    System.err.println("Error al cerrar: " + cierre.getMessage());
                }
            }
        }
    }
}

En resumen, lanzar una nueva excepción en catch tiene sentido al elevar el nivel semántico del error o al encapsular detalles internos de la capa inferior; relanzar la misma es apropiado cuando el nivel actual no puede solucionar el problema y solo desea anotar o limpiar antes de dejar que un manejador superior decida. 
Ambos patrones aprovechan la propagación natural de Java manteniendo claridad y separación de responsabilidades.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta
Una excepción es la “causa” de otra cuando el error original se encadena dentro de una nueva excepción que se lanza a un nivel superior. 

En Java esto se modela con el campo Throwable cause, que permite preservar el contexto del fallo original al traducirlo a una excepción más significativa para la capa actual. Este encadenamiento se establece pasando la causa al constructor de la nueva excepción (super(mensaje, causa)) o, alternativamente, usando initCause(causa) cuando el constructor no la recibe.

Este patrón es útil para ocultar detalles de bajo nivel (por ejemplo, una SQLException o una IOException) y exponer a capas superiores una excepción de dominio más expresiva (p. ej., PedidoException, ServicioUsuariosException). 

Con ello se mantiene la encapsulación: la API pública no filtra tipos técnicos internos, pero no se pierde el diagnóstico, porque el stack trace conserva la cadena de causas. Al imprimirse una excepción con causa (p. ej., con printStackTrace() o registrándola en un logger), la salida muestra secciones con el prefijo Caused by:, evidenciando cada nivel del encadenamiento.

Ejemplo: capturar una excepción de bajo nivel y encapsularla en otra personalizada

// Excepción de alto nivel (de dominio)
class PedidoException extends Exception {
    public PedidoException(String message, Throwable cause) {
        super(message, cause); // preserva la causa original
    }
}

public class ServicioPedidos {

    public void crearPedido(String clienteId) throws PedidoException {
        try {
            insertarEnBD(clienteId);  // puede lanzar SQLException
        } catch (java.sql.SQLException e) {
            // Traducción: de técnica (SQL) a dominio (Pedido)
            throw new PedidoException(
                "No fue posible crear el pedido para el cliente " + clienteId + ".",
                e // causa encadenada
            );
        }
    }

    private void insertarEnBD(String clienteId) throws java.sql.SQLException {
        // ... lógica que interactúa con la base de datos ...
        throw new java.sql.SQLException("Violación de clave única (cliente repetido).");
    }

    public static void main(String[] args) {
        ServicioPedidos s = new ServicioPedidos();
        try {
            s.crearPedido("C123");
        } catch (PedidoException ex) {
            // Al imprimir, se verá el encadenamiento con "Caused by:"
            ex.printStackTrace();
        }
    }
}

Cuando esta excepción sale por pantalla, efectivamente se ve la causa. La traza mostrará primero la PedidoException con su mensaje y, a continuación, una sección Caused by: java.sql.SQLException: ... con su propio stack trace. 

Si hay múltiples niveles de traducción, aparecerá una cadena de “Caused by” anidados, lo que facilita reconstruir el recorrido del error desde el origen técnico hasta el punto donde se decidió encapsularlo con semántica de dominio.