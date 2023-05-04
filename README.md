Download Link: https://assignmentchef.com/product/solved-csc360-operating-systems-programming-assignment-1-p1-a-simple-shell-interpreter-ssi
<br>
<h1> 1          Introduction</h1>

7 In this assignment you will implement a simple shell interpreter (SSI), using system calls and 8 interacting with the system. The SSI will be very similar to the Linux shell bash: it will support 9 the foreground execution of programs, ability to change directories, and background execution.

10 You can implement your solution in C or C++. Your work will be tested on linux.csc.uvic.ca, 11 to which you can remote login by ssh. You can also access Linux computers in ECS labs in person

<ul>

 <li>or remotely by following <a href="https://connex.csc.uvic.ca/portal/site/itsupport/">https://connex.csc.uvic.ca/portal/site/itsupport/</a></li>

 <li>Be sure to test your code on csc.uvic.ca before submission. Many students have 14 developed their programs for their Mac OS X laptops only to find that their code works differently</li>

 <li>on csc.uvic.ca resulting in a <em>substantial </em>loss of marks.</li>

 <li>Be sure to study the man pages for the various systems calls and functions suggested in this 17 These functions are in Section 2 of the man pages, so you should type (for example):</li>

</ul>

18                        $ man 2 waitpid

<h1>19 2         Schedule</h1>

20 In order to help you finish this programming assignment on time successfully, the schedule of this 21 assignment has been synchronized with both the lectures and the tutorials. There are three tutorials 22 arranged during the course of this assignment.

<table width="677">

 <tbody>

  <tr>

   <td width="86">Date</td>

   <td width="406">Tutorial</td>

   <td width="185">Milestones</td>

  </tr>

  <tr>

   <td width="86">Jan 9/10</td>

   <td width="406">C and Linux refreshers, P1 spec go-through, design hints</td>

   <td width="185">design and code skeleton</td>

  </tr>

  <tr>

   <td width="86">Jan 16/17</td>

   <td width="406">system calls, system programming and testing</td>

   <td width="185">code almost done</td>

  </tr>

  <tr>

   <td width="86">Jan 23/24</td>

   <td width="406">final testing and last-minute help</td>

   <td width="185">final deliverable</td>

  </tr>

 </tbody>

</table>

<h1>23 3         Requirements</h1>

<h2>24 3.1            Basic Execution (5 marks)</h2>

<ul>

 <li>Your SSI shows the prompt</li>

 <li>SSI: /home/user &gt;</li>

 <li>for user input. The prompt includes the current directory name in absolute path, e.g., /home/user. 28 You can use getcwd() to obtain the current directory.</li>

</ul>

29 Using fork() and execvp(), implement the ability for the user to execute arbitrary commands 30 using your shell program. For example, if the user types:

<ul>

 <li>SSI: /home/user &gt; ls -l /usr/bin</li>

 <li>your shell should run the ls program with the parameters -l and /usr/bin—which should list the 33 contents of the /usr/bin directory on the screen.</li>

</ul>

34

35 <strong>Note: </strong>The example above uses 2 arguments. We will, however, test your SSI by invoking 36 programs that take more than 2 arguments.

37                      A well-written shell should support as many arguments as given on the command line.

<h2>38 3.2            Changing Directories (5 marks)</h2>

<ul>

 <li>Using the functions getcwd() and chdir(), add functionality so that users can:</li>

 <li>change the current working directory using the command cd</li>

 <li>Note that SSI always shows the current directory at prompt.</li>

 <li>The cd command should take exactly one argument—the name of the directory to change into.</li>

 <li>The special argument .. indicates that the current directory should “move up” by one directory.</li>

 <li>That is, if the current directory is /home/user/subdir and the user types:</li>

 <li>SSI: /home/user/subdir &gt; cd ..</li>

 <li>the current working directory will become /home/user.</li>

 <li>The special argument ∼ indicates the home directory of the current user. If cd is used without 48 any argument, it is equivalent to cd ∼, i.e., returning to the home directory, e.g., /home/user.</li>

</ul>

49

<ul>

 <li>Q: how do you know the user’s home directory location?</li>

 <li>H: from the environment variable with getenv().</li>

 <li><strong>Note: </strong>There is no such a program called cd in the system that you can run directly (as you did 53 with ls) and change the current directory of the <strong>calling </strong>program, even if you created one. You 54 have to use the system call chdir().</li>

</ul>

<h2>55 3.3             Background Execution (5 Marks)</h2>

56                   Many shells allow programs to be started in the background—that is, the program is running, but 57                      the shell continues to accept input from the user.

58 You will implement a simplified version of background execution that supports executing pro59 cesses in the background. The maximum number of background processes is not limited.

60 If the user types: bg cat foo.txt, your SSI shell will start the command cat with the argument 61 foo.txt in the background. That is, the program will execute and the SSI shell will also continue 62 to execute and give the prompt to accept more commands.

