---
title: Chapter 10 - Alternatives to Ten
order: 10
date: 2026-09-10
tags: [books, computer-architecture, code-the-hidden-language]
---

## Chapter notes

### What does a base mean?

Ten feels like a very important number because we use it everywhere. Having ten fingers probably helped make base 10 common, but there is nothing inevitable or magical about it. Other cultures have used systems based on numbers such as five, twelve, twenty, and sixty.

The decimal system is **base 10**, which means it uses ten different digits:

```text
0, 1, 2, 3, 4, 5, 6, 7, 8, 9
```

Once we run out of digits, we carry into a new position. That is how we go from `9` to `10`.

Here is the slightly confusing part: the written numeral `10` does not always mean the decimal number ten. In any positional system, `10` means one group of the base and zero units:

$$
10_2 = 2_{10}, \qquad 10_4 = 4_{10}, \qquad 10_8 = 8_{10}
$$

So if I have two ducks, the quantity has not magically changed, but I can write it as either $2_{10}$ or $10_2$. The number stays the same; only its representation changes.


### Counting in octal

Imagine that humans had eight fingers instead of ten and naturally chose base 8. This system is called **octal**, and it only needs the digits `0` through `7`:

```text
0, 1, 2, 3, 4, 5, 6, 7, 10, 11, 12...
```

After `7`, there is no new digit available, so we carry and write `10`. That octal `10` represents decimal `8`.

This makes familiar quantities look a little strange:

- Snow White still meets $7_8$ dwarfs because seven fits in one octal digit.
- A cartoon character with eight fingers has $10_8$ fingers.
- Beethoven's nine completed symphonies become $11_8$ symphonies.
- Ten human fingers become $12_8$ fingers.
- Twelve months become $14_8$ months.
- Sixteen years become $20_8$ years.
- Twenty-four hours become $30_8$ hours.
- The 26-letter English alphabet has $32_8$ letters.
- A chessboard's 64 squares become $100_8$ squares.
- A 128-player tournament starts with $200_8$ players.

The quantities have not changed. We are only describing them using powers of eight instead of powers of ten.

For example, octal `14` is decimal `12`:

$$
14_8 = 1 \times 8^1 + 4 \times 8^0 = 8 + 4 = 12_{10}
$$


### Converting another base to decimal

The decomposition from the previous chapter works with any positional base. For every digit, we multiply the digit by the base raised to the power of its position, starting with position `0` on the right. Then we add all the results:

$$
N = \sum_{i=0}^{n} d_i \times b^i
$$

Here, $d_i$ is the digit at position $i$, and $b$ is the base.

For example, binary `1001` becomes:

$$
1001_2
= 1 \times 2^3
+ 0 \times 2^2
+ 0 \times 2^1
+ 1 \times 2^0
= 9_{10}
$$


### Comparing binary, quaternary, octal, and decimal

We can apply the same idea to base 4, called **quaternary**, and base 2, called **binary**:

| Power of Two | Binary       | Quaternary | Octal | Decimal |
|:------------:|-------------:|-----------:|------:|--------:|
| 2^0          |            1 |          1 |     1 |       1 |
| 2^1          |           10 |          2 |     2 |       2 |
| 2^2          |          100 |         10 |     4 |       4 |
| 2^3          |         1000 |         20 |    10 |       8 |
| 2^4          |        10000 |        100 |    20 |      16 |
| 2^5          |       100000 |        200 |    40 |      32 |
| 2^6          |      1000000 |       1000 |   100 |      64 |
| 2^7          |     10000000 |       2000 |   200 |     128 |
| 2^8          |    100000000 |      10000 |   400 |     256 |
| 2^9          |   1000000000 |      20000 |  1000 |     512 |
| 2^10         |  10000000000 |     100000 |  2000 |    1024 |
| 2^11         | 100000000000 |     200000 |  4000 |    2048 |

For the same positive number, a lower base generally requires more digits. Binary numerals get long quickly because each position can contain only `0` or `1`.


### Converting decimal to binary

To go in the opposite direction, from decimal to binary, we can repeatedly divide by `2` and record the remainder. The quotient is the whole-number result of the division, while the remainder is always `0` or `1`.

For decimal `150`:

| Number | Divide by 2 | Quotient | Remainder |
|------:|:-----------:|---------:|----------:|
| 150 | 150 / 2 | 75 | 0 |
| 75  | 75 / 2  | 37 | 1 |
| 37  | 37 / 2  | 18 | 1 |
| 18  | 18 / 2  | 9  | 0 |
| 9   | 9 / 2   | 4  | 1 |
| 4   | 4 / 2   | 2  | 0 |
| 2   | 2 / 2   | 1  | 0 |
| 1   | 1 / 2   | 0  | 1 |

The first remainder is the rightmost binary digit, so we read the remainders from bottom to top:

```text
Remainders from top to bottom: 0 1 1 0 1 0 0 1
Read them from bottom to top:  1 0 0 1 0 1 1 0
```

Therefore:

$$
150_{10} = 10010110_2
$$

Each division removes the current rightmost binary digit. The remainder tells us whether that digit was `0` or `1`, and the quotient contains the digits still left to discover.


### Binary addition

Binary arithmetic looks intimidating at first, but its addition table has only four combinations:

| + | 0 | 1 |
|:-:|:-:|:-:|
| 0 | 0 | 1 |
| 1 | 1 | 10 |

The result `10` in the bottom-right cell means that $1 + 1$ produces a result bit of `0` and carries `1` into the next position.

For example:

$$
\begin{array}{r}
  1100101 \\
+ 0110110 \\
\hline
 10011011
\end{array}
$$

We work from right to left, just like in decimal addition. Whenever a column reaches binary `10`, we write `0` and carry `1` into the column on the left. In this example:

$$
1100101_2 + 0110110_2 = 10011011_2
$$


### Binary multiplication

One-bit binary multiplication is even smaller than the addition table:

| × | 0 | 1 |
|:-:|:-:|:-:|
| 0 | 0 | 0 |
| 1 | 0 | 1 |

Now we can multiply two longer binary numerals:

$$
\begin{array}{r}
      1101 \\
\times\ 1011 \\
\hline
      1101 \\
     11010 \\
    000000 \\
   1101000 \\
\hline
  10001111
\end{array}
$$

Each multiplier bit creates a partial product. Multiplying by `0` creates zeros, multiplying by `1` copies the original value, and each new row shifts one position to the left. Finally, we add the partial products:

$$
1101_2 \times 1011_2 = 10001111_2
$$


### Fixed width, LSB, and MSB

Binary values are often padded with zeros so that every value has the same width. In a three-bit group, decimal zero is written as `000` rather than just `0`. The padding does not change its value.

The rightmost bit is the **least significant bit**, or **LSB**, because it represents $2^0$. The leftmost bit is the **most significant bit**, or **MSB**, because it represents the largest power of two in that group.

When we count upward, the LSB changes first. If adding one makes a bit go from `1` to binary `10`, that bit returns to `0` and carries `1` into the next position on the left:

| Binary | Decimal |
|:------:|--------:|
| 000 | 0 |
| 001 | 1 |
| 010 | 2 |
| 011 | 3 |
| 100 | 4 |
| 101 | 5 |
| 110 | 6 |
| 111 | 7 |


### From binary codes to outputs

Binary numbers give us a way to connect quantities with electrical signals. Logic gates can then examine those bits and activate particular outputs.


#### The 3-to-8 decoder

A **3-to-8 decoder** accepts one three-bit input and activates exactly one of eight output lines. Every possible input from `000` through `111` has its own output:

| Binary input | Active output |
|:------------:|:-------------:|
| 000 | D0 |
| 001 | D1 |
| 010 | D2 |
| 011 | D3 |
| 100 | D4 |
| 101 | D5 |
| 110 | D6 |
| 111 | D7 |

![A three-to-eight binary decoder built from AND gates and inverters]({{ '/assets/images/code_hidden_language_chapter_10/decoder_and_inv.svg' | relative_url }}){: .center-img }

*Source: [“Decoder AND INV” by W.Rebel](https://commons.wikimedia.org/wiki/File:Decoder_AND_INV.svg).*

The diagram is a bit intimidating because so many wires cross each other. It becomes easier if we choose one output on the right and trace its three inputs backwards.

For `D7`, the input must be `111`. All three signals go directly into the AND gate, without passing through an inverter:

$$
D_7 = A_2 \land A_1 \land A_0
$$

For `D4`, the input must be `100`. The most significant bit goes directly into the AND gate, while the other two inputs are inverted:

$$
D_4 = A_2 \land \neg A_1 \land \neg A_0
$$

The AND gate for `D4` therefore receives `1, 1, 1` only when the original input is `1, 0, 0`. This is how the inverters let each AND gate recognize one specific binary value.


#### The 8-to-3 encoder

An **8-to-3 encoder** performs the opposite mapping. It accepts eight possible input lines and produces the corresponding three-bit binary code. In this simple circuit, exactly one input is expected to be active at a time.

![An eight-to-three encoder built from three OR gates]({{ '/assets/images/code_hidden_language_chapter_10/8-3-encoder.jpeg' | relative_url }}){: .center-img }

*Image source: [The Instrument Guru](https://theinstrumentguru.com/encoder-and-decoder/).*

Each output bit is produced by an OR gate connected to every input number that needs that bit set:

$$
\begin{aligned}
A_2 &= Y_4 \lor Y_5 \lor Y_6 \lor Y_7 \\
A_1 &= Y_2 \lor Y_3 \lor Y_6 \lor Y_7 \\
A_0 &= Y_1 \lor Y_3 \lor Y_5 \lor Y_7
\end{aligned}
$$

For example, activating `Y4` produces `100`, while activating `Y7` produces `111`. Input `Y0` does not need a connection to any OR gate because its binary code is `000`.

There is one limitation: if several inputs become active simultaneously, their signals are combined and the result might not identify either input correctly. A **priority encoder** solves this by deciding which active input has priority.


### Closing notes

So yeah, `10` is not tied to the quantity ten. Its value depends on the base. Once that clicks, binary stops looking like a weird collection of zeros and ones and starts behaving like any other positional number system.

We can convert binary values, perform arithmetic with them, and use logic gates to decode them into individual signals or encode signals back into binary. This is the bridge between numbers as we write them and numbers represented by electrical circuits.
