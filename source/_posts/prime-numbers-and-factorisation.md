---
title: Prime Numbers & Factorisation
date: 2022-05-11 23:09:37
tags:
---

## Introduction

Checking if a number is prime and finding prime factors of a number has plenty of applications in computer science. So you may face with these questions very often. Let’s look at what are different ways of finding prime numbers and prime factors of numbers.

## Primality Test

Prime number is a number that has only two divisors: 1 and itself. For example, 2, 3, 7, 11 are prime numbers. So, how to check if a given number is prime or not? I know you already have a solution 😛 we can start from 1, go to N - 1 and check if number is divisible by any of this number. And you are correct, this is a solution:

```cpp
bool isPrime(unsigned int n) {
  if (n == 1) return false;
  if (n == 2) return true;
  if (n % 2 == 0) return false;

  for (int i = 3; i < n; i++) {
    if (n % i == 0)
      return false;
  }

  return true;
}
```

Great! We just found a way to check if a number is a prime or not. But is it efficient? Since, we’re checking all numbers till N - 1, time complexity of this algorithm will be _O(N)_. With a very big number, it will take a lot of time just to check it. Suppose that you want to find number of prime numbers between 1 and some arbitrary integer X, then we’ll have quadratic time algorithm 😕

## Better Primality Check Algorithm

The better primality check algorithm would be to go not from 1 to N, but go from 1 to square root of N. If you are better than me at math, then it is probably obvious to you. If it is not obvious, then let’s see why it is true.

Suppose that number `N` is not a prime. Then it can be factored into two divisors like `N = A * B`. Now, both A and B cannot be greater than square root of N, because if they did, then `sqrt(N) * sqrt(N)` would be greater than N . So, at least one of the factors of N must be smaller than square root of N. If we can’t find any factor of N, which is smaller than square root of N, then N is a prime.

```cpp
bool isPrime(unsigned int n) {
  if (n == 1) return false;
  if (n == 2) return true;
  if (n % 2 == 0) return false;

  for (int i = 3; i < floor(sqrt(n) + 1); i++) {
    if (n % i == 0)
      return false;
  }

  return true;
}
```

So, now running time of our function is _O(sqrt(N))_ which is way better than our previous solution. You can try both of these functions with some very big integer and see the difference in performance.

## Even Better Primality Check Algorithm

Finding prime numbers are not always as trivial as we discussed before. What I mean is, often times you won’t be asked to check single number, you will rather be asked to collect some huge number of primes. For example, how many prime numbers are there between 1 and 10<sup>9</sup>? In this example we will need to iterate through each number from 1 to 10<sup>9</sup> and check its primality by using our isPrime function. And if we call this 109 - K, which might even be a bigger number, our time complexity will be _O(K sqrt(N))_. Can we do better? The answer is - yes.

Another cool method of checking primality of numbers, is to collect prime numbers in a data structure like array or a hash table, so that primality test takes O(1) time. But we need to process the numbers first, so that later accesses can be constant time.

The algorithm that we are going to use is called **Sieve of Eratosthenes**. The idea is to create a boolean array of N numbers and set all of them to true by default. So, for us, as a starting point every number is prime number. Then we take 2 and start marking all multiples of 2 false - to indicate that they are not prime (e.g. 4, 6, 8, 10, 12 … are not prime numbers). We do the same for 3, we start marking all multiples of 3 as non prime as well. Then we do the same for the rest of numbers which are prime. It’s better I show it:

<img src="./sieve_animation.gif" alt="Sieve of Eratosthenes animation"/>

Did you get the idea? Okay, I am assuming that you did 🚀 In the end all numbers in the array that has true value will be counted as prime number. And it will only take us O(1) time to access it. The only overhead here is to create this table which will take _O(N log(log N))_ time and _O(N)_ space.

Here is the code:

```cpp
vector<bool> sieveOfEratosthenes(int n) {
  // an array of size n + 1 with value of 'true'
  vector<bool> primes(n + 1, true);

  // 0 and 1 are not prime
  primes[0] = false;
  primes[1] = false;

  for (long long p = 2; p * p <= n; p++) {
    // if primes[i] is not changed, it is a prime
    if (primes[p] == true) {
      // update all multiples of p
      // and mark them as not prime - 'false'
      for (long long i = p * p; i <= n; i += p) {
        primes[i] = false;
      }
    }
  }

  return primes;
}
```

