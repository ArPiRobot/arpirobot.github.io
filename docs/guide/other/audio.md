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

The `ArPiRobot-CoreLib` includes an `AudioManager` object that is capable of playing audio files in `.wav`, `.mp3`, and `.flac` formats. 

The `AudioManager` includes three functions of interest:

- `get_playback_devices()` / `getPlaybackDevices()`: Returns a list of playback devices. Each device has a name and some other information about the device. 
- `play_sound()` / `playSound()`: Plays a given audio file. Can also specify a specific audio device to play it on (one of the devices from `get_playback_devices` / `getPlaybackDevices()`) instead of using the "default" audio device. This function returns an integer to identify a "job" (which really just means a specific file being played).
- `stop_job()` / `stopJob()`: Stops the job with the given identifier (integer returned from `play_sound()` / `playSound()`). This is used to stop a sound "early". If the file has already finished playing, this function has no effect.


=== "Python (`robot.py`)"
    ```py
    from arpirobot.core.audio import AudioManager, AudioDeviceInfo

    def __init__(self):
        super().__init__()
        self.dev: AudioDeviceInfo = None
        self.has_device: bool = False
        self.job: int = -1

    def robot_started(self):
        # Searching for a specific audio device
        # This is an I2S interface's name on a raspberry pi
        for dev in AudioManager.get_playback_devices():
            if dev.name == "snd_rpi_hifiberry_dac, HifiBerry DAC HiFi pcm5102a-hifi-0":
                self.dev = dev
                self.has_device = True
    
    def robot_enabled(self):
        # Start playing an audio file named "enable.mp3"
        # This file is in the "sound_files" folder in the arpirobot project
        # Thus it is placed in "/home/arpirobot/arpirobot/sound_files"
        # Only play on the specific device if the device was found in robot_started
        # Otherwise, play on the "default" playback device
        if self.has_device:
            self.job = AudioManager.play_sound("/home/arpirobot/arpirobot/sound_files/enable.mp3", self.dev)
        else:
            self.job = AudioManager.play_sound("/home/arpirobot/arpirobot/sound_files/enable.mp3")
    
    def robot_disabled(self):
        # Stop playing the enable sound if it is playing
        # If job is -1 or the job has completed, this does nothing
        AudioManager.stop_job(self.job)
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    // Add at top with includes
    #include <arpirobot/core/audio/AudioDeviceInfo.hpp>

    // Add as member variables of Robot class with other member variables
    AudioDeviceInfo dev;
    bool hasDev = false;
    int job = -1;
    ```

=== "C++ (`robot.cpp`)"
    ```cpp
    void robotStarted(){
        // Searching for a specific audio device
        // This is an I2S interface's name on a raspberry pi
        for(auto &dev : AudioManager::getPlaybackDevices()){
            if (dev.name == "snd_rpi_hifiberry_dac, HifiBerry DAC HiFi pcm5102a-hifi-0"){
                this->dev = dev;
                this->hasDev = true;
            }
        }
    }

    void robotEnabled(){
        // Start playing an audio file named "enable.mp3"
        // This file is in the "sound_files" folder in the arpirobot project
        // Thus it is placed in "/home/arpirobot/arpirobot/sound_files"
        // Only play on the specific device if the device was found in robot_started
        // Otherwise, play on the "default" playback device
        if(hasDev){
            job = AudioManager::playSound("/home/arpirobot/arpirobot/sound_files/enable.mp3", dev)
        }else{
            job = AudioManager::playSound("/home/arpirobot/arpirobot/sound_files/enable.mp3")
        }
    }
    
    void robotDisabled(){
        // Stop playing the enable sound if it is playing
        // If job is -1 or the job has completed, this does nothing
        AudioManager::stopJob(job)
    }
    ```

Finally, the project's json file needs to be modified to make the deploy tool copy the `sound_files` folder. Edit `arpirobot-proj.json` and add the following in the `deployFiles` section.

```
"sound_files/"
```

The file should look something like the following

=== "Python Project"
    ```json
    {
        "version": 2,
        "deployFiles": [
            "src/*",
            "main.sh",
            "sound_files/"
        ],
        "coreLibFiles": [
            "lib/*",
            "python_bindings/arpirobot/"
        ]
    }
    ```

=== "C++ Project"
    ```json
    {
        "version": 2,
        "deployFiles": [
            "build/robot",
            "main.sh",
            "sound_files/"
        ],
        "coreLibFiles": [
            "lib/*"
        ]
    }
    ```

You will also need to place an audio file in `mp3` format a folder named `sound_files` in your project (next to the `src` folder). The file must be called `enable.mp3`. Alternatively, the filename could be changed in the example code above.

When run, this example program will play the sound when enabled, and stop it when disabled.
