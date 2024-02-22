# Mini Challenge 1

## Resumen

En este reporte se incluye el proceso para familiarizarse con el funcionamiento de ROS2 (Robot Operative System) realizando un ejemplo que aborda el uso de nodos y tópicos para simular un ambiente de comunicación entre dispositivos. Concretamente, se han creado nodos; el primero se encarga de publicar una señal senoidal a través de un tópico, posteriormente, el siguiente escuchará dicha señal y la utilizará como entrada para modificarla y para producir una nueva. La señal producida también será publicada. Finalmente, ambas señales sarán graficadas simultáneamente para evidenciar los cambios de procesamiento. 

## Objetivos

 - Familiarizarse con el ambiente de ROS2 para crear un entorno de comunicación, aplicando por primera ocasión conceptos como nodos y tópicos. 
 - Identificar comandos básicos para llevar a cabo la construcción y manejo de paquetes dentro del sistema de ROS2. 

## Introducción

Este trabajo aborda la comunicación básica en ROS 2, lo cual es fundamental para comprender su funcionamiento y conceptos clave. El Sistema Operativo Robótico (ROS) es una plataforma de código abierto diseñada para facilitar el desarrollo de aplicaciones de robótica, ofreciendo bibliotecas de software y herramientas que abstraen el hardware, controlan dispositivos de bajo nivel e implementan funciones comunes. Su objetivo es promover la reutilización de código en la investigación y desarrollo de robótica, utilizando un marco distribuido de procesos (nodos) que pueden diseñarse y acoplarse libremente en tiempo de ejecución.

 “Estos procesos se organizan en paquetes y pilas que pueden compartirse y distribuirse fácilmente. Además, ROS admite un sistema federado de repositorios de código que facilita la colaboración y la distribución. Este diseño permite decisiones independientes en el desarrollo y la implementación, pero puede combinarse con las herramientas de infraestructura proporcionadas por ROS.” (Dattalo, 2018)

La comunicación en ROS se basa en un modelo de intercambio de mensajes entre nodos. Los nodos son procesos individuales que realizan tareas específicas, como controlar sensores o ejecutar algoritmos. Estos nodos intercambian mensajes, que son estructuras de datos que contienen información relevante, como lecturas de sensores o comandos de control. Los nodos se conectan a través de tópicos, canales de comunicación unidireccionales donde un nodo puede publicar mensajes y otros nodos pueden suscribirse para recibirlos. Además, ROS permite la comunicación mediante servicios, que son llamadas de procedimiento remoto entre nodos, permitiendo que un nodo proporcione una funcionalidad específica que otros nodos pueden solicitar.

<p align="center">
  <img src="https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/94876975/ece6b092-e39b-4a13-87ac-374091edf285" width="450"> 
</p>
<p align="center">
Ilustración 1. Estructura básica ROS (Open Robotics. 2024)
</p>

El Sistema Operativo Robótico (ROS) ha sido clave en el progreso de la investigación robótica al ofrecer una plataforma modular y gratuita. A pesar de sus contribuciones, la primera versión, ROS 1, tenía limitaciones en características y adaptabilidad a entornos de producción. Con el lanzamiento de ROS 2, una versión completamente rediseñada, se han abordado estas deficiencias y se han introducido mejoras significativas para enfrentar los desafíos actuales en robótica.

ROS 2 ha sido desarrollado desde cero con una arquitectura más flexible y modular, lo que permite una mejor escalabilidad y adaptabilidad a diferentes entornos y requisitos específicos de aplicaciones robóticas. La implementación del estándar DDS (Data Distribution Service) mejora la comunicación entre los componentes del sistema, especialmente en entornos distribuidos y en tiempo real, aspecto crucial para aplicaciones robóticas avanzadas. En términos de seguridad, se han integrado características avanzadas, como el cifrado de comunicaciones y la autenticación de nodos, garantizando un intercambio seguro de información, vital para aplicaciones críticas.

<p align="center">
  <img src="https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/94876975/78bbfcc8-d98b-46ad-9420-6e6082061438" width=450"> 
