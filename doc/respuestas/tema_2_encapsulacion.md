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

### Respuesta
La encapsulación en POO busca agrupar dentro de una misma entidad (la clase) tanto los datos como las operaciones que actúan sobre ellos. El objetivo es que el estado interno del objeto quede protegido frente a accesos o modificaciones indebidas desde el exterior. La ocultación de información, que es una consecuencia directa de la encapsulación, pretende impedir que otros componentes del programa dependan de detalles internos de la implementación, exponiendo solo aquello que realmente se necesita para utilizar el objeto.
Al ocultar información, se consigue que el objeto controle cómo se accede y modifica su estado, normalmente mediante métodos públicos como getters y setters. Esto evita que el resto del programa pueda alterar directamente los atributos internos, reduciendo errores y comportamientos inesperados. Además, este enfoque permite garantizar que cualquier modificación del estado se realice de forma coherente y verificada por la propia clase.
Otra ventaja importante de la ocultación es que facilita el mantenimiento y la evolución del código. Si la implementación interna de una clase cambia, pero su interfaz pública se mantiene, el resto del programa no necesita ser modificado. Esto permite trabajar con mayor modularidad, favorece la reutilización de código y reduce el acoplamiento entre componentes. Como resultado, los programas se vuelven más robustos, más fáciles de depurar y más simples de extender en el futuro.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La interfaz pública de un objeto o clase se entiende como el conjunto de métodos y miembros accesibles desde fuera de la propia clase. Representa aquello que otros componentes del programa pueden utilizar para interactuar con el objeto, sin necesidad de conocer cómo están implementados sus detalles internos. En Java, esta interfaz suele estar formada por métodos públicos, como constructores, getters, setters y otras operaciones que permitan utilizar la clase de forma controlada.
Esta idea se relaciona directamente con la ocultación de información, ya que la interfaz pública actúa como una frontera clara entre lo que se desea exponer y lo que debe permanecer oculto en el interior de la clase. Mientras los atributos y métodos internos se mantienen privados o con acceso restringido, la interfaz pública ofrece solo lo imprescindible para operar con el objeto. De esta manera, se garantiza que el estado interno no sea modificado de manera incontrolada.
Además, la interfaz pública favorece la independencia entre las distintas partes del programa. Otro componente del sistema puede utilizar una clase sin preocuparse por su implementación, siempre que respete los métodos expuestos públicamente. Esto permite que la lógica interna pueda cambiar sin afectar a quienes utilizan la clase, reforzando así la modularidad y el mantenimiento del código.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La interfaz pública de una clase debe diseñarse con cuidado porque actúa como el único punto de contacto entre la clase y el resto del programa. Una interfaz mal planteada puede permitir accesos innecesarios al estado interno, generar dependencias excesivas o dificultar la evolución futura del código. Por este motivo, es importante ofrecer solo los métodos realmente necesarios, manteniendo el resto de elementos con niveles de acceso más restrictivos. La claridad y coherencia de esta interfaz determinarán en gran medida lo fácil que será usar la clase correctamente y evitar errores.
Además, cambiar la interfaz pública no suele ser sencillo, especialmente cuando la clase ya es utilizada en varias partes del programa. Cualquier modificación en los métodos públicos —como cambiar su firma o eliminar algunos de ellos— obliga a revisar y actualizar todos los lugares donde se utilizan. Esto aumenta el riesgo de introducir fallos y complica el mantenimiento. En proyectos grandes o en bibliotecas compartidas, alterar la interfaz pública puede tener un impacto aún mayor, ya que otros desarrolladores podrían depender de su comportamiento original.
Por ello, se considera una buena práctica pensar en la interfaz pública como un “contrato” estable entre la clase y el exterior. Mientras la implementación interna puede evolucionar libremente, mantener estable la interfaz pública asegura que no se rompa la compatibilidad con el resto del sistema. Diseñar de forma cuidadosa esta interfaz ayuda a garantizar que la clase sea fácil de usar, flexible y lo más independiente posible de los detalles internos.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las invariantes de clase son condiciones lógicas que siempre deben cumplirse para que un objeto se considere válido durante toda su vida útil. Suelen estar relacionadas con restricciones del estado interno, como que un atributo no pueda ser negativo, que una colección no contenga valores inválidos o que dos propiedades mantengan siempre una relación coherente entre sí. Estas invariantes deben cumplirse después de construir el objeto y tras la ejecución de cualquier método público que pueda modificar su estado.
La ocultación de información ayuda de forma decisiva a mantener estas invariantes, porque impide que el estado interno pueda ser modificado directamente desde fuera de la clase. Al obligar a que todas las modificaciones pasen a través de métodos controlados, la propia clase puede verificar que se cumplen las condiciones necesarias antes de aceptar cualquier cambio. Esto permite introducir comprobaciones, validaciones o correcciones automáticas sin que el resto del programa tenga acceso directo a los atributos.
Además, al proteger los datos internos, se reduce el riesgo de que el programador que usa la clase rompa accidentalmente las invariantes por desconocimiento o descuido. De este modo, las restricciones importantes quedan garantizadas por la propia clase, lo que contribuye a que los objetos mantengan siempre un estado coherente y a que el programa sea más robusto y fiable.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
A continuación se muestra una posible implementación de una clase Punto en Java que encapsula sus datos (x e y) con acceso privado y expone una interfaz pública mínima y coherente. Se incluyen un constructor, métodos de acceso (getters) y el método solicitado calcularDistanciaAOrigen. De este modo, cualquier modificación futura del almacenamiento interno (por ejemplo, cambiar double por otra representación) no afectaría a quienes usen la clase, siempre que la interfaz pública se mantenga.

