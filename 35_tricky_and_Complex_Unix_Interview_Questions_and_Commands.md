https://www.facebook.com/notes/unixclouddevopssre-help/35-tricky-and-complex-unix-interview-questions-and-commands/1437229406496913

35 Tricky and Complex Unix Interview Questions and Commands
De Khan Ather la duminică, 1 decembrie 2013 la 01:27
35 Tricky and Complex Unix Interview Questions and Commands 

 

Here is the list of 35 complex and tricky unix interview questions and answers. A lot of complex unix commands which are asked in unix interviews are SED, AWK, DU, HEAD, TAIL, WATCH, GREP, CUT, PS, ZIP, UNZIP etc. A lot of tips and tricks are asked about these unix commands during interview. Following questions and unix commands might help you in your unix interview.

 

1. How do you find which processes are using a particular file?

 

By using lsof command in UNIX. It will list down PID of all the processes which are using a particular file.

 

2. How do you find which remote hosts are connecting to your host on a particular port say 10123?

 

By using netstat command

For example: execute netstat -a | grep "port" and it will list the entire hosts which are connected to this host on port 10123.

 
3. How to tell if my process is running in Unix?

 

You can list down all the running processes using [ps] command. Then you can “grep” your user name or process name to see if the process is running.

 

4. What is ephemeral port in UNIX?

 

Ephemeral ports are port used by Operating system for client sockets. There is a specific range on which OS can open any port specified by ephemeral port range.

 

5. How to list down file/folder lists alphabetically?

 

Normally [ls –lt] command lists down file/folder list sorted by modified time. If you want to list then alphabetically, then you should simply specify: [ls –l]

 

6. If one process is inserting data into your MySQL database? How will you check how many rows inserted into every second?

 

By using "watch" command in UNIX

 

7. There is a file Unix_Test.txt which contains words "Unix". How will you replace all Unix to UNIX?

 

By using SED command in UNIX

For example: you can execute sed s/Unix/UNIX/g fileName.

 
8. You have a tab separated file which contains Name, Address and Phone Number. List down all Phone Number without their name and addresses?

 

By using either AWK or CUT command.

 

9. How to check if the last command was successful in Unix?

 

To check the status of last executed command in UNIX, you can check the value of an inbuilt bash variable [$?]. See the below example:

 

$> echo $?

 

10. How to check all the running processes in Unix?

 

The standard command to see this is [ps]. But [ps] only shows you the snapshot of the processes at that instance. If you need to monitor the processes for a certain period of time and need to refresh the results in each interval, consider using the [top] command.

 

$> ps –ef

If you wish to see the % of memory usage and CPU usage, then consider the below switches:

$> ps aux

If you wish to use this command inside some shell script, or if you want to customize the output of [ps] command, you may use “-o” switch like below. By using “-o” switch, you can specify the columns that you want [ps] to print out.

$>ps -e -o stime,user,pid,args,%mem,%cpu

 
11 Your application home directory is full? How will you find which directory is taking how much space?

 

By using disk usage (DU) command in Unix

For example du –sh . | grep G  will list down all the directories which have GIGS in Size.

 
12. How do you find for how many days your Server is up?

 

By using uptime command in UNIX

 

13. How to check if a file is present in a particular directory in Unix?

 

Using command, we can do it in many ways. Based on what we have learnt so far, we can make use of [ls] and [$?] command to do this. See below:

 

$> ls –l file.txt; echo $?

If the file exists, the [ls] command will be successful. Hence [echo $?] will print 0. If the file does not exist, then [ls] command will fail and hence [echo $?] will print 1.

 
14. You have an IP address in your network. How will you find hostname and vice versa?

 

By using nslookup command in UNIX

 

15. How to execute a database stored procedure from Shell script?

 

