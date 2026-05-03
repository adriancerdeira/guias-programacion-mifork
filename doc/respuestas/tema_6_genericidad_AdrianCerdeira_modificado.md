<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
A falta de mecanismos de genericidad, tanto en C como en Java es posible simular estructuras de datos “para cualquier tipo” utilizando un tipo base común que pueda representar cualquier dato. En C se emplea habitualmente void*, que es un puntero genérico capaz de apuntar a cualquier tipo de dato. En Java, el papel equivalente lo desempeña la clase Object, ya que todas las clases heredan de ella de forma implícita.

En C, una estructura de datos como un vector dinámico puede implementarse usando un array de void*. Cada posición del array almacena la dirección de memoria de un dato concreto, independientemente de su tipo real. Esta aproximación permite guardar enteros, struct, cadenas, etc., aunque obliga al programador a realizar conversiones explícitas de tipo al recuperar los datos, con el consiguiente riesgo de errores en tiempo de ejecución.

typedef struct {
    void **datos;
    int capacidad;
    int numElementos;
} Vector;

void insertar(Vector *v, void *elemento) {
    v->datos[v->numElementos++] = elemento;
}

En Java se puede construir una estructura equivalente utilizando un array de Object. Dado que cualquier objeto en Java es una instancia de una clase que hereda de Object, este enfoque permite almacenar valores de distintos tipos en la misma estructura. 

Si se desea guardar valores primitivos, estos deben convertirse a sus clases envoltorio (por ejemplo, int a Integer) mediante autoboxing. Al igual que en C, al recuperar los datos es necesario realizar casting explícito.

public class Contenedor {
    private Object[] datos;
    private int indice;

    public Contenedor(int capacidad) {
        datos = new Object[capacidad];
        indice = 0;
    }

    public void insertar(Object elemento) {
        datos[indice++] = elemento;
    }
}

Este tipo de soluciones permite flexibilidad, pero carece de seguridad de tipos, ya que los errores solo se detectan en tiempo de ejecución. Precisamente estas limitaciones son una de las principales motivaciones para la introducción de la genericidad en lenguajes como Java, que permite expresar estructuras de datos generales sin perder control estático de los tipos.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La programación genérica es un paradigma que consiste en definir algoritmos y estructuras de datos de forma independiente del tipo concreto de los datos que manejan. 

En lugar de escribir una versión distinta de un mismo código para cada tipo (por ejemplo, enteros, reales o cadenas), se expresa el comportamiento de manera abstracta mediante parámetros de tipo. El objetivo principal es reutilizar código sin perder seguridad de tipos, permitiendo que el compilador verifique que los usos de los tipos son correctos antes de ejecutar el programa.

En lenguajes como Java, la programación genérica se materializa mediante genéricos (<T>, <E>, etc.), y en C++ mediante plantillas. Estos mecanismos permiten que una estructura, como una lista o una pila, se defina una sola vez y se utilice con distintos tipos, manteniendo información precisa sobre el tipo de los elementos almacenados. 

Esto evita conversiones manuales y reduce errores que, de otro modo, solo se detectarían en tiempo de ejecución.
El ejemplo anterior basado en void* en C o Object en Java no es programación genérica en sentido estricto, aunque persigue un objetivo similar. 

Se trata de una solución previa a la existencia de mecanismos genéricos, que permite almacenar datos de cualquier tipo sacrificando la comprobación de tipos en tiempo de compilación. En estos casos, el programador asume la responsabilidad de recordar el tipo real de los datos y realizar conversiones explícitas, lo que hace el código más frágil y propenso a errores.

Por tanto, dicho ejemplo puede considerarse como una aproximación rudimentaria o “manual” a la genericidad, pero no como programación genérica propiamente dicha. La programación genérica auténtica se caracteriza por integrar el concepto de tipo genérico directamente en el lenguaje, de forma que el compilador participe activamente en la verificación de la corrección del programa.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El principal problema respecto al chequeo de tipos al emplear void* en C o Object en Java es que se pierde la verificación estática de los tipos en tiempo de compilación. El compilador deja de conocer el tipo real de los elementos almacenados en la estructura de datos y, por tanto, no puede detectar accesos incorrectos, asignaciones incompatibles o usos erróneos. 

