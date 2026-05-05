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

### Respuesta
Un puntero a una función en C es una variable cuyo valor no es un dato convencional (como un entero o una dirección de memoria cualquiera), sino la dirección de una función. 

Esto permite tratar a las funciones como si fueran datos: se pueden almacenar en variables, pasarlas como parámetros o devolverlas como resultado. Este mecanismo es especialmente útil para desacoplar el código, implementar callbacks o emular comportamientos más flexibles, algo que conecta conceptualmente con el uso de métodos como objetos en otros paradigmas.

Desde el punto de vista sintáctico, un puntero a función debe declarar el tipo de retorno y los tipos de los parámetros de la función a la que va a apuntar. A diferencia de Java, donde las referencias a métodos se abstraen mediante interfaces funcionales o expresiones lambda, en C esta relación es explícita y depende completamente del tipo. La invocación a través del puntero se realiza utilizando el propio puntero como si fuera el nombre de la función.

En el ejemplo siguiente se define una función que recibe una cadena de caracteres (char *), convierte sus letras a mayúsculas y devuelve la misma cadena modificada. Posteriormente, se crea un puntero a dicha función en una variable local llamada aMayusculas, y se invoca la función a través de ese puntero. La conversión se realiza carácter a carácter, aprovechando la función estándar toupper.

#include <stdio.h>
#include <ctype.h>

char *convertirAMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "Hola mundo";

    char *(*aMayusculas)(char *) = convertirAMayusculas;

    aMayusculas(texto);

    printf("%s\n", texto);
    return 0;
}
``

Este ejemplo muestra cómo el puntero aMayusculas actúa como intermediario para llamar a la función real, sin necesidad de conocer su nombre directamente en el punto de uso. 

Este concepto resulta fundamental para comprender técnicas avanzadas en C y sirve como base para ideas más modernas presentes en la programación funcional y en características posteriores de lenguajes como Java.

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una función lambda es una función anónima, es decir, una función que no tiene nombre y que puede definirse de forma inline para asignarse a una variable o pasarse como argumento. Su principal objetivo es expresar comportamientos de manera concisa, evitando la declaración explícita de funciones completas cuando solo se necesita una operación concreta.

Conceptualmente, una función lambda permite tratar el código como un valor, algo alineado con ideas de la programación funcional.
A diferencia de C, donde se utilizan punteros a funciones para lograr un efecto similar, lenguajes como JavaScript y Java incorporan las funciones lambda como una construcción del propio lenguaje. 

En JavaScript, las funciones son ciudadanos de primera clase desde su diseño, mientras que en Java las funciones lambda se introducen a partir de Java 8 y siempre están asociadas a un tipo funcional (por ejemplo, Function<T, R>). Esto encaja con los conocimientos previos de programación orientada a objetos, ya que una lambda puede verse como una implementación compacta de una interfaz con un único método.

En JavaScript, una función lambda (normalmente llamada arrow function) puede asignarse directamente a una variable local. En el siguiente ejemplo, la variable aMayusculas referencia una función que recibe una cadena y devuelve su versión en mayúsculas, utilizando métodos estándar del propio lenguaje:

let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

let resultado = aMayusculas("Hola mundo");
console.log(resultado);

En Java, una función lambda debe respetar el tipo de una interfaz funcional. Para simplificar, se utiliza Function<String, String>, que representa una función que recibe un String y devuelve otro String. La variable local aMayusculas apunta a la lambda, y la invocación se realiza mediante el método apply, manteniendo coherencia con el modelo de objetos de Java:

import java.util.function.Function;

public class Ejemplo {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        String resultado = aMayusculas.apply("Hola mundo");
        System.out.println(resultado);
    }
}

