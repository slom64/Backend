### Euclidean Algorithm (using Modulo)
```java
long gcd_iterative(long a, long b) {
    while (b) {
        a %= b;
        swap(a, b);
    }
    return a;
}
```