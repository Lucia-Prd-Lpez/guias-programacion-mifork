<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

La programación orientada a objetos se apoya en cuatro pilares: **encapsulamiento**, **abstracción**, **herencia** y **polimorfismo**. El encapsulamiento oculta el estado interno del objeto y expone operaciones controladas, separando interfaz pública e implementación para proteger la integridad de los datos y reducir el acoplamiento.

La abstracción modela conceptos destacando sus rasgos esenciales y omitiendo detalles irrelevantes, lo que simplifica el razonamiento y permite trabajar con interfaces estables. La herencia reutiliza y extiende comportamiento de clases existentes, organizando jerarquías. El polimorfismo posibilita que una misma operación (interfaz) tenga distintas implementaciones según el tipo concreto, favoreciendo extensibilidad.

---

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Cuatro lenguajes populares con soporte de POO son **Java**, **C++**, **Python** y **C#**. Java y C# promueven un uso sistemático de clases y objetos, lo que facilita la adopción de buenas prácticas orientadas a objetos desde el inicio.

C++ combina paradigmas (estructurado, genérico y orientado a objetos), permitiendo elegir el enfoque según el problema. Python adopta un modelo dinámico en el que casi todo es un objeto, haciendo muy natural el uso de clases, herencia y polimorfismo.

---

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La **programación estructurada** organiza el código mediante secuencias, decisiones y bucles, evitando saltos incontrolados como `goto`. Este enfoque promueve legibilidad, corrección y mantenimiento, y se apoya en la descomposición del problema en funciones bien definidas.

La **programación modular** divide el sistema en módulos cohesionados con responsabilidades claras. Cada módulo agrupa funciones y datos relacionados, mejorando la reutilización, las pruebas y la mantenibilidad. Este enfoque anticipa ideas de POO, como la separación de responsabilidades.

---

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Un objeto se caracteriza por **estado**, **comportamiento** e **identidad**. El **estado** lo componen los atributos que describen su información interna, la cual puede variar durante la ejecución.

El **comportamiento** se materializa en los métodos que operan sobre el estado o interactúan con otros objetos. La **identidad** distingue a un objeto de otro, incluso si comparten exactamente el mismo estado y comportamiento.

---

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una **clase** es una plantilla que define atributos y métodos, es decir, la estructura y el comportamiento compartido por sus objetos. No es lo mismo que un objeto: un objeto es una **instancia** concreta creada a partir de la clase, con estado e identidad propios.

