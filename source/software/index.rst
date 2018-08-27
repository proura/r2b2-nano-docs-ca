===========
4. Software
===========

En aquest apartat es descriurà una proposta de com programar i utilitzar l'R2B2-nano, però cadascú pot realitzar o adaptar la proposta a les seves necessitats o tecnologies.

La programació està separada en tres grans apartats: el servidor de missatjeria MQTT que s'utilitza coma a sistema de comunicació, el firmware de l'R2B2-nano fet amb Arduino IDE i el programa de control fet amb Processing IDE.

.. note:: Els passos que es proposen en tota la documentació estan execuats al terminal d'un sistema operatiu GNU/Linux **Debian** i amb el **mosquito client MQTT** instal·lat.

4.1. MQTT server
----------------

MQTT (Message Queuing Telemetry Transport) és un protocol de missatgeria publish-subscribe basat en el protocol TCP/IP.

Propietats de MQTT :

*  Estàndard obert.
*  Estructura senzilla (mínim nombre de bytes per cada missatge).

    =========================== =========== ========== =================
        Acció                   Pro.l HTTP  Prot. MQTT Relació HTTP/MQTT
    =========================== =========== ========== =================
    Rebre 1 unitat de dades     320 bytes   69 bytes   4,6
    Enviar 1 unitat de dades    320 bytes   47 bytes   6,8
    Rebre 100 unitats de dades  12600 bytes 2445 bytes 5,1
    Enviar 100 unitats de dades 14100 bytes 2126 bytes 6,6
    =========================== =========== ========== =================

*  Fiabilitat (existeix la funció QoS Quality of Service), que ens informa de l'estat de la comunicació.
*  Simplicitat (protocol que està definit en 43 pàgines).

En un protocol de publicació i subscripció pots realitzar els següents funcions:

*  Publicar missatge a un Tòpic.
*  Subscriure't a un Tòpic i per tant rebre tot el que es publiqui en aquell tòpic.
*  Anul·lar una subscripció i per tant deixar de rebre missatges d'aquell tòpic.

Cada R2B2-nano publica informació en un tòpic format per una primera part on indica que és un R2B2, a continuació el seu número d'identificació i una petita informació del contingut sobre que publiquem les dades seguit del missatge que volem publicar.

Exemples: 

.. code-block:: none

    client.publish("r2b2/123456789/info", "Hello from R2B2-nano!"); --> Cada R2B2-nano publica aquest missatge a l'arrencar.
    client.publish("r2b2/123456789/mpu", "425 125 124 25 36 41"); --> Per enviar informació dels valors de Giroscopi/Accelerometre.
    client.publish("r2b2/123456789/tmp", "25.4568"); --> Per enviar la temperatura.

Llavors els dispositius que volen rebre la informació dels R2B2-nanos o d'un sol R2B2-nano tan sols s'ha de subscriure al tòpic que correspongui.

.. code-block:: none 

    client.subscribe("r2b2/123456789/#"); --> Qui estigui subscrit a aquest tòpic rebrà tot el que publiqui l'R2B2-nano 123456789.
    client.subscribe("r2b2/123456789/tasks"); --> Es rebran els missatges publicats a tòpic r2b2/123456789/tasks.

Per tant ja tenim una manera de comunicar dispositius i R2B2-nanos. Cada R2B2-nano publica les dades amb el tòpic format pel seu propi ID ("r2b2/r2b2Id/XXXX") i es subscriu al tòpic que li correspon a ell mateix r2b2/r2b2Id/tasks per rebre les ordres a executar i cada dispositiu que vulgui controlar un R2B2-nano en concret es subscriu al tòpic r2b2/r2b2Id/# i publica a r2b2/r2b2Id/tasks. 

Daquesta manera també podem fer que un dispositiu controli i/o observi més d'un R2B2-nano. Si es subscriu al tòpic "r2b2/#" rebrà la infromació de tots els R2B2-nanos.

.. figure:: 10_MQTT/10_01_MQTT_server.jpeg
    :align: center

    Communicacions R2B2-nano.

Des de la consola ens podem subscriure al servidor MQTT que estiguem fent servir i veure tots els missatges que s'estan intercanviant. Això pot ser molt útil per depurar les comunicacions entre R2B2-nanos i els programes de control.

.. code-block:: console

    r2b2@mqttsrv:~$ mosquitto_sub -h 192.168.82.106 -t "#" -v
    r2b2/3291342516/info Hello from R2B2-nano!
    r2b2/3291342516/info r2b2-3291342516
    r2b2/3291342516/mpu -116 -32 22944 244 204 23
    r2b2/3291342516/tasks mFF 50
    r2b2/3291342516/tasks mFS
    r2b2/3291342516/tasks mBF 100
    r2b2/3291342516/tasks mBS

També podem publicar missatges des la consola per mirar si el Firmware de l'R2B2-nano respon correctament a l'ordre llançada sense necessitat de fer ús de cap programa de control.

