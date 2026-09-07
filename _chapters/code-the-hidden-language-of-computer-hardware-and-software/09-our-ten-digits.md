---
title: Chapter 9 - Our Ten Digits
order: 9
date: 2026-09-07
tags: [books, computer-architecture, code-the-hidden-language]
---

## Chapter notes

### Numbers, numerals, and digits

Language can be understood as a code, a bit like Morse code. Different languages use different words for the same thing: *dog*, *perro*, *Hund*... Something similar happens with numbers, although the Western Arabic digits are understood across many languages:

$$
0\ 1\ 2\ 3\ 4\ 5\ 6\ 7\ 8\ 9
$$

These symbols are widespread, but they are not actually universal. Other writing systems use different digit shapes.

There is also a small but useful distinction here:

- A **number** is an abstract quantity, such as the quantity of four apples.
- A **numeral** is how we write that number, such as `4`, `IV`, or `100` in binary.
- A **digit** is one symbol used inside a numeral, such as `4` or `0`.

So yeah, the decimal numeral `4` and the binary numeral `100` look completely different, but they represent the same number:

$$
4_{10} = 100_2
$$

The little subscripts tell us which base each numeral uses. We will get to binary properly later; for now, the important idea is that the written symbols are not the number itself.


### Why ten digits?

Humans have probably been counting with their fingers for a very long time, which likely helped make number systems based on ten common. There have been plenty of exceptions, including systems based on five, twelve, twenty, and sixty. We can still see a base-60 influence in the way we divide time into minutes and seconds.

Had humans evolved with eight or twelve fingers, maybe another base would feel as natural to us as ten does now. This is obviously speculative, but it helps show that there is nothing inevitable or magical about base 10.

Numbers were useful for counting objects, keeping records, and tracking exchanges. Different cultures developed different ways of writing those quantities.


### Roman numerals: a non-positional system

One familiar historical example is the Roman numeral system. Its main symbols and decimal values are:

| Roman numeral | Decimal value |
|:-------------:|:-------------:|
| I | 1 |
| V | 5 |
| X | 10 |
| L | 50 |
| C | 100 |
| D | 500 |
| M | 1000 |

For example:

$$
XXVII = 10 + 10 + 5 + 1 + 1 = 27
$$

Roman numerals are mostly additive, but the standard modern notation also uses a few subtractive pairs:

| Pair | Value | Explanation |
|:----:|:-----:|:-----------:|
| IV | 4 | 5 - 1 |
| IX | 9 | 10 - 1 |
| XL | 40 | 50 - 10 |
| XC | 90 | 100 - 10 |
| CD | 400 | 500 - 100 |
| CM | 900 | 1000 - 100 |

The rule is not simply “subtract any smaller symbol that appears before a larger one.” Standard Roman notation only allows these combinations:

- `I` can be placed before `V` or `X`.
- `X` can be placed before `L` or `C`.
- `C` can be placed before `D` or `M`.
- `V`, `L`, and `D` are never used for subtraction.

A useful way to remember this is to build each decimal part separately. For example, `49` is made from `40 + 9`:

$$
49 = 40 + 9 = XL + IX = XLIX
$$

Writing `IL` might look logical if we read it as `50 - 1`, but `I` is not allowed before `L`. So yeah, Roman subtraction is a small set of rules, not a general operation we can use with any two symbols.

Some additions and subtractions can be performed by combining and reducing symbols, but multiplication and division become awkward. In practice, people often used tools such as counting boards rather than doing every calculation directly in Roman notation.

Another important detail is that Roman numerals are **non-positional**. Moving an `X` around does not automatically turn it into tens, hundreds, or thousands. Its basic value remains ten.


### The Hindu-Arabic decimal system

The system most of us use today developed in India and reached Europe through mathematicians writing in the Islamic world, which is why it is commonly called the **Hindu-Arabic numeral system**.

It differs from Roman numerals in a few very important ways:

- It uses only ten digits: `0` through `9`.
- It is positional, so a digit's value depends on where it appears.
- It does not need a special symbol for ten. The numeral `10` reuses the digits `1` and `0`.
- It uses `0` both as a number and as a placeholder for an empty position.

That last point is what lets us clearly distinguish `25`, `205`, and `250`. The zero holds a position open even when that position contributes no value.

Both `10` and `1,000,000` contain a single `1`, but that digit represents a different quantity in each numeral because it appears in a different position.


### Positional notation

Let's take the decimal numeral `4825`. Its digits represent:

- `4` thousands.
- `8` hundreds.
- `2` tens.
- `5` units.

Therefore:

$$
4825 = 4000 + 800 + 20 + 5
$$

We can make each position explicit:

$$
\begin{aligned}
4825 ={}& 4 \times 1000 \\
       &+ 8 \times 100 \\
       &+ 2 \times 10 \\
       &+ 5 \times 1
\end{aligned}
$$

Each position corresponds to a power of ten:

$$
\begin{aligned}
4825 ={}& 4 \times 10^3 \\
       &+ 8 \times 10^2 \\
       &+ 2 \times 10^1 \\
       &+ 5 \times 10^0
\end{aligned}
$$

Remember that $10^0 = 1$, which is why the rightmost digit represents units. Counting positions from right to left, we start at position zero:

```plaintext
 Digit:       4       8       2       5
 Position:    3       2       1       0
 Value:    4×10³   8×10²   2×10¹   5×10⁰
```

This four-digit pattern can represent values from `0000` through `9999`. More generally, each digit is multiplied by the base raised to the power of its position, and then all those values are added together.


### Digits after the decimal point

The same idea continues to the right of the decimal point, except those positions use negative powers of ten. For example:

$$
\begin{aligned}
48.25 ={}& 4 \times 10^1 \\
         &+ 8 \times 10^0 \\
         &+ 2 \times 10^{-1} \\
         &+ 5 \times 10^{-2}
\end{aligned}
$$

Since $10^{-1} = \frac{1}{10}$ and $10^{-2} = \frac{1}{100}$, the digit `2` represents two tenths and the digit `5` represents five hundredths.


### Looking ahead to binary

Decimal positions use powers of ten because decimal is base 10. If we change the base, we change the powers used by each position.

Binary is base 2 and uses only the digits `0` and `1`. This is why decimal `4` can be written as binary `100`:

$$
100_2 = 1 \times 2^2 + 0 \times 2^1 + 0 \times 2^0 = 4_{10}
$$

Nothing about the quantity changed; only its representation did. This is the bridge from our familiar ten digits to the binary signals a computer can represent using `0` and `1`.
