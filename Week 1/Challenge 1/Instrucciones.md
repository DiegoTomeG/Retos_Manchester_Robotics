# Week 1: Mini challenge

## Descripción

La actividad consiste en generar dos nodos, de los cuales el primer nodo actuara como un simple generador de señales lo que tendra como objetivo generar una señal senoidal. El segundo nodo actuara como un proceso en el que se tomara la señal generada por el nodo anterior y lo va a modificar de tal forma que se tenga la señal procesada. Las señales tendran que trazarse con ayuda de la funcion "rqt_plot" donde se necesita visualizar la primera señal senoidal y la señal ya procesada.



* **Nodo Signal_generator:**
1. Cree un nuevo paquete llamado "courseworks" (std_msgs y rclpy)
2. Crea un nodo llamado "signal_generator" para generar un onda senoidal con respecto al tiempo, es decir, y = f(t) = sin(𝑡).
3. Publicar el resultado utilizando un ROS estándar Float32mensaje a un tema llamado “/señal”.
4. Publica la hora 𝑡 en otro tema llamado “/time” utilizando el mismo tipo de mensaje.
5. Utilice una frecuencia de 10 Hz para este nodo.
6. Imprima el resultado en el terminal usando rospy.loginfo



* **Nodo process:**
1. Diseñe un segundo nodo llamado “proceso” que se suscriba a “/señal” y Temas “/tiempo”.
2. Procese la señal recibida de alguna manera, ya sea aumentando o disminuyendo la amplitud, agregando un cambio de fase, etc. 
3. Utilice una frecuencia de 10 Hz (puede elegir diferentes frecuencias) para este nodo.
4. El resultado debe imprimirse en el terminal.
5. La señal resultante debe publicarse mediante el mensaje Float32 en un tema llamado “/proc_signal”

<p align="center"><img width="500" alt="capca" src="https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/118226099/13d2caab-b196-4356-b1ba-a7debc3076d3">


