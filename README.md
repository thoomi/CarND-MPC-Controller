## CarND Controls-MPC Project
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

Overview
---
This project is part of the Udacity Self-Driving Car Engineer Nanodegree program Term 2. 

If you are looking for instructions how to build and setup the project environment, please see the [INSTRUCTIONS.md](./INSTRUCTIONS.md) for more details.


### Project Goals
The goals/steps of this project are the following:
* Implement an MPC controller in order to steer a vehicle around a simulated track
* Implement the controller by using a vehicle model
* Deal with a simulated latency between the calculation and the actual actuation
* Tune the MPC controller hyperparameters 


### Results
[videothumb1]: ./results/mpc_example.png "MPC Controller Result"

[![Project video thumbnail][videothumb1]](./results/mpc_result.mp4?raw=true)

Speed: 80 Mph

### Reflection

#### The Model
The vehicle model from the class notes was used. The equations are implemented in the `MPC.cpp` 
file within the lines 122-127.

The vehicles state is represented by six variables:
* **x** - The x position
* **y** - The y position
* **psi** - The orientation of the vehicle
* **v** - The velocity of the vehicle
* **cte** - The Cross Track Error (Distance between current and the optimal position)
* **epsi** - The error of the vehicles orientation compared to the optimal orientation


The MPC-Controller calculates the following two actuator values:
* **steering_angle**
* **acceleration**

#### Timestep Length and Elapsed Duration (N & dt)

N   | dt
--- | ---
20  | 1 / N

The values are chosen to have a prediction horizon of 1 second. 
I started with an N value of 10 and increased the value while I tried to increase the vehicles overall speed. A higher speed needs more steps because 
the vehicle travels farther within one timestep.


#### Polynomial Fitting and MPC Preprocessing

The transformation of the given global position coordinates into values of the vehicle's 
coordinate system can be found in `main.cpp` within the lines 95-100.

Additionally, the coordinates are rotated by 90°. This layouts the fitted function horizontally and 
thus reduces the likelihood of multiple equal y values.

#### Model Predictive Control with Latency

In order to deal with the delay between the command calculation and the 
vehicles actuation I transformed the received state by 100ms into the future.
This future vehicle state is then the input for the MPC-Controller. The calculation
can be found in `main.cpp` at the lines 110-121.