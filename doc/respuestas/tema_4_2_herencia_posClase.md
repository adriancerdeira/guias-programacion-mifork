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
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
En orientación a objetos, la herencia es un mecanismo que permite definir una nueva clase a partir de otra existente, reutilizando y ampliando su definición. La relación que se establece se conoce como “A es un B”, lo que significa que una instancia de la subclase (A) puede considerarse también una instancia de la superclase (B). En Java, esto se expresa mediante la palabra clave extends. Conceptualmente, se usa cuando existe una especialización clara: un tipo más concreto que mantiene la identidad del tipo más general.

La primera implicación importante de la herencia es la compatibilidad de tipos. Si una clase Artillero o Zapador hereda de Soldado, entonces cualquier objeto de estas clases puede tratarse como un Soldado. Esto permite escribir código más general, por ejemplo trabajar con colecciones de Soldado sin preocuparse del subtipo concreto. Esta compatibilidad es la base del polimorfismo y resulta especialmente útil para recorrer arrays o listas y ejecutar comportamientos comunes.

La segunda implicación es la herencia de estado y comportamiento. Las subclases heredan los atributos y métodos accesibles definidos en la superclase, sin necesidad de volver a implementarlos. En este caso, tanto Artillero como Zapador heredan el atributo nombre y el método saludar(). Cada subtipo puede además añadir su propio estado (cohetes o minas) y comportamiento específico mediante nuevos métodos, manteniendo el código común en un único lugar.

A continuación se muestra un ejemplo sencillo en Java que ilustra estas ideas, incluyendo la compatibilidad de tipos mediante un array de Soldado que contiene objetos de distintos subtipos:

public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Soldado("Carlos");
        ejercito[1] = new Artillero("Luis", 5);
        ejercito[2] = new Zapador("Marta", 3);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Al crear un objeto de una clase concreta que hereda de otra, se ejecutan siempre varios constructores. En concreto, se ejecuta el constructor de la clase base y después el de la clase derivada. Este orden es obligatorio: primero se inicializa la parte del objeto que corresponde a la superclase y solo después la parte específica de la subclase. Por ejemplo, al crear un Artillero, se ejecuta primero el constructor de Soldado y luego el de Artillero, aunque desde el punto de vista del programador solo se “cree” un objeto.

Dentro de un constructor, la palabra clave super se utiliza para invocar explícitamente un constructor de la clase base. Esta llamada sirve para inicializar correctamente los atributos heredados y debe ser siempre la primera instrucción del constructor. 

Si no se escribe ninguna llamada a super, Java intenta insertar automáticamente una llamada a super() (el constructor sin parámetros de la clase base). Este comportamiento es implícito y solo funciona si dicho constructor existe y es accesible.

Si la clase base no tiene un constructor sin parámetros visible, entonces es obligatorio llamar explícitamente a super indicando los parámetros necesarios. En caso contrario, el código no compila, ya que Java no sabe cómo construir la parte correspondiente a la superclase. 

Esto ocurre con frecuencia cuando la clase base define únicamente constructores con parámetros, como en el caso de Soldado, que necesita un nombre.

En consecuencia, puede afirmarse que siempre se ejecuta el constructor de la superclase, se use o no herencia directa de forma consciente. Además, si no existe un constructor sin parámetros accesible en la clase base, la llamada a super es obligatoria, mientras que si existe, puede omitirse y Java la insertará automáticamente. Este mecanismo garantiza que los objetos se construyan de forma coherente y completamente inicializada.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. En Java, cuando se crea un objeto de una subclase, este objeto contiene toda la estructura definida por la superclase, incluidos sus atributos privados, además de los atributos propios de la subclase. 

No existen “dos objetos”, sino un único objeto cuyo tamaño en memoria incluye el estado completo heredado y el específico. Por tanto, un Artillero contiene internamente el atributo nombre definido en Soldado, aunque dicho atributo sea private.

Sin embargo, que el atributo exista en memoria no implica que sea accesible desde el código de la subclase. El modificador private restringe el acceso exclusivamente a la clase donde se declara. Esto significa que una subclase no puede referirse directamente a ese atributo, ni leerlo ni modificarlo, ni siquiera usando herencia. La herencia garantiza la existencia del estado, pero el encapsulamiento sigue siendo respetado en tiempo de compilación.

