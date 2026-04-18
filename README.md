# noisedeck

a cyberdeck for dealing in noise

![front_open](images/v0.1/build/front_open.jpeg)

![back_art](images/v0.1/build/open_back_art.jpeg)

![ports](images/v0.1/build/open_left_ports.jpeg)

![btrfld_folded](images/v0.1/build/open_btrfld_closed.jpeg)

![closed](images/v0.1/build/closed.jpeg)


## features
- btrfld keyboard
- trackpad
- headphone jack
- mic jack
- 1x usb c data port
- 1x usb c charge port
- 1x usb a port
- ipad: magnetically mounted
- ipad pencil and charging cradle
- rpi zero 2 w + 5000mah battery

future:
- extra keys in topcase for common hotkeys(?)
- choc key one octave keyboard in topcase(?)
- pitch joystick/wheel that snaps back to the center


the rpi zero 2 w is ran so its usb is in gadget mode, so it tells the ipad it is an ethernet device.
with this you can setup a local network and ssh directly from the ipad to the rpi without using wifi
this leaves the wifi chipset useful for other things

## Size constraints

btrfld:
- 129mm deep
- 215mm wide


## parts
hinges:
https://www.amazon.com/gp/product/B0BFWNJMCY/

hinge fasteners:
M3x8 countersunk fasteners: https://www.amazon.com/HanTof-Countersunk-Machine-Wrenches-Threaded/dp/B0B9HWVV61
M3x10 countersunk fasteners
M3 threaded, DIN 562 flat square nuts: https://www.amazon.com/M3nS-Printers-Stainless-Conform-Quantity/dp/B0BD2N9KTV

power switches:
https://www.amazon.com/dp/B075RC6TFB

apple pencil charging cradle:
https://www.amazon.com/Paiholy-Compatible-Generation-Lightweight-Convenient/dp/B0BMB7362T

midcase fasteners (plastic to plastic):
m2x5 and m2x6 tapered flat head self taping screws

cheap ipad magnetic folio case

btrfld keyboard: https://github.com/SolidHal/btrfld

usbc to 3x usbc (2 data, one power): https://www.amazon.com/gp/product/B09PFR2J82

usbc to 4x usbA : https://www.amazon.com/dp/B09N36LZSQ

usbA dac: https://www.amazon.com/dp/B01DLY3IW8

usbc right angle extension: https://www.amazon.com/dp/B0B71BZ4YF

rpi zero 2 w

5000mah battery

pcb usb hub https://www.amazon.com/dp/B09FX4QN4J

used to get a power only usbc port for charging the rpi battery: https://www.amazon.com/dp/B07VBV1PY5


## Notes

dac must have a power switch, otherwise the ipad will always use it by default and never allow usage of the speakers

TODO:

- holes in midcase/topcase and lid for latch(?)

- extra keys in topcase for common hotkeys(?)
- choc key one octave keyboard in topcase(?)


## Assembly notes

case:
- use 52 gauge drill bit to clean out midcase holes for the m2 screws
- use 46 gauge drill bit to clean out topcase holes for the m2 screws
- use chamfer bit on bottomcase holes screws sit flush
- glue eyeglass nosepads to bottomcase for feet
- hinges need to be filed/ground shorter on the side that would otherwise stick out through the bottom case
- cleaning out the hinge holes in the ipad enclosure is annoying. Recommend using a 46 gauge drill bit
- tear down the ipad folio case to just the parts required to hold the magnets in place. lets call this the "case core"
  - attach a thin film to the side of the case core that magnetizes to the ipad
  - superglue the case core into the ipad enclosure so that the ipad is held into the ipad enclosure magnetically

- the 5000mah battery pack fits nicely under the dac.

wiring:

The wiring is a bit messy, but it works and all fits with plenty of space:

![wiring](images/v0.1/build/messy_wiring.jpeg)


ipad -> extension -> usbc/usbc 3 way splitter -> one usbc charging port, one usbc data port, and the usbc to usbA splitter

usbA splitter:
- port 1:
  - data and ground to headphone port dac
  - power to two switches on the left side of the noisedeck. One switch to the dac, other to pencil charger. Only use one of these at a time.
- port 2:
  - data only to rpi zero 2w data port. Power to nowhere
- port 3:
  - to pcb usb hub:
    - port 3a:
      - to btrfld
    - port 3b:
      - to trackpad
- port 4:
  - glued into the left side of the noisedeck, externally accessible usbA port



## v2

the 3d printed case is good, but the ipad on its own is a lousy computer. i want it around for logic pro and drawing, but otherwise i want it to be a linux machine.

