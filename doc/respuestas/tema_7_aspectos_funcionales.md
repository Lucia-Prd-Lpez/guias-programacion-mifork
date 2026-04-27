<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

En el lenguaje C, un puntero a función es una variable que almacena la dirección de memoria donde reside el código ejecutable de una función. A diferencia de un puntero convencional que apunta a un dato en el montón (*heap*) o en la pila (*stack*), este permite invocar lógica de manera dinámica. Para un programador de C, esta herramienta es fundamental para implementar "callbacks" o tablas de funciones, permitiendo que el comportamiento de un programa se decida en tiempo de ejecución al pasar funciones como argumentos a otras funciones.

La sintaxis para declarar estos punteros suele resultar compleja al principio, ya que requiere especificar tanto el tipo de retorno como la lista exacta de tipos de los parámetros, todo encerrado entre paréntesis para indicar la precedencia. El nombre de la función en C actúa, en la mayoría de los contextos, como la dirección de memoria de la misma, de forma similar a cómo funciona el nombre de un array, lo que facilita la asignación del puntero.

```c
#include <stdio.h>
#include <ctype.h>

// Definición de la función que transforma a mayúsculas
void transformar(char *s) {
    while (*s) {
        *s = toupper((unsigned char)*s);
        s++;
    }
}

int main() {
    char cadena[] = "hola mundo";

    // Declaración del puntero a función:
    // Retorno (void), nombre del puntero (*aMayusculas) y parámetro (char *)
    void (*aMayusculas)(char *) = transformar;

    // Invocación a través del puntero
    aMayusculas(cadena);

    printf("%s\n", cadena); // Imprime: HOLA MUNDO
    return 0;
}
```
****

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una **función lambda** es una función anónima, es decir, una función que se define sin un nombre identificador. Se trata de una construcción sintáctica que permite definir bloques de lógica de manera compacta y directa en el lugar donde se necesitan, tratándolos como datos que pueden ser asignados a variables o pasados como argumentos. A diferencia de las funciones tradicionales en C, las lambdas a menudo permiten la "clausura" (*closure*), capturando variables del entorno léxico donde han sido creadas.

En los lenguajes de alto nivel modernos, las lambdas eliminan la verbosidad de definir estructuras completas (como funciones globales en C o clases anónimas en Java) para tareas sencillas. Su sintaxis suele basarse en el uso de una flecha o símbolo similar que separa los parámetros de entrada del cuerpo de la función. Esto facilita enormemente la programación funcional, permitiendo que el código sea más declarativo y legible al centrarse en la transformación de los datos en lugar de en la gestión de punteros o estados intermedios.

```javascript
// Ejemplo en JavaScript
// Se define la lambda y se asigna a la variable local aMayusculas
const aMayusculas = (s) => s.toUpperCase();

const cadena = "hola mundo";
console.log(aMayusculas(cadena)); // Imprime: HOLA MUNDO
```
****


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El **paradigma funcional** es un modelo de programación centrado en la evaluación de funciones matemáticas y en evitar el cambio de estado y los datos mutables. A diferencia del paradigma imperativo (como el C estándar), donde se dan instrucciones paso a paso para modificar la memoria, en el funcional se construyen programas mediante la composición de funciones puras. Estas funciones se caracterizan por el determinismo: para una misma entrada, siempre producen la misma salida, sin generar efectos secundarios en el sistema.

Se dice que lenguajes como Java 8 en adelante son **multi-paradigma** porque permiten combinar la Programación Orientada a Objetos (POO) con conceptos funcionales. Java nació como un lenguaje estrictamente orientado a objetos, donde todo debía residir dentro de una clase. Sin embargo, con la introducción de las lambdas y las interfaces funcionales, el lenguaje adoptó capacidades funcionales, permitiendo a los desarrolladores elegir la herramienta más adecuada según el problema, integrando la estructura de clases con la agilidad de las funciones.

El concepto de que las funciones son **"ciudadanos de primera clase"** (*first-class citizens*) significa que las funciones son tratadas como cualquier otra variable u objeto del lenguaje. Esto implica que una función puede:
* Ser asignada a una variable local.
* Ser pasada como argumento a otra función (conocidas como funciones de orden superior).
* Ser devuelta como resultado de la ejecución de otra función.
* Ser creada dinámicamente en tiempo de ejecución.