Estos ejemplos muestran cómo las funciones lambda ofrecen una alternativa más expresiva y compacta frente a punteros a funciones o clases anónimas, facilitando la escritura de código más claro y centrado en el comportamiento.

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El paradigma funcional es un modelo de programación que se basa en el uso de funciones puras como unidad principal de construcción del programa. En este paradigma, el énfasis se pone en describir qué se quiere calcular, en lugar de cómo hacerlo paso a paso, reduciendo el uso de estados mutables y efectos secundarios. 

El cálculo se expresa mediante la evaluación de funciones, lo que conceptualmente lo aproxima a las funciones matemáticas y lo diferencia del estilo imperativo típico de C o del enfoque centrado en objetos de Java tradicional.

Se denomina a lenguajes como Java 8 multi‑paradigma porque permiten combinar varios paradigmas dentro del mismo lenguaje, en este caso el orientado a objetos y el funcional. Java sigue teniendo como eje central las clases, los objetos y la herencia, pero desde Java 8 incorpora características funcionales como funciones lambda, interfaces funcionales y operaciones sobre colecciones basadas en funciones (map, filter, reduce). Esto permite elegir el estilo más adecuado según el problema, sin abandonar el ecosistema de la programación orientada a objetos.

Decir que las funciones son “ciudadanos de primera clase” significa que las funciones pueden tratarse igual que cualquier otro valor del lenguaje. Esto implica que pueden almacenarse en variables, pasarse como parámetros a otras funciones, devolverse como resultado y asignarse dinámicamente. En C esto se logra mediante punteros a funciones, mientras que en Java se consigue mediante referencias a funciones lambda asociadas a interfaces funcionales.

Este concepto es clave en el paradigma funcional, ya que permite construir programas mediante la composición de funciones, favoreciendo un código más modular, expresivo y fácil de razonar. Para quien proviene de C o de Java orientado a objetos, esta idea supone un cambio conceptual importante, pero no rompe con los conocimientos previos, sino que los amplía ofreciendo una forma alternativa y complementaria de estructurar el comportamiento del programa.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis básica de una función lambda en Java se introdujo a partir de Java 8 y permite definir implementaciones de métodos de forma compacta, sin necesidad de crear clases explícitas ni clases anónimas. Una función lambda siempre está asociada a una interfaz funcional, es decir, una interfaz que tiene un único método abstracto. 

La expresión lambda representa directamente la implementación de ese método.
Formalmente, la sintaxis general de una lambda en Java es:
(lista de parámetros) -> expresión o bloque de código.

A la izquierda del operador -> se indican los parámetros del método (con o sin tipos explícitos), y a la derecha se define el cuerpo de la función. Si el cuerpo consta de una única expresión, el valor de dicha expresión se devuelve implícitamente; si se usa un bloque de código {}, es necesario emplear return cuando corresponda.

Por ejemplo, una lambda que recibe un String y devuelve ese mismo texto en mayúsculas puede escribirse de forma muy concisa. En este caso, Java infiere automáticamente el tipo de los parámetros a partir del contexto (el tipo de la interfaz funcional):

Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

También es posible usar una sintaxis más explícita cuando se desea mayor claridad, incluyendo los tipos y un bloque de código:

Function<String, String> aMayusculas = (String cadena) -> {
    return cadena.toUpperCase();
};

Esta sintaxis permite expresar comportamiento de forma clara y compacta, facilitando un estilo de programación más funcional dentro del marco orientado a objetos de Java. Al compararlo con C, equivale conceptualmente a asignar una función a una variable, pero con una sintaxis más segura y estrechamente integrada con el sistema de tipos del lenguaje.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Recibir una función como parámetro implica tratar el comportamiento como un dato que puede delegarse a otro método. Este enfoque es característico del paradigma funcional y permite desacoplar la lógica de transformación del flujo general del programa. En lugar de codificar la operación directamente dentro del método, se recibe una función externa que define cómo transformar el valor, haciendo el código más flexible y reutilizable.

