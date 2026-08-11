# rp2040radio

FM transmitter on a Pico. Shows up as a USB sound card — pick it as your output
and whatever plays goes out over the air. Tune it with the BOOTSEL button.

Flash **`../rp2040radio-tunable-picow.uf2`** (Pico W) or
**`../rp2040radio-tunable-pico.uf2`** (plain Pico).

RF comes out on **GPIO21**. Boots on **90.0 MHz**.

## Controls

| Action | Result |
|---|---|
| Tap BOOTSEL | Up one channel (+100 kHz) |
| Hold BOOTSEL | Scan up, 5 channels/sec |
| Power cycle | Back to 90.0 MHz |

Wraps from 108.0 back to 87.5.

The LED blinks the frequency 1.5 s after you stop pressing, digit by digit,
**zero being one long flash**. Press to skip it.

```
 90.0 MHz  ->  9  |  LONG  |  LONG
104.3 MHz  ->  1  |  LONG  |  4  |  3
```

On a **Pico W the LED is GP16** (wire an LED + 330Ω to GND). GPIO25 is not an LED
on that board. On a plain Pico it's the built-in LED.

No LED? Just count taps: boot is 90.0, each tap is +100 kHz.

## Frequencies

All 206 channels from 87.5 to 108.0 MHz work. Deviation is set per channel from
how much room the PLL has — more deviation means louder.

**Most channels get the full 75 kHz.** These 35 get less:

| Deviation | Sounds | Channels |
|---|---|---|
| **7.5 kHz** | very quiet | 88.5, 90.0, 91.5, 93.0, 94.5, 96.0, 97.5, 99.0, 100.0, 100.8, 101.6, 102.4, 103.2, 104.0, 104.8, 105.6, 107.0, 108.0 |
| **42.5 kHz** | slightly quiet | 87.7, 89.2, 90.7, 92.2, 93.7, 95.2, 96.7, 98.2, 99.7 |
| **57.5 kHz** | barely quieter | 87.8, 89.3, 90.8, 92.3, 93.8, 95.3, 96.8, 98.3 |
| **75 kHz** | full volume | everything else (171 channels) |

**90.0 MHz is one of the worst.** It's only the boot frequency because it's what
this firmware originally used. Step off it.

Good picks, all at full 75 kHz: **88.1, 92.1, 94.9, 98.1, 101.1, 104.3, 107.3**.

## Audio

Mono, 12 kHz bandwidth. Your volume slider feeds the modulation.

50 µs pre-emphasis is applied, so it won't sound muffled. Bright material can
clip — turn the volume down if it sounds harsh.

## Build

```sh
./build.sh            # both boards
./build.sh pico_w     # just Pico W
```

Needs `PICO_SDK_PATH` (defaults to `~/pico-sdk`) and arm-none-eabi in
`~/.local/pico-toolchain`.

Sources are kaduhi's `pico-extras` and `pico-playground` forks — the
`usb_sound_card_fm_transmitter` app, which is what the original
`../rp2040radio.uf2` was built from.

## Antenna warning

Bare pin, it's just a clock signal. Attach a wire and it's a transmitter carrying
audio — that's broadcasting, and it's a square wave with strong harmonics.
Use a band you're licensed for and add a low-pass filter.

## Not tested on hardware

Built and verified on the host, but never flashed to a real Pico by me.
