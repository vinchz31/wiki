# Legrand corner & faq

## Overview

Purpose is to present the finding around pairing Legrand/Netatmo devices (Celiane, Mosaic, dooxie to Zigate and Domoticz
A big thanks to @fgrimaldi who contributes heaviliy to this, a big thanks also to @Thorgal789

## Preamble

By default some of the Legrand/Netamoi devices come with a basic firmware, which do not provide all features. 
In that context, I still feel that having the Legrand Hub is a good investement for upgrading your device, and then switching to Zigate to avoid Legrand cloud.

## Validated devices

* Smart plug - with instant power feedback (if upgraded via the Legrand Hub)
* Switch w/o neutral - with dimmer (if upgraded via Legrand Hub)
* Shutter switch with neutral - Open, Close and Stop
* Micromodule switch - On and Off
* Double gangs remote switch - On/Off/Dimmer

## Prerequisites

* In order to enable the Dimmer feature on the Switch w/o neutral, you must have firmware 3.1b


## Add on

Some specific Legrand settings are accessible via the Web GUI settings page. 

* __EnableLedIfOn__ By enabling this setting, the device Led will be on ( blue ) when the device is On
* __EnableLedInDark__ By enabling this setting, the device Led will be on ( blue ) when the device is Off
* __EnableDimmer__ By enabling this setting, the Switch w/o neutral will get dimmer enable.


## Pairing process

1. Remove the device cover in order to get access to the Factory Reset hole (or button). Do not mix the factory reset hole/button with the led.

1. Enable the Zigate pairing mode and make sure that Domoticz accept new hardware is also on
1. Press the factory reset
   * after few seconds it should be flashing __green/red__ (stay pressing)
   * after around 8 seconds it should be flashing __red__ (the pairing process should be starting)
   * click immediatly on the factory reset once more
   * when turning the led is __green__ the pairing process is completed.
   
Alternative is :

When the led is red (not paired):
* press the reset button until it flash green 3 times
* when the led comes back to red, click once on the reset button
 
## Internals

### Wireless devices

* Not able to use the Tap-Tap for pairing the wireless with an equipment. Could be related to the fact that no group was created yet!
* Requires bindings of 0x0001, 0x000f and 0x0003 (in that order)
* Use the Present Value ( cluster 0x000f / 0x0055 ) to get On/Off or Shutter Up and Down
* While with the Legrand HUB, there is no binding of cluster 0x0006 and 0x0008 for the remote switch, in order to get the Level Control when you do long press/long release, there is a need to bind Cluster 0x0006 and 0x0008

* The Remote coming with the hub is named : 'Master remote SW Home / Away'

### Scenes

* From Master remote SW Home / Away
 * Away : GroupId 0xfff6, SceneId 0x01
 * Home : GroupId 0xfff7, SceneId 0x01
 

### Wired devices

#### Plug-in Unit: Connected outlet

1. Add Group 0xfff7   
   1. Add SceneId 0x01 / Cluster 0x0006
1. Add Group 0xfff5
   1. Add SceneId 0x01 / Cluster 0x0006
1. Bind Cluster 0x0006
1. Configure Report 0x0006 / 0x0001
1. Bind Cluster 0x0b04
1. Configure Report 0x0b04 / 0x050b
1. Bind 0x000f
1. Bind Cluster 0x0003


### Micromodule: Micromodule switch

1. Add Group 0xfff6
   1. Add Scene 0x01 / Cluster 0x0006
1. Add Group 0xfff4
   1. Add Scene 0x01 / Cluster 0x0006
1. Bind 0x0006
1. Configure Report 0x0006 / 0x0001
1. Bind 0x000f
1. Bind 0x0003

### Inter w/o neutral: Dimmer switch w/o neutral

1. Add Group 0xfff6
   1. Add Scene 0x01 / Cluster 0x0006
   1. Add Scene 0x01 / Cluster 0x0008
1. Add Group 0xfff4
   1. Add Scene 0x01 / Cluster 0x0006   
   1. Add Scene 0x01 / Cluster 0x0008
1. Bind 0x0006
1. Configure Report 0x0006 / 0x0001   (Bind and Configure Reporting on 0x0008, will be done when enabling dimmer)
1. Bind 0x000f
1. Bind 0x0003


### Shutter: Shutter switch with neutral

1. Bind 0x000f
1. Bind 0x0003


## Reference

* https://faire-ca-soi-meme.fr/domotique/2018/09/24/test-legrand-celiane-by-netatmo-zigate/
* https://www.legrand.fr/catalogue/maison-connectee/prise-connectee
* https://www.legrand.fr/catalogue/maison-connectee/interrupteur-connecte