Este comportamiento puede observarse claramente con el ejemplo de Soldado. Aunque un Artillero tiene un nombre en memoria, no puede acceder a él directamente; solo puede interactuar con ese estado a través de métodos públicos o protegidos definidos en la superclase, como saludar() o un posible getNombre(). De este modo, Java separa claramente dos conceptos: estructura del objeto en memoria (que sí se hereda por completo) y visibilidad desde el código (que depende estrictamente del modificador de acceso).

public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

public class Artillero extends Soldado {
    public Artillero(String nombre) {
        super(nombre);
    }

    public void probarAcceso() {
        // nombre = "Pedro"; // ERROR: nombre es private en Soldado
        saludar(); // Correcto: acceso indirecto al atributo privado
    }
}

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
La compatibilidad a nivel de tipos que proporciona la herencia implica una gran extensibilidad del código. Extensibilidad significa que el sistema puede ampliarse con nuevos comportamientos o tipos sin necesidad de modificar el código ya existente. 

Al poder tratar todos los subtipos como instancias de la clase base (Soldado), el código que opera sobre soldados funciona de manera genérica y no depende de los casos concretos. Esto reduce el acoplamiento y facilita la evolución del programa.

Gracias a esta compatibilidad, el código que recorre un conjunto de Soldado y les pide que se comporten como soldados (por ejemplo, que saluden) no necesita ser modificado cuando aparece un nuevo tipo. Basta con crear una nueva subclase que herede de Soldado y respete su interfaz pública. El resto del sistema sigue funcionando igual, sin recompilar ni reescribir la lógica ya probada.

Esto puede ilustrarse añadiendo, por ejemplo, un nuevo tipo llamado Medico, que también es un Soldado y hereda el nombre y el método saludar(), pero añade su propio estado específico. En ningún momento es necesario tocar el código que recorre el array de soldados, ya que sigue trabajando con referencias de tipo Soldado.

public class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Soldado("Carlos");
        ejercito[1] = new Artillero("Luis", 5);
        ejercito[2] = new Zapador("Marta", 3);
        ejercito[3] = new Medico("Elena", 2);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

Este ejemplo muestra que la extensibilidad se consigue sin modificar el código existente, únicamente añadiendo nuevas clases. Esta es una de las ventajas fundamentales de la herencia y del uso de tipos base comunes en Java, ya que permite construir sistemas abiertos a la ampliación, pero cerrados a la modificación del código ya desarrollado.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
Sí, en Java una referencia del supertipo puede apuntar a un objeto real de un subtipo sin ningún problema. Por ejemplo, una variable de tipo Soldado puede referenciar en tiempo de ejecución a un objeto Artillero o Zapador. Esto es una consecuencia directa de la relación “A es-un B” y es la base del polimorfismo. Sin embargo, el tipo de la referencia determina qué métodos pueden invocarse en tiempo de compilación, no el tipo real del objeto.

Con una referencia del supertipo solo pueden invocarse los métodos que estén declarados en el supertipo, aunque el objeto real sea de un subtipo. No es posible llamar directamente a métodos públicos que existan solo en la subclase, como getCohetes(), usando una referencia de tipo Soldado. Esto evita errores y mantiene el código seguro, ya que no todos los Soldado tienen necesariamente cohetes. Aun así, el método que se ejecuta (si está sobrescrito) depende del objeto real, no del tipo de la referencia.

El upcasting consiste en tratar un objeto de un subtipo como si fuera del supertipo, y es implícito y siempre seguro. Ocurre automáticamente, por ejemplo al asignar un Artillero a una variable Soldado. El downcasting, por el contrario, consiste en convertir una referencia del supertipo a un subtipo concreto y requiere una conversión explícita, ya que puede ser inseguro. Si el objeto real no es de ese subtipo, se produce una excepción en tiempo de ejecución.

Para hacer downcasting de forma segura se utiliza el operador instanceof, que permite comprobar el tipo real del objeto antes de convertirlo. El siguiente ejemplo muestra un recorrido de un array de Soldado en el que, si el objeto real resulta ser un Artillero, se obtiene e imprime su número de cohetes:

Soldado[] ejercito = new Soldado[3];
ejercito[0] = new Soldado("Carlos");
ejercito[1] = new Artillero("Luis", 5);
ejercito[2] = new Zapador("Marta", 3);

for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting seguro
        System.out.println("Cohetes disponibles: " + a.getCohetes());
    }
}

Este patrón es habitual cuando se trabaja con colecciones polimórficas y se necesita acceder a comportamientos específicos de ciertos subtipos sin perder la generalidad del código.

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso protegido forma parte de los mecanismos de ocultación de información y se sitúa entre private y public. Un miembro declarado como protected es accesible desde la propia clase, desde cualquier clase del mismo paquete y, lo más relevante en herencia, desde las subclases, incluso aunque estén en otro paquete. 

