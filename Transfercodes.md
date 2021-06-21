# Transfercodes

This file documents how transfer codes are constructed.

## Construction

A transfer code consists of 9 characters.

The first 8 are randomly chosen from the following 29-character alphabet: `1234567890ABCDEFHKMNPRSTUWXYZ`.

Note that the following characters are left out to reduce confusion when entering a code: "G, I, J, L, O, Q, V”

The final, 9th character is a check character (a.k.a checksum, check digit).
It is computed using the [Luhn mod N algorithm](https://en.wikipedia.org/wiki/Luhn_mod_N_algorithm)
over the above alphabet.

The purpose of this check character is to provide some level of feedback when the transfer code is manually
entered by test center staff.

## Test cases

The following transfer codes are valid:

```
Y8P8ECFN8
HDTYRB66W
YS6R7H88T
K42K6F7R2
3BY8DAZYS
ADWYF11SY
453S6HUA6
WR7UPHB4A
37WDPRSKM
01AWUUB2M
MA4S9CNUK
SY7M684WA
X216WN3YF
3C2YFKCNP
TNKBZ0TSK
```

The following codes are invalid:

```
Y8P8ECFN9
HDTYRC66W
YS6RH788T
K43K6F7R2
3B8YDAZYS
ADWFY11SY
453S6HU6A
WR7UHPB4A
37WDRPSKM
10AWUUB2M
MAS49CNUK
SY7M864WA
$%(*(!@#$_!@*#
```

