# Position and DF Report Data Formats

# FIXME - continue here with heading 2 attribute

> **Position Reports** Lat/Long Position Reports are contained in the
> Information field of an APRS AX.25 frame.
>
> The following diagrams show the permissible formats of these reports,
> together with some examples. The gray areas indicate optional fields,
> and the shaded (yellow) characters are literal ASCII characters. In
> all cases there is a maximum of 43 characters after the Symbol Code.
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
> **DF Reports** DF Reports are contained in the Information field of an
> APRS AX.25 frame. The Bearing and Number/Range/Quality (BRG/NRQ)
> parameters follow the Data Extension field.
>
> **Note**: The BRG/NRQ parameters are only meaningful when the report
> contains the DF symbol (i.e. the Symbol Table ID is **/** and the
> Symbol Code is **\\**).
>
> **Note**: If the DF station is fixed, the Course value is zero. If the
> station is moving, the Course value is non-zero.
>
> Bytes:
