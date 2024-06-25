---
Author(s): M-Nelly
Source: Internal
Subject: Programming
Topic: Pointers and References
Description: Overview of pointers and references in C
Comments: 
tags:
  - compsci
  - programming
---
## To-Do
- [ ] 
## References
- https://www.geeksforgeeks.org/pointers-vs-references-cpp/
- [YouTube: Stanford - Programming Paradigms L3](https://www.youtube.com/watch?v=H4MQXBF6FN4&list=PL9D558D49CA734A02&index=4)
- [YouTube: Low Level Learning - Pointers](https://www.youtube.com/watch?v=2ybLD6_2gKM)
- [Geeks for Geeks: Void Pointer in C](https://www.geeksforgeeks.org/void-pointer-c-cpp/)
- [GNU C Manual: Void Pointers](https://www.gnu.org/software/c-intro-and-ref/manual/html_node/Void-Pointers.html)
## Notes
[Pointers](https://www.geeksforgeeks.org/pointers-in-c-and-c-set-1-introduction-arithmetic-and-array/): A pointer is a variable that holds the memory address of another variable. A pointer needs to be dereferenced with the ******* operator to access the memory location it points to. 

[References](https://www.geeksforgeeks.org/references-in-c/): A reference variable is an alias, that is, another name for an already existing variable. A reference, like a pointer, is also implemented by storing the address of an object.   
A reference can be thought of as a constant pointer (not to be confused with a pointer to a constant value!) with automatic indirection, i.e., the compiler will apply the `*` operator for you.
### Examples
It is important to note that these examples may contain undefined behavior and might cause memory related failures if run. They are purely theoretical explanations of memory manipulation in C programming.  
#### Example 0
The following code stores the first few digits of pi (rounded up) as a double. The next operation stores the value of the first byte of that double as a character. 
```C
double d = 3.1416;

char ch = *(char *)&d;
```
Breaking down that expression from right to left, we see `&d;`. This is a reference to the memory address of `d`. It does not store the value of `d`, only its location in memory. The next bit `(char *)` is a pointer to a character. Putting those together `(char *)&d;` gives us a pointer to a two byte location in memory starting at the address of `d`. In other words, we have a pointer to the first two bytes of `d`. The last bit of this statement is a dereference which allows us to access the value stored at the pointer address. If we verbalize the statement, we get something like, "Character ch is equal to the value pointed to by this pointer which is a two byte character stored at the address of d".  

One way you could rewrite this that exposes a bit more of the process is:
```C
double d = 3.1416;
void* ptr = &d;
char* char_ptr = (char *)ptr;
char ch = *char_ptr;
```
Starting off we declare `d`'s value just like we did in the first example. Then we declare a pointer `ptr` at the address of `d`. This pointer is of type `void` which just means "no type". This also means it can't be dereferenced without being recast. In line 3, we declare `char_ptr` as a pointer to a character which is a pointer to a character at `ptr` which is assigned to the address of `d`. We then dereference that pointer in line 4 which assigns the value of `char_ptr` to `ch`. 
#### Example 1
Using a struct, we will declare a new data type:
```C
struct fraction {
	int num;
	int denum;
}
```
This will give us 8 bytes of memory for a fraction made up of two four-byte integers. Next we will declare a fraction and assign it values:
```C
fraction f;
f.num = 22;
f.denum = 7;
```
We now have a fraction which we can look at as having two four byte values stacked on top of one another. Doing some memory manipulation, we can cast `f.denum` as a fraction and assign it's `num` a value which will reassign the value of `f.denum`. 
```C
((fraction *)&(f.denum)) -> num = 5;
```
If we look at `f` as being two four-byte integers stacked on top of one another, the code above would assign the four-byte block of memory above `f.denum` the value of 5. 
#### Example 2
First we will declare an array which will hold 10 integers and assign some values:
```C
int array[5];
array[3] = 128;
```

> [!IMPORTANT]
C does no balance checking on the length of arrays. Meaning, the 10 in `array[10]` exists only for initial memory allocation. The compiler does is not required to check if we have gone beyond 10 items in the array. This means that we can assign values "out of bounds" of the array. This includes assigning values to negative array values. As such, we can say that `*array === array[0]` and 
`*(array + k) === array[k]`.

That means that if we recast that array as a `short*`, the step value changes, but the initial memory address does not. Example:
```C
((short*)array)[6] = 2;
```
If we were to print this the value of `array[3]` now, it would give us a value of 640. Let's zoom in on the binary to see why this happens, but first let's remember that a `short` is two bytes and an `int` is four bytes. Also remember that arrays start at index \[0]. Based on that logic, `short` \[6] and \[7] overlap with `int` \[3]. The binary value of Int3 would then be assigned the following values:

|  Int[3] & Short[6]  |  Int[3] & Short[7]  | Int[3] Value | Short[6] Value | Short[7] Value |
| :-----------------: | :-----------------: | :----------: | :------------: | :------------: |
| `00000000 00000000` | `00000000 10000000` |     128      |       0        |      128       |
| `00000000 00000010` | `00000000 10000000` |     640      |       2        |      128       |
#### Example 3
```C
struct student {
	char *name;
	char suid[8];
	int numUnits;
}; 

student pupils[4]; 
pupils[0].numUnits = 21;
pupils[2].name = strdup("Adam"); 
pupils[3].name = pupils[0].suid + 6;
strcpy(pupils[1].suid, "40415xx");
```
For this example, we will need a memory diagram; however, working within the constraints of markdown might make this a bit hard to follow. I recommend drawing this out yourself to follow along and reference [this](https://youtu.be/H4MQXBF6FN4?list=PL9D558D49CA734A02&t=1876). 

|          | pupils[0] | pupils[1] | pupils[2] |      pupils[3]       |
| -------- | :-------: | :-------: | :-------: | :------------------: |
| NumUnits |    21     |           |           |                      |
| SUID     |           | 4041xx\0  |           |                      |
| Name     |           |           | &("Adam") | &(pupils[0].suid[5]) |
Walking through what we have currently, the first thing we see in the code block above is a declaration of a struct `student` that will contain a pointer to a character `name`, a character array `suid` with length 8, and an integer `numUnits`. We then initialize an array `pupils` of length 4 and type `student`. Finally, we assign some values to those fields. The only thing worth calling out here is `strdup()` which you can read more about [here](https://en.cppreference.com/w/c/experimental/dynamic/strdup). The next bit of code we'll look at will cause some bits we have already written to overwrite. 
```C
strcpy(pupils[3].name, "123456");
```
Understand that we have assigned `pupils[3]` to point to `pupils[0].suid + 6`. This means that when we write a value to `pupils[3]` it will write that value to the 6th character position of pupil 0's `suid`. In this case, the `suid` of pupil 0 would get the one and two, the `numUnits` would store the values of 3, 4, 5, & 6, and the `name` of pupil 1 would store the null character that ends the sequence. So, zooming into the binary of the `numUnits` field (remember that the numbers stored are stored as a character string, so their value has to be interpreted as the string value of that character) we get an astronomical value:

| Byte Values                           | Decimal Value |
| ------------------------------------- | ------------- |
| `00110011 00110100 00110101 00110110` | `859059510`   |
More interestingly, if we were to print the value of `pupils[3].name` we would get "123456". This is because there is no checking in place to see what that location in memory is supposed to be, the program goes to the location in memory specified and reads character values until it hits a null character. 