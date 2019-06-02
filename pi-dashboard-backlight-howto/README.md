# pi-dashboard-backlight-howto

## Concepts
* [bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell))
* [FullPageOS](https://github.com/guysoft/FullPageOS/)
* [Home Assistant](https://www.home-assistant.io/)
* [JSON](https://www.json.org/)
* [MQTT](http://mqtt.org/)
* [Node-RED](https://nodered.org/)
* [Raspberry Pi](https://www.raspberrypi.org/)

Setup and automations for controlling a Raspberry Pi backlight with Home
Assistant, MQTT and Node-RED.

I use a Raspberry Pi as a home automation dashboard in my living room.
Unfortunately, the touchscreen backlight can be quite bright at night. Sure, it
works as an effective night light but it can be a bit annoying when you're
watching TV.

Since the light is a bit dim at night, the Pi will also briefly increase the
screen brightness when it is touched.

## How it works
The Pi touchscreen is exposed to Home Assistant as an MQTT light using Node-RED.
When receiving the correct MQTT messages, Node-RED uses bash commands to set the
state of the screen. Home Assistant can turn the screen on and off, and set the
brightness of the backlight. Home Assistant scenes and automations set the
brightness through the course of the day.

The Node-RED flow features:
* error checking and accepts only valid inputs for power (0 or 1) and 
backlight (0 to 255 inclusive)
* availability/unavailability announcements to Home Assistant
* state reporting back to Home Assistant
* watching for touches (mouse clicks) and temporarily increasing brightness

## Requirements

### Hardware
1. Raspberry Pi (required; tested with Pi 3B)
2. Official Raspberry Pi 7" Touch Screen (required)
3. MicroSD card with Raspbian (or derivative installed) (required; tested with
  [FullPageOS 0.9.0](https://github.com/guysoft/FullPageOS/releases/tag/0.9.0))
4. Official Raspberry Pi power supply (recommended)
5. Official Raspberry Pi Touch Screen Case (optional)

### Systems
1. [Home Assistant](https://www.home-assistant.io/)
2. MQTT Broker e.g. [mosquitto](https://mosquitto.org/)
    * Home Assistant has a guide on [MQTT brokers](https://www.home-assistant.io/docs/mqtt/broker/)

## Method

1. [Install Node-RED on your Pi](https://nodered.org/docs/hardware/raspberrypi). Be sure to:
    * Install the Pi-specific integrations
    * Set Node-RED to autostart on boot
2. Import [the flows](node-red-pi-dashboard-flows.json) in Node-RED
3. Customise the flows with the details of your MQTT broker
4. Add the MQTT light to Home assistant [configuration](ha-configuration.yaml)
5. Use [scenes](ha-scenes.yaml) and [automations](ha-automations.yaml) to change the
backlight through the course of the day
