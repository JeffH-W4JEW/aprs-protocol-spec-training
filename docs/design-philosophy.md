# THE APRS DESIGN PHILOSOPHY

## Net Cycle Time

It is important to note that APRS is primarily a _real-time, tactical_ communications tool, to help the flow of information for things like special events, emergencies, Skywarn, the Emergency Operations Center and just plain in-the-field use under stress. But like the real world, for 99% of the time it is operating routinely, waiting for the unlikely serious event to happen.

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

Merge this in: [**Digipeater ID rates**](http://www.aprs.org/digis/digi-rates.txt) for optimum APRS networks add to net cycle time

## Packet Timing

Since APRS packets are error-free, but are not guaranteed delivery, APRS transmits information redundantly. To assure rapid delivery of new or changing data, and to preserve channel capacity by reducing interference from old data, APRS should transmit new information more frequently than old information.

There are several algorithms in use to achieve this:

- **Decay Algorithm** — Transmit a new packet once and n seconds later. Double the value of n for each new transmission. When n reaches the net cycle time, continue at that rate. Other factors besides “doubling” may be appropriate, such as for new message lines.

- **Fixed Rate** — Transmit a new packet once and n seconds later. Transmit it x times and stop.

- **Message-on-Heard** — Transmit a _new_ packet according to either algorithm above. If the packet is still valid, and has not been acknowledged, and the net cycle time has been reached, then the recipient is probably not available. However, if a packet is then subsequently heard from the recipient, try once again to transmit the packet.

- **Time-Out** — This term is used to describe a time period beyond which it is reasonable to assume that a station no longer exists or is off the air if no packets have been heard from it. A period of 80 minutes is suggested as the nominal default timeout to account for stations via satellites. This time-out is not used in any transmitting algorithms, but is useful in some programs to decide when to cease displaying stations as “active”. Note that on HF, signals come and go, so decisions about activity may need to be more flexible.

## Generic Digipeating

The power of APRS in the field derives from the use of _generic_ digipeating, in that packets are propagated without a prior knowledge of the network.

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

## Communicating Map Views Unambiguously

APRS is a tactical geographical system. To maximize its operational effectiveness and minimize confusion between operators of different systems, users need to have an unambiguous way to communicate to others the “location” and “size” (or area of coverage) of any map view.

The APRS convention is by reference to a _center_ and _range_ which specify the geographical center and approximate radius of a circle that will fit in the map view independent of aspect ratio. The radius of the circle (in nautical miles, statute miles or km) is known as the “[range scale](http://www.aprs.org/aprs11/RangeScale.txt)”. This convention gives all users a simple common basis for describing any specific map view to others over any communications medium or program.

If you are on a 64 mile "range Scale" that means that everything within 64 miles of the center is visible. If you zoom to the 8 mile range scale, then anyone else that is on an "8 mile range scale" (no matter what version of APRS he is running) he will see the same "area". It is only one line of code to convert from an arbitrary "zoom factor" or "map size" to a meaningful "range scale". And APRS would be so much the better for it...

> **Do these items flow well here?**
>
> **APRS 1.1 SPEC Operating Conventions for the Good of the APRS Network:**
>
> **We don’t have an existing operating conventions section -- probably add to chapter 2.**

- [**APRS Voice-Alert**](http://www.aprs.org/VoiceAlert3.html) for instant voice contact with any APRS mobile

- [**Recommended DIGI paths**](http://www.aprs.org/aprs11/hacks.txt) and striving for _network protection_ ahead of too much user flexibility.

- **Auto-Answer messages** are SPAM to the network in most cases. To limit their impact, all original APRS clients adhered to these rules for auto-answer messages:

  1. Any such AA message text should begin with AA:
  2. Any such AA message should not have a Line number (so it does not cause acks)
  3. Any such AA message is only sent once on each incoming message packet (no forced retries)
  4. Any such AA message should default to OFF on power up.
  5. Any such AA message should be canceled when the client detects the return presence of the operator
  6. Any such AA message can be ended with a }yy REPLY-ACK if the software supports it

- **Where are these AA messages defined?**
