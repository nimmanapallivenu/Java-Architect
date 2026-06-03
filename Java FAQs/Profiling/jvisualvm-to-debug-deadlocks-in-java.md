# jvisualvm to debug deadlocks in Java   Java Success.com

## Table of Contents

This extends JConsole for debugging deadlocks in Java applications , using jvisualvm that gets shipped with your JDK.
Step 1: Java code that creates a dead lock situation by
a) thread-0 holding on to lock1 and waiting for the lock2,
b) and thread-1 holding on to lock2 and waiting for the lock1.
package com.debug .multithread ;
public class DeadLockT est extends Thread {
 public static Object lock1 = new Object ();
 public static Object lock2 = new Object ();
 public void method1 () {
 synchronized (lock1 ) {
 delay (500); //some operation
 System .out.println ("method1: " + Thread .currentThread ().getName ());
 synchronized (lock2 ) {
 System .out.println ("method1 is executing .... " );
 }
 }
 }
 public void method2 () {
 synchronized (lock2 ) {
 delay (500); //some operation
 System .out.println ("method1: " + Thread .currentThread ().getName ());
 synchronized (lock1 ) {
 System .out.println ("method2 is executing .... " );
 }
 }
 }
 @Override
 public void run() {
 method1 ();
 method2 ();
 }
 /**
 * main metghod runs on a main thread
 * @param ar gs
 */
 public static void main (String [] args) {
 DeadLockT est thread1 = new DeadLockT est(); // worker thread1
 DeadLockT est thread2 = new DeadLockT est(); // worker thread2

Step 2: From a DOS command prompt use “ jps” to list the java process ids. This command is in %JA VA_HOME%/bin
Step 3: Process id “9460” is the “DeadLockT est”. So, let’ s open jvisualvm that is shipped with Java in %JA VA_HOME%/bin.
Step 4: jvisualvm shows where the problem is:
Double click on DeadLockT est. thread1 .start();
 thread2 .start();
 }
 /**
 * The delay is to simulate some real operation happening.
 * @param timeInMillis
 */
 private void delay (long timeInMillis ) {
 try {
 Thread .sleep (timeInMillis );
 } catch (InterruptedException e) {
 e.printStackT race();
 }
 }
C:\>jps
9460 DeadLockT est
1952 Jps
8148
C:\>jvisualvm 9460

jvisualvm
Thread states are color coded, and we are interested in the “Blocked” states, which are red in color .

jvisualvm color codes the thread states
How long is the monitor being held for?

jvisualvm the monitor (i.e. lock) has been held for a long time
Click the button “ Thread Dump ” to get more details

Thread dump from jvisualvm



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
