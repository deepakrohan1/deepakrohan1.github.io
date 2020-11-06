# DeepRacer

---
layout: post
title:  "Deep Racer Getting Started"
date:   2020-09-12 12:40:34 -0400
categories: AWS
---

Designing a `Reward Function`

## Reward Function Paramters

`x and y`	The position of the vehicle on the track
`heading`	Orientation of the vehicle on the track
`waypoints`	List of waypoint coordinates
`closest_waypoints`	Index of the two closest waypoints to the vehicle
`progress`	Percentage of track completed
`steps`	Number of steps completed
`track_width`	Width of the track
`distance_from_center`	Distance from track center line
`is_left_of_center`	Whether the vehicle is to the left of the center line
`all_wheels_on_track`	Is the vehicle completely within the track boundary?
`speed`	Observed speed of the vehicle
`steering_angle`	Steering angle of the front wheel

{% highlight python %}
def reward_function(params):
    '''
    Example of rewarding the agent to stay inside the two borders of the track
    '''

    # Read input parameters
    all_wheels_on_track = params['all_wheels_on_track']
    distance_from_center = params['distance_from_center']
    track_width = params['track_width']

    # Give a very low reward by default
    reward = 1e-3

    # Give a high reward if no wheels go off the track and
    # the agent is somewhere in between the track borders
    if all_wheels_on_track and (0.5*track_width - distance_from_center) >= 0.05:
        reward = 1.0

    # Always return a float value
    return float(reward)
{% endhighlight %}


Reward Function based on how far it is from center

{% highlight python %}

def reward_function(params):
    '''
    Example of rewarding the agent to follow center line
    '''

    # Read input parameters
    track_width = params['track_width']
    distance_from_center = params['distance_from_center']

    # Calculate 3 markers that are at varying distances away from the center line
    marker_1 = 0.1 * track_width
    marker_2 = 0.25 * track_width
    marker_3 = 0.5 * track_width

    # Give higher reward if the car is closer to center line and vice versa
    if distance_from_center <= marker_1:
        reward = 1.0
    elif distance_from_center <= marker_2:
        reward = 0.5
    elif distance_from_center <= marker_3:
        reward = 0.1
    else:
        reward = 1e-3  # likely crashed/ close to off track

    return float(reward)

{% endhighlight %}