Su objetivo es permitir que las clases derivadas reutilicen o amplíen el comportamiento interno de la superclase sin exponer dichos miembros al resto del código.

En Java, el acceso protegido se implementa mediante la palabra clave protected. A diferencia de private, que restringe el acceso exclusivamente a la clase donde se declara el miembro, protected permite que las subclases accedan directamente a atributos y métodos heredados. Esto resulta útil cuando una subclase necesita trabajar con el estado interno del objeto para implementar su propia lógica, manteniendo ese estado oculto para códigos no relacionados por herencia.

Aplicado al ejemplo de Soldado, si el atributo nombre se declara como protected, podrá ser utilizado directamente desde una subclase como Zapador. De este modo, el zapador puede usar el nombre del soldado al ejecutar una acción específica, como poner minas, sin romper el encapsulamiento hacia el exterior. El atributo sigue sin ser accesible desde código externo que no herede de Soldado.

public class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        System.out.println("El zapador " + nombre + " ha puesto una mina");
        minas--;
    }

    public int getMinas() {
        return minas;
    }
}

Este uso muestra cómo el acceso protegido permite a la subclase reutilizar directamente información heredada sin necesidad de métodos auxiliares, manteniendo al mismo tiempo un nivel razonable de ocultación frente al resto del programa.

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
En los lenguajes orientados a objetos no existe una regla universal que obligue a que todos los objetos hereden de una única clase base común. La existencia de una “clase raíz” depende del diseño del lenguaje. 

Algunos lenguajes la incluyen para unificar el modelo de objetos y proporcionar comportamiento común, mientras que otros permiten jerarquías independientes o incluso la coexistencia de tipos que no comparten un ancestro común.

En lenguajes como C++, por ejemplo, no existe una clase base implícita para todos los objetos. Una clase solo hereda de otra si se declara explícitamente, y pueden existir jerarquías completamente separadas sin ningún antepasado común. Esto da mucha libertad, pero también hace que ciertos mecanismos (como tratar todos los objetos de forma uniforme) sean más complejos o directamente inexistentes sin usar técnicas adicionales.

En Java, sin embargo, sí existe una clase base común para todos los objetos, llamada Object, que pertenece al paquete java.lang. Toda clase en Java hereda directa o indirectamente de Object, incluso aunque no se indique explícitamente con extends. Esto garantiza que cualquier objeto en Java dispone de un conjunto mínimo de métodos comunes, como toString(), equals() o getClass(), lo que simplifica el diseño del lenguaje y permite un tratamiento uniforme de los objetos.

Por tanto, en Java puede afirmarse que toda jerarquía de herencia tiene como raíz la clase Object, lo cual refuerza la compatibilidad de tipos y facilita el polimorfismo. Esta decisión de diseño distingue a Java de otros lenguajes orientados a objetos más flexibles, pero menos homogéneos en su modelo de tipos, y es clave para entender por qué todas las referencias de objeto en Java comparten ciertas capacidades básicas.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La herencia múltiple es un mecanismo de la programación orientada a objetos mediante el cual una clase puede heredar directamente de más de una clase base. Esto significa que la clase derivada adquiere estado y comportamiento provenientes de varias superclases distintas. Este modelo existe en algunos lenguajes, como C++, y permite una gran reutilización de código, pero también introduce problemas de diseño, especialmente cuando varias clases base definen miembros con el mismo nombre (el conocido problema del diamante).

Java no permite herencia múltiple de clases. En Java, una clase solo puede extender (extends) una única clase base. Esta decisión de diseño simplifica el modelo de herencia y evita ambigüedades relacionadas con la herencia de estado y con la resolución de métodos. De este modo, cuando se hereda de una clase, queda perfectamente claro de dónde provienen los atributos y los métodos, y en qué orden se inicializa el objeto.

No obstante, Java introduce una alternativa controlada a la herencia múltiple mediante el uso de interfaces. Una clase puede implementar (implements) múltiples interfaces, lo que permite heredar comportamiento abstracto (y, desde Java 8, métodos por defecto) sin heredar estado. Así, Java separa claramente la herencia de implementación (clases) de la herencia de tipo y comportamiento abstracto (interfaces), reduciendo los problemas clásicos de la herencia múltiple.