En C, los punteros a funciones proporcionan una funcionalidad similar, pero en lenguajes como JavaScript o Java (mediante interfaces funcionales), esta integración es nativa y mucho más natural dentro de la sintaxis del lenguaje, permitiendo tratar el "comportamiento" con la misma flexibilidad que tratamos a los "datos".


## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis de una función lambda en Java es una expresión compacta que permite implementar el método único de una interfaz funcional. Se compone de tres partes fundamentales: los **parámetros**, el **operador de flecha** (`->`) y el **cuerpo** de la función. Esta estructura elimina la necesidad de declarar explícitamente el nombre del método, el tipo de retorno y los modificadores de acceso, basándose en el contexto para inferir dicha información.

Existen variaciones según la complejidad de la lógica:
* **Parámetros**: Si solo hay un parámetro y el tipo es inferible, se pueden omitir los paréntesis (ej. `s -> ...`). Si no hay parámetros o hay varios, los paréntesis son obligatorios (ej. `() -> ...` o `(a, b) -> ...`).
* **Cuerpo**: Si el cuerpo consiste en una sola expresión, no se requieren llaves `{}` ni la palabra clave `return`, ya que el resultado de la expresión se devuelve automáticamente. Si la lógica requiere varias líneas, se deben usar llaves y especificar el `return` explícitamente.

Un aspecto clave para un programador de C es el concepto de **inferencia de tipos**. Mientras que en C un puntero a función requiere definir estrictamente los tipos en la firma, el compilador de Java analiza la interfaz funcional a la que se asigna la lambda para deducir qué tipos tienen los parámetros. Esto permite que la sintaxis sea extremadamente limpia sin perder la seguridad de tipos estática característica de Java.

```java
// Sintaxis mínima: un parámetro, una expresión
Function<Integer, Integer> cuadrado = x -> x * x;

// Sintaxis completa: múltiples parámetros y cuerpo complejo
BiFunction<Integer, Integer, Integer> sumaExplicita = (a, b) -> {
    int resultado = a + b;
    return resultado;
};

// Sintaxis sin parámetros
Runnable saludo = () -> System.out.println("Hola");
```
****


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

El paso de funciones como parámetros es la base de las **funciones de orden superior**. En este modelo, el método receptor no conoce la lógica específica que se va a aplicar, sino que delega esa responsabilidad en la función que le llega por parámetro. Para un programador de C, esto equivale exactamente a pasar un puntero a función a otro método para que este lo ejecute mediante una retrollamada o *callback*.

En JavaScript, al ser las funciones ciudadanos de primera clase, el proceso es directo y no requiere de interfaces adicionales. En cambio, en Java se debe especificar una **interfaz funcional** (como `Function<T, R>`) que actúe como el "tipo" del parámetro. Esto permite que el método `transformar` sea genérico y reutilizable con cualquier lógica que respete la firma de entrada y salida definida, ya sea para convertir a mayúsculas, limpiar espacios o cifrar el texto.


```javascript
// Ejemplo en JavaScript
const transformar = (texto, funcionTransformadora) => {
    return funcionTransformadora(texto);
};

const aMayusculas = (s) => s.toUpperCase();

// Se pasa la función lambda como segundo argumento
console.log(transformar("hola mundo", aMayusculas));
```
****



## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

La capacidad de definir una función lambda directamente en la llamada de un método es una de las características más potentes del paradigma funcional, ya que permite inyectar comportamiento específico de forma "ad hoc" sin contaminar el espacio de nombres global con funciones de un solo uso. Para un programador acostumbrado a C, esto es equivalente a pasar una dirección de memoria de una función, pero con la ventaja de que el cuerpo de la función se escribe en la misma línea de código donde se necesita.

Este enfoque reduce drásticamente la verbosidad y mejora la cohesión del código, pues la lógica de transformación (en este caso, invertir una cadena) reside exactamente donde se utiliza. En Java, el compilador utiliza el contexto del método `transformar` para saber que la lambda debe cumplir con el contrato de la interfaz `Function<String, String>`, encargándose de la gestión interna de los objetos necesarios para que la operación sea segura.

