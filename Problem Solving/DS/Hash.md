### HashTable
- Implements Red-Black Tree when collision happens.
```java
// Java program to demonstrate working of HashTable
import java.util.*;

class GFG {
    public static void main(String args[])
    {

        // Create a HashTable to store 
        // String values corresponding to integer keys
        Hashtable<Integer, String>
            hm = new Hashtable<Integer, String>();

        // Input the values
        hm.put(1, "Geeks");
        hm.put(12, "forGeeks");
        hm.put(15, "A computer");
        hm.put(3, "Portal");

        // Printing the Hashtable
        System.out.println(hm);
    }
}
```


-  [**LinkedHashMap**](https://www.geeksforgeeks.org/java/hashmap-treemap-java/) **(Similar to HashMap, but keeps order of elements)**
```java
import java.util.*; 
  
public class BasicLinkedHashMap 
{ 
    public static void main(String a[]) 
    { 
        LinkedHashMap<String, String> lhm = 
                       new LinkedHashMap<String, String>(); 
        lhm.put("one", "practice.geeksforgeeks.org"); 
        lhm.put("two", "code.geeksforgeeks.org"); 
        lhm.put("four", "www.geeksforgeeks.org"); 
  
        // It prints the elements in same order  
        // as they were inserted     
        System.out.println(lhm); 
  
        System.out.println("Getting value for key 'one': " 
                                       + lhm.get("one")); 
        System.out.println("Size of the map: " + lhm.size()); 
        System.out.println("Is map empty? " + lhm.isEmpty()); 
        System.out.println("Contains key 'two'? "+  
                                  lhm.containsKey("two")); 
        System.out.println("Contains value 'practice.geeks"
        +"forgeeks.org'? "+ lhm.containsValue("practice"+ 
        ".geeksforgeeks.org")); 
        System.out.println("delete element 'one': " +  
                           lhm.remove("one")); 
        System.out.println(lhm); 
    } 
}

{one=practice.geeksforgeeks.org, two=code.geeksforgeeks.org, four=www.geeksforgeeks.org}
Getting value for key 'one': practice.geeksforgeeks.org
Size of the map: 3
Is map empty? false
Contains key 'two'? true
Contains value 'practice.geeksforgeeks.org'? true
delete element 'one': practice.geeksforgeeks.org
{two=code.geeksforgeeks.org, four=www.geeksforgeeks.org}
```

- **With the help of** [**HashSet**](https://www.geeksforgeeks.org/java/hashset-in-java/) **(Similar to HashMap, but maintains only keys, not pair)**
```java
// Java program to demonstrate working of HashSet
import java.util.*;

class Test {
    public static void main(String[] args)
    {
        HashSet<String> h = new HashSet<String>();

        // Adding elements into HashSet using add()
        h.add("India");
        h.add("Australia");
        h.add("South Africa");
        h.add("India"); // adding duplicate elements

        // Displaying the HashSet
        System.out.println(h);

        // Checking if India is present or not
        System.out.println("\nHashSet contains India or not:"
                           + h.contains("India"));

        // Removing items from HashSet using remove()
        h.remove("Australia");

        // Printing the HashSet
        System.out.println("\nList after removing Australia:" + h);

        // Iterating over hash set items
        System.out.println("\nIterating over list:");
        Iterator<String> i = h.iterator();
        while (i.hasNext())
            System.out.println(i.next());
    }
}

[South Africa, Australia, India]

HashSet contains India or not:true

List after removing Australia:[South Africa, India]

Iterating over list:
South Africa
India
```
- **With the help of** [**LinkedHashSet**](https://www.geeksforgeeks.org/java/linkedhashset-in-java-with-examples/) **(Similar to LinkedHashMap, but maintains only keys, not pair)**


- **With the help of** [**ConcurretHashMap**](https://www.geeksforgeeks.org/java/concurrenthashmap-in-java/)**(Similar to Hashtable, Synchronized, but faster as multiple locks are used)**


---
### Custom datatype

#### How `contains()` Works Internally
When you call `set.contains(myObject)`, the `HashSet` performs a two-step "Security Check" in memory:
1. **Step 1: The Bucket Search (`hashCode`)**: It runs the `hashCode()` method to get an integer. It uses this to find the "bucket" in the hash table. This is extremely fast (O(1)).
2. **Step 2: The Exact Match (`equals`)**: Once it finds the bucket, it uses `equals()` to check if the object you sent is _exactly_ the same as the one in the bucket.
   
	- You should override `equals()`, `hashCode()` methods.
```java
public class User {
    private int id;
    private String name;

    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o; // Downcasting to compare fields
        return id == user.id && Objects.equals(name, user.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
}
```