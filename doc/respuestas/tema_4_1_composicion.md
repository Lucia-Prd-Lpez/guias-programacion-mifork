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

La **composición** es un concepto universal en la Programación Orientada a Objetos (POO) y en lenguajes estructurados como C. Representa la relación **"tiene-un"** (has-a), donde una clase o estructura compleja utiliza otras más simples como piezas de construcción.

---

En C, usamos `struct` para agrupar datos. En Java, usamos `class` para agrupar tanto **datos** (atributos) como **comportamiento** (métodos). 

| Característica | En Lenguaje C | En Java |
| :--- | :--- | :--- |
| **Contenedor** | `struct` | `class` |
| **Relación** | Un `struct` contiene otro `struct`. | Una clase tiene una referencia a otra clase. |
| **Funciones** | Están fuera de la estructura. | Son métodos dentro de la clase. |

---

## 2. Implementación en Código (Relacionando ambos)

Aquí tienes el código solicitado. He mantenido la lógica de la pregunta original pero aplicada a la sintaxis de Java para que veas la transición.

```java
// --- El Componente Básico ---
class Punto {
    double x;
    double y;

    // Constructor: Inicializa las coordenadas
    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método para calcular distancia entre ESTE punto y OTRO
    public double calcularDistancia(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

// --- El Componente Compuesto (Composición) ---
class Linea {
    // Una Linea "tiene-un" Punto de inicio y un Punto de fin
    private Punto p1;
    private Punto p2;

    Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    // Método para hallar la longitud de la línea
    public double calcularLongitud() {
        // Delegación: La línea le pide al punto que calcule la distancia
        return p1.calcularDistancia(p2);
    }
}

// --- Clase Principal para probar ---
public class Main {
    public static void main(String[] args) {
        Punto a = new Punto(0, 0);
        Punto b = new Punto(3, 4);

        // Composición en acción
        Linea miLinea = new Linea(a, b);

        System.out.println("Longitud de la línea: " + miLinea.calcularLongitud());
    }
}

```
  
---


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

```java
import java.util.Objects;

/**
 * Clase Punto: Representa una coordenada en un espacio bidimensional.
 * Se garantiza la inmutabilidad mediante el uso de 'final' y la ausencia de setters.
 */
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    /**
     * Calcula la distancia entre este punto y otro punto recibido por parámetro.
     */
    public double calcularDistancia(Punto otro) {
        Objects.requireNonNull(otro, "El otro punto no puede ser nulo");
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

/**
 * Clase Linea: Ejemplo de composición donde una línea "tiene-un" punto de inicio y fin.
 * La ocultación de información asegura que la estructura interna no sea alterada.
 */
class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        Objects.requireNonNull(inicio, "El punto de inicio no puede ser nulo");
        Objects.requireNonNull(fin, "El punto de fin no puede ser nulo");
        this.inicio = inicio;
        this.fin = fin;
    }

    /**
     * Calcula la longitud de la línea delegando la lógica al método del Punto.
     */
    public double calcularLongitud() {
        return inicio.calcularDistancia(fin);
    }

    public Punto getInicio() {
        return inicio;
    }

    public Punto getFin() {
        return fin;
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(0, 0);
        Punto p2 = new Punto(3, 4);

        Linea miLinea = new Linea(p1, p2);

        System.out.println("Longitud de la línea: " + miLinea.calcularLongitud());
        
        // La inmutabilidad garantiza que ni p1, ni p2, ni miLinea cambien su estado original.
    }
}
```
---

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La **multiplicidad** en la composición (y en las asociaciones en general) define el número de instancias de una clase que pueden estar vinculadas a una instancia de otra clase. Indica los límites inferior y superior de la relación (por ejemplo: 1, 0..1, 1..*, etc.).

En el contexto de la **composición**, la multiplicidad también implica una relación de ciclo de vida: si el objeto "todo" se destruye, los objetos "parte" que lo componen también suelen desaparecer.

En el ejemplo de `Linea` y `Punto`, la multiplicidad se expresa de la siguiente manera:

*   **De `Linea` a `Punto` (1 a 2):**
    Una instancia de `Linea` está compuesta exactamente por **2** instancias de `Punto` (el punto de inicio y el punto de fin). No puede tener ni más ni menos para ser considerada una línea válida en este modelo.

