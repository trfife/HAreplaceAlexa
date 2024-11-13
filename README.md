# Integrating Alexa Echo Devices with Home Assistant

This repository demonstrates integrating Alexa Echo devices with Home Assistant using Node-RED to capture voice commands, process them, and forward the action to Home Assistant Assist. Below is an overview and the Node-RED flow configuration.

## Overview
The goal is to use Alexa Echo for voice recognition and synthesis while handling command processing with Home Assistant Assist. Here’s how it works:

1. **Capture the Echo voice event** via `alexa-remote-event`.
2. **Extract the text prompt** by removing the wake word.
3. **Forward the command to Home Assistant Assist**.

This flow listens for voice activity on the Alexa device, strips out the wake word, clears any extra data, and forwards the cleaned command text to be processed by Home Assistant.

## Node-RED Flow Configuration

```json
[
    {
        "id": "35bdb0e0.53f29",
        "type": "alexa-remote-event",
        "z": "35964044.fcf53",
        "name": "",
        "account": "af59a603f2d84552",
        "event": "ws-device-activity",
        "x": 110,
        "y": 40,
        "wires": [
            [
                "a26a3b25.554268",
                "9f3898f9dbb2e102"
            ]
        ],
        "outputLabels": [
            "Test."
        ]
    },
    {
        "id": "a26a3b25.554268",
        "type": "change",
        "z": "35964044.fcf53",
        "name": "Strip Wake Word",
        "rules": [
            {
                "t": "change",
                "p": "payload.description.summary",
                "pt": "msg",
                "from": "^(alexa )?",
                "fromt": "re",
                "to": "",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 330,
        "y": 40,
        "wires": [
            [
                "b85b7c8f.607a2"
            ]
        ]
    },
    {
        "id": "b85b7c8f.607a2",
        "type": "change",
        "z": "35964044.fcf53",
        "name": "Clear Data",
        "rules": [
            {
                "t": "delete",
                "p": "payload.data",
                "pt": "msg"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 510,
        "y": 40,
        "wires": [
            [
                "64f34a83.dfdb14",
                "c330f587e7946730"
            ]
        ]
    },
    {
        "id": "c330f587e7946730",
        "type": "link out",
        "z": "35964044.fcf53",
        "name": "llm",
        "mode": "link",
        "links": [
            "e80a7fc903c06573",
            "17c4fde9a2d0df8e"
        ],
        "x": 625,
        "y": 140,
        "wires": []
    },
    {
        "id": "af59a603f2d84552",
        "type": "alexa-remote-account",
        "name": "AlexaInfo",
        "authMethod": "proxy",
        "proxyOwnIp": "192.168.86.69",
        "proxyPort": "3456",
        "cookieFile": "",
        "refreshInterval": "10",
        "alexaServiceHost": "pitangui.amazon.com",
        "pushDispatchHost": "",
        "amazonPage": "amazon.com",
        "acceptLanguage": "en-US",
        "onKeywordInLanguage": "on",
        "userAgent": "",
        "usePushConnection": "on",
        "autoInit": "on",
        "autoQueryActivityOnTrigger": "on"
    }
]
