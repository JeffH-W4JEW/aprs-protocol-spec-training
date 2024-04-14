# Status Reports

> A Status Report announces the station’s current mission or any other
> single line status to everyone. The report is contained in the AX.25
> Information field, and starts with the **&gt;** APRS Data Type
> Identifier.
>
> The report may optionally contain a timestamp.
>
> **Note**: The timestamp can _only_ be in DHM _zulu_ format.
>
> The status text occupies the rest of the Information field, and may be
> up to 62 characters long (if there is no timestamp in the report) or
> 55 characters (if there is a timestamp). The text may contain any
> printable ASCII characters except **|** or **~**.
>
> Bytes:
>
> Although the status will usually be plain language text, there are two
> cases where the report can contain special information which can be
> decoded:

- Beam Heading and Power

- Maidenhead grid locator

## Status Report with Beam Heading and Effective Radiated

## Power

> It is useful to include beam heading and ERP in packets in meteor
> scatter work. To keep packets as short as possible, these parameters
> are encoded into two characters, as follows:
>
> H = beam heading / 10
>
> (H=0–9 for 0–90 degrees, and A–Z for 100–350 degrees).
>
> P = ERP code.
>
> The HP value appears as the _last_ two characters of the status text,
> preceded by the **^** character — for example, **^B7** means a beam
> heading of 110 degrees and an ERP of 490 watts.
>
> The HP value may be combined with the Maidenhead grid locator (as
> described below), or with any other plain language status text.

## Status Report with Maidenhead Grid

## Locator

> The Maidenhead grid locator may be 4 or 6 characters long, and must
> immediately follow the **&gt;** Data Type Identifier.
>
> All letters must be transmitted in upper case. Letters may be received
> in upper case or lower case.
>
> The Symbol Table Identifier and Symbol Code follow the locator.
>
> If the report also contains status text, the first character of the
> text _must_ be a space.
>
> A Status Report with Maidenhead locator can not have a timestamp.
>
> Bytes:

## Transmitting Status

## Reports

> Each station should only transmit a Status Report once every net cycle
> time (i.e. once every 10, 20 or 30 minutes), or in response to a
> query.