Si bien muchos lenguajes de POO usan clases (Java, C#, C++), existen enfoques **basados en prototipos** (como el modelo original de JavaScript) que prescinden de clases formales. Por tanto, la orientación a objetos no exige necesariamente el concepto de clase.

---

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

La ubicación en memoria depende del lenguaje y del modo de creación. En **C++**, un objeto local puede residir en la **pila** (stack), mientras que con `new` se ubica en el **montón** (heap). En **Java**, los objetos se crean siempre en el **montón**, gestionado por la máquina virtual.

La **recolección de basura** (garbage collection) libera automáticamente la memoria de objetos que ya no son accesibles, evitando fugas. Lenguajes como Java o Python la integran; en C++ suele realizarse gestión manual (o mediante punteros inteligentes) para controlar el ciclo de vida.

---

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un **método** es una función asociada a una clase que define parte del comportamiento de sus objetos. Puede acceder y modificar el estado interno del objeto, encapsulando la lógica relacionada con ese tipo.

La **sobrecarga de métodos** permite declarar varios métodos con el mismo nombre pero firmas distintas (parámetros diferentes). La resolución elige cuál invocar según los argumentos, ofreciendo múltiples variantes de una misma operación bajo un nombre coherente.

---

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

```java
class Punto {
    int x;
    int y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen()); // 5.0
    }
}

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

El punto de entrada de un programa en Java es el método **`public static void main(String[] args)`**. La JVM busca exactamente esa firma para iniciar la ejecución. El método es `public` para ser accesible desde la JVM, `static` para poder invocarse sin instancia, `void` porque no devuelve resultado, y recibe un arreglo de cadenas con argumentos de línea de comandos.

La palabra clave **`static`** indica pertenencia a la **clase** y no a una instancia. Se utiliza más allá de `main`: sirve para **métodos utilitarios**, **miembros compartidos** (contadores, caches) y **bloques estáticos** de inicialización. Combinado con **`final`**, suele emplearse para **constantes** (`public static final`) inmutables y accesibles sin crear objetos, mejorando claridad e integridad del código.

---

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

La compilación básica desde consola se realiza con `javac Nombre.java`, lo que genera uno o varios archivos **`.class`** en la misma carpeta. Para ejecutar, se usa `java NombreDeLaClase` (sin la extensión), asumiendo que el archivo `.class` está en el classpath actual. Si hay paquetes, se debe invocar desde el directorio raíz del paquete o ajustar el classpath.

Java se **compila a bytecode**, un formato intermedio independiente de la plataforma. Ese bytecode vive en los **ficheros `.class`** y es ejecutado por la **Máquina Virtual de Java (JVM)**, que lo interpreta/JIT-optimiza a código nativo en tiempo de ejecución. Este diseño proporciona **portabilidad**: se compila una vez y se ejecuta en cualquier sistema con una JVM compatible.

---

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

La palabra clave **`new`** crea un objeto en el **montón (heap)** y devuelve una **referencia** a la nueva instancia. Durante este proceso se invoca un **constructor**, que es un método especial con el mismo nombre que la clase y sin tipo de retorno, responsable de **inicializar** el estado del objeto.

Los constructores pueden sobrecargarse para ofrecer distintas formas de creación. También se puede definir un constructor por defecto (sin parámetros) o encadenar constructores usando `this(...)` para centralizar la lógica de inicialización y evitar duplicidades.

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

La referencia **`this`** señala al **objeto actual** dentro de un método de instancia o un constructor. Sirve para **desambiguar** entre atributos y parámetros con el mismo nombre, para **encadenar constructores** (`this(...)`) y para **pasar** la propia referencia del objeto a otros métodos. Aunque muchas veces podría omitirse al acceder a campos de instancia, su uso explícito mejora la **legibilidad** y evita confusiones cuando hay nombres similares en el ámbito local.

No en todos los lenguajes se emplea el mismo identificador. En **C++** también se utiliza `this` (siendo un puntero `T* this`), mientras que en **Python** se usa el parámetro **`self`** de forma explícita en la firma de los métodos. En **Java**, `this` se suministra de forma **implícita** y no aparece en la lista de parámetros del método; simplemente está disponible en el cuerpo del método o constructor.

```java
class Punto {
    int x;
    int y;

    // Desambiguación entre parámetros y atributos con el mismo nombre
    void set(int x, int y) {
        this.x = x;  // 'this.x' es el atributo; 'x' es el parámetro
        this.y = y;
    }

    // Encadenamiento de constructores con this(...)
    Punto() {
        this(0, 0); // delega en el otro constructor
    }

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // Uso de 'this' para claridad al acceder al estado del objeto
    double distanciaA(Punto otro) {
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

Es natural definir un método de instancia que calcule la **distancia euclídea** entre el objeto actual y otro `Punto`. El método recibe un parámetro del mismo tipo, resta coordenadas, y aplica la raíz cuadrada de la suma de cuadrados. Conceptualmente, se trabaja con el vector diferencia `⟨dx, dy⟩` y su norma.

El uso de `this` deja claro que las coordenadas utilizadas pertenecen al objeto receptor. Aunque no es obligatorio para acceder a campos de instancia, mejora la legibilidad, especialmente cuando existen variables con nombres similares en el ámbito local.

```java
class Punto {
    int x;
    int y;

    double distanciaA(Punto otro) {
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función?

### Respuesta

En Java, el paso de parámetros es siempre **por valor**, pero lo que se copia depende del tipo. En el caso de tipos primitivos como `int`, se copia el **valor literal**, por lo que el método recibe una copia independiente. Al modificar el parámetro dentro del método, el valor original del llamador permanece inalterado, de forma similar al comportamiento de C con primitivas.

En el caso de objetos como `Punto`, lo que se copia es la **referencia**, no el objeto. Por tanto, tanto el método como el llamador apuntan al **mismo objeto en memoria**, lo que implica que los cambios sobre los **atributos** dentro del método sí se reflejan fuera. Sin embargo, si se asigna una nueva referencia dentro del método (por ejemplo, `p = new Punto()`), esta reasignación solo afecta a la copia local y no modifica la referencia en el llamador.

---

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

El método `toString()` devuelve una **representación textual** del objeto y se hereda de la clase base `Object`. Aunque su implementación por defecto suele ser poco informativa, es habitual sobrescribirlo para mostrar de forma clara y comprensible el estado interno del objeto. Al imprimir un objeto con `System.out.println`, Java invoca automáticamente este método.

Muchos lenguajes tienen mecanismos equivalentes. Por ejemplo, Python utiliza `__str__()` y C++ emplea la sobrecarga del operador `<<` para `ostream`. En cualquier caso, su propósito es facilitar la depuración y permitir exponer el estado del objeto de forma legible. Un ejemplo en Java sería:

```java
class Punto {
    int x;
    int y;

    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta

Un `struct` en C comparte con una clase la capacidad de agrupar datos bajo un mismo tipo, pero no incorpora los mecanismos propios de la programación orientada a objetos. Le faltan elementos fundamentales como **métodos asociados**, **encapsulamiento**, **constructores**, **destructores**, **herencia** y **polimorfismo**, lo que impide que pueda mantener lógica y restricciones internas del mismo modo que una clase.

Además, un `struct` no ofrece control de visibilidad (`private`, `public`) ni la capacidad de garantizar invariantes del tipo. El programador debe separar manualmente los datos y las funciones que operan sobre ellos, sin ayuda por parte del lenguaje para vincularlos de forma natural. Por esta razón, las variables de un `struct` no se consideran verdaderas **instancias**, ya que carecen de identidad y comportamiento propios integrados en el tipo.

---

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Para emular una clase en C, puede definirse un `struct` que contenga los datos y una función externa que reciba un puntero al `struct` para operar sobre él. De este modo, se logra un comportamiento parecido al de un “método”, aunque la relación entre datos y funciones no está integrada en el lenguaje, sino establecida manualmente por el programador. Esto permite implementar cierta lógica asociada, pero sin encapsulación ni características de orientación a objetos.

Al no existir `this` en C, es necesario **pasar explícitamente** un puntero al objeto como primer argumento de la función. Ese puntero cumple el papel de referencia al “objeto actual”, pero su manejo depende completamente del programador, sin sintaxis ni verificación adicional por parte del lenguaje. Un ejemplo sería:

```c
#include <math.h>

typedef struct {
    int x;
    int y;
} Punto;

double calculaDistanciaAOrigen(const Punto* p) {
    return sqrt((double)p->x * p->x + (double)p->y * p->y);
}