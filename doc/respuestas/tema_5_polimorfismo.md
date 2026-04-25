<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El **polimorfismo** es la capacidad que tiene una única referencia de un tipo base para apuntar a objetos de diferentes subclases y comportarse de manera distinta según el objeto real con el que esté trabajando. En programación orientada a objetos, sirve para simplificar drásticamente el código, permitiendo escribir algoritmos genéricos que operan sobre una superclase sin necesidad de conocer las particularidades de cada hijo. Esto evita el uso excesivo de estructuras condicionales (como `switch` o `if-else` en C) para discernir tipos, facilitando que el sistema sea escalable y fácil de mantener.

Para que el polimorfismo sea efectivo, es indispensable la **sobrescritura** (o *override*). Esta técnica consiste en redefinir en una subclase un método que ya ha sido declarado en la superclase, manteniendo exactamente la misma firma (nombre, parámetros y tipo de retorno). Al sobrescribir, se proporciona una implementación especializada que sustituye a la del padre cuando el método es invocado a través de una referencia que apunta a una instancia del hijo.



El mecanismo que hace posible esto es el **enlace dinámico** (o *dynamic binding*). A diferencia de los lenguajes procedimentales donde la dirección de la función se decide en tiempo de compilación, en Java la decisión de qué método ejecutar se toma en tiempo de ejecución basándose en el objeto real alojado en la memoria. Así, una lista de diversos tipos de soldados puede ser recorrida con un único bucle, logrando que cada uno ejecute su propia versión del método `atacar()` de forma totalmente automática.

```java
class Soldado {
    // Método base
    public void atacar() {
        System.out.println("El soldado realiza una acción de combate.");
    }
}

class Artillero extends Soldado {
    // Sobrescritura: Redefinición del comportamiento para el Artillero
    @Override
    public void atacar() {
        System.out.println("El artillero dispara un cañón de largo alcance.");
    }
}

public class Simulacion {
    public static void main(String[] args) {
        // Polimorfismo: Una referencia de 'Soldado' apunta a un 'Artillero'
        Soldado s = new Artillero();
        
        // Se ejecuta la versión sobrescrita en tiempo de ejecución
        s.atacar(); 
    }
}
```
****


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.
### Respuesta

La **ligadura dinámica** o **enlace tardío** (*dynamic binding*) es el mecanismo por el cual la asociación entre la llamada a un método y su implementación real se pospone hasta el tiempo de ejecución. En lugar de que el compilador decida qué dirección de memoria ejecutar (como ocurre con las funciones estándar en C), el entorno de ejecución de Java consulta el tipo real del objeto en la memoria para determinar qué versión del método invocar. Este proceso es el motor técnico que permite el polimorfismo, ya que garantiza que se ejecute la lógica especializada de la subclase incluso si la referencia utilizada es de la superclase.

La relación con el polimorfismo es de causa y efecto: el polimorfismo es el concepto conceptual, mientras que la ligadura dinámica es la implementación técnica que lo hace posible. Sin este mecanismo, una referencia de tipo `Soldado` siempre ejecutaría el código de la clase `Soldado`, ignorando si el objeto real es un `Artillero` o un `Zapador`. Gracias al enlace tardío, el programa gana flexibilidad, permitiendo que el comportamiento se adapte dinámicamente al contexto de los datos procesados en cada instante.

La necesidad de indicar esto explícitamente depende totalmente del lenguaje. En **Java**, todos los métodos no estáticos y no finales utilizan ligadura dinámica de forma predeterminada; el programador no tiene que solicitarlo. En cambio, en **C++**, el enlace es estático por defecto para ganar eficiencia; si se desea polimorfismo, el programador debe marcar los métodos en la clase base con la palabra reservada `virtual`. Si no se indica `virtual` en C++, el compilador resolverá la llamada según el tipo de la referencia o puntero, perdiendo el comportamiento polimórfico.

