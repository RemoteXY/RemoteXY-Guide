# Структура переменных графического интерфейса

Структура переменных графического интерфейса `RemoteXY` предоставляет доступ к элементам графического интерфейса, таким как кнопки, ползунки, поля ввода и т. д. Структура переменных `RemoteXY` генерируется редактором графических интерфейсов автоматически вместе с исходным кодом. Состав полей структуры полностью соответствует элементам управления и содержит все переменные связанные с элементами управления графического интерфейса. Имена переменных задаются в редакторе графического интерфейса для каждого элемента управления. Для некоторых элементов графического интерфейса можно указать тип переменной. Другие элементы управления однозначно определяют тип переменной. Эти имена и типы переносятся в поля структуры `RemoteXY` . 

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

Структура  `RemoteXY` предоставляет переменные в особом порядке. В начале следуют входные переменные, затем выходные. Соблюдение этого порядка необходимо для обмена данными с мобильным приложением. Не меняйте порядок следования полей в структуре  `RemoteXY` , это приведет к неработоспособности графического интерфейса.

> **Внимание.** Нельзя изменять структуру `RemoteXY`. Не добавляйте и не удаляйте поля структуры `RemoteXY`. Так же не изменяйте порядок полей в структуре. Любые изменения приведут к неработоспособности интерфейса.

#### Взаимодействие с графическим интерфейсом

Что бы получить данные из графического интерфейса необходимо прочитать значения полей структуры или `RemoteXY`. Что бы изменить отображаемые данные элементов интерфейса  необходимо изменить значения полей структуры.

```
void loop() 
{ 
  RemoteXY_Handler ();
  
  RemoteXY.temperature = getTemp ();
  if (RemoteXY.switch_01 == 1) {
    handleMyProcess ();
  }
}
```

#### Где действительно хранятся данные графического интерфейса

Система дистанционного управления **RemoteXY** не использует какие либо серверы для хранения состояния вашего графического интерфейса. Это означает что ваши данные не передаются куда либо еще. Они передаются только между контроллером и мобильным приложением.

Текущее состояние элементов графического интерфейса целиком и полностью хранится в структуре  `RemoteXY` . Каждое поле этой структуры хранит состояние одного из элементов графического интерфейса, его положение, значение или какие либо другие данные. 

Когда мобильное приложение подключается к контроллеру, в первую очередь оно считывает данные структуры  `RemoteXY` . Далее мобильное приложение отображает графический интерфейс в том состоянии, в котором находились данные в полях структуры. Когда пользователь взаимодействует с графическим интерфейсом, новые данные будут сохранены обратно в поля структуры. При закрытии графического интерфейса данные о его состоянии остаются в контроллере в структуре. Если затем подключиться к контроллеру с другого телефона, мобильное приложение на другом телефоне так же считает данные из структуры `RemoteXY` и отобразит графический интерфейс точно так же в том же виде.

#### Направление передачи данных

Все переменные структуры разделены на входные и выходные. Это разделение зависит от вида элемента управления с которым связана переменная. Если элемент управления только отображает данные передаваемые из контроллера, то переменная связанная с ним будет иметь направление **output**. Если элемент управления может взаимодействовать с пользователем и пользователь может изменить его состояние, то данные элемента будут передаваться из графического интерфейса в контроллер, а переменная связанная с ним будет иметь направление  **input**. Однако переменные **input** могут передаваться и в обратном направлении.

- **input** - данные преимущественно передаются из графического интерфейса в контроллер, но могут передаваться и в обратном направлении. Вы можете считывать значения этих переменных получая текущее состояние элемента управления как взаимодействует с ним пользователь. На запись значений накладываются некоторые ограничения.
- **output** - данные всегда передаются из контроллера в графический интерфейс. В контроллере можно свободно изменять значения этих переменных. При считывании данных вы получите предыдущее записанное значение.

#### Изменить input переменную из контроллера.

Состояние входной переменной может быть изменено из контроллера. Тем самым вы можете установить элемент управления в нужное вам состояние. Например вы можете установить переключатель в положение включен даже без пользователя. Для пользователя это будет выглядеть как будто переключатель переключился сам. Вы так же должны понимать, что пользователь в графическом интерфейсе может изменить состояние элемента управления в тот же момент времени. 

Что бы изменить состояние элемента управления достаточно записать значение нового состояния в связанную переменную. В этот момент запустится процесс обратной передачи информации. Пока состояние такого элемента управления передается по каналу связи в мобильное приложение, контроллер не будет получать новые данные о состоянии этого элемента, даже если пользователь изменит его из графического интерфейса. 

Такое новое значение должно быть записано в переменную только один раз и только в момент когда такая необходимость появилась. Если записывать новое значение входной переменной в каждом цикле контроллера, то пользователь никогда не сможет изменить состояние этого элемента управления из графического интерфейса. 

> ***Внимание**.* Изменяйте состояние входной переменной на стороне контроллера только тогда и в тот момент времени когда это необходимо, делайте это один раз.

#### Начальное состояние элементов управления

Когда происходит старт контроллера, его сброс, или может быть случайная перезагрузка, то все переменные в структуре  `RemoteXY` устанавливаются в исходное состояние. Все переменные получают значение 0. Когда вы откроете графический интерфейс после сброса контроллера, все элементы управления окажутся так же в исходном состоянии. 

Если вам необходимо задать какое либо начальное значение элемента управления, вы можете установить это значения в соответствующее поле структуры   `RemoteXY` . Сделать это необходимо в функции `Setup()`.

Подробнее о том [как инициализировать элементы управления](/code/setup/ru.md).

#### Что произойдет когда контроллер сбрасывается

Случайный или не случайный сброс контроллера так же вызывает потерю данных в полях структуры  `RemoteXY` . Если вы хотите сохранить состояние каких либо элементов управления, вы должны позаботиться о восстановлении данных в соответствующих полях структуры. Вы должны позаботиться об этом самостоятельно, например сохранять данные в энергонезависимую память.

Подробнее о том как сохранить и [восстановить состояние элеменов управления](/code/setup/ru.md) при сбросе контроллера.

