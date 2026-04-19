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

### Respuesta
El polimorfismo en programación orientada a objetos es la capacidad que tiene un mismo identificador (normalmente una variable o un método) de comportarse de distintas formas dependiendo del tipo concreto del objeto al que hace referencia. 

En Java, esto se da cuando una variable de un tipo base (por ejemplo, una superclase) puede referirse a objetos de diferentes subclases, y cada uno de ellos responde de manera distinta a la misma llamada a método. Su principal utilidad es permitir escribir código más genérico, flexible y reutilizable, evitando depender de tipos concretos y facilitando la extensibilidad de los programas.

Desde el punto de vista práctico, el polimorfismo permite tratar objetos relacionados por herencia como si fueran del mismo tipo, delegando en cada objeto la responsabilidad de ejecutar su comportamiento específico. Esto recuerda a situaciones en C donde se usan funciones distintas según el contexto, pero en Java esa decisión no se toma manualmente, sino de forma automática en tiempo de ejecución. Gracias a ello, se reduce la necesidad de estructuras como if o switch basadas en el tipo del objeto, mejorando la claridad y el mantenimiento del código.

La sobreescritura de métodos es el mecanismo que hace posible el polimorfismo en Java. Consiste en que una clase hija proporcione su propia implementación de un método que ya existe en la clase padre, respetando exactamente su firma (nombre, parámetros y tipo de retorno). De esta manera, cuando se invoca ese método usando una referencia al tipo padre, se ejecuta la versión correspondiente a la clase real del objeto.

class Animal {
    void hacerSonido() {
        System.out.println("El animal hace un sonido");
    }
}

class Perro extends Animal {
    @Override
    void hacerSonido() {
        System.out.println("El perro ladra");
    }
}

En este ejemplo, si una variable de tipo Animal contiene un objeto Perro, al llamar a hacerSonido() se ejecutará el método de Perro, no el de Animal. Esta selección dinámica del método se conoce como enlace dinámico y es una de las características clave del polimorfismo en la programación orientada a objetos.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La ligadura dinámica o enlace tardío consiste en que la decisión sobre qué versión de un método se ejecuta no se toma en tiempo de compilación, sino en tiempo de ejecución, en función del tipo real del objeto y no del tipo de la referencia que se utiliza. 

Es decir, aunque una variable sea declarada como un tipo base, el método que se ejecuta es el correspondiente a la clase concreta del objeto al que apunta. Este mecanismo contrasta con la ligadura estática o temprana, donde la llamada al método queda completamente resuelta en compilación.

La ligadura dinámica está directamente relacionada con el polimorfismo, ya que es el mecanismo que permite que objetos de distintas subclases respondan de forma diferente a una misma llamada de método. Sin ligadura dinámica, la sobreescritura de métodos no tendría efecto práctico, ya que siempre se ejecutaría el método definido en el tipo de la referencia. Por tanto, puede considerarse que el polimorfismo en tiempo de ejecución se basa en el uso de enlace tardío para seleccionar correctamente el comportamiento de cada objeto.

En C++, la ligadura dinámica no es automática: depende de que los métodos de la clase base se declaren como virtual. Si un método no es virtual, se usa ligadura estática aunque exista herencia. En cambio, en Java la ligadura dinámica es el comportamiento por defecto para todos los métodos de instancia (excepto static, final y private), por lo que el programador no tiene que indicarla explícitamente. Esto simplifica el uso del polimorfismo y reduce errores comunes al trabajar con jerarquías de clases.

En Python, la ligadura dinámica es aún más natural, ya que el lenguaje es de tipado dinámico y todas las llamadas a métodos se resuelven en tiempo de ejecución. No es necesario declarar herencia ni interfaces de forma rígida para beneficiarse de este comportamiento; basta con que el objeto tenga el método que se llama. Este enfoque se suele conocer como duck typing y amplía el concepto de polimorfismo más allá de la herencia clásica, aunque la idea central del enlace tardío sigue siendo la misma.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
A continuación se muestra un ejemplo sencillo que ilustra el polimorfismo en Java mediante una jerarquía de clases Soldado, Zapador y Artillero. La clase base declara un método saludar, que representa un comportamiento común a todos los soldados. Las subclases heredan dicho método, pero una de ellas lo redefine para modificar completamente su comportamiento, lo que permite observar el efecto de la sobreescritura en tiempo de ejecución.

La clase Zapador sobreescribe el método saludar, proporcionando una implementación propia que sustituye por completo a la de la clase Soldado. En cambio, la clase Artillero no redefine el método, por lo que utiliza directamente el comportamiento heredado. Esto permite comprobar que, aunque los objetos se manejen mediante referencias del tipo base, cada uno ejecuta su versión correspondiente del método.

