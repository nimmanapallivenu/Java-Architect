# Sorting a list of numbers by frequencies in Java using a BST tree   Java Success.com

## Table of Contents

This extends Sorting a list of numbers by frequencies in Java using a map to use a BST instead of a Map.
PROBLEM to solve: Sort a list of numbers by frequency of their occurrences. For example
INPUT : [ 5, 3, 7, 7, 7, 5, 4, 8 ]
OUTPUT : [7, 7, 7, 5, 5, 3, 4, 8]
Pr e-r equisite : Assumes that you understand the basics concepts like recursion , tree traversal algorithms and Basic Java T ree structure interview Q&A with
coding .
ALGORITHM to use
1. Create a BST (Binary Search T ree) and while creating BST maintain the frequency of each repeated number .
2. Do an Inorder traversal of BST and flatten every element and frequency of each element in a list.
3. Sort the list by frequency . More frequent to less frequent.
4. Traverse through the sorted by frequency array to repeat the numbers based on the frequency . E.g. if number 7 occurs three times, output 3 times.

Define Data and Node (i.e. T ree) classes as shown below
package com.mytutorial ;
public class Data {
 private int number ;
 private int frequency ;
 public Data (int number , int frequency ) {
 super ();
 this.number = number ;
 this.frequency = frequency ;
 }
 public int getNumber () {
 return number ;
 }
 public void setNumber (int number ) {
 this.number = number ;
 }
 public int getFrequency () {
 return frequency ;
 }
 public void setFrequency (int frequency ) {
 this.frequency = frequency ;
 }
 @Override
 public String toString () {
 return "Data [number=" + number + ", frequency=" + frequency + "]";
 }

package com.mytutorial ;
public class Node {
 private Data data;
 private Node left;
 private Node right ;
 public Node (Data data) {
 this.data = data;
 }
 public Data getData () {
 return data;
 }
 public void setData (Data data) {
 this.data = data;
 }
 public Node getLeft () {
 return left;
 }
 public void setLeft (Node left) {
 this.left = left;
 }
 public Node getRight () {
 return right ;
 }
 public void setRight (Node right ) {
 this.right = right ;

Define “CreateBST” to construct a tree of Nodes from an array and traverse InOrder to flatten to list }
package com.mytutorial ;
mport java.util.List;
public class CreateBST {
 public Node add(Integer [] input ) {
 Node root = null;
 for (Integer i : input ) {
 root = add(root, new Data (i, 1));
 }
 return root;
 }
 private Node add(Node root, Data data) {
 if (root == null) {
 return new Node (data);
 }
 if (data.getNumber () == root.getData ().getNumber ()) { // If already present
 root.getData ().setFrequency (root.getData ().getFrequency () + 1);
 } else if (data.getNumber () < root.getData ().getNumber ()) {
 root.setLeft (add(root.getLeft (), data)); //recursion
 } else {
 root.setRight (add(root.getRight (), data)); //recursion
 }
 return root;
 }

Define the executable main class to take input and create output by executing the algorithm defined in order . public void inOrderT oFlattenedList (Node currRoot , List<Data > listData ) {
 if (currRoot == null) {
 return ;
 }
 inOrderT oFlattenedList (currRoot .getLeft (), listData ); //recursion
 listData .add(currRoot .getData ());
 inOrderT oFlattenedList (currRoot .getRight (), listData ); //recursion
 }
package com.mytutorial ;
mport java.util.ArrayList ;
mport java.util.List;
mport java.util.stream .Collectors ;
public class SortByFreqUsingBST {
 public static void main (String [] args) {
 
 Integer [] input = new Integer [] {5, 3, 7, 7, 7, 5, 4, 8 };
 
 CreateBST bst = new CreateBST ();
 
 //create a BST tree with number & frequencies
 Node added = bst.add(input );
 
 //inorder BST traversal to flatten into array
 List<Data > flattened = new ArrayList <Data >();

Output: bst.inOrderT oFlattenedList (added , flattened );
 
 System .out.println ("Flattened: " + flattened );
 
 //Sort by frequency
 List<Data > listSortedByFrequency = flattened .stream ()
 .sorted ((e1, e2) -> e2.getFrequency () - e1.getFrequency ())
 .collect (Collectors .toList ());
 
 System .out.println ("Sorted:" + listSortedByFrequency );
 
 List<Integer > output = new ArrayList <Integer >();
 
 for (Data data : listSortedByFrequency ) {
 int frequency = data.getFrequency ();
 for (int i = 0; i < frequency ; i++) {
 output .add(data.getNumber ());
 }
 }
 
 System .out.println (output ); 
 }
Flattened : [Data [number =3, frequency =1], Data [number =4, frequency =1], Data [number =5, frequency =2], Data [number =7, frequency =3], Data [number =8,
frequency =1]]
Sorted : [Data [number =7, frequency =3], Data [number =5, frequency =2], Data [number =3, frequency =1], Data [number =4, frequency =1], Data [number =8,
frequency =1]]
7, 7, 7, 5, 5, 3, 4, 8]



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
