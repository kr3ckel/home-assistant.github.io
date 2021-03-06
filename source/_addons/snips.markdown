---
layout: page
title: "Snips.ai"
description: "Enhance your Hass.io installation with a local voice assistant."
date: 2017-04-30 13:28
sidebar: true
comments: false
sharing: true
footer: true
---

[Snips.ai] is an AI-powered voice assistant that runs on the Raspberry Pi 3 and x86 platforms. It runs on-device and is Private by Design.

To get started, follow [their tutorial] to create an assistant and download the training data.

Now install and activate the [Samba] add-on so you can upload your training data. Connect to the "share" Samba share and copy your training data over. Name the file `assistant.zip`.

Now it's time to start Snips for the first time. When the Snips add-on starts, it will output your audio devices:

```plain
**** List of PLAYBACK Hardware Devices ****
card 0: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
  Subdevices: 8/8
  Subdevice #0: subdevice #0
  Subdevice #1: subdevice #1
  Subdevice #2: subdevice #2
  Subdevice #3: subdevice #3
  Subdevice #4: subdevice #4
  Subdevice #5: subdevice #5
  Subdevice #6: subdevice #6
  Subdevice #7: subdevice #7
card 0: ALSA [bcm2835 ALSA], device 1: bcm2835 ALSA [bcm2835 IEC958/HDMI]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

You need to use this information to configure the `mic` and `speaker` configuration options. The format is `<card #>,<device #>`. On a Raspberry Pi 3, `0,0` is the built-in headset port, `0,1` is the HDMI port.

Now that you've found the microphone and speaker addresses, it's time to configure Snips and restart the add-on.

### Add-On configuration

```json
{
  "mic": "1,0",
  "speaker": "1,0",
  "assistant": "assistant.zip",
  "mqtt_bridge": {
    "active": true,
    "host": "172.17.0.1",
    "port": 1883,
    "user": "",
    "password": ""
  },
}
```

Configuration variables:

- **mqtt_bridge** (*Optional*): Snips uses MQTT to communicate and defaults to their own broker. Use this config option to bridge their broker to your own.
- **mic**: This is the hardware address of your microphone. Look at the Snips 

### {% linkable_title Home Assistant configuration %}

Use the Home Assistant [Snips.ai component][comp] to integrate the add-on into Home Assistant.

```yaml
snips:
```

[Snips.ai]: https://snips.ai/
[their tutorial]: https://github.com/snipsco/snips-platform-documentation/wiki/2.-Running-your-first-end-to-end-assistant
[Samba]: /addons/samba/
[comp]: /components/snips/