Finalmente, se crea un array de referencias de tipo Soldado que contiene objetos de distintas subclases. Al recorrer el array y llamar al método saludar, Java selecciona automáticamente qué versión del método ejecutar en función del tipo real del objeto. Este comportamiento evidencia el uso del polimorfismo mediante ligadura dinámica, sin necesidad de comprobaciones manuales del tipo de cada objeto.

class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma general.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        System.out.println("El zapador saluda mientras prepara explosivos.");
    }
}

class Artillero extends Soldado {
    // No sobreescribe el método saludar
}

public class PruebaPolimorfismo {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[3];
        soldados[0] = new Soldado();
        soldados[1] = new Zapador();
        soldados[2] = new Artillero();

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, al sobreescribir un método es posible invocar la implementación de la clase base y, a partir de ella, añadir o modificar parte del comportamiento. Esto resulta útil cuando se desea reutilizar la funcionalidad común definida en la superclase, evitando duplicar código, y únicamente extenderla con detalles específicos de la subclase. De este modo, no se sustituye completamente el comportamiento, sino que se especializa.

En Java, este mecanismo se utiliza frecuentemente cuando la versión del método en la clase base ya realiza una parte correcta del trabajo y la subclase solo debe completar o adaptar el resultado. Conceptualmente, puede verse como una ampliación del comportamiento heredado en lugar de una sustitución total, lo cual encaja bien con la idea de reutilización propia de la herencia.

Para invocar explícitamente el método de la clase base desde una subclase, se emplea la palabra clave super. Esta palabra clave permite acceder a los métodos (y atributos) de la superclase inmediata, ignorando la versión sobreescrita en la subclase. Así, se garantiza que primero se ejecute el comportamiento general y después el específico.

class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma general.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        super.saludar();  // Llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}

En este ejemplo, el método saludar de Zapador reutiliza el saludo del Soldado base y añade un mensaje adicional. La palabra clave utilizada para invocar el método de la clase base es super, y su uso es fundamental para este tipo de extensión controlada del comportamiento en Java.

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al sobreescribir un método en Java existen varias restricciones importantes. La lista de parámetros debe ser exactamente la misma que en el método de la clase base (mismo número, orden y tipos), ya que de lo contrario no se trataría de una sobreescritura. 

El tipo de retorno debe ser el mismo o uno compatible por covarianza, es decir, un subtipo del tipo de retorno original. Además, no se puede reducir la visibilidad del método (por ejemplo, pasar de public a protected), aunque sí ampliarla, y no se pueden lanzar más excepciones comprobadas (checked exceptions) que las declaradas en el método base.

La sobreescritura (overriding) ocurre entre una clase base y una subclase, y su resolución se realiza en tiempo de ejecución, siendo la base del polimorfismo. Por el contrario, la sobrecarga (overloading) consiste en definir varios métodos con el mismo nombre dentro de una misma clase (o jerarquía) pero con distinta lista de parámetros, y su resolución se hace en tiempo de compilación. En la sobrecarga no interviene el polimorfismo, ya que el método que se ejecuta se decide únicamente según los tipos de los argumentos usados en la llamada.

La anotación @Override se utiliza para indicar explícitamente que un método pretende sobreescribir uno definido en la clase base. Su principal utilidad es que el compilador puede verificar que la sobreescritura es correcta; si la firma del método no coincide exactamente o no existe un método equivalente en la superclase, se produce un error de compilación. Esto ayuda a detectar fallos comunes, como errores tipográficos o cambios involuntarios en la firma del método.

Por estas razones, resulta altamente recomendable usar siempre @Override al redefinir métodos. Aunque no es obligatoria, mejora la claridad del código, documenta la intención del programador y aumenta la seguridad del programa frente a errores difíciles de detectar, especialmente en jerarquías de clases más grandes o en proyectos mantenidos por varias personas.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, al estudiar Java se empieza a emplear el polimorfismo desde etapas muy tempranas, a menudo sin que se mencione explícitamente con ese nombre. Esto ocurre porque Java está diseñado de forma que todas las clases heredan directa o indirectamente de Object, y muchos de los primeros ejercicios ya implican trabajar con métodos pensados para ser sobreescritos, como toString, equals o hashCode.

Aunque al principio se presenten como simples “métodos especiales”, en realidad su uso ya se apoya en el polimorfismo y la ligadura dinámica.

Cuando se sobreescribe toString o equals, efectivamente se está utilizando polimorfismo. Por ejemplo, cuando un objeto se imprime con System.out.println(objeto), Java invoca internamente toString() sobre una referencia de tipo Object, pero ejecuta la versión correspondiente a la clase concreta del objeto. 

Esa selección del método en tiempo de ejecución es exactamente el mismo mecanismo que actúa en ejemplos más formales de polimorfismo con herencia y arrays de clases base.

La diferencia es que, en las fases iniciales del aprendizaje, el polimorfismo no se presenta como un concepto abstracto, sino como un comportamiento “natural” del lenguaje. 

Más adelante, cuando se estudian jerarquías de clases y referencias a tipos base, se pone nombre y estructura formal a algo que ya se venía utilizando implícitamente. Por tanto, puede afirmarse que el polimorfismo en Java no solo se aprende pronto, sino que forma parte del lenguaje desde el primer contacto con sus clases y métodos fundamentales.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta es una clase que representa un concepto general e incompleto, pensada para servir como base de otras clases más concretas. Puede contener atributos, métodos implementados normalmente y también métodos sin implementar. Su objetivo principal es definir un comportamiento común y una estructura que las subclases deben respetar, sin llegar a describir completamente cómo se comporta un objeto concreto de ese tipo. Por este motivo, no se pueden crear instancias de una clase abstracta directamente.

Un método abstracto es un método que no tiene implementación, solo su declaración (firma). Al declararlo, se obliga a que todas las subclases concretas proporcionen su propia implementación de dicho método. Esto es especialmente útil cuando todas las subclases deben realizar una acción (por ejemplo, atacar), pero cada una lo hace de forma distinta. Un método abstracto solo puede existir dentro de una clase abstracta.

En Java, la palabra clave abstract debe colocarse tanto en la declaración de la clase abstracta como en la declaración de cada método abstracto. La clase abstracta puede contener métodos normales (como saludar) y métodos abstractos (como atacar). Las subclases están obligadas a implementar todos los métodos abstractos heredados, o de lo contrario también deberán declararse como abstractas.

El siguiente ejemplo redefine Soldado como clase abstracta, mantiene el método saludar con implementación común y declara atacar como método abstracto. Cada tipo concreto de soldado define su propia forma de atacar, haciendo uso del polimorfismo cuando se trabaja con referencias al tipo Soldado.

abstract class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma general.");
    }

    abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    void atacar() {
        System.out.println("El zapador coloca explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    void atacar() {
        System.out.println("El artillero dispara el cañón.");
    }
}