```javascript
// Ejemplo en JavaScript
// Definimos la lógica de inversión directamente en la llamada
const resultadoJS = transformar("hola mundo", (s) => s.split('').reverse().join(''));

console.log(resultadoJS); // Imprime: odnum aloh
```
****


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un **cierre o "closure"** es la capacidad de una función lambda de "capturar" y recordar el entorno léxico en el que fue creada. Esto significa que la lambda puede acceder a variables definidas en su ámbito externo, incluso si la ejecución de la lambda ocurre en un momento o lugar distinto a donde dichas variables fueron declaradas. En esencia, la función no solo transporta su lógica, sino también un paquete con las referencias a los datos que necesita de su contexto original.

En Java, existe una restricción importante: las variables locales capturadas por una lambda deben ser **finales o efectivamente finales** (*effectively final*). Esto implica que, una vez asignado un valor a la variable fuera de la lambda, este no puede ser modificado posteriormente. El compilador de Java impone esta regla para evitar problemas de consistencia y concurrencia, asegurando que la lambda siempre trabaje con un valor predecible y no con un estado que podría cambiar de forma inesperada.

Para un programador de C, esto es un salto conceptual relevante. En C, si una función intentara acceder a una variable local de otra función que ya ha salido de la pila (*stack*), se produciría un error de acceso a memoria o se leería basura. En Java, el *runtime* se encarga de gestionar el ciclo de vida de estas variables capturadas para que sigan estando disponibles mientras la lambda exista, gestionando la memoria de forma segura.


```java
import java.util.function.Function;

public class EjemploClosure {
    
    public static String transformar(String texto, Function<String, String> transformadora) {
        return transformadora.apply(texto);
    }

    public static void main(String[] args) {
        // Variable local en el contexto externo
        String sufijo = " - Editado por Lambda"; 

        // La lambda captura la variable 'sufijo' de su entorno (Closure)
        String resultado = transformar("Archivo1", s -> {
            // Acceso a la variable local externa
            return s + sufijo; 
        });

        // Si intentáramos modificar 'sufijo' aquí, el código no compilaría
        // sufijo = "otro valor"; 
        
        System.out.println(resultado); // Imprime: Archivo1 - Editado por Lambda
    }
}
```
****



## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

La diferencia fundamental reside en el **nivel de abstracción** y la gestión del entorno. Un puntero a función en C es una simple dirección de memoria que apunta a un bloque de código estático y global; no posee estado propio ni conocimiento del contexto donde fue referenciado. En cambio, una función lambda en Java es un objeto (una instancia de una interfaz funcional) que combina código con un entorno de datos capturados.

Esta distinción da lugar al concepto de **clausura o closure**, inexistente en los punteros de C estándar. Mientras que un puntero en C solo puede acceder a sus parámetros y a variables globales, una lambda puede "llevarse consigo" variables locales de su ámbito externo (siempre que sean finales). En C, intentar emular esto requeriría pasar manualmente estructuras de datos adicionales (*void pointers* a contextos) para que la función tuviera acceso a datos locales de otras funciones.


Otra diferencia clave es la **seguridad y el ciclo de vida**. En C, si un puntero a función apunta a una dirección de memoria que ha sido liberada o modificada, el programa sufrirá un error de segmentación o un comportamiento indefinido. En Java, la Máquina Virtual asegura que la lambda y las variables que captura permanezcan en memoria el tiempo necesario. Además, la sintaxis de las lambdas permite la inferencia de tipos, evitando la compleja y frágil declaración de firmas de punteros (`void (*f)(int)`) que suele ser fuente de errores en C.


| Característica | Puntero a Función (C) | Función Lambda (Java) |
| :--- | :--- | :--- |
| **Naturaleza** | Dirección de memoria (primitivo) | Objeto (instancia de interfaz) |
| **Estado** | Sin estado (solo código) | Con estado (captura variables) |
| **Contexto** | Solo variables globales/parámetros | Ámbito léxico (Closures) |
| **Sintaxis** | Rígida y compleja | Concisa con inferencia de tipos |
| **Seguridad** | Riesgo de punteros colgantes | Gestión automática por la JVM |


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

