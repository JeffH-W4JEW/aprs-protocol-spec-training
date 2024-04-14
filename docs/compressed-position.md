# Compressed Position Report Data Formats

In compressed data format, the Information field contains the station’s latitude and longitude, together with course and speed or pre-calculated radio range or altitude. This information is compressed to minimize the length of the transmitted packet (and therefore improve its chances of being received correctly under less than ideal conditions).

The Information field also contains a display Symbol Code, and there may optionally be a plain text comment (uncompressed) as well.

## The Advantages of Data Compression

Compressed data format may be used in place of the numeric lat/long coordinates already described, such as in the **!**, **/**, **@** and **=** formats.

Data compression has several important benefits:

- Fully backwards compatible with all existing formats.

- Fully supports any comment string.

- Speed is accurate to +/-1 mph up to about 40 mph and within 3% at
  600 mph.

- Altitude in feet is accurate to +/- 0.4% from 1 foot to 3000 miles.

- Consistent one-algorithm processing of compressed latitude and
  longitude.

- Improved position to 1 foot worldwide.

- Pre-calculated radio range, compressed to one byte.

- Potential 50% compression of every position format on the air.

- Potential 40% reduction of raw GPS NMEA data length.

- Additional 7-byte reduction for NEMA GGA altitudes.

- Support for TNC compression at the NMEA source (from the GPS
  receiver).

- Digipeater compression of old NMEA trackers on the fly.

- Usage is optional in all cases.

The only minor disadvantages are that the course only resolves to +/- 2 degrees, and this format does not support PHG.

## Compressed Data Format

Compressed data may be generated in several ways:

- by APRS software.

- pre-entered manually into a digipeater’s beacon text.

- by a digipeater converting raw tracker NMEA packets to compressed.

\[In future, there is the possibility that a Kantronics KPC-3 or other tracker TNC will be able to compress data directly from an attached GPS receiver\].

In all cases the compressed format is a fixed 13-character field:

```text
/YYYYXXXX$csT
```

Where
/ is the Symbol Table Identifier YYYY is the compressed latitude

XXXX is the compressed longitude

$ is the Symbol Code

cs is the compressed course/speed or compressed pre-calculated radio range or compressed altitude

T is the compression type indicator

> **COMPRESSED POSITION DATA TABLE HERE**

Compressed format can be used in place of lat/long position format anywhere that …ddmm.hhN/dddmm.hhW$xxxxxxx… occurs.

All bytes except for the **/** and **$** are base-91 printable ASCII characters (**!**..**{**). These are converted to numeric values by subtracting 33 from the decimal ASCII character code. For example, **\#** has an ASCII code of 35, and represents a numeric value of 2 (i.e. 35-33).

### Symbol

The presence of the leading Symbol Table Identifier instead of a digit indicates that this is a compressed Position Report and not a normal lat/long report.

### Lat/Long Encoding

The values of YYYY and XXXX are computed as follows:

YYYY is 380926 x (90 – latitude) \[base 91\]

latitude is positive for north, negative for south, in degrees.

XXXX is 190463 x (180 + longitude) \[base 91\]

longitude is positive for east, negative for west, in degrees.

For example, for a longitude of 72° 45' 00" west (i.e. -72.75 degrees), the math is 190463 x (180 – 72.75) = 20427156. Because this is to base 91, it is then necessary to progressively divide this value by reducing powers of 91, to obtain the numerical values of X:

> 20427156 / 91<sup>3</sup> = **27**, remainder 80739
>
> 80739 / 91<sup>2</sup> = **9**, remainder 6210
>
> 6210 / 91<sup>1</sup> = **68**, remainder **22**
>
> To obtain the corresponding ASCII characters, 33 is added to each of these values, yielding 60 (i.e. 27+33), 42, 101 and 55. From the ASCII Code Table (in Appendix 3), this corresponds to **&lt;\*e7** for XXXX.

### Lat/Long Decoding

To decode a compressed lat/long, the reverse process is needed. That is, if

> YYYY is represented as
> y<sub>1</sub>y<sub>2</sub>y<sub>3</sub>y<sub>4</sub> and XXXX as
> x<sub>1</sub>x<sub>2</sub>x<sub>3</sub>x<sub>4</sub>, then:
>
> Lat = 90 - ((y<sub>1</sub>-33) x 91<sup>3</sup> + (y<sub>2</sub>-33) x
> 91<sup>2</sup> + (y<sub>3</sub>-33) x 91 + y<sub>4</sub>-33) / 380926
>
> Long = -180 + ((x<sub>1</sub>-33) x 91<sup>3</sup> +
> (x<sub>2</sub>-33) x 91<sup>2</sup> + (x<sub>3</sub>-33) x 91 +
> x<sub>4</sub>-33) / 190463
>
> For example, if the compressed value of the longitude is **&lt;\*e7**
> (as computed above), the calculation becomes:

Long = -180 + (27 x 91<sup>3</sup> + 9 x 91<sup>2</sup> + 68 x 91 + 22)
/ 190463

= -180 + (20346417 + 74529 + 6188 + 22) / 190463

> = -180 + 107.25
>
> = -72.75 degrees

### Course/Speed, Pre-Calculated Radio Range and Altitude

The two cs bytes following the Symbol Code character can contain either the compressed course and speed or the compressed pre-calculated radio range or the station’s altitude. These two bytes are in base 91 format.

In the special case of c = **�** (space), there is no course, speed or range data, in which case the csT bytes are ignored.

#### Course/Speed

If the ASCII code for c is in the range **!** to **z** inclusive — corresponding to numeric values in the range 0–89 decimal (i.e. after subtracting 33 from the ASCII code) — then cs represents a compressed course/speed value:

> course = **c** x 4
>
> speed = 1.08**s** – 1
>
> For example, if the cs characters are **7P**, the corresponding values
> of **c** and **s** (after subtracting 33 from the ASCII character
> code) are 22 and 47 respectively. Substituting these values in the
> above equations:
>
> course = **22** x 4 = 88 degrees
>
> speed = 1.08**47** – 1 = 36.2 knots
>
> **Pre-Calculated Radio Range** — If c = **{**, then cs represents a
> compressed pre-calculated radio range value:
>
> range = 2 x 1.08**s**
>
> For example, if the cs bytes are **{?**, the ASCII code for **?** is
> 63, so the value of **s** is 30 (i.e. 63-33). Thus:
>
> range = 2 x 1.08**30**
>
> ~ 20 miles
>
> So APRS will draw a circle of radius 20 miles around the station plot
> on the screen.

### The Compression Type (T) Byte

The T byte follows the cs bytes. The T byte contains several bit fields showing the GPS fix status, the NMEA source of the position data and the origin of the compression.

The T byte is not meaningful if the c byte is **�** (space).

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 13%" />
<col style="width: 15%" />
<col style="width: 9%" />
<col style="width: 7%" />
<col style="width: 13%" />
<col style="width: 13%" />
<col style="width: 13%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="8"><blockquote>
<p><em><strong>Compression Type (T) Byte Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>6</p>
</blockquote></td>
<td><blockquote>
<p>5</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>3</p>
</blockquote></td>
<td><blockquote>
<p>2</p>
</blockquote></td>
<td>1</td>
<td>0</td>
</tr>
<tr class="even">
<td><blockquote>
<p><em>Not used</em></p>
</blockquote></td>
<td><blockquote>
<p><em>Not used</em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>GPS Fix</strong></em></p>
</blockquote></td>
<td colspan="2"><blockquote>
<p><em><strong>NMEA Source</strong></em></p>
</blockquote></td>
<td colspan="3"><blockquote>
<p><em><strong>Compression Origin</strong></em></p>
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
<p>0 = old (last)</p>
</blockquote></td>
<td colspan="2"><blockquote>
<p>0 0 = other</p>
</blockquote></td>
<td colspan="3"><blockquote>
<p>0 0 0 = Compressed</p>
</blockquote></td>
</tr>
<tr class="even">
<td></td>
<td></td>
<td><blockquote>
<p>1 = current</p>
</blockquote></td>
<td colspan="2"><blockquote>
<p>0 1 = GLL</p>
</blockquote></td>
<td colspan="3"><blockquote>
<p>0 0 1 = TNC BText</p>
</blockquote></td>
</tr>
<tr class="odd">
<td></td>
<td></td>
<td></td>
<td colspan="2"><blockquote>
<p>1 0 = GGA</p>
</blockquote></td>
<td colspan="3"><blockquote>
<p>0 1 0 = Software (DOS/Mac/Win/+SA)</p>
</blockquote></td>
</tr>
<tr class="even">
<td></td>
<td></td>
<td></td>
<td colspan="2"><blockquote>
<p>1 1 = RMC</p>
</blockquote></td>
<td colspan="3"><blockquote>
<p>0 1 1 = [tbd]</p>
</blockquote></td>
</tr>
<tr class="odd">
<td></td>
<td></td>
<td></td>
<td colspan="2"></td>
<td colspan="3"><blockquote>
<p>1 0 0 = KPC3</p>
</blockquote></td>
</tr>
<tr class="even">
<td></td>
<td></td>
<td></td>
<td colspan="2"></td>
<td colspan="3"><blockquote>
<p>1 0 1 = Pico</p>
</blockquote></td>
</tr>
<tr class="odd">
<td></td>
<td></td>
<td></td>
<td colspan="2"></td>
<td colspan="3"><blockquote>
<p>1 1 0 = Other tracker [tbd]</p>
</blockquote></td>
</tr>
<tr class="even">
<td></td>
<td></td>
<td></td>
<td colspan="2"></td>
<td colspan="3"><blockquote>
<p>1 1 1 = Digipeater conversion</p>
</blockquote></td>
</tr>
</tbody>
</table>

For example, if the compressed position was derived from an RMC sentence, the fix is current, and the compression was performed by APRSdos software, then the value of T in binary is 0 0 1 11 010, which equates to 58 decimal.

Adding 33 to this value gives the ASCII code for the T byte (i.e. 91), which corresponds to the **\[** character.

Thus, using data from all the earlier examples, if the RMC sentence contains (among other parameters) the following data:

> Latitude = 49° 30' 00" north
>
> Longitude = 72° 45' 00" west Speed = 36.2 knots
>
> Course = 88°
>
> and: the fix is current
>
> compression is performed by APRSdos software the display symbol is a
> “car”
>
> then the complete 13-character compressed location field is
> transmitted as:

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 26%" />
<col style="width: 26%" />
<col style="width: 10%" />
<col style="width: 26%" />
</colgroup>
<thead>
<tr class="header">
<th>/</th>
<th>YYYY</th>
<th>XXXX</th>
<th><blockquote>
<p>$</p>
</blockquote></th>
<th><blockquote>
<p>csT</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>/</strong></td>
<td><strong>5L!!</strong></td>
<td><strong>&lt;*e7</strong></td>
<td><blockquote>
<p><strong>&gt;</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>7P[</strong></p>
</blockquote></td>
</tr>
</tbody>
</table>

### Altitude

If the T byte indicates that the raw data originates from a GGA sentence (i.e. bits 4 and 3 of the T byte are 10), then the sentence contains an altitude value, in feet. After compression, the compressed altitude data is placed in the cs bytes, such that:

> altitude = 1.002**cs** feet

For example, if the received cs bytes are **S\]**, the computation is as follows:

- Subtract 33 from the ASCII code for each character:

> **c** = 83 – 33 = 50
>
> **s** = 93 – 33 = 60

- Multiply **c** by 91 and add **s** to obtain **cs**: **cs** = 50 x
  91 + 60

> = 4610

- Then altitude = 1.002**4610**

= 10004 feet

### New Trackers

Tracker firmware may compress GPS data directly to APRS compressed format. They would use the **!** Data Type Indicator, showing that the position is real-time and that the tracker is not APRS-capable.

If the Position Report is not real-time, then the **/** Data Type Indicator can be used instead, so that the latest fix time may be included.

### Old Trackers

Some digipeaters have the ability to convert raw NMEA strings from existing trackers to compressed data format for further forwarding.

These digipeaters will compress the data if the tracker Destination Address is GPS. (**Note**: This is the 3-letter address GPS, not GPS\*).

Trackers desiring for their packets to not be modified by the APRS network will use any other valid generic APRS Destination Address.

### Compressed Report Formats

Compressed data is contained in the AX.25 Information field, in these formats:

> TABLE Compressed Lat/Long Position Report Format — no Timestamp
