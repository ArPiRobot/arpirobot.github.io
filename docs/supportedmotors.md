# Supported Motor Controllers

The following lists supported motor controllers, which device they connect to (Arduino or Raspberry Pi) and the python libraries or arduino firmware features required to use them.

## Adafruit Motor Hat

**Link**: https://www.adafruit.com/product/2348

**Motors**: 4 DC Motors

**Interface**: I2C via Raspberry Pi GPIO Header (entire header)

**Price**: $22.50

**Python Libraries**: `adafruit-circuitpython-motorkit`

**Notes**: Soldering required to assemble. Optional stacking header allows access to GPIO pins that are otherwise all covered by the hat.

## L298N

**Link**: https://www.amazon.com/s?k=l298n

**Motors**: 2 DC Motors per module

**Interface**: Digital IO and PWM via Raspberry Pi GPIO Pins (individual pins)

**Price**: $7-$10 for 2-4

**Python Libraries**: `gpiozero`

**Notes**: Look through reviews. Some are constructed better than others.