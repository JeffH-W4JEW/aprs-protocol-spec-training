# APRS 101 Spec

+----------------------+------------------------------------------------+
| **Authors** | The APRS Working Group |
+======================+================================================+
| **Document Version** | > **Approved Version 1.0.1** |
+----------------------+------------------------------------------------+
| **Filename** | > aprs101.pdf |
+----------------------+------------------------------------------------+
| **Date of Issue** | > **29 August 2000** |
+----------------------+------------------------------------------------+
| **Copyright** | > ©2000 APRS Working Group All rights reserved |
+----------------------+------------------------------------------------+
| **Technical Editor** | > Ian Wade, G3NRW |
+----------------------+------------------------------------------------+

APRS PROTOCOL REFERENCE

# PREAMBLE

> **APRS Working**

**Group**

> The APRS Working Group is an unincorporated association whose members
> undertake to further the use and enhance the value of the APRS
> protocols by

a.

    publishing and maintaining a formal APRS Protocol Specification; (b)

    :   publishing validation tests and other tools to enable compliance
        with the Specification; (c) supporting an APRS Certification
        program; and (d) generally working to improve the capabilities
        of APRS within the amateur radio community.

+-----------------------+-----------------------+-------------+-----+
| **Document Version | | | |
| Number** | | | |
+=======================+=======================+=============+=====+
| > **APRS Protocol | > **Document | > **Draft** | |
| > Version\*\* **Major | > Release** **Minor | | |
| > Release** | > Release\*\* | | |
+-----------------------+-----------------------+-------------+-----+
| > P. | > p\. | > D | > d |
+-----------------------+-----------------------+-------------+-----+

- Document version number "1.2.3" refers to document release 3 covering

  : APRS Protocol Version 1.2.

- Document version number "1.2.3c" is draft "c" of that document.

```{=html}
<!-- -->
```

- Courier font ASCII characters in APRS data.

- ␣ ASCII space character.

- ... (ellipsis) zero or more characters.

- /\$ Symbol from Primary Symbol Table.

- \\\$ Symbol from Alternate Symbol Table.

- 0x hexadecimal (e.g. 0x1d).

- All callsigns are assumed to have SSID --0 unless otherwise
  specified.

