![Logo](docs/source/images/campusparty_logo.png)

# Crea tu propia música usando el estimador de poses humanas MAX y TensorFlow.js

En este patrón de código, crearás música basada en el movimiento de tus brazos frente a una cámara web.

Está basado en el [Veremin](https://github.com/vabarbosa/veremin) pero modificado para usar el modelo [Estimador de poses humanas](https://developer.ibm.com/exchanges/models/all/max-human-pose-estimator) del [Model Asset eXchange (MAX)](https://developer.ibm.com/exchanges/models). El modelo estimador de poses humanas está entrenado para detectar humanos y sus poses en una imagen dada. Es [convertido](https://github.com/IBM/max-human-pose-estimator-tfjs#converting-the-model) al formato amigable para web [TensorFlow.js](https://js.tensorflow.org).

La aplicación web transmite video desde tu cámara web. El modelo estimador de poses humanas se usa para predecir la ubicación de tus muñecas dentro del video. La aplicación toma las predicciones y las convierte en tonos en el navegador o en valores MIDI que se envían a un dispositivo MIDI conectado.

Los navegadores deben permitir el [acceso a la cámara web](https://caniuse.com/#feat=stream) y soportar la [Web Audio API](https://caniuse.com/#feat=audio-api). Opcionalmente, para poder integrarlo a un dispositivo MIDI el navegador debe soportar el [Web MIDI API](https://caniuse.com/#feat=midi) (por ejemplo, Navegador Chrome version 43 o posteriores).

![Arquitectura](docs/source/images/architecture.png)

## Flujo

1. El modelo estimador de poses humanas se convierte al formato web TensorFlow.js utilizando el convertidor Tensorflow.js.
1. El usuario inicia la aplicación web.
1. La aplicación web carga el modelo de TensorFlow.js.
1. El usuario se para frente a la cámara web y mueve los brazos.
1. La aplicación web captura cuadros de video y los envía al modelo de TensorFlow.js. El modelo retorna una predicción de la pose estimada a la interfaz de usuario Web.
1. La aplicación web procesa la predicción y superpone el esqueleto de la pose estimada en la interfaz de usuario web.
1. La aplicación web convierte la posición de las muñecas del usuario de la pose estimada en un mensaje MIDI, y el mensaje se envía a un dispositivo MIDI conectado o se reproduce el sonido en el navegador.


## Componentes Incluidos

* [Estimador de poses humanas MAX](https://developer.ibm.com/exchanges/models/all/max-human-pose-estimator): Un modelo de Machine Learning que detecta poses humanas.
* [TensorFlow.js](https://js.tensorflow.org/): Una biblioteca de JavaScript para entrenar e implementar modelos de ML en el navegador y en Node.js


## Tecnologías Incluidas

* [Web MIDI API](https://www.w3.org/TR/webmidi): Una API que admite el protocolo MIDI, que permite a las aplicaciones web enumerar y seleccionar dispositivos de entrada y salida MIDI en el sistema cliente y enviar y recibir mensajes MIDI.
* [Web Audio API](https://www.w3.org/TR/webaudio): Una API web de alto nivel para procesar y sintetizar audio en aplicaciones web.
* [Tone.js](https://tonejs.github.io/): Un framework para crear música interactiva en el navegador.

## Demo

[Para probar la aplicación](https://github.com/IBM/max-human-pose-estimator-tfjs#using-the-app) sin necesidad de instalar algo, simplemente visitar [ibm.biz/veremax](https://ibm.biz/veremax) en un navegador web que tiene acceso a una cámara web y soporte para la Web Audio API.

[![Demo del estimador de poses humanas MAX](https://img.youtube.com/vi/QSrRUw2RRqw/0.jpg)](https://youtu.be/QSrRUw2RRqw)


## Pasos

Hay tres maneras de correr tu propio Veremax:

- [Desplegar a IBM Cloud](https://github.com/IBM/max-human-pose-estimator-tfjs#deploy-to-ibm-cloud)
- [Desplegar localmente](https://github.com/IBM/max-human-pose-estimator-tfjs#run-locally)
- [Desplegar en Docker](https://github.com/IBM/max-human-pose-estimator-tfjs#run-in-docker)

### Desplegar a IBM Cloud

Pre-requisitos:

- Tener una [cuenta de IBM Cloud](https://console.bluemix.net/)
- Instalar/Actualizar el [cliente de IBM Cloud](https://console.bluemix.net/docs/cli/reference/ibmcloud/download_cli.html#install_use)
- [Configurar y hacer login](https://console.bluemix.net/docs/cli/index.html#overview) a IBM Cloud usando el CLI

Para desplegar al IBM Cloud, desde una terminal correr:

1. Clonar el `max-human-pose-estimator-tfjs` localmente:

    ```
    $ git clone https://github.com/IBM/max-human-pose-estimator-tfjs
    ```

1. Ubicarse sobre el directorio del repositorio clonado:

    ```
    $  cd max-human-pose-estimator-tfjs
    ```

1. Hacer login a tu cuenta de IBM Cloud:

    ```
    $ ibmcloud login
    ```

1. Apunte a una organización y espacio de Cloud Foundry:

    ```
    $ ibmcloud target --cf
    ```

1. Desplegar la aplicación a IBM Cloud:

    ```
    $ ibmcloud cf push
    ```
    El despliegue puede tardar unos minutos.

1. Puedes visualizar la aplicación con un explorador en la URL que despliega la salida del último comando ejecutado.

    > **Nota**: Dependiendo de tu explorador, podrías necesitar ingresar a la app utlizando el protocolo **`https`** en lugar de **`http`**

### Desplegar localmente

Para correr la aplicación localmente:

1. Desde una terminal, clonar el `max-human-pose-estimator-tfjs` localmente:

    ```
    $ git clone https://github.com/IBM/max-human-pose-estimator-tfjs
    ```

1. Apunte su servidor web al directorio del repositorio clonado (`/max-human-pose-estimator-tfjs`)

    > Por ejemplo:  
    > - usando la extensión **[Web Server for Chrome](https://github.com/kzahel/web-server-chrome)** (disponible en la [Tienda Web de Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb))
    >   
    >   1. Vaya a la página de aplicaciones de su navegador Chrome (chrome://apps)
    >   1. Click en **Web Server**
    >   1. Desde el Web Server, click **CHOOSE FOLDER** o **ELEGIR DIRECTORIO** y vaya al directorio del repositorio clonado
    >   1. Inicie el servidor web
    >   1. Fíjese en el **Web Server URL(s)** (por ejemplo, http://127.0.0.1:8887)
    >   
    > - usando el módulo **HTTP server** de Python
    >   
    >   1. Desde una terminal, ubíquese en el directorio del repositorio clonado
    >   1. Dependiendo de su versión de Python, ingrese alguno de los siguientes comandos:
    >       - Python 2.x: `python -m SimpleHTTPServer 8080`
    >       - Python 3.x: `python -m http.server 8080`
    >   1. Una vez iniciado, la URL del Web Server debería ser http://127.0.0.1:8080
    >   

1. Desde su explorador, vaya a la URL del Web Server

### Desplegar en Docker

Pre-requisito:

- Instalar [Docker](https://www.docker.com/products/docker-desktop)

From a terminal:

1. Clone this repository

   ```
   $ git clone https://github.com/IBM/max-human-pose-estimator-tfjs
   $ cd max-human-pose-estimator-tfjs
   ```

2. [Build the Docker image](https://docs.docker.com/engine/reference/commandline/build/)

   ```
   $ docker build -t veremax .
   ```

3. [Run the Docker container](https://docs.docker.com/engine/reference/commandline/run/)

   ```
   $ docker run -d -p 3000:80 veremax
   ``` 

4. In your browser, open [localhost:3000](http://localhost:3000) and enable the web camera

To stop the Docker container. From a terminal:

1. [Obtain the container id](https://docs.docker.com/engine/reference/commandline/ps/) for `veremax`

   ```
   $ docker ps
   ``` 

2. [Stop the Docker container](https://docs.docker.com/engine/reference/commandline/stop/) using the container id obtained above

   ```
   $ docker stop container_id  
   ```

## Using the app

For best results use in a well-lit area with good contrast between you and the background. And stand back from the webcam so at least half of your body appears in the video.

At a minimum, your browsers must allow [access to the web camera](https://caniuse.com/#feat=stream) and support the [Web Audio API](https://caniuse.com/#feat=audio-api).

In addition, if it supports the [Web MIDI API](https://caniuse.com/#feat=midi), you may connect a MIDI synthesizer to your computer. If you do not have a MIDI synthesizer you can download and run a software synthesizer such as [SimpleSynth](http://notahat.com/simplesynth/).

If your browser does not support the Web MIDI API or no (hardware or software) synthesizer is detected, the app defaults to using the Web Audio API to generate tones in the browser.

Open your browser and go to the app URL. Depending on your browser, you may need to access the app using the **`https`** protocol instead of the **`http`**. You may also have to accept the browser's prompt to allow access to the web camera. Once access is allowed, the Human Pose Estimator model gets loaded.

After the model is loaded, the video stream from the web camera will appear and include an overlay with skeletal information detected by the model. The overlay will also include two adjacent zones/boxes. When your wrists are detected within each of the zones, you should here some sound.

- Move your right hand/arm up and down (in the right zone) to generate different notes
- Move your left hand/arm left and right (in the left zone) to adjust the velocity of the note.

Click on the Controls icon (top right) to open the control panel. In the control panel you are able to change MIDI devices (if more than one is connected), configure post-processing settings, set what is shown in the overlay, and configure additional options.


## Converting the model

The converted MAX Human Pose Estimator model can be found in the [`model`](https://github.com/IBM/max-human-pose-estimator-tfjs/tree/master/model) directory. To convert the model to the TensorFlow.js web friendly format the following steps below.

> **Note**: The Human Pose Estimator model is a frozen graph. The current version of the `tensorflowjs_converter` no longer supports frozen graph models. To convert frozen graphs it is recommended to use an older version of the Tensorflow.js converter (0.8.0) and then run the `pb2json` script from Tensorflow.js converter 1.x.

1. Install the [tensorflowjs 0.8.0](https://pypi.org/project/tensorflowjs/0.8.0/) Python module
1. Download and extract the pre-trained [Human Pose Estimator model](http://max-assets.s3.us.cloud-object-storage.appdomain.cloud/human-pose-estimator/1.0/assets.tar.gz)
1. From a terminal, run the `tensorflowjs_converter`:

    ```
    tensorflowjs_converter \
        --input_format=tf_frozen_model \
        --output_node_names='Openpose/concat_stage7' \
        {model_path} \
        {pb_model_dir}
    ```

    where

    - **{model_path}** is the path to the extracted model  
    - **{pb_model_dir}** is the directory to save the converted model artifacts
    - **output_node_names** (`Openpose/concat_stage7`) is obtained by inspecting the model’s graph. One useful and easy-to-use visual tool for viewing machine learning models is [Netron](https://github.com/lutzroeder/Netron).

1. Clone and install the `tfjs-converter` TypeScipt scripts

    ```
    $ git clone git@github.com:tensorflow/tfjs-converter.git
    $ cd tfjs-converter
    $ yarn
    ```

1. Run the `pb2json` script:

    ```
    yarn ts-node tools/pb2json_converter.ts {pb_model_dir}/ {json_model_dir}/
    ```

    where

    - **{pb_model_dir}** is the directory to converted model artifacts from step 3
    - **{json_model_dir}** is the directory to save the updated model artifacts


When the completed, the contents of **{json_model_dir}** will be the web friendly format of the Human Pose Estimator model for TensorFlow.js 1.x. And the **{pb_model_dir}** will be the web friendly format for TensorFlow.js 0.15.x.


## Links

 - Bring Machine Learning to the Browser With TensorFlow.js - [Part I](https://medium.com/ibm-watson-data-lab/bring-machine-learning-to-the-browser-with-tensorflow-js-part-i-16924457291c), [Part II](https://medium.com/ibm-watson-data-lab/bring-machine-learning-to-the-browser-with-tensorflow-js-part-ii-7555ed9a999e), [Part III](https://medium.com/ibm-watson-data-lab/bring-machine-learning-to-the-browser-with-tensorflow-js-part-iii-62d2b09b10a3)
 - [Model Asset eXchange](https://developer.ibm.com/exchanges/models/)
 - [IBM Cloud](https://console.bluemix.net/)
 - [Getting started with the IBM Cloud CLI](https://console.bluemix.net/docs/cli/index.html#overview)
 - [Prepare the app for deployment - IBM Cloud](https://console.bluemix.net/docs/runtimes/nodejs/getting-started.html#prepare)
 - [Playing with MIDI in JavaScript](https://medium.com/swinginc/playing-with-midi-in-javascript-b6999f2913c3)
 - [Introduction to Web Audio API](https://css-tricks.com/introduction-web-audio-api)
 - Human pose estimation using OpenPose with TensorFlow - [Part 1](https://arvrjourney.com/human-pose-estimation-using-openpose-with-tensorflow-part-1-7dd4ca5c8027), [Part II](https://arvrjourney.com/human-pose-estimation-using-openpose-with-tensorflow-part-2-e78ab9104fc8)


## License

This code pattern is licensed under the Apache Software License, Version 2.  Separate third party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
