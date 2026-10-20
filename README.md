# Shack Relay Controller

A 10-channel relay controller for a ham radio shack, switched from any
phone or browser on the home network through a simple web page. Output
states are kept in EEPROM, so the shack remembers its last state after a
power cut.

## What it controls

- General shack power
- A Flex 6300 SDR transceiver
- Six antenna switching banks

## Hardware

- Arduino (Uno/Duemilanove-class board)
- Arduino Ethernet Shield
- 10-channel relay board, wired to the shack's power and antenna relays

## How it works

- The Arduino runs a small web server on the local network (port 9223 by
  default).
- The page shows one row per channel, with ON/OFF buttons and a status
  LED for each.
- Each button click sends a short query string (e.g. `/?H3` / `/?L3`)
  that the Arduino reads and uses to switch the matching relay.
- Output states are written to EEPROM whenever the page has been idle for
  a minute, so the last state survives a power cut.

## Setup

1. Open `ShackRelayController.ino` in the Arduino IDE.
2. In the `CONFIGURATION` section near the top, set `ip`, `gateway`,
   `subnet` and `mac` to match your own network and Ethernet shield
   (the values in the file are examples only).
3. Wire the relay board's inputs to the pins listed in `outputAddress[]`
   (defaults: 3, 5, 6, 7, 8, 9, 14, 15, 16, 17).
4. Upload the sketch, then open `http://<arduino-ip>:9223/` from a
   device on the same network.

## Known limitations

- No password protection on the control page (inherited from the
  original template) — anyone on the local network can open it.
- Built for a trusted home LAN. Do not port-forward it straight to the
  public internet without adding authentication in front of it.
