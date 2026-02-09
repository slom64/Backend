- A stream is a flow of data between a data source and a data sink (Distination).
- Stream could be used for input or ouput.
- java implements streams within class hierarchies defind in the java.io package.
- java defines two types of streams: `byte` and `character`.
	- `Byte` streams are more appropriate, when readin or writing binary data, our unit is 1 byte.
	- `character` used when we are sure we will read characters "2 bytes" , in some cases character streams are more efficient than byte streams.

---
## Byte Stream Classes
- Byte streams are defined by using two classes hierarchies. At the top are two abstract classes: `InputStream` and `OutputStream` 
- Each abstract class has severl subclasses that handle the differences among various devices, such as disk files, network connections, and even memory buffers.
- Those two abstract classes define several key methods that the other stream classes implements, Example
	- `read()`
	- `write()`

---
## Character Streams Classes
- Character streams are defined by using two class hierarchies. At the top are two abstract classes: `Reader` and `Writer`
	- Two important methods of those classes are `read()` and `write()`
- They operate with character which is 2 bytes.

---
## Predefined Streams
- Java programs automatically import the `java.lang` package, this package defines the **System** class, which encapsulates several aspects of the run-time env.
- Class **System** contain three predefined variables: **in**, **out** and **err**. This fields are declared as **public**, **static** and **final** within **System**. So you didn't need to create an object to use it.
- **System.in** is an object of type **InputStream**, **System.out** and **System.err** are objects of type **PrintStream**.