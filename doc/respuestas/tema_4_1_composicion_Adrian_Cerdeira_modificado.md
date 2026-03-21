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

### Respuesta
A composición en C pódese modelar facendo que unha struct conteña outras struct. No exemplo, unha liña componse de dous puntos, e cada punto ten as súas coordenadas x e y. É conveniente empregar double para as coordenadas e para os cálculos, e declarar as funcións de utilidade con const-correctness para deixar claro que non modifican os argumentos.

Para calcular a distancia entre dous puntos pode usarse hypot (máis estable numéricamente que facer sqrt(dx*dx + dy*dy) manualmente).

A seguinte implementación ilustra a idea “ten-un”: Line ten dous Point. Inclúense funcións para crear puntos e liñas, para calcular a distancia entre puntos e para obter a lonxitude dunha liña (que non é máis que a distancia entre os seus dous extremos). Ao final móstrase un uso mínimo en main para verificar o funcionamento.

#include <stdio.h>
#include <math.h>   // hypot, sqrt

typedef struct {
    double x;
    double y;
} Point;

typedef struct {
    Point a;  // extremo 1
    Point b;  // extremo 2
} Line;

/* Constructores de utilidade (opcionais) */
static inline Point point_new(double x, double y) {
    Point p = { x, y };
    return p;
}

static inline Line line_new(Point a, Point b) {
    Line l = { a, b };
    return l;
}

/* Distancia entre dous puntos */
double point_distance(const Point* p, const Point* q) {
    double dx = q->x - p->x;
    double dy = q->y - p->y;
    /* hypot(dx, dy) é máis robusto fronte a overflow/underflow que sqrt(dx*dx + dy*dy) */
    return hypot(dx, dy);
}

/* Lonxitude dunha liña = distancia entre os seus extremos */
double line_length(const Line* l) {
    return point_distance(&l->a, &l->b);
}

int main(void) {
    Point p1 = point_new(0.0, 0.0);
    Point p2 = point_new(3.0, 4.0);
    Line  seg = line_new(p1, p2);

    printf("Distancia(p1, p2) = %.2f\n", point_distance(&p1, &p2)); // 5.00
    printf("Lonxitude(seg)    = %.2f\n", line_length(&seg));        // 5.00
    return 0;
}

Obsérvase que composición aquí significa que a Line contén (por valor) dous Point. Isto adoita ser preferible cando a liña “é dona” dos puntos (propiedade forte) e non se pretende compartilos con outras estruturas.

Se se necesitase agregación (puntos compartidos), poderían empregarse punteiros a Point; nese caso, habería que documentar quen é responsable da súa xestión de memoria.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
La composición en Java se modela haciendo que una clase “todo” contenga instancias de otras clases “parte”. Aquí, Linea tiene-dos Punto. Para garantizar inmutabilidad (y con ello, una ocultación de información más fuerte que en C), se usan campos private final, clases final (o sin setters) y construcción completamente inicializada en el constructor. La clase Punto expone solo observadores (getters) y operaciones puras (como distanciaA), y la clase Linea calcula su longitud delegando en los puntos que posee.

La inmutabilidad impide cambios en el estado tras la construcción: sin setters, con campos final y sin exponer mutabilidad interna, se asegura que un Punto no pueda alterar sus coordenadas y que una Linea no pueda cambiar sus extremos. Como Punto es inmutable, exponerlo desde Linea no rompe la encapsulación (no hay estado mutable que filtrar). Se añaden comprobaciones de nulidad y se usa Math.hypot para la distancia, por estabilidad numérica.

El siguiente ejemplo muestra ambas clases y un main corto de uso. Obsérvese que se mantiene el contrato de composición (propiedad fuerte): Linea controla a qué puntos apunta desde que nace y ese vínculo no se puede modificar.

// Archivo: Linea.java
public final class Linea {
    private final Punto a; // extremo 1 (inmutable)
    private final Punto b; // extremo 2 (inmutable)

    public Linea(Punto a, Punto b) {
        if (a == null || b == null) {
            throw new NullPointerException("Los puntos no pueden ser nulos");
        }
        this.a = a;
        this.b = b;
    }

    /** Longitud de la línea = distancia entre sus dos extremos */
    public double longitud() {
        return a.distanciaA(b);
    }

    /** Observadores seguros (Punto es inmutable) */
    public Punto extremoA() { return a; }
    public Punto extremoB() { return b; }

    public static void main(String[] args) {
        Punto p1 = new Punto(0.0, 0.0);
        Punto p2 = new Punto(3.0, 4.0);
        Linea seg = new Linea(p1, p2);

        System.out.println("Distancia(p1, p2) = " + p1.distanciaA(p2)); // 5.0
        System.out.println("Longitud(seg)     = " + seg.longitud());    // 5.0
    }
}