La capacidad de una función para devolver otra función es uno de los pilares de la programación funcional y permite la creación de **fábricas de comportamiento**. En este escenario, el método `crearDescuento` no devuelve un valor calculado, sino una lógica personalizada que será ejecutada en el futuro. Para un programador de C, esto sería equivalente a una función que genera y devuelve un puntero a función configurado dinámicamente, algo que en C es extremadamente complejo de implementar sin recurrir a memoria dinámica y estructuras de contexto manuales.

La **clausura (closure)** en este ejemplo es fundamental: la función lambda que se devuelve "atrapa" el valor del parámetro `porcentaje`. Aunque el método `crearDescuento` finalice su ejecución y salga de la pila, la función descuento generada mantiene una copia interna de ese porcentaje. Esto permite que, al invocar la función más tarde sobre un precio, esta sepa exactamente qué reducción debe aplicar, demostrando que la lambda es un paquete que contiene tanto el algoritmo como los datos necesarios para su ejecución.


En Java, el uso de la interfaz `Function<Double, Double>` define un contrato claro: la función resultante recibirá un `Double` (el precio original) y devolverá otro `Double` (el precio con descuento). La seguridad de tipos asegura que no podamos aplicar un descuento a un objeto que no sea numérico, mientras que la flexibilidad de las lambdas permite que cada instancia de "función descuento" sea independiente y mantenga su propio estado de configuración.


```java
import java.util.function.Function;

public class FabricaDescuentos {

    // Método que devuelve una función (Fábrica de funciones)
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // La lambda captura 'porcentaje' (Closure)
        // Recibe el precio (p) y aplica la lógica
        return p -> p - (p * porcentaje / 100);
    }

    public static void main(String[] args) {
        // Creamos dos funciones con comportamientos distintos
        Function<Double, Double> descuentoIVA = crearDescuento(21.0);
        Function<Double, Double> descuentoBlackFriday = crearDescuento(50.0);

        double precioOriginal = 100.0;

        // Aplicamos las funciones creadas
        System.out.println("Precio con IVA: " + descuentoIVA.apply(precioOriginal)); 
        // Imprime: 79.0 (decremento del 21%)
        
        System.out.println("Precio Black Friday: " + descuentoBlackFriday.apply(precioOriginal)); 
        // Imprime: 50.0 (decremento del 50%)
    }
}
```
****


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una **interfaz funcional** es el mecanismo que utiliza Java para integrar las funciones lambda dentro de su sistema de tipos estático. Dado que en Java todo debe ser un objeto, una lambda no es simplemente un bloque de código "suelto", sino que se trata como una implementación de una interfaz específica. Esto permite que el compilador sepa exactamente qué parámetros recibe la lambda y qué tipo de dato debe devolver, manteniendo la robustez del tipado fuerte que caracteriza al lenguaje.

Para que una interfaz sea considerada funcional, debe cumplir un requisito indispensable: poseer **exactamente un método abstracto**. Este método define la "firma" de la función (su contrato). No obstante, la interfaz puede contener cualquier número de métodos predeterminados (*default*) o estáticos, ya que estos poseen una implementación y no rompen la condición de "única operación" que la lambda debe cumplir.


Aunque no es estrictamente obligatorio, se recomienda encarecidamente utilizar la anotación `@FunctionalInterface`. Su función es puramente informativa y de validación: si un desarrollador intenta añadir un segundo método abstracto por error a una interfaz marcada con esta anotación, el compilador generará un error inmediatamente. Esto evita que cambios accidentales en la API rompan la compatibilidad con las funciones lambda que ya la estén utilizando.

Para un programador de C, una interfaz funcional equivale a la **definición del tipo del puntero a función**. Mientras que en C usas un `typedef` para definir la firma de una función, en Java defines una interfaz con un único método. La principal ventaja es que estas interfaces pueden ser genéricas, lo que permite reutilizar la misma estructura para diferentes tipos de datos, algo mucho más complejo de lograr con la sintaxis rígida de los punteros en C.


```java
@FunctionalInterface
public interface Transformador {
    // El único método abstracto: define el contrato
    String ejecutar(String entrada);

    // Puede tener métodos default sin dejar de ser funcional
    default void log(String msj) {
        System.out.println("LOG: " + msj);
    }
}
```
****


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Para crear una **interfaz funcional** personalizada, se debe definir una interfaz que contenga un único método abstracto. En este caso, la interfaz `Transformador` actuará como el molde para cualquier función lambda que acepte un `String` como entrada y produzca otro `String` como salida. Aunque Java ya proporciona `Function<String, String>` en su biblioteca estándar, definir una interfaz propia permite dar una semántica más clara al dominio del problema y añadir métodos auxiliares si fuera necesario.

