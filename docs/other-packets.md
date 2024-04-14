# Other Packets

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