// Archivo: Punto.java
final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    /** Distancia euclídea a otro punto (estable con Math.hypot) */
    public double distanciaA(Punto otro) {
        if (otro == null) {
            throw new NullPointerException("El punto destino no puede ser nulo");
        }
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.hypot(dx, dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

Este diseño cumple con la petición: Punto y Linea son inmutables (no hay setters, los campos son final, y se valida la entrada), y la composición se expresa porque Linea “tiene dos Punto” y no permite cambiar sus extremos una vez creada.

Con ello se refuerza la encapsulación respecto a C y se evita estado compartido mutable, simplificando el razonamiento y el manejo de errores mediante excepciones bien colocadas.

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La multiplicidad en composición describe cuántas instancias de un tipo pueden estar relacionadas con una instancia de otro, y viceversa. 

Es un concepto tomado del modelado UML, y expresa la cardinalidad fija o acotada del vínculo estructural. En composición, además, esa multiplicidad implica propiedad fuerte: las partes pertenecen en exclusiva al todo y no pueden existir, semánticamente, al margen de él. Por eso, la multiplicidad suele reflejar cuántos objetos “parte” posee un objeto “todo” y cómo de rígida es esa relación.

En el ejemplo de Linea y Punto, una Linea está formada exactamente por dos puntos. Eso significa que la multiplicidad desde Linea hacia Punto es 2, representada como 2 o 2..2 según la notación. 
No puede haber líneas con un punto, tres puntos o cero puntos; la estructura exige siempre dos extremos inmutables. En términos de composición, la Linea pertenece a ese conjunto fijo y controla su ciclo de vida lógico.

En el sentido contrario, un Punto no pertenece necesariamente a una única Linea; puede participar en ninguna, una o varias líneas distintas. Como los puntos son inmutables y se pasan por referencia, un mismo Punto puede servir como extremo en varias Linea sin que exista conflicto ni mutabilidad compartida. 

Por tanto, la multiplicidad desde Punto hacia Linea es 0..*, es decir, un punto puede no estar en ninguna línea o estar en tantas como se necesite.

En resumen, la composición del ejemplo se expresa con las multiplicidades:
Linea → Punto: 2
Punto → Linea: 0..*.
Este patrón refleja que la Linea tiene dos puntos, y cada Punto puede intervenir en cero o muchas líneas distintas, sin romper la inmutabilidad ni la encapsulación del diseño propuesto.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
La distinción entre composición fuerte y composición débil señala el nivel de dependencia vital que existe entre el “todo” y sus “partes”. 

En una composición fuerte, las partes no tienen sentido fuera del todo: su existencia está completamente subordinada. En términos de ciclo de vida, implica que, cuando el objeto “todo” deja de existir, sus partes también deben desaparecer (semánticamente, aunque en Java esto se expresa por diseño y no por destrucción explícita). Esta visión corresponde a la composición propiamente dicha, la que suele representarse en UML con un rombo negro.

En la composición débil, por el contrario, las partes pueden existir independientemente del todo y ser compartidas por otros objetos. Aquí el vínculo es más laxo: la desaparición del “todo” no obliga a que las partes desaparezcan. 

En el modelado orientado a objetos, esta situación corresponde a lo que habitualmente se denomina asociación o agregación, representada en UML con un rombo blanco. El ciclo de vida de las partes no está ligado de manera estricta al del todo, por lo que su gestión es más flexible.

La consecuencia clave de esta diferencia radica en la propiedad y el control de la vida útil de los objetos. En una composición fuerte, el todo es el único dueño de sus partes, y estas no deberían compartirse con otros objetos, reforzando así encapsulación y consistencia. 

En la composición débil, no hay propiedad exclusiva, lo que permite reutilizar objetos y facilita diseños donde varios componentes dependen de la misma entidad sin que eso implique restricciones fuertes sobre su creación o eliminación.
En resumen:

Composición débil → normalmente llamada asociación/agregación; las partes pueden existir sin el todo.
Composición fuerte → llamada composición en sentido estricto; el ciclo de vida de las partes depende del del todo.

Esta clasificación ayuda a modelar de forma precisa cómo deben relacionarse los objetos y qué responsabilidades tiene cada uno en la gestión de su estado y existencia.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
Cuando una clase simplemente usa a otra dentro de un método —ya sea recibiéndola como parámetro, devolviéndola como resultado, creándola con new dentro del propio método o empleándola como variable local— no se considera que exista composición, sino una relación de dependencia. 

La idea clave es que, en estos casos, la clase no posee a la otra ni controla su ciclo de vida de forma estructural; simplemente la necesita momentáneamente para realizar alguna operación. El vínculo es temporal y limitado al ámbito de ejecución, sin que forme parte del “estado permanente” del objeto.

La composición, en cambio, implica que una clase contiene a otra como parte de su estructura interna: la almacena como campo, normalmente private, y la relación tiene carácter estable y forma parte de su identidad. En una dependencia, el objeto colaborador puede existir antes, después o independientemente del objeto que depende de él, y no queda ligado a su existencia. Es habitual decir que, si un objeto solo “usa” brevemente a otro, no existe propiedad fuerte ni ciclo de vida subordinado, y por tanto no hay composición.

Además, las dependencias suelen expresar necesidades puntuales: una clase puede depender de muchas otras para realizar tareas específicas, sin que eso implique que forme parte de su diseño estructural. Este tipo de relación ayuda a mantener bajo acoplamiento y modela la interacción funcional entre componentes más que su integración interna. La composición, por su parte, delimita los componentes internos que forman parte estable de un objeto, mientras que la dependencia refleja solo una colaboración temporal.

En resumen, cuando una clase utiliza a otra como parámetro, retorno, variable local o a través de un new dentro de un método, la relación que se establece es de dependencia, no de composición. La composición solo aparece cuando la clase incluye a la otra como parte de su estado fijo y la integra en su estructura interna.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
En composición fuerte, el objeto “todo” posee sus “partes” y controla su ciclo de vida. En Java se refleja creando internamente los objetos parte y no aceptándolos desde fuera, además de evitar exponer referencias que permitan “rescatar” esas partes de su contenedor. 

Abajo, LineaFuerte crea sus puntos a partir de coordenadas primitivas y solo expone observadores de coordenadas; así, los Punto internos son un detalle de implementación. Aunque Punto sea inmutable, ocultarlo refuerza el contrato semántico de propiedad fuerte y evita cualquier posibilidad de compartir instancias accidentalmente.

En composición débil (agregación), el “todo” no es dueño exclusivo de las partes: recibe referencias a Punto desde fuera y puede compartirlas con otras Linea. El ciclo de vida de los puntos es independiente del de la línea; si la línea deja de existir, los puntos pueden seguir vivos y seguir siendo usados por otras líneas. Abajo, LineaDebil acepta Punto en el constructor y los expone tal cual, dejando explícito que el vínculo es una dependencia estructural sin propiedad fuerte.

// ----- Parte común: Punto inmutable -----
final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    /** Distancia euclídea a otro punto (Math.hypot es estable numéricamente). */
    public double distanciaA(Punto otro) {
        if (otro == null) throw new NullPointerException("El punto destino no puede ser nulo");
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.hypot(dx, dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

// ===== Composición fuerte: la línea crea y encapsula sus puntos =====
final class LineaFuerte {
    private final Punto a; // extremos internos, propiedad fuerte
    private final Punto b;

    /** Se reciben coordenadas, no puntos, y se crean internamente. */
    public LineaFuerte(double ax, double ay, double bx, double by) {
        this.a = new Punto(ax, ay);
        this.b = new Punto(bx, by);
    }

    /** Longitud = distancia entre sus extremos internos. */
    public double longitud() {
        return a.distanciaA(b);
    }

    // Observadores seguros: no se exponen los Punto internos.
    public double ax() { return a.getX(); }
    public double ay() { return a.getY(); }
    public double bx() { return b.getX(); }
    public double by() { return b.getY(); }
}

// ===== Composición débil (agregación): la línea usa puntos externos =====
final class LineaDebil {
    private final Punto a; // referencias compartibles
    private final Punto b;

    /** Se aceptan puntos externos; el ciclo de vida de Punto es independiente. */
    public LineaDebil(Punto a, Punto b) {
        if (a == null || b == null) {
            throw new NullPointerException("Los puntos no pueden ser nulos");
        }
        this.a = a;
        this.b = b;
    }

    /** Longitud delegando en puntos posiblemente compartidos. */
    public double longitud() {
        return a.distanciaA(b);
    }

    /** Se exponen los mismos objetos Punto (agregación/compartición explícita). */
    public Punto extremoA() { return a; }
    public Punto extremoB() { return b; }
}

// ----- Ejemplo de uso mínimo -----
// Punto p compartido por dos LineaDebil (agregación) y no compartido en LineaFuerte.
/*
public class Demo {
    public static void main(String[] args) {
        Punto p = new Punto(0, 0);
        Punto q = new Punto(3, 4);
        LineaDebil l1 = new LineaDebil(p, q);
        LineaDebil l2 = new LineaDebil(p, new Punto(0, 5)); // p se comparte
        System.out.println(l1.longitud()); // 5.0
        System.out.println(l2.longitud()); // 5.0

        LineaFuerte lf = new LineaFuerte(0, 0, 3, 4); // crea sus propios puntos
        System.out.println(lf.longitud()); // 5.0
    }
}
*/

En términos de ciclo de vida, en LineaFuerte los puntos nacen y “mueren” con la línea (semánticamente: no se exponen ni se comparten), mientras que en LineaDebil los puntos pueden preceder y sobrevivir a la línea y ser reutilizados por múltiples líneas. Este contraste ilustra la diferencia clave entre propiedad fuerte (composición estricta) y agregación (composición débil).

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta
En Java, incluso en una composición fuerte, el contenedor no destruye explícitamente los objetos parte porque el lenguaje carece de un mecanismo de destrucción manual como free en C o delete en C++. En su lugar, Java utiliza un recolector de basura (garbage collector), cuya responsabilidad es recuperar memoria automáticamente cuando ya no existen referencias accesibles hacia un objeto. Por tanto, cuando un objeto compuesto deja de ser accesible, sus partes también dejan de serlo si nadie más las referencia, y es el sistema quien decide cuándo liberar su memoria.

Esto significa que, en una composición fuerte, la destrucción es semántica, no manual. El contenedor fija el ciclo de vida lógico de las partes: si la Linea deja de ser utilizada, sus Punto internos tampoco pueden seguir existiendo desde el punto de vista del modelo, porque no se han expuesto referencias que permitan rescatarlos. Sin embargo, la liberación real de memoria no ocurre en ese momento exacto, sino cuando el recolector de basura determine que ya no existe ninguna referencia viva hacia ellos.

Otra consecuencia es que el diseño debe ser cuidadoso para mantener la relación de propiedad. Al no haber destrucción manual, la composición fuerte se expresa a través de encapsulación, campos privados, inmutabilidad y ausencia de referencias salientes que puedan permitir que las partes sobrevivan más allá del contenedor. De esta forma, aunque Java no destruya explícitamente las partes, el programador mantiene intacto el contrato semántico de composición fuerte.

En resumen, en Java el contenedor no destruye los objetos porque no existe destrucción explícita: la liberación de memoria la gestiona el garbage collector cuando los objetos quedan inaccesibles. La composición fuerte se mantiene mediante diseño, no mediante instrucciones de destrucción.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta
En este diseño se modela composición débil (agregación): el Departamento usa (no “posee” en exclusiva) instancias de Profesor. 
Se almacenan referencias en una estructura interna con capacidad máxima 50, pero sin exponer el detalle de implementación (no se devuelve el array ni se permite modificarlo desde fuera). Se ofrecen operaciones de alto nivel: añadir al final, eliminar por posición, consultar cuántos profesores hay y obtener un profesor por posición. Se garantiza que siempre hay un director desde el inicio (constructor obliga a ello) y que el director pertenece a la plantilla (se inserta y se protege con la invariante).

La invariante principal es: el director debe formar parte de la lista de profesores en todo momento. Por ello, se impide eliminar al director (lanzando excepción) y para cambiar el director se exige que el nuevo director ya esté en el departamento; en caso contrario, se lanza IllegalArgumentException. Se controla también la capacidad (IllegalStateException al alcanzar 50), la nulidad (NullPointerException), los índices fuera de rango y, opcionalmente, se evita duplicar profesores (referencia repetida) para mantener coherencia. Con esto se implementan a la vez: (1) la agregación entre departamento ↔ profesores y (2) la agregación entre departamento ↔ director (que es uno de esos profesores).

A nivel de encapsulación, se devuelve el conteo y el acceso posicional a un profesor concreto, pero no se expone la colección interna, evitando así filtraciones de representación. Esta elección imita el uso de un contenedor abstracto: desde fuera no se sabe si es un array, una lista o cualquier otra estructura. El diseño mantiene la semántica de composición débil: los Profesor pueden existir y ser compartidos fuera del Departamento.

// ======== Dominio simple: Profesor (puede ser compartido) ========
public final class Profesor {
    private final String nombre;
    private final String id; // identificador interno (opcional, ilustrativo)

    public Profesor(String nombre, String id) {
        if (nombre == null || id == null) {
            throw new NullPointerException("Nombre e id no pueden ser nulos");
        }
        this.nombre = nombre;
        this.id = id;
    }

    public String getNombre() { return nombre; }
    public String getId()     { return id; }

    @Override
    public String toString() {
        return "Profesor{" + nombre + ", id=" + id + "}";
    }
}

// ======== Agregación débil: Departamento ↔ Profesores y Director ========
public final class Departamento {
    private static final int CAPACIDAD_MAX = 50;

    private final Profesor[] profesores; // detalle interno encapsulado
    private int n;                       // número de profesores efectivos
    private Profesor director;           // SIEMPRE uno, y siempre pertenece a profesores[0..n)

    /**
     * Crea el departamento con un director inicial, que pasa a formar
     * parte de la lista de profesores desde el primer momento.
     */
    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new NullPointerException("Debe indicarse un director inicial");
        }
        this.profesores = new Profesor[CAPACIDAD_MAX];
        this.n = 0;
        this.director = directorInicial;
        this.profesores[this.n++] = directorInicial; // el director pertenece a la lista
    }

    /** Número de profesores del departamento (incluye al director). */
    public int numeroProfesores() {
        return n;
    }

    /**
     * Accede a un profesor por posición [0, n). No se expone la estructura interna.
     * @throws IndexOutOfBoundsException si la posición no es válida.
     */
    public Profesor profesorEn(int posicion) {
        verificarRango(posicion);
        return profesores[posicion];
    }

    /**
     * Añade un profesor al final. Evita nulos, capacidad desbordada y referencias duplicadas.
     * (Duplicados opcionalmente prohibidos para coherencia; puede relajarse si se desea.)
     */
    public void anadirProfesor(Profesor p) {
        if (p == null) {
            throw new NullPointerException("El profesor no puede ser nulo");
        }
        if (n >= CAPACIDAD_MAX) {
            throw new IllegalStateException("Capacidad máxima (" + CAPACIDAD_MAX + ") alcanzada");
        }
        if (indiceDe(p) != -1) {
            throw new IllegalArgumentException("El profesor ya pertenece al departamento");
        }
        profesores[n++] = p;
    }

    /**
     * Elimina el profesor en la posición dada desplazando el tramo final.
     * Está prohibido eliminar al director para mantener la invariante.
     * @throws IllegalStateException si se intenta eliminar al director.
     * @throws IndexOutOfBoundsException si la posición no es válida.
     */
    public void eliminarProfesor(int posicion) {
        verificarRango(posicion);
        Profesor aEliminar = profesores[posicion];
        if (aEliminar == director) {
            throw new IllegalStateException(
                "No se puede eliminar al director; cámbielo antes a otro profesor del departamento");
        }
        // Compactación in-place
        for (int i = posicion; i < n - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--n] = null; // ayuda al GC
    }

    /** Devuelve el director actual (que siempre pertenece al departamento). */
    public Profesor getDirector() {
        return director;
    }

    /**
     * Cambia el director a otro profesor que ya debe pertenecer al departamento.
     * @throws IllegalArgumentException si el nuevo director no está en el departamento.
     * @throws NullPointerException si el nuevo director es nulo.
     */
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new NullPointerException("El nuevo director no puede ser nulo");
        }
        int idx = indiceDe(nuevoDirector);
        if (idx == -1) {
            throw new IllegalArgumentException(
                "El nuevo director debe formar parte del departamento (añádalo antes)");
        }
        this.director = nuevoDirector;
    }

    // --------- Utilidades privadas ---------

    private void verificarRango(int posicion) {
        if (posicion < 0 || posicion >= n) {
            throw new IndexOutOfBoundsException("Posición inválida: " + posicion + " (n=" + n + ")");
        }
    }

    /**
     * Busca un profesor por referencia (agregación débil: identidad de objeto).
     * @return índice [0, n) o -1 si no está.
     */
    private int indiceDe(Profesor p) {
        for (int i = 0; i < n; i++) {
            if (profesores[i] == p) { // igualdad por identidad; no se expone equals
                return i;
            }
        }
        return -1;
    }
}

