# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta
La encapsulación es el principio de agrupar datos (atributos) y comportamiento (métodos) dentro de una misma entidad, que es la clase. La idea es que el objeto gestione su propio estado y ofrezca operaciones para interactuar con él, en lugar de permitir que cualquier parte del programa modifique directamente sus datos internos. La ocultación de información es una parte clave de la encapsulación y consiste en restringir el acceso directo a los atributos internos de una clase, exponiendo solo lo necesario mediante una interfaz pública.

Entre las ventajas de la ocultación de información se encuentra la reducción de errores, ya que se evita que el estado interno se modifique de forma inconsistente desde fuera. También mejora la mantenibilidad, porque los detalles internos pueden cambiar sin afectar a otras partes del programa que usan la clase. Además, permite garantizar invariantes de clase, es decir, condiciones que deben mantenerse verdaderas durante la vida del objeto. Por último, facilita la seguridad del software, porque se minimiza el acceso no deseado a datos sensibles o críticos.

---

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La interfaz pública de una clase es el conjunto de métodos y atributos accesibles desde fuera de la clase. Es la forma en la que otros objetos o módulos interactúan con esa clase, sin conocer sus detalles internos. La interfaz pública define qué operaciones se pueden realizar y qué información se puede consultar o modificar.

La relación con la ocultación de información es directa: ocultación significa restringir el acceso a los detalles internos, y la interfaz pública es lo que queda visible y utilizable. Cuanto más controlado esté el acceso externo, más se puede garantizar que el objeto se usa correctamente. En resumen, la interfaz pública es el contrato que se ofrece al exterior, mientras que el resto queda oculto para preservar consistencia y seguridad.

---

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La interfaz pública es el contrato que se ofrece a otras partes del programa, por lo que su diseño afecta directamente a cómo se usa la clase. Si la interfaz es demasiado amplia o mal pensada, puede permitir usos incorrectos o limitar la capacidad de cambiar la implementación interna sin romper el código que depende de ella. Por tanto, es importante diseñarla con cuidado, definiendo solo lo necesario y evitando exponer detalles internos.

Cambiar la interfaz pública no es fácil, porque cualquier cambio puede afectar a todas las partes del código que la utilizan. Si se modifica un método, se cambia su nombre, o se elimina un atributo público, es probable que el código que depende de esa clase deje de compilar o funcione mal. Por eso se considera una práctica recomendada mantener estable la interfaz pública y evolucionarla con cautela, proporcionando versiones nuevas o métodos adicionales en lugar de modificar los existentes.

---

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las invariantes de clase son condiciones que deben ser siempre verdaderas para el estado interno de un objeto, antes y después de cualquier operación. Por ejemplo, en una clase `CuentaBancaria`, una invariante podría ser que el saldo nunca sea negativo. Estas condiciones definen un estado válido y permiten razonar sobre el comportamiento correcto de la clase.

La ocultación de información ayuda porque impide que el estado interno se modifique directamente desde fuera, lo que podría violar las invariantes. Si los atributos solo se pueden cambiar mediante métodos controlados, la clase puede validar y asegurar que las invariantes se mantengan. De este modo, se evita que otras partes del programa establezcan valores inválidos, y se mantiene la consistencia del objeto.

---

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
La ocultación de información se implementa declarando los atributos como privados (`private`) y ofreciendo métodos públicos (`public`) para interactuar con ellos. En el caso de una clase `Punto`, los atributos `x` e `y` se mantienen privados y se proporciona un método público para calcular la distancia al origen. De este modo, se evita que otras partes del programa cambien directamente las coordenadas sin control.

La interfaz pública de `Punto` está formada por los métodos y constructores accesibles desde fuera de la clase, como el constructor, `calcularDistanciaAOrigen` y, si existen, los getters y setters. El modificador `public` indica que el elemento es accesible desde cualquier otra clase, mientras que `private` limita el acceso únicamente al interior de la propia clase. Con esto se protege el estado interno y se define qué operaciones se permiten.

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, los modificadores `public` y `private` se pueden aplicar a los elementos de una clase, es decir, a atributos, métodos y constructores. De este modo se controla qué miembros son accesibles desde fuera de la clase y cuáles quedan ocultos. También se puede aplicar a clases, pero con limitaciones: solo las clases de nivel superior pueden ser `public`, mientras que `private` no es válido para una clase de nivel superior.

