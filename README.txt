------------------------------------------------
Name        :   Megha Agarwal
Roll No     :   201506511
Branch      :   CSIS
************************************************

************SHELL IMPLEMENTATION****************

Functionalities Implemented:

1. Execute all the commands (ls, clear, vi etc)

________________________
Logic(FLOW OF THE CODE)
________________________
 
**** Implement Multiping functionality, by duping at the first, middle and last commands of the pipe command given.
	eg. ls -l  | wc -l | wc
Here, ls -l will be the first command,
wc -l will be the middle command,
wc will be the last command.

* If it is the first command, then dup the stdout to the input of the pipe.
* If it is the middle command, then dup the previous mypipefd[0]-read end to the stdin, and dup to the stdout to again the input of the pipe. i.e. coming from one pipe and entering into another pipe.
* If it is the last command, then just take input from the pipe and print it on the console. i.e. dup the stdin only.

_____________________________________________________________________________________________________________________________________________

**** If there is no pipe, i.e. only one command is given, then only the first command in the pipe will go to execvp(args[0] args).
i.e. No need to handle external commands separately, will run through the function.
		with_pipe_execute()  and pipe_count=0;

_____________________________________________________________________________________________________________________________________________

**** Shell Builtins are checked prior, and are not executed by execvp(). They are called through separte functions. - cd, pwd, export.

_____________________________________________________________________________________________________________________________________________

**** The Environment variables are handled with the echo call.
The program runs fine for maximum of the test cases of echo (parsing), i.e. single inverted commas inside double, double inside single comma.

The program runs fine for the following testcases(with spaces handled)
$echo "HII"
$echo "     'hiiii'  "
$echo       "hiiiiii     "
$echo    '    abcd "abcd" '
$echo "hi" "hello" 'hiiii'
$echo "$PWD"
$echo $PWD
e
The output of the function is again given to the pipe, and execvp is not run in this case, even not if only echo is called.
Only the child process so created is to be killed exlicitly as it not done by printf() statement rather done by execvp.

_____________________________________________________________________________________________________________________________________________

**** The environment variables are set properly, local and global both.
i.e.
export a=5
and
a=5
will both set the value of a to 5, globally and locally respectively.

We can print them using echo $a

_____________________________________________________________________________________________________________________________________________

**** I/O Redirection is working properly withing the pipe as well.
 The following testcases are runninh.
ls -l > new
wc -c  < new
ls -l | wc  | wc | wc -c > new
wc -c < new | wc -l

Steps -
The input buffer is tokenised to get the input / output file names if it has '<' or '>' in it. 
The stdin is duped to the inputfile and the data is readed from it.
The stdout is duped to the output file and data is written into it.
The duping is done in the with_execute_pipe() function only before the command actually executes.

_____________________________________________________________________________________________________________________________________________

**** HISTORY
The concept of History and Bang is quite a large functionality, and can be handled in many different ways.

Steps -
# Getting the current working directory and saving it in a variable for future use (if in case cd is done)
# Always a file is created and read from this directory only. i.e. history.txt file is created.
# Whenever there is an input and its not a (!) operator, the file is processed first -> line count is maintained and all the data is copied into an array of strings.
# The input buffer has to be written into the history file along with the numbers, and the line count is updated. 
# It should have a max buffer of 1000.

i.e. Even if the file can contain more than 1000 lines,
when history is called, it will only print the last 1000 lines.
eg. it can be 501 to 1500 lines.

* History 10 and this with the pipe
i.e History 10  | tail -5 
is successfully executed.


_____________________________________________________________________________________________________________________________________________

**** BANG(!) Operator

!!, !-7, !50 All these are successfull executed.
Flow - According to the input the correct command is fetched to written to the input buffer only. 
The rest procecss goes the same, let it be any command.

#Future_Enhancement - !(String) is not implemented, pattern matching can be done, and the code can be improved further.

_____________________________________________________________________________________________________________________________________________

**** Handling Interrupt Signal
The interrupt Signals are handled by defined SIGINT function.
The program doesnt terminate on pressing Ctrl + C
Only the current command is terminated.

_____________________________________________________________________________________________________________________________________________

**** #FUTURE ENHANCEMENTS

Background and Foreground Process can also be handled! 

_____________________________________________________________________________________________________________________________________________