En resumen, puede afirmarse que Java evita la herencia múltiple de clases de forma deliberada, pero proporciona mecanismos suficientes para lograr reutilización y extensibilidad mediante interfaces. Este enfoque encaja bien con un modelo de herencia más simple y seguro, especialmente adecuado para comprender y mantener sistemas grandes sin introducir ambigüedades estructurales.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
En los lenguajes orientados a objetos, las excepciones son objetos, lo que implica que se definen como clases y participan en jerarquías de herencia. Esto permite crear excepciones personalizadas para representar errores específicos del dominio del problema, en lugar de usar únicamente las excepciones genéricas del lenguaje. Al modelar excepciones propias, el código gana expresividad y permite manejar los errores de forma más precisa y comprensible.

En Java, una excepción personalizada puede ser controlada o no controlada. Para que sea no controlada (unchecked), la clase debe heredar de RuntimeException. Este tipo de excepciones no obliga a ser declaradas ni capturadas explícitamente, lo que resulta adecuado cuando el error representa una situación anómala grave o una violación de supuestos del programa, como que un usuario esperado no exista.

Además, una excepción puede estar compuesta con otros objetos, es decir, contener referencias a información relevante sobre el error. En este caso, la excepción UsuarioNoEncontradoException contiene un objeto Usuario, lo que permite conocer exactamente qué usuario provocó el problema. Este patrón es habitual en diseño orientado a objetos, ya que la excepción no solo indica que ha ocurrido un error, sino que también transporta el contexto necesario para diagnosticarlo.

Por último, es buena práctica permitir que una excepción personalizada encapsule una causa subyacente, encadenando excepciones. Para ello, se sobrecargan los constructores y se delega en los constructores de la superclase, facilitando el rastreo del error original mediante la traza de pila. A continuación se muestra un ejemplo completo:

public class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }
}

public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}

Este diseño permite lanzar la excepción con toda la información relevante, opcionalmente enlazando una causa previa, sin imponer obligaciones adicionales al código que la utiliza.

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
La herencia no debe usarse únicamente como mecanismo de reutilización de código porque establece una relación muy fuerte entre clases basada en el concepto “es‑un”. 

Cuando se hereda, se afirma que la subclase es una especialización válida y coherente de la superclase en todos los contextos. Si esta relación no es conceptualmente correcta y solo se busca “aprovechar métodos o atributos”, se introduce un acoplamiento innecesario que dificulta el mantenimiento y la evolución del sistema.

Uno de los principales problemas es que la herencia acopla fuertemente la subclase a la implementación de la superclase. Los cambios en la clase base pueden afectar de forma inesperada a todas las subclases, incluso aunque estas no se modifiquen. Además, la herencia expone detalles internos (especialmente si se usan miembros protected) que pueden ser utilizados de formas no previstas, empeorando la encapsulación y aumentando el riesgo de errores difíciles de detectar.

La composición, en cambio, consiste en construir una clase usando otras clases como atributos (“tiene‑un”), delegando en ellas parte de su comportamiento. Este enfoque permite reutilizar código sin asumir una relación semántica de herencia, reduce el acoplamiento y facilita el cambio de implementación. Por ejemplo, si un Soldado “tiene” un arma, resulta más flexible modelar el arma como un objeto interno que heredar distintas clases de soldado para cada tipo de arma.

Por esta razón, se suele recomendar el principio “favorecer la composición frente a la herencia”. La herencia debe reservarse para situaciones donde exista una relación clara y estable de especialización, mientras que la composición es preferible cuando el objetivo principal es reutilizar funcionalidad o combinar comportamientos de forma flexible. Esta distinción conduce a diseños más robustos, extensibles y fáciles de mantener.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Se recomienda favorecer la composición frente a la herencia porque la composición produce diseños más flexibles, desacoplados y fáciles de mantener. Mientras que la herencia fija una relación rígida en tiempo de compilación (“es‑un”), la composición establece una relación más débil (“tiene‑un”), en la que una clase delega parte de su comportamiento en otros objetos. Esto permite cambiar o sustituir componentes internos sin afectar a la jerarquía de clases ni al resto del sistema.

Otro motivo fundamental es que la herencia expone el diseño interno de la superclase a las subclases, lo que puede provocar dependencias frágiles. Un cambio en la clase base puede romper el comportamiento de las clases derivadas, incluso aunque estas no se modifiquen. Con la composición, en cambio, las clases interactúan a través de interfaces o métodos públicos, preservando mejor la encapsulación y reduciendo los efectos colaterales de los cambios.

