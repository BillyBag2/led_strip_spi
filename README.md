# led_strip_spi example

Modified by BillyBag2 for ESP32-P4 boards.

## Notes

* SPI2_HOST (HSPI) and SPI3_HOST (VSPI) are functionally the same.
* GPIO46, GPIO47 are not the correct voltage to work with SK9822.

## TODO

* [x] Configure in main.c for SPI3_HOST, GPIO1 (DATA), GPIO2 (CLOCK).
* [x] Test on JC-ESP32P4-m3-DEV board with two SK9822 LEDs.
* [ ] Complete setup for the host board. (RAM, flash size, etc)
* [ ] Add menuconfig option to select SPI2_HOST or SPI3_HOST.
* [ ] Add menuconfig options to select GPIO pins for DATA and CLOCK.
* [ ] Add menuconfig option to select frequency of SPI clock.

Original README.md ...

## What the example does

The example runs the following effects in a loop.

- changes the colours of all the LEDs using [_color wheel_](https://duckduckgo.com/?q=color+wheel).
- simply rotates the colours (B -> G -> R)
- displays rainbow colors on the strip, scrolling the rainbow

## Configuration

You can change the number of pixels in a strip by `idf.py menuconfig` or `make
menuconfig`. See `CONFIG_EXAMPLE_N_PIXEL` under `Example configuration`. The
default is 8 pixels.

## Hookup

| Pin on `SK9822` | Destination                  |
|-----------------|------------------------------|
| `5V`            | `5V`                         |
| `CI`            | `GPIO14` (ESP32 and ESP8266) or `GPIO6` (ESP32C3) |
| `DI`            | `GPIO13` (ESP32 and ESP8266) or `GPIO7` (ESP32C3) |
| `GND`           | `GND`                        |

`SK9822` LED strip has `CI` and `DI` at both end of the strip. Make sure the
direction of data flow is correct. The `GPIO`s must be connected to the first
LED. My strip has arrows on the strip like below.

```text
--------------------- Data flow ------------------>

                  .--------------------------.
                  |                          |
                  |  .-------.    .-------.  |
GND    ----- GND -+--| --> G |----| --> G |--+- GND
GPIO14 ----- CI  -+--|       |----|       |--+- CI
GPIO13 ----- DI  -+--|  LED  |----|  LED  |--+- DI
5V/Vin ----- 5V  -+--|       |----|       |--+- 5V
                  |  `-------'    `-------'  |
                  | the first LED            |
                  `--------------------------'
```

## Notes

Start with a small number of LED pixels. Make sure the 5 V power source is
stiff enough. A `SK9822` draws up to 60 mA.  A cheap `ESP32` development board
with `AMS1117` can source 8 `WS2812` pixels from USB 5V `VIN` _with_ the
default example code.

Make data lines short. The clock speed is an order of `Mhz`.

`SK9822` is 5V device but you _might_ be able to drive it from ESP32, which is
3.3V device, without level shifter. See also [README.md for led_strip](../led_strip/README.md).
