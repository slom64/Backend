- Number of primes in N is:  $\frac{N}{log(n)}$

 - **Inverse of number in mod prime number**:
$$\begin{matrix} 
X^{P} \equiv X \bmod{P}\\
X^{P-1} \equiv 1 \bmod{P}\\
X.X^{P-2} \equiv 1 \bmod{P}\\
we \; know\; inverse\;is\\
X.X^{-1} \equiv 1 \bmod{P}\\
which\; mean\; X^{P-2}\;the\;inverse\\
X^{-1} \equiv X^{P-2} \bmod{P}\\
\end{matrix}$$
- Prime operations

| **Method**                | **Time Complexity**                                         | **Memory** | **Intuition**                                                                                                                                                                                                                                |
| ------------------------- | ----------------------------------------------------------- | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`sieve(n)`**            | **Const:** $O(N \log \log N)$<br>**Query:** $O(1)$          | $O(N)$     | Precomputes a bit-map/boolean array of all primes up to $N$.                                                                                                                                                                                 |
| **`factorization(n)`**    | **Const:** None<br>**Query:** **$O(\sqrt{N})$**             | $O(1)$     | No precomputation. It checks every odd number up to $\sqrt{N}$. It hits composites, but skips the `while` logic.<br>there is another implemenation for **Cons:**$O(\sqrt{N} \log \log \sqrt{N})$, **Query**:$\frac{\sqrt{n}}{\log \sqrt{n}}$ |
| **`getFactorization(n)`** | **Const:** $O(N \log \log N)$<br>**Query:** **$O(\log N)$** | $O(N)$     | Precomputes the Smallest Prime Factor (SPF) for every number up to $N$.                                                                                                                                                                      |
