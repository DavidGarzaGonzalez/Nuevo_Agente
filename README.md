# Nuevo Agente

### Descripción
Este proyecto es un tutorial para construir un Agente de Dialogflow, el cual se pueden crear intents a partir de un Google Sheets. Este proyecto también aprovecha la opción de Fulfillment de Dialogflow para activar una función de Firebase Functions y almacena los datos más importantes de las interacciones con el Agente en una base de datos (Realtime Database)

### Requisitos y recomendaciones:
Se requiere utilizar la misma cuenta de Google para todas las aplicaciones aquí mencionadas (Cloud, Dialogflow, Firebase y Sheets). Debe de tener instalado Node.js y NPM. Ingresar a este [link](https://www.npmjs.com/get-npm). Se recomienda mantener abierta las pestañas que de Cloud, Dialogflow y Firebase debido a que con frecuencia nos movemos entre estas aplicaciones. 

## Crear un proyecto de Google Cloud
Dirigirse a [la consola de Google Cloud](https://console.cloud.google.com/) y crear un nuevo proyecto.

## Crear Agente en Dialogflow y enlazar con Sheets:
Dirigirse a [Dialogflow](https://dialogflow.cloud.google.com/) y crear un nuevo agente. Seleccionar el nombre del agente, idioma y zona horaria. Seleccionar el proyecto que acaba de crear en Google Cloud (en el paso anterior). 

![Image of Dialogflow New Agent]()

Dirigirse al siguiente [Sheets]() . Dar click en *Archivo/Hacer una copia* y nombrar la copia para que pertenezca al proyecto actual El nombre **no** tiene que ser el mismo en todos los proyectos.

Ahora regresamos a Google Cloud y seleccionamos la pestaña de “API & SERVICES” (APIS & SERVICIOS). Ahora dirigirse a “Credentials” (Credenciales), dar click en “+CREATE CREDENTIALS” (+CREAR CREDENCIALES) y seleccionamos “OAuth client ID” (ID de cliente de OAuth).

Dar click en “CONFIGURE CONSENT SCREEN” (Configurar página de consentimiento) y seleccionar “External” (Exterior). Elegir un nombre para el proyecto y un mail para recuperación. 

Regresar a “Credentials/+CREATE CREDENTIALS” y seleccionar “Web application” (Aplicación web), colocar un nombre y en “Authorized JavaScript origins” (Orígenes de JavaScript autorizados) pegar este link: https://script.google.com. Ahora en “Authorized redirect URIs” (URL de redirección autorizados) copiar el link que aparece dentro del sheets, a un lado de “Then go to this site to grant authorization”. 

![Image of the Sheets link]()

Dar click en “CREATE” (Crear) y generará un “client ID” y un “client secret” para el proyecto. Ahora nos dirigimos al Sheets, en la sección de Herramientas/Editor de secuencias de comandos. Buscar la función **storeGoogleCredentials()** y reemplazar el “client_id” y “client_secret” por los que se generaron anteriormente. 

Ahora en la parte superior del script, ubicar donde se señala el nombre actual del proyecto y copiarlo. Ahora ir a *Editar/Buscar* y reemplazar. En buscar pegamos el nombre actual del proyecto y en reemplazar pegamos el “Project ID” que se encuentra en Dialogflow 

![Image of Project ID from Dialogflow]()

Seleccionar reemplazar todos.

## Conectar con Realtime Database

Descargar el archivo de este Github y guardarlo en el escritorio. Abrir el command prompt de la consola. Presiona el botón de Windows y busca “cmd”. Ahora en la consola dirigirse a la carpeta usando el comando “cd”. En la siguiente imagen, se muestra como me muevo desde la dirección *Users\Admin* hacia *Users\Admin\Desktop\Test* usando el comando “cd”. 

```bash
cd desktop/test
```

![Image of the path to Test]()

Una vez en la carpeta, ingresar el siguiente comando:
```bash
npm install -g firebase-tools
```

Una vez terminado, utiliza el comando 

```bash
firebase login
```

e ingresa con la misma cuenta que utilizaste para crear el proyecto. 

![Firebase CLI Login Succesful]()

Ahora ejecutar el comando

```bash
firebase init
```

Seleccionar solamente la opción de Functions. Dar enter en “Use an existing Project” y seleccionar el proyecto deseado. Escoger Javascript y dar si a ESlint. Instalar las dependencias en el momento.

![Firebase parte 1]()
![Firebase parte 2]()

Ahora vamos a ir a [la consola de firebase](https://console.firebase.google.com/) y seleccionamos el mismo proyecto. Ubicamos el menú del lado izquierdo y seleccionamos Database. Creamos una Realtime Database y usamos “Start in locked mode”. A continuación nuestra base de datos se va a construir y aparecerá algo como lo siguiente:  

![Imagen de la base de datos]()

Regresando a su escritorio, abra la carpeta donde se guardó el proyecto. Ahí va a haber una carpeta llamada “functions”, ingrese a la carpeta y eliminará los siguientes archivos: *index.js, package.json* y *package-lock.json*. 
  
Ahora los va a sustituir por los que se encuentran en este repositorio. Ahora hacemos click derecho en este archivo, seleccionamos *abrir con* y utilizamos el block de notas. 

Ubicamos la siguiente sección del archivo y vamos a ingresar el link que aparece en la base de datos donde está entre paréntesis “(url de la base de datos)”

![Imagen del block de notas]()
![Imagen del link de la base de datos]()

* Hay que tener cuidado con lo siguiente: 
  * •	El link debe de empezar con ws:// en lugar de https://
  * •	Todo el link debe de quedar encerrado entre comillas: 'ws://guide-1234.firebaseio.com/ '

Una vez hecho esto, le damos guardar en el archivo de “index.js” y lo cerramos. Ahora regresamos a la consola de comandos “command prompt” y nos dirigimos a la carpeta “functions”.

```bash
cd functions
```

Una vez dentro de la carpeta, utilizamos el siguiente comando: 

```bash
npm install
```

Al finalizar, vamos a ejecutar el siguiente comando y nos debería de regresar a la carpeta del proyecto. 

```bash
cd ..
```

Una vez aquí, utilizamos el siguiente comando: 

```bash
firebase deploy
```

Nos deberá de aparecer un mensaje tipo “Deploy complete!” y dentro del texto que aparece en el comando, debe de haber una línea que indique el url de nuestra función. Vamos a guardar este URL.

![Imagen con URL de la función]()

Ahora vamos Dialogflow y seleccionamos la sección de “Fulfillment” en el menú del lado izquierdo. Ahora seleccionamos Webhook ENABLED y pegamos el URL de la función en el espacio URL y guardamos los cambios.

![Imagen del Fulfillment de Dialogflow]()

### Prueba

El siguiente paso es ingresar a Dialogflow y activar el Webhook call para los intents que se crean por Default. 

![Imagen de la prueba]()

Una vez activados, puede decir hola a su agente y se debería de registrar en la base de datos.
