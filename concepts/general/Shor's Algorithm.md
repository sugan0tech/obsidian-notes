### Shor's Algorithm - Formula and Probability Metrics

1. **Random g selection:**

   A random number \( g \) is chosen.

   ```
   g → g^p/2 ± 1
   ```

   This happens when \( g^p/2 ± 1 \) is not a multiple of \( N \).

   - **Probability**: 37.5% of the time.

2. **Further iterations:**

   After selecting random \( g \), it is tested repeatedly:

   ```
   g^2, g^3, g^4, g^5 ... g^16
   ```

   - **Success Probability**: 99% of the time.

3. **Factorization step:**

   The process involves calculating repeated powers of \( g \), seeking a pattern where:

   ```
   g . g . g ... g (p times) = mN + 1
   ```

   This helps to find \( p \) such that:

   ```
   g^p/2 ± 1
   ```

---

### Notes:

- Shor's algorithm is probabilistic and succeeds in factoring large numbers with very high probability (99%).
- Key steps involve testing a sequence of values derived from a random \( g \), and through repeated operations, finding factors of \( N \).

### References
[How Quantum Computers Break Encryption | Shor's Algorithm Explained](https://www.youtube.com/watch?v=lvTqbM5Dq4Q)