public class Punto {
    // Estado interno oculto
    private double x;
    private double y;

    // Constructor (parte de la interfaz pública)
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        // Aquí podrían añadirse validaciones si hubiera invariantes
    }

    // Getters (parte de la interfaz pública)
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    // Setters opcionales (exponerlos o no depende del diseño)
    public void setX(double x) {
        this.x = x;
    }

    public void setY(double y) {
        this.y = y;
    }

    // Operación pública que no expone detalles internos
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

La interfaz pública de la clase Punto está formada por aquello que puede usarse desde fuera para interactuar con instancias: el constructor público Punto(double, double), los métodos de acceso getX(), getY(), los métodos de modificación setX(double), setY(double) (si se decide exponerlos) y el método de operación calcularDistanciaAOrigen(). Todo lo demás —en este caso, los atributos x e y— permanece oculto y no es accesible directamente desde el exterior. Si se quisiese un diseño más restrictivo (por ejemplo, objetos inmutables), podría omitirse los setters y mantener solo constructor y getters.
En cuanto a los modificadores de acceso, **public** indica que el miembro (clase, constructor o método) es accesible desde cualquier otro código que tenga visibilidad del tipo. Por el contrario, **private** restringe el acceso solo al interior de la propia clase, impidiendo que código externo lea o modifique directamente esos miembros. Esta separación permite aplicar ocultación de información: el estado se protege (private) y las operaciones seguras para el exterior se exponen a través de una interfaz controlada (public), asegurando así un uso coherente y predecible del objeto.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, los modificadores de acceso public y private pueden aplicarse tanto a clases (solo en ciertos casos) como a miembros internos de una clase, es decir, atributos, métodos y constructores. Estos modificadores permiten controlar la visibilidad de cada elemento, definiendo quién puede acceder desde fuera y quién no. En consecuencia, constituyen una herramienta fundamental para aplicar encapsulación y ocultación de información.
En el caso de las clases, únicamente las clases de nivel superior pueden ser public o tener el modificador por defecto (sin palabra clave, también llamado package‑private). No es posible declarar una clase de nivel superior como private, ya que debe ser accesible desde al menos el mismo paquete. Sin embargo, las clases internas (clases dentro de otra clase) sí pueden ser private, lo que permite ocultarlas completamente del exterior cuando solo deben ser utilizadas por la clase que las contiene.
Por otro lado, los atributos y métodos pueden declararse como public o private sin restricciones. Normalmente los atributos se marcan como private para proteger el estado interno, mientras que los métodos que componen la interfaz pública se marcan como public. También los constructores pueden usar estos modificadores: un constructor public permite crear instancias desde cualquier lugar, mientras que un constructor private limita esa posibilidad, siendo útil en patrones como singleton o en factorías internas.
En conjunto, estos modificadores permiten diseñar con precisión qué partes de una clase quedan expuestas y cuáles permanecen ocultas, reforzando la encapsulación y evitando dependencias indeseadas entre componentes.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
En POO, además de la visibilidad pública y privada, suelen existir niveles intermedios que permiten controlar con más precisión quién puede acceder a determinados elementos. Estos niveles adicionales suelen usarse para facilitar la colaboración entre clases relacionadas sin exponer detalles internos a toda la aplicación. Aunque el número y nombre exacto de los niveles varía según el lenguaje, la idea común es disponer de un gradiente de accesibilidad más rico que el simple “todo el mundo” o “solo yo”.
En Java, además de public y private, existen otros dos niveles de visibilidad: protected y el modificador por defecto (llamado package‑private). El nivel protected permite el acceso desde clases del mismo paquete y también desde subclases situadas en cualquier otro paquete. Por otro lado, el acceso por defecto (sin poner ningún modificador) limita el uso a clases del mismo paquete, lo que ofrece una forma de agrupar funcionalidades relacionadas sin exponerlas al exterior. De esta forma, Java proporciona cuatro niveles claros de visibilidad que permiten modularizar código de manera gradual.
En otros lenguajes orientados a objetos también existen diferentes aproximaciones. Por ejemplo, C++ —que sí conoces desde la perspectiva no orientada a objetos— incluye los niveles public, private y protected, pero su comportamiento difiere ligeramente del de Java, ya que no existe el concepto de paquete y parte del control se basa en herencia. En lenguajes modernos como C# existen incluso más niveles, como internal o protected internal, pensados para controlar la accesibilidad a nivel de ensamblado o combinando herencia y ámbito interno del proyecto. Estas diferencias reflejan que la visibilidad es un mecanismo fundamental de encapsulación, pero cada lenguaje lo adapta a su propio modelo de organización del código.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
Los miembros de instancia privados están ocultos para otras clases (a), pero no están ocultos entre instancias de la misma clase (b). En Java, el modificador private restringe el acceso al ámbito de la clase, no al de la instancia; esto significa que cualquier método de la clase puede acceder a los campos privados de cualquier objeto de esa misma clase, incluido el que se recibe como parámetro. Lo que sí queda prohibido es acceder a esos campos privados desde fuera de la clase, aunque sea desde otra clase del mismo paquete o desde una subclase.
En el siguiente ejemplo, el método calcularDistanciaAPunto(Punto otro) accede directamente a otro.x y otro.y, a pesar de que x e y son private. Es válido porque el código que intenta leerlos está definido dentro de la clase Punto. En cambio, desde cualquier otra clase (por ejemplo, Main), no sería posible escribir p1.x o p2.y; habría que usar la interfaz pública (getters u otros métodos).

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Ejemplo: acceso a campos privados de "otro" porque es de la misma clase
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x; // válido: se está dentro de la clase Punto
        double dy = this.y - otro.y; // válido: igual que arriba
        return Math.sqrt(dx * dx + dy * dy);
    }
}