Como consecuencia, muchos errores que podrían detectarse antes de ejecutar el programa pasan a manifestarse únicamente en tiempo de ejecución.

En el caso de C con void*, cualquier puntero puede almacenarse y recuperarse sin que el compilador compruebe su coherencia. Al extraer un elemento, es obligatorio realizar una conversión explícita al tipo esperado, pero el compilador no puede verificar si dicha conversión es correcta. Si el tipo asumido no coincide con el tipo real del dato apuntado, el comportamiento del programa queda indefinido, pudiendo provocar errores difíciles de depurar o incluso fallos en la ejecución.

En Java, aunque el uso de Object es algo más seguro que void*, el problema fundamental es similar. Al recuperar un elemento almacenado como Object, es necesario aplicar casting al tipo concreto esperado. Si el objeto no es realmente de ese tipo, se produce una excepción ClassCastException en tiempo de ejecución. El compilador no puede prevenir este tipo de errores si no se dispone de información más precisa sobre los tipos almacenados.

En ambos casos, el uso de void* u Object obliga al programador a gestionar manualmente la coherencia de tipos, incrementando la probabilidad de errores y haciendo el código más complejo y frágil. 

Precisamente para resolver estos problemas, la programación genérica introduce mecanismos que permiten mantener la flexibilidad de las estructuras de datos generales sin renunciar al chequeo de tipos en tiempo de compilación.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los parámetros de tipo son un mecanismo de la programación genérica que permite definir clases, interfaces o métodos de forma abstracta respecto a los tipos de datos que manejan. 

En lugar de fijar un tipo concreto (int, String, etc.), se introduce un identificador de tipo, normalmente una letra mayúscula como T o E, que actúa como un marcador de posición. Este parámetro será sustituido por un tipo concreto cuando la estructura o el método se utilice, manteniendo así la generalidad del código.

En Java, los parámetros de tipo se especifican entre los símbolos < > y forman parte de la definición del elemento genérico. Por ejemplo, una clase puede definirse como ClaseGenerica<T>, indicando que T representa un tipo aún desconocido. Cuando se crea un objeto de esa clase, ese parámetro se concreta, como en ClaseGenerica<Integer> o ClaseGenerica<String>. De este modo, el compilador conoce el tipo exacto con el que se está trabajando en cada uso concreto.

La principal ventaja de los parámetros de tipo es que permiten combinar flexibilidad con chequeo estático de tipos. A diferencia del uso de Object, el compilador puede verificar que solo se insertan y se recuperan valores del tipo adecuado, evitando conversiones explícitas y posibles errores en tiempo de ejecución. 

Esto mejora tanto la seguridad como la legibilidad del código.
En términos conceptuales, los parámetros de tipo pueden compararse con variables ordinarias, pero en lugar de almacenar valores, representan tipos. Esta idea resulta especialmente potente para definir estructuras de datos reutilizables, como listas, pilas o colas, que funcionan de la misma manera independientemente del tipo de los elementos que contienen, sin renunciar al control de tipos que ofrece el lenguaje.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
En Java, la programación genérica se materializa mediante generics, que permiten parametrizar clases y colecciones con un tipo concreto. Una lista genérica puede declararse indicando explícitamente que solo almacenará objetos de tipo String. 

De este modo, el compilador impide la inserción de elementos de otro tipo y garantiza que, al recorrer la lista, cada elemento recuperado es efectivamente un String, sin necesidad de conversiones explícitas. Esto supone una mejora clara respecto al uso de Object, ya que el chequeo de tipos se realiza en tiempo de compilación.

import java.util.ArrayList;
import java.util.List;

public class EjemploJava {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Programación");
        lista.add("Genérica");

        for (String s : lista) {
            System.out.println(s.toUpperCase());
        }
    }
}