*   **De `Punto` a `Linea` (0..1 o 1..1):**
    En una composición estricta, un `Punto` específico pertenece a **una única** `Linea`. Si el `Punto` es una parte intrínseca de la `Linea`, no puede ser compartido por otras líneas simultáneamente. Por lo tanto, la multiplicidad es **1** (o **0..1** si el punto puede existir antes de ser asignado a una línea, aunque en composición pura el ciclo de vida está ligado).

| Dirección | Multiplicidad | Explicación |
| :--- | :--- | :--- |
| **Linea → Punto** | **2** | Una línea siempre requiere exactamente dos puntos (inicio y fin). |
| **Punto → Linea** | **1** | En composición, la "parte" pertenece exclusivamente a un único "todo". |

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.


La distinción entre estas dos formas de organizar objetos radica en el nivel de dependencia y en quién es el "propietario" de la existencia de las partes.

#### Composición Fuerte (Composición propiamente dicha)
Se refiere a una relación de pertenencia estricta donde las partes **no tienen sentido de existencia** fuera del objeto "todo". 
*   **Ciclo de vida:** Existe una dependencia vital absoluta. Si el objeto contenedor se destruye, las partes se destruyen con él. Las partes suelen ser creadas y gestionadas internamente por el contenedor.
*   **Ejemplo:** Un `Corazón` dentro de un `Humano`. Si el humano deja de existir, el corazón (como parte de ese sistema) también desaparece.

#### Composición Débil (Agregación)
Se refiere a una relación donde las partes son independientes y pueden existir por sí solas, incluso si el contenedor desaparece.
*   **Ciclo de vida:** El ciclo de vida es independiente. Si el objeto contenedor se destruye, las partes permanecen intactas en la memoria para ser usadas por otros objetos.
*   **Ejemplo:** Un `Libro` dentro de una `Biblioteca`. Si la biblioteca se incendia y desaparece como institución, el libro (si se salvó o estaba fuera) sigue siendo un objeto funcional por sí mismo.

---

#### Resumen de términos y consecuencias

| Concepto | Término técnico | Tipo de composición | Símbolo UML | Ciclo de vida |
| :--- | :--- | :--- | :--- | :--- |
| **Composición** | **Composición** | **Fuerte** | Rombo Negro (Relleno) | **Vinculado:** El "Todo" es dueño de la "Parte". |
| **Agregación** | **Agregación** | **Débil** | Rombo Blanco (Vacío) | **Independiente:** La "Parte" sobrevive al "Todo". |

**Consecuencia principal:** En la **composición (fuerte)**, el objeto contenedor tiene la responsabilidad total de la creación y destrucción de sus partes. En la **agregación (débil)**, el contenedor solo tiene una referencia a un objeto que fue creado externamente, permitiendo que ese mismo objeto sea compartido por varios contenedores.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

En estos casos, hablamos de **dependencia** (también conocida como relación de "uso").

A diferencia de la composición o la agregación, donde una clase "tiene" a otra como parte de su estructura permanente (atributo de clase), la dependencia es una relación mucho más débil y transitoria.

#### Características de la Dependencia:
*   **Temporalidad:** La relación solo existe durante la ejecución de un método específico. Una vez que el método termina, la relación desaparece.
*   **Uso, no pertenencia:** La clase A "usa" a la clase B para realizar una tarea puntual, pero la clase B no es un componente de la identidad de A.
*   **Inestabilidad:** Si la clase B cambia, es probable que la clase A deba cambiar (por eso depende de ella), pero A no "es dueña" de B.

#### Escenarios comunes de Dependencia:
1.  **Parámetros:** `public void imprimir(Documento doc)` -> La clase actual depende de `Documento` solo para imprimirlo.
2.  **Variables Locales:** `double resultado = Calculadora.sumar(a, b);` -> Se usa la clase `Calculadora` momentáneamente dentro del bloque.
3.  **Instanciación local (`new`):** Si creas un objeto dentro de un método para una operación rápida y no lo guardas en un atributo, es una dependencia.

| Relación | Concepto | Persistencia | Representación UML |
| :--- | :--- | :--- | :--- |
| **Composición/Agregación** | "Tiene-un" | Permanente (Atributo) | Línea sólida con rombo |
| **Dependencia** | "Usa-un" | Temporal (Método) | Línea discontinua con flecha |

