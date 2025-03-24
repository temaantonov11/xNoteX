[[C]] [[Arduino]] 

## Introduction

Here are some situations where bit math can be helpful:
- Saving memory by packing up to 8 true/false data values in a single byte.
- Turning on/off individuals bits in a control register or hardware port register.
- Performing certain (`определенный`) arithmetic operations involving (`с участием`) multiplying or dividing by powers of 2 (`степень 2-ки`).
## The binary system

In this system, all integer values use only the values 0 and 1 for each digit. This how virtually all modern computers store data internally. Each 0 or 1 digit is called a `bit`, short of `binary digit`.

`C programming languages` allows you to specify binary numbers by prefixing them with `0b`. 
`0b11 == 3`, `0b111 == 7`.
## Bitwise AND

The bitwise AND operator in C++ use a single ampersand, `&`, used between two other integer expressions. Bitwise AND operates on each bit position of the surrounding (`окружающий`) expressions independently, according to this rule: **if both input bits are 1, the resulting output is 1, otherwise (`иначе`) the output is 0**. 

```C++
0 & 0 == 0
0 & 1 == 0
1 & 0 == 0
1 & 1 == 1
```

In Arduino, the type `int` is a 16-bit value, so using `&` between two `int` expressions causes 16 simultaneous AND operations to occur.

```C++
int a = 92; //  in binary: 0000000001011100
int b = 101; // in binary: 0000000001100101
int c = a & b;  // result: 0000000001000100, or 68 in decimal
```

One of the most common uses of bitwise AND is to select a particular bit (or bits) form an integer value, often called *masking*. For example, if you wanted to access the least significant bit in a variable `x`, and store the bit in another variable `y`, you could use the following code:

```C++
int x = 5; // binary: 101
int y = x & 1; // binary: 101 & 001 = 001 => y = 1
x = 4; // binary: 100
y = x % 1; // binary: 100 & 001 = 0 => y = 0
```
## Bitwise OR

The bitwise OR operator in C++ is the vertical bar symbol, `|`. Like the `&` operator, `|` operates independently each bit in its two surrounding integer expressions, but what it does is different. The bitwise OR of two bits is 1 if the either or both of the inputs bits is 1, otherwise it is 0.

```C++
0 | 0 == 0
0 | 1 == 1
1 | 0 == 1
1 | 1 == 1
```

Here is an example of the bitwise OR used in a snippet of C++ code:

```C++
int a = 92; //   in binary:0000000001011100
int b = 101;  // in binary:0000000001100101
int c = a | b;   // result:0000000001111101, or 125 of decimal
```

Bitwise OR is often used to make sure that a given bit is turned on (set to 1) in a given expression. For example, to copy the bits from `a` into `b`, while making sure the lowest bit is set to 1, use the following code:

```C++
b = a | 1;
```
## Bitwise XOR

There is a somewhat unusual operator in C++ called *bitwise exclusive OR*, also known as **bitwise XOR**. The bitwise XOR operator is written using the caret symbol `^`. This operator is similar to the bitwise OR operator `|`, except that it evaluates to 1 for a given position when exactly one of the input bits for that position is 1. If both are 0 or both are 1, the XOR operators evaluates to 0

```C++
0 ^ 0 == 0
0 ^ 1 == 1
1 ^ 0 == 1
1 ^ 1 == 0
```

**Another way to look at bitwise XOR is that each bit in the result is a 1 if the input bits are different, or 0 if they are the same.**

Here is a simple code example:

```C++
int x = 12; // binary: 1100
int y = 10; // binary: 1010
int z = x ^ y; // res: 0110, or decimal 6
```

The `^` **operator is often used to toggle** (i.e. change form 0 to 1, or 1 to 0) some of the bits in an integer expression while leaving others alone. For example:
```C++
y = x ^ 1; // toggle the lowest bit in x, and store the result in y.
```
## Bitwise NOT

The bitwise NOT operator in C++ is the tilde character `~`. Unlike `&` and `|`, the bitwise NOT operator is applied to a single operand to its right. Bitwise NOT changes each bit to its opposite: 0 becomes 1, and 1 becomes 0. For example:
```C++
int a = 103; // binary: 0000000001100111
int b = ~a; //  binary: 1111111110011000 = -104 
```

You might be suprised to see a negative number like -104 as the result of this operation.
This is because the highest bit in an `int` variable is the so-called `sign bit`. If the highest bit is 1, the number is interpreted as negative. This encoding of positive and negative numbers is referred to as *two's complement*.

As an aside, it is interesting to note that for any integer `x`, `~x` is the same as `-x-1`.
## Bit Shift Operators

There are two bit shift operators in C++: 
the **left shift operator** `<<` and **the right shift operator** `>>`
These operators cause the bits in the left operand to be shifted left or right by the number of positions specified by the right operand. 

For example:
```C++
int a = 5;      // binary: 0000000000000101
int b = a << 3; // binary: 0000000000101000, or 40 in decimal
int c = b >> 3; // binary: 0000000000000101, or back to 5 like we started with
```

When you shift a value `x` by `y` bits (`x << y`), the leftmost `y` bits in `x` are lost, literally shifted out of existence:
```C++
int a = 5;       // binary: 0000000000000101
int b = a << 14; // binary: 0100000000000000 - the first 1 in 101 was discarded
```

If you are certain that none of the ones in a value are being shifted to oblivion, a simple way to think of the left-shift operator is that it multiplies the left operand by 2 raised to the right operand power.
For example, to generate powers of 2, the following expressions can be employed:
```C++
1 << 0 == 1
1 << 1 == 2
1 << 2 == 4
1 << 3 == 8
...
1 << 8 == 256
1 << 9 == 512
1 << 10 == 1024
...
```

When you shift `x` right by `y` bits (`x >> y`), and the highest bit in `x` is a 1, the behavior depends on the exact data type of `x`. If `x` is of type `int`, the highest bit is the sign bit, determining whether `x` is negative or not, as we have discussed above. 
In that case, the sign bit is copied into lower bits, for esoteric historical reasons:
```C++
int x = -16;    // binary: 1111111111110000
int y = x >> 3; // binary: 1111111111111110
```
This behavior, called *sign extension*, is often not the behavior you want. Instead (`вместо этого`), you may wish zeros to be shifted in from the left. It turns out that the right shift rules are different for `unsigned int` expression, so you can use a typecasts to suppress ones being copied from the left:
```C++
int x = -16;              // binary: 1111111111110000
int y = unsigned(x) >> 3; // binary: 0001111111111110
```

If you careful to avoid sign extension, you can use the right-shift operator  `>>` as a way to divide by powers of 2. 
For example:
```C++
int x = 1000;
int y = x >> 3; // integer division of 1000 by 8, causing y = 125
```