</p>
<p align="center">
Ilustración 2. Estructura robusta ROS 2 (Macenski, S. 2022)
</p>

En el sistema de archivos de ROS, la organización incluye:

- **Nivel del sistema de archivos ROS**: Estructura principal
- **Metapaquetes**: Grupos de paquetes relacionados, que pueden agruparse libremente.
- **Paquetes**: Unidades básicas del software ROS, conteniendo nodos, bibliotecas, y archivos de configuración.
- **Manifiesto del paquete**: Archivo que proporciona información sobre el paquete, como autor, licencia, y dependencias.
- **Mensajes (.msg)**: Tipo de información enviada entre procesos ROS, definida en archivos `.msg` dentro de la carpeta `msg` de un paquete.
- **Servicios (.srv)**: Interacciones de solicitud/respuesta entre procesos, definidas en archivos `.srv` dentro de la carpeta `srv` de un paquete.
- **Repositorios**: Conjuntos de paquetes mantenidos mediante sistemas de control de versiones (VCS) como Git o Subversion, facilitando la publicación mediante herramientas como Bloom.


<p align="center">
  <img src="https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/94876975/b2e63b37-bd9c-4b80-8ab5-900494994db8" width="350">
</p>

<p align="center"><small>Ilustración 3. Niveles de archivos de sistema en ROS (Joseph, L. 2018)</small></p>



## Solución del problema

### Configuración inicial 
Dadas las instrucciones proporcionadas, en primera instancia debe crearse un nuevo paquete llamado courseworks en donde se almacenarán los nodos correspondientes para procesar la señal senoidal. Por elección grupal y para evitar cualquier tipo de confusión en futuros proyectos, se ha creado una nueva carpeta fuente. 
```
$ mkdir challenge_1
$ cd challenge_1
$ mkdir src
$ cd src
```	
Con la nueva carpeta fuente disponible, se crea dentro un nuevo paquete. Además de agregar el nombre del primer nodo (signal_generator) y del paquete (courseworks), también deben incluirse las dependencias, que son atributos necesarios para compilar el programa. Para este caso se utilizan las dependencias _rclpy_ y _std_msgs_

```
$ ros2 pkg create --build-type ament_python --node-name signal_generator courseworks --dependencies rclpy std_msgs 
```
Una vez que se ha creado el espacio de trabajo, es conveniente construir el archivo ejecutable para verificar que funciona la distribución de archivos. Para ello se utilizará el comando _colcon_, que construye a la par un directorio _build_, _install_ y _log_ para la carpeta src.
```
$ colcon build  
```
Para agregar los elementos necesarios a las rutas de librerias y proporcionar comandos de shell o bash: 
```
$ source install/setup.bash  
```
Finalmente, para ejecutar el contenido dentro del nodo: 
```
$ ros2 run courseworks signal_generator 
```

### Creación de Nodos
#### Nodo signal_generator
Ahora es posible modificar el archivo de python del primer nodo, el cual corresponde a publicar la señal senoidal. Considerando la estructura abordada durante la sesión, el programa tomará las siguientes modificaciones. Como buena pŕactica, deben incluirse las dependencias al inicio del programa y para generar la señal, se utilizará también las librería _math_
```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import Float32
import math

```
Una vez construida la _Clase My_Publisher_ y, a diferencia del ejemplo de la clase, se publicará un tipo de dato Float32 para generar la señal. Después del constructor de inicialización, se asigna el nombre al nodo siendo _signal_generator_ y se agrega la construcción de dos tópicos signal y time, correspondientemente. El mensaje de publicación será desplegado a una frecuencia de 10 hz. 

