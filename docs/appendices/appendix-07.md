# Appendix 7: Document Release History

Document Release History

| Date        | Doc Version | Status/Major Changes                                         |
| ----------- | ----------- | ------------------------------------------------------------ |
| 10 Oct 1999 | 1.0 (Draft) | Protocol Version 1.0. First public draft release.            |
| 3 Dec 1999  | 1.0.1g      | Protocol Version 1.0. Second public draft release. Much extended, incorporating packet format layouts, APRS symbol tables, compressed data format, Mic-E format, telemetry format. |
| 30 Apr 2000 | 1.0.1m      | Protocol Version 1.0. Third public draft release. <br />
|  |  | Major additions/changes to the draft 1.0.1g specification: <br />
|  |  | • Added a section on Map Views and Range Scale. <br />
|  |  | • Changed Destination Address SSID description (specifying generic APRS digipeater paths) to apply to *all* packets, not just Mic-E packets. <br />
|  |  | • Changed APRS destination “callsigns” to “destination addresses”. <br />
|  |  | • Added TEL* to the list of generic destination addresses. <br />
|  |  | • Added brief explanations of how several generic destination addresses are used. <br />
|  |  | • Added “Grid-in-To-Address” (but marked as obsolete). <br />
|  |  | • Extended the description of the Comment field, with pointers to what can appear in the field. <br />
|  |  | • Added explanation of base 91. <br />
|  |  | • Added paragraph on lack of consistency in on-air units, and default GPS datum = WGS84. <br />
|  |  | • APRS Data Type Identifiers Table:<br />- marked Shelter Data and Space Weather as reserved DTIs. <br />
|  |  | - marked the **-** DTI as unused (previously erroneously allocated to Killed Objects). <br />
|  |  | - marked the **'** DTI to mean *Current* Mic-E data in Kenwood TM-D700 radios. <br />
|  |  | - marked the **‘** DTI as *not used* in Kenwood TM-D700 radios. <br />
|  |  | • Position Ambiguity: need only be specified in the latitude — the longitude will have the same level of ambiguity. <br />
|  |  | • Added the options of **.../...** and   **/**   to express unknown course/speed. <br />
|  |  | • Added DFS parameter table. <br />
|  |  | • Added Quality table for BRG/NRQ data. <br />
|  |  | • Position, DF and Compressed Report formats: split the format diagrams into two parts (with and without timestamps). <br />
|  |  | • DF Reports: added notes:<br />- BRG/NRQ data is only valid when the symbol is **/\**. <br />
|  |  | - CSE=000 means the DF station is fixed, CSE non-zero means the station is moving. - <br />
|  |  |  • Compressed position reports: corrected the multiplication/division constants for encoding/ decoding. |
