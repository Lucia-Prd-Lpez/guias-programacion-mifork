<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La **herencia** es uno de los pilares de la Programación Orientada a Objetos que permite a una clase (subclase o clase hija) adquirir las propiedades y métodos de otra (superclase o clase padre). La relación **"A es-un B"** actúa como una prueba de jerarquía: si podemos decir que un "Artillero es un Soldado", entonces la herencia es el mecanismo adecuado para modelar esa relación en el código.

1.  **Compatibilidad de tipos**: Gracias a la herencia, un objeto de una subclase puede ser tratado como si fuera de su superclase. Esto significa que una variable de tipo `Soldado` puede referenciar a un objeto `Artillero`. Es la base del polimorfismo, permitiendo que estructuras de datos genéricas manejen objetos especializados de forma uniforme.
2.  **Herencia de estado y comportamiento**: La subclase hereda los campos de datos (estado) y las funciones (comportamiento) de la clase superior. Esto permite centralizar la lógica común en la superclase para evitar la redundancia, mientras que las subclases solo implementan aquello que las hace únicas o específicas.

#### Ejemplo en Java

```java
// Clase base que define el estado y comportamiento común
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

// Subclase especializada en artillería
class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre); // Reutiliza el constructor del padre
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

// Subclase especializada en minas
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

// Clase de ejecución para demostrar la compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        // Aprovechamos la compatibilidad de tipos: 
        // Creamos un array de la clase padre que almacena diferentes hijos.
        Soldado[] peloton = {
            new Artillero("Ivanov", 5),
            new Zapador("Smith", 10),
            new Soldado("Recluta Generico")
        };

        // Recorremos el array. Aunque cada objeto es distinto, 
        // todos son compatibles con el tipo Soldado y pueden saludar.
        for (Soldado s : peloton) {
            s.saludar();
        }
    }
}
```


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Cuando se instancia una clase heredada, se produce una cadena de invocaciones que garantiza que el estado de la jerarquía completa se inicialice correctamente:

1.  **¿Cuántos constructores se ejecutan y en qué orden?**: Se ejecutan tantos constructores como niveles tenga la jerarquía de herencia. En el ejemplo anterior, al crear un `Artillero`, se ejecutan **dos** constructores. El orden de ejecución es **de arriba hacia abajo**: primero se ejecuta el constructor de la superclase (`Soldado`) y, una vez finalizado, se ejecuta el de la subclase (`Artillero`).
2.  **Significado de `super`**: Dentro de un constructor, la palabra reservada `super` se utiliza para invocar directamente a un constructor de la superclase. Esto permite pasar los argumentos necesarios desde el hijo hacia el padre para inicializar los atributos privados o protegidos de la base.
3.  **Llamada obligatoria a `super`**: Si la clase base **no tiene** un constructor sin parámetros (constructor por defecto) o este no es visible (es privado), la subclase **está obligada** a llamar explícitamente a uno de los constructores disponibles de la superclase mediante `super(...)`. Esta llamada debe ser siempre la **primera línea** de código dentro del constructor del hijo.

#### Ejemplo ilustrativo

```java
class Soldado {
    private String nombre;

    // Único constructor disponible: requiere un String
    public Soldado(String nombre) {
        System.out.println("1. Ejecutando constructor de Soldado");
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        // OBLIGATORIO: llamar a super porque Soldado no tiene constructor vacío
        super(nombre); 
        System.out.println("2. Ejecutando constructor de Zapador");
        this.minas = minas;
    }
}

public class Main {
    public static void main(String[] args) {
        // Esto disparará la cadena: primero Soldado, luego Zapador
        Zapador z = new Zapador("Erik", 10);
    }
}
```

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Al instanciar una subclase como `Artillero`, el objeto resultante en la memoria principal contiene todos los atributos definidos en la jerarquía de herencia, incluyendo aquellos declarados como privados en la superclase `Soldado`. Desde una perspectiva de estructura de datos, similar a cómo una `struct` en C reserva espacio para todos sus miembros, Java reserva un bloque de memoria contiguo donde se alojan tanto los campos específicos de la subclase como los campos heredados de la base. Por lo tanto, el atributo `nombre` reside físicamente dentro de cada instancia de `Artillero`.

