---
title: Vera Plugin for Nest Thermostats and Smoke and Carbon Monoxide Detectors
layout: page
---

## Purpose ##
This plugin will monitor and control your [Nest][] thermostat(s) and/or Protect smoke/CO detector(s) through your [Vera][] home automation gateway.

[nest]: http://www.nest.com
[vera]: http://www.micasaverde.com

## Features ##

* Monitor thermostat mode, fan mode, current and set point temperatures, humidity level, running states, battery level and "home/away" status.

* Control temperature set points, thermostat mode and fan mode.

* Set your house to Home or Away, switching all your Nest thermostats to an energy-saving mode when unoccupied.

* Monitor smoke/CO alarm status and battery level.

<figure>
  <img src="images/screen-shot.jpg" alt="Devices in Vera">
  <figcaption>An example of the Nest devices shown in Vera (UI5).</figcaption>
</figure>

## How to Use the Plugin ##

Open the settings for the Nest device that was created at plugin installation, and set your `nest.com` user name and password.  Also choose a polling frequency (in seconds) if you want to poll more or less often than the default 120 seconds.  You may not poll more often than every 60 seconds, as this might be considered abusive by the `nest.com` servers.

Shortly after saving your changes, the plugin will login to `nest.com` and retrieve all of the locations associated with your account, and all the thermostats and smoke/CO detectors within each location.  Since the thermostat also contains a humidity sensor, the plugin also creates a humidistat device for each thermostat it discovers.  Since the Protect(r) contains both smoke and CO detectors, the plugin will create a separate device for each function.  The plugin will create the devices in Vera using the names discovered from `nest.com`.  Shown below is an example list of devices that have been created in your Vera:

    Nest				(your account information)
    Home				(your home or other location)
    Ground Floor	   		(thermostat functions)
    Ground Floor Humidity		(humidistat functions)
    Master Bedroom			(2nd thermostat in Home)
    Master Bedroom Humidity 	(humidistat functions)
    Hallway (Upstairs) Smoke 	(smoke detector)
    Hallway (Upstairs) CO 		(carbon monoxide detector)
    ...

### Thermostat: Controlling the Precision of Reported Temperatures ###

The Nest thermostat internally reports very precise temperatures in Celsius, but by default the plugin will report whole number values (regardless of whether Fahrenheit or Celsius temperature scales are used).  You can control this however, by specifying your preferred rounding precision with a device variable on the main Nest device called `TemperatureScale`.  The value you provide is the denominator D in the fraction 1/D, the fractional value to which the temperature should be rounded.  For example, providing 2 means that temperatures will be rounded to the nearest 1/2 (0.5) of a degree.  Providing 10 would round to the nearest tenth (0.10) of a degree.

The default value for `TemperatureScale` is 1, meaning only whole degrees are reported by default.

Notes

1. This feature will not allow you to change the setpoint sliders in the user interface to fractional values; that is outside the scope of the plugin.
2. Using this feature may cause unwanted side-effects that are outside the scope of the plugin's control.  Please test your configuration thoroughly before determining that a non-1 value is for you.
3. This feature is agnostic to whether you display temperatures in Fahrenheit or Celsius, but it may be of more value to Celsius users to set `TemperatureScale` to 2 to achieve near-Fahrenheit granularity.


## Notes and Limitations ##

* I am presently only able to test the plugin against UI5 (1.5.622), but there are reports that it is working properly against the latest UI7 firmware.

* The plugin is based on an unsupported interface to Nest, which may break or otherwise become inaccessible at any time.

* The plugin stores your nest.com login credentials in plaintext in device variables which can be displayed clearly in UI5 and potentially other places.

* Updates to the state of the location, thermostat, humidistat, smoke and carbon monoxide detector devices can take up to the polling number of seconds (120 by default) to be reflected in the UPnP devices (or as quickly as 5 seconds).

[me]: http://forum.micasaverde.com/index.php?action=profile;u=19018

* For the thermostat, `BatteryLevel` percentage is tied to a voltage range of 3.6 to 3.9 volts.  Any actual voltage below 3.6 is considered 0% and any voltage above 3.9 is considered 100%.  (Battery level may be important to some users, as the battery is charged by leeching power during heating or cooling.  If there is a problem, the battery level will continue to decline until the device turns off the proximity function, wi-fi and then itself.)

* For the smoke/CO detector, `BatteryLevel` percentage is tied to a voltage range of 4200 to 5400 millivolts.  Any actual millivoltage below 4200 is considered 0%
and any millivoltage above 5400 is considered 100%.  These numbers may change in the future, thereby altering the current percentage after the code change.

* The humidistat device currently only implements the humidity sensor service, but as the [2nd generation Nest thermostats][2g] can also control humidifers and dehumidifiers, this device type will be extended to implement new services.

[2g]: http://nest.com/blog/2012/10/02/the-next-generation-nest-thermostat/

* The "Home/Away" device implements `urn:schemas-upnp-org:service:HouseStatus:1`, but since I didn't find an `S_HouseStatus1.xml` file in my Vera, one is packaged with the plugin. (This device also implements `urn:schemas-upnp-org:service:SwitchPower:1`.)

## License ##

Copyright &copy; 2012-2014  John W. Cocula and others

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program.  If not, see <http://www.gnu.org/licenses/>

The source code repository and issue list can be located at <https://github.com/watou/vera-nest-thermostat>.

## Feedback  ##

Please contact me through the [micasaverde.com forum][me].  All tips are gratefully accepted!