```python
class My_Publisher(Node):
    def __init__(self):
        super().__init__('signal_generator')
        self.signal = self.create_publisher(Float32, 'signal', 10)
        self.time = self.create_publisher(Float32, 'time', 10)
        timer_period = 0.1  # 10 Hz
        self.timer = self.create_timer(timer_period, self.timer_callback)
        self.get_logger().info('Signal generator node successfully initialized!!')
	self.time_msg = Float32()
        self.signal_msg = Float32()

```
Posteriormente, se define un segundo método t_imer_callback_, el cual se encargará de publicar la señal. Utiliza un timer para obtener el tiempo real en segundos y crea un mensaje Float32 para signal y time en sus respectivos tópicos. Finalmente se imprime en la terminal el valor del _time_ y la función evaluada para cada instante de tiempo.

```python
   def timer_callback(self):
        t = self.get_clock().now().nanoseconds / 1e9
        self.time_msg.data = t
        self.signal_msg.data = math.sin(t)

        self.time.publish(self.time_msg)
        self.signal.publish(self.signal_msg)

        self.get_logger().info(f"Time: {self.time_msg.data}, Signal: {self.signal_msg.data}")

```

Una forma simple realizar un debugg para el proceso hasta este punto es el de graficar la señal de entrada del programa. Esto se obtiene mediante el comando: 

```
$ ros2 run rqt_plot rqt_plot
```
Nota: Para ejecutar el comando anterior con fines de debugg, sera necesario ejectuar previamente el nodo: ```ros2 run courseworks signal_generator```
#### Nodo Process

Como bien lo indica su nombre, este segundo nodo se encargará de procesar la señal obtenida al subscribirse al nodo anterior. Siguiendo la misma metodología, se crea un nuevo nodo como archivo de python: process.py. Una vez creado el nodo, este deberá modificarse para actuar como listener y talker simultáneamente para volver a publicar la señal ya alterada.

Nuevamente, se incluyen las librerías con dependencias junto con el tipo de dato FLoat32.

```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import Float32 
```
La estructura de la clase se mantiene igual; sin embargo, deben realizarse las modificaciones pertinentes para que el nuevo nodo actúe como susbcriber en lugar de publisher. Por ejemplo, el nombre de la clase, los métodos y objetos. En cuanto a los tópicos, debe crearse uno como publicador (señal procesada) y dos suscriptores que se encargarán de leer la señal de entrada.
```python
class My_Subscriber(Node):
    def __init__(self):
        super().__init__('process')
        self.subTime = self.create_subscription(Float32, '/time', self.time_callback, 10)
        self.subSignal = self.create_subscription(Float32, '/signal', self.signal_callback, 10)
        self.pub_proc_signal = self.create_publisher(Float32, 'proc_signal', 10)
        self.get_logger().info('Process subscribed to /time and /signal topics!!!')
        self.modified_signal_msg = Float32()

```
Lo siguiente es crear el segundo método para publicar la señal procesada. La función se ejecuta cada vez que un mensaje es recibido a través del tópico de _signal_. Se encarga de procesar la información de la señal ('msg.data'), la modifica y después la publica en el tópico de _proc_signal_. 

```python
    def signal_callback(self, signal_msg):
        self.get_logger().info(f"Received Signal: {signal_msg.data}")
        self.modified_signal_msg.data = signal_msg.data * 2  
        self.pub_proc_signal.publish(self.modified_signal_msg)
    
```

Para compilar este segundo nodo, se debe abrir otra terminal y ejecutar los comandos previos para llevar a cabo el proceso de construcción. Una vez construido y para verificar el funcionamiento adecuado, se debe volver a graficar la función. 
```
$ ros2 run rqt_plot rqt_plot
```













### Implementación del launch file

Hasta el momento, hemos comprobado ya el correcto funcionamiento de nuestro programa, obteniendo los resultados deseados. Sin embargo, resulta importante mencionar que en algunas ocasiones puede llegar a ser bastante tedioso el abrir una considerable cantidad de terminales para ejecutar cada uno de los nodos dentro del paquete, por lo que para hacer mucho más sencilla la ejecución de todos los nodos involucrados haremos uso de un launch file. 

Pero, ¿qué es un launch file? De acuerdo con The Robotics Back-End (2019), un launch file es un archivo que nos permite inicializar todos los nodos (o los necesarios para  el funcionamiento de nuestro programa) ejecutando únicamente un archivo. 
Para implementar un launch file a nuestro programa, utilizamos la información brindada por Manchester Robotics, en conjunto con información encontrada en algunos foros, etc. 

