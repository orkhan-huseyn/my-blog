---
title: A Gentle Introduction to Signed Integer Representation
date: 2019-11-26 22:15:59
---

## Introduction

When we talk about numbers and data types in programming we generally mention how many bytes they occupy in memory. For example, we say that 32 bit integer can hold values from 0 up to 2^32^-1, 64 bit can hold up to 2^64^ - 1 and etc. However, this examples are true when the integer is unsigned, which means that it consists of only positive numbers. In signed integers, the ones that hold both negative and positive values, the minimum and maximum values are different: 32 bit signed integers can hold values from -2^31^ up to 2^31^-1. Why? To understand it deeply, let see it visually.

## Unsigned integers in binary

Let's say we have a 4 bit unsigned integer. Its mimimum value is 0 when the maximum value is 15. Let's write them all down:

```
0000 -- 0 in decimal, this is the minimum
0001 -- 1 in decimal
0010 -- 2 in decimal
0011 -- 3 in decimal
0100 -- 4 in decimal
0101 -- 5 in decimal
0110 -- 6 in decimal
0111 -- 7 in decimal
1000 -- 8 in decimal
1001 -- 9 in decimal
1010 -- 10 in decimal
1011 -- 11 in decimal
1100 -- 12 in decimal
1101 -- 13 in decimal
1110 -- 14 in decimal
1111 -- 15 in decimal, this is the maximum
```

This is pretty clear and an usual way of how we think of binary representation of integers. What about negative numbers? If 4 bit integer was signed then its minimum value would be -8 (-2^3^) when the maximum value would be 7(2^3^-1). Again why? Let's write them down:

```
0000 -- 0 in decimal
0001 -- 1 in decimal
0010 -- 2 in decimal
0011 -- 3 in decimal
0100 -- 4 in decimal
0101 -- 5 in decimal
0110 -- 6 in decimal
0111 -- 7 in decimal, this is the maximum
1000 -- (-8) in decimal, this is the mimimum
1001 -- (-7) in decimal
1010 -- (-6) in decimal
1011 -- (-5) in decimal
1100 -- (-4) in decimal
1101 -- (-3) in decimal
1110 -- (-2) in decimal
1111 -- (-1) in decimal
```

Now, this looks quite confusing, right? One question you may ask is: _Why is 1111 is -1 and 1000 is -8?_. Now, if you add -7 and 7 or -1 and 1 or -5 and 5 in binary, you will see that you get 0 in binary (in 4 bytes, ignore the carry). Why this works? Is it a magic? To understand it clearly we need to understand complements technique. The reason, why negative 1 is 1111, because 1111 is 2's complement of positive one (0001). So, what is complement technique?

## Complement tehcnique

Let's first begin with the decimal world. If we want to find 9's complement of any decimal number, we subtract it from all 9's. For example, if we want to find 9's complement of 58, we subtract 58 from 99:

`99 - 58 = 41 --> is 9's complement of 58`

If we add 1 to 41, we will get 42 and that will be 10's complement. So, adding 1 to the 9's complement gave us 10's complement. This is simply, subtracting 58 from 10.

`99 - 58 + 1 = 42 --> is 10's complement of 58`

Similarly, if we want to find 2's complement of any binary number, we subtract it from all 1's to find 1's complement and add 1 to it to get 2's complement. So, 1's complement of 1001 is: `1111 - 1001 = 0110` and adding 1 to it will give us 2' complement: `0110 + 0001 = 0111`. But, how computers subtract a number from the other?

If you were careful, you would notice that to find 1's complement of binary number we don't need to practically subtract the number from 1111. Just flipping the bits would give us the right answer, so instead of subtracting, we could apply logical NOT operator to the number: `NOT 1001 = 0110 --> 1's complement of 1001`

## Subtracting using complement

Since there is no subtraction operation in computers, we also need to subtract numbers using addition. Luckily, complement technique also helps here.

Again in the decimal world! Let's say we want to subtract 58 from 87. 10's complement of 58 is 42, and if you add 87 and 42 together it will give you 129. Remove, 1 from 129 (because we are only working with 2 digits and ignoring the overflows) and you will get 29. Guess what? 87 - 58 is 29.

Similarly, in binary world, if you want to subtract a number from another, find 2's complement of the small number and add it with the other. For example, to calculate `1100 1011 - 1001 1100`, (203 - 156 in unsigned representation) find 2's complement of `1001 1100` which is `NOT 1001 1100 + 0000 0001 = 0110 0011 + 0000 0001 = 0110 0100` and then adding it with the first number will give us `1100 1011 + 0110 0100 = 010010 1111`. Since we ignore overflows, we will ignore first two digits of the result and our result will be `0010 1111` which is 47 in decimal (203 - 156 = 47).

## Conclusion

Now, hopefully it makes sense why the negative numbers were depicted in the way they are. And as you already understood from the snippeds above any signed integers minimum value will be -2^n-1^ and maximum value will be 2^n-1^-1. If anyone asks you what is, let's say negative 8 in binary, just find what is positive 8 in binary, then two's complement of that number will be negative 8.

The contents of this article is heavily based on [Number Theory for Programmers](https://www.udemy.com/course/number-system/) course on Udemy, so I highly recommend you to enroll and learn more.

Thanks for reading.

If you find any mistakes on article, please let me know.
