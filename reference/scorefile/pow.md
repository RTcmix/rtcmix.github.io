---
title: pow()
layout: ref
---

## pow

Exponentiate one argument by the other.

-----

### Synopsis

val = **pow**(*base, exponent*)

-----

### Description

**pow** returns the value of *base* raised to the *exponent* power,
i.e.:

```
    base<sup>exponent</sup>
```

-----

### Arguments

  - <span id="item_base">*base*</span>  
      
    The base of the exponential expression to be evaluated

  - <span id="item_exponent">*exponent*</span>  
      
    The exponent of the exponential expression to be evaluated

-----

### Examples

```cpp
   val = pow(3.5, 3)
   val = pow(4, 0.7)
   val = pow(7.8, 2.597)
```

-----

### See Also

[abs](abs.html), [log](log.html), [max](max.html), [min](min.html),
[mod](mod.html), [round](round.html), [trunc](trunc.html),
[wrap](wrap.html)