options i have explored are:

1) remove the ipad, take a macbook screen & logic board and design a noisedeck around them.
    - this gets me a "real" computer, but is quite difficult to pull off
        - the hardest part is always
    - no more ipad, which means it can't be used for drawing/reading/tablet things

2) use the ipad as a remote desktop/vnc display for some SBC
    - battery management is annoying, would have to manage the SBC and ipad battery levels
    - rootfs encryption gets funny when a vnc server is required to get display
        - could work around this with ssh decryption, or even stick a light vnc server in the initramfs?
        - or just do it by feel & have ssh available for debug?
    - user login also gets funny, but i could also either do this by feel/ssh for debug or just auto start sway
    - create a headless display in the sway config with the ipad res, run `swaymsg create_output`
    - run wayvnc like `wayvnc 0.0.0.0 --render-cursor --disable-input --max-fps=60 --output=HEADLESS-1`
    - use manual local ethernet connection between the two

3) use the ipad as a display via an HDMI capture card!
    - use something like orion (by lux) (other options available)
    - this limits the ipad to be just a display, uses standard display tech! no special software on the SBC required
    - need to be aware of input lag (hopefully minimal?)

## TODO
- bevel the edges of the lower topcase so it presses into the wrist less?
    - could rotate or move trackpad to get more room to bevel?

## Battery - TODO

- need some way to get 12v for the board
    - actually supports 9V~24V wide voltage input!
- not a lot of room in the palmrest for a battery
- could fit 1x 18650?
    - need something like https://www.aliexpress.us/item/3256810401027521.html
    - https://www.amazon.de/-/en/Kuulaa-Powerbank-External-Battery-Compact/dp/B0GC6J7VTJ does 12v allegedly
- could have an expanded battery pack on the back behind the hinge?
    - 3x 18650s?
- adafruit has https://www.adafruit.com/product/5450 which is a usbc pd to 12v dc barel jack

PLAN:
- 3x 18650s - ordered
- need a 3s BMS
- need a way to get 12v to the BMS
- want to be able to plug any usbc charger in, and be able to charge quickly
- so we need a bidirectional buck/boost usbc that outputs 12v
    - full keywords: bidirectional usbc 3s
    - IP2368 https://old.reddit.com/r/18650masterrace/comments/1euw3zc/ip2368_breakout_board_external_power_output_not/
    - IP5389

- so the schematic is cells --> BMS <----> ip2368
                                       |
                                    controller

- TODO can we fit an IP2368 board in the palmrest?

- could use something like this
    - https://www.aliexpress.us/item/3256805900941544.html?gatewayAdapt=glo2usa4itemAdapt#nav-specification
    - but it only does boost, no buck
    - aparently this doesnt work correctly

- battery case
    - chamfers
    - case screw holes
    - hinge holes TODO: order longer screws!
    - bottom case
    - where to run battery wires?
        - could maybe run them through the midcase wall/bottom in a trench depending on the width?
        - could run them over the btrfld with a connector so the btrfld is removable?
            - near the btrfld hinge perhaps?
                - need to stick a connector either in the battery pack, or in the noisedeck palmrest so we can remove the btrfld
                    - ~35mm in
                    - 4.75 from bottom
                    - 10.2 wide 5.8 tall
            - the wires are ~3mm diameter
            - the connectors are 5.2x10.2 mm

## Cooling - TODO
- long, thin cooler that spans most of the width of the case?
    - fan
        - tip for fan control https://github.com/Joshua-Riek/ubuntu-rockchip/discussions/558
        - these 2 are nearly identical
            - https://www.digikey.com/en/products/detail/sunon-fans/4548HQ-EG50040S1-CG61-S9A/12685816
            - https://www.digikey.de/en/products/detail/wakefield-thermal-solutions/DB0620505H1A-BT0/12610355
                - but this one is in stock
                - WENT WITH THIS
        - TODO: add slots to topcase lower to feed the blower fan
    - heatsink options
        - if it doesn't go over the pencil charging area: max size is 6.87 tall, 53mm wide, ~160mm long (variable)
            - https://www.digikey.de/en/products/detail/fischer-elektronik/SK-511-100-SA/25830853 6mm tall, 100mm long, 45 mm wide
        - if it does (we could thin out the bottom of the pencil charger): 6.87 tall, 67mm tall, ~160mm long
            - WENT WITH THIS
            - https://www.digikey.de/en/products/detail/advanced-thermal-solutions-inc/ATS-1108-C1-R0/4146489

### TODO
    - order screws, m3x5mm for cooler, m2 for fan
