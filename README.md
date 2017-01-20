# RPi Gateways

This build is heavily based on [ttn_zn's build](https://github.com/ttn-zh/ic880a-gateway),
but we're extending it to be standardised for several deployments, IP67 rated,
with some access hardening, NAT punching and remote logging.

## Shopping List

The following list is incomplete:

Part                             | Price                 | Order/link
---------------------------------|-----------------------|--------------------
iC880a concentrator board        | EUR 155.- (excl. tax) | [IMST Webshop](http://webshop.imst.de/ic880a-spi-lorawan-concentrator-868mhz.html)
Pigtail for antenna <sup>1</sup> | EUR 6.5 (excl. tax)   | [IMST Webshop](http://webshop.imst.de/pigtail-for-ic880a-spi-and-ic880a-usb.html)
Raspberry Pi 2                   | £40                   | RPi2: [Farnell](http://de.farnell.com/raspberry-pi/rpi2-modb-v1-2/sbc-raspberry-pi-2-model-b-v1/dp/2612474) and RPi3: [Farnell](http://de.farnell.com/raspberry-pi/raspberrypi-modb-1gb/raspberry-pi-3-model-b/dp/2525225)
MicroSD Card (4Gb or more)       | ~ EUR 8               | A Sandisk Extreme/Ultra in 8Gb or 16Gb is preferred (based on usage bias)
RPi to iC880a interface          | £7                    | supplied from; <ul><li>[Tindie](https://www.tindie.com/products/gnz/imst-ic880a-lorawan-backplane/)</li><li>from @DefProc, with headers</li></ul>


For internal testing only:

Part                             | Price                 | Order/link
---------------------------------|-----------------------|--------------------
Test Antenna <sup>3</sup>             | EUR 3.50 - EUR 8      | 3dBi: [Farnell](http://de.farnell.com/rf-solutions/ant-8whip3h-sma/ant-868mhz-peitsche-gelenk-sma/dp/2305899), 5dBi: [Aliexpress](http://www.aliexpress.com/item/Free-shipping-customized-best-performance-High-Gain-5dBi-GSM-868mhz-900-1800mhz-magnetic-base-antenna/32592017287.html), 7dBi: [Aliexpress](http://www.aliexpress.com/store/product/868MHZ-915MHZ-GSM-3G-antenna-small-sucker-7dbi-aerial-3meters-SMA-male-2/1859567_32512220307.html), 7.5dBi [Aliexpress](http://www.aliexpress.com/item/868MHz-Antenna-7-5dBi-Gain/1870152812.html)
Test Power Supply 2A with micro USB   | EUR 7.84              | [Farnell](http://de.farnell.com/stontronics/t5454dv/netzteil-raspberry-pi-5v-2a-micro/dp/2427498)
