# Prebuilt ArPiRobot Images
There are two types of prebuilt Raspbian Images for ArPiRobot projects: a minimal image and a complete image. The advantages and disadvantages of each type of image are detailed below.

## Minimal Image
The minimal image is based on Raspbian Lite and does not feature a desktop environment or other software that can be used to develop robot code on the robot itself. The minimal image, however, uses less space on the SD Card and uses a readonly file system. This is the most compelling reason to use the minimal image as it allows unplugging the raspberry pi at any time without risking damage to the SD Card. 

The minimal image is designed for "static" projects where the code will not change often and where the robot may be powered off "at random".

Instructions for building the minimal image can be found [here](advanced/buildminimage.md)

## Complete Image
The complete image is based on Raspbian Pixel and comes with software installed to allow developing robot code on the robot itself. It is configured to allow users to connect to the Raspberry Pi on the robot over the network and have a normal Desktop interface to use to write and test robot programs. It also features tools for working with the arduino firmware and is the more commonly used image. The main drawback is that it does *not* use a readonly filesystem and thus the Raspberry Pi must be shutdown with the Deploy tool or from its desktop before it is unplugged.

The complete image is designed to be more user-friendly and provide an easier way to start developing robot code. If in doubt use this image.

## Key differences
| Action | Minimal Image | Complete Image |
|--------|---------------|----------------|
| Developing Robot code | Developed on another PC. Deploy tool used to deploy to robot. Requires extra step of changing to read/write before code can be deployed. | Code can be developed on the robot or on a different PC using the Deploy tool. |
| Booting | Fewer services to start therefore a faster boot time. | More to start so boot takes longer. |
| Powering Off | Can be unplugged at any time. No risk of damage if battery were to die. | Must be shutdown from deploy tool before it can be unplugged safely. If battery were to die the SD Card could become damaged. |
| Uploading Firmware to Arduino | Must be done from another PC. | Can be done from the robot or from another PC. |