# Collatz sequence (or Hailstone sequence)

*18 Jun 2019*

---

The Collatz conjecture is one of those beautifully simple unsolved problems in mathematics. Take any positive integer n. If it's even, divide it by 2. If it's odd, multiply by 3 and add 1. Repeat. The conjecture says you'll always eventually reach 1.

The sequence gets its nickname "Hailstone" because the numbers rise and fall unpredictably, like hailstones in a cloud, before eventually settling down.

**The rules:**

```
if n is even  ->  n / 2
if n is odd   ->  3n + 1
repeat until n = 1
```

**Example:** Starting with 6:

```
6 -> 3 -> 10 -> 5 -> 16 -> 8 -> 4 -> 2 -> 1
```

I wrote a simple Python function to generate and visualise these sequences. What's interesting is how wildly different starting numbers can behave — some reach 1 quickly, others take hundreds of steps, rocketing up to enormous values before crashing back down.

```python
def collatz(n):
    sequence = [n]
    while n != 1:
        if n % 2 == 0:
            n = n // 2
        else:
            n = 3 * n + 1
        sequence.append(n)
    return sequence
```

Despite being so easy to state and implement, no one has been able to prove the Collatz conjecture holds for all numbers. Paul Erdos famously said: "Mathematics is not yet ready for such problems."

The full notebook with visualisations is on my GitHub.
