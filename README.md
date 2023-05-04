Download Link: https://assignmentchef.com/product/solved-csc60-lab-10-write-your-own-unix-shell-part-2
<br>
<strong><u>UNIX Shell</u></strong>

In Lab9 we did the 3 built-in commands:  cd, pwd, exit.

Now we need to implement: a fork, an exec, and code to handle redirection.

<strong><u> FILES TO COPY- </u></strong><strong><u> </u></strong><strong><u> </u></strong><strong><u>Different</u></strong> <strong><u> </u></strong><strong><u>  Instructions this time</u></strong> :

To get the file you need, first move to your class folder by typing:  <strong>cd csc60 </strong>Type: <strong>cp  -R  /gaia/home/faculty/bielr/files_csc60/lab10   .</strong>

Spaces needed:  (1) After the <strong>cp                                                ↑ </strong>Don’t miss the space &amp; dot.

<ul>

 <li>After the <strong>-R</strong></li>

 <li>After the directory name at the end &amp; before the dot.You have now created a lab10 directory and copied in three sample files:</li>

</ul>

execvp.c, redir.c, waitpid.c

Stay in directory <strong>csc60</strong>, and you need to:

type:  <strong>chmod 755 lab10 </strong>type:  <strong>cp  lab9/lab9.c  lab10/lab10.c</strong>

We have copied lab9 code and renamed it to lab10.c for you to start work on it.

Next move to <strong>lab10</strong> directory by typing: <strong>cd lab10</strong>                and type:  <strong>chmod  644  * </strong>This will set permissions on all the files.

Your new lab10 directory should now contain:  lab10.c, waitpid.c, redir.c, and execvp.c The files: waitpid.c, redir.c, and execvp.c, contain code examples that may help you in completing Lab10.

A lot of code to be used in Lab10 is currently commented out.

Use the file <strong>lab9-10 RemoveCommentsGuide.docx </strong>(on Canvas) to guide you to properly remove the extra comments…without removing Every Comment!

<strong>Pseudo Code</strong>  (Yellow highlight indicates the code from Lab9.)

/*———————————————————-*/ int main (void)

