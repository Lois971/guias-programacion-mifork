1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado aMayusculas e invócala con el puntero.
Respuesta
Un puntero a función en C es una variable capaz de almacenar la dirección de memoria de una función. Esto permite tratar funciones de forma similar a los datos, pudiendo almacenarlas en variables, pasarlas como parámetros o invocarlas indirectamente. Este mecanismo se utiliza frecuentemente para implementar comportamientos configurables o callbacks.
El tipo del puntero debe coincidir con la firma de la función a la que apunta, es decir, con el tipo de retorno y los parámetros. A través del puntero se puede invocar la función igual que si se llamase directamente.
#include <stdio.h>#include <ctype.h>void convertirAMayusculas(char* texto) {    for (int i = 0; texto[i] != '\0'; i++) {        texto[i] = toupper(texto[i]);    }}int main() {    char cadena[] = "hola mundo";    void (*aMayusculas)(char*) = convertirAMayusculas;    aMayusculas(cadena);    printf("%s\n", cadena);    return 0;}

2. ¿Qué es una función lambda en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local aMayusculas para apuntar a la función lambda. Por simplicidad, en Java, emplea Function<String, String> para el tipo de la referencia a la función lambda.
Respuesta
Una función lambda es una función anónima que puede almacenarse en variables, pasarse como parámetro o devolverse desde otras funciones. Permite escribir código más compacto y expresivo, especialmente en programación funcional. Las lambdas son habituales en lenguajes modernos que permiten tratar las funciones como valores.
En JavaScript las funciones son ciudadanos de primera clase, por lo que las lambdas forman parte natural del lenguaje. En Java, las expresiones lambda aparecieron en Java 8 y se apoyan en interfaces funcionales como Function<T, R>.
const aMayusculas = texto => texto.toUpperCase();console.log(aMayusculas("hola mundo"));
import java.util.function.Function;public class Main {    public static void main(String[] args) {        Function<String, String> aMayusculas =                texto -> texto.toUpperCase();        System.out.println(aMayusculas.apply("hola mundo"));    }}

3. ¿Qué es el paradigma funcional? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?
Respuesta
El paradigma funcional es un estilo de programación basado en el uso de funciones como elemento principal de construcción de programas. Se centra en aplicar funciones, evitar estados mutables y reducir efectos secundarios. Muchas operaciones se expresan mediante transformaciones de datos en lugar de instrucciones paso a paso.
Lenguajes como Java 8 se consideran multi-paradigma porque permiten combinar programación orientada a objetos con características funcionales, como funciones lambda, referencias a métodos y operaciones sobre colecciones usando programación declarativa. Esto proporciona más flexibilidad y herramientas para resolver problemas.
Decir que las funciones son “ciudadanos de primera clase” significa que pueden tratarse igual que cualquier otro valor: almacenarse en variables, pasarse como parámetros, devolverse desde funciones y almacenarse en estructuras de datos.

4. Explica la sintaxis básica de una función lambda en Java.
Respuesta
La sintaxis básica de una lambda en Java consiste en una lista de parámetros, seguida del operador -> y finalmente el cuerpo de la función. La parte izquierda representa los parámetros y la derecha el comportamiento de la función.
Cuando el cuerpo contiene una única expresión, el valor se devuelve automáticamente. Si el cuerpo tiene varias instrucciones, deben usarse llaves y la palabra clave return cuando corresponda.
(parametros) -> expresion
(String texto) -> texto.toUpperCase()
(String texto) -> {    System.out.println(texto);    return texto.toUpperCase();}

5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado transformar, que reciba un String como parámetro y luego una función transformadora como lo es aMayúsculas y la invoque desde dentro.
Respuesta
Recibir funciones como parámetros es una característica típica del paradigma funcional. Permite separar el algoritmo general del comportamiento concreto que se desea aplicar. En este caso, el método transformar delega la transformación de la cadena en una función recibida como parámetro.
Este diseño mejora la reutilización y evita duplicar código, ya que el mismo método puede trabajar con múltiples transformaciones distintas.
function transformar(texto, funcion) {    return funcion(texto);}const aMayusculas = texto => texto.toUpperCase();console.log(transformar("hola", aMayusculas));
import java.util.function.Function;public class Main {    public static String transformar(            String texto,            Function<String, String> funcion) {        return funcion.apply(texto);    }    public static void main(String[] args) {        Function<String, String> aMayusculas =                t -> t.toUpperCase();        System.out.println(transformar("hola", aMayusculas));    }}

