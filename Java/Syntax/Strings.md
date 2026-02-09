- Strings are immutable, that means you can't change the object string value. so when you change it you are creating new object.

```java 
String str1 = new String("hi");
String str2 = "hi"; // create the string inside the string pool. don't use it.

// if you want to compare there values don't do this:
if( str1 == str2) // XXXXXXX wrong XXXXXXX. this compare there references "pointers"
if(str1.equals(str2))
```