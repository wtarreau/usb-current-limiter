# Fixed or adjustable USB current limiter

When designing USB-powered devices, it's very important to be able to limit the current in order to protect the new device, as well as the USB port (typically the computer's). A simple serial resistor doesn't work as it causes a voltage drop that the device might not necessarily be able to cope with.

## Simple solution

It's easy to make a current limiter using a good old LM317, there are hundreds of such designs everywhere on the net. The main issue is that it drops at least 3 Volts, so you have to bring a 8V power supply on your USB connections, which can be even more dangerous for the board and the PC.

Instead, this project makes use of [Micrel's (now Microchip) MIC2544](http://ww1.microchip.com/downloads/en/DeviceDoc/mic2544.pdf). It's a high-side current limiter switch. It's perfect for hobbyists as it comes in an easily solderable SOP8 package and only requires one external resistor to set the current limit and an optional LED to report the overcurrent condition. The SOP8 package is large enough to let the two USB D+/D- lanes pass under, and allows to make an easy single-sided PCB. It can limit from something like 100mA to 1.5A or so.

The device photographed below was made the evening I found it in my mailbox. It was picked a bit too early from the acid bath, which explains why the copper tracks are not clean, but they are OK. At the last moment I preferred to solder a tiny 1k potentiometer that I already had instead of a fixed divider. The current limit precision is not very precise but we're speaking about saving chips from frying, so even 10% accuracy would be awesome.

![Finished adapter](photos/adapter.jpg?raw=true)

Below it is connected to a freshly assembled [Breadbee board](https://github.com/breadbee/breadbee) board which it managed to protect:

![Finished adapter](photos/connected.jpg?raw=true)

## Schematic

The schematic was made with Eagle 7.7 and is provided in the directory.

![Schematic](output/schematic.png?raw=true)

## Board

The board is single-sided and doens't need any hole. A few versions of the rendered PCB are provided in the [output directory](output/), at 300 and 600 dpi and as a PDF. It's also provided as an Eagle `.brd` file.

## Bill of materials

Name | Value
-----|-------
 IC1 | MIC2544
 R1  | Rset = 230/current. e.g. 1k = 230mA
 R2  | 330-510 ohm (optional, for the overcurrent LED)
 R3  | optional, extra inrush current (230/Iamp)
 C1  | optional, inrush current duration. E.g. 1 µF
 D1  | optional, overcurrent red LED
 J1  | USB-A male connector (or PCB traces)
 X1  | USB-A female connector

## Other options

Other options include [ANALOGIC TECH's AAT4610](http://www1.futureelectronics.com/doc/ANALOGICTECH%20-%20AATI/AAT4610IGV-T1.pdf), which comes in a much smaller SOT23-5 package and doesn't have a LED output, or making your own solution using a shunt resistor and a rail-to-rail op amp such as [Microchip's MCP6001](https://ww1.microchip.com/downloads/en/DeviceDoc/MCP6001-1R-1U-2-4-1-MHz-Low-Power-Op-Amp-DS20001733L.pdf), coupled with a high-side, low-voltage, P-channel MOSFET. In the end, the MIC2544 already has everything included and is quite cheap so it seems to remain the best option for hobbyists.
