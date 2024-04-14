# Weather Reports

## Weather Report

## Types

APRS is an ideal tool for reporting weather conditions via packet. APRS is also ideally suited for the Skywarn weather observer initiative.

APRS supports three types of Weather Report:

- Raw Weather Report (not recommended - sending system should reformat
  data into Complete Weather Report format)

- Positionless Weather Report (not recommended - sending system should
  reformat data into Complete Weather Report format)

- Complete Weather Report

## Data Type Identifiers

The following APRS Data Type Identifiers are used in Weather Reports containing raw data:

**!** Ultimeter 2000

**\#** Peet Bros U-II

**$** Ultimeter 2000

**\*** Peet Bros U-II

**\_** Positionless weather data

In addition, where the raw data has been post-processed (for example, by the insertion of station location information), the four position Data Type Identifiers **!**, **=**, **/** and **@** may be used instead. In this case, the Weather Report is identified with the weather symbol **/\_** or **\\** in the APRS Data.

## Raw Weather

## Reports

Raw weather data from a stand-alone weather station is contained in the Information Field of an APRS AX.25 frame. **Raw Weather Formats are not recommended**. Microprocessors should convert to _complete_ format on RF.

Bytes:

## Positionless Weather Reports

Generic raw weather data from a stand-alone weather station is contained in the Information Field of an APRS AX.25 frame. The WinAPRS "positionless" weather format is not recommended.

Bytes:

## APRS Software

## Type

A Weather Report may contain a single-character code S for the type of APRS software that is running at the weather station:

**d** = APRSdos

**M** = MacAPRS

**P** = pocketAPRS

**S** = APRS+SA

**W** = WinAPRS

**X** = X-APRS (Linux)

## Weather Unit

## Type

A Weather Report may contain a 2–4 character code uuuu for the type of weather station unit. The following codes have been allocated:

= Davis

= Heathkit

= PIC device

= Radio Shack

= Original Ultimeter U-II (auto mode)

= Original Ultimeter U-II (remote mode)

= Ultimeter 500/2000

= Remote Ultimeter logger

= Ultimeter 500

= Remote Ultimeter packet mode

O = Otracker

K = Kenwood

B = Byonics

Y = Yaesu

Users may specify any other 2–4 character code for devices not in this list.

**Positionless Weather Data**

The format of weather data within a Positionless Weather Report differs according to the type of weather station unit, but generically consists of some or all of the following elements:

Bytes:

where: **c** = wind direction (in degrees).

**s** = sustained one-minute wind speed (in mph).

**g** = gust (peak wind speed in mph in the last 5 minutes).

**t** = temperature (in degrees Fahrenheit). Temperatures below zero are expressed as -01 to -99.

**r** = rainfall (in hundredths of an inch) in the last hour.

**p** = rainfall (in hundredths of an inch) in the last 24 hours.

**P** = rainfall (in hundredths of an inch) since midnight.

**h** = humidity (in %. 00 = 100%).

**b** = barometric pressure (in tenths of millibars/tenths of hPascal).

Other parameters that are available on some weather station units include:

**L** = luminosity (in watts per square meter) 000 to 999.

**l** (lower-case letter “L”) = luminosity (in watts per square meter) 1000 and above. (Actual value is 1000 more than 3 digit number.)

(L is inserted in place of one of the rain values).

**s** = snowfall (in inches) in the last 24 hours. 3 digits.

A decimal point is allowed for non-integer values.

**\#** = raw rain counter

**Note**: The weather report must include at least the MDHM date/timestamp, wind direction, wind speed, gust and temperature, but the remaining parameters may be in a different order (or may not even exist).

**Note**: Where an item of weather data is unknown or irrelevant, its value may be expressed as a series of dots or spaces. For example, if there is no wind speed/direction/gust sensor, the wind values could be expressed as:

**c...s...g...** or **csg**

For example, Jim’s rain gauge may produce a report like this:

\_10090556c...s...g...t...P012Jim

(The date/timestamp, wind direction/speed/gust and temperature parameters must be included, even though they are not meaningful).

**Location of a Raw and Positionless Weather Stations**

APRS cannot display weather data on a map until it knows the location of the sending station. In the case of a station transmitting Raw or Positionless Weather Reports, the station has to occasionally send an additional packet containing its position (using any of the legal lat/long and compressed lat/long position formats described earlier).

**Raw Weather Formats** not recommended. Microprocessors should convert to _complete_ format on RF.

**Symbols with Raw and Positionless Weather Stations**

Because Raw and Positionless Weather Reports do not contain a display symbol in the AX.25 Information field, it is possible to specify the symbol in a generic APRS destination address (e.g. GPSHW or GPSE63) instead.

Alternatively, if the weather station is on a balloon, the SSID –11 may be used in the source address (e.g. N0QBF-11).

**Raw Weather Formats** not recommended. Microprocessors should convert to _complete_ format on RF.

See Chapter 20: APRS Symbols for more detail on the usage of symbols.

**Complete Weather**

**Reports with Timestamp and**

**Position**

An APRS Complete Weather Report can contain a timestamp and location information, using any of the legal lat/long and compressed lat/long position formats described earlier. An APRS Object may also have weather information associated with it.

Examples of report formats are shown below. Note that the Symbol Code in every case is the **\_** (underscore). Also, the 7-byte Wind Direction and Wind Speed Data Extension replace the **c**ccc and **s**sss fields of a Positionless Weather Report.

**FIXME: underscore becomes space in examples**

Bytes:

Bytes:

Bytes:

Bytes:

Bytes:

**Storm Data** APRS reports can contain data relating to tropical storms, hurricanes and tropical depressions. The format of the data is as follows:

Bytes:

where: ST = **TS** (Tropical Storm)

**HC** (Hurricane)

**TD** (Tropical Depression).

www = sustained wind speed (in knots).

GGG = gust (peak wind speed in knots).

pppp = central pressure (in millibars/hPascal)

RRR = radius of hurricane winds (in nautical miles). rrr = radius of tropical storm winds (in nautical miles). ggg = radius of “whole gale” (i.e. 50 knot) winds (in nautical miles). Optional.

Storm data will usually be included in an Object Report, but may also be included in a Position Report or an Item Report.

The display symbol will be either:

**\\** Hurricane/Tropical Storm (current position)

**/@** Hurricane (predicted future position)

For example, the progress of Hurricane Brenda could be expressed in Object Reports like these:

;BRENDA\*092345z4903.50N**\\**07202.75W**@**088/036/HC/150^200/0980&gt;090&030%040

;BRENDA\*100045z4905.50N**/**07201.75W**@**101/047/HC/104^123/0980&gt;065&020%040

**National Weather Service Bulletins**

APRS supports the dissemination of National Weather Service bulletins. See [Chapter 14: Messages, Bulletins and Announcements](msg-bulletin.md).
