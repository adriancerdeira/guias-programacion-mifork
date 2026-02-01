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

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta
A la programación orientada a objetos se le atribuyen cuatro características fundamentales: abstracción, encapsulamiento, herencia y polimorfismo. Estas propiedades permiten organizar el código en torno a entidades llamadas objetos, lo que facilita la estructuración y el mantenimiento de programas más complejos que los que se suelen escribir únicamente con funciones y estructuras, como en C.
La abstracción consiste en representar elementos del mundo real mediante clases que capturan solo los detalles esenciales y ocultan los irrelevantes. En lugar de trabajar directamente con datos dispersos y funciones sueltas, se modelan entidades completas que agrupan atributos y comportamientos coherentes. Esto permite centrarse en qué hace un objeto sin necesidad de conocer su funcionamiento interno, favoreciendo el diseño limpio y modular.
El encapsulamiento establece un límite entre el interior de un objeto y el exterior, controlando cómo se accede a sus datos. Los atributos suelen declararse privados y solo se exponen mediante métodos que validan o regulan las operaciones permitidas. Este mecanismo evita errores comunes en C derivados de accesos directos a estructuras mal gestionadas y facilita proteger la integridad del estado interno.
La herencia permite crear nuevas clases a partir de otras ya existentes, reutilizando código y extendiendo funcionalidades. En vez de duplicar estructura y comportamiento, una clase derivada hereda los atributos y métodos de su clase base y puede añadir o modificar características. Por último, el polimorfismo permite que un mismo método o comportamiento pueda variar según el tipo específico del objeto que lo utilice. De esta manera, se pueden definir interfaces comunes y proporcionar implementaciones diferentes, logrando que distintas clases respondan de forma adecuada a la misma operación.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Cuatro lenguajes de programación ampliamente utilizados que permiten la programación orientada a objetos son Java, C++, Python y C#. Estos lenguajes incorporan mecanismos como clases, objetos, herencia y polimorfismo, aunque cada uno los implementa con matices propios. Su popularidad se debe en gran parte a que combinan capacidades orientadas a objetos con otras características que facilitan su adopción en distintos tipos de proyectos.
Java es uno de los lenguajes más asociados con la orientación a objetos, ya que su diseño se centra en este paradigma. Todo se estructura alrededor de clases, lo que promueve una organización clara del código. Por su parte, C++ amplía el lenguaje C con soporte para objetos, permitiendo combinar estilos de programación procedimental y orientado a objetos según convenga, lo que lo convierte en una opción flexible y potente.
Python ofrece un enfoque más dinámico y accesible, ya que permite definir clases y objetos sin necesidad de una sintaxis rígida. Aunque admite otros paradigmas, su soporte para la orientación a objetos es completo y fácil de utilizar, lo que facilita el aprendizaje progresivo. Finalmente, C# incorpora un modelo de objetos moderno y consistente, influido por Java, y se utiliza ampliamente en entornos como .NET y en el desarrollo de videojuegos mediante motores como Unity.
Estos cuatro lenguajes han contribuido significativamente a la difusión de los conceptos de la programación orientada a objetos en múltiples ámbitos, desde aplicaciones empresariales hasta sistemas embebidos o proyectos científicos. Su versatilidad y amplio ecosistema los convierten en referencias habituales al estudiar este paradigma.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La programación estructurada es un paradigma que organiza el código siguiendo un flujo de control claro y predecible. Se fundamenta en tres estructuras básicas: secuencia, selección y repetición. Su objetivo principal es reducir el uso de saltos incontrolados como goto, muy comunes en los primeros lenguajes, para facilitar la lectura, el razonamiento y el mantenimiento del programa. Bajo este enfoque, el programa se descompone en bloques lógicos bien definidos, lo que disminuye la complejidad y favorece un estilo más claro y seguro.
La programación estructurada también promueve que cada parte del programa tenga una responsabilidad clara. En lugar de un flujo desordenado que cambia el estado del programa desde distintos puntos, se busca que cada bloque realice una operación concreta y que el control avance de forma coherente. Esta filosofía fue ampliamente adoptada en lenguajes como C, que aunque permite prácticas no estructuradas, ofrece todas las herramientas necesarias para seguir este modelo de manera disciplinada.
La programación modular va un paso más allá al dividir el programa en unidades independientes llamadas módulos. Cada módulo contiene datos y funciones relacionadas con una tarea específica, lo que permite desarrollar, probar y mantener partes del código de forma aislada. Esta organización facilita la reutilización, ya que un módulo bien diseñado puede integrarse en distintos proyectos sin modificaciones significativas, mejorando la eficiencia del desarrollo.
Además, la modularidad permite que equipos de trabajo distintos desarrollen componentes por separado sin interferencias, siempre que se definan interfaces claras entre módulos. Este enfoque prepara el terreno para la programación orientada a objetos, ya que la idea de agrupar datos y funciones relacionadas anticipa la estructura de las clases. Así, tanto la programación estructurada como la modular representan pasos previos naturales en la evolución hacia modelos más avanzados como la POO.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
En programación orientada a objetos, un objeto se define fundamentalmente por sus atributos, sus métodos y su identidad. Estos tres elementos permiten representar entidades del mundo real o conceptos abstractos dentro de un programa, agrupando datos y comportamientos de manera coherente. La combinación de estos componentes hace posible trabajar con estructuras más ricas que las simples variables o estructuras típicas de C.
Los atributos representan el estado del objeto. Son variables internas que almacenan información relevante sobre la entidad modelada. Por ejemplo, un objeto que represente un coche podría tener atributos como color, velocidad o combustible. Estos datos caracterizan a cada instancia y pueden variar entre objetos de la misma clase, proporcionando valores propios a cada uno.
Los métodos son las operaciones que el objeto puede realizar o las acciones que pueden ejecutarse sobre él. Funcionan como funciones asociadas a una clase y permiten manipular los atributos o interactuar con otros objetos. Siguiendo el ejemplo del coche, métodos como acelerar() o frenar() controlan su comportamiento. Este enfoque integra datos y funciones de forma conjunta, a diferencia de la separación típica en C.
Por último, la identidad distingue a cada objeto de los demás, incluso si comparten la misma clase y atributos con los mismos valores. La identidad garantiza que dos objetos aparentemente iguales sigan siendo entidades distintas en memoria. Esta característica es esencial para gestionar correctamente las referencias, las relaciones entre objetos y el ciclo de vida de cada instancia dentro del programa orientado a objetos.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una clase es una plantilla o modelo que describe cómo serán los objetos que se creen a partir de ella. Define qué atributos y métodos tendrán esas futuras instancias, del mismo modo que una estructura en C define el formato de un conjunto de datos, pero ampliado para incluir también el comportamiento. La clase no ocupa memoria para sus atributos mientras no se cree un objeto; simplemente establece la forma y las capacidades que tendrán esos objetos.
Un objeto no es lo mismo que una clase. Un objeto es una instancia concreta creada a partir de una clase, con su propio estado almacenado en memoria. Si la clase es un plano, el objeto es la construcción final basada en ese plano. Cada objeto tiene sus propios valores para los atributos definidos por la clase y puede ejecutar los métodos que esta proporciona. Por ejemplo, si la clase Coche define atributos como color y métodos como acelerar(), un objeto de tipo Coche representa un coche particular con valores propios para esos atributos.
Una instancia es, por tanto, cada objeto individual derivado de una clase. Instanciar significa crear un objeto real en memoria siguiendo la definición de la clase correspondiente. Pueden existir tantas instancias como se necesite, todas con la misma estructura pero con estados diferentes entre sí. Esta distinción entre definición (clase) y ejemplar (instancia) es esencial para comprender cómo funciona la programación orientada a objetos.
No todos los lenguajes orientados a objetos utilizan el concepto de clase de la misma forma, e incluso algunos no requieren clases para trabajar con objetos. Lenguajes como Java o C++ se basan firmemente en clases, mientras que otros como JavaScript adoptan un modelo basado en prototipos donde los objetos pueden crearse sin necesidad de una definición formal previa. Aun así, la mayoría de los lenguajes orientados a objetos modernos sí emplean clases como mecanismo principal para organizar el código.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
El lugar donde se almacenan los objetos depende del lenguaje y del mecanismo utilizado para crearlos. En lenguajes como Java, los objetos se almacenan siempre en el montículo (heap), una zona de memoria gestionada dinámicamente por la máquina virtual. Solo las referencias a esos objetos se guardan en la pila (stack). En cambio, en C++ es posible crear objetos tanto en la pila como en el montículo: los objetos locales se almacenan en la pila y se destruyen automáticamente al salir del ámbito, mientras que los creados con new se ubican en el montículo y deben liberarse manualmente.
Este comportamiento muestra que no todos los lenguajes gestionan los objetos de la misma manera. En lenguajes con administración automática de memoria, como Java o Python, los programadores no controlan directamente dónde reside el objeto ni cuándo se libera. Por el contrario, en C++ la elección del tipo de almacenamiento influye en el ciclo de vida del objeto, lo que otorga flexibilidad pero también exige mayor responsabilidad para evitar fugas de memoria o accesos inválidos.
La recolección de basura (garbage collection) es un mecanismo automático utilizado por algunos lenguajes para liberar memoria ocupada por objetos que ya no se utilizan. El sistema detecta cuándo un objeto ha quedado inaccesible porque no existen referencias activas que lo apunten, y en ese momento libera su memoria de forma segura. Este proceso reduce errores comunes, como liberar memoria dos veces o no liberarla nunca, que son típicos en lenguajes sin este mecanismo.
Aunque la recolección de basura simplifica la gestión de memoria, también introduce ciertos costes, como pausas breves en la ejecución cuando el recolector revisa el heap. Sin embargo, estos efectos suelen compensarse por la fiabilidad y facilidad de uso que proporcionan. Cada lenguaje implementa su propio tipo de recolector, optimizado según sus necesidades, lo que hace que el comportamiento exacto pueda variar significativamente entre diferentes entornos.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un método es una función asociada a una clase y define una acción que los objetos de esa clase pueden realizar. Mientras que en lenguajes como C las funciones se declaran de forma independiente, en la programación orientada a objetos los métodos forman parte de la estructura de la clase y pueden acceder a los atributos internos del objeto. De este modo, permiten manipular su estado y describir su comportamiento de manera coherente. Por ejemplo, si una clase representa un contador, un método como incrementar() modifica su valor interno siguiendo la lógica definida por la clase.
Además de proporcionar comportamiento, los métodos permiten encapsular reglas y operaciones que dependen directamente de los datos del objeto. Esto refuerza la idea de que una clase integra tanto datos como acciones relacionadas, facilitando el mantenimiento y la claridad del diseño. Cada vez que se invoca un método sobre un objeto, este utiliza su propio estado interno para responder, lo que hace que la interacción resulte intuitiva y natural dentro del paradigma orientado a objetos.
La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una misma clase, siempre que difieran en el número o el tipo de sus parámetros. Este mecanismo permite ofrecer distintas variantes de una misma operación sin necesidad de utilizar nombres diferentes, lo que mejora la legibilidad y proporciona una interfaz más flexible. Por ejemplo, un método sumar() podría tener una versión que reciba dos números enteros y otra que reciba tres, permitiendo al programador escoger la más apropiada según la situación.
La sobrecarga se resuelve en tiempo de compilación, determinándose qué versión del método debe ejecutarse según la firma utilizada en la llamada. Este concepto no está presente de manera idéntica en todos los lenguajes, pero es muy común en lenguajes orientados a objetos como Java o C++. Gracias a esta característica, puede ampliarse la funcionalidad de una clase de forma organizada, evitando duplicación innecesaria y manteniendo una estructura coherente.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
Una clase mínima en Java puede definirse con atributos de visibilidad por defecto (también llamada package-private, es decir, sin modificador como public o private). Para calcular la distancia al origen, se utiliza la fórmula √(x² + y²). A diferencia de C, donde las funciones no están ligadas a estructuras, en Java el método forma parte de la clase y puede acceder directamente a los atributos del objeto.
A continuación, se muestra la clase Punto con dos atributos x e y, y un método de instancia calculaDistanciaAOrigen(). Se emplea Math.sqrt y Math.pow de la librería estándar para el cálculo, aunque podría usarse x*x + y*y para evitar una potencia general si se desea microoptimizar.

