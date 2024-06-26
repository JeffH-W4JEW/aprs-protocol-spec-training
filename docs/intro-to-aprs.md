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
