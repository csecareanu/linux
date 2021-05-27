
Regular expressions allow us to search for specific pattern of text.


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

## D

| Syntax | Description |
| ----------- | ----------- |
|.|Any Character Except New Line|
|\d|Digit (0-9)|
|\D|Not a Digit (0-9)|
|\w|Word Character (a-z, A-Z, 0-9, _)|
|\W|Not a Word Character|
|\s|Whitespace (space, tab, newline)|
|\S|Not Whitespace|
|-|-|
|\b|Word Boundary|
|\B|Not a Word Boundary|
|^||Beginning of a String|
|$|End of a String|
|-|-|
|[]|Matches Characters in brackets|
|[^ ]|Matches Characters NOT in brackets|
|\||Either Or|

---

. 	-  Any Character Except New Line
\d	- Digit (0-9)
\D	- Not a Digit (0-9)
\w	- Word Character (a-z, A-Z, 0-9, _)
\W 	- Not a Word Character
\s	- Whitespace (space, tab, newline)
\S	- Not Whitespace

\b	-Word Boundary
\B	- Not a Word Boundary
^	- Beginning of a String
$	- End of a String

[]	- Matches Characters in brackets
[^ ]	- Matches Characters NOT in brackets
|	- Either Or