$> SqlReturnMsg=`sqlplus -s username/password@database echo $SqlReturnMsg

 

16. How to check the command line arguments in a UNIX command in Shell Script?

 

In a bash shell, you can access the command line arguments using $0, $1, $2, … variables, where $0 prints the command name, $1 prints the first input parameter of the command, $2 the second input parameter of the command and so on.

 

17. How to fail a shell script programmatically?

 

Just put an [exit] command in the shell script with return value other than 0. This is because the exit code of successful Unix program is zero. So, suppose if you write exit -1 inside your program, then your program will throw an error and exit immediately.

 

18. How to print/display the first line of a file?

 

There are many ways to do this. However the easiest way to display the first line of a file is using the [head] command.

 

$> head -1 file.txt

If you specify [head -2] then it would print first 2 records of the file.

 
Another way can be by using [sed] command. [Sed] is a very powerful text editor which can be used for various text manipulation purposes like this.

 

$> sed '2,$ d' file.txt

 

How does the above command work? The 'd' parameter basically tells [sed] to delete all the records from display from line 2 to last line of the file (last line is represented by $ symbol). Of course it does not actually delete those lines from the file, it just does not display those lines in standard output screen. So you only see the remaining line which is the 1st line.]

 

19. How to print/display the last line of a file?

 

The easiest way is to use the [tail] command.

 

$> tail -1 file.txt

If you want to do it using [sed] command, here is what you should write:

 
$> sed -n '$ p' test

From our previous answer, we already know that '$' stands for the last line of the file. So '$ p' basically prints (p for print) the last line in standard output screen. '-n' switch takes [sed] to silent mode so that [sed] does not print anything else in the output.

 
20. How to display n-th line of a file?

 

The easiest way to do it will be by using [sed]. Based on what we already know about [sed] from our previous examples, we can quickly deduce this command:

 

$> sed –n ' p' file.txt

You need to replace with the actual line number. So if you want to print the 4th line, the command will be

 
$> sed –n '4 p' test

Of course you can do it by using [head] and [tail] command as well like below:

 
$> head - file.txt | tail -1

You need to replace with the actual line number. So if you want to print the 4th line, the command will be

 
$> head -4 file.txt | tail -1

 

21. How to remove the first line / header from a file?

 

We already know how [sed] can be used to delete a certain line from the output – by using the'd' switch. So if we want to delete the first line the command should be:

 

$> sed '1 d' file.txt

But the issue with the above command is, it just prints out all the lines except the first line of the file on the standard output. It does not really change the file in-place. So if you want to delete the first line from the file itself, you have two options.

 
Either you can redirect the output of the file to some other file and then rename it back to original file like below:

 

$> sed '1 d' file.txt > new_file.txt$> mv new_file.txt file.txt

Or, you can use an inbuilt [sed] switch '–i' which changes the file in-place. See below:

 
$> sed –i '1 d' file.txt

 

22. How to remove the last line/ trailer from a file in Unix script?

 

Always remember that [sed] switch '$' refers to the last line. So using this knowledge we can deduce the below command:

 

$> sed –i '$ d' file.txt

 

23. How to remove certain lines from a file in Unix?

 

If you want to remove line to line from a given file, you can accomplish the task in the similar method shown above. Here is an example:

 

$> sed –i '5,7 d' file.txt

The above command will delete line 5 to line 7 from the file file.txt

 
24. How to remove the last n-th line from a file?

 

This is bit tricky. Suppose your file contains 100 lines and you want to remove the last 5 lines. Now if you know how many lines are there in the file, then you can simply use the above shown method and can remove all the lines from 96 to 100 like below:

 

$> sed –i '96,100 d' file.txt   # alternative to command [head -95 file.txt]

 

But not always you will know the number of lines present in the file (the file may be generated dynamically, etc.) In that case there are many different ways to solve the problem. There are some ways which are quite complex and fancy. But let's first do it in a way that we can understand easily and remember easily. Here is how it goes:

 

$> tt=`wc -l file.txt | cut -f1 -d' '`;sed –i "`expr $tt - 4`,$tt d" test

 

As you can see there are two commands. The first one (before the semi-colon) calculates the total number of lines present in the file and stores it in a variable called “tt”. The second command (after the semi-colon), uses the variable and works in the exact way as shown in the previous example.

 

25. How to check the length of any line in a file?

 

We already know how to print one line from a file which is this:

 

$> sed –n ' p' file.txt

Where is to be replaced by the actual line number that you want to print. Now once you know it, it is easy to print out the length of this line by using [wc] command with '-c' switch.

$> sed –n '35 p' file.txt | wc –c

The above command will print the length of 35th line in the file.txt.

 
26. How to get the nth word of a line in Unix?

 

Assuming the words in the line are separated by space, we can use the [cut] command. [cut] is a very powerful and useful command and it's real easy. All you have to do to get the n-th word from the line is issue the following command:

 

cut –f -d' '

'-d' switch tells [cut] about what is the delimiter (or separator) in the file, which is space ' ' in this case. If the separator was comma, we could have written -d',' then. So, suppose I want find the 4th word from the below string: “A quick brown fox jumped over the lazy cat”, we will do something like this:

$> echo “A quick brown fox jumped over the lazy cat” | cut –f4 –d' '

And it will print “fox”

 
27. How to reverse a string in unix?

 

Pretty easy. Use the [rev] command.

 

$> echo "unix" | rev

xinu

 
28. How to get the last word from a line in Unix file?

 

We will make use of two commands that we learnt above to solve this. The commands are [rev] and [cut]. Here we go.

 

Let's imagine the line is: “C for Cat”. We need “Cat”. First we reverse the line. We get “taC rof C”. Then we cut the first word, we get 'taC'. And then we reverse it again.

 

$>echo "C for Cat" | rev | cut -f1 -d' ' | rev

Cat

 
29. How to get the n-th field from a Unix command output?

 

We know we can do it by [cut]. Like below command extracts the first field from the output of [wc –c] command

 

$>wc -c file.txt | cut -d' ' -f1

 

But I want to introduce one more command to do this here. That is by using [awk] command. [awk] is a very powerful command for text pattern scanning and processing. Here we will see how may we use of [awk] to extract the first field (or first column) from the output of another command. Like above suppose I want to print the first column of the [wc –c] output. Here is how it goes like this:

 

$>wc -c file.txt | awk ' ''{print $1}'

 

The basic syntax of [awk] is like this:

 

awk 'pattern space''{action space}'

The pattern space can be left blank or omitted, like below:

$>wc -c file.txt | awk '{print $1}'

 
In the action space, we have asked [awk] to take the action of printing the first column ($1).

 

30. How to replace the n-th line in a file with a new line in Unix?

This can be done in two steps. The first step is to remove the n-th line. And the second step is to insert a new line in n-th line position. Here we go.

 
Step 1: remove the n-th line

 

$>sed -i'' '10 d' file.txt       # d stands for delete

Step 2: insert a new line at n-th line position

 
$>sed -i'' '10 i This is the new line' file.txt     # i stands for insert

 

31. How to show the non-printable characters in a file?

 

Open the file in VI editor. Go to VI command mode by pressing [Escape] and then [:]. Then type [set list]. This will show you all the non-printable characters, e.g. Ctrl-M characters (^M) etc., in the file.

 

32. How to zip a file in Linux?

 

Use inbuilt [zip] command in Linux

 

33. How to unzip a file in Linux?

 

Use inbuilt [unzip] command in Linux.

 

$> unzip –j file.zip

 

34. How to test if a zip file is corrupted in Linux?

 

Use “-t” switch with the inbuilt [unzip] command

 

$> unzip –t file.zip

 

35. How to check if a file is zipped in Unix?

 

In order to know the file type of a particular file use the [file] command like below:

$> file file.txtfile.txt: ASCII text

If you want to know the technical MIME type of the file, use “-i” switch.$>file -i file.txtfile.txt: text/plain; charset=us-ascii

If the file is zipped, following will be the result$> file –i file.zipfile.zip: application/x-zip
