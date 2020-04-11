---
layout: post
title: Project Euler
---

[Project Euler](projecteuler.net) is a recreational coding site whose problems are easy to grasp, but are
computationally expensive with naive solutions. I've found it to be a good way to practice coding and also
an interesting way to learn about fields of math I wasn't previously aware of (totient function!?).
I'm currently in the top 1% of competitors. While I'm certain this has more to do with most people solving a handful
and losing interest, I'll still take that as a win.

<center><img src='https://projecteuler.net/profile/whiskeyandwry.png' alt='My Euler score'></center>

# euler_helper
I've done a decent number of problems and have created a Python package that contains 
some functions I found useful.

The [package](https://github.com/TomStarshak/project-euler-helper) can be found on my Github or installed with pip.
```bash
pip install euler_helper
```

At the time of writing there are ten functions of varying levels of usefulness through the ~100 problems I've finished. In no particular order:

* divisors(n): returns a list of divisors of net
* prime_factors(n): returns a list of the prime factors of net
* is_prime(n): check if n is prime
* sieve_of_eratosthenes(n): returns a list of all prime numbers < n
* is_palindrome(n): checks if n is a palindromic numbers
* is_pandigital(n): n is a m-digit pandigital number if it contains all digits 1-m exactly once
* is permutation(a, b): checks if a and b are permutations of each other
* coprime(a, b): check if a and b are coprime
* totient(n): returns Euler's totient functions
* generalized_hamming(x, n): Checks if integer x has no prime factors larger than n

Most functions show up in only a handful of problems, but methods of generating prime numbers comes up constantly and I got great use
out of the [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes). It's also an interesting example of an ancient
algorithm that's still relevant (...in recreational coding).

<center><img src='https://upload.wikimedia.org/wikipedia/commons/9/94/Animation_Sieve_of_Eratosth.gif'></center>


