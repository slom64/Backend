- If we have logic that iterate throw elements of class, there will be no problem to change code inside the class when we change how we store elements. The main issue if we iterate throw elements outside the class, so when we change how we store data, outer classes may break.
- So, from the outside we only need `.hasNext()`, `.current()`, `.next()`.
- best practice to implement the custom iterator class inside as inner class of the main class

![[IteratorDesignPattern|800x400]]

```java
// BrowseHistor.java
public class BrowseHistory {  
    private final ArrayList<String> history = new ArrayList<>();  
	      
    public void add(String s) {  
        history.add(s);  
    }  
	  
    public Iterator<String> iterator() {  
        return new ListBrowseHistoryIterator(this);  
    }  
	  
    // Inner class for defining the Iterator  
    static public class ListBrowseHistoryIterator implements Iterator<String> {  
	  
        BrowseHistory browseHistory;  
        int index;  
		  
        ListBrowseHistoryIterator(BrowseHistory browseHistory) {  
            this.browseHistory = browseHistory;  
            index = 0;  
        }  
		  
        @Override  
        public boolean hasNext() {  
            return index != browseHistory.history.size(); // you can access private fields in parent class.   
        }  
		  
        @Override  
        public String next() {  
            String o = browseHistory.history.get(index);  
            index++;  
            return o;  
        }  
    }  
}
// Main.java
public class Main {  
    public static void main(String[] args) {  
  
        BrowseHistory browseHistory = new BrowseHistory();  
        browseHistory.add("a1");  
        browseHistory.add("a2");  
        browseHistory.add("a3");  
        browseHistory.add("a4");  
        Iterator<String> iterator = browseHistory.iterator();  
        while(iterator.hasNext()){  
            String s = iterator.next();  
            System.out.println(s);  
        }  
    }  
}
```