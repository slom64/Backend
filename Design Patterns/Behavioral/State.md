- **If you found your self checking for the state of the objects multiple of times in different methods**, then you should use state design pattern. 
- As example below shows, we are checking for the state of the object in `mouseDown`, `mouseUp` and maybe there is keyboard action too which mean we should checkt state again.
  So the more tools you support the longer the decision making are going to be.
	- And if we want to add another tool, we should go for every place we check the state of the object and add the new tool.
- **Solution**: Based on the variables that you use to take decision, create an interface or abastract class, which get implemented for each state class.
	- This is an example of **Open/Closed principale**
		- The code is **Open** for extension "adding new features"
		- But **Closed** for modification. The current methods can't be changed.

**Problem**
```java
public class Canvas{
	Private ToolType currentTool;
	
	public void mouseDown(){
		if(currentTool == ToolType.SELECTION)
			System.out.println("Selection Icon");
		else if(currentTool == ToolType.BRUSH)
			System.out.println("Brush Icon");
		else if(currentTool == ToolType.ERASER)
			System.out.println("Eraser Icon");
	}
	
	public void mouseUp(){
		if(currentTool == ToolType.SELECTION)
			System.out.println("Draw dashed rectangle");
		else if(currentTool == ToolType.BRUSH)
			System.out.println("Draw a line");
		else if(currentTool == ToolType.ERASER)
			System.out.println("Erase something");
	}
}
```

**Solution**
```java
public interface Tool{
	void mouseDown();
	void mouseUp();
}

public class SelectionTool implements Tool{
	public void mouseDown(){System.out.println("Selection Icon");}
	public void mouseUp(){System.out.println("Draw dashed rectangle");}
}...etc...

public class Canvas{
	public Tool tool;
	public void mUp(){tool.mouseUp();}
	public void mDown(){tool.mouseDown();}
}

public class Main{
	public static void main(String[] args){
		var canvas = new Canvas();
		canvas.tool = new SelectionTool();
		canvas.mDown();
		canvas.mUp();
	}
}
```

![[StateDesignPattern|700]]