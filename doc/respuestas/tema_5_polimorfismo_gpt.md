1. Qué es el polimorfismo y qué es la sobreescritura

El polimorfismo es la capacidad de que un mismo mensaje (una llamada a un método) produzca comportamientos distintos según el tipo real del objeto que lo recibe. Permite tratar objetos diferentes a través de una referencia común, normalmente la de una superclase, y que cada uno responda de forma específica. Esto facilita diseñar programas extensibles, donde nuevas clases pueden integrarse sin modificar el código que ya usa la superclase.

La sobreescritura de métodos consiste en redefinir en una subclase un método que ya existía en la clase base, sustituyendo su comportamiento. Para que haya sobreescritura, el método debe tener exactamente la misma firma (nombre y parámetros) y un tipo de retorno compatible. Gracias a la sobreescritura, el polimorfismo puede funcionar, ya que la llamada al método se resuelve según el tipo dinámico del objeto.
2. Ligadura dinámica y relación con el polimorfismo

La ligadura dinámica, o enlace tardío, es el mecanismo por el cual la elección del método que se ejecuta se decide en tiempo de ejecución y no en tiempo de compilación. Esto permite que una referencia de tipo base invoque métodos que han sido sobreescritos en subclases, activando así el polimorfismo. El lenguaje determina si este enlace es automático o si debe indicarse explícitamente.

En C++, la ligadura dinámica solo ocurre si se usa la palabra clave virtual en los métodos de la clase base. Si no se usa, el enlace es estático y no hay polimorfismo real. En Java, en cambio, todos los métodos no static ni final usan ligadura dinámica automáticamente, por lo que el programador no debe indicar nada especial. En Python, la ligadura dinámica es la norma: todo se resuelve en tiempo de ejecución y no existe distinción entre métodos virtuales o no.
3. Ejemplo con Soldado, Zapador y Artillero

En este ejemplo, Zapador sobreescribe completamente el método saluda, mientras que Artillero hereda el comportamiento tal cual. El polimorfismo se observa al recorrer un array de referencias de tipo Soldado, donde cada objeto responde según su tipo real.
java

class Soldado {
    public void saluda() {
        System.out.println("Soldado saludando");
    }
}

class Zapador extends Soldado {
    @Override
    public void saluda() {
        System.out.println("Zapador saludando con estilo propio");
    }
}

class Artillero extends Soldado {
}

public class Main {
    public static void main(String[] args) {
        Soldado[] escuadra = { new Zapador(), new Artillero() };

        for (Soldado s : escuadra) {
            s.saluda(); // polimorfismo
        }
    }
}

4. Invocar al método base desde una sobreescritura

Cuando se sobreescribe un método, es posible invocar el método original de la clase base para reutilizar parte de su comportamiento. Esto permite extender la funcionalidad en lugar de reemplazarla completamente. En Java, la palabra clave utilizada para acceder al método de la clase base es super.

En el siguiente ejemplo, Zapador llama primero al saludo estándar del soldado y luego añade su propio mensaje. Esto demuestra cómo combinar herencia y sobreescritura para construir comportamientos más complejos.
java

