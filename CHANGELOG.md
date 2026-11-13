# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v1.0.0/spec.version.html).

## [1.0.0] - 2026-11-13

### Initial Release

- Initial release of the Advanced Alexa Media Player Notification Script Blueprint.
- **Support for 3 Notification Types**: Text-to-Speech (TTS), Announce, and Mobile Push.
- **Flexible Targeting Modes**: All devices, selected devices, dynamic target from a sensor, and Last Called device.
- **Advanced Voice & Speech Controls**:
  - International Alexa voices across Italian, English (US/UK), Spanish, French, and German.
  - Speech Rate control via SSML `<prosody>`.
  - Speech Volume control (`volume`) via SSML `prosody` (`silent` to `x-loud`).
  - Speech Pitch control (`pitch`) via SSML `prosody` (`x-low` to `x-high`).
  - Official Amazon Sound Bank audio clip prerolls via SSML `<audio>`.
  - Whispered speech support with automatic safety check preventing conflicts with the Bianca voice.
  - Full SSML markup support for both TTS and Announce (despite integration docs).
- **Execution Condition Helpers**: Require specific `input_boolean` helpers to be ON and/or OFF.
- **Smart Title Fallback**: Automatically defaults push notifications to "Home Assistant" if empty.
- **Parallel Execution**: Configured in `parallel` mode (`max: 10`) for concurrent notifications.
- Official Amazon SSML documentation reference links.
