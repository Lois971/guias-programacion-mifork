Tema 4.1. Composición
1. En C, podemos crear estructuras mayores componiendo unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando struct, de una línea de puntos, donde puntos tienen dos coordenadas (x e y), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.
Respuesta

En C, la composición se puede implementar utilizando struct para definir objetos simples y combinarlos dentro de otros objetos. En este ejemplo, se define una estructura Punto con coordenadas x e y, y otra estructura Linea que contiene dos puntos.

#include <stdio.h>
#include <math.h>

typedef struct {
    double x, y;
} Punto;

typedef struct {
    Punto p1, p2;
} Linea;

double distancia(Punto a, Punto b) {
    return sqrt((b.x - a.x)*(b.x - a.x) + (b.y - a.y)*(b.y - a.y));
}

double longitudLinea(Linea l) {
    return distancia(l.p1, l.p2);
}

int main() {
    Linea l = {{0,0},{3,4}};
    printf("Longitud de la linea: %.2f\n", longitudLinea(l));
    return 0;
}

Esta implementación muestra cómo la estructura Linea tiene objetos de tipo Punto. Las funciones proporcionan abstracción al cálculo de distancias, aunque los puntos y la línea son modificables desde cualquier parte del código, lo que limita la encapsulación.

2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de composición en orientación a objetos. Crea una clase Punto, y una clase Linea. La clase Punto debe tener un método para calcular distancia a otro Punto y Linea debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.
Respuesta

En Java, la composición se implementa mediante clases y referencias a objetos. Se pueden declarar los atributos como private y finales, lo que asegura que los objetos sean inmutables. Por ejemplo:

public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.sqrt(Math.pow(this.x - otro.x, 2) + Math.pow(this.y - otro.y, 2));
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

La ventaja sobre C radica en la encapsulación: los puntos son inmutables y la línea no puede cambiar sus extremos después de ser creada, asegurando que la composición refleje fielmente la relación “tiene-un”.

3. ¿Qué significa la multiplicidad en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre Linea y Punto? Indícalo expresando la multiplicidad en ambas direcciones, de Linea a Punto y de Punto a Linea.
Respuesta

La multiplicidad indica cuántos objetos de una clase pueden estar relacionados con un objeto de otra clase en una relación de composición. Representa la cardinalidad mínima y máxima de la relación.

En el ejemplo de Linea y Punto, cada Linea tiene exactamente dos puntos. Desde Linea a Punto, la multiplicidad es 2; cada línea contiene dos instancias de Punto. Desde Punto a Linea, un punto puede ser parte de 0 o 1 líneas, dependiendo de si se comparte o no, por lo que la multiplicidad es 0..1.

4. ¿Qué significa composición fuerte y composición débil? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como "asociación o agregación" y a cuál como "composición" propiamente.
Respuesta

La composición fuerte indica que los objetos contenidos dependen totalmente del objeto contenedor para su existencia. Cuando el contenedor es destruido, los objetos contenidos también lo son. Este tipo de relación se conoce como composición propiamente dicha.

La composición débil, también llamada agregación o asociación, significa que los objetos contenidos pueden existir de manera independiente. Su ciclo de vida no depende del contenedor y pueden ser compartidos entre varios contenedores. La distinción es importante para modelar correctamente la dependencia y garantizar integridad de datos en sistemas orientados a objetos.

5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer new dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de "dependencia"?
Respuesta

Se trata de dependencia, no de composición. La dependencia ocurre cuando un objeto necesita otro solo de manera temporal, generalmente durante la ejecución de un método. El ciclo de vida del objeto dependiente no está ligado al objeto que lo usa. Este tipo de relación indica que la clase depende de otra para cumplir una función concreta, pero no posee ni controla el objeto de forma permanente.

6. En el ejemplo anterior de línea y punto, programa la relación entre Linea y Punto de dos formas. Una como composición fuerte, donde el ciclo de vida de los puntos está ligado al de Linea y otra como composición débil, donde no.
Respuesta

Composición fuerte:

public class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

Los puntos son creados dentro de LineaFuerte, su ciclo de vida depende de la línea.

Composición débil:

public class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
}

Los puntos pueden existir antes y después de la línea, son independientes, por lo que se trata de una composición débil o agregación.

7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que Linea destruya los Punto explícitamente, ¿Por qué?
Respuesta

