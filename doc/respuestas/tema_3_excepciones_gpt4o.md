<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C no existen excepciones, por lo que el control de errores debe diseñarse explícitamente mediante valores de retorno o parámetros adicionales. Una primera opción consiste en devolver un valor especial que indique error. Por ejemplo, la función puede devolver -1 cuando recibe un número negativo, y el código llamador debe comprobar ese resultado antes de usarlo. Esta técnica es simple, pero tiene el inconveniente de que puede haber ambigüedad si el valor especial también podría ser un resultado válido en otro contexto.

#include <stdio.h>
#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1.0;  // valor especial de error
    }
    return sqrt(x);
}

int main() {
    float resultado = raiz(-9);
    if (resultado == -1.0) {
        printf("Error: número negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }
}

Una segunda opción consiste en devolver un código de error y utilizar un parámetro adicional para almacenar el resultado correcto. Esta aproximación es más robusta, ya que separa claramente el valor calculado del estado de error. Es una técnica muy utilizada en programación en C, donde muchas funciones devuelven 0 si todo va bien y un valor distinto en caso de error.

#include <stdio.h>
#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 1;  // código de error
    }
    *resultado = sqrt(x);
    return 0;      // éxito
}

int main() {
    float valor;
    if (raiz(-9, &valor) != 0) {
        printf("Error: número negativo\n");
    } else {
        printf("Resultado: %f\n", valor);
    }
}


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un mecanismo que permite representar y gestionar situaciones anómalas que ocurren durante la ejecución de un programa. En lugar de devolver códigos de error manualmente, el sistema interrumpe el flujo normal cuando se detecta un problema y transfiere el control a un manejador preparado para tratar esa situación.

El objetivo de utilizar excepciones es separar claramente la lógica normal del programa del tratamiento de errores. Cuando se implementa una función, se puede indicar que ante determinadas condiciones se lanzará una excepción. Cuando se llama a esa función, se puede decidir si se captura la excepción o si se deja que se propague hacia niveles superiores. Esto permite escribir código más limpio y más cercano al modelo lógico del problema.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, el control de errores mediante excepciones permite evitar valores especiales o códigos de retorno. Se puede definir un método dentro de una clase Calculadora que lance una excepción si el número es negativo. El llamador decidirá cómo tratar ese error.

class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double resultado = raiz(-9);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

En este caso, el método raiz no devuelve un valor especial. Si el argumento es incorrecto, se lanza una excepción. El método main es quien decide capturarla mediante un bloque try-catch, mostrando el mensaje correspondiente. De esta forma, el control del error se realiza desde fuera del método, pero sin necesidad de comprobar manualmente códigos de retorno.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

"Lanzar" una excepción consiste en crear un objeto excepción y transferir el control del programa fuera del flujo normal mediante la instrucción throw. "Capturar" o "controlar" una excepción significa interceptarla mediante un bloque catch, evitando que continúe propagándose. "Propagarse" implica que, si no se captura en el método actual, la excepción asciende por la pila de llamadas buscando un manejador adecuado.

Cuando una excepción se propaga, cada método en la pila de llamadas termina inmediatamente su ejecución sin completar las instrucciones pendientes. Es decir, no se reanudan después del punto donde ocurrió el error. El sistema va eliminando métodos de la pila hasta encontrar uno que tenga un catch adecuado o hasta llegar al método principal.

En el ejemplo de la raíz en Java, si raiz lanza la excepción y no la captura internamente, el método main puede capturarla. Si tampoco lo hiciera, la ejecución terminaría mostrando el error por pantalla. Los métodos intermedios que no la controlen simplemente finalizan abruptamente.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

En C, cada función debe comprobar explícitamente los códigos de error devueltos por las funciones que llama. Si se olvida esa comprobación, el error puede pasar desapercibido. En Java, la propagación natural permite que el error ascienda automáticamente por la pila sin necesidad de escribir comprobaciones repetitivas.

Esto simplifica el código y reduce la probabilidad de errores humanos. Además, permite centralizar el tratamiento de errores en niveles superiores del programa, en lugar de dispersarlo por todas las funciones intermedias. El resultado es un diseño más modular y más limpio.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En programación orientada a objetos, las excepciones suelen ser objetos. En Java, todas las excepciones heredan de la clase base Exception, lo que significa que forman parte de la jerarquía de clases del lenguaje. Esto permite que una excepción no sea simplemente un código de error, sino una entidad con estado y comportamiento propios.

