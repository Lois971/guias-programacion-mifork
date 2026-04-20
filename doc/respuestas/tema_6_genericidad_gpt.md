1. Estructura usando void* en C o Object en Java

Una forma clásica de simular genericidad sin soporte del lenguaje consiste en almacenar punteros sin tipo en C (void*) o referencias a Object en Java. En ambos casos, el contenedor no sabe realmente qué tipo de dato contiene, pero permite guardar cualquier cosa. Esto se logra usando un array primitivo y almacenando en él elementos sin información de tipo. El programador es responsable de recordar qué tipo real tiene cada elemento.

En C, esto se hace con void*, que puede apuntar a cualquier tipo. En Java, como todas las clases derivan de Object, un array de Object puede almacenar cualquier referencia. El inconveniente es que al recuperar un elemento es necesario hacer downcasting, lo que puede producir errores en tiempo de ejecución si el tipo no coincide.
c

// C con void*
typedef struct {
    void* datos[100];
    int size;
} Lista;

void add(Lista* l, void* elem) {
    l->datos[l->size++] = elem;
}

java

// Java con Object
class Lista {
    private Object[] datos = new Object[100];
    private int size = 0;

    public void add(Object o) { datos[size++] = o; }
    public Object get(int i) { return datos[i]; }
}

2. Qué significa programación genérica

La programación genérica consiste en escribir código que funciona para múltiples tipos sin duplicarlo. En lugar de trabajar con un tipo concreto, se parametriza el código con un tipo que se especificará más adelante. Esto permite crear estructuras de datos y algoritmos reutilizables, evitando errores y reduciendo la repetición. El compilador se encarga de verificar que el uso de los tipos es correcto.

El ejemplo anterior con void* u Object es una forma rudimentaria de genericidad, pero no es programación genérica real. No hay verificación de tipos en tiempo de compilación, y el programador debe hacer conversiones manuales. La programación genérica auténtica, como la que ofrecen Java y C++, añade seguridad de tipos y evita errores en tiempo de ejecución.
3. Problemas de usar void* u Object

El principal problema es la pérdida de información de tipos. Cuando se almacena un dato como void* u Object, el compilador deja de saber qué tipo real tiene. Esto obliga a hacer downcasting al recuperar el valor, lo que introduce riesgo de errores en tiempo de ejecución si el tipo no coincide. En C, esto puede incluso provocar corrupción de memoria.

Otro problema es que el compilador no puede detectar usos incorrectos. Por ejemplo, se puede insertar accidentalmente un tipo distinto al esperado, y el error no se detectará hasta que se intente usar. Esto hace que el código sea menos seguro y más difícil de mantener. La programación genérica moderna soluciona estos problemas mediante parámetros de tipo.
4. Qué son los parámetros de tipo

Los parámetros de tipo son variables que representan tipos, igual que los parámetros normales representan valores. Permiten definir clases y métodos que funcionan con tipos desconocidos hasta el momento de su uso. En Java se escriben entre < >, como <T>, y permiten que el compilador verifique que el código usa los tipos correctamente.

Gracias a los parámetros de tipo, una estructura de datos puede almacenar elementos de un tipo concreto sin perder seguridad. El compilador sustituye el parámetro por el tipo real cuando se instancia la clase o se llama al método. Esto evita conversiones manuales y reduce errores.
5. Ejemplo de generics en Java y templates en C++

En Java, los generics permiten crear colecciones seguras sin necesidad de conversiones. En C++, los templates generan código especializado para cada tipo. Ambos mecanismos permiten trabajar con tipos concretos sin perder seguridad.
java

// Java
List<String> lista = new ArrayList<>();
lista.add("Hola");
lista.add("Mundo");
for (String s : lista) {
    System.out.println(s.toUpperCase());
}

cpp

// C++
#include <vector>
#include <string>
#include <iostream>
int main() {
    std::vector<std::string> v;
    v.push_back("Hola");
    v.push_back("Mundo");
    for (const std::string& s : v) {
        std::cout << s << std::endl;
    }
}

6. Qué hace el compilador con los tipos genéricos