/* ======== Ejemplo de uso mínimo ========
public class Demo {
    public static void main(String[] args) {
        Profesor p1 = new Profesor("Ana", "A1");
        Profesor p2 = new Profesor("Luis", "L2");
        Profesor p3 = new Profesor("Marta", "M3");

        Departamento d = new Departamento(p1); // p1 entra como director y profesor
        d.anadirProfesor(p2);
        d.anadirProfesor(p3);

        // Cambiar director a un profesor existente del departamento:
        d.cambiarDirector(p3);

        // Eliminar por posición (no se puede eliminar al director actual):
        d.eliminarProfesor(1); // elimina a p2 si está en pos 1
        System.out.println("Profesores: " + d.numeroProfesores());
        System.out.println("Director: " + d.getDirector());
    }
}
*/


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
Al usar List (por ejemplo, ArrayList) en lugar de arrays primitivos, se simplifica la gestión de la colección: se elimina la lógica de compactación manual al borrar, no hace falta “poner a null” huecos, y se sustituyen búsquedas e inserciones a mano por utilidades como contains, indexOf o remove(int). 

Aun así, se mantiene la invariante: el director debe existir desde el inicio y siempre formar parte del departamento; por tanto, no se permite eliminar al director, y para cambiarlo se exige que el nuevo ya pertenezca a la lista. Se conserva el límite máximo de 50 profesores como una precondición explícita (aunque ArrayList gestione crecimiento dinámico).

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Objects;