El uso de la anotación `@FunctionalInterface` es una buena práctica que actúa como un seguro de vida para el código. Al incluirla, se le indica explícitamente al compilador y a otros programadores que esta interfaz está diseñada para ser utilizada con lambdas. Si en el futuro alguien intentara añadir un segundo método abstracto, el proyecto no compilaría, protegiendo así todas las llamadas que ya dependan de la estructura funcional de `Transformador`.


Desde la perspectiva de un programador de C, este proceso es el equivalente a definir un contrato de comportamiento. Mientras que en C te asegurarías de que la firma del puntero coincida, en Java te aseguras de que el objeto lambda implemente el método `transformar`. La ventaja es que, al ser una interfaz, este `Transformador` puede ser utilizado en cualquier lugar del código donde se requiera una manipulación de texto, proporcionando un tipado mucho más descriptivo que un simple tipo genérico.

```java
// Definición manual de la interfaz funcional
@FunctionalInterface
public interface Transformador {
    // El único método abstracto que define la firma de la lambda
    String transformar(String s);
}

public class UsoInterfazManual {
    
    // El método ahora usa nuestro tipo específico en lugar de Function<String, String>
    public static String ejecutarOperacion(String texto, Transformador operacion) {
        return operacion.transformar(texto);
    }

    public static void main(String[] args) {
        // Implementación de la interfaz mediante una lambda
        Transformador aMayusculas = s -> s.toUpperCase();
        
        String resultado = ejecutarOperacion("hola mundo", aMayusculas);
        System.out.println(resultado); // Imprime: HOLA MUNDO
    }
}
```
****


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

Para dotar a nuestra interfaz de la máxima flexibilidad, debemos recurrir a los **generics**. Al definir `Transformador<T, R>`, estamos creando una estructura abstracta donde `T` representa el tipo de entrada y `R` el tipo de resultado. Esta aproximación es idéntica a la filosofía de las plantillas (*templates*) en C++, permitiendo que una misma interfaz funcional sirva para cualquier transformación imaginable, ya sea de texto a número, de objeto a booleano o, como en este caso, de un valor decimal a uno entero.

La principal ventaja de usar tipos genéricos en una interfaz funcional es la **reutilización de código con seguridad de tipos**. El compilador de Java se encarga de verificar que la lógica de la lambda sea coherente con los tipos declarados. Si definimos un transformador que recibe un `Double` y devuelve un `Integer`, el compilador impedirá que intentemos devolver una cadena de texto, capturando posibles errores de lógica durante la fase de desarrollo y no en tiempo de ejecución.


En el ejemplo del redondeo, la lambda recibe un objeto de tipo `Double` y utiliza métodos de la clase `Math` para procesarlo. Es importante notar que, debido a que los genéricos en Java trabajan con objetos, se produce un proceso de **autoboxing** y **unboxing** (conversión automática entre tipos primitivos como `double` y sus clases envolventes como `Double`), lo que permite escribir una sintaxis limpia y fluida mientras se mantiene la estructura de clases necesaria.


```java
// Interfaz funcional genérica: T es el tipo de entrada, R el de salida
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}

public class EjemploGenericsFuncional {
    public static void main(String[] args) {
        // Definimos un transformador de Double a Integer
        // Usamos la lógica de redondeo de Math.round
        Transformador<Double, Integer> redondear = d -> (int) Math.round(d);

        Double valorOriginal = 7.6;
        Integer resultado = redondear.transformar(valorOriginal);

        System.out.println("Valor original: " + valorOriginal); // 7.6
        System.out.println("Valor redondeado: " + resultado);   // 8
        
        // La misma interfaz sirve para otros tipos sin cambiar el código
        Transformador<String, Integer> contarLetras = s -> s.length();
        System.out.println("Longitud: " + contarLetras.transformar("Java")); // 4
    }
}
```
****


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