En JavaScript, esto resulta especialmente natural porque las funciones son ciudadanos de primera clase desde el diseño del lenguaje. El método transformar puede recibir una cadena y una función transformadora, e invocar dicha función desde su interior sin ninguna sintaxis adicional. La variable local aMayusculas apunta a una función lambda (arrow function), que se pasa directamente como argumento:

let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

function transformar(texto, transformador) {
    return transformador(texto);
}

let resultado = transformar("Hola mundo", aMayusculas);
console.log(resultado);

En Java, el proceso es similar en concepto, aunque sintácticamente más explícito debido al sistema de tipos. El método transformar recibe un String y una referencia a función del tipo Function<String, String>. Desde dentro del método, la función se invoca mediante apply, manteniendo la coherencia con el modelo orientado a objetos del lenguaje:

import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        String resultado = transformar("Hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}

Este patrón muestra claramente cómo un método puede delegar parte de su comportamiento a una función externa, logrando una separación más clara de responsabilidades. Para quien proviene de C o Java clásico, puede verse como una evolución natural del uso de punteros a funciones o del polimorfismo, pero con una sintaxis más expresiva y orientada a la composición de comportamientos.

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Invocar un método pasando una función lambda definida directamente en la llamada permite expresar el comportamiento de forma inmediata, sin necesidad de crear una variable intermedia. Este estilo es muy habitual en programación funcional, ya que facilita la lectura cuando la lógica es breve y está estrechamente ligada al punto donde se utiliza. De esta forma, el código se centra más en qué transformación se desea aplicar que en detalles estructurales.

En JavaScript, esta técnica resulta especialmente sencilla, ya que la función lambda puede escribirse inline como segundo parámetro de transformar. En el siguiente ejemplo, se pasa directamente una función que invierte la cadena recibida, utilizando métodos estándar del lenguaje:

function transformar(texto, transformador) {
    return transformador(texto);
}

let resultado = transformar("Hola mundo", (cadena) => {
    return cadena.split("").reverse().join("");
});

console.log(resultado);

En Java, el planteamiento es equivalente desde el punto de vista conceptual, aunque el sistema de tipos obliga a que la lambda sea compatible con Function<String, String>. La función de inversión se define justo en la llamada al método transformar, sin variables adicionales, manteniendo el código compacto y expresivo:

import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String resultado = transformar("Hola mundo", cadena ->
            new StringBuilder(cadena).reverse().toString()
        );

        System.out.println(resultado);
    }
}

Este uso directo de funciones lambda refuerza la idea de que el comportamiento puede tratarse como un valor más del programa. Para alguien con experiencia en C o en Java orientado a objetos clásico, este estilo supone una evolución natural hacia un código más declarativo, donde el foco está en la transformación de datos y no en la estructura de control que la rodea.

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un cierre o closure es el mecanismo por el cual una función lambda puede capturar y utilizar variables definidas en el contexto donde fue creada, incluso cuando la ejecución de la lambda ocurre en otro lugar. 

En otras palabras, la lambda no solo contiene su propio código, sino también una referencia a ciertas variables externas que estaban en alcance en el momento de su definición. Este concepto es fundamental en el paradigma funcional y permite escribir funciones más expresivas y contextuales.

En Java, una función lambda puede acceder a variables locales del método que la contiene, siempre que dichas variables sean efectivamente finales. Esto significa que su valor no cambia después de ser inicializado, aunque no se marque explícitamente como final. Esta restricción garantiza la seguridad y coherencia del modelo de ejecución, especialmente teniendo en cuenta la concurrencia y el modelo de memoria del lenguaje.

A continuación se modifica el ejemplo anterior añadiendo una nueva transformación que concatena una cadena adicional definida fuera de la lambda. La variable local sufijo pertenece al contexto del método main, pero la función lambda puede acceder a ella gracias al mecanismo de cierre:

import java.util.function.Function;

public class Ejemplo {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = " - PROCESADO";

        String resultado = transformar("Hola mundo", cadena ->
            cadena + sufijo
        );

        System.out.println(resultado);
    }
}