Además, la composición facilita la combinación dinámica de comportamientos. Mediante herencia, cada variación suele requerir una nueva subclase, lo que puede llevar a una explosión de clases. 
Con composición, los comportamientos pueden añadirse o intercambiarse usando distintos objetos colaboradores, incluso en tiempo de ejecución, sin necesidad de crear nuevas jerarquías. Esto resulta especialmente valioso cuando las responsabilidades no encajan claramente en una relación de especialización.

Por estas razones, la herencia debe considerarse una herramienta potente pero específica, adecuada solo cuando exista una relación clara, estable y semánticamente correcta de “es‑un”. En el resto de casos, la composición ofrece mayor robustez y adaptabilidad, lo que justifica la recomendación general de priorizarla en el diseño orientado a objetos.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Cuando se afirma que la herencia “rompe la encapsulación”, se está haciendo referencia a que la herencia obliga a una subclase a depender de detalles internos de la superclase, incluso de aquellos que idealmente deberían permanecer ocultos. La encapsulación persigue que una clase exponga solo lo necesario mediante una interfaz pública y que sus detalles internos puedan cambiar sin afectar al resto del sistema. Sin embargo, con herencia, la subclase queda intrínsecamente ligada a cómo está implementada la clase base.

Un ejemplo claro de esta ruptura aparece cuando la superclase define atributos o métodos como protected. Aunque esto permite que las subclases reutilicen directamente estado y comportamiento, también las expone a supuestos internos de la superclase. Si la implementación de estos miembros cambia, las subclases pueden dejar de funcionar correctamente, incluso sin modificar su propio código. De este modo, la superclase ya no es libre de cambiar su implementación sin considerar a todas sus subclases, lo que debilita la encapsulación original.

Además, incluso cuando solo se accede a métodos públicos, la subclase depende del comportamiento concreto heredado, no solo de un contrato abstracto. Si un método de la superclase tiene efectos secundarios o reglas implícitas no documentadas, la subclase puede verse afectada indirectamente o necesitar conocer esos detalles para funcionar correctamente. En la práctica, esto lleva a que las subclases “conozcan demasiado” sobre la clase base.

Por esta razón se dice que la herencia puede romper la encapsulación: no porque elimine los modificadores de acceso, sino porque propaga decisiones internas hacia las subclases, creando dependencias fuertes. En contraste, la composición mantiene la encapsulación al interactuar únicamente a través de interfaces bien definidas, permitiendo que los cambios internos de una clase no se filtren hacia el resto del sistema.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
A continuación se muestran dos formas alternativas de modelar el mismo problema, una mediante herencia y otra mediante composición, utilizando los mismos datos comunes: DNI y nombre. En ambos casos se pretende reutilizar dichos datos, pero el enfoque y las implicaciones de diseño son distintas.

Modelado mediante herencia
En este enfoque se identifica una relación clara de tipo “es‑un”. Tanto Estudiante como Trabajador son personas, por lo que resulta natural extraer los datos comunes a una superclase Persona. Las clases concretas heredan estos atributos y pueden añadir su propio estado o comportamiento específico. Este modelo es sencillo y expresivo cuando la relación semántica es estable y no se prevé una variación significativa en la jerarquía.

Sin embargo, este diseño introduce una dependencia directa entre las clases derivadas y la superclase. Cualquier cambio en Persona afecta potencialmente a todas sus subclases, lo que puede limitar la evolución del sistema si los requisitos cambian o si aparecen nuevas entidades que comparten datos pero no encajan bien en la jerarquía.

public class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

public class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}


Modelado mediante composición
En el enfoque basado en composición no se establece una relación “es‑un”, sino una relación “tiene‑un”. Los datos comunes se encapsulan en una clase independiente, DatosPersonales, y tanto Estudiante como Trabajador contienen una instancia de dicha clase. De este modo, se reutiliza el código sin imponer una jerarquía rígida entre las clases principales.

Este diseño ofrece mayor flexibilidad y desacoplamiento, ya que DatosPersonales puede reutilizarse en otros contextos y evolucionar de forma independiente. Además, se evita que Estudiante y Trabajador dependan de una superclase común, lo que resulta ventajoso cuando las entidades no comparten realmente una identidad conceptual fuerte más allá de algunos datos.

public class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatosPersonales() {
        return datos;
    }
}

public class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatosPersonales() {
        return datos;
    }
}

Ambos modelos son correctos desde el punto de vista técnico. La elección entre herencia y composición debe basarse en si existe realmente una relación “es‑un” sólida y estable, o si, por el contrario, se desea reutilizar datos y comportamiento manteniendo un mayor grado de independencia entre las clases.