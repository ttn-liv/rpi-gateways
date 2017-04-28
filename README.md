# RPi Gateways

This build is heavily based on [ttn_zn's build](https://github.com/ttn-zh/ic880a-gateway),
but we're extending it to be standardised for several deployments, IP67 rated,
with some access hardening, NAT punching and remote logging.

Deployment is controlled via ansible, instructions are in the
[deployment directory](deployment/README.md).

## Shopping List

### Case

For the sensor, case and power supply:

Part                               | Price                 | Order/link
-----------------------------------|-----------------------|--------------------
LoRaWAN concentrator board         | £200                  | ic880a ([IMST Webshop](http://webshop.imst.de/ic880a-spi-lorawan-concentrator-868mhz.html))
UF.L (IPAX) to N-type pigtail lead | £7                    | [Solwise PIG-IPAXNCHSSKT25](http://www.solwise.co.uk/wireless-cable.htm)
RPi to iC880a interface            | £7                    | supplied from; <ul><li>[Tindie](https://www.tindie.com/products/gnz/imst-ic880a-lorawan-backplane/)</li><li>from @DefProc, with headers</li></ul>
Raspberry Pi 2                     | £32                   | RPi2: <ul><li>[Farnell 2612474](http://uk.farnell.com/raspberry-pi/rpi2-modb-v1-2/sbc-raspberry-pi-2-model-b-v1/dp/2612474)</li><li>[Amazon](http://amzn.to/2kUvuHh)</li>
MicroSD Card (8Gb+)                | £7                    | A Sandisk Extreme/Ultra in 8Gb or 16Gb is preferred (based on usage bias) [Amazon](https://www.amazon.co.uk/gp/product/B012VKUSIA)
up to 32V input down to to 5V  output DC-DC voltage converter | £11 | Pololu 5V 5A Step-Down Voltage Regulator D24V50F5 ([hobbytronics](http://www.hobbytronics.co.uk/d24v50f5-5v-step-down-regulator))
2.1mm DC socket to micro-B USB plug | £1 | ([Amazon](https://www.amazon.co.uk/gp/product/B01K1TR7HS/))
IP68 Ethernet DC splitter         | £5 | ([Aerial.net](https://www.aerial.net/shop/product_info.php?products_id=1574))
IP 67 rated enclosure with Ethernet and N-type holes, wall and pole mounting kit | £20 (plus £20 p&p) | EZNET EZ-SOE01W ([Aerial.net](http://www.aerial.net/shop/product/39/1102/ezynet-outdoor-enclosure-1-ethernet.html))
LMR200/HDF200 co-axial cable       | £1/meter | [Solwise HDF200](http://www.solwise.co.uk/wireless_connectorssundries.htm)
N-type Plug for LMR200/HDF200 cable (50ohm) ×2 | £3.10 | [Solwise N-Type 200 Plug](http://www.solwise.co.uk/wireless_connectorssundries.htm)
N-Type plug/jack inline coaxial surge arrestor | £31 | Diamond SP-3000P <ul><li>[Waters and Stanton](http://hamradiostore.co.uk/diamond-sp-3000p-lightning-arrester.html)</li><li>[Radio World](http://www.radioworld.co.uk/static_discharge_protector/diamond-sp-3000p-lightning-surge-arrester-dc-3000-mhz-400w-pep-n-type-plug-to-n-type-socket)</li></ul>
Ethernet Network surge arrestor    | £20 | <ul><li>eg [APC PNET1GB](http://amzn.to/2kzWMq8)</li><li>or [LRS01-E100](http://amzn.to/2m9gVjl)</li></ul>

 * The IP67 rated case that we're using has N-type connector holes, so we're
standardising on N-type connectors for the antenna lead.
 * There are two types of N-type connectors, 75Ω and 50Ω. They are not directly
compatible, despite appearing superficially similar. This project uses
**only 50Ω** connectors.
 * This build uses LMR200 or equivalent coaxial cable for the antenna; this
 is a very similar diameter to RG58. The larger diameter LMR400 would also be
 suitable (and would require alternative N-type connectors), this would provide
 a much lower resistance (0.1Ω/m vs 0.3Ω/m) and longer range.
 * Regardless of the cable diameter, is recommended that the the coaxial length
 is kept to the minimum, ideally less than 1.5m.

### Board Mounting

For the Raspberry Pi, interface board and concentrator board:

Part | Qty | Link
-----|-----|-----
M2.5×8 nylon set screw | 6 | [Farnell 2472704](http://uk.farnell.com/duratool/dtrnse-1207-m2-5-8/set-screw-slotted-cheese-m2-5/dp/2472704)
M2.5 nylon nut | 6 | [Farnell 2472686](http://uk.farnell.com/duratool/dtrnne-34814-m2-5/nut-nylon-full-d-chamfer-m2-5/dp/2472686)
Brass spacer 4.5×3mm | 4 | [Farnell 1466898](http://uk.farnell.com/ettinger/05-52-033/spacer-2-5-4-5x3-ni/dp/1466898)
Brass Jack screw (male-female standoff) M2.5×11 | 4 | [Farnell 2494583](http://uk.farnell.com/ettinger/05-12-113/standoff-hex-m-f-brass-11mm-m2/dp/2494583)

For the Pololu DC-DC converter:

Part | Qty | Link
-----|-----|-----
M2x5 nylon set screw | 4 | [Farnell 2528936](http://uk.farnell.com/unbranded/mrpm020005cb/screw-pan-head-pz-m2-x-5mm-pk50/dp/2528936)
M2×10 standoff (female-female) | 2 | [Farnell 2494574](http://uk.farnell.com/ettinger/05-01-103/standoff-hex-female-brass-10mm/dp/2494574)

### Power Supply

To reduce the cabling and case openings in the IP67 case, the raspberry pi is
supplied with power using Power over Ethernet (PoE). The chosen Pololu DC-DC
converter can step down voltages up to 32V, but PoE supplied at 12V or 24V
will be sufficient.

Part | Cost | Link
-----|------|-----
A 12V supply with a 2.1mm DC plug *OR* a proper POE injector | |
Cat5E or Cat6 Ethernet cable (for external use) | |
8P8C (aka RJ45) connectors | |

### Antenna option 1

Quarter wave ground plane:

Part | Price | link
-----|-------|------
¼ wave ground plane omnidirectional antenna | £45 | Aurel GP868 [(Conrad Electronic)](http://www.conrad-electronic.co.uk/ce/en/product/190123/Aurel-650200599-GP-868-Ground-Plane-Antenna-Assembly-kit-NA)
BNC jack to N-type plug adaptor | £5 | TE Connectivity 1-1337567-0 [(Farnell)](http://uk.farnell.com/webapp/wcs/stores/servlet/ProductDisplay?partNumber=1205981)

 * This antenna is has an F-type jack and is supplied with 2.5m RG58 cable
which is terminated in a BNC plug. The BNC jack to N-type plug adaptor allows connection
to the N-type pigtail in the case.

### Antenna option 2

Half wave dipole:

Part | Price | link
-----|-------|-----
Barracuda 5dbi omnidirectional antenna | £90 | Digikey 931-1255-ND [Taoglas OMB.8912.05F21](http://www.digikey.co.uk/products/en?keywords=OMB.8912.05F21)

### Tools:

Part | Price | Order link
-----|-------|-----------
N-type coax connector crimping tool for LMR200/RG58 | £24 | <ul><li>HT-336G ([TME](http://www.tme.eu/en/details/ht-336g/crimping-tools-for-hf-connectors/))</li><li>336G ([Solwise](http://www.solwise.co.uk/wireless_sundries.htm))</li></ul>
RG58/LMR200/HDF200 coaxial stripping tool | £12 | 332D Stripping tool ([Solwise](http://www.solwise.co.uk/wireless_sundries.htm))

### Testing

The following parts are not required for the build, but might be useful for
on-the-bench testing. No more than one of each is needed:

Part                             | Price                 | Order/link
---------------------------------|-----------------------|--------------------
UF.L to SMA pigtail lead for test antenna  | EUR 6.5 (excl. tax)   | [IMST Webshop](http://webshop.imst.de/pigtail-for-ic880a-spi-and-ic880a-usb.html)
SMA ¼ wavelength Test Antenna    | £5–£10      | <ul><li>3dBi: [Farnell](http://uk.farnell.com/rf-solutions/ant-8whip3h-sma/ant-868mhz-peitsche-gelenk-sma/dp/2305899)</li><li>5dBi: [Aliexpress](http://www.aliexpress.com/item/Free-shipping-customized-best-performance-High-Gain-5dBi-GSM-868mhz-900-1800mhz-magnetic-base-antenna/32592017287.html)</li><li>7dBi: [Aliexpress](http://www.aliexpress.com/store/product/868MHZ-915MHZ-GSM-3G-antenna-small-sucker-7dbi-aerial-3meters-SMA-male-2/1859567_32512220307.html)</li><li>7.5dBi [Aliexpress](http://www.aliexpress.com/item/868MHz-Antenna-7-5dBi-Gain/1870152812.html)</li>
RPi Power Supply 2A with micro USB   | £9              | [Farnell](http://uk.farnell.com/stontronics/t5454dv/netzteil-raspberry-pi-5v-2a-micro/dp/2427498)
