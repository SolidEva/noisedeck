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

## Battery

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

- can we fit an IP2368 board in the palmrest?
    - yea, i think so?

- could use something like this
    - https://www.aliexpress.us/item/3256805900941544.html?gatewayAdapt=glo2usa4itemAdapt#nav-specification
    - but it only does boost, no buck
    - aparently this doesnt work correctly

- battery case
    - chamfers
    - case screw holes
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

    - longer hinge screws to connect battery pack to laptop

## Cooling
- long, thin cooler that spans most of the width of the case?
    - fan
        - tip for fan control https://github.com/Joshua-Riek/ubuntu-rockchip/discussions/558
        - these 2 are nearly identical
            - https://www.digikey.com/en/products/detail/sunon-fans/4548HQ-EG50040S1-CG61-S9A/12685816
            - https://www.digikey.de/en/products/detail/wakefield-thermal-solutions/DB0620505H1A-BT0/12610355
                - but this one is in stock
                - WENT WITH THIS
    - heatsink options
        - if it doesn't go over the pencil charging area: max size is 6.87 tall, 53mm wide, ~160mm long (variable)
            - https://www.digikey.de/en/products/detail/fischer-elektronik/SK-511-100-SA/25830853 6mm tall, 100mm long, 45 mm wide
        - if it does (we could thin out the bottom of the pencil charger): 6.87 tall, 67mm tall, ~160mm long
            - WENT WITH THIS
            - https://www.digikey.de/en/products/detail/advanced-thermal-solutions-inc/ATS-1108-C1-R0/4146489

    - screws, m3x5mm for cooler, m2 for fan
