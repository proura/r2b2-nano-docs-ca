4. Software
===========

4.1. MQTT server
------------_---

.. image:: 10_MQTT/10_01_MQTT_server.jpeg

.. code-block:: console

    r2b2@mqttsrv:~$ mosquitto_sub -h 192.168.82.106 -t "#" -v
    r2b2/3291342516/info Hello from R2B2-nano!
    r2b2/3291342516/info r2b2-3291342516
    r2b2/3291342516/mpu -116 -32 22944 244 204 23
    r2b2/3291342516/tasks mFF 50
    r2b2/3291342516/tasks mFS
    r2b2/3291342516/tasks mBF 100
    r2b2/3291342516/tasks mBS

.. code-block:: console

    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "getGyrAcc"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mFF 50"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mFS"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mBF 100"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mFS"
    r2b2@mqttsrv:~$ mosquitto_pub -h 192.168.82.106 -t "r2b2/3291342516/tasks" -m "mBS"



4.2. Firmware
-------------

`Arduino IDE <https://www.arduino.cc/>`_

Llibreries
*  Wifi
*  ESPmDNS
*  PubSubClient by Nick O'Leary
*  MPU6050_tockn

.. code-block:: none

    const char* ssid = "BuLan";
    const char* password = "00009999";
    const char* mqttServer = "192.168.82.106";
    const int   mqttPort = 1883;


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


