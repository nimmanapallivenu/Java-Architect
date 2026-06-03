# Unix interview questions & answers for Java developers

## Table of Contents

Q1 How do you remove the Control-M characters from a file?
A1 Using the sed command that replaces Control-M with nothing
Note: The ^M is typed on the command line with ctrl+v and ctrl+M
Q2 How will you search for a property named “inbox” within a number of .properties files including all sub folders?
A2 Using the find and grep commands
Q3 How will you run a shell script even after you log out of the session?
A3 “nohup” to run even after logging out of the session. & to run as a background process. The output will be directed to nohup.outsed 's/^M//g' ReadW riteFile .sh > ReadW riteFileNew .sh
dos2unix ReadW riteFile .sh ReadW riteFile .sh
$find . -type file -name "*.properties" | xargs grep inbox \{\};
$find . -type f -name "*.properties" -exec grep "inbox" {} /dev/null \;

If you don’ t want the output to be directed to nohup.out.
Q4 How will identify the jar file that has a particular class or resource file?
A4 For example, to find the jar files that has the “ MyConnection.class ” class file. This is handy for identifying class loading issues in Java.
Q5 How will reuse some of the commands that you have already used?
A5 Using the “history” command.
Q6 How will find the list of log files that have userid “B1234”?
A6 Using the “grep” command with option “l”.$nohup ./myapp .sh &
$nohup ./myapp .sh > /dev/null 2>&1 &
find . -name '*.jar' -print0 | xargs -0 -I '{}' sh -c 'jar tf {} | grep MyConnection .class
$history # list the history
$history | grep mkdir # list mkdir commands
$!cd # execute last cd command
$!35 # execute command number 35 from the list

1. Use the up arrow to view the previous command and press enter to execute it.
2. Type !! and press enter from the command line to execute the last command again
3. Type !-1 and press enter from the command line to execute the last command !-2 for command before the last and so on.
4. Type history followed by enter , which prints a list of last few commands, and then !25 to execute a particular command
Q7 What do you uderstand by 2>&1 in UNIX?
A7 In UNIX you have STDIN, which is denoted by number 0, STDOUT , which is denoted by 1, and STDERR, which is denoted by 2. So, the above line
means, the error messages go to STDERR, which is redirected to STDOUT . So, the error messages go to where ever the STDOUT goes to. For example, The
following command creates a multiple directories for a maven based Java project. if the the directory creation is successful, change directory to “project” and
print the directory tree structure. The STDOUT and STDERR are directed to the file named maven-project.log under the project folder .
The output will be something likegrep -l "B1234" *.log
mkdir -p project /{src/{main /{java,resources },test/{java,resources }}} && cd project ; find . -type d -print > maven -project .log
/src
/src/main
/src/main /java
/src/main /resources
/src/test
/src/test/java
/src/test/resources

Q8 What is /dev/null?
A8 It is a blackhole.
will fail silently and nothing will be printed if there is no “temp” folder . The message has gone into the blackhole. If there is a “temp” folder , the present
working directory (i.e. pwd) will be printed out.
Q8 How would you go about the following scenario — you had to move to another directory temporarily to look at a file, and then move back to the directory
where you were?
A8 One way is to when you are already in /tmp folder
The better way is to use the “ pushd ” and “ popd ” commands. These commands make use of a “stack” data structure using the “Last In First Out” approach.
when you are already in /tmp folder
changes to the /projects/JMeter folder and prints the stack
The /projects/JMeter will be popped out of the stack and the directory will change back to /tmp.$ cd temp > /dev/null 2>&1 && echo $(pwd)
$ cd ../projects /JMeter
$ cd ../../tmp
$ pushd ../projects /JMeter

If you want pushd to not print the stack, you could direct the output to the black hole /dev/null as shown below .
The above is a trivial example, but in real life, you may want to navigate between more number of directories and this stack based approach will come in very
handy without having to use the “cd” command. Also, very useful in Shell scripts. Use it astutely without having to build up a gigantic directory stack full of
useless directories.
Q9 In Unix, only nine command line ar guments can be accessed using positional parameters. How would you go about having access to more than 9
argumnets?
A9 Using the “shift” command. For example, in unix, when you run a command like
The ${0} is test-bash.sh, and ${1} is file1, ${2} file2 and so on till ${9}, which is file9. In the program, if you want to access file10 after processing file1 to
file9, you need to use the “ shift ” command.$ popd
$ pushd ../projects /JMeter > /dev/null
$ sh test-bash.sh file1 file2 file3 file4 file5 file6 file7 file8 file9, file10 , file11
shift

All it does is move all the command line ar guments to the left by 1 position. Which means the file1 will be moved out, and file2 becomes ${1} and file10
becomes ${2}. If you shift it agian, the file3 becomes ${1} and file1 1 becomes ${9}. In a nutshell
Q10 How will you empty or clear the contents of a file?
A10 Direct /dev/null (i.e. black hole) to the file
Q11 What does if [ $val -eq $? ] mean in a shell script?
A11 $? returns the status code of the last command. 0 means success and othe numbers mean failure exit codes.
Q12 How will you go about concatenating the account numbers in a number of text files to a single file?
Let’s say we have a accounts.patch file with 4 files containing account numbers._cFile10 =$9
shift
_cFile1 1=$9
${#} - T otal number of ar guments
${0} - Command or the script name
${1},${2}, ${3} - First, second and third args respectively .
${*} - All the command line arguments starting from $1.
${@} - Same as ${*} except when it is quoted "${@}" will pass the positional parameters unevaluated . For example , echo "${@}" will print
$ /dev/null > /tmp/simple -out.csv

A12 Firstly , cd to the folder where the “accounts.patch” file is, and then type the following command on a shell command line.
Explanation:
Q13 How will you go about executing multiple maven commands on unix? For example, build two separate projects one after another , for example — project1
and project2. The project2 to should only build if project1 successfully builds.
A13 Use the && contr ol operator to combine two commands so that the second is run only if the first command returns a zero exit status. In other words, if
the first command runs successfully , the second command runs. If the first command fails, the second command does not run at all. For example:/output /account_file_1 .txt
/output /account_file_1 .txt
/output /account_file_1 .txt
/output /account_file_1 .txt
for txt-file in $(sed -n '1,4p' accounts .patch ); do cat $txt-file >> accounts_combined .txt; done
1, 4p // print lines 1 to 4 from accounts.patch
for sql in $(sed -n '1,4p' account_sqls .patch ); // for each line read from accounts.patch
do cat $txt-file >> accounts_combined .txt // append (i.e >>) the contents of each file to a dif ferent file "accounts_combined.txt".
$ mvn --file project1 /pom.xml clean install -Dmaven .test.skip && mvn --file project2 /pom.xml clean install -Dmaven .test.skip

Similarly , the || control operator separates two commands and runs the second command only if the first command returns a non-zero exit status, that is if it fails.
In other words, if the first command is successful, the second command does not run. This operator is often used when testing for whether a given directory
exists and, if not, it creates one.
the -p command allows you to create any relevant parent directories as well. The && and || control characters can be used together .
Another control character thsat separates commands is “ ;“.
Q14 How will you go about listing all the users’ cron jobs?
A14 If you are a root user , you can list the cron jobs for all the users as shown below:
To edit cron jobs
To list the jobscd project1 /src/main || mkdir -p project1 /src/main
for user in $(cut -f1 -d: /etc/passwd ); do echo $user; crontab -l $user; done
crontab -e

crontab -l



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