{

while (TRUE)

{

int childPid; char *cmdLine;

print the prompt();      /* <em>i.e. <strong>csc60mshell </strong></em><strong>&gt;</strong> , Use printf*/

<strong> more on next page</strong>

fgets(cmdline, MAXLINE, stdin);

/* You have to write the call. The function itself is <em>provided: </em>function parseline */ Call the function <strong>parseline</strong>, sending in <strong>cmdline</strong> &amp; <strong>argv</strong>, getting back <strong>argc</strong>

/* code to print out the argc and the agrv list to make sure it all came in. Required.*/

Print a line. Ex: “Argc = %i” loop starting at zero, thru less than agrc, increment by one.       print each argv[loop counter] [(“Argv %i = %s 
”, i, argv[i]);]

/* Start processing the built-in commands */ if ( argc compare equal to zero)

/* a command was not entered, might have been just Enter or a space&amp;Enter */        <em>continue</em> to end of while(TRUE)-loop

// next deal with the built-in commands

//   Use <em>strcmp</em> to do the test

//   after <strong>each</strong> command, do a <em>continue</em> to end of while(TRUE)-loop if (“exit”)       issue an exit call else if (“pwd”)

declare a char variable array of size MAX_PATH_LENGTH to hold the path       do a getcwd       print the path else if (“cd”)

declare a char variable <em>dir </em>as a pointer (with an *)       if the argc is 1

use the getenv call with “HOME” and return the value from the call to variable <em>dir</em>

<table width="390">

 <tbody>

  <tr>

   <td width="96">      else</td>

   <td width="294"> </td>

  </tr>

  <tr>

   <td colspan="2" width="390">variable <em>dir</em> gets assigned the value of argv[1]</td>

  </tr>

 </tbody>

</table>

execute a call to chdir(dir) with error checking. Message = “error changing directory”

<strong>void process_input (int argc, char **argv)                            </strong>// <u>Child Process</u>

{

call <strong>handle_redir</strong> passing it argc and argv

call <strong>execvp </strong>passing in argv[0] and argv and return a value to an <em>integer</em> variable

(Example:  ret)

if (ret == -1) error check and do  _exit(EXIT_FAILURE)

}

<strong>void handle_redir(int argc, char *agrv[])                              </strong>// <u>Child Process</u>

<strong>{  </strong>

You need two integer variables to keep track of the location in the string of the redirection symbols, (one for <strong>out_loc</strong> (&gt;), one for <strong>in_loc</strong> (&lt;) ). Initialize them to zero.

<strong>for</strong> loop from 0 to &lt; argc

if ( “&gt;” == 0)                 // use strcmp function

if out_loc not equal 0

Cannot output to more than one file.  fprint error. _exit failure.

else if loop_counter compares equal 0

No command entered. print error. _exit failure.

set out_loc to the current loop_counter.

<strong> more pseudo code on next page</strong>

else if (“&lt;” == 0)          // use strcmp function if (in_loc not equal 0)

Cannot input from more than one file.  print error. _exit failure.

else if loop_counter compares equal 0

No command entered. print error. _exit failure.

set in_loc to the current loop_counter.

// end of the <strong>if </strong>// end of the <strong>for</strong> loop

if(out_loc != 0)  if argv (indexed by out_loc +1) contains a NULL

There is no file, so print an error, and _exit in failure.

<strong>Open</strong> the file using name from argv, indexed by out_loc+1,                           and assign returned value to fd.  [See 9-Unix, slides 6-10]                 use <strong>flags</strong>:  to read/write; to create file if needed;             to truncate existing file to zero length

use <strong>permission</strong> bits for:  user-read; user-write

Error check the open. perror &amp;  _exit

Call <strong>dup2</strong> to switch standard-out to the value of the file descriptor.

<strong>Close </strong>the file

Set things up for the future exec call by setting argv[out_loc] to NULL        // end of if(out_loc != 0)

if(in_loc != 0) if argv (indexed by in_loc +1) contains a NULL

There is no file, so print an error, and _exit in failure.

<strong>Open</strong> the file using name from argv, indexed by in_loc+1                  and assign returned value to fd.  use flags; for read only

Error check the open. perror &amp; _exit

Call <strong>dup2</strong> to switch standard-in to the value of the file descriptor.

<strong>Close</strong> the file

Set things up for the future exec call by setting argv[in_loc] to NULL

//end of if(in_loc != 0)

Word of warning:  In the past many students have duplicated the code for OpenOutputFile to be used for OpenInputFile.  If you do that, lots of little things need to be changed.  Be careful.

 <strong>more on next page</strong>




<strong>Resources</strong>

<strong>Useful Unix System Calls:    </strong>See PowerPoint Slides file named<strong> Lab10 Slides C Library functions:</strong>

<table width="623">

 <tbody>

  <tr>

   <td width="623">#include &lt;string.h&gt; <em>String compare:</em>int strcmp(const char *s1, const char *s2); //Function prototype from string.h <strong>if(strcmp(argv[0], “exit”) == 0)         //Sample. One line completed. </strong>     strcmp(argv[0],”pwd”)      strcmp(argv[0],”cd”)      strcmp(….,”&gt;”)      strcmp(….,”&lt;“)<em>print a system error message:</em>perror(“Shell Program error 
”);</td>

  </tr>

 </tbody>

</table>

<strong>Compilation &amp; Building your program</strong>

The use of <em>gcc</em> is just fine.  If you want to have the executable with a different name than a.out, type: gcc –o  name-of-executable  name-of-source-code

<strong>or </strong>gcc  name-of-source-code  –o  name-of-executable

<strong>Partnership</strong>

Students may form a group of 2 students (maximum) to work on this lab.  As usual, please always contact your instructor for questions or clarification.  Your partner does not have to attend the same section.

All code files should include both names.

Using <strong>vim</strong>, create a small name file with both of your names in it. When you start your script file, <em>cat</em> that name file so both names show up in the script file.

<strong>You must BOTH submit your effort</strong>.  As both of your names occur on everything, when I or another grader find the first submission, we will then give the same grade to the second student.

<strong>Marks Distribution</strong>

Lab 10 is worth 76 points.

<strong> more on next page</strong>

<strong>Hints</strong>

<em>Our compiler does not like:</em>  <strong>for (int i = 0; …..)</strong>

You will receive the following errors:

test_loopcounter.c:6: error: ‘for’ loop initial declarations are only allowed in C99 mode test_loopcounter.c:6: note: use option -std=c99 or -std=gnu99 to compile your code These errors imply that on every “gcc” line, you must add:  -std=c99   OR  -std=gnu99.

<em>It does like it on two lines:</em>

int i;

for (i = 0; ……)




Keep versions of your code. This is in case you need to go back to your older version due to an unforeseen bug/issue.

A lot of code to be used in Lab10 was commented out.

Use the file <strong>lab9-10 RemoveCommentsGuide.docx </strong>to guide you to remove a set of the extra comments…without deleting Every Comment.

<strong>Deliverables</strong>

Submit <strong>two</strong> files to Canvas<strong>:</strong>

<ol>

 <li>c</li>

 <li>txt

  <ul>

   <li>Your program’s output test (with various test cases).</li>

   <li>Please use the UNIX <strong>script </strong>command to capture your program’s output.</li>

   <li>Details below. (Do not include lab10.c in this file)</li>

  </ul></li>

</ol>

 More on next page.

<strong>Preparing your script file:    </strong>

Be located in <strong>csc60/lab10</strong> directory.

When all is well and correct, type:  <strong>script StudentName_lab10.txt</strong>

At the prompt, type:   <strong>gcc lab10.c -Wall        </strong>to compile the code                             type:  <strong>a.out     </strong>to run the program Enter in sequence:

<ol>

 <li><em>If you are on a team, </em>cat<em> your name file here. </em></li>

</ol>

<table width="525">

 <tbody>

  <tr>

   <td width="144">2.   ls &gt; lsout</td>

   <td colspan="2" width="381">// should work with output going to file</td>

  </tr>

  <tr>

   <td width="144">3.   cat lsout</td>

   <td colspan="2" width="381">// display the contents of the output file</td>

  </tr>

  <tr>

   <td width="144">4.   ls &gt; lsout &gt; file1</td>

   <td colspan="2" width="381">// should produce an error</td>

  </tr>

  <tr>

   <td width="144">5.   cat foo.txt</td>

   <td colspan="2" width="381">// should produce an error</td>

  </tr>

  <tr>

   <td width="144">6.   &gt; lsout</td>

   <td colspan="2" width="381">// should produce an error</td>

  </tr>

  <tr>

   <td width="144">7.  &lt; lsout</td>

   <td colspan="2" width="381">// should produce an error/* <strong><em>wc</em></strong><em> prints newline, word, and byte counts for each file</em> */</td>

  </tr>

  <tr>

   <td width="144">8.   wc &lt; lsout</td>

   <td colspan="2" width="381">// output will go to the screen.</td>

  </tr>

  <tr>

   <td colspan="3" width="525">9.          wc &lt; lsout &gt; wcout          // output will go to a file10.      cat wcout // display the output11.      wc &lt; lsout &lt; wcout          // should produce an error</td>

  </tr>

  <tr>

   <td width="144">12.   cd ../lab1</td>

   <td width="182">// move to lab1 directory</td>

   <td width="199"> </td>

  </tr>

  <tr>

   <td width="144">13.   gcc lab1.c</td>

   <td width="182">// show that the exec works</td>

   <td width="199"> </td>

  </tr>

  <tr>

   <td width="144">14.   a.out</td>

   <td width="182">// show output of lab1</td>

   <td width="199"> </td>

  </tr>

  <tr>

   <td width="144">15.   exit</td>

   <td width="182">// (exit from the shell)</td>

   <td width="199"> </td>

  </tr>

  <tr>

   <td width="144">16.   exit</td>

   <td width="182">// (exit from the script)</td>

   <td width="199"> </td>

  </tr>

 </tbody>

</table>

When finished, submit your two files (the C file and the Script file) to Canvas.

(The script file will NOT contain the contents of lab10.c).

No zip files please.