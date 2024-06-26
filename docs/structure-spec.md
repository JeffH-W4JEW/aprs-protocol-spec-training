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
