# Messages, Bulletins, and Annoucements

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
  