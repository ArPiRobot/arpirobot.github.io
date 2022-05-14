# Using Actions

## Challenges with Periodic Programming Model

TODO: Why is the periodic model limited

TODO: Explain how a series of sequential actions could work

TODO: Explain the challenge with parallel actions

TODO: Explain why not modular


## What is an Action

TODO: Modular reusable code is easy using objects. This can be extended to different robot tasks. Additionally many actions can run in parallel

### Action Structure

TODO: Explain begin, process, finish, should_continue

TODO: Mention the actions files in robot code

TODO: Example code with empty "template" for an action that can be pasted into robot code files


### Using Actions

TODO: Explain action manager and how using actions works (manual method)

TODO: Example code

TODO: Explain action triggers

TODO: Example code w/ gamepad triggers


### Ownership of Devices

TODO: Explain locking devices and how that works (note specifically that it interrupts any previous owner)

TODO: Example code locking devices

TODO: Explain why this is beneficial

TODO: Explain that you must be careful with using periodic code if an action may lock the device you are using

TODO: Example code of an issue and show use of isLocked method to address in some cases


### Running Actions Sequentially

TODO: Explain ActionSeries and give an example

TODO: ActionSeries example code


## Making Drive Code into an Action

TODO: Move existing drive code into its own action

TODO: Start the action in robot started let it run forever


## Automated Routines using Actions

TODO: Explain the goal of this section. Note that not using sensors is not the best way to do this. Conceptually, using encoders to do a DriveDistance is better and IMU to do a RotateAngle is better than using time

### Drive Time Action

TODO: Implement this


### Rotate Time Action

TODO: Implement this


### Using the Actions

TODO: Implement an action series showing how this works. Drive a "square"

TODO: Comment on tuning drive times and rotate times to drive the square correctly


## Using Sensors Instead of Time

TODO: Explain goal. Improve time-based drive to allow easier automated routines.

### Rotate Angle Action

TODO: Replace rotate time with rotate angle that uses no PID and just stops at the given angle


### Drive Distance ACtion

TODO: Add support for encoders in robot program

TODO: Use encoders + brake mode to drive until a distance threshold has passed

### Using the Actions

TODO: Implement driving a square and triangle on button presses using automated routines

### Limitations

TODO: Explain limitations of this method. Drive distance can go too far. It also doesn't use the IMU to ensure a straight line. Rotate angle can overshoot and does not correct. Rotating to an angle is actually a perfect application for a PID loop, shown in the next section of the guide.
