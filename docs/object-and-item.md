# Object and Item Reports

> **Objects and Items** Any APRS station can manually report the
> position of an APRS entity (e.g. another station or a weather
> phenomenon). This is intended for situations where the entity is not
> capable of reporting its own position.
>
> APRS provides two types of report to support this:

- Object Reports

- Item Reports

> Object Reports specify an Object’s position, can have an optional
> timestamp, and can include course/speed information or other Extended
> Data. Object Reports are intended primarily for plotting the positions
> of moving objects (e.g. spacecraft, storms, marathon runners without
> trackers).
>
> Item Reports specify an Item’s position, but cannot have a timestamp.
> While Item reports may also include course/speed or other Extended
> Data, they are really intended for inanimate things that are
> occasionally posted on a map (e.g. marathon checkpoints or first-aid
> posts). Otherwise they are handled in the same way as Object Reports.
>
> Objects are distinguished from each other by having different Object
> names. Similarly, Items are distinguished from each other by having
> different Item names.
>
> <u>Implementation Recommendation</u>: When an APRS Object/Item is
> displayed on the screen, the callsign of the station sending the
> report should be associated with the Object/Item.
>
> **Replacing an Object / Item**
>
> A fundamental precept of APRS is that any station may take over the
> reporting responsibility for an APRS Object or Item, by simply
> transmitting a new report with the same Object/Item name.
>
> The replacement report may specify the existing location or a new
> location.
>
> The original station will cease transmitting an Object/Item Report
> when it sees an incoming report with the same name from another
> station.
>
> **Killing an Object / Item**
>
> To kill an Object/Item, a station transmits a new Object/Item Report,
> with a “kill” character following the Object/Item name.
>
> <u>Implementation Recommendation</u>: When an Object/Item is killed it
> should be removed from display on the screen. However, the data
> associated with the Object/Item should be retained internally in case
> it is needed later.

**Object Report**

**Format**

> An Object Report has a _fixed_ 9-character Object name field, which
> may consist of any printable ASCII characters, including embedded
> spaces. Trailing spaces are used to make the field 9 characters wide.
> These are not part of the name.
>
> Object names are case-sensitive.
>
> The ; is the APRS Data Type Identifier for an Object Report, and a
> **\*** or **\_**
>
> separates the Object name from the rest of the report:
>
> **\*** indicates a live Object.
>
> **\_** indicates a killed Object.

- The position may be in lat/long or compressed lat/long format, and
  the report may also contain Extended Data. Compressed Objects are
  not recommended for use on RF due to incompatibilities. For 1 foot
  precision, use the !DAO! format.

> An Object always has a timestamp.
>
> The Comment field may contain any appropriate APRS data (see the
> _Comment Field_ section in Chapter 5: APRS Data in the AX.25
> Information Field).
>
> Bytes:
>
> Bytes:

**Item Report**

**Format**

> An Item Report has a _variable-length_ Item name, 3–9 characters long.
> The name may consist of any printable ASCII characters, including
> embedded space, _except_ **!** or **\_** because they are the field
> terminator.
>
> Item names are case-sensitive.
>
> Note that the name field is variable length, rather than fixed, as
> with objects.
>
> The **)** is the APRS Data Type Identifier for an Item Report, and a
> **!** or **\_**
>
> separates the Item name from the rest of the report:
>
> **!** indicates a live Item.
>
> **\_** is the Item “kill” character.
>
> The position may be in lat/long or compressed lat/long format. There
> is no provision for a timestamp. The report may also contain Extended
> Data.
>
> The Comment field may contain any appropriate APRS data (see the
> _Comment Field_ section in Chapter 5: APRS Data in the AX.25
> Information Field).
>
> APRS 1.1: The Item Format is not recommended on RF due to
> incompatibilities.
>
> Bytes:
>
> Bytes:
>
> **Area Objects** Using the **\l** symbol (i.e. the lower-case letter
> “L” symbol from the Alternate Symbol Table) it is possible to define
> circle, line, ellipse, triangle and box objects in all colors, either
> open or filled in, any size from 60 feet to 100 miles.
>
> These Objects are useful for real-time events such as for a
> search-and-rescue, or adding a special road or route for a special
> event.
>
> The Object format is specified as a 7-character APRS Data Extension
>
> Tyy/Cxx immediately following the **l** Symbol Code. For example:
>
> ;OBJECT\*ddmm.hhN**\\**dddmm.hhW **l** Tyy/Cxx
>
> where:
>
> T is the type of object shape.
>
> /C is the color of the object.
>
> yy is the square root of (the latitude offset in degrees / 1500).
>
> xx is the square root of (the longitude offset in degrees / 1500).

On the receiving end:

offset_in_degrees = (offset_in_packet^2) / 1500

> NOTE: This specification originally had a scaling factor of 100.
> <http://www.aprs.org/aprs11/areaobjects.txt> contains the correction
> to make it 1500. All of the modern applications checked use 1500.
>
> The object type and color codes are as follows:

- Triangles are always isosceles triangles, oriented vertically and
  the points of reference are the top APEX and the lower right corner.

- Circles are defined by a square box referenced by the upper left
  corner and the center.

- Lines are referenced to the upper end.

> The 'offset reference' position of the object is the upper left corner
>
> of the object and the offsets are the distance from the lower right
>
> corner (or center of a circle) back to this "offset reference"
> position.
>
> (An exception is the special case of a type-6 line which is drawn down
>
> and to the left).
>
> Here are some examples of Object Position Reports. The latitude and
> longitude offsets are each 4 minutes = 4/60 degree = 100/1500 of a
> degree), so
>
> yy = xx = √100 = 10.
>
> ;SEARCH\*092345z4903.50N**\\**07201.75W **l 710/310**
>
> A high intensity cyan filled ellipse, yy=10, xx=10
>
> ;SEARCH\*092345z4903.50N**\\**07201.75W **l 8101310**
>
> A low intensity violet filled triangle, yy=10, xx=10
>
> Further, with the line option (Type 1 and Type 6) it is possible to
> specify a “corridor” either side of the central line. The width of the
> corridor (in miles) either side of the line is specified in the
> comment text, enclosed by **{}**.
>
> For example:
>
> ;FLIGHTPTH\*4903.50N**\\**07201.75W **l 610/310{100}**
>
> A high intensity cyan line, with a 100-mile corridor either side
>
> **Note**: The color fill option should be used with care, since a
> color-filled object will obscure information displayed underneath it.
>
> **Signpost Objects/Items**
>
> Signpost Objects/Items (with the symbol **\m**) display as a yellow
> box with a 1–3-character overlay on them. The overlay is specified by
> enclosing the 1–3 characters in braces in the comment field. Thus a
> signpost with {55} would appear as a sign with **55** on it.
>
> For example:
>
> )I913N!4903.50N**\\**07201.75W**m{55}**
>
> This was originally designed for posting the speed of traffic past
> speed measuring devices, but can be used for any purpose.
>
> <u>Implementation Recommendation</u>: Signposts should not display any
> callsign or name, and to avoid clutter should only be displayed at
> close range.

**Obsolete Object**

**Format**

> Some stations transmit Object reports without the **;** APRS Data Type
> Identifier. This format is obsolete. Some software may still decode
> such data as an Object, but it should now be interpreted as a Status
> Report.
