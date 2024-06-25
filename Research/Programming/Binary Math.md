---
Author(s): M-Nelly
Source: Internal
Subject: Programming
Topic: Binary Math
Description: A brief overview of binary and bitwise operations.
Comments: 
tags:
  - programming
  - compsci
---
## References
- [C++ Bitwise Operators](https://www.programiz.com/cpp-programming/bitwise-operators)
- [Python Bitwise Operators](https://realpython.com/python-bitwise-operators/)
## Binary Intro
Binary is a base two numbering system where each "bit" can have a value of either zero (0) or one (1). A byte is made up of eight (8) bits. Each byte has a total of 256 or 2<sup>8</sup> possible combinations. Thinking in numerical representation of a byte, you can assign each possible byte a number between 0 and 256. The least significant bit has a value of 2<sup>0</sup> and the most significant has a value of 2<sup>7</sup>. 

An unsigned integer (unsigned means it can only be positive) with the value of 519 could be represented in binary as `00000010 00000111`. This would put the theoretical maximum two byte integer at a value of 2<sup>16</sup> or 65,535. For the purposes of this document, I will be focusing on 8-bit, unsigned integers.

[[Signed Integers]] follow a rule known as [Two's Compliment](https://en.wikipedia.org/wiki/Two%27s_complement) to allow positive and negative integers to be added using the same bitwise addition. 
## Decimal to Binary Conversion:
Most Significant Bit = Leftmost Bit
Least Significant Bit = Rightmost Bit

Least Significant Bit = 2<sup>0</sup> = 1
Second Significant Bit = 2<sup>1</sup> = 2
Third Significant Bit = 2<sup>2</sup> = 4
Fourth Significant Bit = 2<sup>3</sup> = 8
Fifth Significant Bit = 2<sup>4</sup> = 16
Sixth Significant Bit = 2<sup>5</sup> = 32
Seventh Significant Bit = 2<sup>6</sup> = 64
Eighth Significant Bit = 2<sup>7</sup> = 128

| 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   | Total |
| --- | --- | --- | --- | --- | --- | --- | --- | ----- |
| 1   | 1   | 0   | 0   | 1   | 0   | 1   | 1   | 203   |
| 0   | 0   | 1   | 0   | 1   | 0   | 0   | 0   | 40    |
| 0   | 0   | 0   | 1   | 0   | 1   | 0   | 1   | 21    |
Formula: 2<sup>n-1</sup> where n is the number of positions from the right.

Example: 155 -> Binary

1. Start with the most significant bit as a 1 and all others zero.
	`1 0 0 0 0 0 0 0` = 128

2. Add next most significant bit and compare total to 155
	`1 1 0 0 0 0 0 0` = 192

3. If the total is greater than 155, change the bit to a zero and 
	move on to the next.
	`1 0 1 0 0 0 0 0` = 140

4. Repeat steps 2 & 3 until you reach 155.
	`1 0 1 1 0 0 0 0` = 156
	`1 0 1 0 1 0 0 0` = 148
	`1 0 1 0 1 1 0 0` = 152
	`1 0 1 0 1 1 1 0` = 154
	`1 0 1 0 1 1 1 1` = 155
## Bitwise Operations
Bitwise operators work at the bit level. This means that rather than working on the logical representation of the data they are acting on, they work on the binary representation of that data. 
### Comparison Operators
Comparison operators take two bytes as arguments. 

| Logic | Operator | Description                                          |
| :---: | :------: | :--------------------------------------------------- |
|  AND  |  **&**   | Returns 1 if both bits are 1, otherwise returns 0    |
|  OR   |  **\|**  | Returns 1 if either bit is 1, otherwise returns 0    |
|  XOR  |  **^**   | Returns 1 if one bit or the other is 1, but not both |
### Modification Operators
Modification operators act on a single byte. 

|    Logic    | Operator | Description                                          |
| :---------: | :------: | :--------------------------------------------------- |
|     NOT     |  **~**   | Flips bit values of each bit in the byte             |
| SHIFT LEFT  |  **<<**  | Shifts bit pattern left and pads with zeroes         |
| SHIFT RIGHT |  **>>**  | Shifts bit pattern right and drops the rightmost bit |
