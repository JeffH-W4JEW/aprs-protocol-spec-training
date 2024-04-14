# AUTHORS’ FOREWORD

This reference document describes what is known as _APRS Protocol Version 1.0_, and is essentially a description of how APRS operates today.

It is intended primarily for the programmer who wishes to develop APRS- compliant applications, but will also be of interest to the ordinary user who wants to know more about what goes on “under the hood”.

It is not intended, however, to be a dry-as-dust, pedantic, RFC-style programming specification, to be read and understood only by the Mr Spocks of this world. We have included many items of general information which, although strictly not part of the formal protocol description, provide a useful background on how APRS is actually used on the air, and how it is implemented in APRS software. We hope this will put APRS into perspective, will make the document more readable, and will not offend the purists too much.

It is important to realize how APRS originated, and to understand the design philosophy behind it. In particular, we feel strongly that APRS is, and should remain, a light-weight tactical system — almost anyone should be able to use it in temporary situations (such as emergencies or mobile work or weather watching) with the minimum of training and equipment.

This document is the result of inputs from many people, and collated and massaged by the APRS Working Group. Our sincere thanks go to everyone who has contributed in putting it together and getting it onto the street. If you discover any errors or omissions or misleading statements, please let us know.

The best way to do this is via the TAPR _aprsspec_ mailing list at [www.tapr.org.](http://www.tapr.org/)

> **FIXME**

Finally, users throughout the world are continually coming up with new ideas and suggestions for extending and improving APRS. We welcome them.

Again, the best way to discuss these is via the _aprsspec_ list.

The APRS Working Group August 2000

**Disclaimer** Like any navigation system, APRS is not infallible. No one should rely blindly on APRS for navigation, or in life-and-death situations. Similarly, this specification is not infallible.

The members of the APRS Working Group have done their best to define the APRS protocol, but this protocol description may contain errors, or there may be omissions. It is very likely that not all APRS implementations will fully or correctly implement this specification, either today or in the future.

We urge anyone using or writing a program that implements this specification to exercise caution and good judgement. The APRS Working Group and the specification’s Editor disclaim all liability for injury to persons or property that may result from the use of this specification or software implementing it.

# THE STRUCTURE OF THIS SPECIFICATION

This specification describes the overall requirements for developing software that complies with APRS Protocol Version 1.0. The information flow starts with the standard AX.25 UI-frame, and progresses downwards into more and more detail as the use of each field in the frame is explored.

A key feature of the specification is the inclusion of dozens of detailed examples of typical APRS packets and related math computations.

Here is an outline of the chapters:

**Introduction to APRS** — A brief background to APRS and a summary of its main features.

**The APRS Design Philosophy** — The fundamentals of APRS, highlighting its use as a real-time tactical communications tool, the timing of APRS transmissions and the use of generic digipeating.

**APRS and AX.25** — A brief refresher on the structure of the AX.25 UI-frame, with particular reference to the special ways in which APRS uses the Destination and Source Address fields and the Information field.

**APRS Data in the AX.25 Destination and Source Address Fields** — Details of generic APRS callsigns and callsigns that specify display symbols and APRS software version numbers. Also a summary of how Mic-E encoded data is stored in the Destination Address field, and how the Source Address SSID can specify a display icon.

**APRS Data in the AX.25 Information Field** — Details of the principal constituents of APRS data that are stored in the Information field. Contains the APRS Data Type Identifiers table, and a summary of all the different types of data that the Information field can hold.

**Time and Position Formats** — Information on formats for timestamps, latitude, longitude, position ambiguity, Maidenhead locators, NMEA data and altitude.

**APRS Data Extensions** — Details of optional data extensions for station course/speed, wind speed/direction, power/height/gain, pre-calculated radio range, DF signal strength and Area Object descriptor.

**Position and DF Report Data Formats** — Full details of these report formats.

**Compressed Position Report Data Formats** — Full details of how station position and APRS data extensions are compressed into very short packets.

**Mic-E Data Format** —Mic-E encoding of station lat/long position, altitude, course, speed, Mic-E position comment, telemetry data and APRS digipeater path into the AX.25 Destination Address and Information fields.

**Object and Item Reports** — Full information on how to set up APRS Objects and Items, and details of the encoding of Area Objects (circles, lines, ellipses etc).

**Weather Reports** — Full format details for weather reports from stand- alone (positionless) weather stations and for reports containing position information. Also details of storm data format.

**Telemetry Data** — A description of the MIM/KPC-3+ telemetry data format, with supporting information on how to tailor the interpretation of the raw data to individual circumstances.

**Telemetry Metadata** — A description of how to interpret the raw numeric telemetry data. This includes properties such as name, units, and scaling.

**Messages, Bulletins and Announcements** — Full format information.

**Station Capabilities, Queries and Responses** — Details of the ten different types of query and expected responses.

**Status Reports** — The format of general status messages, plus the special cases of using a status report to contain meteor scatter beam heading/power and Maidenhead locator.

**Network Tunneling** — The use of the Source Path Header to allow tunneling of APRS packets through third-party networks that do not understand AX.25 addresses, and the use of the third-party Data Type Identifier.

**User-Defined Data Format** — APRS allows users to define their own data formats for special purposes. This chapter describes how to do this.

**Other Packets** — A general statement on how APRS is to handle any other packet types that are not covered by this specification.

**APRS Symbols** —How to specify APRS symbols and symbol overlays, in position reports and in generic GPS destination callsigns.

**APRS Data Formats** — An appendix containing all the APRS data formats collected together for easy reference.

**The APRS Symbol Tables** —A complete listing of all the symbols in the Primary and Alternate Symbol Tables.

**ASCII Code Table** — The full ASCII code, including decimal and hex codes for each character (the decimal code is needed for compressed lat/long and altitude computations), together with the hex codes for bit-shifted ASCII characters in AX.25 addresses (useful for Mic-E decoding and general on-air packet monitoring).

**Glossary** — A handy one-stop reference for the many APRS-specific terms used in this specification.

**References** — Pointers to other documents that are relevant to this specification.

# INTRODUCTION TO APRS

**What is APRS?** APRS is short for _Automatic Packet Reporting System_, which was designed by Bob Bruninga, WB4APR, and introduced by him at the 1992 TAPR/ ARRL Digital Communications Conference.

Fundamentally, APRS is a packet communications protocol for disseminating live data to everyone on a network in real time. Its most visual feature is the combination of packet radio with the Global Positioning System (GPS) satellite network, enabling radio amateurs to automatically display the positions of radio stations and other objects on maps on a PC. Other features not directly related to position reporting are supported, such as weather station reporting, direction finding and messaging.

APRS is different from regular packet in several ways:

- It provides maps and other data displays, for vehicle/personnel location and weather reporting in real time.

- It performs all communications using a one-to-many protocol, so that everyone is updated immediately.

- It uses generic digipeating, with well-known callsign aliases, so that prior knowledge of network topology is not required.

- It supports intelligent digipeating, with callsign substitution to reduce network flooding.

- Using AX.25 UI-frames, it supports two-way messaging and distribution of bulletins and announcements, leading to fast dissemination of text information.

- It supports communications with the Kenwood TH-D7/D72/D74 and TM-D700/D710 radios, which have built-in TNC and APRS firmware.

Conventional packet radio is really only useful for passing bulk message traffic from point to point, and has traditionally been difficult to apply to real-time events where information has a very short lifetime. APRS turns packet radio into a real-time tactical communications and display system for emergencies and public service applications.

APRS provides universal connectivity to all stations, but avoids the complexity, time delays and limitations of a connected network. It permits any number of stations to exchange data just like voice users would on a voice net. Any station that has information to contribute simply sends it, and all stations receive it and log it.

APRS recognizes that one of the greatest real-time needs at any special event or emergency is the tracking of key assets. Where is the marathon leader?

Where are the emergency vehicles? What’s the weather at various points in the county? Where are the power lines down? Where is the head of the parade? Where is the mobile ATV camera? Where is the storm?

To address these questions, APRS provides a fully featured automatic vehicle location and status reporting system. It can be used over any two-way radio system including amateur radio, marine band, and cellular phone. There is even an international live APRS tracking network on the Internet.

## APRS

## Features

APRS applications are available for most platforms, including DOS, Windows, MacOS, Linux, and Android. Most implementations on these platforms support the main features of APRS:

- **Maps** — APRS station positions can be plotted in real-time on maps, with coverage from a few hundred yards to worldwide. Stations reporting a course and speed are dead-reckoned to their present position. Overlay databases of the locations of APRS digipeaters, US National Weather Service sites and even amateur radio stores are available. It is possible to zoom in to any point on the globe.

- **Weather Station Reporting** — APRS supports the automatic display of remote weather station information on the screen.

- **DX Cluster Reporting** — APRS an ideal tool for the DX cluster user. Small numbers of APRS stations connected to DX clusters can relay DX station information to many other stations in the local area, reducing overall packet load on the clusters.

- **Internet Access** — The Internet can be used transparently to cross-link local radio nets anywhere on the globe. It is possible to telnet into Internet APRS servers and see hundreds of stations from all over the world live. Everyone connected can feed their locally heard packets into the APRS server system and everyone everywhere can see them.

- **Messages** — Messages are two-way messages with acknowledgement. All incoming messages alert the user on arrival and are held on the message screen until killed.

- **Bulletins and Announcements** —Bulletins and announcements are addressed to everyone. Bulletins are sent a few times an hour for a few hours, and announcements less frequently but possibly over a few days.

- **Fixed Station Tracking** — In addition to automatically tracking mobile GPS/LORAN-equipped stations, APRS also tracks from manual reports or grid squares.

- **Objects** — Any user can place an APRS Object on his own map, and within seconds that object appears on all other station displays. This is particularly useful for tracking assets or people that are not equipped with trackers. Only one packet operator needs to know where things are (e.g. by monitoring voice traffic), and as he maintains the positions and movements of assets on his screen, all other stations running APRS will display the same information.

# THE APRS DESIGN PHILOSOPHY

**Net Cycle Time** It is important to note that APRS is primarily a _real-time, tactical_ communications tool, to help the flow of information for things like special events, emergencies, Skywarn, the Emergency Operations Center and just plain in-the-field use under stress. But like the real world, for 99% of the time it is operating routinely, waiting for the unlikely serious event to happen.

Anything which is done to enhance APRS must not undermine its ability to operate in local areas under stress. Here are the details of that philosophy:

1. APRS uses the concept of a “net cycle time”. This is the time within which a user should be able to hear (at least once) all APRS stations within range, to obtain a more or less complete picture of APRS activity. The net cycle time will vary according to local conditions and with the number of digipeaters through which APRS data travels.

2. The objective is to have a net cycle time of 10 minutes for local use. This means that within 10 minutes of arrival on the scene, it is possible to captured the entire tactical picture.

3. All stations, even fixed stations, should beacon their position at the net cycle time rate. In a stress situation, stations are coming and going all the time. The position reports show not only where stations are without asking, but also that they are still active.

4. It is not reasonable to assume that all APRS users responding to a stress event understand the ramifications of APRS and the statistics of the channel — user settings cannot be relied on to avoid killing a stressed net. Thus, to try to anticipate when the channel is under stress, APRS automatically adjusts its net cycle time according to the number of digipeaters in the UNPROTO path:

- Direct operation (no digipeaters): 10 minutes (probably an event).

- Via one digipeater hop: 10 minutes (probably an event).

- Via two digipeater hops: 20 minutes.

- Via three or more digipeater hops: 30 minutes.

5. Since almost all home stations set their paths to three or more digipeaters, the default net cycle time for routine daily operation is 30 minutes. This should be a universal standard that everyone can bank on — if you routinely turn on your radio and APRS and do nothing else, then in 30 minutes you should have virtually the total picture of all APRS stations within range.

6. Since knowing where the digipeaters are located is fundamental to APRS connectivity, digipeaters should use multiple beacon commands to transmit position reports at different rates over different paths; i.e. every 10 minutes for sending position reports locally, and every 30 minutes for sending them via three digipeaters (plus others rates and distances as needed).

7. If the net cycle time is too long, users will be tempted to send queries for APRS stations. This will increase the traffic on the channel unnecessarily. Thus the recommended extremes for net cycle time are 10 and 30 minutes — this gives network designers the fundamental assumptions for channel loading necessary for good engineering design.

Merge this in: [**Digipeater ID rates**](http://www.aprs.org/digis/digi-rates.txt) for optimum APRS
networks add to net cycle time

**Packet Timing** Since APRS packets are error-free, but are not guaranteed delivery, APRS transmits information redundantly. To assure rapid delivery of new or changing data, and to preserve channel capacity by reducing interference from old data, APRS should transmit new information more frequently than old information.

There are several algorithms in use to achieve this:

- **Decay Algorithm** — Transmit a new packet once and n seconds later. Double the value of n for each new transmission. When n reaches the net cycle time, continue at that rate. Other factors besides “doubling” may be appropriate, such as for new message lines.

- **Fixed Rate** — Transmit a new packet once and n seconds later. Transmit it x times and stop.

- **Message-on-Heard** — Transmit a _new_ packet according to either algorithm above. If the packet is still valid, and has not been acknowledged, and the net cycle time has been reached, then the recipient is probably not available. However, if a packet is then subsequently heard from the recipient, try once again to transmit the packet.

- **Time-Out** — This term is used to describe a time period beyond which it is reasonable to assume that a station no longer exists or is off the air if no packets have been heard from it. A period of 80 minutes is suggested as the nominal default timeout to account for stations via satellites. This time-out is not used in any transmitting algorithms, but is useful in some programs to decide when to cease displaying stations as “active”. Note that on HF, signals come and go, so decisions about activity may need to be more flexible.

**Generic Digipeating** The power of APRS in the field derives from the use of _generic_ digipeating, in that packets are propagated without a prior knowledge of the network.

Do not use RELAY, TRACE, or plain WIDE, as seen in outdated literature, because they have been obsolete since around 2004.

1. **WIDEn-N** — Rather than using specific digipeater names, the normal procedure is to use generic aliases. A typical digipeater via path is “WIDE1-1,WIDE2-1”. Typically, low range “fill-in” digipeaters will respond to only “WIDE1-\*”. Those with very wide area coverage will respond to “WIDE2-\*”.

In this context, the SSID is actually a remaining use count. For example, “WIDE2-2” is equivalent to “WIDE2-1,WIDE2-1”.

Digipeaters look at the first unused address in the via path of a packet. If their configuration contains the prefix pattern, the processing depends on the value of the SSID.

N = 0 Do not digipeat.

N = 1 Digipeat and replace generic form with digipeater callsign.

Digipeaters should always identify themselves, and mark their address used, so the actual path taken is known.

N ≥ 2 Decrement N and leave address in path. Leave it marked as unused. Insert digipeater callsign before it and mark as Used.

The used part of the via path (before the “\*” character in the human-readable monitoring format) reveals the path the packet has taken.

All digipeaters retain a copy of each packet retransmitted for a short amount of time, typically about 28 seconds. An identical packet, ignoring the via path, will not be transmitted again during this time. For efficiency, many implementations will keep a checksum, rather than the entire packet.

1. **SSn-N** — Digipeaters can also be configured to respond to other prefixes representing geographical regions or special events. In the U.S., regions might correspond to a state, county or ARRL section. In Europe, the regions are based on a combination of two iso-standards. ISO 3166-1 country code und ISO 3166-2 subdivision code.

2. **GATE** — This generic callsign is used by HF-to-VHF Gateway digipeaters. Any packet heard on HF via GATE will be digipeated locally on VHF. This permits local networks to keep an eye on the national and international picture.

<table>
<colgroup>
<col style="width: 6%" />
<col style="width: 93%" />
</colgroup>
<thead>
<tr class="header">
<th>A</th>
<th>An Alt. Freq. input digi (Typically on 144.99 MHz)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>R</td>
<td>RELAY only (Obsolete in 2005.)</td>
</tr>
<tr class="even">
<td>W</td>
<td>WIDE and RELAY (Obsolete in 2005.)</td>
</tr>
<tr class="odd">
<td>T</td>
<td>PacComm RELAY,WIDE and TRACE (Obsolete in 2005.)</td>
</tr>
<tr class="even">
<td>N</td>
<td>WIDEn-N and NOID is set (Obsolete in 2005 - all digipeaters should
identify themselves.)</td>
</tr>
<tr class="odd">
<td>I</td>
<td>Digipeater is also an IGate.</td>
</tr>
<tr class="even">
<td><strong>S</strong></td>
<td><strong>The New n-N Paradigm digi with State SSn-N support (and ID
is on)</strong></td>
</tr>
<tr class="odd">
<td>P</td>
<td>PacComm digi supporting New n-N Paradigm</td>
</tr>
<tr class="even">
<td>U</td>
<td>UI-DIGI firmware (should now be upgraded to S)</td>
</tr>
<tr class="odd">
<td>D</td>
<td>DIGI_NED (should now be upgraded to S)</td>
</tr>
</tbody>
</table>

## Communicating

## Map Views Unambiguously

APRS is a tactical geographical system. To maximize its operational effectiveness and minimize confusion between operators of different systems, users need to have an unambiguous way to communicate to others the “location” and “size” (or area of coverage) of any map view.

The APRS convention is by reference to a _center_ and _range_ which specify the geographical center and approximate radius of a circle that will fit in the map view independent of aspect ratio. The radius of the circle (in nautical miles, statute miles or km) is known as the “[range scale](http://www.aprs.org/aprs11/RangeScale.txt)”. This convention gives all users a simple common basis for describing any specific map view to others over any communications medium or program.

If you are on a 64 mile "range Scale" that means that everything within 64 miles of the center is visible. If you zoom to the 8 mile range scale, then anyone else that is on an "8 mile range scale" (no matter what version of APRS he is running) he will see the same "area". It is only one line of code to convert from an arbitrary "zoom factor" or "map size" to a meaningful "range scale". And APRS would be so much the better for it...

> **Do these items flow well here?**

**APRS 1.1 SPEC Operating Conventions for the Good of the APRS Network:**

We don’t have an existing operating conventions section -- probably add
to chapter 2.

- [**APRS Voice-Alert**](http://www.aprs.org/VoiceAlert3.html) for instant voice contact with any APRS mobile

- [**Recommended DIGI paths**](http://www.aprs.org/aprs11/hacks.txt) and striving for _network protection_ ahead of too much user flexibility.

- **Auto-Answer messages** are SPAM to the network in most cases. To limit their impact, all original APRS clients adhered to these rules for auto-answer messages:

  1. Any such AA message text should begin with AA:
  2. Any such AA message should not have a Line number (so it does not
     cause acks)
  3. Any such AA message is only sent once on each incoming message
     packet (no forced retries)
  4. Any such AA message should default to OFF on power up.
  5. Any such AA message should be canceled when the client detects
     the return presence of the operator
  6. Any such AA message can be ended with a }yy REPLY-ACK if the
     software supports it

- **Where are these AA messages defined?**

# APRS AND AX.25

**Protocols** At the data link layer, APRS uses AX.25 frames, as defined in _AX.25 Link Access Protocol for Amateur Packet Radio_ (see Appendix 6 for details), utilizing Unnumbered Information (UI) frames exclusively. This means that APRS runs in _connectionless_ mode, whereby AX.25 frames are transmitted without expecting any response, and reception at the other end is not guaranteed.

At a higher level, APRS supports a messaging protocol that allows users to send short messages (one line of text) to nominated stations, and expects to receive acknowledgements from those stations.

This specification does not mandate a particular type of modem for the physical layer. In practice it is most often 1200 bps AFSK with tones of 1200 and 2200 Hz.

**The AX.25 Frame** All APRS transmissions use AX.25 UI-frames, with 9 fields of data:

- **Flag** — The flag field at each end of the frame is the bit sequence 0x7e that separates each frame. A single flag can be both the end of one frame and the beginning of the next.

- **Destination Address** — Unlike Packet Radio, this is not used for the destination for the packet. This field is used in multiple ways for the different APRS data types. APRS data is encoded to ensure that the field conforms to the standard AX.25 callsign format (i.e. up to 6 upper case alphanumeric characters plus SSID). Most often it will be of the form AP*xxxx* to identify the type of system that generated the packet. If the SSID is non-zero, it specifies a generic APRS digipeater path.

> **Did I see somewhere that is obsolete?**

In accordance with the AX.25 specification, for UI frames, the C bit of the Destination SSID octet should be 1. Receiving applications must ignore it because this has not been done consistently.

**Source Address** — This field contains the callsign and SSID of the transmitting station. In some cases, if the SSID is non-zero, the SSID may specify an APRS display Symbol Code.

In accordance with the AX.25 specification, for UI frames, the C bit of the Source SSID octet should be 0. Receiving applications must ignore it because this has not been done consistently.

- **Digipeater Addresses** — From zero to 8 digipeater callsigns may be included in this field. These can be actual station names but are are usually aliases so you don’t need to know about the network topology. **Note**: These digipeater addresses may be overridden by a generic APRS digipeater path (specified in the Destination Address SSID).

> **Did I read somewhere that was obsolete? I bet it is not widely implemented.**

- **Control Field** — This field is set to 0x03 (UI-frame).

- **Protocol ID** — This field is set to 0xf0 (no layer 3 protocol).

- **Information Field** — This field contains more APRS data. The first character of this field is the APRS Data Type Identifier (DTI) that specifies the nature of the data that follows.

- **Frame Check Sequence** — The FCS is a sequence of 16 bits used for checking the integrity of a received frame.

# APRS DATA IN THE AX.25 DESTINATION AND SOURCE ADDRESS FIELDS

## The AX.25

## Destination Address Field

> The AX.25 Destination Address field can contain 6 different types of
> APRS information:

- A generic APRS address.

- A generic APRS address with a symbol.

- An APRS software version number.

- Mic-E encoded data.

- A Maidenhead Grid Locator (obsolete).

- An Alternate Net (ALTNET) address.

> In all of these cases, the Destination Address SSID may specify a
> generic APRS digipeater path.

## Generic APRS Destination Addresses

> IS most/all of this THIS OBSOLETE ?? I’ve never seen the vast majority
> and it seems stupid to use BEACON rather than the device type. I think
> we should encourage the system type identifier, APxxxx.
>
> APRS uses the following generic beacon-style destination addresses:
>
> The asterisk is a wildcard, allowing the address to be extended (up to
> a total of 6 alphanumeric characters). Thus, for example, WX1, WX12
> and WX12CD are all valid APRS destination addresses.
>
> <s>† The **AIR**\* and **ZIP**\* addresses are being phased out, but
> are needed at present for backward compatibility.(this was in 2000)</s>
>
> All of these addresses have an SSID of –0. Non-zero SSIDs are reserved
> for generic APRS digipeating.
>
> These addresses are copied by everyone. All APRS software must accept
> packets with these destination addresses.
>
> <s>The address **GPS** (i.e. the 3-letter address **GPS**, not
> **GPS\***) is specifically intended for use by trackers sending
> lat/long positions via digipeaters which have the capability of
> converting positions to compressed data format.</s>
>
> The addresses **DGPS** and **RTCM** are used by differential GPS
> correction stations. Most software will not make use of packets using
> this address, other than to pass them on to an attached GPS unit.
>
> The address **SKY** is used for Skywarn stations.
>
> Packets addressed to **SPCL** are intended for special events. APRS
> software can display such packets to the exclusion of all others, to
> minimize clutter on
>
> the screen from other stations not involved in the special event. The
> addresses **TEL** and **TLM** is used for telemetry stations.
>
> The use of **APRS**, in the destination field, is obsolete. Use an
> <span class="mark">AP</span>xxxx software version number as explained
> below.

## Generic APRS Address with

## Symbol

> APRS uses several of the above-listed generic addresses in a special
> way, to specify not only an address but also a display symbol. These
> special addresses are GPSxyz, GPSCnn, GPSEnn, SPCxyz and SYMxyz, and
> are intended for use where it is not possible to include the symbol in
> the AX.25 Information field.
>
> The GPS addresses above are for general use.
>
> The SPC addresses are intended for special events. The SYM addresses
> are reserved for future use.
>
> The characters xy and nn refer to entries in the APRS Symbol Tables.
> The character z specifies a symbol overlay. See Chapter 20: APRS
> Symbols and Appendix 2 for more information.

## APRS Software Version Number

> When not using the MIC-E format, the AX.25 Destination Address field
> will normally contain the version number of the APRS software that is
> generating the packet. Knowledge of the system type can be very useful
> when debugging.
>
> The following software version types are reserved (xx and xxx indicate
> a version number): Can we remove dead products from here?
>
> **APC**xxx APRS/CE, Windows CE
>
> **APD**xxx Linux aprsd server
>
> **APE**xxx PIC-Encoder
>
> **API**xxx Icom radios (future)
>
> **APIC**xx ICQ messaging
>
> **APK**xxx Kenwood radios
>
> **APM**xxx MacAPRS
>
> **APP**xxx pocketAPRS
>
> **APR**xxx APRSdos
>
> **APS**xxx APRS+SA
>
> **APW**xxx WinAPRS
>
> **APX**xxx X-APRS
>
> **APY**xxx Yaesu radios
>
> **APZ**xxx Experimental
>
> For example, a station using version 3.2.6 of MacAPRS could use the
> version APM326.
>
> Developers of new products should obtain a unique version number to
> identify the product. The traditional list at
> <http://aprs.org/aprs11/tocalls.txt> is now longer being maintained.
> The current location is <https://github.com/aprsorg/aprs-deviceid>
>
> The APZxxx Experimental destination is designated for _temporary_ use
> only while a product is being developed, before a special APRS
> Software Version address is assigned to it.

## Mic-E Encoded

## Data

> Another alternative use of the AX.25 Destination Address field is to
> contain Mic-E encoded data. This data includes:

- The latitude of the station.

- A West/East Indicator and a Longitude Offset Indicator (used in
  longitude computations).

- A Position Comment.

- The APRS digipeater path.

> This data is used with associated data in the AX.25 Information field
> to provide a complete Position Report and other information about the
> station (see Chapter 10: Mic-E Data Format).

## Maidenhead Grid

## Locator in Destination Address

> The AX.25 Destination Address field may contain a 6-character
> Maidenhead Grid Locator. For example: **IO91SX**. This format is
> typically used by meteor scatter and satellite operators who need to
> keep packets as short as possible.
>
> This format is now obsolete.

So can we just drop it to reduce the clutter?

> **Alternate Nets** Any other destination address not included in the
> specific generic list or the other categories mentioned above may be
> used in Alternate Nets (ALTNETs) by groups of individuals for special
> purposes. Thus they can use the APRS infrastructure for a variety of
> experiments without cluttering up the maps and lists of other APRS
> stations. Only stations using the same ALTNET address should see their
> data. HUH?

## Generic APRS Digipeater Path

> The SSID in the Destination Address field of all packets is coded to
> specify the APRS digipeater path.
>
> If the Destination Address SSID is –0, the packet follows the standard
> AX.25 digipeater (“VIA”) path contained in the Digipeater Addresses
> field of the AX.25 frame.
>
> If the Destination Address SSID is non-zero, the packet follows one of
> 15 generic APRS digipeater paths.
>
> The SSID field in the Destination Address (i.e. in the 7th address
> byte) is encoded as follows:
>
> **APRS Digipeater Paths in Destination Address SSID**

## The AX.25 Source Address SSID to specify Symbols

> The AX.25 Source Address field contains the callsign and SSID of the
> originating station. If the SSID is –0, APRS does not treat it in any
> special way.
>
> If, however, the Source Address SSID is non-zero, APRS interprets it
> as a display icon. This is intended for use only with stand-alone
> trackers where there is no other method of specifying a display symbol
> or a destination address (e.g. MIM trackers or NMEA trackers).
>
> For more information, see Chapter 20: APRS Symbols.

# APRS DATA IN THE AX.25 INFORMATION FIELD

## Generic Data

## Format

Bytes:

## APRS Data Type

## Identifier

> In general, the AX.25 Information field can contain some or all of the
> following information:

- APRS Data Type Identifier

- APRS Data

- APRS Data Extension

- Comment

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 34%" />
<col style="width: 16%" />
<col style="width: 36%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><em><strong>Generic APRS Information Field</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong>Data Type ID</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>APRS Data</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>APRS Data Extension</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Comment</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Every APRS packet contains an APRS Data Type Identifier (DTI). This
> determines the format of the remainder of the data in the Information
> field, as follows:
>
> **APRS Data Type Identifiers**
>
> **Note**: The Kenwood TM-D700 radio uses the **'** DTI for _current_
> Mic-E data. The radio does not use the ‘ DTI.

## APRS Data and Data Extension

> There are 10 main types of APRS Data:

- Position

- Direction Finding

- Objects and Items

- Weather

- Telemetry

- Messages, Bulletins and Announcements

- Queries

- Responses

- Status

- Other

> Some of this data may also have an APRS Data Extension that provides
> additional information.
>
> The APRS Data and optional Data Extension follow the Data Type
> Identifier.
>
> The table on the next page shows a complete list of all the different
> possible types of APRS Data and APRS Data Extension.
>
> **Comment Field** In general, any APRS packet can contain a plain text
> comment (such as a beacon message) in the Information field,
> immediately following the APRS Data or APRS Data Extension.
>
> There is no separator between the APRS data and the comment unless
> otherwise stated.
>
> The freeform text part of the comment may contain any printable ASCII
> or UTF-8 character.
>
> Do not put any carriage return (0x0d) or line feed (0x0a) at the end.
> IGate stations will remove them, resulting in slightly different
> contents.
>
> The maximum length of the comment field depends on the report —
> details are included in the description of each report.
>
> In special cases, the Comment field can also contain further APRS
> data:

- **Altitude** in comment text (see Chapter 6: Time and Position
  Formats), or in Mic-E status text (see Chapter 10: Mic-E Data
  Format).

- **Maidenhead Locator** (grid square), in a Mic-E status text field
  (see Chapter 10: Mic-E Data Format) or in a Status Report (see
  Chapter 16: Status Reports).

- **Bearing and Number/Range/Quality** parameters (/BRG/NRQ), in DF
  reports (see Chapter 7: APRS Data Extensions).

- **Area Object Line Widths** (see Chapter 11: Object and Item
  Reports).

- **Signpost Objects** (see Chapter 11: Object and Item Reports).

- **Weather and Storm Data** (see Chapter 12: Weather Reports).

- **Beam Heading and Power**, in Status Reports (see Chapter 16:
  Status Reports).

> **Base-91 Notation** Two APRS data formats use base-91 notation:
> lat/long coordinates in compressed format (see Chapter 9) and the
> altitude in Mic-E format (see Chapter 10).
>
> Base-91 data is compressed into a short string of characters. All the
> characters are printable ASCII, with character codes in the range
> 33–124 decimal (i.e. **!** through **|**).
>
> To compute the base-91 ASCII character string for a given data value,
> the value is divided by progressively reducing powers of 91 until the
> remainder is less than 91. At each step, 33 is added to the modulus of
> the division process to obtain the corresponding ASCII character code.
>
> For example, for a data value of 12345678:

<table>
<colgroup>
<col style="width: 31%" />
<col style="width: 4%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p>12345678 / 91<sup>3</sup></p>
</blockquote></th>
<th><blockquote>
<p>=</p>
</blockquote></th>
<th><blockquote>
<p>modulus <strong>16</strong>, remainder 288542</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>288542 / 91<sup>2</sup></p>
</blockquote></td>
<td><blockquote>
<p>=</p>
</blockquote></td>
<td><blockquote>
<p>modulus <strong>34</strong>, remainder 6988</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>6988 / 91<sup>1</sup></p>
</blockquote></td>
<td><blockquote>
<p>=</p>
</blockquote></td>
<td><blockquote>
<p>modulus <strong>76</strong>, remainder <strong>72</strong></p>
</blockquote></td>
</tr>
</tbody>
</table>

> The four ASCII character codes are thus 49 (i.e. **16**+33), 67 (i.e.
> **34**+33), 109 (i.e. **76**+33) and 105 (i.e. **72**+33),
> corresponding to the ASCII string **1Cmi**.
>
> **APRS Data Units** For historical reasons there is some lack of
> consistency between units of data in APRS packets — some speeds are in
> knots, others in miles per hour; some altitudes are in feet, others in
> meters, and so on. It is emphasized that this specification describes
> the units of data as they are transmitted on-air. It is the
> responsibility of APRS applications to convert the on-air units to
> more suitable units if required.
>
> The default GPS earth datum is World Geodetic System (WGS) 1984 but
> Continental options such as OSG for the UK are OK.

# TIME AND POSITION FORMATS

> **Time Formats** APRS timestamps are expressed in three different
> ways:

- Day/Hours/Minutes format

- Hours/Minutes/Seconds format

- Month/Day/Hours/Minutes format

> In all three formats, the 24-hour clock is used.
>
> **Day/Hours/Minutes** (DHM) format is a fixed 7-character field,
> consisting of a 6-digit _day/time_ group followed by a single _time
> indicator_ character (**z** or
>
> **/**). The day/time group consists of a two-digit day-of-the-month
> (01–31) and a four-digit time in hours and minutes.
>
> Times can be expressed in _zulu_ (UTC/GMT) or _local_ time. For
> example:
>
> 092345**z** is 2345 hours _zulu_ time on the 9th day of the month.
>
> 092345**/** is 2345 hours _local_ time on the 9th day of the month.
>
> It is recommended that future APRS implementations only transmit zulu
> format on the air.
>
> **Note**: The time in Status Reports may _only_ be in zulu format.
>
> **Hours/Minutes/Seconds** (HMS) format is a fixed 7-character field,
> consisting of a 6-digit time in hours, minutes and seconds, followed
> by the **h** time-indicator character. For example:
>
> 234517**h** is 23 hours 45 minutes and 17 seconds _zulu_.
>
> **Note:** This format may _not_ be used in Status Reports.
>
> **Month/Day/Hours/Minutes** (MDHM) format is a fixed 8-character
> field, consisting of the month (01–12) and day-of-the-month (01–31),
> followed by the time in hours and minutes zulu. For example:
>
> 10092345 is 23 hours 45 minutes zulu on October 9th.
>
> This format is only used in reports from stand-alone “positionless”
> weather stations (i.e. reports that do not contain station position
> information).
>
> **Use of Timestamps** When a station transmits a report _without_ a
> [timestamp](http://www.aprs.org/aprs11/timestamps.txt), an APRS
> receiving station can make an internal record of the time it was
> received, if required. This record is the _receiving_ station’s notion
> of the time the report was created.
>
> On the other hand, when a station transmits a report _with_ a
> timestamp, that timestamp represents the _transmitting_ station’s
> notion of the time the report was created.
>
> In other words, reports sent _without_ a timestamp can be regarded as
> real-time, “current” reports (and the _receiving_ station has to
> record the time they were received), whereas reports sent _with_ a
> timestamp may or may not be real- time, and may possibly be (very)
> “old”.
>
> Four APRS Data Type Identifiers specify whether or not a report
> contains a timestamp, depending on whether the station has APRS
> messaging capability or not:

<table>
<colgroup>
<col style="width: 62%" />
<col style="width: 18%" />
<col style="width: 18%" />
</colgroup>
<thead>
<tr class="header">
<th></th>
<th><blockquote>
<p><em><strong>No APRS</strong></em></p>
<p><em><strong>Messaging</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>With APRS Messaging</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><em><strong>(Current/real-time) Report without
timestamp</strong></em></td>
<td><blockquote>
<p><strong>!</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>=</strong></p>
</blockquote></td>
</tr>
<tr class="even">
<td><em><strong>(Old/non-real-time) Report with
timestamp</strong></em></td>
<td><blockquote>
<p><strong>/</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>@</strong></p>
</blockquote></td>
</tr>
</tbody>
</table>

> Stations without APRS messaging capability are typically stand-alone
> trackers or digipeaters. Stations reporting without a timestamp are
> generally (but not necessarily) fixed stations.
>
> **Latitude Format** Latitude is expressed as a fixed 8-character
> field, in degrees and decimal minutes (to two decimal places),
> followed by the upper case letter **N** for north or **S** for south.
>
> Latitude degrees are in the range 00 to 90. Latitude minutes are
> expressed as whole minutes and hundredths of a minute, separated by a
> decimal point.
>
> For example:
>
> 4903**.**50**N** is 49 degrees 3 minutes 30 seconds north.
>
> In generic format examples, the latitude is shown as the 8-character
> string
>
> ddmm.hhN (i.e. degrees, minutes and hundredths of a minute north).
>
> **Longitude Format** Longitude is expressed as a fixed 9-character
> field, in degrees and decimal minutes (to two decimal places),
> followed by the upper case letter **E** for east or **W** for west.
>
> Longitude degrees are in the range 000 to 180. Longitude minutes are
> expressed as whole minutes and hundredths of a minute, separated by a
> decimal point.
>
> For example:
>
> 07201**.**75**W** is 72 degrees 1 minute 45 seconds west.
>
> In generic format examples, the longitude is shown as the 9-character
> string
>
> dddmm.hhW (i.e. degrees, minutes and hundredths of a minute west).

## Position Coordinates

> Position coordinates are a combination of latitude and longitude,
> separated by a display Symbol Table Identifier, and followed by a
> Symbol Code. For example:
>
> 4903**.**50N**/**07201.75W**-**
>
> The **/** character between latitude and longitude is the Symbol Table
> Identifier (in this case indicating use of the Primary Symbol Table),
> and the **–** character at the end is the Symbol Code from that table
> (in this case, indicating a “house” icon).
>
> A description of display symbols is included in Chapter 20: APRS
> Symbols. The full Symbol Table listing is in Appendix 2.
>
> **Position Ambiguity** In some instances — for example, where the
> exact position is not known — the sending station may wish to reduce
> the number of digits of precision in the latitude and longitude. In
> this case, the mm and hh digits in the latitude may be progressively
> replaced by a  (space) character as the amount of imprecision
> increases. For example:
>
> 4903.5N represents latitude to nearest 1/10th of a minute.
>
> 4903.N represents latitude to nearest minute. 490.N represents
> latitude to nearest 10 minutes. 49.N represents latitude to
> nearest degree.
>
> The level of ambiguity specified in the latitude will automatically
> apply to the longitude as well — it is permissible but not necessary
> to include any  characters in the longitude.
>
> For example, the coordinates:
>
> 4903**.**N/07201.75W-
>
> represent the position to the nearest minute. That is, the hundredths
> of minutes of latitude and longitude may take any value in the range
> 00–99.
>
> Thus the station may be located anywhere inside a bounding box having
> the following corner coordinates:
>
> North West corner: 49 deg 3.99 mins N, 72 deg 1.99 mins W
>
> North East corner: 49 deg 3.99 mins N, 72 deg 1.00 mins W
>
> South East corner: 49 deg 3.00 mins N, 72 deg 1.00 mins W
>
> South West corner: 49 deg 3.00 mins N, 72 deg 1.99 mins W

## Default Null Position

> Where a station does not have _any_ specific position information to
> transmit (for example, a Mic-E unit without a GPS receiver connected
> to it), the station must transmit a default null position in the
> location field.
>
> The null position corresponds to 0° 0' 0" north, 0° 0' 0" west.
>
> The null position should be include the **\\** symbol
> (unknown/indeterminate position). For example, a Position Report for a
> station with unknown position will contain the coordinates
> …0000.00N**\\**00000.00W**.…**

## Maidenhead

## Locator (Grid Square)

> An alternative method of expressing a station’s location is to provide
> a Maidenhead locator (grid square). There are four ways of doing this:

- In a Status Report — e.g. IO91SX/-

> (/- represents the symbol for a “house”).

- In Mic-E Status Text — e.g. IO91SX**/G**

> (**/G** indicates a “grid square”).

- In the Destination Address — e.g. IO91SX. (obsolete).

- In AX.25 beacon text, with the **\[** APRS Data Type Identifier —
  e.g.

> **\[**IO91SX\] (obsolete).
>
> Grid squares may be in 6-character form (as above) or in the shortened
> 4-character form (e.g. IO91).
>
> **NMEA Data** APRS recognizes raw ASCII data strings conforming to the
> NMEA 0183 Version 2.0 specification, originating from navigation
> equipment such as GPS and LORAN receivers. It is recommended that APRS
> stations interpret at least the following NMEA Received Sentence
> types:
>
> GGA Global Positioning System Fix Data
>
> GLL Geographic Position, Latitude/Longitude Data
>
> RMC Recommended Minimum Specific GPS/Transit Data VTG Velocity and
> Track Data
>
> WPL Way Point Location
>
> **Altitude** Altitude may be expressed in two ways:

- In the comment text.

- In Mic-E format.

> **Altitude in Comment Text** — The comment may contain an altitude
> value, in the form **/A=**aaaaaa, where aaaaaa is the altitude in
> feet. It must contain exactly 6 digits. For example: /A=001234. The
> altitude may appear anywhere in the comment.
>
> Although not in the official standard, many applications also
> recognize negative values in the form **/A=**-aaaaa . It must contain
> exactly 5 digits after the minus sign.
>
> **Altitude in Mic-E format** — The optional Mic-E status field can
> contain altitude data. See Chapter 10: Mic-E Data Format.

# APRS DATA EXTENSIONS

> A fixed-length 7-byte field may follow APRS position data. This field
> is an APRS Data Extension. The extension may be one of the following:

- CSE**/**SPD Course and Speed (this may be followed by a further 8
  bytes containing DF bearing and Number/Range/Quality parameters)

- DIR**/**SPD Wind Direction and Wind Speed

- **PHG**phgd Station Power and Effective Antenna Height/Gain/
  Directivity

- **RNG**rrrr Pre-Calculated Radio Range

- **DFS**shgd DF Signal Strength and Effective Antenna Height/Gain

- Tyy/Cxx Area Object Descriptor

> **Course and Speed** The 7-byte CSE**/**SPD Data Extension can be used
> to represent the course and speed of a vehicle or APRS Object.
>
> The course is expressed in degrees (001-360), clockwise from due
> north. The speed is expressed in knots. A slash **/** character
> separates the two.
>
> For example:
>
> 088/036 represents a course 88 degrees, traveling at 36 knots.
>
> If the course and speed are unknown or not relevant, they can be set
> to
>
> **000/000** or **.../...** or **⭸⭸⭸⭸⭸⭸/⭸⭸⭸⭸⭸⭸**. **WHAT WAS THIS
> ORIGINALLY**
>
> **Note**: In the special case of DF reports, a course of 000 means
> that the DF station is fixed. If the course is non-zero, the station
> is mobile.

## Wind Direction and Wind Speed

> The 7-byte DIR**/**SPD Data Extension can be used to represent the
> wind direction and sustained one-minute wind speed in a Weather
> Report.
>
> The wind direction is expressed in degrees (001-360), clockwise from
> due north. The speed is expressed in knots. A slash **/** character
> separates the two.
>
> For example:
>
> 220/004 represents a wind direction of 220 degrees and a speed of 4
> knots.
>
> If the wind direction and speed are unknown or not relevant, they can
> be set to **000/000** or **.../...** or **⭸⭸⭸⭸⭸⭸/⭸⭸⭸⭸⭸⭸**.

## Power, Effective Antenna

## Height/Gain/ Directivity

### PHG Codes

> The 7-byte **PHG**phgd Data Extension specifies the transmitter power,
> effective antenna height-above-average-terrain, antenna gain and
> antenna directivity. APRS uses this information to plot radio range
> circles around stations.
>
> The 7 characters of this Data Extension are encoded as follows:
> Characters 1–3: **PHG** (fixed)
>
> Character 4: p Power code Character 5: h Height code Character 6: g
> Antenna gain code Character 7: d Directivity code
>
> The PHG codes are listed in the table below:

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 7%" />
<col style="width: 6%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
</colgroup>
<thead>
<tr class="header">
<th>phgd <em><strong>Code:</strong></em></th>
<th><blockquote>
<p><strong>0</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>1</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>2</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>3</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>4</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>5</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>6</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>7</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>8</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>9</strong></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Units</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Power</td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>16</p>
</blockquote></td>
<td><blockquote>
<p>25</p>
</blockquote></td>
<td><blockquote>
<p>36</p>
</blockquote></td>
<td><blockquote>
<p>49</p>
</blockquote></td>
<td><blockquote>
<p>64</p>
</blockquote></td>
<td><blockquote>
<p>81</p>
</blockquote></td>
<td><blockquote>
<p>watts</p>
</blockquote></td>
</tr>
<tr class="even">
<td>Height</td>
<td><blockquote>
<p>10</p>
</blockquote></td>
<td><blockquote>
<p>20</p>
</blockquote></td>
<td><blockquote>
<p>40</p>
</blockquote></td>
<td><blockquote>
<p>80</p>
</blockquote></td>
<td><blockquote>
<p>160</p>
</blockquote></td>
<td><blockquote>
<p>320</p>
</blockquote></td>
<td><blockquote>
<p>640</p>
</blockquote></td>
<td><blockquote>
<p>1280</p>
</blockquote></td>
<td><blockquote>
<p>2560</p>
</blockquote></td>
<td><blockquote>
<p>5120</p>
</blockquote></td>
<td><blockquote>
<p>feet</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>Gain</td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>2</p>
</blockquote></td>
<td><blockquote>
<p>3</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>5</p>
</blockquote></td>
<td><blockquote>
<p>6</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>8</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>dBi</p>
</blockquote></td>
</tr>
<tr class="even">
<td>Directivity</td>
<td><blockquote>
<p>omni</p>
</blockquote></td>
<td><blockquote>
<p>45</p>
<p>NE</p>
</blockquote></td>
<td><blockquote>
<p>90</p>
<p>E</p>
</blockquote></td>
<td><blockquote>
<p>135</p>
<p>SE</p>
</blockquote></td>
<td><blockquote>
<p>180</p>
<p>S</p>
</blockquote></td>
<td><blockquote>
<p>225</p>
<p>SW</p>
</blockquote></td>
<td><blockquote>
<p>270</p>
<p>W</p>
</blockquote></td>
<td><blockquote>
<p>315</p>
<p>NW</p>
</blockquote></td>
<td><blockquote>
<p>360</p>
<p>N</p>
</blockquote></td>
<td></td>
<td><blockquote>
<p>deg</p>
</blockquote></td>
</tr>
</tbody>
</table>

> The height code represents the effective height of the antenna _above
> average local terrain_, not above ground or sea level — this is to
> provide a rough indication of the antenna’s effectiveness in the local
> area .
>
> The height code may in fact be any ASCII character 0–9 and above. This
> is so that larger heights for balloons, aircraft or satellites may be
> specified.
>
> For example:
>
> **:** is the height code for 10240 feet (approximately 1.9 miles).
>
> **;** is the height code for 20480 feet (approximately 3.9 miles), and
> so on.
>
> The Directivity code offsets the PHG circle by one third in the
> indicated direction. This means a front-to-back ratio of 2 to 1. Most
> often this is used to indicate a favored direction or a null, even if
> an omni antenna is at the site.
>
> An example of the PHG Data Extension:
>
> PHG5132 means a power of 25 watts,
>
> an antenna height of 20 feet above the average local terrain, an
> antenna gain of 3 dBi,
>
> and maximum gain due east.
>
> **Range Circle Plot** On receipt, APRS uses the **p**, **h**, **g**
> and **d** codes to calculate the usable radio range (in miles), for
> plotting a range circle representing the local radio horizon around
> the station. The radio range is calculated as follows:
>
> power = **p**2
>
> Height-above-average-terrain (haat) = 10 x 2**h**
>
> gain = 10(**g**/10)
>
> range = –( 2 x haat x –( (power/10) x (gain/2) ) ) Thus, for PHG5132:
>
> power = **5**2 = 25 watts
>
> haat = 10 x 2**1** = 20 feet
>
> gain = 10(**3**/10) = 1.995262
>
> range = –( 2 x 20 x –( (25/10) x (1.995262/2) ) )
>
> ~ 7.9 miles
>
> As the direction of maximum gain is due east, APRS will draw a range
> circle of radius 8 miles around the station, offset by 2.7 miles (i.e.
> one third of 8 miles) in an easterly direction.
>
> **Note**: In the absence of any PHG data, stations are assumed to be
> running 10 watts to a 3dBi omni antenna at 20 feet, resulting in a
> 6-mile radius range circle, centered on the station.
>
> For more information: [The Importance of PHG Range
> Circles](http://www.aprs.org/phg.html)

## Pre-Calculated Radio Range

> The 7-byte **RNG**rrrr Data Extension allows users to transmit a pre-
> calculated omni-directional radio range, where rrrr is the range in
> miles (with leading zeros).
>
> For example, RNG0050 indicates a radio range of 50 miles. APRS can use
> this value to plot a range circle around the station.

## Omni-DF Signal

## Strength

> The 7-byte **DFS**shgd Data Extension lets APRS localize jammers by
> plotting the overlapping signal strength contours of all stations
> hearing the signal.
>
> This Omni-DF format replaces the PHG format to indicate DF signal
> strength, in that the transmitter power field is replaced with the
> relative signal strength (s) from 0 to 9.

### DFS Codes

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 7%" />
<col style="width: 6%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 9%" />
</colgroup>
<thead>
<tr class="header">
<th>shgd <em><strong>Code:</strong></em></th>
<th><blockquote>
<p><strong>0</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>1</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>2</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>3</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>4</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>5</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>6</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>7</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>8</strong></p>
</blockquote></th>
<th><blockquote>
<p><strong>9</strong></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Units</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Strength</td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>2</p>
</blockquote></td>
<td><blockquote>
<p>3</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>5</p>
</blockquote></td>
<td><blockquote>
<p>6</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>8</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>S-points</p>
</blockquote></td>
</tr>
<tr class="even">
<td>Height</td>
<td><blockquote>
<p>10</p>
</blockquote></td>
<td><blockquote>
<p>20</p>
</blockquote></td>
<td><blockquote>
<p>40</p>
</blockquote></td>
<td><blockquote>
<p>80</p>
</blockquote></td>
<td><blockquote>
<p>160</p>
</blockquote></td>
<td><blockquote>
<p>320</p>
</blockquote></td>
<td><blockquote>
<p>640</p>
</blockquote></td>
<td><blockquote>
<p>1280</p>
</blockquote></td>
<td><blockquote>
<p>2560</p>
</blockquote></td>
<td><blockquote>
<p>5120</p>
</blockquote></td>
<td><blockquote>
<p>feet</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>Gain</td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>2</p>
</blockquote></td>
<td><blockquote>
<p>3</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>5</p>
</blockquote></td>
<td><blockquote>
<p>6</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>8</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>dB</p>
</blockquote></td>
</tr>
<tr class="even">
<td>Directivity</td>
<td><blockquote>
<p>omni</p>
</blockquote></td>
<td><blockquote>
<p>45</p>
<p>NE</p>
</blockquote></td>
<td><blockquote>
<p>90</p>
<p>E</p>
</blockquote></td>
<td><blockquote>
<p>135</p>
<p>SE</p>
</blockquote></td>
<td><blockquote>
<p>180</p>
<p>S</p>
</blockquote></td>
<td><blockquote>
<p>225</p>
<p>SW</p>
</blockquote></td>
<td><blockquote>
<p>270</p>
<p>W</p>
</blockquote></td>
<td><blockquote>
<p>315</p>
<p>NW</p>
</blockquote></td>
<td><blockquote>
<p>360</p>
<p>N</p>
</blockquote></td>
<td></td>
<td><blockquote>
<p>deg</p>
</blockquote></td>
</tr>
</tbody>
</table>

> For example, DFS2360 represents a weak signal (around strength S2)
> heard on an omni antenna with 6 dB gain at 80 feet.
>
> A signal strength of zero (0) is particularly significant, because
> APRS uses these 0 signal reports to draw (usually black) circles where
> the jammer is _not_ heard. These black circles are extremely valuable
> since there will be a lot more reports from stations that do not hear
> the jammer than from those that do. This quickly eliminates a lot of
> territory.

## Bearing and Number/Range/

## Quality

> DF reports contain an 8-byte field **/**BRG**/**NRQ that follows the
> CSE**/**SPD Data Extension, specifying the course, speed, bearing and
> NRQ (Number/Range/ Quality) value of the report. NRQ indicates the
> Number of hits, the approximate Range and the Quality of the report.
>
> For example, in:
>
> …088/036/270/729… course = 88 degrees, speed = 36 knots,
>
> bearing = 270 degrees, N = 7, R = 2, Q = 9
>
> If N is 0, then the NRQ value is meaningless. Values of N from 1 to 8
> give an indication of the number of hits per period relative to the
> length of the time period — thus a value of 8 means 100% of all
> samples possible got a hit. A value of 9 for N indicates to other
> users that the report is manual.
>
> The N value is not processed, but is just another indicator from the
> automatic DF units.
>
> The range (R) indicates the approximate area of interest. The range
> limits the length of the line to the original map’s scale of the
> sending station. The range is 2R so, for R=4, the range will be 16
> miles.
>
> The Q byte is QUALITY and is a relative measure of the degree of
> unknown in the beam heading. Q is a single digit in the range 0–9, and
> provides an indication of bearing accuracy. It is best interpreted as
> a BEAMWIDTH as follows:
>
> See original [DF.TXT](http://www.aprs.org/APRS-docs/DF.TXT) and
> [PROTOCOL.TXT](http://www.aprs.org/APRS-docs/PROTOCOL.TXT) for more
> information
>
> If the course and speed parameters are not appropriate, they should
> have the value **000/000** or **.../...** or **⭸⭸⭸/⭸⭸⭸**.
> **FIXME weird characters**

## Area Object Descriptor

> The 7-byte Tyy/Cxx Data Extension is an Area Object Descriptor. The T
>
> parameter specifies the type of object (square, circle, triangle, etc)
> and the
>
> /C parameter specifies its fill color.
>
> Area Objects are described in Chapter 11: Object and Item Reports.

# POSITION AND DF REPORT DATA FORMATS

# FIXME - continue here with heading 2 attribute

> **Position Reports** Lat/Long Position Reports are contained in the
> Information field of an APRS AX.25 frame.
>
> The following diagrams show the permissible formats of these reports,
> together with some examples. The gray areas indicate optional fields,
> and the shaded (yellow) characters are literal ASCII characters. In
> all cases there is a maximum of 43 characters after the Symbol Code.
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> **DF Reports** DF Reports are contained in the Information field of an
> APRS AX.25 frame. The Bearing and Number/Range/Quality (BRG/NRQ)
> parameters follow the Data Extension field.
>
> **Note**: The BRG/NRQ parameters are only meaningful when the report
> contains the DF symbol (i.e. the Symbol Table ID is **/** and the
> Symbol Code is **\\**).
>
> **Note**: If the DF station is fixed, the Course value is zero. If the
> station is moving, the Course value is non-zero.
>
> Bytes:

# COMPRESSED POSITION REPORT DATA FORMATS

> In compressed data format, the Information field contains the
> station’s latitude and longitude, together with course and speed or
> pre-calculated radio range or altitude.
>
> This information is compressed to minimize the length of the
> transmitted packet (and therefore improve its chances of being
> received correctly under less than ideal conditions).
>
> The Information field also contains a display Symbol Code, and there
> may optionally be a plain text comment (uncompressed) as well.
>
> **The Advantages of Data Compression**
>
> Compressed data format may be used in place of the numeric lat/long
> coordinates already described, such as in the **!**, **/**, **@** and
> **=** formats.
>
> Data compression has several important benefits:

- Fully backwards compatible with all existing formats.

- Fully supports any comment string.

- Speed is accurate to +/-1 mph up to about 40 mph and within 3% at
  600 mph.

- Altitude in feet is accurate to +/- 0.4% from 1 foot to 3000 miles.

- Consistent one-algorithm processing of compressed latitude and
  longitude.

- Improved position to 1 foot worldwide.

- Pre-calculated radio range, compressed to one byte.

- Potential 50% compression of every position format on the air.

- Potential 40% reduction of raw GPS NMEA data length.

- Additional 7-byte reduction for NEMA GGA altitudes.

- Support for TNC compression at the NMEA source (from the GPS
  receiver).

- Digipeater compression of old NMEA trackers on the fly.

- Usage is optional in all cases.

> The only minor disadvantages are that the course only resolves to +/-
> 2 degrees, and this format does not support PHG.

**Compressed Data**

**Format**

Bytes:

> Compressed data may be generated in several ways:

- by APRS software.

- pre-entered manually into a digipeater’s beacon text.

- by a digipeater converting raw tracker NMEA packets to compressed.

> \[In future, there is the possibility that a Kantronics KPC-3 or other
> tracker TNC will be able to compress data directly from an attached
> GPS receiver\].
>
> In all cases the compressed format is a fixed 13-character field:
>
> /YYYYXXXX$csT
>
> where / is the Symbol Table Identifier YYYY is the compressed latitude
> XXXX is the compressed longitude
>
> $ is the Symbol Code
>
> cs is the compressed course/speed or compressed pre-calculated radio
> range or compressed altitude
>
> T is the compression type indicator

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 19%" />
<col style="width: 19%" />
<col style="width: 13%" />
<col style="width: 26%" />
<col style="width: 10%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="6"><blockquote>
<p><em><strong>Compressed Position Data</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="3"><blockquote>
<p><em><strong>Sym Table ID</strong></em></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Compressed Lat</strong></em> YYYY</p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Compressed Long</strong></em> XXXX</p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Symbol Code</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Compressed Course/Speed</strong></em></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Comp Type</strong></em> T</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong>Compressed Radio Range</strong></em></p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><em><strong>Compressed Altitude</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>2</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Compressed format can be used in place of lat/long position format
> anywhere that …ddmm.hhN/dddmm.hhW$xxxxxxx… occurs.
>
> All bytes except for the **/** and **$** are base-91 printable ASCII
> characters (**!**..**{**). These are converted to numeric values by
> subtracting 33 from the decimal ASCII character code. For example,
> **\#** has an ASCII code of 35, and represents a numeric value of 2
> (i.e. 35-33).
>
> **Symbol** The presence of the leading Symbol Table Identifier instead
> of a digit indicates that this is a compressed Position Report and not
> a normal lat/long report.
>
> **Lat/Long Encoding** The values of YYYY and XXXX are computed as
> follows:
>
> YYYY is 380926 x (90 – latitude) \[base 91\]
>
> latitude is positive for north, negative for south, in degrees.
>
> XXXX is 190463 x (180 + longitude) \[base 91\]
>
> longitude is positive for east, negative for west, in degrees.
>
> For example, for a longitude of 72° 45' 00" west (i.e. -72.75
> degrees), the math is 190463 x (180 – 72.75) = 20427156. Because this
> is to base 91, it is then necessary to progressively divide this value
> by reducing powers of 91, to obtain the numerical values of X:
>
> 20427156 / 91<sup>3</sup> = **27**, remainder 80739
>
> 80739 / 91<sup>2</sup> = **9**, remainder 6210
>
> 6210 / 91<sup>1</sup> = **68**, remainder **22**
>
> To obtain the corresponding ASCII characters, 33 is added to each of
> these values, yielding 60 (i.e. 27+33), 42, 101 and 55. From the ASCII
> Code Table (in Appendix 3), this corresponds to **&lt;\*e7** for XXXX.
>
> **Lat/Long Decoding** To decode a compressed lat/long, the reverse
> process is needed. That is, if
>
> YYYY is represented as
> y<sub>1</sub>y<sub>2</sub>y<sub>3</sub>y<sub>4</sub> and XXXX as
> x<sub>1</sub>x<sub>2</sub>x<sub>3</sub>x<sub>4</sub>, then:
>
> Lat = 90 - ((y<sub>1</sub>-33) x 91<sup>3</sup> + (y<sub>2</sub>-33) x
> 91<sup>2</sup> + (y<sub>3</sub>-33) x 91 + y<sub>4</sub>-33) / 380926
>
> Long = -180 + ((x<sub>1</sub>-33) x 91<sup>3</sup> +
> (x<sub>2</sub>-33) x 91<sup>2</sup> + (x<sub>3</sub>-33) x 91 +
> x<sub>4</sub>-33) / 190463
>
> For example, if the compressed value of the longitude is **&lt;\*e7**
> (as computed above), the calculation becomes:

Long = -180 + (27 x 91<sup>3</sup> + 9 x 91<sup>2</sup> + 68 x 91 + 22)
/ 190463

= -180 + (20346417 + 74529 + 6188 + 22) / 190463

> = -180 + 107.25
>
> = -72.75 degrees
>
> **Course/Speed, Pre-Calculated Radio Range and**

**Altitude**

> The two cs bytes following the Symbol Code character can contain
> either the compressed course and speed or the compressed
> pre-calculated radio range or the station’s altitude. These two bytes
> are in base 91 format.
>
> In the special case of c = **�** (space), there is no course, speed or
> range data, in which case the csT bytes are ignored.
>
> **Course/Speed** — If the ASCII code for c is in the range **!** to
> **z** inclusive — corresponding to numeric values in the range 0–89
> decimal (i.e. after subtracting 33 from the ASCII code) — then cs
> represents a compressed course/speed value:
>
> course = **c** x 4
>
> speed = 1.08**s** – 1
>
> For example, if the cs characters are **7P**, the corresponding values
> of **c** and **s** (after subtracting 33 from the ASCII character
> code) are 22 and 47 respectively. Substituting these values in the
> above equations:
>
> course = **22** x 4 = 88 degrees
>
> speed = 1.08**47** – 1 = 36.2 knots
>
> **Pre-Calculated Radio Range** — If c = **{**, then cs represents a
> compressed pre-calculated radio range value:
>
> range = 2 x 1.08**s**
>
> For example, if the cs bytes are **{?**, the ASCII code for **?** is
> 63, so the value of **s** is 30 (i.e. 63-33). Thus:
>
> range = 2 x 1.08**30**
>
> ~ 20 miles
>
> So APRS will draw a circle of radius 20 miles around the station plot
> on the screen.
>
> **The Compression Type (T) Byte**

Bit:

Value:

> The T byte follows the cs bytes. The T byte contains several bit
> fields showing the GPS fix status, the NMEA source of the position
> data and the origin of the compression.
>
> The T byte is not meaningful if the c byte is **�** (space).

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 13%" />
<col style="width: 15%" />
<col style="width: 9%" />
<col style="width: 7%" />
<col style="width: 13%" />
<col style="width: 13%" />
<col style="width: 13%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="8"><blockquote>
<p><em><strong>Compression Type (T) Byte Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>6</p>
</blockquote></td>
<td><blockquote>
<p>5</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>3</p>
</blockquote></td>
<td><blockquote>
<p>2</p>
</blockquote></td>
<td>1</td>
<td>0</td>
</tr>
<tr class="even">
<td><blockquote>
<p><em>Not used</em></p>
</blockquote></td>
<td><blockquote>
<p><em>Not used</em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>GPS Fix</strong></em></p>
</blockquote></td>
<td colspan="2"><blockquote>
<p><em><strong>NMEA Source</strong></em></p>
</blockquote></td>
<td colspan="3"><blockquote>
<p><em><strong>Compression Origin</strong></em></p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0 = old (last)</p>
</blockquote></td>
<td colspan="2"><blockquote>
<p>0 0 = other</p>
</blockquote></td>
<td colspan="3"><blockquote>
<p>0 0 0 = Compressed</p>
</blockquote></td>
</tr>
<tr class="even">
<td></td>
<td></td>
<td><blockquote>
<p>1 = current</p>
</blockquote></td>
<td colspan="2"><blockquote>
<p>0 1 = GLL</p>
</blockquote></td>
<td colspan="3"><blockquote>
<p>0 0 1 = TNC BText</p>
</blockquote></td>
</tr>
<tr class="odd">
<td></td>
<td></td>
<td></td>
<td colspan="2"><blockquote>
<p>1 0 = GGA</p>
</blockquote></td>
<td colspan="3"><blockquote>
<p>0 1 0 = Software (DOS/Mac/Win/+SA)</p>
</blockquote></td>
</tr>
<tr class="even">
<td></td>
<td></td>
<td></td>
<td colspan="2"><blockquote>
<p>1 1 = RMC</p>
</blockquote></td>
<td colspan="3"><blockquote>
<p>0 1 1 = [tbd]</p>
</blockquote></td>
</tr>
<tr class="odd">
<td></td>
<td></td>
<td></td>
<td colspan="2"></td>
<td colspan="3"><blockquote>
<p>1 0 0 = KPC3</p>
</blockquote></td>
</tr>
<tr class="even">
<td></td>
<td></td>
<td></td>
<td colspan="2"></td>
<td colspan="3"><blockquote>
<p>1 0 1 = Pico</p>
</blockquote></td>
</tr>
<tr class="odd">
<td></td>
<td></td>
<td></td>
<td colspan="2"></td>
<td colspan="3"><blockquote>
<p>1 1 0 = Other tracker [tbd]</p>
</blockquote></td>
</tr>
<tr class="even">
<td></td>
<td></td>
<td></td>
<td colspan="2"></td>
<td colspan="3"><blockquote>
<p>1 1 1 = Digipeater conversion</p>
</blockquote></td>
</tr>
</tbody>
</table>

> For example, if the compressed position was derived from an RMC
> sentence, the fix is current, and the compression was performed by
> APRSdos software, then the value of T in binary is 0 0 1 11 010, which
> equates to 58 decimal.
>
> Adding 33 to this value gives the ASCII code for the T byte (i.e. 91),
> which
>
> corresponds to the **\[** character.
>
> Thus, using data from all the earlier examples, if the RMC sentence
> contains (among other parameters) the following data:
>
> Latitude = 49° 30' 00" north
>
> Longitude = 72° 45' 00" west Speed = 36.2 knots
>
> Course = 88°
>
> and: the fix is current
>
> compression is performed by APRSdos software the display symbol is a
> “car”
>
> then the complete 13-character compressed location field is
> transmitted as:

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 26%" />
<col style="width: 26%" />
<col style="width: 10%" />
<col style="width: 26%" />
</colgroup>
<thead>
<tr class="header">
<th>/</th>
<th>YYYY</th>
<th>XXXX</th>
<th><blockquote>
<p>$</p>
</blockquote></th>
<th><blockquote>
<p>csT</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>/</strong></td>
<td><strong>5L!!</strong></td>
<td><strong>&lt;*e7</strong></td>
<td><blockquote>
<p><strong>&gt;</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>7P[</strong></p>
</blockquote></td>
</tr>
</tbody>
</table>

> **Altitude** If the T byte indicates that the raw data originates from
> a GGA sentence (i.e. bits 4 and 3 of the T byte are 10), then the
> sentence contains an altitude value, in feet. After compression, the
> compressed altitude data is placed in the cs bytes, such that:
>
> altitude = 1.002**cs** feet
>
> For example, if the received cs bytes are **S\]**, the computation is
> as follows:

- Subtract 33 from the ASCII code for each character:

> **c** = 83 – 33 = 50
>
> **s** = 93 – 33 = 60

- Multiply **c** by 91 and add **s** to obtain **cs**: **cs** = 50 x
  91 + 60

> = 4610

- Then altitude = 1.002**4610**

= 10004 feet

> **New Trackers** Tracker firmware may compress GPS data directly to
> APRS compressed format. They would use the **!** Data Type Indicator,
> showing that the position is real-time and that the tracker is not
> APRS-capable.
>
> If the Position Report is not real-time, then the **/** Data Type
> Indicator can be used instead, so that the latest fix time may be
> included.
>
> **Old Trackers** Some digipeaters have the ability to convert raw NMEA
> strings from existing trackers to compressed data format for further
> forwarding.
>
> These digipeaters will compress the data if the tracker Destination
> Address is
>
> GPS. (**Note**: This is the 3-letter address GPS, not GPS\*).
>
> Trackers desiring for their packets to not be modified by the APRS
> network will use any other valid generic APRS Destination Address.

**Compressed Report**

**Formats**

> Compressed data is contained in the AX.25 Information field, in these
> formats:
>
> Bytes:
>
> Bytes:

# MIC-E DATA FORMAT

> **Mic-E Data Format** In Mic-E data format, the station’s position,
> course, speed and display symbol, together with an APRS digipeater
> path and Mic-E Position Comment, are packed into the AX.25 Destination
> Address and Information fields.
>
> The Information field can also optionally contain either Mic-E
> telemetry data or Mic-E status. The Mic-E Status can contain the
> station’s Maidenhead locator and altitude.
>
> Mic-E packets can be very short. At the minimum, with no callsigns in
> the Digipeater Addresses field and no optional telemetry data or Mic-E
> status text, a complete Mic-E packet is just 25 bytes long (excluding
> FCS and flags).
>
> Mic-E data format is not only used in the [Microphone Encoder
> unit](https://web.tapr.org/product_docs/Mic-E/mic-edev/mic-e.manual.pdf);
> it is also used in the [PIC
> Encoder](https://www.tapr.org/pdf/DCC1999-APRS-MicLite-WB4APR.pdf) and
> in the Kenwood TH-D7 and TM-D700 radios.
>
> **Mic-E Data Payload** The Mic-E data format allows a large amount of
> data to be carried in a very short packet. The data is split between
> the Destination Address field and the Information field of a standard
> AX.25 UI-frame.
>
> **Destination Address Field** — The 7-byte Destination Address field
> contains the following encoded information:

- The 6 latitude digits.

- A 3-bit Mic-E position comment, specifying one of 7 Standard Mic-E
  Position Comment Codes or one of 7 Custom Position Comments or an
  Emergency Position Comment.

- The North/South and West/East Indicators.

- The Longitude Offset Indicator.

- The generic APRS digipeater path code.

> Although the destination address appears to be quite unconventional,
> it is still a valid AX.25 address, consisting only of printable 7-bit
> ASCII values (shifted one bit left) — see the _Amateur Packet-Radio
> Link-Layer Protocol_ specification for full details of the format of
> standard AX.25 addresses.
>
> **Information Field** — This field contains the following data:

- The encoded longitude.

- The encoded course and speed.

- The display Symbol Code and Symbol Table Identifier.

- An optional field, containing either Mic-E telemetry data or a Mic-E
  status text string. The status string can contain plain text,
  Maidenhead

> locator or the station’s altitude.
>
> **Mic-E Destination Address Field**
>
> The standard AX.25 Destination Address field consists of 7 bytes,
> containing 6 callsign characters and the SSID (plus a number of other
> bits that are not of interest here). When used to carry Mic-E data,
> however, this field has a quite different format:
>
> Bytes:
>
> The Destination Address field contains:

- Six encoded latitude digits specifying degrees (digits 1 and 2),
  minutes (digits 3 and 4) and hundredths of minutes (digits 5 and 6).

- 3-bit Mic-E Position Comment identifier (bits A, B and C).

- North/South latitude indicator.

- Longitude offset (adds 0 degrees or 100 degrees to the longitude
  computation in the Information field).

- West/East longitude indicator.

- Generic APRS digipeater path (encoded in the SSID).

> **Destination Address Field**

**Encoding**

> The table on the next page shows the encoding of the first 6 bytes of
> the Destination Address field, for all combinations of latitude digit,
> the 3-bit Mic-E message identifier (A/B/C), the latitude/longitude
> indicators and the longitude offset.
>
> The encoding supports position ambiguity.
>
> The ASCII characters shown in the table are left-shifted one bit
> position prior to transmission.
>
> **Mic-E Destination Address Field Encoding (Bytes 1–6)**
>
> **Note**: the ASCII characters **A**–**K** are not used in address
> bytes 4–6.
>
> For example, for a station at a latitude of 33 degrees 25.64 minutes
> north, in the western hemisphere, with longitude offset +0 degrees,
> and transmitting standard position comment identifier bits 1/0/0, the
> encoding of the first 6 bytes of the Destination Address field is as
> follows:
>
> Destin
>
> **Mic-E Position Comments**
>
> The first three bytes of the Destination Address field contain three
> position comment bits: A, B and C. These bits allow one of 15 position
> comment types to be specified:

- 7 Standard

- 7 Custom

- 1 Emergency

> For the 7 Standard position comments, one or more of the identifier
> bits is a
>
> **1**, shown in the Mic-E Destination Address Field Encoding table as
> 1 (Std).
>
> For the 7 Custom position comments, one or more of the identifier bits
> is a **1**, shown in the Mic-E Destination Address Field Encoding
> table as 1 (Custom).
>
> For the Emergency position comment, all three identifier bits are
> **0**.
>
> The following table shows the encoding of Mic-E message types, for all
> combinations of the A/B/C message identifier bits:
>
> **Mic-E Position Comment Types**

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 36%" />
<col style="width: 36%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>A</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>B</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>C</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Standard Mic-E</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Custom Mic-E</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>M0: Off Duty</p>
</blockquote></td>
<td><blockquote>
<p>C0: Custom-0</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>M1: En Route</p>
</blockquote></td>
<td><blockquote>
<p>C1: Custom-1</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>M2: In Service</p>
</blockquote></td>
<td><blockquote>
<p>C2: Custom-2</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>M3: Returning</p>
</blockquote></td>
<td><blockquote>
<p>C3: Custom-3</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>M4: Committed</p>
</blockquote></td>
<td><blockquote>
<p>C4: Custom-4</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>M5: Special</p>
</blockquote></td>
<td><blockquote>
<p>C5: Custom-5</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>M6: Priority</p>
</blockquote></td>
<td><blockquote>
<p>C6: Custom-6</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td colspan="2"><blockquote>
<p>Emergency</p>
</blockquote></td>
</tr>
</tbody>
</table>

> The Standard position comments and the Emergency position comment have
> the same meaning for all APRS stations. The Custom position comments
> may be assigned any arbitrary meaning.
>
> **Note**: Support for Custom position comments is optional. Original
> Mic-E units do not support Custom position comments.
>
> **Note**: If the A/B/C position comment identifier bits contain a
> mixture of Standard **1**s and Custom **1**s, the position comment
> type is “unknown”.
>
> Some examples of MIC-E Position Comment type encoding:

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 36%" />
<col style="width: 17%" />
<col style="width: 21%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>First 3 Destination Address Bytes</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Identifier Bits A/B/C</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Type</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Meaning</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>S32</strong></p>
</blockquote></td>
<td><blockquote>
<p>Standard 1 / 0 / 0</p>
</blockquote></td>
<td><blockquote>
<p>Standard</p>
</blockquote></td>
<td><blockquote>
<p>M3: Returning</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>F2D</strong></p>
</blockquote></td>
<td><blockquote>
<p>Custom 1 / 0 / Custom 1</p>
</blockquote></td>
<td><blockquote>
<p>Custom</p>
</blockquote></td>
<td><blockquote>
<p>C2: Custom-2</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>234</strong></p>
</blockquote></td>
<td><blockquote>
<p>0 / 0 / 0</p>
</blockquote></td>
<td><blockquote>
<p>Emergency</p>
</blockquote></td>
<td><blockquote>
<p>Emergency</p>
</blockquote></td>
</tr>
</tbody>
</table>

> **Destination Address SSID Field**
>
> The SSID in the Destination Address field of a Mic-E packet is coded
> to specify either a conventional digipeater VIA path (contained in the
> Digipeater Addresses field of the AX.25 frame), or one of 15 generic
> APRS digipeater paths. See Chapter 4: APRS Data in the AX.25
> Destination and Source Address Fields.

**Mic-E Information**

**Field**

> The Information field is used to complete the Position Report that was
> begun in the Destination Address field. The encoding used is different
> from the destination address since the content is not constrained to
> be printable, shifted 7-bit ASCII, as it is in the address. However,
> full 8-bit binary is not used — all values are offset by 28 and
> further operations (described below) are performed on some of the
> values to make almost all of the data printable ASCII.
>
> The format of the Information field is as follows:
>
> Bytes:

**Information Field**

**Data**

> The first 9 bytes of the Information field contain the APRS Data Type
> Identifier, longitude, speed, course and symbol data.
>
> The APRS Data Type Identifier is one of:
>
> ‘ Current GPS data
>
> (but not used in Kenwood TM-D700 radios) .
>
> **'** Old GPS data
>
> (or _Current_ GPS data in Kenwood TM-D700 radios).
>
> 0x1c Current GPS data (Rev. 0 beta units only). 0x1d Old GPS data
> (Rev. 0 beta units only).
>
> **<u>IMPORTANT NOTE</u>**: As explained in detail below, some of the
> bytes in the Information field are non-printing ASCII characters. In
> certain circumstances (such as incorrect TNC setup or inappropriate
> software), some of these non-printing characters may be dropped,
> making the Information field appear shorter than it really is. This
> will lead to incorrect decoding. (In particular, if the Information
> field appears to be less than 9 bytes long, the packet must be
> ignored).

**Longitude Degrees**

**Encoding**

> The **d+28** byte in the Information field contains the encoded value
> of the longitude degrees, in the range 0–179 degrees.
>
> (Note that for longitude values in the range 0–9 degrees, the
> longitude offset is +100 degrees):
>
> **Mic-E Longitude Degrees Encoding**
>
> Note from the table that the encoding is split into four separate
> pieces:

- <u>0–9 degrees</u>: **d+28** is in the range 118–127 decimal,
  corresponding to the ASCII characters **v** to **DEL**.

> **Important Note**: The longitude offset is set to **+100 degrees**
> when the longitude is in the range 0–9 degrees.

- <u>10–99 degrees</u>: **d+28** is in the range 38–127 decimal
  (corresponding to the ASCII characters **&** to **DEL**), and the
  longitude offset is +0 degrees.

  - <u>100–109 degrees</u>: **d+28** is in the range 108–117
    decimal, corresponding to the ASCII characters **l** (lower-case
    letter “L”) to **“u”**, and the longitude offset is +100
    degrees.

  - <u>110–179 degrees</u>: **d+28** is in the range 38–107 decimal
    (corresponding to the ASCII characters **&** to **“k”**), and
    the longitude offset is +100 degrees.

> Thus the overall range of valid **d+28** values is 38–127 decimal
> (corresponding to ASCII characters **&** to **DEL**).
>
> All of these characters (<u>except **DEL**, for 9 and 99 degrees</u>)
> are printable ASCII characters.
>
> To decode the longitude degrees value:

1.  subtract 28 from the **d+28** value to obtain **d**.

2.  if the longitude offset is +100 degrees, add 100 to **d**.

3.  subtract 80 if 180 ˜ **d** ˜ 189

> (i.e. the longitude is in the range 100–109 degrees).

1.  or, subtract 190 if 190 ˜ **d** ˜ 199.

> (i.e. the longitude is in the range 0–9 degrees).

**Longitude Minutes**

**Encoding**

> The **m+28** byte in the Information field contains the encoded value
> of the longitude minutes, in the range 0–59 minutes:
>
> **Mic-E Longitude Minutes Encoding**
>
> Note from the table that the encoding is split into two separate
> pieces:

- <u>0–9 minutes</u>: **m+28** is in the range 88–97 decimal,
  corresponding to the ASCII characters **X** to **a**.

- <u>10–59 minutes</u>: **m+28** is in the range 38–87 decimal
  (corresponding to the ASCII characters **&** to **W**).

> Thus the overall range of valid **m+28** values is 38–97 decimal
> (corresponding
>
> to ASCII characters **&** to **a**). All of these characters are
> printable ASCII characters.
>
> To decode the longitude minutes value:

1.  subtract 28 from the **m+28** value to obtain **m**.

2.  subtract 60 if **m** ™ 60.

> (i.e. the longitude minutes is in the range 0–9).
>
> **Longitude Hundredths of Minutes Encoding**
>
> The **h+28** byte in the Information field contains the encoded value
> of the longitude hundredths of minutes, in the range 0–99 minutes.
> This byte takes a value in the range 28 decimal (corresponding to 0
> hundredths of a minute) through 127 decimal (corresponding to 99
> hundredths of a minute).
>
> To decode the longitude hundredths of minutes value, subtract 28 from
> the
>
> **h+28** value.
>
> All of the possible values are printable ASCII characters (<u>except
> 0–3 and 99</u> <u>hundredths of a minute</u>).

**Speed and Course**

**Encoding**

> The speed and course of a station are encoded in 3 bytes, designated
> **SP+28**, **DC+28** and **SE+28**.
>
> The speed is in the range 0–799 knots, and the course is in the range
> 0–360 degrees (0 degrees represents an unknown or indefinite course,
> and 360 degrees represents due north).
>
> The encoded speed and course are spread over the three bytes, as
> follows:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 18%" />
<col style="width: 16%" />
<col style="width: 31%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="2"><blockquote>
<p><em><strong>Speed</strong></em></p>
</blockquote></th>
<th colspan="2"><blockquote>
<p><em><strong>Course</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>Encoded Speed (hundreds/tens of knots)</p>
</blockquote></td>
<td colspan="2"><blockquote>
<p>Encoded Speed (units) and Encoded Course (hundreds of degrees)</p>
</blockquote></td>
<td><blockquote>
<p>Encoded Course (tens/units)</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>SP+28</strong></p>
</blockquote></td>
<td colspan="2"><blockquote>
<p><strong>DC+28</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>SE+28</strong></p>
</blockquote></td>
</tr>
</tbody>
</table>

> **SP+28 Encoding** The **SP+28** byte contains the encoded speed, in
> hundreds/tens of knots, according to this table:
>
> **SP+28 Speed Encoding (hundreds/tens of knots)**
>
> **Note**: The ASCII characters shown in white on a black background
> are non- printing characters.
>
> **Note**: For speeds in the range 0–199 knots, there are two encoding
> schemes in existence. Hence there are two columns for the ASCII
> character, and two columns for the corresponding **SP+28** byte
> values.
>
> For example, for a speed of 73 knots (i.e. in the range 70–79), the
> **SP+28** byte may contain either **s** or **\#**, depending on the
> encoding method used. Both are equally valid.
>
> The decoding algorithm described later handles either of these
> encoding schemes.
>
> **DC+28 Encoding** The **DC+28** byte contains the encoded units of
> speed, plus the encoded course in hundreds of degrees:
>
> **DC+28 Speed / Course Encoding (units of knots/hundreds of degrees)**
>
> **Note**: The ASCII characters shown in white on a black background
> are non- printing characters.
>
> **Note**: There are two encoding schemes in existence for the
> **DC+28** byte. Hence there are two columns for the ASCII character,
> and two columns for the corresponding **DC+28** byte values.
>
> For example, for a speed of 73 knots (i.e. units=3) and a bearing of
> 294 degrees (i.e. in the range 200–299), the **DC+28** byte may
> contain either **@** or
>
> **&lt;**, depending on the encoding method used. Both are equally
> valid.
>
> The decoding algorithm described later handles either of these
> encoding schemes.
>
> **SE+28 Encoding** The **SE+28** byte contains the encoded tens and
> units of degrees of the course:
>
> **SE+28 Course Encoding (tens/units of degrees)**
>
> **Example of Mic-E Speed and Course**

**Encoding**

> For a speed of 86 knots and a course of 194 degrees, the encoding is:
>
> **SP+28**: The speed is in the range 80–89 knots. From the **SP+28**
> encoding table, the **SP+28** byte may be either **t** or **$**.
>
> **DC+28**: The units of speed are 6, and the course is in the range
> 100–199 degrees. From the **DC+28** encoding table, the **DC+28** byte
> may be either **\]** or **Y**.
>
> **SE+28**: The course in tens and units of degrees is 94. From the
> **SE+28**
>
> encoding table, the **SE+28** byte will be **z**.
>
> **Decoding the Speed and Course**
>
> To decode the speed and course:
>
> **SP+28**: To obtain the speed in tens of knots, subtract 28 from the
> **SP+28**
>
> value and multiply by 10.
>
> **DC+28**: Subtract 28 from the **DC+28** value and divide the result
> by 10. The quotient is the units of speed. The remainder is the course
> in hundreds of degrees.
>
> **SE+28**: To obtain the tens and units of degrees, subtract 28 from
> the **SE+28**
>
> value.
>
> Finally, make these speed and course adjustments:

- If the computed speed is ™ 800 knots, subtract 800.

- If the computed course is ™ 400 degrees, subtract 400.

> **Example of Decoding the Information Field**

**Data**

> If the first 9 bytes of the Information field contain ‘**(\_fn "Oj/**,
> and the destination address specifies that the station is in the
> western hemisphere with a longitude offset of +100 degrees, then the
> data is decoded as follows:

- ‘ is the APRS Data Type Identifier for a Mic-E packet containing
  current GPS data.

- **(** is the **d+28** byte. The **(** character has the value 40
  decimal. Subtracting 28 gives 12. The longitude offset (in the
  destination address) is +100 degrees, so the longitude is 100 + 12 =
  112 degrees.

- **\_** is the **m+28** byte. The **\_** character has the value 95
  decimal. Subtracting 28 gives 67. This is ™ 60, so subtracting 60
  gives a value of 7 minutes longitude.

- **f** is the **h+28** byte. The **f** character has the value 102
  decimal. Subtracting 28 gives 74 hundredths of a minute.

> Thus the longitude is 112 degrees 7.74 minutes west. The speed and
> course are calculated as follows:

- **n** is the **SP+28** byte. The **n** character has the value 110
  decimal. After subtracting 28, the result is 82. As this is ™ 80, a
  further 80 is subtracted, leaving a result of 2 tens of knots.

- **"** is the **DC+28** byte. The **"** character has the value 34
  decimal. Subtracting 28 gives 6. Dividing this by 10 gives a
  quotient of 0 (units of speed). Adding the first part of the speed,
  multiplied by 10 (i.e. 20) to the quotient (0) gives a final
  computed speed of 20 knots.

> The remainder from the division is 6. Subtracting 4 gives the course
> in hundreds of degrees; i.e. 2.

- **O** (upper-case letter “O”) is the **SE+28** byte. The **O**
  character has the value 79 decimal. Subtracting 28 gives 51. Adding
  this to the remainder calculated above, multiplied by 100 (i.e.
  200), gives the final value of 251 degrees for the course.

> The last two characters (**j/**) represent the jeep symbol from the
> Primary Symbol Table.
>
> **Mic-E Position Ambiguity**
>
> As mentioned in Chapter 6 (Time and Position Formats), a station may
> reduce the precision of its position by introducing position
> ambiguity. This is also possible in Mic-E data format.
>
> The position ambiguity is specified for the latitude (in the
> destination address). The same degree of ambiguity will then also
> apply to the longitude.
>
> For example, if the destination address is **T4SQZZ**, the last two
> digits of the
>
> latitude are ambiguous (represented by **ZZ**). Then, if the longitude
> data in the Information field is **(\_f** , as in the above example,
> the last two digits of the computed longitude will be ignored — that
> is, the longitude will be 112 degrees 7 minutes.

**Mic-E Telemetry**

**Data**

Bytes:

> The Information field may optionally contain either Mic-E telemetry
> data values or Mic-E status text.
>
> If the byte following the Symbol Table Identifier is one of the
> Telemetry Flag characters (**‘**,**'** or 0x1d), then telemetry data
> follows:

<table>
<colgroup>
<col style="width: 24%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="6"><blockquote>
<p><em><strong>Optional Mic-E Telemetry Data</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong>Telemetry Flag</strong></em></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p><em><strong>Telemetry Data Channels</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>F</p>
</blockquote></td>
<td><blockquote>
<p>Ch 1</p>
</blockquote></td>
<td><blockquote>
<p>Ch 2</p>
</blockquote></td>
<td><blockquote>
<p>Ch 3</p>
</blockquote></td>
<td><blockquote>
<p>Ch 4</p>
</blockquote></td>
<td><blockquote>
<p>Ch 5</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
</tr>
</tbody>
</table>

> The Telemetry Flag F is one of:
>
> **‘** 2 printable hex telemetry values follow (channels 1 and 3).
>
> **'** 5 printable hex telemetry values follow.
>
> 0x1d 5 binary telemetry values follow (Rev. 0 beta units only).
>
> If F is **‘** or **'**, each channel requires 2 bytes, containing a
> 2-digit printable hexadecimal representation of a value ranging from
> 0–255. For example, 254 is represented as **FE**.
>
> If F is 0x1d, each channel requires one byte, containing an 8-bit
> binary value.
>
> For example, if the telemetry data is **'7200007100**, the **'**
> indicates that 5 bytes of telemetry follow, coded in hexadecimal:
>
> 0x72 = 114 decimal 0x00 = 0 decimal 0x00 = 0 decimal 0x71 = 113
> decimal 0x00 = 0 decimal
>
> **Mic-E Status Text** As an alternative to telemetry data, the packet
> may include Mic-E status text. The status text may be any length that
> fits in the rest of the Information field.
>
> The Mic-E status text must not start with **‘**,**'** or 0x1d,
> otherwise it will be confused with telemetry data.
>
> It is possible to include a standard APRS-formatted position in the
> Mic-E status text field. A suitable position will cause the APRS
> display software to override any position data the Mic-E has encoded.
> This is useful if using a Mic-E without a GPS receiver.
>
> **Note**: The Kenwood radios automatically insert a special type code
> at the front of the status text string (i.e. in the 10th character of
> the Information field):
>
> Kenwood TH-D7: **&gt;**
>
> Kenwood TM-D700: **\]**
>
> These characters should not be confused with the APRS Data Type
> Identifier that appears at the start of reports.
>
> It is envisaged that other Mic-E-compatible devices will be allocated
> their own type codes in future.
>
> **Note**: When Kenwood radios receive the status, they can only
> display a small number of text characters:
>
> Kenwood TH-D7: 20 characters Kenwood TM-D700: 28 characters
>
> **Note**: The Kenwood TM-D700 radio uses the ' (apostrophe) instead of
> the ‘ (grave) APRS Data Type Identifier to represent current GPS data.
> A suggested way of detecting this situation is to examine the first
> and 10th characters of the Information field; if they are ' and **\]**
> respectively, then the packet is almost certainly from a TM-D700.
>
> **Maidenhead Locator in the Mic-E Status Text Field**
>
> The Mic-E status text field can contain a Maidenhead locator.
>
> If the locator is followed by a plain text comment, the first
> character of the text _must_ be a space. For example:
>
> IO91SX/G**** Helloworld (from a Mic-E or PIC-E)
>
> &gt;IO91SX/G**** Helloworld (from a Kenwood TH-D7)
>
> \]IO91SX/G**** Helloworld (from a Kenwood TM-D700) (**/G** is the
> grid locator symbol).
>
> **Altitude in the Mic-E Status Text**

**Field**

> The Mic-E status text field can contain the station’s altitude. The
> altitude is expressed in the form xxx**}**, where xxx is in meters
> relative to 10km below mean sea level (the deepest ocean), to base 91.
>
> For example, to compute the xxx characters for an altitude of 200
> feet: 200 feet = 61 meters = 10061 meters relative to the datum
>
> 10061 / 91<sup>2</sup> = **1**, remainder 1780
>
> 1780 / 91 = **19**, remainder **51**
>
> Adding 33 to each of the highlighted values gives 34, 52 and 84 for
> the ASCII codes of xxx.
>
> Thus the 4-character altitude string is **"4T}**
>
> Some examples:
>
> "4T}
>
> &gt;"4T}
>
> \]"4T}
>
> Optional altitude should be _first_ after the Mic-E **type** byte.
>
> **Mic-E Data in Non-APRS**
>
> **Networks**
>
> Some parts of the Mic-E AX.25 Information field may contain binary
> data (i.e. non-printable ASCII characters). If such a packet is
> constrained to the APRS network, this should not cause any
> difficulties.
>
> If, however, the packet is to be forwarded via a network that does not
> reliably preserve binary data (e.g. the Internet), then it is
> necessary to convert the data to a format that will preserve it.
>
> Further, if the packet subsequently re-emerges back onto the APRS
> network, it will then be necessary to re-convert the data back to its
> original format.

# OBJECT AND ITEM REPORTS

> **Objects and Items** Any APRS station can manually report the
> position of an APRS entity (e.g. another station or a weather
> phenomenon). This is intended for situations where the entity is not
> capable of reporting its own position.
>
> APRS provides two types of report to support this:

- Object Reports

- Item Reports

> Object Reports specify an Object’s position, can have an optional
> timestamp, and can include course/speed information or other Extended
> Data. Object Reports are intended primarily for plotting the positions
> of moving objects (e.g. spacecraft, storms, marathon runners without
> trackers).
>
> Item Reports specify an Item’s position, but cannot have a timestamp.
> While Item reports may also include course/speed or other Extended
> Data, they are really intended for inanimate things that are
> occasionally posted on a map (e.g. marathon checkpoints or first-aid
> posts). Otherwise they are handled in the same way as Object Reports.
>
> Objects are distinguished from each other by having different Object
> names. Similarly, Items are distinguished from each other by having
> different Item names.
>
> <u>Implementation Recommendation</u>: When an APRS Object/Item is
> displayed on the screen, the callsign of the station sending the
> report should be associated with the Object/Item.
>
> **Replacing an Object / Item**
>
> A fundamental precept of APRS is that any station may take over the
> reporting responsibility for an APRS Object or Item, by simply
> transmitting a new report with the same Object/Item name.
>
> The replacement report may specify the existing location or a new
> location.
>
> The original station will cease transmitting an Object/Item Report
> when it sees an incoming report with the same name from another
> station.
>
> **Killing an Object / Item**
>
> To kill an Object/Item, a station transmits a new Object/Item Report,
> with a “kill” character following the Object/Item name.
>
> <u>Implementation Recommendation</u>: When an Object/Item is killed it
> should be removed from display on the screen. However, the data
> associated with the Object/Item should be retained internally in case
> it is needed later.

**Object Report**

**Format**

> An Object Report has a _fixed_ 9-character Object name field, which
> may consist of any printable ASCII characters, including embedded
> spaces. Trailing spaces are used to make the field 9 characters wide.
> These are not part of the name.
>
> Object names are case-sensitive.
>
> The ; is the APRS Data Type Identifier for an Object Report, and a
> **\*** or **\_**
>
> separates the Object name from the rest of the report:
>
> **\*** indicates a live Object.
>
> **\_** indicates a killed Object.

- The position may be in lat/long or compressed lat/long format, and
  the report may also contain Extended Data. Compressed Objects are
  not recommended for use on RF due to incompatibilities. For 1 foot
  precision, use the !DAO! format.

> An Object always has a timestamp.
>
> The Comment field may contain any appropriate APRS data (see the
> _Comment Field_ section in Chapter 5: APRS Data in the AX.25
> Information Field).
>
> Bytes:
>
> Bytes:

**Item Report**

**Format**

> An Item Report has a _variable-length_ Item name, 3–9 characters long.
> The name may consist of any printable ASCII characters, including
> embedded space, _except_ **!** or **\_** because they are the field
> terminator.
>
> Item names are case-sensitive.
>
> Note that the name field is variable length, rather than fixed, as
> with objects.
>
> The **)** is the APRS Data Type Identifier for an Item Report, and a
> **!** or **\_**
>
> separates the Item name from the rest of the report:
>
> **!** indicates a live Item.
>
> **\_** is the Item “kill” character.
>
> The position may be in lat/long or compressed lat/long format. There
> is no provision for a timestamp. The report may also contain Extended
> Data.
>
> The Comment field may contain any appropriate APRS data (see the
> _Comment Field_ section in Chapter 5: APRS Data in the AX.25
> Information Field).
>
> APRS 1.1: The Item Format is not recommended on RF due to
> incompatibilities.
>
> Bytes:
>
> Bytes:
>
> **Area Objects** Using the **\l** symbol (i.e. the lower-case letter
> “L” symbol from the Alternate Symbol Table) it is possible to define
> circle, line, ellipse, triangle and box objects in all colors, either
> open or filled in, any size from 60 feet to 100 miles.
>
> These Objects are useful for real-time events such as for a
> search-and-rescue, or adding a special road or route for a special
> event.
>
> The Object format is specified as a 7-character APRS Data Extension
>
> Tyy/Cxx immediately following the **l** Symbol Code. For example:
>
> ;OBJECT\*ddmm.hhN**\\**dddmm.hhW **l** Tyy/Cxx
>
> where:
>
> T is the type of object shape.
>
> /C is the color of the object.
>
> yy is the square root of (the latitude offset in degrees / 1500).
>
> xx is the square root of (the longitude offset in degrees / 1500).

On the receiving end:

offset_in_degrees = (offset_in_packet^2) / 1500

> NOTE: This specification originally had a scaling factor of 100.
> <http://www.aprs.org/aprs11/areaobjects.txt> contains the correction
> to make it 1500. All of the modern applications checked use 1500.
>
> The object type and color codes are as follows:

- Triangles are always isosceles triangles, oriented vertically and
  the points of reference are the top APEX and the lower right corner.

- Circles are defined by a square box referenced by the upper left
  corner and the center.

- Lines are referenced to the upper end.

> The 'offset reference' position of the object is the upper left corner
>
> of the object and the offsets are the distance from the lower right
>
> corner (or center of a circle) back to this "offset reference"
> position.
>
> (An exception is the special case of a type-6 line which is drawn down
>
> and to the left).
>
> Here are some examples of Object Position Reports. The latitude and
> longitude offsets are each 4 minutes = 4/60 degree = 100/1500 of a
> degree), so
>
> yy = xx = √100 = 10.
>
> ;SEARCH\*092345z4903.50N**\\**07201.75W **l 710/310**
>
> A high intensity cyan filled ellipse, yy=10, xx=10
>
> ;SEARCH\*092345z4903.50N**\\**07201.75W **l 8101310**
>
> A low intensity violet filled triangle, yy=10, xx=10
>
> Further, with the line option (Type 1 and Type 6) it is possible to
> specify a “corridor” either side of the central line. The width of the
> corridor (in miles) either side of the line is specified in the
> comment text, enclosed by **{}**.
>
> For example:
>
> ;FLIGHTPTH\*4903.50N**\\**07201.75W **l 610/310{100}**
>
> A high intensity cyan line, with a 100-mile corridor either side
>
> **Note**: The color fill option should be used with care, since a
> color-filled object will obscure information displayed underneath it.
>
> **Signpost Objects/Items**
>
> Signpost Objects/Items (with the symbol **\m**) display as a yellow
> box with a 1–3-character overlay on them. The overlay is specified by
> enclosing the 1–3 characters in braces in the comment field. Thus a
> signpost with {55} would appear as a sign with **55** on it.
>
> For example:
>
> )I913N!4903.50N**\\**07201.75W**m{55}**
>
> This was originally designed for posting the speed of traffic past
> speed measuring devices, but can be used for any purpose.
>
> <u>Implementation Recommendation</u>: Signposts should not display any
> callsign or name, and to avoid clutter should only be displayed at
> close range.

**Obsolete Object**

**Format**

> Some stations transmit Object reports without the **;** APRS Data Type
> Identifier. This format is obsolete. Some software may still decode
> such data as an Object, but it should now be interpreted as a Status
> Report.

# WEATHER REPORTS

## Weather Report

## Types

> APRS is an ideal tool for reporting weather conditions via packet.
> APRS is also ideally suited for the Skywarn weather observer
> initiative.
>
> APRS supports three types of Weather Report:

- Raw Weather Report (not recommended - sending system should reformat
  data into Complete Weather Report format)

- Positionless Weather Report (not recommended - sending system should
  reformat data into Complete Weather Report format)

- Complete Weather Report

## Data Type Identifiers

> The following APRS Data Type Identifiers are used in Weather Reports
> containing raw data:
>
> **!** Ultimeter 2000
>
> **\#** Peet Bros U-II
>
> **$** Ultimeter 2000
>
> **\*** Peet Bros U-II
>
> **\_** Positionless weather data
>
> In addition, where the raw data has been post-processed (for example,
> by the insertion of station location information), the four position
> Data Type Identifiers **!**, **=**, **/** and **@** may be used
> instead. In this case, the Weather Report is identified with the
> weather symbol **/\_** or **\\** in the APRS Data.

## Raw Weather

## Reports

> Raw weather data from a stand-alone weather station is contained in
> the Information Field of an APRS AX.25 frame. **Raw Weather Formats
> are not recommended**. Microprocessors should convert to _complete_
> format on RF.
>
> Bytes:

## Positionless Weather Reports

> Generic raw weather data from a stand-alone weather station is
> contained in the Information Field of an APRS AX.25 frame. The WinAPRS
> "positionless" weather format is not recommended.
>
> Bytes:

## APRS Software

## Type

> A Weather Report may contain a single-character code S for the type of
> APRS software that is running at the weather station:
>
> **d** = APRSdos
>
> **M** = MacAPRS
>
> **P** = pocketAPRS
>
> **S** = APRS+SA
>
> **W** = WinAPRS
>
> **X** = X-APRS (Linux)

## Weather Unit

## Type

> A Weather Report may contain a 2–4 character code uuuu for the type of
> weather station unit. The following codes have been allocated:
>
> = Davis
>
> = Heathkit
>
> = PIC device
>
> = Radio Shack
>
> = Original Ultimeter U-II (auto mode)
>
> = Original Ultimeter U-II (remote mode)
>
> = Ultimeter 500/2000
>
> = Remote Ultimeter logger
>
> = Ultimeter 500
>
> = Remote Ultimeter packet mode

O = Otracker

K = Kenwood

B = Byonics

Y = Yaesu

> Users may specify any other 2–4 character code for devices not in this
> list.
>
> **Positionless Weather Data**
>
> The format of weather data within a Positionless Weather Report
> differs according to the type of weather station unit, but generically
> consists of some or all of the following elements:
>
> Bytes:
>
> where: **c** = wind direction (in degrees).
>
> **s** = sustained one-minute wind speed (in mph).
>
> **g** = gust (peak wind speed in mph in the last 5 minutes).
>
> **t** = temperature (in degrees Fahrenheit). Temperatures below zero
> are expressed as -01 to -99.
>
> **r** = rainfall (in hundredths of an inch) in the last hour.
>
> **p** = rainfall (in hundredths of an inch) in the last 24 hours.
>
> **P** = rainfall (in hundredths of an inch) since midnight.
>
> **h** = humidity (in %. 00 = 100%).
>
> **b** = barometric pressure (in tenths of millibars/tenths of
> hPascal).
>
> Other parameters that are available on some weather station units
> include:
>
> **L** = luminosity (in watts per square meter) 000 to 999.
>
> **l** (lower-case letter “L”) = luminosity (in watts per square meter)
> 1000 and above. (Actual value is 1000 more than 3 digit number.)
>
> (L is inserted in place of one of the rain values).
>
> **s** = snowfall (in inches) in the last 24 hours. 3 digits.
>
> A decimal point is allowed for non-integer values.
>
> **\#** = raw rain counter
>
> **Note**: The weather report must include at least the MDHM
> date/timestamp, wind direction, wind speed, gust and temperature, but
> the remaining parameters may be in a different order (or may not even
> exist).
>
> **Note**: Where an item of weather data is unknown or irrelevant, its
> value may be expressed as a series of dots or spaces. For example, if
> there is no wind speed/direction/gust sensor, the wind values could be
> expressed as:
>
> **c...s...g...** or **csg**
>
> For example, Jim’s rain gauge may produce a report like this:
>
> \_10090556c...s...g...t...P012Jim
>
> (The date/timestamp, wind direction/speed/gust and temperature
> parameters must be included, even though they are not meaningful).
>
> **Location of a Raw and Positionless Weather Stations**
>
> APRS cannot display weather data on a map until it knows the location
> of the sending station. In the case of a station transmitting Raw or
> Positionless Weather Reports, the station has to occasionally send an
> additional packet containing its position (using any of the legal
> lat/long and compressed lat/long position formats described earlier).
>
> **Raw Weather Formats** not recommended. Microprocessors should
> convert to _complete_ format on RF.
>
> **Symbols with Raw and Positionless Weather Stations**
>
> Because Raw and Positionless Weather Reports do not contain a display
> symbol in the AX.25 Information field, it is possible to specify the
> symbol in a generic APRS destination address (e.g. GPSHW or GPSE63)
> instead.
>
> Alternatively, if the weather station is on a balloon, the SSID –11
> may be used in the source address (e.g. N0QBF-11).
>
> **Raw Weather Formats** not recommended. Microprocessors should
> convert to _complete_ format on RF.
>
> See Chapter 20: APRS Symbols for more detail on the usage of symbols.

**Complete Weather**

> **Reports with Timestamp and**

**Position**

> An APRS Complete Weather Report can contain a timestamp and location
> information, using any of the legal lat/long and compressed lat/long
> position formats described earlier. An APRS Object may also have
> weather information associated with it.
>
> Examples of report formats are shown below. Note that the Symbol Code
> in every case is the **\_** (underscore). Also, the 7-byte Wind
> Direction and Wind Speed Data Extension replace the **c**ccc and
> **s**sss fields of a Positionless Weather Report.

FIXME: underscore becomes space in examples

> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> **Storm Data** APRS reports can contain data relating to tropical
> storms, hurricanes and tropical depressions. The format of the data is
> as follows:
>
> Bytes:
>
> where: ST = **TS** (Tropical Storm)
>
> **HC** (Hurricane)
>
> **TD** (Tropical Depression).
>
> www = sustained wind speed (in knots).
>
> GGG = gust (peak wind speed in knots).
>
> pppp = central pressure (in millibars/hPascal)
>
> RRR = radius of hurricane winds (in nautical miles). rrr = radius of
> tropical storm winds (in nautical miles). ggg = radius of “whole gale”
> (i.e. 50 knot) winds (in
>
> nautical miles). Optional.
>
> Storm data will usually be included in an Object Report, but may also
> be included in a Position Report or an Item Report.
>
> The display symbol will be either:
>
> **\\** Hurricane/Tropical Storm (current position)
>
> **/@** Hurricane (predicted future position)
>
> For example, the progress of Hurricane Brenda could be expressed in
> Object Reports like these:
>
> ;BRENDA\*092345z4903.50N**\\**07202.75W**@**088/036/HC/150^200/0980&gt;090&030%040
>
> ;BRENDA\*100045z4905.50N**/**07201.75W**@**101/047/HC/104^123/0980&gt;065&020%040
>
> **National Weather Service Bulletins**
>
> APRS supports the dissemination of National Weather Service bulletins.
> See Chapter 14: Messages, Bulletins and Announcements.

# TELEMETRY DATA

**Telemetry Report**

**Format**

> The AX.25 Information field can contain telemetry data. The APRS Data
> Type Identifier is **T**.
>
> The report Sequence Number is a 3-character value — typically a
> 3-digit number, or the three letters **MIC**. In the case of **MIC**,
> there may or may not be a comma preceding the first analog data value.
>
> There are five 8-bit unsigned analog data values (expressed as 3-digit
> decimal numbers in the range 000–255), followed by a single 8-bit
> digital data value (expressed as 8 bytes, each containing **1** or
> **0**).
>
> The Kantronics KPC-3+ TNC and [APRS Micro Interface Module
> (MIM)](http://www.aprs.org/mic-lite.html) use this format.
>
> In reality, the restriction of fixed width 3 digits is often ignored.
> It is common to see much longer variable width values including
> decimal points. Most modern applications recognize this relaxed
> format. Others are encouraged to do the same.
>
> Bytes:

**On-Air Definition of**

> **Telemetry Parameters**
>
> In principle, received telemetry data may be interpreted in any
> appropriate way. In practice, however, an APRS user can define the
> telemetry parameters (such as quadratic coefficients for the analog
> values, or the meaning of the binary data) at any time, and then send
> these definitions as APRS messages. Other stations receiving these
> messages will then know how to interpret the data.
>
> This is achieved by sending four messages:

- A Parameter Name message.

- A Unit/Label message.

- An Equation Coefficients message.

- A Bit Sense/Project Name message.

> The messages addressee is the callsign of the station transmitting the
> telemetry data. For example, if N0QBF launches a balloon with the
> callsign N0QBF-11, then the four messages are addressed to N0QBF-11.
>
> See Chapter 14: Messages, Bulletins and Announcements for full details
> of message formats.

**Parameter Name**

**Message**

> The Parameter Name message contains the names (N) associated with the
> five analog channels and the 8 digital channels. Its format is as
> follows:
>
> Bytes:
>
> **Historical Note**: The field widths are not all the same (this is a
> legacy arising from earlier limitations in display screen width). Note
> also that the byte counts _include_ the comma separators where shown.
>
> The list can terminate after any field.
>
> Compatibility note: Many implementations ignore these overly
> restrictive inconsistent name lengths. It is not uncommon to see names
> with a dozen characters or more. New/upgraded applications should
> handle what is in common use.
>
> **Unit/Label Message** The Unit/Label message specifies the units (U)
> for the analog values, and the labels (L) associated with the digital
> channels:
>
> Bytes:
>
> **Historical Note**: Again, the field widths are not all the same, and
> the byte counts
>
> _include_ the comma separators where shown. The list can terminate
> after any field.
>
> Compatibility note: Many implementations ignore these overly
> restrictive inconsistent unit lengths. It is not uncommon to see much
> longer unit descriptions. New/upgraded applications should handle what
> is in common use.
>
> **Equation Coefficients Message**
>
> The Equation Coefficients message contains three coefficients (a, b
> and c) for each of the five analog channels.
>
> Bytes:
>
> To obtain the final value of an analog channel, these coefficients are
> substituted into the equation:
>
> a x v<sup>2</sup> + b x v + c
>
> where v is the raw received analog value.
>
> For example, analog channel A1 in the above beacon examples relates to
> the battery voltage, expressed in hundredths of volts, and a = 0, b =
> 5.2, c = 0. If the raw received value v is 199, then the voltage is
> calculated as:
>
> voltage = 0 x 199<sup>2</sup> + 5.2 x 199 + 0
>
> = 1034.8 hundredths of a volt
>
> = 10.348 volts
>
> **Bit Sense/ Project Name**

**Message**

> The Bit Sense/Project Name message contains two types of information:

- An 8-bit pattern of ones and zeros, specifying the sense of each
  digital channel that matches the corresponding label.

- The name of the project associated with the telemetry station.

> Bytes:
>
> Thus in the above message examples, if digital channel B1 is 1, this
> indicates the camera has clicked. If channel B2 is 0, the parachute
> has opened, and so on.

# MESSAGES, BULLETINS AND ANNOUNCEMENTS

> APRS messages, bulletins and announcements are packets containing free
> format text strings, and are intended to convey human-readable
> information. A message is intended for reception by a single specified
> recipient, and an acknowledgement is usually expected. Bulletins and
> announcements are intended for reception by multiple recipients, and
> are not acknowledged.
>
> **Messages** An APRS message is a text string with a specified
> addressee. The addressee is a fixed 9-character field (padded with
> spaces if necessary) following the **:** Data Type Identifier. The
> addressee field is followed by another **:**, then the text of the
> message.
>
> The message text may be up to 67 characters long, and may contain any
> printable ASCII characters except **|**, **~** or **{**.
>
> A message may also have an optional message identifier, which is
> appended to the message text. The message identifier consists of the
> character **{** followed by a message number (up to 5 alphanumeric
> characters, no spaces) to identify the message.
>
> Messages _without_ a message identifier are not to be acknowledged.
>
> Messages _with_ a message identifier are intended to be acknowledged
> by the addressee. The sending station will repeatedly send the message
> until it receives an acknowledgement, or it is canceled, or it times
> out.
>
> Bytes:

## Message Acknowledgement

Bytes:

> A message acknowledgement is similar to a message, except that the
> message text field contains just the letters **ack**, and this is
> followed by the Message Number being acknowledged.
>
> A station should respond to a message only when it is the addressee.
>
> **Message Rejection** If a station is unable to accept a message,
> addressed to itselft, it can send a **rej** message instead of an
> **ack** message:
>
> Bytes:
>
> A station should respond to a message only when it is the addressee.

## Multiple Acknowledgements

> If a station receives a particular message more than once, it will
> respond with an acknowledgement for each instance of the message.
>
> If a station receives a message over a long path, it may respond with
> a reasonable number of multiple copies of the acknowledgement, to
> improve the chances of the originating station receiving at least one
> of the copies.
>
> In either of these two situations, multiple message acknowledgements
> should be separated by at least 30 seconds (this is because some
> network components such as digipeaters will suppress duplicated
> messages within a 30-second period).

## New Message

## Number Format

## (Dec. 1999)

> The original message number format was 1 to 5 alphanumeric characters.
> A newer format allows more efficient conversation between two stations
> by including an ack within an outgoing message.
>
> This is 100% backwards compatible with all code. The REPLY ACKS are
> embedded in the 5 digit line number. Old code doesn't care.
>
> The format for the line number for outgoing message numbers is
> "{MM}AA" where MM is the outgoing message number and AA is the "free
> ACK" if needed. If no ACK is pending, then the message \# is "{MM}".
>
> The non-base91 character "}" was chosen as the separator so that there
> is no chance that an existing line number could be misinterpreted.
> Notice, that even if there is no ACK, the presence of the trailing "}"
> tells the other end that the sender is REPLY-ACK capable and can
> accept a REPLY-ACK.
>
> This is actually very simple to implement, but it does affect several
> areas of your code:

1.  Send all MSG numbers in the new {MM}AA format. AA is null if none.
    But AA is only appended at the INSTANT of transmission. For the same
    MM message, the AA must always represent the "latest" outstanding
    ACK for that station. It may change at future retries...

2.  RECEIVE messages and look for AA. IF AA matches the MM of one of
    your outgoing messages, then consider that message ACKED.

3.  RECEIVE messages and Strip off the AA from the end of the message
    text before you store it. THis is necessary so that your DUPE
    checker will still work. Since the AA may be different for multiple
    copies of the same incoming MM...

4.  When you receive a message line XXX.., send the normal existing
    "ackXXX..". This algoirithm is unchanged. Even if XXX.. is MM}AA
    then the ack is just the exact copy as before "ackMM}AA".

5.  When you receive a message line MM}AA, do \#4 above as normal, but
    also then save the incoming MM number as now it is your "latest ACK"
    for that station. Now then this becomes the AA REPLY-ACK number for
    your next outgoing message to that station. (Step \#1 above).

6.  Note, that in \#1, above, that when the user prepares each message
    line, that it is buffered up with only the {MM} line number on the
    end. The AA (if pending) is not attached until the instant that
    packet is transmitted.

7.  Thus, in step \#2, when you get a new REPLY-ACK, then you simply
    match up the "AA" with the {MM} in your outgoing message queue. But
    if you get the old "ackMM}AA" ack, then you must pull out the "MM"
    here and use IT to match with the outstanding "{MM}" in your
    outgoing message queue.

> **Message Groups** An APRS receiving station can specify special
> Message Groups, containing lists of callsigns that the station will
> read messages from (in addition to messages addressed to itself). Such
> Message Groups are defined internally by the user at the receiving
> station, and are used to filter received message traffic.
>
> The receiving station will read all messages with the Addressee field
> set to
>
> ALL, QST or CQ.
>
> The receiving station will only acknowledge messages addressed to
> itself, and not any messages received which were addressed to any
> group callsign.
>
> **Note**: The receiving station will acknowledge all messages
> addressed to itself, even if it is operating in an Alternate Net (see
> Chapter 4: APRS Data in the AX.25 Destination and Source Address
> Fields).
>
> **General Bulletins** General bulletins are messages where the
> addressee consists of the letters **BLN** followed by a single-_digit_
> bulletin identifier, followed by 5 filler spaces. General bulletins
> are generally transmitted a few times an hour for a few hours, and
> typically contain time sensitive information (such as weather status).
>
> Bulletin text may be up to 67 characters long, and may contain any
> printable ASCII characters except **|** or **~**.
>
> Bytes:
>
> **Announcements** Announcements are similar to general bulletins,
> except that the letters **BLN**
>
> are followed by a single upper-case _letter_ announcement identifier.
> Announcements are transmitted much less frequently than bulletins (but
> perhaps for several days), and although possibly timely in nature they
> are usually not time critical.
>
> Announcements are typically made for situations leading up to an
> event, in contrast to bulletins which are typically used within the
> event.
>
> Users should be alerted on arrival of a new bulletin or announcement.
>
> Bytes:
>
> Bytes:
>
> **Group Bulletins** Bulletins may be sent to _bulletin groups_. A
> bulletin group address consists of the letters **BLN**, followed by a
> single-_digit_ group bulletin identifier, followed in turn by the name
> of the group (up to 5 characters long, with filler spaces to pad the
> name to 5 characters).

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 7%" />
<col style="width: 15%" />
<col style="width: 11%" />
<col style="width: 3%" />
<col style="width: 58%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="6"><blockquote>
<p><em><strong>Group Bulletin Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>BLN</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Group Bulletin ID</strong></em></p>
<p>n</p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Group Name</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Group Bulletin Text (max 67 characters)</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>3</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>5</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0-67</p>
</blockquote></td>
</tr>
<tr class="odd">
<td colspan="6"><blockquote>
<p><u>Example</u></p>
</blockquote>
<p>:BLN4WX:Stand by your snowplows Group bulletin number 4 to the WX
group.</p>
<p>(Note the filler spaces in the group name).</p></td>
</tr>
</tbody>
</table>

> A receiving station can specify a list of bulletin groups of interest.
> The list is defined internally by the user at the receiving station.
> If a group is selected from the list, the station will only copy
> bulletins for that group, plus any general bulletins. If the list is
> empty, all bulletins are received and generate alerts.

## National Weather Service Bulletins

> Standard APRS message formats can be used for a variety of other
> applications. For example, in the United States, special formatted
> messages addressed to the generic callsign **NWS-**xxxxx are used to
> highlight map areas involved in weather warnings, using the following
> format:
>
> Bytes:
>
> More Information about APRS NSW Weather can be found at
> <https://www.aprs-is.net/wx/> .
>
> **NTS Radiograms** APRS can be used to transport NTS radiograms. This
> uses the existing APRS message format for backwards compatibility, by
> adding a 3-character NTS format identifier **N**x**\\** at the start
> of the APRS Message Text, as follows:
>
> **N#\\**number**\\**precedence**\\**handling**\\**originator**\\**check**\\**place**\\**time**\\**date
> **NA\\**address_line1**\\**address_line2**\\**address_line3**\\**address_line4
> **NP\\**phone number
>
> **N1\\**line 1 of NTS message text **N2\\**line 2 of NTS message text
> **N3\\**line 3 of NTS message text **N4\\**line 4 of NTS message text
> **N5\\**line 5 of NTS message text **N6\\**line 6 of NTS message text
> **NS\\**Signature block
>
> **NR\\**Received from**\\**date_time**\\**sent_to**\\**date_time
>
> All of these fields comes from the ARRL NTS Radiogram form and are
> described in the NTS handbook.
>
> Each message line is addressed to the same station.
>
> The **N#\\**, **NA\\** and **NR\\** lines are multiple fields combined
> for APRS transmission efficiency. The backslash separator is used so
> that conventional forward slashes may be embedded in messages. (The
> backslash does not exist in the RTTY or CW alphabets, so it therefore
> cannot appear in an NTS radiogram).
>
> Each line may be up 67 characters long, including the 3-character NTS
> format identifier. Lines in excess of 67 characters will be truncated.
>
> There is a maximum of 6 lines of NTS message text.
>
> **Note**: The **N#\\**, **NA\\**, **NS\\** and **NR\\** fields are
> required. The others are optional.
>
> Serialization of each line is handled by the normal APRS Message ID
>
> **{**xxxxx.
>
> An APRS application is not required to understand or generate these
> messages. The information can be read and understood in the normal
> message display.

## Obsolete Bulletin and Announcement

## Format

> Some stations transmit bulletins and announcements without the **:**
> APRS Data Type Identifier. This format is obsolete. Some software may
> still decode such data as a bulletin or announcement, but it should
> now be interpreted as a Status Report.

## Bulletin and Announcement Implementation Recommendations

> Bulletins and announcements are seen as a way for all participants in
> an event/emergency/net to see all common information posted to the
> group. In this sense they are visualized as a mountain-top billboard
> or a bulletin board on the wall of an Emergency Operations Control
> Center.
>
> Information that everyone must see is to be posted there. Information
> is added and removed. Space is limited. Only so much information can
> be posted before it becomes too busy for anyone to see everything.
> Thus things are supposed to be posted, updated, and cleared to keep
> the big billboard uncluttered and current with what everyone needs to
> know at the present time. It should not be cluttered with obsolete
> information.
>
> This can be implemented in an APRS display system as a “Bulletin
> Screen”. Everyone has this screen, and anyone can post or update lines
> on this screen. At any instant, everyone in the network sees exactly
> the same screen.
>
> Everything is arranged and displayed in exactly the same way. Thus,
> everyone, everywhere is looking at the same mountain-top billboard or
> bulletin board. There is no ambiguity as to who sees what information,
> in what order at what time.
>
> To make this work, a number of issues should be considered:

- **Sorting**: Bulletins/Announcements are almost always multi-line,
  and may arrive out of sequence. They must be sorted before
  presentation on the Bulletin Screen, and re-sorted if necessary when
  each new line arrives. This includes sorting by originating callsign
  and Bulletin/ Announcement ID.

- **Replacement**: Stations sending bulletins/announcements can send
  new lines to replace lines sent earlier, re-using the original
  Bulletin/ Announcement IDs. (Note: It is only necessary to re-send
  replacement lines. It is not necessary to re-send the whole
  bulletin/announcement). Receipt of a new line with the same
  Bulletin/Announcement ID as one already received from the same
  station should result in the existing line being overwritten
  (replaced).

- **Clearing**: A user should be able to clear any or all of the
  bulletins/ announcements from the Bulletin Screen once he has read
  them. Any bulletins/announcements that are still valid will
  re-appear in due course because of the way they are redundantly
  re-transmitted.

- **Alerts**: On receipt of any new or replacement line for the
  Bulletin Screen, an alarm should be sounded and re-sounded
  periodically until the user acknowledges it. Thus, this vital
  information is “pushed” to the operator. There is no excuse for not
  being aware of the current bulletin/ announcement state — this is
  important in the hurried and crisis-laden scenario of an APRS event.

- **Logging**: Old bulletins/announcements should be logged in
  sequential APRS log files in case they are subsequently needed.

# STATION CAPABILITIES, QUERIES AND RESPONSES

> **Station Capabilities** A station may define a set of one or more
> attributes of the station, known as Station Capabilities. The station
> transmits its capabilities in response to an IGATE query (see below),
> using the **&lt;** Data Type Identifier.
>
> Each capability is a TOKEN or a TOKEN=VALUE pair. More than one
> capability may be on a line, with each capability separated by a
> comma.
>
> Currently defined capabilities include:
>
> IGATE,MSG_CNT=n,LOC_CNT=n
>
> where IGATE defines the station as an IGate, MSG_CNT is the number of
> messages transmitted, and LOC_CNT is the number of “local” stations
> (those to which the IGate will pass messages in the local RF network).

## Queries and Responses

> There are two types of APRS queries. One is general to all stations
> and the other is in a message format directed to a single individual
> station.
>
> Queries always begin with a **?**, are one-time transmissions, do not
> have a message identifier and should not be acknowledged. Similarly
> the responses to queries are one-time transmissions that also do not
> have a message identifier, so that they too are not acknowledged.
>
> Each query contains a Query Type (in upper-case). The following Query
> Types and expected responses are supported:

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 41%" />
<col style="width: 44%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>Query Type</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Query</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Response</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>APRS</strong></p>
</blockquote></td>
<td><blockquote>
<p>General — All stations query</p>
</blockquote></td>
<td><blockquote>
<p>Station’s position and status</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>APRSD</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for stations heard direct</p>
</blockquote></td>
<td><blockquote>
<p>List of stations heard direct</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>APRSH</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query if an individual station has heard a particular
station</p>
</blockquote></td>
<td><blockquote>
<p>Position of heard station as an APRS Object, plus heard statistics
for the last 8 hours</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>APRSM</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for outstanding unacknowledged
or undelivered messages</p>
</blockquote></td>
<td><blockquote>
<p>All outstanding messages for the querying station</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>APRSO</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for its Objects</p>
</blockquote></td>
<td><blockquote>
<p>Station’s Objects</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>APRSP</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for its position</p>
</blockquote></td>
<td><blockquote>
<p>Station’s position</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>APRSS</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for its status</p>
</blockquote></td>
<td><blockquote>
<p>Station’s status</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>APRST</strong> or</p>
<p><strong>PING?</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for a trace (i.e. path by
which the packet was heard)</p>
</blockquote></td>
<td><blockquote>
<p>Route trace</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>IGATE</strong></p>
</blockquote></td>
<td><blockquote>
<p>General — Query all Internet Gateways</p>
</blockquote></td>
<td><blockquote>
<p>IGate station capabilities</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>WX</strong></p>
</blockquote></td>
<td><blockquote>
<p>General — Query all weather stations</p>
</blockquote></td>
<td><blockquote>
<p>Weather report (and the station’s position if it is not included in
the Weather Report)</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:
>
> If a queried station has no relevant information to include in a
> response, it need not respond.
>
> A queried station should ignore any query that it does not recognize.

**General Queries** The format of a general query is as follows:

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 7%" />
<col style="width: 3%" />
<col style="width: 6%" />
<col style="width: 3%" />
<col style="width: 7%" />
<col style="width: 3%" />
<col style="width: 9%" />
<col style="width: 55%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="8"><blockquote>
<p><em><strong>General Query Format</strong></em></p>
</blockquote></th>
<th rowspan="4"></th>
</tr>
<tr class="odd">
<th rowspan="2"><blockquote>
<p><strong>?</strong></p>
</blockquote></th>
<th rowspan="2"><blockquote>
<p><em><strong>Query Type</strong></em></p>
</blockquote></th>
<th rowspan="2"><blockquote>
<p><strong>?</strong></p>
</blockquote></th>
<th colspan="5"><blockquote>
<p><em><strong>Target Footprint</strong></em></p>
</blockquote></th>
</tr>
<tr class="header">
<th><blockquote>
<p><em><strong>Lat</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><strong>,</strong></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Long</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><strong>,</strong></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Radius</strong></em></p>
</blockquote></th>
</tr>
<tr class="odd">
<th><blockquote>
<p>1</p>
</blockquote></th>
<th><blockquote>
<p>n</p>
</blockquote></th>
<th><blockquote>
<p>1</p>
</blockquote></th>
<th><blockquote>
<p>n</p>
</blockquote></th>
<th><blockquote>
<p>1</p>
</blockquote></th>
<th><blockquote>
<p>n</p>
</blockquote></th>
<th><blockquote>
<p>1</p>
</blockquote></th>
<th><blockquote>
<p>4</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td colspan="9"><blockquote>
<p><u>Examples</u></p>
<p><u>Query</u> <u>Typical Response</u></p>
<p>?APRS? /092345z4903.50N/07201.75W&gt;</p>
<p>General query, with standard posit and status reply. &gt;092345zNet
Control Center</p>
<p>?APRS?<strong></strong>34.02,-117.15,0200 /3402.78N11714.02W-</p>
<p>General query for stations within a target footprint &gt;Digi on low
power</p>
<p>of radius 200 miles centered on 34.02 degrees north, 117.15 degrees
west, with standard posit and status reply. (Note the leading space in
the latitude, as its value is positive, see below).</p>
<p>?IGATE? &lt;IGATE,MSG_CNT=43,LOC_CNT=14</p>
<p>General query for IGate stations, with a Station Capabilities
reply.</p>
<p>?WX? _10090556c220s004g005t077…</p>
<p>Query for weather stations, with a standard
/090556z4903.50N/07201.75W&gt;</p>
<p>Weather Report reply (without a position), followed by a standard
posit.</p>
</blockquote></td>
</tr>
</tbody>
</table>

> In the case of an ?APRS? query for stations within a particular target
> footprint, the latitude and longitude parameters are in _floating
> point_ degrees (_not_ in APRS lat/long position format).

- North and east coordinates are positive values, indicated by a
  leading ****

> (space).

- South and west coordinates are negative values.

- The radius of the footprint is in miles, expressed as a fixed
  4-digit number in whole miles.

> All stations inside the specified coverage circle should respond with
> a Position Report and a Status Report.

## Directed Station

## Queries

> Queries addressed to individual stations are in APRS message format
> (except that they never include a message identifier). The addressee
> is the callsign of the station being queried.
>
> The message text is the Query Type. This is followed optionally by
> another callsign — this callsign does not need filler spaces as it is
> at the end of the data.
>
> Bytes:

# STATUS REPORTS

> A Status Report announces the station’s current mission or any other
> single line status to everyone. The report is contained in the AX.25
> Information field, and starts with the **&gt;** APRS Data Type
> Identifier.
>
> The report may optionally contain a timestamp.
>
> **Note**: The timestamp can _only_ be in DHM _zulu_ format.
>
> The status text occupies the rest of the Information field, and may be
> up to 62 characters long (if there is no timestamp in the report) or
> 55 characters (if there is a timestamp). The text may contain any
> printable ASCII characters except **|** or **~**.
>
> Bytes:
>
> Although the status will usually be plain language text, there are two
> cases where the report can contain special information which can be
> decoded:

- Beam Heading and Power

- Maidenhead grid locator

## Status Report with Beam Heading and Effective Radiated

## Power

> It is useful to include beam heading and ERP in packets in meteor
> scatter work. To keep packets as short as possible, these parameters
> are encoded into two characters, as follows:
>
> H = beam heading / 10
>
> (H=0–9 for 0–90 degrees, and A–Z for 100–350 degrees).
>
> P = ERP code.
>
> The HP value appears as the _last_ two characters of the status text,
> preceded by the **^** character — for example, **^B7** means a beam
> heading of 110 degrees and an ERP of 490 watts.
>
> The HP value may be combined with the Maidenhead grid locator (as
> described below), or with any other plain language status text.

## Status Report with Maidenhead Grid

## Locator

> The Maidenhead grid locator may be 4 or 6 characters long, and must
> immediately follow the **&gt;** Data Type Identifier.
>
> All letters must be transmitted in upper case. Letters may be received
> in upper case or lower case.
>
> The Symbol Table Identifier and Symbol Code follow the locator.
>
> If the report also contains status text, the first character of the
> text _must_ be a space.
>
> A Status Report with Maidenhead locator can not have a timestamp.
>
> Bytes:

## Transmitting Status

## Reports

> Each station should only transmit a Status Report once every net cycle
> time (i.e. once every 10, 20 or 30 minutes), or in response to a
> query.

# NETWORK TUNNELING AND THIRD-PARTY DIGIPEATING

## Third-Party Networks

> APRS provides a mechanism for formatting packets that are to be
> transported through third-party (i.e. non AX.25) networks, such as the
> Internet, an Ethernet LAN or a direct wire connection.
>
> These networks do not understand APRS source, destination and
> digipeater addresses, so it is necessary to send them as data, along
> with the original data being transmitted.

FIXME - could use some rewording

> **Source Path Header** Prior to sending an APRS packet into the
> third-party network, the APRS address path is prepended to the Data
> Type Identifier and the rest of the original data.
>
> The prepended address path is known as the Source Path Header. It
> consists of the source, destination and digipeater callsigns, with
> associated SSIDs.
>
> The main purpose of introducing the Source Path Header is to allow
> receiving stations on the far side of the third-party network to
> identify the sender — this is needed when acknowledging receipt of a
> message, for example. Knowledge of the source path is also useful in
> diagnosing network problems.
>
> Bytes:
>
> The Source Path Header must be in the “TNC-2” format.
>
> The APRS Working Group has agreed the AEA format is deprecated.

Bytes:

> The format of this headers is as follows:

<table style="width:100%;">
<colgroup>
<col style="width: 22%" />
<col style="width: 6%" />
<col style="width: 25%" />
<col style="width: 6%" />
<col style="width: 29%" />
<col style="width: 9%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="6"><blockquote>
<p><em><strong>Source Path Header — “TNC-2” Format</strong></em></p>
<p>An asterisk follows the digipeater callsign heard.</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="2"><blockquote>
<p><strong><em>Source Callsign</em>
(-</strong>SSID<strong>)</strong></p>
</blockquote></td>
<td rowspan="2"><blockquote>
<p><strong>&gt;</strong></p>
</blockquote></td>
<td rowspan="2"><blockquote>
<p><em><strong>Destination Callsign</strong></em></p>
<p><strong>(-</strong>SSID<strong>)</strong></p>
</blockquote></td>
<td colspan="2"><blockquote>
<p><em><strong>0-8 Digipeaters</strong></em></p>
</blockquote></td>
<td rowspan="2"><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Digipeater Callsign</strong></em></p>
<p><strong>(-</strong>SSID<strong>)(*)</strong></p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>1-9</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1-9</p>
</blockquote></td>
<td colspan="2"><blockquote>
<p>0-81</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
</tr>
<tr class="even">
<td colspan="6"><blockquote>
<p><u>Example</u></p>
<p>WB4APR-14&gt;APxxxx,WIDE1*,WIDE2-1:</p>
</blockquote>
<p>(WIDE2-1 digipeater “unused”)</p></td>
</tr>
</tbody>
</table>

> The SSID should not be displayed, in human readable format, if it is
> –0.
>
> The callsign of the digipeater from which the incoming packet was
> heard is indicated with an asterisk. No more than a single “\*” should
> appear. It implies that all preceding digipeaters listed have been
> used. No “\*” means source station was heard.
>
> Any digipeaters following the callsign of the station from which the
> packet was heard are termed “unused”. These unused digipeaters are
> stripped out when building a Third-Party Header (see below).
>
> **Third-Party Header** After a packet emerges from a third-party
> network, the receiving gateway station encapsulates it it (by
> inserting a **}** Third-Party Data Type Identifier and modifying the
> Source Path Header) before transmitting it on the local APRS network.
>
> The modified Source Path Header is called the Third-Party Header.
>
> Bytes:
>
> The Third-Party Header must be in “TNC-2” format.
>
> Bytes:
>
> The “unused” digipeater callsigns (i.e. those digipeater callsigns
> after the asterisk) in the original Source Path Header are stripped
> out. The asterisk itself is also stripped out of the Source Path
> Header.
>
> Then two additional callsigns are inserted:

- The Third-Party Network Identifier (e.g. TCPIP). This is a dummy
  “callsign” that identifies the nature of the third-party network.

- The callsign of the receiving gateway station, followed by an
  asterisk.

## Action on Receiving a Third-

## Party packet

> FIXME- rewrite When another station receives a third-party packet, it
> can extract the callsign of the original sending station from the
> Third-Party Header, if it is needed to acknowledge receipt of a
> message.
>
> The source address in the encapsulated third-party packet is a
> character string and does not need to adhere to the AX.25. It can
> contain 1 to 9 printable ASCII characters, other than “&gt;” which is
> the field terminator. Stations connected directly to APRS-IS often
> have names not following the AX.25 name restrictions.
>
> The other addresses in the Third-Party Header may be useful for
> network diagnostic purposes.

## An Example of Sending a Message through the Internet

> The Scenario: **FIXME - this needs help.**

- WB4APR-14 wants to send a message via the Internet to G3NRW.

- The nearest Internet gateway to WB4APR-14 is K4HG, reachable via a

> WIDE1-1,WIDE2-1 path.

- The nearest Internet gateway to G3NRW is G9RXG. The Process:

- In the normal way, WB4APR-14 builds a message packet that contains:

> **:G3NRW :Hi Ian{001**

- WB4APR-14 transmits the packet with his via path WIDE1-1,WIDE2-1.

- The Internet gateway K4HG happens to receive this packet from the
  first digipeater in the path.

- K4HG builds a new packet that contains the source path and the
  original message:

> **WB4APR-14&gt;APxxxx,WIDE1-1\*,WIDE2-1::G3NRW :Hi Ian{001**

- K4HG sends this packet (using telnet) to an APRServer on the
  Internet.

- All Internet gateways throughout the world that are connected to the
  APRServe network (including G9RXG) receive the packet.

- G9RXG converts the packet into a Third-Party packet:

> **}WB4APR-14&gt;APxxxx,WIDE1-1,TCPIP,G9RXG\*::G3NRW :Hi Ian{001**
>
> Note that the WIDE digipeater was stripped out of the header because
> it was unused.

- G9RXG transmits the packet over the local APRS network.

- G3NRW receives the packet, strips out the Third-Party Header, and
  discovers that the packet contains a message for him. From the
  header, G3NRW then establishes that the acknowledgement is to go
  back to WB4APR-14.

Work all of these in somehow:

- **NOGATE and RFONLY** in the RF DIGI field should not be forwarded
  into the APRS-IS by IGates.

- **!x!** means **no archive**. Any packet containing this string
  should not be archived by any of thes APRS-IS data bases. Positions,
  status or messages.

- [**APRS-IS Core**](http://core.aprs.net/) and
  [**Tier-2**](http://www.aprs2.net/) servers web pages and [**how
  they work**](http://www.aprs-is.net).

- [**The q-construct:**](http://www.aprs-is.net/q.htm) Marks the
  source of entry of all packets into the APRS-IS.

- **IGATE Status Report Format:** Left*bracket then *"IGate,
  MSG*CNT=N, LOC_CNT=N"*

# USER-DEFINED DATA FORMAT

> The APRS protocol defines many different data formats, but it cannot
> anticipate every possible data type that programmers may wish to send.
> The User-Defined data format is designed to fill these gaps. Under
> this system, program authors are free to send data in any format they
> choose.
>
> The data in the AX.25 Information field consists of a three-character
> header:
>
> **{** APRS Data Type Identifier.
>
> U A one-character User ID.
>
> X A one-character user-defined packet type.
>
> The APRS Working Group will issue User IDs to program authors who
> express a need.
>
> \[Keep in mind there is a limited number of available User IDs, so
> please do not request one unless you have a true need. The Working
> Group may require an explanation of your need prior to issuing a
> character. If only one or two data formats are needed, those may be
> issued from a User ID pool\].
>
> For experimentation, or prior to being issued a User ID, anyone may
> utilize the User ID character of **{** without prior notification or
> approval (i.e. packets beginning with **{{** are experimental, and may
> be sent by anyone).
>
> **Important Note**: Although there is no restriction on the nature of
> user- defined data, it is highly recommended that it is represented in
> printable 7-bit ASCII character form.
>
> Bytes:
>
> This is envisioned as a way for authors to experiment and build in
> features specific to their programs, without the danger of a
> non-standard packet crashing other authors’ programs. In keeping with
> the spirit of the APRS protocol, authors are encouraged to make these
> formats public.
>
> The APRS Working Group maintains a web site defining all of the
> [assigned User IDs](http://www.aprs.org/aprs11/expfmts.txt), and
> either the packet formats provided by the author, or links to their
> own web sites which define their formats.
>
> Generally, all formats using this method will be considered optional.
> No program is required to decode any of these packets, and must ignore
> any it does not decode. However, it is possible that in the future
> some of these formats may prove to be of sufficient utility and
> interest to the entire APRS community that they will be specifically
> included in future versions of the APRS protocol.

# OTHER PACKETS

## Invalid Data or

## Test Data Packets

> To indicate that a packet contains invalid data, or test data that
> does not conform to any standard APRS format, the **,** Data Type
> Identifier is used.
>
> For example, the Mic-E unit will generate such a packet if it detects
> that a received GPS sentence is not valid.
>
> Bytes:
>
> **All Other Packets** Packets that do not meet any of the formats
> described in this document are assumed to be non-APRS beacons.
> Programs can decide to handle these, or ignore them, but they must be
> able to process them without ill effects.
>
> APRS programs may treat such packets as APRS Status Reports. This
> allows APRS to accept any UI packet addressed to the typical beacon
> address to be captured as a status message. Typical TNC ID packets
> fall into this category. Once a proper Status Report (with the APRS
> Data Type Identifier **&gt;**) has been received from a station it
> will not be overwritten by other non-APRS packets from that station.
>
> Normally, [**HID should be OFF**](http://www.aprs.org/aprs11/HID.txt)
> in all APRS TNCs.
>
> 20<sup>th</sup> century TNC's had a command called HID which enables a
> special "ID" packet once every 10 minutes if the TNC is used in
> repeater service. But this is for use on conventional packet channels
> for Digipeaters, and Nodes to identify their presence. In APRS, this
> HID function has been replaced by the much more valuable APRS POSITION
> packet which not only identifies the digi, but also, its type,
> position, elevation and range. Thus the HID is supposed to be OFF and
> remain off.

# APRS SYMBOLS

> **Three Methods** There are three methods of specifying an APRS symbol
> (display icon):

- In the AX.25 Information field.

- In the AX.25 Destination Address.

- In the SSID of the AX.25 Source Address.

> The preferred method is to include the symbol in the Information
> field. However, where this is not possible (for example, in
> stand-alone trackers with no means of introducing the symbol into the
> Information field), either of the other two methods may be used
> instead.
>
> **The Symbol Tables** There are two APRS Symbol Tables:

- Primary Symbol Table

- Alternate Symbol Table

> See Appendix 2 for a full listing of these tables.
>
> The essential difference between the Primary and Alternate Symbol
> Tables is that some of the symbols in the Alternate Symbol Table can
> be overlaid with an alphanumeric character. For example, a “car” icon
> in the Alternate Symbol Table could be overlaid with the digit “3”, to
> indicate it is car \#3.
>
> Symbols capable of taking an overlay are marked as **\*\[with
> overlay**\]\*. None of the symbols in the Primary Symbol Table can be
> overlaid. In the tables, each symbol is coded in three ways:

- **/$** or **\\** — for symbols in the Information field.

- **GPSxyz** — for generic Destination addresses containing symbols.

- **GPSCnn** or **GPSEnn** — another form of generic Destination
  addresses containing systems.

> In addition, 15 of the symbols in the Primary Symbol Table have an
> associated SSID (e.g. a small aircraft has SSID -7). The SSID is
> intended for use in the AX.25 Source Address of stand-alone trackers
> which have no other means of specifying the symbol.

## Symbols in the AX.25 Information

## Field

> A symbol in the AX.25 Information field is a combination of a
> one-character Symbol Table Identifier and a one-character Symbol Code.
>
> For example, in the Position Report:
>
> @092345z4903.50N**/**07201.75W**&gt;**088/036…
>
> the forward slash **/** is the Symbol Table Identifier and the
> **&gt;** character is the Symbol Code (in this case representing a
> “car” icon) from the selected table.
>
> The Symbol Table Identifier character selects one of the two Symbol
> Tables, or it may be used as single-character (alpha or numeric)
> overlay, as follows:

<table>
<colgroup>
<col style="width: 28%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>Symbol Table Identifier</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Selected Table or Overlay Symbol</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>/</strong></p>
</blockquote></td>
<td><blockquote>
<p>Primary Symbol Table (mostly stations)</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>\</strong></p>
</blockquote></td>
<td><blockquote>
<p>Alternate Symbol Table (mostly Objects)</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>0</strong>-<strong>9</strong></p>
</blockquote></td>
<td><blockquote>
<p>Numeric overlay. Symbol from Alternate Symbol Table
(<em>uncompressed</em> lat/long data format)</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>a</strong>-<strong>j</strong></p>
</blockquote></td>
<td><blockquote>
<p>Numeric overlay. Symbol from Alternate Symbol Table
(<em>compressed</em> lat/long data format only). i.e. a-j maps to
0-9</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>A</strong>-<strong>Z</strong></p>
</blockquote></td>
<td><blockquote>
<p>Alpha overlay. Symbol from Alternate Symbol Table</p>
</blockquote></td>
</tr>
</tbody>
</table>

> In the generic case, a symbol from the Primary Symbol Table is
> represented as the character-pair **/$**, and a symbol from the
> Alternate Symbol Table as
>
> **\\**.

## Overlays with Symbols in the AX.25 Information

## Field

> Where the Symbol Table Identifier is 0-9 or A-Z (or a-j with
> _compressed_ position data only), the symbol comes from the
> _Alternate_ Symbol Table, and is overlaid with the identifier (as a
> single digit or a capital letter).
>
> For example, in the _uncompressed_ Position Report:
>
> @092345z4903.50N**3**07201.75W**&gt;**…
>
> the digit **3** following the latitude will cause the number “3” to be
> overlaid on top of the “car” icon (**Note**: Because the symbol is
> overlaid, the **&gt;** Symbol Code here comes from the _Alternate_
> Symbol Table).
>
> Similarly, to overlay a “car” icon with the letter “B” in a
> _compressed_
>
> Position Report, the report will look something like:
>
> =**B**L!!&lt;\*e7 **&gt;**7P\[
>
> However, in a _compressed_ Position Report, it is not permissible to
> use a _numeric_ Symbol Table Identifier (0-9) — _compressed_ positions
> never start with a digit. If a numeric overlay is required, the report
> must use a lower-case letter instead (in the range **a**-**j**) as the
> Symbol Table Identifier. The lower- case letter is then mapped to the
> digits **0**-**9** (i.e. a=0, b=1, c=2, d=3 etc).
>
> Thus, in the _compressed_ Position Report:
>
> =**d**5L!!&lt;\*e7 **&gt;**7P\[
>
> the letter **d** maps to overlay character “3”.
>
> As noted above, not all symbols from the Alternate Symbol Table may be
> overlaid in this way — those that can be overlaid are marked as
> **_\[with overlay\]_** in Appendix 2. This means that they are
> _capable_ of taking an overlay, but they do not necessarily need to
> have one. Thus, for example, the following report uses the car symbol
> from the Alternate Symbol Table, but does not display an overlay:
>
> @092345z4903.50N**\\**07201.75W**&gt;**…

## Symbols in the AX.25 Destination

## Address

> Where it is not possible to include a symbol in the Information field,
> the symbol may be specified in the AX.25 Destination Address instead,
> using the following generic destination addresses: GPSxyz, GPSCnn,
> GPSEnn, SPCxyz and SYMxyz.
>
> The characters xy and nn refer to entries in the APRS Symbol Tables.
> For example, from the Primary Symbol Table, a tracker could use the
> Destination Address GPS**MV** or GPS**30** to specify a “car” icon.
>
> The character z specifies the overlay character (where permitted), or
> is a **** (space) — the space is a filler character, as all AX.25
> addresses must be exactly 6 characters long.
>
> The GPS/SPC/SYMxy and GPSCnn/GPSEnn addresses can be used
> interchangeably. Thus, for example, GPSBM , SPCBM , SYMBM and
> GPSC12 all specify a “Boy Scouts” icon (from the Primary Symbol
> Table), and GPSOM , SPCOM , SYMOM and GPSE12 all specify a “Girl
> Scouts” icon (from the Alternate Symbol Table).

## Overlays with Symbols in the AX.25 Destination

## Address

> If the z character in a GPSxyz, SPCxyz or SYMxyz address is not a
> space, it specifies an alphanumeric overlay character, in the range
> 0-9 or A-Z.
>
> Overlays can only be used with symbols from the Alternate Symbol Table
> marked with the legend **_\[with overlay\]_**.
>
> For example, if the “car” icon is to be overlaid with a digit “3”, the
> Destination Address will be GPS**NV3**.
>
> However, even if the address is overlay-capable, it is not actually
> necessary to specify an overlay; e.g. GPS**NV**.
>
> GPSCnn and GPSEnn symbols can not have overlays.

## Symbol in the Source Address

## SSID

> Where it is not possible to include a symbol in the Information field
> or in the Destination Address, the symbol may be specified in the SSID
> of the Source Address instead:
>
> **SSID-Specified Icons in the AX.25 Source Address Field**
>
> **Symbol Precedence** APRS packets should not contain more than one
> symbol. However, it is conceivably possible to (erroneously) construct
> a packet containing up to three different symbols.
>
> For example:

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 22%" />
<col style="width: 22%" />
<col style="width: 40%" />
</colgroup>
<thead>
<tr class="header">
<th rowspan="2"></th>
<th><blockquote>
<p><em><strong>Source Address SSID</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Destination Address</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Information Field</strong></em></p>
</blockquote></th>
</tr>
<tr class="odd">
<th><blockquote>
<p>G3NRW<strong>-7</strong></p>
</blockquote></th>
<th><blockquote>
<p>GPS<strong>MV</strong></p>
</blockquote></th>
<th><blockquote>
<p>!0123.45N<strong>/</strong>01234.56W<strong>j</strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong>Symbol</strong></em></p>
</blockquote></td>
<td><blockquote>
<p>Small Aircraft</p>
</blockquote></td>
<td><blockquote>
<p>Car</p>
</blockquote></td>
<td><blockquote>
<p>Jeep</p>
</blockquote></td>
</tr>
</tbody>
</table>

> In such a situation:

- The symbol in the Information field takes precedence over any other
  symbol.

- If there is no symbol in the Information field, the symbol in the
  Destination Address takes precedence over the symbol in the Source
  Address SSID.

## SSID Recommendations

> It is very convenient to other mobile operators or others, looking at
> callsigns flashing by, to be able to recognize some common
> applications at a glance. Here are the recommendations for the 16
> possible SSIDs. Note, The SSID of zero is dropped by most display
> applications. So a callsign with no
>
> SSID has an SSID of 0.

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 89%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>SSID</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Type of Station</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>-0</strong></p>
</blockquote></td>
<td><blockquote>
<p>Your primary station, usually fixed and message capable</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>-1</strong></p>
</blockquote></td>
<td><blockquote>
<p>Generic additional station, digi, mobile, wx, etc.</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>-2</strong></p>
</blockquote></td>
<td><blockquote>
<p>Generic additional station, digi, mobile, wx, etc.</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>-3</strong></p>
</blockquote></td>
<td><blockquote>
<p>Generic additional station, digi, mobile, wx, etc.</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>-4</strong></p>
</blockquote></td>
<td><blockquote>
<p>Generic additional station, digi, mobile, wx, etc.</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>-5</strong></p>
</blockquote></td>
<td><blockquote>
<p>Other networks (Dstar, iPhones, Androids, Blackberrys etc.)</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>-6</strong></p>
</blockquote></td>
<td><blockquote>
<p>Special activity, Satellite ops, camping or 6 meters, etc.</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>-7</strong></p>
</blockquote></td>
<td><blockquote>
<p>Walkie talkies, HTs or other human portable</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>-8</strong></p>
</blockquote></td>
<td><blockquote>
<p>Boats, sailboats, RVs or second main mobile</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>-9</strong></p>
</blockquote></td>
<td><blockquote>
<p>Primary Mobile (usually message capable)</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>-10</strong></p>
</blockquote></td>
<td><blockquote>
<p>Internet, IGates, echolink, winlink, AVRS, APRN, etc.</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>-11</strong></p>
</blockquote></td>
<td><blockquote>
<p>Balloons, aircraft, spacecraft, etc</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>-12</strong></p>
</blockquote></td>
<td><blockquote>
<p>APRStt, DTMF, RFID, devices, one-way trackers*, etc.</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>-13</strong></p>
</blockquote></td>
<td><blockquote>
<p>Weather stations</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>-14</strong></p>
</blockquote></td>
<td><blockquote>
<p>Truckers or generally full time drivers</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>-15</strong></p>
</blockquote></td>
<td><blockquote>
<p>Generic additional station, digi, mobile, wx, etc.</p>
</blockquote></td>
</tr>
</tbody>
</table>

# APPENDIX 1: APRS DATA FORMATS

> This Appendix contains format diagrams for all APRS data formats. The
> gray fields are optional. Shaded (yellow) characters are literal ASCII
> characters.
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:

<table>
<colgroup>
<col style="width: 6%" />
<col style="width: 9%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 9%" />
<col style="width: 19%" />
<col style="width: 7%" />
<col style="width: 23%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="9"><blockquote>
<p><em><strong>Compressed Lat/Long Position Report Format — with
Timestamp</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="3"><blockquote>
<p><strong>/</strong> <em>or</em></p>
<p><strong>@</strong></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Time DHM / HMS</strong></em></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Sym Table ID</strong></em></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Comp Lat</strong></em> YYYY</p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Comp Long</strong></em> XXXX</p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Symbol Code</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Compressed Course/Speed</strong></em></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Comp Type</strong></em> T</p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Comment (max 40 chars)</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong>Compressed Radio Range</strong></em></p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><em><strong>Compressed Altitude</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>2</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0-40</p>
</blockquote></td>
</tr>
</tbody>
</table>

Bit:

Value:

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="7"><blockquote>
<p><em><strong>Mic-E Data — DESTINATION ADDRESS FIELD
Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong>Lat Digit 1</strong></em></p>
<p><em><strong>+ Message Bit A</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 2</strong></em></p>
<p><em><strong>+ Message Bit B</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 3</strong></em></p>
<p><em><strong>+ Message Bit C</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 4</strong></em></p>
<p><em><strong>+ N/S Lat Indicator</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 5</strong></em></p>
<p><em><strong>+ Longitude Offset</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 6</strong></em></p>
<p><em><strong>+ W/E Long Indicator</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>APRS</strong></em></p>
<p><em><strong>Digi Path Code</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:

<table>
<colgroup>
<col style="width: 4%" />
<col style="width: 9%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 6%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 12%" />
<col style="width: 11%" />
<col style="width: 7%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="12"><blockquote>
<p><em><strong>Complete Weather Report Format — with Object and Lat/Long
position</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>*</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Object Name</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><strong>*</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Time DHM / HMS</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Sym Table ID</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Long</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Symbol Code</strong></em></p>
<p><strong>_</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Wind Directn/ Speed</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Weather Data</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>APRS</strong></em></p>
<p><em><strong>Software</strong></em></p>
<p>S</p>
</blockquote></td>
<td><blockquote>
<p><em><strong>WX</strong></em></p>
<p><em><strong>Unit</strong></em></p>
<p>uuuu</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>8</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>2-4</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:

<table style="width:100%;">
<colgroup>
<col style="width: 4%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 12%" />
<col style="width: 23%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="9"><blockquote>
<p><em><strong>Telemetry Report Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>T</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong><em>Sequence No</em> #</strong>nnn<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 1</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 2</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 3</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 4</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 5</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Digital Value</strong></em> bbbbbbbb</p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Comment</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>5</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>8</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 12%" />
<col style="width: 3%" />
<col style="width: 63%" />
<col style="width: 3%" />
<col style="width: 14%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="6"><blockquote>
<p><em><strong>Message Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="2"><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td rowspan="2"><blockquote>
<p><em><strong>Addressee</strong></em></p>
</blockquote></td>
<td rowspan="2"><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td rowspan="2"><blockquote>
<p><em><strong>Message Text (max 67 chars)</strong></em></p>
</blockquote></td>
<td colspan="2"><blockquote>
<p><em><strong>Message ID</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><strong>{</strong></td>
<td><blockquote>
<p><em><strong>Message No</strong></em></p>
<p>xxxxx</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0-67</p>
</blockquote></td>
<td>1</td>
<td><blockquote>
<p>1-5</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 33%" />
<col style="width: 8%" />
<col style="width: 16%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="5"><blockquote>
<p><em><strong>Message Acknowledgement Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Addressee</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>ack</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Message No</strong></em> xxxxx</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>3</p>
</blockquote></td>
<td><blockquote>
<p>1–5</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 7%" />
<col style="width: 14%" />
<col style="width: 74%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><em><strong>User-Defined Data Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>{</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>User ID</strong></em> U</p>
</blockquote></td>
<td><blockquote>
<p><em><strong>User-Defined Packet Type</strong></em> X</p>
</blockquote></td>
<td><blockquote>
<p><em><strong>User-defined data (printable ASCII
recommended)</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 96%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="2"><blockquote>
<p><em><strong>Invalid Data / Test Data Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Invalid Data or Test Data</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
</tr>
</tbody>
</table>

# APPENDIX 2: THE APRS SYMBOL TABLES

> (Each highlighted character in the Alternate Symbol Table may be
> replaced with an overlay character).

Note 1: Red dot (with overlay) marks a mobile's destination. Drawn with
a line between the mobile and its waypoint destination.

## APRS SYMBOL TABLES (continued)

> (Each highlighted character in the Alternate Symbol Table may be
> replaced with an overlay character).

## APRS SYMBOL TABLES (continued)

> (Each highlighted character in the Alternate Symbol Table may be
> replaced with an overlay character).
>
> Originally, APRS had 192 symbols. It was expanded to thousands, in
> 2007, by allowing every alternate symbol to have 36 alphanumeric
> overlays.
>
> The new symbol sets are extensible and evolving so they should not be
> compiled into applications. Instead, [applications should read these
> files](http://www.aprs.org/symbols/symbols-background.txt), or their
> equivalents, at runtime to make upgrading easier.

- [Current SYMBOLS SPEC](http://www.aprs.org/symbols/symbolsX.txt)
  (version 1.1 plus non conflicting parts of 1.2).

- [New version 1.2
  symbols](http://www.aprs.org/symbols/symbols-new.txt) with new
  expandable Overlay blocks.

> For more details see [APRS Symbol Upgrade 2014 (Not part of 1.1, but a
> peek into where this is going.)](http://www.aprs.org/symbols.html)

# APPENDIX 3: 7-BIT ASCII CODE TABLE

> In addition to listing the ASCII character codes in their usual form,
> this table also expresses the hexadecimal codes for the ASCII digits
> 0–9 and the upper-case letters A–Z in _shifted_ form; i.e. shifted one
> bit left. This is particularly useful for decoding callsigns and Mic-E
> position information contained in the address fields of AX.25 frames.

### Part 1: Codes 0–31 decimal (00–1f hexadecimal)

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 11%" />
<col style="width: 19%" />
<col style="width: 19%" />
<col style="width: 40%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>Dec</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Hex</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Char</strong></em></p>
</blockquote></th>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>0</strong></p>
</blockquote></td>
<td><blockquote>
<p>00</p>
</blockquote></td>
<td><blockquote>
<p>NUL</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-@</p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>1</strong></p>
</blockquote></td>
<td><blockquote>
<p>01</p>
</blockquote></td>
<td><blockquote>
<p>SOH</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-A</p>
</blockquote></td>
<td><blockquote>
<p>Start of Header</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>2</strong></p>
</blockquote></td>
<td><blockquote>
<p>02</p>
</blockquote></td>
<td><blockquote>
<p>STX</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-B</p>
</blockquote></td>
<td><blockquote>
<p>Start of Text</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>3</strong></p>
</blockquote></td>
<td><blockquote>
<p>03</p>
</blockquote></td>
<td><blockquote>
<p>ETX</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-C</p>
</blockquote></td>
<td><blockquote>
<p>End of Text</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>4</strong></p>
</blockquote></td>
<td><blockquote>
<p>04</p>
</blockquote></td>
<td><blockquote>
<p>EOT</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-D</p>
</blockquote></td>
<td><blockquote>
<p>End of Transmission</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>5</strong></p>
</blockquote></td>
<td><blockquote>
<p>05</p>
</blockquote></td>
<td><blockquote>
<p>ENQ</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-E</p>
</blockquote></td>
<td><blockquote>
<p>Enquiry (Poll)</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>6</strong></p>
</blockquote></td>
<td><blockquote>
<p>06</p>
</blockquote></td>
<td><blockquote>
<p>ACK</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-F</p>
</blockquote></td>
<td><blockquote>
<p>Acknowledge</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>7</strong></p>
</blockquote></td>
<td><blockquote>
<p>07</p>
</blockquote></td>
<td><blockquote>
<p>BEL</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-G</p>
</blockquote></td>
<td><blockquote>
<p>Bell</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>8</strong></p>
</blockquote></td>
<td><blockquote>
<p>08</p>
</blockquote></td>
<td><blockquote>
<p>BS</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-H</p>
</blockquote></td>
<td><blockquote>
<p>Backspace</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>9</strong></p>
</blockquote></td>
<td><blockquote>
<p>09</p>
</blockquote></td>
<td><blockquote>
<p>HT</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-I</p>
</blockquote></td>
<td><blockquote>
<p>Horizontal Tab</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>10</strong></p>
</blockquote></td>
<td><blockquote>
<p>0a</p>
</blockquote></td>
<td><blockquote>
<p>LF</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-J</p>
</blockquote></td>
<td><blockquote>
<p>Line Feed</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>11</strong></p>
</blockquote></td>
<td><blockquote>
<p>0b</p>
</blockquote></td>
<td><blockquote>
<p>VT</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-K</p>
</blockquote></td>
<td><blockquote>
<p>Vertical Tab</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>12</strong></p>
</blockquote></td>
<td><blockquote>
<p>0c</p>
</blockquote></td>
<td><blockquote>
<p>FF</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-L</p>
</blockquote></td>
<td><blockquote>
<p>Form Feed</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>13</strong></p>
</blockquote></td>
<td><blockquote>
<p>0d</p>
</blockquote></td>
<td><blockquote>
<p>CR</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-M</p>
</blockquote></td>
<td><blockquote>
<p>Carriage Return</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>14</strong></p>
</blockquote></td>
<td><blockquote>
<p>0e</p>
</blockquote></td>
<td><blockquote>
<p>SO</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-N</p>
</blockquote></td>
<td><blockquote>
<p>Shift Out</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>15</strong></p>
</blockquote></td>
<td><blockquote>
<p>0f</p>
</blockquote></td>
<td><blockquote>
<p>SI</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-O</p>
</blockquote></td>
<td><blockquote>
<p>Shift In</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>16</strong></p>
</blockquote></td>
<td><blockquote>
<p>10</p>
</blockquote></td>
<td><blockquote>
<p>DLE</p>
</blockquote></td>
<td><blockquote>
<p>CRTL-P</p>
</blockquote></td>
<td><blockquote>
<p>Data Link Escape</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>17</strong></p>
</blockquote></td>
<td><blockquote>
<p>11</p>
</blockquote></td>
<td><blockquote>
<p>DC1/XON</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-Q</p>
</blockquote></td>
<td><blockquote>
<p>Device Control 1</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>18</strong></p>
</blockquote></td>
<td><blockquote>
<p>12</p>
</blockquote></td>
<td><blockquote>
<p>DC2</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-R</p>
</blockquote></td>
<td><blockquote>
<p>Device Control 2</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>19</strong></p>
</blockquote></td>
<td><blockquote>
<p>13</p>
</blockquote></td>
<td><blockquote>
<p>DC3/XOFF</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-S</p>
</blockquote></td>
<td><blockquote>
<p>Device Control 3</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>20</strong></p>
</blockquote></td>
<td><blockquote>
<p>14</p>
</blockquote></td>
<td><blockquote>
<p>DC4</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-T</p>
</blockquote></td>
<td><blockquote>
<p>Device Control 4</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>21</strong></p>
</blockquote></td>
<td><blockquote>
<p>15</p>
</blockquote></td>
<td><blockquote>
<p>NAK</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-U</p>
</blockquote></td>
<td><blockquote>
<p>Negative Acknowledge</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>22</strong></p>
</blockquote></td>
<td><blockquote>
<p>16</p>
</blockquote></td>
<td><blockquote>
<p>SYN</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-V</p>
</blockquote></td>
<td><blockquote>
<p>Synchronous Idle</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>23</strong></p>
</blockquote></td>
<td><blockquote>
<p>17</p>
</blockquote></td>
<td><blockquote>
<p>ETB</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-W</p>
</blockquote></td>
<td><blockquote>
<p>End of Transmission Block</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>24</strong></p>
</blockquote></td>
<td><blockquote>
<p>18</p>
</blockquote></td>
<td><blockquote>
<p>CAN</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-X</p>
</blockquote></td>
<td><blockquote>
<p>Cancel</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>25</strong></p>
</blockquote></td>
<td><blockquote>
<p>19</p>
</blockquote></td>
<td><blockquote>
<p>EM</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-Y</p>
</blockquote></td>
<td><blockquote>
<p>End of Medium</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>26</strong></p>
</blockquote></td>
<td><blockquote>
<p>1a</p>
</blockquote></td>
<td><blockquote>
<p>SUB</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-Z</p>
</blockquote></td>
<td><blockquote>
<p>Substitute</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>27</strong></p>
</blockquote></td>
<td><blockquote>
<p>1b</p>
</blockquote></td>
<td><blockquote>
<p>ESC</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-[</p>
</blockquote></td>
<td><blockquote>
<p>Escape</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>28</strong></p>
</blockquote></td>
<td><blockquote>
<p>1c</p>
</blockquote></td>
<td><blockquote>
<p>FS</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-\</p>
</blockquote></td>
<td><blockquote>
<p>File Separator</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>29</strong></p>
</blockquote></td>
<td><blockquote>
<p>1d</p>
</blockquote></td>
<td><blockquote>
<p>GS</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-]</p>
</blockquote></td>
<td><blockquote>
<p>Group Separator</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>30</strong></p>
</blockquote></td>
<td><blockquote>
<p>1e</p>
</blockquote></td>
<td><blockquote>
<p>RS</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-^</p>
</blockquote></td>
<td><blockquote>
<p>Record Separator</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>31</strong></p>
</blockquote></td>
<td><blockquote>
<p>1f</p>
</blockquote></td>
<td><blockquote>
<p>US</p>
</blockquote></td>
<td><blockquote>
<p>CTRL-_</p>
</blockquote></td>
<td><blockquote>
<p>Unit Separator</p>
</blockquote></td>
</tr>
</tbody>
</table>

> **Part 2: Codes 32–127 decimal (20–7f hexadecimal), including hex
> codes for shifted 0–9/A–Z**

# APPENDIX 4: DECIMAL-TO-HEX CONVERSION TABLE

# APPENDIX 5: GLOSSARY

> **Altitude** 1. In Mic-E format, the altitude in meters relative to
> 10km below mean sea level.
>
> 2\. In Comment text, the altitude in feet above mean sea level.
>
> **Announcement** An APRS message that is repeated a few times an hour,
> perhaps for several days.

**Announcement Identifier** A single letter A-Z that identifies a
particular announcement.

> **Antenna Height** In NMEA sentences, the height of the antenna in
> meters relative to mean sea level. (The antenna height in GPS NMEA
> sentences fluctuates wildly because of Selective Availability, and
> should only be used if DGPS correction is applied).
>
> **APRS** Automatic Packet Reporting System.
>
> **APRS Data** The data that follows the APRS Data Type Identifier in
> the AX.25 Information field and precedes the APRS Data Extension.
>
> **APRS Data Extension** A 7-byte extension to APRS Data. The Data
> Extension includes one of Course/Speed, Wind Direction/Wind Speed,
> Station Power/Antenna Effective Height/Gain/Directivity,
> Pre-Calculated Radio Range, DF Signal Strength/Effective Antenna
> Height/Gain, Area Object Descriptor.
>
> **APRS Digipeater Path** A digipeater path via repeaters with station
> names or more often aliases. Used in Mic-E compressed location format.
> FIXME-. What does MiC-E have to do with it?
>
> **APRS Data Type Identifier** The single-byte identifier that
> specifies what kind of APRS information is contained in the AX.25
> Information field.
>
> **Area Object** A user-defined graphic object (circle, ellipse,
> triangle, box and line).
>
> **ASCII** American Standard Code for Information Interchange. A 7-bit
> character code conforming to ANSI X3.4 (1968) — see Appendix 3 for
> character definitions.
>
> **AX.25** Amateur Packet-Radio Link-Layer Protocol.
>
> **Base 91** Number base used to ensure that numeric values are
> transmitted as printable ASCII characters. To obtain the character
> string corresponding to a numeric value, divide the value
> progressively by decreasing powers of 91, and add 33 decimal to the
> result at each step. Printable characters are in the range
> **!**..**{**. Used in compressed lat/long and altitude computation.
>
> **Bulletin** An APRS message that is repeated several times an hour,
> for a small number of hours. A General Bulletin is addressed to no-one
> in particular. A Group Bulletin is addressed to a named group (e.g.
> WX).

**Bulletin Identifier** A single digit 0-9 that identifies a particular
bulletin.

> **Destination Address field** The AX.25 Destination Address field,
> which can contain an APRS destination callsign or Mic-E encoded data.
>
> **DF Report** A report containing DF bearing and range.
>
> **DGPS** Differential GPS. Used to overcome the errors arising from
> Selective Availability.
>
> **DHM** 7-character timestamp: day-of-the-month, hour, minute, zulu or
> local time.
>
> **DHMz** 7-character timestamp: day-of-the-month, hour, minute, zulu
> only.
>
> **Digipeater** A station that relays AX.25 packets. A chain of up to 8
> digipeaters may be specified.

**Digipeater Addresses field** The AX.25 field containing 0–8 digipeater
callsigns (or aliases).

> **Directivity** The favored direction of an antenna. Used in the PHG
> Data Extension.
>
> **DX Cluster** A network host that collects and disseminates user
> reports of DX activity.
>
> **ECHO** A generic APRS digipeater callsign alias, for an HF
> digipeater.
>
> **Effective Antenna Height** The height of an antenna above the local
> terrain (not above sea level). A first-order indicator of the
> antenna’s effectiveness in the local area. Used in the PHG Data

Extension.

> **ERP** Effective Radiated Power. Used in Status Reports containing
> Beam Heading and Power data (typically for meteor scatter use).
>
> **FCS** Frame Check Sequence. A sequence of 16 bits that follows the
> AX.25 Information field, used to verify the integrity of the packet.
>
> **GATE** A gateway between HF and VHF APRS networks. Used primarily to
> relay long- distance HF APRS traffic onto local VHF networks.
>
> **GGA Sentence** A standard NMEA sentence, containing the receiving
> station’s lat/long position and antenna height relative to mean sea
> level, and other data.
>
> **GLL Sentence** A standard NMEA sentence, containing the receiving
> station’s lat/long position and other data.
>
> **GMT** Greenwich Mean Time (=UTC=zulu).
>
> **GPS** Global Positioning System. A global network of 24 satellites
> that provide lat/long and antenna height of a receiving station.
>
> **GPSxyz** An APRS destination callsign that specifies a display
> symbol from either the Primary Symbol Table or the Alternate Symbol
> Table. Some symbols from the Alternate Symbol Table can be overlaid
> with a digit or a letter. Used by trackers that cannot specify the
> symbol in the AX.25 Information field.
>
> **GPSCnn** An APRS destination callsign that specifies a display
> symbol from the Primary Symbol Table. The symbol can not be overlaid.
> Used by trackers that cannot specify the symbol in the AX.25
> Information field.
>
> **GPSEnn** An APRS destination callsign that specifies a display
> symbol from the Alternate Symbol Table. The symbol can not be
> overlaid. Used by trackers that cannot specify the symbol in the AX.25
> Information field.
>
> **HMS** 1. In NMEA sentences, a 6-character timestamp: hour, minute,
> second UTC.
>
> 2\. In APRS Data, a 7-character timestamp: hour, minute, second, zulu
> or local.
>
> **ICQ** International CQ chat.
>
> **IGate** A gateway between a VHF and/or HF APRS network and the
> Internet.
>
> **Information field** The AX.25 Information field containing APRS
> information.
>
> **Item** A type of display object.
>
> **Item Report** A report containing the location of an APRS Item.
>
> **Killed Object** An Object that an APRS user has assumed control of.
>
> **knots** International nautical miles per hour.
>
> **KPC-3** A Terminal Node Controller from Kantronics Co Inc.
>
> **Longitude Offset** An offset of +100 degrees longitude (used in
> Mic-E longitude computation).
>
> **LORAN** Long Range Navigation System (a terrestrial precursor to
> GPS).

**Maidenhead Locator** A 4- or 6-character grid locator specifying a
station’s position.

> **MDHM** 8-byte timestamp: month, day, hour, minute (used in
> positionless weather station reports).
>
> **Message** A one-line text message addressed to a particular station.

**Message Acknowledgement** An optional acknowledgement of receipt of a
message.

**Message Group** A user-defined group to receive messages.

**Message Identifier** A 1–5 character message identifier (typically a
line number).

> **Mic-E** Originally Microphone Encoder, a unit that encodes location,
> course and speed information into a very short packet, for
> transmission when releasing the microphone PTT button. The Mic-E
> encoding algorithm is now used in other devices (e.g. in the
>
> PIC-E and the Kenwood TH-D7/TM-D700 radios).
>
> **Mic-E Message Identifier** A 3-bit identifier (A/B/C) specifying a
> standard Mic-E message or custom message code.
>
> **Mic-E Message Code** A 3-bit code specifying a Standard or Custom
> Mic-E message.
>
> **MIM** Micro Interface Module. A complete telemetry TNC transmitter
> on a chip.
>
> **mph** miles per hour.
>
> **Net Cycle Time** The time within which it should be possible to gain
> the complete picture of APRS activity (typically 10, 20 or 30 minutes,
> depending on the number of digipeaters traversed and local
> conditions). Stations should not transmit status or position
> information more frequently unless mobile, or in response to a Query.
>
> **NMEA** National Marine Electronic Association (United States).
> Producer of the _NMEA 0183 Version 2.0_ specification that governs the
> format of Received Sentences from navigation equipment (such as GPS
> and LORAN receivers). See Appendix 6 for a reference to NMEA sentence
> formats.
>
> **NMEA (Received) Sentence** The ASCII data stream received from
> navigation equipment (such as GPS receivers) conforming to the NMEA
> 0182 Version 2.0 specification. APRS supports five NMEA Sentences:
> GGA, GLL, RMC, VTG and WPL.
>
> **NRQ** Number/Rate/Quality. A measure of confidence in DF Bearing
> reports.
>
> **Null Position** Default position to be reported if the actual
> position is unknown or indeterminate. The null position is 0° 0' 0"
> north, 0° 0' 0" west.
>
> **NWS** National Weather Service (United States).
>
> **Object** A display object that is (usually) not a station. For
> example, a weather front or a marathon runner.
>
> **Object Report** A report containing the position of an object, with
> optional timestamp and APRS Data Extension.
>
> **PHG** APRS Data Extension specifying Power, Effective Antenna
> Height/Gain/Directivity.
>
> **PIC** Programmable Interface Controller.

**PIC-E** A PIC implementation of the Mic-E microphone encoder.

> **Position Ambiguity** A reduction in the accuracy of APRS position
> information (implemented by replacing low-order lat/long digits with
> spaces). Used when the exact position is not known.
>
> **Position Report** A report containing lat/long position, optionally
> with timestamp and Data Extension.
>
> **Pre-Calculated Radio Range** A station’s estimate of
> omni-directional radio range (in miles). Used in compressed
>
> lat/long format.
>
> **Query** A request for information. Queries may be addressed to
> stations in general or to specific stations.

**Range Circle** Usable radio range (in miles), computed from PHG data.

> **Response** A reply to a query.
>
> **RMC Sentence** A standard NMEA sentence, containing the receiving
> station’s lat/long position, course and speed, and other data.
>
> **RTCM** Radio Technical Commission for Maritime Services. The RTCM
> SC104 data format specification describes the requirements for
> differential GPS data correction.
>
> **Selective Availability** Deliberate GPS position dithering,
> introducing significant received position errors in latitude,
> longitude _and_ antenna height. Errors can be greatly reduced with
> differential GPS.
>
> **Sentence** See NMEA (Received) Sentence.
>
> **Signpost** A special signpost icon that displays user-defined
> variable information (such as a
>
> speed limit or mileage) as an overlay.
>
> **Skywarn** A weather spotter initiative coordinated by the United
> States National Weather Service.
>
> **Source Address Field** The AX.25 Source Address field, containing
> the callsign of the originating station. A non-zero SSID specifies a
> display symbol.
>
> **Source Path Header** The digipeater path followed prior to a packet
> entering a Third-Party Network.
>
> **SPCL** A generic APRS destination callsign used for special
> stations.
>
> **SSID** Secondary Station Identifier. A number in the range 0-15, as
> an adjunct to an AX.25 address. If the SSID in a source address is
> non-zero, it specifies a display symbol. (This is used when the
> station is unable to specify the symbol in the AX.25 Destination
> Address field or Information field).
>
> **Station Capabilities** A list of station characteristics that is
> sent in reply to a query.
>
> **Status Report** A report containing station status information (and
> optionally a Maidenhead locator).

**Switch Stream Character** A character normally used for switching TNC
channels.

> **Symbol** A display icon. Consists of a Symbol Table
> Identifier/Symbol Code pair. Generically,
>
> **/$** represents a symbol from the Primary Symbol Table, and **\\**
> represents a symbol from the Alternate Symbol Table.

**Symbol Code** A code for a symbol within a Symbol Table.

> **Symbol Table Identifier** An ASCII code specifying the Primary
> Symbol Table (**/**) or Alternate Symbol Table (**\\**).
>
> The Symbol Table Identifier is also implicit in GPSCnn and GPSEnn
> destination callsigns.
>
> **Target Footprint** A target area for queries. The querying station
> asks for responses from stations within a specified number of miles of
> a lat/long position.
>
> **TH-D7** A combined VHF/UHF handheld radio and APRS-compatible TNC
> from Kenwood.
>
> **TM-D700** A combined VHF/UHF mobile radio and APRS-compatible TNC
> from Kenwood.
>
> **Third Party Network** A non-APRS network that does not understand
> AX.25 addresses (e.g. the Internet).
>
> **Third-Party Header** A Path Header with the Third-Party Network
> Identifier and the callsign of the receiving gateway inserted.
>
> **TNC** Terminal Node Controller. A combined AX.25 packet
> assembler/disassembler and modem.
>
> **Trace** An APRS query that asks for the route taken by a packet to a
> specified station.
>
> **Tracker** A unit comprising a GPS receiver (to obtain the current
> geographical position) and a radio transmitter (to transmit the
> position to other stations).
>
> **Tunneling** Passing APRS AX.25 traffic through a third-party network
> that does not understand AX.25 addressing. The AX.25 addresses are
> carried as data (in the Source Path Header) through the tunneled
> network.
>
> **UI-Frame** AX.25 Unnumbered Information frame. APRS uses only
> UI-frames — that is, it operates entirely in connectionless (UNPROTO)
> mode.

**UNPROTO Path** The digipeater path to the destination callsign.

> **UTC** Coordinated Universal Time (=zulu=GMT).
>
> **VTG Received Sentence** A standard NMEA sentence, containing the
> receiving station’s course and speed.
>
> **WIDEn-N** A generic APRS digipeater callsign alias, for a digipeater
> with wide area coverage (N=0-7). As a packet passes through a
> digipeater, the value of N is decremented by 1 until it reaches zero.
> The digipeater keeps a record of each packet (or its FCS) as it
>
> passes through, and will not digipeat the packet again if it has
> digipeated it already within the last 28 seconds. A digipeater should
> insert its own callsign in the digipeater path so a receiving station
> will know the path taken by the packet.
>
> **WPL Sentence** A standard NMEA sentence, containing waypoints.
>
> **WX** Weather.
>
> **Ziplan** A cheap twisted-pair LAN connecting PCs via their serial
> I/O ports. Designed for use in emergency situations.
>
> **Zulu** UTC/GMT.
>
> **Units Conversion Table**

<table>
<colgroup>
<col style="width: 29%" />
<col style="width: 29%" />
<col style="width: 19%" />
<col style="width: 21%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>To convert from</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>to</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>multiply by</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>divide by</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>feet</td>
<td><blockquote>
<p>meters</p>
</blockquote></td>
<td><blockquote>
<p>0.3048</p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td>meters</td>
<td><blockquote>
<p>feet</p>
</blockquote></td>
<td></td>
<td><blockquote>
<p>0.3048</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>miles</td>
<td><blockquote>
<p>km</p>
</blockquote></td>
<td><blockquote>
<p>1.609344</p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td>km</td>
<td><blockquote>
<p>miles</p>
</blockquote></td>
<td></td>
<td><blockquote>
<p>1.609344</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>miles</td>
<td><blockquote>
<p>nautical miles</p>
</blockquote></td>
<td><blockquote>
<p>0.8689762</p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td>nautical miles</td>
<td><blockquote>
<p>miles</p>
</blockquote></td>
<td></td>
<td><blockquote>
<p>0.8689762</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>miles per hour (mph)</td>
<td><blockquote>
<p>knots</p>
</blockquote></td>
<td><blockquote>
<p>0.8689762</p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td>knots</td>
<td><blockquote>
<p>miles per hour (mph)</p>
</blockquote></td>
<td></td>
<td><blockquote>
<p>0.8689762</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>knots</td>
<td><blockquote>
<p>meters / second</p>
</blockquote></td>
<td><blockquote>
<p>0.51444’</p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td>meters / second</td>
<td><blockquote>
<p>knots</p>
</blockquote></td>
<td></td>
<td><blockquote>
<p>0.51444’</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>miles per hour (mph)</td>
<td><blockquote>
<p>meters / second</p>
</blockquote></td>
<td><blockquote>
<p>0.44704</p>
</blockquote></td>
<td></td>
</tr>
<tr class="even">
<td>meters / second</td>
<td><blockquote>
<p>miles per hour (mph)</p>
</blockquote></td>
<td></td>
<td><blockquote>
<p>0.44704</p>
</blockquote></td>
</tr>
</tbody>
</table>

> **Fahrenheit / Celsius Temperature Conversion Equations**
>
> F = ( C x 1.8 ) + 32 C = <u>( F – 32 ) x 5</u>
>
> 9

# APPENDIX 6: REFERENCES

> _AX.25 Link Access Protocol for Amateur Packet Radio, version 2.2,
> July 1998,_ at

<http://www.tapr.org/pdf/AX25.2.2.pdf>

> _NMEA 0183 Interface Standard,_ at
> <https://www.nmea.org/content/STANDARDS/NMEA_0183_Standard>
>
> NMEA Sentence Formats, in the _Garmin GPS25 Technical Reference
> Manual_, at
>
> [<u>http://www.garmin.com/manuals/spec25.pdf</u>](http://www.garmin.com/manuals/spec25.pdf)
>
> Maidenhead Locator, in the _IARU Region 1 VHF Manager’s Manual_, at
>
> [<u>http://www.scit.wlv.ac.uk/vhfc/iaru.r1.vhfm.4e/index.html</u>](http://www.scit.wlv.ac.uk/vhfc/iaru.r1.vhfm.4e/index.html)

# APPENDIX 7: DOCUMENT RELEASE HISTORY

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 11%" />
<col style="width: 75%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>Date</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Doc Version</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Status / Major Changes</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>10 Oct 1999</td>
<td><blockquote>
<p>1.0 (Draft)</p>
</blockquote></td>
<td><blockquote>
<p>Protocol Version 1.0. First public draft release.</p>
</blockquote></td>
</tr>
<tr class="even">
<td>3 Dec 1999</td>
<td><blockquote>
<p>1.0.1g</p>
</blockquote></td>
<td><blockquote>
<p>Protocol Version 1.0. Second public draft release. Much extended,
incorporating packet format layouts, APRS symbol tables, compressed data
format, Mic-E format, telemetry format.</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>30 Apr 2000</td>
<td><blockquote>
<p>1.0.1m</p>
</blockquote></td>
<td><blockquote>
<p>Protocol Version 1.0. Third public draft release.</p>
<p>Major additions/changes to the draft 1.0.1g specification:</p>
</blockquote>
<ul>
<li><p>Added a section on Map Views and Range Scale.</p></li>
<li><p>Changed Destination Address SSID description (specifying generic
APRS digipeater paths) to apply to <em>all</em> packets, not just Mic-E
packets.</p></li>
<li><p>Changed APRS destination “callsigns” to “destination
addresses”.</p></li>
<li><p>Added TEL* to the list of generic destination addresses.</p></li>
<li><p>Added brief explanations of how several generic destination
addresses are used.</p></li>
<li><p>Added “Grid-in-To-Address” (but marked as obsolete).</p></li>
<li><p>Extended the description of the Comment field, with pointers to
what can appear in the field.</p></li>
<li><p>Added explanation of base 91.</p></li>
<li><p>Added paragraph on lack of consistency in on-air units, and
default GPS datum = WGS84.</p></li>
<li><p>APRS Data Type Identifiers Table:</p></li>
</ul>
<blockquote>
<p>marked Shelter Data and Space Weather as reserved DTIs.</p>
<p>marked the <strong>-</strong> DTI as unused (previously erroneously
allocated to Killed Objects). marked the <strong>'</strong> DTI to mean
<em>Current</em> Mic-E data in Kenwood TM-D700 radios. marked the
<strong>‘</strong> DTI as <em>not used</em> in Kenwood TM-D700
radios.</p>
</blockquote>
<ul>
<li><p>Position Ambiguity: need only be specified in the latitude — the
longitude will have the same level of ambiguity.</p></li>
<li><p>Added the options of <strong>.../...</strong> and
<strong>/</strong> to express unknown course/speed.</p></li>
<li><p>Added DFS parameter table.</p></li>
<li><p>Added Quality table for BRG/NRQ data.</p></li>
<li><p>Position, DF and Compressed Report formats: split the format
diagrams into two parts (with and without timestamps).</p></li>
<li><p>DF Reports: added notes:</p></li>
</ul>
<blockquote>
<p>BRG/NRQ data is only valid when the symbol is
<strong>/\</strong>.</p>
<p>CSE=000 means the DF station is fixed, CSE non-zero means the station
is moving.</p>
</blockquote>
<ul>
<li><p>Compressed position reports: corrected the
multiplication/division constants for encoding/ decoding.</p></li>
<li><p>Mic-E chapter rewritten and expanded. Emphasized the need to
ensure that non-printing ASCII characters are not dropped. Corrected the
Mic-E telemetry data format.</p></li>
<li><p>Expanded the introductory description of Objects/Items. All
Objects must have a timestamp.</p></li>
<li><p>Added Area Object Extended Data field to Object and Item format
diagrams.</p></li>
<li><p>Added Object/Item format diagrams with compressed location
data.</p></li>
<li><p>Killed Objects/Items: now indicated by underscore after the
name.</p></li>
</ul>
<blockquote>
<p>(continued on the next page)</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 11%" />
<col style="width: 75%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>Date</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Doc Version</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Status / Major Changes</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td></td>
<td><blockquote>
<p>1.0.1m</p>
<p>(continued)</p>
</blockquote></td>
<td><ul>
<li><p>Re-categorized weather reports: Raw, Positionless and
Complete.</p></li>
<li><p>Added a statement that temperatures below zero are expressed as
-01 to -99.</p></li>
<li><p>Added the options of <strong>...</strong> and
<strong></strong> to express unknown weather parameter
values.</p></li>
<li><p>Corrected the storm data format. Also, central pressure is now
/ppppp (tenths of millibar).</p></li>
<li><p>Corrected the telemetry parameter data (now APRS
<em>messages</em> instead of AX.25 UI <em>beacons</em>).</p></li>
<li><p>Added optional comment field to the Telemetry (T)
format.</p></li>
<li><p>Added a section describing the handling of multiple message
acknowledgements.</p></li>
<li><p>Added a section on NTS radiograms.</p></li>
<li><p>Added Bulletin/Announcement implementation
recommendations.</p></li>
<li><p>Queries and Responses:</p></li>
</ul>
<blockquote>
<p>Query Names (e.g. APRSD): all upper-case.</p>
<p>A queried station need not respond if it has no relevant information
to send. A queried station should ignore any query type that it does not
recognize. APRSH: callsigns must be padded to 9 characters.</p>
</blockquote>
<ul>
<li><p>Added PING as a synonym of APRST.</p></li>
<li><p>Extended meteor scatter ERP beyond 810 watts, and added a lookup
table.</p></li>
<li><p>Maidenhead Locator: all letters must be transmitted in upper
case, but may be received in either upper or lower case.</p></li>
<li><p>Changed the definition of non-APRS packets — these are not APRS
Status Messages, but may optionally be treated as such.</p></li>
<li><p>APRS Symbols chapter substantially rewritten..</p></li>
<li><p>Added section on Symbol Precedence (where more than one symbol
appears in an APRS packet).</p></li>
<li><p>Clarified some of the descriptions in the APRS Symbol
Tables.</p></li>
<li><p>Added overlay capability to the \a symbol (ARES/RACES
etc).</p></li>
<li><p>Separated the 7-bit ASCII table from the Dec/Hex (0x80-0xff)
conversion table.</p></li>
<li><p>Added several new entries and a units conversion table to the
Glossary.</p></li>
<li><p>Added new references to NMEA sentence formats and Maidenhead
Locator formats.</p></li>
</ul></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 11%" />
<col style="width: 75%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>Date</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Doc Version</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Status / Major Changes</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>29 Aug 2000</p>
</blockquote></td>
<td><blockquote>
<p>1.0.1</p>
</blockquote></td>
<td><blockquote>
<p>Protocol Version 1.0. Approved public release.</p>
<p>Minor additions/changes to the draft 1.0.1m specification:</p>
</blockquote>
<ul>
<li><p>Added Foreword.</p></li>
<li><p>Replaced section on Map Views and Range Scale.</p></li>
<li><p>APRS Software Version No: added <strong>APD</strong>xxx (Linux
aprsd server).</p></li>
<li><p>APRS Data Type Identifier: Designated <strong>[</strong> as
Maidenhead grid locator (but noted as obsolete).</p></li>
<li><p>Position Ambiguity: added a bounding box example.</p></li>
<li><p>Compressed Position Formats: for course/speed, corrected the
range of possible values of the “c” byte to 0–89.</p></li>
<li><p>Mic-E: replaced the latitude example table, to show more
explicitly how the N/S/E/W/Long offset bits are encoded.</p></li>
<li><p>Mic-E: removed the paragraph stating that there must be a space
between the altitude and comment text — no space is required.</p></li>
<li><p>Mic-E: removed the note on inaccurate altitude data, as GPS
Selective Availability has been switched off.</p></li>
<li><p>Object Reports: added timestamps to some of the examples (an
Object Report must always have a timestamp).</p></li>
<li><p>Signposts: can be Objects or Items.</p></li>
<li><p>Storm Data: changed central pressure format to
<strong>/</strong>pppp (i.e. to the nearest millibar/hPascal).</p></li>
<li><p>Storm Data: Hurricane Brenda examples: inserted a leading zero in
the central pressure field (central pressure is 4 digits).</p></li>
<li><p>Telemetry Data: Added <strong>MIC</strong> as an alternative form
of Sequence Number. <strong>MIC</strong> may or may not be followed by a
comma.</p></li>
<li><p>Messages: added the reject message format.</p></li>
<li><p>Appendix 1: Agrelo format: changed the separator between Bearing
and Quality to <strong>/</strong>.</p></li>
<li><p>Symbol Table: changed <strong>/(</strong> symbol from “Cloudy” to
“Mobile Satellite Groundstation”.</p></li>
<li><p>Reformatted the Units Conversion Table.</p></li>
</ul></td>
</tr>
</tbody>
</table>

\*\*  
\*\*

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 11%" />
<col style="width: 75%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>Date</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Doc Version</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Status / Major Changes</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>29 Nov 2023</p>
</blockquote></td>
<td><blockquote>
<p>1.1c draft</p>
</blockquote></td>
<td><blockquote>
<p><del>Protocol Version 1.1. Approved public release.</del> NOT YET</p>
</blockquote>
<p>Merged in updates from <a
href="http://www.aprs.org/aprs11.html">http://www.aprs.org/aprs11.html</a></p>
<p>This copy of the APRS SPEC Addendum 1.1 was approved by the APRS-<a
href="http://www.aprs.org/aprs11/aprswg.txt"><strong>WG</strong></a> .
(Vote was called 30 July 2004).<br />
This APRS1.1 page is always maintained current such as the SYMBOLS and
TOCALLS links below.<br />
All new updates and additions to APRS since 2004 are found on the <a
href="http://www.aprs.org/aprs12.html"><strong>APRS 1.2</strong></a>
proposed draft addendum.</p>
<p><strong>APRS SPEC FINALIZED as of July 2004:</strong> This APRS
Specification version 1.1 represents additions, corrections, and
deletions since the original APRS1.0 spec of 21 June 2000. This edition
represents the state of the APRS protocol and its usage through July
2004. As of this date, the <a
href="http://www.aprs.org/aprs11/aprsstats.txt"><strong>state of the
APRS users</strong></a> showed almost 27,000 stations worldwide of
which:</p>
<ul>
<li><p>33% are fixed or home stations (includes WX stations)</p></li>
<li><p>36% are infrastructure (30% digis and 6% IGates)</p></li>
<li><p>31% are mobiles</p></li>
</ul>
<p><strong>CORRECTIONS:</strong></p>
<ul>
<li><p>Page 18 X1J exception. Abandoned/deprecated in 2012. Now the !
DTI is only at the beginning. ✔</p></li>
<li><p>Page 55 Mic-E Altitude. Optional altitude should be
<em>first</em> after the Mic-E <strong>type</strong> byte.
(<em>type</em> byte was extended in addendum 1.2) ✔</p></li>
<li><p>Page 25 refers to a 'WPT' NMEA sentence type. Curt, WE7U assumes
that should be 'WPL'? ✔</p></li>
<li><p>The Antenna Gain in the PHG format on page 28 is in dBi.
✔</p></li>
<li><p>Page 34 DF NRQ is not defined in spec. See original <a
href="http://www.aprs.org/APRS-docs/DF.TXT">DF.TXT</a> and <a
href="http://www.aprs.org/APRS-docs/PROTOCOL.TXT">PROTOCOL.TXT</a>
✔</p></li>
<li><p>Page 45, We now avoid the term "Mic-E Messages" and use the term
"Mic-E Position Comment" for these bits. ✔</p></li>
<li><p>Page 48, 100-109 goes to "u" instead of DEL and 110-179 is 38-107
going to "k" instead of DEL. ✔</p></li>
<li><p><strong>AX-25 Table:</strong> The AX-25 UI-Frame TABLE lists
Flags at the end of a packet being 2 bytes.<br />
. . The correct number is 1, and it may be shared with the next packet.
(From VK2TDS). ✔</p></li>
<li><p><strong>Default Paths of WIDE2-2, not RELAY or WIDE.</strong>
(page 11) (updated June 2006) ✔</p></li>
<li><p>Normally, <a
href="http://www.aprs.org/aprs11/HID.txt"><strong>HID should be
OFF</strong></a> in all APRS TNC's (page 91). ✔</p></li>
<li><p><a href="http://www.aprs.org/aprs11/SSIDs.txt"><strong>SSID
Conventions</strong></a> in user calls (page 95) ✔</p></li>
<li><p><strong>DATUM:</strong> Standard is WGS-84, but Continental
options such as OSG for the UK are OK.(page 21) See !DAO! concept in
APRS1.2 ✔</p></li>
<li><p><a href="http://www.aprs.org/aprs11/RangeScale.txt"><strong>Range
Scale:</strong></a> The standard <em>view size</em> descriptor for APRS
displays. ( <a
href="http://www.aprs.org/aprs11/APRSrangeX.gif"><strong>map
example</strong></a> ). (page 11) ✔</p></li>
<li><p><a href="http://www.aprs.org/aprs11/timestamps.txt"><strong>Time
Stamps on receipt</strong></a> (page 23). ✔</p></li>
</ul>
<p><strong>SYMBOL UPDATES:</strong></p>
<ul>
<li><p><a href="http://www.aprs.org/symbols.html"><strong>See all about
Symbols!</strong></a> ✔</p></li>
<li><p><a href="http://www.aprs.org/aprs11/SSIDs.txt"><strong>SSID
Conventions</strong></a> for quick visual identification ✔</p></li>
<li><p><a
href="http://www.aprs.org/symbols/symbolsX.txt"><strong>Updated SYMBOL
tables</strong></a>. . But see also all the APRS1.2 proposals <a
href="http://www.aprs.org/symbols/symbols-new.txt"><strong>for new
Symbol Extensions</strong></a> ✔</p></li>
<li><p><a
href="http://www.aprs.org/symbols/Overlayed-symbols.gif"><strong>Overlayable
Symbols</strong> subset.</a> . These were always defined as overlays in
the original APRS. ✔</p></li>
<li><p><a
href="http://www.aprs.org/symbols/symbols-background.txt"><strong>Upgrading
your symbol set</strong></a> gives background on fixing up your symbol
set. ✔</p></li>
<li><p><strong>JUST MOBILE PRIMARY SYMBOLS:</strong>
<em>!'&lt;=&gt;()*0CFOPRSUXY[\^abefgjkpsuv</em> . . . &lt;== [added !F\
]</p></li>
<li><p><strong>JUST MOBILE ALTERNATE SYMBOLS:</strong>
<em>&gt;KOS^ksuv</em>. . . . . . . . . . . . . . . . . . . . . .
&lt;==[removed /0An]</p></li>
<li><p><strong>JUST WEATHER PRIMARY SYMBOLS:</strong> <em>_ and
W</em></p></li>
<li><p><strong>JUST WEATHER ALTERNATE SYMBOLS:</strong>
<em>([*:&lt;@BDEFGHIJTUW_efgptwy{</em></p></li>
</ul>
<blockquote>
<p>I don’t know what theses JUST MOBILE …SYMBOLS are referring to.</p>
</blockquote>
<ul>
<li><p><strong>Timeout Old Stations (fade-to-gray)</strong>. Now 80 mins
instead of 2 hrs to account for stations via satellites (page 10).
✔</p></li>
<li><p><a href="http://www.aprs.org/symbols.html"><strong>Symbol
Attributes</strong></a> for map displays. (page 92) And Position
Ambiguity (p-24)</p></li>
<li><p><a href="http://www.aprs.org/digi-overlays.html"><strong>DIGI
Overlay Characters</strong></a> (new for page 11) ✔</p></li>
<li><p><a href="http://www.aprs.org/symbols/PHGmobile.txt"><strong>Have
PHG Range Circles</strong></a> to account for real-world PHG range due
to multipath &amp; fading. (page 29) <a
href="http://www.aprs.org/phg.html"><strong>See all about
PHG</strong></a> ✔</p></li>
<li><p><strong>New WAYPOINT symbol:</strong> Red dot (with overlay)
marks a mobile's destination. Drawn with a line between the mobile and
its waypoint destination. ✔</p></li>
</ul>
<p>OBJECT (and NAME) CLARIFICATIONS:</p>
<ul>
<li><p><a
href="http://www.aprs.org/aprs11/aprs-names-and-spaces.txt"><strong>Name/Call
equality and SPACES-in-names Manefesto</strong></a> ✔</p></li>
<li><p><a
href="http://www.aprs.org/aprs11/Objects.txt"><strong>OBJECTS:</strong></a>
Amplifying comments on ownership, killing and equality with stations.
(page58)</p></li>
<li><p><strong>OBJECT names should not have any punctuation</strong>
that cannot be converted to valid Wapoints on most GPSs. That would
depend on GPS Vendor. Where is this defined?</p></li>
<li><p><a href="http://www.aprs.org/aprs11/areaobjects.txt"><strong>AREA
Objects</strong></a> are poorly described in the spec. (page 60)
✔</p></li>
<li><p><a
href="http://www.wxsvr.net/APRS_Multiline_Protocol.html"><strong>Polygon
and Line OBJECTS</strong></a> (new for page 31) URL NOT VALID</p></li>
</ul>
<p>http://www.wxsvr.net/APRS_Multiline_Protocol.html</p>
<ul>
<li><p><strong>Compressed Objects:</strong> not recommended for use on
RF due to incompatibilities (page 58)<br />
For 1 foot precision, use $GPGGA, or 3rd party compressed posits, or the
proposed !DAO! format. ✔</p></li>
<li><p><strong>ITEM Format</strong> is not recommended on RF due to
incompatibilities (page 37) ✔</p></li>
</ul>
<p>UPDATED TABLES and INFO:</p>
<ul>
<li><p><a
href="http://www.aprs.org/aprs11/C-bits-SSID.txt"><strong>WB2OSZ
description of SSID - C bits</strong></a> ✔</p></li>
<li><p><a href="http://www.aprs.org/aprs11/tocalls.txt"><strong>TO-CALLS
and Version ID's</strong></a> updated (page 14) ✔</p></li>
<li><p><a
href="http://www.aprs.org/aprs11/expfmts.txt"><strong>Experimental
Formats</strong></a> (page 89) ✔</p></li>
</ul>
<p>WEATHER RELATED ISSUES:</p>
<ul>
<li><p><strong>Raw Weather Formats</strong> not recommended.
Microprocessors should convert to <em>complete</em> format on RF (pg 62)
✔</p></li>
<li><p><a href="http://www.aprs.org/aprs11/spec-wx.txt"><strong>Weather
Details</strong></a>. Amplifying comments on the original spec 1.0.
(page 64) ✔</p></li>
<li><p><a href="http://wxsvr.net/protocol.html"><strong>The WXSVR
Protocols</strong></a> managed by Dale Hugley. <a
href="http://www.aprs.org/aprs11/symbolWP.jpg"><strong>See
example</strong></a>. See description: <a
href="http://www.aprs.org/APRS-docs/MOBILE.TXT"><strong>MOBILE.TXT</strong></a>.
<strong>Site shutdown in 2009.. Skip this??? Danger of external
references.</strong></p></li>
</ul>
<p>APRS-IS (Internet System):</p>
<ul>
<li><p><strong>NOGATE and RFONLY</strong> in the RF DIGI field should
not be forwarded into the APRS-IS by IGates.</p></li>
<li><p><strong>!x!</strong> means <strong>no archive</strong>. Any
packet containing this string should not be archived by any of the
APRS-IS data bases. Positions, status or messages. IS THAT literally x
or is that a wildcard?</p></li>
<li><p><a href="http://core.aprs.net/"><strong>APRS-IS Core</strong></a>
and <a href="http://www.aprs2.net/"><strong>Tier-2</strong></a> servers
web pages and <a href="http://www.aprs-is.net"><strong>how they
work</strong></a>.</p></li>
<li><p><a href="http://www.aprs-is.net/q.htm"><strong>The
q-construct:</strong></a> Marks the source of entry of all packets into
the APRS-IS.</p></li>
<li><p><strong>IGATE Status Report Format:</strong> Left_bracket then
<em>"IGate, MSG_CNT=N, LOC_CNT=N"</em></p></li>
</ul>
<p><strong>APRS 1.1 SPEC Operating Conventions for the Good of the APRS
Network:</strong></p>
<ul>
<li><p><a href="http://www.aprs.org/fix14439.html"><strong>The New-N
Paradigm</strong></a> obsoleted old RELAY and WIDE and TRACE for
300-500% network improvement ✔</p></li>
<li><p><a href="http://www.aprs.org/VoiceAlert3.html"><strong>APRS
Voice-Alert</strong></a> for instant voice contact with any APRS
mobile</p></li>
<li><p><a
href="http://www.aprs.org/aprs11/replyacks.txt"><strong>Reply-ACKS
Algorithm:</strong></a> Really makes message QSO's FLY! (page 73)
✔</p></li>
<li><p><a
href="http://www.aprs.org/aprs11/hacks.txt"><strong>Recommended DIGI
paths</strong></a> and striving for <em>network protection</em> ahead of
too much user flexibility.</p></li>
<li><p><a
href="http://www.aprs.org/digis/digi-rates.txt"><strong>Digipeater ID
rates</strong></a> for optimum APRS networks</p></li>
<li><p>Under New-N, All APRS KPC-3+ compatible digis should use UIFLOOD
set to ID instead of NOID. Does configuration for one specific type of
previous Century equipment belong in the protocol spec or in the
application notes / FAQ?</p></li>
<li><p><a href="http://www.aprs.org/D7xx/d700digi.txt"><strong>D-700
Digipeater Settings</strong></a> are <em>different</em> from KPC-3+
settings (do not use UIFLOOD WIDE,28,NOID). Does configuration for one
specific type of previous Century equipment belong in the protocol spec
or in the application notes / FAQ?</p></li>
<li><p><strong>Auto-Answer messages</strong> are SPAM to the network in
most cases. To limit their impact, all original APRS clients adhered to
these rules for auto-answer messages:<br />
1) Any such AA message text should begin with AA:<br />
2) Any such AA message should not have a Line number (so it does not
cause acks)<br />
3) Any such AA message is only sent once on each incoming message packet
(no forced retries)<br />
4) Any such AA message should default to OFF on power up.<br />
5) Any such AA message should be canceled when the client detects the
return presence of the operator<br />
6) Any such AA message can be ended with a }yy REPLY-ACK if the software
supports it</p></li>
<li><p><strong>Where are these AA messages defined?</strong></p></li>
</ul>
<p><strong>The above is the complete APRS 1.1 Spec Addendum.</strong> It
was a compilation of all the accumulated feedback from users and authors
about errata, errors, typos and any omissions in the spec that had been
accumulated from the time of the original spec to June 2004. This APRS
errata page had been running continuously for years and was updated
whenever these items were discovered and each of these items were widely
published and discussed on the APRSSIG and the APRSSPEC working group
for public discussion. But it was decided best to finally Freeze the
accumulation in June 2004 as APRS1.1 and then after public posting
approve it.</p>
<p>Then to begin working on any new issues as APRS1.2.</p></td>
</tr>
</tbody>
</table>

> END OF DOCUMENT
