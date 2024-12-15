# Interaction with the Graphical Interface

The `loop()` function is the main function where the code for executing your task is placed, just like in a regular Arduino IDE sketch. It should also contain the command to call the handler function from the `RemoteXY.h` library.

```
void loop() 
{ 
  RemoteXY_Handler ();

  // TODO you loop code
}
```

To interact with the graphical interface, you need to interact with the fields of the `RemoteXY` structure. Each field of the structure represents a variable associated with its corresponding user interface element. By writing and reading the values of these variables, you gain access to the state of the user interface elements.

All fields of the structure are divided into input and output.

- **input** – data is primarily transferred from the graphical interface to the controller, but can also be transmitted in the opposite direction. You can read the values of these variables to get the current state of the control element as the user interacts with it. There are some limitations on writing values to these variables.
- **output** – data is always transferred from the controller to the graphical interface. You can freely modify the values of these variables in the controller. When reading the data, you will get the previously written value.

To send a value to a graphical interface element, write that value to the corresponding field of the structure.

```
void loop() 
{ 
  RemoteXY_Handler ();

  RemoteXY.temperature = getTemp ();
}
```

To determine the state of a graphical interface element or what data the user has entered, read the value from the corresponding field of the structure.

```
void loop() 
{ 
  RemoteXY_Handler ();

  if (RemoteXY.switch == 1) handleMyProcess ();
}
```

#### Changing an input variable from the controller

The state of an input variable can be modified from the controller. This allows you to set a control element to the desired state, for example, you can turn on a switch even without user interaction. To the user, it will appear as if the switch toggled automatically. You should also understand that the user may change the state of the control element at the same time in the graphical interface.

To change the state of a control element, simply write the new state value to the associated variable. This will trigger the process of sending the updated information. While the state of that control element is being transmitted through the communication channel to the mobile application, the controller will not receive new data about the state of this element, even if the user changes it from the graphical interface.

This new value should only be written to the variable once, and only when necessary. If you continuously update the input variable in each controller cycle, the user will never be able to change the state of the control element from the graphical interface.

***Note:*** Only modify the input variable on the controller side when necessary, and do so once.

#### Programming without delay()

The function `RemoteXY_Handler();` must be called regularly and frequently, ideally in every cycle of the `loop()`. 

When data is received from the mobile application, it is stored in buffers associated with each communication channel. When you call the `RemoteXY_Handler();` function, it reads and decodes the data from these buffers. If the handler is not called in a timely manner, the buffers will overflow, and the data will be lost, leading to a communication failure. It is crucial to avoid long delays between handler calls to ensure the proper functioning of the data exchange process.

If any part of the code takes a long time to execute, ensure that the handler is called periodically during its execution.

Replace all calls to the `delay()` function with the `RemoteXY_delay()` function. This function includes periodic calls to the `RemoteXY_Handler();` while the delay is active, ensuring that the handler is called during the delay and preventing communication issues.

Try modifying your code so that you no longer need to use the `delay()` function at all. This is typically done by using the controller's current time and calculating time intervals by calling the `millis()` function. This way, your code will become more responsive. If your code is paused by `delay()`, it cannot respond to any events. You may also miss user actions in the graphical interface and process them untimely.