```java
// Ejemplo en Java: La ligadura dinámica es automática
class Soldado {
    void atacar() { System.out.println("Ataque base"); }
}

class Artillero extends Soldado {
    @Override
    void atacar() { System.out.println("Disparo Artillero"); }
}

// En ejecución, s.atacar() busca el método en el objeto real, no en la referencia.
Soldado s = new Artillero();
s.atacar(); // Salida: Disparo Artillero
```
****

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

El polimorfismo permite tratar objetos de diferentes subclases de manera uniforme a través de una referencia de la superclase. Al definir un array de tipo `Soldado`, es posible almacenar instancias tanto de `Zapador` como de `Artillero`. Gracias a la ligadura dinámica, al recorrer este array e invocar el método `saludar`, el entorno de ejecución de Java seleccionará automáticamente la versión del método que corresponda al objeto real almacenado en cada posición.

En este ejemplo, el `Zapador` utiliza la anotación `@Override` para sustituir íntegramente la lógica del padre por una propia. Por el contrario, el `Artillero` no redefine el método, por lo que heredará y ejecutará el comportamiento estándar definido en `Soldado`. Esta capacidad de respuesta diferenciada ante una misma llamada es la esencia del comportamiento polimórfico.

```java
class Soldado {
    public void saludar() {
        System.out.println("Soldado informándose.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("¡Zapador abriendo paso!");
    }
}

class Artillero extends Soldado {
    // No sobrescribe, usa el saludo genérico de la base
}

public class EjercicioPolimorfismo {
    public static void main(String[] args) {
        // Array de la superclase conteniendo diferentes subtipos
        Soldado[] peloton = {
            new Zapador(),
            new Artillero()
        };

        // Recorrido polimórfico
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }
}
```
****


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Es posible invocar la implementación de la superclase desde un método sobrescrito para reutilizar y ampliar su funcionalidad. Esta práctica es común cuando se desea que el comportamiento del subtipo sea una extensión del comportamiento base en lugar de una sustitución total. Para realizar esta llamada, se utiliza la palabra clave **`super`**, que permite acceder a los miembros (métodos o atributos) de la clase progenitora.

La instrucción `super.nombreDelMetodo()` indica al entorno de ejecución que debe buscar y ejecutar el código definido en la jerarquía superior inmediata. En el caso del `Zapador`, se invoca el saludo estándar del `Soldado` y, posteriormente, se añade la frase específica. Este mecanismo de delegación es fundamental para mantener la integridad de la lógica base mientras se especializa el comportamiento de las subclases.



```java
class Soldado {
    public void saludar() {
        System.out.print("Soldado informándose... ");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        // Se usa 'super' para llamar al método de la clase base
        super.saludar();
        
        // Se añade el comportamiento específico a continuación
        System.out.println("ZAPADOR A SUS ORDENES.");
    }
}

public class Prueba {
    public static void main(String[] args) {
        Soldado z = new Zapador();
        z.saludar(); // Imprime la combinación de ambos métodos
    }
}
```
****


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al realizar la **sobrescritura** de un método, el lenguaje impone restricciones estrictas para asegurar que la firma sea compatible. Los parámetros deben ser exactamente los mismos en cantidad, orden y tipo. Respecto al tipo de retorno, Java permite lo que se conoce como "tipos de retorno covariantes", es decir, el método sobrescrito puede devolver un objeto que sea una subclase del tipo devuelto por el método original, lo cual resulta útil para ganar especificidad en el subtipo.

La distinción entre **sobrescritura** (*overriding*) y **sobrecarga** (*overloading*) radica en la jerarquía y la firma. La sobrescritura ocurre entre clases distintas (padre e hijo) y mantiene la misma firma para proporcionar una implementación especializada. La sobrecarga, por el contrario, ocurre generalmente dentro de una misma clase y consiste en definir múltiples métodos con el mismo nombre pero con diferentes parámetros (en tipo o cantidad), comportándose como funciones distintas en tiempo de compilación.

