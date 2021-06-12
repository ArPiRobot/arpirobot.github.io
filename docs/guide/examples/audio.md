# Playing Audio

The Raspberry Pi is capable of playing audio either via HDMI or through the 3.5mm audio jack (for the 3A+ and 3B+, not the zero). On a robot, using HDMI for audio will likely not be useful, meaning the 3.5mm audio jack is the primary option. However, there is a third option using the builtin I2S bus.

## Adafruit MAX98357A Breakout (I2S AMP) Setup

**Connections:**

| MAX98357A Breakout Pin | Raspberry Pi Pin | Recommended Wire Color |
| ---------------------- | ---------------- | ---------------------- |
| VIN                    | 5V               | Red                    |
| GND                    | GND              | Black                  |
| SD                     | (not connected)  | N/A                    |
| GAIN                   | (not connected)  | N/A                    |
| DIN                    | GPIO 21          | Blue                   |
| BCLK                   | GPIO 18          | Yellow                 |
| LRC                    | GPIO 19          | Green                  |

The GAIN pin is used to adjust the amplifier gain. By default this is 9dB. This can be adjusted by using external resistors. See [Adafruit's documentation](https://learn.adafruit.com/adafruit-max98357-i2s-class-d-mono-amp/pinouts) for details.

The SD pin is used to select the amplifier mode. For a raspberry pi, this should be in stereo average mode. This is the default when powered from a 5V supply, but if powering from a 3.3V supply, you will need to change this with some external resistors. See [Adafruit's documentation](https://learn.adafruit.com/adafruit-max98357-i2s-class-d-mono-amp/pinouts) for details.

**Setup Pi:**

By default, the Pi has the builtin audio interface enabled and the I2S interface disabled. To change this edit `/boot/config.txt`. The easiest way to edit this file is by connecting the SD card to your computer via an SD adapter (if needed). A storage device labeled "boot" should become visible. On this device there will be a file called `config.txt`. Open this file in a text editor (eg Notepad on Windows, TextEdit on macOS).

There are three changes that need to be made to this file.
- First find the line `#dtparam=i2s=on` and remove the hash (`#`) from the front of the line.
- Next below the `dtparam=i2s=on` line add the following on its own line: `dtoverlay=hifiberry-dac`
- Finally, if you want to disable the builtin audio bus (recommended so the audio will play through the I2S bus by default) find the line that reads `dtparam=audio=on` and add a hash (`#`) in front of the line so it reads `##dtparam=audio=on`.

Put the SD card back into the Pi and power it on. The audio should then play through the I2S bus.


## Playing Audio
- The I2S bus will only support playing stereo audio files on the Pi.
- The ArPiRobot framework only supports playing `.wav` files. More complex formats such as `.mp3` or `.acc` are not supported.
