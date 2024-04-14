# Station Capabilities, Queries, and Responses

> **Station Capabilities** A station may define a set of one or more
> attributes of the station, known as Station Capabilities. The station
> transmits its capabilities in response to an IGATE query (see below),
> using the **&lt;** Data Type Identifier.
>
> Each capability is a TOKEN or a TOKEN=VALUE pair. More than one
> capability may be on a line, with each capability separated by a
> comma.
>
> Currently defined capabilities include:
>
> IGATE,MSG_CNT=n,LOC_CNT=n
>
> where IGATE defines the station as an IGate, MSG_CNT is the number of
> messages transmitted, and LOC_CNT is the number of “local” stations
> (those to which the IGate will pass messages in the local RF network).

## Queries and Responses

> There are two types of APRS queries. One is general to all stations
> and the other is in a message format directed to a single individual
> station.
>
> Queries always begin with a **?**, are one-time transmissions, do not
> have a message identifier and should not be acknowledged. Similarly
> the responses to queries are one-time transmissions that also do not
> have a message identifier, so that they too are not acknowledged.
>
> Each query contains a Query Type (in upper-case). The following Query
> Types and expected responses are supported:

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 41%" />
<col style="width: 44%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p><em><strong>Query Type</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Query</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Response</strong></em></p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>APRS</strong></p>
</blockquote></td>
<td><blockquote>
<p>General — All stations query</p>
</blockquote></td>
<td><blockquote>
<p>Station’s position and status</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>APRSD</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for stations heard direct</p>
</blockquote></td>
<td><blockquote>
<p>List of stations heard direct</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>APRSH</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query if an individual station has heard a particular
station</p>
</blockquote></td>
<td><blockquote>
<p>Position of heard station as an APRS Object, plus heard statistics
for the last 8 hours</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>APRSM</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for outstanding unacknowledged
or undelivered messages</p>
</blockquote></td>
<td><blockquote>
<p>All outstanding messages for the querying station</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>APRSO</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for its Objects</p>
</blockquote></td>
<td><blockquote>
<p>Station’s Objects</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>APRSP</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for its position</p>
</blockquote></td>
<td><blockquote>
<p>Station’s position</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>APRSS</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for its status</p>
</blockquote></td>
<td><blockquote>
<p>Station’s status</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>APRST</strong> or</p>
<p><strong>PING?</strong></p>
</blockquote></td>
<td><blockquote>
<p>Directed — Query an individual station for a trace (i.e. path by
which the packet was heard)</p>
</blockquote></td>
<td><blockquote>
<p>Route trace</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>IGATE</strong></p>
</blockquote></td>
<td><blockquote>
<p>General — Query all Internet Gateways</p>
</blockquote></td>
<td><blockquote>
<p>IGate station capabilities</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>WX</strong></p>
</blockquote></td>
<td><blockquote>
<p>General — Query all weather stations</p>
</blockquote></td>
<td><blockquote>
<p>Weather report (and the station’s position if it is not included in
the Weather Report)</p>
</blockquote></td>
</tr>
</tbody>
</table>

> Bytes:
>
> If a queried station has no relevant information to include in a
> response, it need not respond.
>
> A queried station should ignore any query that it does not recognize.

**General Queries** The format of a general query is as follows:

<table>
<colgroup>
<col style="width: 3%" />
<col style="width: 7%" />
<col style="width: 3%" />
<col style="width: 6%" />
<col style="width: 3%" />
<col style="width: 7%" />
<col style="width: 3%" />
<col style="width: 9%" />
<col style="width: 55%" />
</colgroup>
<thead>
<tr class="header">
<th colspan="8"><blockquote>
<p><em><strong>General Query Format</strong></em></p>
</blockquote></th>
<th rowspan="4"></th>
</tr>
<tr class="odd">
<th rowspan="2"><blockquote>
<p><strong>?</strong></p>
</blockquote></th>
<th rowspan="2"><blockquote>
<p><em><strong>Query Type</strong></em></p>
</blockquote></th>
<th rowspan="2"><blockquote>
<p><strong>?</strong></p>
</blockquote></th>
<th colspan="5"><blockquote>
<p><em><strong>Target Footprint</strong></em></p>
</blockquote></th>
</tr>
<tr class="header">
<th><blockquote>
<p><em><strong>Lat</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><strong>,</strong></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Long</strong></em></p>
</blockquote></th>
<th><blockquote>
<p><strong>,</strong></p>
</blockquote></th>
<th><blockquote>
<p><em><strong>Radius</strong></em></p>
</blockquote></th>
</tr>
<tr class="odd">
<th><blockquote>
<p>1</p>
</blockquote></th>
<th><blockquote>
<p>n</p>
</blockquote></th>
<th><blockquote>
<p>1</p>
</blockquote></th>
<th><blockquote>
<p>n</p>
</blockquote></th>
<th><blockquote>
<p>1</p>
</blockquote></th>
<th><blockquote>
<p>n</p>
</blockquote></th>
<th><blockquote>
<p>1</p>
</blockquote></th>
<th><blockquote>
<p>4</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td colspan="9"><blockquote>
<p><u>Examples</u></p>
<p><u>Query</u> <u>Typical Response</u></p>
<p>?APRS? /092345z4903.50N/07201.75W&gt;</p>
<p>General query, with standard posit and status reply. &gt;092345zNet
Control Center</p>
<p>?APRS?<strong></strong>34.02,-117.15,0200 /3402.78N11714.02W-</p>
<p>General query for stations within a target footprint &gt;Digi on low
power</p>
<p>of radius 200 miles centered on 34.02 degrees north, 117.15 degrees
west, with standard posit and status reply. (Note the leading space in
the latitude, as its value is positive, see below).</p>
<p>?IGATE? &lt;IGATE,MSG_CNT=43,LOC_CNT=14</p>
<p>General query for IGate stations, with a Station Capabilities
reply.</p>
<p>?WX? _10090556c220s004g005t077…</p>
<p>Query for weather stations, with a standard
/090556z4903.50N/07201.75W&gt;</p>
<p>Weather Report reply (without a position), followed by a standard
posit.</p>
</blockquote></td>
</tr>
</tbody>
</table>

> In the case of an ?APRS? query for stations within a particular target
> footprint, the latitude and longitude parameters are in _floating
> point_ degrees (_not_ in APRS lat/long position format).

- North and east coordinates are positive values, indicated by a
  leading ****

> (space).

- South and west coordinates are negative values.

- The radius of the footprint is in miles, expressed as a fixed
  4-digit number in whole miles.

> All stations inside the specified coverage circle should respond with
> a Position Report and a Status Report.

## Directed Station

## Queries

> Queries addressed to individual stations are in APRS message format
> (except that they never include a message identifier). The addressee
> is the callsign of the station being queried.
>
> The message text is the Query Type. This is followed optionally by
> another callsign — this callsign does not need filler spaces as it is
> at the end of the data.
>
> Bytes:
