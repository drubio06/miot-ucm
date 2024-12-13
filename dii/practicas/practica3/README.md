---
title: Práctica 3: Computación en la nube.
author:
 - Pablo C. Alcalde
 - David Rubio Martín
---

# Parte 1

Nos disponemos a crear un microservicio de una aplicación web en Azure siguiendo los pasos del [tutorial](https://learn.microsoft.com/en-us/azure/app-service/quickstart-nodejs?tabs=windows&pivots=development-environment-cli) que facilita microsoft.

## Configuración de entorno
+ Creamos una nueva [cuenta](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension) de azure.
+ La instalación de node.js no fué necesaría ya que ambos disponiamos la misma.
  ![](./img/node-installed.png)
+ Hemos decidido usar Azure CLI por la comodidad de trabajar desde la terminal.
  ![](./img/azure-cli-installed.png)
  Tras instalarla nos dispusimos a iniciar sesión en nuestra cuenta de Azure.
  ```{bash}
  az login
  ```
  ```
  A web browser has been opened at https://login.microsoftonline.com/organizations/oauth2/v2.0/authorize. Please continue the login in the web browser. If no web browser is available or if the web browser fails to open, use device code flow with `az login --use-device-code`.
  Opening in existing browser session.
  
  Retrieving tenants and subscriptions for the selection...
  
  [Tenant and subscription selection]
  
  No     Subscription name    Subscription ID                       Tenant
  -----  -------------------  ------------------------------------  ---------------------------------------
  [1] *  Azure for Students   ********-****-****-****-************  Universidad Complutense de Madrid (UCM)
  
  The default is marked with an *; the default tenant is 'Universidad Complutense de Madrid (UCM)' and subscription is 'Azure for Students' (********-****-****-****-************).
  
  Select a subscription and tenant (Type a number or Enter for no changes):
  
  Tenant: Universidad Complutense de Madrid (UCM)
  Subscription: Azure for Students (********-****-****-****-************)
  
  [Announcements]
  With the new Azure CLI login experience, you can select the subscription you want to use more easily. Learn more about it and its configuration at https://go.microsoft.com/fwlink/?linkid=2271236
  
  If you encounter any problem, please open an issue at https://aka.ms/azclibug
  
  [Warning] The login output has been updated. Please be aware that it no longer displays the full list of available subscriptions by default.
  ```

## Creación de la aplicación Node.js

1. Creamos la aplicación usando [Express Generator](https://expressjs.com/starter/generator.html)
    ![](./img/npx-express-gen.png)
2. Instalamos los paquetes necesarios en el directorio de nuestra nueva aplicación.
    ![](./img/npm-install.png)
3. Actualizamos las dependencias a su versión más segura
    ![](./img/fix-vuln.png)
4. Corremos el servidor en local.
    ![](./img/run-server.png)
5. Comprobamos el despliegue.
    ![](./img/welcome-express.png)
## Desplegar en Azure

Desde la terminal, en el directorio de la aplicación `myEspressApp` ejecutamos el siguiente comando.
```bash
az webapp up --sku F1 --name dii-p3-pd
```
![](./img/deployment.png)

Y podemos observar que el despliegue se realiza correctamente accediendo a la [url](https://dii-p3-pd.azurewebsites.net/)
![](./img/azure-deployment.png)
## Redesplegar

Realizamos en primer lugar las modificaciones solicitadas por el tutorial, cambiando el título por `Azure` y volvemos a desplegar con el siguiente comando:
```bash
az webapp up
```

![](./img/azure-redeployment.png)
Comprobamos desde nuestro navegador que todo funciona como cabría esperar.
![](./img/browser-azure.png)
## Redespelgar Curriculum
Aprovechamos que uno de los integrantes de nuestro grupo tiene una web curriculum incompleta y simplemente actualizamos los archivos necesarios para incluirla en nuestra app.

![](./img/cv-web-browser1.png)
![](./img/cv-web-mobile.png)

# Parte 2

## Parte 2a
+ Comprobamos nuestros [créditos](https://portal.azure.com/?Microsoft_Azure_Education_correlationId=&Microsoft_Azure_Education_newA4E=true&Microsoft_Azure_Education_asoSubGuid=befdf1fb-e360-4639-8c28-ee1c4cc7ce22#view/Microsoft_Azure_Education/EducationMenuBlade/~/overview)
![](./img/coste.png)
+ Comprobamos que existe un amplio abanico de servicios que son gratuitos hasta alcanzar el límite mensual, especialmente los primeros 12 meses tras crear la cuenta, entre ellos los siguientes nos parecieron interesantes:
  - AI Document Inteligence
  - Azure Database for PostgreSQL
  - Azure DevOps for private git repos.
  - Microsoft IoT Hub up to 8000 messages/day and .5 Kb message meter size.

### Creacion de VM
Vamos a crear una máquina virtual de Linux, las siguientes configuraciones son de nuestro interés.

- Región
  Colocaremos nuestra máquina virtual lo más cerca posible, en este caso en Francia Central.
- Image
  Usaremos Debian 11 "Bullseye".
- Size
  Tomaremos el tamaño de serie.
- Authentication type
  SSH public key para poder entrar usando una clave ssh que generará automáticamente.
- Inbound port rules
  Dejamos abierto el puerto 22 para conexiones SSH como se nos solicita.

### Conexión a la VM
Nos conectamos por ssh tras incluir la clave a nuestro llavero por medio del siguiente comando
```bash
chmod 600 dii-p3-pd.pem
ssh-add dii-p3-pd.pem
```
Acto seguido nos conectamos utilizando nuestro usuario e ip.

Una vez conectados creamos el fichero `prueba.txt` y lo mostramos en pantalla.

![](./img/prueba-txt-added.png)
Acto seguido paramos la máquina virtual para evitar costes añadidos.
![](./img/stop-vm.png)
Al ver que lo necesitabamos para evitar costes también ejecutamos el siguiente comando.
![](./img/deallocate-vm.png)
Como observación podemos volver a comenzar nuestra máquina con el siguiente comando:
```bash
az vm start --resource-group dii-p3-pd --name dii-p3-pd
```



## Parte 2b

Ahora vamos a experimentar libremente con los servicios que proporciona Azure. Antes de explorar las diferentes funcionalidades que podemos explorar dentro de nuestra máquina virtual, también probamos a crear una máquina virtual desde windows.

![MV1](https://github.com/user-attachments/assets/c50e3389-1bdc-4a1f-9d6e-8e7f6ca46ba2)

Como podemos ver vamos a seleccionar que la autenticación sea vía clave pública SSH

![MV2](https://github.com/user-attachments/assets/ce8a37ae-2c9c-4bb9-a162-e1e5a76eb5c7)

En este caso la máquina virtual que creamos es Ubuntu.
![MV4](https://github.com/user-attachments/assets/75acafe3-5216-4c7f-99c1-69b0e727f54a)

Vamos a especificar que el puerto de enrtrada sea el de SSH (22).

![MV6](https://github.com/user-attachments/assets/7221af12-6e29-43af-b66b-c36b88fa0c84)

Especificamos el tamaño del disco del SO, el tipo de disco del SO y la clave será administrada por la plataforma.

![MV7](https://github.com/user-attachments/assets/709d38b3-d132-4ae1-89c1-16de57aef95d)

Para proteger la máquina virtual sería una buena práctica habilitar Microsoft Defender for Cloud. Esto hace que podamos ser más conscientes de nuestro nivel de seguridad y se nos facilitarán recomendaciones para preveer o solucionar posibles amenazas. 

Una vez hayamos implementado adecuadamente la máquina virtual podremos comenzar a trabajar con ella.

![MV11](https://github.com/user-attachments/assets/d47d690d-7d7a-429b-9bf9-c73d16b8983d)

Comenzamos a explorar por lo tanto las diferentes funcionalidades que tenemos.
Para comenzar vamos a dejar programado un apagado automático de la MV. Debemos completar los campos que aparecen en la siguiente figura: 

![mv13](https://github.com/user-attachments/assets/41a7d988-8da0-4a1d-97f5-cdcd17f571dd)

Cuando nuestra MV vaya a ser apagada de nos deberá enviar una notificación a nuestra dirección de correo electrónico.

En el apartado de automatización podemos crear Tareas:

![image](https://github.com/user-attachments/assets/1b48b3ba-2e21-4c15-a065-ef9b81d933c4)

En nuestro caso vamos a probar la opción de enviar el coste del recurso: 

![MV14](https://github.com/user-attachments/assets/4b4540d3-4111-4337-8c52-fdbd1bd45b32)

En la configuración de la tarea seleccionaremos el día y hora de inicio de esta y la frecuencia con la que se ejecutará. Nos enviará las notificaciones pertinentes a nuestro correo electrónico.

Tenemos la opción también de crear reglas de alertas.

![MV15](https://github.com/user-attachments/assets/e5e270d8-8980-428a-b2fe-30fb341052aa)

Tras ajustar las opciones que vemos en la imagen anterior vamos a detallar la importancia que tiene nuestra alerta y algunos detalles de esta alerta.

![image](https://github.com/user-attachments/assets/e14f699d-f25e-4066-8bff-7f3911f73e24)

Dentro de las opciones de configuración podemos gestionar los discos con los que trabajamos en la MV.

![MV18](https://github.com/user-attachments/assets/cc34b643-9a83-4528-9721-b1c0abe9648c)

Ahora vamos a explorar la posibilidad de crear una base de datos con SQL.

![MV21](https://github.com/user-attachments/assets/598d788c-5b64-47a5-9a4c-f92b9a1c96be)

Primeramente, vamos a especificar el nombre del servidor SQL.

![MV22](https://github.com/user-attachments/assets/d38050ce-eca1-40c1-aa6d-dc5ac7117ddd)

Continuamos especificando el ID del inicio de sesión de Microsoft, en nuestro caso nos hemos seleccionado a nosotros mismos.

![BD2](https://github.com/user-attachments/assets/5ad15f0c-6366-4427-a154-619e7f119cdc)

En cuanto al método de autenticación nosotros vamos a usar la autenticación solo de Microsoft Entra.
Creamos la base de datos SQL completando el resto de opciones que debemos seleccionar. Una de las configuraciones que parece interesante nombrar es la directiva de conexión. Podremos utilizar la directica Redirect para todas las conexiones de clientes que se originan dentro de Azure y Proxy para las conexiones que se originan fuera de Azure, también podremos hacer que para todas las conexiones se redireccionen mediante proxy o por redirección (los clientes van a establecer con el nodo en el que se encuentra la base de datos).
Las conexiones van a ser cifradas con TLS.

![BD7](https://github.com/user-attachments/assets/748fa5ca-634d-43b5-bd2a-e26375efc2c5)

En cuanto la seguridad vamos a habilitar Microsoft Defender para SQL ya que tenemos una prueba gratuita.

![BD8](https://github.com/user-attachments/assets/1a792264-d942-488d-bc00-b1f479479de0)

Una vez hemos creado la base de datos debemos introducir datos  en ella.

![BD10](https://github.com/user-attachments/assets/ef9012a8-a2bb-4767-b73d-13f2d97a06e1)

Siguiendo la dinámica de la asignatura hemos querido introducir datos que hayamos utilizado en otra práctica. En nuestro caso queremos introducir los datos de un .csv que utilizamos en uno de lso enrtregables de la asignatura. Vamos a introducir los datso de los accidentes de tráfico en madrid de 2024. Para ello vamos a utilizar la opción que se nos ofrece de Microsoft Fabric:

![BD14](https://github.com/user-attachments/assets/da9aced9-ee24-459e-94f6-7bdd6a3c1915)

Descargaremos el csv que tenemos en (https://datos.madrid.es/portal/site/egob/menuitem.c05c1f754a33a9fbe4b2e4b284f1a5a0/?vgnextoid=7c2843010d9c3610VgnVCM2000001f4a900aRCRD&vgnextchannel=374512b9ace9f310VgnVCM100000171f5a0aRCRD&vgnextfmt=default) el portal de datos abiertos de la comunidad de Madrid.

![BD11](https://github.com/user-attachments/assets/9d770402-77c3-4a9c-925d-1fc5f5fceec9) 

Vemos como tenemos una vista previa de los datos con los que vamos a trabajar: 

![BD12](https://github.com/user-attachments/assets/f24ba799-1dc8-441d-b48a-f79c65797361)

Ahora desde la máquina virtual vamos a generar una gráfica en la que el eje de las abscisas tendremos el tiempo y en el eje de las ordenadas tendremos los accidentes en los que los coductores han dado positivo en consumo de alcohol o drogas.

![BD13](https://github.com/user-attachments/assets/156cf506-10df-42bb-af11-3b1622c8e4b5)

Para el apartado de Machine learning automatizado vamos a realizar el tutorial de inicialización para el entrenamiento de un modelo de clasificación con aprendizaje automático automatizado sin código en el Estudio de Azure Machine Learning:

1- Descargamos el archivo de datos bankmarketing_train.csv

2- Creamos un nuevo área de trabajo siguiendo las siguientes especificaciones:

![image](https://github.com/user-attachments/assets/f72760c2-c26c-48fe-a447-bc176e397a29)

3- Vamos a crear un nuevo trabajo de ML automatizado.

![image](https://github.com/user-attachments/assets/9c473875-4a88-44c0-bfe0-4aad4b7eedbf)

4- Le damos nombre al experimento y pasamos a la creación y carga de nuestro conjunto de datos como un recurso de datos.

5- Vamos a Seleccionar datos y pulsamos en Crear, lo primero que haremos en elegir el nombre descripción y tipo de datos. Los datos serán de tipo Tabular.

6- Elegiremos que el origen de los datos sea desde los archivos locales.

7- En el Tipo de almacenamiento de destino, seleccionamos el almacén de datos predeterminado: workspaceblobstore. 

8- Cargamos los datos:

![image](https://github.com/user-attachments/assets/8c097337-00f4-465e-a68a-b2fb2973218b)

9- Pasamos a Configuración donde no haremos cambios y por último paso para crear el recurso de datos a Esquema. Vamos a seleccionar el conmutador de alternancia de la característica day_of_week para excluirla.

10- Una vez pulsamos en crear hemos introducido los datos con los que deseamos trabajar. El tipo de tarea va a ser Clasificación.

11- Pasamos a la configuración de tareas y rellenamos como en la figura siguiente la configuración adicional.


![image](https://github.com/user-attachments/assets/763db1c8-9c92-42c8-a596-9ad8064d4ff5)

12- En validar y probar vamops a seleccionar validación cruzada de k iteraciones y 2 validaciones cruzadas.

13- Seleccionaremos cluster de proceso como tipo de proceso , seleccionamos Crear y ya tendremos nuestro primer trabajo con ML automatizado creado.

14- Mientras se entrenan los modelos vamos a modelos y trabajos secundarios donde los modelos estarán ordenados por puntuación de métrica a medida que se completan. Vamos a ver en el algoritmo con mayor puntuación de métrica y vamos a seleccionar las pestañas Información general y Métricas para obtener información sobre el trabajo. En la pestaña de métrica podremos ver las tablas de precision, la matriz de confusión y una serie de parámetros que nos interesan.

![image](https://github.com/user-attachments/assets/076e3092-f0df-45d6-9afe-051137bb4a2f)

15-En este experimento, la implementación em un servicio web significa que la institución financiera tiene ahora una solución web iterativa y escalable para identificar posibles clientes de depósitos a plazo fijo. Una vez se ha completado el trabajo la página Detalles se rellena con una sección Mejor resumen del modelo. El mejor modelo es VotingEnsemble según la métrica AUCWeighted. Debemos seleccionar dicho modelo e implementarlo.



16- Una vez lo hayamos hecho ya tenemos un servicio web operativo para generar predicciones.