## A Small Task for You

A prime number p is called a [Sophie Germain number](https://en.wikipedia.org/wiki/Safe_and_Sophie_Germain_primes) if 2p+1 is also a prime number. Go ahead and find number of Sophie Germain numbers between 1 and 100000. The task is to do it using with all three methods of primality check that we discussed and see the performance for yourself. Good luck! 🤓

## Prime Factorization

Prime factorization is a process of finding prime factors of a number. There are plenty of applications of this in computer science that are out of scope of this article, but if you are curious you can look at [RSA encryption algorithm](<https://en.wikipedia.org/wiki/RSA_(cryptosystem)>) and how it works.

### Bruce Force Method

Let’s take a number 30 as an example. 30 can be written as 2 _ 15. However, 15 is not a prime number so we need to reduce it to its factors as well. So, 30 = 2 _ 3 \* 5. As you see 2, 3 and 5 are prime numbers. How to write a code that returns prime factors of a number?

The brute force method is quite simple. Start from 2 and go to the number itself (let’s say N = 30, then i <= 30) and while the number is divisible by the number i, divide it by i (N = N / i). It is better I show the code:

```cpp
vector<int> getPrimeFactors(int n) {
    vector<int> prime_factors;

    for (int i = 2; i <= n; i++) {
        if (n % i == 0) {
            while (n % i == 0) {
                n = n / i;
                prime_factors.push_back(i);
            }
        }
    }

    return prime_factors;
}
```

Go ahead and try it on your code editor and play with it. And let’s not forget about time complexity of this algorithm. It is _O(N)_.

### Better Approach

Let’s try to find what’s bad about the brute force approach. Yeah, it is the name 😆

No, seriously, let’s take number 99 as an example. 99 equals to 3 multiplied by 3 and 11 if we prime factorize it.

But in our previous code we start from 2 and go all the way to N to check if it is divisible by the current number i. Let’s dry run our previous algorithm and see what it would do in this case:

1. i = 2; N = 99; is 99 divisible by 2? No.
2. i = 3; N = 99; is 99 divisible by 3? Yes. So, 3 is a prime factor and N = 99 / 3;
3. i = 3; N = 33; is 33 divisible by 3? Yes. So, 3 is another prime factor and N = 33 / 3;
4. i = 3; N = 11; is 11 divisible by 3? No.
5. i = 4; N = 11; is 11 divisible by 4? No.
6. i = 5; N = 11; is 11 divisible by 5? No.
7. i = 6; N = 11; is 11 divisible by 6? No.
8. i = 7; N = 11; is 11 divisible by 7? No.
9. i = 8; N = 11; is 11 divisible by 8? No.
10. i = 9; N = 11; is 11 divisible by 9? No.
11. i = 10; N = 11; is 11 divisible by 10? No.
12. i = 11; N = 11; is 11 divisible by 11? Yes. So, 11 is another prime factor and N = 11 / 11;
13. i = 12; N = 11; we break the loop (i <= N)

As we see, even though 11 is already a prime number (which implies that it is a prime factor) we did all those necessary steps by checking if it is divisible by 3, 4, 5, 6 through 11. What if it wasn’t 11 but a very large prime number? We would again do all those steps.

So our observation is that, once N is reduced to a prime number, the only factor of N will be N itself.

As we’ve learnt earlier, if a number does not have any factors till its square root (sqrt(N)) then it is a prime number. So, in this algorithm the optimal solution is to go not from 2 to N, but from 2 to sqrt(N). And after our loop finishes, if N is 1 it means everything went well and we did not have any numbers such as 11. If it is not 1 then reduced N itself is also prime factor of our first N.

```cpp
vector<int> getPrimeFactors(int n) {
    vector<int> prime_factors;

    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            while (n % i == 0) {
                n = n / i;
                prime_factors.push_back(i);
            }
        }
    }

    if (n != 1) {
        prime_factors.push_back(n);
    }

    return prime_factors;
}
```

I suggest dry running this algorithm and previous algorithm by yourself and try to understand the reason why we did it. You can try N = 99 and check it on paper. And time complexity of this function will be _O(sqrt(N))_.