63 The command bglist will have the SSI shell display a list of all the programs, including their 64 execution arguments, currently executing in the background, e.g.,:

<ul>

 <li>123: /home/user/a1/foo 1</li>

 <li>456: /home/user/a1/foo 2</li>

 <li>Total Background jobs: 2</li>

 <li>In this case, there are 2 background jobs, both running the program foo, the first one with 69 process ID 123 and execution argument 1 and the second one with PID 456 and argument 2.</li>

 <li>Your SSI shell must indicate to the user after background jobs have terminated. Read the man</li>

 <li>page for the waitpid() system call. You are suggested to use the WNOHANG E.g.,</li>

 <li>SSI: /home/user/subdir &gt; cd</li>

 <li>456: /home/user/a1/foo 2 has terminated.</li>

 <li>SSI: /home/user &gt;</li>

 <li>Q: how do you make sure your SSI has this behavior?</li>

 <li>H: check the list of background processes every time processing a user input.</li>

</ul>

<h1>77 4         Bonus Features</h1>

78 Only a simplified shell with limited functionality is required in this assignment. However, students 79 have the option to extend their design and implementation to include more features in a regular 80 shell or a remote shell (e.g., kill/pause/resuming background processes, capturing and redirecting 81 program output, handling many remote clients at the same time, etc).

82 If you want to design and implement a bonus feature, you should contact the course instructor by 83 email for permission one week before the due date, and clearly indicate the feature in the submission 84 of your code. The credit for correctly implemented bonus features will not exceed 20% of the full 85 marks for this assignment.

<h1>86 5          Odds and Ends</h1>

<h2>87 5.1           Compilation</h2>

88 You will be provided with a Makefile that builds the sample code. It takes care of linking-in the 89 GNU readline library for you. The sample code shows you how to use readline() to get input 90 from the user, only if you choose to use readline library.

<h2>91 5.2          Submission</h2>

92 Submit a tar.gz archive named p1.tar.gz of your assignment through connex, with the Makefile 93 You can create a tar.gz archive of the current directory by typing:

<ul>

 <li>tar zcvf p1.tar.gz *</li>

 <li>Please do not submit .o files or executable files (out) files. Erase them before creating the 96 archive.</li>

</ul>

<h2>97 5.3           Helper Programs</h2>

<ul>

 <li><strong>3.1 </strong>inf.c</li>

 <li>This program takes two parameters:</li>

 <li><strong>tag: </strong>a single word which is printed repeatedly</li>

 <li><strong>interval: </strong>the interval, in seconds, between two printings of the tag</li>

 <li>The purpose of this program is to help you with debugging background processes. It acts a trivial 103 background process, whose presence can be “felt” since it prints a tag (specified by you) every few 104            seconds (as specified by you). This program takes a tag so that even when multiple instances of it 105         are executing, you can tell the difference between each instance.</li>

</ul>

106 This program considerably simplifies the programming of the part of your SSI shell. You can 107 find the running process by ps -ef and kill -9 a process by its process ID pid.

<ul>

 <li><strong>3.2 </strong>args.c</li>

 <li>This is a very trivial program which prints out a list of all arguments passed to it.</li>

 <li>This program is provided so that you can verify that your shell passes <em>all </em>arguments supplied on 111 the command line — Often, people have off-by-1 errors in their code and pass one argument less.</li>

</ul>

<h2>112 5.4           Code Quality</h2>

<ul>

 <li>We cannot specify completely the coding style that we would like to see but it includes the following:</li>

 <li>Proper decomposition of a program into subroutines — A 500 line program as a single routine 115 won’t suffice.</li>

</ul>

116                 2. Comment—judiciously, but not profusely. Comments serve to help a marker. To further 117                elaborate:

118 (a) Your favorite quote from Star Wars or Douglas Adams’ Hitch-hiker’s Guide to the Galaxy 119 does not count as comments. In fact, they simply count as anti-comments, and will result

<ul>

 <li>in a loss of marks.</li>

 <li>(b) Comment your code in English. It is the official language of this university.</li>

 <li>Proper variable names—leia is not a good variable name, it never was and never will be.</li>

 <li>Small number of global variables, if any. Most programs need a very small number of global 124 variables, if any. (If you have a global variable named temp, think again.)</li>

</ul>

125 5. <strong>The return values from all system calls listed in the assignment specification </strong>126 <strong>should be checked and all values should be dealt with appropriately.</strong>

<ul>

 <li>If you are in doubt about how to write good C code, you can easily find <a href="http://www.maultech.com/chrislott/resources/cstyle/">many C style guides on the Net</a><a href="http://www.maultech.com/chrislott/resources/cstyle/">.</a></li>

 <li>The <a href="http://www.maultech.com/chrislott/resources/cstyle/indhill-cstyle.html">Indian Hill Style Guide</a> is an excellent short style guide.</li>

</ul>