.. code-block:: console

    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "getGyrAcc"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mFF 50"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mFS"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mBF 100"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mFS"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mBS"



4.2. Firmware
-------------

4.2.1. Arduino IDE
******************

El firmware de l'R2B2-nano està desenvolupat amb l'`Arduino IDE <https://www.arduino.cc/>`_. 

Per poder compilar el firmware caldrà afegir la placa ESP32 seguint les instruccions oficials del `GitHub d'espressif <https://github.com/espressif/arduino-esp32>`_ i instal·lar les llibreries **PubSubClient** i **MPU6050_tockn** a través del menú Sketch-->Include Library-->Manage Libraries. 

La llibreria PubSubClients és per publicar i subscriure en un servidor MQTT i la llibreira MPU6050_tockn per llegir els valors de Giroscopi i acceleròmetre. 

Un cop fet això ja podem obrir el projecte 99_R2B2_Nano_Firmware d'Arduino IDE que trobarem a la carpeta codi del projecte R2B2-nano, configurar la placa com a ESP32 Dev Module i seleccionar el Port on estigui connectat.

.. figure:: 20_Firmware/20_01_Firmware_AIDE.jpg
    :align: center

    Configuració Arduino IDE.

.. warning:: El primer cop que es puja el firmware s'ha de fer amb un conversor de USB a TTY connectant el port RX de R2B2-nano al TX del conversor TTY, el port TX de R2B2-nano al RX del conversor TTY, connectar el GND de l'R2B2-nano amb el GND del conversor TTY i posant la placa en mode "serial bootloader", això es fa posant el port GPIO0 (BOOT) a LOW/GND i fent un reset posant l'enable (EN) a LOW/GND. Un cop feta la primera pujada ens apareixera l'R2B2 al llistat de ports i podrem actualitzar el firmware a través de WiFi.

.. figure:: 20_Firmware/20_02_Firmware_AIDE.jpg
    :align: center

    R2B2-nano connectat per USB.

4.2.2. Codi del Firmware
************************

4.2.2.1. Configuració
+++++++++++++++++++++

Cal configurar 4 variables abans de pujar el firmware a l'R2B2-nano. La variable **ssid** que defineix a quina WiFi connectar, **password** per la contrasenya de la WiFi, a la variable **mqttServer** hi hem d'especificar la direcció IP del servidor MQTT i per finalitzar **mqttPort** per definir a través de quin port ha de realitzar la connexió MQTT.

.. code-block:: none

    const char* ssid = "BuLan";
    const char* password = "00009999";
    const char* mqttServer = "192.168.82.106";
    const int   mqttPort = 1883;

4.2.2.2. OTA Over-the-Air
+++++++++++++++++++++++++

Per que podem actualitzar el firmware a través de WiFi cal que sempre hi hagi els següent talls de codi a l'Sketch d'Arduino. Si no ho posem o fem una mala configuració haurem d'actualitzar el firmware l'R2B2-nano amb el conversor USB to TTY.

.. code-block:: c

    //Dins a SETUP
    ...
    initializeOTA((char*)R2B2id);
    ...

    //Dins a LOOP
    ...
    ArduinoOTA.handle();
    ...

    //FUNCIÓ

    void initializeOTA(const char * host_name){
    sprintf(host, "r2b2-%u", host_name);
    ArduinoOTA.setHostname(host);
    ArduinoOTA
        .onStart([]() {
        String type;
        if (ArduinoOTA.getCommand() == U_FLASH) type = "sketch";
        else type = "filesystem";
        Serial.println("Start updating " + type);
        })
        .onEnd([]() {
        Serial.println("\nEnd");
        })
        .onProgress([](unsigned int progress, unsigned int total) {
        Serial.printf("Progress: %u%%\r", (progress / (total / 100)));

        //Send Upload Info to MQTT
        if((millis() - timer > 250) || ((progress / (total / 100)) == 100 )){
            sprintf(msg, "upload %u%%\r", (progress / (total / 100)));
            sprintf(topic, "r2b2/%u/info", R2B2id);
            client.publish(topic, msg);
            timer = millis();
        }
        
        })
        .onError([](ota_error_t error) {
        Serial.printf("Error[%u]: ", error);
        if (error == OTA_AUTH_ERROR) Serial.println("Auth Failed");
        else if (error == OTA_BEGIN_ERROR) Serial.println("Begin Failed");
        else if (error == OTA_CONNECT_ERROR) Serial.println("Connect Failed");
        else if (error == OTA_RECEIVE_ERROR) Serial.println("Receive Failed");
        else if (error == OTA_END_ERROR) Serial.println("End Failed");
        });
    
    ArduinoOTA.begin();
    }


4.3. Control
------------

`Processing IDE <https://processing.org/>`_

Llibreries
*  MQTT
*  TocxicLibs
*  G4P
*  Geomerative

.. code-block:: none

    String r2b2Id = "3291342516";
    String MQTTServer = "192.168.82.106";


