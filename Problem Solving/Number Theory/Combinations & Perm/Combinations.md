We have:
- \( n \) positions (or trials)
- Each position: **W** (white) or **R** (red), equally likely (\( p = 0.5 \))
- Outcomes are equally likely ordered sequences (like coin flips)
- But sometimes we ask probability for **counts** of white balls, not caring about order

---

## **2. Event: "Exactly \( k \) white balls" (order doesn’t matter)**

The probability that *exactly* \( k \) positions have white balls is:
$$
P(\text{exactly } k \text{ white}) = \frac{\binom{n}{k}}{2^n}
$$
Reason:  
- Total possible ordered outcomes = \( 2^n \)
- Number of outcomes with exactly \( k \) white balls = \( \binom{n}{k} \) (choose which \( k \) positions are white)
- All outcomes equally likely

---

## **3. Specific examples**

**Case \( n=2 \), \( k=2 \) (both white):**

$$
P = \frac{\binom{2}{2}}{2^2} = \frac{1}{4}
$$

**Case \( n=2 \), \( k=1 \) (one white, one red):**

$$
P = \frac{\binom{2}{1}}{2^2} = \frac{2}{4} = \frac12
$$

## **What if balls are not equally likely?** (e.g., \( p \neq 0.5 \))

Then it's binomial probability:

$$
P(\text{exactly } k \text{ white}) = \binom{n}{k} p^k (1-p)^{n-k}
$$

Where \( p \) = probability of white in one position.
