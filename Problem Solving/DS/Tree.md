- Uses Red-Black trees.
- Construction time **O(NlogN)**
- **Search/Insert/Delete:** **O(logN)**.



**With the help of** [**TreeSet**](https://www.geeksforgeeks.org/java/treeset-in-java-with-examples/) **(Implements the SortedSet interface, Objects are stored in a sorted and ascending order).**
```java
// Java program to demonstrate working of TreeSet

import java.util.*; 
  
class TreeSetDemo { 
    public static void main(String[] args) 
    { 
        TreeSet<String> ts1 = new TreeSet<String>(); 
  
        // Elements are added using add() method 
        ts1.add("A"); 
        ts1.add("B"); 
        ts1.add("C"); 
  
        // Duplicates will not get insert 
        ts1.add("C"); 
  
        // Elements get stored in default natural 
        // Sorting Order(Ascending) 
        System.out.println("TreeSet: " + ts1); 

        // Checking if A is present or not
        System.out.println("\nTreeSet contains A or not:"
                           + ts1.contains("A"));

        // Removing items from TreeSet using remove()
        ts1.remove("A");

        // Printing the TreeSet
        System.out.println("\nTreeSet after removing A:" + ts1);

        // Iterating over TreeSet items
        System.out.println("\nIterating over TreeSet:");
        Iterator<String> i = ts1.iterator();
        while (i.hasNext())
            System.out.println(i.next());
    }
}

TreeSet: [A, B, C]

TreeSet contains A or not:true

TreeSet after removing A:[B, C]

Iterating over TreeSet:
B
C
```