# Appendix 1: APRS Data Formats

> This Appendix contains format diagrams for all APRS data formats. The gray fields are optional. Shaded (yellow) characters are literal ASCII characters.
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:

<table>
<colgroup>
<col style="width: 6%" />
<col style="width: 9%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 9%" />
<col style="width: 19%" />
<col style="width: 7%" />
<col style="width: 23%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="9"><blockquote>
<p><em><strong>Compressed Lat/Long Position Report Format — with
Timestamp</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="3"><blockquote>
<p><strong>/</strong> <em>or</em></p>
<p><strong>@</strong></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Time DHM / HMS</strong></em></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Sym Table ID</strong></em></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Comp Lat</strong></em> YYYY</p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Comp Long</strong></em> XXXX</p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Symbol Code</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Compressed Course/Speed</strong></em></p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Comp Type</strong></em> T</p>
</blockquote></td>
<td rowspan="3"><blockquote>
<p><em><strong>Comment (max 40 chars)</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><em><strong>Compressed Radio Range</strong></em></p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><em><strong>Compressed Altitude</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>2</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0-40</p>
</blockquote></td>
</tr>
</tbody>
</table>

Bit:

Value:

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="7"><blockquote>
<p><em><strong>Mic-E Data — DESTINATION ADDRESS FIELD
Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><em><strong>Lat Digit 1</strong></em></p>
<p><em><strong>+ Message Bit A</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 2</strong></em></p>
<p><em><strong>+ Message Bit B</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 3</strong></em></p>
<p><em><strong>+ Message Bit C</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 4</strong></em></p>
<p><em><strong>+ N/S Lat Indicator</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 5</strong></em></p>
<p><em><strong>+ Longitude Offset</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat Digit 6</strong></em></p>
<p><em><strong>+ W/E Long Indicator</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>APRS</strong></em></p>
<p><em><strong>Digi Path Code</strong></em></p>
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
<p>1</p>
</blockquote></td>
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
<p>1</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:

<table>
<colgroup>
<col style="width: 4%" />
<col style="width: 9%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 6%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 12%" />
<col style="width: 11%" />
<col style="width: 7%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="12"><blockquote>
<p><em><strong>Complete Weather Report Format — with Object and Lat/Long
position</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>*</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Object Name</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><strong>*</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Time DHM / HMS</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Lat</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Sym Table ID</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Long</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Symbol Code</strong></em></p>
<p><strong>_</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Wind Directn/ Speed</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Weather Data</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>APRS</strong></em></p>
<p><em><strong>Software</strong></em></p>
<p>S</p>
</blockquote></td>
<td><blockquote>
<p><em><strong>WX</strong></em></p>
<p><em><strong>Unit</strong></em></p>
<p>uuuu</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>8</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>7</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>2-4</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:

<table style="width:100%;">
<colgroup>
<col style="width: 4%" />
<col style="width: 11%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 9%" />
<col style="width: 12%" />
<col style="width: 23%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="9"><blockquote>
<p><em><strong>Telemetry Report Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>T</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong><em>Sequence No</em> #</strong>nnn<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 1</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 2</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 3</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 4</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Analog Value 5</strong></em> aaa<strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Digital Value</strong></em> bbbbbbbb</p>
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
<p>5</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>4</p>
</blockquote></td>
<td><blockquote>
<p>8</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 12%" />
<col style="width: 3%" />
<col style="width: 63%" />
<col style="width: 3%" />
<col style="width: 14%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="6"><blockquote>
<p><em><strong>Message Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td rowspan="2"><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td rowspan="2"><blockquote>
<p><em><strong>Addressee</strong></em></p>
</blockquote></td>
<td rowspan="2"><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td rowspan="2"><blockquote>
<p><em><strong>Message Text (max 67 chars)</strong></em></p>
</blockquote></td>
<td colspan="2"><blockquote>
<p><em><strong>Message ID</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><strong>{</strong></td>
<td><blockquote>
<p><em><strong>Message No</strong></em></p>
<p>xxxxx</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>0-67</p>
</blockquote></td>
<td>1</td>
<td><blockquote>
<p>1-5</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 8%" />
<col style="width: 33%" />
<col style="width: 8%" />
<col style="width: 16%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="5"><blockquote>
<p><em><strong>Message Acknowledgement Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Addressee</strong></em></p>
</blockquote></td>
<td><blockquote>
<p><strong>:</strong></p>
</blockquote></td>
<td><blockquote>
<p><strong>ack</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Message No</strong></em> xxxxx</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>9</p>
</blockquote></td>
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>3</p>
</blockquote></td>
<td><blockquote>
<p>1–5</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:
>
> Bytes:

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 7%" />
<col style="width: 14%" />
<col style="width: 74%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="4"><blockquote>
<p><em><strong>User-Defined Data Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>{</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>User ID</strong></em> U</p>
</blockquote></td>
<td><blockquote>
<p><em><strong>User-Defined Packet Type</strong></em> X</p>
</blockquote></td>
<td><blockquote>
<p><em><strong>User-defined data (printable ASCII
recommended)</strong></em></p>
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
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 96%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="2"><blockquote>
<p><em><strong>Invalid Data / Test Data Format</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>,</strong></p>
</blockquote></td>
<td><blockquote>
<p><em><strong>Invalid Data or Test Data</strong></em></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><blockquote>
<p>n</p>
</blockquote></td>
</tr>
</tbody>
</table>