6. Ahora, invoca transformar, con una nueva función lambda directamente en la llamada a transformar, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.
Respuesta
Las funciones lambda también pueden definirse directamente en la llamada a un método, sin necesidad de almacenarlas previamente en variables. Esto resulta útil cuando la función solo se va a utilizar una vez.
El código se vuelve más compacto y expresivo, especialmente cuando se combinan operaciones funcionales encadenadas.
console.log(    transformar("hola", texto =>        texto.split("").reverse().join("")    ));
System.out.println(    transformar("hola",        texto -> new StringBuilder(texto)                    .reverse()                    .toString()));

7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida.
Respuesta
Un closure o cierre es una función que captura variables del contexto donde fue creada. Aunque la función se ejecute posteriormente, sigue teniendo acceso a esas variables externas. Esto permite construir funciones más flexibles y personalizadas.
En Java, las variables capturadas por una lambda deben ser efectivamente finales, es decir, no modificarse después de su inicialización.
import java.util.function.Function;public class Main {    public static void main(String[] args) {        String sufijo = "!!!";        Function<String, String> transformar =                texto -> texto + sufijo;        System.out.println(transformar.apply("Hola"));    }}

8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?
Respuesta
Los punteros a funciones en C únicamente almacenan la dirección de memoria de una función existente. No capturan variables del contexto ni almacenan estado adicional. Funcionan como referencias directas a código ejecutable.
Las funciones lambda modernas, en cambio, pueden actuar como closures, capturando variables del entorno donde fueron definidas. Además, suelen integrarse con sistemas de tipos más avanzados y permiten escribir código más expresivo y flexible.
Otra diferencia importante es que en lenguajes como Java las lambdas están integradas con interfaces funcionales y el sistema orientado a objetos, mientras que en C los punteros a función son una característica más básica y de bajo nivel.

9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa Function<Double, Double> para su tipo. La función crearDescuento(porcentaje), recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.
Respuesta

Las funciones también pueden devolverse como resultado de otras funciones. Esto permite construir funciones personalizadas dinámicamente. En este caso, la función crearDescuento genera y devuelve otra función que aplica un porcentaje de descuento concreto sobre una cantidad.

La función devuelta recuerda el porcentaje recibido originalmente gracias a un closure. Aunque crearDescuento termine su ejecución, la lambda mantiene acceso al valor del porcentaje capturado en su contexto.

import java.util.function.Function;

public class Main {

    public static Function<Double, Double>
        crearDescuento(double porcentaje) {

        return cantidad ->
                cantidad - (cantidad * porcentaje / 100);
    }

    public static void main(String[] args) {

        Function<Double, Double> descuento10 =
                crearDescuento(10);

        Function<Double, Double> descuento25 =
                crearDescuento(25);

        System.out.println(descuento10.apply(200.0));
        System.out.println(descuento25.apply(200.0));
    }
}
10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como interfaz funcional. ¿Qué es una interfaz funcional? ¿Qué requisitos tiene?
Respuesta

Una interfaz funcional es una interfaz que define exactamente un único método abstracto. Este método representa la firma de la función que implementará una expresión lambda. Gracias a ello, Java puede asociar automáticamente una lambda con una interfaz concreta.

Las interfaces funcionales pueden contener métodos default o static, pero únicamente un método abstracto. Es habitual marcar estas interfaces con la anotación @FunctionalInterface, que permite al compilador verificar que realmente cumplen los requisitos.

Ejemplos de interfaces funcionales estándar en Java son Function, Consumer, Supplier y Predicate.

11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale Transformador, que define una función que convierte una cadena de texto (String) en otra (String).
Respuesta

Las interfaces funcionales personalizadas permiten representar operaciones específicas del dominio de la aplicación. En este caso, se define una interfaz Transformador que representa cualquier función capaz de transformar una cadena en otra.

Una vez creada la interfaz, puede utilizarse con expresiones lambda de forma equivalente a las interfaces funcionales estándar de Java.

@FunctionalInterface
public interface Transformador {

    String transformar(String texto);
}
public class Main {

    public static void main(String[] args) {

        Transformador aMayusculas =
                texto -> texto.toUpperCase();

        System.out.println(
                aMayusculas.transformar("hola"));
    }
}
12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un Transformador de un tipo en otro. Pon un ejemplo de un transformador que redondea un Double en un Integer.
Respuesta

La genericidad permite reutilizar la misma interfaz funcional con diferentes tipos de datos. En lugar de trabajar únicamente con String, se pueden parametrizar tanto el tipo de entrada como el de salida.

Este enfoque hace que la interfaz sea mucho más flexible y reutilizable en distintos contextos.

@FunctionalInterface
public interface Transformador<T, R> {

    R transformar(T valor);
}
public class Main {

    public static void main(String[] args) {

        Transformador<Double, Integer> redondear =
                numero -> (int)Math.round(numero);

        System.out.println(
                redondear.transformar(3.7));
    }
}
13. Transformador, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es Function<T, R>. Muestra las interfaces funcionales predefinidas que hay en Java.
Respuesta