// Desde fuera de la clase (otra clase), lo siguiente NO es válido:
// Punto a = new Punto(1, 2);
// Punto b = new Punto(4, 6);
// double dx = a.x - b.x; // ERROR: x es private en Punto

En resumen, private protege los miembros frente a otras clases, pero no impide la cooperación entre instancias de la misma clase a través de sus propios métodos. Esta regla facilita implementar operaciones que requieren comparar o combinar estados internos (como distancias, igualdad estructural o copias profundas) sin romper la ocultación frente al exterior.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos getter y setter son operaciones públicas utilizadas para acceder y modificar, respectivamente, los atributos privados de un objeto. En la mayoría de lenguajes orientados a objetos, incluyendo Java, se emplean como parte de la interfaz pública de una clase con el fin de proteger el estado interno, evitando accesos directos desde fuera. Un getter devuelve el valor actual de un atributo, mientras que un setter permite actualizarlo siguiendo las reglas que la propia clase establezca.
Estos métodos resultan fundamentales para mantener la encapsulación, ya que permiten controlar cómo se leen y modifican los datos internos. Un getter puede devolver valores calculados o adaptados, en lugar de exponer directamente el contenido del atributo. Por otro lado, un setter puede incluir validaciones, comprobaciones de invariantes o restricciones que impidan asignar valores incorrectos. Así, la clase conserva el control sobre su propio estado sin exponer campos directamente.
El uso de getters y setters también facilita la evolución del código. Aunque internamente cambie la representación de un atributo, la interfaz pública puede mantenerse intacta si los métodos siguen cumpliendo el mismo propósito. Esto reduce el acoplamiento con otras partes del programa y evita que los usuarios de la clase dependan de detalles de implementación. En consecuencia, estos métodos se consideran una herramienta clave para crear clases robustas, seguras y fáciles de mantener.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
No, cuando se afirma que la ocultación de información mejora la seguridad de un programa en el contexto de la POO, no se está hablando de evitar que sea hackeado en el sentido tradicional de la seguridad informática. El término se refiere a una seguridad lógica interna del diseño, destinada a evitar que partes del programa accedan o modifiquen datos que no deberían manipular directamente. Es decir, protege la integridad del estado interno de los objetos frente a errores de programación, usos indebidos o dependencias innecesarias entre módulos.
La idea principal es que, al limitar el acceso a los detalles internos de una clase, se evita que otros componentes dependan de esos detalles y los manipulen de forma errónea. Esto reduce la probabilidad de inconsistencias, datos corruptos o fallos difíciles de depurar. La clase se convierte en la única responsable de gestionar su propio estado, aplicando validaciones y manteniendo sus invariantes siempre que se use su interfaz pública.
Por tanto, la mejora de “seguridad” en este contexto está ligada a la robustez y fiabilidad del software, no a la protección frente a ataques externos. Aunque ambos conceptos comparten la idea de “proteger algo”, la ocultación de información pertenece al ámbito del diseño orientado a objetos, y no al de la ciberseguridad. En resumen: la encapsulación no evita hackeos, pero sí ayuda a construir programas más consistentes, mantenibles y menos propensos a fallos internos.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un miembro de instancia es un atributo o método cuyo valor pertenece a cada objeto individual creado a partir de una clase. Esto significa que cada instancia mantiene su propio estado independiente: cada objeto tiene sus propias copias de los atributos de instancia, y los métodos de instancia operan sobre ese estado concreto. En términos de memoria, se crea una copia de esos miembros para cada objeto, lo que permite que cada instancia almacene valores diferentes y se comporte de manera autónoma.
Por el contrario, un miembro de clase (marcado con la palabra clave static en Java) pertenece a la clase en sí misma y no a cada objeto individual. Solo existe una única copia de ese atributo o método, compartida por todas las instancias. Esto es útil para datos globales a todos los objetos, como contadores, constantes o funciones que no dependen del estado específico de un objeto. Los métodos estáticos tampoco pueden acceder directamente al estado de instancia, ya que no requieren un objeto concreto para ser usados.
Respecto a la ocultación de información, los miembros de clase también pueden protegerse mediante modificadores de acceso como private, public, protected o el acceso por defecto. De este modo, es posible disponer de atributos estáticos privados (por ejemplo, un contador interno no expuesto) y métodos estáticos públicos que sí formen parte de la interfaz externa. La encapsulación se aplica por igual tanto a miembros de instancia como a miembros de clase, puesto que el objetivo sigue siendo controlar quién puede usar o modificar ciertos elementos.
Esta capacidad de ocultar miembros estáticos resulta especialmente útil cuando se desea mantener datos compartidos sin permitir modificaciones desde el exterior, o cuando se implementan patrones de diseño como singleton o utilidades internas. En cualquier caso, el uso de static no afecta al mecanismo de visibilidad: un miembro estático sigue pudiendo ser ocultado completamente si se declara como private.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Sí, tiene sentido que los constructores sean privados en ciertos diseños específicos, aunque no es lo habitual en la mayoría de clases. Un constructor privado impide que se puedan crear instancias directamente desde fuera de la propia clase, lo que obliga a controlar estrictamente cómo y cuándo se generan los objetos. Esto puede resultar útil cuando la creación de instancias requiere restricciones especiales, validaciones internas o un número limitado y predefinido de objetos.
Un uso frecuente aparece en el patrón Singleton, donde se desea que solo exista una única instancia de la clase en todo el programa. Al hacer el constructor privado y proporcionar un método estático que devuelve siempre la misma instancia, se evita la creación accidental de más objetos. Otro caso habitual son las clases de utilidades, que solo contienen métodos estáticos y no deben poder instanciarse; un constructor privado vacío evita que otros programadores creen objetos innecesarios.
También se emplean constructores privados cuando se desea que las instancias solo puedan ser creadas mediante métodos factoría estáticos. Estos métodos pueden devolver distintas implementaciones, aplicar condiciones adicionales o gestionar la reutilización de objetos. En estos escenarios, el constructor privado ofrece un mayor control sobre el proceso de creación, garantizando que la clase mantenga sus invariantes desde el primer momento y se utilice de manera coherente.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
En Java, los miembros de clase se indican con el modificador static. Pertenecen a la clase y no a cada objeto concreto: existe una sola copia compartida por todas las instancias. Por ese motivo, se accede a ellos preferentemente mediante el nombre de la clase (por ejemplo, Punto.getMaxX()), y los métodos estáticos no pueden usar directamente miembros de instancia (this) porque no están ligados a un objeto específico.
A continuación se muestra una versión de Punto que incorpora dos atributos estáticos para registrar los máximos globales de x e y establecidos en todas las instancias creadas o modificadas hasta el momento. Estos atributos se mantienen privados y se exponen a través de getters estáticos públicos que forman parte de la interfaz. La actualización de los máximos se realiza en el constructor y en los setters, de modo que cualquier cambio de coordenadas queda reflejado.

