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

Hay dos maneras de correr tu propio Veremax:

- [Desplegar a IBM Cloud](https://github.com/IBM/max-human-pose-estimator-tfjs#deploy-to-ibm-cloud)
- [Desplegar localmente](https://github.com/IBM/max-human-pose-estimator-tfjs#run-locally)

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


## Usando la aplicación web

Para obtener mejores resultados, utilícelo en un área bien iluminada con buen contraste entre usted y el fondo. Y aléjese de la cámara web para que al menos la mitad de su cuerpo aparezca en el video.

Como mínimo, sus exploradores deben permitir [acceso a la cámara web](https://caniuse.com/#feat=stream) y soportar la [Web Audio API](https://caniuse.com/#feat=audio-api).

Además, si soporta la  [Web MIDI API](https://caniuse.com/#feat=midi), puedes conectar un sintetizador MIDI. Si no tienes un sintetizador MIDI puedes descargar un software de sintetizador como el [SimpleSynth](http://notahat.com/simplesynth/).

Si su navegador no es compatible con la Web MIDI API o no se detecta ningún sintetizador (hardware o software), la aplicación usa de manera predeterminada la API de audio web para generar tonos en el navegador.

Abra su navegador y vaya a la URL de la aplicación. Dependiendo de su navegador, es posible que necesite acceder a la aplicación utilizando el protocolo **`https`** en lugar de **`http`**. Es posible que también deba aceptar la solicitud del navegador para permitir el acceso a la cámara web. Una vez que se permite el acceso, se carga el modelo del estimador de poses humanas.

Después de cargar el modelo, aparecerá la transmisión de video de la cámara web e incluirá una superposición con información esquelética detectada por el modelo. La superposición también incluirá dos zonas / cajas adyacentes. Cuando se detectan sus muñecas dentro de cada una de las zonas, debería escuchar algo de sonido.

- Mueva su mano / brazo derecho hacia arriba y hacia abajo (en la zona derecha) para generar diferentes notas
- Mueva su mano / brazo izquierdo hacia la izquierda y hacia la derecha (en la zona izquierda) para ajustar la velocidad de la nota.

Haga click en el icono de Controles (arriba a la derecha) para abrir el panel de control. En el panel de control, puede cambiar los dispositivos MIDI (si hay más de uno conectado), configurar los ajustes de posprocesamiento, establecer lo que se muestra en la superposición y configurar opciones adicionales.

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
