<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

En el lenguaje C, para lograr una estructura de datos universal se suelen emplear punteros de tipo `void*`. Este tipo de puntero es genérico y puede almacenar la dirección de memoria de cualquier variable, independientemente de su naturaleza. Sin embargo, este enfoque carece de seguridad, ya que el programador es el único responsable de recordar qué tipo de dato hay en cada posición de memoria y de realizar el "casting" correcto al recuperar la información, lo que suele derivar en errores de segmentación.

En Java, antes de la llegada de la genericidad, se utilizaba una estrategia análoga empleando la clase `Object`. Dado que en Java todas las clases heredan de `Object`, un array de tipo `Object[]` tiene la capacidad polimórfica de almacenar cualquier objeto. Al igual que con los punteros `void*`, este método obliga a realizar una conversión de tipo (downcasting) cada vez que se extrae un elemento para poder acceder a sus métodos específicos, lo que aumenta el riesgo de excepciones en tiempo de ejecución.

El siguiente ejemplo ilustra cómo una clase `Almacen` puede contener un array de objetos para guardar cualquier elemento. Aunque es flexible, se observa que al recuperar los datos el compilador no puede ayudar a verificar si el tipo es correcto, dejando esa validación en manos del programador y su conocimiento del contenido del array.

```java
class Almacen {
    private Object[] elementos;
    private int contador = 0;

    public Almacen(int capacidad) {
        elementos = new Object[capacidad];
    }

    public void insertar(Object obj) {
        if (contador < elementos.length) {
            elementos[contador++] = obj;
        }
    }

    public Object extraer(int indice) {
        return elementos[indice];
    }
}

public class Main {
    public static void main(String[] args) {
        Almacen bolsa = new Almacen(5);
        bolsa.insertar("Texto"); // Almacena un String
        bolsa.insertar(100);     // Almacena un Integer (Autoboxing)

        // Problema: Es obligatorio hacer casting y podemos equivocarnos
        String s = (String) bolsa.extraer(0); 
        // Integer i = (Integer) bolsa.extraer(0); // Esto lanzaría ClassCastException
    }
}
```
****

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La **programación genérica** es un paradigma que permite diseñar algoritmos y estructuras de datos de forma que el tipo de dato exacto sea irrelevante durante su definición. Su propósito es permitir que una misma lógica funcione para múltiples tipos de datos, garantizando al mismo tiempo la **seguridad de tipos** (*type safety*). En lenguajes modernos como Java, esto se traduce en que el compilador "entiende" qué tipo de objeto se está guardando y recuperando, evitando errores de conversión.

El ejemplo anterior, basado en la clase `Object` o en punteros `void*`, se considera **programación universal o polimórfica**, pero no es estrictamente "programación genérica" en el sentido moderno del término. Aunque permite alojar cualquier dato, carece de la característica fundamental de la genericidad: el control de tipos en tiempo de compilación. En el ejemplo anterior, el lenguaje pierde la noción de qué hay dentro de la estructura, tratándolo todo como un "objeto genérico" y delegando la responsabilidad de la seguridad al programador.

La verdadera programación genérica en Java utiliza **parámetros de tipo** (como `<T>`). La diferencia clave es que, con la genericidad real, si se intenta extraer un `Integer` de una lista declarada para `String`, el programa no llegará a ejecutarse porque el compilador detectará el error. En el ejemplo de `Object`, el error solo saltaría cuando el usuario estuviera utilizando la aplicación, lo cual es mucho más peligroso.

```java
// Programación Genérica Real (con parámetros de tipo)
class AlmacenGenerico<T> {
    private T contenido;

    public void set(T contenido) { this.contenido = contenido; }
    public T get() { return contenido; }
}

// Aquí el compilador sabe que 'caja' SOLO puede tener Strings
AlmacenGenerico<String> caja = new AlmacenGenerico<>();
caja.set("Elemento");
String s = caja.get(); // No hace falta casting, es seguro.
```
****

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El principal problema de emplear `void*` o `Object` es la **pérdida de la seguridad de tipos** (*type safety*). Al utilizar estos contenedores universales, el compilador deja de supervisar qué tipo de datos se están insertando, tratando todos los elementos como una masa uniforme sin identidad específica. Esto traslada la responsabilidad del chequeo de tipos del compilador al programador, aumentando drásticamente la probabilidad de introducir errores lógicos que no serán detectados hasta que el programa esté en funcionamiento.