public class Punto {
    // --- Estado de instancia (oculto) ---
    private double x;
    private double y;

    // --- Estado de clase (compartido por todas las instancias) ---
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    // Getters de instancia
    public double getX() { return x; }
    public double getY() { return y; }

    // Setters de instancia (si se desean objetos mutables)
    public void setX(double x) {
        this.x = x;
        actualizarMaximos(this.x, this.y);
    }

    public void setY(double y) {
        this.y = y;
        actualizarMaximos(this.x, this.y);
    }

    // Operaciones de instancia
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    // --- Interfaz pública de clase (miembros estáticos) ---
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // Método auxiliar privado de clase (no forma parte de la interfaz externa)
    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    // (Opcional) Reinicializar máximos, útil para tests
    public static void reiniciarMaximos() {
        maxX = Double.NEGATIVE_INFINITY;
        maxY = Double.NEGATIVE_INFINITY;
    }
}

En este diseño, maxX y maxY son miembros de clase (static) y están ocultos (private); se exponen únicamente mediante getMaxX() y getMaxY(). De este modo, se preserva la encapsulación: el resto del código puede consultar los máximos globales sin poder modificarlos arbitrariamente. En contextos concurrentes, tendría sentido considerar sincronización (por ejemplo, synchronized o AtomicDouble/acumuladores) alrededor de la actualización de máximos, pero en escenarios secuenciales este enfoque es suficiente y claro para ilustrar el uso de miembros de clase.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
Aquí tienes un ejemplo de método factoría dentro de la clase Punto que recibe dos double, los redondea al entero más cercano y devuelve un nuevo objeto Punto.
Sí, se utiliza static, porque un método factoría pertenece a la clase, no a una instancia concreta.