A pesar de que estos atributos forman parte de la instancia en memoria, el principio de encapsulación restringe su acceso directo desde el código de la subclase. Si un atributo en `Soldado` se define con el modificador `private`, este no es visible para los métodos de `Artillero`, aunque hereden su existencia. El lenguaje impone esta barrera de visibilidad para asegurar que solo la clase que definió el dato pueda manipularlo directamente, protegiendo así la integridad del estado del objeto.

Para que la subclase pueda interactuar con esos datos privados que posee en memoria, se debe recurrir a la interfaz pública de la superclase. En el ejemplo propuesto, si un `Artillero` necesita utilizar el `nombre` para una operación específica, no podrá acceder a la variable directamente, sino que deberá invocar a un método "getter" público o protegido definido en `Soldado`. Esta distinción entre la existencia física del dato y su accesibilidad lógica es fundamental para mantener el desacoplamiento entre los niveles de la jerarquía.

```java
class Soldado {
    // El atributo existe en la memoria de cualquier hijo, pero es privado
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    // Método público para permitir el acceso controlado al estado privado
    public String getNombre() {
        return nombre;
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public void mostrarIdentidad() {
        // ERROR DE COMPILACIÓN: nombre tiene acceso privado en Soldado
        // System.out.println(nombre); 

        // FORMA CORRECTA: Se usa el comportamiento heredado
        System.out.println("Soy el artillero: " + getNombre());
    }
}
```
****

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos en una jerarquía de herencia es un pilar fundamental para lograr la extensibilidad del código. Este concepto permite que un sistema sea diseñado para interactuar con la clase base (el tipo más genérico), de modo que el código sea agnóstico respecto a las implementaciones específicas que puedan aparecer en el futuro. Al trabajar con referencias de la superclase, se crea un sistema flexible donde se pueden añadir nuevas funcionalidades o variantes sin necesidad de reescribir o recompilar la lógica que gestiona el conjunto.

En términos prácticos, esta característica reduce drásticamente el mantenimiento y el riesgo de introducir errores al ampliar el programa. Si se diseñara un sistema mediante estructuras de control condicionales (como `if` o `switch` en C) para diferenciar cada tipo de objeto, la adición de un nuevo elemento obligaría a modificar cada una de esas estructuras. Con la compatibilidad de tipos, el código existente permanece intacto, ya que el comportamiento se resuelve mediante el mecanismo de enlace dinámico de Java, cumpliendo así con el principio de diseño de estar "abierto a la extensión, pero cerrado a la modificación".

A continuación, se demuestra cómo la incorporación de una nueva clase `Medico` no requiere alterar el bucle que procesa la lista de soldados. El método `saludar()` es invocado de forma uniforme, independientemente de que el objeto real sea un `Artillero`, un `Zapador` o la nueva incorporación. Esta capacidad de tratar objetos de distintas clases bajo un tipo común simplifica la arquitectura y permite que el software crezca de manera orgánica.

```java
// Clase base ya existente
class Soldado {
    private String nombre;
    public Soldado(String nombre) { this.nombre = nombre; }
    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

// NUEVA CLASE: Se añade al sistema sin tocar el código de gestión principal
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() { return botiquines; }
}

public class Main {
    public static void main(String[] args) {
        // El sistema puede crecer añadiendo Médicos, Francotiradores, etc.
        Soldado[] peloton = {
            new Artillero("Ivanov", 5),
            new Zapador("Erik", 10),
            new Medico("Sánchez", 3) // Nueva subclase integrada perfectamente
        };

        // ESTE BLOQUE NO SE MODIFICA: Es extensible por naturaleza.
        // El bucle no sabe (ni necesita saber) qué es un Medico.
        for (Soldado s : peloton) {
            s.saludar();
        }
    }
}
```
****

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

