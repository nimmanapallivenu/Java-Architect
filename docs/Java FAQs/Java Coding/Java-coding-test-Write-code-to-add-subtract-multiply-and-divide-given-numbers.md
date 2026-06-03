# Java coding test Write code to add, subtract, multiply, and divide given numbers

## Table of Contents

A trivial coding example (i.e. a Calculator) tackled using the following programming paradigms in Java not only to perform well in coding interviews, but
also to learn these programming paradigms.
Appr oach 1 : Procedural Programming
Appr oaches 2 – 4 : Object Oriented Programming
Appr oach 5 : Functional Programming (Java 8)
Approach 1: Procedural
public interface Calculate {
 abstract int calculate (int operand1 , int oerand2 , Operator operator );
}
public enum Operator {
 ADD , SUBTRACT , DIVIDE , MUL TIPL Y;
}
public class CalculateImpl implements Calculate {
 @Override
 public int calculate (int operand1 , int operand2 , Operator operator ) {
 switch (operator ) {
 case ADD :

Output : result=13
Approach 2: OOP return operand1 + operand2 ;
 case SUBTRACT :
 return operand1 - operand2 ;
 case MUL TIPL Y:
 return operand1 * operand2 ;
 case DIVIDE :
 return operand1 / operand2 ;
 }
 throw new RuntimeException (operator + "is unsupported" );
 }
public class CalculatorT est {
 public static void main (String [] args) {
 Calculate calc = new CalculateImpl ();
 int result = calc.calculate (5,6,Operator .ADD );
 result = calc.calculate (result ,6,Operator .MUL TIPL Y);
 result = calc.calculate (result ,1,Operator .SUBTRACT );
 result = calc.calculate (result ,5,Operator .DIVIDE );
 
 System .out.println ("result=" + result );
 }
public interface MathCommand <E> {
 abstract E execute (E operand1 , E operand2 );

}
public class AddCommand implements MathCommand <Integer >{
 @Override
 public Integer execute (Integer operand1 , Integer operand2 ) {
 return operand1 + operand2 ;
 } 
}
public class SubtractCommand implements MathCommand <Integer >{
 @Override
 public Integer execute (Integer operand1 , Integer operand2 ) {
 return operand1 - operand2 ;
 } 
}
public class MultiplyCommand implements MathCommand <Integer >{
 @Override
 public Integer execute (Integer operand1 , Integer operand2 ) {
 return operand1 * operand2 ;
 } 
} 

When you have more mathematical operations, add more command classes. In OOP , switch statements are unsightly and hard to maintain. The above OOP
approach eliminates the need for switches. This is also a good example of the “Open-Close design principle”.
Output : result=13
Approach 3: OOP
This extends appr oach-2 to make the client code more elegant to use with “*”, “+”, etc.public class DivideCommand implements MathCommand <Integer >{
 @Override
 public Integer execute (Integer operand1 , Integer operand2 ) {
 return operand1 / operand2 ;
 } 
}
public class CalculatorT est2 {
 public static void main (String [] args) {
 MathCommand <Integer > command = new AddCommand ();
 Integer result = command .execute (5, 6);
 command = new MultiplyCommand ();
 result = command .execute (result , 6);
 command = new SubtractCommand ();
 result = command .execute (result , 1);
 command = new DivideCommand ();
 result = command .execute (result , 5);
 System .out.println ("result=" + result );
 }

mport java.util.HashMap ;
mport java.util.Map;
public final class Calculator {
 private static final Map<Character , MathCommand <Integer >> mapOperations =
 new HashMap <Character , MathCommand <Integer >>();
 
 public Calculator () {
 init();
 }
 public void init() {
 mapOperations .put('+', new AddCommand ());
 mapOperations .put('*', new MultiplyCommand ());
 mapOperations .put('-', new SubtractCommand ());
 mapOperations .put('/', new DivideCommand ());
 }
 