Java proporciona en el paquete `java.util.function` un conjunto de interfaces funcionales estándar diseñadas para cubrir la inmensa mayoría de los casos de uso comunes. Estas interfaces se clasifican principalmente según el número de parámetros que reciben y si devuelven o no un resultado. Al utilizar estas interfaces predefinidas en lugar de crear las propias (como `Transformador`), se mejora la interoperabilidad del código, ya que la mayoría de las librerías de Java y el propio API de Streams las utilizan de forma nativa.

Las cuatro interfaces fundamentales sobre las que se construye el resto son:
* **`Function<T, R>`**: Representa una función que acepta un argumento de tipo `T` y produce un resultado de tipo `R`. Es la base para transformaciones de datos.
* **`Predicate<T>`**: Recibe un argumento de tipo `T` y devuelve un valor booleano. Se utiliza típicamente para filtros o validaciones de condiciones.
* **`Consumer<T>`**: Acepta un argumento de tipo `T` pero no devuelve ningún resultado (su retorno es `void`). Suele usarse para realizar acciones laterales como imprimir o guardar datos.
* **`Supplier<T>`**: No recibe ningún parámetro y devuelve un resultado de tipo `T`. Es útil para la generación de datos o la inicialización perezosa (*lazy initialization*).


Además de estas, existen variantes especializadas para trabajar con tipos primitivos (como `IntPredicate` o `DoubleFunction`) que evitan el coste de rendimiento del *autoboxing*. También existen versiones para dos parámetros, como `BiFunction<T, U, R>`, `BiPredicate<T, U>` y `BiConsumer<T, U>`. Para un programador con base en C, esto simplifica enormemente la gestión de tipos, eliminando la necesidad de definir estructuras complejas o punteros genéricos `void*` cada vez que se quiere pasar una lógica diferente a un método.


| Interfaz Funcional | Método Abstracto | Descripción |
| :--- | :--- | :--- |
| `Function<T, R>` | `R apply(T t)` | Transforma una entrada en una salida. |
| `Predicate<T>` | `boolean test(T t)` | Evalúa una condición sobre la entrada. |
| `Consumer<T>` | `void accept(T t)` | Procesa una entrada sin devolver nada. |
| `Supplier<T>` | `T get()` | Proporciona un valor sin recibir entrada. |
| `BiFunction<T, U, R>` | `R apply(T t, U u)` | Transforma dos entradas en una salida. |
| `UnaryOperator<T>` | `T apply(T t)` | Función donde entrada y salida son del mismo tipo. |

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método `forEach` es una operación de orden superior que permite iterar sobre una colección de manera declarativa, eliminando la necesidad de gestionar manualmente los índices o los iteradores del bucle `for` tradicional. En lugar de explicarle al programa **cómo** debe recorrer la lista (instrucción por instrucción), simplemente se le indica **qué** acción debe realizar sobre cada elemento. Internamente, `forEach` recibe un parámetro de tipo `Consumer<T>`, cuya firma (`void accept(T t)`) encaja perfectamente con una lambda que procesa el dato sin devolver nada.

Para un programador acostumbrado a los bucles `for` de C, este cambio supone delegar la responsabilidad del flujo de control al lenguaje. Mientras que en C un error en el límite del array o en el incremento del puntero puede provocar fallos de memoria, en Java el `forEach` garantiza que se visitarán todos los elementos de la lista de forma segura. Además, al usar lambdas dentro de este método, el código resulta mucho más legible y compacto, concentrando la lógica de negocio en una sola línea.

En el ejemplo solicitado, se utiliza un condicional dentro de la lambda para filtrar visualmente los números. Es importante notar que, aunque estemos dentro de una estructura funcional, todavía podemos ejecutar lógica imperativa (como un `if`) dentro del cuerpo de la lambda. No obstante, este es el primer paso para entender que en Java moderno, las colecciones no son solo contenedores de datos, sino entidades capaces de procesar su propio contenido mediante comportamientos inyectados.


```java
import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        // Creamos una lista de enteros (positivos y negativos)
        List<Integer> numeros = Arrays.asList(5, -2, 10, 0, -8, 3);

        System.out.println("Números positivos encontrados:");

        // Usamos forEach con una lambda de tipo Consumer<Integer>
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("El número " + n + " es positivo.");
            }
        });
    }
}
```
****

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

