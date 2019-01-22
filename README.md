[:fr:](LISEZMOI.md) [:uk:](README.md)

# VGA_Zero
VGA interface for Raspberry Pi Zéro &amp; 2,3,B+

buy on ![https://ir.ebaystatic.com/rs/v/fxxj3ttftm5ltcqnto1o4baovyl.png](http://rover.ebay.com/rover/1/709-53476-19255-0/1?icep_ff3=2&pub=5575378759&campid=5338273189&customid=&icep_item=153295944438&ipn=psmain&icep_vectorid=229480&kwid=902099&mtid=824&kw=lg&toolid=11111)

![](img/VgaZero.jpg)

### DPI (Display parallel Interface)
Like [VGA666](https://github.com/fenlogic/vga666), this pcb uses dpi interface (in mode 3 to free gpio 18 & 19)

![](img/dpi-packing.png)

see https://www.raspberrypi.org/documentation/hardware/raspberrypi/dpi/README.md

only the necessary bits of the dpi are redirected to the 40-pin GPIO port, with the [vga565.dtbo](overlays/vga565.dtbo?raw=true) file loaded at startup

here is the code of the file:

    /dts-v1/;
    /plugin/;
    /{
    	compatible = "brcm,bcm2708";
    	// There is no VGA driver module, but we need a platform device
    	// node (that doesn't already use pinctrl) to hang the pinctrl
    	// reference on - leds will do
    	fragment@0 {
    		target = <&leds>;
    		__overlay__ {
    			pinctrl-names = "default";
    			pinctrl-0 = <&vga565_pins>;
    		};
	    };
    	fragment@1 {
    		target = <&gpio>;
    		__overlay__ {
    			vga565_pins: vga565_pins {
        			brcm,pins = <2 3 4 5 6 7 8 12 13 14 15 16 17 20 21 22 23 24>;
    				brcm,function = <6>; /* alt2 */
				    brcm,pull = <0>; /* no pull */
			    };
		    };
	    };
    };

### Audio Interface
audio interface is connected to gpio 18 & 19 (PWM)

see https://learn.adafruit.com/adding-basic-audio-ouput-to-raspberry-pi-zero/pi-zero-pwm-audio

    # Enable audio for PiZero(loads snd_bcm2835)
    dtoverlay=pwm-2chan,pin=18,func=2,pin2=19,func2=2
    dtparam=audio=on

### SPI Interface
GPIO 9 , 10, 11 , 25, 26, 27 can be used as SPI , whit overlay spi0-cs, with CS redirection to gpio 26 & 27

    # uncomment to use spi0 with cs0 & cs1 retirected on pin 26 & 27 
    dtparam=spi=on
    dtoverlay=spi0-cs,cs0_pin=26,cs1_pin=27

## Schematic
![sch](img/sch.PNG)

## PCB
![pcb](img/3D.PNG)

## Installation
Copy [vga565.dtbo](overlays/vga565.dtbo?raw=true) to /boot/overlays/
and copy content of [config-example.txt](overlays/config-example.txt?raw=true) to /boot/config.txt


## Révision
rev1
- bad vga footprint, hole too small

rev2
- change vga footprint
- move j3 & j5