En Java, la destrucción de objetos se gestiona mediante recolección de basura (Garbage Collector). No se necesita destruir explícitamente los objetos contenidos en una composición fuerte.

Cuando el contenedor (LineaFuerte) deja de estar referenciado, el recolector de basura también elimina los objetos Punto que únicamente eran referenciados por él. Esto asegura que el ciclo de vida de los objetos compuestos siga la composición fuerte sin requerir código adicional de destrucción.

8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo Profesor[], con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un Profesor al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.
Respuesta
class Profesor {
    private final String nombre;
    public Profesor(String nombre) { this.nombre = nombre; }
    public String getNombre() { return nombre; }
}

class Departamento {
    private Profesor[] profesores = new Profesor[50];
    private int count = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Debe haber un director");
        this.director = directorInicial;
        addProfesor(directorInicial);
    }

    public void addProfesor(Profesor p) {
        if (count >= 50) throw new IllegalStateException("Máximo 50 profesores");
        profesores[count++] = p;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= count) throw new IndexOutOfBoundsException();
        if (profesores[pos] == director) throw new IllegalStateException("No se puede eliminar al director");
        for (int i = pos; i < count - 1; i++) profesores[i] = profesores[i + 1];
        profesores[--count] = null;
    }

    public void setDirector(Profesor p) {
        boolean encontrado = false;
        for (int i = 0; i < count; i++) {
            if (profesores[i] == p) { encontrado = true; break; }
        }
        if (!encontrado) throw new IllegalArgumentException("El director debe ser profesor del departamento");
        this.director = p;
    }

    public int getNumProfesores() { return count; }
    public Profesor getProfesor(int pos) { 
        if (pos < 0 || pos >= count) throw new IndexOutOfBoundsException();
        return profesores[pos]; 
    }

    public Profesor getDirector() { return director; }
}

La clase asegura que siempre exista un director y que forme parte de los profesores.

Se oculta la implementación interna del array, cumpliendo la encapsulación.

9. En Java, en la composición débil, ¿cómo sería con List?
Respuesta
import java.util.ArrayList;
import java.util.List;

class DepartamentoList {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public DepartamentoList(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Debe haber un director");
        this.director = directorInicial;
        profesores.add(directorInicial);
    }

    public void addProfesor(Profesor p) { profesores.add(p); }

    public void removeProfesor(int pos) {
        if (profesores.get(pos) == director) throw new IllegalStateException("No se puede eliminar al director");
        profesores.remove(pos);
    }

    public void setDirector(Profesor p) {
        if (!profesores.contains(p)) throw new IllegalArgumentException("El director debe ser profesor del departamento");
        this.director = p;
    }

    public int getNumProfesores() { return profesores.size(); }
    public Profesor getProfesor(int pos) { return profesores.get(pos); }
}

Usando List se eliminan los límites de tamaño fijo y se simplifican los métodos add y remove.

Si se devolviera la lista interna directamente, se rompería la encapsulación; se puede devolver una copia o una lista inmutable (Collections.unmodifiableList) para protegerla.

10. Composiciones recursivas con Persona inmutable
Respuesta
public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }
}

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Bea", abuela);
        Persona nieto = new Persona("Carlos", madre);

        System.out.println("Nombre del nieto: " + nieto.getNombre());
        System.out.println("Nombre de la madre: " + nieto.getMadre().getNombre());
    }
}

Composiciones recursivas típicas incluyen estructuras de árbol, directorios de archivos, nodos en listas enlazadas y excepciones anidadas (Throwable.getCause()).

11. Relaciones de composición "bidireccionales"
Respuesta

Una relación bidireccional implica que ambos objetos conocen al otro. En el ejemplo de Profesor y Departamento, un profesor tendría una referencia a su departamento y el departamento mantiene una lista de profesores.

Para implementarlo:

class ProfesorBidireccional {
    private final String nombre;
    private DepartamentoBidireccional depto;

    public ProfesorBidireccional(String nombre) { this.nombre = nombre; }

    public void setDepartamento(DepartamentoBidireccional depto) { this.depto = depto; }
}

class DepartamentoBidireccional {
    private List<ProfesorBidireccional> profesores = new ArrayList<>();

    public void addProfesor(ProfesorBidireccional p) {
        profesores.add(p);
        p.setDepartamento(this);
    }
}

Es necesario mantener la sincronización: cuando se añade un profesor al departamento, también se actualiza la referencia del profesor al departamento.