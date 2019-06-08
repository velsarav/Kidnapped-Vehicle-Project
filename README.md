# Kidnapped Vehicle Project 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

WIP
## Project Introduction
The robot has been kidnapped and transported to a new location! Luckily it has a map of this location, a (noisy) GPS estimate of its initial location, and lots of (noisy) sensor and control data.

The goal of this project is to implement a 2 dimensional particle filter in C++. The
particle filter will be given a map and some initial localization information (analogous to what a GPS would provide). At each time step the filter will also get observation and control data.

## Rubric Points

The [rubric](https://review.udacity.com/#!/rubrics/747/view) points were individually addressed in the [implementation](https://github.com/velsarav/Kidnapped-Vehicle-Project/tree/master/src)

Please refer to the base project [README](https://github.com/udacity/CarND-Kidnapped-Vehicle-Project) to compile C++ code and run the simulator.

[//]: # (Image References)

[image1]: ./docs/Algorithm_steps.png "Algorithm"
[image2]: ./docs/Success_result.png "Performance"
[image3]: ./docs/Initialization.png "Initialization"
[image4]: ./docs/Prediction.png "Prediction"
[image5]: ./docs/Update.png "Update"
[image6]: ./docs/Resampling.png "Resampling"
[image7]: ./docs/Return.png "Return"

## Particle Filter
Particle filters estimate the state of dynamical systems from sensor information. The Particle Filter algorithms is a recursive implementation of the Monte Carlo based statistical signal processing method. The Monte Carlo localization algorithm is similar to [Kalman Filters](https://github.com/velsarav/project-extended-kalman-filter) estimates the posterior distribution of a robot's position and orientation based on sensory information but instead of using Gaussians it uses particles to model state.

## Algorithm
The algorithm is similar to KF where motion and sensor updates are calculated in a loop  but PF adds one additional step: a particle filter resampling process where particles with large importance weights will survive while particles with low weights are ignored.

![alt text][image1]


### Psuedo Code
The Pseudo code steps corresponds to the steps in the algorithm flow chart, initialization, prediction, particle weight updates, and resampling.


![alt text][image3]

At the initialization step we estimate the position of GPS input. The subsequent steps in the process will refine this estimate to localize the robot.

![alt text][image4]

During the prediction step add the control input (yaw rate and velocity) for all particles.

![alt text][image5]

Update step will update the particle weights using map landmark positions and feature measurements.

![alt text][image6]

Resample M times (M is range of 0 to num_particles) drawing a particle i (i is the particle index) proportional to its weight. 

![alt text][image7]

The new set of particles represent the Bayes filter posterior probability. This provides a refined estimate of the robots position based on input evidence.



## C++ Random generation

## Particle Filter Project Visualizer


# Implementing the Particle Filter
The directory structure of this repository is as follows:

```
root
|   build.sh
|   clean.sh
|   CMakeLists.txt
|   README.md
|   run.sh
|
|___data
|   |   
|   |   map_data.txt
|   
|   
|___src
    |   helper_functions.h
    |   main.cpp
    |   map.h
    |   particle_filter.cpp
    |   particle_filter.h
```

## Inputs to the Particle Filter
You can find the inputs to the particle filter in the `data` directory.

#### The Map*
`map_data.txt` includes the position of landmarks (in meters) on an arbitrary Cartesian coordinate system. Each row has three columns
1. x position
2. y position
3. landmark id

### All other data the simulator provides, such as observations and controls.

> * Map data provided by 3D Mapping Solutions GmbH.

## Success Criteria
If your particle filter passes the current grading code in the simulator (you can make sure you have the current version at any time by doing a `git pull`), then you should pass!

The things the grading code is looking for are:


1. **Accuracy**: your particle filter should localize vehicle position and yaw to within the values specified in the parameters `max_translation_error` and `max_yaw_error` in `src/main.cpp`.

2. **Performance**: Particle filter completed execution within 49.06 seconds.
![alt text][image2]