En Java, es perfectamente posible y habitual que una referencia de una superclase apunte a un objeto real de una subclase. Esta capacidad es la esencia de la compatibilidad de tipos mencionada anteriormente; sin embargo, existe una limitación importante: a través de una referencia de tipo `Soldado`, solo se pueden invocar métodos que hayan sido definidos en la clase `Soldado`. Aunque el objeto en memoria sea un `Artillero`, el compilador solo garantiza la existencia de los miembros de la superclase, por lo que no permitirá llamar directamente a métodos específicos del subtipo (como `getCohetes()`) sin una conversión previa.

Los términos **"upcasting"** y **"downcasting"** describen los cambios de perspectiva sobre un objeto en la jerarquía. El **upcasting** es la conversión hacia arriba, de subclase a superclase (por ejemplo, tratar un `Artillero` como `Soldado`); es automático y seguro, ya que un hijo siempre cumple con la interfaz del padre. Por el contrario, el **downcasting** es la conversión hacia abajo, de superclase a subclase. Esta operación es potencialmente peligrosa y requiere un moldeado explícito (o *cast*), similar a la conversión de tipos en C, porque el compilador no puede asegurar por sí mismo que el objeto sea realmente del tipo esperado.

Para realizar un **downcasting** de forma segura y evitar excepciones en tiempo de ejecución, se utiliza el operador `instanceof`. Este operador realiza una comprobación booleana para verificar si un objeto pertenece a una clase determinada o a alguna de sus descendientes. En un entorno de desarrollo profesional, se emplea dentro de una estructura condicional antes de realizar la conversión explícita, garantizando así que el programa no intente tratar a un `Zapador` como si fuera un `Artillero`.

En el siguiente ejemplo se ilustra cómo, durante el recorrido de una colección genérica de soldados, se identifica dinámicamente a los artilleros para acceder a su estado específico. Se utiliza `instanceof` para filtrar el tipo y, posteriormente, se realiza el *cast* para habilitar la llamada al método `getCohetes()`.

```java
public class GestionCuartel {
    public static void main(String[] args) {
        Soldado[] peloton = {
            new Artillero("Ivanov", 8),
            new Zapador("Erik", 15),
            new Artillero("Volkov", 3)
        };

        for (Soldado s : peloton) {
            // El upcasting ocurre automáticamente al estar en el array
            s.saludar(); 

            // Se comprueba si el objeto real es de tipo Artillero
            if (s instanceof Artillero) {
                // Downcasting: Tratamos la referencia 's' como un Artillero
                Artillero a = (Artillero) s; 
                System.out.println("-> Munición: " + a.getCohetes() + " cohetes.");
            }
        }
    }
}
```
****


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso **"protegido"** (protected) es un nivel de visibilidad intermedio entre el acceso privado y el público. En lenguajes como C++, este modificador permite que los miembros de una clase sean inaccesibles para el resto del programa, pero totalmente visibles para las clases derivadas. En Java, el acceso protegido permite que un atributo o método sea accesible por cualquier clase que se encuentre en el mismo paquete y, además, por cualquier subclase, incluso si esta se encuentra en un paquete diferente.

La implementación en Java se realiza mediante la palabra reservada `protected` antes de la declaración del tipo del atributo o método. Al declarar un campo como protegido en la superclase `Soldado`, se está rompiendo parcialmente la encapsulación estricta a favor de la comodidad en la herencia. Esto permite que las subclases manipulen el estado heredado directamente sin necesidad de pasar por métodos "getters" o "setters", lo cual puede simplificar el código de los descendientes, aunque aumenta el acoplamiento entre el padre y sus hijos.

