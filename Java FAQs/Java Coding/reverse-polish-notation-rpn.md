# Reverse Polish Notation (RPN)   Java Success.com

## Table of Contents

- [Q1: Can you explain Reverse Polish Notation (RPN)?](#q1)

---

## Q1: Can you explain Reverse Polish Notation (RPN)?

**Answer:**

You have already heard about the following from your elementary schooling:
“Please Excuse My Dear Aunt Sally” (meaning Parentheses, Exponentiation (roots and powers), Multiplication, Division, Addition, and Subtraction
BODMAS, tells us to attend to Brackets, Of, Division, Multiplication, Addition, and Subtraction
This means – using BODMAS or “Please Excuse My Dear Aunt Sally”
Polish Notation was invented in the 1920’ s by Polish mathematician Jan Lukasiewicz, who showed that by writing operators in front of their operands, instead
of between them, brackets were made unnecessary .
Using Reverse Polish Notion (RPN):
The algorithm for RPN is:
Keep pushing the numbers into a stack (i.e. LIFO), and when it is an operator , pop two numbers from the stack, do the calculation, and push the result back into
the stack.
Diagram:1+2)*3 = 9
3 2 1 + x = 9 

Java code:
It can be coded in Java 7, as shown below . Java 7 supports string in switch statements.
mport java.util.ArrayDeque ;
mport java.util.Deque ;
mport java.util.Stack ;
public class ReversePolishNotationT est {
 public static void main (String [] args) {
 String [] tokens = new String [] { "3", "2", "1", "+", "*" };
 System .out.println (evalRPN (tokens ));

 }
 public static int evalRPN (String [] tokens ){
 int result = 0;
 String operators = "+-*/" ;
 //Double ended queue can be used as a stack LIFO(push/pop) or queue FIFO (of fer/poll).
 Deque <String > stack = new ArrayDeque <String >();
 for (String string : tokens ) {
 //keep pushing the numbers
 if (!operators .contains (string )) {
 stack .push (string );
 } else { //for operators
 int a = Integer .valueOf (stack .pop());
 int b = Integer .valueOf (stack .pop());
 switch (string ) {
 case "+":
 stack .push (String .valueOf (a + b));
 break ;
 case "-":
 stack .push (String .valueOf (b - a));
 break ;
 case "*":
 stack .push (String .valueOf (a * b));
 break ;
 case "/":
 stack .push (String .valueOf (b / a));
 break ;
 }
 }
 }
 //get the value left in the stack
 result = Integer .valueOf (stack .pop());
 return result ;
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
