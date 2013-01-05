<!--	Vera Plugin for Nest Thermostats	-->

![Devices in Vera](http://cocu.la/vera/nest/shot5.jpg)

## Purpose ##
This plugin will monitor and control your [Nest][] thermostat(s) through your [Vera][] home automation gateway.

[nest]: http://www.nest.com
[vera]: http://www.micasaverde.com

## Features ##

* Monitor thermostat mode, fan mode, current and set point temperatures, humidity level, running states, battery level and "home/away" status.

* Control temperature set points, thermostat mode and fan mode.

* Set your house to Home or Away, switching all your Nest thermostats to an energy-saving mode when unoccupied.

## How to Use the Plugin ##

Open the settings for the Nest device that was created at plugin installation, and set your `nest.com` user name and password.  Also choose a polling frequency (in seconds) if you want to poll more or less often than the default 120 seconds.  You may not poll more often than every 60 seconds, as this might be considered abusive by the `nest.com` servers.

Shortly after saving your changes, the plugin will login to `nest.com` and retrieve all of the locations associated with your account, and all the thermostats within each location.  Since the Nest also contains a humidity sensor, the plugin also creates a humidistat device for each thermostat it discovers.  The plugin will create the devices in Vera using the names discovered from `nest.com`.  Shown below is an example list of devices that have been created in your Vera:

    Nest			(your account information)
    Home			(your home or other location)
    Ground Floor	(thermostat functions)
    Ground Floor	(humidistat functions)
    Master Bedroom	(2nd thermostat in Home)
    Master Bedroom	(humidistat functions)
    ...

## Notes and Limitations ##

* Only works with Vera UI5 1.5.408 or later.

* The plugin is based on an unsupported interface to Nest, which may break or otherwise become inaccessible at any time.

* Updates to the state of the location, thermostat and humidistat devices can take up to the polling number of seconds (120 by default) to be reflected in the UPnP devices (or as quickly as 5 seconds).  I believe that the current approach will work for almost all users, but please [contact me][me] if you have different needs.

[me]: http://forum.micasaverde.com/index.php?action=profile;u=19018

* `BatteryLevel` percentage is tied to a voltage range of 3.6 to 3.9 volts.  Any actual voltage below 3.6 is considered 0% and any voltage above 3.9 is considered 100%.  (Battery level may be important to some users, as the battery is charged by leeching power during heating or cooling.  If there is a problem, the battery level will continue to decline until the device turns off the proximity function, wi-fi and then itself.)

* The humidistat device currently only implements the humidity sensor service, but as the [2nd generation Nest thermostats][2g] can also control humidifers and dehumidifiers, this device type will be extended to implement new services.

[2g]: http://nest.com/blog/2012/10/02/the-next-generation-nest-thermostat/

* The "Home/Away" device implements `urn:schemas-upnp-org:service:HouseStatus:1`, but since I didn't find an `S_HouseStatus1.xml` file in my Vera, one is packaged with the plugin. (This device also implements `urn:schemas-upnp-org:service:SwitchPower:1`.)

* The thermostat device creates the `UserSuppliedWattage` variable, set initially to `0,0,0`, but it doesn't yet do anything else to implement the `urn:micasaverde-com:serviceId:EnergyMetering1` service.

## License ##

Copyright &copy; 2012  John W. Cocula and others

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program.  If not, see <http://www.gnu.org/licenses/>

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

