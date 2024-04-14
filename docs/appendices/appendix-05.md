# Appendix 5: Glossary

 **Altitude**

1. In Mic-E format, the altitude in meters relative to 10km below mean sea level.

2. In Comment text, the altitude in feet above mean sea level.

**Announcement**

An APRS message that is repeated a few times an hour, perhaps for several days.

**Announcement Identifier**

A single letter A-Z that identifies a particular announcement.

**Antenna Height**

In NMEA sentences, the height of the antenna in meters relative to mean sea level. (The antenna height in GPS NMEA sentences fluctuates wildly because of Selective Availability, and should only be used if DGPS correction is applied).

**APRS**
Automatic Packet Reporting System.

**APRS Data**

The data that follows the APRS Data Type Identifier in the AX.25 Information field and precedes the APRS Data Extension.

**APRS Data Extension**

A 7-byte extension to APRS Data. The Data Extension includes one of Course/Speed, Wind Direction/Wind Speed, Station Power/Antenna Effective Height/Gain/Directivity, Pre-Calculated Radio Range, DF Signal Strength/Effective Antenna Height/Gain, Area Object Descriptor.

**APRS Digipeater Path**

A digipeater path via repeaters with station names or more often aliases. Used in Mic-E compressed location format.

> FIXME-. What does MiC-E have to do with it?

**APRS Data Type Identifier**

The single-byte identifier that specifies what kind of APRS information is contained in the AX.25 Information field.

**Area Object**

A user-defined graphic object (circle, ellipse, triangle, box and line).

**ASCII**

American Standard Code for Information Interchange. A 7-bit character code conforming to ANSI X3.4 (1968) — see Appendix 3 for character definitions.

**AX.25**

Amateur Packet-Radio Link-Layer Protocol.

**Base 91**