**Conclusión:** Para que exista **composición**, el objeto debe formar parte del estado de la clase (ser un atributo). Si el objeto solo "pasa por allí" dentro de un método, es simplemente una **dependencia**.

---

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.


La diferencia técnica en Java radica principalmente en **quién crea los objetos** y si se permite que las referencias existan fuera de la clase contenedora.

#### Opción A: Composición Fuerte
Aquí, la `Linea` es dueña absoluta de los puntos. Los crea internamente y no permite que nadie de fuera le pase puntos ya existentes, garantizando que esos puntos solo existan para esa línea.

```java
class LineaFuerte {
    private final Punto inicio;
    private final Punto fin;

    // El constructor recibe coordenadas, NO objetos Punto
    // La Linea se encarga de INSTANCIAR sus propias partes
    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }
}
```
---

## 7. En Java, en la composición fuerte, ¿cuándo el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿por qué?

En Java no existe una instrucción explícita (como el `free` de **C** o el `delete` de **C++**) para destruir objetos manualmente. Esto se debe a la existencia del **Garbage Collector (Recolector de Basura)**.

La destrucción de los objetos en una composición fuerte ocurre siguiendo esta lógica:

1.  **Inalcanzabilidad:** Cuando el objeto contenedor (`Linea`) deja de ser utilizado (por ejemplo, su variable sale del ámbito o se asigna a `null`), el Garbage Collector detecta que ya no es "alcanzable" por el programa.
2.  **Pérdida de Referencias Internas:** Al ser una **composición fuerte**, los objetos `Punto` solo existen porque la `Linea` los referencia. Si la `Linea` es marcada para ser eliminada, las referencias a sus atributos `inicio` y `fin` también desaparecen.
3.  **Recolección Automática:** Si no hay ninguna otra variable en todo el programa apuntando a esos puntos, el Garbage Collector entiende que son "basura" y libera su memoria de forma automática.

**¿Por qué no se observa código de destrucción?**
Porque en Java la gestión de memoria es **implícita**. El programador se ocupa de decidir cuándo un objeto deja de ser necesario (quitando sus referencias), y el entorno de ejecución (JVM) se encarga de la limpieza física de los bytes en la memoria RAM.

> **Resumen:** En Java, "destruir" un objeto significa simplemente **dejar de apuntar a él**. Si el dueño (la Línea) muere, sus pertenencias (los Puntos) mueren con él porque nadie más tiene sus "direcciones".
>
> ---

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.


Este ejemplo muestra una **agregación (composición débil)** ya que los profesores existen independientemente del departamento. La clave aquí es mantener la **invariante de integridad**: el director siempre debe estar presente en la lista de profesores.

```java
/**
 * Clase Profesor: Representa la entidad básica (Parte).
 */
class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null) throw new IllegalArgumentException("El nombre no puede ser nulo");
        this.nombre = nombre;
    }

    public String getNombre() { return nombre; }
}

/**
 * Clase Departamento: Contenedor que gestiona la composición débil.
 * Invariante: El director SIEMPRE debe estar en la lista de profesores.
 */
class Departamento {
    private final Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    /**
     * Constructor: Establece el director inicial, quien automáticamente 
     * pasa a ser el primer profesor de la lista.
     */
    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Un departamento debe tener un director desde el inicio.");
        }
        // Agregamos al director como primer profesor para cumplir la invariante
        this.profesores[0] = directorInicial;
        this.director = directorInicial;
        this.numProfesores = 1;
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("No se puede añadir un profesor nulo.");
        if (numProfesores >= 50) throw new IllegalStateException("Departamento completo (máx 50).");
        
        profesores[numProfesores] = p;
        numProfesores++;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición de profesor inválida.");
        }

        // INVARIANTE: No se puede eliminar al profesor que actualmente es director
        if (profesores[posicion] == director) {
            throw new IllegalStateException("No se puede eliminar al profesor mientras sea el director actual.");
        }

        // Desplazamiento del array para cubrir el hueco (ocultando la implementación interna)
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    /**
     * Cambia el director. Solo se permite si el nuevo director ya es 
     * un profesor del departamento.
     */
    public void setDirector(Profesor nuevoDirector) {
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }

        if (!encontrado) {
            throw new IllegalArgumentException("El director debe ser un profesor existente del departamento.");
        }
        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }

    public int getCantidadProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        return profesores[posicion];
    }
}
```
---

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?


