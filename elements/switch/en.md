# Switch

The **Switch** control element has two states, for example, "Off" and "On". The user changes the switch's position by dragging the slider, altering its state to perform some action. The state information of the **Switch** is transmitted to the controller. The **Switch** can have different shapes and colors, and may contain text.

The **Switch** is located on the **Control** tab in the left panel of the editor. Click on the image of the switch and drag it onto the editor screen.

![en_01](en_01.jpg)

### Data

| Data         | Type    | Value                                                        | Direction |
| ------------ | ------- | ------------------------------------------------------------ | --------- |
| Switch state | uint8_t | 0 - off, the switch is in the left position,<br/>1 - on, the switch is in the right position. | input     |

### Configuration

![en_03](en_03.jpg)

- **Variable.** Specifies the name of the variable that will be bound to the switch. The name is set according to the rules for naming C++ variables.

- **Draw style.** Specifies the appearance of the switch.

- **Color.** The color of the switch.

- **Background color.** The background  color of the switch.

  ##### Switch ON

- **Caption**: The text and color to be displayed on the right side of the switch when it is turned on.

  ##### Switch OFF

- **Caption**: The text and color to be displayed on the left side of the switch when it is turned off.

### Display options

![en_02](en_02.jpg)

As a text, you can use special encoding characters, such as emojis. To do this, find the symbol you need, for example, in a browser on any website, and copy it into the **Caption** field in the element settings.

### How it works

When the user moves the switch lever to the right or left position, the state of the switch changes. The state is transmitted to the controller through the associated variable. When the position of the switch changes in the graphical interface, the value of the associated variable will be updated.

- When the switch is in the left position, it is OFF, and the variable value is 0.
- When the switch is in the right position, it is ON, and the variable value is 1.

##### Change the state from the controller

The state of the switch can be changed from the controller. To do this, you need to write the desired state of the switch to the associated variable. **Note:** If you constantly write a new value to the switch variable in every loop cycle, the user will not be able to change it. Only change the state of the switch on the controller side when necessary, and do this once.

##### Set initial state

You can set the initial state of the switch when the controller starts. To do this, in the `setup()` function, write the initial value to the associated variable.

### Source code examples

Check the state of the switch.

```
void loop () {
  RemoteXY_Handler ();
  
  if (RemoteXY.switch != 0) {
    // TODO Now the switch is on
  }
  else {
    // TODO Now the switch is off
  }
}
```

Get the state change moment

```
uint8_t prevSwitchState = 0;

void loop () {
  RemoteXY_Handler ();
  
  if (RemoteXY.switch != prevSwitchState) {
    if (RemoteXY.switch != 0) {
      // TODO catch the moment when it was turned on
    }
    else {
      // TODO catch the moment when it was turned off
    }    
  }
  prevSwitchState = RemoteXY.switch;
}
```

Change the state of the controller output contact

```
#define PIN_SWITCH 13

void setup () {
  pinMode(PIN_SWITCH, OUTPUT);
}

void loop () {
  RemoteXY_Handler ();
  
  if (RemoteXY.switch != 0) {
    digitalWrite(PIN_SWITCH, HIGH);
  }
  else {
    digitalWrite(PIN_SWITCH, LOW);
  }
} 
```

Change the state of a switch in the GUI

```
uint8_t getMotorState () {
  // TODO return current motor state, 0 or 1
}

uint8_t prevMotorState = 0;

void loop () {
  RemoteXY_Handler ();
  
  if (prevMotorState != getMotorState ()) {
    // changes only at the moment when the engine is turned on and off
    RemoteXY.switch = getMotorState ();
  }
  prevMotorState = getMotorState ();
}
```

Control of motor on/off. The motor can be turned on either from the graphical interface or automatically by the control program based on some condition. In this case, when the motor is turned on automatically, the switch state in the interface should also be updated.

```
uint8_t getMotorState () {
  // TODO return current motor state, 0 or 1
}
void setMotorOn () {
  // TODO steps to turn on the motor
}
void setMotorOff () {
  // TODO steps to turn off the motor
}

uint8_t prevMotorState = 0;
uint8_t prevSwitchState = 0;

void loop () {
  RemoteXY_Handler ();
  
  if (prevMotorState != getMotorState ()) {
    RemoteXY.switch = getMotorState ();
  }
  prevMotorState = getMotorState ();
  
  if (RemoteXY.switch != prevSwitchState) {
    if (RemoteXY.switch != getMotorState ()) {
      if (RemoteXY.switch != 0) {
        setMotorOn ();
      }
      else {
        setMotorOff ();
      }
    }
  }
  prevSwitchState = RemoteXY.switch;

}
```

Set the initial state of the switch when the controller starts.

```
void setup () {
  RemoteXY_Init ();  
  RemoteXY.switch = 1; 
}
```

### 

