# Kidnapped Vehicle Project 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

## Project Introduction
The robot has been kidnapped and transported to a new location! Luckily it has a map of this location, a (noisy) GPS estimate of its initial location, and lots of (noisy) sensor and control data.

The goal of this project is to implement a 2-dimensional particle filter in C++. The
particle filter will be given a map and some initial localization information (analogous to what a GPS would provide). At each time step, the filter will also get observation and control data.

## Rubric Points

The [rubric](https://review.udacity.com/#!/rubrics/747/view) points were individually addressed in the [implementation](https://github.com/velsarav/Kidnapped-Vehicle-Project/tree/master/src)


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
The algorithm is similar to KF where motion and sensor updates are calculated in a loop but PF adds one additional step: a particle filter resampling process where particles with large importance weights will survive while particles with low weights are ignored.

![alt text][image1]


### Pseudo Code
The Pseudocode steps correspond to the steps in the algorithm flow chart, initialization, prediction, particle weight updates, and resampling.


![alt text][image3]

At the initialization step, we estimate the position of GPS input. The subsequent steps in the process will refine this estimate to localize the robot.

![alt text][image4]

During the prediction, step adds the control input (yaw rate and velocity) for all particles.

![alt text][image5]

Update step will update the particle weights using map landmark positions and feature measurements.

![alt text][image6]

Resample M times (M is a range of 0 to num_particles) drawing a particle "i"(i is the particle index) proportional to its weight. 

![alt text][image7]

The new set of particles represents the Bayes filter posterior probability. This provides a refined estimate of the position of the robot based on input evidence.


## C++ Implementation

The above Pseudo code was implemented in C++ with the following function

### ParticleFilter::init
Set the number of particles. Initialize all particles to the first position (based on estimates of x, y, theta and their uncertainties from GPS) and all weights to 1. Add random Gaussian noise to each particle. 

### ParticleFilter::prediction
Add measurements to each particle and add random Gaussian noise.

### ParticleFilter::dataAssociation
Find the predicted measurement that is closest to each observed measurement and assign the observed measurement to this particular landmark.

### ParticleFilter::updateWeights
Update the weights of each particle using a mult-variate Gaussian distribution.

### ParticleFilter::resample
Resample particles with replacement with probability proportional to their weight. 

To add the random noise C++ std::normal_distribution and std::default_random_engine were used.

## Particle Filter Project Visualizer
Udacity provided [simulator](https://github.com/udacity/self-driving-car-sim/releases) was used in this project, C++ uWebSocketIO is used to communicate with the simulator. The simulator provides the script for the noisy position data, vehicle controls, and noisy observations. It also feeds back the best particle state.

The simulator can also display the best particle's sensed positions, along with the corresponding map ID associations.  This can be extremely helpful to make sure transition and association calculations were done correctly.


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

## C++ Code compilation
Please refer to the base project [README](https://github.com/udacity/CarND-Kidnapped-Vehicle-Project) to compile the C++ code and run the simulator.

## Grading code
The things the grading code is looking for are:


1. **Accuracy**: particle filter should localize vehicle position and yaw to within the values specified in the parameters `max_translation_error` and `max_yaw_error` in `src/main.cpp`.

2. **Performance**: Particle filter completed execution within 49.06 seconds.

![alt text][image2]

## Reference
* [Particle filter introduction](https://papers.nips.cc/paper/2305-real-time-particle-filters.pdf)

* [Unscented Kalman Filters and Particle Filter Methods for Nonlinear State Estimation](https://www.sciencedirect.com/science/article/pii/S2212017313006427)

* [Particle filters - Medium Article](https://medium.com/@fernandojaruchenunes/udacity-robotics-nd-project-6-where-am-i-8cd657063585)

* [Particle Filters in Robotics](http://robots.stanford.edu/papers/thrun.pf-in-robotics-uai02.pdf)

* [Kidnapped Vehicle](https://github.com/dkarunakaran/carnd-kidnapped-vehicle-term2-p3)

