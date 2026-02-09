| Divisibility by Number                                                              | Divisibility Rule                                                                                                            |
| ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| [Divisibility by 2](https://www.geeksforgeeks.org/maths/divisibility-rule-of-2/)    | The last digit should be even (0, 2, 4, 6, or 8).                                                                            |
| [Divisibility by 3](https://www.geeksforgeeks.org/maths/divisibility-rule-of-3/)    | The sum of the digits should be divisible by 3.                                                                              |
| [Divisibility by 4](https://www.geeksforgeeks.org/aptitude/divisibility-rule-of-4/) | The number formed by the last two digits should be divisible by 4. Ex. 3012 --> 12 --> divisible                             |
| [Divisibility by 5](https://www.geeksforgeeks.org/maths/divisibility-rule-of-5/)    | The last digit should either be 0 or 5.                                                                                      |
| [Divisibility by 6](https://www.geeksforgeeks.org/maths/divisibility-rule-of-6/)    | The number should be divisible by both 2 and 3.                                                                              |
| [Divisibility by 7](https://www.geeksforgeeks.org/maths/divisibility-rule-of-7/)    | The double of the last digit, when subtracted from the rest of the number, the difference obtained should be divisible by 7. |
| [Divisibility by 8](https://www.geeksforgeeks.org/maths/divisibility-rule-of-8/)    | The number formed by the last three digits should be divisible by 8.                                                         |
| [Divisibility by 9](https://www.geeksforgeeks.org/maths/divisibility-rule-of-9/)    | The sum of the digits should be divisible by 9.                                                                              |
| [Divisibility by 10](https://www.geeksforgeeks.org/maths/divisibility-rule-of-10/)  | The last digit should be 0.                                                                                                  |
| [Divisibility by 11](https://www.geeksforgeeks.org/maths/divisibility-rule-of-11/)  | The difference of the alternating sum of digits should be divisible by 11.                                                   |
| [Divisibility by 12](https://www.geeksforgeeks.org/maths/divisibility-rule-of-12/)  | The number should be divisible by both 3 and 4.                                                                              |
| [Divisibility by 13](https://www.geeksforgeeks.org/maths/divisibility-rule-of-13/)  | The four times of the last digit, when added to the rest of the number, the result obtained should be divisible by 13.       |
| [Divisibility by 14](https://www.geeksforgeeks.org/maths/divisibility-rule-for-14/) | Upon adding the last two digits to twice the sum of the remaining digits, the result should be divisible by 14               |
| [Divisibility by 15](https://www.geeksforgeeks.org/maths/divisibility-rule-for-15/) | The number should be divisible by both 5 and 3.                                                                              |
| [Divisibility by 16](https://www.geeksforgeeks.org/maths/divisibility-rule-of-16/)  | The last four digits should be divisible by 16.                                                                              |
| [Divisibility by 17](https://www.geeksforgeeks.org/maths/divisibility-rule-of-17/)  | Five times the last digit, when subtracted from the rest of the number, should be divisible by 17.                           |
| [Divisibility by 19](https://www.geeksforgeeks.org/maths/divisibility-rule-of-19/)  | Double the last digit, and add it to the rest of the number. If the result is divisible by 19, so is the original number.    |

---

## Number of divisible integers on N in range i
- divisible numbers of 2 in 6 --> 2,4,6 -> 3
- divisible numbers of 4 in 6 --> 4 -> 1
- divisible numbers of 3 in 9 --> 3,6,9 -> 3
Equation
```
floor[ i / N ]
[ 6 / 2 ] = 3
[ 6 / 5 ] = 1
[ 9 / 3 ] = 3
```

This can be used in the sum of divisible integers in range i

---
## sum of divisible integers in range i
O(sqrt(n))
Rules
- Number of distinct integer values of floor of range N divided by X, where 1<= X<= N. 
	- Ex N=20  
		- `2*root(20) = 8`  Values 20, 10, 5, 4, 2,1 which is 6 <= 8 
$$
Number \  of\ distinct\ values\ of\  \lfloor N/i \rfloor \ is \  \leq 2\sqrt{n}
$$
- Range of integer values i that give me x while x = floor(N,i)
$$\frac{N}{q+1} < i \le \frac{N}{q}$$

The main idea = Unique_divisible_values X * Range_of_its_occurance

$$
\sum_{i=1}^{N} i \left\lfloor \frac{N}{i}  \right\rfloor =
\sum_{k=1}^{2*\sqrt N} k 
\sum_{\lfloor \frac{N}{k+1} \rfloor = i}^{\frac{N}{k}} i  
$$
The inner sum becomes an arithmetic progression:

[  
\sum_{i=\lfloor N/(k+1) \rfloor + 1}^{\lfloor N/k \rfloor} i  
]

This form is standard in (O(\sqrt{N})) algorithms.
