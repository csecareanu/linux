
Tutorial: https://www.youtube.com/watch?v=sa-TUpSx1JA

## Regular expressions allow us to search for specific pattern of text.

The __reqular_exp.txt__ file used to run the exampless for this tutorial:
```
abcdefghijklmnopqurtuvwxyz
ABCDEFGHIJKLMNOPQRSTUVWXYZ
1234567890

Ha HaHa

MetaCharacters (Need to be escaped):
.[{()\^%|?*+

coreyms.com

321-555-4321
123.555.1234

Mr. Schafer
Mr Smith
Ms Davis
Mrs. Robinson
Mr. T
```


## Metacharcters

__Metacharacters__ are symbols or characters that have a special meaning within a __regular expression__.

The list of supported regular expression metacharacters: __[] * ? + - % _ | () {} \ ^ $ . :__

If you want to search for a metacharacter you have to escape it.\
To escape character we can use __backslash__.

`grep "." reqular_exp.txt`
will match the entire file because the '__.__' metacharacter matches any single character.

If you want to serach for the '.' character then you have to escpe it because it is a methacharacter within a regular expression:\
`grep '\.' reqular_exp.txt`

The '\\' charater is a special character also. So if you want to search specifically for a backslash then you have to escape it itself:\
`grep '\\' reqular_exp.txt`

## Syntax

| Syntax | Description |
| ----------- | ----------- |
|.|Any Character Except New Line|
|\d|Digit (0-9)|
|\D|Not a Digit (0-9)|
|\w|Word Character (a-z, A-Z, 0-9, _)|
|\W|Not a Word Character|
|\s|Whitespace (space, tab, newline)|
|\S|Not Whitespace|
|-|_<sub>Anchors. They don't actually match any characters. They match invisible position before or after characters</sub>_|
|\b|Word Boundary `grep '\bHa' reqular_exp.txt`, `grep 'Ha\b' reqular_exp.txt`, `grep '\bHa\b' reqular_exp.txt`|
|\B|Not a Word Boundary `grep '\BHa' reqular_exp.txt`, `grep 'Ha\B' reqular_exp.txt`, `grep '\BHa\B' reqular_exp.txt`|
|^||Beginning of a String `grep '^Ha' reqular_exp.txt`|
|$|End of a String `grep 'Ha$' reqular_exp.txt`|
|-|-|
|[]|Character set. Matches Characters in brackets. <sub>Don't need to escape special characters in a character set</sub>|
|[^ ]|Matches Characters NOT in brackets|
|\||Either Or|
|()|Group|
|-|<sub>Quantifiers</sub>|
|*|0 or More|
|+|1 or More|
|?|0 or One|
|{3}|Exact Number|
|{3,4}|Range of Numbers|

---

- Match all digits: `\d`
- Match three digits in a row: `\d\d\d`
- Match three digists in a row fallowed by '.' or '-' characters: `\d\d\d[.-]`
- Match a phone number of format "321-555-4321" or "123.555.1234": `\d\d\d[.-]\d\d\d[.-]\d\d\d\d`
- `\d\d\d[.-]\d\d\d[.-]\d\d\d\d` will match "321-555-4321" but will not match "321--555-4321" (`[.-]` searches for only one character)
- Match only phone numbers which start with "800" or "900" `[89]00[.-]\d\d\d[.-]\d\d\d\d
- Match phone numbers using _quatifiers_: `\d{3}[.-]\d{3}[.-]\d{4}`
- Match only digits form 1 to 7: `[1-7]`
- Match all lower case characters from a to z: `[a-z]`
- Match all upper case or lower case characters: `[a-zA-Z]`
- `[^]`: In a character set the "^" symbol negates the set and matches everything that is not in the set.
- Match everything that is not lower case letter: `[^a-z]`
- Match everything that is not 'b' followed by 'at': will match 'cat', 'mat' but will not match 'pat'
- Match strings of the form: "Mr. Schafer", "Mr Smith", "Mr. T": `Mr\.?\s[A-Z]\w*`
- Match strings of the form (using groups): "Mr. Schafer", "Mr Smith", "Mr. T", "Ms Davis", "Mrs. Robinson" : `M(r|s|rs)\.?\s[A-Z]\w*`
