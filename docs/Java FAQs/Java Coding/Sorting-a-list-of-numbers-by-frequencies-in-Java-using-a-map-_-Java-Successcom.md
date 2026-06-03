# Sorting a list of numbers by frequencies in Java using a map   Java Success.com

## Table of Contents

PROBLEM to solve: Sort a a list of numbers by frequency of their occurrences. For example
INPUT : [ 5, 3, 7, 7, 7, 5, 4, 8 ]
OUTPUT : [7, 7, 7, 5, 5, 3, 4, 8]
ALGORITHM to use
1. Group the list by count. Y ou can do this by creating a Map object where the key is the number , and the value is the count. This will have the complexity of
O(n).
2. WIth the previous map where the numbers are grouped by counts, sort the “map keys” by its “value”. The key is the number itself and the value is the number
of occurrences. This will have the complexity of the Collections.sort(…), which depends on the Java version. The Java 7 onwards uses the T imSort algorithm
with O(n) and Java 6 uses the mer ge sort algorithm with O(n log(n)).
3. Loop through the sorted keys and add the numbers to an output list. If a number is repeated 3 times, add it 3 times. This will have a complexity of O(n).
Java code with the above logic
The above three logical steps are encapsulated in their own private methods like – groupByCount(…), sortByCount(…), and expandT oAList(…..).

Sort elements by frequency in Java
package com.mytutorial ;

mport java.util.ArrayList ;
mport java.util.Collections ;
mport java.util.Comparator ;
mport java.util.HashMap ;
mport java.util.List;
mport java.util.Map;
public class SortByFrequency {
 public static void main (String [] args) {
 Integer [] input = new Integer [] { 5, 3, 7, 7, 7, 5, 4, 8 };
 SortByFrequency sbf = new SortByFrequency ();
 List<Integer > output = sbf.startSortingByFrequency (input );
 System .out.println (output ); //
 }
 public List<Integer > startSortingByFrequency (Integer [] input ) {
 if (input == null) {
 throw new IllegalAr gumentException ("Input cannot be null" );
 }
 Map<Integer , Integer > groupedByCount = groupByCount (input );
 return sortByCount (groupedByCount );
 }
 /**
 * O(N)
 *
 * @param input
 * @return
 */
 private Map<Integer , Integer > groupByCount (Integer [] input ) {
 Map<Integer , Integer > counter = new HashMap <>();
 for (Integer in : input ) {
 counter .put(in, counter .containsKey (in) ? counter .get(in) + 1 : 1);
 }
 System .out.println (counter ); // {3=1, 4=1, 5=2, 7=3, 8=1}
 return counter ;
 }
 /**
 * Sorting is O(N) T imSort in Java 7 onwards & N (LOG N) for Java 6 mer ge

 * sort Converting back to a list: O(N)
 *
 * @param groupedByCount
 * @return
 */
 private List<Integer > sortByCount (Map<Integer , Integer > groupedByCount ) {
 List<Integer > keys = new ArrayList <Integer >(groupedByCount .keySet ());
 System .out.println (keys); // [3, 4, 5, 7, 8]
 // Anonymous inner class for sorting
 Collections .sort(keys, new Comparator <Integer >() {
 @Override
 public int compare (Integer o1, Integer o2) {
 // use the count from the value to sort the keys
 return groupedByCount .get(o2) - groupedByCount .get(o1);
 }
 });
 // keys are sorted by values
 System .out.println (keys); // e.g. [7, 5, 3, 4, 8]
 return expandT oAList (groupedByCount , keys);
 }
 private List<Integer > expandT oAList (Map<Integer , Integer > groupedByCount , List<Integer > sortedKeys ) {
 // Expand the keys by number of occurrences
 List<Integer > out = new ArrayList <Integer >(10);
 for (Integer key : sortedKeys ) {
 Integer count = groupedByCount .get(key);
 int i = 0;
 while (i < count ) {
 out.add(key);
 ++i;
 }
 }
 
 return out;
 }

Output:
{3=1, 4=1, 5=2, 7=3, 8=1}
[3, 4, 5, 7, 8]
[7, 5, 3, 4, 8]
[7, 7, 7, 5, 5, 3, 4, 8]
NEXT : BST to sort elements by frequencies in Java



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