class Zapador extends Soldado {
    @Override
    public void saluda() {
        super.saluda();
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}

5. Restricciones al sobreescribir y diferencia con sobrecarga

Al sobreescribir un método en Java, los parámetros deben ser exactamente los mismos y el tipo de retorno debe ser igual o un subtipo (covariante). Además, no se pueden reducir las restricciones de visibilidad, aunque sí ampliarlas. La sobreescritura siempre implica polimorfismo, ya que redefine un método heredado.

La sobrecarga, en cambio, consiste en definir varios métodos con el mismo nombre pero con parámetros distintos. No tiene relación con el polimorfismo dinámico, ya que la elección del método se hace en tiempo de compilación. La anotación @Override sirve para indicar explícitamente que un método está siendo sobreescrito, y es recomendable porque permite detectar errores si la firma no coincide exactamente.
6. ¿Se usa polimorfismo desde el principio en Java?

En Java se emplea polimorfismo desde los primeros pasos, incluso sin darse cuenta. Cuando se sobreescribe toString, equals o hashCode, se está aplicando polimorfismo, ya que estas llamadas se resuelven dinámicamente según el tipo real del objeto. Esto ocurre aunque se trabaje con referencias de tipo Object.

El polimorfismo está tan integrado en el lenguaje que forma parte del diseño básico de sus bibliotecas. Por ello, incluso en programas sencillos, se está utilizando sin necesidad de conocer todavía todos sus detalles teóricos.
7. Clases abstractas y métodos abstractos

Una clase abstracta es una clase que no puede instanciarse directamente y que sirve como plantilla para otras clases. Puede contener métodos normales y métodos abstractos. Un método abstracto es aquel que se declara sin implementación, obligando a las subclases a definirlo. Esto permite diseñar jerarquías donde ciertas acciones deben ser implementadas por cada subtipo.

En el ejemplo siguiente, Soldado se convierte en abstracto y define un método abstracto atacar. Cada tipo de soldado implementa su propia versión. La palabra clave abstract debe colocarse tanto en la clase como en el método.
java

abstract class Soldado {
    public void saluda() {
        System.out.println("Soldado saludando");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Zapador colocando explosivos");
    }
}

8. Efecto de final en métodos y clases

La palabra clave final aplicada a un método impide que sea sobreescrito en subclases. Esto limita el polimorfismo, ya que evita que el comportamiento pueda modificarse dinámicamente. Cuando se aplica a una clase, impide que pueda heredarse, bloqueando completamente la extensión mediante subclases.

Este mecanismo se usa para garantizar seguridad, estabilidad o evitar modificaciones no deseadas. Un ejemplo clásico de clase final en la API estándar de Java es String, que no puede heredarse para evitar problemas de seguridad y coherencia interna.
9. Qué son las interfaces en Java

Las interfaces son contratos que especifican un conjunto de métodos que una clase debe implementar. A diferencia de las clases abstractas, no contienen estado (salvo constantes) y permiten definir comportamientos comunes sin imponer una jerarquía de herencia. Esto permite que clases no relacionadas compartan capacidades.

Una clase puede implementar múltiples interfaces, lo que proporciona una forma de herencia múltiple de comportamiento sin los problemas de la herencia múltiple clásica. Las interfaces son fundamentales en Java para diseñar sistemas flexibles y desacoplados.
10. Ejemplo con Punto 2D, Punto 3D y Linea

En este diseño, Punto es abstracto y define un método abstracto para calcular la distancia. Las subclases implementan el cálculo según sus dimensiones. Se usa instanceof y downcasting para asegurar que ambos puntos son del mismo tipo antes de calcular la distancia.
java

abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;

    public Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) throw new IllegalArgumentException();
        Punto2D p = (Punto2D) otro;
        return Math.hypot(x - p.x, y - p.y);
    }
}

class Punto3D extends Punto {
    double x, y, z;

    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) throw new IllegalArgumentException();
        Punto3D p = (Punto3D) otro;
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
    }
}

class Linea {
    private Punto a, b;

    public Linea(Punto a, Punto b) { this.a = a; this.b = b; }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}

11. Herencia de interfaces

La herencia de interfaces consiste en que una interfaz puede extender a otra, heredando sus métodos. Esto permite construir jerarquías de capacidades sin imponer una estructura rígida de clases. En Java existe herencia múltiple de interfaces, ya que una interfaz puede extender varias a la vez.

En el siguiente ejemplo, Fichero define un método de lectura, mientras que FicheroEscribible amplía la interfaz añadiendo métodos para escribir y borrar. Esto permite que una clase implemente capacidades adicionales sin modificar la interfaz original.
java

interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void borrar();
}