Desde el punto de vista de la encapsulación, esto ofrece una ventaja importante: la información del error puede almacenarse dentro del objeto excepción. En lugar de depender de variables globales o códigos numéricos poco descriptivos, el objeto puede contener un mensaje, datos adicionales e incluso métodos para acceder a esa información. De este modo, el tratamiento del error es más claro y estructurado.

Sí, se pueden crear excepciones personalizadas. Basta con definir una clase que herede de Exception o de alguna de sus subclases. Esto permite modelar errores específicos del dominio del problema, manteniendo coherencia con el diseño orientado a objetos.

class RaizNegativaException extends Exception {
    public RaizNegativaException(String mensaje) {
        super(mensaje);
    }
}

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

A diferencia del enfoque en C, donde normalmente solo se dispone de un código numérico, en Java un objeto excepción contiene información esencial muy útil. En primer lugar, incluye un mensaje descriptivo que explica la causa del error. Este mensaje puede recuperarse mediante el método getMessage().

En segundo lugar, la excepción almacena automáticamente la traza de la pila de llamadas (stack trace). Esta información indica qué métodos estaban activos en el momento en que ocurrió el error y en qué línea concreta se produjo. Esto resulta extremadamente útil para depuración, ya que permite localizar con precisión el origen del problema.

Además, puede incluir una referencia a una causa previa (otra excepción que originó el problema). Esta información estructurada convierte a la excepción en un mecanismo mucho más potente que un simple valor de retorno.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

En Java se pueden tener varios bloques catch asociados a un mismo bloque try. Esto permite tratar de manera diferente distintos tipos de excepciones. Cada bloque catch especifica el tipo de excepción que puede manejar.

Sin embargo, aunque existan varios bloques catch, solo se ejecutará uno de ellos: el primero que sea compatible con el tipo de la excepción lanzada. Una vez que una excepción es capturada por un catch, no se ejecutan los demás bloques.

Es importante ordenar los catch desde el más específico al más general. Si se coloca primero un tipo más general, como Exception, impediría que se ejecuten los bloques más específicos que aparezcan después.

try {
    double r = Calculadora.raiz(-9);
} catch (IllegalArgumentException e) {
    System.out.println("Argumento ilegal");
} catch (Exception e) {
    System.out.println("Error general");
}


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Cuando se produce una excepción, el flujo normal del programa se interrumpe. Sin embargo, puede existir código que deba ejecutarse siempre, como el cierre de ficheros o la liberación de recursos. Para ello se utiliza el bloque finally, que se ejecuta independientemente de si ocurre o no una excepción.

El bloque finally puede acompañar a un try-catch, asegurando que el código de limpieza se ejecute tras capturar la excepción. Esto garantiza que los recursos no queden abiertos, incluso si se produce un error inesperado.

try {
    double r = Calculadora.raiz(-9);
} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Bloque finally ejecutado");
}

También puede utilizarse sin catch, dejando que la excepción se propague pero asegurando igualmente la ejecución del código final.

try {
    double r = Calculadora.raiz(-9);
} finally {
    System.out.println("Siempre se ejecuta");
}

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

En Java, el bloque finally puede existir sin un bloque catch. En ese caso, si ocurre una excepción, esta se propagará automáticamente después de ejecutarse el finally. Si no ocurre ninguna excepción, también se ejecutará el bloque finally antes de continuar.

En condiciones normales, el bloque finally se ejecuta siempre, tanto si ocurre una excepción como si no. Incluso si existe una instrucción return dentro del bloque try, el bloque finally se ejecutará antes de que el método termine definitivamente.

Esto significa que finally es el lugar adecuado para realizar tareas críticas de limpieza. La única situación en la que podría no ejecutarse es si la máquina virtual termina abruptamente (por ejemplo, mediante System.exit()).

public static int ejemplo() {
    try {
        return 1;
    } finally {
        System.out.println("Se ejecuta antes del return definitivo");
    }
}


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java existen excepciones controladas (checked) y no controladas (unchecked). Las excepciones controladas son aquellas que el compilador obliga a declarar o capturar. Si un método puede producir una de estas excepciones, debe indicarlo con throws o capturarla con try-catch. En cambio, las no controladas heredan de RuntimeException y no es obligatorio declararlas ni capturarlas.

La clase RuntimeException actúa como base de las excepciones que representan errores de programación, como accesos fuera de rango o argumentos inválidos. Por ejemplo, IOException es una excepción controlada típica cuando se trabaja con ficheros, mientras que IllegalArgumentException o NullPointerException son no controladas.

