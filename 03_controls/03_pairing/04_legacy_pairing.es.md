---
layout: control
title: BSAM-PA-04
summary: Rechazo del emparejamiento Legacy
description: Comprueba que tu dispositivo realiza un emparejamiento Secure Connections mode durante el pairing mode
parent: Emparejamiento
grand_parent: Controles
nav_order: 4
lang: es
page_id: cont_pair_04
permalink: controles/rechazo-legacy-connection-bluetooth/
tags:
- BR/EDR
- BLE
---

Bluetooth permite distintos métodos de emparejamiento. Uno de ellos es el emparejamiento heredado (legacy pairing), diseñado para dispositivos con capacidades computacionales limitadas. Debido a esas limitaciones, el emparejamiento heredado emplea algoritmos de generación de claves obsoletos e inseguros.

Durante la fase inicial del procedimiento de emparejamiento, los dispositivos intercambian sus capacidades mediante el proceso Pairing Feature Exchange. Este intercambio determina la derivación de la clave a largo plazo (LTK), que se utilizará posteriormente para establecer nuevas conexiones.

Siempre que las capacidades del dispositivo lo permitan, el emparejamiento debe usar exclusivamente el modo Secure Connections Only, ya que el emparejamiento heredado se considera inseguro y no debe utilizarse en ningún caso. Secure Connections proporciona seguridad criptográfica moderna que evita ataques conocidos contra métodos antiguos.

## Descripción del proceso

En un emparejamiento de Bluetooth, se distingue entre el dispositivo que inicia el _Pairing Feature Exchange_, denominado __iniciador__, y aquel que responde a estas solicitudes, conocido como __respondedor__. Durante este intercambio de características, uno de los campos se llama requisitos de autenticación (_AuthReq_), y dentro de este, encontramos el subcampo Secure Connections (_SC_) de 1 bit. Un valor de 0 indica que el dispositivo utilizará _Legacy pairing_, mientras que un valor de 1 habilitará el uso de _LE Secure connection_.

Es importante destacar que un dispositivo que admite la capacidad de _LE Secure Connection_ puede, en teoría, admitir conexiones de tipo _Legacy Pairing_, pero esto no es recomendable según el tipo de dispositivo y su uso previsto y será evaluado por el auditor.

Para verificar que se cumple el control de seguridad, se deben considerar las siguientes casuísticas:

Para comprobar que se cumple el control se dan las siguientes casuísticas:

* Campo _SC_ 0 del __iniciador__ debe producir una respuesta _Pairing Failure_ del __respondedor__. Se considera satisfactorio el control para el __respondedor__.

* Campo _SC_ 0 del _respondedor_ debe producir una respuesta _Pairing Failure_ del __iniciador__. Se considera satisfactorio el control para el __iniciador__.

* Campo _SC_ 1 de ambos dispositivos y una solicitud _Legacy pairing_ del __iniciador__ debe recibir una respuesta _Pairing Failure_ del __respondedor__. Se considera satisfactorio el control para el __respondedor__.

* Campo _SC_ 1 de ambos dispositivos y una solicitud _Legacy pairing_ del __respondedor__ debe recibir una respuesta _Pairing Failure_ del __iniciador__. Se considera satisfactorio el control para el __iniciador__.


## Caso de ejemplo

Abriendo wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) permite capturar los paquetes de emparejamiento entre el ordenador y otro dispositivo.

Durante el proceso de emparejamiento en Bluetooth entre dos dispositivos estos realizan un intercambio de capacidades, que se inician con el envío del comando _Pairing Request_ y para indicar que la conexión sea _LE Secure Connection_ o _Legacy Pairing_ se indica mediante la bandera _Secure Connection_. En este caso está a `0` (`False`).

![Wireshark Legacy Pairing Response]({{ '/assets/img/bsam-pa-04_pairing_legacy_resquest_no_sec.png' | relative_url}})

El dispositivo que responde a la solicitud de emparejamiento tiene dos opciones, desconectar porque no soporta la _Legacy pairing_ o continuar con la conexión con el comando _Pairing Response_ indicando su bandera _Secure Connection_ soportada. En este caso a `0` (`False`).

![Wireshark Legacy Pairing Request]({{ '/assets/img/bsam-pa-04_pairing_legacy_response_no_sec.png' | relative_url}})

Finalmente ambos dispositivos aceptan la conexión insegura.

![Wireshark Legacy Pairing Confirm]({{ '/assets/img/bsam-pa-04_pairing_legacy_confirm.png' | relative_url}})

El resultado del control será _FAIL_ si el dispositivo devuelve flag _Secure Connection_ en valor `0` (`False`) o si devuelve `1` (`True`) pero le podemos responder que usaremos el valor `0` (`False`) y acepta.

## Referencias externas 

* Bluetooth Core V5.3, Vol. 3, Part H, Section 2 Security Manager
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.1 Introduction
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.2 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3. Pairing Methods
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.1 Selecting Key Generation Method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.5 LE Legacy pairing phase 2
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.6 LE Secure Connections pairing phase 2
* Bluetooth Core V5.3, Vol. 3, Part C, Section 10.2.4 Secure Connections Only Mode