// ======== Dominio simple: Profesor (puede ser compartido) ========
public final class Profesor {
    private final String nombre;
    private final String id;

    public Profesor(String nombre, String id) {
        this.nombre = Objects.requireNonNull(nombre, "nombre");
        this.id     = Objects.requireNonNull(id, "id");
    }

    public String getNombre() { return nombre; }
    public String getId()     { return id; }

    @Override
    public String toString() {
        return "Profesor{" + nombre + ", id=" + id + "}";
    }

    // Igualdad por identidad de objeto (como en el ejemplo anterior) al no sobreescribir equals/hashCode.
    // Si interesara evitar duplicados por igualdad lógica, implementar equals/hashCode por 'id'.
}

// ======== Agregación débil con List ========
public final class Departamento {
    private static final int CAPACIDAD_MAX = 50;

    private final List<Profesor> profesores; // detalle interno encapsulado
    private Profesor director;               // siempre pertenece a 'profesores'

    public Departamento(Profesor directorInicial) {
        Objects.requireNonNull(directorInicial, "directorInicial");
        this.profesores = new ArrayList<>(CAPACIDAD_MAX);
        this.profesores.add(directorInicial); // el director forma parte desde el inicio
        this.director = directorInicial;
    }

    /** Número de profesores (incluye al director). */
    public int numeroProfesores() {
        return profesores.size();
    }

