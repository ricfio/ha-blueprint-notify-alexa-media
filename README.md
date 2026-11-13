# 📢 Advanced Alexa Media Player Notification Script for Home Assistant

[![Home Assistant min version](https://img.shields.io/badge/Home%20Assistant-%3E%3D%202025.4.4-blue.svg)](https://homeassistant.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A powerful, feature-rich Home Assistant script blueprint designed to handle notifications through the **[Alexa Media Player](https://github.com/custom-components/alexa_media)** integration (see official [Notification Service Documentation](https://github.com/custom-components/alexa_media/wiki/Notification-Service)) with advanced targeting, SSML formatting, international voice options, speech rate control, sound effects, and execution conditions.

---

## ✨ Features

- **Multiple Notification Types**:
  - **Text-to-Speech (TTS)**: Speak text directly on Alexa devices.
  - **Announce**: Send an Alexa announcement, optionally preceded by a sound preset or custom sound URI.
  - **Mobile Push**: Send push notifications to the Alexa app.
- **Flexible Targeting Modes**:
  - **All Alexa Devices**: Broadcasts to every discovered `alexa_media` device.
  - **Selected Alexa Devices**: Target specific media player entities.
  - **Dynamic Alexa Device**: Resolve target dynamically from a sensor entity (e.g. `sensor.last_alexa`).
  - **Last Called Alexa Device**: Automatically target the device that was most recently interacted with.
- **Advanced Voice & Speech Controls**:
  - **International Voices**: Supports multi-language Polly/Alexa voices across Italian (`it-IT`), English US (`en-US`), English UK (`en-GB`), Spanish (`es-ES`/`es-MX`), French (`fr-FR`), and German (`de-DE`).
  - **Speech Rate, Volume & Pitch Control**: Adjust speech speed (`x-slow` to `x-fast`), volume (`silent` to `x-loud`), and pitch (`x-low` to `x-high`) via SSML `prosody`.
  - **Sound Bank Prerolls**: Prepend official Amazon sound effects (doorbells, chimes, alarms) using SSML `<audio>`.
  - Whispered speech effect (with built-in safety check to prevent conflicts with unsupported voices like Bianca).
  - Full **[Amazon Alexa SSML Reference](https://developer.amazon.com/en-US/docs/alexa/custom-skills/speech-synthesis-markup-language-ssml-reference.html)** support (fully functional for both TTS and Announce, contrary to official integration documentation).
- **Condition Helpers**:
  - Require specific `input_boolean` helpers to be **ON** or **OFF** before executing the notification.
- **Smart Title Fallback**:
  - Automatically defaults push notifications to `"Home Assistant"` if no custom title is provided.
- **Parallel Execution**:
  - Configured in `parallel` mode (`max: 10`) to handle simultaneous notification triggers smoothly.

---

## 📋 Prerequisites

1. Home Assistant version `2025.4.4` or newer.
2. [Alexa Media Player](https://github.com/custom-components/alexa_media) integration installed and configured via HACS.

---

## 📥 Installation

Click the badge below to import this blueprint directly into your Home Assistant instance:

[![Open your Home Assistant instance and show the blueprint import dialog](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?repository_url=https%3A%2F%2Fgithub.com%2Fricfio%2Fha-blueprint-notify-alexa-media)

Alternatively, you can manually copy `blueprints/script/notify-alexa-media.yaml` into your Home Assistant `config/blueprints/script/notify-alexa-media.yaml` file.

---

## 🎛️ Input Parameters Reference

| Field | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| **Notification Type** | Select | `tts` | Delivery method: TTS, Announce, or Mobile Push. |
| **Notification Title** | Text | *Blank* | Title for Announce and Mobile Push (defaults to "Home Assistant" for push if empty). |
| **Notification Message** | Text (Multiline) | **Required** | Message content. Supports Alexa [SSML markup](https://developer.amazon.com/en-US/docs/alexa/custom-skills/speech-synthesis-markup-language-ssml-reference.html). |
| **Voice** | Select | `(default)` | International Alexa voices (IT, US, UK, ES, FR, DE). Note: Bianca does not support whispers. |
| **Whispered Speech** | Boolean | `false` | Enables whispered speech effect (TTS/Announce only). |
| **Speech Rate** (`rate`) | Select | `medium` | Controls speech speed via SSML prosody (`x-slow` to `x-fast`). |
| **Speech Volume** (`volume`) | Select | `medium` | Controls speech volume via SSML prosody (`silent` to `x-loud`). |
| **Speech Pitch** (`pitch`) | Select | `medium` | Controls speech pitch/tone via SSML prosody (`x-low` to `x-high`). |
| **Sound Preset / Chime Preroll** | Select | `none` | Prepend a curated Amazon Sound Bank audio clip before the message. |
| **Sound URI** | Text | *Blank* | Optional custom Amazon Sound Bank URI; overrides the preset. See the full [Amazon Sound Library](https://developer.amazon.com/en-US/docs/alexa/custom-skills/ask-soundlibrary.html). |
| **Display Announcement** (`display`) | Boolean | `false` | Also display the announcement on compatible Alexa devices (Announce only). |
| **Alexa Target Selection** | Select | `selected` | Target mode (`all`, `selected`, `current`, `last_called`). |
| **Alexa Target Sensor** | Entity (Sensor) | *None* | Sensor containing device entity ID (used in Dynamic mode). |
| **Media Player Entities** | Entity (Media Player) | `[]` | Specific Alexa devices to target (used in Selected mode). |
| **Skip Unavailable Devices** (`skip_unavailable`) | Boolean | `true` | Exclude unavailable or unknown media players. |
| **Required Helpers ON** (`require_on`) | Entity (Input Boolean) | `[]` | Send only if all selected helpers are **ON**. |
| **Required Helpers OFF** (`require_off`) | Entity (Input Boolean) | `[]` | Send only if all selected helpers are **OFF**. |

---

## 🚀 Usage Example in an Automation

Once you create a script from this blueprint (e.g., named `script.alexa_notify`), you can call it from any automation:

```yaml
alias: "Notify Living Room Alexa"
trigger:
  - platform: state
    entity_id: binary_sensor.front_door
    to: "on"
action:
  - service: script.alexa_notify
    data:
      type: announce
      title: "Front Door"
      message: "The front door has been opened."
      voice: Carla
      rate: medium
      sound_uri: "soundbank://soundlibrary/home/amzn_sfx_doorbell_chime_01"
      whisper: false
      target_mode: selected
      target_entities:
        - media_player.living_room_echo
```

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