 public Integer calc(Character operator , Integer operand1 , Integer operand2 ) {
 MathCommand <Integer > op = mapOperations .get(operator );
 if (op != null) {
 return op.execute (operand1 , operand2 );
 }
 else {
 throw new RuntimeException (operator + "is unsupported" );
 }
 }
public class CalculatorT est {
 public static void main (String [] args) {

Output : result=13
Approach 4: OOP
This extends appr oach-2 & appr oach-3 to make the client code more elegant “.” notations [e.g. blah.calc(‘+’, 6).calc(‘*’, 6).calc(‘-‘, 1).blah] with the help of
“Builder” design pattern. Calculator calc = new Calculator ();
 Integer result = calc.calc('+', 5, 6);
 result = calc.calc('*', result , 6);
 result = calc.calc('-', result , 1);
 result = calc.calc('/', result , 5);
 
 System .out.println ("result=" + result );
 }
mport java.util.HashMap ;
mport java.util.Map;
public final class Calculator {
 Integer result = 0;
 private static final Map<Character , MathCommand <Integer >> mapOperations =
 new HashMap <Character , MathCommand <Integer >>();
 public Calculator (CalculationBuilder builder ) {
 this.result = builder .result ;
 }
 public Integer getResult (){
 return this.result ;
 }
 //inner static class applying the builder design pattern
 public static class CalculationBuilder {

 protected Integer result ;
 
 CalculationBuilder (Integer result ){
 init();
 this.result = result ;
 }
 
 public void init() {
 mapOperations .put('+', new AddCommand ());
 mapOperations .put('*', new MultiplyCommand ());
 mapOperations .put('-', new SubtractCommand ());
 mapOperations .put('/', new DivideCommand ());
 }
 CalculationBuilder calc(Character operator , Integer operand ) {
 MathCommand <Integer > op = mapOperations .get(operator );
 if (op != null) {
 this.result = op.execute (result , operand );
 } else {
 throw new RuntimeException (operator + "is unsupported" );
 }
 return this;
 }
 }
public class CalculatorT est {
 public static void main (String [] args) {
 //more elegant to build mathematical operations
 Calculator .CalculationBuilder calcBuilder = new Calculator .CalculationBuilder (5)
 .calc('+', 6)
 .calc('*', 6)
 .calc('-', 1)
 .calc('/', 5);
 Calculator calc = new Calculator (calcBuilder );

Output : result=13
Approach 5: FP
Java 8 functional programming. Y ou can see Lambdas, functional interfaces, default methods, and static methods in action. System .out.println ("result=" + calc.getResult ());
 }
package com.java8 .examples ;
mport java.util.function .BinaryOperator ;
mport java.util.function .Function ;
mport java.util.Objects ;
@FunctionalInterface
public interface MathOperation <Intetger > {
 
 //SAM -- Single Abstract Method.
 //identifier abstract is optional
 Integer operate (Integer operand );
 
 default MathOperation <Integer > add(Integer o){
 return (o1) -> operate (o1) + o;
 }
 
 default MathOperation <Integer > multiply (Integer o){
 return (o1) -> operate (o1) * o;
 }
 
 default MathOperation <Integer > subtract (Integer o){
 return (o1) -> operate (o1) - o;
 }
 
 default MathOperation <Integer > divide (Integer o){
 return (o1) -> operate (o1) / o;

Output : result=13 }
 
 default Integer getResult () {
 return operate (0);
 }
 
 default void print (){
 System .out.println ("result=" + getResult ());
 }
 
 //static helper to initialize
 static Integer init(Integer input ) {
 return input ;
 }
 
package com.java8 .examples ;
public class CalculatorT est {
 
 public static void main (String [] args) {
 
 //An expressive static helper method
 MathOperation <Integer > calc = (x) -> MathOperation .init(5);
 
 MathOperation <Integer > complexOp = calc.add(6)
 .multiply (6)
 .subtract (1)
 .divide (5);
 
 complexOp .print ();
 }

This is a very trivial example, and some solutions could be bit of an over kill.
Q. Which one would you favor , and why?
Q. Would you provide a dif ferent solution?



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