    /** Acceso posicional sin exponer la colección interna. */
    public Profesor profesorEn(int posicion) {
        return profesores.get(posicion); // delega en List la comprobación de rango
    }

    /** Añade al final, evitando nulos, capacidad excedida y duplicados (por identidad u equals). */
    public void anadirProfesor(Profesor p) {
        Objects.requireNonNull(p, "profesor");
        if (profesores.size() >= CAPACIDAD_MAX) {
            throw new IllegalStateException("Capacidad máxima (" + CAPACIDAD_MAX + ") alcanzada");
        }
        if (profesores.contains(p)) {
            throw new IllegalArgumentException("El profesor ya pertenece al departamento");
        }
        profesores.add(p);
    }

    /**
     * Elimina por posición; prohibido eliminar al director para mantener la invariante.
     */
    public void eliminarProfesor(int posicion) {
        Profesor aEliminar = profesores.get(posicion); // lanza si pos inválida
        if (aEliminar == director) {
            throw new IllegalStateException(
                "No se puede eliminar al director; cámbielo antes a otro profesor del departamento");
        }
        profesores.remove(posicion); // List compacta automáticamente
    }

    /** Director actual (pertenece a la lista). */
    public Profesor getDirector() {
        return director;
    }

    /**
     * Cambia el director por otro profesor ya existente en el departamento.
     */
    public void cambiarDirector(Profesor nuevoDirector) {
        Objects.requireNonNull(nuevoDirector, "nuevoDirector");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException(
                "El nuevo director debe formar parte del departamento (añádalo antes)");
        }
        this.director = nuevoDirector;
    }

    // --- Versión de solo lectura de la lista (si se quisiera exponer todos los profesores) ---
    public List<Profesor> profesoresInmutables() {
        // Ver alternativas más abajo (unmodifiable view vs copia defensiva)
        return Collections.unmodifiableList(profesores);
    }
}

