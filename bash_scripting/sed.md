
# Sed

__Sed__ stands for _Stream Editor_

Command syntax: __sed [options] 'commands' file/files__


__Functions of sed command on a file__
* viewing file content
* searching
* find and replace
* insertion or deletion

Sed also supports __regular expressions_ which allows it to perform complex pattern matching.
 
## Replace
 
### General syntax
```
sed 's/string1/string2/g' filename
```
__s__ stantds for - _substitute_\
__g__ stands for - _global_

* `sed 's/unix/linux/g' sed.txt`
 
### Match exaclty a word
```
sed 's/\bstring1\b/string2/g' filename
```
__\b__ stands for - _boundary_
 
* `sed 's/\bopen\b/closed/g' sed.txt`
 
### Change a word if it is only at the beginning of a line
```
sed 's/^string1/string2/g' filename
```
 * `sed 's/^unix/linux/g' sed.txt`
 
### Change a word if it is only at the end of a line
```
sed 's/string1$/string2/g' filename
```
* `sed 's/os.$/system./g' sed.txt`

### Change a word with a value derived from itself
`&`: refer to that portion of the pattern space which matched

* `sed 's/\blocalhost\b/& 192.168.25.50/' file`

## Delete

__d__ - delete lines from an existing file

### Delete any line that contains part of or the whole string
```
sed '/string/d' filename
```
* `sed '/unix/d' sed.txt`
* To modify the file: `sed -i '/unix/d' sed.txt`

### Delete the specified line numbers
```
sed '<line_no>d' filename
```

* `sed '2d' sed.txt`
* To modify the file: `sed -i '2d' sed.txt`
* `sed '1,3d' sed.txt`
* To modify the file: `sed -i '1,3d' sed.txt`
* 
## Make changes permanent
Use the __-i__ parameter to make a change permanent (to modify the input file)
```
sed  -i 's/\bstring1\b/string2/g' filename
```
* `sed -i 's/unix/linux/g' sed.txt`

## Insert a new line

__i__, __a__: insert a new line in an existing file (before a particular line number -i-, after a particular line number -a-)

### Insert before the specified line
```
sed '<line\_no>i <text>' <file\_name>
```
* `sed '1i S_NO Name Salary' sed_employee_info.txt`
* To modify the file: `sed -i '1i S_NO Name Salary' sed_employee_info.txt`
 
### Insert after the specified line
```
sed '<line\_no>a <text>' <file\_name>
```
* `sed '1a ----------------' sed_employee_info.txt`
* To modify the file: `sed -i '1a ----------------' sed_employee_info.txt`
 
### Insert after the last line
```
sed -i '$a  <text>'  <file\_name>
```
* `sed '$a ----------------' sed_employee_info.txt`
* To modify the file: `sed -i '$a ----------------' sed_employee_info.txt`

### Insert a new line before/after all lines which contain a specified text
* `sed -i '/Narendra/i 0 Mounika 46000' sed_employee_info.txt`
* `sed -i '/Clinton/a 5 Anil 56000' sed_employee_info.txt`

