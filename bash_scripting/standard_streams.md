
# Standard streams

## Scurta introducere
Iesirea unui comenzi rulate in "command line poate" fi:
* afisata pe ecran
  * ex: `ls` - afiseaza pe ecran
* "redirected"
  * ex: `ls > fisiere.txt` - scrie in fisierul _fisiere.txt_ si nu afiseaza pe ecran
* piped
  * ex: `ls | grep ....` - nu afiseaza pe ecran, ci trimipte prin "pipe" catre comanda _grep_. Comanda _grep_, la randul ei, poate sa afiseze rezultatul pe ecran, sa il scrie in fisier, sau sa il trimita prin "pipe" catre alta comanda.
    * `ls | grep "..."` - comanda grep primeste prin pipe de la comanda ls si afiseaza pe ecran
    * `ls | grep "..." > rezultate.txt" - comanda grep primeste prin pipe de la comanda ls si  scrie in fisier
    * `ls | grep "..." | sed ...` - comanda grep primeste prin pipe de la comanda ls si trimite prin "pipe" catre comanda `sed`. Comanda send are, la randul ei, cele 3 optiuni de afisare.
  

Fiecare fisier in Linux are un **File Descriptor** asociat cu el.\
Un File Descriptor este un numar.


Orice program rulat in linia de comanda are 3 "file descriptor-i" (asa ii primeste de la kernerl-ul de linux cand programul este rulat".
* File descriptoru-ul cu valoarea 0 care este Standard Input Device.
 * Tastatura este Standard Input Device implicit.
* File descriptoru-ul cu valoarea 1 care este Standard Output Device.
 * Ecranul este Standard Output Device implicit.
* File descriptoru-ul cu valoarea 2 care este Standard Error Device.
 * Ecranul este Standard Error Device implicit.

Programul scrie erorile in Standard Error (adica in file descriptor-ul cu nr. 3), informatiile in Standard Output (adica in file descriptor-ul cu nr. 2) si citeste valori de intrare din Standard Input (adica file descriptor-ul cu nr. 0).

Daca kernel-ul i-a dat pentru Stdandard Erorr si Standard Output ecranul, atunci, cand programul va scrie in File Descriptor-ii 2 si 3, va scrie pe eran.\
Daca kernel-ul i-a dat pentru Stdandard Erorr un fisier si pentru si Standard Output un File Descriptor, atunci va scrie erorile in fisier si informatia pe ecran.\
Daca kernel-ul i-a dat pentru Stdandard Erorr si Standard Output fisisere, atunci programul nu va mai afisa nimic pe ecran cand va scrie in File Descriptorii 2 si 3, ci se va duce totul in fisiere.

Operatori:
* ">" este operatorul de redirectare a al iesirii
* ">>" adauga date la un fisier deja existent
* "<" este operatorul de redirectare al intrarii
* ">&" Redirecteaza iesirea unui fisier in altul

## Summary
`command > output.txt`: The standard output stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, it gets overwritten.
 
`command >> output.txt`: The standard output stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

`command 2> output.txt`: The standard error stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, it gets overwritten.

`command 2>> output.txt`: The standard error stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

`command &> output.txt` or `command > output.txt 2>&1`: Both the standard output and standard error stream will be redirected to the file only, nothing will be visible in the terminal. If the file already exists, it gets overwritten.

`command &>> output.txt` or `command >> output.txt 2>&1`: Both the standard output and standard error stream will be redirected to the file only, nothing will be visible in the terminal. If the file already exists, the new data will get appended to the end of the file..

`command | tee output.txt`: The standard output stream will be copied to the file, it will still be visible in the terminal. If the file already exists, it gets overwritten.

`command | tee -a output.txt`: The standard output stream will be copied to the file, it will still be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

`command |& tee output.txt`: Both the standard output and standard error streams will be copied to the file while still being visible in the terminal. If the file already exists, it gets overwritten.

`command |& tee -a output.txt`: Both the standard output and standard error streams will be copied to the file while still being visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

## Standard streams
Standard streams are interconnected input and output communication channels between a computer program and its environment when it begins execution. 
The three input/output (I/O) connections are called standard input (stdin), standard output (stdout) and standard error (stderr).

Data streams, like water streams, have two ends. 
They have a source and an outflow. \
Whichever Linux command you’re using provides one end of each stream. The other end is determined by the shell that launched the command. That end will be connected to the terminal window, connected to a pipe, or redirected to a file or other command, according to the command line that launched the command.

In Linux, **stdin** is the standard input stream. This accepts text as its input. \
Text output from the command to the shell is delivered via the **stdout** (standard out) stream. \
Error messages from the command are sent through the **stderr** (standard error) stream.\
_stdin_, _stdout_, and _stderr_ are three data streams created when you launch a Linux command.

**Streams Are Handled Like Files**

Streams in Linux—like almost everything else—are treated as though they were files.

These values are always used for _stdin_, _stdout_, and _stderr_:
* 0: stdin
* 1: stdout
* 2: stderr

## Redirect standard output and standard error (Redirecting stdout and stderr)
There’s an advantage to having error messages delivered by a dedicated stream. \
It means we can redirect a command’s output (stdout) to a file and still see any error messages (stderr) in the terminal window.
Create a scripts named _error.sh_:
```
#!/bin/bash