En este caso, la lambda utiliza la variable sufijo sin que forme parte de sus parámetros, demostrando cómo el cierre permite combinar datos externos con el comportamiento definido en la función. Este patrón resulta muy potente, ya que facilita la creación de funciones parametrizadas por su contexto, sin recurrir a atributos de clase ni a estructuras más complejas propias de la programación orientada a objetos tradicional.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
Una función lambda y un puntero a función en C comparten la idea básica de permitir que una función sea tratada como un valor y utilizada de forma indirecta, pero difieren de manera importante en su modelo conceptual y en sus capacidades. 

En C, un puntero a función es simplemente una dirección de memoria que apunta a una función concreta, sin información adicional sobre el contexto en el que se usa. Su comportamiento está limitado a la ejecución del código de la función, sin posibilidad de capturar estado externo.

Las funciones lambda, en cambio, están diseñadas como una abstracción de más alto nivel. No solo representan código ejecutable, sino que pueden ir acompañadas de un contexto capturado mediante cierres (closures), permitiendo acceder a variables locales del entorno donde se definieron.

Esta capacidad no existe en los punteros a funciones de C, donde cualquier dato adicional debe pasarse explícitamente como parámetro o gestionarse mediante estructuras globales o punteros adicionales.

Otra diferencia clave reside en la integración con el sistema de tipos. En C, los punteros a funciones dependen de una sintaxis compleja y frágil, y no ofrecen comprobaciones avanzadas más allá del tipo de la firma. En Java, las funciones lambda están estrictamente tipadas mediante interfaces funcionales, lo que permite una mayor seguridad en tiempo de compilación y una integración natural con otros mecanismos del lenguaje, como genéricos y colecciones.

En resumen, aunque ambos mecanismos permiten desacoplar la invocación de una función de su definición concreta, los punteros a funciones representan una solución de bajo nivel centrada en direcciones de memoria, mientras que las funciones lambda ofrecen una solución más expresiva, segura y flexible. Esta diferencia refleja la evolución hacia paradigmas más funcionales y declarativos en lenguajes modernos, sin abandonar completamente modelos más imperativos como los presentes en C.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Devolver funciones consiste en crear una función cuyo resultado es otra función, lo que permite generar comportamientos personalizados a partir de ciertos parámetros iniciales. En el contexto del paradigma funcional, este patrón es muy habitual, ya que facilita construir funciones más específicas a partir de otras más generales. En Java, esto es posible gracias al uso de funciones lambda y de interfaces funcionales como Function<T, R>.

En este caso, se define una función crearDescuento, que recibe un porcentaje y devuelve una función “descuento”. La función devuelta recibe una cantidad (Double) y aplica sobre ella el porcentaje indicado. El tipo de la función de descuento es Function<Double, Double>, lo que encaja perfectamente con una lambda que transforma un valor numérico en otro. A continuación se muestra el ejemplo completo junto con la creación y uso de dos descuentos distintos:

import java.util.function.Function;

public class EjemploDescuentos {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad * (1 - porcentaje / 100);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento25 = crearDescuento(25);

        double precio = 200.0;

        System.out.println(descuento10.apply(precio));
        System.out.println(descuento25.apply(precio));
    }
}

En este ejemplo, crearDescuento(10) y crearDescuento(25) generan dos funciones distintas, aunque se basan en el mismo código. Cada una recuerda el valor del porcentaje con el que fue creada, y lo utiliza más tarde cuando se aplica a una cantidad concreta. Esto permite reutilizar la lógica sin duplicar código y sin necesidad de clases adicionales.
Aquí entra en juego el concepto de closure. 

La función lambda que se devuelve captura la variable local porcentaje, definida en crearDescuento, y la conserva como parte de su contexto interno. Aunque la ejecución de crearDescuento ya ha terminado, la lambda sigue teniendo acceso a ese valor. Este comportamiento no existe en los punteros a funciones de C y es una de las razones por las que las funciones lambda resultan más expresivas y potentes en lenguajes modernos como Java.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
En Java, una interfaz funcional es una interfaz que representa un tipo de función, es decir, define un único comportamiento abstracto que puede ser implementado mediante una función lambda. Dado que Java es un lenguaje con comprobación estática de tipos, las funciones lambda no existen “sin tipo”, sino que siempre están asociadas a una interfaz funcional concreta. 

De este modo, una lambda puede verse como una implementación concisa del único método abstracto de dicha interfaz.
El requisito fundamental de una interfaz funcional es que contenga exactamente un método abstracto. Puede incluir otros métodos, pero solo si son métodos default o static, ya que estos no requieren ser implementados por la lambda. Esta restricción garantiza que no exista ambigüedad sobre qué método está implementando la función lambda, permitiendo al compilador inferir correctamente el comportamiento asociado.

Java proporciona la anotación @FunctionalInterface para marcar explícitamente una interfaz como funcional. Aunque su uso no es obligatorio, resulta recomendable, ya que permite al compilador comprobar que la interfaz cumple el requisito de tener un único método abstracto. Si se añade otro método abstracto por error, el compilador generará un error, ayudando a mantener la coherencia del diseño.

Interfaces como Function<T, R>, Predicate<T>, Consumer<T> o Supplier<T> son ejemplos clásicos de interfaces funcionales incluidas en la biblioteca estándar de Java. Gracias a este mecanismo, Java puede integrar programación funcional sin abandonar su base orientada a objetos, proporcionando un puente claro entre el tipado estático del lenguaje y la flexibilidad de las funciones lambda.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Una interfaz funcional definida a mano permite crear un tipo específico que represente una operación concreta, en este caso una transformación de una cadena de texto en otra. Conceptualmente, cumple el mismo papel que Function<String, String>, pero con un nombre más expresivo y alineado con el dominio del problema. Esto mejora la legibilidad del código y hace más explícita la intención de la función que se va a pasar o devolver.

Para que una interfaz pueda considerarse funcional, debe cumplir el requisito de tener un único método abstracto. Ese método define la firma de la función que posteriormente implementarán las funciones lambda. En este ejemplo, el método abstracto recibe un String y devuelve otro String, representando una transformación de texto. Se puede añadir la anotación @FunctionalInterface para reforzar esta restricción en tiempo de compilación.

La definición de la interfaz funcional Transformador quedaría de la siguiente forma:

@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}

Esta interfaz puede utilizarse directamente como tipo de referencia para funciones lambda, del mismo modo que se hacía con Function<String, String>, pero con una semántica más clara. De esta forma, Java combina su sistema de tipos estático con principios funcionales, permitiendo modelar el comportamiento como un contrato bien definido y explícito.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Una interfaz funcional genérica permite abstraer no solo el comportamiento, sino también los tipos de entrada y salida de la función. Mediante generics, la interfaz deja de estar ligada a un tipo concreto como String y puede reutilizarse para múltiples transformaciones distintas. 

Este enfoque resulta especialmente útil en lenguajes con tipado estático como Java, ya que combina flexibilidad con seguridad en tiempo de compilación.

Para conseguirlo, la interfaz Transformador puede declararse con dos parámetros genéricos: uno para el tipo de entrada y otro para el tipo de salida. El requisito de interfaz funcional se mantiene, es decir, debe existir un único método abstracto. La interfaz genérica podría definirse de la siguiente manera:

@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}

A partir de esta definición, es posible crear transformadores muy distintos sin cambiar la interfaz. Por ejemplo, un transformador que reciba un Double y devuelva un Integer, redondeando el valor, puede definirse mediante una función lambda claramente tipada:

public class Ejemplo {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear =
            valor -> (int) Math.round(valor);

        Integer resultado = redondear.transformar(3.6);
        System.out.println(resultado);
    }
}

