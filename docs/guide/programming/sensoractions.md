## Using Sensors instead of Time

TODO: Explain goal. Improve time-based drive to allow easier automated routines.

### Rotate Angle Action

TODO: Replace rotate time with rotate angle that uses no PID and just stops at the given angle


### Drive Distance Action

TODO: Add support for encoders in robot program

TODO: Use encoders + brake mode to drive until a distance threshold has passed

### Using the Actions

TODO: Implement driving a square and triangle on button presses using automated routines

### Limitations

TODO: Explain limitations of this method. Drive distance can go too far. It also doesn't use the IMU to ensure a straight line. Rotate angle can overshoot and does not correct. Rotating to an angle is actually a perfect application for a PID loop, shown in the next section of the guide.