1. Crearemos una carpeta para el launch file dentro de nuestro paquete (Manchester Robotics, 2024): 
```
$ cd Challenge1/src/courseworks
$ mkdir launch
$ cd launch
```				

Una vez en la carpeta, crearemos nuestro archivo utilizando el comando touch y nos aseguramos de darle permisos de ejecución: 
```
$ touch plotter_launch.py
$ chmod +x plotter_launch.py
```
							
2. De igual forma, tenemos que agregar la siguiente línea al archivo package.xml: ```<exec_depend>ros2launch</exec_depend> ``` (Manchester Robotics, 2024).

``` python
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>courseworks</name>
  <version>0.0.0</version>
  <description>TODO: Package description</description>
  <maintainer email="pamelahdzmontero@todo.todo">pamelahdzmontero</maintainer>
  <license>TODO: License declaration</license>

  <depend>rclpy</depend>
  <depend>std_msgs</depend>
  <exec_depend>ros2launch</exec_depend> #Agregamos línea de código

  <test_depend>ament_copyright</test_depend>
  <test_depend>ament_flake8</test_depend>
  <test_depend>ament_pep257</test_depend>
  <test_depend>python3-pytest</test_depend>

  <export>
    <build_type>ament_python</build_type>
  </export>
</package>
```

3. Dentro del archivo setup.py, específicamente dentro de data_files, agregamos las siguientes líneas de código (Manchester Robotics, 2024): 
``` python
 (os.path.join('share', package_name, 'launch'),
glob(os.path.join('launch', '*launch.[pxy][yma]*')))
```

Por supuesto, es importante no olvidar importar las librerías correspondientes: 
``` python
import os
from glob import glob
```
Dicho esto, nuestro archivo setup.py deberá quedar de la siguiente manera: 
``` python
import os
from glob import glob
from setuptools import find_packages, setup

package_name = 'courseworks'

setup(
    name=package_name,
    version='0.0.0',
    packages=find_packages(exclude=['test']),
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
        (os.path.join('share', package_name, 'launch'),
         glob(os.path.join('launch', '*launch.[pxy][yma]*')))
    ],
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='pamelahdzmontero',
    maintainer_email='pamelahdzmontero@todo.todo',
    description='TODO: Package description',
    license='TODO: License declaration',
    tests_require=['pytest'],
    entry_points={
        'console_scripts': [
            'signal_generator = courseworks.signal_generator:main',
            'process = courseworks.process:main'
        ],
    },
)
```
4. Procedemos a agregar código al archivo plotter_launch.py. Para este caso, nuestro objetivo será crear un launch file que sea capaz de: 

* Ejecutar ambos nodos, signal_generator y process, en diferentes terminales, una para cada nodo. 
* Abrir un plotter utilizando rqt_plot, en donde se puedan observar ambas señales. 
* Como extra, el launch file deberá abrir una gráfica, utilizado rqt_graph, en donde se puedan visualizar todos los elementos activos, tanto nodos como tópicos.