En C++, el mecanismo equivalente son las templates (plantillas), que permiten definir clases y estructuras parametrizadas por tipo sin pérdida de eficiencia ni seguridad. 

Un contenedor genérico como std::vector puede instanciarse indicando el tipo concreto que contendrá, en este caso std::string. El compilador genera una versión específica del vector para ese tipo, asegurando que solo se almacenen cadenas y que los elementos recuperados sean tratados directamente como tales.

#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> v;

    v.push_back("Hola");
    v.push_back("Programación");
    v.push_back("Genérica");

    for (const std::string& s : v) {
        std::cout << s << std::endl;
    }
}

En ambos lenguajes, el uso de generics o templates permite expresar claramente la intención del programador: la estructura solo admite un tipo concreto y lo maneja de forma segura. 

El recorrido de los elementos no requiere conversiones ni comprobaciones manuales, ya que el compilador conoce el tipo exacto en todo momento. Esto ejemplifica la programación genérica en su forma más representativa: reutilización de estructuras con total seguridad de tipos y sin penalización conceptual ni práctica para el programador.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Cuando se instancia una clase con parámetros de tipo, el compilador debe decidir cómo manejar esa información genérica para producir código ejecutable.

El parámetro de tipo no existe como tal en tiempo de ejecución, por lo que el compilador aplica una transformación que permite reutilizar el código genérico con tipos concretos manteniendo las reglas del lenguaje. El comportamiento exacto depende del lenguaje y del modelo de implementación de la genericidad que este adopte.

En Java, el compilador aplica un mecanismo denominado type erasure (borrado de tipos). Durante la compilación, los parámetros de tipo se eliminan y se sustituyen por su límite superior, que por defecto es Object si no se especifica otro. Así, una clase como List<String> y otra como List<Integer> comparten exactamente el mismo código bytecode. 

El compilador inserta automáticamente conversiones de tipo (casts) allí donde son necesarias y verifica en tiempo de compilación que los usos sean correctos, aunque en tiempo de ejecución no exista información sobre el tipo genérico concreto.

En C++, el enfoque es completamente distinto y se basa en la instanciación de plantillas. Cuando se utiliza una plantilla con un tipo concreto, el compilador genera una versión específica del código para ese tipo. Por ejemplo, vector<string> y vector<int> son clases distintas tras la compilación, cada una con su propio código. 

Esto permite mantener la información completa del tipo en tiempo de compilación y de ejecución, y posibilita optimizaciones más agresivas por parte del compilador, a costa de aumentar el tamaño del código generado.

Por tanto, C++ y Java no hacen lo mismo al instanciar clases genéricas. Java prioriza la compatibilidad binaria y la reutilización del código mediante el borrado de tipos, mientras que C++ prioriza la especialización y la eficiencia mediante la generación de código específico para cada tipo. Ambos enfoques permiten programación genérica con seguridad de tipos, pero con implicaciones distintas en rendimiento, diagnóstico de errores y comportamiento en tiempo de ejecución.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
En Java, una clase con parámetros de tipo puede definirse para representar una entidad que agrupa valores de tipos distintos sin perder seguridad de tipos. Un ejemplo típico es una clase Par, que permite almacenar dos elementos potencialmente heterogéneos. Para ello, la clase se declara con dos parámetros de tipo, por ejemplo <A, B>, que representan de forma abstracta los tipos de los dos valores contenidos. Estos parámetros se concretan cuando la clase se utiliza.

La clase Par puede incluir un constructor que inicialice ambos valores y métodos getter para acceder a cada uno de ellos. Gracias a los genéricos, el compilador garantiza que los tipos utilizados al crear el objeto son coherentes y que los valores recuperados mediante los getter tienen el tipo correcto, sin necesidad de conversiones explícitas. Esto permite expresar de manera clara que cada componente del par tiene un tipo distinto y conocido.

public class Par<A, B> {
    private A primero;
    private B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}

