---
title: chance()
layout: ref
---

## chance

Return true or false based on the fractional odds specified by the arguments.

-----

### Synopsis

odds = **chance**(*numerator*, *denominator*)

-----

### Description

**chance** is a short-hand utility to allow a logical choice based on a fractional probability.  This probability is calculated by dividing the first argument by the second.  **chance** uses the [random](random.html) function internally to generate the weighted values.

-----

### Arguments

  - *numerator*  
    Any non-negative number, but usually an integer, representing the "X" in "X in Y odds".
  - *denominator*  
    Any positive number, but usually an integer, representing the "Y" in "X in Y odds".

-----

### Examples

```cpp
// Simulate 100 coin flips
for (i = 0; i < 100; ++i) {
	heads = chance(1, 2);	// "1 in 2" chance, or 50%
	if (heads) {
		printf("Coin came up heads\n");
	}
	else {
		printf("Coin came up tails\n");
	}
}
```

-----

### See Also
[rand](rand.html), [random](random.html), [srand](srand.html)

