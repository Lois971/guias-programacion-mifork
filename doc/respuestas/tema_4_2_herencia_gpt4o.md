Tema 4.2. Herencia

1. ¿Qué es la herencia y su relación con “A es-un B”?
Respuesta

La herencia es un mecanismo de la programación orientada a objetos que permite definir una clase nueva a partir de otra ya existente. La nueva clase, llamada subclase, reutiliza atributos y métodos de la clase original, denominada superclase, y además puede añadir comportamiento propio. Conceptualmente, la herencia expresa una relación de tipo “A es-un B”. Por ejemplo, un Artillero es un Soldado, y un Zapador es un Soldado.

La primera implicación importante es la compatibilidad de tipos. Esto significa que cualquier objeto de una subclase puede utilizarse donde se espere un objeto de la superclase. Por ello, un Artillero puede almacenarse en una variable o array de tipo Soldado. La segunda implicación es la herencia de estado y comportamiento: los atributos y métodos definidos en Soldado pasan a formar parte de Artillero y Zapador, salvo restricciones de acceso.

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}
Soldado[] ejercito = {
    new Artillero("Carlos", 5),
    new Zapador("Luis", 3),
    new Artillero("Ana", 7)
};

for (Soldado s : ejercito) {
    s.saludar();
}

2. Constructores y super
Respuesta

Al crear un objeto de una subclase, se ejecutan dos constructores como mínimo: primero el de la superclase y después el de la subclase. El orden siempre es de arriba hacia abajo en la jerarquía de herencia, ya que primero debe construirse la parte común del objeto y posteriormente la parte específica.

La palabra clave super dentro de un constructor se utiliza para invocar explícitamente el constructor de la clase padre. Esto resulta necesario cuando la superclase no dispone de constructor sin parámetros, o cuando se desea inicializar sus atributos con valores concretos.

public Artillero(String nombre, int cohetes) {
    super(nombre);
    this.cohetes = cohetes;
}

Si la clase base no tiene visible el constructor vacío, sí debe llamarse a super(...) obligatoriamente, ya que Java lo necesita para inicializar la parte heredada del objeto.

3. Atributos privados en memoria
Respuesta

Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Un objeto Artillero contiene internamente tanto los atributos heredados de Soldado como sus propios atributos.

Sin embargo, que existan físicamente en memoria no implica que puedan accederse directamente desde el código de la subclase. La palabra private restringe el acceso exclusivamente a la clase donde se declara.

Por ejemplo, nombre existe dentro del objeto Artillero, pero no puede usarse directamente así:

// ERROR
System.out.println(nombre);

Debe accederse mediante métodos públicos o protegidos definidos en Soldado.

4. Extensibilidad
Respuesta

La compatibilidad de tipos mejora enormemente la extensibilidad del código. Permite añadir nuevos subtipos sin modificar el código que ya trabaja con la superclase.

Por ejemplo, si se añade un Francotirador, el recorrido del array no necesita cambios.

class Francotirador extends Soldado {
    public Francotirador(String nombre) {
        super(nombre);
    }
}
Soldado[] ejercito = {
    new Artillero("Carlos", 5),
    new Zapador("Luis", 3),
    new Francotirador("Marta")
};

for (Soldado s : ejercito) {
    s.saludar();
}

Esto facilita la evolución del software y reduce modificaciones innecesarias.

5. Referencias, upcasting y downcasting
Respuesta

Sí, una referencia del supertipo puede apuntar a objetos reales de un subtipo. Esto es precisamente la compatibilidad de tipos.

Esto se denomina upcasting, y ocurre de forma automática:

Soldado s = new Artillero("Carlos", 5);

Sin embargo, con la referencia s solo pueden invocarse métodos visibles en Soldado. Para acceder a métodos específicos del subtipo es necesario hacer downcasting.

for (Soldado s : ejercito) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println(a.getCohetes());
    }
}

instanceof permite comprobar el tipo real del objeto antes del casting.

6. Acceso protegido
Respuesta

El acceso protegido (protected) permite que un atributo o método sea accesible desde la propia clase y desde sus subclases.

En Java se implementa con la palabra clave protected.

class Soldado {
    protected String nombre;
}
class Zapador extends Soldado {
    public void ponerMinas() {
        System.out.println(nombre + " está colocando minas");
    }
}

Esto permite reutilización dentro de la jerarquía manteniendo cierto control.

7. Clase base común
Respuesta

En muchos lenguajes orientados a objetos existe una clase raíz común.

En Java, todas las clases heredan implícitamente de Java Platform, Standard Edition Object, aunque no se escriba explícitamente.

class Soldado extends Object

Esto permite que todos los objetos compartan métodos comunes como toString(), equals() o hashCode().

8. Herencia múltiple
Respuesta

La herencia múltiple consiste en que una clase pueda heredar directamente de varias clases a la vez.

Java no permite herencia múltiple de clases, para evitar ambigüedades.

// No permitido en Java
class C extends A, B

Sin embargo, sí permite implementar varias interfaces.

9. Excepción personalizada
Respuesta

Las excepciones son objetos y pueden heredarse para crear excepciones propias.

Una excepción no controlada hereda de RuntimeException.

class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }
}
class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super(causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
10. Herencia vs reutilización
Respuesta

No debe emplearse herencia únicamente para reutilizar código porque la herencia expresa una relación conceptual fuerte de “es-un”.

Si esa relación no existe realmente, el diseño se vuelve incorrecto y rígido.

Para reutilización simple suele ser preferible la composición, ya que desacopla mejor las clases.

11. Favorecer composición
Respuesta

Se recomienda favorecer la composición porque ofrece mayor flexibilidad y menor acoplamiento.

Con composición se construyen objetos usando otros objetos como componentes, evitando dependencias rígidas entre jerarquías.

Esto facilita cambios futuros y pruebas.

12. La herencia rompe la encapsulación
Respuesta

Se dice que la herencia rompe la encapsulación porque la subclase depende de detalles internos de la superclase.

Si cambia la implementación interna de la clase padre, las subclases pueden verse afectadas.

Esto aumenta el acoplamiento entre clases.

13. Herencia vs composición
Respuesta
Herencia
class Persona {
    protected String dni;
    protected String nombre;
}
class Estudiante extends Persona {}
class Trabajador extends Persona {}
Composición
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}
class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}
class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