Considerando esto, creamos el código de la siguiente manera, apoyándonos de la documentación oficial para [ROS2](https://docs.ros.org/en/foxy/Tutorials/Intermediate/Launch/Creating-Launch-Files.html)



Importamos las librerías correspondientes para el uso del launch file: 
``` python
from launch import LaunchDescription
from launch_ros.actions import Node
```
Declaramos la función generate_launch_description, la cual contendrá todos los nodos que deseamos inicializar: 
``` python
def generate_launch_description():
   return LaunchDescription([
       Node(
           package='courseworks',
           executable='signal_generator',
           output = 'screen',
           prefix='gnome-terminal --'
       ),
       Node(
           package='courseworks',
           executable='process',
           output = 'screen',
           prefix='gnome-terminal --'
       ),
       Node(
           package = 'rqt_graph',
           executable = 'rqt_graph',
           prefix='gnome-terminal --'
       ),
       Node(
           package = 'rqt_plot',
           executable = 'rqt_plot',
           arguments= ['/signal/data', '/proc_signal/data']  
       )
   ])
```

Tal y como se puede observar, dentro de esta función se agregan los nodos que se desean ejecutar, indicando el paquete y el ejecutable en donde se encuentran. 

* Observemos que tanto para el nodo signal_generator como para el nodo process agregamos otro atributo (prefix) con el valor gnome-terminal --. Este atributo, según Nadeem (2015), indicaría al sistema que dicho nodo se deberá ejecutar en una nueva terminal. 

* De igual forma, utilizando la función generate_launch_description, podemos ejecutar rqt_graph y rqt_plot, lo que nos permitirá visualizar gráficamente las señales activas. Nótese, de igual manera, que utilizando el prefijo ‘gnome-terminal --’ para el nodo rqt_graph, indicamos que lo ejecute desde una nueva terminal. 

* Finalmente, en el nodo encargado de ejecutar rqt_plot, agregamos el atributo arguments, que según Rodriguez (2015), nos permitirá indicar específicamente que señales deseamos graficar. 


Ahora bien, para verificar que nuestro launch file funcione correctamente, debemos ubicarnos en la carpeta Challenge y realizar la compilación de los paquetes: 
```
$ colcon build
$ source install/setup.bash
```
Finalmente, ha llegado la hora de probar el funcionamiento de nuestro launch file: 
```
$ ros2 launch courseworks plotter_launch.py
```

Véase los resultados en la siguiente sección. 
## Resultados

Como podemos observar a continuación primeramente vamos a ver nuestro diagrama, el cual correctamente se puede observar que los óvalos son los que se representan como nuestros nodos "signal_generator" y "process" además de que signal_generator tiene unas flechas apuntando a unos recuadros /time y /signal los cuales son los topic en el que se estará publicando la hora _t_ y, por otro lado, estará publicando valores de la señal senoidal utilizando un mensaje Float32 estándar de ROS. Justo el nodo de "process" subscribe estos dos topic, procesando la señal recibida y modificándola haciendo que publique la señal modificada en el topic /proc_signal

<p align="center">
  <img src="https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/118226099/e9e694eb-fc6c-4e41-bf80-0bfbf82aedc0" width=600"> 
</p>
<p align="center">

Lo siguiente que observamos son nuestras dos terminales ejecutándose el nodo "signal_generator" donde del lado izquierdo observamos como se van mostrando los datos del tiempo y también el dato de la señal senoidal que se están mandando. Del lado derecho tenemos ejecutándose el nodo "process" donde se observan los datos del tiempo que se recibe y también de la señal que ya es procesada, pero es recibida y se está publicando.

<p align="center">
  <img src="https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/118226099/6117d0de-de59-4794-8729-86efb8797001" width=800"> 
</p>
<p align="center">

Por consiguiente, gracias al comando de ```ros2 run rqt_plot rqt_plot``` podemos visualizar la siguiente gráfica, la línea azul es la señal que se está mandando por el topic /signal del nodo "signal_generator" y graficándose correctamente como una onda senoidal. Por otro lado, tenemos la línea roja que es la señal ya procesada por el topic /proc_signal del nodo "process" mandando la señal pero modificando los parámetros de la original.

<p align="center">
  <img src="https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/118226099/0beb1519-3d75-4b44-8386-24f71a904641" width=800"> 
</p>
<p align="center">

### Comprobación

Si desea comprobar los resultados obtenidos, puede descargar el archivo challenge_1.zip para poder verificarlo. Tendrá que ejecutar primeramente el comando de ```colcon build```, posteriormente ```source install/setup.bash``` y finalmente ```ros2 launch courseworks plotter_launch.py``` lo que le abrirá todas las ventanas que anteriormente ya hemos explicado en los resultados.

Es posible que a la hora de ejecutar la ventana de rqt_plot pueda salir vacía, pero puede agregar manualmente los topic en esta parte de la venta, donde pondremos el nombre de signal/data para poder mostrar la onda senoidal original y darles al botón de + también haremos lo mismo para proc_signal/data que mostrará la onda senoidal modificada y agregarla con el botón de +.

<p align="center">
  <img src="https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/118226099/d0da2708-1813-413b-91ec-ce216cfc2fb8" width=700"> 
</p>
<p align="center">



## Conclusiones

Durante el desarrollo del desafío, logramos cumplir satisfactoriamente los objetivos propuestos, nos familiarizamos de manera efectiva con el entorno de ROS2, lo que nos permitió utilizar las funciones de ROS2 desde la terminal con precisión. Además, logramos crear exitosamente los dos nodos, signal_generator y process, así como los tópicos /signal, /time y /proc_signal, los cuales funcionaron correctamente, como se demostró en la terminal. 

El uso de herramientas como rqt_graph y rqt_plot nos permite visualizar el esquema esperado de manera precisa, mostrando las conexiones entre los nodos y los tópicos que facilitan la transmisión de información. De igual forma observamos con claridad la generación de la onda senoidal en el tópico /signal y la señal procesada en /proc_signal, evidenciando los ajustes realizados en el nodo process para procesar la señal senoidal. 

En el transcurso del proyecto, enfrentamos algunos desafíos relacionados con la generación de señales erróneas, lo que resultó en conexiones incorrectas entre los nodos en nuestro diagrama. Sin embargo, tras ajustar ciertos requisitos y optimizar el proceso de compilación en ROS2, logramos resolver este problema de manera efectiva. Para mejorar la metodología implementada, sería recomendable establecer un proceso de prueba para detectar y corregir posibles errores en la generación de señales, esto podría incluir la implementación de pruebas automatizadas que validen la integridad de los datos transmitidos entre los nodos y los tópicos. Al crear este proceso nos permitiría mejorar la integridad y la confiabilidad de nuestro sistema, asegurando un rendimiento óptimo en entornos futuros, además de que la experiencia adquirida nos enseñó la importancia de la optimización de un proceso de compilación en ROS2 con ayuda de generar un archivo launch para una rápida ejecución de la solución.


## Bibliografía o referencias

Dattalo, A. (8 de Agosto del 2018). ¿Qué es ROS?. ROS wiki. Recuperado el 20 de Febrero de 2024 de http://wiki.ros.org/ROS/Introduction

Joseph, L., & Cacace, J. (2018). Mastering ROS for Robotics Programming: Design, build, and simulate complex robots using the Robot Operating System. Packt Publishing Ltd.

Macenski, S., Foote, T., Gerkey, B., Lalancette, C., & Woodall, W. (2022). Robot Operating System 2: Design, architecture, and uses in the wild. Science Robotics, 7(66), eabm6074.

Manchester Robotics. (2024). Robot Operating System - ROS [Diapositivas de PowerPoint]. https://github.com/ManchesterRoboticsLtd/TE3001B_Robotics_Foundation_2024/blob/main/Week1/Slides/2%20-%20MCR2_ROS_BASICS.pdf

Nadeem, H. [HassanNadeem]. (3 de Febrero, 2015). launching nodes in a new terminal with roslaunch [Publicación en un foro online]. Mensaje publicado en https://answers.ros.org/question/30754/launching-nodes-in-a-new-terminal-with-roslaunch/

Open Robotics. (2021). ¿Por qué ROS?. ROS. Recuperado el 20 de Febrero del 2024 de https://www.ros.org/blog/why-ros/

Open Robotics. (2024). Documentacion: Galactic.https://docs.ros.org/en/galactic/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html

Rodriguez, A. [Alfonso Rodriguez T]. (27 de Mayo, 2015). Save rqt_plot settings [Publicación en foro online]. Mensaje publicado en https://answers.ros.org/question/209964/save-rqt_plot-settings/

The Robotics Back-End. (2019, Febrero 4). What is a ROS launch file? The Robotics Back-End.https://roboticsbackend.com/what-is-a-ros-launch-file/