Este ejemplo muestra cómo una única interfaz funcional genérica puede adaptarse a múltiples escenarios. Gracias al uso de generics, el compilador garantiza la coherencia de tipos, mientras que las funciones lambda permiten definir el comportamiento de forma concisa y expresiva, reforzando el carácter multiparadigma de Java.

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
En Java existe un conjunto de interfaces funcionales predefinidas que se encuentran principalmente en el paquete java.util.function. Estas interfaces cubren los casos de uso más habituales cuando se trabaja con funciones lambda, evitando la necesidad de definir interfaces propias para operaciones comunes. Todas ellas cumplen el requisito de tener un único método abstracto y están pensadas para encajar con el sistema de tipos estático del lenguaje.

Una de las más importantes es Function<T, R>, que representa una función que transforma un valor de tipo T en otro de tipo R. Muy relacionada con ella está UnaryOperator<T>, que es un caso particular donde el tipo de entrada y el de salida son el mismo. También existe BiFunction<T, U, R> para funciones que reciben dos parámetros y devuelven un resultado, junto con su variante BinaryOperator<T>, cuando ambos parámetros y el resultado comparten el mismo tipo.

Otra familia relevante es la de las funciones que no devuelven valor o no reciben parámetros. Consumer<T> representa una operación que recibe un valor y no devuelve resultado, mientras que BiConsumer<T, U> acepta dos parámetros. En el extremo opuesto se encuentra Supplier<T>, que no recibe ningún argumento y produce un valor. Estas interfaces son muy habituales en procesamiento de colecciones y en APIs basadas en eventos.

Por último, destacan las interfaces que representan condiciones lógicas, como Predicate<T>, que recibe un valor y devuelve un boolean, y BiPredicate<T, U>, que evalúa una condición sobre dos valores. Además, Java proporciona versiones especializadas para tipos primitivos (IntFunction, DoublePredicate, LongConsumer, etc.) con el objetivo de evitar el boxing y mejorar el rendimiento. 

En conjunto, estas interfaces explican por qué Java suele considerarse un lenguaje multi‑paradigma: gran parte de la programación funcional práctica puede realizarse sin crear nuevas interfaces, reutilizando las que ya ofrece la biblioteca estándar.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
El método forEach de List proporciona una forma funcional de recorrer una colección, sustituyendo al bucle for tradicional por la aplicación de una función a cada elemento. En lugar de controlar explícitamente el índice o el iterador, se delega el qué hacer con cada elemento a una función lambda, lo que encaja con la idea funcional de aplicar una operación sobre una colección de valores.

Desde el punto de vista conceptual, forEach recibe un Consumer<T>, es decir, una función que acepta un elemento de la lista y no devuelve ningún resultado. Esto lo diferencia de otros usos funcionales como map o filter, pero lo convierte en una alternativa clara al bucle for cuando el objetivo es realizar una acción (por ejemplo, mostrar información o registrar datos) para cada elemento. El control del recorrido queda en manos de la colección, no del programador.

En el siguiente ejemplo, se recorre una lista de Integer y se muestra un mensaje únicamente cuando el número es positivo. La condición se expresa dentro de la función lambda pasada a forEach, manteniendo el código compacto y expresivo:

import java.util.Arrays;
import java.util.List;

public class Ejemplo {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(3, -2, 0, 7, -5);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("Número positivo: " + n);
            }
        });
    }
}

Este estilo evita la estructura repetitiva del bucle clásico y pone el foco en la lógica que se desea aplicar a cada elemento. Para quien proviene del for imperativo de C o de Java clásico, forEach representa un primer paso claro hacia un uso más funcional de las colecciones, sin abandonar la seguridad del tipado estático ni el modelo orientado a objetos de Java.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
En la firma de List.forEach, se utiliza Consumer<? super T> en lugar de Consumer<T> para aumentar la flexibilidad del método sin perder seguridad de tipos. El método forEach solo consume elementos de la lista (los recibe como parámetro), pero no produce valores de tipo T. 