En el ejemplo solicitado, al marcar el atributo `nombre` como `protected`, la clase `Zapador` puede concatenar dicha variable directamente dentro de su lógica específica para poner bombas. Esto evita llamadas a métodos adicionales y permite un acceso directo a la memoria del objeto que, como se analizó en puntos anteriores, ya contenía físicamente ese dato. Sin embargo, se debe usar con cautela, ya que cualquier cambio en el nombre o tipo del atributo en la superclase obligará a revisar todas las subclases que lo accedan directamente.

```java
class Soldado {
    // Uso de 'protected' para permitir acceso a las subclases
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soldado " + nombre + " presente.");
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void colocarExplosivo() {
        // Acceso directo al atributo 'nombre' del padre por ser protected
        System.out.println(nombre + " está instalando una mina. Minas restantes: " + --minas);
    }
}

public class Main {
    public static void main(String[] args) {
        Zapador z = new Zapador("Erik", 5);
        z.colocarExplosivo();
    }
}
```
****


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En el ámbito de la programación orientada a objetos, el concepto de una raíz común en la jerarquía de tipos varía significativamente entre lenguajes. Algunos lenguajes, como Java, C# o Python, implementan lo que se conoce como una jerarquía de herencia única o unificada, donde existe una clase base universal de la que derivan todas las demás. Sin embargo, esto no ocurre en todos los lenguajes; por ejemplo, en C++, no existe una clase raíz obligatoria, permitiendo que existan múltiples jerarquías independientes (bosques de clases) sin una ascendencia común.

En el caso específico de Java, todas las clases derivan implícitamente de la clase `java.lang.Object`. Aunque un programador no utilice la palabra reservada `extends` al definir una clase, el compilador de Java la vincula automáticamente a `Object`. Esto garantiza que todos los objetos en el entorno Java compartan un conjunto mínimo de comportamientos fundamentales, como la capacidad de compararse (`equals`), convertirse a una representación de texto (`toString`) o sincronizarse en entornos multihilo (`wait`/`notify`).

Esta arquitectura de clase base universal facilita enormemente la creación de código genérico. Al saber que cualquier instancia, sea de una clase predefinida del sistema o de una creada por el usuario, "es-un" `Object`, se pueden diseñar estructuras de datos (como las colecciones antes de la introducción de los Genéricos) que almacenen cualquier tipo de dato mediante el uso de referencias a la clase raíz. En el contexto de los conocimientos previos en C, esto es conceptualmente similar a la versatilidad de un puntero `void*`, pero con la seguridad y los métodos integrados que ofrece el tipado de Java.

```java
// Aunque no se especifica 'extends', esta clase hereda de Object
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    // Al heredar de Object, podemos sobrescribir métodos como toString()
    @Override
    public String toString() {
        return "Objeto Soldado - Nombre: " + nombre;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado s = new Soldado("Marcus");
        
        // Uso de una referencia de la clase raíz universal
        Object obj = s; 
        
        // El objeto tiene métodos de la clase Object por herencia
        System.out.println(obj.getClass().getName());
        System.out.println(obj.toString());
    }
}
```
****


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La **herencia múltiple** es una característica de algunos lenguajes de programación orientada a objetos que permite que una clase derive de más de una clase base. Esto implica que una subclase podría heredar el estado y el comportamiento de varios padres de forma simultánea. En lenguajes como C++, esta capacidad es común; sin embargo, introduce problemas de ambigüedad técnica, como el conocido "problema del diamante", donde el compilador no puede determinar qué implementación de un método debe usar si dos clases padre definen el mismo método heredado de un abuelo común.

En el lenguaje Java, la herencia múltiple de clases **no existe**. Los diseñadores de Java decidieron omitir esta funcionalidad para evitar la complejidad y los errores que genera la ambigüedad en las jerarquías. En Java, una clase solo puede extender (`extends`) a una única superclase. Esta restricción garantiza una estructura de árbol clara y evita conflictos de nombres o de lógica entre diferentes ramas de herencia, simplificando tanto el diseño del compilador como la legibilidad del código para el programador.

