# RPi Gateways

This build is heavily based on [ttn_zn's build](https://github.com/ttn-zh/ic880a-gateway),
but we're extending it to be standardised for several deployments, IP67 rated,
with some access hardening, NAT punching and remote logging.

## Shopping List

For the sensor, case and power supply:

Part                               | Price                 | Order/link
-----------------------------------|-----------------------|--------------------
LoRaWAN concentrator board          | £200                  | ic880a ([IMST Webshop](http://webshop.imst.de/ic880a-spi-lorawan-concentrator-868mhz.html))
UF.L (IPAX) to N-type pigtail lead | £7                    | [Solwise PIG-IPAXNCHSSKT25](http://www.solwise.co.uk/wireless-cable.htm)
RPi to iC880a interface            | £7                    | supplied from; <ul><li>[Tindie](https://www.tindie.com/products/gnz/imst-ic880a-lorawan-backplane/)</li><li>from @DefProc, with headers</li></ul>
Raspberry Pi 2                     | £32                   | RPi2: [Farnell](http://de.farnell.com/raspberry-pi/rpi2-modb-v1-2/sbc-raspberry-pi-2-model-b-v1/dp/2612474)
MicroSD Card (8Gb+)                | £7                    | A Sandisk Extreme/Ultra in 8Gb or 16Gb is preferred (based on usage bias)
≤32V to 5V DC-DC voltage converter | | Pololu
2.1mm DC socket to micro-B USB plug | | Amazon
POE midspan injector leads | | Pimoroni
IP 67 rated enclosure with Ethernet and N-type holes, wall and pole mounting kit | £20 (plus £20 p&p) | EZNET EZ-SOE01W ([Aerial.net](http://www.aerial.net/shop/product/39/1102/ezynet-outdoor-enclosure-1-ethernet.html))
LMR200/HDF200 co-axial cable | £1/meter | Solwise
N-type Plug for LMR200/HDF200 cable (50ohm) | £3.10 | Solwise
N-Type plug/jack inline coaxial lightning arrestor | £31 | Diamond SP-3000P ([Waters and Stanton](http://hamradiostore.co.uk/diamond-sp-3000p-lightning-arrester.html))
Ethernet Network surge arrestor | £20 | APC PNET1GB

 * The IP67 rated case that we're using has N-type connector holes, so we're
standardising on N-type connectors for the antenna lead.
 * This build uses LMR200 or equivalent coaxial cable for the antenna; this
 is a very similar diameter to RG58. LMR400 would also be suitable, but seems
 overkill for the signal strength required.

Antenna option 1, a ¼ wave monopole with virtual ground connections:

Part | Price | link
-----|-------|------
¼ wave ground plane omnidirectional antenna | £45 | Aurel GP868 [(Conrad Electronic)](http://www.conrad-electronic.co.uk/ce/en/product/190123/Aurel-650200599-GP-868-Ground-Plane-Antenna-Assembly-kit-NA)
BNC jack to N-type plug adaptor | £5 | TE Connectivity 1-1337567-0 [(Farnell)](http://uk.farnell.com/webapp/wcs/stores/servlet/ProductDisplay?partNumber=1205981)

 * This antenna is has an F-type jack and is supplied with 2.5m RG58 cable
which is terminated in a BNC plug. The BNC jack to N-type plug adaptor allows connection
to the N-type pigtail in the case.

Antenna option 2, a ½ wave dipole:

Part | Price | link
-----|-------|-----
Barracuda 5dbi omnidirectional antenna | £90 | Digikey 931-1255-ND [Taoglas OMB.8912.05F21](http://www.digikey.co.uk/products/en?keywords=OMB.8912.05F21)


For internal testing only:

Part                             | Price                 | Order/link
---------------------------------|-----------------------|--------------------
UF.L to SMA pigtail lead for test antenna  | EUR 6.5 (excl. tax)   | [IMST Webshop](http://webshop.imst.de/pigtail-for-ic880a-spi-and-ic880a-usb.html)
SMA ¼ wavelength Test Antenna    | EUR 3.50 - EUR 8      | 3dBi: [Farnell](http://de.farnell.com/rf-solutions/ant-8whip3h-sma/ant-868mhz-peitsche-gelenk-sma/dp/2305899), 5dBi: [Aliexpress](http://www.aliexpress.com/item/Free-shipping-customized-best-performance-High-Gain-5dBi-GSM-868mhz-900-1800mhz-magnetic-base-antenna/32592017287.html), 7dBi: [Aliexpress](http://www.aliexpress.com/store/product/868MHZ-915MHZ-GSM-3G-antenna-small-sucker-7dbi-aerial-3meters-SMA-male-2/1859567_32512220307.html), 7.5dBi [Aliexpress](http://www.aliexpress.com/item/868MHz-Antenna-7-5dBi-Gain/1870152812.html)
RPi Power Supply 2A with micro USB   | EUR 7.84              | [Farnell](http://de.farnell.com/stontronics/t5454dv/netzteil-raspberry-pi-5v-2a-micro/dp/2427498)
N-type crimping tool | | HT-336G ([TME](http://www.tme.eu/en/details/ht-336g/crimping-tools-for-hf-connectors/))
RG58/LMR200/HDF200 coaxial stripping tool | | CR-336G