- **Yellow marker** (appears as light gray background in hard copy).

  : Marks text of interest --- especially useful for highlighting
  single literal ASCII characters (e.g. **\"**) where they appear
  in APRS data.

- Shaded areas in packet format diagrams are optional fields.

# AUTHORS' FOREWORD

> This reference document describes what is known as _APRS Protocol
> Version 1.0_, and is essentially a description of how APRS operates
> today.
>
> It is intended primarily for the programmer who wishes to develop
> APRS- compliant applications, but will also be of interest to the
> ordinary user who wants to know more about what goes on "under the
> hood".
>
> It is not intended, however, to be a dry-as-dust, pedantic, RFC-style
> programming specification, to be read and understood only by the Mr
> Spocks of this world. We have included many items of general
> information which, although strictly not part of the formal protocol
> description, provide a useful background on how APRS is actually used
> on the air, and how it is implemented in APRS software. We hope this
> will put APRS into perspective, will make the document more readable,
> and will not offend the purists too much.
>
> It is important to realize how APRS originated, and to understand the
> design philosophy behind it. In particular, we feel strongly that APRS
> is, and should remain, a light-weight tactical system --- almost
> anyone should be able to use it in temporary situations (such as
> emergencies or mobile work or weather watching) with the minimum of
> training and equipment.
>
> This document is the result of inputs from many people, and collated
> and massaged by the APRS Working Group. Our sincere thanks go to
> everyone who has contributed in putting it together and getting it
> onto the street. If you discover any errors or omissions or misleading
> statements, please let us know
>
> --- the best way to do this is via the TAPR _aprsspec_ mailing list at
> [www.tapr.org.](http://www.tapr.org/)
>
> Finally, users throughout the world are continually coming up with new
> ideas and suggestions for extending and improving APRS. We welcome
> them.
>
> Again, the best way to discuss these is via the _aprsspec_ list.
>
> The APRS Working Group August 2000
>
> **Disclaimer** Like any navigation system, APRS is not infallible. No
> one should rely blindly on APRS for navigation, or in life-and-death
> situations. Similarly, this specification is not infallible.
>
> The members of the APRS Working Group have done their best to define
> the APRS protocol, but this protocol description may contain errors,
> or there may be omissions. It is very likely that not all APRS
> implementations will fully or correctly implement this specification,
> either today or in the future.
>
> We urge anyone using or writing a program that implements this
> specification to exercise caution and good judgement. The APRS Working
> Group and the specification's Editor disclaim all liability for injury
> to persons or property that may result from the use of this
> specification or software implementing it.

# THE STRUCTURE OF THIS SPECIFICATION

> This specification describes the overall requirements for developing
> software that complies with APRS Protocol Version 1.0. The information
> flow starts with the standard AX.25 UI-frame, and progresses downwards
> into more and more detail as the use of each field in the frame is
> explored.
>
> A key feature of the specification is the inclusion of dozens of
> detailed examples of typical APRS packets and related math
> computations.
>
> Here is an outline of the chapters:
>
> **Introduction to APRS** --- A brief background to APRS and a summary
> of its main features.
>
> **The APRS Design Philosophy** --- The fundamentals of APRS,
> highlighting its use as a real-time tactical communications tool, the
> timing of APRS transmissions and the use of generic digipeating.
>
> **APRS and AX.25** --- A brief refresher on the structure of the AX.25
>
> UI-frame, with particular reference to the special ways in which APRS
> uses the Destination and Source Address fields and the Information
> field.
>
> **APRS Data in the AX.25 Destination and Source Address Fields** ---
> Details of generic APRS callsigns and callsigns that specify display
> symbols and APRS software version numbers. Also a summary of how Mic-E
> encoded data is stored in the Destination Address field, and how the
> Source Address SSID can specify a display icon.
>
> **APRS Data in the AX.25 Information Field** --- Details of the
> principal constituents of APRS data that are stored in the Information
> field. Contains the APRS Data Type Identifiers table, and a summary of
> all the different types of data that the Information field can hold.
>
> **Time and Position Formats** --- Information on formats for
> timestamps, latitude, longitude, position ambiguity, Maidenhead
> locators, NMEA data and altitude.
>
> **APRS Data Extensions** --- Details of optional data extensions for
> station course/speed, wind speed/direction, power/height/gain,
> pre-calculated radio range, DF signal strength and Area Object
> descriptor.
>
> **Position and DF Report Data Formats** --- Full details of these
> report formats.
>
> **Compressed Position Report Data Formats** --- Full details of how
> station position and APRS data extensions are compressed into very
> short packets.
>
> **Mic-E Data Format** ---Mic-E encoding of station lat/long position,
> altitude, course, speed, Mic-E message code, telemetry data and APRS
> digipeater path into the AX.25 Destination Address and Information
> fields.
>
> **Object and Item Reports** --- Full information on how to set up APRS
> Objects and Items, and details of the encoding of Area Objects
> (circles, lines, ellipses etc).
>
> **Weather Reports** --- Full format details for weather reports from
> stand- alone (positionless) weather stations and for reports
> containing position information. Also details of storm data format.
>
> **Telemetry Data** --- A description of the MIM/KPC-3+ telemetry data
> format, with supporting information on how to tailor the
> interpretation of the raw data to individual circumstances.
>
> **Messages, Bulletins and Announcements** --- Full format information.
>
> **Station Capabilities, Queries and Responses** --- Details of the ten
> different types of query and expected responses.
>
> **Status Reports** --- The format of general status messages, plus the
> special cases of using a status report to contain meteor scatter beam
> heading/power and Maidenhead locator.
>
> **Network Tunneling** --- The use of the Source Path Header to allow
> tunneling of APRS packets through third-party networks that do not
> understand AX.25 addresses, and the use of the third-party Data Type
> Identifier.
>
> **User-Defined Data Format** --- APRS allows users to define their own
> data formats for special purposes. This chapter describes how to do
> this.
>
> **Other Packets** --- A general statement on how APRS is to handle any
> other packet types that are not covered by this specification.
>
> **APRS Symbols** ---How to specify APRS symbols and symbol overlays,
> in position reports and in generic GPS destination callsigns.
>
> **APRS Data Formats** --- An appendix containing all the APRS data
> formats collected together for easy reference.
>
> **The APRS Symbol Tables** ---A complete listing of all the symbols in
> the Primary and Alternate Symbol Tables.
>
> **ASCII Code Table** --- The full ASCII code, including decimal and
> hex codes for each character (the decimal code is needed for
> compressed lat/long and altitude computations), together with the hex
> codes for bit-shifted ASCII characters in AX.25 addresses (useful for
> Mic-E decoding and general on-air packet monitoring).
>
> **Glossary** --- A handy one-stop reference for the many APRS-specific
> terms used in this specification.
>
> **References** --- Pointers to other documents that are relevant to
> this specification.

# INTRODUCTION TO APRS

> **What is APRS?** APRS is short for _Automatic Position Reporting
> System_, which was designed by Bob Bruninga, WB4APR, and introduced by
> him at the 1992 TAPR/ ARRL Digital Communications Conference.
>
> Fundamentally, APRS is a packet communications protocol for
> disseminating live data to everyone on a network in real time. Its
> most visual feature is the combination of packet radio with the Global
> Positioning System (GPS) satellite network, enabling radio amateurs to
> automatically display the positions of radio stations and other
> objects on maps on a PC. Other features not directly related to
> position reporting are supported, such as weather station reporting,
> direction finding and messaging.
>
> APRS is different from regular packet in several ways:

- It provides maps and other data displays, for vehicle/personnel

  : location and weather reporting in real time.

- It performs all communications using a one-to-many protocol, so that

  : everyone is updated immediately.

- It uses generic digipeating, with well-known callsign aliases, so

  : that prior knowledge of network topology is not required.

- It supports intelligent digipeating, with callsign substitution to

  : reduce network flooding.

- Using AX.25 UI-frames, it supports two-way messaging and distribution

  : of bulletins and announcements, leading to fast dissemination of
  text information.

- It supports communications with the Kenwood TH-D7 and TM-D700 radios,

  : which have built-in TNC and APRS firmware.

**APRS**

> **Features**
>
> APRS runs on most platforms, including DOS, Windows 3.x, Windows
> 95/98, MacOS, Linux and Palm. Most implementations on these platforms
> support the main features of APRS:

- **Maps** --- APRS station positions can be plotted in real-time on

  : maps, with coverage from a few hundred yards to worldwide.
  Stations reporting a course and speed are dead-reckoned to their
  present position. Overlay databases of the locations of APRS
  digipeaters, US National Weather Service sites and even amateur
  radio stores are available. It is possible to zoom in to any
  point on the globe.

- **Weather Station Reporting** --- APRS supports the automatic display

  : of remote weather station information on the screen.

- **DX Cluster Reporting** --- APRS an ideal tool for the DX cluster

  : user. Small numbers of APRS stations connected to DX clusters
  can relay DX station information to many other stations in the
  local area, reducing overall packet load on the clusters.

- **Internet Access** --- The Internet can be used transparently to

  : cross-link local radio nets anywhere on the globe. It is
  possible to telnet into Internet APRS servers and see hundreds
  of stations from all over the world live. Everyone connected can
  feed their locally heard packets into the APRS server system and
  everyone everywhere can see them.

- **Messages** --- Messages are two-way messages with acknowledgement.

  : All incoming messages alert the user on arrival and are held on
  the message screen until killed.

- **Bulletins and Announcements** ---Bulletins and announcements are

  : addressed to everyone. Bulletins are sent a few times an hour
  for a few hours, and announcements less frequently but possibly
  over a few days.

- **Fixed Station Tracking** --- In addition to automatically tracking

  : mobile GPS/LORAN-equipped stations, APRS also tracks from manual
  reports or grid squares.

- **Objects** --- Any user can place an APRS Object on his own map, and

  : within seconds that object appears on all other station
  displays. This is particularly useful for tracking assets or
  people that are not equipped with trackers. Only one packet
  operator needs to know where things are (e.g. by monitoring
  voice traffic), and as he maintains the positions and movements
  of assets on his screen, all other stations running APRS will
  display the same information.

# THE APRS DESIGN PHILOSOPHY

> **Net Cycle Time** It is important to note that APRS is primarily a
> _real-time, tactical_ communications tool, to help the flow of
> information for things like special events, emergencies, Skywarn, the
> Emergency Operations Center and just plain in-the-field use under
> stress. But like the real world, for 99% of the time it is operating
> routinely, waiting for the unlikely serious event to happen.
>
> Anything which is done to enhance APRS must not undermine its ability
> to operate in local areas under stress. Here are the details of that
> philosophy:

1.  APRS uses the concept of a "net cycle time". This is the time within

    : which a user should be able to hear (at least once) all APRS
    stations within range, to obtain a more or less complete picture
    of APRS activity. The net cycle time will vary according to
    local conditions and with the number of digipeaters through
    which APRS data travels.

2.  The objective is to have a net cycle time of 10 minutes for local

    : use. This means that within 10 minutes of arrival on the scene,
    it is possible to captured the entire tactical picture.

3.  All stations, even fixed stations, should beacon their position at

    : the net cycle time rate. In a stress situation, stations are
    coming and going all the time. The position reports show not
    only where stations are without asking, but also that they are
    still active.

4.  It is not reasonable to assume that all APRS users responding to a

    : stress event understand the ramifications of APRS and the
    statistics of the channel --- user settings cannot be relied on
    to avoid killing a stressed net. Thus, to try to anticipate when
    the channel is under stress, APRS automatically adjusts its net
    cycle time according to the number of digipeaters in the UNPROTO
    path:

    - Direct operation (no digipeaters): 10 minutes (probably an
      event).
    - Via one digipeater hop: 10 minutes (probably an event).
    - Via two digipeater hops: 20 minutes.
    - Via three or more digipeater hops: 30 minutes.

5.  Since almost all home stations set their paths to three or more

    : digipeaters, the default net cycle time for routine daily
    operation is 30 minutes. This should be a universal standard
    that everyone can bank on

```{=html}
<!-- -->
```

6.  Since knowing where the digipeaters are located is fundamental to

    : APRS

```{=html}
<!-- -->
```

7.  If the net cycle time is too long, users will be tempted to send

    : queries for APRS stations. This will increase the traffic on the
    channel unnecessarily. Thus the recommended extremes for net
    cycle time are 10 and 30 minutes --- this gives network
    designers the fundamental assumptions for channel loading
    necessary for good engineering design.

- **Decay Algorithm** --- Transmit a new packet once and n seconds later.

  : Double the value of n for each new transmission. When n reaches
  the net cycle time, continue at that rate. Other factors besides
  "doubling" may be appropriate, such as for new message lines.

- **Fixed Rate** --- Transmit a new packet once and n seconds later.

  : Transmit it x times and stop.

- **Message-on-Heard** --- Transmit a _new_ packet according to either

  : algorithm above. If the packet is still valid, and has not been
  acknowledged, and the net cycle time has been reached, then the
  recipient is probably not available. However, if a packet is
  then subsequently heard from the recipient, try once again to
  transmit the packet.

- **Time-Out** --- This term is used to describe a time period beyond

  : which it is reasonable to assume that a station no longer exists
  or is off the air if no packets have been heard from it. A
  period of 2 hours is suggested as the nominal default timeout.
  This time-out is not used in any transmitting algorithms, but is
  useful in some programs to decide when to cease displaying
  stations as "active". Note that on HF, signals come and go, so
  decisions about activity may need to be more flexible.

1.  **RELAY** --- Every VHF APRS TNC is assumed to have an alias of

```{=html}
<!-- -->
```

2.  **ECHO** --- HF stations use the alias of ECHO as an alternative to

    : RELAY. (However, bearing in mind the nature of HF propagation,
    this has the potential of causing interference over a wide area,
    and should only be used sparingly by mobile stations).

3.  **WIDE** --- Every high-site digipeater is assumed to have an alias of

    : WIDE

```{=html}
<!-- -->
```

4.  **TRACE** --- Every high-site digipeater that is using callsign

    : substitution is assumed to have the alias of TRACE. These
    digipeaters self-identify packets they digipeat by inserting
    their own call in place of RELAY, WIDE or TRACE.

5.  **WIDEn-N** --- A digipeater that supports WIDEn-N digipeating will

    : digipeat any WIDEn-N packet that is "new" and will subtract 1
    from the SSID until the SSID reaches --0. The digipeater keeps a
    copy or a checksum of the packet and will not digipeat that
    packet again within (typically) 28 seconds. This considerably
    reduces the number of superfluous digipeats in areas with many
    digipeaters in radio range of each other.

6.  **GATE** --- This generic callsign is used by HF-to-VHF Gateway

    : digipeaters. Any packet heard on HF via GATE will be digipeated
    locally on VHF. This permits local networks to keep an eye on
    the national and international picture.

**Communicating**

> **Map Views Unambiguously**
>
> APRS is a tactical geographical system. To maximize its operational
> effectiveness and minimize confusion between operators of different
> systems, users need to have an unambiguous way to communicate to
> others the "location" and "size" (or area of coverage) of any map
> view.
>
> The APRS convention is by reference to a _center_ and _range_ which
> specify the geographical center and approximate radius of a circle
> that will fit in the map view independent of aspect ratio. The radius
> of the circle (in nautical miles, statute miles or km) is known as the
> "range scale". This convention gives all users a simple common basis
> for describing any specific map view to others over any communications
> medium or program.

# APRS AND AX.25

> **Protocols** At the link level, APRS uses the AX.25 protocol, as
> defined in _Amateur Packet-Radio Link-Layer Protocol_ (see Appendix 6
> for details), utilizing Unnumbered Information (UI) frames
> exclusively. This means that APRS runs in _connectionless_ mode,
> whereby AX.25 frames are transmitted without expecting any response,
> and reception at the other end is not guaranteed.
>
> At a higher level, APRS supports a messaging protocol that allows
> users to send short messages (one line of text) to nominated stations,
> and expects to receive acknowledgements from those stations.
>
> **The AX.25 Frame** All APRS transmissions use AX.25 UI-frames, with 9
> fields of data:
>
> Bytes:

- **Flag** --- The flag field at each end of the frame is the bit

  : sequence 0x7e that separates each frame.

- **Destination Address** --- This field can contain an APRS destination

  : callsign or APRS data. APRS data is encoded to ensure that the
  field conforms to the standard AX.25 callsign format (i.e. 6
  alphanumeric characters plus SSID). If the SSID is non-zero, it
  specifies a generic APRS digipeater path.

- **Source Address** --- This field contains the callsign and SSID of the

  : transmitting station. In some cases, if the SSID is non-zero,
  the SSID may specify an APRS display Symbol Code.

- **Digipeater Addresses** --- From zero to 8 digipeater callsigns may be

  : included in this field. **Note**: These digipeater addresses may
  be overridden by a generic APRS digipeater path (specified in
  the Destination Address SSID).

- **Control Field** --- This field is set to 0x03 (UI-frame).

- **Protocol ID** --- This field is set to 0xf0 (no layer 3 protocol).

- **Information Field** --- This field contains more APRS data. The first

  : character of this field is the APRS Data Type Identifier that
  specifies the nature of the data that follows.

- **Frame Check Sequence** --- The FCS is a sequence of 16 bits used for

  : checking the integrity of a received frame.

# APRS DATA IN THE AX.25 DESTINATION AND SOURCE ADDRESS FIELDS

> **The AX.25**
>
> **Destination Address Field**
>
> The AX.25 Destination Address field can contain 6 different types of
> APRS information:

- A generic APRS address.
- A generic APRS address with a symbol.
- An APRS software version number.
- Mic-E encoded data.
- A Maidenhead Grid Locator (obsolete).
- An Alternate Net (ALTNET) address.

+-----------+-----------+-----------+-----------+-----------+---+
| \*\* | \*\* | \* \*AP\** | \ | \* *DF\*M | |
| AIR\**\* | A | *DX\*R | \*\*BEACON | ICE\*\*\* | |
| †D | LL\*_\*DR | TCM_\*\* | | SYM\*\*\* | |
| GPS\*\*\* | ILL\*\*\* | | C | | |
| QST\*\*\* | QTH\*\*\* | | Q\*\*\*\*\* | | |
| | | | GPS\*\*\** | | |
| | | | *ID\*J | | |
| | | | AVA\*M | | |
| | | | AIL\*\*\* | | |
| | | | S | | |
| | | | KY\*\*\*SP | | |
| | | | ACE\**\* | | |
| | | | SPC\*\*\* | | |
+===========+===========+===========+===========+===========+===+
| > \*\* | > \*\*T | > \*\* | > - | > \*\* | |
| | | | | | |
| TEL\*\*\* | EST\*\*\* | TLM\*\*\* | *WX\*\*\* | ZIP\*\*\* | |
| | | | | | |
| | | | | : † | |
+-----------+-----------+-----------+-----------+-----------+---+

**Symbol**

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
>
> **APRS Software Version Number**
>
> The AX.25 Destination Address field can contain the version number of
> the APRS software that is running at the station. Knowledge of the
> version number can be useful when debugging.
>
> The following software version types are reserved (xx and xxx indicate
> a version number):
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
> **APRS** older versions of APRSdos
>
> **APRSM** older versions of MacAPRS
>
> **APRSW** older versions of WinAPRS
>
> **APS**xxx APRS+SA
>
> **APW**xxx WinAPRS
>
> **APX**xxx X-APRS
>
> **APY**xxx Yaesu radios (future)
>
> **APZ**xxx Experimental
>
> This table will be added to by the APRS Working Group.
>
> For example, a station using version 3.2.6 of MacAPRS could use the
>
> destination callsign APM326.
>
> The Experimental destination is designated for _temporary_ use only
> while a product is being developed, before a special APRS Software
> Version address is assigned to it.

**Mic-E Encoded**

**Data**

> Another alternative use of the AX.25 Destination Address field is to
> contain Mic-E encoded data. This data includes:

- The latitude of the station.

- A West/East Indicator and a Longitude Offset Indicator (used in

  : longitude computations).

- A Message Code.

- The APRS digipeater path.

**Maidenhead Grid**

> **Locator in Destination Address**
>
> The AX.25 Destination Address field may contain a 6-character
> Maidenhead Grid Locator. For example: **IO91SX**. This format is
> typically used by meteor scatter and satellite operators who need to
> keep packets as short as possible.
>
> This format is now obsolete.
>
> **Alternate Nets** Any other destination address not included in the
> specific generic list or the other categories mentioned above may be
> used in Alternate Nets (ALTNETs) by groups of individuals for special
> purposes. Thus they can use the APRS infrastructure for a variety of
> experiments without cluttering up the maps and lists of other APRS
> stations. Only stations using the same ALTNET address should see their
> data.
>
> **Generic APRS Digipeater Path**
>
> The SSID in the Destination Address field of all packets is coded to
> specify the APRS digipeater path.
>
> If the Destination Address SSID is --0, the packet follows the
> standard AX.25 digipeater ("VIA") path contained in the Digipeater
> Addresses field of the AX.25 frame.
>
> If the Destination Address SSID is non-zero, the packet follows one of
> 15 generic APRS digipeater paths.
>
> The SSID field in the Destination Address (i.e. in the 7th address
> byte) is encoded as follows:
>
> **APRS Digipeater Paths in Destination Address SSID**
>
> **The AX.25 Source Address SSID to specify Symbols**
>
> The AX.25 Source Address field contains the callsign and SSID of the
> originating station. If the SSID is --0, APRS does not treat it in any
> special way.
>
> If, however, the Source Address SSID is non-zero, APRS interprets it
> as a display icon. This is intended for use only with stand-alone
> trackers where there is no other method of specifying a display symbol
> or a destination address (e.g. MIM trackers or NMEA trackers).
>
> For more information, see Chapter 20: APRS Symbols.

# APRS DATA IN THE AX.25 INFORMATION FIELD

**Generic Data**

**Format**

Bytes:

> **APRS Data Type**

**Identifier**

> In general, the AX.25 Information field can contain some or all of the
> following information:

- APRS Data Type Identifier
- APRS Data
- APRS Data Extension
- Comment

+----------------+----------------+----------------+----------------+
| **Generic APRS | | | |
| Information | | | |
| Field** | | | |
+================+================+================+================+
| > **Data Type | > **APRS | > **APRS Data | > **Comment** |
| > ID** | > Data** | > Extension** | |
+----------------+----------------+----------------+----------------+
| > 1 | > n | > 7 | > n |
+----------------+----------------+----------------+----------------+

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

+----------------------+----------------------+----------------------+
| | **Possible APRS | **Possible APRS Data |
| | Data** | Extension** |
+======================+======================+======================+
| **Position** | > Time (DHM or HMS) | > Course and Speed |
| | > Lat/long | > |
| | > coordinates | > Power, Effective |
| | > | > Antenna Hei |
| | > Compressed lat/lon | |
| | | ght/Gain/Directivity |
| | g/course/speed/radio | |
| | | : Pre-Calculated |
| | : range/altitude | Radio Range |
| | Symbol Table ID | |
| | and Symbol Code | Omni DF Signal |
| | | Strength Storm |
| | Mic-E longitude, | Data (in Comment |
| | speed and | field) |
| | course, | |
| | telemetry or | |
| | status Raw GPS | |
| | NMEA sentence | |
| | | |
| | Raw weather | |
| | station data | |
+----------------------+----------------------+----------------------+
| **Direction | > Time (DHM or HMS) | > Course and Speed |
| Finding** | > Lat/long | > |
| | > coordinates | > Power, Effective |
| | > | > Antenna Hei |
| | > Compressed lat/lon | |
| | | ght/Gain/Directivity |
| | g/course/speed/radio | |
| | | : Pre-Calculated |
| | : range/altitude | Radio Range |
| | Symbol Table ID | |
| | and Symbol Code | Omni DF Signal |
| | | Strength |
| | | |
| | | Bearing and |
| | | |
| | | Number/Range/Quality |
| | | |
| | | : (in Comment |
| | | field) |
+----------------------+----------------------+----------------------+
| | > Object name | > Course and Speed |
+----------------------+----------------------+----------------------+
| | > Item name | > Power, Effective |
| | | > Antenna Hei |
| | | |
| | | ght/Gain/Directivity |
+----------------------+----------------------+----------------------+
| **Objects and** | > Time (DHM or HMS) | > Pre-Calculated |
| | > Lat/long | > Radio Range Omni |
| **Items** | > coordinates | > DF Signal Strength |
| | > | > Area Object |
| | > Compressed lat/lon | |
| | | |
| | g/course/speed/radio | |
| | | |
| | : range/altitude | |
+----------------------+----------------------+----------------------+
| | > Symbol Table ID | > Storm Data (in |
| | > and Symbol Code | > Comment field) |
+----------------------+----------------------+----------------------+
| | > Raw weather | |
| | > station data | |
+----------------------+----------------------+----------------------+
| **Weather** | > Time (MDHM) | > Wind Direction and |
| | > Lat/long | > Speed Storm Data |
| | > coordinates | > (in Comment field) |
| | > | |
| | > Compressed lat/lon | |
| | | |
| | g/course/speed/radio | |
| | | |
| | : range/altitude | |
| | Symbol Table ID | |
| | and Symbol Code | |
| | | |
| | Raw weather | |
| | station data | |
+----------------------+----------------------+----------------------+
| **Telemetry** | > Telemetry (non | |
| | > Mic-E) | |
+----------------------+----------------------+----------------------+
| | > Addressee | |
+----------------------+----------------------+----------------------+
| > **Messages, | > Message Text | |
| > Bulletins and | > Message Identifier | |
| > Announcements** | > | |
| | > Message | |
| | > Acknowledgement | |
| | > Bulletin ID, | |
| | > Announcement ID | |
+----------------------+----------------------+----------------------+
| | > Group Bulletin ID | |
+----------------------+----------------------+----------------------+
| **Queries** | > Query Type | |
| | > | |
| | > Query Target | |
| | > Footprint | |
| | > Addressee | |
| | > (Directed Query) | |
+----------------------+----------------------+----------------------+
| | > Position | > Course and Speed |
+----------------------+----------------------+----------------------+
| | > Object/Item | > Power, Effective |
| | | > Antenna Hei |
| | | |
| | | ght/Gain/Directivity |
+----------------------+----------------------+----------------------+
| | > Weather | > Pre-Calculated |
| | | > Radio Range |
+----------------------+----------------------+----------------------+
| | > Status | > Omni DF Signal |
| | | > Strength |
+----------------------+----------------------+----------------------+
| **Responses** | > Message Digipeater | > Area Object |
| | > Trace | > |
| | | > Wind Direction and |
| | | > Speed |
+----------------------+----------------------+----------------------+
| | > Stations Heard | |
+----------------------+----------------------+----------------------+
| | > Heard Statistics | |
+----------------------+----------------------+----------------------+
| | > Station | |
| | > Capabilities | |
+----------------------+----------------------+----------------------+
| **Status** | > Time (DHM zulu) | |
| | > Status text | |
| | > | |
| | > Meteor Scatter | |
| | > Beam Heading/Power | |
| | > Maidenhead Locator | |
| | > (Grid Square) | |
| | > Altitude (Mic-E) | |
| | > | |
| | > E-mail message | |
+----------------------+----------------------+----------------------+
| **Other** | > Third-Party | |
| | > forwarding Invalid | |
| | > Data/Test Data | |
+----------------------+----------------------+----------------------+

- **Altitude** in comment text (see Chapter 6: Time and Position

  : Formats), or in Mic-E status text (see Chapter 10: Mic-E Data
  Format).

- **Maidenhead Locator** (grid square), in a Mic-E status text field

  : (see Chapter 10: Mic-E Data Format) or in a Status Report (see
  Chapter 16: Status Reports).

- **Bearing and Number/Range/Quality** parameters (/BRG/NRQ), in DF

  : reports (see Chapter 7: APRS Data Extensions).

- **Area Object Line Widths** (see Chapter 11: Object and Item

  : Reports).

- **Signpost Objects** (see Chapter 11: Object and Item Reports).

- **Weather and Storm Data** (see Chapter 12: Weather Reports).

- **Beam Heading and Power**, in Status Reports (see Chapter 16: Status

  : Reports).

+------------------+-----+------------------------------------+
| 12345678 / 91^3^ | = | modulus **16**, remainder 288542 |
+==================+=====+====================================+
| > 288542 / 91^2^ | > = | > modulus **34**, remainder 6988 |
+------------------+-----+------------------------------------+
| > 6988 / 91^1^ | > = | > modulus **76**, remainder **72** |
+------------------+-----+------------------------------------+

# TIME AND POSITION FORMATS

> **Time Formats** APRS timestamps are expressed in three different
> ways:

- Day/Hours/Minutes format
- Hours/Minutes/Seconds format
- Month/Day/Hours/Minutes format

+------------------------+------------------+------------------------+
| | **No APRS** | **With APRS |
| | | Messaging** |
| | **Messaging** | |
+========================+==================+========================+
| **(Current/real-time) | > **!** | > **=** |
| Report without | | |
| timestamp** | | |
+------------------------+------------------+------------------------+
| **(Old/non-real-time) | > **/** | > **@** |
| Report with | | |
| timestamp** | | |
+------------------------+------------------+------------------------+

**Maidenhead**

> **Locator (Grid Square)**
>
> An alternative method of expressing a station's location is to provide
> a Maidenhead locator (grid square). There are four ways of doing this:

- In a Status Report --- e.g. IO91SX/-

```{=html}
<!-- -->
```

- In Mic-E Status Text --- e.g. IO91SX**/G**

```{=html}
<!-- -->
```

- In the Destination Address --- e.g. IO91SX. (obsolete).
- In AX.25 beacon text, with the **\[** APRS Data Type Identifier ---
  e.g.

```{=html}
<!-- -->
```

- In the comment text.
- In Mic-E format.

# APRS DATA EXTENSIONS

> A fixed-length 7-byte field may follow APRS position data. This field
> is an APRS Data Extension. The extension may be one of the following:

- CSE**/**SPD Course and Speed (this may be followed by a further 8

  : bytes containing DF bearing and Number/Range/Quality parameters)

- DIR**/**SPD Wind Direction and Wind Speed

- **PHG**phgd Station Power and Effective Antenna Height/Gain/

  : Directivity

- **RNG**rrrr Pre-Calculated Radio Range

- **DFS**shgd DF Signal Strength and Effective Antenna Height/Gain

- Tyy/Cxx Area Object Descriptor

## PHG Codes

> The 7-byte **PHG**phgd Data Extension specifies the transmitter power,
> effective antenna height-above-average-terrain, antenna gain and
> antenna directivity. APRS uses this information to plot radio range
> circles around stations.
>
> The 7 characters of this Data Extension are encoded as follows:
> Characters 1--3: **PHG** (fixed)
>
> Character 4: p Power code Character 5: h Height code Character 6: g
> Antenna gain code Character 7: d Directivity code
>
> The PHG codes are listed in the table below:

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
</colgroup>
<thead>
<tr class="header">
<th>phgd <strong>Co de:</strong></th>
<th><strong>0</strong></th>
<th><strong>1</strong></th>
<th><strong>2</strong></th>
<th><strong>3</strong></th>
<th><strong>4</strong></th>
<th><strong>5</strong></th>
<th><strong>6</strong></th>
<th><strong>7</strong></th>
<th><strong>8</strong></th>
<th><strong>9</strong></th>
<th><strong>Un its</strong></th>
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
<td>watts</td>
</tr>
<tr class="even">
<td>H eight</td>
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
<td>160</td>
<td>320</td>
<td>640</td>
<td>1280</td>
<td>2560</td>
<td>5120</td>
<td>feet</td>
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
<td>D irect ivity</td>
<td>omni</td>
<td><blockquote>
<p>45</p>
<p>NE</p>
</blockquote></td>
<td><blockquote>
<p>90</p>
<p>E</p>
</blockquote></td>
<td><p>135</p>
<blockquote>
<p>SE</p>
</blockquote></td>
<td><p>180</p>
<blockquote>
<p>S</p>
</blockquote></td>
<td><p>225</p>
<blockquote>
<p>SW</p>
</blockquote></td>
<td><p>270</p>
<blockquote>
<p>W</p>
</blockquote></td>
<td><p>315</p>
<blockquote>
<p>NW</p>
</blockquote></td>
<td><p>360</p>
<blockquote>
<p>N</p>
</blockquote></td>
<td></td>
<td>deg</td>
</tr>
</tbody>
</table>

## DFS Codes

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
</colgroup>
<thead>
<tr class="header">
<th>shgd <strong>Co de:</strong></th>
<th><strong>0</strong></th>
<th><strong>1</strong></th>
<th><strong>2</strong></th>
<th><strong>3</strong></th>
<th><strong>4</strong></th>
<th><strong>5</strong></th>
<th><strong>6</strong></th>
<th><strong>7</strong></th>
<th><strong>8</strong></th>
<th><strong>9</strong></th>
<th><strong>Un its</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Str ength</td>
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
<td>S-p oints</td>
</tr>
<tr class="even">
<td>H eight</td>
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
<td>160</td>
<td>320</td>
<td>640</td>
<td>1280</td>
<td>2560</td>
<td>5120</td>
<td>feet</td>
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
<td>D irect ivity</td>
<td>omni</td>
<td><blockquote>
<p>45</p>
<p>NE</p>
</blockquote></td>
<td><blockquote>
<p>90</p>
<p>E</p>
</blockquote></td>
<td><p>135</p>
<blockquote>
<p>SE</p>
</blockquote></td>
<td><p>180</p>
<blockquote>
<p>S</p>
</blockquote></td>
<td><p>225</p>
<blockquote>
<p>SW</p>
</blockquote></td>
<td><p>270</p>
<blockquote>
<p>W</p>
</blockquote></td>
<td><p>315</p>
<blockquote>
<p>NW</p>
</blockquote></td>
<td><p>360</p>
<blockquote>
<p>N</p>
</blockquote></td>
<td></td>
<td>deg</td>
</tr>
</tbody>
</table>

**Quality**

> DF reports contain an 8-byte field **/**BRG**/**NRQ that follows the
> CSE**/**SPD Data Extension, specifying the course, speed, bearing and
> NRQ (Number/Range/ Quality) value of the report. NRQ indicates the
> Number of hits, the approximate Range and the Quality of the report.
>
> For example, in:
>
> ...088/036/270/729... course = 88 degrees, speed = 36 knots,
>
> bearing = 270 degrees, N = 7, R = 2, Q = 9
>
> If N is 0, then the NRQ value is meaningless. Values of N from 1 to 8
> give an indication of the number of hits per period relative to the
> length of the time period --- thus a value of 8 means 100% of all
> samples possible got a hit. A value of 9 for N indicates to other
> users that the report is manual.
>
> The N value is not processed, but is just another indicator from the
> automatic DF units.
>
> The range limits the length of the line to the original map's scale of
> the sending station. The range is 2R so, for R=4, the range will be 16
> miles.
>
> Q is a single digit in the range 0--9, and provides an indication of
> bearing accuracy:
>
> If the course and speed parameters are not appropriate, they should
> have the value **000/000** or **\.../\...** or **␣␣␣/␣␣␣**.
>
> **Area Object Descriptor**
>
> The 7-byte Tyy/Cxx Data Extension is an Area Object Descriptor. The T
>
> parameter specifies the type of object (square, circle, triangle, etc)
> and the
>
> /C parameter specifies its fill color.
>
> Area Objects are described in Chapter 11: Object and Item Reports.

# POSITION AND DF REPORT DATA FORMATS

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
> station's latitude and longitude, together with course and speed or
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

- Speed is accurate to +/-1 mph up to about 40 mph and within 3% at 600

  : mph.

- Altitude in feet is accurate to +/- 0.4% from 1 foot to 3000 miles.

- Consistent one-algorithm processing of compressed latitude and

  : longitude.

- Improved position to 1 foot worldwide.

- Pre-calculated radio range, compressed to one byte.

- Potential 50% compression of every position format on the air.

- Potential 40% reduction of raw GPS NMEA data length.

- Additional 7-byte reduction for NEMA GGA altitudes.

- Support for TNC compression at the NMEA source (from the GPS

  : receiver).

- Digipeater compression of old NMEA trackers on the fly.

- Usage is optional in all cases.

**Compressed Data**

**Format**

Bytes:

> Compressed data may be generated in several ways:

- by APRS software.
- pre-entered manually into a digipeater's beacon text.
- by a digipeater converting raw tracker NMEA packets to compressed.

+----------+----------+----------+----------+----------+----------+
| \*\*Co | | | | | |
| mpressed | | | | | |
| | | | | | |
| Position | | | | | |
| | | | | | |
| Data\*\* | | | | | |
+==========+==========+==========+==========+==========+==========+
| > **Sym | > \*\*Co | > \*\*Co | \* | > \*\*Co | \*\*Comp |
| > Table | | | \*Symbol | | |
| > ID** | mpressed | mpressed | | mpressed | > |
| | | | > | | Type\*\* |
| | : | > | Code\*\* | > Course | > |
| | Lat\*\* | Long\*\* | | | > : T |
| | YYYY | > | | /S | |
| | | > | | peed\*\* | |
| | | : XXXX | | | |
+----------+----------+----------+----------+----------+----------+
| | | | | > \*\*Co | |
| | | | | | |
| | | | | mpressed | |
| | | | | | |
| | | | | : | |
| | | | | Radio | |
| | | | | | |
| | | | | R | |
| | | | | ange\*\* | |
+----------+----------+----------+----------+----------+----------+
| | | | | > \*\*Co | |
| | | | | | |
| | | | | mpressed | |
| | | | | | |
| | | | | : Al | |
| | | | | | |
| | | | | ti | |
| | | | | tude\*\* | |
+----------+----------+----------+----------+----------+----------+
| > 1 | > 4 | > 4 | > 1 | > 2 | > 1 |
+----------+----------+----------+----------+----------+----------+

Long = -180 + (27 x 91^3^ + 9 x 91^2^ + 68 x 91 + 22) / 190463

= -180 + (20346417 + 74529 + 6188 + 22) / 190463

> = -180 + 107.25
>
> = -72.75 degrees
>
> **Course/Speed, Pre-Calculated Radio Range and**

**Altitude**

> The two cs bytes following the Symbol Code character can contain
> either the compressed course and speed or the compressed
> pre-calculated radio range or the station's altitude. These two bytes
> are in base 91 format.
>
> In the special case of c = **␣** (space), there is no course, speed or
> range data, in which case the csT bytes are ignored.
>
> **Course/Speed** --- If the ASCII code for c is in the range **!** to
> **z** inclusive --- corresponding to numeric values in the range 0--89
> decimal (i.e. after subtracting 33 from the ASCII code) --- then cs
> represents a compressed course/speed value:
>
> course = **c** x 4
>
> speed = 1.08**s** -- 1
>
> For example, if the cs characters are **7P**, the corresponding values
> of **c** and **s** (after subtracting 33 from the ASCII character
> code) are 22 and 47 respectively. Substituting these values in the
> above equations:
>
> course = **22** x 4 = 88 degrees
>
> speed = 1.08**47** -- 1 = 36.2 knots
>
> **Pre-Calculated Radio Range** --- If c = **{**, then cs represents a
> compressed pre-calculated radio range value:
>
> range = 2 x 1.08**s**
>
> For example, if the cs bytes are **{?**, the ASCII code for **?** is
> 63, so the value of **s** is 30 (i.e. 63-33). Thus:
>
> range = 2 x 1.08**30**
>
> \~ 20 miles
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
> The T byte is not meaningful if the c byte is **␣** (space).

+--------+--------+--------+--------+--------+------+---+---+
| \* | | | | | | | |
| \ | | | | | | | |
| _Compr | | | | | | | |
| ession | | | | | | | |
| | | | | | | | |
| Type | | | | | | | |
| (T) | | | | | | | |
| | | | | | | | |
| Byte | | | | | | | |
| Fo | | | | | | | |
| rm | | | | | | | |
| at\*\* | | | | | | | |
+========+========+========+========+========+======+===+===+
| > 7 | > 6 | > 5 | > 4 | > 3 | > 2 | 1 | 0 |
+--------+--------+--------+--------+--------+------+---+---+
| \*Not | \*Not | \ | \*\*NMEA | > - | | | |
| | | _\*GPS | So | | | | |
| > | > | | urce\** | *Compr | | | |
| used\* | used\* | > F | | ession | | | |
| | | ix\*\* | | Or | | | |
| | | | | i | | | |
| | | | | gin\*\* | | | |
+--------+--------+--------+--------+--------+------+---+---+
| > 0 | > 0 | > 0 = | > 0 0 | > 0 0 | | | |
| | | > old | > = | > 0 = | | | |
| | | | > | > | | | |
| | | (last) | > | > Comp | | | |
| | | | other | | | | |
| | | | | ressed | | | |
+--------+--------+--------+--------+--------+------+---+---+
| | | > 1 = | > 0 1 | > 0 0 | | | |
| | | > c | > = | > 1 = | | | |
| | | | > GLL | > TNC | | | |
| | | urrent | | > | | | |
| | | | | > | | | |
| | | | | BText | | | |
+--------+--------+--------+--------+--------+------+---+---+
| | | | > 1 0 | > 0 1 | | | |
| | | | > = | > 0 = | | | |
| | | | > GGA | > So | | | |
| | | | | | | | |
| | | | | ftware | | | |
| | | | | | | | |
| | | | | > | | | |
| | | | | (DOS/ | | | |
| | | | | | | | |
| | | | | Mac/Wi | | | |
| | | | | n/+SA) | | | |
+--------+--------+--------+--------+--------+------+---+---+
| | | | > 1 1 | > 0 1 | | | |
| | | | > = | > 1 = | | | |
| | | | > RMC | > | | | |
| | | | | > \ | | | |
| | | | | [tbd\] | | | |
+--------+--------+--------+--------+--------+------+---+---+
| | | | | > 1 0 | | | |
| | | | | > 0 = | | | |
| | | | | > | | | |
| | | | | > KPC3 | | | |
+--------+--------+--------+--------+--------+------+---+---+
| | | | | > 1 0 | | | |
| | | | | > 1 = | | | |
| | | | | > | | | |
| | | | | > Pico | | | |
+--------+--------+--------+--------+--------+------+---+---+
| | | | | > 1 1 | | | |
| | | | | > 0 = | | | |
| | | | | > | | | |
| | | | | > | | | |
| | | | | Other | | | |
| | | | | > t | | | |
| | | | | | | | |
| | | | | racker | | | |
| | | | | | | | |
| | | | | > \ | | | |
| | | | | [tbd\] | | | |
+--------+--------+--------+--------+--------+------+---+---+
| | | | | > 1 1 | | | |
| | | | | > 1 = | | | |
| | | | | > | | | |
| | | | | > Digi | | | |
| | | | | | | | |
| | | | | peater | | | |
| | | | | | | | |
| | | | | > conv | | | |
| | | | | | | | |
| | | | | ersion | | | |
+--------+--------+--------+--------+--------+------+---+---+

+-------+----------+------------+----------+------------+
| / | YYYY | XXXX | \$ | csT |
+=======+==========+============+==========+============+
| **/** | **5L!!** | **\<\*e7** | > **\>** | > **7P\[** |
+-------+----------+------------+----------+------------+

- Subtract 33 from the ASCII code for each character:

```{=html}
<!-- -->
```

- Multiply **c** by 91 and add **s** to obtain **cs**: **cs** = 50 x 91

  : - 60

```{=html}
<!-- -->
```

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

> **Mic-E Data Format** In Mic-E data format, the station's position,
> course, speed and display symbol, together with an APRS digipeater
> path and Mic-E Message Code, are packed into the AX.25 Destination
> Address and Information fields.
>
> The Information field can also optionally contain either Mic-E
> telemetry data or Mic-E status. The Mic-E Status can contain the
> station's Maidenhead locator and altitude.
>
> Mic-E packets can be very short. At the minimum, with no callsigns in
> the Digipeater Addresses field and no optional telemetry data or Mic-E
> status text, a complete Mic-E packet is just 25 bytes long (excluding
> FCS and flags).
>
> Mic-E data format is not only used in the Microphone Encoder unit; it
> is also used in the PIC Encoder and in the Kenwood TH-D7 and TM-D700
> radios.
>
> **Mic-E Data Payload** The Mic-E data format allows a large amount of
> data to be carried in a very short packet. The data is split between
> the Destination Address field and the Information field of a standard
> AX.25 UI-frame.
>
> **Destination Address Field** --- The 7-byte Destination Address field
> contains the following encoded information:

- The 6 latitude digits.

- A 3-bit Mic-E message identifier, specifying one of 7 Standard Mic-E

  : Message Codes or one of 7 Custom Message Codes or an Emergency
  Message Code.

- The North/South and West/East Indicators.

- The Longitude Offset Indicator.

- The generic APRS digipeater path code.

```{=html}
<!-- -->
```

- The encoded longitude.

- The encoded course and speed.

- The display Symbol Code and Symbol Table Identifier.

- An optional field, containing either Mic-E telemetry data or a Mic-E

  : status text string. The status string can contain plain text,
  Maidenhead

```{=html}
<!-- -->
```

- Six encoded latitude digits specifying degrees (digits 1 and 2),

  : minutes (digits 3 and 4) and hundredths of minutes (digits 5 and
  6).

- 3-bit Mic-E message identifier (message bits A, B and C).

- North/South latitude indicator.

- Longitude offset (adds 0 degrees or 100 degrees to the longitude

  : computation in the Information field).

- West/East longitude indicator.

- Generic APRS digipeater path (encoded in the SSID).

```{=html}
<!-- -->
```

- 7 Standard messages
- 7 Custom messages
- 1 Emergency message

+----------+----------+----------+-----------------+-----------------+
| **A** | **B** | **C** | **Standard | **Custom Mic-E |
| | | | Mic-E Message | Message Type** |
| | | | Type** | |
+==========+==========+==========+=================+=================+
| > 1 | > 1 | > 1 | > M0: Off Duty | > C0: Custom-0 |
+----------+----------+----------+-----------------+-----------------+
| > 1 | > 1 | > 0 | > M1: En Route | > C1: Custom-1 |
+----------+----------+----------+-----------------+-----------------+
| > 1 | > 0 | > 1 | > M2: In | > C2: Custom-2 |
| | | | > Service | |
+----------+----------+----------+-----------------+-----------------+
| > 1 | > 0 | > 0 | > M3: Returning | > C3: Custom-3 |
+----------+----------+----------+-----------------+-----------------+
| > 0 | > 1 | > 1 | > M4: Committed | > C4: Custom-4 |
+----------+----------+----------+-----------------+-----------------+
| > 0 | > 1 | > 0 | > M5: Special | > C5: Custom-5 |
+----------+----------+----------+-----------------+-----------------+
| > 0 | > 0 | > 1 | > M6: Priority | > C6: Custom-6 |
+----------+----------+----------+-----------------+-----------------+
| > 0 | > 0 | > 0 | > Emergency | |
+----------+----------+----------+-----------------+-----------------+

+----------------+----------------+----------------+----------------+
| **First 3 | **Message | **Message | **Message** |
| Destination | Identifier | Type** | |
| Address | Bits A/B/C** | | |
| Bytes** | | | |
+================+================+================+================+
| **S32** | > Standard 1 / | > Standard | > M3: |
| | > 0 / 0 | | > Returning |
+----------------+----------------+----------------+----------------+
| **F2D** | > Custom 1 / 0 | > Custom | > C2: Custom-2 |
| | > / Custom 1 | | |
+----------------+----------------+----------------+----------------+
| **234** | > 0 / 0 / 0 | > Emergency | > Emergency |
+----------------+----------------+----------------+----------------+

**Mic-E Information**

**Field**

> The Information field is used to complete the Position Report that was
> begun in the Destination Address field. The encoding used is different
> from the destination address since the content is not constrained to
> be printable, shifted 7-bit ASCII, as it is in the address. However,
> full 8-bit binary is not used --- all values are offset by 28 and
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
> 'Current GPS data
>
> (but not used in Kenwood TM-D700 radios) .
>
> **\'** Old GPS data
>
> (or _Current_ GPS data in Kenwood TM-D700 radios).
>
> 0x1c Current GPS data (Rev. 0 beta units only). 0x1d Old GPS data
> (Rev. 0 beta units only).
>
> **IMPORTANT NOTE**: As explained in detail below, some of the bytes in
> the Information field are non-printing ASCII characters. In certain
> circumstances (such as incorrect TNC setup or inappropriate software),
> some of these non-printing characters may be dropped, making the
> Information field appear shorter than it really is. This will lead to
> incorrect decoding. (In particular, if the Information field appears
> to be less than 9 bytes long, the packet must be ignored).

**Longitude Degrees**

**Encoding**

> The **d+28** byte in the Information field contains the encoded value
> of the longitude degrees, in the range 0--179 degrees.
>
> (Note that for longitude values in the range 0--9 degrees, the
> longitude offset is +100 degrees):
>
> **Mic-E Longitude Degrees Encoding**
>
> Note from the table that the encoding is split into four separate
> pieces:

- 0--9 degrees: **d+28** is in the range 118--127 decimal, corresponding

  : to the ASCII characters **v** to **DEL**.

```{=html}
<!-- -->
```

- 10--99 degrees: **d+28** is in the range 38--127 decimal (corresponding

  : to the ASCII characters **&** to **DEL**), and the longitude
  offset is +0 degrees.

  \- 100--109 degrees: **d+28** is in the range 108--117 decimal,

  : corresponding to the ASCII characters **l** (lower-case letter
  "L") to **DEL**, and the longitude offset is +100 degrees.

  \- 110--179 degrees: **d+28** is in the range 38--127 decimal

  : (corresponding to the ASCII characters **&** to **DEL**), and
  the longitude offset is +100 degrees.

1.  subtract 28 from the **d+28** value to obtain **d**.
2.  if the longitude offset is +100 degrees, add 100 to **d**.
3.  subtract 80 if 180 ˜ **d** ˜ 189

```{=html}
<!-- -->
```

4.  or, subtract 190 if 190 ˜ **d** ˜ 199.

**Longitude Minutes**

**Encoding**

> The **m+28** byte in the Information field contains the encoded value
> of the longitude minutes, in the range 0--59 minutes:
>
> **Mic-E Longitude Minutes Encoding**
>
> Note from the table that the encoding is split into two separate
> pieces:

- 0--9 minutes: **m+28** is in the range 88--97 decimal, corresponding to

  : the ASCII characters **X** to **a**.

- 10--59 minutes: **m+28** is in the range 38--87 decimal (corresponding

  : to the ASCII characters **&** to **W**).

1.  subtract 28 from the **m+28** value to obtain **m**.
2.  subtract 60 if **m** ™ 60.

**Speed and Course**

**Encoding**

> The speed and course of a station are encoded in 3 bytes, designated
> **SP+28**, **DC+28** and **SE+28**.
>
> The speed is in the range 0--799 knots, and the course is in the range
> 0--360 degrees (0 degrees represents an unknown or indefinite course,
> and 360 degrees represents due north).
>
> The encoded speed and course are spread over the three bytes, as
> follows:

+----------------------+----------------------+----------------------+
| **Speed** | **Course** | |
+======================+======================+======================+
| > Encoded Speed | > Encoded Speed | > Encoded Course |
| > (hundreds/tens of | > (units) and | > (tens/units) |
| > knots) | > Encoded Course | |
| | > (hundreds of | |
| | > degrees) | |
+----------------------+----------------------+----------------------+
| > **SP+28** | > **DC+28** | > **SE+28** |
+----------------------+----------------------+----------------------+

- If the computed speed is ™ 800 knots, subtract 800.
- If the computed course is ™ 400 degrees, subtract 400.

**Data**

> If the first 9 bytes of the Information field contain '**(\_f n
> \"Oj/**, and the destination address specifies that the station is in
> the western hemisphere with a longitude offset of +100 degrees, then
> the data is decoded as follows:

- ' is the APRS Data Type Identifier for a Mic-E packet containing

  : current GPS data.

- **(** is the **d+28** byte. The **(** character has the value 40

  : decimal. Subtracting 28 gives 12. The longitude offset (in the
  destination address) is +100 degrees, so the longitude is 100 +
  12 = 112 degrees.

- **\_** is the **m+28** byte. The **\_** character has the value 95

  : decimal. Subtracting 28 gives 67. This is ™ 60, so subtracting
  60 gives a value of 7 minutes longitude.

- **f** is the **h+28** byte. The **f** character has the value 102

  : decimal. Subtracting 28 gives 74 hundredths of a minute.

```{=html}
<!-- -->
```

- **n** is the **SP+28** byte. The **n** character has the value 110

  : decimal. After subtracting 28, the result is 82. As this is ™
  80, a further 80 is subtracted, leaving a result of 2 tens of
  knots.

- **\"** is the **DC+28** byte. The **\"** character has the value 34

  : decimal. Subtracting 28 gives 6. Dividing this by 10 gives a
  quotient of 0 (units of speed). Adding the first part of the
  speed, multiplied by 10 (i.e. 20) to the quotient (0) gives a
  final computed speed of 20 knots.

```{=html}
<!-- -->
```

- **O** (upper-case letter "O") is the **SE+28** byte. The **O**

  : character has the value 79 decimal. Subtracting 28 gives 51.
  Adding this to the remainder calculated above, multiplied by 100
  (i.e. 200), gives the final value of 251 degrees for the course.

**Mic-E Telemetry**

**Data**

Bytes:

> The Information field may optionally contain either Mic-E telemetry
> data values or Mic-E status text.
>
> If the byte following the Symbol Table Identifier is one of the
> Telemetry Flag characters (**'**,**\'** or 0x1d), then telemetry data
> follows:

+------------+------------+---------+---------+---------+---------+
| \* | | | | | |
| \*Optional | | | | | |
| Mic-E | | | | | |
| | | | | | |
| Telemetry | | | | | |
| Data\*\* | | | | | |
+============+============+=========+=========+=========+=========+
| > - | > - | | | | |
| | | | | | |
| _Telemetry | \ | | | | |
| Flag_\* | \*Telemetry | | | | |
| | | | | | |
| | : Data | | | | |
| | | | | | |
| | Ch | | | | |
| | annels\*\* | | | | |
+------------+------------+---------+---------+---------+---------+
| > F | Ch 1 | > Ch 2 | > Ch 3 | > Ch 4 | > Ch 5 |
+------------+------------+---------+---------+---------+---------+
| > 1 | 1/2 | > 1/2 | > 1/2 | > 1/2 | > 1/2 |
+------------+------------+---------+---------+---------+---------+

# OBJECT AND ITEM REPORTS

> **Objects and Items** Any APRS station can manually report the
> position of an APRS entity (e.g. another station or a weather
> phenomenon). This is intended for situations where the entity is not
> capable of reporting its own position.
>
> APRS provides two types of report to support this:

- Object Reports
- Item Reports

**Object Report**

**Format**

> An Object Report has a _fixed_ 9-character Object name, which may
> consist of any printable ASCII characters.
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
>
> The position may be in lat/long or compressed lat/long format, and the
> report may also contain Extended Data.
>
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

> An Item Report has a _variable-length_ Item name, 3--9 characters
> long. The name may consist of any printable ASCII characters _except_ > **!** or **\_**.
>
> Item names are case-sensitive.
>
> The **)** is the APRS Data Type Identifier for an Item Report, and a
> **!** or **\_**
>
> separates the Item name from the rest of the report:
>
> **!** indicates a live Item.
>
> **\_** is the Item "kill" character.
>
> The position may be in lat/long or compressed lat/long format. There
> is no provision for a timestamp. The report may also contain Extended
> Data.
>
> The Comment field may contain any appropriate APRS data (see the
> _Comment Field_ section in Chapter 5: APRS Data in the AX.25
> Information Field).
>
> Bytes:
>
> Bytes:
>
> **Area Objects** Using the **\\l** symbol (i.e. the lower-case letter
> "L" symbol from the Alternate Symbol Table) it is possible to define
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
> ;OBJECT␣␣␣\*ddmm.hhN**\\**dddmm.hhW **l** Tyy/Cxx
>
> where:
>
> T is the type of object shape.
>
> /Cis the color of the object.
>
> yyis the square root of the latitude offset in 1/100ths of a degree.
>
> xxis the square root of the longitude offset in 1/100ths of a degree.
> The object type and color codes are as follows:
>
> The latitude/longitude position is the upper left corner of the
> object, and the offsets are relative to this position --- the yy
> offset is _down_ from this position and the xx offset is to the
> _right_ of this position. (An exception is the special case of a Type
> 6 line which is drawn down and to the _left_).
>
> Here are some examples of Object Position Reports. The latitude and
> longitude offsets are each one degree (i.e. 100/100ths of a degree),
> so yy = xx = --100 = 10.
>
> ;SEARCH␣␣␣\*092345z4903.50N**\\**07201.75W **l 710/310**
>
> A high intensity cyan filled ellipse, yy=10, xx=10
>
> ;SEARCH␣␣␣\*092345z4903.50N**\\**07201.75W **l 8101310**
>
> A low intensity violet filled triangle, yy=10, xx=10
>
> Further, with the line option (Type 1 and Type 6) it is possible to
> specify a "corridor" either side of the central line. The width of the
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
> Signpost Objects/Items (with the symbol **\\m**) display as a yellow
> box with a 1--3-character overlay on them. The overlay is specified by
> enclosing the 1--3 characters in braces in the comment field. Thus a
> signpost with {55} would appear as a sign with **55** on it.
>
> For example:
>
> )I91␣3N!4903.50N**\\**07201.75W**m{55}**
>
> This was originally designed for posting the speed of traffic past
> speed measuring devices, but can be used for any purpose.
>
> Implementation Recommendation: Signposts should not display any
> callsign or name, and to avoid clutter should only be displayed at
> close range.

**Obsolete Object**

**Format**

> Some stations transmit Object reports without the **;** APRS Data Type
> Identifier. This format is obsolete. Some software may still decode
> such data as an Object, but it should now be interpreted as a Status
> Report.

# WEATHER REPORTS

**Weather Report**

**Types**

> APRS is an ideal tool for reporting weather conditions via packet.
> APRS supports serial data transmissions from the Peet Brothers,
> Ultimeter and Davis home weather stations. It is even possible to
> mount an Ultimeter remotely with only a TNC and radio to report and
> plot conditions. APRS is also ideally suited for the Skywarn weather
> observer initiative.
>
> APRS supports three types of Weather Report:

- Raw Weather Report
- Positionless Weather Report
- Complete Weather Report

**Raw Weather**

**Reports**

> Raw weather data from a stand-alone weather station is contained in
> the Information Field of an APRS AX.25 frame:
>
> Bytes:
>
> **Positionless Weather Reports**
>
> Generic raw weather data from a stand-alone weather station is
> contained in the Information Field of an APRS AX.25 frame:
>
> Bytes:
>
> **APRS Software**

**Type**

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

**Weather Unit**

**Type**

> A Weather Report may contain a 2--4 character code uuuu for the type
> of weather station unit. The following codes have been allocated:
>
> **Dvs** = Davis
>
> **HKT** = Heathkit **PIC** = PIC device **RSW** = Radio Shack
>
> **U-II** = Original Ultimeter U-II (auto mode) **U2R** = Original
> Ultimeter U-II (remote mode) **U2k** = Ultimeter 500/2000
>
> **U2kr** = Remote Ultimeter logger
>
> **U5** = Ultimeter 500
>
> **Upkm** = Remote Ultimeter packet mode
>
> Users may specify any other 2--4 character code for devices not in
> this list.
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
> **L** = luminosity (in watts per square meter) 999 and below.
>
> **l** (lower-case letter "L") = luminosity (in watts per square meter)
> 1000 and above.
>
> (L is inserted in place of one of the rain values).
>
> **s** = snowfall (in inches) in the last 24 hours.
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
> **c\...s\...g\...** or **c␣␣␣s␣␣␣g␣␣␣**
>
> For example, Jim's rain gauge may produce a report like this:
>
> \_10090556c\...s\...g\...t\...P012Jim
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
> **Symbols with Raw and Positionless Weather Stations**
>
> Because Raw and Positionless Weather Reports do not contain a display
> symbol in the AX.25 Information field, it is possible to specify the
> symbol in a generic APRS destination address (e.g. GPSHW or GPSE63)
> instead.
>
> Alternatively, if the weather station is on a balloon, the SSID --11
> may be used in the source address (e.g. N0QBF-11).
>
> See Chapter 20: APRS Symbols for more detail on the usage of symbols.
>
> **Complete Weather Reports with Timestamp and**

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
> tropical storm winds (in nautical miles). ggg = radius of "whole gale"
> (i.e. 50 knot) winds (in
>
> nautical miles). Optional.
>
> Storm data will usually be included in an Object Report, but may also
> be included in a Position Report or an Item Report.
>
> The display symbol will be either:
>
> **\\@** Hurricane/Tropical Storm (current position)
>
> **/@** Hurricane (predicted future position)
>
> For example, the progress of Hurricane Brenda could be expressed in
> Object Reports like these:
>
> ;BRENDA␣␣␣\*092345z4903.50N**\\**07202.75W**@**088/036/HC/150\^200/0980\>090&030%040
>
> ;BRENDA␣␣␣\*100045z4905.50N**/**07201.75W**@**101/047/HC/104\^123/0980\>065&020%040
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
> The report Sequence Number is a 3-character value --- typically a
> 3-digit number, or the three letters **MIC**. In the case of **MIC**,
> there may or may not be a comma preceding the first analog data value.
>
> There are five 8-bit unsigned analog data values (expressed as 3-digit
> decimal numbers in the range 000--255), followed by a single 8-bit
> digital data value (expressed as 8 bytes, each containing **1** or
> **0**).
>
> The Kantronics KPC-3+ TNC and APRS Micro Interface Module (MIM) use
> this format.
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

**Parameter Name**

**Message**

> The Parameter Name message contains the names (N) associated with the
> five analog channels and the 8 digital channels. Its format is as
> follows:
>
> Bytes:
>
> **Note**: The field widths are not all the same (this is a legacy
> arising from earlier limitations in display screen width). Note also
> that the byte counts _include_ the comma separators where shown.
>
> The list can terminate after any field.
>
> **Unit/Label Message** The Unit/Label message specifies the units (U)
> for the analog values, and the labels (L) associated with the digital
> channels:
>
> Bytes:
>
> **Note**: Again, the field widths are not all the same, and the byte
> counts
>
> _include_ the comma separators where shown. The list can terminate
> after any field.
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
> a x v^2^ + b x v + c
>
> where v is the raw received analog value.
>
> For example, analog channel A1 in the above beacon examples relates to
> the battery voltage, expressed in hundredths of volts, and a = 0, b =
> 5.2, c = 0. If the raw received value v is 199, then the voltage is
> calculated as:
>
> voltage = 0 x 199^2^ + 5.2 x 199 + 0
>
> = 1034.8 hundredths of a volt
>
> = 10.348 volts
>
> **Bit Sense/ Project Name**
>
> **Message**
>
> The Bit Sense/Project Name message contains two types of information:

- An 8-bit pattern of ones and zeros, specifying the sense of each

  : digital channel that matches the corresponding label.

- The name of the project associated with the telemetry station.

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
> printable ASCII characters except **\|**, **\~** or **{**.
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
>
> **Message Acknowledgement**

Bytes:

> A message acknowledgement is similar to a message, except that the
> message text field contains just the letters **ack**, and this is
> followed by the Message Number being acknowledged.
>
> **Message Rejection** If a station is unable to accept a message, it
> can send a **rej** message instead of an **ack** message:
>
> Bytes:
>
> **Multiple Acknowledgements**
>
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
>
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
> printable ASCII characters except **\|** or **\~**.
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

+----------+----------+----------+----------+----------+----------+
| \ | | | | | |
| _\*Group | | | | | |
| | | | | | |
| Bulletin | | | | | |
| | | | | | |
| Fo | | | | | |
| rmat\*\* | | | | | |
+==========+==========+==========+==========+==========+==========+
| **:** | **BLN** | \ | \ | > **:** | \ |
| | | _\*Group | _\*Group | | _\*Group |
| | | | | | |
| | | Bulletin | > | | Bulletin |
| | | | Name\*\* | | |
| | | : | | | : Text |
| | | ID\*\* | | | (max |
| | | | | | 67 |
| | | n | | | |
| | | | | | chara |
| | | | | | |
| | | | | | ct |
| | | | | | ers)\*\* |
+----------+----------+----------+----------+----------+----------+
| 1 | > 3 | > 1 | > 5 | > 1 | > 0-67 |
+----------+----------+----------+----------+----------+----------+
| Example | | | | | |
| | | | | | |
| :BLN4WX␣ | | | | | |
| ␣␣:Stand | | | | | |
| by your | | | | | |
| | | | | | |
| > snowpl | | | | | |
| | | | | | |
| owsGroup | | | | | |
| | | | | | |
| bulletin | | | | | |
| | | | | | |
| > number | | | | | |
| > | | | | | |
| > : 4 | | | | | |
| > to | | | | | |
| > | | | | | |
| the | | | | | |
| > WX | | | | | |
| > | | | | | |
| > group. | | | | | |
| > | | | | | |
| > | | | | | |
| > (Note | | | | | |
| > > the | | | | | |
| > | | | | | |
| > filler | | | | | |
| > | | | | | |
| > spaces | | | | | |
| > | | | | | |
| > : in | | | | | |
| > | | | | | |
| the | | | | | |
| > | | | | | |
| group | | | | | |
| > | | | | | |
| > name). | | | | | |
+----------+----------+----------+----------+----------+----------+

**Format**

> Some stations transmit bulletins and announcements without the **:**
> APRS Data Type Identifier. This format is obsolete. Some software may
> still decode such data as a bulletin or announcement, but it should
> now be interpreted as a Status Report.
>
> **Bulletin and Announcement Implementation Recommendations**
>
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
> This can be implemented in an APRS display system as a "Bulletin
> Screen". Everyone has this screen, and anyone can post or update lines
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

  : and may arrive out of sequence. They must be sorted before
  presentation on the Bulletin Screen, and re-sorted if necessary
  when each new line arrives. This includes sorting by originating
  callsign and Bulletin/ Announcement ID.

- **Replacement**: Stations sending bulletins/announcements can send

  : new lines to replace lines sent earlier, re-using the original
  Bulletin/ Announcement IDs. (Note: It is only necessary to
  re-send replacement lines. It is not necessary to re-send the
  whole bulletin/announcement). Receipt of a new line with the
  same Bulletin/Announcement ID as one already received from the
  same station should result in the existing line being
  overwritten (replaced).

- **Clearing**: A user should be able to clear any or all of the

  : bulletins/ announcements from the Bulletin Screen once he has
  read them. Any bulletins/announcements that are still valid will
  re-appear in due course because of the way they are redundantly
  re-transmitted.

- **Alerts**: On receipt of any new or replacement line for the

  : Bulletin Screen, an alarm should be sounded and re-sounded
  periodically until the user acknowledges it. Thus, this vital
  information is "pushed" to the operator. There is no excuse for
  not being aware of the current bulletin/ announcement state ---
  this is important in the hurried and crisis-laden scenario of an
  APRS event.

- **Logging**: Old bulletins/announcements should be logged in

  : sequential APRS log files in case they are subsequently needed.

# STATION CAPABILITIES, QUERIES AND RESPONSES

> **Station Capabilities** A station may define a set of one or more
> attributes of the station, known as Station Capabilities. The station
> transmits its capabilities in response to an IGATE query (see below),
> using the **\<** Data Type Identifier.
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
> messages transmitted, and LOC_CNT is the number of "local" stations
> (those to which the IGate will pass messages in the local RF network).
>
> **Queries and Responses**
>
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

+-------------------+-----------------------+-----------------------+
| **Query Type** | **Query** | **Response** |
+===================+=======================+=======================+
| > **APRS** | > General --- All | > Station's position |
| | > stations query | > and status |
+-------------------+-----------------------+-----------------------+
| > **APRSD** | > Directed --- Query | > List of stations |
| | > an individual | > heard direct |
| | > station for | |
| | > stations heard | |
| | > direct | |
+-------------------+-----------------------+-----------------------+
| > **APRSH** | > Directed --- Query | > Position of heard |
| | > if an individual | > station as an APRS |
| | > station has heard a | > Object, plus heard |
| | > particular station | > statistics for the |
| | | > last 8 hours |
+-------------------+-----------------------+-----------------------+
| > **APRSM** | > Directed --- Query | > All outstanding |
| | > an individual | > messages for the |
| | > station for | > querying station |
| | > outstanding | |
| | > unacknowledged or | |
| | > undelivered | |
| | > messages | |
+-------------------+-----------------------+-----------------------+
| > **APRSO** | > Directed --- Query | > Station's Objects |
| | > an individual | |
| | > station for its | |
| | > Objects | |
+-------------------+-----------------------+-----------------------+
| > **APRSP** | > Directed --- Query | > Station's position |
| | > an individual | |
| | > station for its | |
| | > position | |
+-------------------+-----------------------+-----------------------+
| > **APRSS** | > Directed --- Query | > Station's status |
| | > an individual | |
| | > station for its | |
| | > status | |
+-------------------+-----------------------+-----------------------+
| > **APRST** or | > Directed --- Query | > Route trace |
| > | > an individual | |
| > **PING?** | > station for a trace | |
| | > (i.e. path by which | |
| | > the packet was | |
| | > heard) | |
+-------------------+-----------------------+-----------------------+
| > **IGATE** | > General --- Query | > IGate station |
| | > all Internet | > capabilities |
| | > Gateways | |
+-------------------+-----------------------+-----------------------+
| > **WX** | > General --- Query | > Weather report (and |
| | > all weather | > the station's |
| | > stations | > position if it is |
| | | > not included in the |
| | | > Weather Report) |
+-------------------+-----------------------+-----------------------+

+-------+-------+-------+-------+-------+-------+-------+-------+---+
| \ | | | | | | | | |
| \*\*Ge | | | | | | | | |
| neral | | | | | | | | |
| | | | | | | | | |
| Query | | | | | | | | |
| | | | | | | | | |
| For | | | | | | | | |
| ma | | | | | | | | |
| t\*\* | | | | | | | | |
+=======+=======+=======+=======+=======+=======+=======+=======+===+
| **?** | > | **?** | **T | | | | | |
| | \*\* | | arget | | | | | |
| | | | F | | | | | |
| | Query | | ootpr | | | | | |
| | | | int** | | | | | |
| | : T | | | | | | | |
| | | | | | | | | |
| | yp | | | | | | | |
| | e\*\* | | | | | | | |
+-------+-------+-------+-------+-------+-------+-------+-------+---+
| | | | > | **,** | **L | **,** | **Rad | |
| | | | \*\* | | ong** | | ius** | |
| | | | | | | | | |
| | | | La | | | | | |
| | | | t\*\* | | | | | |
+-------+-------+-------+-------+-------+-------+-------+-------+---+
| > 1 | > n | > 1 | > n | > 1 | > n | > 1 | > 4 | |
+-------+-------+-------+-------+-------+-------+-------+-------+---+
| Exa | | | | | | | | |
| mples | | | | | | | | |
| | | | | | | | | |
| Query | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| Ty | | | | | | | | |
| | | | | | | | | |
| pical | | | | | | | | |
| | | | | | | | | |
| > Res | | | | | | | | |
| | | | | | | | | |
| ponse | | | | | | | | |
| | | | | | | | | |
| > ? | | | | | | | | |
| | | | | | | | | |
| APRS? | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| /0 | | | | | | | | |
| | | | | | | | | |
| 92345 | | | | | | | | |
| z4903 | | | | | | | | |
| .50N/ | | | | | | | | |
| 07201 | | | | | | | | |
| . | | | | | | | | |
| 75W\> | | | | | | | | |
| | | | | | | | | |
| > Ge | | | | | | | | |
| | | | | | | | | |
| neral | | | | | | | | |
| | | | | | | | | |
| : q | | | | | | | | |
| | | | | | | | | |
| uery, | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| with | | | | | | | | |
| > | | | | | | | | |
| > | | | | | | | | |
| > sta | | | | | | | | |
| | | | | | | | | |
| ndard | | | | | | | | |
| | | | | | | | | |
| posit | | | | | | | | |
| | | | | | | | | |
| > and | | | | | | | | |
| > | | | | | | | | |
| > | | | | | | | | |
| : s | | | | | | | | |
| | | | | | | | | |
| tatus | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| re | | | | | | | | |
| | | | | | | | | |
| p | | | | | | | | |
| ly.\> | | | | | | | | |
| 09234 | | | | | | | | |
| 5zNet | | | | | | | | |
| Co | | | | | | | | |
| ntrol | | | | | | | | |
| C | | | | | | | | |
| enter | | | | | | | | |
| | | | | | | | | |
| > ?AP | | | | | | | | |
| | | | | | | | | |
| RS? | | | | | | | | |
| **␣** | | | | | | | | |
| 34. | | | | | | | | |
| 02,-1 | | | | | | | | |
| 17.15 | | | | | | | | |
| ,0200 | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| /340 | | | | | | | | |
| | | | | | | | | |
| 2.78N | | | | | | | | |
| 11714 | | | | | | | | |
| .02W- | | | | | | | | |
| | | | | | | | | |
| > Ge | | | | | | | | |
| | | | | | | | | |
| neral | | | | | | | | |
| | | | | | | | | |
| query | | | | | | | | |
| | | | | | | | | |
| > for | | | | | | | | |
| > | | | | | | | | |
| > sta | | | | | | | | |
| | | | | | | | | |
| tions | | | | | | | | |
| | | | | | | | | |
| : w | | | | | | | | |
| | | | | | | | | |
| ithin | | | | | | | | |
| | | | | | | | | |
| : a | | | | | | | | |
| t | | | | | | | | |
| | | | | | | | | |
| arget | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| foot | | | | | | | | |
| | | | | | | | | |
| print | | | | | | | | |
| \ | | | | | | | | |
| >Digi | | | | | | | | |
| on | | | | | | | | |
| | | | | | | | | |
| > low | | | | | | | | |
| | | | | | | | | |
| power | | | | | | | | |
| | | | | | | | | |
| > of | | | | | | | | |
| > r | | | | | | | | |
| | | | | | | | | |
| adius | | | | | | | | |
| | | | | | | | | |
| > 200 | | | | | | | | |
| | | | | | | | | |
| miles | | | | | | | | |
| | | | | | | | | |
| > cen | | | | | | | | |
| | | | | | | | | |
| tered | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| on | | | | | | | | |
| | | | | | | | | |
| 34.02 | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| de | | | | | | | | |
| | | | | | | | | |
| grees | | | | | | | | |
| | | | | | | | | |
| : n | | | | | | | | |
| | | | | | | | | |
| orth, | | | | | | | | |
| | | | | | | | | |
| : 1 | | | | | | | | |
| | | | | | | | | |
| 17.15 | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| de | | | | | | | | |
| | | | | | | | | |
| grees | | | | | | | | |
| | | | | | | | | |
| west, | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| with | | | | | | | | |
| > | | | | | | | | |
| > | | | | | | | | |
| > sta | | | | | | | | |
| | | | | | | | | |
| ndard | | | | | | | | |
| | | | | | | | | |
| posit | | | | | | | | |
| | | | | | | | | |
| > and | | | | | | | | |
| > | | | | | | | | |
| > | | | | | | | | |
| : s | | | | | | | | |
| | | | | | | | | |
| tatus | | | | | | | | |
| | | | | | | | | |
| : r | | | | | | | | |
| | | | | | | | | |
| eply. | | | | | | | | |
| | | | | | | | | |
| (Note | | | | | | | | |
| | | | | | | | | |
| > the | | | | | | | | |
| > | | | | | | | | |
| > : | | | | | | | | |
| le | | | | | | | | |
| | | | | | | | | |
| ading | | | | | | | | |
| | | | | | | | | |
| space | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| in | | | | | | | | |
| | | | | | | | | |
| | | | | | | | | |
| the | | | | | | | | |
| | | | | | | | | |
| | | | | | | | | |
| lati | | | | | | | | |
| | | | | | | | | |
| tude, | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| as | | | | | | | | |
| | | | | | | | | |
| | | | | | | | | |
| its | | | | | | | | |
| | | | | | | | | |
| value | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| is | | | | | | | | |
| | | | | | | | | |
| | | | | | | | | |
| posi | | | | | | | | |
| | | | | | | | | |
| tive, | | | | | | | | |
| | | | | | | | | |
| > see | | | | | | | | |
| > | | | | | | | | |
| > : | | | | | | | | |
| be | | | | | | | | |
| | | | | | | | | |
| low). | | | | | | | | |
| | | | | | | | | |
| > ?I | | | | | | | | |
| | | | | | | | | |
| GATE? | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| \<IG | | | | | | | | |
| | | | | | | | | |
| ATE,M | | | | | | | | |
| S | | | | | | | | |
| G_CN | | | | | | | | |
| T=43, | | | | | | | | |
| L | | | | | | | | |
| OC_C | | | | | | | | |
| NT=14 | | | | | | | | |
| | | | | | | | | |
| > Ge | | | | | | | | |
| | | | | | | | | |
| neral | | | | | | | | |
| | | | | | | | | |
| query | | | | | | | | |
| | | | | | | | | |
| > for | | | | | | | | |
| | | | | | | | | |
| IGate | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| stat | | | | | | | | |
| | | | | | | | | |
| ions, | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| with | | | | | | | | |
| > | | | | | | | | |
| > | | | | | | | | |
| : a | | | | | | | | |
| > | | | | | | | | |
| St | | | | | | | | |
| | | | | | | | | |
| ation | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| Ca | | | | | | | | |
| | | | | | | | | |
| pabil | | | | | | | | |
| ities | | | | | | | | |
| r | | | | | | | | |
| eply. | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| ?WX? | | | | | | | | |
| > | | | | | | | | |
| > : | | | | | | | | |
| \_ | | | | | | | | |
| | | | | | | | | |
| 10090 | | | | | | | | |
| 556c2 | | | | | | | | |
| 20s00 | | | | | | | | |
| 4g005 | | | | | | | | |
| t0 | | | | | | | | |
| 77... | | | | | | | | |
| | | | | | | | | |
| Query | | | | | | | | |
| | | | | | | | | |
| > for | | | | | | | | |
| > | | | | | | | | |
| > : | | | | | | | | |
| we | | | | | | | | |
| | | | | | | | | |
| ather | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| stat | | | | | | | | |
| | | | | | | | | |
| ions, | | | | | | | | |
| | | | | | | | | |
| > | | | | | | | | |
| with | | | | | | | | |
| > | | | | | | | | |
| > | | | | | | | | |
| : a | | | | | | | | |
| | | | | | | | | |
| stand | | | | | | | | |
| ard/0 | | | | | | | | |
| 90556 | | | | | | | | |
| z4903 | | | | | | | | |
| .50N/ | | | | | | | | |
| 07201 | | | | | | | | |
| . | | | | | | | | |
| 75W\> | | | | | | | | |
| We | | | | | | | | |
| ather | | | | | | | | |
| R | | | | | | | | |
| eport | | | | | | | | |
| | | | | | | | | |
| reply | | | | | | | | |
| | | | | | | | | |
| > (wi | | | | | | | | |
| | | | | | | | | |
| thout | | | | | | | | |
| | | | | | | | | |
| : a | | | | | | | | |
| | | | | | | | | |
| posit | | | | | | | | |
| ion), | | | | | | | | |
| | | | | | | | | |
| > fol | | | | | | | | |
| | | | | | | | | |
| lowed | | | | | | | | |
| | | | | | | | | |
| : | | | | | | | | |
| by | | | | | | | | |
| a | | | | | | | | |
| | | | | | | | | |
| | | | | | | | | |
| sta | | | | | | | | |
| | | | | | | | | |
| ndard | | | | | | | | |
| | | | | | | | | |
| : p | | | | | | | | |
| | | | | | | | | |
| osit. | | | | | | | | |
+-------+-------+-------+-------+-------+-------+-------+-------+---+

- North and east coordinates are positive values, indicated by a

  : leading **␣**

```{=html}
<!-- -->
```

- South and west coordinates are negative values.

- The radius of the footprint is in miles, expressed as a fixed 4-digit

  : number in whole miles.

**Directed Station**

**Queries**

> Queries addressed to individual stations are in APRS message format
> (except that they never include a message identifier). The addressee
> is the callsign of the station being queried.
>
> The message text is the Query Type. This is followed optionally by
> another callsign --- this callsign does not need filler spaces as it
> is at the end of the data.
>
> Bytes:

# STATUS REPORTS

> A Status Report announces the station's current mission or any other
> single line status to everyone. The report is contained in the AX.25
> Information field, and starts with the **\>** APRS Data Type
> Identifier.
>
> The report may optionally contain a timestamp.
>
> **Note**: The timestamp can _only_ be in DHM _zulu_ format.
>
> The status text occupies the rest of the Information field, and may be
> up to 62 characters long (if there is no timestamp in the report) or
> 55 characters (if there is a timestamp). The text may contain any
> printable ASCII characters except **\|** or **\~**.
>
> Bytes:
>
> Although the status will usually be plain language text, there are two
> cases where the report can contain special information which can be
> decoded:

- Beam Heading and Power
- Maidenhead grid locator

**Power**

> It is useful to include beam heading and ERP in packets in meteor
> scatter work. To keep packets as short as possible, these parameters
> are encoded into two characters, as follows:
>
> H = beam heading / 10
>
> (H=0--9 for 0--90 degrees, and A--Z for 100--350 degrees).
>
> P = ERP code.
>
> The HP value appears as the _last_ two characters of the status text,
> preceded by the **\^** character --- for example, **\^B7** means a
> beam heading of 110 degrees and an ERP of 490 watts.
>
> The HP value may be combined with the Maidenhead grid locator (as
> described below), or with any other plain language status text.
>
> **Status Report with Maidenhead Grid**

**Locator**

> The Maidenhead grid locator may be 4 or 6 characters long, and must
> immediately follow the **\>** Data Type Identifier.
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

**Transmitting Status**

**Reports**

> Each station should only transmit a Status Report once every net cycle
> time (i.e. once every 10, 20 or 30 minutes), or in response to a
> query.

# NETWORK TUNNELING AND THIRD-PARTY DIGIPEATING

> **Third-Party Networks**
>
> APRS provides a mechanism for formatting packets that are to be
> transported through third-party (i.e. non AX.25) networks, such as the
> Internet, an Ethernet LAN or a direct wire connection.
>
> These networks do not understand APRS source, destination and
> digipeater addresses, so it is necessary to send them as data, along
> with the original data being transmitted.
>
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
> identify the sender --- this is needed when acknowledging receipt of a
> message, for example. Knowledge of the source path is also useful in
> diagnosing network problems.
>
> Bytes:
>
> The Source Path Header may be in either of two formats, known as the
> "TNC-2" format and the "AEA" format (so called because when TNC-2 or
> AEA-compatible TNCs are operating in terminal MONitor mode they
> automatically produce headers in these formats).
>
> The APRS Working Group has agreed to move towards standardization on
> the "TNC-2" format in future implementations.
>
> In most cases, AEA TNCs will produce Source Path Headers in "TNC-2"
> format when BBSMSGS is set to ON.

Bytes:

Bytes:

> The formats of these headers are as follows:

+-----------+----------+-----------+-----------+-----------+---+
| \ | | | | | |
| _\*Source | | | | | |
| Path | | | | | |
| Header | | | | | |
| --- | | | | | |
| | | | | | |
| "TNC-2" | | | | | |
| | | | | | |
| F | | | | | |
| ormat\*\* | | | | | |
| | | | | | |
| An | | | | | |
| | | | | | |
| asterisk | | | | | |
| | | | | | |
| follows | | | | | |
| the d | | | | | |
| igipeater | | | | | |
| | | | | | |
| callsign | | | | | |
| heard. | | | | | |
+===========+==========+===========+===========+===========+===+
| \ | > **\>** | > \*\*De | > \*\*0-8 | > **:** | |
| _\*Source | | | > Digi | | |
| | | stination | | | |
| > | | | pe | | |
| Callsign | | : C | aters\*\* | | |
| > | | | | | |
| > | | al | | | |
| (-\*\*SS | | lsign\*\* | | | |
| | | | | | |
| ID**)** | | > - | | | |
| | | | | | |
| | | _(-SS | | | |
| | | ID)_\* | | | |
+-----------+----------+-----------+-----------+-----------+---+
| | | | > **,** | > \*\*D | |
| | | | | | |
| | | | | igipeater | |
| | | | | | |
| | | | | : C | |
| | | | | | |
| | | | | al | |
| | | | | lsign\*\* | |
| | | | | | |
| | | | | > \*\*(- | |
| | | | | | |
| | | | | **SSID | |
| | | | | **)(_)_\* | |
+-----------+----------+-----------+-----------+-----------+---+
| > 1-9 | > 1 | > 1-9 | > 0-81 | > 1 | |
+-----------+----------+-----------+-----------+-----------+---+
| Example | | | | | |
| | | | | | |
| WB4APR-14 | | | | | |
| \ | | | | | |
| >APRS,REL | | | | | |
| A | | | | | |
| Y\*,WIDE: | | | | | |
| | | | | | |
| > (WIDE d | | | | | |
| | | | | | |
| igipeater | | | | | |
| | | | | | |
| "unused") | | | | | |
+-----------+----------+-----------+-----------+-----------+---+

+-----------+-----------+-----------+-----------+----------+---+
| \ | | | | | |
| _\*Source | | | | | |
| Path | | | | | |
| Header | | | | | |
| --- "AEA" | | | | | |
| | | | | | |
| F | | | | | |
| ormat\*\* | | | | | |
| | | | | | |
| An | | | | | |
| | | | | | |
| asterisk | | | | | |
| | | | | | |
| follows | | | | | |
| the | | | | | |
| source or | | | | | |
| d | | | | | |
| igipeater | | | | | |
| | | | | | |
| callsign | | | | | |
| heard. | | | | | |
+===========+===========+===========+===========+==========+===+
| **Source | > \*\*0-8 | > **\>** | > \*\*De | > **:** | |
| C | > Digi | | | | |
| allsign** | | | stination | | |
| | pe | | | | |
| > \*\*(- | aters\*\* | | : C | | |
| | | | | | |
| **SSID | | | al | | |
| **)(_)_\* | | | lsign\*\* | | |
| | | | | | |
| | | | > - | | |
| | | | | | |
| | | | _(-SS | | |
| | | | ID)_\* | | |
+-----------+-----------+-----------+-----------+----------+---+
| | > **\>** | > \*\*D | | | |
| | | | | | |
| | | igipeater | | | |
| | | | | | |
| | | : C | | | |
| | | | | | |
| | | al | | | |
| | | lsign\*\* | | | |
| | | | | | |
| | | > \*\*(- | | | |
| | | | | | |
| | | **SSID | | | |
| | | **)(_)\*\* | | | |
+-----------+-----------+-----------+-----------+----------+---+
| > 1-10 | > 0-81 | > 1 | > 1-9 | > 1 | |
+-----------+-----------+-----------+-----------+----------+---+
| Example | | | | | |
| | | | | | |
| WB4APR-14 | | | | | |
| \>R | | | | | |
| ELAY\*\>W | | | | | |
| I | | | | | |
| DE\>APRS: | | | | | |
| | | | | | |
| > (WIDE d | | | | | |
| | | | | | |
| igipeater | | | | | |
| | | | | | |
| "unused") | | | | | |
+-----------+-----------+-----------+-----------+----------+---+

- The Third-Party Network Identifier (e.g. TCPIP). This is a dummy

  : "callsign" that identifies the nature of the third-party
  network.

- The callsign of the receiving gateway station, followed by an

  : asterisk.

```{=html}
<!-- -->
```

- WB4APR-14 wants to send a message via the Internet to G3NRW.
- The nearest Internet gateway to WB4APR-14 is K4HG, reachable via a

```{=html}
<!-- -->
```

- The nearest Internet gateway to G3NRW is G9RXG. The Process:
- In the normal way, WB4APR-14 builds a message packet that contains:

```{=html}
<!-- -->
```

- WB4APR-14 transmits the packet via his UNPROTO path RELAY,WIDE.

- The Internet gateway K4HG happens to receive this packet from the

  : RELAY

```{=html}
<!-- -->
```

- K4HG builds a new packet that contains the source path and the

  : original message:

```{=html}
<!-- -->
```

- K4HG sends this packet (using telnet) to an APRServer on the

  : Internet.

- All Internet gateways throughout the world that are connected to the

  : APRServe network (including G9RXG) receive the packet.

- G9RXG converts the packet into a Third-Party packet:

```{=html}
<!-- -->
```

- G9RXG transmits the packet over the local APRS network.

- G3NRW receives the packet, strips out the Third-Party Header, and

  : discovers that the packet contains a message for him. From the
  header, G3NRW then establishes that the acknowledgement is to go
  back to WB4APR-14.

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
> **{**APRS Data Type Identifier.
>
> UA one-character User ID.
>
> XA one-character user-defined packet type.
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
> non-standard packet crashing other authors' programs. In keeping with
> the spirit of the APRS protocol, authors are encouraged to make these
> formats public. The APRS Working Group will maintain a web site
> defining all of the assigned User IDs, and either the packet formats
> provided by the author, or links to their
>
> own web sites which define their formats.
>
> Generally, all formats using this method will be considered optional.
> No program is required to decode any of these packets, and must ignore
> any it does not decode. However, it is possible that in the future
> some of these formats may prove to be of sufficient utility and
> interest to the entire APRS community that they will be specifically
> included in future versions of the APRS protocol.

# OTHER PACKETS

**Invalid Data or**

> **Test Data Packets**
>
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
> Data Type Identifier **\>**) has been received from a station it will
> not be overwritten by other non-APRS packets from that station.

# APRS SYMBOLS

> **Three Methods** There are three methods of specifying an APRS symbol
> (display icon):

- In the AX.25 Information field.
- In the AX.25 Destination Address.
- In the SSID of the AX.25 Source Address.

```{=html}
<!-- -->
```

- Primary Symbol Table
- Alternate Symbol Table

```{=html}
<!-- -->
```

- **/\$** or **\\\$** --- for symbols in the Information field.

- **GPSxyz** --- for generic Destination addresses containing symbols.

- **GPSCnn** or **GPSEnn** --- another form of generic Destination

  : addresses containing systems.

**Field**

> A symbol in the AX.25 Information field is a combination of a
> one-character Symbol Table Identifier and a one-character Symbol Code.
>
> For example, in the Position Report:
> \@092345z4903.50N**/**07201.75W**\>**088/036...
>
> the forward slash **/** is the Symbol Table Identifier and the **\>**
> character is the Symbol Code (in this case representing a "car" icon)
> from the selected table.
>
> The Symbol Table Identifier character selects one of the two Symbol
> Tables, or it may be used as single-character (alpha or numeric)
> overlay, as follows:

+--------------------------------+------------------------------------+
| **Symbol Table Identifier** | **Selected Table or Overlay |
| | Symbol** |
+================================+====================================+
| > **/** | > Primary Symbol Table (mostly |
| | > stations) |
+--------------------------------+------------------------------------+
| > **\\** | > Alternate Symbol Table (mostly |
| | > Objects) |
+--------------------------------+------------------------------------+
| > **0**-**9** | > Numeric overlay. Symbol from |
| | > Alternate Symbol Table |
| | > (_uncompressed_ lat/long data |
| | > format) |
+--------------------------------+------------------------------------+
| > **a**-**j** | > Numeric overlay. Symbol from |
| | > Alternate Symbol Table |
| | > (_compressed_ lat/long data |
| | > format only). i.e. a-j maps to |
| | > 0-9 |
+--------------------------------+------------------------------------+
| > **A**-**Z** | > Alpha overlay. Symbol from |
| | > Alternate Symbol Table |
+--------------------------------+------------------------------------+

**Field**

> Where the Symbol Table Identifier is 0-9 or A-Z (or a-j with
> _compressed_ position data only), the symbol comes from the
> _Alternate_ Symbol Table, and is overlaid with the identifier (as a
> single digit or a capital letter).
>
> For example, in the _uncompressed_ Position Report:
>
> \@092345z4903.50N**3**07201.75W**\>**...
>
> the digit **3** following the latitude will cause the number "3" to be
> overlaid on top of the "car" icon (**Note**: Because the symbol is
> overlaid, the **\>** Symbol Code here comes from the _Alternate_
> Symbol Table).
>
> Similarly, to overlay a "car" icon with the letter "B" in a
> _compressed_
>
> Position Report, the report will look something like:
>
> =**B**L!!\<_e7\>_\*7P\[
>
> However, in a _compressed_ Position Report, it is not permissible to
> use a _numeric_ Symbol Table Identifier (0-9) --- _compressed_
> positions never start with a digit. If a numeric overlay is required,
> the report must use a lower-case letter instead (in the range
> **a**-**j**) as the Symbol Table Identifier. The lower- case letter is
> then mapped to the digits **0**-**9** (i.e. a=0, b=1, c=2, d=3 etc).
>
> Thus, in the _compressed_ Position Report:
>
> =**d**5L!!\<_e7\>_\*7P\[
>
> the letter **d** maps to overlay character "3".
>
> As noted above, not all symbols from the Alternate Symbol Table may be
> overlaid in this way --- those that can be overlaid are marked as
> **\[with overlay\]** in Appendix 2. This means that they are _capable_
> of taking an overlay, but they do not necessarily need to have one.
> Thus, for example, the following report uses the car symbol from the
> Alternate Symbol Table, but does not display an overlay:
>
> \@092345z4903.50N**\\**07201.75W**\>**...
>
> **Symbols in the AX.25 Destination**

**Address**

> Where it is not possible to include a symbol in the Information field,
> the symbol may be specified in the AX.25 Destination Address instead,
> using the following generic destination addresses: GPSxyz, GPSCnn,
> GPSEnn, SPCxyz and SYMxyz.
>
> The characters xy and nn refer to entries in the APRS Symbol Tables.
> For example, from the Primary Symbol Table, a tracker could use the
> Destination Address GPS**MV␣** or GPS**30** to specify a "car" icon.
>
> The character z specifies the overlay character (where permitted), or
> is a **␣**
>
> (space) --- the space is a filler character, as all AX.25 addresses
> must be
>
> exactly 6 characters long.
>
> The GPS/SPC/SYMxy␣ and GPSCnn/GPSEnn addresses can be used
> interchangeably. Thus, for example, GPSBM␣ , SPCBM␣ , SYMBM␣ and
> GPSC12 all specify a "Boy Scouts" icon (from the Primary Symbol
> Table), and GPSOM␣ , SPCOM␣ , SYMOM␣ and GPSE12 all specify a "Girl
> Scouts" icon (from the Alternate Symbol Table).
>
> **Overlays with Symbols in the AX.25 Destination**

**Address**

> If the z character in a GPSxyz, SPCxyz or SYMxyz address is not a
> space, it specifies an alphanumeric overlay character, in the range
> 0-9 or A-Z.
>
> Overlays can only be used with symbols from the Alternate Symbol Table
> marked with the legend **\[with overlay\]**.
>
> For example, if the "car" icon is to be overlaid with a digit "3", the
> Destination Address will be GPS**NV3**.
>
> However, even if the address is overlay-capable, it is not actually
> necessary to specify an overlay; e.g. GPS**NV␣**.
>
> GPSCnn and GPSEnn symbols can not have overlays.
>
> **Symbol in the Source Address**

**SSID**

> Where it is not possible to include a symbol in the Information field
> or in the Destination Address, the symbol may be specified in the SSID
> of the Source Address instead:

### SSID-Specified Icons in the AX.25 Source Address Field

> **Symbol Precedence** APRS packets should not contain more than one
> symbol. However, it is conceivably possible to (erroneously) construct
> a packet containing up to three different symbols.
>
> For example:

+---------------+----------------+----------------+----------------+
| | **Source | **Destination | **Information |
| | Address SSID** | Address** | Field** |
+===============+================+================+================+
| | > G3NR | > **MV**!0123 | .45N **/**01 |
| | | | 234.56W**j** |
| | W**-7**GPS | | |
+---------------+----------------+----------------+----------------+
| > **Symbol** | > Small | > Car | > Jeep |
| | > Aircraft | | |
+---------------+----------------+----------------+----------------+

- The symbol in the Information field takes precedence over any other

  : symbol.

- If there is no symbol in the Information field, the symbol in the

  : Destination Address takes precedence over the symbol in the
  Source Address SSID.

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

+-------+-------+-------+-------+-------+-------+-------+-------+-------+
| \*\* | | | | | | | | |
| Compr | | | | | | | | |
| essed | | | | | | | | |
| | | | | | | | | |
| Lat | | | | | | | | |
| /Long | | | | | | | | |
| | | | | | | | | |
| Pos | | | | | | | | |
| ition | | | | | | | | |
| R | | | | | | | | |
| eport | | | | | | | | |
| F | | | | | | | | |
| ormat | | | | | | | | |
| --- | | | | | | | | |
| | | | | | | | | |
| with | | | | | | | | |
| T | | | | | | | | |
| imest | | | | | | | | |
| am | | | | | | | | |
| p\*\* | | | | | | | | |
+=======+=======+=======+=======+=======+=======+=======+=======+=======+
| **/** | > | \* | > | > | **S | > | > | \ |
| | - | \*Sym | - | - | ymbol | \*\* | - | *\*Co |
| > | | | | | C | | | mment |
| *or* | \ | Table | \ | *Comp | ode** | Compr | *Com | |
| | *Time | | _Comp | L | | essed | p T y | > |
| **@** | | > I | | o | | | pe_\* | (max |
| | > DHM | D\*\* | La | ng\*\* | | > | | > |
| | > | | t\*\* | | | Cour | : T | > : |
| | > | | | > | | | | 40 |
| | : / | | > | XXXX | | se/Sp | | > |
| | | | YYYY | | | ee | | > |
| | HM | | | | | d\*\* | | cha |
| | S\*\* | | | | | | | |
| | | | | | | | | rs |
| | | | | | | | | )\*\* |
+-------+-------+-------+-------+-------+-------+-------+-------+-------+
| | | | | | | > | | |
| | | | | | | \*\* | | |
| | | | | | | | | |
| | | | | | | Compr | | |
| | | | | | | essed | | |
| | | | | | | | | |
| | | | | | | Radio | | |
| | | | | | | | | |
| | | | | | | : | | |
| | | | | | | Ra | | |
| | | | | | | | | |
| | | | | | | ng | | |
| | | | | | | e\*\* | | |
+-------+-------+-------+-------+-------+-------+-------+-------+-------+
| | | | | | | > | | |
| | | | | | | \*\* | | |
| | | | | | | | | |
| | | | | | | Compr | | |
| | | | | | | essed | | |
| | | | | | | | | |
| | | | | | | Altit | | |
| | | | | | | ud | | |
| | | | | | | e\*\* | | |
+-------+-------+-------+-------+-------+-------+-------+-------+-------+
| > 1 | > 7 | > 1 | > 4 | > 4 | > 1 | > 2 | > 1 | 0-40 |
+-------+-------+-------+-------+-------+-------+-------+-------+-------+

Bit:

Value:

+---------+---------+---------+---------+---------+---------+---------+
| \* | | | | | | |
| \*Mic-E | | | | | | |
| Data | | | | | | |
| --- | | | | | | |
| DEST | | | | | | |
| INATION | | | | | | |
| | | | | | | |
| ADDRESS | | | | | | |
| | | | | | | |
| FIELD F | | | | | | |
| or | | | | | | |
| mat\*\* | | | | | | |
+=========+=========+=========+=========+=========+=========+=========+
| \*\*Lat | \*\*Lat | \*\*Lat | \*\*Lat | \*\*Lat | \*\*Lat | > - |
| | | | | | | |
| > Digit | > Digit | > Digit | > Digit | > Digit | > Digit | _ |
| > | > | > | > | > | > | APRS_\* |
| > : | > : | > : | > : | > : | > : | |
| 1\*\* | 2\*\* | 3\*\* | 4\*\* | 5\*\* | 6\*\* | > \ |
| > | > | > | > | > | > | \*\*Digi |
| > | > | > | > | > | > | > |
| \*\*+ | \*\*+ | \*\*+ | \*\*+ | \*\*+ | \*\*+ | > : |
| | | | > | > | > | Path |
| Message | Message | Message | N/S | Lo | W/E | > |
| | | | > | | > | > C |
| : Bit | : Bit | : Bit | Lat | ngitude | Long | ode\*\* |
| | | | > | | > | |
| A\*\* | B\*\* | C\*\* | Indi | : O | Indi | |
| | | | | | | |
| | | | ca | ff | ca | |
| | | | tor\*\* | set\*\* | tor\*\* | |
+---------+---------+---------+---------+---------+---------+---------+
| > 1 | > 1 | > 1 | > 1 | > 1 | > 1 | > 1 |
+---------+---------+---------+---------+---------+---------+---------+

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 8%" />
</colgroup>
<thead>
<tr class="header">
<th><p>**Com plete We ather R eport F ormat —</p>
<p>with O bject</p>
<p>and</p>
<p>Lat /Long</p>
posit ion**</th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<ul>
<li></li>
</ul>
</blockquote>
<em>*</em>*</td>
<td><strong>O bject N ame</strong></td>
<td><blockquote>
<ul>
<li></li>
</ul>
</blockquote>
<em>*</em>*</td>
<td><blockquote>
<ul>
<li></li>
</ul>
</blockquote>
<p>*Time</p>
<blockquote>
<dl>
<dt>DHM</dt>
<dd><p>/</p>
</dd>
</dl>
</blockquote>
<p>HMS**</p></td>
<td><blockquote>
<p>**</p>
</blockquote>
Lat**</td>
<td><p>**Sym</p>
<p>Table</p>
<blockquote>
<p>ID**</p>
</blockquote></td>
<td><strong>L ong</strong></td>
<td><p><strong>S ymbol C ode</strong></p>
<blockquote>
<ul>
<li></li>
</ul>
</blockquote>
<p><em>_</em>*</p></td>
<td><blockquote>
<ul>
<li></li>
</ul>
</blockquote>
<p>*Wind</p>
<blockquote>
<p>Dir</p>
</blockquote>
<dl>
<dt>ectn/</dt>
<dd><p>Sp</p>
</dd>
</dl>
<p>eed**</p></td>
<td><strong>We ather D ata</strong></td>
<td><p><strong>A PRS</strong></p>
<blockquote>
<p>**</p>
</blockquote>
<p>Softw are**</p>
<blockquote>
<p>S</p>
</blockquote></td>
<td><blockquote>
<ul>
<li></li>
</ul>
</blockquote>
<p><em>WX</em>*</p>
<blockquote>
<p>**U</p>
</blockquote>
<p>nit**</p>
<blockquote>
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
<td>2-4</td>
</tr>
</tbody>
</table>

+-------+-------+-------+-------+-------+-------+-------+-------+-------+
| \* | | | | | | | | |
| \ | | | | | | | | |
| *Tele | | | | | | | | |
| metry | | | | | | | | |
| R | | | | | | | | |
| eport | | | | | | | | |
| | | | | | | | | |
| For | | | | | | | | |
| ma | | | | | | | | |
| t\*\* | | | | | | | | |
+=======+=======+=======+=======+=======+=======+=======+=======+=======+
| **T** | \* | \*\*A | \*\*A | \*\*A | \*\*A | \*\*A | **Di | > |
| | \*Seq | nalog | nalog | nalog | nalog | nalog | gital | - |
| | uence | | | | | | Va | |
| | No | Value | Value | Value | Value | Value | lue** | *Comm |
| | | | | | | | | e |
| | \# | > | > | > | > | > | > bbb | nt\*\* |
| | \*_nn | 1\*\* | 2\*\* | 3\*\* | 4\*\* | 5\*\* | | |
| | n,_\* | | | | | | bbbbb | |
| | | aaa | aaa | aaa | aaa | aaa | | |
| | | **,** | **,** | **,** | **,** | **,** | | |
+-------+-------+-------+-------+-------+-------+-------+-------+-------+
| > 1 | > 5 | > 4 | > 4 | > 4 | > 4 | > 4 | > 8 | > n |
+-------+-------+-------+-------+-------+-------+-------+-------+-------+

+----------+----------+----------+----------+----------+----------+
| \* | | | | | |
| \ | | | | | |
| *Message | | | | | |
| | | | | | |
| Fo | | | | | |
| rmat\*\* | | | | | |
+==========+==========+==========+==========+==========+==========+
| > **:** | > | > **:** | > - | > - | |
| | \*\*Add | | | | |
| | | | \ | *Message | |
| | re | | _Message | ID_\* | |
| | ssee\*\* | | | | |
| | | | : Text | | |
| | | | (max | | |
| | | | 67 | | |
| | | | | | |
| | | | ch | | |
| | | | ars)\*\* | | |
+----------+----------+----------+----------+----------+----------+
| | | | | > **{** | > - |
| | | | | | |
| | | | | | _Message |
| | | | | | No_\* |
| | | | | | |
| | | | | | > xxxxx |
+----------+----------+----------+----------+----------+----------+
| > 1 | > 9 | > 1 | > 0-67 | > 1 | > 1-5 |
+----------+----------+----------+----------+----------+----------+

+--------------+--------------+----------+------------+--------------+
| **Message | | | | |
| Ack | | | | |
| nowledgement | | | | |
| Format** | | | | |
+==============+==============+==========+============+==============+
| **:** | > - | > **:** | > **ack** | > **Message |
| | | | | > No** xxxxx |
| | _ | | | |
| | Addressee_\* | | | |
+--------------+--------------+----------+------------+--------------+
| 1 | > 9 | > 1 | > 3 | > 1--5 |
+--------------+--------------+----------+------------+--------------+

+----------------+----------------+----------------+----------------+
| **User-Defined | | | |
| Data Format** | | | |
+================+================+================+================+
| **{** | > **User ID** | **User-Defined | \* |
| | > U | Packet Type** | \*User-defined |
| | | X | data |
| | | | (printable |
| | | | ASCII |
| | | | |
| | | | re |
| | | | commended)\*\* |
+----------------+----------------+----------------+----------------+
| 1 | > 1 | > 1 | > n |
+----------------+----------------+----------------+----------------+

+-------------------------------------+---------------------------------+
| **Invalid Data / Test Data Format** | |
+=====================================+=================================+
| **,** | > **Invalid Data or Test Data** |
+-------------------------------------+---------------------------------+
| 1 | > n |
+-------------------------------------+---------------------------------+

# APPENDIX 2: THE APRS SYMBOL TABLES

> (Each highlighted character in the Alternate Symbol Table may be
> replaced with an overlay character).

## APRS SYMBOL TABLES (continued)

> (Each highlighted character in the Alternate Symbol Table may be
> replaced with an overlay character).

## APRS SYMBOL TABLES (continued)

> (Each highlighted character in the Alternate Symbol Table may be
> replaced with an overlay character).

# APPENDIX 3: 7-BIT ASCII CODE TABLE

> In addition to listing the ASCII character codes in their usual form,
> this table also expresses the hexadecimal codes for the ASCII digits
> 0--9 and the upper-case letters A--Z in _shifted_ form; i.e. shifted
> one bit left. This is particularly useful for decoding callsigns and
> Mic-E position information contained in the address fields of AX.25
> frames.
>
> **Part 1: Codes 0--31 decimal (00--1f hexadecimal)**

+---------+---------+------------+-----------+-----------------------------+
| **Dec** | **Hex** | **Char** | | |
+=========+=========+============+===========+=============================+
| > **0** | > 00 | > NUL | > CTRL-@ | |
+---------+---------+------------+-----------+-----------------------------+
| > **1** | > 01 | > SOH | > CTRL-A | > Start of Header |
+---------+---------+------------+-----------+-----------------------------+
| > **2** | > 02 | > STX | > CTRL-B | > Start of Text |
+---------+---------+------------+-----------+-----------------------------+
| > **3** | > 03 | > ETX | > CTRL-C | > End of Text |
+---------+---------+------------+-----------+-----------------------------+
| > **4** | > 04 | > EOT | > CTRL-D | > End of Transmission |
+---------+---------+------------+-----------+-----------------------------+
| > **5** | > 05 | > ENQ | > CTRL-E | > Enquiry (Poll) |
+---------+---------+------------+-----------+-----------------------------+
| > **6** | > 06 | > ACK | > CTRL-F | > Acknowledge |
+---------+---------+------------+-----------+-----------------------------+
| > **7** | > 07 | > BEL | > CTRL-G | > Bell |
+---------+---------+------------+-----------+-----------------------------+
| > **8** | > 08 | > BS | > CTRL-H | > Backspace |
+---------+---------+------------+-----------+-----------------------------+
| > **9** | > 09 | > HT | > CTRL-I | > Horizontal Tab |
+---------+---------+------------+-----------+-----------------------------+
| **10** | > 0a | > LF | > CTRL-J | > Line Feed |
+---------+---------+------------+-----------+-----------------------------+
| **11** | > 0b | > VT | > CTRL-K | > Vertical Tab |
+---------+---------+------------+-----------+-----------------------------+
| **12** | > 0c | > FF | > CTRL-L | > Form Feed |
+---------+---------+------------+-----------+-----------------------------+
| **13** | > 0d | > CR | > CTRL-M | > Carriage Return |
+---------+---------+------------+-----------+-----------------------------+
| **14** | > 0e | > SO | > CTRL-N | > Shift Out |
+---------+---------+------------+-----------+-----------------------------+
| **15** | > 0f | > SI | > CTRL-O | > Shift In |
+---------+---------+------------+-----------+-----------------------------+
| **16** | > 10 | > DLE | > CRTL-P | > Data Link Escape |
+---------+---------+------------+-----------+-----------------------------+
| **17** | > 11 | > DC1/XON | > CTRL-Q | > Device Control 1 |
+---------+---------+------------+-----------+-----------------------------+
| **18** | > 12 | > DC2 | > CTRL-R | > Device Control 2 |
+---------+---------+------------+-----------+-----------------------------+
| **19** | > 13 | > DC3/XOFF | > CTRL-S | > Device Control 3 |
+---------+---------+------------+-----------+-----------------------------+
| **20** | > 14 | > DC4 | > CTRL-T | > Device Control 4 |
+---------+---------+------------+-----------+-----------------------------+
| **21** | > 15 | > NAK | > CTRL-U | > Negative Acknowledge |
+---------+---------+------------+-----------+-----------------------------+
| **22** | > 16 | > SYN | > CTRL-V | > Synchronous Idle |
+---------+---------+------------+-----------+-----------------------------+
| **23** | > 17 | > ETB | > CTRL-W | > End of Transmission Block |
+---------+---------+------------+-----------+-----------------------------+
| **24** | > 18 | > CAN | > CTRL-X | > Cancel |
+---------+---------+------------+-----------+-----------------------------+
| **25** | > 19 | > EM | > CTRL-Y | > End of Medium |
+---------+---------+------------+-----------+-----------------------------+
| **26** | > 1a | > SUB | > CTRL-Z | > Substitute |
+---------+---------+------------+-----------+-----------------------------+
| **27** | > 1b | > ESC | > CTRL-\[ | > Escape |
+---------+---------+------------+-----------+-----------------------------+
| **28** | > 1c | > FS | > CTRL-\\ | > File Separator |
+---------+---------+------------+-----------+-----------------------------+
| **29** | > 1d | > GS | > CTRL-\] | > Group Separator |
+---------+---------+------------+-----------+-----------------------------+
| **30** | > 1e | > RS | > CTRL-\^ | > Record Separator |
+---------+---------+------------+-----------+-----------------------------+
| **31** | > 1f | > US | > CTRL-\_ | > Unit Separator |
+---------+---------+------------+-----------+-----------------------------+

# APPENDIX 4: DECIMAL-TO-HEX CONVERSION TABLE

> **APPENDIX 5: GLOSSARY**
>
> **Altitude** 1. In Mic-E format, the altitude in meters relative to
> 10km below mean sea level.
>
> 2.  In Comment text, the altitude in feet above mean sea level.
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
> **APRS** Automatic Position Reporting System.
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
> **APRS Digipeater Path** A digipeater path via repeaters with RELAY,
> WIDE and related aliases. Used in Mic-E compressed location format.
>
> **APRS Data Type Identifier** The single-byte identifier that
> specifies what kind of APRS information is contained in the AX.25
> Information field.
>
> **Area Object** A user-defined graphic object (circle, ellipse,
> triangle, box and line).
>
> **ASCII** American Standard Code for Information Interchange. A 7-bit
> character code conforming to ANSI X3.4 (1968) --- see Appendix 3 for
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

**Digipeater Addresses field** The AX.25 field containing 0--8
digipeater callsigns (or aliases).

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
> antenna's effectiveness in the local area. Used in the PHG Data

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
> station's lat/long position and antenna height relative to mean sea
> level, and other data.
>
> **GLL Sentence** A standard NMEA sentence, containing the receiving
> station's lat/long position and other data.
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
station's position.

> **MDHM** 8-byte timestamp: month, day, hour, minute (used in
> positionless weather station reports).
>
> **Message** A one-line text message addressed to a particular station.

**Message Acknowledgement** An optional acknowledgement of receipt of a
message.

**Message Group** A user-defined group to receive messages.

**Message Identifier** A 1--5 character message identifier (typically a
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
> navigation equipment (such as GPS receivers)
>
> conforming to the NMEA 0182 Version 2.0 specification. APRS supports
> five NMEA Sentences: GGA, GLL, RMC, VTG and WPT.
>
> **NRQ** Number/Rate/Quality. A measure of confidence in DF Bearing
> reports.
>
> **Null Position** Default position to be reported if the actual
> position is unknown or indeterminate. The null position is 0° 0\' 0\"
> north, 0° 0\' 0\" west.
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
>
> **PIC-E** A PIC implementation of the Mic-E microphone encoder.
>
> **Position Ambiguity** A reduction in the accuracy of APRS position
> information (implemented by replacing low-order lat/long digits with
> spaces). Used when the exact position is not known.

**Position Report** A report containing lat/long position, optionally
with timestamp and Data Extension.

**Pre-Calculated Radio Range** A station's estimate of omni-directional
radio range (in miles). Used in compressed

> lat/long format.
>
> **Query** A request for information. Queries may be addressed to
> stations in general or to specific stations.

**Range Circle** Usable radio range (in miles), computed from PHG data.

> **RELAY** A generic APRS digipeater callsign alias, for a VHF/UHF
> digipeater with limited local coverage.
>
> **Response** A reply to a query.
>
> **RMC Sentence** A standard NMEA sentence, containing the receiving
> station's lat/long position, course and speed, and other data.
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
> **/\$** represents a symbol from the Primary Symbol Table, and
> **\\\$** represents a symbol from the Alternate Symbol Table.

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
> **TRACE** A generic digipeater callsign alias, for digipeaters that
> performs callsign substitution. These digipeaters self-identify
> packets they digipeat, by inserting their own callsign in place of
> RELAY,WIDE or TRACE.
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
> UI-frames --- that is, it operates entirely in connectionless
> (UNPROTO) mode.

**UNPROTO Path** The digipeater path to the destination callsign.

> **UTC** Coordinated Universal Time (=zulu=GMT).
>
> **VTG Received Sentence** A standard NMEA sentence, containing the
> receiving station's course and speed.
>
> **WIDE** A generic APRS digipeater callsign alias, for a digipeater
> with wide area coverage.
>
> **WIDEn-N** A generic APRS digipeater callsign alias, for a digipeater
> with wide area coverage (N=0-7). As a packet passes through a
> digipeater, the value of N is decremented by 1 until it reaches zero.
> The digipeater keeps a record of each packet (or its FCS) as it
>
> passes through, and will not digipeat the packet again if it has
> digipeated it already within the last 28 seconds.

**WPT Sentence** A standard NMEA sentence, containing waypoints.

> **WX** Weather.
>
> **Ziplan** A cheap twisted-pair LAN connecting PCs via their serial
> I/O ports. Designed for use in emergency situations.
>
> **Zulu** UTC/GMT.

#### Units Conversion Table

+----------------+----------------+----------------+----------------+
| **To convert | **to** | **multiply | **divide by** |
| from** | | by** | |
+================+================+================+================+
| feet | > meters | > 0.3048 | |
+----------------+----------------+----------------+----------------+
| meters | > feet | | > 0.3048 |
+----------------+----------------+----------------+----------------+
| miles | > km | > 1.609344 | |
+----------------+----------------+----------------+----------------+
| km | > miles | | > 1.609344 |
+----------------+----------------+----------------+----------------+
| miles | > nautical | > 0.8689762 | |
| | > miles | | |
+----------------+----------------+----------------+----------------+
| nautical miles | > miles | | > 0.8689762 |
+----------------+----------------+----------------+----------------+
| miles per hour | > knots | > 0.8689762 | |
| (mph) | | | |
+----------------+----------------+----------------+----------------+
| knots | > miles per | | > 0.8689762 |
| | > hour (mph) | | |
+----------------+----------------+----------------+----------------+
| knots | > meters / | > 0.51444' | |
| | > second | | |
+----------------+----------------+----------------+----------------+
| meters / | > knots | | > 0.51444' |
| second | | | |
+----------------+----------------+----------------+----------------+
| miles per hour | > meters / | > 0.44704 | |
| (mph) | > second | | |
+----------------+----------------+----------------+----------------+
| meters / | > miles per | | > 0.44704 |
| second | > hour (mph) | | |
+----------------+----------------+----------------+----------------+

##### F = ( C x 1.8 ) + 32 C = ( F -- 32 ) x 5

9

# APPENDIX 6: REFERENCES

> _AX.25 Amateur Packet-Radio Link-Layer Protocol Version 2.0, October
> 1984,_ at
>
> <http://www.tapr.org/tapr/html/ax25.html>
>
> _NMEA 0183 ASCII Interface Specification,_ at
> <http://www.nmea.org/0183.htm>
>
> NMEA Sentence Formats, in the _Garmin GPS25 Technical Reference
> Manual_, at
>
> <http://www.garmin.com/manuals/spec25.pdf>
>
> Maidenhead Locator, in the _IARU Region 1 VHF Manager's Manual_, at
>
> <http://www.scit.wlv.ac.uk/vhfc/iaru.r1.vhfm.4e/index.html>

# APPENDIX 7: DOCUMENT RELEASE HISTORY

+-------------+--------------------+---------------------------------+
| **Date** | **Doc Version** | **Status / Major Changes** |
+=============+====================+=================================+
| 10 Oct 1999 | > 1.0 (Draft) | > Protocol Version 1.0. First |
| | | > public draft release. |
+-------------+--------------------+---------------------------------+
| 3 Dec 1999 | > 1.0.1g | > Protocol Version 1.0. Second |
| | | > public draft release. Much |
| | | > extended, incorporating |
| | | > packet format layouts, APRS |
| | | > symbol tables, compressed |
| | | > data format, Mic-E format, |
| | | > telemetry format. |
+-------------+--------------------+---------------------------------+
| 30 Apr 2000 | > 1.0.1m | > Protocol Version 1.0. Third |
| | | > public draft release. |
| | | > |
| | | > Major additions/changes to |
| | | > the draft 1.0.1g |
| | | > specification: |
| | | |
| | | - |
| | | |
| | | |
| | | Added a section on Map Views |
| | | |
| | | : and Range Scale. |
| | | |
| | | - |
| | | |
| | | Changed Destination Address |
| | | |
| | | : SSID description |
| | | (specifying generic |
| | | APRS digipeater paths) |
| | | to apply to _all_ |
| | | packets, not just Mic-E |
| | | packets. |
| | | |
| | | - |
| | | |
| | | Changed APRS destination |
| | | |
| | | : "callsigns" to |
| | | "destination |
| | | addresses". |
| | | |
| | | - |
| | | |
| | | Added TEL\* to the list of |
| | | |
| | | : generic destination |
| | | addresses. |
| | | |
| | | - |
| | | |
| | | Added brief explanations of |
| | | |
| | | : how several generic |
| | | destination addresses |
| | | are used. |
| | | |
| | | - |
| | | |
| | | Added "Grid-in-To-Address" |
| | | |
| | | : (but marked as |
| | | obsolete). |
| | | |
| | | - |
| | | |
| | | Extended the description of |
| | | |
| | | : the Comment field, with |
| | | pointers to what can |
| | | appear in the field. |
| | | |
| | | - |
| | | |
| | | Added explanation of base |
| | | |
| | | : 91\. |
| | | |
| | | - |
| | | |
| | | Added paragraph on lack of |
| | | |
| | | : consistency in on-air |
| | | units, and default GPS |
| | | datum = WGS84. |
| | | |
| | | - |
| | | |
| | | APRS Data Type Identifiers |
| | | |
| | | : Table: |
| | | |
| | | `{=html}                      |
|             |                    | <!-- -->                        |
|             |                    | ` |
| | | - |
| | | |
| | | Position Ambiguity: need |
| | | |
| | | : only be specified in |
| | | the latitude --- the |
| | | longitude will have the |
| | | same level of |
| | | ambiguity. |
| | | |
| | | - |
| | | |
| | | Added the options of |
| | | |
| | | : **\.../\...** and |
| | | **␣␣␣/␣␣␣** to express |
| | | unknown course/speed. |
| | | |
| | | - Added DFS parameter table. |
| | | |
| | | - |
| | | |
| | | Added Quality table for |
| | | |
| | | : BRG/NRQ data. |
| | | |
| | | - |
| | | |
| | | Position, DF and Compressed |
| | | |
| | | : Report formats: split |
| | | the format diagrams |
| | | into two parts (with |
| | | and without |
| | | timestamps). |
| | | |
| | | - DF Reports: added notes: |
| | | |
| | | `{=html}                      |
|             |                    | <!-- -->                        |
|             |                    | ` |
| | | - |
| | | |
| | | |
| | | Compressed position reports: |
| | | |
| | | : corrected the |
| | | multiplication/division |
| | | constants for encoding/ |
| | | decoding. |
| | | |
| | | - |
| | | |
| | | Mic-E chapter rewritten and |
| | | |
| | | : expanded. Emphasized |
| | | the need to ensure that |
| | | non-printing ASCII |
| | | characters are not |
| | | dropped. Corrected the |
| | | Mic-E telemetry data |
| | | format. |
| | | |
| | | - |
| | | |
| | | Expanded the introductory |
| | | |
| | | : description of |
| | | Objects/Items. All |
| | | Objects must have a |
| | | timestamp. |
| | | |
| | | - |
| | | |
| | | Added Area Object Extended |
| | | |
| | | : Data field to Object |
| | | and Item format |
| | | diagrams. |
| | | |
| | | - |
| | | |
| | | Added Object/Item format |
| | | |
| | | : diagrams with |
| | | compressed location |
| | | data. |
| | | |
| | | - |
| | | |
| | | Killed Objects/Items: now |
| | | |
| | | : indicated by underscore |
| | | after the name. |
+-------------+--------------------+---------------------------------+

+-------------+--------------------+---------------------------------+
| **Date** | **Doc Version** | **Status / Major Changes** |
+=============+====================+=================================+
| | > 1.0.1m | - |
| | > | |
| | > (continued) | Re-categorized weather |
| | | |
| | | : reports: Raw, |
| | | Positionless and |
| | | Complete. |
| | | |
| | | - |
| | | |
| | | Added a statement that |
| | | |
| | | : temperatures below zero |
| | | are expressed as -01 to |
| | | -99. |
| | | |
| | | - |
| | | |
| | | |
| | | Added the options of **\...** |
| | | |
| | | : and **␣␣␣** to express |
| | | unknown weather |
| | | parameter values. |
| | | |
| | | - |
| | | |
| | | Corrected the storm data |
| | | |
| | | : format. Also, central |
| | | pressure is now /ppppp |
| | | (tenths of millibar). |
| | | |
| | | - |
| | | |
| | | Corrected the telemetry |
| | | |
| | | : parameter data (now |
| | | APRS _messages_ instead |
| | | of AX.25 UI _beacons_). |
| | | |
| | | - |
| | | |
| | | |
| | | Added optional comment field |
| | | |
| | | : to the Telemetry (T) |
| | | format. |
| | | |
| | | - |
| | | |
| | | Added a section describing |
| | | |
| | | : the handling of |
| | | multiple message |
| | | acknowledgements. |
| | | |
| | | - |
| | | |
| | | Added a section on NTS |
| | | |
| | | : radiograms. |
| | | |
| | | - |
| | | |
| | | Added Bulletin/Announcement |
| | | |
| | | : implementation |
| | | recommendations. |
| | | |
| | | - Queries and Responses: |
| | | |
| | | `{=html}                      |
|             |                    | <!-- -->                        |
|             |                    | ` |
| | | - |
| | | |
| | | Added PING as a synonym of |
| | | |
| | | : APRST. |
| | | |
| | | - |
| | | |
| | | Extended meteor scatter ERP |
| | | |
| | | : beyond 810 watts, and |
| | | added a lookup table. |
| | | |
| | | - |
| | | |
| | | Maidenhead Locator: all |
| | | |
| | | : letters must be |
| | | transmitted in upper |
| | | case, but may be |
| | | received in either |
| | | upper or lower case. |
| | | |
| | | - |
| | | |
| | | Changed the definition of |
| | | |
| | | : non-APRS packets --- |
| | | these are not APRS |
| | | Status Messages, but |
| | | may optionally be |
| | | treated as such. |
| | | |
| | | - |
| | | |
| | | APRS Symbols chapter |
| | | |
| | | : substantially |
| | | rewritten.. |
| | | |
| | | - |
| | | |
| | | Added section on Symbol |
| | | |
| | | : Precedence (where more |
| | | than one symbol appears |
| | | in an APRS packet). |
| | | |
| | | - |
| | | |
| | | Clarified some of the |
| | | |
| | | : descriptions in the |
| | | APRS Symbol Tables. |
| | | |
| | | - |
| | | |
| | | Added overlay capability to |
| | | |
| | | : the \\a symbol |
| | | (ARES/RACES etc). |
| | | |
| | | - |
| | | |
| | | Separated the 7-bit ASCII |
| | | |
| | | : table from the Dec/Hex |
| | | (0x80-0xff) conversion |
| | | table. |
| | | |
| | | - |
| | | |
| | | Added several new entries |
| | | |
| | | : and a units conversion |
| | | table to the Glossary. |
| | | |
| | | - |
| | | |
| | | |
| | | Added new references to NMEA |
| | | |
| | | : sentence formats and |
| | | Maidenhead Locator |
| | | formats. |
+-------------+--------------------+---------------------------------+

+----------------+--------------------+------------------------------+
| **Date** | **Doc Version** | **Status / Major Changes** |
+================+====================+==============================+
| > 29 Aug 2000 | > 1.0.1 | > Protocol Version 1.0. |
| | | > Approved public release. |
| | | > |
| | | > Minor additions/changes to |
| | | > the draft 1.0.1m |
| | | > specification: |
| | | |
| | | - Added Foreword. |
| | | |
| | | - |
| | | |
| | | Replaced section on Map |
| | | |
| | | : Views and Range |
| | | Scale. |
| | | |
| | | - |
| | | |
| | | |
| | | APRS Software Version No: |
| | | |
| | | : added **APD**xxx |
| | | (Linux aprsd |
| | | server). |
| | | |
| | | - |
| | | |
| | | APRS Data Type |
| | | |
| | | : Identifier: |
| | | Designated **\[** as |
| | | Maidenhead grid |
| | | locator (but noted |
| | | as obsolete). |
| | | |
| | | - |
| | | |
| | | |
| | | Position Ambiguity: added |
| | | |
| | | : a bounding box |
| | | example. |
| | | |
| | | - |
| | | |
| | | Compressed Position |
| | | |
| | | : Formats: for |
| | | course/speed, |
| | | corrected the range |
| | | of possible values |
| | | of the "c" byte to |
| | | 0--89. |
| | | |
| | | - |
| | | |
| | | Mic-E: replaced the |
| | | |
| | | : latitude example |
| | | table, to show more |
| | | explicitly how the |
| | | N/S/E/W/Long offset |
| | | bits are encoded. |
| | | |
| | | - |
| | | |
| | | Mic-E: removed the |
| | | |
| | | : paragraph stating |
| | | that there must be a |
| | | space between the |
| | | altitude and comment |
| | | text --- no space is |
| | | required. |
| | | |
| | | - |
| | | |
| | | Mic-E: removed the note |
| | | |
| | | : on inaccurate |
| | | altitude data, as |
| | | GPS Selective |
| | | Availability has |
| | | been switched off. |
| | | |
| | | - |
| | | |
| | | Object Reports: added |
| | | |
| | | : timestamps to some |
| | | of the examples (an |
| | | Object Report must |
| | | always have a |
| | | timestamp). |
| | | |
| | | - |
| | | |
| | | |
| | | Signposts: can be Objects |
| | | |
| | | : or Items. |
| | | |
| | | - |
| | | |
| | | Storm Data: changed |
| | | |
| | | : central pressure |
| | | format to **/**pppp |
| | | (i.e. to the nearest |
| | | millibar/hPascal). |
| | | |
| | | - |
| | | |
| | | Storm Data: Hurricane |
| | | |
| | | : Brenda examples: |
| | | inserted a leading |
| | | zero in the central |
| | | pressure field |
| | | (central pressure is |
| | | 4 digits). |
| | | |
| | | - |
| | | |
| | | Telemetry Data: Added |
| | | |
| | | : **MIC** as an |
| | | alternative form of |
| | | Sequence Number. |
| | | **MIC** may or may |
| | | not be followed by a |
| | | comma. |
| | | |
| | | - |
| | | |
| | | Messages: added the |
| | | |
| | | : reject message |
| | | format. |
| | | |
| | | - |
| | | |
| | | Appendix 1: Agrelo |
| | | |
| | | : format: changed the |
| | | separator between |
| | | Bearing and Quality |
| | | to **/**. |
| | | |
| | | - |
| | | |
| | | Symbol Table: changed |
| | | |
| | | : **/(** symbol from |
| | | "Cloudy" to "Mobile |
| | | Satellite |
| | | Groundstation". |
| | | |
| | | - |
| | | |
| | | Reformatted the Units |
| | | |
| | | : Conversion Table. |
+----------------+--------------------+------------------------------+
