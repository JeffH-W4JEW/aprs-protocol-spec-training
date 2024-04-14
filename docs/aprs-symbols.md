# APRS Symbols

**Three Methods** There are three methods of specifying an APRS symbol (display icon):

- In the AX.25 Information field.

- In the AX.25 Destination Address.

- In the SSID of the AX.25 Source Address.

The preferred method is to include the symbol in the Information field. However, where this is not possible (for example, in stand-alone trackers with no means of introducing the symbol into the Information field), either of the other two methods may be used instead.

**The Symbol Tables** There are two APRS Symbol Tables:

- Primary Symbol Table

- Alternate Symbol Table

**See Appendix 2 for a full listing of these tables.**

The essential difference between the Primary and Alternate Symbol Tables is that some of the symbols in the Alternate Symbol Table can be overlaid with an alphanumeric character. For example, a “car” icon in the Alternate Symbol Table could be overlaid with the digit “3”, to indicate it is car \#3.

Symbols capable of taking an overlay are marked as **\*\[with overlay**\]\*. None of the symbols in the Primary Symbol Table can be overlaid. In the tables, each symbol is coded in three ways:

- **/$** or **\\** — for symbols in the Information field.

- **GPSxyz** — for generic Destination addresses containing symbols.

- **GPSCnn** or **GPSEnn** — another form of generic Destination
  addresses containing systems.

In addition, 15 of the symbols in the Primary Symbol Table have an associated SSID (e.g. a small aircraft has SSID -7). The SSID is intended for use in the AX.25 Source Address of stand-alone trackers which have no other means of specifying the symbol.

## Symbols in the AX.25 Information

### Field

A symbol in the AX.25 Information field is a combination of a one-character Symbol Table Identifier and a one-character Symbol Code.

For example, in the Position Report:

@092345z4903.50N**/**07201.75W**&gt;**088/036…

the forward slash **/** is the Symbol Table Identifier and the **&gt;** character is the Symbol Code (in this case representing a “car” icon) from the selected table.

The Symbol Table Identifier character selects one of the two Symbol Tables, or it may be used as single-character (alpha or numeric) overlay, as follows:

<table>
<colgroup>
<col style="width: 28%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>Symbol Table Identifier</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Selected Table or Overlay Symbol</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>/</strong></p>
</blockquote></td>
<td><blockquote>
<p>Primary Symbol Table (mostly stations)</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>\</strong></p>
</blockquote></td>
<td><blockquote>
<p>Alternate Symbol Table (mostly Objects)</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>0</strong>-<strong>9</strong></p>
</blockquote></td>
<td><blockquote>
<p>Numeric overlay. Symbol from Alternate Symbol Table
(<em>uncompressed</em> lat/long data format)</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>a</strong>-<strong>j</strong></p>
</blockquote></td>
<td><blockquote>
<p>Numeric overlay. Symbol from Alternate Symbol Table
(<em>compressed</em> lat/long data format only). i.e. a-j maps to
0-9</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>A</strong>-<strong>Z</strong></p>
</blockquote></td>
<td><blockquote>
<p>Alpha overlay. Symbol from Alternate Symbol Table</p>
</blockquote></td>
</tr>
</tbody>
</table>

In the generic case, a symbol from the Primary Symbol Table is represented as the character-pair **/$**, and a symbol from the Alternate Symbol Table as

**\\**.

## Overlays with Symbols in the AX.25 Information

### Field

Where the Symbol Table Identifier is 0-9 or A-Z (or a-j with _compressed_ position data only), the symbol comes from the _Alternate_ Symbol Table, and is overlaid with the identifier (as a single digit or a capital letter).

For example, in the _uncompressed_ Position Report:

@092345z4903.50N**3**07201.75W**&gt;**…

The digit **3** following the latitude will cause the number “3” to be overlaid on top of the “car” icon (**Note**: Because the symbol is overlaid, the **&gt;** Symbol Code here comes from the _Alternate_ Symbol Table).

Similarly, to overlay a “car” icon with the letter “B” in a _compressed_ Position Report, the report will look something like:

