**Escuela Colombiana de Ingeniería**
**Arquitecturas de Software - ARSW*****Taller – Reflexividad de los lenguajes.******Ejercicio individual o en parejas.***_NOTA 1: debe abrir una cuenta en GitLab o en BitBucket, crear un repositorio privado, compartirlo con su compañero (si lo tiene) y en el mismo ir llevando el avance del laboratorio. NO USE GITHUB, ya que dejaría sus avances públicos._

_NOTA 2: el uso de GIT NO ES OPCIONAL, el proyecto entregado debe mostrar evidencia de su uso con los históricos de los commits._
En este ejercicio va a desarrollar una herramienta que apoye el proceso de detección de cuellos de botella en aplicaciones (un tipo de ‘perfilamiento’). La herramienta, haciendo uso de las caractarísticas de reflexividad de Java, recibe como insumo el nombre de la clase que se quiere analizar, la instancia, y mide el tiempo de ejecución de sus métodos. Este podría verse como un ejemplo de 'metaprogramación', pues esta es una herramienta que usa como insumo otros programas.


**Para entregar en clase**: Parte 1.

**Para antes del próximo laboratorio**: Partes 2 y 3.

Nota: la entrega se debe hacer en un ZIP en moodle, teniendo cuidado que dicho ZIP contenga el reposotorio GIT (la carpeta .git), ya que en este ejercicio se tendrán dos partes en dos ramas diferentes. Para garantizar que se haga el zip, puede hacerlo desde una terminal (no olvidar el punto al final):

```
zip -r integrante1.integrante2.zip .
```

**Parte I.**

Clone el proyecto con GIT (no lo descargue directamente!) y ábralo con NetBeans. En esta primera verisión (que quedará en la rama 'master' de Git), para que una clase pueda ser 'perfilada' debe implementar la interfaz 'Perfilable':


```	/**	* @obj retornar los nombres de los métodos que se pueden perfilar en esta aplicación	* @return Un arreglo, sin elementos nulos, con los nombres de los métodos que se deben probar	*/	public String[] getTesteableMethodNames();	/**	* @obj retornar un arreglo con el mismo número de elementos de la propiedad 'testeableMethods',	*      donde cada posición corresponde al número de veces que se deben ejectuar los métodos indicados	*      en dicha propiedad (la correspondencia número de veces/método la da el índice)	*/	public int[] getTestCount();``` 
1.	Haga un programa que funcione por línea de comando, y que reciba como parámetro el nombre de la clase que se va a probar. Por ejemplo, para perfilar una clase  (que implemente la interfaz indicada), la herramienta se debe poder invocar como:```~shell>  mvn exec:java -Dexec.mainClass="edu.eci.pdsw.view.Perfilator"   -Dexec.args="paquete1.paquete2.ClaseAProbar"
```
Para hacer esto, utilice el API de Reflection de manera que se pueda:
1.	Verificar que la clase dada como parámetro implementa la interfaz indicada. En caso de que no, mostrar un mensaje de error.
2.	En caso de que sí implemente la interfaz, crear una instancia de la clase, e invocar los métodos implementados de dicha interfaz para identificar qué métodos se quieren probar de la clase.
3.	Ejecutar en la instancia antes creda los métodos indicados por 'getTesteableMethodNames', el número de veces indicado, midiendo el tiempo total que toma su ejecución.
4.	Al terminar, el programa debe imprimir los nombres de los métodos ejecutados, el número de veces cuya ejecución fue repetida, y el tiempo total tomado.5. Haga los ajustes que sean necesarios para que la clase ‘ClaseAProbar’ suministrada se pueda perfilar. Luego, realice la medición para la misma.**Parte II.**

Para este punto cree, en el repositorio GIT del proyecto, [una rama](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell) con nombre 'parte2', y cambie a dicha rama antes de hacer lo indicado (git checkout parte2).
La primera versión del perfilador tiene un defecto importante: obliga a agregar código no pertinente a las clases que se quieren perfilar, lo que les quita cohesión. Por esto, se quiere que desarrolle una segunda versión del perfilador (en la nueva rama) que use como metadatos anotaciones (usted decide si son anotaciones de clase o de método, y cómo usarlas, dependiendo de qué considere más práctico). 

En esta nueva versión, para poder perfilar una clase, la misma ya no debe implementar la interfaz ‘Perfilable’, sino utilizar de alguna manera la anotación creada. Tenga en cuenta que se deben conservar las mismas características de la primera versión: se puede elegir qué métodos se perfilarán, y cuantas veces se repetirá la ejecución de los mismos. Para esto, tenga en cuenta que [las anotaciones pueden tener propiedades opcionales y obligatorias](http://tutorials.jenkov.com/java-reflection/annotations.html).

Al termianar esta parte, haga commit usando como comentario "finalización parte II".


**Parte III.**

Se quiere que la nueva versión del perfilador permita, de manera opcional, definir quien va a recibir y procesar los datos obtenidos (tiempos de ejecución). Para esto se requiere que:

1. Defina una Nueva Anotación que se pueda usar a nivel de Clase, la cual sirva para demarcar cuando se quiere que un componente externo procese los datos del perfilamiento. Haga que la anotación tenga un parámetro en el que se le pueda asociar el nombre de la clase que manejará los datos del perfilamiento.

2. Modifique el programa para que identifique la presencia de la nueva anotación antes creada, y para que cuando esto ocurra, cree una instancia de la clase dada como parámetro y le envié a ésta los datos del perfilamiento (en lugar de imprimirlos por pantalla). Para hacer esto, ponga como requisito (generando error si no es así) que las clases dadas en el parámetro antes mencionado implementen la interfaz ProfilingOutputHandler. Al finalizar el perfilamiento, haciendo uso del manejador de datos externo, se debe imprimir la salida del método 'getReport' del mismo.

3. Implemente dos manejadores de datos de perfilamiento:
	1. Uno que genere como reporte el listado de los tiempos de perfilamiento, indicando el nombre, el tiempo, y ordenados de mayor a menor.
	2. Uno que sólo imprima el nombre del método más lento, incluyendo el tiempo del mismo.

Al termianar esta parte, haga commit usando como comentario "finalización parte III".