// Archivo: Punto.java
class Punto {
    // Visibilidad por defecto (package-private): sin public/protected/private
    double x;
    double y;

    // Constructor por conveniencia
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método de instancia que calcula la distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(Math.pow(this.x, 2) + Math.pow(this.y, 2));
        // Alternativa equivalente (ligeramente más directa):
        // return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

Para ilustrar su uso, se crea una instancia de Punto y se invoca el método. En Java, el punto de entrada es el método main dentro de alguna clase. No es necesario que main esté en la misma clase que Punto, pero para un ejemplo mínimo se puede colocar todo en un único archivo con dos clases package-private.

// Archivo: EjemploUso.java
class EjemploUso {
    public static void main(String[] args) {
        Punto p = new Punto(3.0, 4.0);
        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia); // Debería imprimir 5.0
    }
}

// La clase Punto podría estar en este mismo archivo (como abajo) o en Punto.java
class Punto {
    double x; // visibilidad por defecto
    double y; // visibilidad por defecto

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

En entornos reales, suele preferirse colocar cada clase pública en su propio archivo con el mismo nombre, pero en este ejemplo se evita usar public para ajustarse al requisito de visibilidad por defecto en los atributos y mantener el ejemplo compacto. Además, al trabajar con doubles, el resultado puede estar sujeto a pequeñas imprecisiones de coma flotante, lo que es normal en representaciones IEEE 754.

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
El punto de entrada de un programa en Java es siempre el método main con la firma exacta
public static void main(String[] args). La máquina virtual de Java lo busca al iniciar la ejecución y comienza a interpretar el programa desde esa función. Aunque existan muchas clases y métodos en el proyecto, la ejecución solo arranca desde este punto concreto. Si la firma no coincide exactamente, Java no lo reconocerá como método de entrada y el programa no podrá iniciarse de forma autónoma.
La palabra clave static indica que un miembro pertenece a la clase, y no a una instancia concreta de ella. Esto significa que puede utilizarse sin crear objetos, razón por la cual main tiene que ser estático: al comenzar la ejecución, aún no existe ningún objeto del que invocar un método. Lo estático también se utiliza para constantes compartidas, métodos de utilidad y atributos comunes a todas las instancias, ya que todos ellos funcionan independientemente de los objetos concretos que se creen.
El modificador static no se limita al método main. Puede aplicarse a métodos, atributos e incluso bloques estáticos, y su principal utilidad es crear elementos que pertenezcan al nivel de clase. Esto permite, por ejemplo, definir funciones auxiliares sin necesidad de instanciar objetos, como ocurre con los métodos de la clase Math en Java. Asimismo, los atributos estáticos se emplean cuando un dato debe compartirse entre todas las instancias, manteniendo un único valor global.
Cuando se combina static con final, se define un valor constante asociado a la clase, conocido típicamente como constante de clase. Este valor no puede modificarse después de su inicialización y es común a todas las instancias. Por ello, se utiliza ampliamente para representar parámetros fijos, como static final double PI = 3.14159;. Esta combinación mejora la seguridad del código y facilita dar nombres claros a valores que no deben cambiar, evitando errores y aumentando la legibilidad.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
Para compilar y ejecutar un programa básico en Java desde la línea de comandos, se utilizan los comandos javac y java. Primero se compila el archivo fuente (.java) con javac NombreArchivo.java, lo que genera uno o varios ficheros .class. Después, la ejecución se realiza con java NombreClase, indicando únicamente el nombre de la clase que contiene el método main, sin la extensión. Este proceso separa claramente la fase de compilación de la fase de ejecución, algo familiar para quien viene de C/C++, aunque con diferencias importantes en el formato del código resultante.
Java se considera un lenguaje compilado y a la vez interpretado, ya que no se compila directamente a código máquina nativo. En su lugar, el compilador transforma el código fuente en bytecode, un formato intermedio independiente de la plataforma. Este bytecode se almacena en los ficheros .class generados por el compilador. La portabilidad del bytecode es uno de los rasgos clave de Java: un mismo archivo .class puede ejecutarse en cualquier sistema que tenga instalada una máquina virtual compatible.
La máquina virtual de Java (JVM) es el componente encargado de interpretar o compilar dinámicamente (mediante técnicas como Just-In-Time compilation) el bytecode y convertirlo en instrucciones nativas del sistema donde se ejecuta el programa. Este mecanismo proporciona aislamiento, seguridad y portabilidad, ya que el programa no depende de un tipo de procesador o sistema operativo concreto. Además, permite incorporar características avanzadas como la recolección de basura, la carga dinámica de clases o la gestión de excepciones.
El bytecode es un conjunto de instrucciones compactas y optimizadas que representan el programa en un nivel más abstracto que el ensamblador tradicional. Los ficheros .class son simplemente contenedores de dicho bytecode, junto con información adicional como las definiciones de clases, constantes y metadatos. Gracias a esta arquitectura, Java mantiene un equilibrio entre rendimiento aceptable y gran portabilidad, ya que la JVM realiza en tiempo de ejecución las optimizaciones necesarias para adaptarse al hardware concreto en el que se ejecuta el programa.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
En Java, new es el operador que reserva memoria en el heap para un objeto e invoca a su constructor para inicializarlo. El resultado de new es una referencia al objeto recién creado, que puede almacenarse en una variable del tipo de la clase. En comparación con C, donde podría usarse malloc para reservar memoria y luego inicializar manualmente la estructura, en Java la reserva y la inicialización quedan unificadas en una sola expresión mediante new y el constructor.
Un constructor es un método especial cuya firma no declara tipo de retorno y cuyo nombre coincide exactamente con el de la clase. Su misión es dejar el objeto en un estado válido estableciendo valores iniciales para los atributos o realizando comprobaciones. Si no se declara ningún constructor, el compilador genera uno por defecto sin parámetros. Los constructores pueden sobrecargarse (mismo nombre, distinta lista de parámetros) para ofrecer diferentes formas de crear objetos. Suele emplearse this para distinguir entre parámetros y atributos cuando comparten nombre.
A modo de ejemplo, se presenta una clase Empleado con tres atributos (DNI, nombre y apellidos) y dos constructores: uno por defecto y otro parametrizado. El constructor parametrizado valida y asigna los valores recibidos.

class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor por defecto (sin parámetros)
    Empleado() {
        // Valores iniciales razonables o vacíos
        this.dni = "";
        this.nombre = "";
        this.apellidos = "";
    }

