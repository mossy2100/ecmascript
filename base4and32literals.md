

# Proposal For Adding Base 4 and Base 32 Integer Literals to EcmaScript

Shaun Moss

October 2018

## Introduction

The C language introduced the practice of using the prefixes "0" and "0x"
to denote octal and hexadecimal integer literals respectively. Many subsequent languages
descended from C have incorporated this notation, which has since evolved in a fairly standard way.
For example, following Perl, Python, Ruby and other languages, ES6 (EcmaScript 2015) added
binary literals, using the prefix "0b", and the alternative prefix of "0o" for octal literals. 

In this article, an extension to this syntax for integer literals is proposed that will additionally
support base 4 (_quaternary_) and base 32 (_triacontakaidecimal_) integer literals.

Although the utility of base 4 and base 32 literals may be less than that of other radixes,
historically there has been sufficient interest in base 32 literal encodings that at least four
similar notations have been proposed.
By contrast, base 4 integer literals appear to have never been used in computing before; however,
they could easily be included for the sake of completeness, and because their usefulness may yet
be revealed by creative programmers.
While we don't want useless features in JavaScript, this addition of this feature would hardly be
intrusive or bloat the language, and it will be interesting to see how programmers make use of
them.

## Summary of Syntax Design

The prefixes "0q" and "0t" are used to indicate quaternary (base 4) and triacontakaidecimal
(base 32) integer literals respectively, paralleling the existing syntax for binary, octal, and
hexadecimal literals.

| Bits per digit | Radix (base) | Name | Prefix | Valid Digits                 |
|---|----|-------------------------|--------|----------------------------------|
| 1 | 2  | binary                  | 0b     | 01                               |
| 2 | 4  | __quaternary__          | __0q__ | 0123                             |
| 3 | 8  | octal                   | 0o     | 01234567                         |
| 4 | 16 | hexadecimal             | 0x     | 0123456789ABCDEF                 |
| 5 | 32 | __triacontakaidecimal__ | __0t__ | 0123456789ABCDEFGHJKMNPQRSUVWXYZ |

Both prefixes and letter-digits may be coded as either upper or lower case for
consistency with hexadecimal literals.

## Quaternary (base 4)

Quaternary integer literals are introduced here for several reasons:
* for the sake of completeness
* to support the development of programs related to genetics, Hilbert curves, 2B1Q data
transmission line codes, and other potential uses
* to see what kinds of interesting things creative programmers and engineers use them for
* to be a point of difference for EcmaScript

Notation for quaternary numbers is very simple: a prefix of "0q" followed by a string comprised of
the digits 0, 1, 2 and 3, e.g. _0q32011120_.

## Triacontakaidecimal (base 32)

A new syntax for triacontakaidecimal (base 32) integer literals is proposed. This is very similar
to the system created by Douglas Crockford (author of "JavaScript: The Good Parts"), with one
slight improvement (discussed below).

Several RFCs have proposed a system for base 32, but none (it seems) have yet been introduced
into a programming language for use in literals. All systems proposed to date derive a set of 32
digit characters by starting with a set of 36 characters comprising the 10 digits and 26 letters
of the English alphabet, and then excluding four of these. All systems accept both upper and
lower-case letters, with the value of an upper-case letter equal to its lower-case variant.

Triacontakaidecimal digit characters should be backwards compatible with their values in other
integer literals, i.e. The digits 0..9 and A..F should thus have the same value in both hexadecimal
and triacontakaidecimal, just as decimal and octal digits have the same value in hexadecimal. This
will be consistent, compatible with the JavaScript parseInt() function, simplify implementation,
reduce programmer error and bugs, and improve learnability of the notation. 

#### RFC 4648

The letters of the alphabet are assigned the values 0–25, and the digits 2-7 are assigned the values
26-31. The digits 0, 1, 8, and 9 are excluded. This system is not compatible with hexadecimal
(i.e. A means 0 instead of 10, etc.) and is therefore suboptimal.

See:
- S. Josefsson, Ed., RFC 3548, Network Working Group, July 2003
http://www.ietf.org/rfc/rfc3548.txt
- S. Josefsson, RFC 4648, Network Working Group, October 2006
http://www.ietf.org/rfc/rfc4648.txt


#### z-base-32

This system excludes 0, 1, V, and 2 due to their similarity with O, I, U and Z respectively. The
32 digit characters are mapped to the values 0-31 in an unobvious and seemingly random fashion.
For example, in this system, Y means 0. Again, the digit character values are not compatible with
literals in other radixes.

See: Z. O'Whielacronx, "Human-Oriented Base-32 Encoding", November 2009
http://philzimmermann.com/docs/human-oriented-base-32-encoding.txt

#### base32hex

This system is compatible with hexadecimal, using the digits 0-9 and the first 22 letters
of the alphabet, thus excluding W, X, Y, and Z. However, the potential exists for confusion between
the digits 0 and 1, and the letters O, I, and L.

See: G. Klyne and L. Masinter, RFC 2938, Network Working Group, September 2000
http://www.ietf.org/rfc/rfc2938.txt

#### Crockford

Crockford's system uses all digits and letters, excluding "O" to avoid confusion between
upper-case 'O' and the digit '0', and excluding "I" and "L" to avoid confusion between the digit
"1", upper-case "I", and lower-case "L" (i.e. "l").

The letter "U" is also excluded, to reduce the likelihood that curse words appearing in literals.
However, this reason is weak, since:
- plenty of curse words don't contain "U", so it's not a solution to the imagined problem
- the main readers of code are programmers, and the one language all programmers know is profanity
- the likelihood that anyone will be exposed to triacontakaidecimal integer
  literals on a regular enough basis that a curse word would accidentally appear in one and the user
  would be offended rather than amused is vanishgly small and of minimal consequence

See: Douglas Crockford, "Base32 Encoding"
http://www.crockford.com/wrmg/base32.html
  
#### New system

The proposal offered here is a slight modification of Crockford's, excluding "T" rather than "U",
to avoid confusion between the "0t" prefix and the digit characters.
The excluded letters can easily be remembered thusly: "Programming should be fun, not TOIL".

*Digits and equivalent decimal values*

| Digit | Value |   | Digit | Value |
|---|----|---|---|----|
| 0 | 0  |   | G | 16 | 
| 1 | 1  |   | H | 17 | 
| 2 | 2  |   | J | 18 | 
| 3 | 3  |   | K | 19 | 
| 4 | 4  |   | M | 20 | 
| 5 | 5  |   | N | 21 | 
| 6 | 6  |   | P | 22 | 
| 7 | 7  |   | Q | 23 | 
| 8 | 8  |   | R | 24 | 
| 9 | 9  |   | S | 25 | 
| A | 10 |   | U | 26 | 
| B | 11 |   | V | 27 | 
| C | 12 |   | W | 28 | 
| D | 13 |   | X | 29 | 
| E | 14 |   | Y | 30 | 
| F | 15 |   | Z | 31 |

##### Interpretation of invalid digits

In Crockford's system, the letter "O" is accepted as 0, and the letters "I" and "L" as 1.

For consistency with the existing language and to reduce programmer error, this behaviour is
not proposed for inclusion in the feature. The presence of any invalid letters or other characters,
including "O", "I" and "L" , should trigger a syntax error, and parseint() should return "NaN" (or
ignore trailing invalid characters) as it does when parsing integer literals in other radixes.

## Proposed implementation

@todo