La anotación **`@Override`** es una instrucción para el compilador que indica la intención explícita de sobrescribir un método de la superclase. Su uso es altamente recomendable porque actúa como un mecanismo de seguridad: si el programador comete un error tipográfico en el nombre del método o se equivoca en los parámetros, el compilador generará un error inmediatamente al no encontrar un método equivalente en la clase base. Sin ella, el programa podría compilar creando accidentalmente un método nuevo (sobrecarga), provocando errores de lógica difíciles de detectar.

```java
class Soldado {
    public Number obtenerRango() { return 1; }
    public void atacar() { System.out.println("Ataque base"); }
}

class Zapador extends Soldado {
    // Uso de @Override para seguridad
    // Ejemplo de retorno covariante: Integer es subclase de Number
    @Override
    public Integer obtenerRango() { return 10; }

    // SOBRECARGA: Mismo nombre, distintos parámetros
    public void atacar(String objetivo) { 
        System.out.println("Atacando a " + objetivo); 
    }

    @Override
    public void atacar() { 
        System.out.println("Ataque de zapador"); 
    }
}
```
****


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?
### Respuesta

Efectivamente, en Java se emplea el polimorfismo de manera intrínseca desde el inicio del aprendizaje, incluso sin ser consciente de ello. Dado que todas las clases heredan automáticamente de `java.lang.Object`, cualquier método de dicha clase raíz es susceptible de ser sobrescrito. Al redefinir métodos como `toString()` o `equals()`, se está proporcionando una implementación especializada que el sistema utilizará polimórficamente cada vez que necesite representar el objeto como texto o compararlo con otro.

Un ejemplo cotidiano de este comportamiento ocurre al imprimir un objeto mediante `System.out.println(objeto)`. Este método no sabe qué tipo de objeto está recibiendo, solo sabe que es un `Object` y que posee un método `toString()`. Gracias a la ligadura dinámica, Java ejecutará la versión personalizada que se haya definido en la clase específica (como `Soldado`), demostrando que el polimorfismo es el pilar que permite la interacción uniforme entre los componentes del lenguaje.



```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    // Sobrescritura de un método de la clase base universal Object
    @Override
    public String toString() {
        return "Soldado: " + nombre;
    }
}

public class Main {
    public static void main(String[] args) {
        Object s = new Soldado("Ryan");
        
        // Uso de polimorfismo: se invoca el toString() de Soldado 
        // aunque la referencia sea de tipo Object.
        System.out.println(s.toString());
    }
}
```
****


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?


Una **clase abstracta** es una clase que no puede ser instanciada por sí misma y que sirve como un molde o plantilla para otras subclases. Su propósito principal es definir una base común y forzar una jerarquía, dejando ciertas implementaciones incompletas para que los hijos las definan. Por otro lado, un **método abstracto** es aquel que se declara en la clase base pero carece de cuerpo (implementación); solo define la firma para asegurar que todas las subclases proporcionen su propia versión obligatoriamente.

No es posible crear instancias (objetos) de una clase abstracta mediante el operador `new`. Intentar hacerlo provocaría un error de compilación, ya que la clase se considera "incompleta" por diseño. Para utilizarla, se debe crear una subclase que herede de ella y que implemente todos los métodos abstractos pendientes, permitiendo así que el objeto final tenga un comportamiento definido para cada una de sus funciones.

En Java, la palabra clave `abstract` debe colocarse tanto en la declaración de la clase como en la declaración del método que no tiene cuerpo. Si una clase contiene al menos un método abstracto, la clase misma debe ser declarada obligatoriamente como abstracta. Esto garantiza que la estructura del programa sea coherente y que no existan objetos con comportamientos "vacíos".

