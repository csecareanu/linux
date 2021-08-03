# Standard streams

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

## Redirecting stdout and stderr
There’s an advantage to having error messages delivered by a dedicated stream. \
It means we can redirect a command’s output (stdout) to a file and still see any error messages (stderr) in the terminal window.
Create a scripts named _error.sh_:
```
#!/bin/bash

echo "About to try to access a file that doesn't exist"
cat bad-filename.txt
```

Make the script executable with this command: `chmod +x error.sh`

The first line of the script echoes text to the terminal window, via the stdout stream. The second line tries to access a file that doesn’t exist. This will generate an error message that is delivered via stderr.

Run the script with this command: `./error.sh`\
The console will display:
```
About to try to access a file that doesn't exist
cat: bad-filename.txt: No such file or directory
```
We can see that both streams of output, _stdout_ and _stderr_, have been displayed in the terminal windows.

Let’s try to redirect the output to a file: `./error.sh > capture.txt`
The console will display:
```
cat: bad-filename.txt: No such file or directory
```
The error message that is delivered via _stderr_ is still sent to the terminal window. 
We can check the contents of the file to see whether the _stdout_ output went to the file.
```
cat capture.txt
```
The output from stdin was redirected to the file as expected:
```
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

Let’s try to our test again, and this time we’ll use 2>:
