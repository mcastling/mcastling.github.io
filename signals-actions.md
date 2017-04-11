
# Signals and Actions

In DALI applications a _signal and slot_ mechanism is used for communication between objects.

A **signal** is a notfication from a DALi object containing event information.
A **slot** is an application function that recieves the signal - an event handler.

Examples of signals used in Dali are Touch data, Key events and Input signals.

## Signals

Objects in DALi can provide 'signals' to inform an application of user actions or events.
The application can connect to these signals if it requires notification of these events.

A class method can be connected to the signals if local data needs to be accessed. If local data does
not need to be accessed standard C Style functions can be used.

DALi provides safe automatic signal disconnection when the connecting object is deleted.
Applications can also manually disconnect from signals when required. 

## Actions

Classes in Dali can provide 'actions'. For example animations which provide the ability to
"play" or "stop" using an action. Actions can be invoked either directly via the
API, or 'scripting' using a generic action method. A simple example is setting visibility of a View - 
via SetVisible() or Obj.DoAction("SHOW"). The scripting approach simplifies common functionality.


