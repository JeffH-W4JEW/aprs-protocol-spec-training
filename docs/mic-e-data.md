# Mic-E Data Format

In Mic-E data format, the station’s position, course, speed and display symbol, together with an APRS digipeater path and Mic-E Position Comment, are packed into the AX.25 Destination Address and Information fields.

The Information field can also optionally contain either Mic-E telemetry data or Mic-E status. The Mic-E Status can contain the station’s Maidenhead locator and altitude.

Mic-E packets can be very short. At the minimum, with no callsigns in the Digipeater Addresses field and no optional telemetry data or Mic-E status text, a complete Mic-E packet is just 25 bytes long (excluding FCS and flags).

Mic-E data format is not only used in the [Microphone Encoder unit](https://web.tapr.org/product_docs/Mic-E/mic-edev/mic-e.manual.pdf); it is also used in the [PIC Encoder](https://www.tapr.org/pdf/DCC1999-APRS-MicLite-WB4APR.pdf) and in the Kenwood TH-D7 and TM-D700 radios.

## Mic-E Data Payload

The Mic-E data format allows a large amount of data to be carried in a very short packet. The data is split between the Destination Address field and the Information field of a standard AX.25 UI-frame.

**Destination Address Field** — The 7-byte Destination Address field contains the following encoded information:

- The 6 latitude digits.

- A 3-bit Mic-E position comment, specifying one of 7 Standard Mic-E
  Position Comment Codes or one of 7 Custom Position Comments or an
  Emergency Position Comment.

- The North/South and West/East Indicators.

- The Longitude Offset Indicator.

- The generic APRS digipeater path code.

Although the destination address appears to be quite unconventional, it is still a valid AX.25 address, consisting only of printable 7-bit ASCII values (shifted one bit left) — see the _Amateur Packet-Radio Link-Layer Protocol_ specification for full details of the format of standard AX.25 addresses.

**Information Field** — This field contains the following data:

- The encoded longitude.

- The encoded course and speed.

- The display Symbol Code and Symbol Table Identifier.

- An optional field, containing either Mic-E telemetry data or a Mic-E status text string. The status string can contain plain text, Maidenhead locator or the station’s altitude.

## Mic-E Destination Address Field

The standard AX.25 Destination Address field consists of 7 bytes, containing 6 callsign characters and the SSID (plus a number of other bits that are not of interest here). When used to carry Mic-E data, however, this field has a quite different format:

The Destination Address field contains:

- Six encoded latitude digits specifying degrees (digits 1 and 2), minutes (digits 3 and 4) and hundredths of minutes (digits 5 and 6).

- 3-bit Mic-E Position Comment identifier (bits A, B and C).

- North/South latitude indicator.

- Longitude offset (adds 0 degrees or 100 degrees to the longitude computation in the Information field).

- West/East longitude indicator.

- Generic APRS digipeater path (encoded in the SSID).

## Destination Address Field Encoding

The table on the next page shows the encoding of the first 6 bytes of the Destination Address field, for all combinations of latitude digit, the 3-bit Mic-E message identifier (A/B/C), the latitude/longitude indicators and the longitude offset.

The encoding supports position ambiguity.

The ASCII characters shown in the table are left-shifted one bit position prior to transmission.

> **TABLE - Mic-E Destination Address Field Encoding (Bytes 1–6)**

**Note**: the ASCII characters **A**–**K** are not used in address bytes 4–6.

For example, for a station at a latitude of 33 degrees 25.64 minutes north, in the western hemisphere, with longitude offset +0 degrees, and transmitting standard position comment identifier bits 1/0/0, the encoding of the first 6 bytes of the Destination Address field is as follows:

> **Mic-E Position Comments**
>
> The first three bytes of the Destination Address field contain three
> position comment bits: A, B and C. These bits allow one of 15 position
> comment types to be specified:

- 7 Standard

- 7 Custom

- 1 Emergency

For the 7 Standard position comments, one or more of the identifier bits is a

- **1**, shown in the Mic-E Destination Address Field Encoding table as 1 (Std).

- For the 7 Custom position comments, one or more of the identifier bits is a **1**, shown in the Mic-E Destination Address Field Encoding table as 1 (Custom).

- For the Emergency position comment, all three identifier bits are **0**.

The following table shows the encoding of Mic-E message types, for all combinations of the A/B/C message identifier bits:

## Mic-E Position Comment Types

<table>
<colgroup>
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 36%" />
<col style="width: 36%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>A</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>B</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>C</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Standard Mic-E</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Custom Mic-E</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>M0: Off Duty</p>
</blockquote></td>
<td><blockquote>
<p>C0: Custom-0</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>M1: En Route</p>
</blockquote></td>
<td><blockquote>
<p>C1: Custom-1</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>M2: In Service</p>
</blockquote></td>
<td><blockquote>
<p>C2: Custom-2</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>M3: Returning</p>
</blockquote></td>
<td><blockquote>
<p>C3: Custom-3</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>M4: Committed</p>
</blockquote></td>
<td><blockquote>
<p>C4: Custom-4</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>M5: Special</p>
</blockquote></td>
<td><blockquote>
<p>C5: Custom-5</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>M6: Priority</p>
</blockquote></td>
<td><blockquote>
<p>C6: Custom-6</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td><blockquote>
<p>0</p>
</blockquote></td>
<td colspan="2"><blockquote>
<p>Emergency</p>
</blockquote></td>
</tr>
</tbody>
</table>

The Standard position comments and the Emergency position comment have the same meaning for all APRS stations. The Custom position comments may be assigned any arbitrary meaning.

**Note**: Support for Custom position comments is optional. Original Mic-E units do not support Custom position comments.

**Note**: If the A/B/C position comment identifier bits contain a mixture of Standard **1**s and Custom **1**s, the position comment type is “unknown”.

Some examples of MIC-E Position Comment type encoding:

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 36%" />
<col style="width: 17%" />
<col style="width: 21%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>First 3 Destination Address Bytes</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Identifier Bits A/B/C</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Type</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Meaning</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>S32</strong></p>
</blockquote></td>
<td><blockquote>
<p>Standard 1 / 0 / 0</p>
</blockquote></td>
<td><blockquote>
<p>Standard</p>
</blockquote></td>
<td><blockquote>
<p>M3: Returning</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>F2D</strong></p>
</blockquote></td>
<td><blockquote>
<p>Custom 1 / 0 / Custom 1</p>
</blockquote></td>
<td><blockquote>
<p>Custom</p>
</blockquote></td>
<td><blockquote>
<p>C2: Custom-2</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>234</strong></p>
</blockquote></td>
<td><blockquote>
<p>0 / 0 / 0</p>
</blockquote></td>
<td><blockquote>
<p>Emergency</p>
</blockquote></td>
<td><blockquote>
<p>Emergency</p>
</blockquote></td>
</tr>
</tbody>
</table>

## Destination Address SSID Field\*\*

The SSID in the Destination Address field of a Mic-E packet is coded to specify either a conventional digipeater VIA path (contained in the Digipeater Addresses field of the AX.25 frame), or one of 15 generic APRS digipeater paths. See Chapter 4: APRS Data in the AX.25 Destination and Source Address Fields.

## Mic-E Information Field

The Information field is used to complete the Position Report that was begun in the Destination Address field. The encoding used is different from the destination address since the content is not constrained to be printable, shifted 7-bit ASCII, as it is in the address. However, full 8-bit binary is not used — all values are offset by 28 and further operations (described below) are performed on some of the values to make almost all of the data printable ASCII.

The format of the Information field is as follows:

> Bytes:

## Information Field Data\*\*

The first 9 bytes of the Information field contain the APRS Data Type Identifier, longitude, speed, course and symbol data.

The APRS Data Type Identifier is one of:

- **‘** Current GPS data

> (but not used in Kenwood TM-D700 radios)

- **'** Old GPS data

> (or _Current_ GPS data in Kenwood TM-D700 radios).

- **0x1c** Current GPS data (Rev. 0 beta units only).
- **0x1d** Old GPS data (Rev. 0 beta units only).

**<u>IMPORTANT NOTE</u>**:

As explained in detail below, some of the bytes in the Information field are non-printing ASCII characters. In certain circumstances (such as incorrect TNC setup or inappropriate software), some of these non-printing characters may be dropped, making the Information field appear shorter than it really is. This will lead to incorrect decoding. (In particular, if the Information field appears to be less than 9 bytes long, the packet must be ignored).

## Longitude Degrees Encoding

The **d+28** byte in the Information field contains the encoded value of the longitude degrees, in the range 0–179 degrees.

(Note that for longitude values in the range 0–9 degrees, the longitude offset is +100 degrees):

> **TABLE Mic-E Longitude Degrees Encoding**

Note from the table that the encoding is split into four separate pieces:

- <u>0–9 degrees</u>: **d+28** is in the range 118–127 decimal,
  corresponding to the ASCII characters **v** to **DEL**.

**Important Note**

The longitude offset is set to **+100 degrees** when the longitude is in the range 0–9 degrees.

- <u>10–99 degrees</u>: **d+28** is in the range 38–127 decimal
  (corresponding to the ASCII characters **&** to **DEL**), and the
  longitude offset is +0 degrees.

- <u>100–109 degrees</u>: **d+28** is in the range 108–117 decimal, corresponding to the ASCII characters **l** (lower-case letter “L”) to **“u”**, and the longitude offset is +100 degrees.

- <u>110–179 degrees</u>: **d+28** is in the range 38–107 decimal (corresponding to the ASCII characters **&** to **“k”**), and the longitude offset is +100 degrees.

Thus the overall range of valid **d+28** values is 38–127 decimal (corresponding to ASCII characters **&** to **DEL**).

All of these characters (<u>except **DEL**, for 9 and 99 degrees</u>) are printable ASCII characters.

To decode the longitude degrees value:

1. Subtract 28 from the **d+28** value to obtain **d**.

2. If the longitude offset is +100 degrees, add 100 to **d**.

3. Subtract 80 if 180 ˜ **d** ˜ 189 (i.e. the longitude is in the range 100–109 degrees).

4. Or, subtract 190 if 190 ˜ **d** ˜ 199. (i.e. the longitude is in the range 0–9 degrees).

## Longitude Minutes Encoding

The **m+28** byte in the Information field contains the encoded value of the longitude minutes, in the range 0–59 minutes:

> **TABLE Mic-E Longitude Minutes Encoding**

Note from the table that the encoding is split into two separate pieces:

- <u>0–9 minutes</u>: **m+28** is in the range 88–97 decimal, corresponding to the ASCII characters **X** to **a**.

- <u>10–59 minutes</u>: **m+28** is in the range 38–87 decimal (corresponding to the ASCII characters **&** to **W**).

Thus the overall range of valid **m+28** values is 38–97 decimal (corresponding to ASCII characters **&** to **a**). All of these characters are printable ASCII characters.

To decode the longitude minutes value:

1. Subtract 28 from the **m+28** value to obtain **m**.

2. Subtract 60 if **m** ™ 60. (i.e. the longitude minutes is in the range 0–9).

## Longitude Hundredths of Minutes Encoding

The **h+28** byte in the Information field contains the encoded value of the longitude hundredths of minutes, in the range 0–99 minutes. This byte takes a value in the range 28 decimal (corresponding to 0 hundredths of a minute) through 127 decimal (corresponding to 99 hundredths of a minute).

To decode the longitude hundredths of minutes value, subtract 28 from the **h+28** value.

All of the possible values are printable ASCII characters (<u>except 0–3 and 99</u> <u>hundredths of a minute</u>).

## Speed and Course Encoding

The speed and course of a station are encoded in 3 bytes, designated **SP+28**, **DC+28** and **SE+28**.

The speed is in the range 0–799 knots, and the course is in the range 0–360 degrees (0 degrees represents an unknown or indefinite course, and 360 degrees represents due north).

The encoded speed and course are spread over the three bytes, as follows:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 18%" />
<col style="width: 16%" />
<col style="width: 31%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="2"><blockquote>
<p><em><strong>Speed</strong></em></p>
</blockquote></th>
<th colspan="2"><blockquote>
<p><em><strong>Course</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>Encoded Speed (hundreds/tens of knots)</p>
</blockquote></td>
<td colspan="2"><blockquote>
<p>Encoded Speed (units) and Encoded Course (hundreds of degrees)</p>
</blockquote></td>
<td><blockquote>
<p>Encoded Course (tens/units)</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>SP+28</strong></p>
</blockquote></td>
<td colspan="2"><blockquote>
<p><strong>DC+28</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>SE+28</strong></p>
</blockquote></td>
</tr>
</tbody>
</table>

## SP+28 Encoding

The **SP+28** byte contains the encoded speed, in hundreds/tens of knots, according to this table:

> **SP+28 Speed Encoding (hundreds/tens of knots)**

**Note**: The ASCII characters shown in white on a black background are non- printing characters.

**Note**: For speeds in the range 0–199 knots, there are two encoding schemes in existence. Hence there are two columns for the ASCII character, and two columns for the corresponding **SP+28** byte values.

For example, for a speed of 73 knots (i.e. in the range 70–79), the **SP+28** byte may contain either **s** or **\#**, depending on the encoding method used. Both are equally valid.

The decoding algorithm described later handles either of these encoding schemes.

## DC+28 Encoding

The **DC+28** byte contains the encoded units of speed, plus the encoded course in hundreds of degrees:

> **DC+28 Speed / Course Encoding (units of knots/hundreds of degrees)**

**Note**: The ASCII characters shown in white on a black background are non- printing characters.

**Note**: There are two encoding schemes in existence for the **DC+28** byte. Hence there are two columns for the ASCII character, and two columns for the corresponding **DC+28** byte values.

For example, for a speed of 73 knots (i.e. units=3) and a bearing of 294 degrees (i.e. in the range 200–299), the **DC+28** byte may contain either **@** or **&lt;**, depending on the encoding method used. Both are equally valid.

The decoding algorithm described later handles either of these encoding schemes.

## SE+28 Encoding

The **SE+28** byte contains the encoded tens and units of degrees of the course:

> **TABLE SE+28 Course Encoding (tens/units of degrees)**

## Example of Mic-E Speed and Course Encoding

For a speed of 86 knots and a course of 194 degrees, the encoding is:

> **SP+28**: The speed is in the range 80–89 knots. From the **SP+28** encoding table, the **SP+28** byte may be either **t** or **$**.
>
> **DC+28**: The units of speed are 6, and the course is in the range 100–199 degrees. From the **DC+28** encoding table, the **DC+28** byte may be either **\]** or **Y**.
>
> **SE+28**: The course in tens and units of degrees is 94. From the **SE+28** encoding table, the **SE+28** byte will be **z**.