Number base used to ensure that numeric values are transmitted as printable ASCII characters. To obtain the character string corresponding to a numeric value, divide the value progressively by decreasing powers of 91, and add 33 decimal to the result at each step. Printable characters are in the range

 **!**..**{**. Used in compressed lat/long and altitude computation.

**Bulletin**

An APRS message that is repeated several times an hour, for a small number of hours. A General Bulletin is addressed to no-one in particular. A Group Bulletin is addressed to a named group (e.g. WX).

**Bulletin Identifier**

A single digit 0-9 that identifies a particular bulletin.

**Destination Address field**

The AX.25 Destination Address field, which can contain an APRS destination callsign or Mic-E encoded data.

**DF Report**

A report containing DF bearing and range.

**DGPS**

Differential GPS. Used to overcome the errors arising from Selective Availability.

**DHM**

7-character timestamp: day-of-the-month, hour, minute, zulu or local time.

**DHMz**

7-character timestamp: day-of-the-month, hour, minute, zulu only.

**Digipeater**

A station that relays AX.25 packets. A chain of up to 8 digipeaters may be specified.

**Digipeater Addresses field**

The AX.25 field containing 0–8 digipeater callsigns (or aliases).

**Directivity**

The favored direction of an antenna. Used in the PHG Data Extension.

**DX Cluster**

A network host that collects and disseminates user reports of DX activity.

**ECHO**

A generic APRS digipeater callsign alias, for an HF digipeater.

**Effective Antenna Height**

The height of an antenna above the local terrain (not above sea level). A first-order indicator of the antenna’s effectiveness in the local area. Used in the PHG Data Extension.

**ERP**

Effective Radiated Power. Used in Status Reports containing Beam Heading and Power data (typically for meteor scatter use).

**FCS (Frame Check Sequence)**

A sequence of 16 bits that follows the AX.25 Information field, used to verify the integrity of the packet.

**GATE**

A gateway between HF and VHF APRS networks. Used primarily to relay long- distance HF APRS traffic onto local VHF networks.

**GGA Sentence**

A standard NMEA sentence, containing the receiving station’s lat/long position and antenna height relative to mean sea level, and other data.

**GLL Sentence**

A standard NMEA sentence, containing the receiving station’s lat/long position and other data.

**GMT**

Greenwich Mean Time (UTC -or- Zulu).

**GPS (Global Positioning System)**

A global network of 24 satellites that provide lat/long and antenna height of a receiving station.

**GPSxyz**

An APRS destination callsign that specifies a display symbol from either the Primary Symbol Table or the Alternate Symbol Table. Some symbols from the Alternate Symbol Table can be overlaid with a digit or a letter. Used by trackers that cannot specify the  symbol in the AX.25 Information field.

**GPSCnn**

An APRS destination callsign that specifies a display symbol from the Primary Symbol Table. The symbol can not be overlaid. Used by trackers that cannot specify the symbol in the AX.25 Information field.

**GPSEnn**

An APRS destination callsign that specifies a display symbol from the Alternate Symbol Table. The symbol can not be overlaid. Used by trackers that cannot specify the symbol in the AX.25 Information field.

**HMS**

1. In NMEA sentences, a 6-character timestamp: hour, minute, second UTC.

2. In APRS Data, a 7-character timestamp: hour, minute, second, zulu or local.

**ICQ**

International CQ chat.

**IGate**

A gateway between a VHF and/or HF APRS network and the Internet.

**Information field**

The AX.25 Information field containing APRS information.

**Item**

A type of display object.

**Item Report**

A report containing the location of an APRS Item.

**Killed Object**

An Object that an APRS user has assumed control of.

**knots**

International nautical miles per hour.

**KPC-3**

A Terminal Node Controller from Kantronics Co Inc.

**Longitude Offset**

An offset of +100 degrees longitude (used in Mic-E longitude computation).

**LORAN**

Long Range Navigation System (a terrestrial precursor to GPS).

**Maidenhead Locator**

A 4- or 6-character grid locator specifying a station’s position.

**MDHM**

8-byte timestamp: month, day, hour, minute (used in positionless weather station reports).

**Message**

A one-line text message addressed to a particular station.

**Message Acknowledgement**

An optional acknowledgement of receipt of amessage.

**Message Group**

A user-defined group to receive messages.

**Message Identifier**

A 1–5 character message identifier (typically aline number).

**Mic-E**

Originally Microphone Encoder, a unit that encodes location, course and speed information into a very short packet, for transmission when releasing the microphone PTT button. The Mic-> encoding algorithm is now used in other devices (e.g. in the PIC-E and the Kenwood TH-D7/TM-D700 radios).

**Mic-E Message Identifier**

A 3-bit identifier (A/B/C) specifying a standard Mic-E message or custom message code.

**Mic-E Message Code**

A 3-bit code specifying a Standard or Custom Mic-E message.

**MIM**

Micro Interface Module. A complete telemetry TNC transmitter on a chip.

**mph**

Miles per hour.

**Net Cycle Time**

The time within which it should be possible to gain the complete picture of APRS activity (typically 10, 20 or 30 minutes, depending on the number of digipeaters traversed and local conditions). Stations should not transmit status or position information more frequently unless mobile, or in response to a Query.

**NMEA**

National Marine Electronic Association (United States). Producer of the _NMEA 0183 Version 2.0_ specification that governs the format of Received Sentences from navigation equipment (such as GPS and LORAN receivers). See Appendix 6 for a reference to NMEA sentence  formats.

**NMEA (Received) Sentence**

The ASCII data stream received from navigation equipment (such as GPS receivers) conforming to the NMEA 0182 Version 2.0 specification. APRS supports five NMEA Sentences: GGA, GLL, RMC, VTG and WPL.

**NRQ (Number/Rate/Quality)**

A measure of confidence in DF Bearing reports.

**Null Position**

Default position to be reported if the actual position is unknown or indeterminate. The null position is 0° 0' 0" north, 0° 0' 0" west.

**NWS**

National Weather Service (United States).

**Object**

A display object that is (usually) not a station. For example, a weather front or a marathon runner.

**Object Report**

A report containing the position of an object, with optional timestamp and APRS Data Extension.

**PHG (Power-Height-Gain)**

APRS Data Extension specifying Power, Effective Antenna Height/Gain/Directivity.

**PIC**

Programmable Interface Controller.

**PIC-E**

A PIC implementation of the Mic-E microphone encoder.

**Position Ambiguity**

A reduction in the accuracy of APRS position information (implemented by replacing low-order lat/long digits with spaces). Used when the exact position is not known.

**Position Report**

A report containing lat/long position, optionally with timestamp and Data Extension.

**Pre-Calculated Radio Range**

A station’s estimate of omni-directional radio range (in miles). Used in compressed lat/long format.

**Query**

A request for information. Queries may be addressed to stations in general or to specific stations.

**Range Circle**

Usable radio range (in miles), computed from PHG data.

**Response**

A reply to a query.

**RMC Sentence**

A standard NMEA sentence, containing the receiving station’s lat/long position, course and speed, and other data.

**RTCM**

Radio Technical Commission for Maritime Services. The RTCM SC104 data format specification describes the requirements for differential GPS data correction.

**Selective Availability**

Deliberate GPS position dithering, introducing significant received position errors in latitude, longitude _and_ antenna height. Errors can be greatly reduced with differential GPS.

**Sentence**

See NMEA (Received) Sentence.

**Signpost**

A special signpost icon that displays user-defined variable information (such as a speed limit or mileage) as an overlay.

**Skywarn**

A weather spotter initiative coordinated by the United States National Weather Service.

**Source Address Field**

The AX.25 Source Address field, containing the callsign of the originating station. A non-zero SSID specifies a display symbol.

**Source Path Header**

The digipeater path followed prior to a packet entering a Third-Party Network.

**SPCL**

A generic APRS destination callsign used for special stations.

**SSID (Secondary Station Identifier)**

A number in the range 0-15, as an adjunct to an AX.25 address. If the SSID in a source address is non-zero, it specifies a display symbol. (This is used when the station is unable to specify the symbol in the AX.25 Destination Address field or Information field).

**Station Capabilities**

A list of station characteristics that is sent in reply to a query.

**Status Report**

A report containing station status information (and optionally a Maidenhead locator).

**Switch Stream Character**

A character normally used for switching TNC channels.

**Symbol**

A display icon. Consists of a Symbol Table Identifier/Symbol Code pair. Generically, **/$** represents a symbol from the Primary Symbol Table, and **\S** represents a symbol from the Alternate Symbol Table.

**Symbol Code**

A code for a symbol within a Symbol Table.

**Symbol Table Identifier**

An ASCII code specifying the Primary Symbol Table (**/**) or Alternate Symbol Table (**\\**).

The Symbol Table Identifier is also implicit in GPSCnn and GPSEnn destination callsigns.

**Target Footprint**

A target area for queries. The querying station asks for responses from stations within a specified number of miles of a lat/long position.

**TH-D7**

A combined VHF/UHF handheld radio and APRS-compatible TNC from Kenwood.

**TM-D700**

A combined VHF/UHF mobile radio and APRS-compatible TNC from Kenwood.

**Third Party Network**

A non-APRS network that does not understand AX.25 addresses (e.g. the Internet).

**Third-Party Header**

A Path Header with the Third-Party Network Identifier and the callsign of the receiving gateway inserted.

**TNC (Terminal Node Controller)**

A combined AX.25 packet assembler/disassembler and modem.

**Trace**

An APRS query that asks for the route taken by a packet to a specified station.

**Tracker**

A unit comprising a GPS receiver (to obtain the current geographical position) and a radio transmitter (to transmit the position to other stations).

**Tunneling**

Passing APRS AX.25 traffic through a third-party network that does not understand AX.25 addressing. The AX.25 addresses are carried as data (in the Source Path Header) through the tunneled network.

**UI-Frame**

AX.25 Unnumbered Information frame. APRS uses only UI-frames — that is, it operates entirely in connectionless (UNPROTO) mode.

**UNPROTO Path**

The digipeater path to the destination callsign.

**UTC**

Coordinated Universal Time (UTC -or- Zulu).

**VTG Received Sentence**

A standard NMEA sentence, containing the receiving station’s course and speed.

**WIDEn-N**

A generic APRS digipeater callsign alias, for a digipeater with wide area coverage (N=0-7). As a packet passes through a digipeater, the value of N is decremented by 1 until it reaches zero. The digipeater keeps a record of each packet (or its FCS) as it passes through, and will not digipeat the packet again if it has digipeated it already within the last 28 seconds. A digipeater should insert its own callsign in the digipeater path so a receiving station will know the path taken by the packet.

**WPL Sentence**

A standard NMEA sentence, containing waypoints.

**WX**

Weather.

**Ziplan**

A cheap twisted-pair LAN connecting PCs via their serial I/O ports. Designed for use in emergency situations.

**Zulu** UTC/GMT.

## **Units Conversion Table**

| **_To convert from_** | **_to_**             | **_multiply by_** | **_divide by_** |
| --------------------- | -------------------- | ----------------- | --------------- |
| feet                  | meters               | 0.3048            |                 |
| meters                | feet                 |                   | 0.3048          |
| miles                 | km                   | 1.609344          |                 |
| km                    | miles                |                   | 1.609344        |
| miles                 | nautical miles       | 0.8689762         |                 |
| nautical miles        | miles                |                   | 0.8689762       |
| miles per hour (mph)  | knots                | 0.8689762         |                 |
| knots                 | miles per hour (mph) |                   | 0.8689762       |
| knots                 | meters / second      | 0.51444’          |                 |
| meters / second       | knots                |                   | 0.51444’        |
| miles per hour (mph)  | meters / second      | 0.44704           |                 |
| meters / second       | miles per hour (mph) |                   | 0.44704         |

**Fahrenheit / Celsius Temperature Conversion Equations**
>
> F = ( C x 1.8 ) + 32 C = <u>( F – 32 ) x 5</u>
>
> 9
