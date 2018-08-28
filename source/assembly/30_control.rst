===================
5.3. Xassís Control
===================

5.3.1. Que necessitem?
**********************

5.3.1.1 Material
----------------

*  1x `Xassís Control <../design/index.html#xassis>`_  `(Fitxer STL) <https://github.com/r2b2osrov/r2b2-nano/blob/master/design/stl/chassis_b.stl>`_
*  1x `R2B2-nano-board <../electro/index.html#placa-pcb-r2b2-nano>`_ `R2B2-nano <https://easyeda.com/r2b2osrov/r2b2-nano>`_
*  1x `ESP32 WROVER <../electro/index.html#esp32-wrover-i-espressif>`_
*  2x `Mòdul diver motor TB6612FNG <../electro/index.html#tb6612fng-thosiba>`_
*  1x `Mòdul Giroscopi/Acceleròmetre MPU6050 <../electro/index.html#mpu-6050-invensense>`_
*  1x Registència 12k
*  1x Condensador tantalum 10 UF 16 V 106C
*  2x Condensador ceràmic 0.1 UF 104 
*  1x `Regulador voltage <../electro/index.html#reguladors-de-voltatge>`_ 3.3V
*  1x `Connector IPEX/U.FL 20278 1.13mm <../electro/index.html#connector-ipex-u-fl-20278-1-13mm>`_
*  1x `Cable coaxial d'antena 1.13mm i 50ohm de 5metres <80_materials.html#cable-coaxial>`_
*  `Cable elèctic de silicona 28 AWG <80_materials.html#cable-silicona-28awg>`_
*  `Estany <80_materials.html#estany>`_
*  `Resina de poliester i catalitzador <80_materials.html#resina-poliester>`_

5.3.1.2 Eines
-------------

*  `Soldador <81_tools.html#soldador>`_
*  Estenalles d'electrònica 
*  Guants
*  Mascara
*  Ulleres de protecció

5.3.2 Muntatge
**************

.. figure:: 30_control_images/30_01_control_material.jpg
    :align: center

    Material per sla R2B2-nano-board.

Si hi ha cap dubte de on soltar cada component es pot consultar l'esquema electrònic "r2b2-nano-board" al projecte públic `R2B2-nano <https://easyeda.com/r2b2osrov/r2b2-nano>`_ allotjat a EasyEDA.

A l'apartat de `Electrònica-->Control <../electro/index.html#control>`_ es proposen altres maneres de muntar el sistema de control.

.. image:: ../electro/20_control/20_01_control_schematic.png

Per a soldar els components a sobre la placa és recomanable començar per l'ESP32 (per facilitar la soldarura d'aquest component és recomanable fer servir algun tipus de pasta de soldadura Flux NC-559 o similars). Llavors podeu anar soldant tots els altres components tal i com és mostren a la següent imatge.

.. image:: 30_control_images/30_01_control_assembly.jpg

Un cop es tinguin tots els components soldats és convenient comprovar amb un tester que totes les connexions estan ben soldades. Per això posarem el tester que ens avisi amb senyal acustica si hi ha continuitat i comprovarem els punts de soldadura seguint l'esquema. 

Si tot els punts de soldadura estan bé, alimentarem la placa i farem la primera pujada del Firmware com s'explica a l'apartat de `Software-->Firmware <../software/index.html#firmware>`_. Així podrem comprovar que tots els ports a on connectar els motors i el Giroscopi/Acceleròmetre funcionen correctament.

.. image:: 30_control_images/30_02_control_assembly.jpg

Un cop tinguem la placa de control funcionant ja hi poden soldar tots els cables i posar-la dins el xassís de control.

.. image:: 30_control_images/30_03_control_assembly.jpg

Agafarem el cable coaxial i hi muntarem el connector IPEX.

.. image:: 30_control_images/30_04_control_assembly.jpg
.. image:: 30_control_images/30_05_control_assembly.jpg

Conectarem l'antena a la placa i col·locarem tots els cables apunt per enresinar el xassís. 

.. image:: 30_control_images/30_06_control_assembly.jpg
.. image:: 30_control_images/30_07_control_assembly.jpg

Prepararem la resina i la bolcarem dins el xassís cobrint tots els element electrònics.

.. image:: 30_control_images/30_08_control_assembly.jpg
.. image:: 30_control_images/30_09_control_assembly.jpg

