# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---


## Introduction

This project implemented a model predictive model to drive a car in a simulated track. The car commicates with the car via websocket, get the
waypoints while sending back speed and steering signals back to the simulator.

## MPC

The model is a kinematic bicycle model including position x, y; orientation psi; speed v; cross track error cte and psi error epsi.
Control signals are steering angle delta and acceleration a.

The model predicts the current state based on previous state with the following formulas:
`
x[t+1] = x[t] + v[t] * cos(psi[t]) * dt;
y[t+1] = y[t] + v[t] * sin(psi[t]) * dt;
psi[t+1] = psi[t] + v[t]/Lf * delta[t] * dt;
v[t+1] = v[t] + a[t] * dt;
cte[t+1] = f(x[t]) - y[t] + v[t] * sin(epsi[t]) * dt;
epsi[t+1] = psi[t] - psi_des + v[t]/Lf * delta[t] * dt;
`

## Timestep Length and Elapsed Duration (N & dt)

The timestep was 10 and elapsed duration was 0.1 second. This is a tradeoff between computation cost, accuracy. While high timesteps would predict
a longer period but low less accuracy. With lower N would lead to inefficiency in predicting the outputs for driving along the provied track.

## Polynomial Fitting and MPC Preprocessing

The waypoints were transformed into car's prospective, or in other words into car's coordinates. It's done by substracting the car position
from the waypoints. This locates car at the origin and heading orientation angle at 0. The Eigne vector was used for the polyfit and processing.

## Model Predictive Control with Latency

There is 0.1 second latency in telemetry, the car drifting due to the latency.
To deal with this, the position and oritentaion of the car was compensated before transformation.

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.

* **Ipopt and CppAD:** Please refer to [this document](https://github.com/udacity/CarND-MPC-Project/blob/master/install_Ipopt_CppAD.md) for installation instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

## Tips

1. It's recommended to test the MPC on basic examples to see if your implementation behaves as desired. One possible example
is the vehicle starting offset of a straight line (reference). If the MPC implementation is correct, after some number of timesteps
(not too many) it should find and track the reference line.
2. The `lake_track_waypoints.csv` file has the waypoints of the lake track. You could use this to fit polynomials and points and see of how well your model tracks curve. NOTE: This file might be not completely in sync with the simulator so your solution should NOT depend on it.
3. For visualization this C++ [matplotlib wrapper](https://github.com/lava/matplotlib-cpp) could be helpful.)
4.  Tips for setting up your environment are available [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)
5. **VM Latency:** Some students have reported differences in behavior using VM's ostensibly a result of latency.  Please let us know if issues arise as a result of a VM environment.