Al permitir un Consumer de un supertipo de T, se posibilita pasar funciones que acepten no solo T, sino también cualquier tipo más general, como Object, lo que amplía los casos en los que el método puede reutilizarse.

Esta decisión se basa en el principio conocido como PECS, siglas de Producer Extends, Consumer Super. Este principio indica que, cuando una estructura produce valores de un tipo (get), se debe usar ? extends T, y cuando consume valores (set, accept), se debe usar ? super T. En el caso de forEach, la lista produce elementos de tipo T, pero la función pasada como argumento los consume, por lo que el uso correcto es Consumer<? super T>.

Aplicando este principio al método transformar, se puede mejorar su definición genérica. Si el método recibe un valor de tipo T y usa una función para transformarlo en R, la función consume un T y produce un R. Por tanto, una firma más flexible sería permitir que la función acepte un supertipo de T y devuelva un subtipo de R:

public static <T, R> R transformar(
    T valor,
    Function<? super
)

Esta versión respeta PECS y permite reutilizar el método con funciones más generales sin comprometer el tipado estático. De este modo, se aprovecha al máximo la genericidad de Java, alineando la programación funcional con los principios del sistema de tipos del lenguaje.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las referencias a métodos permiten tratar métodos ya existentes como funciones, sin ejecutarlos en el momento de la referencia. En lugar de invocar directamente el método, se obtiene una referencia que puede almacenarse en una variable y ejecutarse más tarde. 

Este mecanismo se apoya en las mismas ideas que las funciones lambda: tratar el comportamiento como un valor. La diferencia principal es que, en lugar de definir una función nueva, se reutiliza un método ya definido en una clase u objeto.

En JavaScript, los métodos de los objetos son funciones y, por tanto, pueden referenciarse directamente. Al obtener la referencia a un método, es importante tener en cuenta el contexto (this), ya que puede perderse si no se gestiona adecuadamente. En el ejemplo siguiente, se define una clase Persona con un método saludar, se crea un objeto y se guarda una referencia a su método ya ligada al objeto:

class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const persona = new Persona("Adrián");

// Referencia al método, manteniendo el contexto con bind
const saludo = persona.saludar.bind(persona);

// Invocación mediante la referencia
saludo();

En Java, las referencias a métodos están integradas en el lenguaje a partir de Java 8 y se expresan mediante el operador ::. Una referencia a método de instancia queda asociada a un objeto concreto, por lo que no se pierde el contexto. El método referenciado debe ser compatible con el tipo de una interfaz funcional, como Runnable o Consumer. En este caso, saludar no recibe parámetros ni devuelve valor, por lo que encaja con Runnable:

public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Ejemplo {
    public static void main(String[] args) {
        Persona persona = new Persona("Adrián");

        // Referencia al método de instancia
        Runnable saludo = persona::saludar;

        // Invocación mediante la referencia
        saludo.run();
    }
}

Estos ejemplos muestran cómo tanto JavaScript como Java permiten reutilizar métodos existentes como valores funcionales. En JavaScript esto se apoya en la naturaleza dinámica del lenguaje, mientras que en Java se integra de forma segura mediante interfaces funcionales y el sistema de tipos estático.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
En Java existen cuatro tipos principales de referencias a método, todas ellas introducidas en Java 8 como una forma compacta y legible de reutilizar métodos existentes allí donde se espera una interfaz funcional. Las referencias a métodos no ejecutan el método en el momento de declararse, sino que crean una referencia compatible con el método abstracto de la interfaz funcional asociada. Sintácticamente se utilizan mediante el operador ::.

El primer tipo es la referencia a un método estático, que apunta a un método de clase sin necesidad de crear instancias. Se utiliza cuando el método estático es compatible con la firma de la interfaz funcional. Por ejemplo, una referencia a Integer.parseInt puede usarse como una función que transforma un String en un Integer:

Function<String, Integer> aEntero = Integer::parseInt;
Integer valor = aEntero.apply("42");

Otro tipo es la referencia a un método de instancia de un objeto concreto, donde la referencia queda ligada a una instancia específica. Este caso es habitual cuando el método no recibe parámetros y actúa sobre el estado interno del objeto. Por ejemplo, una referencia a saludar de una instancia persona:

Persona persona = new Persona("Adrián");
Runnable saludo = persona::saludar;
saludo.run();

También existe la referencia a un método de instancia sobre cualquier instancia de una clase. En este caso, el objeto sobre el que se invoca el método se pasa implícitamente como primer parámetro. Este tipo se usa especialmente con colecciones y operaciones funcionales:

Function<String, Integer> longitud = String::length;
Integer size = longitud.apply("Hola");

Por último, se encuentran las referencias a constructores, que permiten tratar un constructor como una función que crea objetos. Dependiendo de la firma del constructor, se usan interfaces funcionales como Supplier o Function. Por ejemplo, una referencia al constructor de Persona que recibe un String:

Function<String, Persona> crearPersona = Persona::new;
Persona p = crearPersona.apply("Adrián");

Estos cuatro tipos cubren la mayoría de los escenarios habituales y permiten escribir código más expresivo y reutilizable. En conjunto, las referencias a métodos refuerzan el carácter funcional y multiparadigma de Java, integrándose de forma natural con lambdas e interfaces funcionales.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
La ordenación de colecciones es un caso muy representativo del uso de expresiones lambda en Java, ya que permite definir el criterio de comparación de forma concisa y localizada. 

El método Collections.sort recibe como segundo parámetro un Comparator<T>, que define cómo comparar dos elementos de la colección. Al usar lambdas, se evita la necesidad de clases anónimas y se hace más legible la lógica de ordenación.

En primer lugar, se muestra una versión en la que la función de comparación se implementa manualmente dentro de la expresión lambda. En este enfoque se comparan explícitamente las edades de las dos personas y, en caso de empate, se compara el nombre usando el orden alfabético. Esta forma resulta clara para entender el proceso paso a paso y se asemeja a un estilo imperativo tradicional, aunque expresado de forma funcional:

import java.util.*;

public class EjemploManual {

    static class Persona {
        String nombre;
        int edad;

        Persona(String nombre, int edad) {
            this.nombre = nombre;
            this.edad = edad;
        }
    }

    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Carlos", 30),
            new Persona("Ana", 25),
            new Persona("Beatriz", 30),
            new Persona("David", 25)
        );

        Collections.sort(personas, (p1, p2) -> {
            if (p1.edad != p2.edad) {
                return p1.edad - p2.edad;
            }
            return p1.nombre.compareTo(p2.nombre);
        });
    }
}
``

En una segunda versión, se emplean métodos auxiliares de Comparator, lo que permite expresar el mismo criterio de forma más declarativa y compacta. Este estilo es más idiomático en Java moderno, ya que aprovecha la composición de comparadores y reduce la cantidad de lógica explícita escrita por el programador:

import java.util.*;

public class EjemploComparator {

    static class Persona {
        String nombre;
        int edad;

        Persona(String nombre, int edad) {
            this.nombre = nombre;
            this.edad = edad;
        }
    }

    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Carlos", 30),
            new Persona("Ana", 25),
            new Persona("Beatriz", 30),
            new Persona("David", 25)
        );

        Collections.sort(
            personas,
            Comparator.comparingInt((Persona p) -> p.edad)
                      .thenComparing(p -> p.nombre)
        );
    }
}

Ambas versiones producen exactamente el mismo resultado, pero la segunda enfatiza el qué se quiere hacer más que el cómo. Este ejemplo ilustra claramente cómo el enfoque funcional en Java mejora la expresividad del código sin abandonar el tipado estático ni las estructuras propias de la programación orientada a objetos.