En este diseño, abstract se coloca en la clase Soldado y en el método atacar, indicando que la clase no se puede instanciar y que el comportamiento de ataque debe ser definido obligatoriamente por cada subclase concreta.

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave final en Java se utiliza para restringir la herencia y la modificación del comportamiento. Cuando se aplica a una clase, indica que dicha clase no puede ser heredada; cuando se aplica a un método, significa que ese método no puede ser sobreescrito por las subclases. Su finalidad principal es proteger un diseño ya completo, asegurando que su comportamiento no se altere en jerarquías de clases posteriores.

En relación con el polimorfismo, el uso de final limita directamente sus posibilidades. El polimorfismo basado en herencia requiere que los métodos puedan ser sobreescritos para que cada subclase ofrezca su propio comportamiento. Si un método es declarado final, todas las clases hijas heredarán exactamente esa implementación, y la ligadura dinámica no podrá seleccionar una versión distinta en tiempo de ejecución. 

Del mismo modo, si una clase es final, no puede participar como clase base en una jerarquía, por lo que tampoco puede haber polimorfismo a partir de ella.

El uso de final no es negativo en sí mismo, sino una herramienta de diseño. Se emplea cuando se desea garantizar seguridad, coherencia o eficiencia, evitando cambios imprevistos en clases críticas. También puede ayudar al compilador y a la máquina virtual a realizar ciertas optimizaciones, ya que se conoce con certeza que no existirá una implementación alternativa del método.

Un ejemplo muy conocido de clase final en la API estándar de Java es String. Esta clase no puede ser heredada, lo que garantiza que el comportamiento de las cadenas sea inmutable y consistente en todo el lenguaje. Otras clases finales habituales son Integer, Double y la clase Math. En estos casos, se sacrifica deliberadamente la posibilidad de polimorfismo a cambio de seguridad, simplicidad y fiabilidad en el diseño básico de la plataforma.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
En Java, una interfaz es una construcción que define un conjunto de métodos que una clase se compromete a implementar, sin especificar cómo deben realizarse. 

Es, por tanto, un contrato que establece qué operaciones ofrece una clase, pero no su implementación concreta. Tradicionalmente, una interfaz solo declaraba métodos (sin cuerpo) y constantes, lo que la hace especialmente adecuada para describir comportamientos comunes que pueden ser compartidos por clases muy diferentes entre sí.

