# Huffman coding in Java   Java Success.com

## Table of Contents

- [Q1: What is Huf fman coding?](#q1)
- [Q2: How will you write code in Java that prints the Huf fman codes for a given strin](#q2)

---

## Q1: What is Huf fman coding?

**Answer:**

Huffman coding is a compression technique used to reduce the number of bits needed to send or store a message. For example, if you have a word “Business”,
which is a 8 character word and if you were to assign
8 bits for each character then 8 * 8 = 64 bits .
4 bits for each character then 8 * 4 = 32 bits .
The Huf fman coding is based on the idea that frequently-appearing letters should have shorter bit representations and less common letters should have longer
representations. It firstly constructs a binary tree based on this concept and then assigns “ 0s” and “ 1s” to each node. For example, “0s” for the left side of the tr ee
and “1s” for the right side of the tr ee. Here is tree diagram showing that Huf fman codes only take 20 bits .

Evaluating Huf fman codes for a given string

Total bits = 01 1 010 10 000 1 1 001 10 10 = 20 bits , which is much less compared to 32 or 64 bits.

---

## Q2: How will you write code in Java that prints the Huf fman codes for a given string?

**Answer:**

The key steps are 1) Evaluate the frequency of each character 2) Create a binary T ree of nodes and leaves. Each parent node is the sum of the frequencies of
child nodes & leaves 3) Traverse down the tree and assign binary codes of “0” for the LHS and “1” for the RHS.
Step 1: Define the abstract base class that is comparable and maintains the state of frequencies.
Step 2: The tree node that can contain other nodes or leaves. It contains LEFT and RIGHT node or leaf. Also, note that left and right node/leaf frequencies ans
summed in the constructor .package com.test.huffman ;
public abstract class HTree implements Comparable <HTree> {
 private int frequencies ;
 public HTree(int frequencies ) {
 super ();
 this.frequencies = frequencies ;
 }
 
 public int compareT o(HTree o) {
 return this.frequencies - o.frequencies ;
 }
 public int getFrequencies () {
 return frequencies ;
 }

Step 3: The leaf that maintains the state of the character read from the given input string.package com.test.huffman ;
public class HNode extends HTree {
 
 private HTree l, r;
 
 public HNode (HTree l, HTree r) {
 super (l.getFrequencies () + r.getFrequencies ());
 this.l = l;
 this.r = r;
 }
 public HTree getL() {
 return l;
 }
 public void setL(HTree l) {
 this.l = l;
 }
 public HTree getR () {
 return r;
 }
 public void setR(HTree r) {
 this.r = r;
 }
 public void setL(HNode l) {
 this.l = l;
 }
 public void setR(HNode r) {
 this.r = r;
 }

Step 4: The value object (i.e. POJO) that keep track of each character and its frequency before adding to the tree.package com.test.huffman ;
public class HLeaf extends HTree {
 private final char c;
 public HLeaf (int frequencies , char c) {
 super (frequencies );
 this.c = c;
 }
 public char getC () {
 return c;
 }
package com.test.huffman ;
public class CharFrequency {
 private char character ;
 private int frequency ;
 public CharFrequency (char character , int frequency ) {
 super ();
 this.character = character ;
 this.frequency = frequency ;
 }

Step 5: The processor interface and implementation class.
Build the tree and construct the frequencies. public char getCharacter () {
 return character ;
 }
 public void setCharacter (char character ) {
 this.character = character ;
 }
 public int getFrequency () {
 return frequency ;
 }
 public void setFrequency (int frequency ) {
 this.frequency = frequency ;
 }
 @Override
 public String toString () {
 return "CharFrequency [character=" + character + ", frequency="
 + frequency + "]";
 }
package com.test.huffman ;
public interface HProcessor {
 void process (String input );
}

package com.test.huffman ;
mport java.util.ArrayDeque ;
mport java.util.ArrayList ;
mport java.util.Collections ;
mport java.util.Comparator ;
mport java.util.Deque ;
mport java.util.HashMap ;
mport java.util.List;
mport java.util.Map;
public class HProcessorImpl implements HProcessor {
 public void process (String input ) {
 if(input == null || input .length () == 0) {
 throw new IllegalAr gumentException ("Invalid Input: " + input );
 }
 
 Map<Character , CharFrequency > charFreq = constructFrequencies (input );
 
 HTree tree = buildT ree(charFreq );
 
 System .out.println ("Character" + "\t" + "freequency" + "\t" + "Huf fman code" );
 
 evaluateHuf fmanCodeAndPrint (tree, new StringBuilder ());
 
 }
 