Otro inconveniente fundamental es la necesidad de realizar **conversiones de tipo explícitas** (*casting*) al recuperar los datos. Como el sistema solo devuelve una referencia genérica, el programador debe forzar el tipo manualmente para acceder a las propiedades del objeto original. Si por error se intenta convertir un dato a un tipo que no le corresponde (por ejemplo, tratar un `Integer` como un `String`), se producirá una excepción en tiempo de ejecución (`ClassCastException` en Java o un fallo de segmentación en C), lo que compromete la estabilidad y robustez de la aplicación.

Finalmente, este enfoque perjudica la **legibilidad y el mantenimiento** del código. Al observar la declaración de una estructura basada en `Object`, no existe ninguna pista visual sobre qué tipo de elementos se espera que contenga. Esto obliga a documentar exhaustivamente el código o a confiar en la memoria del desarrollador, lo que dificulta el trabajo en equipo y hace que la reutilización de la estructura de datos sea una tarea propensa a fallos accidentales.

```java
// Ejemplo del peligro con Object
Almacen bolsa = new Almacen(10);
bolsa.insertar("Manzana");
bolsa.insertar(25); // El compilador permite mezclar tipos sin avisar

// El error ocurre en ejecución, no al programar
String fruta = (String) bolsa.extraer(1); // ERROR: Intentando convertir 25 a String
```
****


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los **parámetros de tipo** son marcadores de posición o "variables de tipo" que se definen entre los símbolos de diamante (`< >`) en la declaración de una clase, interfaz o método. Funcionan como etiquetas simbólicas (normalmente letras mayúsculas como `T`, `E` o `K`) que representan un tipo de dato real que el usuario especificará más tarde, al crear una instancia. Para un programador de C, equivalen a una forma de "macro" inteligente que el compilador procesa para generar un código específico para el tipo solicitado sin duplicar la lógica manualmente.

Al utilizar un parámetro de tipo, se establece un **contrato de coherencia** en toda la clase. Si se define una clase `Caja<T>`, el parámetro `T` se utiliza en los métodos de entrada (como argumentos) y en los de salida (como tipo de retorno). De este modo, si se instancia la clase como `Caja<String>`, el compilador sustituye virtualmente todas las `T` por `String`, garantizando que solo se puedan guardar cadenas y que, al recuperarlas, el lenguaje devuelva exactamente una cadena sin necesidad de realizar conversiones manuales.

Existen convenciones estandarizadas en Java para nombrar estos parámetros, lo que facilita la comprensión del código. Por ejemplo, se utiliza `T` para un tipo genérico (*Type*), `E` para elementos de una colección (*Element*), `K` para claves (*Key*) y `V` para valores (*Value*). Estas letras no son obligatorias por sintaxis, pero seguirlas permite que cualquier desarrollador identifique rápidamente la función del parámetro dentro de la estructura de datos.

```java
// 'T' es el parámetro de tipo
public class Par<T> {
    private T primero;
    private T segundo;

    public Par(T primero, T segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
}

// Al instanciar, el parámetro de tipo se concreta
Par<Integer> coordenadas = new Par<>(10, 20);
Integer x = coordenadas.getPrimero(); // El compilador ya sabe que es Integer
```
****


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

En **C++**, la programación genérica se implementa mediante **plantillas** (*templates*). Cuando se instancia un `std::vector<std::string>`, el compilador genera en tiempo de compilación una versión específica de la clase vector diseñada exclusivamente para manejar cadenas de texto. El chequeo de tipos es estricto; si se intenta insertar un entero, el compilador arrojará un error. Al recorrer el vector, los elementos se tratan directamente como `std::string`, lo que permite acceder a sus métodos sin conversiones.

En **Java**, se utiliza la sintaxis de **Generics**. A diferencia de C++, Java no crea una clase nueva para cada tipo (debido a un proceso llamado *type erasure*), pero el compilador realiza todas las comprobaciones de seguridad necesarias. Al declarar un `ArrayList<String>`, se garantiza que la lista solo contenga referencias a objetos `String`. Al iterar sobre ella, el compilador inserta automáticamente los "castings" necesarios de forma invisible para el programador, asegurando que cada elemento sea tratado como el tipo concreto especificado.

