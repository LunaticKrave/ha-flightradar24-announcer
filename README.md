# FlightRadar24 Low-Flying Plane Announcer

A Home Assistant automation that announces low-flying aircraft overhead using FlightRadar24 data and AI-generated voice notifications.

## Overview

This automation monitors aircraft passing overhead and announces details about planes flying below 15,000 feet. It uses ChatGPT to generate natural, enthusiastic announcements about the aircraft, including flight details, route information, and interesting facts about special or vintage aircraft.

Perfect for aviation enthusiasts who want to know when interesting planes are flying over their home!

## Features

- 🛩️ **Real-time monitoring** of visible flights via FlightRadar24
- 📢 **Voice announcements** on Home Assistant voice satellites
- 🤖 **AI-generated descriptions** using ChatGPT for natural-sounding announcements
- ⏰ **Time-based filtering** (only announces during specified hours)
- 🏠 **Presence detection** (only announces when you're home)
- 🔇 **Smart volume management** (temporarily adjusts volume, then restores)
- ⏱️ **Cooldown period** to prevent announcement spam
- ✈️ **Local airport awareness** (converts airport codes to city/airport names)

## Requirements

### Integrations

1. **FlightRadar24 Integration**
   - Install from HACS or manually
   - Configure with your location and preferences
   - Must have a `sensor.visible_flights` entity

2. **OpenAI Conversation Integration**
   - Configure ChatGPT as a conversation agent
   - Required for generating natural language announcements

3. **Voice Assistant/Satellite**
   - A configured Home Assistant voice satellite or media player
   - Capable of text-to-speech announcements

### Entities Required

- `sensor.visible_flights` - FlightRadar24 sensor
- `person.home_user` - Person entity for presence detection
- `media_player.your_voice_assistant` - Media player for announcements
- `conversation.chatgpt` - ChatGPT conversation agent

## Installation

1. **Clone or download this repository**

2. **Copy the YAML file** to your Home Assistant configuration

3. **Edit the configuration** (see Configuration section below)

4. **Reload automations** in Home Assistant

5. **Test** by triggering manually or waiting for a low-flying plane

## Configuration

Open `low-flight-announcer.yaml` and update the following:

### Required Changes

| Setting | Line(s) | What to Change |
|---------|---------|----------------|
| Person Entity | ~17 | Replace `person.home_user` with your person entity |
| Device ID | ~49 | Replace `YOUR_DEVICE_ID_HERE` with your voice satellite device ID |
| Media Player | ~38, ~42, ~54 | Replace `media_player.your_voice_assistant` with your media player entity |

### Finding Your Device ID

1. Go to **Settings** → **Devices & Services**
2. Find your voice assistant device
3. Click on it and look for the device ID in the URL or Device Info section

### Optional Customization

| Setting | Line(s) | Default | Description |
|---------|---------|---------|-------------|
| Active Hours | ~9-11 | 8am-11pm | Time window when announcements are active |
| Altitude Threshold | ~13 | 15,000 feet | Maximum altitude to trigger announcements |
| Cooldown Period | ~56 | 5 minutes | Wait time between announcements |
| Volume Level | ~42 | 0.5 (50%) | Temporary volume for announcements |

### Local Airport Configuration

The automation includes built-in awareness of two local airports:
- **KLCK/LCK** - Rickenbacker Airport
- **KCMH/CMH** - Port Columbus Airport

To add your local airports, modify the ChatGPT prompt around line 44 to include your airport codes and names.

## How It Works

1. **Trigger**: Monitors `sensor.visible_flights` for state changes
2. **Conditions Check**:
   - Current time is between 8am and 11pm
   - At least one plane is below 15,000 feet
   - You are home
3. **Data Collection**: Identifies the lowest flying aircraft
4. **AI Generation**: ChatGPT creates a natural announcement with flight details
5. **Announcement**: 
   - Sets volume to 50%
   - Creates a persistent notification
   - Announces via voice satellite
   - Restores original volume
6. **Cooldown**: Waits 5 minutes before the next possible announcement

## Example Announcements

> "A Southwest Airlines Boeing 737 is passing overhead at 8,500 feet, traveling from Chicago to Port Columbus Airport at 420 knots."

> "An American Airlines Airbus A320 is descending toward Port Columbus at 12,000 feet, coming in from Dallas."

> "Heads up! A vintage DC-3 is flying low at just 3,200 feet over the area—what a rare sight!"

## Troubleshooting

### No announcements happening

- Check that FlightRadar24 integration is working (`sensor.visible_flights` has data)
- Verify you're home (person entity state is "home")
- Confirm current time is within the active window (8am-11pm)
- Check that there are actually planes below 15,000 feet
- Look at Home Assistant logs for errors

### Announcements are too frequent

- Increase the cooldown period (line ~56)
- Raise the altitude threshold to catch fewer planes

### Volume issues

- Adjust the temporary volume level (line ~42)
- Check that your media player supports volume control

### Airport codes not converting

- Update the ChatGPT prompt with your local airport codes and names
- Ensure the prompt clearly maps codes to city/airport names

## Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest new features
- Submit pull requests
- Share your customizations

## License

This project is open source and available under the MIT License.

## Acknowledgments

- FlightRadar24 for flight data
- Home Assistant community
- OpenAI for ChatGPT integration

## Support

If you find this automation useful, please star the repository! ⭐

For issues or questions, please open an issue on GitHub.

---

**Note**: This automation requires active FlightRadar24 integration and may be affected by API rate limits or data availability. Announcement frequency and accuracy depend on the quality of FlightRadar24 data in your area.
