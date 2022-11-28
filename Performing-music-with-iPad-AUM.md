# Performing music with an iPad and AUM

## Audio ecosystem on iPhone and iPad

iOS devices such as the iPhone and especially an iPad are the perfect music tools: powerful and at the same time very portable and there are many different music apps are available.

Making music on an iPad works really well as many apps support at least one of the various standards for interchanging audio between applications:

* [AudioBus](https://audiob.us) was the first app for exchanging audio between apps.
* [Inter-App Audio](https://en.wikipedia.org/wiki/Inter-App_Audio) (IAA) is technology developed by Apple which routes audio and MIDI signals between applications similar to AudioBus. It was introduced with iOS 7 and is [deprecated since iOS 13](https://cdm.link/2019/06/iaa-audiobus-ios-13/). 
* [Audio Unit version 3](https://developer.apple.com/documentation/audiotoolbox/audio_unit_v3_plug-ins) (AUv3) is a newer standard for audio plugins defined by Apple and is currently mainly used by iOS/iPadOS apps (but it is also coming to the Mac). Apps are run as plugins in a so-called host application such as [Cubasis](https://www.steinberg.net/de/cubasis/), [Loopy Pro](https://loopypro.com/), or [AUM](http://kymatica.com/apps/aum).

See [this article](https://futuresonic.io/discussion/audiobus-iaa-and-auv3-whats-the-difference/) for an explanation of the pros and cons of the various protocols and standards.

## AUM (Audio Mixer)

[AUM](http://kymatica.com/apps/aum) is a flexible audio mixer, plugin host, recorder, and connection hub developed by [Kymatica](http://kymatica.com/). It has multi-channel interface support, MIDI routing, busses, fx sends, built-in EQs, and much more.

AUM hosts MIDI and audio plugins using either AUv3, IAA, or Audiobus.

## Connecting to the Teenage Engineering Choir using AUM

In order to connect to the choir, AUM needs to advertise the device (iPad or iPhone) as BLE peripheral.

Once AUM says that the device is visible, connecting to the choir is simple: when the choir is active, simply press and hold the single button on any doll until it sings (literally!) "Searching for controller". This will now connect with any BLE Midi device in advertising mode. Use `000000` (6 times zero) when being prompted for a PIN. Once connected it will again sing "Connected to the controller".

This works as with the OP-1 Field [as shown in a video](https://www.youtube.com/watch?v=3Pcp0au-IBY).

<a href="images/discovery-pairing.png"><image src="images/discovery-pairing.png" width="400"></a>

All relevant MIDI messages are routed to the `CH-8` (Choir) BLE Midi device:

<a href="images/midi-routing.png"><image src="images/midi-routing.png" width="400"></a>

## Conducting the choir

Conducting the choir requires a source of MIDI notes, such as a connected MIDI keyboard, the software keyboard built-in AUM or a sequencer such as Victor Porof's [Atom Piano Roll 2](https://audioveek.com/).

Additionally, a software keyboard such as Kai Arras' [KB-1](https://numericalaudio.com/kb1/) can be used to send both regular MIDI notes (for the pitch) as well as control messages (CC) using a special layout.

### Example setup with AUM, MidiSpy

In this example, there are two instances each of [KB-1](https://numericalaudio.com/kb1/) for a keyboard and controller as well as [MIDISpy](https://apps.apple.com/de/app/midispy/id1444652196) for showing the outgoing messages.

<a href="images/aum-with-plugins.png"><image src="images/aum-with-plugins.png" width="400"></a>

[Atom Piano Roll 2](https://audioveek.com/) is used for actually playing a song with the choir:

<a href="images/atom-pianoroll.png"><image src="images/atom-pianoroll.png" width="400"></a>



### 