# Interaction with the Graphical Interface

Interaction with the graphical interface is carried out through the fields of the `RemoteXY` structure. The structure's fields correspond to the graphical interface variables. Each field of the structure is linked to a variable of a specific graphical interface element. The variable name is assigned in the interface editor for each element. By writing and reading variable values, you gain access to the state of the graphical interface controls.

```
struct {    

  // input variables  
  uint8_t switch_01;   
  float setTemperature;  
  
  // output variables  
  float temperature;    
  uint8_t led_01;   
  
} RemoteXY;
```

All fields of the structure are divided into input and output.

- **input** – data is primarily transmitted from the graphical interface to the controller.
- **output** – data is always transmitted from the controller to the graphical interface.



> **Attention.** Do not modify the `RemoteXY` structure. Do not add or remove fields from the `RemoteXY` structure. Also, do not change the order of fields in the structure. Any changes will lead to interface failures.



#### Retrieving Data from the Graphical Interface

To determine the current state of a graphical interface element or the data entered by the user, read the value from the corresponding structure field.

```
void loop() 
{ 
  RemoteXY_Handler ();
  
  if (RemoteXY.switch_01 == 1) {
    handleMyProcess ();
  }
}
```

#### Sending Data to the Graphical Interface

To send a value to a graphical interface element, write this value into the corresponding structure field.

```
void loop() 
{ 
  RemoteXY_Handler ();
  
  RemoteXY.temperature = getTemp ();
}
```

#### Initial Interface Initialization

You can easily set the initial values for input variables that should be established after a reset. For example, you might need to set an initial temperature value different from zero. This can be done in the `setup()` function after initializing the interface.

```
void setup () {
  RemoteXY_Init ();  
  
  RemoteXY.setTemperature = 25; 
}
```

