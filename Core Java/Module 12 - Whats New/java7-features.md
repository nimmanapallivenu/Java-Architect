# Java 7 Features

> **Module**: Whats New  
> **Topic**: Java 7 Features

---

## 📋 Table of Contents



There are several small new features and enhancements in Java 7. The major features and enhancements are in Java 8. Let’ s look at the Java 7 new features.
#1: string in switch statement :
public class Java7Feature1 {
private static String color = "BLUE" ;
private enum Color {
RED, GREEN
;
public static void main (String [] args) {
// Pre Java 5
if (color .equals ("RED" )) {
 System.out.println ("Color is Red" );
} else if (color .equals ("GREEN" )) {
 System.out.println ("Color is Green" );
} else {
 System.out.println ("Color not found" );
}
// Java 5 enum. try/catch is required for colours other than RED and GREEN
try {
 switch (Color .valueOf (color )) {
 case RED :
 System.out.println ("Color is Red" );
 break ;
 case GREEN :
 System.out.println ("Color is Green" );
 }
} catch (IllegalargumentException e) {
 System.out.println ("Color not found" );
}

Output is:
#2 Binary integral literals// Java 7 String in switch statement for simplicity & better readability
//JDK 7 switch performs better than if-else
//using types with enums is only useful when it serves a meaningful purpose
//the value for color could come from database, and string in switch is handy for this
switch (color ) {
case "RED" :
 System.out.println ("Color is Red" );
 break ;
case "GREEN" :
 System.out.println ("Color is Green" );
 break ;
default :
 System.out.println ("Color not found" );
}
Color not found
Color not found
Color not found
public class Java7Feature2 {

Output:
#3: Underscor es for better r eadability in numeric literalspublic static void main (String [] args) {
// Pre Java 7
int n = Integer .parseInt ("10000000", 2);
System.out.println (n);
n = 1 << 7;
System.out.println (n);
// Java 7
n = 0b10000000; // 128 = 2^7
System.out.println (n);
128
128
128
public class Java7Feature3 {
public static void main (String [] args) {
 //pre Java 7
 int million = 1000000 ;
 System.out.println (million );

Output :
#4: AutoCloseable interface. 
Java 5 introduced the Closeable interface and Java 7 has introduced the AutoCloseable interface to avoid the unsightly try/catch/finally(within finally try/catch)
blocks to close a resource. It also prevents potential resource leaks due to not properly closing a resource. The java.io.InputStr eam and java.io.OutputStream
now implements the AutoCloseable interface. //Java 7. More readable
 million = 1_000_000 ;
 System.out.println (million );
 
 //consecutive underscores are allowed
 int ten_million = 10__000_000 ;
 System.out.println (ten_million );
 
 //underscores can be used in other numeric types
 double million_dollars_5_cents = 1_000_000.0_5d ;
 System.out.println (million_dollars_5_cents );
 //illegal to have underscores
 //1. start or end a literal with an underscore _10.00, 10.00_
 //2. have underscores before or after a decimal point 10_.00, 10._00
1000000
1000000
10000000
1000000.05

try-with-resources is one of the most useful additions in Java 7.
mport java.io.BufferedReader ;
mport java.io.File;
mport java.io.FileInputStream ;
mport java.io.IOException ;
mport java.io.InputStream ;
mport java.io.InputStreamReader ;
public class Java7Feature4 {
public static void main (String [] args) {
// pre Java 7
BufferedReader br = null;
try {
 File f = new File("c://temp/simple.txt" );
 InputStream is = new FileInputStream (f);
 InputStreamReader isr = new InputStreamReader (is);
 br = new BufferedReader (isr);
 String read;
 while ((read = br.readLine ()) != null) {
 System.out.println (read);
 }
} catch (IOException ioe) {
 ioe.printStackT race();

The output is:} finally {
 try {
 if (br != null)
 br.close ();
 } catch (IOException ex) {
 ex.printStackT race();
 }
}
// Java 7 -- more concise 1 1 lines as opposed to 20 lines
try (InputStream is = new FileInputStream (new File("c://temp/simple.txt" ));
 InputStreamReader isr = new InputStreamReader (is);
 BufferedReader br2 = new BufferedReader (isr);) {
 String read;
 while ((read = br2.readLine ()) != null) {
 System.out.println (read);
 }
}
catch (IOException ioe) {
 ioe.printStackT race();
}
Big
brown fox
umped over the fence
Big

try can now have multiple statements in the parenthesis and each statement should create an object which implements the new java.lang. AutoCloseable
interface. The AutoCloseable interface consists of just one method.
void close () throws Exception {}. Each AutoClosable resource created in the try statement will be automatically closed! If an exception is thrown in the try
block and another Exception is thrown while closing the resource, the first Exception is the one eventually thrown to the caller .
Think of the close( ) method as implicitly being called as the last line in the try block.
#5 Multi-catch to avoid code duplicationbrown fox
umped over the fence
public class Java7Feature4 {
public static void main (String [] args) {
//pre Java 7
try {
 someMethod ();
} catch (CustomException1 ex1) {
 ex1.printStackT race();
} catch (CustomException2 ex2) {
 ex2.printStackT race();
}
//Java 7 -- 5 lines as opposed to 7 lines.
//no code duplication
try {
 someMethod ();
} catch (CustomException1 |CustomException2 ex) {
 ex.printStackT race();
}
public static void someMethod () throws CustomException1, CustomException2 {

Note that the pipe ‘|’ character is used as the delimiter .
#6 Impr oved type infer ence for generic instance cr eation
This is only a small change that makes generics declaration a little less verbose. As shown below, you can just use empty diamond “<>” in Java 7 on the RHS.public static class CustomException1 extends Exception {
private static final long serialV ersionUID = 1L;
public static class CustomException2 extends Exception {
private static final long serialV ersionUID = 1L;
mport java.util.Collections ;
mport java.util.HashMap ;
mport java.util.List;
mport java.util.Map;
public class Java7Feature4 {
public static void main (String [] args) {
 //Pre Java 7
getEmployeesWithManagersOld ("a102" );
//Java 7
getEmployeesWithManagersNew ("a102" );
public static Map<String, List<Employee >> getEmployeesWithManagersOld (String empCode ){
 if(empCode == null){
 return Collections .emptyMap ();

#7: Mor e new I/O APIs for the Java platform (NIO – 2.0)
Those who worked with Java IO may still remember the headaches that framework caused. It was never easy to work seamlessly across operating systems or
multi-file systems. The NIO 2.0 has come forward with many enhancements. It’ s also introduced new classes to ease the life of a developer when working with
multiple file systems with classes and interfaces such as Path, Paths, FileSystem, FileSystems and others.
Another very handy feature is the WatchService for file change notifications. It can monitor a directory for changes as demonstrated below. }
 //gives type safety warning. You need to add <String, List<Employee>> again on the RHS
 Map<String, List<Employee >> mapEmployees = new HashMap ();
 return mapEmployees ;
/Java 7
public static Map<String, List<Employee >> getEmployeesWithManagersNew (String empCode ){
 if(empCode == null){
 return Collections .emptyMap ();
 }
 //no duplication of generic inference
 Map<String, List<Employee >> mapEmployees = new HashMap <>();
 //do something with mapEmployees
 return mapEmployees ;
tatic class Employee {}

mport java.io.IOException ;
mport java.nio.file.FileSystems ;
mport java.nio.file.Files ;
mport java.nio.file.Path;
mport java.nio.file.Paths ;
mport java.nio.file.StandardW atchEventKinds ;
mport java.nio.file.WatchEvent ;
mport java.nio.file.WatchEvent .Kind ;
mport java.nio.file.WatchKey ;
mport java.nio.file.WatchService ;
public class Java7Feature7 {
public static void main (String [] args) throws IOException, InterruptedException {
// Java 7
Path path = Paths .get("c:\\T emp\\simple.txt" );
System.out.println (path.getFileName ());
System.out.println (path.getRoot ());
System.out.println (path.getParent ());
// Java 7 file change watch service
WatchService watchService = FileSystems .getDefault ().newW atchService ();
//register T emp folder with the watch service for addition of new file, modification of a file name, and deletion of a file
path.getParent ().register (watchService, StandardW atchEventKinds .ENTR Y_CREA TE,
 StandardW atchEventKinds .ENTR Y_MODIFY, StandardW atchEventKinds .ENTR Y_DELETE );
//wait for incoming events
while (true) {
 final WatchKey key = watchService .take();
 for (WatchEvent <?> watchEvent : key.pollEvents ()) {
 final Kind <?> kind = watchEvent .kind();
 // Overflow event
 if (StandardW atchEventKinds .OVERFLOW == kind) {
 continue; // loop
 } else if (StandardW atchEventKinds .ENTR Y_CREA TE == kind || StandardW atchEventKinds .ENTR Y_MODIFY == kind
 || StandardW atchEventKinds .ENTR Y_DELETE == kind) {
 @SuppressW arnings ("unchecked" )
 final WatchEvent <Path> watchEventPath = (WatchEvent <Path>) watchEvent ;
 final Path entry = watchEventPath .context ();

The output will be something like:
#8: Fork and Join
Java 7 has incorporated the feature that would distribute the work across multiple cores and then join them to return the result set as a Fork and Join framework.
he ef fective use of parallel cores in a Java program has always been a challenge. It’ s a divide-and-conquer algorithm where Fork-Join breaks the task at hand System.out.println (kind + "-->" + entry );
 }
 }
 if (!key.reset ()) {
 break ;
 }
}
// deleting a file is as easy as.
Files .deleteIfExists (path); // Java 7 feature as well.
simple .txt
c:\
c:\Temp
ENTR Y_CREA TE-->New Text Document .txt
ENTR Y_DELETE -->New Text Document .txt
ENTR Y_CREA TE-->File1 .txt
ENTR Y_MODIFY -->File1 .txt

into mini-tasks until the mini-task is simple enough that it can be solved without further breakups. One important concept to note in this framework is that
ideally no worker thread is idle. They implement a work-stealing algorithm in that idle workers “steal” the work from those workers who are busy .
The example below demonstrates this with a simple task of summing up 10 numbers. If the count of numbers to be added are greater than 5, it is forked into
chunks of 5 to be processed by separate thread, and the forked sum are then joined to give the overall total of 10 numbers from 1 to 10, which is 55. The total of
numbers 1 to 5 is 15, and 6 to 10 is 40.
mport java.io.IOException ;
mport java.util.ArrayList ;
mport java.util.Arrays ;
mport java.util.List;
mport java.util.concurrent .ForkJoinPool ;
mport java.util.concurrent .RecursiveT ask;
public class Java7Feature8 {
tatic int[] numbers = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
public static void main (String [] args) throws IOException, InterruptedException {
 int numberOfCpuCores = Runtime .getRuntime ().availableProcessors ();
 ForkJoinPool forkJoinPool = new ForkJoinPool (numberOfCpuCores );
 int sum = forkJoinPool .invoke (new ChunkingT ask(numbers ));
 System.out.println (sum);
/inner class
tatic class SumCalculatorT ask extends RecursiveT ask<Integer > {
int[] numbers ;
SumCalculatorT ask(int[] numbers ) {
 this.numbers = numbers ;
}
@Override
protected Integer compute () {
 int sum = 0;
 for (int i : numbers ) {
 sum += i;

 }
 System.out.println (Thread .currentThread ().getName () + " sum = " + sum);
 return sum;
}
/inner class
**
*
*chunking size is 5
*/
tatic class ChunkingT ask extends RecursiveT ask<Integer > {
private static final int CHUNK_SIZE = 5;
int[] numbers ;
ChunkingT ask(int[] numbers ) {
 this.numbers = numbers ;
}
@Override
protected Integer compute () {
 int sum = 0;
 List<RecursiveT ask<Integer >> forks = new ArrayList <>();
 //if the numbers size is > CHUNK_SIZE fork them
 if (numbers .length > CHUNK_SIZE ) {
 ChunkingT ask chunk1 = new ChunkingT ask(Arrays .copyOfRange (numbers, 0, numbers .length / 2));
 ChunkingT ask chunk2 = new ChunkingT ask(Arrays .copyOfRange (numbers, numbers .length / 2, numbers .length ));
 forks .add(chunk1 );
 forks .add(chunk2 );
 chunk1 .fork();
 chunk2 .fork();
 //size is less than or equal to CHUNK_SIZE start summing them
 } else {
 SumCalculatorT ask sumCalculatorT ask = new SumCalculatorT ask(numbers );
 forks .add(sumCalculatorT ask);
 sumCalculatorT ask.fork();
 }
 // Combine the result from all the tasks
 //join

Output is:
Java 8’ s Arrays.parallelSort( … ) make use of this fork and join feature to sort an array in parallel.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » for (RecursiveT ask<Integer > task : forks ) {
 sum += task.join();
 }
 return sum;
}
ForkJoinPool -1-worker -2 sum = 15
ForkJoinPool -1-worker -2 sum = 40
55



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03


---

## 📚 Related Topics

- [Java Overview](../Module%2001%20-%20Java%20Overview/)
- [Java Data Types](../Module%2002%20-%20Java%20Data%20Types/)
- [OOP Concepts](../Module%2006%20-%20OOP%20and%20FP/)

---

## 💡 Key Takeaways

Review the questions above and ensure you understand:
- Core concepts and their practical applications
- Real-world scenarios and use cases
- Best practices and common pitfalls

---

**[⬆ Back to Top](#)**