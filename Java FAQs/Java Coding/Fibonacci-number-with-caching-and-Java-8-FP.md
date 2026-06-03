# Fibonacci number with caching and Java 8 FP

## Table of Contents

Complimenting Fibonacci number coding – iterative and recursive approach , we can improve performance by caching. If you run this
Outputpublic class RecursiveFibonacci {
public int fibonacci (int n) {
 if (n == 0 || n == 1)
 return n;
 System .out.println ("evaluating fibonacci(" + n + ")");
 return fibonacci (n - 2) + fibonacci (n - 1);
}
public static void main (String [] args) {
 int nThfibonacciNo = new RecursiveFibonacci ().fibonacci (5);
 System .out.println (nThfibonacciNo );
}
evaluating fibonacci (5)
evaluating fibonacci (3)
evaluating fibonacci (2)
evaluating fibonacci (4)
evaluating fibonacci (2)
evaluating fibonacci (3)
evaluating fibonacci (2)
5

and you can see “fibonacci(3)” is repeated 2 times, “fibonacci(2)” is repeated 3 times, and so on. If you pick a lar ger number like 21, there will be many repeats.
Let’s use cache to store evaluated values to improve performance.
mport java.util.Map;
mport java.util.concurrent .ConcurrentHashMap ;
public class RecursiveFibW ithCache {
private Map<Integer , Integer > cache = new ConcurrentHashMap <>(20);
public int fibonacci (int n) {
 if (n == 0 || n == 1)
 return n;
 Integer result = cache .get(n);
 if (result == null) {
 synchronized (cache ) {
 result = cache .get(n);
 if (result == null) {
 System .out.println ("evaluating fibonacci(" + n + ")");
 result = fibonacci (n - 2) + fibonacci (n - 1);
 cache .put(n, result );
 }
 }
 }
 
 return result ;
 
}
public static void main (String [] args) {
 int nThfibonacciNo = new RecursiveFibW ithCache ().fibonacci (5);
 System .out.println (nThfibonacciNo );
}

Output
Now , no repetitions. Can we further improve on this? If we use Java 8, we can make use of the Concurr entHashMap.computeIfAbsent(..) addition.
Here is more compact code with Java 8 functional programmingevaluating fibonacci (5)
evaluating fibonacci (3)
evaluating fibonacci (2)
evaluating fibonacci (4)
5
public V computeIfAbsent (K key, Function <? super K,? extends V> mappingFunction )
mport java.util.Map;
mport java.util.concurrent .ConcurrentHashMap ;
public class RecursiveFibW ithCache {
private Map<Integer , Integer > cache = new ConcurrentHashMap <>(20);

Output
If you remove the print statement, it becomes even simplerpublic int fibonacci (int n) {
 if (n == 0 || n == 1)
 return n;
 return cache .computeIfAbsent (n, (key) -> {
 System .out.println ("evaluating fib(" + n + ")");
 return fibonacci (n - 2) + fibonacci (n - 1);
 });
}
public static void main (String [] args) {
 int nThfibonacciNo = new RecursiveFibW ithCache ().fibonacci (5);
 System .out.println (nThfibonacciNo );
}
evaluating fibonacci (5)
evaluating fibonacci (3)
evaluating fibonacci (2)
evaluating fibonacci (4)
5
return cache .computeIfAbsent (n, (key) -> fib(n - 2) + fib(n - 1));



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
