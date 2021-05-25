
# Sed

Sed stands for _Stream Editor_

Functions of sed command on a file
* viewing file content
* searching
* find and replace
* insertion or deletion

__ __
 
## Replace
 
### General syntax
`sed 's/string1/string2/g' filename`
*  __s__ stantds for - _substitute_
*  __g__ - stands for - _global_
 
E.g.:`sed 's/unix/linux/g' sed.txt`
 
### Match exaclty a word
`sed 's/\bstring1\b/string2/g' filename`
* __\b__ stands for - _boundary_
 
E.g.: `sed 's/\bopen\b/closed/g' sed.txt`
 
### Change a word if it is only at the beginning of a line
`sed 's/^string1/string2/g' filename`
 
E.g.: `sed 's/^unix/linux/g' sed.txt`
 
### Change a word if it is only at the end of a line
`sed 's/string1/string2$/g' filename`

E.g.:`sed 's/os.$/system./g' sed.txt`

## Delete

### Delete any line that contains part of or the whole string
`sed '/string/d' filename`

E.g.: `sed '/unix/d' sed.txt`

## Make changes permanent
`sed  -i 's/\bstring1\b/string2/g' filename`

E.g.: `sed -i 's/unix/linux/g' sed.txt`
