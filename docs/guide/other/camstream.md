
The [ArPiRobot-CameraStreaming](https://github.com/ArPiRobot/ArPiRobot-CameraStreaming) component is pre-installed on all ArPiRobot OS images. This component includes scripts to generate and invoke gstreamer pipelines for realtime camera streaming. An RTSP server is also installed ([rtsp-simple-server](https://github.com/aler9/rtsp-simple-server)). The combination of these two components allows for multiple low-latency camera streams over the robot's wifi network.

## Configuring Camera Streams

There are three ways to configure camera streams

1. The `camstream.py` script can be run directly (over ssh for example). In this case, the options described below are passed as command line flags (see `camstream.py --help` or the `README.md` file in the github repository for specific flags).

2. Text (`.txt`) files in `/home/arpirobot/camstream` are configurations for camera streams. These files contain command line flags. The name of the text file is the name of the stream. The system service `camstream` will launch a camestream pipeline for each `.txt` file when it is started.

3. The Deploy Tool can configure camera streams in the "Camera Stream" tab. This is a GUI frontend to edit the text file described in method 2. It also includes buttons to play streams as well as start / stop the `camstream` service and the `rtsp` server. Note that the `camstream` service must be restarted (stop then start again) after modifying configurations.


### Source Options

Three camera "drivers" are currently supported:

- RasPiCam
- LibCamera w/ LibCamera Apps
- Video4Linux2 (V4L2)

The first two methods currently only support Raspberry Pi devices using the Raspberry Pi Camera. Also note that RasPiCam is replaced by LibCamera, however for now raspicam is used instead on ArPiRobot OS images for Raspberry Pi devices. As such, libcamera will not work (without modifications). The third method (v4l2) is a generic linux camera input method that should work with most cameras on most devices. Any USB webcam should work with this input method.


When using the v4l2 driver there are two additional options available

- `device`: This is used to specify a v2l4 device (`/dev/video#`)
- `iomode`: This is used to specify a v4l2 iomode. Use `auto` unless you get high latency. For usb webcams, using `dmabuf` may reduce latency

Additionally, for all drivers, there is a `vconvert` (video convert) option for inputs. This adds a `videoconvert` element to the pipeline and can help in cases where camera input format is not directly handled by the next elements in the pipeline. Basically, if something doesn't work try turning this on. Otherwise, it should not be needed.


### Video Options

- `width`: Video resolution width&ast;
- `height`: Video resolution height&ast;
- `framerate`: Video framerate&ast;
- `flip`: Flip the video one one or both axes (`hflip`, `vflip` flags)
- `rotate`: Rotate the video. Most drivers / cameras only support 0 or 180. raspicam supports 90 and 270.
- `gain`: Supported only for the Raspberry Pi Camera using the raspicam or libcamera drivers. Adjusts the camera's "sensitivity" to light.

&ast;*The combination of these three parameters must be a supported input mode of the camera. Check your specific camera's specs for supported input modes.*


### Stream Options

Two output formats are supported using the `format` option: `h264` and `mjpeg`. `h264` will take a little more time to encode / decode, but uses *much* less bandwidth for a given resolution / framerate. `mjpeg` is rarely useful for realtime streams.

When using the `h264` format, the following options are used

- `encoder`: Which H.264 encoder to use. `libav-omx` uses omx hardware acceleration on Raspberry Pi devices. The `omx` encoder also uses hardware acceleration, but is not available on some newer systems (thus `libav-omx` is recommended instead). `libx264` is a software encoder that should work on any device.
- `profile`: Which H.264 profile to use (baseline, main, or high). Baseline is typically recommended for realtime streams (especially if software encoding is used).
- `bitrate`: Target stream bitrate in bits / second

When using the `mjpeg` format, the followint options are used

- `quality`: Integer between 1 and 100. Controls jpeg image quality of frames. Lower numbers reduce bitrate and image quality. Note that this parameter has no effect with the raspicam driver. If using raspicam, use the `bitrate` option instead. This option sets target stream bitrate and automatically adjusts quality to achieve this.


### Network Options

- `netmode`: Controls stream network mode. Either `tcp`, `udp`, or `rtsp`. Defaults to `rtsp` which is generally recommended for robots. `tcp` and `udp` stream one camera directly to one computer. `rtsp` is a server that can handle multiple streams and clients. It also makes using `udp` easier than directly using `udp` mode (which is better for realtime streams).
- `address`: Address to send the stream to. For `tcp` mode this is a local address to host a `tcp` server. For `udp` mode this is the address of the computer to send the stream to. For `rtsp` mode this is the address of the computer hosing the rtsp server (typically the same device so `127.0.0.1` or `localhost`).
- `port`: Port associated with the address. For the included `rtsp` server, use port `8554`. For `tcp` and `udp` modes, choose a port number.
- `rtspkey`: Only applies in `rtsp` mode. This is a unique identifier for this stream (rtsp servers can have multiple streams; this identifies which stream). Each stream on the rtsp server must have a unique identifier.


## Playing Camera Streams

Many programs can play the streams in various supported netmodes, however often additional configuration is needed to achieve low latency playback. As such, the camera streaming repository includes a `playstream.py` script that will launch `ffplay`, `mpv`, or `mplayer` to play a stream. See the `README.md` file in the github repository for more info.

Additionally, the Deploy Tool provides a GUI to play streams using any of these three players. The players themselves though must be installed on your system separately (and be in the system `PATH`).