Ambos lenguajes logran el mismo objetivo de seguridad de tipos, eliminando los peligros de los punteros `void*` o las referencias a `Object`. La principal ventaja es que el código que realiza el recorrido es limpio y predecible, ya que la estructura de datos garantiza por contrato que su contenido es homogéneo.

```cpp
// Ejemplo en C++ utilizando Templates
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> lista;
    lista.push_back("Alfa");
    lista.push_back("Bravo");

    // Recorrido seguro: 's' es de tipo std::string
    for (const std::string& s : lista) {
        std::cout << "Elemento: " << s << std::endl;
    }
    return 0;
}
```
****


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

El comportamiento del compilador varía drásticamente entre ambos lenguajes debido a sus filosofías de diseño. En **C++**, se produce la **instanciación de plantillas**: cada vez que se usa un tipo nuevo (como `vector<int>` y `vector<float>`), el compilador genera físicamente dos copias completas del código de la clase en el binario. Esto optimiza el rendimiento al máximo, ya que el código resultante es específico para cada tipo, aunque puede incrementar el tamaño del archivo ejecutable.

Por el contrario, Java utiliza el **Type Erasure** (borrado de tipos). El compilador emplea los genéricos solo para validar la seguridad durante la escritura del código; una vez comprobado que no hay errores de tipo, "borra" los parámetros (como `T`) y los sustituye por la clase raíz `Object`. Esta decisión se tomó para garantizar la **compatibilidad hacia atrás**, permitiendo que el código nuevo con genéricos funcione en máquinas virtuales antiguas que no conocían este concepto.

Como consecuencia del borrado de tipos, la información sobre el tipo concreto desaparece en tiempo de ejecución. Para la Máquina Virtual de Java (JVM), tanto un `ArrayList<String>` como un `ArrayList<Integer>` son exactamente el mismo tipo de objeto en memoria. Esto impone restricciones que en C++ no existen: en Java no se pueden crear instancias directamente de un tipo genérico (`new T()`) ni crear arrays de `T`, ya que la JVM no conoce el tipo real ni cuánta memoria reservar en ejecución.

En resumen, mientras que C++ crea código a medida para cada tipo (especialización), Java utiliza una única implementación genérica y gestiona la seguridad mediante inserciones automáticas de "castings" que el programador no ve, pero que aseguran que el programa sea robusto.

```java
// Código escrito por el programador:
ArrayList<String> lista = new ArrayList<>();
lista.add("Hola");
String s = lista.get(0);

// Lo que el compilador genera internamente (tras el Type Erasure):
ArrayList lista = new ArrayList();
lista.add("Hola");
String s = (String) lista.get(0); // El compilador añade el casting automáticamente
```
****

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

La clase genérica `Par` se define utilizando dos parámetros de tipo, convencionalmente denominados `K` y `V` (o `T1` y `T2`), lo que permite que cada instancia de la clase maneje tipos de datos independientes entre sí. Esto proporciona una flexibilidad superior a las estructuras fijas de C, permitiendo que un mismo contenedor sirva para emparejar un `String` con un `Integer`, o en este caso, dos valores de tipo `Double`.

Al utilizar esta clase como tipo de retorno en una función, se resuelve la limitación de Java de devolver un único valor. En lugar de recurrir a efectos colaterales (como modificar punteros pasados por referencia en C) o crear una clase específica para cada combinación, se emplea `Par<Double, Double>` para encapsular ambos resultados estadísticos de forma segura y legible.

```java
// Clase genérica para dos tipos distintos
class Par<K, V> {
    private K primero;
    private V segundo;

    public Par(K primero, V segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public K getPrimero() { return primero; }
    public V getSegundo() { return segundo; }
}

public class Estadistica {
    public static Par<Double, Double> calcular(double[] datos) {
        double media = 0, desv = 0;
        // ... lógica de cálculo ...
        return new Par<>(media, desv);
    }

    public static void main(String[] args) {
        double[] valores = {1.0, 2.5, 3.0};
        Par<Double, Double> resultado = calcular(valores);
        System.out.println("Media: " + resultado.getPrimero());
    }
}
```
****

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Los métodos genéricos permiten aplicar la genericidad de forma puntual, sin necesidad de que toda la clase que los contiene esté parametrizada. Al declarar el parámetro de tipo `<T>` justo antes del tipo de retorno, el método adquiere la capacidad de adaptar su firma según los argumentos que recibe. Para un programador con base en C, esto funciona como una "plantilla" que el compilador de Java utiliza para validar que los tipos de entrada y salida sean coherentes entre sí.

