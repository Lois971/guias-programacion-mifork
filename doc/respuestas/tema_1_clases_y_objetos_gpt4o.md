# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Las cuatro características básicas de la programación orientada a objetos son **encapsulación**, **abstracción**, **herencia** y **polimorfismo**. La **encapsulación** consiste en agrupar datos (atributos) y comportamiento (métodos) dentro de una misma entidad (la clase) y controlar el acceso a sus datos mediante modificadores de visibilidad. La **abstracción** permite modelar conceptos del mundo real en clases, seleccionando solo los detalles relevantes y ocultando la complejidad interna.

La **herencia** permite que una clase derive de otra, heredando atributos y métodos, lo que favorece la reutilización de código y la creación de jerarquías. El **polimorfismo** permite que una misma operación se comporte de manera distinta según el tipo del objeto con el que se esté trabajando, por ejemplo, mediante métodos sobreescritos o interfaces. Estas características combinadas permiten diseñar programas más modularizados, mantenibles y cercanos al dominio real.

---

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Cuatro lenguajes populares que permiten la programación orientada a objetos son **Java**, **C++**, **Python** y **C#**. Java fue diseñado desde sus inicios con un enfoque totalmente orientado a objetos, y su modelo de clases es muy claro y estructurado. C++ soporta la orientación a objetos como una extensión del lenguaje C, manteniendo también programación procedural y baja abstracción cuando se requiere.

Python ofrece un modelo de objetos dinámico y flexible, donde casi todo es un objeto, y su sintaxis facilita la creación rápida de clases. C# es un lenguaje moderno orientado a objetos desarrollado por Microsoft, con un modelo de clases similar al de Java y con soporte fuerte para programación orientada a objetos en entornos .NET.

---

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La **programación estructurada** es un paradigma que se basa en el uso de estructuras de control como secuencias, condicionales e iteraciones, evitando el uso indiscriminado de saltos incondicionales (por ejemplo, `goto`). Su objetivo es mejorar la legibilidad y el control del flujo del programa, promoviendo una organización más clara y predecible del código. En este enfoque, se prioriza la división del programa en bloques lógicos con una entrada y salida definidas.

La **programación modular** es una evolución de la estructurada que consiste en dividir el programa en módulos independientes, cada uno con una funcionalidad específica y bien definida. Un módulo puede ser una función, un archivo, o una unidad lógica que agrupa varias funciones relacionadas. La modularidad facilita la reutilización, el mantenimiento y la colaboración, ya que cada módulo se puede desarrollar, probar y modificar sin afectar al resto del sistema.

---

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Un objeto se define por **estado**, **comportamiento** e **identidad**. El **estado** se representa por los valores de sus atributos en un momento dado. El **comportamiento** está definido por los métodos que permiten operar sobre esos atributos o realizar acciones relacionadas con el objeto. La **identidad** es lo que distingue a un objeto de otro, incluso si tienen el mismo estado; dos objetos pueden tener los mismos valores pero seguir siendo entidades distintas en memoria.

El estado y el comportamiento se encapsulan dentro de la clase que define el tipo del objeto, mientras que la identidad se mantiene mediante una referencia única en memoria o un identificador interno del sistema. Esta combinación permite modelar entidades del mundo real con una estructura clara y manipulable.

---

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una **clase** es una plantilla o un “molde” que define los atributos y métodos que caracterizan a un tipo de objeto. No es un objeto por sí misma, sino la definición de cómo debe ser un objeto. Una **instancia** es un objeto concreto creado a partir de una clase; es decir, cuando se crea un objeto en memoria se dice que se está instanciando una clase.

No todos los lenguajes orientados a objetos manejan el concepto de clase de la misma forma. Lenguajes como Java, C++ o C# son **class-based**, es decir, basados en clases. Otros, como JavaScript (en su esencia), Ruby o Python, permiten un modelo más flexible basado en **prototipos** o en objetos dinámicos, donde las clases pueden existir o no según el diseño del lenguaje. En esos casos, los objetos se pueden crear y modificar sin necesidad de una clase explícita.

---

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**?

### Respuesta

En muchos lenguajes modernos, los objetos se almacenan en el **heap** (memoria dinámica), mientras que las referencias a esos objetos se guardan en la pila (stack) o en variables de contexto. Sin embargo, no es igual en todos los lenguajes; por ejemplo, en C++ los objetos pueden crearse en stack o en heap según cómo se declaren. En Java, casi todos los objetos se crean en el heap y su gestión de memoria se delega al sistema.

La **recolección de basura** (garbage collection) es un mecanismo automático que libera memoria de objetos que ya no tienen referencias activas, evitando fugas de memoria. El recolector analiza qué objetos siguen accesibles desde variables o estructuras activas y libera los que no lo son. No todos los lenguajes tienen recolección automática; en C++ el programador debe gestionar la memoria manualmente, aunque existen herramientas como smart pointers para ayudar.

---

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**?

### Respuesta

Un **método** es una función asociada a una clase que define un comportamiento de los objetos de ese tipo. Los métodos pueden operar sobre los atributos del objeto y pueden recibir parámetros, devolver valores o realizar acciones. En el paradigma orientado a objetos, los métodos son la forma principal de interactuar con el estado interno de un objeto de manera controlada.

La **sobrecarga de métodos** es la posibilidad de definir varios métodos con el mismo nombre pero con diferentes listas de parámetros (diferente número o tipos). El compilador o intérprete selecciona el método adecuado en función de los argumentos con los que se llama. Esto permite usar el mismo nombre de método para acciones similares que se aplican a diferentes tipos de datos o diferentes cantidades de información.

