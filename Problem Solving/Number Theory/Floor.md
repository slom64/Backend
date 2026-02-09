## Number of distinct integer values of floor in range N divided by X, where 1<= X<= N.
- Ex N=20  
	- `2*root(20) = 8`  Values 20, 10, 5, 4, 2,1 which is 6 <= 8 
$$
Number \  of\ distinct\ values\ of\  \lfloor \frac{N}{i} \rfloor \ is \  \leq 2\sqrt{N}
$$

---

## Determine range of values `i` that give me x while floor(N,i)
$$
x =\lfloor \frac{N}{i} \rfloor
$$
- Example. what is the range `i` that will give me `2` while `N=10`. 

$$
2 \le \frac{10}{i} < 3  
$$

$$  
\frac{10}{3} < i \le \frac{10}{2}  
$$

$$
3.33 < i \le 5  
$$


 and if `i` is integer values only
 $$4<i≤5$$
 How?



We want **all `i` such that**:

$$
\left\lfloor \frac{N}{i} \right\rfloor = q  \\ 
$$
  
$$q \le \frac{N}{i} < q+1$$  
$$\frac{N}{q+1} < i \le \frac{N}{q}$$


---
