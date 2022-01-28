[TOC]

# VideoCardEvent

```
#!json

 {
        "Event" : "VideoCardEvent",
        "EventID" : "0",
        "Command" : "EnableOutput",
        "Number" : "0",
        "Value" : "0.0",
        "ReferenceMode" : "FreeRun"
 }
```


**Command**:

-   EnableOutput
-   DisableOutput
-   EnableKey
-   DisableKey
-   SetReferenceMode
-   SetReferenceOffsetH
-   SetReferenceOffsetV
-   EnableInput
-   DisableInput
-   GetReferenceSignalInfo
-   IsLocked

**ReferenceMode**:

-   FreeRun
-   AnalogBlackBurst
-   AnalogTriLevel
-   DigitalInput1
-   DigitalInput2
-   Interlock
-   AnalogBlackBurstAux
-   AnalogTriLevelAux

## Commands

### EnableOutput
### DisableOutput
### EnableKey
### DisableKey
### SetReferenceMode


```
#!json

{
	"Event" : "VideoCardEvent",
	"Command" : "SetReferenceMode",
	"EventID" : "333",
    "VideoCardID" : "0",
    "ReferenceMode" : "AnalogBlackBurst"
}
```

### SetReferenceOffsetH


```
#!json
{
	"Event" : "VideoCardEvent",
	"Command" : "SetReferenceOffsetH",
	"EventID" : "333",
    "VideoCardID" : "0",
    "Number" : "123"
}

```


### SetReferenceOffsetV


```
#!json

{
	"Event" : "VideoCardEvent",
	"Command" : "SetReferenceOffsetV",
	"EventID" : "333",
    "VideoCardID" : "0",
    "Number" : "123"
}
```


### EnableInput
### DisableInput
### GetReferenceSignalInfo


```
#!json

{
	"Event" : "VideoCardEvent",
	"Command" : "GetReferenceSignalInfo",
	"EventID" : "333",
    "VideoCardID" : "0"
}
```

**Example response:**


```
#!json
{
   "EventID" : "333",
   "Success" : "true",
   "cmd" : "GetReferenceSignalInfo",
   "referenceSignal" : {
      "referenceH" : "123",
      "referenceMode" : "AnalogBlackBurst",
      "referenceV" : "0"
   }
}
```



### IsLocked