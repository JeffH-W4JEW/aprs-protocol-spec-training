# Telemetry Data

**Telemetry Report**

**Format**

> The [AX.25](https://www.tapr.org/pdf/AX25.2.2.pdf) Information field can contain telemetry data. The APRS Data
> Type Identifier is **T**.
>
> The report Sequence Number is a 3-character value — typically a 3-digit number, or the three letters **MIC**. In the case of **MIC**, there may or may not be a comma preceding the first analog data value.

There are five 8-bit unsigned analog data values (expressed as 3-digit decimal numbers in the range 000–255), followed by a single 8-bit digital data value (expressed as 8 bytes, each containing **1** or **0**).

The Kantronics KPC-3+ TNC and [APRS Micro Interface Module (MIM)](http://www.aprs.org/mic-lite.html) use this format.

In reality, the restriction of fixed width 3 digits is often ignored. It is common to see much longer variable width values including decimal points. Most modern applications recognize this relaxed format. Others are encouraged to do the same.

Bytes:

**On-Air Definition of**

**Telemetry Parameters**

In principle, received telemetry data may be interpreted in any appropriate way. In practice, however, an APRS user can define the telemetry parameters (such as quadratic coefficients for the analog values, or the meaning of the binary data) at any time, and then send these definitions as APRS messages. Other stations receiving these messages will then know how to interpret the data.

This is achieved by sending four messages:

- A Parameter Name message.

- A Unit/Label message.

- An Equation Coefficients message.

- A Bit Sense/Project Name message.

The messages addressee is the callsign of the station transmitting the telemetry data. For example, if N0QBF launches a balloon with the callsign N0QBF-11, then the four messages are addressed to N0QBF-11.

See Chapter 14: Messages, Bulletins and Announcements for full details of message formats.

**Parameter Name**

**Message**

The Parameter Name message contains the names (N) associated with the five analog channels and the 8 digital channels. Its format is as follows:

Bytes:

**Historical Note**: The field widths are not all the same (this is a legacy arising from earlier limitations in display screen width). Note also that the byte counts _include_ the comma separators where shown.

The list can terminate after any field.
>
> Compatibility note: Many implementations ignore these overly
> restrictive inconsistent name lengths. It is not uncommon to see names
> with a dozen characters or more. New/upgraded applications should
> handle what is in common use.
>
> **Unit/Label Message** The Unit/Label message specifies the units (U)
> for the analog values, and the labels (L) associated with the digital
> channels:
>
> Bytes:
>
> **Historical Note**: Again, the field widths are not all the same, and
> the byte counts
>
> _include_ the comma separators where shown. The list can terminate
> after any field.
>
> Compatibility note: Many implementations ignore these overly
> restrictive inconsistent unit lengths. It is not uncommon to see much
> longer unit descriptions. New/upgraded applications should handle what
> is in common use.
>
> **Equation Coefficients Message**
>
> The Equation Coefficients message contains three coefficients (a, b
> and c) for each of the five analog channels.
>
> Bytes:
>
> To obtain the final value of an analog channel, these coefficients are
> substituted into the equation:
>
> a x v<sup>2</sup> + b x v + c
>
> where v is the raw received analog value.
>
> For example, analog channel A1 in the above beacon examples relates to
> the battery voltage, expressed in hundredths of volts, and a = 0, b =
> 5.2, c = 0. If the raw received value v is 199, then the voltage is
> calculated as:
>
> voltage = 0 x 199<sup>2</sup> + 5.2 x 199 + 0
>
> = 1034.8 hundredths of a volt
>
> = 10.348 volts
>
> **Bit Sense/ Project Name**

**Message**

> The Bit Sense/Project Name message contains two types of information:

- An 8-bit pattern of ones and zeros, specifying the sense of each
  digital channel that matches the corresponding label.

- The name of the project associated with the telemetry station.

> Bytes:
>
> Thus in the above message examples, if digital channel B1 is 1, this
> indicates the camera has clicked. If channel B2 is 0, the parachute
> has opened, and so on.