Para paliar la falta de herencia múltiple y permitir que un objeto pueda comportarse de múltiples formas o cumplir con varios contratos, Java propone el uso de **Interfaces**. Mientras que la herencia de estado (atributos) está limitada a un solo padre, una clase puede implementar múltiples interfaces. Esto permite simular una forma de herencia múltiple de comportamiento sin los riesgos asociados a la herencia múltiple de estado. Desde la perspectiva de un programador de C, esto es comparable a la imposibilidad de heredar de varias `structs` con datos, pero manteniendo la libertad de cumplir con múltiples protocolos de funciones.

```java
// Clase base única
class Persona {
    protected String nombre;
}

// En Java NO se puede hacer: class Zapador extends Persona, Soldado { ... }
// Solo se permite una única superclase
class Zapador extends Persona {
    private int minas;

    public Zapador(String nombre, int minas) {
        this.nombre = nombre;
        this.minas = minas;
    }
}

/* Para obtener funcionalidades de distintos orígenes, se usarían interfaces:
   class Zapador extends Persona implements Explosivo, Combatiente { ... }
*/
```
****


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En Java, las excepciones son objetos integrados en la jerarquía de herencia, lo que permite extender su funcionalidad mediante la creación de clases personalizadas. Para definir una excepción como **no controlada** (unchecked), la nueva clase debe heredar de `RuntimeException`. A diferencia de las excepciones controladas (`Exception`), las no controladas no obligan al programador a declararlas en la firma del método mediante la cláusula `throws` ni a capturarlas de forma obligatoria, siendo ideales para errores de lógica o condiciones inesperadas que el programa no puede recuperar fácilmente.

La potencia de crear excepciones como objetos reside en la capacidad de añadirles estado y comportamiento, tal como se haría con cualquier otra clase de Java. Al incluir un atributo de tipo `Usuario` dentro de `UsuarioNoEncontradoException`, se está aplicando el concepto de **composición** dentro de la herencia. Esto permite que el bloque de código que capture la excepción no solo reciba un mensaje de error, sino también el objeto técnico o los datos específicos que causaron el fallo, facilitando enormemente las tareas de depuración y registro (logging).

Para cumplir con las buenas prácticas de la plataforma Java, es fundamental proporcionar constructores que permitan la **propagación de causas**. Mediante la sobrecarga de constructores y el uso de la instrucción `super`, se puede enviar hacia la clase base tanto el mensaje descriptivo como una excepción previa (la causa) que disparó el error actual. Esto mantiene la "traza de la pila" (stack trace) completa, permitiendo rastrear el origen del problema a través de las distintas capas de la aplicación, de forma similar a como se encadenarían errores en sistemas complejos.

```java
// Clase de apoyo para el ejemplo
class Usuario {
    private String id;
    public Usuario(String id) { this.id = id; }
    public String getId() { return id; }
}

// Excepción personalizada no controlada (hereda de RuntimeException)
class UsuarioNoEncontradoException extends RuntimeException {
    // Composición: la excepción contiene el objeto que falló
    private final Usuario usuario;

    // Constructor estándar con el usuario afectado
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Sobrecarga del constructor para incluir una causa (Throwable)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}

// Ejemplo de uso
public class GestionUsuarios {
    public void buscar(Usuario u) {
        // Lógica simulada que lanza la excepción
        throw new UsuarioNoEncontradoException("El usuario no existe en la BD", u);
    }
}
```
****


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

El uso de la herencia exclusivamente para la reutilización de código se considera una mala práctica debido a que crea un acoplamiento extremadamente fuerte entre la clase padre y la clase hija. Al heredar, la subclase se vuelve dependiente de la implementación interna de la superclase, lo que en ingeniería de software se denomina el "problema de la superclase frágil". Cualquier modificación en la estructura o en la lógica de la clase base puede propagar errores inesperados a todas sus descendientes, rompiendo funcionalidades que antes operaban correctamente y dificultando el mantenimiento a largo plazo.