Java incluye numerosas interfaces funcionales predefinidas dentro del paquete java.util.function. Estas interfaces cubren los casos más habituales y evitan tener que crear interfaces nuevas constantemente.

Algunas de las más importantes son:

Function<T, R>: transforma un objeto de tipo T en uno de tipo R.
Consumer<T>: consume un objeto sin devolver resultado.
Supplier<T>: genera un objeto sin recibir parámetros.
Predicate<T>: evalúa una condición y devuelve boolean.
UnaryOperator<T>: transforma un objeto del mismo tipo.
BinaryOperator<T>: combina dos objetos del mismo tipo.

Estas interfaces se utilizan ampliamente junto con lambdas y streams en Java moderno.

14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el List.forEach, como versión funcional del bucle for. Emplea el forEach para recorrer una lista de Integer y que muestre un mensaje si el entero es positivo.
Respuesta

El método forEach permite recorrer colecciones de forma funcional, evitando escribir explícitamente un bucle for. Recibe una función que se ejecuta para cada elemento de la colección.

Este enfoque hace que el código sea más declarativo y expresivo, especialmente cuando se combinan operaciones funcionales sobre colecciones.

import java.util.List;

public class Main {

    public static void main(String[] args) {

        List<Integer> numeros =
                List.of(3, -2, 7, -1, 5);

        numeros.forEach(numero -> {

            if (numero > 0) {
                System.out.println(
                        numero + " es positivo");
            }
        });
    }
}
15. Repasando el tema de genericidad, fíjate en la firma de forEach, ¿por qué se usa Consumer<? super T> y no Consumer<T>? Explica qué significa PECS, y explícalo para el caso de mejorar el ejemplo del método transformar la hora de definir el tipo de la función transformadora.
Respuesta

La regla PECS significa “Producer Extends, Consumer Super”. Indica cómo deben utilizarse los comodines (? extends y ? super) en genericidad. Si una estructura produce objetos, suele usarse extends; si consume objetos, suele usarse super.

En el caso de forEach, el Consumer consume elementos de tipo T, por eso se utiliza Consumer<? super T>. Esto permite usar consumidores más generales, aumentando la flexibilidad del código.

En el método transformar, podría definirse:

Function<? super String, ? extends String>

De este modo, la función podría aceptar tipos más generales que String como entrada y devolver subtipos compatibles como salida.

16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase Persona con un método saludar. En el código principal, crea una Persona con un nombre, y obtén una referencia a su método saludar en una variable local. Invoca saludar con esa referencia a su método saludar.
Respuesta

Las referencias a métodos permiten almacenar métodos existentes como funciones reutilizables. Son similares a las lambdas, pero reutilizando directamente métodos ya definidos.

Esto hace que el código sea más compacto y legible cuando simplemente se desea reutilizar un método existente sin crear una lambda explícita.

class Persona {

    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const p = new Persona("Ana");

const saludo = p.saludar.bind(p);

saludo();
public class Persona {

    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
public class Main {

    public static void main(String[] args) {

        Persona p = new Persona("Ana");

        Runnable saludo = p::saludar;

        saludo.run();
    }
}
17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.
Respuesta

Java permite varios tipos de referencias a métodos. Todas ellas utilizan el operador :: y sirven como alternativa compacta a ciertas lambdas sencillas.

Los tipos principales son:

// Método estático
Integer::parseInt

// Constructor
Persona::new

// Método de instancia concreta
persona::saludar

// Método de instancia sobre cualquier objeto
String::toUpperCase

Ejemplo completo:

Function<String, Integer> convertir =
        Integer::parseInt;

Supplier<Persona> crear =
        Persona::new;

Persona p = new Persona("Ana");

Runnable saludo =
        p::saludar;

Function<String, String> mayusculas =
        String::toUpperCase;
18. Otro ejemplo expresivo. Ordena una lista de Persona, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de Persona con Collections.sort, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando Comparator.
Respuesta

Las expresiones lambda permiten definir comparadores de forma compacta directamente en la llamada a sort. Esto evita crear clases auxiliares únicamente para implementar la comparación.

También puede utilizarse la clase Comparator, que proporciona métodos auxiliares muy expresivos para construir comparaciones complejas encadenadas.

import java.util.*;

class Persona {

    String nombre;
    int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

Versión manual:

Collections.sort(personas, (p1, p2) -> {

    if (p1.edad != p2.edad) {
        return p1.edad - p2.edad;
    }

    return p1.nombre.compareTo(p2.nombre);
});

Versión usando Comparator:

Collections.sort(
    personas,
    Comparator.comparingInt((Persona p) -> p.edad)
              .thenComparing(p -> p.nombre)
);