Un uso habitual de este tipo de clase es modelar valores de retorno compuestos. Por ejemplo, una función puede devolver simultáneamente la media y la desviación típica de un array de double utilizando un Par<Double, Double>. De este modo, se evita crear clases específicas para cada caso o recurrir a estructuras menos seguras, manteniendo un diseño claro y tipado.

public static Par<Double, Double> mediaYDesviacion(double[] datos) {
    double suma = 0.0;
    for (double d : datos) {
        suma += d;
    }
    double media = suma / datos.length;

    double varianza = 0.0;
    for (double d : datos) {
        varianza += (d - media) * (d - media);
    }
    double desviacion = Math.sqrt(varianza / datos.length);

    return new Par<>(media, desviacion);
}

// Ejemplo de uso
double[] valores = {2.0, 4.0, 6.0, 8.0};
Par<Double, Double> resultado = mediaYDesviacion(valores);
System.out.println(resultado.getPrimero());  // media
System.out.println(resultado.getSegundo());  // desviación típica

Este ejemplo ilustra cómo los parámetros de tipo permiten definir estructuras reutilizables y expresivas, en las que el compilador conoce exactamente el tipo de cada componente, reforzando la seguridad y la claridad del código.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
En Java, además de definir parámetros de tipo a nivel de clase, es posible declararlos a nivel de método, lo que permite que un método sea genérico aunque la clase que lo contiene no lo sea. Un método genérico introduce sus propios parámetros de tipo justo antes del tipo de retorno. Esto resulta especialmente útil para definir operaciones reutilizables y tipadas de forma segura, sin obligar a parametrizar toda la clase.

Si el método seleccionaUno se define utilizando Object, el método puede aceptar dos objetos cualesquiera y devolver uno de ellos de forma aleatoria. Sin embargo, el tipo del valor devuelto será Object, por lo que quien invoque el método deberá realizar un downcasting explícito para recuperar el tipo concreto. Además, el compilador no puede garantizar que ambos parámetros sean del mismo tipo, lo que abre la puerta a usos incorrectos.

public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso
String s = (String) seleccionaUno("Hola", "Adiós");

Si el mismo método se define mediante un parámetro de tipo, el compilador puede forzar que ambos argumentos sean del mismo tipo y garantizar que el valor devuelto es de ese mismo tipo concreto. De esta forma, se elimina completamente la necesidad de casting y se detectan usos erróneos en tiempo de compilación, no en ejecución.

public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso seguro
String s = seleccionaUno("Hola", "Adiós");
// seleccionaUno("Hola", 3);  // Error de compilación

En términos comparativos, el método basado en Object no evita el downcasting y no impide mezclar tipos distintos, mientras que el método genérico garantiza ambas cosas de forma explícita. Este ejemplo ilustra claramente cómo los parámetros de tipo a nivel de método refuerzan la seguridad de tipos y expresan mejor las restricciones lógicas que debe cumplir el código.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí, en Java es posible establecer restricciones en los parámetros de tipo mediante bounded type parameters. Esto permite indicar que un parámetro genérico debe ser, como mínimo, una subclase de una clase concreta o implementar una interfaz determinada. 

La sintaxis general es <T extends TipoBase>. De este modo, el compilador garantiza que T posee, al menos, las operaciones definidas en TipoBase, lo que permite tratar los objetos genéricos conforme a esas capacidades sin perder seguridad de tipos.

Una primera solución, sin genéricos, consiste en definir un Punto cuyas coordenadas sean simplemente de tipo Number. Esto permite utilizar cualquier subtipo numérico (Integer, Double, Float, etc.), pero se pierde información sobre el tipo concreto de número con el que se trabaja. Además, todas las operaciones numéricas requieren convertir explícitamente a un tipo concreto, normalmente double, lo que reduce la precisión del chequeo de tipos en tiempo de compilación.

public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

Una segunda solución más robusta introduce genéricos con restricción, reforzando el chequeo de tipos y dejando claro con qué tipo numérico trabaja el punto. En este caso, se define la clase como Punto<T extends Number>, lo que obliga a que ambas coordenadas sean del mismo tipo numérico concreto. Esto evita mezclar, por ejemplo, Integer con Double sin que el programador sea consciente, y documenta mejor la intención del código.