El uso de esta técnica resuelve drásticamente el problema del **downcasting**. Si el método se definiera utilizando la clase `Object`, el valor devuelto sería siempre una referencia genérica, obligando al programador a realizar una conversión manual (por ejemplo, de `Object` a `String`) para poder utilizar el objeto. Con genéricos, el método devuelve directamente el tipo `T` inferido, por lo que el objeto recuperado mantiene su tipo específico sin riesgo de errores en tiempo de ejecución.

Además, los parámetros de tipo permiten **forzar la homogeneidad** de los argumentos. Mientras que una versión basada en `Object` permitiría pasar un `String` y un `Integer` simultáneamente sin que el compilador detecte un problema, la versión genérica con el mismo parámetro `T` para ambos argumentos obliga a que ambos objetos pertenezcan al mismo tipo. Esto asegura que la lógica del programa sea consistente y evita comportamientos inesperados que, de otro modo, solo se detectarían al ejecutar la aplicación.

```java
public class Utilidades {

    // (ii) El parámetro <T> fuerza que a, b y el retorno sean del mismo tipo
    public static <T> T seleccionaUno(T a, T b) {
        return Math.random() > 0.5 ? a : b;
    }

    public static void main(String[] args) {
        // (i) Evita downcasting: 'resultado' recibe un String directamente
        String resultado = seleccionaUno("Cara", "Cruz");

        // El compilador impediría lo siguiente por incoherencia de tipos:
        // Integer error = seleccionaUno("Texto", 100); 
        
        System.out.println("Elegido: " + resultado);
    }
}
```
****



## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Es posible establecer restricciones en los parámetros de tipo mediante la cláusula `extends`. En Java, esto se conoce como **Bounded Type Parameters** (parámetros de tipo acotados). Al declarar `<T extends Number>`, se le indica al compilador que `T` puede ser cualquier clase que herede de `Number` (como `Integer`, `Double` o `Float`). Esto permite que, dentro de la clase genérica, se puedan invocar métodos de la clase `Number`, como `doubleValue()`, algo que sería imposible con un tipo genérico sin restricciones.

En la primera solución, al usar directamente la clase `Number` (polimorfismo clásico), el `Punto` es flexible pero perdemos la homogeneidad. Podríamos tener un `Punto` con una coordenada `Integer` y otra `Double`. En cambio, con la solución genérica `<T extends Number>`, obligamos a que ambas coordenadas sean del mismo tipo específico de número, lo que refuerza el chequeo de tipos y evita mezclas accidentales de precisión.

Respecto al **type erasure**, cuando un parámetro de tipo tiene una restricción, el compilador no lo sustituye por `Object`. En su lugar, lo sustituye por el **primer límite de la restricción** (el *leftmost bound*). Por lo tanto, tras la compilación, el tipo `T` desaparece y se convierte en `Number`. Esto permite que el *bytecode* resultante sea compatible con cualquier subclase de número, manteniendo la eficiencia sin necesidad de duplicar el código para cada tipo numérico.

```java
// Solución 1: Usando polimorfismo simple (sin generics)
class PuntoNumber {
    private Number x, y;
    public PuntoNumber(Number x, Number y) { this.x = x; this.y = y; }
    public double getX() { return x.doubleValue(); }
    public double getY() { return y.doubleValue(); }
}

// Solución 2: Usando Genericidad Acotada (Bounded Generics)
class Punto<T extends Number> {
    private T x, y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

public class Main {
    public static void main(String[] args) {
        // Obligamos a que sea un Punto de Integers
        Punto<Integer> p1 = new Punto<>(0, 0);
        Punto<Integer> p2 = new Punto<>(3, 4);
        System.out.println("Distancia: " + p1.calcularDistanciaA(p2));
    }
}
```
****



## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

