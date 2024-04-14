# APRS and AX.25

**Protocols** At the data link layer, APRS uses AX.25 frames, as defined in _AX.25 Link Access Protocol for Amateur Packet Radio_ (see Appendix 6 for details), utilizing Unnumbered Information (UI) frames exclusively. This means that APRS runs in _connectionless_ mode, whereby AX.25 frames are transmitted without expecting any response, and reception at the other end is not guaranteed.

At a higher level, APRS supports a messaging protocol that allows users to send short messages (one line of text) to nominated stations, and expects to receive acknowledgements from those stations.

This specification does not mandate a particular type of modem for the physical layer. In practice it is most often 1200 bps AFSK with tones of 1200 and 2200 Hz.

**The AX.25 Frame** All APRS transmissions use AX.25 UI-frames, with 9 fields of data:

- **Flag** — The flag field at each end of the frame is the bit sequence 0x7e that separates each frame. A single flag can be both the end of one frame and the beginning of the next.

- **Destination Address** — Unlike Packet Radio, this is not used for the destination for the packet. This field is used in multiple ways for the different APRS data types. APRS data is encoded to ensure that the field conforms to the standard AX.25 callsign format (i.e. up to 6 upper case alphanumeric characters plus SSID). Most often it will be of the form AP*xxxx* to identify the type of system that generated the packet. If the SSID is non-zero, it specifies a generic APRS digipeater path.

**Did I see somewhere that is obsolete?**

In accordance with the AX.25 specification, for UI frames, the C bit of the Destination SSID octet should be 1. Receiving applications must ignore it because this has not been done consistently.

- **Source Address** — This field contains the callsign and SSID of the transmitting station. In some cases, if the SSID is non-zero, the SSID may specify an APRS display Symbol Code.

In accordance with the AX.25 specification, for UI frames, the C bit of the Source SSID octet should be 0. Receiving applications must ignore it because this has not been done consistently.

- **Digipeater Addresses** — From zero to 8 digipeater callsigns may be included in this field. These can be actual station names but are are usually aliases so you don’t need to know about the network topology.

!!! note
These digipeater addresses may be overridden by a generic APRS digipeater path (specified in the Destination Address SSID).

**Did I read somewhere that was obsolete? I bet it is not widely implemented.**

- **Control Field** — This field is set to 0x03 (UI-frame).

- **Protocol ID** — This field is set to 0xf0 (no layer 3 protocol).

- **Information Field** — This field contains more APRS data. The first character of this field is the APRS Data Type Identifier (DTI) that specifies the nature of the data that follows.

- **Frame Check Sequence** — The FCS is a sequence of 16 bits used for checking the integrity of a received frame.
