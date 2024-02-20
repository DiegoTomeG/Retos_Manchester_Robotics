# Week 1: Mini challenge

**Descripción del challenge:**

La actividad consiste en generar dos nodos, de los cuales el primer nodo actuara como un simple generador de señales lo que tendra como objetivo generar una señal senoidal. El segundo nodo actuara como un proceso en el que se tomara la señal generada por el nodo anterior y lo va a modificar de tal forma que se tenga la señal procesada. Las señales tendran que trazarse con ayuda de la funcion "rqt_plot" donde se necesita visualizar la primera señal senoidal y la señal ya procesada.

**Nodo Signal_generator:**
1. Cree un nuevo paquete llamado "courseworks" (std_msgs y rclpy)
2. Crea un nodo llamado "signal_generator" para generar un onda senoidal con respecto al tiempo, es decir, y = f(t) = sin(𝑡).
3. Publicar el resultado utilizando un ROS estándar Float32mensaje a un tema llamado “/señal”.
4. Publica la hora 𝑡 en otro tema llamado “/time” utilizando el mismo tipo de mensaje.
5. Utilice una frecuencia de 10 Hz para este nodo.
6. Imprima el resultado en el terminal usando rospy.loginfo

**Nodo process:**
1. Diseñe un segundo nodo llamado “proceso” que se suscriba a “/señal” y Temas “/tiempo”.
2. Procese la señal recibida de la siguiente manera
Compensa la señal recibida (g 𝑡 = f(t) + 𝛼) de modo que quede positivo para siempre 𝑡 ≥ 0 
Reducir la amplitud de la señal recibida a la mitad.
Agregar un cambio de fase a la señal recibida (como parámetro de usuario o variable) a la señal original.
Para este ejercicio, este parámetro se puede codificar.
4. Utilice una frecuencia de 10 Hz (puede elegir diferentes frecuencias) para este nodo.
5. El resultado debe imprimirse en el terminal.
6. La señal resultante debe publicarse mediante el mensaje Float32 en un tema llamado “/proc_signal”


Resumen

Es la descripción breve de los elementos más notables del proceso de investigación llevado a cabo.

Objetivos

Describir el objetivo general y los objetivos particulares del reto.

Introducción

Es el espacio donde se presenta el tema de la investigación, así como cuestiones relevantes como el contexto. Todo ello desde una perspectiva general que ayude al lector a idear un mapa metal de lo que se desarrollará dentro del reporte. Incluir una descripción de qué es ROS, la estructura básica de funcionamiento y los principales agentes o elementos que integran su red de comunicación.

Solución del problema

Presentar la metodología realizada para lograr los objetivos del reto, así como la descripción de los elementos, funciones y seguimiento del código de programación implementado.

Resultados

En este apartado se insertan las evidencias del funcionamiento del reto, agregando descripciones de lo que representa cada una de estas, así como un análisis de tales resultados, para saber si se consideran resultados satisfactorios o completos, basado en los objetivos de la actividad.

Conclusiones

Son los logros alcanzados en el proceso de investigación realizado. Incluir un análisis del cumplimiento de los objetivos,

-¿Se lograron los objetivos? ¿Por qué?, 

-¿No se cumplieron completamente los objetivos? ¿A qué se debe?, 

-¿Cuál sería una posible solución o mejora a la metodología implementada?.

Incluir en la redacción de las conclusiones las posibles respuestas a estas cuestiones, con el fin de analizar la comprensión de los procesos involucrados para el cumplimiento de los objetivos.

Bibliografía o referencias

En este apartado se anexan los elementos consultadas para el desarrollo del tema de investigación.