    // Constructor parametrizado
    Empleado(String dni, String nombre, String apellidos) {
        // Ejemplo simple de validación mínima
        if (dni == null || dni.isEmpty()) {
            throw new IllegalArgumentException("El DNI no puede ser vacío");
        }
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

En el uso típico, la creación de un empleado concreto se realiza con new Empleado("12345678A", "Ada", "Lovelace");. De esta forma, el operador new crea la instancia y el constructor garantiza su correcta inicialización, evitando objetos en estados inconsistentes y facilitando un código más seguro y mantenible.

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
La referencia this en Java señala al objeto actual sobre el que se está ejecutando un método o un constructor. Sirve para acceder a los atributos y métodos de instancia del propio objeto, y también para desambiguar cuando los nombres de parámetros coinciden con los nombres de atributos. Además, permite encadenar constructores dentro de la misma clase mediante this(...), delegando la inicialización en otro constructor.
No se llama igual en todos los lenguajes, aunque la idea es equivalente. En Java, C++ y C# se utiliza this con un significado muy similar; en Python se emplea el parámetro convencional self en los métodos de instancia; y en JavaScript se usa también this, pero su valor depende del contexto de invocación (no solo de la clase), lo que puede requerir atarlo explícitamente o usar funciones flecha en ciertos casos. Pese a las diferencias, el propósito común es referirse a la instancia actual para manipular su estado y comportamiento.
A continuación, un ejemplo en la clase Punto mostrando desambiguación de nombres en el constructor y uso de this dentro de un método:

class Punto {
    double x; // visibilidad por defecto
    double y; // visibilidad por defecto

    // Constructor: desambiguación entre parámetros y atributos
    Punto(double x, double y) {
        this.x = x; // 'this.x' es el atributo; 'x' es el parámetro
        this.y = y;
    }

    // Método de instancia que usa atributos a través de 'this'
    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Método que traslada el punto y devuelve la propia instancia (fluido)
    Punto trasladar(double dx, double dy) {
        this.x += dx;
        this.y += dy;
        return this; // permite encadenar llamadas si se desea
    }
}

Este patrón hace explícito cuándo se accede al estado del objeto actual y evita confusiones con variables locales o parámetros, mejorando la legibilidad y la corrección del código.

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
Para calcular la distancia entre el objeto actual y otro punto recibido como parámetro, se implementa un método de instancia distanciaA(Punto otro). La fórmula empleada es la distancia euclídea en 2D: √((x₂ − x₁)² + (y₂ − y₁)²). En este contexto, this identifica las coordenadas del punto actual y el parámetro otro aporta las coordenadas del segundo punto. Resulta conveniente validar que el parámetro no sea null para evitar errores en tiempo de ejecución.
A continuación, se muestra la clase Punto con el nuevo método, manteniendo la visibilidad por defecto en los atributos, y un ejemplo rápido de uso. Se utiliza la misma convención vista previamente, apoyándose en Math.sqrt y operaciones aritméticas básicas para el cálculo.

class Punto {
    double x; // visibilidad por defecto
    double y; // visibilidad por defecto

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    // Nuevo método: distancia entre 'this' y el punto 'otro'
    double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto de destino no puede ser null");
        }
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

Ejemplo de uso: se crean dos instancias y se calcula la distancia entre ellas. En este caso, los puntos (3,4) y (0,0) están separados por una distancia de 5.0, coherente con el triángulo 3-4-5.

class EjemploUso {
    public static void main(String[] args) {
        Punto p1 = new Punto(3.0, 4.0);
        Punto p2 = new Punto(0.0, 0.0);
        double d = p1.distanciaA(p2);
        System.out.println("Distancia entre puntos: " + d); // Imprime 5.0
    }
}

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
En Java, el paso de argumentos es siempre por valor. Cuando se pasa un objeto como Punto, lo que se copia es la referencia al objeto (no el objeto en sí). Por ello, si dentro del método se modifican los atributos del objeto referenciado (por ejemplo, p.x = 10;), esos cambios sí se observan fuera del método, porque tanto el argumento externo como el parámetro formal apuntan al mismo objeto en el heap. En cambio, si dentro del método se reasigna el parámetro a una nueva referencia (p = new Punto(…);), esa reasignación no afecta a la variable original del llamador, ya que la copia local de la referencia es la que cambia.
Este matiz suele resumirse como “pass-by-value of the reference” para objetos: el valor que se copia es la referencia. A efectos prácticos, esto equivale a pasar un puntero por valor en C/C++: modificar el contenido al que apunta el puntero es visible fuera, pero cambiar el puntero local para que apunte a otra cosa no se propaga al llamador. Por tanto, al diseñar métodos que reciben objetos, conviene diferenciar entre mutar el estado del objeto (visible fuera) y reasignar la variable parámetro (no visible fuera).
Para los tipos primitivos (como int, double, boolean), también se aplica el paso por valor, pero en este caso lo que se copia es el valor primitivo. Si un método recibe un int y lo modifica dentro del cuerpo, el cambio no afecta a la variable original del llamador. Este comportamiento contrasta con el de C++ cuando se usan referencias o punteros explícitos para permitir modificaciones externas; Java no ofrece paso por referencia para parámetros, por lo que cualquier efecto exterior al método, con primitivos, no es posible sin envolverlos en un objeto mutable.

Ejemplo ilustrativo:
class Punto {
    double x, y;
    Punto(double x, double y) { this.x = x; this.y = y; }
}

class Demo {
    static void mutarPunto(Punto p) {
        p.x = 100;           // Mutación del objeto: SÍ afecta fuera
        p = new Punto(0, 0); // Reasignación de la referencia local: NO afecta fuera
    }

    static void mutarEntero(int v) {
        v = 999;             // Cambia solo la copia local: NO afecta fuera
    }

    public static void main(String[] args) {
        Punto a = new Punto(1, 2);
        mutarPunto(a);
        // a.x == 100 (visible), pero 'a' NO fue reemplazado por (0,0)

        int n = 5;
        mutarEntero(n);
        // n sigue valiendo 5 fuera del método
    }
}

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
El método toString() en Java es una función heredada de la clase base Object que devuelve una representación textual del objeto. Su propósito es ofrecer una descripción legible y útil para depuración, logging o salida por consola; por ejemplo, cuando se concatena un objeto con una cadena o se pasa a System.out.println, Java invoca automáticamente toString(). Si no se sobreescribe, la implementación por defecto muestra el nombre de la clase y un identificador basado en el hash del objeto, lo que suele ser poco informativo.
Este concepto existe en otros lenguajes con nombres y matices similares. En C# se emplea ToString(), también heredado de object. En Python, el análogo idiomático es __str__ (y, para representación no ambigua orientada a depuración, __repr__). En JavaScript, todos los objetos heredan un toString(), aunque su utilidad práctica varía según el tipo. En C++ no existe un toString universal, pero se acostumbra a sobrecargar el operador de inserción << para std::ostream o a proporcionar un método to_string propio.
A continuación, un ejemplo de toString() en la clase Punto, devolviendo un formato legible y estable. Se ilustra también su uso:

class Punto {
    double x; // visibilidad por defecto
    double y; // visibilidad por defecto

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public String toString() {
        // Representación compacta y clara
        return String.format("Punto(x=%.2f, y=%.2f)", this.x, this.y);
        // Alternativa sin formato:
        // return "Punto(x=" + this.x + ", y=" + this.y + ")";
    }
}

class EjemploUso {
    public static void main(String[] args) {
        Punto p = new Punto(3.0, 4.0);
        System.out.println(p);              // Invoca implícitamente toString()
        System.out.println(p.toString());   // Invocación explícita (equivalente)
    }
}

Este patrón hace que los objetos sean auto-descriptivos en salidas de logs y facilita la inspección del estado durante el desarrollo y las pruebas. Además, respetar un formato consistente ayuda a identificar rápidamente valores relevantes cuando se trabaja con colecciones o estructuras complejas.

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta
Una clase puede recordar superficialmente a un struct de C, ya que ambos permiten agrupar datos bajo un mismo tipo. Sin embargo, esta semejanza es limitada: un struct solo contiene datos, mientras que una clase integra datos y comportamiento mediante métodos. Además, una clase incorpora mecanismos como encapsulación, visibilidad (public, private), constructores, herencia y polimorfismo, todos ellos ausentes en el struct clásico de C. Por tanto, aunque ambos agregan información bajo una misma entidad, la clase ofrece un modelo muchísimo más completo y expresivo.
Al struct de C le falta principalmente la capacidad de asociar funciones internas que operen directamente sobre sus datos y formen parte de su definición. En C, las funciones se declaran por separado y reciben punteros a estructuras, lo que crea una relación menos integrada entre datos y comportamiento. La orientación a objetos busca precisamente unir ambos, de modo que cada instancia contenga su propio estado y formas de operar sobre él. Para que un struct se acercase a ser una clase, sería necesario incorporar esta capacidad de vincular métodos a la estructura.
Además, faltan mecanismos de control de acceso y ocultación de información, características fundamentales de la POO. En C, todos los campos de un struct son públicos por defecto y no existe forma nativa de restringir su acceso o validar modificaciones. Las clases permiten proteger el estado evitando que otras partes del programa lo modifiquen de forma incorrecta, lo cual mejora la seguridad y la robustez del diseño. La ausencia de estos mecanismos hace que el struct sea adecuado para agrupar datos, pero no para representar objetos con comportamiento autónomo.
Finalmente, un struct tampoco ofrece constructores automáticos ni soporta instancias con ciclo de vida gestionado, como sí ocurre en Java o C++. En C, la inicialización es manual y no existe una función especial asociada al tipo que garantice un estado inicial válido. Tampoco hay herencia, ni polimorfismo, ni métodos virtuales, por lo que los struct carecen de los pilares clave de la programación orientada a objetos. En resumen, el struct es una herramienta útil en C, pero queda muy por debajo del concepto de clase tal como se entiende en los lenguajes orientados a objetos modernos.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
Para “emular” una clase en C se agrupan los datos en un struct y se definen funciones libres que operan sobre punteros a ese struct. Se suele usar namespacing por prefijo en los nombres de las funciones (por ejemplo, punto_distancia_origen) y, si se quiere imitar un constructor, se añade una función de inicialización (punto_init). No hay encapsulación nativa: los campos quedan públicos, y la disciplina de acceso depende del programador y de la organización del código (p. ej., exponer solo el header necesario).
En este esquema, this no existe como referencia implícita; en su lugar, se pasa explícitamente un puntero al objeto como primer parámetro de cada función. Por convención, ese puntero puede llamarse p, self o incluso this si se desea reflejar la intención. Usar const en el puntero cuando no se modifican campos ayuda a expresar inmutabilidad y a permitir optimizaciones del compilador.

// punto.h (opcional, para declarar la "interfaz")
#ifndef PUNTO_H
#define PUNTO_H

typedef struct {
    double x;
    double y;
} Punto;

void punto_init(Punto* p, double x, double y);
double punto_distancia_origen(const Punto* p);

#endif

// punto.c
#include <math.h>
#include "punto.h"

void punto_init(Punto* p, double x, double y) {
    p->x = x;
    p->y = y;
}

double punto_distancia_origen(const Punto* p) {
    // √(x^2 + y^2)
    return sqrt(p->x * p->x + p->y * p->y);
}

// main.c
#include <stdio.h>
#include "punto.h"

int main(void) {
    Punto p;
    punto_init(&p, 3.0, 4.0);
    double d = punto_distancia_origen(&p);
    printf("Distancia al origen: %.2f\n", d); // 5.00
    return 0;
}

Compilación y enlace (necesaria la libm para sqrt):
gcc -std=c11 -O2 main.c punto.c -lm -o demo_punto
./demo_punto


En resumen, el “método” se convierte en una función con un puntero al struct como primer parámetro; eso es lo que ocupa el papel de this. Se logra una aproximación razonable al estilo de la POO, aunque sin encapsulación, herencia ni polimorfismo nativos. Si se quisiese acercar más el estilo, podrían añadirse tablas de punteros a función dentro del struct para simular métodos o v-tables, a costa de más complejidad.