Además, dentro de una clase se pueden declarar clases internas, y estas sí pueden ser `private` o `public`. De forma habitual, los atributos se declaran `private` para ocultar el estado interno, mientras que los métodos de acceso o comportamiento se declaran `public` para formar la interfaz pública de la clase.

---

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
Sí, además de pública y privada existen otros niveles de visibilidad. En Java, además de `public` y `private`, se dispone de `protected` y del acceso por defecto (sin modificador), que se conoce como **package-private**. `protected` permite acceso desde subclases y desde clases del mismo paquete, mientras que el acceso por defecto solo permite acceso dentro del mismo paquete. Esto permite modularizar el código y decidir qué se comparte entre clases relacionadas.

En otros lenguajes el modelo puede variar. En C++ existen `public`, `private` y `protected`, y también se pueden usar amigos (`friend`) para permitir accesos concretos. En Python, la visibilidad es más una convención, porque no existe una protección estricta del lenguaje; se utilizan prefijos como `_` o `__` para indicar que algo es “privado”. En general, el concepto de visibilidad existe en todos los lenguajes orientados a objetos, pero su implementación y niveles disponibles dependen del lenguaje.

---

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
En Java, los miembros privados están ocultos para otras clases, pero **no** para otras instancias de la misma clase. Esto significa que un método de la clase puede acceder a los atributos privados de cualquier objeto de esa clase, aunque sea una instancia distinta. La privacidad se aplica a nivel de clase, no a nivel de objeto.

Por ejemplo, en una clase `Punto` con atributos privados `x` e `y`, un método `calcularDistanciaAPunto(Punto otro)` puede leer directamente `otro.x` y `otro.y`, porque el método pertenece a la misma clase. Esto permite comparar o combinar estados entre objetos de la misma clase sin exponer esos atributos al exterior.

```java
public class Punto {
    private double x;
    private double y;

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos *getter* y *setter* son métodos públicos que permiten leer y modificar atributos privados de una clase sin acceder directamente a ellos. El getter devuelve el valor de un atributo, mientras que el setter permite asignar un nuevo valor. Su uso permite mantener la encapsulación, ya que el acceso a los datos queda controlado por la clase y no se expone el estado interno directamente.

Además, los getters y setters permiten validar valores, registrar cambios o ejecutar lógica adicional al acceder o modificar el estado. Por ejemplo, un setter puede rechazar valores fuera de rango, evitando que el objeto quede en un estado inválido. De esta forma se protege la invariante de la clase y se reduce el acoplamiento con el código externo.

---

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
La seguridad que aporta la ocultación de información no se refiere a evitar ataques externos o “hackeos”, sino a evitar que el código de otras partes del programa pueda manipular el estado interno de forma incorrecta. Al restringir el acceso a los datos, se reduce la probabilidad de que se asignen valores inválidos o se rompan invariantes por error.

Además, la encapsulación mejora la robustez del diseño porque reduce los puntos desde los que se puede modificar el estado de un objeto. Esto facilita el mantenimiento y la evolución del código, ya que se controla mejor cómo se puede interactuar con el objeto y se limita la posibilidad de introducir fallos por efectos secundarios inesperados.

---

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un miembro de instancia pertenece a cada objeto creado a partir de una clase, por lo que cada instancia tiene su propia copia de ese atributo o estado. En cambio, un miembro de clase (estático) pertenece a la clase en sí y se comparte entre todas las instancias. Esto se usa para valores o comportamientos comunes, como contadores o constantes compartidas.

Los miembros de clase también pueden ocultarse con los mismos modificadores de visibilidad (`private`, `public`, etc.). De hecho, es habitual declarar atributos estáticos como privados y exponerlos mediante getters estáticos si se desea. La encapsulación funciona igual, independientemente de si el miembro es de instancia o de clase.

---

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Sí, puede tener sentido cuando se quiere controlar estrictamente la creación de instancias. Un constructor privado impide que el código externo cree objetos directamente, lo que es útil en patrones como *singleton* o cuando se desea que la creación de objetos pase por métodos de fábrica. De esta forma se garantiza que se cumplen ciertas condiciones o se limita el número de instancias.

También es útil cuando una clase solo proporciona métodos estáticos y no debe instanciarse, por ejemplo, en clases de utilidades. Un constructor privado evita que se creen objetos sin sentido y refuerza la intención de diseño, evitando errores de uso.

---

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
En Java, los miembros de clase se indican con el modificador `static`. Un atributo `static` pertenece a la clase en sí y su valor se comparte entre todas las instancias. Los métodos estáticos también se declaran con `static` y pueden acceder únicamente a miembros estáticos.

Para registrar los valores máximos de `x` e `y` entre todos los puntos creados, se pueden declarar dos atributos estáticos y actualizar su valor en el constructor. Así, cada vez que se crea un `Punto`, se compara el valor con los máximos registrados y se actualizan si procede.

```java
public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}
## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
Un método factoría es un método estático que crea y devuelve instancias de la clase, permitiendo encapsular la lógica de creación. En este caso, el método recibe dos coordenadas `double`, las redondea al entero más cercano y devuelve un nuevo objeto `Punto` con esos valores. La ventaja es que se puede controlar la creación sin que el código externo tenga que repetir la misma lógica de redondeo.

