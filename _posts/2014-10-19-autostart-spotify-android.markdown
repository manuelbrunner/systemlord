---
layout: post
title: Autostart Spotify Playlist on Android
---

The goal is to start a spotify playlist based on an event, e.g. when coming home or after connecting the headset in the gym.

# Prerequisites

- Spotify
- Tasker
- AutoInput

# 1) Copy Playlist URL

In the Spotify App, open menu next to your playlist and select “share”, then “copy to clipboard”.

# 2) Create new Tasker Task

- Add action: Net – Browse URL – [Long Click] – Paste
- Add action: Task – Wait – 2 Seconds (time needed to open playlist may vary on your device)
- Add action: Plugin – AutoInput Action – Configuration:
  - Action: Click
  - Field Type: Id
  - Field Id: com.spotify.music:id/labels