Al emplear `java.util.List` (generalmente a través de `ArrayList`), el código se vuelve más legible y menos propenso a errores de gestión de memoria manual.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Departamento {
    // Usamos List en lugar de array primitivo
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director no puede ser nulo.");
        }
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public void añadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor nulo.");
        // Ya no necesitamos controlar el máximo de 50 manualmente si no queremos
        profesores.add(p);
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        
        // Invariante: No eliminar al director
        if (profesores.get(posicion) == director) {
            throw new IllegalStateException("No se puede eliminar al director actual.");
        }

        profesores.remove(posicion);
    }

    public void setDirector(Profesor nuevoDirector) {
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento.");
        }
        this.director = nuevoDirector;
    }

    public int getCantidadProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int posicion) {
        return profesores.get(posicion);
    }

    /**
     * Solución al problema de seguridad: Devolver una vista inmodificable.
     */
    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
```
---

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

Una **composición recursiva** ocurre cuando una clase tiene un atributo cuyo tipo es la propia clase. Esto permite crear estructuras en cadena o en árbol.

```java
/**
 * Clase Persona Inmutable con composición recursiva.
 */
class Persona {
    private final String nombre;
    private final Persona madre; // Composición recursiva: una Persona "tiene-una" Persona

    // Constructor para personas con madre conocida
    public Persona(String nombre, Persona madre) {
        if (nombre == null) throw new IllegalArgumentException("El nombre es obligatorio");
        this.nombre = nombre;
        this.madre = madre;
    }

    // Constructor para la "ancestra" (madre desconocida/null)
    public Persona(String nombre) {
        this(nombre, null);
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }

    @Override
    public String toString() {
        return nombre + (madre != null ? " (hijo/a de " + madre.getNombre() + ")" : " (raíz familiar)");
    }
}

public class MainRecursivo {
    public static void main(String[] args) {
        // Creamos la familia desde la raíz (Abuela) hacia abajo
        Persona abuela = new Persona("Elena");
        Persona madre = new Persona("Sofía", abuela);
        Persona nieto = new Persona("Lucas", madre);

        System.out.println("Linaje familiar:");
        System.out.println("Nieto: " + nieto);
        System.out.println("Madre: " + nieto.getMadre());
        System.out.println("Abuela: " + nieto.getMadre().getMadre());
    }
}
```
---
## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?


Una relación **bidireccional** es aquella en la que ambos objetos involucrados en la relación tienen conocimiento el uno del otro. Mientras que en una relación unidireccional solo el "contenedor" conoce a sus "partes" (el `Departamento` conoce sus `Profesores`), en la bidireccional la "parte" también guarda una referencia al "todo" (el `Profesor` sabe a qué `Departamento` pertenece).

#### ¿Qué habría que hacer para implementar esto?

Para transformar el ejemplo anterior en una relación bidireccional, se deben realizar los siguientes cambios técnicos:

1.  **Atributo en la clase contenida:** Se debe añadir un atributo de tipo `Departamento` dentro de la clase `Profesor`.
2.  **Gestión de la consistencia:** Es el punto más crítico. Cuando añades un profesor a un departamento, debes asegurarte de que el atributo `departamento` de ese profesor se actualice para apuntar al departamento correcto.
3.  **Sincronización de métodos:** Los métodos que modifican la relación (como `añadirProfesor` o `eliminarProfesor`) deben actualizar ambos lados de la relación para evitar estados inconsistentes (por ejemplo, que un profesor crea que está en el Departamento A, pero el Departamento A no lo tenga en su lista).

#### Ejemplo de implementación lógica:

```java
class Profesor {
    private String nombre;
    private Departamento departamento; // Referencia al "Todo" (Bidireccionalidad)

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    // Método para que el departamento se asigne a sí mismo al profesor
    protected void setDepartamento(Departamento dept) {
        this.departamento = dept;
    }

    public Departamento getDepartamento() {
        return departamento;
    }
}

class Departamento {
    private List<Profesor> listaProfesores = new ArrayList<>();

    public void añadirProfesor(Profesor p) {
        if (p != null) {
            listaProfesores.add(p);
            // Sincronización: informamos al profesor que ahora pertenece a este departamento
            p.setDepartamento(this); 
        }
    }
}
```