En Java, este tipo de método se declara con `static` porque se debe poder invocar sin necesidad de tener una instancia previa. Además, la factoría puede devolver una instancia existente en lugar de crear una nueva, si se quiere optimizar o aplicar patrones como el *flyweight*.

```java
public static Punto crearRedondeado(double x, double y) {
    long xRedondeado = Math.round(x);
    long yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}
## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
Se puede cambiar la representación interna de la clase `Punto` usando un array `double[]` de dos posiciones para almacenar `x` e `y`. La interfaz pública se mantiene igual si los métodos `getX()`, `getY()`, `setX()`, `setY()` y `calcularDistanciaAOrigen()` siguen existiendo y se comportan como antes. De esta forma, el código externo no nota el cambio interno y sigue usando la clase como si tuviera atributos separados.

Es importante que el array no se exponga directamente, porque permitiría modificar el estado interno sin control, rompiendo la encapsulación. Si se necesita devolver el array, se debe hacer una copia defensiva para evitar que el código cliente altere el estado del objeto. La clase también debe garantizar que el array siempre tenga tamaño 2 y que no sea `null`.

```java
public class Punto {
    private double[] coords;

    public Punto(double x, double y) {
        coords = new double[2];
        coords[0] = x;
        coords[1] = y;
    }

    public double getX() {
        return coords[0];
    }

    public double getY() {
        return coords[1];
    }

    public void setX(double x) {
        coords[0] = x;
    }

