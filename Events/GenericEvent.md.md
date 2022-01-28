[TOC]

# Generic Event

## Commands

### SetWeightCenter

### AdjustToScreen

### DebugEventsCount

Event powstał w celach debugowych. Zwraca liczbę eventów, który dotarły do silnika oraz liczbę odpowiedzi, które zostały wysłane od początku działania silnika.



```
#!c++

{
    "Event" : "GenericEvent",
    "Command" : "DebugEventsCount"
}

```


```
#!c++

{
   "EventID" : "2147483647",
   "EventsCount" : "9",
   "ReponseCount" : "10",
   "Success" : "true",
   "cmd" : "DebugEventsCount"
}
```