```java
// La clase debe llevar 'abstract'
abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado presentándose.");
    }

    // El método abstracto no tiene llaves, termina en punto y coma
    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Colocando explosivos en el objetivo.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Disparando artillería de largo alcance.");
    }
}

public class Main {
    public static void main(String[] args) {
        // Soldado s = new Soldado(); // ERROR: No se puede instanciar
        
        Soldado s1 = new Zapador(); // Polimorfismo permitido
        s1.atacar(); 
    }
}
```
****


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave **`final`** actúa como un modificador de restricción que impide la extensión o modificación de elementos. Aplicada a una **clase**, indica que esta no puede tener descendencia; es decir, ninguna otra clase puede heredar de ella. Cuando se aplica a un **método**, permite que sea heredado por las subclases pero prohíbe terminantemente su sobrescritura. Esto asegura que el comportamiento del método permanezca inalterado a lo largo de toda la jerarquía.

En relación con el polimorfismo, el uso de `final` supone una limitación directa. Dado que el polimorfismo depende de la capacidad de las subclases para redefinir comportamientos mediante la sobrescritura, un método `final` rompe esta dinámica al garantizar que siempre se ejecute la versión de la superclase. Es una herramienta de diseño utilizada para proteger la integridad de algoritmos críticos que no deben ser alterados por programadores que hereden de nuestra clase.

Un ejemplo emblemático de clase `final` en la API estándar de Java es la clase **`String`**. Los diseñadores del lenguaje la definieron así por razones de seguridad y optimización (como el uso del *String Pool*). Si `String` no fuera final, cualquier programador podría crear una subclase que alterase el comportamiento básico de las cadenas de texto, comprometiendo la estabilidad de prácticamente todas las aplicaciones Java.

```java
// Clase final: nadie puede heredar de ella
final class SistemaDeSeguridad {
    public void validar() {
        System.out.println("Validación crítica inalterable.");
    }
}

class Soldado {
    // Método final: las subclases lo heredan pero NO pueden sobrescribirlo
    public final void identificarse() {
        System.out.println("Identificación biométrica obligatoria.");
    }
}

class Zapador extends Soldado {
    // @Override 
    // public void identificarse() { ... } // ERROR DE COMPILACIÓN
}
```
****


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las **interfaces** en Java son entidades que definen un **contrato** de comportamiento sin implementar la lógica. Se pueden entender como una colección de métodos abstractos y constantes que especifican *qué* debe hacer una clase, pero no *cómo*. A diferencia de las clases, no almacenan estado (variables de instancia) ni tienen constructores. Para un programador de C, una interfaz funciona de forma similar a un archivo de cabecera que solo contiene prototipos de funciones que deben ser desarrolladas obligatoriamente por quien las use.

Aunque presentan similitudes con las clases abstractas, existen diferencias clave. Mientras que una clase abstracta puede tener métodos con implementación y atributos comunes, una interfaz tradicional es puramente declarativa. Además, la jerarquía de clases abstractas se basa en la identidad ("es-un"), mientras que las interfaces se centran en capacidades o roles ("es-capaz-de"). Por ejemplo, un `Soldado` es una `Persona` (clase abstracta), pero también puede ser `Entrenable` o `Transportable` (interfaces).

Una de las mayores ventajas de las interfaces es que permiten la **herencia múltiple de tipos**. En Java, una clase solo puede heredar de una única superclase, pero tiene la libertad de implementar tantas interfaces como sea necesario. Esto permite que un objeto adquiera múltiples comportamientos independientes entre sí, facilitando un diseño modular y altamente desacoplado que no queda atrapado en una única línea sucesoria.

```java
interface Entrenable {
    void realizarEntrenamiento(); // Método abstracto por defecto
}

interface Conductor {
    void conducirVehiculo();
}

// Una clase implementa múltiples interfaces
class Zapador extends Soldado implements Entrenable, Conductor {
    @Override
    public void realizarEntrenamiento() {
        System.out.println("Entrenando desactivación de explosivos.");
    }

    @Override
    public void conducirVehiculo() {
        System.out.println("Conduciendo camión de transporte.");
    }

    @Override
    public void atacar() {
        System.out.println("Ataque de zapador.");
    }
}
```
****


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

