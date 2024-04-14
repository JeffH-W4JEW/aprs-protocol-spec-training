# Network Tunneling and Third-Party Digipeating

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
