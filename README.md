# HA_video_Hack

## Overview
This project provides a guide and automation script for integrating Kodi with Home Assistant to control video playback using local storage. The automation script is designed to start playing a video playlist on Kodi at a specific time.

## Prerequisites
- Home Assistant installed and running on your local network.
- Kodi installed on a Batocera distribution with the web interface enabled.
- Local video files stored on the same network as Home Assistant and Kodi.

## Setup Instructions

### 1. Enable Kodi Web Interface
Ensure that the web interface is enabled on your Kodi installation. This is necessary for Home Assistant to discover and control Kodi.

1. Open Kodi and go to `Settings`.
2. Select `Services`.
3. Under `Control`, enable `Allow remote control via HTTP`.
4. Note the port number (default is 8080) and the username/password if set.

### 2. Add Kodi Integration to Home Assistant
Add the Kodi integration to your Home Assistant instance using the following steps:

1. Open Home Assistant and go to `Configuration`.
2. Select `Integrations`.
3. Click on the `+ Add Integration` button.
4. Search for `Kodi` and select it.
5. Follow the prompts to complete the setup, providing the IP address, port, username, and password for your Kodi installation.

### 3. Create Automation Script
Create an automation script in Home Assistant to control Kodi for video playlist playback.

1. Navigate to the Home Assistant configuration directory.
2. Create a new file named `kodi_automation.yaml` with the following content:

```yaml
automation:
  - id: kodi_play_video_playlist
    alias: "Kodi: Play Video Playlist"
    trigger:
      - platform: time
        at: "20:00:00"  # This is the time you want the playlist to start, for example, 8 PM.
    action:
      - service: media_player.turn_on
        target:
          entity_id: media_player.kodi
      - service: kodi.call_method
        target:
          entity_id: media_player.kodi
        data:
          method: Playlist.Clear
          params:
            playlistid: 1  # Playlist ID for video
      - service: kodi.call_method
        target:
          entity_id: media_player.kodi
        data:
          method: Playlist.Add
          params:
            playlistid: 1  # Playlist ID for video
            item:
              file: "/local/media/playlist.m3u8"  # Replace with the path to your playlist file
      - service: kodi.call_method
        target:
          entity_id: media_player.kodi
        data:
          method: Player.Open
          params:
            item:
              playlistid: 1  # Playlist ID for video
```

### 4. Modify Script for Your Setup
Edit the `kodi_automation.yaml` file to match your specific setup:

- Replace `media_player.kodi` with the entity ID of your Kodi media player in Home Assistant.
- Replace `/local/media/playlist.m3u8` with the path to your actual playlist file.

### 5. Reload Automations
Reload the automations in Home Assistant to recognize the new automation script:

1. Open Home Assistant and go to `Configuration`.
2. Select `Server Controls`.
3. Click on the `Reload Automations` button.

## Conclusion
You have successfully set up an automation script to control Kodi for video playlist playback using Home Assistant. The script will start playing the specified video playlist on Kodi at the configured time.

For any issues or further customization, refer to the Home Assistant and Kodi documentation.

## Repository
The automation script and setup instructions are available in this GitHub repository: [HA_video_Hack](https://github.com/jhacksman/HA_video_Hack)
