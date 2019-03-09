# MMRC Properties

Version: **0.1.0**

Date: **9 Mar 2019**

This document defines which payloads different MMRC properties may have.

## Layout properties


### Signal
For the **`mmrc/[device ID]/[node ID]/signal/set`** topic you can have the following payloads:

| Topic       | Description                                                   | Retained  |
|:------------|:--------------------------------------------------------------|-----------|
| go          |                                                               | No        |
| stop        |                                                               | No        |
| slow        |                                                               | No        |

For example:

```java
mmrc/deviceID/nodeID/signal/set → "stop"
mmrc/deviceID/nodeID/signal → "stop"
```


### turnout
For the **`mmrc/[device ID]/[node ID]/turnout/set`** topic you can have the following payloads:

| Payload     | Description                                                   | Retained  |
|:------------|:--------------------------------------------------------------|-----------|
| normal      | Move a 2-way turnout into "straight forwad" position          | No        |
| reverse     | Move a 2-way turnout into "turning" position                  | No        |


And for the **`mmrc/[device ID]/[node ID]/turnout`** topic you can have the following payloads:

| Payload     | Description                                                   | Retained  |
|:------------|:--------------------------------------------------------------|-----------|
| normal      | The turnout has been moved into "straight forward" position   | Yes       |
| reverse     | The turnout has been moved into "turning left/right" position | Yes       |
| moving      | Optional status to set when turnout is moving into position   | Yes       |


For example:

```java
mmrc/deviceID/nodeID/turnout/set → "normal"
mmrc/deviceID/nodeID/turnout → "moving"
mmrc/deviceID/nodeID/turnout → "normal"

```

## Operations properties

### fastclock

### warrent (tåganmälan)

## Train properties


## Accessory properties

### button

### output