echo "About to try to access a file that doesn't exist"
cat bad-filename.txt
```

Make the script executable with this command: `chmod +x error.sh`

The first line of the script echoes text to the terminal window, via the _stdout_ stream. The second line tries to access a file that doesn’t exist. This will generate an error message that is delivered via _stderr_.

Run the script with this command: `./error.sh`\
The console will display:
```
$ ./error.sh
About to try to access a file that doesn't exist
cat: bad-filename.txt: No such file or directory
```
We can see that both streams of output, _stdout_ and _stderr_, have been displayed in the terminal windows.

Let’s try to redirect the output to a file: `./error.sh > capture.txt`
The console will display:
```
$ ./error.sh > capture.txt
cat: bad-filename.txt: No such file or directory
```
The error message that is delivered via _stderr_ is still sent to the terminal window. 
We can check the content of the file to see whether the _stdout_ output went to the file.
```
cat capture.txt
```
The output from _stdout_ was redirected to the file as expected:
```
$ cat capture.txt
About to try to access a file that doesn't exist
```

The **>** redirection symbol works with _stdout_ by default. You can use one of the numeric file descriptors to indicate which standard output stream you wish to redirect.

To explicitly redirect _stdout_, use this redirection instruction:
```
1>
```
To explicitly redirect  stderr, use this redirection instruction:
```
2>
```

Let’s try to our test again, and this time we’ll use `2>`:
```
./error.sh 2> capture.txt
```
The console will display:
```
$ ./error.sh 2> capture.txt
About to try to access a file that doesn't exist
```
The error message is redirected and the _stdout_ `echo` message is sent to the terminal window:
```
About to try to access a file that doesn't exist
```

Let’s see what is in the `capture.txt` file.
```
$ cat capture.txt
cat: bad-filename.txt: No such file or directory
```

## Redirect stdout and stderr (Redirecting Both stdout and stderr)
This command will direct _stdout_ to a file called capture.txt and _stderr_ to a file called error.txt.
```
./error.sh 1> capture.txt 2> error.txt
```
Because both streams of output–standard output and standard error—are redirected to files, there is no visible output in the terminal window. We are returned to the command line prompt as though nothing has occurred.

Let’s check the contents of each file:
```
$ cat capture.txt
About to try to access a file that doesn't exist
```
```
$ cat error.txt
cat: bad-filename.txt: No such file or directory
```

## Redirect stdout and stderr to the same file (Redirecting stdout and stderr to the Same File)
We’ve got each of the standard output streams going to its own dedicated file. The only other combination we can do is to send both _stdout_ and _stderr_ to the same file.

To write the output from one file to the input of another file: `>&`.

To redirect error output (_stderr_) to standard output (_stdout_) which in turn is redirected to file _capture.txt_:
```
./error.sh > capture.txt 2>&1
```
Both output _stderr_ and _stdout_ are redirected to _capture.txt_:
```
$ cat capture.txt
About to try to access a file that doesn't exist
cat: bad-filename.txt: No such file or directory
```

## Redirecting output to /dev/null
In certain situations, the output may not be useful at all. Using redirection, we can dump all the output into the void.

Find does not have access to a lot of directories and files and we can suppress the errors:
```
find / -name ".bash_history" 2> /dev/null
```