Además, la herencia impone una jerarquía rígida que se define en tiempo de compilación y no puede alterarse durante la ejecución del programa. Si se utiliza la herencia simplemente para "tomar prestados" unos métodos, se obliga al objeto a cargar con todo el estado y comportamiento del padre, lo que puede resultar en objetos excesivamente pesados o con responsabilidades que no le corresponden. Este fenómeno vulnera el principio de diseño de que una clase debe tener una única razón para cambiar, ya que la subclase ahora es responsable tanto de su propia lógica como de la lógica heredada.

Por el contrario, la composición (la relación "A tiene-un B") ofrece una alternativa mucho más flexible y modular. En lugar de ser una versión de otra clase, un objeto simplemente contiene una instancia de la clase cuya funcionalidad desea reutilizar. Esto permite cambiar el comportamiento en tiempo de ejecución, facilita las pruebas unitarias mediante la inyección de dependencias y mantiene las clases aisladas. Desde la perspectiva de alguien con experiencia en C, la composición es similar a incluir una estructura dentro de otra para usar sus campos, lo cual es mucho más seguro y predecible que forzar una relación jerárquica artificial.

En definitiva, la herencia debe reservarse estrictamente para relaciones semánticas de tipo "es-un", donde la especialización es clara y necesaria. Para cualquier otro escenario donde el objetivo principal sea compartir lógica o comportamiento entre clases, la composición es la opción preferida. Al favorecer la composición sobre la herencia, se obtienen sistemas más desacoplados, fáciles de extender y con una arquitectura mucho más limpia y profesional.

```java
// Ejemplo de REUTILIZACIÓN mediante COMPOSICIÓN (Recomendado)
// En lugar de heredar de una clase compleja, la usamos como herramienta.

class Motor {
    public void arrancar() {
        System.out.println("Motor en marcha...");
    }
}

class Coche {
    // Relación "tiene-un": El coche tiene un motor, no ES un motor.
    private Motor motor = new Motor();

    public void iniciarViaje() {
        // Reutilizamos el código de Motor sin heredar sus problemas
        motor.arrancar();
        System.out.println("Coche desplazándose.");
    }
}

public class Main {
    public static void main(String[] args) {
        Coche miCoche = new Coche();
        miCoche.iniciarViaje();
    }
}
```
****


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

Favorecer la composición frente a la herencia es un principio de diseño que busca reducir el **acoplamiento** entre las clases. En la herencia, la subclase depende directamente de la implementación de la superclase; si la base cambia, el impacto se propaga a toda la cadena, lo que puede introducir errores en cascada. La composición, al basarse en la relación **"tiene-un"**, permite que un objeto utilice funcionalidades de otros sin heredar sus problemas estructurales o restricciones de tipo.

Además, la composición ofrece una flexibilidad superior en tiempo de ejecución. Mientras que la herencia es estática (un objeto no puede dejar de ser un `Soldado` una vez creado), la composición permite cambiar dinámicamente los componentes internos de una clase. Por ejemplo, un objeto `Soldado` podría cambiar su comportamiento de ataque simplemente sustituyendo un objeto `Arma` por otro, algo imposible de lograr mediante una jerarquía de clases rígida sin recurrir a una explosión de subclases innecesarias.

Finalmente, este enfoque respeta mejor el principio de **encapsulación**. En la herencia, las subclases a menudo tienen acceso a detalles internos del padre (especialmente con el uso de `protected`), lo que expone la lógica interna. Con la composición, los objetos interactúan exclusivamente a través de interfaces públicas bien definidas, facilitando que cada componente evolucione de forma independiente sin afectar al resto del sistema.

