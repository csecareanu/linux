
Regular expressions allow us to search for specific pattern of text.


## Metacharcters

__Metacharacters__ are symbols or characters that have a special meaning within a __regular expression__.

The list of supported regular expression metacharacters: __[] * ? + - % _ | () {} \ ^ $ . :__

If want to search for a metacharacter we have to escape it.\
To escape character we can use backslash.

`grep "." reqular_exp.txt` will match the entire file because the __.__ metacharacter matches any single character.

If you want to serach for the '.' character then you have to escpe it because it is a methacharacter withing a regular expression.
`grep '\.' reqular_exp.txt`

The '\' is a special character also.\
So if you want to search specifically for a backslash then you have to escape it.

`grep '\\' reqular_exp.txt`
