# Choirama
Findings about [Teenage Engineering Choir](https://teenage.engineering/products/choir) (CH-8)

## What is it

The Choir celebrates the beginning of Teenage Engineering as a company by re-creating their very first project, the Absolut Vodka Choir, an art installation, as a new product. 

8 unique, wooden dolls sing together multi-harmonic pieces. But it can also be used to play songs via Bluetooth LE Midi, [as shown in a video](https://www.youtube.com/watch?v=3Pcp0au-IBY).

As the [user guide](https://teenage.engineering/guides/choir) is not very detailed, this repository serves as a place to collect additional information and hints.

Also, the choir dolls are interesting targets for tinkering, so there is also some technical information that may serve as a starting point for enterprising musicians and hackers.

## Dolls

The choir consists of 8 different dolls:

| Doll  | origin | product serial prefix [1] |  voice type | musical range | frequency range | sound module orientation | comments  |
|---|---|---|---|---|---|---|---|
| **Hatsheput**  | Egypt       | `S7DPQ___` | mezzo soprano | A3 – F5  | 220 – 698,46hz     | horizontal | Spelled Hatshepsut on TE website |
| **Leila**      | Palestine   | `S4DPQ___` | soprano       | C#4 – A5 | 277,18 – 880hz     | horizontal |   |
| **Olga**       | Russia      | `S8DPQ___` | contralto     | E3 – C5  | 161,82 – 523,25hz  | vertical   |   |
| **Bogdan**     | Cossack     | `S6DPQ___` | bass          | E2 – C4  | 82 – 261,60hz      | vertical   |   |
| **Carlo**      | Italy       | `S2DPQ___` | baritone      | G2 – D#4 | 98 – 293,66hz      | horizontal |   |
| **Ivana**      | Netherlands | `S5DPQ___` | alto          | F#3 – D5 | 174,61 – 587,339hz | vertical   |   |
| **Miki**       | Japan       | `S1DPQ___` | tenor         | B2 – G4  | 116,54 – 391,99hz  | horizontal |   |
| **Giesela**    | Germany     | `S3DPQ___` | mezzo soprano | B3 – G5  | 246,94 – 783,99hz  | horizontal | Spelled Gisela on TE website |

Notes:

[1] the product serial is contained in the `version.txt` file.


## Songs

See [here](https://teenage.engineering/guides/choir) for a list of out-of-the-box songs.

## Playing via MIDI

### Connecting via MIDI

Connecting to the choir is simple: when the choir is active, simply press and hold the single button on any doll until it sings (literally!) "Searching for controller". This will now connect with any BLE Midi device in advertising mode. Once connected it will again sing "Connected to the controller".

This works with the OP-1 Field [as shown in a video](https://www.youtube.com/watch?v=3Pcp0au-IBY), but also with other devices like an iPad, MacBook or any other MIDI BLE capable device.

Important note:

* the device needs to advertise itself as MIDI peripheral
* when the choir tries to connect, a PIN is requested. Simply use `000000` (6 times zero) as PIN

With a MIDI device connected, you can now play harmonies. All notes are distributed between the members of the choir (also works for "incomplete" ensembles, e.g. just 4 or even just a single doll).

[Performing music with the choir using an iPad with AUM](Performing-music-with-iPad-AUM.md) is described in a separate tutorial.

### Voicing and Pitch

The pitch is controlled by the actual MIDI note, e.g. 48 for a C3. The voicing can be influenced by sending CC commands **before** playing the note (it will not change while the not is playing!)

The following CC controllers are available:

* **CC1 vibrato**: this is mostly noticeable for longer notes
* **CC2 consonant**: the CC value will select the consonant of the syllable being sung by the doll. See the [mapping table](ConsonantMappings.md) of CC value to consonant.
* **CC3 vowel**: the CC value will select the vowel of the syllable being sung by the doll. See the [mapping table](VowelMappings.md) of CC value to vowel.
* **CC4 reverb**: the CC value affects the reverb duration. Again, this is mostly noticeable for longer notes

### Velocity

_tbd_

### Manual control

#### Volume

Tilt any doll left for lower volume and right for higher volume. Keep tilted until the desired volume is reached or do it in steps. The doll will sing some la-las to show the current volume.

#### Next song

_tbd_ (possible?)

#### Turning the Choir on or off

Tapping a doll on the head will wake it (it will sing a note, others will chime in, then will start into a random song).

Tapping a doll on the head again will turn it off.

## Hints & Troubleshooting

* sometimes the choir reverts back to singing, even when actually being connected to a MIDI device. This might be a bug. Currently, it helps to shut them off (tap a doll on the head), reactivate it (another tap), an reconnect MIDI (see above)
* sometimes the members of the choir get out of sync. They might catch up and re-sync after a few seconds, but it might also be required to stop and restart playing
* Changed voicings will only be used for the next note. When playing with different by issuing CC controls, it helps to have some kind of repeat mode for playing notes (or a sequence in a loop) so the CC values can be adjusted and will instantly be played by the next playout of a note.


## Technical Details

Each doll contains a little sound module with an embedded computer and a battery. The battery can be chared via USB-C.

When plugging the sound module into a computer (or phone or tablet) using USB-C, it will mount a small **USB drive** which contains a version.txt file with doll type and serial number as well as a readme file with instructions **for firmware updates**.

The latest firmware version is 1.1.3 (released 2022/12/22) available [here](https://teenage.engineering/downloads/choir).

Additionally, the sound module will register as a **CDC-ACM device** which might be used to **transfer data between host computer and sound module**. Connecting to this device with a terminal program (`minicom`) didn't provide any feedback, though, so the communication protocol is currently unknown.

<a href="images/linux-dmesg.jpg"><image src="images/linux-dmesg.jpg" width="300"></a>

### Tear down of a sound module

The sound module can easily be opened and peeked inside by unscrewing the lid over the speaker.

<a href="images/unscrewed-lid.jpg"><image src="images/unscrewed-lid.jpg" width="100"></a>
<a href="images/view-inside.jpg"><image src="images/view-inside.jpg" width="100"></a>
<a href="images/speaker.jpg"><image src="images/speaker.jpg" width="100"></a>
<a href="images/disassembled.jpg"><image src="images/disassembled.jpg" width="100"></a>
<a href="images/spacer.jpg"><image src="images/spacer.jpg" width="100"></a>

After moving the speaker aside (beware of the cable, it's connected to the rest of the system!) the rest of the system becomes visible: a tiny circuit board and a battery.

<a href="images/pcb-upper.jpg"><image src="images/pcb-upper.jpg" width="100"></a>
<a href="images/pcb-spacer.jpg"><image src="images/pcb-spacer.jpg" width="100"></a>
<a href="images/pcb-bottom.jpg"><image src="images/pcb-bottom.jpg" width="100"></a>
<a href="images/pcb-closeup.jpg"><image src="images/pcb-closeup.jpg" width="100"></a>
<a href="images/pcb-closeup2.jpg"><image src="images/pcb-closeup2.jpg" width="100"></a>

Depending on the doll the sound module is placed either horizontally or vertically. The vertical ones have an additional beige ring on top.

<a href="images/moduletypes.jpg"><image src="images/moduletypes.jpg" width="100"></a>
<a href="images/moduletypes2.jpg"><image src="images/moduletypes2.jpg" width="100"></a>


### Circuit Board details

* Nordic Semiconductor [N52840 SoC](https://www.nordicsemi.com/products/nrf52840) including an ARM Cortex M4 CPU ([spec sheet](https://nsscprodmedia.blob.core.windows.net/prod/software-and-other-downloads/product-briefs/nrf52840-soc-v3.0.pdf)), used variant: `N52840-Q1AAD0-2025KR`

  From the [docs](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fstruct_nrf52%2Fstruct%2Fnrf52840.html):

  > nRF52840 is an ultra-low power 2.4 GHz wireless system on chip (SoC) integrating a multiprotocol 2.4 GHz transceiver, an Arm® Cortex®-M4F CPU and flash program memory. It is the ultimate SoC for any short range wireless personal area network or IPv6-enabled automation application.
  > 
  > nRF52840 supports Bluetooth® Low Energy, the ANT™, 802.15.4, and user-proprietary 2.4 GHz protocols. Fully qualified Bluetooth Low Energy and ANT stacks are available for the SoC. An 802.15.4 protocol library and a certified USB stack are also available.
  > 
  > The nRF52840 SoC is part of the nRF52 Series that offers code-compatible devices across the series and simple software migration from the nRF51 Series.
  
* Amplifier: Maxim Integrated [MAX98390](https://datasheets.maximintegrated.com/en/ds/MAX98390.pdf)

  Boosted Class-D Amplifier with Integrated Dynamic Speaker Management

* Power management: Texas Instruments [TI BQ25892](https://www.ti.com/product/BQ25892)

* LiPo battery 3.7V, 850mAh, 3.145Wh

  <a href="images/battery.jpg"><image src="images/battery.jpg" width="100"></a>

* `C4066` analog switch (?)

* `6W808 2025QBR` (function?)

The upper side of the board (visible after unscrewing the lid and removing the speaker) contains a couple of circular points, likely for testing and/or programming the sound module.

Also, there is a small sticker with a QR code and the serial number which corresponds to the one from the `version.txt` file.

Communication between the choir members and to a MIDI controller works via Bluetooth Low Energy (BLE), advertisements via BLE beacons.

### Operating System

The operating system is currently unknown, but likely some Linux variant.

### Contents of USB disk

#### File version.txt

    (c) teenage engineering 2021
    
    firmware upgrade instructions:
    
    1. find the latest firmware for your product at https://teenage.engineering/
    2. drop the file here, in the root directory of the virtual usb drive
    3. eject the usb drive safely
        * follow the instructions for your operating system
        * this step takes about 10 seconds
    4. disconnect usb cable
    5. press and hold the button on the device for 2 seconds
        * the led will turn on
    6. wait for the upgrade to complete
        * this step can take a minute
        * the led will turn off at the end

#### File readme.txt

    product: CH-8
    variant: Hatsheput
    software version: 1.0.78+0
    product serial: S7DPQXXX
    pcba serial: 8300006832XXXXXXXX
    upgrade status: ok