public static Punto crearRedondeado(double x, double y) {
    long rx = Math.round(x);
    long ry = Math.round(y);
    return new Punto(rx, ry);
}

Si se prefiere mantener todo como double:

public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
A continuación se muestra una versión de Punto cuya representación interna cambia de dos double a un array privado de dos posiciones, sin alterar la interfaz pública (constructor, getters, setters y métodos de operación). Este cambio ilustra el beneficio de la encapsulación: se puede modificar la implementación interna sin que el código cliente tenga que cambiar, siempre que se mantengan los mismos nombres, firmas y comportamiento de los métodos públicos.
Para evitar “números mágicos” al indexar el array, se definen constantes privadas IDX_X y IDX_Y. El array se declara final para que la referencia no pueda reasignarse (aunque sus elementos sigan siendo mutables), manteniendo la misma semántica externa que en la versión anterior. Toda la lógica pública se mantiene igual, por lo que el uso desde fuera de la clase no se ve afectado.

public class Punto {
    // --- Representación interna: array oculto ---
    private static final int IDX_X = 0;
    private static final int IDX_Y = 1;
    private final double[] coord; // referencia inmutable; contenido mutable

    // Constructor (misma firma pública)
    public Punto(double x, double y) {
        this.coord = new double[2];
        this.coord[IDX_X] = x;
        this.coord[IDX_Y] = y;
    }

    // Getters (misma interfaz pública)
    public double getX() { return coord[IDX_X]; }
    public double getY() { return coord[IDX_Y]; }

    // Setters (misma interfaz pública; mantenerlos o no depende del diseño)
    public void setX(double x) { coord[IDX_X] = x; }
    public void setY(double y) { coord[IDX_Y] = y; }

    // Operaciones públicas (mismos nombres y firmas)
    public double calcularDistanciaAOrigen() {
        double x = coord[IDX_X];
        double y = coord[IDX_Y];
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coord[IDX_X] - otro.coord[IDX_X];
        double dy = this.coord[IDX_Y] - otro.coord[IDX_Y];
        return Math.sqrt(dx * dx + dy * dy);
    }
}