La diferencia fundamental entre ambas soluciones radica en la **granularidad del control de tipos**. En la solución sin genéricos (basada en la clase `Number`), es perfectamente posible crear un punto con coordenadas heterogéneas, como una `x` de tipo `Integer` y una `y` de tipo `Double`. Esto ocurre porque el array o los atributos están definidos con el tipo base, aceptando cualquier subclase de forma mezclada. Sin embargo, en la solución genérica `<T extends Number>`, el uso de la misma variable de tipo `T` para ambas coordenadas impone una restricción de **homogeneidad**: una vez que se instancia como `Punto<Integer>`, el compilador prohibirá que cualquiera de sus coordenadas sea un `Double`.

Esta distinción se refleja claramente en el valor de retorno del método `getX`. En la solución sin genéricos, el método devuelve siempre una referencia de tipo `Number`. Si el programador necesita realizar una operación específica de enteros (como un desplazamiento de bits o usar un método propio de `Integer`), se verá obligado a realizar un **casting** manual. Por el contrario, en la solución con genéricos, `getX` devuelve el tipo exacto `T` con el que se instanció la clase; si el punto es de tipo `Punto<Integer>`, el método devolverá un `Integer`, permitiendo el acceso directo a toda su funcionalidad sin conversiones adicionales.

En términos de robustez, la solución genérica ofrece una ventaja crítica al detectar errores de diseño en tiempo de compilación. Mientras que la solución con `Number` permite mezclar tipos por accidente (lo que podría causar errores de precisión inesperados en cálculos complejos), los genéricos obligan al desarrollador a ser explícito sobre la naturaleza numérica de las coordenadas. Esto reduce la carga cognitiva del programador, ya que el compilador actúa como un sistema de vigilancia que garantiza que los datos que entran y salen del objeto mantienen su integridad original.

```java
public class Reflexion {
    public static void main(String[] args) {
        // SOLUCIÓN SIN GENÉRICOS: Permite mezclar (menos seguro)
        PuntoNumber pNum = new PuntoNumber(10, 5.5); 
        Number coord = pNum.getX(); // Devuelve Number, requiere casting para Integer
        
        // SOLUCIÓN CON GENÉRICOS: Fuerza homogeneidad (más seguro)
        Punto<Integer> pGen = new Punto<>(10, 20);
        Integer x = pGen.getX(); // Devuelve Integer directamente
        
        // El compilador impide la mezcla en la versión genérica:
        // Punto<Integer> error = new Punto<>(10, 5.5); // Error de compilación
    }
}
```
****



## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

Para solucionar este problema, debemos parametrizar la interfaz `Punto` con un tipo `T`. La clave está en definir la interfaz como `Punto<T>`, de modo que el método `distanciaA` reciba un objeto de tipo `T` en lugar de una interfaz genérica. Al implementar la interfaz, cada clase concreta (como `Punto2D`) se define a sí misma como el tipo esperado, lo que se conoce técnicamente como el **Curiously Recurring Template Pattern** (CRTP) adaptado a Java.

Con este cambio, el compilador garantiza que un `Punto2D` solo pueda compararse con otro `Punto2D`. Desaparece la necesidad de usar `instanceof` para verificar el tipo en tiempo de ejecución y el **downcasting** manual, ya que el argumento `p` llega al método con el tipo concreto correcto. Si intentas pasar un `Punto3D` a un método que espera un `Punto2D`, el error se producirá en tiempo de compilación, evitando excepciones en pleno funcionamiento del programa.

```java
// Interfaz parametrizada: T representa el tipo de punto compatible
public interface Punto<T> { 
    public double distanciaA(T p); 
} 

public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 

    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    // Ahora p es directamente Punto2D, sin castings ni instanceof
    public double distanciaA(Punto2D p) { 
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2)); 
    } 
} 

public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z; 

    public Punto3D(double x, double y, double z) { 
        this.x = x; this.y = y; this.z = z; 
    } 

    @Override 
    public double distanciaA(Punto3D p) { 
        return Math.sqrt(Math.pow(x - p.x, 2) + 
                         Math.pow(y - p.y, 2) + 
                         Math.pow(z - p.z, 2)); 
    } 
}
```
****


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

