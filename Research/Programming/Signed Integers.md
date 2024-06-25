---
Author(s): M-Nelly
Source: Internal
Subject: Programming
Topic: Signed Integers
Description: Overview of signed integers and Two's Complement
Comments: 
tags:
  - programming
  - compsci
---
## Introduction
When learning [[Binary Math]], you will very often start with unsigned integers. These are integers that are always positive. There is; however, a need for signed integers (those with positive or negative values). There are a few solutions to this problem with the most widely accepted solution being [Two's Compliment](https://en.wikipedia.org/wiki/Two%27s_complement). While you certainly could jump right into learning Two's Compliment, it is recommended to step through the logic outlined in this document to understand what it is and where it comes from. 
## Sign Bit/Sign Column
One method for representing negative numbers is to use the most significant bit to represent a sign value. This approach has a few issues, but it is important to understand why this doesn't work. An example using 4 bit numbers would be `7 = 0111` and `-7 = 1111`. This removes the ability to count to 8 using 4 bits. It also does not allow binary addition of negative numbers. An example gives us `7 + (-5) = 4` plus an overflow bit in the theoretical fifth position:
$$
\begin{array}{c}
&\ 0111&\ \ \ 7\\
&\ 1101&-5\\
\hline
&0100&\ \ \ 4\\
\end{array}
$$
Regardless of understanding the binary math involved, it is easy to see that this is not correct and therefore would lead to issues unless additional logic was created to handle these negative numbers. 
## One's Compliment
A much better solution would be to invert the number such that when the two binary numbers are added, the correct value is subtracted. Enter One's Compliment where we flip the bits of the number we want the negative value of, so `5 = 0101` and `-5 = 1010`. Example:
$$
\begin{array}{c}
&\ 0101&\ \ \ 5\\
&\ 1010&-5\\
\hline
&1111&-0\\
\end{array}
$$
Remember that in One's Compliment, we are still using the most significant bit as a sign bit. Meaning a four bit number would have a minimum of `-7` and a maximum of `+7`. This also means that `0000 = 0` and `1111 = -0`. This is problematic because it means that we have two values for zero and, more interestingly, when adding a positive number to a negative number that is not its equal opposite, the resulting value is off by a value of `-1`. Example:
$$
\begin{array}{c}
&\ 0101&\ \ \ 5\\
&\ 1101&-2\\
\hline
&0010&\ \ \ 2\\
\end{array}
$$
## Two's Compliment
The solution to this can be found in [Two's Compliment](https://en.wikipedia.org/wiki/Two%27s_complement). The first step in finding Two's Compliment is to find One's Compliment by flipping each bit in the number to its opposite, so `7` `0111` is turned into `1000`. Next we must add one (`0001`) to that value  resulting in `1001`. If we add the two's compliment to the original number, we should get zero every time. 
$$
\begin{array}{c}
&0111&\ \ \ 7\\
+&1001&-7\\
\hline
&0000&\ \ \ 0\\
\end{array}
$$
Additionally, if we add the two's compliment of a number to any other number, we should get the correct resultant value: 
$$
\begin{array}{c}
&0101&\ \ \ 5\\
+&1110& -2\\
\hline
&0101&\ \ \ 3\\
\end{array}
$$
## Conclusion
An easy way to think about this is that the sign column or sign bit is the same significance as it would be in an unsigned number, but negative. The table below shows binary values and their associated decimal values:

| -8  |  4  |  2  |  1  | Decimal |     | -8  |  4  |  2  |  1  | Decimal |
| :-: | :-: | :-: | :-: | ------- | --- | :-: | :-: | :-: | :-: | ------- |
|  1  |  0  |  0  |  0  | *-8*    |     |  0  |  0  |  0  |  0  | *0*     |
|  1  |  0  |  0  |  1  | *-7*    |     |  0  |  0  |  0  |  1  | *1*     |
|  1  |  0  |  1  |  0  | *-6*    |     |  0  |  0  |  1  |  0  | *2*     |
|  1  |  0  |  1  |  1  | *-5*    |     |  0  |  0  |  1  |  1  | *3*     |
|  1  |  1  |  0  |  0  | *-4*    |     |  0  |  1  |  0  |  0  | *4*     |
|  1  |  1  |  0  |  1  | *-3*    |     |  0  |  1  |  0  |  1  | *5*     |
|  1  |  1  |  1  |  0  | *-2*    |     |  0  |  1  |  1  |  0  | *6*     |
|  1  |  1  |  1  |  1  | *-1*    |     |  0  |  1  |  1  |  1  | *7*     |