Con esta modificación, se demuestra cómo la ocultación de información permite cambiar la estructura de datos interna (dos double → un array) sin romper el contrato externo. Como contrapartida, usar índices puede restar legibilidad frente a campos con nombre; por eso se recomiendan constantes para documentar la posición de cada coordenada y evitar errores de mantenimiento.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
La idea de declarar un atributo público suele parecer tentadora si ya existe un getter y un setter públicos, pero en realidad va en contra del principio de encapsulación. Aunque un getter y un setter permitan acceder y modificar el atributo, la clave está en que el acceso se realiza de forma controlada. Si el atributo fuese público, cualquier parte del programa podría modificarlo sin restricciones, sin validaciones y sin respetar las reglas internas establecidas por la clase. Con métodos de acceso, en cambio, la clase conserva el control y puede aplicar comprobaciones, transformaciones o mantener invariantes.
La convención más habitual y ampliamente recomendada en POO —y muy especialmente en Java— es declarar todos los atributos como privados (private). Esta práctica aparece en prácticamente todas las guías de estilo formales del lenguaje y de la industria, ya que evita la exposición accidental de detalles internos y permite cambiar la representación interna sin afectar a quienes usan la clase. Aunque algunos lenguajes modernos ofrecen atajos como propiedades, el principio subyacente sigue siendo el mismo: el estado interno debe estar protegido y gestionado desde dentro de la propia clase.
Esta decisión de mantener los atributos privados está directamente relacionada con las invariantes de clase. Las invariantes son reglas que deben cumplirse siempre para que el objeto esté en un estado válido; por ejemplo, que una coordenada no sea negativa o que dos valores mantengan una relación lógica determinada. Si los atributos fuesen públicos, cualquier código externo podría romper esas reglas sin que la clase pueda intervenir. Con setters, en cambio, la clase puede verificar cada cambio y asegurar que las invariantes se mantienen en todo momento.
Por tanto, incluso cuando existen getters y setters, los atributos deben seguir siendo privados. La encapsulación no consiste solo en ocultar el estado, sino en controlar cómo se accede a él. El uso de atributos públicos rompe este control y expone la clase a inconsistencias, dificultando la evolución del código y la preservación de sus reglas internas.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase inmutable es aquella cuyo estado interno no puede cambiar después de haber sido creada. Esto significa que todos sus atributos permanecen constantes tras el constructor y no existen métodos que alteren su contenido. Para lograr esta propiedad, se suelen usar atributos privados y finales, y no se proporcionan setters ni métodos que modifiquen las estructuras internas. La inmutabilidad garantiza que cada instancia represente un valor fijo y consistente durante toda su vida útil.
Un método modificador es cualquier método que cambia el estado interno del objeto. Esto puede incluir setters, pero también operaciones que alteren colecciones internas, actualicen contadores o modifiquen atributos indirectamente. Por tanto, un método modificador no es siempre un setter: puede existir un método que modifique el estado sin tener el nombre típico de un setter ni actuar sobre un único atributo. En una clase inmutable, simplemente no existen este tipo de métodos.
La inmutabilidad presenta varias ventajas importantes. En primer lugar, hace que los objetos sean más seguros desde el punto de vista lógico, porque no pueden cambiar de forma inesperada, reduciendo errores difíciles de rastrear. También facilita enormemente el razonamiento sobre el código, dado que un objeto inmutable puede compartirse sin riesgo entre métodos o hilos de ejecución. Esta característica mejora también la programación concurrente, ya que no se requieren mecanismos de sincronización para proteger un objeto cuyo estado nunca varía.
Además, la inmutabilidad encaja bien con el principio de las invariantes de clase, ya que estas solo deben garantizarse en el momento de construcción. Dado que el objeto no podrá cambiar después, no existe el riesgo de que el estado deje de cumplir las reglas que lo hacen válido. Por estas razones, aunque no siempre sea la mejor opción para todos los diseños, las clases inmutables son una herramienta poderosa para obtener programas más robustos, predecibles y fáciles de mantener.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No, no es recomendable incluir métodos setter siempre ni como una convención automática. La decisión de proporcionar un setter debe basarse en si realmente tiene sentido permitir que el atributo correspondiente pueda cambiar después de creada la instancia. Incluir setters por defecto convierte la clase en totalmente mutable, incluso cuando quizá su diseño no lo requiera. Esto va en contra del principio de encapsulación, ya que se abre la posibilidad de que el estado interno se modifique de maneras no deseadas o en momentos inapropiados.
Además, añadir setters sin necesidad suele generar un diseño más débil: cualquier parte del programa podría alterar atributos libremente, lo que incrementa el riesgo de inconsistencias y dificulta mantener las invariantes de clase. Si la clase necesita controlar estrictamente su estado interno —por ejemplo, verificando límites, relaciones entre atributos o manteniendo coherencia lógica—, conviene evitar setters y realizar las validaciones en el constructor o en métodos específicos preparados para ello.
En muchos casos, es preferible que las clases sean inmutables, o al menos parcialmente inmutables, evitando que algunos atributos cambien después de la construcción. Incluso en clases mutables, la modificación suele centralizarse en métodos más específicos que expresen una intención clara (por ejemplo, mover(dx, dy) en un punto), en lugar de exponer setters directos que permitan cambios arbitrarios. De este modo, el diseño se vuelve más claro, robusto y fácil de mantener.
Por tanto, la convención más aceptada en la programación orientada a objetos —especialmente en Java— es que los atributos deben ser privados y que los setters solo deben añadirse cuando exista una verdadera necesidad de modificación controlada. La interfaz pública debe diseñarse pensando en las operaciones que tiene sentido permitir, no en exponer el estado sin restricciones.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
La clase String en Java es inmutable, lo que significa que una vez creada una cadena, su contenido no puede modificarse. Cada operación que parezca alterar una cadena, como asignar un nuevo valor o concatenarla con otra, en realidad crea un nuevo objeto String. Este comportamiento garantiza que las cadenas sean seguras para compartir, predecibles y libres de efectos secundarios, pero también puede implicar un coste en rendimiento cuando se realizan muchas modificaciones sucesivas.
Cuando se concatenan dos cadenas, Java no modifica ninguna de las originales, sino que crea una nueva cadena que contiene el resultado. Por ejemplo, la expresión "hola" + "mundo" crea internamente un objeto nuevo. Si este proceso se repite dentro de un bucle o en construcciones que concatenan numerosos fragmentos, se generan múltiples objetos intermedios innecesarios, lo que provoca un mayor uso de memoria y un coste extra en tiempo de ejecución.
Para casos en los que se necesita construir una cadena muy larga de forma incremental, con muchas concatenaciones, se recomienda utilizar las clases StringBuilder (o StringBuffer si se requiere sincronización). StringBuilder es mutable y permite modificar el contenido sin crear objetos adicionales en cada operación. Esto reduce el coste de memoria y mejora el rendimiento, especialmente en bucles o generadores de cadenas extensas.
En resumen, String es inmutable y eficiente para usos generales, pero cuando se requiere un número elevado de operaciones de concatenación, conviene emplear StringBuilder para optimizar tanto la velocidad como el consumo de recursos.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En POO, la comparación entre objetos puede realizarse de dos maneras: por identidad o por contenido. La identidad se refiere a si dos referencias apuntan exactamente al mismo objeto en memoria, mientras que la comparación por contenido determina si los datos internos de ambos objetos son equivalentes. En Java, el operador == compara identidad, no contenido, por lo que dos objetos distintos pero con el mismo valor serán considerados diferentes si se usa ==. Para comparar por contenido, las clases suelen definir su propio criterio mediante un método específico.
Ese método es equals, heredado de la clase base Object. Su propósito es indicar si dos objetos deben considerarse iguales según sus valores internos. Por defecto, la implementación de equals en Object funciona igual que ==: compara identidad, no contenido. Por tanto, a menos que una clase sobrescriba equals, dos objetos con datos idénticos seguirán considerándose distintos si se construyeron por separado. Muchas clases estándar de Java, como String, Integer o LocalDate, sobrescriben equals para implementar comparación por contenido, lo que las hace más útiles en colecciones y estructuras de datos.
Para comparar correctamente dos cadenas en Java, debe usarse el método equals. Dado que String es inmutable y sobrescribe equals, este método comprueba caracter por caracter si ambas cadenas contienen el mismo texto. En cambio, == solo indica si ambas referencias apuntan al mismo objeto, lo cual no es fiable para comprobar igualdad textual. Por ello, expresiones como "hola" == new String("hola") devolverán false, mientras que "hola".equals(new String("hola")) devolverán true, lo que demuestra que equals es el mecanismo adecuado para comparar contenido.
En resumen, la comparación en POO debe basarse en el criterio más adecuado para el diseño de la clase: identidad cuando se quiere saber si dos referencias apuntan al mismo objeto físico, y contenido cuando interesa determinar si ambos representan conceptualmente lo mismo. En Java, el método equals es la herramienta fundamental para este propósito, y las cadenas deben compararse siempre mediante equals, nunca con ==, para obtener resultados correctos y coherentes.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
Las clases wrapper son clases que encapsulan (envuelven) un tipo primitivo dentro de un objeto, permitiendo tratar valores simples como int, double o boolean como objetos completos. En Java existen wrappers estándar para cada tipo primitivo, como Integer, Double o Boolean. Estas clases proporcionan métodos adicionales (por ejemplo, para convertir valores, comparar, o trabajar en colecciones), algo que no es posible directamente con tipos primitivos. De esta forma, los wrappers permiten integrar tipos básicos dentro del paradigma orientado a objetos, que requiere objetos para muchas operaciones.
En Java este proceso puede hacerse de forma manual creando un objeto wrapper mediante su constructor o métodos estáticos, pero también existe un mecanismo automático llamado autoboxing y unboxing. El autoboxing convierte automáticamente un valor primitivo en su wrapper correspondiente, mientras que el unboxing hace la operación inversa. Por ejemplo, al asignar int x = 5; Integer n = x;, Java convierte x en un Integer sin necesidad de escribir código explícito. Este mecanismo facilita el trabajo en listas genéricas (List<Integer>) o en otras estructuras que solo aceptan objetos.
Las ventajas principales de las clases wrapper incluyen la posibilidad de trabajar con tipos primitivos en contextos donde se necesiten objetos, como colecciones, métodos genéricos o APIs que requieran referencias. Además, los wrappers proporcionan métodos de utilidad relacionados con conversiones, comparación y manipulación, lo que los hace más versátiles que los tipos primitivos. También permiten la existencia de valores nulos (como Integer x = null), lo cual es útil en bases de datos, estructuras opcionales o contextos donde un valor pueda no estar definido.
No todos los lenguajes orientados a objetos distinguen entre tipos primitivos y objetos. Lenguajes como Python o Ruby tratan todos los valores como objetos, por lo que no necesitan wrappers especiales. En cambio, lenguajes como Java y C# sí incluyen tipos primitivos por razones de eficiencia, y ofrecen wrappers para integrarlos en el modelo de objetos. Cada lenguaje equilibra rendimiento y simplicidad de manera diferente, y la necesidad de wrappers depende de si el lenguaje separa tipos primitivos de objetos o los unifica bajo un mismo sistema.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
Un tipo de dato enumerado en POO es un conjunto finito y predefinido de valores posibles para una variable. Su propósito es representar de forma clara y segura un conjunto de opciones cerradas, como los días de la semana, los colores básicos o los estados de un proceso. Al usar un enumerado, se evita el empleo de literales sueltos (como cadenas o números), lo que reduce errores y mejora la legibilidad del código. Además, los enumerados permiten expresar de manera explícita que ciertos valores son los únicos válidos dentro de un dominio concreto.
En Java, un tipo enumerado es realmente una clase especial, definida con la palabra clave enum. Aunque se declara de forma compacta, internamente funciona como una clase completa que extiende java.lang.Enum. Cada valor del enumerado es, en realidad, una instancia única de esa clase. Esto significa que los enumerados pueden contener atributos, métodos, constructores privados e incluso comportamientos diferenciados por cada valor, aunque su uso básico sea simplemente representar un conjunto de constantes.
Los enumerados en Java tienen ventajas claras en términos de encapsulación. En primer lugar, no permiten crear nuevos valores fuera de los definidos en el propio enum, lo que asegura que el dominio de valores permanezca cerrado y controlado. En segundo lugar, el propio enumerado es responsable de encapsular cualquier detalle interno, como atributos asociados a cada constante o métodos auxiliares, evitando que el usuario tenga que conocer cómo están implementados. Finalmente, como cada valor es un objeto, es posible añadir lógica relacionada directamente dentro del propio enumerado, manteniendo la organización y cohesión del código sin exponer detalles innecesarios al exterior.
Gracias a este diseño, los enumerados en Java combinan seguridad, claridad y encapsulación, facilitando la creación de tipos robustos que expresan de manera precisa los valores permitidos dentro de un determinado contexto lógico.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
A continuación se define el tipo enumerado Mes con las doce instancias posibles. Cada valor encapsula dos atributos privados: el ordinal en el año (1–12) y el número de días (sin contemplar años bisiestos; febrero se fija en 28). Se expone una interfaz pública con getDias() y getOrdinal(), además de cuatro métodos estacionales que indican si el mes tiene algunos días de primavera, verano, otoño o invierno, parametrizando el hemisferio con un booleano. Para las estaciones se adopta la convención por mes completo (NH: Invierno Dic–Feb, Primavera Mar–May, Verano Jun–Ago, Otoño Sep–Nov; en el hemisferio sur se invierten), lo que simplifica los solsticios/equinoccios a efectos prácticos.
Este diseño ilustra cómo los enumerados en Java permiten encapsular datos y comportamiento por cada constante, manteniendo un dominio cerrado y coherente. Si se necesitase contemplar años bisiestos, podría añadirse un método adicional (por ejemplo, getDias(boolean esBisiesto)) que devuelva 29 cuando el mes sea FEBRERO y esBisiesto == true, sin cambiar el contrato actual.