<div  style="text-align:center">
<form action="https://www.paypal.com/cgi-bin/webscr" method="post">
<input type="hidden" name="cmd" value="_s-xclick">
<input type="hidden" name="hosted_button_id" value="ZX4FRDJ5PDRTG">
<input type="image" src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif" border="0" name="submit" alt="PayPal - The safer, easier way to pay online!">
<img alt="" border="0" src="https://www.paypalobjects.com/en_US/i/scr/pixel.gif" width="1" height="1">
</form>
</div>

## Thanks ##

`futzle`, `garretwp`, `guessed`, `RichardTSchaefer` and others on the forum for their kind assistance.
`hugheaves` for his `decompressScript` shell script workaround for the problem with deploying compressed modules and for providing his open-source plugin for a different make/model thermostat.

## Future Plans ##

* See about integrating historical energy usage data into the plugin.
* Have status updates arrive more quickly (if it turns out to be feasible).
* Report the heating or cooling stage, since Nest supports multiple heating and cooling stages.
* More automation.
* Implement Humidistat functionality to control humidity from Vera.

## History ##

### 2014-08-08    v1.8

Fixed issue:

* Remove bad version checking code ([#35](https://github.com/watou/vera-nest-thermostat/issues/35))

### 2014-07-30    v1.7

Enhancements:

* Added the creation of a smoke detector device and carbon monoxide detector device for each Nest Protect(r) that is listed in the user's account at nest.com. ([#32](https://github.com/watou/vera-nest-thermostat/issues/32)).  (Reminder of disclaimer: Do not rely on any of these devices or any aspect of the plugin to protect the health or safety of anyone or anything.  Please read the full license for further information.)
* Add "where" location info to device names (e.g., Hallway, Den, Basement, etc.) on device creation if specified ([#33](https://github.com/watou/vera-nest-thermostat/issues/33))
* Append meaningful descriptions to child device names (Humidity, Smoke, CO) ([#34](https://github.com/watou/vera-nest-thermostat/issues/34))

### 2014-03-22    v1.6

Fixed issue:

* Now, on attempting to get status, nest.com's transport servers can return HTTP 200 (OK) but not the JSON payload we're expecting (likely when the token has expired), and this was causing the code to expect data that wasn't there, killing future polling cycles.  This fix adds better error checking. ([#31](https://github.com/watou/vera-nest-thermostat/issues/31))

### 2014-03-15    v1.5

Fixed issue:

* Replaced use of sslv3 with tlsv1 to fix a new connectivity issue reported by some users ([#30](https://github.com/watou/vera-nest-thermostat/issues/30))

### 2014-01-16    v1.4

Fixed issues:

* Disable Heat or Cool buttons when not can_heat or can_cool ([#28](https://github.com/watou/vera-nest-thermostat/issues/28))
* Move the "Home/Away" device from DEVICE_CATEGORY_SWITCH (3) to DEVICE_CATEGORY_HVAC (5) ([#29](https://github.com/watou/vera-nest-thermostat/issues/29))

### 2013-10-09    v1.3

Fixed issues:

* Added triggers for ModeState changes ([#25](https://github.com/watou/vera-nest-thermostat/issues/25))
* Cached ModeStatus on set so that immediate setpoint changes work ([#26](https://github.com/watou/vera-nest-thermostat/issues/26))
* Added `TemperatureScale` device variable for (optionally) more precise current temperature values ([#27](https://github.com/watou/vera-nest-thermostat/issues/27))

### 2013-07-01    v1.2

Fixed issues:

* Setting home/away fails on Vera2 ([#21](https://github.com/watou/vera-nest-thermostat/issues/21))
* Detect when there is no fan installed ([#24](https://github.com/watou/vera-nest-thermostat/issues/24))

### 2013-04-06    v1.1

Fixed issue:

* The Home/Away command stopped working ([#19](https://github.com/watou/vera-nest-thermostat/issues/19))

### 2013-02-13    v1.0

Fixed issues:

* Send away_timestamp in milliseconds, not seconds ([#9](https://github.com/watou/vera-nest-thermostat/issues/9))
* Improve task message on login failure ([#10](https://github.com/watou/vera-nest-thermostat/issues/10))
* Triggers needed for setting heat/cool setpoints in the other direction ([#11](https://github.com/watou/vera-nest-thermostat/issues/11))
* Added a `Logging` variable to the main Nest account device to increase log output ([#15](https://github.com/watou/vera-nest-thermostat/issues/15))
* Added variable text under the setpoint sliders that shows the current ModeState ([#16](https://github.com/watou/vera-nest-thermostat/issues/16))

### 2012-12-27    v0.9
* Only set device variables if they have changed (to decrease log output)
* Set the `urn:micasaverde-com:serviceId:HaDevice1` variable `LastUpdate` for the location, thermostat and humidistat devices.  For the location device, this is the Unix timestamp most recently retrieved from `nest.com` for that location's status.  For the thermostat and humidistat devices, `LastUpdate` (and `BatteryDate`) are the Unix timestamp from `nest.com` (also without any synchronization to the Vera's clock) that indicates the last time the thermostat connected to `nest.com`.

### 2012-12-15    v0.8
* Decreased the potential for excessive status polling.
* Removed `Clearing...` log message from every poll and other log/debug cleanup.
* Rounded incoming Celsius degrees just like Fahrenheit.  (Sorry for loss of precision; see <http://forum.micasaverde.com/index.php?topic=6133.5>.)

### 2012-12-11    v0.7
* Removed the special case where heat and cool setpoints would show the same value.
* Fixed the plugin's inability to set the thermostat mode to `Off`.

### 2012-12-11    v0.6
* Initial plugin upload to <http://apps.mios.com>

