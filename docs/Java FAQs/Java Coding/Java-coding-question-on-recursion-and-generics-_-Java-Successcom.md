# Java coding question on recursion and generics   Java Success.com

## Table of Contents

Q. Can you write Java code to compute a collection of numbers supplied to it? The computation could be addition, subtraction, etc. Use recursion to compute
the numbers. Here are some requirements to take into considerations.
1. It should be flexible enough to convert from Recursion to iteration if required.
2. Computation will initially be “addition”, but should be extendable to multiplication, subtraction, etc.
3. Should handle integers and floating point numbers.
4. Make use of generics .
Here is a sample test class.
A. Firstly , define the “Compute” interface and the “Recursion” implementation.package recursiontoiteration ;
mport java.util.Queue ;
mport java.util.concurrent .ArrayBlockingQueue ;
public class RecursionT est
{
 public static void main (String [] args)
 {
 Queue <Integer > q = new ArrayBlockingQueue <Integer >(5);
 q.add(5);
 q.add(10);
 q.add(12);
 
 Compute <Double , Integer > compute = new Recursion <Double , Integer >();
 Double result = compute .compute (null, q, new Multiply <Double , Integer >());
 System .out.println (result );
 }

R and N are generic types meaning say Result and Number respectively . In the above example, they will be inferred at compile time as Double and Integer
types respectively . Here is the Recursion implementation.package recursiontoiteration ;
mport java.util.Queue ;
public interface Compute <R, N>
{
 R compute (R r, Queue <N> q, Function <R, N> function );
package recursiontoiteration ;
mport java.util.Queue ;
public class Recursion <R, N> implements Compute <R, N>
{
 public R compute (R r, Queue <N> q, Function <R, N> function )
 {
 //recursion exit condition - no items in the queue to process
 if (q.size() == 0)
 {
 return r;
 }
 
 r = compute (function .apply (r, q.poll()), q, function );
 
 return r;
 
 }

Let’s define the Function interface to represent mathematical operations like Sum , Multiply , Divide , etc.
The implementation Sum will be:package recursiontoiteration ;
public interface Function <R, N>
{
 R apply (R r, N n);
}
package recursiontoiteration ;
mport java.math .BigDecimal ;
public class Sum<R, N> implements Function <R, N>
{
 
 //add two numbers
 @SuppressW arnings ("unchecked" )
 @Override
 public R apply (R r, N n)
 {
 Number result = Double .valueOf (0);
 Number number = Double .valueOf (0);
 if (r != null)

Multiply will be implemented as shown below . {
 result = (Double ) r;
 }
 
 if (n != null)
 {
 number = (Number ) n;
 }
 
 //big decimal is better for rounding values
 BigDecimal addedV alue = new BigDecimal (result .doubleV alue()).add(new BigDecimal (number .doubleV alue()));
 
 return (R) (Number ) Double .valueOf (addedV alue.toPlainString ());
 }
package recursiontoiteration ;
mport java.math .BigDecimal ;
public class Multiply <R, N> implements Function <R, N>
{
 
 @SuppressW arnings ("unchecked" )
 @Override
 public R apply (R r, N n)
 {
 Number result = Double .valueOf (1);
 Number number = Double .valueOf (1);
 if (r != null)
 {
 result = (Double ) r;
 }

Even though it is a trivial example, many beginner to intermediate level developers struggle with –> recursion, generics, and writing extendable programs with
proper interfaces.
In the next blog post, we will find how to convert recursion to iteration. if (n != null)
 {
 number = (Number ) n;
 }
 
 //big decimal is better for rounding values
 BigDecimal multipliedV alue = new BigDecimal (result .doubleV alue())
 .multiply (new BigDecimal (number .doubleV alue()));
 
 return (R) (Number ) Double .valueOf (multipliedV alue.toPlainString ());
 }



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
