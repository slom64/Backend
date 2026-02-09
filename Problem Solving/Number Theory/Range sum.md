
$$
sum = \frac{L + R}{2} \times {(R  -L + 1)}
$$

### `sum(r) - sum(l-1)`:
$$
T_r - T_{l-1} = \frac{r(r+1)}{2} - \frac{(l-1)l}{2}

= \frac{r^2 + r - l^2 + l}{2} = \frac{(r-l)(r+l) + (r+l)}{2}


= \frac{(r+l)(r-l+1)}{2} = \frac{l+r}{2} \times (r-l+1)

$$

**This matches exactly!**

