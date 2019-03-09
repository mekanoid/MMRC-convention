Modern Model Railroad Control - Documentation for MMRC in swedish and english.

## Documentation
For the MMRC concept you can find the following documentation. You can also find Readme files in the repositories for each of the MMRC software and the source codes are fully documented.


### MMRC Overview
[View the document](MMRC Overview.md).
_Swedish_

This document decribes the concept of MMRC and explains how to use it and how it communicates. It is a **work in progress**.

### MMRC Convention
[View the document](MMRC Convention.md).
_English_

The convention defines the structure of MQTT topics and the properties to use in MMRC.



### MMRC Properties
[View the document](MMRC Properties.md).
_English_

Description of the different payloads all MMRC properties should use. This document is a **work in progress**.


## Software
Since each MMRC client has its own specific software, there are several respositories to choose from. Right now you can find the following:


### MMRC Start
[View the repositiory](https://github.com/mekanoid/MMRC-start)

Here you can find a basic application that turns off/on the built-in LED on a Arduino board. To use as a start for your own MMRC client project!


### MMRC Two turnout
[View the repositiory](https://github.com/mekanoid/MMRC-twoturnout)

This application controls two separate 2-way turnouts independently. You can control each turnout with a physical button or from a remote client/MQTT app and you can have LEDs indicating the turnout position.