## Decoding the Speed and Course

To decode the speed and course:

> **SP+28**: To obtain the speed in tens of knots, subtract 28 from the **SP+28** value and multiply by 10.
>
> **DC+28**: Subtract 28 from the **DC+28** value and divide the result by 10. The quotient is the units of speed. The remainder is the course in hundreds of degrees.
>
> **SE+28**: To obtain the tens and units of degrees, subtract 28 from the **SE+28** value.

Finally, make these speed and course adjustments:

- If the computed speed is ™ 800 knots, subtract 800.

- If the computed course is ™ 400 degrees, subtract 400.

## Example of Decoding the Information Field Data

If the first 9 bytes of the Information field contain ‘**(\_fn "Oj/**, and the destination address specifies that the station is in the western hemisphere with a longitude offset of +100 degrees, then the data is decoded as follows:

- ‘ is the APRS Data Type Identifier for a Mic-E packet containing current GPS data.

- **(** is the **d+28** byte. The **(** character has the value 40 decimal. Subtracting 28 gives 12. The longitude offset (in the destination address) is +100 degrees, so the longitude is 100 + 12 = 112 degrees.

- **\_** is the **m+28** byte. The **\_** character has the value 95 decimal. Subtracting 28 gives 67. This is ™ 60, so subtracting 60 gives a value of 7 minutes longitude.

- **f** is the **h+28** byte. The **f** character has the value 102 decimal. Subtracting 28 gives 74 hundredths of a minute.

Thus the longitude is 112 degrees 7.74 minutes west. The speed and course are calculated as follows:

- **n** is the **SP+28** byte. The **n** character has the value 110 decimal. After subtracting 28, the result is 82. As this is ™ 80, a further 80 is subtracted, leaving a result of 2 tens of knots.

- **"** is the **DC+28** byte. The **"** character has the value 34 decimal. Subtracting 28 gives 6. Dividing this by 10 gives a quotient of 0 (units of speed). Adding the first part of the speed, multiplied by 10 (i.e. 20) to the quotient (0) gives a final computed speed of 20 knots.

The remainder from the division is 6. Subtracting 4 gives the course in hundreds of degrees; i.e. 2.

- **O** (upper-case letter “O”) is the **SE+28** byte. The **O** character has the value 79 decimal. Subtracting 28 gives 51. Adding this to the remainder calculated above, multiplied by 100 (i.e. 200), gives the final value of 251 degrees for the course.

The last two characters (**j/**) represent the jeep symbol from the Primary Symbol Table.

## Mic-E Position Ambiguity

As mentioned in Chapter 6 (Time and Position Formats), a station may reduce the precision of its position by introducing position ambiguity. This is also possible in Mic-E data format.

The position ambiguity is specified for the latitude (in the destination address). The same degree of ambiguity will then also apply to the longitude.

For example, if the destination address is **T4SQZZ**, the last two digits of the latitude are ambiguous (represented by **ZZ**). Then, if the longitude data in the Information field is **(\_f** , as in the above example, the last two digits of the computed longitude will be ignored — that is, the longitude will be 112 degrees 7 minutes.

## Mic-E Telemetry Data

The Information field may optionally contain either Mic-E telemetry data values or Mic-E status text.

If the byte following the Symbol Table Identifier is one of the Telemetry Flag characters (**‘**,**'** or 0x1d), then telemetry data follows:

<table>
<colgroup>
<col style="width: 24%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 15%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="6"><blockquote>
<p><em><strong>Optional Mic-E Telemetry Data</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong>Telemetry Flag</strong></em></p>
</blockquote></td>
<td colspan="5"><blockquote>
<p><em><strong>Telemetry Data Channels</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>F</p>
</blockquote></td>
<td><blockquote>
<p>Ch 1</p>
</blockquote></td>
<td><blockquote>
<p>Ch 2</p>
</blockquote></td>
<td><blockquote>
<p>Ch 3</p>
</blockquote></td>
<td><blockquote>
<p>Ch 4</p>
</blockquote></td>
<td><blockquote>
<p>Ch 5</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
<td><blockquote>
<p>1/2</p>
</blockquote></td>
</tr>
</tbody>
</table>

The Telemetry Flag F is one of:

> **‘** 2 printable hex telemetry values follow (channels 1 and 3).
>
> **'** 5 printable hex telemetry values follow.
>
> 0x1d 5 binary telemetry values follow (Rev. 0 beta units only).
>
> If F is **‘** or **'**, each channel requires 2 bytes, containing a 2-digit printable hexadecimal representation of a value ranging from 0–255. For example, 254 is represented as **FE**.
>
> If F is 0x1d, each channel requires one byte, containing an 8-bit binary value.

For example, if the telemetry data is **'7200007100**, the **'** indicates that 5 bytes of telemetry follow, coded in hexadecimal:

> 0x72 = 114 decimal 0x00 = 0 decimal 0x00 = 0 decimal 0x71 = 113 decimal 0x00 = 0 decimal

## Mic-E Status Text

As an alternative to telemetry data, the packet may include Mic-E status text. The status text may be any length that fits in the rest of the Information field.

The Mic-E status text must not start with **‘**,**'** or 0x1d, otherwise it will be confused with telemetry data.

It is possible to include a standard APRS-formatted position in the Mic-E status text field. A suitable position will cause the APRS display software to override any position data the Mic-E has encoded. This is useful if using a Mic-E without a GPS receiver.

**Note**: The Kenwood radios automatically insert a special type code at the front of the status text string (i.e. in the 10th character of the Information field):

- Kenwood TH-D7: **&gt;**

- Kenwood TM-D700: **\]**

These characters should not be confused with the APRS Data Type Identifier that appears at the start of reports.

It is envisaged that other Mic-E-compatible devices will be allocated their own type codes in future.

**Note**: When Kenwood radios receive the status, they can only display a small number of text characters:

- Kenwood TH-D7: 20 characters Kenwood TM-D700: 28 characters

**Note**: The Kenwood TM-D700 radio uses the ' (apostrophe) instead of the ‘ (grave) APRS Data Type Identifier to represent current GPS data. A suggested way of detecting this situation is to examine the first and 10th characters of the Information field; if they are ' and **\]** respectively, then the packet is almost certainly from a TM-D700.

## Maidenhead Locator in the Mic-E Status Text Field

The Mic-E status text field can contain a Maidenhead locator.

If the locator is followed by a plain text comment, the first character of the text _must_ be a space. For example:

> IO91SX/G**** Helloworld (from a Mic-E or PIC-E)
>
> &gt;IO91SX/G**** Helloworld (from a Kenwood TH-D7)
>
> \]IO91SX/G**** Helloworld (from a Kenwood TM-D700) (**/G** is the
> grid locator symbol).

## Altitude in the Mic-E Status Text Field

The Mic-E status text field can contain the station’s altitude. The altitude is expressed in the form xxx**}**, where xxx is in meters relative to 10km below mean sea level (the deepest ocean), to base 91.

For example, to compute the xxx characters for an altitude of 200 feet: 200 feet = 61 meters = 10061 meters relative to the datum

> 10061 / 91<sup>2</sup> = **1**, remainder 1780
>
> 1780 / 91 = **19**, remainder **51**

Adding 33 to each of the highlighted values gives 34, 52 and 84 for the ASCII codes of xxx.

Thus the 4-character altitude string is **"4T}**

Some examples:

> "4T}
>
> &gt;"4T}
>
> \]"4T}

Optional altitude should be _first_ after the Mic-E **type** byte.

## Mic-E Data in Non-APRS Networks

Some parts of the Mic-E AX.25 Information field may contain binary data (i.e. non-printable ASCII characters). If such a packet is constrained to the APRS network, this should not cause any difficulties.

If, however, the packet is to be forwarded via a network that does not reliably preserve binary data (e.g. the Internet), then it is necessary to convert the data to a format that will preserve it.

Further, if the packet subsequently re-emerges back onto the APRS network, it will then be necessary to re-convert the data back to its original format.
