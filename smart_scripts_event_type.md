# smart_scripts_event_type

It provides information about the types of events.

- SMART_EVENT_UPDATE_IC

| Name                  | Value | Param1     | Param2     | Param3    | Param4    | Param5 | Comment    |
|-----------------------|-------|------------|------------|-----------|-----------|--------|------------|
| SMART_EVENT_UPDATE_IC | 0     | InitialMin | InitialMax | RepeatMin | RepeatMax |        | In combat. |

**Example**

| event_type | event_param1 | event_param2 | event_param3 | event_param4 | event_param5 |
|------------|--------------|--------------|--------------|--------------|--------------|
| 0          | 7000         | 8000         | 10000        | 11000        | 0            |

- SMART_EVENT_UPDATE_OOC

| Name                   | Value | Param1     | Param2     | Param3    | Param4    | Param5 | Comment        |
|------------------------|-------|------------|------------|-----------|-----------|--------|----------------|
| SMART_EVENT_UPDATE_OOC | 1     | InitialMin | InitialMax | RepeatMin | RepeatMax |        | Out of combat. |

**Example**

| event_type | event_param1 | event_param2 | event_param3 | event_param4 | event_param5 |
|------------|--------------|--------------|--------------|--------------|--------------|
| 1          | 0            | 0            | 180000       | 180000       | 0            |
