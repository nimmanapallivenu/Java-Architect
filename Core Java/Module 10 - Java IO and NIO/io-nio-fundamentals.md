# Java I/O & NIO Fundamentals

> **Module**: Java IO and NIO  
> **Topic**: Java I/O & NIO Fundamentals

---

## 📋 Table of Contents



- [Q1: What is the purpose of Java I/O System?](#q1)
- [Q2: What is the difference between InputStream/OutputStream class hierarchy and Rea](#q2)
- [Q3: What is an I/O filter?](#q3)
- [Q4: How will you go about capturing user input in Java via command line?](#q4)
- [Q5: Can you explain the use of the decorator design pattern in Java I/O?](#q5)
- [Q6: Is it possible to connect a Reader to an InputStream?](#q6)
- [Q7: What is the purpose of the File class? What is the purpose of a System class?](#q7)
- [Q9: Why do we need new I/O? What is wrong with the old I/O?](#q9)
- [Q10: What are the core components of NIO?](#q10)
- [Q11: What do you understand by decoding and encoding? What do you understand by slici](#q11)
- [Q12: What are scattering and gathering I/O operations? Where are these useful?](#q12)
- [Q13: What is the central object in non-blocking I/O?](#q13)
- [Q14: How would you go about creating and writing to large files?](#q14)
- [Q15: What are some of the best practices or tips relating to Java I/O?](#q15)

---

## 🔹 Q1: What is the purpose of Java I/O System?

**Answer:**

The purpose is to provide an abstraction for all types of I/O like memory, file, directory, network, and pipe. Pipes in Java IO provides the ability for two
threads running in the same JVM to communicate with each other. A PipedInputStream should be connected to a PipedOutputStream. The data written to the
PipedOutputStream by first thread, can be read from the second thread connected to the PipedInputStream. Using the same thread for read( ) and write( ) may
result in the thread deadlocking itself.
Java I/O

---

## 🔹 Q2: What is the difference between InputStream/OutputStream class hierarchy and Reader/W riter class hierarchy?

**Answer:**

The InputStream/OutputStream class hierarchy is byte oriented. It uses 8 bit ASCII byte streams.
The Reader/W riter class hierarchy is character oriented. It uses 16 bit Unicode character streams.

---

## 🔹 Q3: What is an I/O filter?

**Answer:**

The InputStream and OutputStream classes are used for reading from and writing to byte streams, respectively. FilterInputStream and FilterOutputStream

are used for data transformation and manipulation.

---

## 🔹 Q4: How will you go about capturing user input in Java via command line?

**Answer:**

#1: Using the System.console( ). This will not work within eclipse IDE. You can try it on a DOS or Unix command-line.
Console console = System .console (); 
String input = console .readLine ("What is your name:" ); 

#2: The Scanner class.
#3: Decorating System.in with Buf feredReader and SystemInputReader .

---

## 🔹 Q5: Can you explain the use of the decorator design pattern in Java I/O?

**Answer:**

The java.io.* classes use the decorator design pattern. The decorator design pattern attaches r esponsibilities to objects at runtime. Decorators are more
flexible than inheritance because the inheritance attaches responsibility to classes at compile time. The java.io.* classes use the decorator pattern to construct
different combinations of behavior at runtime based on some basic classes.
Decorators decorate an object by enhancing or restricting functionality of an object it decorates. The decorators add or restrict functionality to decorated objects
either before or after forwarding the request.System.out.println ("User input = " + input );
Scanner reader = new Scanner (System .in); 
System.out.println ("Enter first operand?" ); 
nt operand1 =reader .nextInt ();
BufferedReader br = new BufferedReader ( new InputStreamReader (System .in)); 
System.out.println ("Enter first operand?" ); 
nt operand1 = Integer .parseInt (br.readLine ());

IO
For example, LineNunberInputStr eam code shown below illustrates that at runtime the LineNumberReader (lnr), which is a decorator (aka a wrapper around
decorated object), forwards the method call to its decorated object Buf feredReader (br). The “br” forwards the method call to its decorated object StringReader
(sr). The “br” and “lnr” are used to enhance the functionality of the “sr”. The “lnr” enhances the functionality by evaluating the line numbers and the “br”
enhances the functionality by buf fering the data for better performance.

---

## 🔹 Q6: Is it possible to connect a Reader to an InputStream?

**Answer:**

Yes. You can bridge this gap using InputStreamreader .

---

## 🔹 Q7: What is the purpose of the File class? What is the purpose of a System class?

**Answer:**

The File class provides access to files and directories in a uniform manner using the composite design pattern. It provides an abstraction to hide the
differences between operating systems in terms of path separators, permissions, directories, drive letters, etc.
The New I/O 2 API replaces the java.io.File with java.nio.file.Path. The FileSystem and Path classes provide the abstraction to read, write, navigate, copy, and
move files. The New I/O 2 also include true asynchronous I/O on both files & sockets, virtual file systems, and watch lists.
The purpose of a System class is to provide access to system resources.StringReader sr = new StringReader (sb.toString ());
BufferedReader br = new BufferedReader (sr);
nr = new LineNumberReader (br);
String operatingSystem = System .getProperty ("os.name" );
System.out.println ("OS=" + operatingSystem );
String homeFolder = System .getProperty ("user .home" );
System.out.println ("home-dir=" + homeFolder );
String path = homeFolder + File.separator + filename ;

Q8 What is the advantage of using a RandomAccessFile? What are the different modes in which you can use it?
A8. RandomAccessFile enable you to read or write to a specific location in the file at the file pointer. Imagine the file as a large array of data that have their
own index.
You can instantiate a RandomAccessFile in a read only mode as shown below:
or read and write mode as shown below:

---

## 🔹 Q9: Why do we need new I/O? What is wrong with the old I/O?

**Answer:**

There is nothing wrong with the old I/O. The new I/O was introduced in JDK 1.4, and it brings host of powerful capabilities like:
1) Non-blocking modes: A server ’s ability to handle several client requests ef fectively depends on how it uses its I/O streams. When a server has to handle
hundreds of clients simultaneously, it must be able to use I/O services concurrently. One way to cater for this scenario in Java is to use either polling or create
many threads. Even though multi-threading in Java has become very ef ficient since Java versions 5 and 6, having almost one-to-one ratio of threads (200 clients
will have 200 threads) can be prone to enormous thread overhead and can result in performance and scalability problems due to consumption of memory stacks
(i.e. each thread has its own stack) and CPU context switching (i.e. switching between threads as opposed to doing real computation.). T o overcome thisSystem .arraycopy (array1, 0, array2, 2, 2);
ong nanoT ime = System .nanoT ime();
SecurityManager secManager = System .getSecurityManager ();
System .gc();
System .exit(1);
RandomAccessFile raf = new RandomAccessFile ("MyBook.dat", "r");
RandomAccessFile raf = new RandomAccessFile ("MyBook.dat", "rw");

scalability problem, a new set of non-blocking I/O classes have been introduced to the Java platform in java.nio package. The non-blocking I/O mechanism is
built around Selectors and Channels. Channels, Buf fers and Selectors are the core of the NIO.
2) More efficient r eads and writes in terms of blocks of data using buf fers as opposed to reading or writing in byte streams. large files can be created and
modified with memory mapped files. You can pretend that the entire file is in memory .
3) File locks .
4) Buffers to manipulate primitive data with positioning, flipping, clearing, slicing, scattering/gathering, read only buf fers, etc.

---

## 🔹 Q10: What are the core components of NIO?

**Answer:**

Buffers, Channels, and Selectors.
Java non-blocking I/O
Buffers hold data. Buf fers are to primitive types as collections are to objects. All data is handled with buf fers. When data is read, it is read directly into a buf fer.
When data is written, it is written to a buf fer. Channels can fill and drain Buf fers. Buf fers replace the need for you to do your own buf fer management using byte
arrays. There are different types of Buf fers like ByteBuf fer, CharBuf fer, DoubleBuf fer, etc.

A buf fer is really a glorified array. Pay attention to terms like:
Position – keeps track of how much data you have read or written. Every time you read, the position changes. If you read 4 bytes from a channel into a buf fer,
the buf fer’s position will be set to 4, referring to the fifth element of the underlying array. If you have written 6 bytes to a channel from a buf fer, that buf fer’s
position will be set to 6 referring to the seventh element of the underlying array .
Limit – specifies how much data there is left to get or how much room there is left to put. The position is always less than, or equal to, the limit. If you invoke
the method flip() before you are ready to write the data, it will set the limit to the current position and resets the position to 0. So, everything between 0 and the
limit will be written. If you invoke the method clear( ) before reading, it will set the limit to the capacity (e.g. 1024) and resets the position to 0. It will also clear
the whole buf fer.
Capacity – specifies the maximum amount of data that can be stored in the underlying array. The limit can never be larger than the capacity .
A Channel class is similar to InputStream and OutputStream. Unlike streams channels are bi-directional and can read more than one byte of data. Channels use
buffers to read and write blocks of data between data sources such as a socket, a file, or an application component, which is capable of performing one or more
I/O operations such as reading or writing. Channels can be non-blocking, which means, no I/O operation will wait for data to be read or written to the network.
The good thing about NIO channels is that they can be asynchronously interrupted and closed. So if a thread is blocked in an I/O operation on a channel, another
thread can interrupt that blocked thread.
A Selector class enables multiplexing (i.e. combining multiple streams into a single stream) and demultiplexing (i.e. separating a single stream into multiple
streams) of I/O events, and makes it possible for a single thread to ef ficiently manage many I/O channels. A Selector monitors selectable channels, which are
registered with it for I/O events like connect, accept, read and write. The keys (e.g. key1, key2) represented by the SelectionKey class encapsulate the
relationship between a specific selectable channel and a specific selector .

---

## 🔹 Q11: What do you understand by decoding and encoding? What do you understand by slicing and data sharing when it comes to buf fers?

**Answer:**

The decoders and encoders are used for converting bit-by-bit representation of a string into actual char values and to convert the character values back to
bits. In general the sequence is like this:
You can create a Charset (e.g. US-ASCII, ISO-8859-1, UTF-8, UTF-16, etc), which is a named mapping between sequences of 16 bit Unicode characters and
sequences of bytes. You can use a Charset for a given character encoding, and use that for both coding and decoding.
Slicing is used to create a snapshot from an existing buf fer. It creates a new sub-buf fer that shares its data with a portion of the original buf fer.

---

## 🔹 Q12: What are scattering and gathering I/O operations? Where are these useful?

**Answer:**

A scattering read is like a regular read but it reads into an array of buf fers as opposed to a single buf fer. Likewise, a gathering writes data from an array of
buffers as opposed to a single buf fer.
Useful for dividing a piece of data into sections. For example, you can have a message payload with a fixed length header, a fixed length body and a fixed
length footer. Scattering can be used to read these 3 sections into their own buf fers.

---

## 🔹 Q13: What is the central object in non-blocking I/O?

**Answer:**

Selectors are the central component for non-blocking or asynchronous I/O operations. A Selector is where you register your interest in various I/O events,
and it is the object that notifies you when those events occur. This uses a design patter know as the reactor design pattern. The reactor pattern is similar to an
observer pattern (aka publisher and subscriber design pattern), but an observer pattern handles only a single source of events (i.e. a single publisher with
multiple subscribers) where a reactor pattern handles multiple event sources (i.e. multiple publishers with multiple subscribers).
Writing non-blocking TCP or UDP servers using NIO from scratch is not a trivial task, and when required to do so, refer to proven frameworks like Apache
MINA .

---

## 🔹 Q14: How would you go about creating and writing to large files?

**Answer:**

Another sought after functionality of NIO is its ability to map a file to memory. There is a specialized form of a Buf fer known as “MappedByteBuf fer”,
which represents a buf fer of bytes mapped to a file. T o map a file to “MappedByteBuf fer”, you must first get a channel for a file. Once you get a channel then
you map it to a buf fer and subsequently you can access it like any other “ByteBuf fer”. Once you map an input file to a “CharBuf fer”, you can do pattern
matching on the file contents. This is similar to running “grep” on a UNIX file system.
Another feature of NIO is its ability to lock and unlock files. Locks can be exclusive or shared and can be held on a contiguous portion of a file. But file locks
are subject to the control of the underlying operating system.

---

## 🔹 Q15: What are some of the best practices or tips relating to Java I/O?

**Answer:**

1) Don’ t work with file names as Strings. Use the FileSystem and Path classes.
2) Enterprise applications need to be portable and are generally clustered. Hence don’ t use absolute file names. Use relative file names, which should be
resolved relative to the JVM’ s current directory. This directory setting depends on the details of the JVM’ s launch process. Load the configuration files relative
to the classpath.

Use handy methods like ClassLoader .getResourceAsStr eam(“relevant/package/config.properties”); and
Class .getResourceAsStr eam(“relevant/package/config.properties”);
3) Avoid reading or writing byte by byte. Prefer using buf fered streams or buf fered readers. Disable line buf fering. T une the buf fer size (bigger is usually better
if memory is available). The Java NIO packages offer mor e efficient r ead and write operations in blocks using the buf fer classes. It also of fers direct
buffers, which use the OS memory as opposed to the JVM memory. The decision to use direct buf fers or non-direct (i.e. heap) buf fers is application dependent.
Large files can be manipulated using the memory mapped byte buf fers.
4) Use non-blocking I/O in the Java NIO packages for better scalability. Prefer using frameworks like Apache MINA as opposed to trying to write your own.
In Java EE 7, Servlets use non blocking I/O.
5) When you read external data or write data to an external system, you have to understand the encoding used in the external system and decide where the
conversion should take place. Consider if support is required for internationalization. If your application contains String objects that need to be translated into
different languages, you should store these String objects in a ResourceBundle that is backed up by a set of properties files.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »public void loadProperties (String baseConfigFile ) {
 if (isNotEmpty (baseConfigFile )) {
 try {
 URL url = Thread .currentThread ()
 .getContextClassLoader ().getResource (
 baseConfigFile );
 props = new Properties ();
 props .load(url.openStream ());
 } catch (Exception e) {
 throw new RuntimeException ("Error loading file: " + e);
 }
 }
ResourceBundle .getBundle (“relevant .package .resource ”);

---



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