Ejemplos de situaciones donde se prefieren excepciones controladas:

Apertura de un fichero que puede no existir.

Acceso a una base de datos remota.

Lectura desde red.

Procesamiento de datos externos impredecibles.

Ejemplos donde se prefieren excepciones no controladas:

Paso de argumentos inválidos a un método.

Uso incorrecto de una API.

Acceso a un índice fuera de rango.

Estado interno inconsistente por error del programador.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La palabra clave throws se utiliza en la firma de un método para indicar que puede lanzar determinadas excepciones. Esto informa al compilador y a quien use el método de que existe la posibilidad de que ocurra un error que no será gestionado internamente.

throws es una alternativa a capturar la excepción dentro del propio método. En lugar de manejar el problema localmente, se decide delegar la responsabilidad en el método llamador. De esta manera, el control del error puede centralizarse en niveles superiores del programa.

Este mecanismo es obligatorio para excepciones controladas, lo que obliga a pensar explícitamente cómo se gestionarán los posibles errores. Con ello se evita que situaciones excepcionales pasen desapercibidas.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

Si un método abre un fichero pero no desea manejar la excepción en caso de que no exista, puede declararlo mediante throws. Así, la excepción se propagará hacia el método llamador. No obstante, puede utilizarse finally para garantizar el cierre de recursos.

import java.io.*;

public static void leerFichero(String nombre) throws IOException {
    FileReader fr = null;
    try {
        fr = new FileReader(nombre);
        System.out.println("Fichero abierto");
    } finally {
        if (fr != null) {
            fr.close();
        }
    }
}

En este ejemplo, si el fichero no existe, se lanzará una IOException que se propagará. El método no la captura, pero sí garantiza mediante finally que, si el fichero se abrió correctamente, será cerrado antes de salir del método.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí, se pueden declarar excepciones no controladas en la cláusula throws, como RuntimeException. Sin embargo, no es obligatorio hacerlo, ya que el compilador no exige su declaración.

El método llamador no está obligado a capturarlas mediante try-catch. Si no se capturan, se propagarán hasta terminar el programa. Declararlas puede tener sentido como documentación, para indicar que el método puede fallar bajo ciertas condiciones.

En general, no se suele forzar la captura de excepciones no controladas, ya que representan errores de programación que deberían corregirse, más que manejarse en tiempo de ejecución.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Las excepciones controladas se recomiendan cuando el error puede ocurrir por causas externas al programa y es razonable que el llamador pueda recuperarse. Por ejemplo, fallos de entrada/salida o ausencia de un recurso externo. En estos casos, obligar a capturar o declarar la excepción mejora la robustez del sistema.

Las excepciones no controladas se recomiendan cuando el error indica un uso incorrecto del programa, como argumentos inválidos o estados inconsistentes. En estos casos, el problema suele ser un fallo lógico que debe corregirse en el código.

No todos los lenguajes distinguen entre excepciones controladas y no controladas. Java es uno de los más conocidos que sí lo hace. En muchos otros lenguajes solo existen excepciones no controladas, siendo este modelo el más habitual en lenguajes modernos.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí tiene sentido lanzar una excepción dentro de un bloque catch. Esto puede hacerse para transformar una excepción de bajo nivel en otra más adecuada al nivel de abstracción actual.

try {
    double r = Calculadora.raiz(-9);
} catch (IllegalArgumentException e) {
    throw new RuntimeException("Error en cálculo matemático", e);
}

También se puede relanzar la misma excepción capturada utilizando simplemente throw e;. Esto tiene sentido cuando se desea realizar alguna acción adicional (por ejemplo, registrar el error) antes de permitir que continúe propagándose.

try {
    double r = Calculadora.raiz(-9);
} catch (IllegalArgumentException e) {
    System.out.println("Registrando error");
    throw e;
}

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Una excepción puede ser la causa de otra cuando se encapsula una excepción original dentro de una nueva, utilizando un constructor que acepte una causa. Esto permite mantener la información original mientras se adapta el error a un nivel más alto de abstracción.

class ErrorCalculoException extends Exception {
    public ErrorCalculoException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

try {
    double r = Calculadora.raiz(-9);
} catch (IllegalArgumentException e) {
    throw new ErrorCalculoException("Fallo en operación matemática", e);
}

Cuando una excepción con causa se muestra por pantalla, la traza incluye tanto la excepción principal como la causa original. Esto resulta muy útil para depuración, ya que permite ver la cadena completa de errores desde el origen hasta el nivel superior donde se manejó o terminó el programa.
