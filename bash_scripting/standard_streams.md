# Standard streams

Standard streams are interconnected input and output communication channels between a computer program and its environment when it begins execution. 
The three input/output (I/O) connections are called standard input (stdin), standard output (stdout) and standard error (stderr).

Data streams, like water streams, have two ends. 
They have a source and an outflow. \
Whichever Linux command you’re using provides one end of each stream. The other end is determined by the shell that launched the command. T
hat end will be connected to the terminal window, connected to a pipe, or redirected to a file or other command, according to the command line that launched the command.

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

