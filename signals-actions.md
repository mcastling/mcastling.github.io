
# Signals and Actions

## Signals

Classes in DALi provide 'signals' - methods invoked whenever a certain action or event occurs.
The application can connect to these signals if it requires notification of this event.

A class method can be connected to the signals if local data needs to be accessed.
Standard C Style functions can be used to connect to these signals if local data does not need to be accessed.

DALi provides safe automatic signal disconnection when the connecting object is deleted.
Applications can also manually disconnect from signals when required. 

## Actions

Classes in Dali can provide 'actions' - functionality which can be invoked either directly via the
API, or 'scripting' using a generic action method. A simple example is setting visibility of an actor - 
SetVisible() or Obj.DoAction("SHOW"). Another example is animations which provide the ability to
"play" or "stop" using an action. The scripting approach simplifies common fucntionality.


