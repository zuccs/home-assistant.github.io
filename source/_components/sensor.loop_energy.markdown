---
layout: page
title: "Loop Energy"
description: "Instructions on how to integrate Loop Energy devices within Home Assistant."
date: 2016-04-02 22:00
sidebar: true
comments: false
sharing: true
footer: true
logo: loop.png
ha_category: Energy
ha_release: 0.17
ha_iot_class: "Cloud Push"
---


Integrate your [Loop Energy](https://www.your-loop.com/) meter information into Home Assistant. To use this sensor you need the client serial number and secret keys for your devices.

The library used to get the data isn't officially supported and the only way to get the keys is to log into loop energy's website and type a command into your browser console.

To do this log into [Loop Energy](https://www.your-loop.com/). Once you're logged in you should be able see your live readings on the web page.

You can then open your browser's console window, how you do this varies by browser but in Chrome you click on `More Tools / Developer Tools' and click on the console window. You then type:

`Drupal.settings.navetas_realtime.`

This should show something like

```yaml
client_ip: "127.0.0.1"
gas_secret: "GAS_SECRET"
gas_serial: "GAS_SERIAL"
host: "www.your-loop.com"
...
secret: "ELECTRICAL_SECRET"
serial: "ELECTRICAL_SERIAL"
```

The serial and secret tokens are the ones you need. If you just have an electricity monitor then you won't see the gas keys.

Now you have the keys, add the following lines to your `configuration.yaml`, replacing the `*_SERIAL` and `*_SECRET` keys with the ones you found in the console:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: loopenergy
    electricity:
      electricity_serial: 'ELECTRICAL_SERIAL'
      electricity_secret: 'ELECTRICAL_SECRET'
    gas:
      gas_serial: 'GAS_SERIAL'
      gas_secret: 'GAS_SECRET'
```

Configuration variables:

- **electricity_serial** (*Required*): Serial number of your electricity sensor
- **electricity_secret** (*Required*): Secret key for your electricity sensor
- **gas_serial** (*Optional*): Serial number for your gas sensor.
- **gas_secret** (*Optional*): Secret key for your gas sensor.
- **gas_type** (*Optional*): Type of meter `imperial` or `metric`. Defaults to `metric`.
- **gas_calorific** (*Optional*): Calorific value of your gas supply (usually on your gas bill). Defaults to 39.11.

The electricity readings are updated every 10 seconds and the gas readings every 15 minutes.

The gas readings are experimental and not all gas meters are properly supported. So if the data you see doesn't agree with the readings you see via loop energy please report an issue.
