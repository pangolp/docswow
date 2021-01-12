# smart_scripts_event_type

It provides information about the types of events.

### SMART_EVENT_UPDATE_IC

This event is used when entering a combat.

| Name                  | Value | Param1     | Param2     | Param3    | Param4    | Param5 |
|-----------------------|-------|------------|------------|-----------|-----------|--------|
| SMART_EVENT_UPDATE_IC | 0     | InitialMin | InitialMax | RepeatMin | RepeatMax |        |

**Example**

| event_type | event_param1 | event_param2 | event_param3 | event_param4 | event_param5 |
|------------|--------------|--------------|--------------|--------------|--------------|
| 0          | 7000         | 8000         | 10000        | 11000        | 0            |

Initially, between 7 and 8 seconds, it is executed for the first time, and then repeated, between 10 and 11 seconds.

It could be associated, for example, with the use of a spells.

### SMART_EVENT_UPDATE_OOC

This event, is used, when going out of combat

| Name                   | Value | Param1     | Param2     | Param3    | Param4    | Param5 |
|------------------------|-------|------------|------------|-----------|-----------|--------|
| SMART_EVENT_UPDATE_OOC | 1     | InitialMin | InitialMax | RepeatMin | RepeatMax |        |

**Example**

| event_type | event_param1 | event_param2 | event_param3 | event_param4 | event_param5 |
|------------|--------------|--------------|--------------|--------------|--------------|
| 1          | 0            | 0            | 180000       | 180000       | 0            |

It has no start time, and directly repeats itself after 18 seconds.

It could be associated, for example, with the use of a spells.


### SMART_EVENT_HEALTH_PCT

| Name                   | Value | Param1 | Param2 | Param3    | Param4    | Param5 |
|------------------------|-------|--------|--------|-----------|-----------|--------|
| SMART_EVENT_HEALTH_PCT | 2     | HPMin% | HPMax% | RepeatMin | RepeatMax |        |

**Example**

| event_type | event_param1 | event_param2 | event_param3 | event_param4 | event_param5 |
|------------|--------------|--------------|--------------|--------------|--------------|
| 2          | 1            | 30           | 15000        | 20000        | 0            |

Between 1 and 30% health, and between 15 and 20 seconds.

Generally used to use healing spells, to increase the damage with (enrage for example)