    public void setY(double y) {
        coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
}
## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
Aunque un atributo tenga getter y setter públicos, no es recomendable declararlo público porque se perdería el control sobre cómo se modifica el estado del objeto. Un setter puede incluir validaciones o acciones adicionales antes de cambiar el valor, mientras que un atributo público permite asignar cualquier valor directamente, lo que puede romper invariantes o dejar el objeto en un estado inválido. La encapsulación busca precisamente controlar el acceso para mantener la consistencia interna.

La convención más habitual en Java es declarar los atributos como `private` y proporcionar getters y setters solo cuando sea necesario. Esto permite cambiar la implementación interna sin afectar al código externo y facilita mantener invariantes de clase. Las invariantes son condiciones que siempre deben cumplirse para que el objeto sea válido; mantener los atributos privados ayuda a garantizar estas condiciones al centralizar la lógica de modificación en métodos controlados.

---

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase es inmutable cuando sus objetos no pueden cambiar su estado después de ser creados. Esto se logra declarando los atributos como finales o evitando exponer métodos que alteren el estado, de forma que cualquier operación que parezca modificar el objeto devuelva una nueva instancia en lugar de cambiar la existente. La inmutabilidad simplifica el razonamiento sobre el código y evita efectos secundarios inesperados.

Un método modificador es cualquier método que cambia el estado interno del objeto, no necesariamente un setter. Puede ser un método que actualice varios atributos a la vez o que realice cálculos que alteren el estado interno. En una clase inmutable, no existirán métodos modificadores; en su lugar, se ofrecerán métodos que generen nuevas instancias con el estado actualizado.

La inmutabilidad aporta ventajas como menor probabilidad de errores por efectos secundarios, mejor comportamiento en entornos concurrentes y mayor facilidad para razonar sobre el código. También permite usar objetos como claves en colecciones sin preocuparse de que cambien su estado mientras se usan.

---

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No es recomendable incluir setters por defecto. Solo deben añadirse cuando sea necesario permitir cambios de estado de forma controlada y coherente con el diseño de la clase. Añadir setters sin criterio puede llevar a objetos que se pueden dejar en estados inconsistentes o inválidos, o a clases con un diseño demasiado abierto que dificulta mantener invariantes.

La mejor práctica es diseñar la clase pensando en qué operaciones deben estar permitidas desde fuera y cuáles no. Si el estado debe permanecer estable, se recomienda no exponer setters y, en su lugar, ofrecer métodos específicos que modifiquen el objeto solo de forma válida o no permitir cambios tras la creación. Esto mejora la robustez y la claridad del diseño.

---

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
La clase `String` en Java es inmutable. Una vez creada una cadena, no puede modificarse. Cuando se concatenan cadenas, Java crea una nueva instancia con el resultado, dejando las originales sin cambios. Por ello, concatenar repetidamente con `+` puede generar muchas cadenas intermedias y aumentar el consumo de memoria y el coste de tiempo.

Si se necesita construir una cadena larga mediante muchas concatenaciones, lo recomendable es usar `StringBuilder` (o `StringBuffer` si se requiere sincronización). Estas clases son mutables y permiten añadir texto de forma eficiente sin crear nuevas instancias en cada operación. Al final, se obtiene la cadena final llamando a `toString()`.

---

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En POO, por defecto los objetos se comparan por identidad, es decir, si son el mismo objeto en memoria. En Java, el método `equals()` heredado de `Object` compara por identidad, equivalente a usar `==`. Para comparar por contenido, se debe sobrescribir `equals()` definiendo qué atributos determinan la igualdad y también sobrescribir `hashCode()` cuando se usa en colecciones.

Para comparar cadenas en Java por contenido se usa `equals()`. Usar `==` con cadenas compara referencias, por lo que solo devuelve `true` si ambas variables apuntan al mismo objeto. Para comparar cadenas sin distinguir mayúsculas y minúsculas, se usa `equalsIgnoreCase()`.

---

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
Las clases wrapper son clases que envuelven tipos primitivos para tratarlos como objetos. En Java, ejemplos son `Integer` para `int`, `Double` para `double`, `Boolean` para `boolean`, etc. Java realiza la conversión automática entre primitivos y wrappers mediante autoboxing y unboxing, lo que facilita el uso de primitivos en contextos que requieren objetos, como colecciones genéricas.

Las ventajas incluyen poder usar métodos asociados al objeto, permitir valores nulos y trabajar con APIs que exigen objetos. Sin embargo, usar wrappers puede tener un coste de rendimiento por la creación de objetos adicionales. No todos los lenguajes orientados a objetos tienen tipos primitivos; algunos solo manejan objetos, por lo que no necesitan wrappers. En otros lenguajes, los wrappers existen pero pueden no ser automáticos ni tan integrados como en Java.

---

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
Un tipo enumerado (enum) es un tipo que define un conjunto limitado de valores posibles, representando una lista cerrada de opciones. En Java, un `enum` es una clase especial cuyos valores son instancias predefinidas. Esto permite asociar datos y comportamiento a cada valor enumerado, lo que lo convierte en una herramienta más potente que una simple lista de constantes.

En términos de encapsulación, los enums evitan el uso de cadenas o números sueltos que pueden ser incorrectos, porque solo se pueden usar los valores definidos. Además, los enumerados permiten encapsular lógica específica para cada valor y ocultar detalles internos, haciendo el código más seguro y fácil de mantener. También facilitan la verificación en tiempo de compilación, evitando errores típicos de valores inválidos.

---

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
```java
public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinal;

    Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }

    public boolean esDePrimavera(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        }
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        } else {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        }
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        } else {
            return this == MARZO || this == ABRIL || this == MAYO;
        }
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        }
    }
}
