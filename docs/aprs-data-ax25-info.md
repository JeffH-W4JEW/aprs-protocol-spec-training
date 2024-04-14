# APRS Data in the AX.25 Information Field

## Generic Data

## Format

Bytes:

## APRS Data Type

## Identifier

In general, the AX.25 Information field can contain some or all of the following information:

- APRS Data Type Identifier

- APRS Data

- APRS Data Extension

- Comment

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 34%" />
<col style="width: 16%" />
<col style="width: 36%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><em><strong>Generic APRS Information Field</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong>Data Type ID</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>APRS Data</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>APRS Data Extension</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Comment</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
</tr>
</tbody>
</table>

Every APRS packet contains an APRS Data Type Identifier (DTI). This determines the format of the remainder of the data in the Information field, as follows:

**APRS Data Type Identifiers**

**Note**: The Kenwood TM-D700 radio uses the **'** DTI for _current_ Mic-E data. The radio does not use the ‘ DTI.

## APRS Data and Data Extension

There are 10 main types of APRS Data:

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

Some of this data may also have an APRS Data Extension that provides additional information.

The APRS Data and optional Data Extension follow the Data Type Identifier.

The table on the next page shows a complete list of all the different possible types of APRS Data and APRS Data Extension.

**Comment Field** In general, any APRS packet can contain a plain text comment (such as a beacon message) in the Information field, immediately following the APRS Data or APRS Data Extension.

There is no separator between the APRS data and the comment unless otherwise stated.

The freeform text part of the comment may contain any printable ASCII or UTF-8 character.

Do not put any carriage return (0x0d) or line feed (0x0a) at the end. IGate stations will remove them, resulting in slightly different contents.

The maximum length of the comment field depends on the report — details are included in the description of each report.

In special cases, the Comment field can also contain further APRS data:

- **Altitude** in comment text (see Chapter 6: Time and Position Formats), or in Mic-E status text (see Chapter 10: Mic-E Data Format).

- **Maidenhead Locator** (grid square), in a Mic-E status text field (see Chapter 10: Mic-E Data Format) or in a Status Report (see Chapter 16: Status Reports).

- **Bearing and Number/Range/Quality** parameters (/BRG/NRQ), in DF reports (see Chapter 7: APRS Data Extensions).

- **Area Object Line Widths** (see Chapter 11: Object and Item Reports).

- **Signpost Objects** (see Chapter 11: Object and Item Reports).

- **Weather and Storm Data** (see Chapter 12: Weather Reports).

- **Beam Heading and Power**, in Status Reports (see Chapter 16: Status Reports).

**Base-91 Notation** Two APRS data formats use base-91 notation: lat/long coordinates in compressed format (see Chapter 9) and the altitude in Mic-E format (see Chapter 10).

Base-91 data is compressed into a short string of characters. All the characters are printable ASCII, with character codes in the range 33–124 decimal (i.e. **!** through **|**).

To compute the base-91 ASCII character string for a given data value, the value is divided by progressively reducing powers of 91 until the remainder is less than 91. At each step, 33 is added to the modulus of the division process to obtain the corresponding ASCII character code.

For example, for a data value of 12345678:

<table>
<colgroup>
<col style="width: 31%" />
<col style="width: 4%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p>12345678 / 91<sup>3</sup></p>
</blockquote></th>
<th><blockquote>
<p>=</p>
</blockquote></th>
<th><blockquote>
<p>modulus <strong>16</strong>, remainder 288542</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>288542 / 91<sup>2</sup></p>
</blockquote></td>
<td><blockquote>
<p>=</p>
</blockquote></td>
<td><blockquote>
<p>modulus <strong>34</strong>, remainder 6988</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>6988 / 91<sup>1</sup></p>
</blockquote></td>
<td><blockquote>
<p>=</p>
</blockquote></td>
<td><blockquote>
<p>modulus <strong>76</strong>, remainder <strong>72</strong></p>
</blockquote></td>
</tr>
</tbody>
</table>

The four ASCII character codes are thus 49 (i.e. **16**+33), 67 (i.e. **34**+33), 109 (i.e. **76**+33) and 105 (i.e. **72**+33), corresponding to the ASCII string **1Cmi**.

**APRS Data Units** For historical reasons there is some lack of consistency between units of data in APRS packets — some speeds are in knots, others in miles per hour; some altitudes are in feet, others in meters, and so on. It is emphasized that this specification describes the units of data as they are transmitted on-air. It is the responsibility of APRS applications to convert the on-air units to more suitable units if required.

The default GPS earth datum is World Geodetic System (WGS) 1984 but Continental options such as OSG for the UK are OK.
