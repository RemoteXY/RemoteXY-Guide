# RemoteXY

RemoteXY is a platform for creating custom graphical user interfaces (GUI) for remote control of microcontroller-based devices via a smartphone or tablet. A user-friendly editor allows you to design graphical interfaces of any complexity. The editor automatically generates a template of the source code for the microcontroller. The RemoteXY mobile application enables connection to the device and opens the graphical interface. Various communication methods are supported for data exchange between the device and the mobile application.

#### Features of RemoteXY

- The graphical interface code is stored directly in the controller. RemoteXY does not use any services or servers for data storage. All data is kept only in the device’s memory and the mobile application. 
- When connecting via Bluetooth, Wi-Fi access point, or USB, there is no need for an internet connection to load the graphical interface code from a server.
- When using cloud server connection, you can access the device’s graphical interface from anywhere in the world without configuring network routing. The cloud server only serves as a transit and does not store any data.
- RemoteXY allows multiple users to connect to the device simultaneously and supports various communication methods for a single device in different combinations.
- With a single mobile application, you can control all your devices without limitations on the number of connected devices.
- The microcontroller library enables easy integration of additional communication methods that are not natively supported by the platform.

##### Supported communication methods:

- Over the Internet from anywhere via a cloud server;
- Bluetooth;
- Wi-Fi in both client and access point modes;
- Ethernet via IP address or URL;
- USB OTG.

##### Supported controllers::

- Arduino (UNO, Mega, Nano, Leonardo, etc.)
- ESP8266 (NodeMCU, Wemos D1, etc.)
- ESP32;
- STM32F1;
- nRF51822.

##### Supported external communication modules::

- Bluetooth HC-05, HC-06 or compatible
- Bluetooth BLE HM-10 or other UART BLE modules;
- ESP8266 as modem;
- Ethernet W5100, W5500;

##### Supported IDE:

- Arduino IDE;
- FLProg IDE;
- Visuino IDE.