public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

Respecto al type erasure, en ambos casos el tipo genérico se elimina tras la compilación. En la versión genérica, Punto<T extends Number> se transforma internamente en una clase que trabaja con Number, ya que ese es el límite superior del parámetro de tipo. 

Es decir, el tipo final en tiempo de ejecución es Punto con campos de tipo Number, de forma similar a la primera solución, aunque durante la compilación sí se ha beneficiado de un control de tipos mucho más estricto y expresivo.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
Ambas soluciones permiten reutilizar la clase Punto para trabajar con distintos tipos numéricos, pero difieren de forma significativa en el refuerzo del chequeo de tipos que ofrece el compilador. 

En la solución sin genéricos, al declarar las coordenadas como Number, no existe ninguna restricción que obligue a que ambas coordenadas sean del mismo tipo concreto. Por tanto, es perfectamente posible crear un punto cuya coordenada x sea un Integer y cuya coordenada y sea un Double, sin que el compilador lo considere un error.

En cambio, en la solución con genéricos (Punto<T extends Number>), el parámetro de tipo T se utiliza para ambas coordenadas. Esto implica que, al crear un objeto, el tipo concreto se fija una única vez y debe ser el mismo para x e y. 

En consecuencia, no se puede crear un punto con una coordenada entera y otra real dentro de la misma instancia, ya que el compilador forzará que ambas sean, por ejemplo, Integer o ambas Double. Este es un claro ejemplo de cómo los genéricos permiten expresar restricciones lógicas del dominio directamente en el sistema de tipos.

Respecto al tipo devuelto por los métodos de acceso, también se aprecia una diferencia relevante. En la solución sin genéricos, el método getX devuelve siempre un Number, independientemente del tipo real almacenable. Esto obliga a tratar el valor de forma genérica o a realizar conversiones posteriores si se necesita un tipo concreto. El compilador, en este caso, no puede aportar más información sobre el tipo exacto del valor retornado.

En la solución con genéricos, el método getX devuelve el tipo T, que es el subtipo concreto de Number usado al crear el objeto. Esto significa que, si se declara un Punto<Integer>, getX devuelve un Integer, y si se declara un Punto<Double>, devuelve un Double. Este refuerzo del chequeo de tipos mejora la expresividad del código y permite detectar incoherencias en tiempo de compilación, incluso aunque tras el type erasure ambos enfoques terminen trabajando internamente con Number.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta
En este ejemplo, el problema principal es que la interfaz Punto no expresa ninguna relación entre el tipo del objeto que recibe el método distanciaA y el tipo concreto de la clase que lo implementa. 

Debido a ello, el método acepta cualquier implementación de Punto, obligando a usar instanceof y downcasting para comprobar en tiempo de ejecución si el argumento es compatible. Esto es un síntoma claro de que falta información en el sistema de tipos que debería haberse expresado en la interfaz.

La solución consiste en introducir genéricos autorreferenciados (un patrón similar al CRTP de C++) en la interfaz. La interfaz se define con un parámetro de tipo que representa “el propio tipo de punto”, y dicho parámetro se restringe para que extienda a la interfaz misma. De este modo, el método distanciaA queda tipado para aceptar únicamente puntos del mismo subtipo concreto, y el compilador se encarga de garantizar esta coherencia.

public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

Cada implementación concreta especifica ahora explícitamente su propio tipo al implementar la interfaz. Esto elimina cualquier necesidad de comprobaciones dinámicas, ya que el método recibe directamente el tipo correcto. El compilador impide automáticamente que se intente calcular la distancia entre un Punto2D y un Punto3D.

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                       + Math.pow(y - p.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2)
                       + Math.pow(y - p.y, 2)
                       + Math.pow(z - p.z, 2));
    }
}

