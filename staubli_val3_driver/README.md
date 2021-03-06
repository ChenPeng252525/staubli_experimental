# ROS-Industrial driver (server) for Staubli robots

## Overview

This ROS-I driver was developed in Staubli's VAL3 language for use with 6-axis
Staubli robot manipulators.

It is advisable to try this driver on Staubli's emulator first.


## Requirements

* Staubli 6-axis robot manipulator
* Staubli CS8 controller
  * it may work with other controllers, but this driver was tested on CS8 only
* VAL3 version s7.7.2 or greater
  * this is very important, since this implementation uses return values of `sioGet()`
    only available from s7.7.2 onwards


## Installation

Installing the driver to a Staubli controller simply consists of transferring the
contents of the `val3_driver` folder to the controller itself.

### Clone this repository

Clone branch `val3_server-devel` of [staubli_experimental](https://github.com/ocadotechnology/staubli_experimental):

```shell
git clone https://github.com/ocadotechnology/staubli_experimental -b val3_server-devel
```

### Transfer driver to Staubli CS8 controller

There are several ways of transferring VAL3 applications to a Staubli controller.
The simplest method is probably copying the contents of `val3_driver` folder into
a USB memory stick (<2GB given the CS8 limitations), plugging the stick to the
controller and using the teach pendant to copy the folders.

### Open the VAL3 application with Staubli SRS

Although it is possible to edit the source files with any text editor (they are
essentially XML files), it is advisable to use the Staubli Robotics Suite:

* Copy contents of folder `val3_driver` into the `usrapp` folder of the Staubli cell
* Open the `ros_server` VAL3 project located inside the `ros_server` folder

SRS offers autocompletion, syntax highlighting and syntax checking, amongst other
useful features, such as a Staubli controller/teach pendant emulator.


## Usage

### Load driver from (real or emulated) teach pendant

From `Main menu`:

1. Application manager --> Val3 applications
2. +Disk --> ros_server

### Configuration

The TCP sockets on the CS8 controller/emulator must be configured prior to using
the driver, otherwise a runtime error will be displayed on the teach pendant and
the driver will not work.

Two sockets (TCP Servers) are required. From `Main menu`:

1. Control panel --> I/O --> Socket --> TCP Servers
2. Configure two sockets
   * Name: Feedback, Port: 11002, Timeout: -1, Delimiter: 13, Nagle: Off
   * Name: Motion, Port: 11000, Timeout: -1, Delimiter: 13, Nagle: Off

### Run the driver (ROS-I server)

Check that:

1. The contents of the `val3_driver` folder (both `ros_server` and `ros_libs` folders)
have been transferred to the Staubli controller
2. The VAL3 application `ros_server` has been loaded
3. Both TCP Server sockets have been configured properly

Then simply press the `Run` button, ensure that `ros_server` is highlighted,
then press `F8` (Ok).

Notice that depending on which mode of operation is currently active, the motors
may need to be enabled manually (a message will pop up on the screen). Likewise,
the robot will only move if the `Move` button has been pressed (or is kept pressed
if in manual mode).

### Run the industrial_robot_client node (ROS-I client)

The `val3_server-devel` branch provides launch files (within the `staubli_val3_driver`
ROS package). Simply run:

```shell
roslaunch staubli_val3_driver robot_interface_streaming.launch robot_ip:=<CS8 controller IP address>
```

## Bugs, suggestions and feature requests

Please report any bugs you may find, make any suggestions you may have and request
new features you may find useful via GitHub.