El uso de `Consumer<? super T>` en lugar de un simple `Consumer<T>` es una aplicación del principio de **contravarianza**. En Java, los genéricos son invariantes (un `Consumer<Number>` no es un `Consumer<Integer>`), lo que limitaría la flexibilidad. Al usar `? super T`, permitimos que el método acepte un consumidor capaz de manejar no solo el tipo exacto `T`, sino también cualquier superclase de este. Esto tiene sentido lógico: si tienes una lista de `Integer`, cualquier función que sepa procesar un `Number` o un `Object` será perfectamente capaz de procesar un `Integer`.

El acrónimo **PECS** significa **Producer Extends, Consumer Super**. Es una regla mnemotécnica para recordar cómo usar los comodines (*wildcards*):
* **Producer Extends**: Si la estructura de datos va a "producir" elementos (vas a leer de ella), usa `? extends T`. Esto permite leer elementos de `T` o cualquier subclase.
* **Consumer Super**: Si la estructura va a "consumir" elementos (vas a escribir en ella o pasarle datos), usa `? super T`. Esto permite que la estructura maneje objetos de tipo `T`.

En el contexto de nuestro método `transformar`, la función transformadora actúa como un **consumidor** del dato de entrada y un **productor** del resultado. Para maximizar la reutilización, la entrada debería aceptar superclases de `T` y la salida debería permitir subclases de `R`. Si tenemos un método que transforma un `String` en un `Number`, debería ser capaz de aceptar una función que trabaje con `Object` (superclase de String) y devuelva un `Integer` (subclase de Number).

Al aplicar PECS al método `transformar`, la firma se vuelve mucho más potente. Un programador de C apreciará esto como una forma de polimorfismo paramétrico avanzado que el compilador gestiona automáticamente, asegurando que los tipos "encajen" jerárquicamente. Sin esta flexibilidad, estaríamos obligando al usuario a proporcionar funciones con tipos exactos, lo cual resulta frustrante y menos natural en el diseño de APIs orientadas a objetos.

```java
// Aplicando PECS: 
// 1. El argumento de entrada de la función es CONSUMIDO (? super T)
// 2. El resultado de la función es PRODUCIDO (? extends R)
public static <T, R> R transformar(T valor, Function<? super T, ? extends R> funcion) {
    return funcion.apply(valor);
}

public static void main(String[] args) {
    String texto = "Hola";
    
    // Una función que acepta Object (super de String) 
    // y devuelve Integer (sub de Number) funciona gracias a PECS
    Function<Object, Integer> f = obj -> obj.hashCode();
    
    Number resultado = transformar(texto, f);
    System.out.println(resultado);
}
```
****

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las **referencias a métodos** son una forma aún más compacta de expresar una lambda cuando esta solo se limita a invocar un método que ya existe. En lugar de escribir la estructura completa de la lambda `(parámetros) -> objeto.metodo(parámetros)`, simplemente referenciamos el método por su nombre. Para un programador de C, esto es lo más cercano a tomar la dirección de una función específica o un método de instancia y guardarlo en un puntero para su ejecución diferida.

En JavaScript, esto se logra simplemente accediendo a la propiedad del objeto que contiene la función, aunque suele ser necesario usar `.bind(objeto)` para asegurar que la palabra clave `this` siga apuntando a la instancia correcta cuando la función se ejecute fuera de su contexto original. En Java, se utiliza el operador de doble dos puntos (`::`), el cual permite al compilador crear una instancia de la interfaz funcional necesaria apuntando directamente al método de la clase o del objeto.

Existen diferentes tipos de referencias a métodos en Java (estáticos, de instancia de un objeto particular, de instancia de un objeto arbitrario o constructores). En el caso de una referencia a un método de un objeto ya existente, la sintaxis es `objeto::nombreMetodo`. Esto resulta extremadamente útil para limpiar el código y hacerlo más legible, ya que elimina el "ruido" visual de los parámetros de la lambda cuando estos simplemente se pasan tal cual al método interno.


```javascript
// Ejemplo en JavaScript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log(`Hola, soy ${this.nombre}`);
    }
}

const p = new Persona("Carlos");

// Obtenemos la referencia. Usamos bind para mantener el contexto de 'p'
const saludarReferencia = p.saludar.bind(p);

// Invocación a través de la referencia
saludarReferencia(); // Imprime: Hola, soy Carlos
```
****


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

