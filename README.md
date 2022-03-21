# AWS ElasticBeanstalk + EC2 + Github Action setup

Este documento describe brevemente los pasos necesarios para ejecutar tu aplicación (ya sea web, api o servicio) en la nube de [AWS](https://aws.amazon.com/console/). 

En un enfoque tradicional y simple AWS provee, entre sus muchos servicios, la posibilidad de ejecutar máquinas virtuales denominadas instancias [EC2](https://aws.amazon.com/ec2/) que no son más que computadoras 💻 como las nuestras pero en otro lugar. Para hacer uso de las mismas, deberemos seleccionar las características del hardware, configurar su acceso y finalmente conectarnos para instalar las aplicaciones correspondientes. Luego, manejaremos nuestras aplicaciones conectándonos a la misma para realizar modificaciones.

Sin embargo, AWS también provee un servicio [Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/), un servicio para el deployment y administración de aplicaciones que complementa estas instancias EC2 con configuraciones adicionales para la provicion de escalabilidad, seteo de variables de ambiente, configuración de la red, balanceo de carga, monitoreo, entre otros.

Para que el proceso de deployment sea aún más sencillo, proponemos incluir un Github Action :octocat: para el release de las nuevas versiones de nuestra aplicación, de forma tal que cada nuevo push a un determinado branch realizará la actualización correspondiente en Elastic Beanstalk y al mismo tiempo en la instancia EC2 que existe detrás.

Se detallan a continuación los pasos necesarios para dejar nuestra app lista en la nube de AWS: 🗒️

1. Acceder a la cuenta de AWS de Zircon [acá](https://670171959034.signin.aws.amazon.com/console). Solicitar usuario de AWS en caso de no tenerlo.
2. Dirigirse a Elastic Beanstalk a la sección de [aplicaciones](https://us-east-1.console.aws.amazon.com/elasticbeanstalk/home?region=us-east-1#/applications)
3. Crear una nueva aplicación para el proyecto y darle un nombre

![image](https://user-images.githubusercontent.com/4985062/159368665-acccb13e-1daa-4f9b-ba2f-5523311246cd.png)

4. [Crear un nuevo environment](https://us-east-1.console.aws.amazon.com/elasticbeanstalk/home?region=us-east-1#/newEnvironment) dentro de la nueva aplicación, cada environment deberá representar un stage de nuestro pipeline (ej: dev, uat, qa, prod). En la mayoría de los casos desearemos crear un environment del tipo Web Server.
5. Ingresar la información correspondiente en los diferentes campos:
    1. Nombre del ambiente. Ej: MyApp-Dev
    2. Plataforma, que indica el setup inicial para correr cierto tipo de aplicaciones. Ej: NodeJS (se recomienda siempre seleccionar la última versión disponible.
    3. Seleccionar "Sample application" para asegurarnos que todo funciona correctamente

6. Por defecto se asignará una instancia EC2 de la free tier para nuestra aplicación. Una vez creado el ambiente, visitar la sección de configuración donde se encontrará mayor detalle de esta y otras configuraciones. Por ejemplo la sección de "Software" nos permitirá añadir variables de entorno que estarán disponibles para nuestra aplicación. 
![image](https://user-images.githubusercontent.com/4985062/159371063-4c77d8bd-14b7-4266-9e4d-3af079aa6445.png)

7. Al guardar modificaciones en las variables de entorno, realizar actualizaciones del codigo de nuestra aplicacion u otros cambios, EB automáticamente se encargará de aplicar las configuraciones correspondientes sin necesidad de conectarnos directamente a las instancias. De hecho, esto no deberá ser necesario, salvo excepciones, ya que Beanstalk también se encarga del autoscaling de nuestra aplicación haciendo provisioning de sub-instancias para mantener un determinado nivel de performance. Esto quiere decir que las instancias que funcionan debajo seran constantemente reemplazadas por lo que no debemos depender de una configuracion especifica dentro de las mismas.