/* ======== Ejemplo mínimo ========
public class Demo {
    public static void main(String[] args) {
        Profesor p1 = new Profesor("Ana", "A1");
        Profesor p2 = new Profesor("Luis", "L2");
        Profesor p3 = new Profesor("Marta", "M3");

        Departamento d = new Departamento(p1);
        d.anadirProfesor(p2);
        d.anadirProfesor(p3);
        d.cambiarDirector(p3);
        d.eliminarProfesor(1); // elimina a p2 si está en posición 1
        System.out.println("n = " + d.numeroProfesores());
        System.out.println("dir = " + d.getDirector());
    }
}
*/

Respecto a qué parte del código original se ha ahorrado, ya no es necesario: (1) gestionar la compactación manual del array al eliminar (List lo hace con remove(int)), (2) “barrer” a null el último elemento para ayudar al GC, (3) mantener un contador n separado de la capacidad, ni (4) escribir una función indiceDe personalizada para buscar (se usa contains/indexOf). También desaparece la lógica de verificación de rango explícita en eliminación, porque List lanza IndexOutOfBoundsException por sí misma en get/remove (aunque la semántica es la misma).

Por último, si en lugar de profesorEn(int) existiera un método que devolviera todos los profesores, no convendría retornar la lista interna directamente, porque rompería la encapsulación: desde fuera podrían añadirse/eliminarse elementos y violar la invariante (por ejemplo, sacar al director de la lista). 