Con esta versión, el chequeo de tipos se refuerza notablemente: ya no es posible pasar un punto de dimensión distinta, ni se necesita instanceof ni conversiones explícitas. El diseño expresa de forma clara y segura una restricción lógica del dominio —la distancia solo tiene sentido entre puntos del mismo tipo— y delega por completo su cumplimiento en el compilador.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
Aunque String es un subtipo de Object, no significa que List<String> sea subtipo de List<Object>. En cambio, sí es cierto que String[] es subtipo de Object[]. Esta diferencia se debe a cómo Java define la relación de subtipado para los genéricos frente a los arrays, y a las garantías (o riesgos) que cada mecanismo asume en tiempo de compilación y ejecución.

Las colecciones genéricas en Java son invariantes respecto a su parámetro de tipo. Esto significa que, aunque String herede de Object, List<String> y List<Object> se consideran tipos completamente distintos e incompatibles. 

Si se permitiese esa relación de subtipado, sería posible insertar un Integer en una lista que conceptualmente debería contener solo String, rompiendo la seguridad de tipos. Por ello, el compilador rechaza directamente ese uso y evita el problema antes de ejecutar el programa.

Los arrays, en cambio, son covariantes en Java: si String es subtipo de Object, entonces String[] es subtipo de Object[]. Esto permite asignaciones como Object[] a = new String[10];, que resultan útiles en algunos contextos antiguos del lenguaje. Sin embargo, esta decisión introduce un riesgo: a través de la referencia Object[] se puede intentar almacenar un objeto que no sea un String, provocando un error en tiempo de ejecución.

Object[] a = new String[2];
a[0] = "Hola";
a[1] = 5; // ArrayStoreException en tiempo de ejecución

A partir de estos ejemplos, se pueden definir los conceptos de variancia. Un tipo genérico es covariante si Contenedor<Subtipo> es subtipo de Contenedor<Tipo>; es contravariante si ocurre lo contrario; y es invariante si no existe ninguna relación de subtipado entre Contenedor<Subtipo> y Contenedor<Tipo>, aunque exista entre sus parámetros. 

En Java, los arrays son covariantes, las clases genéricas son invariantes por defecto, y la covarianza o contravarianza controlada solo se introduce explícitamente mediante comodines (? extends, ? super) para mantener la seguridad de tipos sin introducir errores en tiempo de ejecución.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un wildcard (?) en Java es un marcador de tipo desconocido que se utiliza en tipos genéricos para relajar o ajustar las restricciones de subtipado de forma controlada. Permite expresar que una estructura genérica trabaja con algún tipo, sin necesidad de conocer exactamente cuál es, pero imponiendo límites sobre su relación con otros tipos. Los wildcards son el mecanismo mediante el cual Java recupera covarianza y contravarianza en genéricos sin comprometer la seguridad de tipos.
La forma List<? extends T> indica una cota superior: la lista contiene elementos de algún tipo que es T o un subtipo de T. Esta forma es covariante y se utiliza cuando la lista solo se va a leer. El compilador garantiza que cualquier elemento obtenido es, como mínimo, de tipo T, pero impide insertar nuevos elementos (salvo null), ya que no se conoce el subtipo exacto almacenado. Es el caso típico de métodos que procesan valores, como calcular una suma.

public static double sumar(List<? extends Number> nums) {
    double suma = 0.0;
    for (Number n : nums) {
        suma += n.doubleValue();
    }
    return suma;
}

La forma List<? super T> indica una cota inferior: la lista contiene elementos de tipo T o de algún supertipo de T. Esta forma es contravariante y se emplea cuando la lista se va a escribir. El compilador permite añadir objetos de tipo T, ya que son compatibles con cualquier supertipo, pero al leer solo se garantiza que los elementos son Object, al no conocerse el tipo más específico almacenado.

public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}

En resumen, ? extends T se utiliza cuando se necesita producir valores (lectura) y preservar covarianza, mientras que ? super T se usa cuando se necesita consumir valores (escritura) y permitir contravarianza. Esta distinción suele resumirse en la regla PECS (Producer Extends, Consumer Super), que ayuda a elegir correctamente el wildcard en función del uso previsto de la colección.