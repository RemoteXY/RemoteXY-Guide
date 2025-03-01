# Source Code Overview

The graphical interface editor enables you to generate source code for a microcontroller. The generated code is fully functional, allowing you to test your graphical interface immediately.

In the Arduino IDE, select your board, compile the code, and upload it to the board. Check how the graphical interface works in the mobile application. It is recommended to verify the interface functionality before modifying the code. After testing, you can change the code to suit your specific task.

This section provides an overview of the components of the source code and their purposes.

### Includes and defines

This is the initial section of the source code, which includes several `#define` directives and additional library includes that set up the communication method and other necessary parameters. Following these definitions, the `RemoteXY.h` library is included. An example snippet of the code:

```
// you can enable debug logging to Serial at 115200
//#define REMOTEXY__DEBUGLOG    

// RemoteXY select connection mode and include library
#define REMOTEXY_MODE__SOFTSERIAL

#include <SoftwareSerial.h>

// RemoteXY connection settings 
#define REMOTEXY_SERIAL_RX 2
#define REMOTEXY_SERIAL_TX 3
#define REMOTEXY_SERIAL_SPEED 9600

#include <RemoteXY.h>
```

The first line, `#define REMOTEXY_MODE__XXXX`, defines the communication method that will be used to connect to the mobile application. The communication method is selected in the graphical interface editor. In the code example above, the communication method is set to use a software serial port, which might be connected to a Bluetooth communication module. RemoteXY supports a variety of communication methods, and this definition may have different values.

Next, libraries required for implementing the selected communication method are included. In the example above, the `#include <SoftwareSerial.h>` library is included. The `RemoteXY.h` library uses other standard libraries to establish communication. If a different communication method is selected, you may see other libraries being included.

Next, configuration parameters for the selected communication method are defined. Each communication method has its own unique set of parameters. These parameters are also configured in the graphical interface editor, but they can be modified directly in the code if needed. For example, in the code above, you can easily change the pins for the software serial port and the communication speed.

At the end of all definitions, the `#include <RemoteXY.h>` library is included, which will be configured according to the parameters defined above.

The example also contains a commented line `//#define REMOTEXY__DEBUGLOG`. If you uncomment it, the `RemoteXY.h` library will be configured to output debug information to the Serial port at a speed of 115200. You can change the debug output direction if necessary, for example, as follows:

```
#define REMOTEXY__DEBUGLOG
#define REMOTEXY__DEBUGLOG_SERIAL Serial
#define REMOTEXY__DEBUGLOG_SPEED 9600
```

Debug information can be helpful if you encounter difficulties establishing a connection with the mobile application or in other situations.

### Configuration of the graphical interface

The next section of the code contains the `RemoteXY_CONF` array, which uniquely defines the appearance and data structure of the graphical interface.

```
// RemoteXY GUI configuration   
#pragma pack(push, 1)  
uint8_t RemoteXY_CONF[] =   // 46 bytes
  { 255,1,0,4,0,39,0,19,0,0,0,0,31,1,106,200,1,1,2,0,
  2,29,19,44,22,0,2,26,31,31,79,78,0,79,70,70,0,67,29,72,
  40,10,78,2,26,2 };
```

This array of numbers is used by both the `RemoteXY.h` library and the mobile application to create and open the graphical interface. The array uses a unique data packing format. You do not need to try to understand how the data is packed in this array. Simply use it as you received it from the GUI editor.

If you need to make changes to the interface, modify it in the interface editor and then get the new code. If you begin to modify the data in this array manually, it is likely that the data will not be unpacked correctly by the mobile application.

The code for your graphical interface is not stored on the internet on any server. It is only stored in your controller within this array of numbers. When the mobile app connects to the controller, it will receive this array, unpack it, and display the graphical interface.

The line #pragma pack(push, 1) instructs the compiler not to align the subsequent data to the controller's word size (it uses 1-byte alignment). Just do not remove it.

### The structure of variables.

The next section of the code defines the structure of the `RemoteXY` interface variables. This is the main data structure that you will use to interact with the graphical interface.

```
// this structure defines all the variables and events of your control interface  
struct {

    // input variables
  uint8_t switch_01; //  =1 if switch ON and =0 if OFF

    // output variables
  float value_01;

    // other variable
  uint8_t connect_flag;  // =1 if wire connected, else =0

} RemoteXY;   
#pragma pack(pop)
```

The structure contains all variables related to the graphical interface elements. The composition and order of the structure's variables are strictly important. The sequence of variables in the structure is directly related to the `RemoteXY_CONF` array and cannot be considered separately. If you make changes in the interface editor and generate new source code, you must copy both the `RemoteXY_CONF` array and the `RemoteXY` variable structure together. Do not modify the structure's composition or change the order of variables. Use the variable structure as it was generated by the interface editor.

The line `#pragma pack(pop)` instructs the compiler to stop adjusting the data to match the controller's word alignment. Just do not remove it.

More about the [variable structure](/code/structure/en.md).

### The Setup function

The `setup()` function contains the initialization command for the `RemoteXY.h` library. In this function, you also place the code for initializing your main task, just as you would in a regular Arduino IDE sketch.

```
void setup() 
{
  RemoteXY_Init (); 

  // TODO you setup code
}
```

The function call `RemoteXY_Init();` initializes the necessary data structures for the operation of the library.

In the `setup()` function, you can perform the initial initialization of variables associated with the graphical interface elements.

For more details about what can be done in the [setup function](/code/setup/en.md).

### The Loop function

The `loop()` function contains the command to call the handler of the `RemoteXY.h` library. In this function, you also place the code for your main task and interaction with the graphical interface, just like you would in a regular Arduino IDE sketch.

```
void loop() 
{ 
  RemoteXY_Handler ();

  // TODO you loop code
}
```

The function `RemoteXY_Handler();` cyclically handles all data exchange with the mobile application and should be called frequently, at least once in every cycle of the `loop()`.

For more details on how to interact with the graphical interface in the `loop()` function, see the [loop() function](/code/interaction/en.md).