La forma correcta es no exponer mutabilidad: (a) devolver una vista de solo lectura con Collections.unmodifiableList(profesores) o List.copyOf(profesores) (Java 10+), o (b) devolver una copia defensiva nueva (new ArrayList<>(profesores)) si se prefiere aislarse de cualquier intento de cast o modificación indirecta. Dado que Profesor es inmutable, basta con proteger la estructura; si los elementos fueran mutables, habría que considerar copias profundas o exponer interfaces de solo lectura para los elementos.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta
La composición recursiva aparece cuando una clase contiene una referencia a otra instancia de su mismo tipo. En el ejemplo, Persona es inmutable y tiene un campo madre de tipo Persona. La inmutabilidad se garantiza con clase final, campos private final, ausencia de setters y validación en el constructor. Para evitar ciclos infinitos al imprimir, conviene que toString() no recorra recursivamente sin límite (por ejemplo, limitando la profundidad o mostrando solo el nombre de la madre). Si se desea permitir que alguien no tenga madre registrada, se acepta null como “desconocida/no disponible” (alternativamente podría usarse Optional<Persona>).

A continuación, se muestra un ejemplo con una pequeña familia desde la abuela hasta el nieto (nodo hoja). La relación es de propiedad conceptual, no de propiedad fuerte a nivel de memoria, ya que estas personas podrían compartirse en otros contextos. No obstante, el vínculo recursivo modela bien estructuras genealógicas, jerarquías y árboles en general. Se incluye un método de utilidad para navegar hacia arriba por la cadena de madres (ancestros()) y un toString() que evita expandirse sin límites.

public final class Persona {
    private final String nombre;
    private final int edad;
    private final Persona madre; // composición recursiva: otra Persona

    public Persona(String nombre, int edad, Persona madre) {
        if (nombre == null) throw new NullPointerException("nombre");
        if (edad < 0) throw new IllegalArgumentException("edad negativa");
        this.nombre = nombre;
        this.edad = edad;
        this.madre = madre; // puede ser null si no se conoce
    }

    public String getNombre() { return nombre; }
    public int getEdad()      { return edad; }
    public Persona getMadre() { return madre; }

    /** Devuelve una representación acotada: solo el nombre de la madre si existe. */
    @Override
    public String toString() {
        String madreStr = (madre == null) ? "¿desconocida?" : madre.nombre;
        return "Persona{nombre='" + nombre + "', edad=" + edad + ", madre=" + madreStr + "}";
    }

    /** Recorre ancestros por la línea materna y devuelve los nombres concatenados. */
    public String ancestros() {
        StringBuilder sb = new StringBuilder();
        Persona actual = this;
        while (actual != null) {
            sb.append(actual.nombre);
            actual = actual.madre;
            if (actual != null) sb.append(" <- ");
        }
        return sb.toString();
    }

    // --- Ejemplo de uso ---
    public static void main(String[] args) {
        Persona abuela = new Persona("Pilar", 78, null);
        Persona madre  = new Persona("Laura", 50, abuela);
        Persona nieto  = new Persona("Diego", 20, madre);

        System.out.println(abuela); // madre desconocida
        System.out.println(madre);  // madre = Pilar
        System.out.println(nieto);  // madre = Laura

        // Cadena de ancestros maternos
        System.out.println("Línea materna de Diego: " + nieto.ancestros());
        // Salida: "Diego <- Laura <- Pilar"
    }
}

Otros ejemplos clásicos de composiciones recursivas incluyen: (1) nodos de árbol (cada nodo tiene hijos que son nodos), como en árboles de directorios (Carpeta que contiene Carpeta/Archivo), (2) listas enlazadas (Nodo con referencia a Nodo siguiente), (3) estructuras expresión/AST en compiladores (una Expresion compuesta de subexpresiones Expresion), (4) menús y submenús en interfaces (patrón Composite), y (5) comentarios encadenados o hilos de discusión (un Comentario con respuestas que son también Comentario). 

En todos los casos, la clase compuesta se refiere a sí misma para modelar jerarquías o secuencias con profundidad arbitraria, manteniendo invariantes mediante encapsulación e, idealmente, inmutabilidad donde resulte adecuado.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
Las relaciones de composición bidireccionales (más precisamente, asociaciones bidireccionales con propiedad fuerte o débil) son aquellas en las que cada lado mantiene una referencia al otro y la invariante del modelo exige que ambas referencias estén siempre sincronizadas. En términos de diseño, esto implica que al añadir o eliminar una parte en el “todo” debe ajustarse también el enlace inverso en la “parte”, y viceversa. 