Las interfaces se parecen a las clases abstractas, pero no son exactamente lo mismo. Ambas pueden declarar métodos sin implementación y se utilizan para favorecer el polimorfismo, pero una clase abstracta puede tener atributos, métodos implementados y estado interno, mientras que una interfaz se centra únicamente en el qué se puede hacer, no en el cómo. 

Además, una clase abstracta representa una relación “es-un”, mientras que una interfaz suele expresar más bien un “puede-hacer”.
Una diferencia clave es que una clase puede implementar más de una interfaz, algo que no es posible con las clases abstractas debido a que Java no permite herencia múltiple de clases. 

Esto convierte a las interfaces en una herramienta fundamental para evitar problemas de diseño y para combinar comportamientos de distintas fuentes. Al implementar una interfaz, la clase queda obligada a proporcionar implementación para todos sus métodos abstractos.

interface Atacante {
    void atacar();
}

class Zapador implements Atacante {
    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos.");
    }
}

Gracias a las interfaces, se puede escribir código que dependa únicamente del comportamiento declarado y no de clases concretas, reforzando el uso del polimorfismo y facilitando la reutilización y extensibilidad del software.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
A continuación se presenta un ejemplo de polimorfismo con clases abstractas, donde se define un tipo genérico Punto que representa la idea común de un punto, pero sin fijar el número de dimensiones. 

El método calcularDistanciaA se declara como abstracto porque la forma de calcular la distancia depende de si el punto es bidimensional o tridimensional. De esta manera, se obliga a que cada subtipo proporcione su propia implementación, manteniendo una interfaz común.

Para garantizar que la distancia solo se calcule entre puntos del mismo subtipo, se usa el operador instanceof junto con downcasting. Esto permite comprobar dinámicamente el tipo real del objeto recibido como parámetro y acceder a sus atributos específicos. Aunque no es el diseño más elegante en todos los casos, resulta didáctico para comprender cómo el polimorfismo actúa en tiempo de ejecución y cómo puede controlarse el tipo concreto de los objetos cuando es necesario.

Este diseño se aprovecha posteriormente en la clase Linea, que trabaja únicamente con referencias de tipo Punto, sin conocer si se trata de puntos 2D o 3D. Gracias al polimorfismo, la clase Linea puede delegar el cálculo de la distancia en los propios puntos, obteniendo la longitud correcta sin depender de la dimensión del espacio. De este modo, se desacopla el cálculo de la lógica concreta de cada tipo de punto.

abstract class Punto {
    abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;

    Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }
        Punto2D p = (Punto2D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Punto3D extends Punto {
    double x, y, z;

    Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Punto incompatible");
        }
        Punto3D p = (Punto3D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}

class Linea {
    private Punto inicio;
    private Punto fin;

    Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    double longitud() {
        return inicio.calcularDistanciaA(fin);
    }
}

En este ejemplo, la clase Linea no necesita conocer si trabaja en dos o tres dimensiones: simplemente confía en el comportamiento polimórfico del método calcularDistanciaA. Esto ilustra cómo el polimorfismo permite escribir código genérico que funciona correctamente con distintos tipos concretos, siempre que compartan una interfaz común bien definida.

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La herencia de interfaces en Java consiste en la capacidad que tiene una interfaz de extender otra interfaz, heredando la declaración de sus métodos. De este modo, una interfaz puede especializar o ampliar el contrato definido por otra, añadiendo nuevas operaciones sin eliminar ni modificar las existentes. Este mecanismo permite construir jerarquías de comportamientos, de forma similar a la herencia entre clases, pero centradas exclusivamente en la definición de capacidades y no en la implementación.

En Java sí existe herencia múltiple de interfaces, es decir, una interfaz puede extender varias interfaces a la vez. Esto no provoca los problemas clásicos de la herencia múltiple de clases, ya que las interfaces no contienen estado ni implementaciones (al menos en el enfoque clásico), sino únicamente contratos. Gracias a esto, una clase puede acabar implementando comportamientos procedentes de múltiples interfaces sin conflictos estructurales.

Esta característica es especialmente útil para enriquecer un comportamiento básico añadiendo responsabilidades opcionales. Por ejemplo, puede definirse una interfaz Fichero que represente la capacidad básica de leer contenido, y luego una interfaz más especializada FicheroEscribible que herede de ella y añada operaciones de escritura y eliminación. Cualquier clase que implemente FicheroEscribible queda obligada automáticamente a cumplir también el contrato de Fichero.

interface Fichero {
    String leerContenido();
}

interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

En este ejemplo, FicheroEscribible hereda el método leerContenido y añade nuevos métodos relacionados con la modificación del fichero. Desde el punto de vista del polimorfismo, permite tratar objetos como simples Fichero cuando solo se necesita leer, o como FicheroEscribible cuando se requiere un comportamiento más avanzado, sin acoplar el código a clases concretas.