El uso de **clases abstractas** permite definir operaciones generales, como calcular la distancia, dejando la lógica matemática específica a las subclases. Al declarar `calcularDistanciaA` como abstracto en la clase `Punto`, se obliga a `Punto2D` y `Punto3D` a implementar sus propias fórmulas (como el Teorema de Pitágoras en dos o tres dimensiones). Esto permite que otras clases, como `Linea`, trabajen con la abstracción `Punto` sin preocuparse por la complejidad dimensional interna.

Para garantizar que el cálculo se realice entre tipos compatibles, se utiliza el operador **`instanceof`** seguido de un **downcasting**. Dado que el método recibe un `Punto` genérico por polimorfismo, es necesario verificar que el objeto recibido sea efectivamente del mismo subtipo (por ejemplo, que un `Punto2D` reciba otro `Punto2D`) antes de forzar la conversión de tipo para acceder a sus coordenadas específicas. Sin esta comprobación, el programa lanzaría una excepción `ClassCastException` en tiempo de ejecución.

Este diseño demuestra la potencia del polimorfismo en la clase `Linea`. Al contener dos referencias de tipo `Punto`, la línea puede calcular su longitud invocando simplemente `inicio.calcularDistanciaA(fin)`. La clase `Linea` no necesita saber si está en un espacio 2D o 3D; la ligadura dinámica se encarga de ejecutar la versión correcta del cálculo basándose en el tipo real de los puntos que se le pasaron al constructor.

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;
    Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p2 = (Punto2D) otro; // Downcasting
            return Math.sqrt(Math.pow(p2.x - this.x, 2) + Math.pow(p2.y - this.y, 2));
        }
        return 0;
    }
}

class Linea {
    private Punto p1, p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double obtenerLongitud() {
        // Polimorfismo en acción: Linea no sabe si es 2D o 3D
        return p1.calcularDistanciaA(p2);
    }
}
```
****

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La **herencia de interfaces** ocurre cuando una interfaz hereda de otra mediante la palabra clave `extends`. A diferencia de las clases, este mecanismo permite que una interfaz hija agrupe los contratos de su progenitora y añada nuevas responsabilidades. Una clase que decida implementar la interfaz hija estará obligada por el compilador a proporcionar la implementación de todos los métodos definidos en la jerarquía completa.

En Java, a diferencia de la herencia de clases, sí existe la **herencia múltiple de interfaces**. Una interfaz puede extender varias interfaces simultáneamente (por ejemplo: `interface C extends A, B`). Esto es posible porque las interfaces no heredan estado ni implementaciones conflictivas, evitando el famoso "problema del diamante" de lenguajes como C++. Esto permite crear jerarquías de tipos muy ricas y modulares sin las restricciones de la herencia única.

En el ejemplo solicitado, se observa cómo `FicheroEscribible` especializa a `Fichero`. Cualquier clase que represente un archivo editable deberá cumplir con el contrato base (lectura) y con el contrato extendido (escritura y borrado), garantizando que los objetos sean tratados polimórficamente según el nivel de acceso que se requiera en el programa.

```java
interface Fichero {
    String leerContenido();
}

// Herencia de interfaces: FicheroEscribible "es un" Fichero
interface FicheroEscribible extends Fichero {
    void escribirContenido(String datos);
    void eliminar();
}

class DocumentoTexto implements FicheroEscribible {
    @Override
    public String leerContenido() {
        return "Contenido del archivo...";
    }

    @Override
    public void escribirContenido(String datos) {
        System.out.println("Guardando: " + datos);
    }

    @Override
    public void eliminar() {
        System.out.println("Archivo eliminado del sistema.");
    }
}
```
****