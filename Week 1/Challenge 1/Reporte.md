# Mini Challenge 1

## Resumen

En este reporte se incluye el proceso para familiarizarse el funcionamiento de ROS2 (Robot Operative System). Se incluye un ejemplo que aborda el uso de nodos y tópicos para simular un ambiente de comunicación entre dispositivos. Concretamente, se han creado nodos; Por una parte, el primer nodo será para publicar una señal senoidal a través de un tópico, posteriormente, el siguiente nodo escuchará dicha señal y la utilizará como entrada para modificarla y para producir una nueva. La señal producida también será publicada. 

## Objetivos

Experimentar y familiarizarse con el ambiente de ROS2 para crear un entorno de comunicación, aplicando por primera ocasión conceptos como nodos y tópicos. 

## Introducción

Este trabajo aborda la comunicación básica en ROS 2, lo cual es fundamental para comprender su funcionamiento y conceptos clave. El Sistema Operativo Robótico (ROS) es una plataforma de código abierto diseñada para facilitar el desarrollo de aplicaciones de robótica, ofreciendo bibliotecas de software y herramientas que abstraen el hardware, controlan dispositivos de bajo nivel e implementan funciones comunes. Su objetivo es promover la reutilización de código en la investigación y desarrollo de robótica, utilizando un marco distribuido de procesos (nodos) que pueden diseñarse y acoplarse libremente en tiempo de ejecución.

 “Estos procesos se organizan en paquetes y pilas que pueden compartirse y distribuirse fácilmente. Además, ROS admite un sistema federado de repositorios de código que facilita la colaboración y la distribución. Este diseño permite decisiones independientes en el desarrollo y la implementación, pero puede combinarse con las herramientas de infraestructura proporcionadas por ROS.” (Dattalo, 2018)

La comunicación en ROS se basa en un modelo de intercambio de mensajes entre nodos. Los nodos son procesos individuales que realizan tareas específicas, como controlar sensores o ejecutar algoritmos. Estos nodos intercambian mensajes, que son estructuras de datos que contienen información relevante, como lecturas de sensores o comandos de control. Los nodos se conectan a través de tópicos, canales de comunicación unidireccionales donde un nodo puede publicar mensajes y otros nodos pueden suscribirse para recibirlos. Además, ROS permite la comunicación mediante servicios, que son llamadas de procedimiento remoto entre nodos, permitiendo que un nodo proporcione una funcionalidad específica que otros nodos pueden solicitar.

![image](https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/94876975/ece6b092-e39b-4a13-87ac-374091edf285)

Ilustración 1. Estructura básica ROS (Open Robotics. 2024)

El Sistema Operativo Robótico (ROS) ha sido clave en el progreso de la investigación robótica al ofrecer una plataforma modular y gratuita. A pesar de sus contribuciones, la primera versión, ROS 1, tenía limitaciones en características y adaptabilidad a entornos de producción. Con el lanzamiento de ROS 2, una versión completamente rediseñada, se han abordado estas deficiencias y se han introducido mejoras significativas para enfrentar los desafíos actuales en robótica.

ROS 2 ha sido desarrollado desde cero con una arquitectura más flexible y modular, lo que permite una mejor escalabilidad y adaptabilidad a diferentes entornos y requisitos específicos de aplicaciones robóticas. La implementación del estándar DDS (Data Distribution Service) mejora la comunicación entre los componentes del sistema, especialmente en entornos distribuidos y en tiempo real, aspecto crucial para aplicaciones robóticas avanzadas. En términos de seguridad, se han integrado características avanzadas, como el cifrado de comunicaciones y la autenticación de nodos, garantizando un intercambio seguro de información, vital para aplicaciones críticas.

![image](https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/94876975/78bbfcc8-d98b-46ad-9420-6e6082061438)

Ilustración 2. Estructura robusta ROS 2 (Macenski, S. 2022)

En el sistema de archivos de ROS, la organización incluye:
Nivel del sistema de archivos ROS: Estructura principal.
Metapaquetes: Grupos de paquetes relacionados, que pueden agruparse libremente.
Paquetes: Unidades básicas del software ROS, conteniendo nodos, bibliotecas, y archivos de configuración.
Manifiesto del paquete: Archivo que proporciona información sobre el paquete, como autor, licencia, y dependencias.
Mensajes (.msg): Tipo de información enviada entre procesos ROS, definida en archivos .msg dentro de la carpeta msg de un paquete.
Servicios (.srv): Interacciones de solicitud/respuesta entre procesos, definidas en archivos .srv dentro de la carpeta srv de un paquete.
Repositorios: Conjuntos de paquetes mantenidos mediante sistemas de control de versiones (VCS) como Git o Subversion, facilitando la publicación mediante herramientas como Bloom.

<p align="center">
  <img src="https://github.com/DiegoTomeG/Retos_Manchester_Robotics/assets/94876975/b2e63b37-bd9c-4b80-8ab5-900494994db8" width="500">
</p>

Ilustración 3. Niveles de archivos de sistema en ROS (Joseph, L. 2018)

## Solución del problema

Presentar la metodología realizada para lograr los objetivos del reto, así como la descripción de los elementos, funciones y seguimiento del código de programación implementado.

### Implementación del launch file

## Resultados

En este apartado se insertan las evidencias del funcionamiento del reto, agregando descripciones de lo que representa cada una de estas, así como un análisis de tales resultados, para saber si se consideran resultados satisfactorios o completos, basado en los objetivos de la actividad.

## Conclusiones

Son los logros alcanzados en el proceso de investigación realizado. Incluir un análisis del cumplimiento de los objetivos,

-¿Se lograron los objetivos? ¿Por qué?, 

-¿No se cumplieron completamente los objetivos? ¿A qué se debe?, 

-¿Cuál sería una posible solución o mejora a la metodología implementada?.

Incluir en la redacción de las conclusiones las posibles respuestas a estas cuestiones, con el fin de analizar la comprensión de los procesos involucrados para el cumplimiento de los objetivos.

## Bibliografía o referencias

En este apartado se anexan los elementos consultadas para el desarrollo del tema de investigación.