```java
// REUTILIZACIÓN MEDIANTE COMPOSICIÓN
class AtaqueConFusil {
    void ejecutar() { System.out.println("Disparando fusil..."); }
}

class Soldado {
    // El comportamiento se "compone" en lugar de heredarse
    private AtaqueConFusil ataque = new AtaqueConFusil();

    void realizarAccion() {
        ataque.ejecutar();
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado s = new Soldado();
        s.realizarAccion();
    }
}
```
****

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

La afirmación de que la herencia rompe la encapsulación se basa en que una subclase no solo utiliza la interfaz pública de su superclase, sino que a menudo depende de los detalles de su implementación interna para funcionar correctamente. A diferencia de un cliente externo que solo interactúa con los métodos públicos, la subclase está estrechamente ligada a cómo el padre gestiona su estado. Si el funcionamiento interno de la superclase cambia, incluso manteniendo su interfaz pública intacta, el comportamiento de la subclase puede verse alterado o corrompido de forma inesperada.

Este fenómeno es especialmente evidente cuando se utiliza el modificador `protected`. Al permitir que los hijos accedan directamente a los atributos o métodos internos del padre, se elimina la barrera de seguridad que la encapsulación debería proporcionar. El "contrato" entre las clases deja de ser exclusivamente a través de métodos controlados y pasa a ser una dependencia estructural. Esto genera que la superclase sea "frágil", ya que cualquier mejora o corrección en su lógica interna puede invalidar las asunciones bajo las cuales se construyeron las subclases.

En comparación con la composición, donde un objeto se comunica con otro estrictamente a través de sus métodos públicos como si fuera una "caja negra", en la herencia la subclase tiene una visión de "caja blanca" sobre la base. Esta visibilidad excesiva dificulta el mantenimiento del código a largo plazo, ya que el programador pierde la libertad de modificar la clase base sin el riesgo de generar fallos en cascada en toda la jerarquía de herencia.

```java
class ListaEnteros {
    private int contador = 0;

    public void añadir(int numero) {
        contador++;
        // Lógica de inserción...
    }

    public void añadirTodos(int[] numeros) {
        for (int n : numeros) {
            añadir(n); // La subclase depende de que se llame a añadir()
        }
    }
}

class MiLista extends ListaEnteros {
    // Si la superclase cambia su implementación interna de añadirTodos,
    // esta subclase podría contar mal los elementos o fallar,
    // porque su lógica depende de CÓMO está escrita la clase padre.
}
```
****

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

El modelado mediante **herencia** se basa en la jerarquía de tipos, donde tanto `Estudiante` como `Trabajador` se definen como especializaciones de una superclase `Persona`. En este esquema, el objeto hijo adquiere automáticamente el estado y el comportamiento del padre, estableciendo una relación rígida de "es-un". Esto simplifica el acceso a los datos comunes, pero acopla fuertemente las clases a la estructura de la clase base.

Por otro lado, el modelado por **composición** utiliza la relación "tiene-un", donde las clases `Estudiante` y `Trabajador` contienen una referencia a un objeto de la clase `DatosPersonales`. En este caso, los datos comunes se encapsulan en un objeto externo que se inyecta a través del constructor. Este enfoque es más flexible y permite que la lógica de los datos personales evolucione de forma independiente a la jerarquía de las ocupaciones, evitando la rigidez de la herencia única de Java.

A continuación se presentan ambos modelos para comparar su implementación:

```java
// --- OPCIÓN 1: HERENCIA ---
class Persona {
    protected String dni;
    protected String nombre;
    
    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante extends Persona {
    private String colegio;
    
    public Estudiante(String dni, String nombre, String colegio) {
        super(dni, nombre);
        this.colegio = colegio;
    }
}

// --- OPCIÓN 2: COMPOSICIÓN ---
class DatosPersonales {
    private String dni;
    private String nombre;
    
    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    // Getters necesarios...
}

class Trabajador {
    private String empresa;
    private DatosPersonales datos; // Composición

    public Trabajador(DatosPersonales datos, String empresa) {
        this.datos = datos;
        this.empresa = empresa;
    }
}
```
****