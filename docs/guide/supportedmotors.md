# Supported Motor Controllers

The following lists supported motor controllers, which device they connect to (Arduino or Raspberry Pi) and the python libraries or arduino firmware features required to use them.

## Adafruit Motor Hat

**Link**: https://www.adafruit.com/product/2348

**Motors**: 4 DC Motors

**Interface**: I2C via Raspberry Pi GPIO Header (entire header)

**Price**: $22.50

**Python Libraries**: `adafruit-circuitpython-motorkit`

**Notes**: Soldering required to assemble. Optional stacking header allows access to GPIO pins that are otherwise all covered by the hat.

## Geekworm Motor Hat

**Link**: https://www.amazon.com/Raspberry-Function-Expansion-Support-Stepper/dp/B0721MTJ3P/ref=sr_1_1?keywords=geekworm+motor+hat&qid=1566228003&s=gateway&sr=8-1

**Motors**: 4 DC Motors

**Interface**: I2C via Raspberry Pi GPIO Header (entire header)

**Price**: $28.09

**Python Libraries**: `smbus2`

**Notes**: No soldering required to assemble, but lower quality than Adafruit motor hat.

## L298N

**Link**: https://www.amazon.com/s?k=l298n

**Motors**: 2 DC Motors per module

**Interface**: Digital IO and PWM via Raspberry Pi GPIO Pins (individual pins)

**Price**: $7-$10 for 2-4

**Python Libraries**: `gpiozero`

**Notes**: Look through reviews. Some are constructed better than others.

## Adafruit DRV8833 Breakout

**Link**: https://www.adafruit.com/product/3297

**Motors**: 2 DC Motors per module

**Interface**: Digital IO and PWM via Raspberry Pi GPIO Pins (individual pins)

**Price**: $4.95

**Python Libraries**: `gpiozero`

**Notes**: Easiest way to connect motors to this module is by using a breadboard and putting the motor wires in the breadboard. The better way is to solder wires with female dupont connector onto the motors and connect those to the header pins. If using the second option it is easiest to solder the header pins on the same side of the breakout board as the terminal block for power input.

## Adafruit TB6612 Breakout

**Link**: https://www.adafruit.com/product/2448

**Motors**: 2 DC Motors per module

**Interface**: Digital IO and PWM via Raspberry Pi GPIO Pins (individual pins)

**Price**: $4.95

**Python Libraries**: `gpiozero`

**Notes**: Easiest way to connect motors to this module is by using a breadboard and putting the motor wires in the breadboard. The better way is to solder wires with female dupont connector onto the motors and connect those to the header pins. If using the second option it is easiest to solder the header pins on the same side of the breakout board as the terminal block for power input. Power input terminal blocks must be purchased seperatly (https://www.adafruit.com/product/724) and are highly recommended.