 private void evaluateHuf fmanCodeAndPrint (HTree tree, StringBuilder huffmanCodes ) {
 if (tree instanceof HLeaf ) {
 HLeaf leaf = (HLeaf )tree;
 // print out character , frequency , and code for this leaf (which is just the huf fmanCodes)
 System .out.println (leaf.getC () + "\t\t " + leaf.getFrequencies () + "\t\t " + huffmanCodes );
 } else if (tree instanceof HNode ) {
 HNode node = (HNode )tree;
 // traverse left

 huffmanCodes .append ('1');
 evaluateHuf fmanCodeAndPrint (node .getL(), huffmanCodes );
 huffmanCodes .deleteCharAt (huffmanCodes .length ()-1);
 // traverse right
 huffmanCodes .append ('0');
 evaluateHuf fmanCodeAndPrint (node .getR (), huffmanCodes );
 huffmanCodes .deleteCharAt (huffmanCodes .length ()-1);
 }
 
 }
 private HTree buildT ree(Map<Character , CharFrequency > charFreq ) {
 
 List<CharFrequency > frequencies = new ArrayList <CharFrequency >(charFreq .values ());
 Collections .sort(frequencies , new Comparator <CharFrequency >() {
 public int compare (CharFrequency o1, CharFrequency o2) {
 return o1.getFrequency () - o2.getFrequency ();
 }
 });
 
 Deque <HTree> queue = new ArrayDeque <HTree>();
 
 for (CharFrequency freq : frequencies ) {
 if(freq.getFrequency () > 0) {
 queue .offer(new HLeaf (freq.getFrequency (), freq.getCharacter ()));
 }
 }
 
 // loop until there is only one tree left
 while (queue .size() > 1) {
 // two trees with least frequency
 HTree leaf1 = queue .poll();
 HTree leaf2 = queue .poll();
 // put into new node. Constructor calculates the total frequency
 queue .offer(new HNode (leaf1 , leaf2 ));
 }
 
 return queue .poll();
 }
 /**
 * Calculate frequencies of each character in the input

Step 6: Finally , the executable main class. T akes an input string via command line. Enter “QUIT” to exit the application. * @param input
 * @return
 */
 private Map<Character , CharFrequency > constructFrequencies (String input ) {
 
 char[] charArray = input .toCharArray ();
 
 Map<Character , CharFrequency > mapCharFreq = new HashMap <Character , CharFrequency >();
 
 for (char c : charArray ) {
 if(mapCharFreq .containsKey (c)) {
 CharFrequency freq = mapCharFreq .get(c);
 freq.setFrequency (freq.getFrequency () + 1);
 } else {
 CharFrequency cf = new CharFrequency (c, 1);
 mapCharFreq .put(c, cf);
 }
 }
 
 return mapCharFreq ;
 }
package com.test;
mport java.util.Scanner ;
mport com.test.huffman .HProcessor ;
mport com.test.huffman .HProcessorImpl ;
**
* Constructs a binary tree from a given String input and assigns/prints Huf fman codes. Used for encoding & decoding.

* @author arulk
*
*/
public class HuffmanMain {
 public static void main (String [] args) {
 Scanner scanner = null;
 HProcessor processor = new HProcessorImpl ();
 String input = "INIT" ;
 try {
 do {
 System .out.println ("Enter 'quit' to exit" );
 // prompt for the user's name
 System .out.print ("Enter String to evaluate Huf fman codes: " );
 scanner = new Scanner (System .in);
 input = scanner .next();
 if (input .toUpperCase ().equalsIgnoreCase ("QUIT" )) {
 System .out.println ("Quiting the App..." );
 System .exit(0);
 }
 try {
 processor .process (input );
 } catch (IllegalAr gumentException iae) {
 System .out.println (iae.getMessage ());
 } catch (Exception ex) {
 System .out.println (ex.getMessage ());
 System .exit(1);
 }
 } while (input != null);
 } finally {
 scanner .close ();
 }
 }

Output for an input string of : “BUSINESS BUDDY”
Enter 'quit' to exit
Enter String to evaluate Huffman codes : BUSINESS BUDDY
Character freequency Huffman code
N 1 11
S 3 10
B 1 011
U 1 010
E 1 001
 1 000
Enter 'quit' to exit
Enter String to evaluate Huffman codes :

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