La mayor dificultad no está en el “qué”, sino en el “cómo”: hay que centralizar la mutación en un punto (una “clase dueña” de la relación) para evitar estados intermedios inconsistentes, duplicados o referencias colgantes. Además, conviene evitar que el exterior pueda romper la invariante exponiendo mutabilidad no controlada.

Aplicado a Profesor y Departamento, se propone que Departamento sea el lado propietario de la relación: solo a través de sus métodos (anadirProfesor, eliminarProfesor, cambiarDirector) se modifica la plantilla, y dentro de esos métodos se actualiza el vínculo inverso del Profesor con setDepartamento(this) o setDepartamento(null). A su vez, el Profesor mantiene un campo departamento y no expone un setDepartamento público, o bien lo expone con visibilidad restringida/documentada para uso exclusivo de Departamento. Se debe, además, conservar la invariante del director: el director siempre pertenece a la lista, no puede eliminarse sin antes cambiarlo, y al cambiar el director el nuevo debe estar ya en el departamento.

También es importante prevenir bucles infinitos o inconsistencias al tocar ambos lados: los métodos deben ser idempotentes y cortocircuitar si el enlace ya está en el estado deseado. Si se expone un método en Profesor que permita cambiar su departamento, este debe delegar en Departamento (por ejemplo, retirándose del antiguo y añadiéndose al nuevo) para mantener una única fuente de verdad. 

Finalmente, se recomienda no exponer la colección interna del Departamento y, si se ofrece una lista pública, hacerlo como vista inmodificable o copia defensiva. A continuación, se muestra un esqueleto ilustrativo:

// Lado “parte”
public final class Profesor {
    private final String nombre;
    private Departamento departamento; // vínculo inverso (agregación débil)

    public Profesor(String nombre) {
        if (nombre == null) throw new NullPointerException("nombre");
        this.nombre = nombre;
    }

    public String getNombre() { return nombre; }
    public Departamento getDepartamento() { return departamento; }

    // Visibilidad package-private o restricción documental: SOLO para Departamento.
    void setDepartamento(Departamento nuevo) {
        if (this.departamento == nuevo) return; // idempotente
        this.departamento = nuevo;
    }

    @Override public String toString() {
        // Evitar expandirse por el grafo: no incluir lista completa del departamento.
        String dep = (departamento == null) ? "sin depto" : departamento.getNombre();
        return "Profesor{" + nombre + ", " + dep + "}";
    }
}

// Lado “todo” (propietario de la relación)
public final class Departamento {
    private final String nombre;
    private final java.util.List<Profesor> profesores = new java.util.ArrayList<>();
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        if (nombre == null || directorInicial == null) throw new NullPointerException();
        this.nombre = nombre;
        anadirProfesor(directorInicial); // sincroniza ambos lados
        this.director = directorInicial;
    }

    public String getNombre() { return nombre; }
    public int numeroProfesores() { return profesores.size(); }
    public Profesor profesorEn(int i) { return profesores.get(i); }
    public Profesor getDirector() { return director; }

    public void anadirProfesor(Profesor p) {
        if (p == null) throw new NullPointerException("profesor");
        if (profesores.contains(p)) throw new IllegalArgumentException("Duplicado");
        // Si el profesor ya pertenece a otro departamento, decidir política:
        // aquí se prohíbe explícitamente para ejemplo simple.
        if (p.getDepartamento() != null && p.getDepartamento() != this) {
            throw new IllegalStateException("Profesor ya pertenece a otro departamento");
        }
        profesores.add(p);
        p.setDepartamento(this); // sincroniza vínculo inverso
    }

    public void eliminarProfesor(int pos) {
        Profesor aEliminar = profesores.get(pos);
        if (aEliminar == director) {
            throw new IllegalStateException("No se puede eliminar al director actual");
        }
        profesores.remove(pos);
        aEliminar.setDepartamento(null); // corta vínculo inverso
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) throw new NullPointerException("nuevoDirector");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento");
        }
        this.director = nuevoDirector;
    }

    // Si se expone la plantilla, hacerlo sin mutabilidad externa:
    public java.util.List<Profesor> profesoresInmutables() {
        return java.util.Collections.unmodifiableList(profesores);
    }
}

Este patrón mantiene la bidireccionalidad consistente al centralizar las mutaciones en Departamento, actualizando el enlace inverso en Profesor de forma controlada. Con ello, se conserva la encapsulación, se respeta la invariante del director y se evitan estados intermedios incoherentes que suelen aparecer cuando ambos lados se modifican sin coordinación.