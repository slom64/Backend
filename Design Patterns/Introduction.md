- Design patterns follows `SOLID` pillars:
	- S: **Signle responsibility**
		- We can identify this by asking if what does the class do? if we enconter an answer like "this class do 1 **AND** 2" then we violate this rule.
		- The Solution of this is to create another class that do the additional missions.
	- O: **Open Closed**
		- Any class or interface shouldn't be open for modification so the existance code don't change, You should make intermediate interface that all classes inherit it.
- **Coupling**: means how much a class is **dependent** on another class. if we changed a very small thing in the dependent on class, all other classes that depend on it will have to be recompiled even if nothing changed on those classes. (ex. changing single character in stdout of the dependent on class). To get **loosely coupled** classes we will use interfaces.

---
### Open Closed
![[OpenClosedDesignPattern|700]]


---
### Types
- Creational
	- How to create object
- Structural
	- Classes that are used inside another class
- Behavioral
	- interaction between different classes