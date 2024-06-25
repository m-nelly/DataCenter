---
Author(s): M-Nelly
Source: Internal
Subject: Programming
Topic: Floating Point Numbers
Description: Overview of the standard used to calculate floating point numbers
Comments: 
tags:
  - programming
  - compsci
---
## References
- https://www.itu.dk/~sestoft/bachelor/IEEE754_article.pdf
- https://www.youtube.com/watch?v=gc1Nl3mmCuY
- https://www.h-schmidt.net/FloatConverter/IEEE754.html
## Notes
### Overview
In a 32 bit floating point number, the first bit is a sign bit, the next eight bits are an unsigned integer, and the remaining 23 bits are fractional values represented by 2<sup>-1</sup>, 2<sup>-2</sup>, 2<sup>-3</sup>, etc. The decimal equivalents would be 0.5, 0.25, 0.125, etc. or 1/2, 1/4, 1/8, etc. This means that most floating point numbers cannot be represented with 100% precision. 

> Squeezing infinitely many real numbers into a finite number of bits requires an approximate representation. Although there are infinitely many integers, in most programs the result of integer computations can be stored in 32 bits. In contrast, given any fixed number of bits, most calculations with real numbers will produce quantities that cannot be exactly represented using that many bits. Therefore the result of a floating-point calculation must often be rounded in order to fit back into its finite representation. This rounding error is the characteristic feature of floating-point computation. The section Relative Error and Ulps describes how it is measured. [^1]

| Sign |    Exponent     |                  Significand                  | Value  |
| :--: | :-------------: | :-------------------------------------------: | :----: |
|  0   | 1 0 0 0 0 0 1 1 | 0 0 0 1 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 | 17.125 |
Sign Bit - Controls whether the number is positive or negative
Exponent - Controls the base value which is raised to the power of the significand
Significand - Fractional value which is multiplied by the Exponent

The sign bit works exactly the same way it does in [[Signed Integers]]. 

The exponent is valued with the same logic as a normal integer with the one exception of being offset by -127. This means that `10000000 = 128` when offset is valued at 1. This number is then used as the exponent in 2<sup>x</sup>. 

The significand contains a "hidden" bit which is always on and has a value of 2<sup>0</sup>. This means that every significand has a value of 1 + some fractional value. 
### Calculation
Calculating the value of `01000001 10010000 00000000 00000000` is a bit tedious, but not impossible to do by hand. 

| Section     | Value                                         | Calculations                                  |
| ----------- | --------------------------------------------- | --------------------------------------------- |
| Sign        | 0                                             | Positive Number                               |
| Exponent    | 1 0 0 0 0 0 1 1                               | 131 - 127 = 4                                 |
| Significand | 0 0 0 1 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 | 1 + 2<sup>-4</sup> + 2<sup>-7</sup> = 137/128 |
Note that 2<sup>-4</sup> can be written as 1/16 or 8/128 and 2<sup>-7</sup> is 1/128. Adding those fractions together with 128/128 gives us 137/128. We now have to multiply the exponent by the significand: 
$$2^4\times\frac{137}{128} = \frac{2192}{128} = 17.125$$

| Section     | Value                                         | Calculations                                                     |
| ----------- | --------------------------------------------- | ---------------------------------------------------------------- |
| Sign        | 1                                             | Negative Number                                                  |
| Exponent    | 1 0 0 0  0 0 1 0                              | 130 - 127 = 3                                                    |
| Significand | 0 0 1 0 0 0 1 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 | 1 + 2<sup>-3</sup> + 2<sup>-7</sup> +2<sup>-11</sup> = 2321/2048 |
$$-1\times2^3\times\frac{2321}{2048} = \frac{18568}{2048} = -9.06640625$$

---
[^1]: [[What Every Computer Scientist Should Know About Floating-Point Arithmetic.pdf]]