public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28), // simplificado: sin bisiesto
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    // Atributos encapsulados
    private final int ordinalEnAnno; // 1..12
    private final int dias;          // días del mes (sin bisiesto)

    // Constructor privado del enum
    Mes(int ordinalEnAnno, int dias) {
        this.ordinalEnAnno = ordinalEnAnno;
        this.dias = dias;
    }

    // Interfaz pública
    public int getOrdinal() {
        return ordinalEnAnno;
    }

    public int getDias() {
        return dias;
    }

    // Estaciones por mes completo:
    // Hemisferio Norte: Inv(Dic-Ene-Feb), Pri(Mar-Abr-May), Ver(Jun-Jul-Ago), Oto(Sep-Oct-Nov)
    // Hemisferio Sur:   Inv(Jun-Jul-Ago), Pri(Sep-Oct-Nov), Ver(Dic-Ene-Feb), Oto(Mar-Abr-May)

    public boolean esDePrimavera(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        }
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        } else {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        }
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        } else {
            return this == MARZO || this == ABRIL || this == MAYO;
        }
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        }
    }

    // (Opcional) Si se quisiera contemplar años bisiestos:
    // public int getDias(boolean esBisiesto) {
    //     if (this == FEBRERO && esBisiesto) return 29;
    //     return dias;
    // }
}

