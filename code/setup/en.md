# Initialization of the Graphical Interface

The initialization of the graphical interface is performed in the `Setup()` function, just like in a regular Arduino IDE sketch.

```
void setup() 
{
  RemoteXY_Init (); 

  // TODO you setup code
}
```

It is mandatory to call the function `RemoteXY_Init ();`. This function initializes the necessary data structures for the operation of the `RemoteXY.h` library.

After that, graphical interface elements can be initialized.

Learn where the [data of the graphical interface elements](/code/structure/en.md) are stored.

#### Initializing Graphical Interface Elements

You can set the initial state of any graphical interface elements when the controller starts. For example, you can define the state of a switch as "on" or set an initial value in a number input field.

This can be done by writing the initial value to the variable associated with the element. This code should be placed in the `setup()` function, after initializing the `RemoteXY.h` library itself.

```
void setup () {
  RemoteXY_Init ();  
  
  RemoteXY.switch = 1; // switch on
}
```

#### Restoring Graphical Interface Elements After Reset

To preserve the initial state of a control element after a controller reset, you can save the value of the associated variable to EEPROM whenever it changes. Upon startup, the controller should restore the value of the variable from EEPROM and load it into the associated variable.

```
uint8_t readSwitchStateFromEEPROM () {
  // TODO read eeprom
}
void writeSwitchStateToEEPROM (uint8_t state) {
  // TODO write eeprom
}

void setup () {
  RemoteXY_Init ();  
  RemoteXY.switch = readSwitchStateFromEEPROM ();
}

uint8_t prevSwitchState = 0;

void loop () {
  RemoteXY_Handler ();
  
  if (RemoteXY.switch != prevSwitchState) {
    writeSwitchStateToEEPROM (RemoteXY.switch);   
  }
  prevSwitchState = RemoteXY.switch;
}
```

