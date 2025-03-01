# Programming without delay

The function `RemoteXY_Handler();` must be called regularly and frequently, ideally in every cycle of the `loop()`. 

```
void loop() 
{ 
  RemoteXY_Handler ();
}
```

When data is received from the mobile application, it is stored in buffers associated with each communication channel. When you call the `RemoteXY_Handler();` function, it reads and decodes the data from these buffers. If the handler is not called in a timely manner, the buffers will overflow, and the data will be lost, leading to a communication failure. It is crucial to avoid long delays between handler calls to ensure the proper functioning of the data exchange process.

If any part of the code takes a long time to execute, ensure that the handler is called periodically during its execution.

Replace all calls to the `delay()` function with the `RemoteXY_delay()` function. This function includes periodic calls to the `RemoteXY_Handler();` while the delay is active, ensuring that the handler is called during the delay and preventing communication issues.

```
void loop() 
{ 
  RemoteXY_Handler ();
  
  if (RemoteXY.button != 0) {
    digitalWrite (LED_BUILTIN, HIGH);  
    RemoteXY_delay (1000);  // wait for a second
    digitalWrite (LED_BUILTIN, LOW);   
    RemoteXY_delay (1000);  // wait for a second
    digitalWrite (LED_BUILTIN, HIGH);  
    RemoteXY_delay (1000);  // wait for a second
    digitalWrite (LED_BUILTIN, LOW);   
  }
}
```

Keep in mind that using `RemoteXY_delay()` only ensures that the connection with the controller won't be lost due to buffer overflow. However, your code is still paused and does not respond to external events or graphical interface events. In the previous example, the code wouldn't respond to a button press for a full 2 seconds.

#### Don't use delays at all

If your code is paused, it cannot respond to any events. Try modifying your code so that you don't need to use the `delay()` or `RemoteXY_delay()` functions at all. This is typically done by using the controller's current time via `millis()` and calculating time intervals. You'll also need a variable to store the current state. The following example shows how to make the previous code more responsive.

```
int ledState = 0;
uint32_t ledTime = 0;

void loop() 
{ 
  RemoteXY_Handler ();
  
  uint32_t currentMillis = millis();
  
  if (RemoteXY.button == 1) { // Checking state of GUI button
    ledState = 1;
    ledTime = currentMillis;
  }
  
  if (ledState > 0) {
    if (currentMillis - ledTime > 1000) {
      ledState = ledState + 1;
      ledTime = currentMillis;
      if (ledState == 4) {
        ledState = 0;
      }
    }
  }
  
  if ((ledState == 1) || (ledState == 3)) {
    digitalWrite (LED_BUILTIN, HIGH); 
  }
  else {
    digitalWrite (LED_BUILTIN, LOW); 
  }

}
```



