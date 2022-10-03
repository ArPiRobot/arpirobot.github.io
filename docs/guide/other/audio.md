# Playing Audio

Many single board computers include interfaces capable of outputting audio. Typically, this is a 3.5mm audio jack or an HDMI interface. Many also have an "I2S" interface which can use an external audio amplifier and speaker to play audio. This is often the most compact solution for use on robots, however it is more involved to setup.


## Example I2S Setup

This section describes setting up an Adafruit MAX98357A Breakout (I2S AMP) on a Raspberry Pi. This process will vary on different systems.

**Connections:**

| MAX98357A Breakout Pin | SBC Pin                  | Raspberry Pi Pin | Recommended Wire Color |
| ---------------------- | ------------------------ | ---------------- | ---------------------- |
| VIN                    | 5V                       | 5V               | Red                    |
| GND                    | GND                      | GND              | Black                  |
| SD                     | (not connected)          | (not connected)  | N/A                    |
| GAIN                   | (not connected)          | (not connected)  | N/A                    |
| DIN                    | PCM DOUT / I2S DOUT      | GPIO 21          | Blue                   |
| BCLK                   | PCM CLK / I2S CLK        | GPIO 18          | Yellow                 |
| LRC                    | PCM FS / I2S FS          | GPIO 19          | Green                  |

See [Adafruit's documentation](https://learn.adafruit.com/adafruit-max98357-i2s-class-d-mono-amp/pinouts) for more details.

**Software Setup:**

- Follow [Adafruit's Guide](https://learn.adafruit.com/adafruit-max98357-i2s-class-d-mono-amp/raspberry-pi-usage#detailed-install-2712959). Note that since the robot does not have internet access, the automated install process will not work. You must follow the steps in "Detailed Install".
- You will need to [Connect to the robot via SSH](./ssh.md) to follow these steps.
- These steps are for a Raspberry Pi. The software setup will not be the same for other single board computers. For other systems, it is recommended to search the SBC's name followed by "I2S". There will likely be an existing guide to enable I2S on the board.


## Playing Audio in a Robot Program

TODO
