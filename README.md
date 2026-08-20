# PicoFM

FM transmitter on a pi pico w.

Flash **`../picow-bt-fm.uf2`** (Pico W)

RF comes out on **GPIO21**.

**I don't recommend connecting an antenna to GPIO21 AS THAT COULD BE ILLEGAL**
since it extends the transmit distance.

HUGE thanks to [THIS GUY](https://github.com/kaduhi/pico-fractional-pll) for
sharing his beautiful work.

## Setup

1. Power it on.
2. Connect your phone or computer to the WiFi network it puts out:
   - **SSID:** `fmbeast`
   - **Password:** `dinozaver`
3. Go to **`192.168.4.1`** in a browser to set the frequency.
4. Pair to it over Bluetooth.

## Frequencies

All 206 channels from 87.5 to 108.0 MHz work. Deviation is set per channel

| Deviation | Sounds | Channels |
|---|---|---|
| **7.5 kHz** | very quiet | 88.5, 90.0, 91.5, 93.0, 94.5, 96.0, 97.5, 99.0, 100.0, 100.8, 101.6, 102.4, 103.2, 104.0, 104.8, 105.6, 107.0, 108.0 |
| **42.5 kHz** | slightly quiet | 87.7, 89.2, 90.7, 92.2, 93.7, 95.2, 96.7, 98.2, 99.7 |
| **57.5 kHz** | barely quieter | 87.8, 89.3, 90.8, 92.3, 93.8, 95.3, 96.8, 98.3 |
| **75 kHz** | full volume | everything else (171 channels) |

Good picks, all at full 75 kHz: **88.1, 92.1, 94.9, 98.1, 101.1, 104.3, 107.3**.

## Audio

Mono, 12 kHz bandwidth.

50 µs pre-emphasis is applied, so it won't sound muffled. Bright material can
clip turn the volume down if it sounds harsh.

Sources are kaduhi's `pico-extras` and `pico-playground` forks — the
`usb_sound_card_fm_transmitter`.
