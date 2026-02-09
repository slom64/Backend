- You can solve it using `bottom up` (loop) or `top down`(recursive).
	- Using `bottom up` makes you need to save all the possiblities values. This is good if you are sure you will need all the possiblitites answers and doesn't have recuresion overhead.
		- you will need to start from the base case then go towards n. 0 --> N.
	- Using `top down` saves only the values that you need, but there is recursion overhead time on function calls, extra storage used in stack information.
- You should save the current state of the solution like fib(X,Y). which effects the dimension of the array that you will use. or you can use dictionary that combine all the arguments.



```
knapsack
Max benefit = KS(Wight, N) # N is index represnt item id.
```