En Java, los generics funcionan mediante type erasure. Esto significa que el compilador elimina la información de tipos genéricos después de verificarla. En tiempo de ejecución, todos los tipos genéricos se convierten en Object. Esto mantiene compatibilidad con versiones antiguas del lenguaje, pero limita algunas operaciones, como crear arrays genéricos.

En C++, los templates funcionan de forma distinta: el compilador genera una copia del código para cada tipo concreto que se use. Esto se llama instanciación de plantillas. El resultado es que el tipo se conserva en tiempo de ejecución, y se pueden hacer operaciones que en Java no son posibles, como crear arrays del tipo genérico.
7. Clase genérica Par

Una clase genérica permite almacenar dos valores de tipos distintos sin perder seguridad. El compilador verifica que los tipos usados coinciden con los declarados. Esto permite devolver múltiples valores de forma clara y segura.
java

class Par<A, B> {
    private A primero;
    private B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() { return primero; }
    public B getSegundo() { return segundo; }
}

Ejemplo de uso:
java

Par<Double, Double> estadisticas = calcularMediaYDesviacion(datos);

8. Parámetros de tipo a nivel de método

Los métodos genéricos permiten definir parámetros de tipo solo para un método concreto. Esto evita conversiones y garantiza que los argumentos sean del mismo tipo. Si se usaran Object, habría que hacer downcasting y no se detectaría si los tipos no coinciden.
java

// Versión insegura
public Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}

// Versión genérica segura
public <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}

9. Restricciones en parámetros de tipo

En Java se pueden restringir los parámetros de tipo usando extends. Esto permite exigir que un tipo sea, como mínimo, un subtipo de otro. Para trabajar con números, se puede exigir que el tipo sea Number, lo que permite usar métodos como doubleValue().

Primera solución (sin generics avanzados):
java

class Punto {
    private Number x, y;
}

Segunda solución (con generics):
java

class Punto<T extends Number> {
    private T x, y;
}

Tras type erasure, el tipo real en tiempo de ejecución es Number.
10. Comparación de ambas soluciones

La solución sin generics permite mezclar tipos, como un Integer y un Double, porque ambos son Number. Esto puede ser útil, pero también puede introducir inconsistencias. El método getX() devuelve siempre un Number, por lo que el usuario debe convertirlo manualmente al tipo deseado.

La solución con generics refuerza el chequeo de tipos: un Punto<Integer> solo puede tener enteros, y un Punto<Double> solo reales. Esto evita mezclar tipos dentro del mismo punto. Además, getX() devuelve exactamente el tipo T, sin necesidad de conversiones.
11. Ejemplo avanzado con generics en interfaz Punto

Se puede parametrizar la interfaz para asegurar que cada implementación solo acepte puntos del mismo tipo. Esto elimina la necesidad de instanceof y downcasting, ya que el compilador garantiza que los tipos coinciden.
java

public interface Punto<T extends Punto<T>> {
    double distanciaA(T otro);
}

public class Punto2D implements Punto<Punto2D> {
    private double x, y;

    @Override
    public double distanciaA(Punto2D p) {
        return Math.hypot(x - p.x, y - p.y);
    }
}

12. ¿List<String> es subtipo de List<Object>? ¿Y String[] de Object[]?

En Java, List<String> no es subtipo de List<Object>. Los tipos genéricos son invariantes para evitar errores. Si se permitiera, sería posible insertar un Integer en una lista que se supone de String. En cambio, los arrays sí son covariantes: un String[] es subtipo de Object[]. Esto puede causar errores en tiempo de ejecución, como ArrayStoreException.

La covarianza significa que un tipo parametrizado con un subtipo puede usarse donde se espera el supertipo. La contravarianza es lo contrario: aceptar supertipos. La invariancia significa que no hay relación entre List<A> y List<B> aunque A sea subtipo de B.
13. Wildcards: ? extends y ? super

Un wildcard (?) representa un tipo desconocido. ? extends T indica que el tipo es T o un subtipo, y se usa para leer datos de forma segura. ? super T indica que el tipo es T o un supertipo, y se usa para escribir datos de forma segura.

Ejemplo con ? extends:
java

double suma(List<? extends Number> lista) {
    double total = 0;
    for (Number n : lista) total += n.doubleValue();
    return total;
}

Ejemplo con ? super:
java

void añadeEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
}

