# AUTHORS’ FOREWORD

This reference document describes what is known as _APRS Protocol Version 1.0_, and is essentially a description of how APRS operates today.

It is intended primarily for the programmer who wishes to develop APRS- compliant applications, but will also be of interest to the ordinary user who wants to know more about what goes on “under the hood”.

It is not intended, however, to be a dry-as-dust, pedantic, RFC-style programming specification, to be read and understood only by the Mr Spocks of this world. We have included many items of general information which, although strictly not part of the formal protocol description, provide a useful background on how APRS is actually used on the air, and how it is implemented in APRS software. We hope this will put APRS into perspective, will make the document more readable, and will not offend the purists too much.

It is important to realize how APRS originated, and to understand the design philosophy behind it. In particular, we feel strongly that APRS is, and should remain, a light-weight tactical system — almost anyone should be able to use it in temporary situations (such as emergencies or mobile work or weather watching) with the minimum of training and equipment.

This document is the result of inputs from many people, and collated and massaged by the APRS Working Group. Our sincere thanks go to everyone who has contributed in putting it together and getting it onto the street. If you discover any errors or omissions or misleading statements, please let us know.

The best way to do this is via the TAPR _aprsspec_ mailing list at [www.tapr.org.](http://www.tapr.org/)

> **FIXME**

Finally, users throughout the world are continually coming up with new ideas and suggestions for extending and improving APRS. We welcome them.

Again, the best way to discuss these is via the _aprsspec_ list.

The APRS Working Group August 2000

**Disclaimer** Like any navigation system, APRS is not infallible. No one should rely blindly on APRS for navigation, or in life-and-death situations. Similarly, this specification is not infallible.

The members of the APRS Working Group have done their best to define the APRS protocol, but this protocol description may contain errors, or there may be omissions. It is very likely that not all APRS implementations will fully or correctly implement this specification, either today or in the future.

We urge anyone using or writing a program that implements this specification to exercise caution and good judgement. The APRS Working Group and the specification’s Editor disclaim all liability for injury to persons or property that may result from the use of this specification or software implementing it.