=**B**L!!&lt;\*e7 **&gt;**7P\[

However, in a _compressed_ Position Report, it is not permissible to use a _numeric_ Symbol Table Identifier (0-9) — _compressed_ positions never start with a digit. If a numeric overlay is required, the report must use a lower-case letter instead (in the range **a**-**j**) as the Symbol Table Identifier. The lower- case letter is then mapped to the digits **0**-**9** (i.e. a=0, b=1, c=2, d=3 etc).

Thus, in the _compressed_ Position Report:

=**d**5L!!&lt;\*e7 **&gt;**7P\[

the letter **d** maps to overlay character “3”.

As noted above, not all symbols from the Alternate Symbol Table may be overlaid in this way — those that can be overlaid are marked as **_\[with overlay\]_** in Appendix 2. This means that they are _capable_ of taking an overlay, but they do not necessarily need to have one. Thus, for example, the following report uses the car symbol from the Alternate Symbol Table, but does not display an overlay:

@092345z4903.50N**\\**07201.75W**&gt;**…

## Symbols in the AX.25 Destination

### Address

Where it is not possible to include a symbol in the Information field, the symbol may be specified in the AX.25 Destination Address instead, using the following generic destination addresses: GPSxyz, GPSCnn, GPSEnn, SPCxyz and SYMxyz.

The characters xy and nn refer to entries in the APRS Symbol Tables. For example, from the Primary Symbol Table, a tracker could use the Destination Address GPS**MV** or GPS**30** to specify a “car” icon.

The character z specifies the overlay character (where permitted), or is a **** (space) — the space is a filler character, as all AX.25 addresses must be exactly 6 characters long.

The GPS/SPC/SYMxy and GPSCnn/GPSEnn addresses can be used interchangeably. Thus, for example, GPSBM , SPCBM , SYMBM and GPSC12 all specify a “Boy Scouts” icon (from the Primary Symbol Table), and GPSOM , SPCOM , SYMOM and GPSE12 all specify a “Girl Scouts” icon (from the Alternate Symbol Table).

## Overlays with Symbols in the AX.25 Destination

### Address

If the z character in a GPSxyz, SPCxyz or SYMxyz address is not a space, it specifies an alphanumeric overlay character, in the range 0-9 or A-Z.

Overlays can only be used with symbols from the Alternate Symbol Table marked with the legend **_\[with overlay\]_**.

For example, if the “car” icon is to be overlaid with a digit “3”, the Destination Address will be GPS**NV3**.

However, even if the address is overlay-capable, it is not actually necessary to specify an overlay; e.g. GPS**NV**.

GPSCnn and GPSEnn symbols can not have overlays.

## Symbol in the Source Address

## SSID

Where it is not possible to include a symbol in the Information field or in the Destination Address, the symbol may be specified in the SSID of the Source Address instead:

**SSID-Specified Icons in the AX.25 Source Address Field**

**Symbol Precedence** APRS packets should not contain more than one symbol. However, it is conceivably possible to (erroneously) construct a packet containing up to three different symbols.

For example:

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 22%" />
<col style="width: 22%" />
<col style="width: 40%" />
</colgroup>
<thead>
<tr class="header">
<th rowspan="2"></th>
<th><blockquote>
<p><em><strong>Source Address SSID</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Destination Address</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Information Field</strong></em></p>
</blockquote></th>
</tr>
<tr class="odd">
<th><blockquote>
<p>G3NRW<strong>-7</strong></p>
</blockquote></th>
<th><blockquote>
<p>GPS<strong>MV</strong></p>
</blockquote></th>
<th><blockquote>
<p>!0123.45N<strong>/</strong>01234.56W<strong>j</strong></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong>Symbol</strong></em></p>
</blockquote></td>
<td><blockquote>
<p>Small Aircraft</p>
</blockquote></td>
<td><blockquote>
<p>Car</p>
</blockquote></td>
<td><blockquote>
<p>Jeep</p>
</blockquote></td>
</tr>
</tbody>
</table>

In such a situation:

- The symbol in the Information field takes precedence over any other
  symbol.

- If there is no symbol in the Information field, the symbol in the
  Destination Address takes precedence over the symbol in the Source
  Address SSID.

## SSID Recommendations

It is very convenient to other mobile operators or others, looking at callsigns flashing by, to be able to recognize some common applications at a glance. Here are the recommendations for the 16 possible SSIDs. Note, The SSID of zero is dropped by most display applications. So a callsign with no SSID has an SSID of 0.

| SSID | Type of Station                                             |
| ---- | ----------------------------------------------------------- |
| -0   | Your primary station, usually fixed and message capable     |
| -1   | Generic additional station, digi, mobile, wx, etc.          |
| -2   | Generic additional station, digi, mobile, wx, etc.          |
| -3   | Generic additional station, digi, mobile, wx, etc.          |
| -4   | Generic additional station, digi, mobile, wx, etc.          |
| -5   | Other networks (Dstar, iPhones, Androids, Blackberrys etc.) |
| -6   | Special activity, Satellite ops, camping or 6 meters, etc.  |
| -7   | Walkie talkies, HTs or other human portable                 |
| -8   | Boats, sailboats, RVs or second main mobile                 |
| -9   | Primary Mobile (usually message capable)                    |
| -10  | Internet, IGates, echolink, winlink, AVRS, APRN, etc.       |
| -11  | Balloons, aircraft, spacecraft, etc.                        |
| -12  | APRStt, DTMF, RFID, devices, one-way trackers\*, etc.       |
| -13  | Weather stations                                            |
| -14  | Truckers or generally full time drivers                     |
| -15  | Generic additional station, digi, mobile, wx, etc.          |
