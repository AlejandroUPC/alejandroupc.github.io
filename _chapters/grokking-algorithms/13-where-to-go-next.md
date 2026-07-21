---
title: Chapter 13 - Where to go next
order: 13
date: 2026-07-21
tags: [books, algorithms, grokking-algorithms]
---

## Chapter intro


- Overview of other 10 algos and some tips on what to keep learning.



## Linear regression

You are trying to sell your `3000` sqm house, you happen to check some other hosues in the neighborhood and its size:

```markdown
|house|size|price|
|-|-|-|
|A|1000|200|
|B|2000|250|
|C|4000|300|
```

Now you could draw a line:

![linear regression line]({{ '/assets/images/grokking_algos_chapter_13/regr-line.svg' | relative_url }}){: .center-img }

Once we have a line relating size to price, we can use the line to predict the price of a `3000` sqm house:

![linear regression prediction]({{ '/assets/images/grokking_algos_chapter_13/regr-pred.svg' | relative_url }}){: .center-img }

So our house would be around `271`. The line is chosen by minimizing the squared vertical errors between the known prices and the line. From there, we can predict the price for a new house size.


## Inverted indexes

To see how a simplified search engine works, imagine we have three web pages with simple content:

```markdown
| website | text |
|-|-|
|A| hi there|
|B| hi adit|
|C| there we go|
```

We could build a hash table per word:

```markdown
|word|website|
|-|-|
|hi|A,B|
|there|A,C|
|adit|B|
|we|C|
|go|C|
```

If a user searches `hi` the sites `A` and `B` would show up, a hash map that maps word to places they appear is called inverted index.


## The Fourier transform


The Fourier transform is useful, especially for signal processing, because it can represent a signal as a combination of frequencies.

## Parallel algorithms

One way to speed up algorithms is to split work into parts that can be computed at the same time, so multiple cores can run in parallel. This is different from dynamic programming, where subproblems may depend on results computed earlier.

For example, comparison-based sorting usually takes around `O(n log n)` total work. A parallel sorting algorithm can reduce the elapsed time by distributing that work across processors, but its running time depends on the algorithm and the number of available processors; parallel quicksort does not automatically become `O(n)`.

But creating and running parallel algorithms adds also some complexity, its not that easy, and time gains aren't always linear based on:

-  Overhead of managing parallelism: More cores can lead to mroe problems, if you have a 1000 array item you have to decide how to split em, is it just fine to split in the half or there are some other ways that are more effective?

- Amdahl's law: Imagine it takes 20h to paint, but you want to do it in 10h. You could optimize two steps, the sketching and the painting. If sketching takes only 5 minutes after the optimization but painting still takes 19 hours, the total time is still about 19 hours. The possible speedup is limited by the part of the work that was not optimized.
What the law says is that when you optimize one part of a system the performance gains is limited on how much time that part actually takes, so you should try and optimize (if possible and easy) the heaviest.

- Load balancing: Similar to first point, you happen to have 10 tasks and two cores, you give 5 to core `A`, but are very easy and is done in 10 seconds, but `B` gets the hardes is not done until 5 minutes, so `A` was idle for almost the entire 5 minutes, can we predict this load of work and distribute equally?


## Map Reduce

One of the most relevant distributed programming models, especially in the era of big data, is MapReduce. The map step processes input records and emits key-value pairs; the reduce step groups values by key and combines them. This makes it possible to process large datasets across many machines.

## Bloom filters and HyperLogLog

Finding elements in a hash table can get out of control, there are some filters (e.g imagine a hash table indexing all sites).

### Bloom filters

Bloom filters are probabilistic data structures for membership queries. They can say that an element is "possibly present" or "definitely absent":

- False positives are possible: it can say that you crawled a website when you have not.
- False negatives aren't possible in a standard Bloom filter when it is used correctly: if it says an element is absent, it is absent.

So this property is great, also take very little space.

### HyperLogLog

Imagine Google wants to count the number of unique searches performed by its users. It would take a lot of space to keep a log of every unique search and check each new search against it.

HyperLogLog is able to approximate the number of unique elements in a set, it does not give you an exact answer but it comes very close, while being very cheap resource-wise.


## HTTPS and Deffie-Hellman key exchange

`HTTPS` uses TLS to provide an encrypted and authenticated connection between a client and a server. Certificate validation helps prevent a man-in-the-middle from impersonating the server.

Public-key cryptography can use a public key to encrypt a message and a private key to decrypt it. Diffie-Hellman is different: it lets two parties establish a shared secret over an insecure connection.

So you can write to anyone using its public key, but only the reader that holds the private key, can read it.

Some buzzwords around HTTPS:

- Transport Layer Security (TLS) is a protocol to establish secure connections.
- SSL (old unamintainted TLS cuz security issues).
- Symmetric key encryption: both sides use the same key. HTTPS uses asymmetric cryptography during setup and symmetric encryption for the actual data transfer.
- HTTPS can use an ephemeral version of Diffie-Hellman to establish a shared secret. Symmetric encryption then protects the actual data transfer.


## Locality-sensitive hashing

With a cryptographic hash, changing a small part of the input usually produces a completely different hash. This is called the avalanche effect. Collisions are theoretically possible, but good hash functions make them very unlikely.

What happens if there is a case where you'd want a locality-sensitive hash, where similar inputs are likely to generate similar hashes? That's what SimHash is designed to do.

## Min heaps and priority queues

Min heaps are tree-based data structures that store values while keeping the smallest value at the root. You can look at the minimum element in `O(1)`, or remove the minimum and restore the heap in `O(log n)`.
Heaps can be used for sorting: repeatedly removing the minimum element produces a sorted sequence in `O(n log n)` time.


Max heaps are the same but keeping the highest value. These two algorithms are great for priority queues (queues we saw FIFO and LIFO that are based on insertion order), but there might be priority we set ourselves.

## Linear programming

Linear programming optimizes a linear objective function subject to linear constraints. The variables can represent how many units of each product to make, while the constraints represent available resources. For example, if your company makes two products:

1. Shirts: need 1m of fabric and 5 buttons, you make 2$ profit per piece.
2. Totes: needs 2m of fabric and two buttons, you make 3% profit per piece.

Right now you have 11 meters of frabci and 20 buttons, how do you maximize profit?

Linear programming can be solved with the Simplex algorithm, among other methods such as interior-point methods.

## Closing up

To remember:

- Linear regression predicts a target value from one or more features. In the house example, size is the feature and price is the target.

- The regression line is chosen by minimizing the squared errors between the known target values and the predictions.

- In parallel algorithms, total work is the computation done by all processors, while elapsed time is how long the program takes to finish.

- Amdahl's law says that the part of a program that cannot be improved or parallelized limits the total speedup.

- A standard Bloom filter can return "possibly present" or "definitely absent". It can have false positives, but not false negatives.

- Diffie-Hellman establishes a shared secret. Symmetric encryption uses that secret to protect the actual data.

- Cryptographic hashes make small input changes produce very different hashes, while locality-sensitive hashes such as SimHash make similar inputs likely to produce similar hashes.

- A min heap lets us read the minimum in `O(1)` and remove it in `O(log n)`. Repeatedly removing the minimum can sort values in `O(n log n)`.

- Linear programming optimizes a linear objective function subject to linear constraints. Variables represent decisions and constraints represent limits.
