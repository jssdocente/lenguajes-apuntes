# JAVA I/O y New IO

### Entrada / Salida Básica
Esta lección cubre las clases de la plataforma Java utilizadas para E/S básica. Primero se enfoca en los _flujos de E/S_ , un concepto poderoso que simplifica enormemente las operaciones de E/S. La lección también analiza la serialización, que permite a un programa escribir objetos completos en secuencias y volver a leerlos. Luego, la lección analiza la E/S de archivos y las operaciones del sistema de archivos, incluidos los archivos de acceso aleatorio.

La mayoría de las clases cubiertas en la `I/O Streams`sección están en el `java.io`paquete. La mayoría de las clases cubiertas en la `File I/O`sección están en el `java.nio.file`paquete.

## [Flujos de E/S](https://docs.oracle.com/javase/tutorial/essential/io/streams.html)

-   [Los flujos de bytes](https://docs.oracle.com/javase/tutorial/essential/io/bytestreams.html) manejan la E/S de datos binarios sin procesar.
-   [Los flujos de caracteres](https://docs.oracle.com/javase/tutorial/essential/io/charstreams.html) manejan la E/S de datos de caracteres, manejando automáticamente la traducción hacia y desde el juego de caracteres local.
-   [Las secuencias almacenadas en búfer](https://docs.oracle.com/javase/tutorial/essential/io/buffers.html) optimizan la entrada y la salida al reducir la cantidad de llamadas a la API nativa.
-   [Escanear y formatear](https://docs.oracle.com/javase/tutorial/essential/io/scanfor.html) permite que un programa lea y escriba texto formateado.
-   [E/S desde la línea de comandos](https://docs.oracle.com/javase/tutorial/essential/io/cl.html) describe las secuencias estándar y el objeto de la consola.
-   [Los flujos de datos](https://docs.oracle.com/javase/tutorial/essential/io/datastreams.html) manejan E/S binarias de tipos y `String`valores de datos primitivos .
-   [Los flujos de objetos](https://docs.oracle.com/javase/tutorial/essential/io/objectstreams.html) manejan la E/S binaria de objetos.

## [E/S de archivos (con NIO.2)](https://docs.oracle.com/javase/tutorial/essential/io/fileio.html)

-   [¿Qué es un camino?](https://docs.oracle.com/javase/tutorial/essential/io/path.html) examina el concepto de ruta en un sistema de archivos.
-   [La clase Path](https://docs.oracle.com/javase/tutorial/essential/io/pathClass.html) presenta la clase de piedra angular del `java.nio.file`paquete.
-   [Path Operations](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html) analiza los métodos de la `Path`clase que se ocupan de las operaciones sintácticas.
-   [Operaciones de archivos](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html) presenta conceptos comunes a muchos de los métodos de E/S de archivos.
-   [Verificar un archivo o directorio](https://docs.oracle.com/javase/tutorial/essential/io/check.html) muestra cómo verificar la existencia de un archivo y su nivel de accesibilidad.
-   [Eliminar un archivo o directorio](https://docs.oracle.com/javase/tutorial/essential/io/delete.html) .
-   [Copia de un archivo o directorio](https://docs.oracle.com/javase/tutorial/essential/io/copy.html) .
-   [Mover un archivo o directorio](https://docs.oracle.com/javase/tutorial/essential/io/move.html) .
-   [Gestión de metadatos](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html) explica cómo leer y establecer atributos de archivo.
-   [Leer, escribir y crear archivos](https://docs.oracle.com/javase/tutorial/essential/io/file.html) muestra los métodos de flujo y canal para leer y escribir archivos.
-   [Archivos de acceso aleatorio](https://docs.oracle.com/javase/tutorial/essential/io/rafs.html) muestra cómo leer o escribir archivos de manera no secuencial.
-   [La creación y lectura de directorios](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html) cubre la API específica de los directorios, por ejemplo, cómo enumerar el contenido de un directorio.
-   [Enlaces, simbólicos o de otro tipo](https://docs.oracle.com/javase/tutorial/essential/io/links.html) cubre temas específicos de enlaces físicos y simbólicos.
-   [Caminar por el árbol de archivos](https://docs.oracle.com/javase/tutorial/essential/io/walk.html) demuestra cómo visitar de forma recursiva cada archivo y directorio en un árbol de archivos.
-   [Encontrar archivos](https://docs.oracle.com/javase/tutorial/essential/io/find.html) muestra cómo buscar archivos mediante la coincidencia de patrones.
-   [Ver cambios en un directorio](https://docs.oracle.com/javase/tutorial/essential/io/notification.html) muestra cómo utilizar el servicio de vigilancia para detectar archivos que se agregan, eliminan o actualizan en uno o más directorios.
-   [Otros métodos útiles](https://docs.oracle.com/javase/tutorial/essential/io/misc.html) cubren API importantes que no encajan en ningún otro lugar de la lección.
-   [El código de E/S de archivo heredado](https://docs.oracle.com/javase/tutorial/essential/io/legacy.html) muestra cómo aprovechar la `Path`funcionalidad si tiene un código anterior utilizando la `java.io.File`clase. Se proporciona una `java.io.File`API de mapeo de tablas a `java.nio.file`API.


# Flujos de E/S
## Secuencias de bytes

Los programas utilizan _flujos de bytes_ para realizar la entrada y salida de bytes de 8 bits. Todas las clases de flujo de bytes descienden de [`InputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/InputStream.html)y [`OutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/OutputStream.html).

Hay muchas clases de flujo de bytes. Para demostrar cómo funcionan las secuencias de bytes, nos centraremos en las secuencias de bytes de E/S de archivos [`FileInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/FileInputStream.html)y [`FileOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/FileOutputStream.html). Otros tipos de flujos de bytes se utilizan de la misma forma; difieren principalmente en la forma en que están construidos.

#### Uso de secuencias de bytes

Exploraremos `FileInputStream`y `FileOutputStream`examinaremos un programa de ejemplo llamado [`CopyBytes`](https://docs.oracle.com/javase/tutorial/essential/io/examples/CopyBytes.java), que usa flujos de bytes para copiar `xanadu.txt`, un byte a la vez.

    import java.io.FileInputStream; 
    import java.io.FileOutputStream; 
    import java.io.IOException; 
    
    public class CopyBytes { 
        public static void main (String [] args) lanza IOException { 
    
            FileInputStream in = null; 
            FileOutputStream out = null; 
    
            try { 
                in = new FileInputStream ("xanadu.txt"); 
                out = new FileOutputStream ("outagain.txt"); 
                int c; 
    
                while ((c = in.read ())! = -1) { 
                    out.write (c); 
                } 
            } finalmente { 
                if (in! = null) { 
                    in.close (); 
                } 
                if (out! = null) {
                    out.close (); 
                } 
            } 
        } 
    }

`CopyBytes`pasa la mayor parte de su tiempo en un bucle simple que lee el flujo de entrada y escribe el flujo de salida, un byte a la vez, como se muestra en la siguiente figura .

![Entrada y salida de flujo de bytes simple.](https://docs.oracle.com/javase/tutorial/figures/essential/byteStream.gif)

Entrada y salida de flujo de bytes simple.

#### Cerrar siempre las corrientes

Cerrar una secuencia cuando ya no se necesita es muy importante, tan importante que `CopyBytes`usa un `finally`bloque para garantizar que ambas secuencias se cerrarán incluso si ocurre un error. Esta práctica ayuda a evitar fugas graves de recursos.

Un posible error es que `CopyBytes`no pudo abrir uno o ambos archivos. Cuando eso sucede, la variable de flujo correspondiente al archivo nunca cambia de su `null`valor inicial . Es por eso `CopyBytes`que se asegura de que cada variable de flujo contenga una referencia de objeto antes de invocar `close`.

#### Cuándo no utilizar secuencias de bytes

`CopyBytes`parece un programa normal, pero en realidad representa una especie de E/S de bajo nivel que debe evitar. Dado que `xanadu.txt`contiene datos de caracteres, el mejor enfoque es utilizar [secuencias de caracteres](https://docs.oracle.com/javase/tutorial/essential/io/charstreams.html) , como se explica en la siguiente sección. También hay flujos para tipos de datos más complicados. Los flujos de bytes solo deben usarse para las E/S más primitivas.

Entonces, ¿por qué hablar de flujos de bytes? Porque todos los demás tipos de flujos se basan en flujos de bytes.

# Flujos de caracteres

La plataforma Java almacena valores de caracteres mediante convenciones Unicode. La E/S de flujo de caracteres traduce automáticamente este formato interno hacia y desde el juego de caracteres local. En las configuraciones regionales occidentales, el juego de caracteres local suele ser un superconjunto de 8 bits de ASCII.

Para la mayoría de las aplicaciones, la E/S con flujos de caracteres no es más complicada que la E/S con flujos de bytes. La entrada y salida realizada con clases de flujo se traduce automáticamente hacia y desde el juego de caracteres local. Un programa que utiliza flujos de caracteres en lugar de flujos de bytes se adapta automáticamente al conjunto de caracteres local y está listo para la internacionalización, todo sin esfuerzo adicional por parte del programador.

Si la internacionalización no es una prioridad, simplemente puede usar las clases de flujo de caracteres sin prestar mucha atención a los problemas del juego de caracteres. Más adelante, si la internacionalización se convierte en una prioridad, su programa se puede adaptar sin necesidad de una recodificación extensa. Consulte la Ruta de [internacionalización](https://docs.oracle.com/javase/tutorial/i18n/index.html) para obtener más información.

## Uso de flujos de caracteres

Todas las clases de flujo de caracteres descienden de [`Reader`](https://docs.oracle.com/javase/8/docs/api/java/io/Reader.html)y [`Writer`](https://docs.oracle.com/javase/8/docs/api/java/io/Writer.html). Al igual que con los flujos de bytes, existen clases de flujo de caracteres que se especializan en E/S de archivos: [`FileReader`](https://docs.oracle.com/javase/8/docs/api/java/io/FileReader.html)y [`FileWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/FileWriter.html). El [`CopyCharacters`](https://docs.oracle.com/javase/tutorial/essential/io/examples/CopyCharacters.java)ejemplo ilustra estas clases.

```java
    import java.io.FileReader;
    import java.io.FileWriter;
    import java.io.IOException;
    
    CopyCharacters de clase pública {
        public static void main (String [] args) lanza IOException {
    
            FileReader inputStream = null;
            FileWriter outputStream = null;
    
            tratar {
                inputStream = new FileReader ("xanadu.txt");
                outputStream = new FileWriter ("characteroutput.txt");
    
                int c;
                while ((c = inputStream.read ())! = -1) {
                    outputStream.write (c);
                }
            } finalmente {
                if (inputStream! = null) {
                    inputStream.close ();
                }
                if (outputStream! = null) {
                    outputStream.close ();
                }
            }
        }
    }
```
`CopyCharacters`es muy similar a `CopyBytes`. La diferencia más importante es que `CopyCharacters`utiliza `FileReader`y `FileWriter`para entrada y salida en lugar de `FileInputStream`y `FileOutputStream`. Observe que ambos `CopyBytes`y `CopyCharacters`usan una `int`variable para leer y escribir. Sin embargo, en `CopyCharacters`, la `int`variable tiene un valor de carácter en sus últimos 16 bits; en `CopyBytes`, la `int`variable tiene un `byte`valor en sus últimos 8 bits.

### Secuencias de caracteres que utilizan secuencias de bytes

Los flujos de caracteres suelen ser "envoltorios" para los flujos de bytes. El flujo de caracteres utiliza el flujo de bytes para realizar la E/S física, mientras que el flujo de caracteres maneja la traducción entre caracteres y bytes. `FileReader`, por ejemplo, usos `FileInputStream`, while `FileWriter`usos `FileOutputStream`.

Hay dos flujos "puente" de bytes a caracteres de propósito general: [`InputStreamReader`](https://docs.oracle.com/javase/8/docs/api/java/io/InputStreamReader.html)y [`OutputStreamWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/OutputStreamWriter.html). Úselos para crear secuencias de caracteres cuando no haya clases de secuencias de caracteres empaquetadas que satisfagan sus necesidades. La [lección de sockets](https://docs.oracle.com/javase/tutorial/networking/sockets/readingWriting.html) en la [ruta de redes](https://docs.oracle.com/javase/tutorial/networking/index.html) muestra cómo crear flujos de caracteres a partir de los flujos de bytes proporcionados por las clases de socket.

## E/S orientadas a línea

La E/S de caracteres suele ocurrir en unidades más grandes que los caracteres individuales. Una unidad común es la línea: una cadena de caracteres con un terminador de línea al final. Un terminador de línea puede ser una secuencia de retorno de carro / avance de línea ( `"\r\n"`), un solo retorno de carro ( `"\r"`) o un solo avance de línea ( `"\n"`). La compatibilidad con todos los terminadores de línea posibles permite a los programas leer archivos de texto creados en cualquiera de los sistemas operativos más utilizados.

Modifiquemos el `CopyCharacters`ejemplo para usar E/S orientadas a líneas. Para hacer esto, tenemos que usar dos clases que no hemos visto antes [`BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)y [`PrintWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintWriter.html). Exploraremos estas clases con mayor profundidad en [E/S](https://docs.oracle.com/javase/tutorial/essential/io/buffers.html) en [búfer](https://docs.oracle.com/javase/tutorial/essential/io/buffers.html) y [formato](https://docs.oracle.com/javase/tutorial/essential/io/formatting.html) . En este momento, solo estamos interesados ​​en su soporte para E/S orientadas a línea.

El [`CopyLines`](https://docs.oracle.com/javase/tutorial/essential/io/examples/CopyLines.java)ejemplo invoca `BufferedReader.readLine`y `PrintWriter.println`para hacer entrada y salida una línea a la vez.

import java.io.FileReader; import java.io.FileWriter; import java.io.BufferedReader; import java.io.PrintWriter; import java.io.IOException;
 CopyLines de clase pública { public static void main (String [] args) lanza IOException {
 BufferedReader inputStream = null; PrintWriter outputStream = null;
 tratar { inputStream = new BufferedReader (nuevo FileReader ("xanadu.txt")); outputStream = new PrintWriter (nuevo FileWriter ("characteroutput.txt"));
 String l; while ((l = inputStream.readLine ())! = null) { outputStream.println (l); } } finalmente { if (inputStream! = null) { inputStream.close (); } if (outputStream! = null) { outputStream.close (); } } } }

La invocación `readLine`devuelve una línea de texto con la línea. `CopyLines`envía cada línea usando `println`, que agrega el terminador de línea para el sistema operativo actual. Es posible que este no sea el mismo terminador de línea que se utilizó en el archivo de entrada.

Hay muchas formas de estructurar la entrada y salida de texto más allá de caracteres y líneas. Para obtener más información, consulte [Escaneo y formateo](https://docs.oracle.com/javase/tutorial/essential/io/scanfor.html) .

# Secuencias almacenadas en búfer

La mayoría de los ejemplos que hemos visto hasta ahora utilizan E/S _sin búfer_ . Esto significa que cada solicitud de lectura o escritura es manejada directamente por el sistema operativo subyacente. Esto puede hacer que un programa sea mucho menos eficiente, ya que cada una de estas solicitudes a menudo desencadena el acceso al disco, la actividad de la red o alguna otra operación que es relativamente costosa.

Para reducir este tipo de sobrecarga, la plataforma Java implementa flujos de E/S en _búfer_ . Los flujos de entrada _almacenados_ en _búfer_ leen datos de un área de memoria conocida como _búfer_ ; la API de entrada nativa se llama solo cuando el búfer está vacío. De manera similar, los flujos de salida almacenados en búfer escriben datos en un búfer y la API de salida nativa se llama solo cuando el búfer está lleno.

Un programa puede convertir una secuencia sin búfer en una secuencia con búfer usando el lenguaje de envoltura que hemos usado varias veces ahora, donde el objeto de la secuencia sin búfer se pasa al constructor para una clase de secuencia con búfer. Así es como puede modificar las invocaciones del constructor en el `CopyCharacters`ejemplo para usar E/S almacenadas en búfer:

inputStream = new BufferedReader (nuevo FileReader ("xanadu.txt")); outputStream = new BufferedWriter (nuevo FileWriter ("characteroutput.txt"));

Hay cuatro clases de flujo almacenado en búfer que se utilizan para envolver flujos sin búfer: [`BufferedInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedInputStream.html)y [`BufferedOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedOutputStream.html)crear flujos de bytes almacenados en búfer, while [`BufferedReader`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)y [`BufferedWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedWriter.html)crear flujos de caracteres almacenados en búfer.

## Vaciado de corrientes en búfer

A menudo tiene sentido escribir un búfer en puntos críticos, sin esperar a que se llene. Esto se conoce como _vaciar_ el búfer.

Algunas clases de salida almacenadas en búfer admiten el _vaciado automático_ , especificado por un argumento de constructor opcional. Cuando se habilita el vaciado automático, ciertos eventos clave hacen que el búfer se vacíe. Por ejemplo, un `PrintWriter`objeto de vaciado automático vacía el búfer en cada invocación de `println`o `format`. Consulte [Formateo](https://docs.oracle.com/javase/tutorial/essential/io/formatting.html) para obtener más información sobre estos métodos.

Para vaciar una secuencia manualmente, invoque su `flush`método. El `flush`método es válido en cualquier flujo de salida, pero no tiene ningún efecto a menos que el flujo esté almacenado en búfer.

# Escaneo y formateo

La programación de E/S a menudo implica traducir hacia y desde los datos cuidadosamente formateados con los que a los humanos les gusta trabajar. Para ayudarlo con estas tareas, la plataforma Java proporciona dos API. La API del [escáner](https://docs.oracle.com/javase/tutorial/essential/io/scanning.html) divide la entrada en tokens individuales asociados con bits de datos. La API de [formato](https://docs.oracle.com/javase/tutorial/essential/io/formatting.html) reúne los datos en un formato legible por humanos.


# Exploración

Los objetos de tipo [`Scanner`](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)son útiles para dividir la entrada formateada en tokens y traducir tokens individuales según su tipo de datos.

## Romper la entrada en tokens

De forma predeterminada, un escáner utiliza espacios en blanco para separar tokens. (Los caracteres de espacio en blanco incluyen espacios en blanco, tabulaciones y terminadores de línea. Para obtener la lista completa, consulte la documentación de [`Character.isWhitespace`](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#isWhitespace-char-).) Para ver cómo funciona el escaneo, veamos [`ScanXan`](https://docs.oracle.com/javase/tutorial/essential/io/examples/ScanXan.java)un programa que lee las palabras individuales `xanadu.txt`y las imprime, una por línea.

```java
import java.io. *; 
import java.util.Scanner;

public class ScanXan { 
  public static void main (String [] args) throw IOException {
	  Scanner s = null;
	 Try { 
		 s = new Scanner (new BufferedReader (new FileReader ("xanadu.txt")));
	    while (s.hasNext ()) { 
		    System.out.println (s.next ()); 
	    }
	 } finally {
	  if (s! = null) { s.close ();
	 } 
   } 
  }
}
```
Observe que `ScanXan`invoca `Scanner`el `close`método de cuando se hace con el objeto del escáner. Aunque un escáner no es una secuencia, debe cerrarlo para indicar que ha terminado con su secuencia subyacente.

La salida de se `ScanXan`ve así:

En Xanadu hizo Kubla Kan UN majestuoso cúpula de placer ...

Para usar un separador de token diferente, invoca `useDelimiter()`, especificando una expresión regular. Por ejemplo, suponga que desea que el separador de tokens sea una coma, opcionalmente seguida de un espacio en blanco. Invocarías

```java
s.useDelimiter (", \\ s *"); 
```
## Traducir tokens individuales

El `ScanXan`ejemplo trata todos los tokens de entrada como `String`valores simples . `Scanner`también admite tokens para todos los tipos primitivos del lenguaje Java (excepto para `char`), así como `BigInteger`y `BigDecimal`. Además, los valores numéricos pueden utilizar separadores de miles. Por lo tanto, en una `US`configuración regional, `Scanner`lee correctamente la cadena "32,767" como representando un valor entero.

Tenemos que mencionar la configuración regional, porque los separadores de miles y los símbolos decimales son específicos de la configuración regional. Por lo tanto, el siguiente ejemplo no funcionaría correctamente en todas las configuraciones regionales si no especificamos que el escáner debería usar la `US`configuración regional. Eso no es algo de lo que deba preocuparse normalmente, porque sus datos de entrada generalmente provienen de fuentes que usan la misma configuración regional que usted. Pero este ejemplo es parte del Tutorial de Java y se distribuye por todo el mundo.

El [`ScanSum`](https://docs.oracle.com/javase/tutorial/essential/io/examples/ScanSum.java)ejemplo lee una lista de `double`valores y los suma. Aquí está la fuente:

import java.io.FileReader; import java.io.BufferedReader; import java.io.IOException; import java.util.Scanner; import java.util.Locale;
 clase pública ScanSum { public static void main (String [] args) lanza IOException {
 Escáner s = nulo; suma doble = 0;
 tratar { s = new Scanner (new BufferedReader (new FileReader ("usnumbers.txt"))); s.useLocale (Locale.US);
 while (s.hasNext ()) { if (s.hasNextDouble ()) { suma + = s.nextDouble (); } más { s.siguiente (); }  } } finalmente { s.close (); }
 System.out.println (suma); } }

Y aquí está el archivo de entrada de muestra, [`usnumbers.txt`](https://docs.oracle.com/javase/tutorial/essential/io/examples/usnumbers.txt)

8.5 32,767 3.14159 1.000.000,1

La cadena de salida es "1032778.74159". El punto será un carácter diferente en algunas configuraciones regionales, porque `System.out`es un `PrintStream`objeto, y esa clase no proporciona una forma de anular la configuración regional predeterminada. Podríamos anular la configuración regional para todo el programa, o simplemente podríamos usar el formato, como se describe en el siguiente tema, [Formato](https://docs.oracle.com/javase/tutorial/essential/io/formatting.html) .

# Formateo

Los objetos de flujo que implementan formato son instancias de [`PrintWriter`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintWriter.html)una clase de flujo de caracteres o [`PrintStream`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html)una clase de flujo de bytes.

----------

**Nota:** Los únicos `PrintStream`objetos que probablemente necesitará son [`System.out`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#out)y [`System.err`](https://docs.oracle.com/javase/8/docs/api/java/lang/System.html#err). (Consulte [E/S desde la línea de comandos](https://docs.oracle.com/javase/tutorial/essential/io/cl.html) para obtener más información sobre estos objetos). Cuando necesite crear un flujo de salida formateado, cree una instancia `PrintWriter`, no `PrintStream`.

----------

Como todos los objetos de flujo de caracteres y bytes, instancias de un conjunto estándar de métodos para la salida de caracteres y bytes simples , `PrintStream`e `PrintWriter`implementan un conjunto estándar `write`. Además, tanto `PrintStream`y `PrintWriter`aplicar el mismo conjunto de métodos para la conversión de datos internos en la salida con formato. Se proporcionan dos niveles de formato:

-   `print`y `println`formatear valores individuales de forma estándar.
-   `format` formatea casi cualquier número de valores basado en una cadena de formato, con muchas opciones para un formato preciso.

## Los métodos `print`y`println`

Invocar `print`o `println`generar un valor único después de convertir el valor utilizando el `toString`método apropiado . Podemos ver esto en el [`Root`](https://docs.oracle.com/javase/tutorial/essential/io/examples/Root.java)ejemplo:

raíz de clase pública { public static void main (String [] args) { int i = 2; doble r = Math.sqrt (i);
         System.out.print ("La raíz cuadrada de"); System.out.print (i); System.out.print ("es"); System.out.print (r); System.out.println (".");
 i = 5; r = Math.sqrt (i); System.out.println ("La raíz cuadrada de" + i + "es" + r + "."); } }

Aquí está el resultado de `Root`:

La raíz cuadrada de 2 es 1.4142135623730951. La raíz cuadrada de 5 es 2.23606797749979.

Las variables `i`y `r`se formatean dos veces: la primera vez usando código en una sobrecarga de `print`, la segunda vez por código de conversión generado automáticamente por el compilador de Java, que también utiliza `toString`. Puede formatear cualquier valor de esta manera, pero no tiene mucho control sobre los resultados.

## El `format`método

El `format`método formatea múltiples argumentos basados ​​en una _cadena de formato_ . La cadena de formato consta de texto estático incrustado con _especificadores de formato_ ; excepto para los especificadores de formato, la cadena de formato se emite sin cambios.

Las cadenas de formato admiten muchas funciones. En este tutorial, solo cubriremos algunos conceptos básicos. Para obtener una descripción completa, consulte [`format string syntax`](https://docs.oracle.com/javase/8/docs/api/java/util/Formatter.html#syntax)la especificación API.

El [`Root2`](https://docs.oracle.com/javase/tutorial/essential/io/examples/Root2.java)ejemplo formatea dos valores con una sola `format`invocación:

public class Root2 { public static void main (String [] args) { int i = 2; doble r = Math.sqrt (i);
         System.out.format ("La raíz cuadrada de% d es% f.% N", i, r); } }

Aquí está el resultado:

La raíz cuadrada de 2 es 1.414214. 

Al igual que los tres utilizados en este ejemplo, todos los especificadores de formato comienzan con ay `%`terminan con una _conversión de_ 1 o 2 caracteres que especifica el tipo de salida formateada que se genera. Las tres conversiones utilizadas aquí son:

-   `d` da formato a un valor entero como un valor decimal.
-   `f` da formato a un valor de coma flotante como un valor decimal.
-   `n` emite un terminador de línea específico de la plataforma.

Aquí hay algunas otras conversiones:

-   `x` da formato a un entero como valor hexadecimal.
-   `s` da formato a cualquier valor como una cadena.
-   `tB` formatea un entero como un nombre de mes específico de la configuración regional.

Hay muchas otras conversiones.

----------

**Nota:**

Excepto por `%%`y `%n`, todos los especificadores de formato deben coincidir con un argumento. Si no lo hacen, se lanza una excepción.

En el lenguaje de programación Java, el `\n`escape siempre genera el carácter de salto de línea ( `\u000A`). No lo use a `\n`menos que desee específicamente un carácter de salto de línea. Para obtener el separador de línea correcto para la plataforma local, utilice `%n`.

----------

Además de la conversión, un especificador de formato puede contener varios elementos adicionales que personalizan aún más la salida formateada. Aquí hay un ejemplo, [`Format`](https://docs.oracle.com/javase/tutorial/essential/io/examples/Format.java)que utiliza todos los tipos de elementos posibles.

Formato de clase pública { public static void main (String [] args) { System.out.format ("% f,% 1 $ + 020.10f% n", Math.PI); } }

Aquí está el resultado:

3.141593, +00000003.1415926536 

Los elementos adicionales son todos opcionales. La siguiente figura muestra cómo el especificador más largo se divide en elementos.

![Elementos de un especificador de formato](https://docs.oracle.com/javase/tutorial/figures/essential/io-spec.gif)

Elementos de un especificador de formato.

Los elementos deben aparecer en el orden mostrado. Trabajando desde la derecha, los elementos opcionales son:

-   **Precisión** . Para valores de coma flotante, esta es la precisión matemática del valor formateado. Para `s`y otras conversiones generales, este es el ancho máximo del valor formateado; el valor se trunca a la derecha si es necesario.
-   **Ancho** . El ancho mínimo del valor formateado; el valor se rellena si es necesario. De forma predeterminada, el valor se rellena a la izquierda con espacios en blanco.
-   **Las banderas** especifican opciones de formato adicionales. En el `Format`ejemplo, la `+`bandera especifica que el número siempre debe estar formateado con un signo, y la `0`bandera especifica que `0`es el carácter de relleno. Otros indicadores incluyen `-`(panel a la derecha) y `,`(número de formato con separadores de miles específicos de la configuración regional). Tenga en cuenta que algunos indicadores no se pueden utilizar con otros indicadores determinados o con determinadas conversiones.
-   El **índice de argumentos le** permite hacer coincidir explícitamente un argumento designado. También puede especificar `<`que coincida con el mismo argumento que el especificador anterior. Así, el ejemplo podría haber dicho:`System.out.format("%f, %<+020.10f %n", Math.PI);`

# E/S desde la línea de comandos

Un programa a menudo se ejecuta desde la línea de comandos e interactúa con el usuario en el entorno de la línea de comandos. La plataforma Java admite este tipo de interacción de dos formas: a través de las secuencias estándar y a través de la consola.

## Secuencias estándar

Los flujos estándar son una característica de muchos sistemas operativos. De forma predeterminada, leen la entrada del teclado y escriben la salida en la pantalla. También admiten E/S en archivos y entre programas, pero esa característica está controlada por el intérprete de línea de comandos, no por el programa.

La plataforma Java admite tres secuencias _estándar_ : _entrada estándar_ , a la que se accede mediante `System.in`; _Salida estándar_ , accesible a través de `System.out`; y _Error estándar_ , a los que se accede mediante `System.err`. Estos objetos se definen automáticamente y no es necesario abrirlos. Salida estándar y Error estándar son ambos para salida; tener una salida de error por separado permite al usuario desviar la salida regular a un archivo y aún poder leer los mensajes de error. Para obtener más información, consulte la documentación de su intérprete de línea de comandos.

Es de esperar que los flujos estándar sean flujos de caracteres, pero, por razones históricas, son flujos de bytes. `System.out`y `System.err`se definen como [`PrintStream`](https://docs.oracle.com/javase/8/docs/api/java/io/PrintStream.html)objetos. Aunque técnicamente es un flujo de bytes, `PrintStream`utiliza un objeto de flujo de caracteres interno para emular muchas de las características de los flujos de caracteres.

Por el contrario, `System.in`es un flujo de bytes sin características de flujo de caracteres. Para utilizar la entrada estándar como una secuencia de caracteres, envuelva `System.in`en `InputStreamReader`.

InputStreamReader cin = nuevo InputStreamReader (System.in); 

## La consola

Una alternativa más avanzada a las transmisiones estándar es la consola. Este es un objeto de tipo único y predefinido [`Console`](https://docs.oracle.com/javase/8/docs/api/java/io/Console.html)que tiene la mayoría de las características proporcionadas por los flujos estándar, y otros además. La consola es particularmente útil para la entrada segura de contraseñas. El objeto Console también proporciona flujos de entrada y salida que son verdaderos flujos de caracteres, a través de sus métodos `reader`y `writer`.

Antes de que un programa pueda utilizar la consola, debe intentar recuperar el objeto de la consola invocando `System.console()`. Si el objeto Console está disponible, este método lo devuelve. Si `System.console`regresa `NULL`, las operaciones de la consola no están permitidas, ya sea porque el sistema operativo no las admite o porque el programa se inició en un entorno no interactivo.

El objeto de la consola admite la entrada de contraseña segura a través de su `readPassword`método. Este método ayuda a proteger la entrada de la contraseña de dos formas. Primero, suprime el eco, por lo que la contraseña no es visible en la pantalla del usuario. En segundo lugar, `readPassword`devuelve una matriz de caracteres, no a `String`, por lo que la contraseña se puede sobrescribir, eliminándola de la memoria tan pronto como ya no se necesite.

El [`Password`](https://docs.oracle.com/javase/tutorial/essential/io/examples/Password.java)ejemplo es un programa prototipo para cambiar la contraseña de un usuario. Demuestra varios `Console`métodos.
```java
import java.io.Console;
import java.util.Arrays;
import java.io.IOException;

public class Password {
    
    public static void main (String args[]) throws IOException {

        Console c = System.console();
        if (c == null) {
            System.err.println("No console.");
            System.exit(1);
        }

        String login = c.readLine("Enter your login: ");
        char [] oldPassword = c.readPassword("Enter your old password: ");

        if (verify(login, oldPassword)) {
            boolean noMatch;
            do {
                char [] newPassword1 = c.readPassword("Enter your new password: ");
                char [] newPassword2 = c.readPassword("Enter new password again: ");
                noMatch = ! Arrays.equals(newPassword1, newPassword2);
                if (noMatch) {
                    c.format("Passwords don't match. Try again.%n");
                } else {
                    change(login, newPassword1);
                    c.format("Password for %s changed.%n", login);
                }
                Arrays.fill(newPassword1, ' ');
                Arrays.fill(newPassword2, ' ');
            } while (noMatch);
        }

        Arrays.fill(oldPassword, ' ');
    }
    
    // Dummy change method.
    static boolean verify(String login, char[] password) {
        // This method always returns
        // true in this example.
        // Modify this method to verify
        // password according to your rules.
        return true;
    }

    // Dummy change method.
    static void change(String login, char[] password) {
        // Modify this method to change
        // password according to your rules.
    }
}
```

La `Password`clase sigue estos pasos:

1.  Intente recuperar el objeto de la consola. Si el objeto no está disponible, cancele.
2.  Invoque `Console.readLine`para solicitar y leer el nombre de inicio de sesión del usuario.
3.  Invocar `Console.readPassword`para solicitar y leer la contraseña existente del usuario.
4.  Invocar `verify`para confirmar que el usuario está autorizado a cambiar la contraseña. (En este ejemplo, `verify`es un método ficticio que siempre devuelve `true`).
5.  Repita los siguientes pasos hasta que el usuario ingrese la misma contraseña dos veces:
    1.  Invoque `Console.readPassword`dos veces para solicitar y leer una nueva contraseña.
    2.  Si el usuario ingresó la misma contraseña en ambas ocasiones, invoca `change`para cambiarla. (Nuevamente, `change`es un método ficticio).
    3.  Sobrescriba ambas contraseñas con espacios en blanco.
6.  Sobrescriba la contraseña anterior con espacios en blanco.

# Flujos de datos

Los flujos de datos apoyan binario I / O de los valores de tipos de datos primitivos ( `boolean`, `char`, `byte`, `short`, `int`, `long`, `float`, y `double`), así como valores de cadena. Todos los flujos de datos implementan la [`DataInput`](https://docs.oracle.com/javase/8/docs/api/java/io/DataInput.html)interfaz o la [`DataOutput`](https://docs.oracle.com/javase/8/docs/api/java/io/DataOutput.html)interfaz. Esta sección se centra en las implementaciones más utilizadas de estas interfaces [`DataInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/DataInputStream.html)y [`DataOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/DataOutputStream.html).

El [`DataStreams`](https://docs.oracle.com/javase/tutorial/essential/io/examples/DataStreams.java)ejemplo demuestra flujos de datos escribiendo un conjunto de registros de datos y luego leyéndolos nuevamente. Cada registro consta de tres valores relacionados con un artículo en una factura, como se muestra en la siguiente tabla:

Examinemos el código crucial en `DataStreams`. Primero, el programa define algunas constantes que contienen el nombre del archivo de datos y los datos que se escribirán en él:

    static final String dataFile = "invoicedata";
    static final double[] prices = { 19.99, 9.99, 15.99, 3.99, 4.99 };
    static final int[] units = { 12, 8, 13, 29, 50 };
    static final String[] descs = {
        "Java T-shirt",
        "Java Mug",
        "Duke Juggling Dolls",
        "Java Pin",
        "Java Key Chain"
    };

`DataStreams`abre un flujo de salida. Dado que un `DataOutputStream`solo se puede crear como un contenedor para un objeto de flujo de bytes existente, `DataStreams`proporciona un flujo de bytes de salida de archivo almacenado en búfer.
```java
    out = new DataOutputStream(new BufferedOutputStream(new FileOutputStream(dataFile)));
```

`DataStreams` escribe los registros y cierra el flujo de salida.

```java
 for (int i = 0; i < prices.length; i ++) {
     out.writeDouble(prices[i]);
     out.writeInt(units[i]);
     out.writeUTF(descs[i]);
 }
```
El `writeUTF`método escribe `String`valores en una forma modificada de UTF-8. Esta es una codificación de caracteres de ancho variable que solo necesita un byte para los caracteres occidentales comunes.

Ahora `DataStreams`vuelve a leer los datos. Primero debe proporcionar un flujo de entrada y variables para contener los datos de entrada. Me gusta `DataOutputStream`, `DataInputStream`debe construirse como un contenedor para un flujo de bytes.
```java
    in = new DataInputStream(new BufferedInputStream(new FileInputStream(dataFile)));
    
    double price;
    int unit;
    String desc;
    double total = 0.0;
```

Ahora `DataStreams`puede leer cada registro en la transmisión, informando sobre los datos que encuentra.
```java
   try {
       while (true) {
           price = in.readDouble();
           unit = in.readInt();
           desc = in.readUTF();
           System.out.format("You ordered %d" + " units of %s at $%.2f%n",
               unit, desc, price);
           total += unit * price;
       }
   } catch (EOFException e) {
   }
```
Observe que `DataStreams`detecta una condición de fin de archivo al capturar [`EOFException`](https://docs.oracle.com/javase/8/docs/api/java/io/EOFException.html), en lugar de probar un valor de retorno no válido. Todas las implementaciones de `DataInput`métodos utilizan en `EOFException`lugar de valores de retorno.

También tenga `write`en cuenta que cada especializado en `DataStreams`se corresponde exactamente con el correspondiente especializado `read`. Depende del programador asegurarse de que los tipos de salida y los tipos de entrada coincidan de esta manera: el flujo de entrada consta de datos binarios simples, sin nada que indique el tipo de valores individuales o dónde comienzan en el flujo.

`DataStreams`usa una técnica de programación muy mala: usa números de punto flotante para representar valores monetarios. En general, el punto flotante es malo para valores precisos. Es particularmente malo para las fracciones decimales, porque los valores comunes (como `0.1`) no tienen una representación binaria.

El tipo correcto para usar para valores de moneda es [`java.math.BigDecimal`](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html). Desafortunadamente, `BigDecimal`es un tipo de objeto, por lo que no funcionará con flujos de datos. Sin embargo, `BigDecimal`  _será_ trabajar con corrientes de objetos, que están cubiertos en la siguiente sección.

Aquí está el código completo
```java
    import java.io.FileInputStream;
    import java.io.FileOutputStream;
    import java.io.DataInputStream;
    import java.io.DataOutputStream;
    import java.io.BufferedInputStream;
    import java.io.BufferedOutputStream;
    import java.io.IOException;
    import java.io.EOFException;
     
    public class DataStreams {
        static final String dataFile = "invoicedata";
     
        static final double[] prices = { 19.99, 9.99, 15.99, 3.99, 4.99 };
        static final int[] units = { 12, 8, 13, 29, 50 };
        static final String[] descs = { "Java T-shirt",
                "Java Mug",
                "Duke Juggling Dolls",
                "Java Pin",
                "Java Key Chain" };
     
        public static void main(String[] args) throws IOException {
     
      
            DataOutputStream out = null;
             
            try {
                out = new DataOutputStream(new
                        BufferedOutputStream(new FileOutputStream(dataFile)));
     
                for (int i = 0; i < prices.length; i ++) {
                    out.writeDouble(prices[i]);
                    out.writeInt(units[i]);
                    out.writeUTF(descs[i]);
                }
            } finally {
                out.close();
            }
     
            DataInputStream in = null;
            double total = 0.0;
            try {
                in = new DataInputStream(new
                        BufferedInputStream(new FileInputStream(dataFile)));
     
                double price;
                int unit;
                String desc;
     
                try {
                    while (true) {
                        price = in.readDouble();
                        unit = in.readInt();
                        desc = in.readUTF();
                        System.out.format("You ordered %d units of %s at $%.2f%n",
                                unit, desc, price);
                        total += unit * price;
                    }
                } catch (EOFException e) { }
                System.out.format("For a TOTAL of: $%.2f%n", total);
            }
            finally {
                in.close();
            }
        }
    }
```

# Flujos de objetos

Así como los flujos de datos admiten la E/S de tipos de datos primitivos, los flujos de objetos admiten la E/S de objetos. La mayoría, pero no todas, las clases estándar admiten la serialización de sus objetos. Aquellos que implementan la interfaz de marcador [`Serializable`](https://docs.oracle.com/javase/8/docs/api/java/io/Serializable.html).

Las clases de flujo de objetos son [`ObjectInputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/ObjectInputStream.html)y [`ObjectOutputStream`](https://docs.oracle.com/javase/8/docs/api/java/io/ObjectOutputStream.html). Estas clases implementan [`ObjectInput`](https://docs.oracle.com/javase/8/docs/api/java/io/ObjectInput.html)y [`ObjectOutput`](https://docs.oracle.com/javase/8/docs/api/java/io/ObjectOutput.html), que son subinterfaces de `DataInput`y `DataOutput`. Eso significa que todos los métodos de E/S de datos primitivos cubiertos en [Flujos de datos](https://docs.oracle.com/javase/tutorial/essential/io/datastreams.html) también se implementan en flujos de objetos. Por tanto, una secuencia de objetos puede contener una mezcla de valores primitivos y de objeto. El [`ObjectStreams`](https://docs.oracle.com/javase/tutorial/essential/io/examples/ObjectStreams.java)ejemplo ilustra esto. `ObjectStreams`crea la misma aplicación que `DataStreams`, con un par de cambios. Primero, los precios ahora son [`BigDecimal`](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html)objetos, para representar mejor los valores fraccionarios. En segundo lugar, [`Calendar`](https://docs.oracle.com/javase/8/docs/api/java/util/Calendar.html)se escribe un objeto en el archivo de datos, lo que indica una fecha de factura.

Si `readObject()`no devuelve el tipo de objeto esperado, intentar convertirlo al tipo correcto puede generar un [`ClassNotFoundException`](https://docs.oracle.com/javase/8/docs/api/java/lang/ClassNotFoundException.html). En este ejemplo simple, eso no puede suceder, por lo que no intentamos detectar la excepción. En cambio, notificamos al compilador que estamos al tanto del problema agregando `ClassNotFoundException`a la cláusula `main`del método `throws`.

## Salida y entrada de objetos complejos

Los métodos `writeObject`y `readObject`son fáciles de usar, pero contienen una lógica de gestión de objetos muy sofisticada. Esto no es importante para una clase como Calendar, que simplemente encapsula valores primitivos. Pero muchos objetos contienen referencias a otros objetos. Si `readObject`va a reconstituir un objeto a partir de una corriente, debe poder reconstituir todos los objetos a los que se refiere el objeto original. Estos objetos adicionales pueden tener sus propias referencias y así sucesivamente. En esta situación, `writeObject`atraviesa toda la red de referencias de objetos y escribe todos los objetos de esa red en la secuencia. Por tanto, una sola invocación de `writeObject`puede provocar que se escriban una gran cantidad de objetos en la secuencia.

Esto se demuestra en la siguiente figura, donde `writeObject`se invoca para escribir un solo objeto llamado **a** . Este objeto contiene referencias a los objetos **b** y **c** , mientras que **b** contiene referencias a **d** y **e** . La invocación `writeobject(a)`escribe no solo **a** , sino todos los objetos necesarios para reconstituir **a** , por lo que los otros cuatro objetos de esta web también se escriben. Cuando **una** es posterior lectura por `readObject`, los otros cuatro objetos se leen también, y se conservan todas las referencias a objetos originales.

![E/S de múltiples objetos referidos](https://docs.oracle.com/javase/tutorial/figures/essential/io-trav.gif)

E/S de múltiples objetos referidos

Quizás se pregunte qué sucede si dos objetos en la misma secuencia contienen referencias a un solo objeto. ¿Ambos se referirán a un solo objeto cuando se lean? La respuesta es sí." Una secuencia solo puede contener una copia de un objeto, aunque puede contener cualquier número de referencias a él. Por lo tanto, si escribe explícitamente un objeto en una secuencia dos veces, en realidad solo está escribiendo la referencia dos veces. Por ejemplo, si el siguiente código escribe un objeto `ob`dos veces en una secuencia:
```java
Object ob = new Object(); 
out.writeObject (ob); out.writeObject (ob);
```
Cada uno `writeObject`tiene que coincidir con a `readObject`, por lo que el código que lee la transmisión se verá así:
```java
Object ob1 = in.readObject (); Objeto ob2 = in.readObject ();
```
Esto da como resultado dos variables, `ob1`y `ob2`, que son referencias a un solo objeto.

Sin embargo, si un solo objeto se escribe en dos secuencias diferentes, se duplicará de manera efectiva: un solo programa que lea ambas secuencias verá dos objetos distintos.

# E / S de archivos (con NIO.2)

El `java.nio.file`paquete y su paquete relacionado `java.nio.file.attribute`, brindan soporte completo para E / S de archivos y para acceder al sistema de archivos predeterminado. Aunque la API tiene muchas clases, debe concentrarse solo en unos pocos puntos de entrada. Verás que esta API es muy intuitiva y fácil de usar.

El tutorial comienza preguntando [qué es una ruta.](https://docs.oracle.com/javase/tutorial/essential/io/path.html) Luego, se introduce la [clase Path](https://docs.oracle.com/javase/tutorial/essential/io/pathClass.html) , el punto de entrada principal para el paquete. Se explican los métodos de la `Path`clase relacionados con [las operaciones sintácticas](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html) . Luego, el tutorial pasa a la otra clase principal del paquete, la `Files`clase, que contiene métodos que se ocupan de las operaciones de archivos. Primero, se presentan algunos conceptos comunes a muchas [operaciones de archivos](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html) . Luego, el tutorial cubre métodos para [verificar](https://docs.oracle.com/javase/tutorial/essential/io/check.html) , [eliminar](https://docs.oracle.com/javase/tutorial/essential/io/delete.html) , [copiar](https://docs.oracle.com/javase/tutorial/essential/io/copy.html) y [mover](https://docs.oracle.com/javase/tutorial/essential/io/move.html) archivos.

Los espectáculos tutorial de cómo [los metadatos](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html) se gestiona, antes de pasar a [presentar de E / S](https://docs.oracle.com/javase/tutorial/essential/io/file.html) y el [directorio de E / S](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html) . Se explican los [archivos de acceso aleatorio](https://docs.oracle.com/javase/tutorial/essential/io/rafs.html) [y](https://docs.oracle.com/javase/tutorial/essential/io/links.html) se examinan los problemas específicos de [los enlaces físicos y simbólicos](https://docs.oracle.com/javase/tutorial/essential/io/links.html) .

A continuación, se tratan algunos de los temas muy poderosos, pero más avanzados. Primero, se demuestra la capacidad de [recorrer el árbol de archivos de forma recursiva](https://docs.oracle.com/javase/tutorial/essential/io/walk.html) , seguida de información sobre cómo [buscar archivos mediante comodines](https://docs.oracle.com/javase/tutorial/essential/io/find.html) . A continuación, se explica y demuestra cómo [observar cambios en un directorio](https://docs.oracle.com/javase/tutorial/essential/io/notification.html) . Luego, se presta cierta atención a los [métodos que no encajan en otros lugares](https://docs.oracle.com/javase/tutorial/essential/io/misc.html) .

Finalmente, si tiene un código de E / S de archivo escrito antes del lanzamiento de Java SE 7, hay un [mapa de la API anterior a la API nueva](https://docs.oracle.com/javase/tutorial/essential/io/legacy.html#mapping) , así como información importante sobre el `File.toPath`método para los desarrolladores que deseen [aprovechar la nueva API. sin reescribir el código existente](https://docs.oracle.com/javase/tutorial/essential/io/legacy.html#interop) .


# ¿Qué es un camino? (Y otros hechos del sistema de archivos)

Un sistema de archivos almacena y organiza archivos en algún tipo de medio, generalmente uno o más discos duros, de tal manera que se pueden recuperar fácilmente. La mayoría de los sistemas de archivos que se utilizan actualmente almacenan los archivos en una estructura de árbol (o _jerárquica_ ). En la parte superior del árbol hay uno (o más) nodos raíz. Bajo el nodo raíz, hay archivos y directorios ( _carpetas_ en Microsoft Windows). Cada directorio puede contener archivos y subdirectorios, que a su vez pueden contener archivos y subdirectorios, y así sucesivamente, potencialmente hasta una profundidad casi ilimitada.

Esta sección cubre lo siguiente:

-   [¿Qué es un camino?](https://docs.oracle.com/javase/tutorial/essential/io/path.html#path)
-   [¿Relativo o absoluto?](https://docs.oracle.com/javase/tutorial/essential/io/path.html#relative)
-   [Enlaces simbólicos](https://docs.oracle.com/javase/tutorial/essential/io/path.html#symlink)

## ¿Qué es un camino?

La siguiente figura muestra un árbol de directorio de muestra que contiene un único nodo raíz. Microsoft Windows admite varios nodos raíz. Cada nodo raíz se asigna a un volumen, como `C:\`o `D:\`. El sistema operativo Solaris admite un único nodo raíz, que se indica con el carácter de barra `/`.

![Estructura de directorio de muestra](https://docs.oracle.com/javase/tutorial/figures/essential/io-dirStructure.gif)

Un archivo se identifica por su ruta a través del sistema de archivos, comenzando desde el nodo raíz. Por ejemplo, el `statusReport`archivo de la figura anterior se describe mediante la siguiente notación en el sistema operativo Solaris:

/home/sally/statusReport 

En Microsoft Windows, `statusReport`se describe mediante la siguiente notación:

C:\home\sally\statusReport 

El carácter que se usa para separar los nombres de los directorios (también llamado _delimitador_ ) es específico del sistema de archivos: el sistema operativo Solaris usa la barra inclinada ( `/`) y Microsoft Windows usa la barra invertida ( `\`).

## ¿Relativo o absoluto?

Un camino es _relativo_ o _absoluto_ . Una ruta absoluta siempre contiene el elemento raíz y la lista completa de directorios necesaria para ubicar el archivo. Por ejemplo, `/home/sally/statusReport`es una ruta absoluta. Toda la información necesaria para localizar el archivo está contenida en la cadena de ruta.

Una ruta relativa debe combinarse con otra ruta para acceder a un archivo. Por ejemplo, `joe/foo`es una ruta relativa. Sin más información, un programa no puede ubicar de manera confiable el `joe/foo`directorio en el sistema de archivos.

## Enlaces simbólicos

Los objetos del sistema de archivos suelen ser directorios o archivos. Todo el mundo está familiarizado con estos objetos. Pero algunos sistemas de archivos también admiten la noción de enlaces simbólicos. Un enlace simbólico también se conoce como _enlace simbólico_ o _enlace flexible_ .

Un _enlace simbólico_ es un archivo especial que sirve como referencia a otro archivo. En su mayor parte, los enlaces simbólicos son transparentes para las aplicaciones y las operaciones en los enlaces simbólicos se redirigen automáticamente al destino del enlace. (El archivo o directorio al que se apunta se denomina _destino_ del vínculo). Las excepciones son cuando se elimina o se cambia el nombre de un vínculo simbólico, en cuyo caso el vínculo en sí se elimina o se renombra y no el destino del vínculo.

En la siguiente figura, `logFile`parece ser un archivo normal para el usuario, pero en realidad es un enlace simbólico a `dir/logs/HomeLogFile`. `HomeLogFile`es el objetivo del enlace.

![Ejemplo de enlace simbólico](https://docs.oracle.com/javase/tutorial/figures/essential/io-symlink.gif)

Ejemplo de enlace simbólico.

Un enlace simbólico suele ser transparente para el usuario. Leer o escribir en un enlace simbólico es lo mismo que leer o escribir en cualquier otro archivo o directorio.

La frase _resolver un enlace_ significa sustituir la ubicación real en el sistema de archivos por el enlace simbólico. En el ejemplo, resolviendo `logFile`rendimientos `dir/logs/HomeLogFile`.

En escenarios del mundo real, la mayoría de los sistemas de archivos hacen un uso liberal de enlaces simbólicos. De vez en cuando, un vínculo simbólico creado descuidadamente puede provocar una referencia circular. Una referencia circular se produce cuando el destino de un enlace apunta al enlace original. La referencia circular puede ser indirecta: el directorio `a`apunta al directorio `b`, que apunta al directorio `c`, que contiene un subdirectorio que apunta al directorio `a`. Las referencias circulares pueden causar estragos cuando un programa recorre recursivamente una estructura de directorio. Sin embargo, este escenario se ha tenido en cuenta y no hará que su programa se repita infinitamente.

La página siguiente analiza el corazón del soporte de E / S de archivos en el lenguaje de programación Java, la `Path`clase.

# La clase Path

La [`Path`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html)clase, introducida en la versión Java SE 7, es uno de los puntos de entrada principales del [`java.nio.file`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/package-summary.html)paquete. Si su aplicación usa E / S de archivos, querrá aprender sobre las poderosas funciones de esta clase.

----------
**Nota de la versión:** Si tiene un código anterior a JDK7 que usa `java.io.File`, aún puede aprovechar la `Path`funcionalidad de la clase usando el [`File.toPath`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#toPath--)método. Consulte [Código de E / S de archivo heredado](https://docs.oracle.com/javase/tutorial/essential/io/legacy.html) para obtener más información.

----------

Como su nombre lo indica, la `Path`clase es una representación programática de una ruta en el sistema de archivos. Un `Path`objeto contiene el nombre del archivo y la lista de directorios utilizados para construir la ruta y se utiliza para examinar, localizar y manipular archivos.

Una `Path`instancia refleja la plataforma subyacente. En el SO Solaris, a `Path`usa la sintaxis de Solaris ( `/home/joe/foo`) y en Microsoft Windows, a `Path`usa la sintaxis de Windows ( `C:\home\joe\foo`). A `Path`no es independiente del sistema. No puede comparar a `Path`de un sistema de archivos de Solaris y esperar que coincida con un `Path`de un sistema de archivos de Windows, incluso si la estructura de directorios es idéntica y ambas instancias ubican el mismo archivo relativo.

Es posible que el archivo o directorio correspondiente al archivo `Path`no exista. Puede crear una `Path`instancia y manipularla de varias maneras: puede agregarla, extraer partes de ella, compararla con otra ruta. En el momento adecuado, puede utilizar los métodos de la [`Files`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)clase para comprobar la existencia del archivo correspondiente `Path`, crear el archivo, abrirlo, eliminarlo, cambiar sus permisos, etc.

# Operaciones de ruta

La [`Path`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html)clase incluye varios métodos que se pueden utilizar para obtener información sobre la ruta, acceder a elementos de la ruta, convertir la ruta a otras formas o extraer partes de una ruta. También existen métodos para hacer coincidir la cadena de ruta y métodos para eliminar redundancias en una ruta. Esta lección trata estos `Path`métodos, a veces llamados operaciones _sintácticas_ , porque operan en la ruta misma y no acceden al sistema de archivos.

Esta sección cubre lo siguiente:

-   [Creando un camino](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#create)
-   [Recuperar información sobre una ruta](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#info)
-   [Eliminar redundancias de una ruta](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#normal)
-   [Convertir una ruta](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#convert)
-   [Uniendo dos caminos](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#resolve)
-   [Crear un camino entre dos caminos](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#relativize)
-   [Comparando dos caminos](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#compare)

## Creando un camino

Una `Path`instancia contiene la información utilizada para especificar la ubicación de un archivo o directorio. En el momento en que se define, `Path`se proporciona a una serie de uno o más nombres. Se puede incluir un elemento raíz o un nombre de archivo, pero no es obligatorio. A `Path`podría constar de un solo directorio o nombre de archivo.

Puede crear fácilmente un `Path`objeto utilizando uno de los siguientes `get`métodos de la [`Paths`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html)clase auxiliar (tenga en cuenta el plural):
```java
    Path p1 = Paths.get ("/ tmp / foo"); 
    Path p2 = Paths.get (args [0]); 
    Path p3 = Paths.get (URI.create ("file:///Users/joe/FileTest.java"));
```

El `Paths.get`método es una abreviatura del siguiente código:
```java
    Path p4 = FileSystems.getDefault (). GetPath ("/ users / sally"); 
```
El siguiente ejemplo crea `/u/joe/logs/foo.log`asumiendo que su directorio de inicio es `/u/joe`, o `C:\joe\logs\foo.log`si está en Windows.
```java
Path p5 = Paths.get (System.getProperty ("user.home"), "logs", "foo.log"); 
```
## Recuperar información sobre una ruta

Puede pensar en el `Path`almacenamiento de estos elementos de nombre como una secuencia. El elemento más alto en la estructura del directorio estaría ubicado en el índice 0. El elemento más bajo en la estructura del directorio estaría ubicado en el índice `[n-1]`, donde `n`es el número de elementos de nombre en `Path`. Hay métodos disponibles para recuperar elementos individuales o una subsecuencia del `Path`uso de estos índices.

Los ejemplos de esta lección utilizan la siguiente estructura de directorios.

![Estructura de directorio de muestra](https://docs.oracle.com/javase/tutorial/figures/essential/io-dirStructure.gif)

El siguiente fragmento de código define una `Path`instancia y luego invoca varios métodos para obtener información sobre la ruta:
```java
    // Ninguno de estos métodos requiere que el archivo correspondiente
    // al Camino existe.
    // Sintaxis de Microsoft Windows
    Path path = Paths.get ("C:\\home\\joe\\foo");
    
    // Sintaxis de Linux
    Path path = Paths.get ("/home/joe/foo");
    
    System.out.format("toString: %s%n", path.toString());
    System.out.format("getFileName: %s%n", path.getFileName());
    System.out.format("getName(0): %s%n", path.getName(0));
    System.out.format("getNameCount: %d%n", path.getNameCount());
    System.out.format("subpath(0,2): %s%n", path.subpath(0,2));
    System.out.format("getParent: %s%n", path.getParent());
    System.out.format("getRoot: %s%n", path.getRoot());
```
El ejemplo anterior muestra la salida de una ruta absoluta. 

|Método| Retorna|
|--|--|
|toString|home/joe/foo |
|getFileName|foo |
|getName(0)|home  |
|getNameCount|3  |
|subpath(0,2)|home/joe  |
|getParent|/home/joe  |
|getRoot| / |

En el siguiente ejemplo, se especifica una ruta relativa:
```java
    // Sintaxis de Linux 
    Path path = Paths.get ("sally / bar");
    // Sintaxis de Microsoft Windows 
    Path path = Paths.get ("sally \\ bar");
```
Resultado para estas operaciones
|Método| Retorna|
|--|--|
|toString|sally/bar  |
|getFileName|bar  |
|getName(0)|sally  |
|getNameCount|2  |
|subpath(0,1)|sally  |
|getParent|sally  |
|getRoot| null |

## Eliminación de redundancias de una ruta

Muchos sistemas de archivos utilizan "." notación para indicar el directorio actual y ".." para indicar el directorio principal. Es posible que tenga una situación en la que `Path`contenga información de directorio redundante. Quizás un servidor esté configurado para guardar sus archivos de registro en el `/dir/logs/.`directorio " " y desee eliminar la `/.`notación " " final de la ruta.

Los siguientes ejemplos incluyen redundancias:

    /home/./joe/foo
    /home/sally/../joe/foo

El `normalize`método elimina cualquier elemento redundante, que incluye cualquier " `.`" o " `_directory_/..`" ocurrencias. Los dos ejemplos anteriores se normalizan a `/home/joe/foo`.

Es importante tener en cuenta que `normalize`no verifica el sistema de archivos cuando limpia una ruta. Es una operación puramente sintáctica. En el segundo ejemplo, si `sally`fuera un enlace simbólico, eliminarlo `sally/..`podría dar como resultado `Path`que ya no localice el archivo deseado.

Para limpiar una ruta y asegurarse de que el resultado ubica el archivo correcto, puede usar el `toRealPath`método. Este método se describe en la siguiente sección, [Conversión de una ruta](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#convert) .

## Convertir una ruta

Puede utilizar tres métodos para convertir el `Path`. Si necesita convertir la ruta en una cadena que se pueda abrir desde un navegador, puede usar [`toUri`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#toUri--). Por ejemplo:
```java
    Path p1 = Paths.get("/home/logfile");
    // Result is **file:///home/logfile**
    System.out.format("%s%n", p1.toUri());
```
El [`toAbsolutePath`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#toAbsolutePath--)método convierte una ruta en una ruta absoluta. Si la ruta pasada ya es absoluta, devuelve el mismo `Path`objeto. El `toAbsolutePath`método puede resultar muy útil al procesar nombres de archivos introducidos por el usuario. Por ejemplo:

```java
    public class FileTest {
        public static void main(String[] args) {
    
            if (args.length < 1) {
                System.out.println("usage: FileTest file");
                System.exit(-1);
            }
    
            // Converts the input string to a Path object.
            Path inputPath = Paths.get(args[0]);
    
            **// Converts the input Path
            // to an absolute path.
            // Generally, this means prepending
            // the current working
            // directory.  If this example
            // were called like this:
            //     java FileTest foo
            // the getRoot and getParent methods
            // would return null
            // on the original "inputPath"
            // instance.  Invoking getRoot and
            // getParent on the "fullPath"
            // instance returns expected values.
            Path fullPath = inputPath.toAbsolutePath();**
        }
    }
```
El `toAbsolutePath`método convierte la entrada del usuario y devuelve un valor `Path`que devuelve valores útiles cuando se consulta. No es necesario que el archivo exista para que este método funcione.

El [`toRealPath`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#toRealPath-java.nio.file.LinkOption...-)método devuelve la ruta _real_ de un archivo existente. Este método realiza varias operaciones en una:

-   Si `true`se pasa a este método y el sistema de archivos admite enlaces simbólicos, este método resuelve cualquier enlace simbólico en la ruta.
-   Si `Path`es relativo, devuelve una ruta absoluta.
-   Si `Path`contiene elementos redundantes, devuelve una ruta con esos elementos eliminados.

Este método genera una excepción si el archivo no existe o no se puede acceder a él. Puede detectar la excepción cuando desee manejar cualquiera de estos casos. Por ejemplo:
```java
    try {
        Path fp = path.toRealPath();
    } catch (NoSuchFileException x) {
        System.err.format("%s: no such" + " file or directory%n", path);
        // Logic for case when file doesn't exist.
    } catch (IOException x) {
        System.err.format("%s%n", x);
        // Logic for other sort of file error.
    }
```
## Uniendo dos caminos

Puede combinar rutas mediante el `resolve`método. Pasa en una _ruta parcial_ , que es una ruta que no incluye un elemento raíz, y esa ruta parcial se agrega a la ruta original.

Por ejemplo, considere el siguiente fragmento de código:
```java
    // Solaris
    Path p1 = Paths.get("/home/joe/foo");
    // Result is **/home/joe/foo/bar**
    System.out.format("%s%n", p1.resolve("bar"));
    
    or
    
    // Microsoft Windows
    Path p1 = Paths.get("C:\\home\\joe\\foo");
    // Result is **C:\home\joe\foo\bar**
    System.out.format("%s%n", p1.resolve("bar"));
```
Pasando una ruta absoluta para resolver el método de retorno pasado en el Path:
```java
     // Result is **/home/joe**
     Paths.get("foo").resolve("/home/joe");
```

## Crear un camino entre dos caminos

Un requisito común al escribir código de E / S de archivo es la capacidad de construir una ruta desde una ubicación en el sistema de archivos a otra ubicación. Puedes cumplir con esto usando el `relativize`método. Este método construye una ruta que se origina en la ruta original y termina en la ubicación especificada por la ruta pasada. La nueva ruta es _relativa_ a la ruta original.

Por ejemplo, considere dos rutas relativas definidas como `joe`y `sally`:
```java
    Path p1 = Paths.get("joe");
    Path p2 = Paths.get("sally");
```
En ausencia de cualquier otra información, se supone que `joe`y `sally`son hermanos, es decir, nodos que residen en el mismo nivel en la estructura del árbol. Para navegar de `joe`a `sally`, esperaría navegar primero un nivel hasta el nodo principal y luego hacia abajo a `sally`:

``` java
// Result is **../sally**
Path p1_to_p2 = p1.relativize(p2);
// Result is **../joe**
Path p2_to_p1 = p2.relativize(p1);
```

Considere un ejemplo un poco más complicado:

``` java
Path p1 = Paths.get("home");
Path p3 = Paths.get("home/sally/bar");
// Result is **sally/bar**
Path p1_to_p3 = p1.relativize(p3);
// Result is **../..**
Path p3_to_p1 = p3.relativize(p1);
```

En este ejemplo, las dos trayectorias comparten el mismo nodo, `home`. Para navegar de `home`a `bar`, primero navega un nivel hacia abajo `sally`y luego un nivel más hacia abajo `bar`. Navegar de `bar`a `home`requiere subir dos niveles.

No se puede construir una ruta relativa si solo una de las rutas incluye un elemento raíz. Si ambas rutas incluyen un elemento raíz, la capacidad de construir una ruta relativa depende del sistema.

El [ `Copy` ](https://docs.oracle.com/javase/tutorial/essential/io/examples/Copy.java)ejemplo recursivo usa los métodos `relativize`y `resolve`.

## Comparando dos caminos

La `Path`clase admite [`equals`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#equals-java.lang.Object-), lo que le permite probar dos caminos para la igualdad. Los métodos [`startsWith`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#startsWith-java.nio.file.Path-)y le [`endsWith`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#endsWith-java.nio.file.Path-)permiten probar si una ruta comienza o termina con una cadena en particular. Estos métodos son fáciles de usar. Por ejemplo:

``` java
Path path = ...;
Path otherPath = ...;
Path beginning = Paths.get("/home");
Path ending = Paths.get("foo");

if (path.equals(otherPath)) {
    // _equality logic here_
} else if (path.startsWith(beginning)) {
    // _path begins with "/home"_
} else if (path.endsWith(ending)) {
    // _path ends with "foo"_
}
```

La `Path`clase implementa la [`Iterable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html)interfaz. El [`iterator`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#iterator--)método devuelve un objeto que le permite iterar sobre los elementos del nombre en la ruta. El primer elemento devuelto es el más cercano a la raíz en el árbol de directorios. El siguiente fragmento de código itera sobre una ruta, imprimiendo cada elemento del nombre:

```java
Path path = ...;
for (Path name: path) {
    System.out.println(name);
}
```

La `Path`clase también implementa la [`Comparable`](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)interfaz. Puede comparar `Path`objetos utilizando lo `compareTo`que sea útil para ordenar.

También puede poner `Path`objetos en un `Collection`. Consulte la ruta [Colecciones](https://docs.oracle.com/javase/tutorial/collections/index.html) para obtener más información sobre esta potente función.

Cuando desee verificar que dos `Path`objetos ubican el mismo archivo, puede usar el `isSameFile`método, como se describe en [Verificar si dos rutas](https://docs.oracle.com/javase/tutorial/essential/io/check.html#same) ubican el mismo archivo .

# Operaciones con Ficheros

The  [`Files`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html)  class is the other primary entrypoint of the  `java.nio.file`  package. This class offers a rich set of static methods for reading, writing, and manipulating files and directories. The  `Files`  methods work on instances of  `Path`  objects. Before proceeding to the remaining sections, you should familiarize yourself with the following common concepts:

-   [Releasing System Resources](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#resources)
-   [Catching Exceptions](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#exception)
-   [Varargs](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#varargs)
-   [Atomic Operations](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#atomic)
-   [Method Chaining](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#chaining)
-   [What  _Is_  a Glob?](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob)
-   [Link Awareness](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#linkaware)

## Releasing System Resources

Many of the resources that are used in this API, such as streams or channels, implement or extend the  [`java.io.Closeable`](https://docs.oracle.com/javase/8/docs/api/java/io/Closeable.html)  interface. A requirement of a  `Closeable`  resource is that the  `close`  method must be invoked to release the resource when no longer required. Neglecting to close a resource can have a negative implication on an application's performance. The  `try-`with-resources statement, described in the next section, handles this step for you.

## Catching Exceptions

With file I/O, unexpected conditions are a fact of life: a file exists (or doesn't exist) when expected, the program doesn't have access to the file system, the default file system implementation does not support a particular function, and so on. Numerous errors can be encountered.

All methods that access the file system can throw an  `IOException`. It is best practice to catch these exceptions by embedding these methods into a  `try-`with-resources statement, introduced in the Java SE 7 release. The  `try-`with-resources statement has the advantage that the compiler automatically generates the code to close the resource(s) when no longer required. The following code shows how this might look:

```java
Charset charset = Charset.forName("US-ASCII");
String s = ...;
try (BufferedWriter writer = Files.newBufferedWriter(file, charset)) {
    writer.write(s, 0, s.length());
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
}
```

For more information, see  [The try-with-resources Statement](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html).

Alternatively, you can embed the file I/O methods in a  `try`  block and then catch any exceptions in a  `catch`  block. If your code has opened any streams or channels, you should close them in a  `finally`  block. The previous example would look something like the following using the try-catch-finally approach:

```java
Charset charset = Charset.forName("US-ASCII");
String s = ...;
BufferedWriter writer = null;
try {
    writer = Files.newBufferedWriter(file, charset);
    writer.write(s, 0, s.length());
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
} finally {
    if (writer != null) writer.close();
}
```

For more information, see  [Catching and Handling Exceptions](https://docs.oracle.com/javase/tutorial/essential/exceptions/handling.html).

In addition to  `IOException`, many specific exceptions extend  [`FileSystemException`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html). This class has some useful methods that return the file involved  [(`getFile`)](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html#getFile--), the detailed message string  [(`getMessage`)](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html#getMessage--), the reason why the file system operation failed  [(`getReason`)](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html#getReason--), and the "other" file involved, if any  [(`getOtherFile`)](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystemException.html#getOtherFile--).

The following code snippet shows how the  `getFile`  method might be used:

```java
try (...) {
    ...    
} catch (NoSuchFileException x) {
    System.err.format("%s does not exist\n", x.getFile());
}
```
For purposes of clarity, the file I/O examples in this lesson may not show exception handling, but your code should always include it.

## Varargs

Several  `Files`  methods accept an arbitrary number of arguments when flags are specified. For example, in the following method signature, the ellipses notation after the  `CopyOption`  argument indicates that the method accepts a variable number of arguments, or  _varargs_, as they are typically called:

Path Files.move(Path, Path, **CopyOption...**)

When a method accepts a varargs argument, you can pass it a comma-separated list of values or an array (`CopyOption[]`) of values.

In the  `move`  example, the method can be invoked as follows:

import static java.nio.file.StandardCopyOption.*;
```java
Path source = ...;
Path target = ...;
Files.move(source,
           target,
           REPLACE_EXISTING,
           ATOMIC_MOVE);
```
For more information about varargs syntax, see  [Arbitrary Number of Arguments](https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html#varargs).

## Atomic Operations

Several  `Files`  methods, such as  `move`, can perform certain operations atomically in some file systems.

An  _atomic file operation_  is an operation that cannot be interrupted or "partially" performed. Either the entire operation is performed or the operation fails. This is important when you have multiple processes operating on the same area of the file system, and you need to guarantee that each process accesses a complete file.

## Method Chaining

Many of the file I/O methods support the concept of  _method chaining_.

You first invoke a method that returns an object. You then immediately invoke a method on  _that_  object, which returns yet another object, and so on. Many of the I/O examples use the following technique:

```java
String value = Charset.defaultCharset().decode(buf).toString();
UserPrincipal group =
    file.getFileSystem().getUserPrincipalLookupService().
         lookupPrincipalByName("me");
```
This technique produces compact code and enables you to avoid declaring temporary variables that you don't need.

## What  _Is_  a Glob?

Two methods in the  `Files`  class accept a glob argument, but what is a  _glob_?

You can use glob syntax to specify pattern-matching behavior.

A glob pattern is specified as a string and is matched against other strings, such as directory or file names. Glob syntax follows several simple rules:

-   An asterisk,  `*`, matches any number of characters (including none).
-   Two asterisks,  `**`, works like  `*`  but crosses directory boundaries. This syntax is generally used for matching complete paths.
-   A question mark,  `?`, matches exactly one character.
-   Braces specify a collection of subpatterns. For example:
    -   `{sun,moon,stars}`  matches "sun", "moon", or "stars".
    -   `{temp*,tmp*}`  matches all strings beginning with "temp" or "tmp".
-   Square brackets convey a set of single characters or, when the hyphen character (`-`) is used, a range of characters. For example:
    
    -   `[aeiou]`  matches any lowercase vowel.
    -   `[0-9]`  matches any digit.
    -   `[A-Z]`  matches any uppercase letter.
    -   `[a-z,A-Z]`  matches any uppercase or lowercase letter.
    
    Within the square brackets,  `*`,  `?`, and  `\`  match themselves.
-   All other characters match themselves.
-   To match  `*`,  `?`, or the other special characters, you can escape them by using the backslash character,  `\`. For example:  `\\`  matches a single backslash, and  `\?`  matches the question mark.

Here are some examples of glob syntax:

-   `*.html`  – Matches all strings that end in  _.html_
-   `???`  – Matches all strings with exactly three letters or digits
-   `*[0-9]*`  – Matches all strings containing a numeric value
-   `*.{htm,html,pdf}`  – Matches any string ending with  _.htm_,  _.html_  or  _.pdf_
-   `a?*.java`  – Matches any string beginning with  `a`, followed by at least one letter or digit, and ending with  _.java_
-   `{foo*,*[0-9]*}`  – Matches any string beginning with  _foo_  or any string containing a numeric value

----------

**Note:** If you are typing the glob pattern at the keyboard and it contains one of the special characters, you must put the pattern in quotes (`"*"`), use the backslash (`\*`), or use whatever escape mechanism is supported at the command line.

----------

The glob syntax is powerful and easy to use. However, if it is not sufficient for your needs, you can also use a regular expression. For more information, see the  [Regular Expressions](https://docs.oracle.com/javase/tutorial/essential/regex/index.html)  lesson.

For more information about the glob sytnax, see the API specification for the  [`getPathMatcher`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getPathMatcher-java.lang.String-)  method in the  `FileSystem`  class.

## Link Awareness

The  `Files`  class is "link aware." Every  `Files`  method either detects what to do when a symbolic link is encountered, or it provides an option enabling you to configure the behavior when a symbolic link is encountered.

# Checking a File or Directory

You have a  `Path`  instance representing a file or directory, but does that file exist on the file system? Is it readable? Writable? Executable?

## Verifying the Existence of a File or Directory

The methods in the  `Path`  class are syntactic, meaning that they operate on the  `Path`  instance. But eventually you must access the file system to verify that a particular  `Path`  exists, or does not exist. You can do so with the  [`exists(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#exists-java.nio.file.Path-java.nio.file.LinkOption...-)  and the  [`notExists(Path, LinkOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#notExists-java.nio.file.Path-java.nio.file.LinkOption...-)  methods. Note that  `!Files.exists(path)`  is not equivalent to  `Files.notExists(path)`. When you are testing a file's existence, three results are possible:

-   The file is verified to exist.
-   The file is verified to not exist.
-   The file's status is unknown. This result can occur when the program does not have access to the file.

If both  `exists`  and  `notExists`  return  `false`, the existence of the file cannot be verified.

## Checking File Accessibility

To verify that the program can access a file as needed, you can use the  [`isReadable(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isReadable-java.nio.file.Path-),  [`isWritable(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isWritable-java.nio.file.Path-), and  [`isExecutable(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isExecutable-java.nio.file.Path-)  methods.

The following code snippet verifies that a particular file exists and that the program has the ability to execute the file.

```java
Path file = ...;
boolean isRegularExecutableFile = Files.isRegularFile(file) &
         Files.isReadable(file) & Files.isExecutable(file);
```
----------

**Note:** Once any of these methods completes, there is no guarantee that the file can be accessed. A common security flaw in many applications is to perform a check and then access the file. For more information, use your favorite search engine to look up  `TOCTTOU`  (pronounced  _TOCK-too_).

----------

## Checking Whether Two Paths Locate the Same File

When you have a file system that uses symbolic links, it is possible to have two different paths that locate the same file. The  [`isSameFile(Path, Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#isSameFile-java.nio.file.Path-java.nio.file.Path-)  method compares two paths to determine if they locate the same file on the file system. For example:
```java
Path p1 = ...;
Path p2 = ...;

if (Files.isSameFile(p1, p2)) {
    // Logic when the paths locate the same file
}
```

# Deleting a File or Directory

You can delete files, directories or links. With symbolic links, the link is deleted and not the target of the link. With directories, the directory must be empty, or the deletion fails.

The  `Files`  class provides two deletion methods.

The  [`delete(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#delete-java.nio.file.Path-)  method deletes the file or throws an exception if the deletion fails. For example, if the file does not exist a  `NoSuchFileException`  is thrown. You can catch the exception to determine why the delete failed as follows:
```java
try {
    Files.delete(path);
} catch (NoSuchFileException x) {
    System.err.format("%s: no such" + " file or directory%n", path);
} catch (DirectoryNotEmptyException x) {
    System.err.format("%s not empty%n", path);
} catch (IOException x) {
    // File permission problems are caught here.
    System.err.println(x);
}
```

The  [`deleteIfExists(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#deleteIfExists-java.nio.file.Path-)  method also deletes the file, but if the file does not exist, no exception is thrown. Failing silently is useful when you have multiple threads deleting files and you don't want to throw an exception just because one thread did so first.

# Copying a File or Directory

You can copy a file or directory by using the  [`copy(Path, Path, CopyOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#copy-java.nio.file.Path-java.nio.file.Path-java.nio.file.CopyOption...-)  method. The copy fails if the target file exists, unless the  `REPLACE_EXISTING`  option is specified.

Directories can be copied. However, files inside the directory are not copied, so the new directory is empty even when the original directory contains files.

When copying a symbolic link, the target of the link is copied. If you want to copy the link itself, and not the contents of the link, specify either the  `NOFOLLOW_LINKS`  or  `REPLACE_EXISTING`  option.

This method takes a varargs argument. The following  `StandardCopyOption`  and  `LinkOption`  enums are supported:

-   `REPLACE_EXISTING`  – Performs the copy even when the target file already exists. If the target is a symbolic link, the link itself is copied (and not the target of the link). If the target is a non-empty directory, the copy fails with the  `FileAlreadyExistsException`  exception.
-   `COPY_ATTRIBUTES`  – Copies the file attributes associated with the file to the target file. The exact file attributes supported are file system and platform dependent, but  `last-modified-time`  is supported across platforms and is copied to the target file.
-   `NOFOLLOW_LINKS`  – Indicates that symbolic links should not be followed. If the file to be copied is a symbolic link, the link is copied (and not the target of the link).

If you are not familiar with  `enums`, see  [Enum Types](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html).

The following shows how to use the  `copy`  method:

```java
import static java.nio.file.StandardCopyOption.*;
...
Files.copy(source, target, REPLACE_EXISTING);
```

In addition to file copy, the  `Files`  class also defines methods that may be used to copy between a file and a stream. The  [`copy(InputStream, Path, CopyOptions...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#copy-java.io.InputStream-java.nio.file.Path-java.nio.file.CopyOption...-)  method may be used to copy all bytes from an input stream to a file. The  [`copy(Path, OutputStream)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#copy-java.nio.file.Path-java.io.OutputStream-)  method may be used to copy all bytes from a file to an output stream.

The  [ `Copy` ](https://docs.oracle.com/javase/tutorial/essential/io/examples/Copy.java)  example uses the  `copy`  and  `Files.walkFileTree`  methods to support a recursive copy. See  [Walking the File Tree](https://docs.oracle.com/javase/tutorial/essential/io/walk.html)  for more information.

# Moving a File or Directory

You can move a file or directory by using the  [`move(Path, Path, CopyOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#move-java.nio.file.Path-java.nio.file.Path-java.nio.file.CopyOption...-)  method. The move fails if the target file exists, unless the  `REPLACE_EXISTING`  option is specified.

Empty directories can be moved. If the directory is not empty, the move is allowed when the directory can be moved without moving the contents of that directory. On UNIX systems, moving a directory within the same partition generally consists of renaming the directory. In that situation, this method works even when the directory contains files.

This method takes a varargs argument – the following  `StandardCopyOption`  enums are supported:

-   `REPLACE_EXISTING`  – Performs the move even when the target file already exists. If the target is a symbolic link, the symbolic link is replaced but what it points to is not affected.
-   `ATOMIC_MOVE`  – Performs the move as an atomic file operation. If the file system does not support an atomic move, an exception is thrown. With an  `ATOMIC_MOVE`  you can move a file into a directory and be guaranteed that any process watching the directory accesses a complete file.

The following shows how to use the  `move`  method:

```java
import static java.nio.file.StandardCopyOption.*;
...
Files.move(source, target, REPLACE_EXISTING);
```

Though you can implement the  `move`  method on a single directory as shown, the method is most often used with the file tree recursion mechanism. For more information, see  [Walking the File Tree](https://docs.oracle.com/javase/tutorial/essential/io/walk.html).

# Managing Metadata (File and File Store Attributes)

Para esta sección ver directamente la sección del tutorial ...    https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html


# Reading, Writing, and Creating Files

This page discusses the details of reading, writing, creating, and opening files. There are a wide array of file I/O methods to choose from. To help make sense of the API, the following diagram arranges the file I/O methods by complexity.

![Line drawing with file I/O methods arranged from least complex (on the left) to most complex (on the right).](https://docs.oracle.com/javase/tutorial/figures/essential/io-fileiomethods.gif)

File I/O Methods Arranged from Less Complex to More Complex

On the far left of the diagram are the utility methods  `readAllBytes`,  `readAllLines`, and the  `write`  methods, designed for simple, common cases. To the right of those are the methods used to iterate over a stream or lines of text, such as  `newBufferedReader`,  `newBufferedWriter`, then  `newInputStream`  and  `newOutputStream`. These methods are interoperable with the  `java.io`  package. To the right of those are the methods for dealing with  `ByteChannels`,  `SeekableByteChannels`, and  `ByteBuffers`, such as the  `newByteChannel`  method. Finally, on the far right are the methods that use  `FileChannel`  for advanced applications needing file locking or memory-mapped I/O.

----------

**Note:** The methods for creating a new file enable you to specify an optional set of initial attributes for the file. For example, on a file system that supports the POSIX set of standards (such as UNIX), you can specify a file owner, group owner, or file permissions at the time the file is created. The  [Managing Metadata](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)  page explains file attributes, and how to access and set them.

----------

This page has the following topics:

-   [The  `OpenOptions`  Parameter](https://docs.oracle.com/javase/tutorial/essential/io/file.html#openOptions)
-   [Commonly Used Methods for Small Files](https://docs.oracle.com/javase/tutorial/essential/io/file.html#common)
-   [Buffered I/O Methods for Text Files](https://docs.oracle.com/javase/tutorial/essential/io/file.html#textfiles)
-   [Methods for Unbuffered Streams and Interoperable with  `java.io`  APIs](https://docs.oracle.com/javase/tutorial/essential/io/file.html#streams)
-   [Methods for Channels and  `ByteBuffers`](https://docs.oracle.com/javase/tutorial/essential/io/file.html#channels)
-   [Methods for Creating Regular and Temporary Files](https://docs.oracle.com/javase/tutorial/essential/io/file.html#creating)

----------

## The  `OpenOptions`  Parameter

Several of the methods in this section take an optional  `OpenOptions`  parameter. This parameter is optional and the API tells you what the default behavior is for the method when none is specified.

The following  `StandardOpenOptions`  enums are supported:

-   `WRITE`  – Opens the file for write access.
-   `APPEND`  – Appends the new data to the end of the file. This option is used with the  `WRITE`  or  `CREATE`  options.
-   `TRUNCATE_EXISTING`  – Truncates the file to zero bytes. This option is used with the  `WRITE`  option.
-   `CREATE_NEW`  – Creates a new file and throws an exception if the file already exists.
-   `CREATE`  – Opens the file if it exists or creates a new file if it does not.
-   `DELETE_ON_CLOSE`  – Deletes the file when the stream is closed. This option is useful for temporary files.
-   `SPARSE`  – Hints that a newly created file will be sparse. This advanced option is honored on some file systems, such as NTFS, where large files with data "gaps" can be stored in a more efficient manner where those empty gaps do not consume disk space.
-   `SYNC`  – Keeps the file (both content and metadata) synchronized with the underlying storage device.
-   `DSYNC`  – Keeps the file content synchronized with the underlying storage device.

----------

## Commonly Used Methods for Small Files

### Reading All Bytes or Lines from a File

If you have a small-ish file and you would like to read its entire contents in one pass, you can use the  [`readAllBytes(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#readAllBytes-java.nio.file.Path-)  or  [`readAllLines(Path, Charset)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#readAllLines-java.nio.file.Path-java.nio.charset.Charset-)  method. These methods take care of most of the work for you, such as opening and closing the stream, but are not intended for handling large files. The following code shows how to use the  `readAllBytes`  method:

Path file = ...;
byte[] fileArray;
fileArray = Files.readAllBytes(file);

### Writing All Bytes or Lines to a File

You can use one of the write methods to write bytes, or lines, to a file.

-   [`write(Path, byte[], OpenOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#write-java.nio.file.Path-byte:A-java.nio.file.OpenOption...-)
-   [`write(Path, Iterable< extends CharSequence>, Charset, OpenOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#write-java.nio.file.Path-java.lang.Iterable-java.nio.charset.Charset-java.nio.file.OpenOption...-)

The following code snippet shows how to use a  `write`  method.

Path file = ...;
byte[] buf = ...;
Files.write(file, buf);

----------

## Buffered I/O Methods for Text Files

The  `java.nio.file`  package supports channel I/O, which moves data in buffers, bypassing some of the layers that can bottleneck stream I/O.

### Reading a File by Using Buffered Stream I/O

The  [`newBufferedReader(Path, Charset)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newBufferedReader-java.nio.file.Path-java.nio.charset.Charset-)  method opens a file for reading, returning a  `BufferedReader`  that can be used to read text from a file in an efficient manner.

The following code snippet shows how to use the  `newBufferedReader`  method to read from a file. The file is encoded in "US-ASCII."
```java
Charset charset = Charset.forName("US-ASCII");
try (BufferedReader reader = Files.newBufferedReader(file, charset)) {
    String line = null;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
}
```

### Writing a File by Using Buffered Stream I/O

You can use the  [`newBufferedWriter(Path, Charset, OpenOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newBufferedWriter-java.nio.file.Path-java.nio.charset.Charset-java.nio.file.OpenOption...-)  method to write to a file using a  `BufferedWriter`.

The following code snippet shows how to create a file encoded in "US-ASCII" using this method:
```java
Charset charset = Charset.forName("US-ASCII");
String s = ...;
try (BufferedWriter writer = Files.newBufferedWriter(file, charset)) {
    writer.write(s, 0, s.length());
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
}
```
----------

## Methods for Unbuffered Streams and Interoperable with  `java.io`  APIs

### Reading a File by Using Stream I/O

To open a file for reading, you can use the  [`newInputStream(Path, OpenOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newInputStream-java.nio.file.Path-java.nio.file.OpenOption...-)  method. This method returns an unbuffered input stream for reading bytes from the file.
```java
Path file = ...;
try (InputStream in = Files.newInputStream(file);
    BufferedReader reader =
      new BufferedReader(new InputStreamReader(in))) {
    String line = null;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException x) {
    System.err.println(x);
}
```
### Creating and Writing a File by Using Stream I/O

You can create a file, append to a file, or write to a file by using the  [`newOutputStream(Path, OpenOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newOutputStream-java.nio.file.Path-java.nio.file.OpenOption...-)  method. This method opens or creates a file for writing bytes and returns an unbuffered output stream.

The method takes an optional  `OpenOption`  parameter. If no open options are specified, and the file does not exist, a new file is created. If the file exists, it is truncated. This option is equivalent to invoking the method with the  `CREATE`  and  `TRUNCATE_EXISTING`  options.

The following example opens a log file. If the file does not exist, it is created. If the file exists, it is opened for appending.
```java
import static java.nio.file.StandardOpenOption.*;
import java.nio.file.*;
import java.io.*;

public class LogFileTest {

  public static void main(String[] args) {

    // Convert the string to a
    // byte array.
    String s = "Hello World! ";
    byte data[] = s.getBytes();
    Path p = Paths.get("./logfile.txt");

    try (OutputStream out = new BufferedOutputStream(
      Files.newOutputStream(p, CREATE, APPEND))) {
      out.write(data, 0, data.length);
    } catch (IOException x) {
      System.err.println(x);
    }
  }
}
```
----------

## Methods for Channels and  `ByteBuffers`

### Reading and Writing Files by Using Channel I/O

While stream I/O reads a character at a time, channel I/O reads a buffer at a time. The  [`ByteChannel`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/ByteChannel.html)  interface provides basic  `read`  and  `write`  functionality. A  [`SeekableByteChannel`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/SeekableByteChannel.html)  is a  `ByteChannel`  that has the capability to maintain a position in the channel and to change that position. A  `SeekableByteChannel`  also supports truncating the file associated with the channel and querying the file for its size.

The capability to move to different points in the file and then read from or write to that location makes random access of a file possible. See  [Random Access Files](https://docs.oracle.com/javase/tutorial/essential/io/rafs.html)  for more information.

There are two methods for reading and writing channel I/O.

-   [`newByteChannel(Path, OpenOption...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newByteChannel-java.nio.file.Path-java.nio.file.OpenOption...-)
-   [`newByteChannel(Path, Set<? extends OpenOption>, FileAttribute<?>...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newByteChannel-java.nio.file.Path-java.util.Set-java.nio.file.attribute.FileAttribute...-)

----------

**Note:** The  `newByteChannel`  methods return an instance of a  `SeekableByteChannel`. With a default file system, you can cast this seekable byte channel to a  [`FileChannel`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/FileChannel.html)  providing access to more advanced features such mapping a region of the file directly into memory for faster access, locking a region of the file so other processes cannot access it, or reading and writing bytes from an absolute position without affecting the channel's current position.

----------

Both  `newByteChannel`  methods enable you to specify a list of  `OpenOption`  options. The same  [open options](https://docs.oracle.com/javase/tutorial/essential/io/file.html#openOptions)  used by the  `newOutputStream`  methods are supported, in addition to one more option:  `READ`  is required because the  `SeekableByteChannel`  supports both reading and writing.

Specifying  `READ`  opens the channel for reading. Specifying  `WRITE`  or  `APPEND`  opens the channel for writing. If none of these options is specified, the channel is opened for reading.

The following code snippet reads a file and prints it to standard output:
```java
// Defaults to READ
try (SeekableByteChannel sbc = Files.newByteChannel(file)) {
    ByteBuffer buf = ByteBuffer.allocate(10);

    // Read the bytes with the proper encoding for this platform.  If
    // you skip this step, you might see something that looks like
    // Chinese characters when you expect Latin-style characters.
    String encoding = System.getProperty("file.encoding");
    while (sbc.read(buf) > 0) {
        buf.rewind();
        System.out.print(Charset.forName(encoding).decode(buf));
        buf.flip();
    }
} catch (IOException x) {
    System.out.println("caught exception: " + x);
}
```

The following example, written for UNIX and other POSIX file systems, creates a log file with a specific set of file permissions. This code creates a log file or appends to the log file if it already exists. The log file is created with read/write permissions for owner and read only permissions for group.

```java
import static java.nio.file.StandardOpenOption.*;
import java.nio.*;
import java.nio.channels.*;
import java.nio.file.*;
import java.nio.file.attribute.*;
import java.io.*;
import java.util.*;

public class LogFilePermissionsTest {

  public static void main(String[] args) {
  
    // Create the set of options for appending to the file.
    Set<OpenOption> options = new HashSet<OpenOption>();
    options.add(APPEND);
    options.add(CREATE);

    // Create the custom permissions attribute.
    Set<PosixFilePermission> perms =
      PosixFilePermissions.fromString("rw-r-----");
    FileAttribute<Set<PosixFilePermission>> attr =
      PosixFilePermissions.asFileAttribute(perms);

    // Convert the string to a ByteBuffer.
    String s = "Hello World! ";
    byte data[] = s.getBytes();
    ByteBuffer bb = ByteBuffer.wrap(data);
    
    Path file = Paths.get("./permissions.log");

    try (SeekableByteChannel sbc =
      Files.newByteChannel(file, options, attr)) {
      sbc.write(bb);
    } catch (IOException x) {
      System.out.println("Exception thrown: " + x);
    }
  }
}
```

----------

## Methods for Creating Regular and Temporary Files

### Creating Files

You can create an empty file with an initial set of attributes by using the  [`createFile(Path, FileAttribute<?>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createFile-java.nio.file.Path-java.nio.file.attribute.FileAttribute...-)  method. For example, if, at the time of creation, you want a file to have a particular set of file permissions, use the  `createFile`  method to do so. If you do not specify any attributes, the file is created with default attributes. If the file already exists,  `createFile`  throws an exception.

In a single atomic operation, the  `createFile`  method checks for the existence of the file and creates that file with the specified attributes, which makes the process more secure against malicious code.

The following code snippet creates a file with default attributes:
```java
Path file = ...;
try {
    // Create the empty file with default permissions, etc.
    Files.createFile(file);
} catch (FileAlreadyExistsException x) {
    System.err.format("file named %s" +
        " already exists%n", file);
} catch (IOException x) {
    // Some other sort of failure, such as permissions.
    System.err.format("createFile error: %s%n", x);
}
```
[POSIX File Permissions](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html#posix)  has an example that uses  `createFile(Path, FileAttribute<?>)`  to create a file with pre-set permissions.

You can also create a new file by using the  `newOutputStream`  methods, as described in  [Creating and Writing a File using Stream I/O](https://docs.oracle.com/javase/tutorial/essential/io/file.html#createStream). If you open a new output stream and close it immediately, an empty file is created.

### Creating Temporary Files

You can create a temporary file using one of the following  `createTempFile`  methods:

-   [`createTempFile(Path, String, String, FileAttribute<?>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createTempFile-java.nio.file.Path-java.lang.String-java.lang.String-java.nio.file.attribute.FileAttribute...-)
-   [`createTempFile(String, String, FileAttribute<?>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createTempFile-java.lang.String-java.lang.String-java.nio.file.attribute.FileAttribute...-)

The first method allows the code to specify a directory for the temporary file and the second method creates a new file in the default temporary-file directory. Both methods allow you to specify a suffix for the filename and the first method allows you to also specify a prefix. The following code snippet gives an example of the second method:

```java
try {
    Path tempFile = Files.createTempFile(null, ".myapp");
    System.out.format("The temporary file" +
        " has been created: %s%n", tempFile)
;
} catch (IOException x) {
    System.err.format("IOException: %s%n", x);
}
```
The result of running this file would be something like the following:

The temporary file has been created: /tmp/509668702974537184.myapp

The specific format of the temporary file name is platform specific.

# Random Access Files

_Random access files_  permit nonsequential, or random, access to a file's contents. To access a file randomly, you open the file, seek a particular location, and read from or write to that file.

This functionality is possible with the  [`SeekableByteChannel`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/SeekableByteChannel.html)  interface. The  `SeekableByteChannel`  interface extends channel I/O with the notion of a current position. Methods enable you to set or query the position, and you can then read the data from, or write the data to, that location. The API consists of a few, easy to use, methods:

-   [`position`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/SeekableByteChannel.html#position--)  – Returns the channel's current position
-   [`position(long)`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/SeekableByteChannel.html#position-long-)  – Sets the channel's position
-   [`read(ByteBuffer)`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/SeekableByteChannel.html#read-java.nio.ByteBuffer-)  – Reads bytes into the buffer from the channel
-   [`write(ByteBuffer)`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/SeekableByteChannel.html#write-java.nio.ByteBuffer-)  – Writes bytes from the buffer to the channel
-   [`truncate(long)`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/SeekableByteChannel.html#truncate-long-)  – Truncates the file (or other entity) connected to the channel

[Reading and Writing Files With Channel I/O](https://docs.oracle.com/javase/tutorial/essential/io/file.html#channelio)  shows that the  `Path.newByteChannel`  methods return an instance of a  `SeekableByteChannel`. On the default file system, you can use that channel as is, or you can cast it to a  [`FileChannel`](https://docs.oracle.com/javase/8/docs/api/java/nio/channels/FileChannel.html)  giving you access to more advanced features, such as mapping a region of the file directly into memory for faster access, locking a region of the file, or reading and writing bytes from an absolute location without affecting the channel's current position.

The following code snippet opens a file for both reading and writing by using one of the  `newByteChannel`  methods. The  `SeekableByteChannel`  that is returned is cast to a  `FileChannel`. Then, 12 bytes are read from the beginning of the file, and the string "I was here!" is written at that location. The current position in the file is moved to the end, and the 12 bytes from the beginning are appended. Finally, the string, "I was here!" is appended, and the channel on the file is closed.

```java
String s = "I was here!\n";
byte data[] = s.getBytes();
ByteBuffer out = ByteBuffer.wrap(data);

ByteBuffer copy = ByteBuffer.allocate(12);

try (FileChannel fc = (FileChannel.open(file, READ, WRITE))) {
    // Read the first 12
    // bytes of the file.
    int nread;
    do {
        nread = fc.read(copy);
    } while (nread != -1 && copy.hasRemaining());

    // Write "I was here!" at the beginning of the file.
    fc.position(0);
    while (out.hasRemaining())
        fc.write(out);
    out.rewind();

    // Move to the end of the file.  Copy the first 12 bytes to
    // the end of the file.  Then write "I was here!" again.
    long length = fc.size();
    fc.position(length-1);
    copy.flip();
    while (copy.hasRemaining())
        fc.write(copy);
    while (out.hasRemaining())
        fc.write(out);
} catch (IOException x) {
    System.out.println("I/O Exception: " + x);
}
```
# Creating and Reading Directories

Some of the methods previously discussed, such as  `delete`, work on files, links  _and_  directories. But how do you list all the directories at the top of a file system? How do you list the contents of a directory or create a directory?

This section covers the following functionality specific to directories:

-   [Listing a File System's Root Directories](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html#listall)
-   [Creating a Directory](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html#create)
-   [Creating a Temporary Directory](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html#createTemp)
-   [Listing a Directory's Contents](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html#listdir)
-   [Filtering a Directory Listing By Using Globbing](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html#glob)
-   [Writing Your Own Directory Filter](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html#filter)

## Listing a File System's Root Directories

You can list all the root directories for a file system by using the  [`FileSystem.getRootDirectories`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getRootDirectories--)  method. This method returns an  `Iterable`, which enables you to use the  [enhanced for](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html)  statement to iterate over all the root directories.

The following code snippet prints the root directories for the default file system:

Iterable<Path> dirs = FileSystems.getDefault().getRootDirectories();
for (Path name: dirs) {
    System.err.println(name);
}

## Creating a Directory

You can create a new directory by using the  [`createDirectory(Path, FileAttribute<?>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createDirectory-java.nio.file.Path-java.nio.file.attribute.FileAttribute...-)  method. If you don't specify any  `FileAttributes`, the new directory will have default attributes. For example:

Path dir = ...;
Files.createDirectory(path);

The following code snippet creates a new directory on a POSIX file system that has specific permissions:

Set<PosixFilePermission> perms =
    PosixFilePermissions.fromString("rwxr-x---");
FileAttribute<Set<PosixFilePermission>> attr =
    PosixFilePermissions.asFileAttribute(perms);
Files.createDirectory(file, attr);

To create a directory several levels deep when one or more of the parent directories might not yet exist, you can use the convenience method,  [`createDirectories(Path, FileAttribute<?>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createDirectories-java.nio.file.Path-java.nio.file.attribute.FileAttribute...-). As with the  `createDirectory(Path, FileAttribute<?>)`  method, you can specify an optional set of initial file attributes. The following code snippet uses default attributes:

Files.createDirectories(Paths.get("foo/bar/test"));

The directories are created, as needed, from the top down. In the  `foo/bar/test`  example, if the  `foo`  directory does not exist, it is created. Next, the  `bar`  directory is created, if needed, and, finally, the  `test`  directory is created.

It is possible for this method to fail after creating some, but not all, of the parent directories.

## Creating a Temporary Directory

You can create a temporary directory using one of  `createTempDirectory`  methods:

-   [`createTempDirectory(Path, String, FileAttribute<?>...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createTempDirectory-java.nio.file.Path-java.lang.String-java.nio.file.attribute.FileAttribute...-)
-   [`createTempDirectory(String, FileAttribute<?>...)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#createTempDirectory-java.lang.String-java.nio.file.attribute.FileAttribute...-)

The first method allows the code to specify a location for the temporary directory and the second method creates a new directory in the default temporary-fle directory.

## Listing a Directory's Contents

You can list all the contents of a directory by using the  [`newDirectoryStream(Path)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newDirectoryStream-java.nio.file.Path-)  method. This method returns an object that implements the  [`DirectoryStream`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/DirectoryStream.html)  interface. The class that implements the  `DirectoryStream`  interface also implements  `Iterable`, so you can iterate through the directory stream, reading all of the objects. This approach scales well to very large directories.

----------

**Remember:** The returned  `DirectoryStream`  is a  _stream_. If you are not using a  `try-`with-resources statement, don't forget to close the stream in the  `finally`  block. The  `try-`with-resources statement takes care of this for you.

----------

The following code snippet shows how to print the contents of a directory:
```java
Path dir = ...;
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir)) {
    for (Path file: stream) {
        System.out.println(file.getFileName());
    }
} catch (IOException | DirectoryIteratorException x) {
    // IOException can never be thrown by the iteration.
    // In this snippet, it can only be thrown by newDirectoryStream.
    System.err.println(x);
}
```
The  `Path`  objects returned by the iterator are the names of the entries resolved against the directory. So, if you are listing the contents of the  `/tmp`  directory, the entries are returned with the form  `/tmp/a`,  `/tmp/b`, and so on.

This method returns the entire contents of a directory: files, links, subdirectories, and hidden files. If you want to be more selective about the contents that are retrieved, you can use one of the other  `newDirectoryStream`  methods, as described later in this page.

Note that if there is an exception during directory iteration then  `DirectoryIteratorException`  is thrown with the  `IOException`  as the cause. Iterator methods cannot throw exception exceptions.

## Filtering a Directory Listing By Using Globbing

If you want to fetch only files and subdirectories where each name matches a particular pattern, you can do so by using the  [`newDirectoryStream(Path, String)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newDirectoryStream-java.nio.file.Path-java.lang.String-)  method, which provides a built-in glob filter. If you are not familiar with glob syntax, see  [What Is a Glob?](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob)

For example, the following code snippet lists files relating to Java:  _.class_,  _.java_, and  _.jar_  files.:
```java
Path dir = ...;
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir, "*.{java,class,jar}")) {
    for (Path entry: stream) {
        System.out.println(entry.getFileName());
    }
} catch (IOException x) {
    // IOException can never be thrown by the iteration.
    // In this snippet, it can // only be thrown by newDirectoryStream.
    System.err.println(x);
}
```
## Writing Your Own Directory Filter

Perhaps you want to filter the contents of a directory based on some condition other than pattern matching. You can create your own filter by implementing the  [`DirectoryStream.Filter<T>`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/DirectoryStream.Filter.html)  interface. This interface consists of one method,  `accept`, which determines whether a file fulfills the search requirement.

For example, the following code snippet implements a filter that retrieves only directories:
```java
DirectoryStream.Filter<Path> filter = newDirectoryStream.Filter<Path>() {
    public boolean accept(Path file) throws IOException {
        try {
            return (Files.isDirectory(path));
        } catch (IOException x) {
            // Failed to determine if it's a directory.
            System.err.println(x);
            return false;
        }
    }
};
```
Once the filter has been created, it can be invoked by using the  [`newDirectoryStream(Path, DirectoryStream.Filter<? super Path>)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#newDirectoryStream-java.nio.file.Path-java.nio.file.DirectoryStream.Filter-)  method. The following code snippet uses the  `isDirectory`  filter to print only the directory's subdirectories to standard output:
```java
Path dir = ...;
try (DirectoryStream<Path> stream = Files.newDirectoryStream(dir, filter)) {
    for (Path entry: stream) {
        System.out.println(entry.getFileName());
    }
} catch (IOException x) {
    System.err.println(x);
}
```
This method is used to filter a single directory only. However, if you want to find all the subdirectories in a file tree, you would use the mechanism for  [Walking the File Tree](https://docs.oracle.com/javase/tutorial/essential/io/walk.html).


# Walking the File Tree

Do you need to create an application that will recursively visit all the files in a file tree? Perhaps you need to delete every  `.class`  file in a tree, or find every file that hasn't been accessed in the last year. You can do so with the  [`FileVisitor`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html)  interface.

This section covers the following:

-   [The FileVisitor Interface](https://docs.oracle.com/javase/tutorial/essential/io/walk.html#filevisitor)
-   [Kickstarting the Process](https://docs.oracle.com/javase/tutorial/essential/io/walk.html#invoke)
-   [Considerations When Creating a FileVisitor](https://docs.oracle.com/javase/tutorial/essential/io/walk.html#order)
-   [Controlling the Flow](https://docs.oracle.com/javase/tutorial/essential/io/walk.html#return)
-   [Examples](https://docs.oracle.com/javase/tutorial/essential/io/walk.html#ex)

## The FileVisitor Interface

To walk a file tree, you first need to implement a  `FileVisitor`. A  `FileVisitor`  specifies the required behavior at key points in the traversal process: when a file is visited, before a directory is accessed, after a directory is accessed, or when a failure occurs. The interface has four methods that correspond to these situations:

-   [`preVisitDirectory`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#preVisitDirectory-T-java.nio.file.attribute.BasicFileAttributes-)  – Invoked before a directory's entries are visited.
-   [`postVisitDirectory`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#postVisitDirectory-T-java.io.IOException-)  – Invoked after all the entries in a directory are visited. If any errors are encountered, the specific exception is passed to the method.
-   [`visitFile`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#visitFile-T-java.nio.file.attribute.BasicFileAttributes-)  – Invoked on the file being visited. The file's  `BasicFileAttributes`  is passed to the method, or you can use the  [file attributes](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)  package to read a specific set of attributes. For example, you can choose to read the file's  `DosFileAttributeView`  to determine if the file has the "hidden" bit set.
-   [`visitFileFailed`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitor.html#visitFileFailedy-T-java.io.IOException-)  – Invoked when the file cannot be accessed. The specific exception is passed to the method. You can choose whether to throw the exception, print it to the console or a log file, and so on.

If you don't need to implement all four of the  `FileVisitor`  methods, instead of implementing the  `FileVisitor`  interface, you can extend the  [`SimpleFileVisitor`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/SimpleFileVisitor.html)  class. This class, which implements the  `FileVisitor`  interface, visits all files in a tree and throws an  `IOError`  when an error is encountered. You can extend this class and override only the methods that you require.

Here is an example that extends  `SimpleFileVisitor`  to print all entries in a file tree. It prints the entry whether the entry is a regular file, a symbolic link, a directory, or some other "unspecified" type of file. It also prints the size, in bytes, of each file. Any exception that is encountered is printed to the console.

The  `FileVisitor`  methods are shown in bold:
```java
import static java.nio.file.FileVisitResult.*;

public static class PrintFiles
    extends SimpleFileVisitor<Path> {

    // Print information about
    // each type of file.
    @Override
    public FileVisitResult visitFile(Path file,BasicFileAttributes attr) {
        if (attr.isSymbolicLink()) {
            System.out.format("Symbolic link: %s ", file);
        } else if (attr.isRegularFile()) {
            System.out.format("Regular file: %s ", file);
        } else {
            System.out.format("Other: %s ", file);
        }
        System.out.println("(" + attr.size() + "bytes)");
        return CONTINUE;
    }

    // Print each directory visited.
    @Override
    public FileVisitResult postVisitDirectory(Path dir, IOException exc) {
        System.out.format("Directory: %s%n", dir);
        return CONTINUE;
    }

    // If there is some error accessing
    // the file, let the user know.
    // If you don't override this method
    // and an error occurs, an IOException 
    // is thrown.
    @Override
    public FileVisitResult visitFileFailed(Path file, IOException exc) {
        System.err.println(exc);
        return CONTINUE;
    }
}
```

## Kickstarting the Process

Once you have implemented your  `FileVisitor`, how do you initiate the file walk? There are two  `walkFileTree`  methods in the  `Files`  class.

-   [`walkFileTree(Path, FileVisitor)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#walkFileTree-java.nio.file.Path-java.nio.file.FileVisitor-)
-   [`walkFileTree(Path, Set<FileVisitOption>, int, FileVisitor)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Files.html#walkFileTree-java.nio.file.Path-java.util.Set-int-java.nio.file.FileVisitor-)

The first method requires only a starting point and an instance of your  `FileVisitor`. You can invoke the  `PrintFiles`  file visitor as follows:
```java
Path startingDir = ...;
PrintFiles pf = new PrintFiles();
Files.walkFileTree(startingDir, pf);
```

The second  `walkFileTree`  method enables you to additionally specify a limit on the number of levels visited and a set of  [`FileVisitOption`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitOption.html)  enums. If you want to ensure that this method walks the entire file tree, you can specify  `Integer.MAX_VALUE`  for the maximum depth argument.

You can specify the  `FileVisitOption`  enum,  `FOLLOW_LINKS`, which indicates that symbolic links should be followed.

This code snippet shows how the four-argument method can be invoked:

import static java.nio.file.FileVisitResult.*;

Path startingDir = ...;

EnumSet<FileVisitOption> opts = EnumSet.of(FOLLOW_LINKS);

Finder finder = new Finder(pattern);
Files.walkFileTree(startingDir, opts, Integer.MAX_VALUE, finder);

## Considerations When Creating a FileVisitor

A file tree is walked depth first, but you cannot make any assumptions about the iteration order that subdirectories are visited.

If your program will be changing the file system, you need to carefully consider how you implement your  `FileVisitor`.

For example, if you are writing a recursive delete, you first delete the files in a directory before deleting the directory itself. In this case, you delete the directory in  `postVisitDirectory`.

If you are writing a recursive copy, you create the new directory in  `preVisitDirectory`  before attempting to copy the files to it (in  `visitFiles`). If you want to preserve the attributes of the source directory (similar to the UNIX  `cp -p`  command), you need to do that  _after_  the files have been copied, in  `postVisitDirectory`. The  [ `Copy` ](https://docs.oracle.com/javase/tutorial/essential/io/examples/Copy.java)  example shows how to do this.

If you are writing a file search, you perform the comparison in the  `visitFile`  method. This method finds all the files that match your criteria, but it does not find the directories. If you want to find both files and directories, you must also perform the comparison in either the  `preVisitDirectory`  or  `postVisitDirectory`  method. The  [ `Find` ](https://docs.oracle.com/javase/tutorial/essential/io/examples/Find.java)  example shows how to do this.

You need to decide whether you want symbolic links to be followed. If you are deleting files, for example, following symbolic links might not be advisable. If you are copying a file tree, you might want to allow it. By default,  `walkFileTree`  does not follow symbolic links.

The  `visitFile`  method is invoked for files. If you have specified the  `FOLLOW_LINKS`  option and your file tree has a circular link to a parent directory, the looping directory is reported in the  `visitFileFailed`  method with the  `FileSystemLoopException`. The following code snippet shows how to catch a circular link and is from the  [ `Copy` ](https://docs.oracle.com/javase/tutorial/essential/io/examples/Copy.java)  example:

```java
@Override
public FileVisitResult
    visitFileFailed(Path file,
        IOException exc) {
    if (exc instanceof FileSystemLoopException) {
        System.err.println("cycle detected: " + file);
    } else {
        System.err.format("Unable to copy:" + " %s: %s%n", file, exc);
    }
    return CONTINUE;
}
```

This case can occur only when the program is following symbolic links.

## Controlling the Flow

Perhaps you want to walk the file tree looking for a particular directory and, when found, you want the process to terminate. Perhaps you want to skip specific directories.

The  `FileVisitor`  methods return a  [`FileVisitResult`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileVisitResult.html)  value. You can abort the file walking process or control whether a directory is visited by the values you return in the  `FileVisitor`  methods:

-   `CONTINUE`  – Indicates that the file walking should continue. If the  `preVisitDirectory`  method returns  `CONTINUE`, the directory is visited.
-   `TERMINATE`  – Immediately aborts the file walking. No further file walking methods are invoked after this value is returned.
-   `SKIP_SUBTREE`  – When  `preVisitDirectory`  returns this value, the specified directory and its subdirectories are skipped. This branch is "pruned out" of the tree.
-   `SKIP_SIBLINGS`  – When  `preVisitDirectory`  returns this value, the specified directory is not visited,  `postVisitDirectory`  is not invoked, and no further unvisited siblings are visited. If returned from the  `postVisitDirectory`  method, no further siblings are visited. Essentially, nothing further happens in the specified directory.

In this code snippet, any directory named  `SCCS`  is skipped:
```java
import static java.nio.file.FileVisitResult.*;

public FileVisitResult preVisitDirectory(Path dir,BasicFileAttributes attrs) {
    (if (dir.getFileName().toString().equals("SCCS")) {
         return SKIP_SUBTREE;
    }
    return CONTINUE;
}
```
In this code snippet, as soon as a particular file is located, the file name is printed to standard output, and the file walking terminates:
```java
import static java.nio.file.FileVisitResult.*;

// The file we are looking for.
Path lookingFor = ...;

public FileVisitResult
    visitFile(Path file,
        BasicFileAttributes attr) {
    if (file.getFileName().equals(lookingFor)) {
        System.out.println("Located file: " + file);
        return TERMINATE;
    }
    return CONTINUE;
}
```
## Examples

The following examples demonstrate the file walking mechanism:

-   [ `Find` ](https://docs.oracle.com/javase/tutorial/essential/io/examples/Find.java)  – Recurses a file tree looking for files and directories that match a particular glob pattern. This example is discussed in  [Finding Files](https://docs.oracle.com/javase/tutorial/essential/io/find.html).
-   [ `Chmod` ](https://docs.oracle.com/javase/tutorial/essential/io/examples/Chmod.java)  – Recursively changes permissions on a file tree (for POSIX systems only).
-   [ `Copy` ](https://docs.oracle.com/javase/tutorial/essential/io/examples/Copy.java)  – Recursively copies a file tree.
-   [ `WatchDir` ](https://docs.oracle.com/javase/tutorial/essential/io/examples/WatchDir.java)  – Demonstrates the mechanism that watches a directory for files that have been created, deleted or modified. Calling this program with the  `-r`  option watches an entire tree for changes. For more information about the file notification service, see  [Watching a Directory for Changes](https://docs.oracle.com/javase/tutorial/essential/io/notification.html).

# Finding Files

If you have ever used a shell script, you have most likely used pattern matching to locate files. In fact, you have probably used it extensively. If you haven't used it, pattern matching uses special characters to create a pattern and then file names can be compared against that pattern. For example, in most shell scripts, the asterisk,  `*`, matches any number of characters. For example, the following command lists all the files in the current directory that end in  `.html`:
```bash
% ls *.html
```

The  `java.nio.file`  package provides programmatic support for this useful feature. Each file system implementation provides a  [`PathMatcher`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/PathMatcher.html). You can retrieve a file system's  `PathMatcher`  by using the  [`getPathMatcher(String)`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/FileSystem.html#getPathMatcher-java.lang.String-)  method in the  `FileSystem`  class. The following code snippet fetches the path matcher for the default file system:
```java
String pattern = ...;
PathMatcher matcher =
    FileSystems.getDefault().getPathMatcher("glob:" + pattern);
```
The string argument passed to  `getPathMatcher`  specifies the syntax flavor and the pattern to be matched. This example specifies  _glob_  syntax. If you are unfamiliar with glob syntax, see  [What is a Glob](https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob).

Glob syntax is easy to use and flexible but, if you prefer, you can also use regular expressions, or  _regex_, syntax. For further information about regex, see the  [Regular Expressions](https://docs.oracle.com/javase/tutorial/essential/regex/index.html)  lesson. Some file system implementations might support other syntaxes.

If you want to use some other form of string-based pattern matching, you can create your own  `PathMatcher`  class. The examples in this page use glob syntax.

Once you have created your  `PathMatcher`  instance, you are ready to match files against it. The  `PathMatcher`  interface has a single method,  [`matches`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/PathMatcher.html#matches-java.nio.file.Path-), that takes a  `Path`  argument and returns a boolean: It either matches the pattern, or it does not. The following code snippet looks for files that end in  `.java`  or  `.class`  and prints those files to standard output:
```java
PathMatcher matcher = FileSystems.getDefault().getPathMatcher("glob:*.{java,class}");
Path filename = ...;
if (matcher.matches(filename)) {
    System.out.println(filename);
}
```

## Recursive Pattern Matching

Searching for files that match a particular pattern goes hand-in-hand with walking a file tree. How many times do you know a file is  _somewhere_  on the file system, but where? Or perhaps you need to find all files in a file tree that have a particular file extension.

The  [`Find`](https://docs.oracle.com/javase/tutorial/essential/io/examples/Find.java)  example does precisely that.  `Find`  is similar to the UNIX  `find`  utility, but has pared down functionally. You can extend this example to include other functionality. For example, the  `find`  utility supports the  `-prune`  flag to exclude an entire subtree from the search. You could implement that functionality by returning  `SKIP_SUBTREE`  in the  `preVisitDirectory`  method. To implement the  `-L`  option, which follows symbolic links, you could use the four-argument  `walkFileTree`  method and pass in the  `FOLLOW_LINKS`  enum (but make sure that you test for circular links in the  `visitFile`  method).

To run the Find application, use the following format:
```bash
% java Find <path> -name "<glob_pattern>"
```
The pattern is placed inside quotation marks so any wildcards are not interpreted by the shell. For example:
```bash
% java Find . -name "*.html"
```
Here is the source code for the  `Find`  example:
```java
/**
 * Sample code that finds files that match the specified glob pattern.
 * For more information on what constitutes a glob pattern, see
 * https://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob
 *
 * The file or directories that match the pattern are printed to
 * standard out.  The number of matches is also printed.
 *
 * When executing this application, you must put the glob pattern
 * in quotes, so the shell will not expand any wild cards:
 *              java Find . -name "*.java"
 */

import java.io.*;
import java.nio.file.*;
import java.nio.file.attribute.*;
import static java.nio.file.FileVisitResult.*;
import static java.nio.file.FileVisitOption.*;
import java.util.*;


public class Find {

    public static class Finder
        extends SimpleFileVisitor<Path> {

        private final PathMatcher matcher;
        private int numMatches = 0;

        Finder(String pattern) {
            matcher = FileSystems.getDefault()
                    .getPathMatcher("glob:" + pattern);
        }

        // Compares the glob pattern against
        // the file or directory name.
        void find(Path file) {
            Path name = file.getFileName();
            if (name != null && matcher.matches(name)) {
                numMatches++;
                System.out.println(file);
            }
        }

        // Prints the total number of
        // matches to standard out.
        void done() {
            System.out.println("Matched: "
                + numMatches);
        }

        // Invoke the pattern matching
        // method on each file.
        @Override
        public FileVisitResult visitFile(Path file,
                BasicFileAttributes attrs) {
            find(file);
            return CONTINUE;
        }

        // Invoke the pattern matching
        // method on each directory.
        @Override
        public FileVisitResult preVisitDirectory(Path dir,
                BasicFileAttributes attrs) {
            find(dir);
            return CONTINUE;
        }

        @Override
        public FileVisitResult visitFileFailed(Path file,
                IOException exc) {
            System.err.println(exc);
            return CONTINUE;
        }
    }

    static void usage() {
        System.err.println("java Find <path>" +
            " -name \"<glob_pattern>\"");
        System.exit(-1);
    }

    public static void main(String[] args)
        throws IOException {

        if (args.length < 3 || !args[1].equals("-name"))
            usage();

        Path startingDir = Paths.get(args[0]);
        String pattern = args[2];

        Finder finder = new Finder(pattern);
        Files.walkFileTree(startingDir, finder);
        finder.done();
    }
}
```

# Legacy File I/O Code

## Interoperability With Legacy Code

Prior to the Java SE 7 release, the  `java.io.File`  class was the mechanism used for file I/O, but it had several drawbacks.

-   Many methods didn't throw exceptions when they failed, so it was impossible to obtain a useful error message. For example, if a file deletion failed, the program would receive a "delete fail" but wouldn't know if it was because the file didn't exist, the user didn't have permissions, or there was some other problem.
-   The  `rename`  method didn't work consistently across platforms.
-   There was no real support for symbolic links.
-   More support for metadata was desired, such as file permissions, file owner, and other security attributes.
-   Accessing file metadata was inefficient.
-   Many of the  `File`  methods didn't scale. Requesting a large directory listing over a server could result in a hang. Large directories could also cause memory resource problems, resulting in a denial of service.
-   It was not possible to write reliable code that could recursively walk a file tree and respond appropriately if there were circular symbolic links.

Perhaps you have legacy code that uses  `java.io.File`  and would like to take advantage of the  `java.nio.file.Path`  functionality with minimal impact to your code.

The  `java.io.File`  class provides the  [`toPath`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#toPath--)  method, which converts an old style  `File`  instance to a  `java.nio.file.Path`  instance, as follows:

Path input = file.toPath();

You can then take advantage of the rich feature set available to the  `Path`  class.

For example, assume you had some code that deleted a file:

file.delete();

You could modify this code to use the  `Files.delete`  method, as follows:

Path fp = file.toPath();
Files.delete(fp);

Conversely, the  [`Path.toFile`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html#toFile--)  method constructs a  `java.io.File`  object for a  `Path`  object.

## Mapping java.io.File Functionality to java.nio.file

Because the Java implementation of file I/O has been completely re-architected in the Java SE 7 release, you cannot swap one method for another method. If you want to use the rich functionality offered by the  `java.nio.file`  package, your easiest solution is to use the  [`File.toPath`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#toPath--)  method as suggested in the previous section. However, if you do not want to use that approach or it is not sufficient for your needs, you must rewrite your file I/O code.

There is no one-to-one correspondence between the two APIs, but the following table gives you a general idea of what functionality in the  `java.io.File`  API maps to in the  `java.nio.file`  API and tells you where you can obtain more information.

| java.io.File Funcionality | java.nio.file Funcionality | Tutorial coverage |
|--|--|--|
| java.io.File | java.nio.file.Path |[The Path Class](https://docs.oracle.com/javase/tutorial/essential/io/pathClass.html)|
| java.io.RandomAccessFile | The `SeekableByteChannel` functionality. |[Random Access Files](https://docs.oracle.com/javase/tutorial/essential/io/rafs.html)|
| `File.canRead`, `canWrite`, `canExecute` | `Files.isReadable`, `Files.isWritable`, and `Files.isExecutable` On UNIX file systems, the [Managing Metadata (File and File Store Attributes)](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html) package is used to check the nine file permissions.| [Random Access Files](https://docs.oracle.com/javase/tutorial/essential/io/rafs.html)|
`File.isDirectory()`, `File.isFile()`, and `File.length()`|`Files.isDirectory(Path, LinkOption...)`, `Files.isRegularFile(Path, LinkOption...)`, and `Files.size(Path)`|[Managing Metadata](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)|
|`File.lastModified()` and `File.setLastModified(long)`| `Files.getLastModifiedTime(Path, LinkOption...)` and `Files.setLastMOdifiedTime(Path, FileTime)`|[Managing Metadata](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)|
|The `File` methods that set various attributes: `setExecutable`, `setReadable`, `setReadOnly`, `setWritable`|These methods are replaced by the `Files` method `setAttribute(Path, String, Object, LinkOption...)`.|[Managing Metadata](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html)|
|new File(parent, "newfile")|parent.resolve("newfile")|[Path Operations](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html)|
|File.renameTo|Files.move|[Moving a File or Directory](https://docs.oracle.com/javase/tutorial/essential/io/move.html)
|File.delete|Files.delete|[Deleting a File or Directory](https://docs.oracle.com/javase/tutorial/essential/io/delete.html)|
|File.createNewFile|Files.createFile|[Creating Files](https://docs.oracle.com/javase/tutorial/essential/io/file.html#createFile)|
|File.deleteOnExit|Replaced by the `DELETE_ON_CLOSE` option specified in the `createFile` method.|[Creating Files](https://docs.oracle.com/javase/tutorial/essential/io/file.html#createFile)|
|File.createTempFile|Files.createTempFile(Path,String, FileSttributes<?>)||
|File.exists|Files.exists and Files.notExists|[Verifying the Existence of a File or Directory](https://docs.oracle.com/javase/tutorial/essential/io/check.html)|
|File.compareTo and equals|Path.compareTo and equals|[Comparing Two Paths](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#compare)|
|File.getAbsolutePath and getAbsoluteFile|Path.toAbsolutePath|[Converting a Path](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#convert)|
|File.toURI|Path.toURI|[Converting a Path](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#convert)|
|File.isHidden|Files.isHidden|[Retrieving Information About the Path](https://docs.oracle.com/javase/tutorial/essential/io/pathOps.html#info)|
|File.list and listFiles|Path.newDirectoryStream|[Listing a Directory's Contents](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html#listdir)|
|File.mkdir and mkdirs|Files.createDirectory|[Creating a Directory](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html#create)|
|File.listRoots|FileSystem.getRootDirectories|[Listing a File System's Root Directories](https://docs.oracle.com/javase/tutorial/essential/io/dirs.html#listall)|
|File.getTotalSpace, File.getFreeSpace, File.getUsableSpace|`FileStore.getTotalSpace`, `FileStore.getUnallocatedSpace`, `FileStore.getUsableSpace`, `FileStore.getTotalSpace`|[File Store Attributes](https://docs.oracle.com/javase/tutorial/essential/io/fileAttr.html#store)|