En Java, la respuesta es no para los genéricos pero sí para los arrays. Aunque `String` sea un subtipo de `Object`, **`List<String>` no es subtipo de `List<Object>`**. Se dice que los genéricos son **invariantes**. Si Java permitiera esta relación, podrías pasar una lista de strings a un método que acepte una lista de objetos; dicho método podría intentar insertar un `Integer` en ella, rompiendo la seguridad de tipos cuando intentaras leer de nuevo la lista original esperando solo cadenas.

Por el contrario, los arrays sí son **covariantes**: **`String[]` es un subtipo de `Object[]`**. Esto se decidió antes de que existieran los genéricos para permitir algoritmos polimórficos simples, pero conlleva un riesgo. Si tratas un `String[]` como un `Object[]` e intentas guardar un número en él, el compilador lo permitirá, pero la Máquina Virtual de Java lanzará una excepción `ArrayStoreException` en tiempo de ejecución al detectar que el tipo real del array no admite ese objeto.

A partir de este comportamiento, podemos definir la varianza de la siguiente manera:

* **Invariante**: No existe relación de herencia entre los tipos complejos aunque sus parámetros la tengan. Si $A$ es subtipo de $B$, $C<A>$ no tiene relación con $C<B>$. Java Generics es invariante por defecto.
* **Covariante**: La relación de herencia se mantiene. Si $A$ es subtipo de $B$, entonces $C<A>$ es subtipo de $C<B>$. Los arrays en Java son covariantes.
* **Contravariante**: La relación de herencia se invierte. Si $A$ es subtipo de $B$, entonces $C<B>$ es subtipo de $C<A>$. En Java, esto se logra mediante comodines (*wildcards*) como `<? super T>`.

```java
// Ejemplo del problema con Arrays (Covarianza insegura)
String[] strings = new String[2];
Object[] objetos = strings; // Permitido por covarianza
objetos[0] = 10; // Compila, pero lanza ArrayStoreException en ejecución

// Ejemplo con Generics (Invarianza segura)
List<String> listaStr = new ArrayList<>();
// List<Object> listaObj = listaStr; // Error de compilación: Invarianza
```
****


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.
Un **wildcard** o comodín (`?`) representa un tipo desconocido en Java Generics. Se utiliza para flexibilizar la restricción de invarianza, permitiendo que un método acepte diferentes tipos relacionados por herencia. Dependiendo de si usamos `extends` o `super`, habilitamos la lectura o la escritura segura siguiendo el principio **PECS** (*Producer Extends, Consumer Super*).

`List<? extends T>` establece un **límite superior**. Indica que la lista puede contener objetos de tipo `T` o de cualquier subclase de `T`. Como sabemos que todo lo que salga de la lista será, al menos, un `T`, es ideal para **leer** datos (Productor). Sin embargo, no permite añadir elementos (salvo `null`), ya que el compilador no sabe si la lista real es de `T` o de una subclase específica con la que el nuevo objeto podría ser incompatible.

`List<? super T>` establece un **límite inferior**. Indica que la lista es de tipo `T` o de cualquier superclase de `T` (como `Object`). Esto garantiza que es seguro **añadir** objetos de tipo `T` o sus subclases (Consumidor), porque la lista siempre será capaz de almacenarlos. En cambio, al leer, solo podemos estar seguros de que obtendremos un `Object`, perdiendo la información del tipo específico.

```java
import java.util.List;
import java.util.ArrayList;

public class EjemploWildcards {

    // (i) COVARIANZA: ? extends Number
    // Permite leer cualquier tipo de número para sumarlo.
    public static double sumarLista(List<? extends Number> lista) {
        double suma = 0;
        for (Number n : lista) { // Lectura segura como Number
            suma += n.doubleValue();
        }
        return suma;
    }

    // (ii) CONTRAVARIANZA: ? super Integer
    // Permite añadir enteros en listas de Integer, Number o Object.
    public static void añadirEnteros(List<? super Integer> lista) {
        lista.add(10); // Escritura segura
        lista.add(20);
    }

    public static void main(String[] args) {
        List<Double> listaDouble = List.of(1.5, 2.5);
        System.out.println("Suma: " + sumarLista(listaDouble)); // Funciona por extends

        List<Number> listaNum = new ArrayList<>();
        añadirEnteros(listaNum); // Funciona por super
    }
}
```
****

