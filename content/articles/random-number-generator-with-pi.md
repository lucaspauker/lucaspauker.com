---
title: "Random Number Generator with Pi"
date: 2019-08-06
draft: false
---

Recently, I wondered if I could create a random number generator using the digits of pi.

Disclaimer: this article's purpose is not to create a computationally efficient random number generator; it is just a fun proof of concept.

In order to create a random number generator with pi, we must first verify that the digits of pi are randomly distributed. This can be accomplished in a few lines of python code.

We will use the `mpmath` library, which provides high precision for constants such as pi.

```
from mpmath import mp
```

Let's write a function to find the distribution of the base 10 digits in pi.

```
def get_pi_frequency(num_digits):
    mp.dps = num_digits
    digits = range(10)
    pi = mp.pi
    frequency_dict = {}
    for digit in digits:
        frequency_dict[str(digit)] = 0
    for d in str(pi):
        if d == ".": continue  # Disregard the decimal in pi
        frequency_dict[d] += 1
    return frequency_dict
```

The `frequency_dict` that gets returned from `get_pi_frequency` will map each digit (0…9) to the number of times it occurs in `num_digits` of pi. Let's test out the function and see if the digits in pi are randomly distributed.

```
n = 100_000
print(get_pi_frequency(n))
```

Here is the output:

```
{'0': 9999, '1': 10137, '2': 9908, '3': 10026, '4': 9970, '5': 10027, '6': 10028, '7': 10025, '8': 9978, '9': 9902}
```

This looks very random by inspection - it is an even distribution. The standard deviation is 67.52, which is small compared to `n`.

Now let's jump into random number generation. The general idea is that we will write a function that takes a seed value `s` and then we will return a random list of numbers after the `s` digit of pi. For example, if the seed is 100 and we want 10 random numbers, we will return the 100th-110th digits of pi.

```
def get_random_number(k, seed=22_000):
    """
    Returns k random numbers as an array.
    """
    mp.dps = seed + k
    pi = mp.pi
    out = []
    for d in str(pi)[seed + 1:]:
        out.append(d)
    return out
```

Cool! Now we can output random numbers. However, there is one remaining issue: if we call `get_random_number` twice with the same seed, we will get the same "random" numbers. Thus, we should think of a way to systematically update the seed. One way to do it is to pick a random five-digit number to be the new seed.

```
def get_new_seed(seed):
    return int(''.join(get_random_number(5, seed)))
```

Now, we can run a test and see if we get random numbers. We will print 10 sets of 10 random numbers, updating the seed value each time.

```
seed = 22_000
for _ in range(10):
    print(get_random_number(10, seed))
    seed = get_new_seed(seed)
```

Here is the output:

```
['7', '8', '7', '0', '0', '4', '2', '5', '0', '3']
['7', '3', '2', '8', '1', '6', '0', '9', '6', '3']
['6', '8', '9', '4', '2', '8', '0', '9', '3', '1']
['1', '4', '0', '3', '8', '1', '4', '9', '8', '3']
['8', '2', '8', '3', '0', '2', '5', '8', '4', '8']
['8', '0', '3', '1', '5', '5', '9', '0', '2', '5']
['1', '7', '9', '0', '1', '2', '4', '0', '1', '9']
['7', '8', '4', '2', '6', '4', '5', '6', '7', '6']
['8', '1', '3', '1', '1', '0', '2', '4', '7', '3']
['4', '0', '3', '5', '5', '1', '8', '9', '8', '2']
```

Now we have a fully functioning pseudorandom number generator based on the digits of pi!
