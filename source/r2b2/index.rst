===========================
1. R2B2-nano OpenSource ROV
===========================

L'R2B2-nano és un projecte OpenSource per a la realització d'un ROV imprimible en 3D. Un ROV, acrònim de l'anglès Remotely Operated underwater Vehicle o vehicle submarí operat remotament, és un vehicle submari controlat a distància per un operador a la superfície. 

**Principals caracteristiques de l'R2B2-nano:**

*  Microcontrolador ESP32 (Espressif)
*  Giroscopi i accelerometre MPU6050 (InvenSense)
*  Comunicacions Wifi i Bluetooth
*  Càrrega sense fils de les bateries
*  4 Propulsors (2 horitzontals i 2 verticals)
*  Actualització del Firmaware amb Wireless
*  Firmware programable amb Arduino IDE
*  Programa de control per Android
*  Xassis imprimible en 3D
*  Baix cost


1.1. Descàrrega del projecte
----------------------------

Per a poder crear el teu R2B2-nano cal que et descarreguis els dissenys i programes que hi ha als repositoris de `GitHub <https://github.com/r2b2osrov/r2b2-nano>`_.

.. note:: Els passos que es proposen en tota la documentació estan execuats al terminal d'un sistema operatiu **Debian** amb el **git** instal·lat. No hi ha d'haver cap problema per realitzar les mateixes tasques sobre altres sistemes operatius com Window o macOS.

1.1.1. GitHub R2B2-nano
***********************

El Disseny 3D, el Firmware pel microcontrolador ESP32 i el codi per generar l'applicació mòbil és pot trobar al GitHub del projecte `r2b2-nano <https://github.com/r2b2osrov/r2b2-nano>`_.

També es pot descarregar amb la comanda següent:

.. code-block:: console

    r2b2@r2b2os:~$ git clone https://github.com/r2b2osrov/r2b2-nano.git
    Cloning into 'r2b2-nano'...
    remote: Counting objects: 425, done.
    remote: Compressing objects: 100% (178/178), done.
    remote: Total 425 (delta 114), reused 235 (delta 85), pack-reused 155
    Receiving objects: 100% (425/425), 21.77 MiB | 2.52 MiB/s, done.
    Resolving deltas: 100% (198/198), done.

    r2b2@r2b2os:~$ cd r2b2-nano/
    r2b2@r2b2os:~$ ls -l
    total 52
    drwxrwxr-x 8 r2b2 r2b2  4096 ago 24 08:59 code
    drwxrwxr-x 4 r2b2 r2b2  4096 ago 24 08:59 design
    drwxrwxr-x 5 r2b2 r2b2  4096 ago 24 08:59 electronics
    -rw-rw-r-- 1 r2b2 r2b2 35147 ago 24 08:59 LICENSE
    -rw-rw-r-- 1 r2b2 r2b2    32 ago 24 08:59 README.md

Tots el dissenys 3D de les peces realitzats amb OpenSCAD i els seus STL per enviar a imprimir estan dins la carpeta **design**. Per com fer-ne us i personalitzar el vostre R2B2-nano mireu l'apartat `Disseny <../design/index.html>`_.

Dins la carpeta **code** hi podem trobar diferents programes d'`Arduino IDE <https://www.arduino.cc/>`_ per testejar la placa i els components electrònics, el Firmware de l'R2B2-nano i el prgrama de control fet amb `Processing IDE <https://processing.org/>`_ per controlar l'R2B2-nano des del mòbil. Tots aquest punts estan explicats amb més detall a l'apartat `Software <../software/index.html>`_.

1.1.2. EasyEDA 
**************

Tots els esquemes electronics i els dissenys de les plaques PCB estan allotjades en un projecte public de `EasyEDA <https://easyeda.com/r2b2osrov/r2b2-nano>`_. Des d'aquesta mateixa pàgina es poden demanar les plaques PCB a JLPCB o descarregar els fitxer Gerber per enviar la placa a producció a qualsevol altre lloc.

1.1.3. Documentació
*******************

Aquesta documentació està realitzada amb `SPHINX <http://www.sphinx-doc.org/>`_  i també està disponible per descàrrega i edició al GitHub `Documentació R2B2-nano <https://github.com/r2b2osrov/r2b2-nano-docs-ca>`_.

També es pot descarregar amb la comanda següent:

.. code-block:: console

    r2b2@r2b2os:~$ git clone https://github.com/r2b2osrov/r2b2-nano-docs-ca
    Cloning into 'r2b2-nano-docs-ca'...
    remote: Counting objects: 222, done.
    remote: Compressing objects: 100% (181/181), done.
    remote: Total 222 (delta 26), reused 214 (delta 18), pack-reused 0
    Receiving objects: 100% (222/222), 14.79 MiB | 2.56 MiB/s, done.
    Resolving deltas: 100% (26/26), done.

Un cop descarregada podem editar-la i generar localment els fitxers html per comprovar que ho hem fet correctament. Per generar la documentació cal que estigui instal·lat el programa SPHINX disponible per varis Sistemes Operatius en el següent `enllaç <http://www.sphinx-doc.org/en/master/usage/installation.html>`_.

.. code-block:: console

    r2b2@r2b2os:~$ cd r2b2-nano-docs-ca/
    r2b2@r2b2os:~/r2b2-nano-docs-ca$ make html
    Running Sphinx v1.7.2
    loading translations [ca]... done
    making output directory...
    loading pickled environment... not yet created
    building [mo]: targets for 0 po files that are out of date
    building [html]: targets for 13 source files that are out of date
    updating environment: 13 added, 0 changed, 0 removed
    reading sources... [100%] software/index                                                                
    looking for now-outdated files... none found
    pickling environment... done
    checking consistency... done
    preparing documents... done
    writing output... [100%] software/index                                                                 
    generating indices... genindex
    writing additional pages... search
    copying images... [100%] assembly/40_thrusters_images/40_06_thrusters_assembly.jpg                      
    copying static files... done
    copying extra files... done
    dumping search index in English (code: en) ... done
    dumping object inventory... done
    build succeeded, 6 warnings.

    The HTML pages are in build/html.

    pau@kisilu:~/r2b2-nano-docs-ca$ cd build/html/
    pau@kisilu:~/r2b2-nano-docs-ca/build/html$ ls
    assembly  design  electro  genindex.html  _images  index.html  objects.inv  r2b2  search.html  searchindex.js  software  _sources  _static

1.2. Col·labora
---------------

l'R2B2-nano és un projecte OpenSource i està obert a qualsevol tipus de col·laboració. Com pots col·laborar?

**Construeix el teu R2B2** i comenta-ho a les xarxes, envia'ns feedback i disfruta del projecte!!! 

**Escriu Codi** per millorar els Firmware i el programa per controlar el R2B2-nano.

**Reporta Issues** als nostres projectes publics com `GitHub <https://github.com/r2b2osrov/r2b2-nano>`_ o `EasyEDA <https://easyeda.com/r2b2osrov/r2b2-nano>`_. 

**Contribueix en la Documentació** tan sigui millorant-la per que es pugui entendre millor com tranduint-la a altres idiomes.