---

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

```java
public class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen()); // 5.0
    }
}
9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es static y para qué vale? ¿Sólo se emplea para ese método main? ¿Para qué se combina con final?
Respuesta

El punto de entrada en un programa Java es el método main, con la firma public static void main(String[] args). Es el método que la JVM ejecuta al iniciar el programa y debe ser static para poder invocarse sin crear una instancia de la clase. El modificador static indica que un elemento pertenece a la clase y no a un objeto concreto, por lo que se puede acceder sin instanciarla.

El static no se usa solo en main; también se puede aplicar a atributos y métodos que deban existir una única vez para toda la clase (por ejemplo, un contador de instancias). final se usa para indicar que un valor no puede cambiar (en variables) o que un método no puede ser sobreescrito, o que una clase no puede tener subclases. En combinación, static final se utiliza para constantes de clase, como static final double PI = 3.1415.

10. Intenta ejecutar un poco de Java de forma básica, con los comandos javac y java. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la máquina virtual? ¿Qué es el byte-code y los ficheros .class?
Respuesta

Java es un lenguaje compilado e interpretado. El código fuente (.java) se compila con javac a bytecode, que se almacena en archivos .class. Ese bytecode no es código máquina nativo, sino un formato intermedio diseñado para ejecutarse en la JVM (Java Virtual Machine). La JVM interpreta o compila dinámicamente ese bytecode en código máquina para el sistema donde se ejecuta.

Para compilar y ejecutar desde línea de comandos, se usa:

javac Punto.java
java Punto


Esto genera un archivo Punto.class y ejecuta el programa. La JVM permite la portabilidad del código: el mismo bytecode puede ejecutarse en cualquier plataforma que tenga una JVM compatible.

11. En el código anterior de la clase Punto ¿Qué es new? ¿Qué es un constructor? Pon un ejemplo de constructor en una clase Empleado que tenga DNI, nombre y apellidos
Respuesta

La palabra clave new se usa para crear una nueva instancia de una clase, reservando memoria en el heap y devolviendo una referencia al objeto creado. Un constructor es un método especial que se ejecuta al crear un objeto y que inicializa sus atributos. El constructor tiene el mismo nombre que la clase y no devuelve valor.

Ejemplo en una clase Empleado:

public class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

12. ¿Qué es la referencia this? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de this en la clase Punto
Respuesta

La referencia this es un puntero o referencia que apunta al objeto actual dentro de un método o constructor. Se usa para acceder a los atributos y métodos del objeto que invocó el método. No todos los lenguajes usan el nombre this; por ejemplo, en Python se usa self, y en C++ también se usa this, pero el comportamiento y la sintaxis pueden variar según el lenguaje.

Ejemplo en Punto:

double calculaDistanciaAOrigen() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
}

13. Añade ahora otro nuevo método que se llame distanciaA, que reciba un Punto como parámetro y calcule la distancia entre this y el punto proporcionado
Respuesta
double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}

14. El paso del Punto como parámetro a un método, es por copia o por referencia, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un Punto, se recibiese un entero (int) y dicho entero se modificase dentro de la función?
Respuesta

En Java, los parámetros de tipo objeto se pasan por valor de la referencia. Esto significa que se copia la referencia al objeto, no el objeto completo. Por tanto, si dentro del método se modifica el estado del objeto referenciado (por ejemplo, cambiando atributos), esos cambios se reflejan fuera del método, porque ambos apuntan al mismo objeto. Sin embargo, si se asigna una nueva referencia dentro del método, no afecta a la referencia externa.

En cambio, los tipos primitivos como int se pasan por valor, es decir, se copia el valor. Si se modifica el parámetro dentro del método, el valor externo no cambia, porque se está trabajando con una copia independiente.

15. ¿Qué es el método toString() en Java? ¿Existe en otros lenguajes? Pon un ejemplo de toString() en la clase Punto en Java
Respuesta

toString() es un método definido en la clase base Object que devuelve una representación textual del objeto. Se suele sobreescribir para proporcionar una cadena significativa que describa el estado del objeto. Muchos lenguajes orientados a objetos tienen un equivalente, por ejemplo __str__ en Python o toString() en C#.

Ejemplo en Punto:

@Override
public String toString() {
    return "(" + x + ", " + y + ")";
}

16. Reflexiona: ¿una clase es como un struct en C? ¿Qué le falta al struct para ser como una clase y las variables de ese tipo ser instancias?
Respuesta

Un struct en C es una agrupación de datos sin comportamiento asociado, por lo que se parece a una clase en cuanto a que define un tipo compuesto. Sin embargo, a un struct le faltan los métodos, la encapsulación y el control de acceso (visibilidad), que son elementos esenciales de una clase. En C, un struct no puede contener funciones dentro de sí mismo, y no tiene herencia ni polimorfismo.

Para que un struct se parezca más a una clase, sería necesario asociarle funciones que operen sobre sus datos (como métodos), y establecer mecanismos para controlar el acceso a sus campos, por ejemplo, mediante funciones getters/setters y encapsulación manual.

17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con struct en C, la clase Punto, con su función para calcular la distancia al origen? ¿Qué ha pasado con this?
Respuesta

En C, se puede emular una clase usando un struct para los datos y funciones externas que reciben un puntero al struct como parámetro. El equivalente a this sería ese puntero explícito, que se pasa a la función para operar sobre el “objeto”.

Ejemplo:

#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

double calculaDistanciaAOrigen(Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}


En este caso, el puntero p actúa como this, pero se debe pasar explícitamente en cada función, porque C no tiene un concepto de objeto implícito.