En Java, las referencias a métodos proporcionan una forma de delegar la implementación de una interfaz funcional a un método ya existente, permitiendo un código aún más limpio que las expresiones lambda. El compilador se encarga de verificar que la firma del método referenciado coincida con el contrato del método abstracto de la interfaz funcional. Para un programador de C, estas referencias se comportan de forma similar a los punteros a funciones, pero con la capacidad añadida de manejar el contexto del objeto (*this*).

Existen cuatro tipos principales de referencias a métodos, cada uno diseñado para un escenario de uso específico:
1. **Método estático**: Se refiere a un método de clase.
2. **Método de instancia de un objeto particular**: Vincula la función a una instancia específica ya existente.
3. **Método de instancia de un objeto arbitrario de un tipo determinado**: El primer argumento de la interfaz funcional se convierte en el receptor del método.
4. **Referencia a constructor**: Utiliza la palabra clave `new` para crear nuevas instancias a través de una interfaz funcional (como una factoría).

Desde el punto de vista de la eficiencia, las referencias a métodos son idénticas a las lambdas, ya que el compilador las trata internamente de la misma manera. Sin embargo, su uso es preferible cuando la lambda simplemente "pasa" los argumentos a un método existente, ya que la intención del código se vuelve mucho más clara y descriptiva al usar el nombre del método directamente.

```java
import java.util.function.*;
import java.util.ArrayList;
import java.util.List;

public class TiposReferencias {
    public static void main(String[] args) {
        
        // 1. Referencia a método ESTÁTICO (Clase::metodo)
        // Equivale a: (s) -> Integer.parseInt(s)
        Function<String, Integer> conversor = Integer::parseInt;
        Integer numero = conversor.apply("123");

        // 2. Referencia a método de INSTANCIA CONCRETA (objeto::metodo)
        // Equivale a: (s) -> System.out.println(s)
        Consumer<String> impresor = System.out;::println;
        impresor.accept("Hola Mundo");

        // 3. Referencia a método de instancia de OBJETO ARBITRARIO (Clase::metodo)
        // El primer String es el objeto que llama a .length()
        // Equivale a: (s) -> s.length()
        Function<String, Integer> medidor = String::length;
        int longi = medidor.apply("Java");

        // 4. Referencia a CONSTRUCTOR (Clase::new)
        // Equivale a: () -> new ArrayList<String>()
        Supplier<List<String>> fabricaListas = ArrayList::new;
        List<String> lista = fabricaListas.get();
    }
}
```
****


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.
Ordenar colecciones es uno de los casos de uso más comunes donde la programación funcional brilla por su expresividad. Para un programador de C, ordenar una lista mediante una lambda es conceptualmente idéntico a usar `qsort` y pasarle un puntero a una función de comparación. La diferencia es que en Java, el `Comparator` es una interfaz funcional que permite una sintaxis mucho más fluida, evitando la aritmética de punteros y los *casts* manuales de `void*`.

El método `Collections.sort` (o el más moderno `List.sort`) espera un `Comparator<T>`, cuyo único método abstracto es `compare(T o1, T o2)`. Este método debe devolver un entero: negativo si el primero es menor, positivo si es mayor, y cero si son iguales. Al usar lambdas, podemos definir esta lógica de decisión en el mismo lugar donde se invoca la ordenación.



### 1. Implementación manual con Lambda
En esta versión, escribimos nosotros mismos la lógica de decisión dentro del cuerpo de la lambda. Es la forma más directa de traducir un algoritmo de comparación tradicional a una expresión funcional, permitiendo manejar múltiples criterios mediante estructuras condicionales anidadas.

```java
import java.util.*;

class Persona {
    String nombre;
    int edad;

    Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    @Override
    public String toString() { return nombre + " (" + edad + ")"; }
}

public class OrdenarManual {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Ana", 25),
            new Persona("Zoe", 20),
            new Persona("Beto", 25)
        );

        // Versión manual: lógica explícita dentro de la lambda
        // Comparamos edad y, si es igual, comparamos nombre
        Collections.sort(personas, (p1, p2) -> {
            if (p1.edad != p2.edad) {
                return Integer.compare(p1.edad, p2.edad);
            }
            return p1.nombre.compareTo(p2.nombre);
        });

        System.out.println("Orden manual: " + personas);
    }
}
```
****