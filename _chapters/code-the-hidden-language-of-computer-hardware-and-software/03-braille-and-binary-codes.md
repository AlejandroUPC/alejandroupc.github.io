---
title: Chapter 3 - Braille and Binary Codes
order: 3
date: 2026-08-06
tags: [books, computer-architecture, code-the-hidden-language]
---

## Chapter notes

Morse is not the only way to represent language using a code. Braille is another example: its standard cell has six positions, and each position has one of two states—raised or flat.

Braille was developed as a tactile reading and writing system for blind people. Instead of reproducing the shapes of printed letters, it represents characters as patterns of raised dots that can be read by touch.

A standard Braille character is encoded in a two-column by three-row cell. The positions are numbered from top to bottom on the left, followed by top to bottom on the right:

```text
1 | ○ ○ | 4
2 | ○ ○ | 5
3 | ○ ○ | 6
```

For these examples, `○` means flat and `●` means raised. Six binary positions produce $$2^6 = 64$$ possible cells. That total includes the completely blank cell, so 63 combinations contain at least one raised dot. These patterns are not all letters: they can represent letters, punctuation, contractions, indicators, and other symbols.

The basic lowercase alphabet looks like this:

```text
     a             b             c             d             e             f             g             h             i             j
1 | ● ○ | 4   1 | ● ○ | 4   1 | ● ● | 4   1 | ● ● | 4   1 | ● ○ | 4   1 | ● ● | 4   1 | ● ● | 4   1 | ● ○ | 4   1 | ○ ● | 4   1 | ○ ● | 4
2 | ○ ○ | 5   2 | ● ○ | 5   2 | ○ ○ | 5   2 | ○ ● | 5   2 | ○ ● | 5   2 | ● ○ | 5   2 | ● ● | 5   2 | ● ● | 5   2 | ● ○ | 5   2 | ● ● | 5
3 | ○ ○ | 6   3 | ○ ○ | 6   3 | ○ ○ | 6   3 | ○ ○ | 6   3 | ○ ○ | 6   3 | ○ ○ | 6   3 | ○ ○ | 6   3 | ○ ○ | 6   3 | ○ ○ | 6   3 | ○ ○ | 6

     k             l             m             n             o             p             q             r             s             t
1 | ● ○ | 4   1 | ● ○ | 4   1 | ● ● | 4   1 | ● ● | 4   1 | ● ○ | 4   1 | ● ● | 4   1 | ● ● | 4   1 | ● ○ | 4   1 | ○ ● | 4   1 | ○ ● | 4
2 | ○ ○ | 5   2 | ● ○ | 5   2 | ○ ○ | 5   2 | ○ ● | 5   2 | ○ ● | 5   2 | ● ○ | 5   2 | ● ● | 5   2 | ● ● | 5   2 | ● ○ | 5   2 | ● ● | 5
3 | ● ○ | 6   3 | ● ○ | 6   3 | ● ○ | 6   3 | ● ○ | 6   3 | ● ○ | 6   3 | ● ○ | 6   3 | ● ○ | 6   3 | ● ○ | 6   3 | ● ○ | 6   3 | ● ○ | 6

     u             v             w             x             y             z
1 | ● ○ | 4   1 | ● ○ | 4   1 | ○ ● | 4   1 | ● ● | 4   1 | ● ● | 4   1 | ● ○ | 4
2 | ○ ○ | 5   2 | ● ○ | 5   2 | ● ● | 5   2 | ○ ○ | 5   2 | ○ ● | 5   2 | ○ ● | 5
3 | ● ● | 6   3 | ● ● | 6   3 | ○ ● | 6   3 | ● ● | 6   3 | ● ● | 6   3 | ● ● | 6
```
{: .braille-alphabet}

Braille does not use duration-based spacing like Morse code. Letters occupy adjacent cells with standardized physical spacing, while a blank cell normally separates words.

There are some interesting patterns in the alphabet:

1. The letters `a` through `j` use only the four upper positions: dots 1, 2, 4, and 5.
2. The letters `k` through `t` repeat those ten patterns with dot 3 added.
3. The letters `u`, `v`, `x`, `y`, and `z` are based on `a` through `e` with dots 3 and 6 added.
4. The letter `w` is the exception. It was added later because `w` was uncommon in French; its pattern is `j` with dot 6 added.

For example, `a` is dots 1 (`⠁`), `k` adds dot 3 (`⠅`), and `u` adds both dots 3 and 6 (`⠥`):

```text
     a             k             u
1 | ● ○ | 4   1 | ● ○ | 4   1 | ● ○ | 4
2 | ○ ○ | 5   2 | ○ ○ | 5   2 | ○ ○ | 5
3 | ○ ○ | 6   3 | ● ○ | 6   3 | ● ● | 6
```

The six positions also give us a few useful combinatorial observations:

- Six cells contain exactly one raised dot; only dot 1 represents a lowercase English letter (`a`).
- Four exactly-two-dot cells contain a vertically adjacent pair: dots 1–2, 2–3, 4–5, or 5–6. Only dots 1–2 represent a lowercase English letter (`b`).
- Three exactly-two-dot cells contain a horizontal pair: dots 1–4, 2–5, or 3–6. Only dots 1–4 represent a lowercase English letter (`c`).

These patterns make the alphabet easier to reason about, but they should not be treated as proof of a specific design motivation.

Codes can still fail: the sender can introduce an encoding error, the reader can make a decoding error, or the message can suffer a transmission error while traveling through its medium.

### Contracted Braille

What was historically called Grade 2 is now usually called **contracted Braille**. It uses cells or groups of cells as contractions for common words and letter combinations. For example, where contraction rules permit it, the cell for `b` (`⠃`) standing alone represents the word “but.” Context and Grade 1 rules distinguish the letter from its contracted meaning.

### Indicators and modes

Some cells act as indicators that change how following cells are interpreted. They are conceptually similar to the Shift key on a keyboard, but each indicator has its own scope.

A single dot 6 is the capital indicator (`⠠`):

```text
1 | ○ ○ | 4
2 | ○ ○ | 5
3 | ○ ● | 6
```

It capitalizes the next letter. Two capital indicators establish capital-word mode, and three can begin a capitalized passage.

Dots 3-4-5-6 form the numeric indicator (`⠼`):

```text
1 | ○ ● | 4
2 | ○ ● | 5
3 | ● ● | 6
```

After this indicator, the cells for `a` through `j` represent the digits `1` through `0`. For example, `⠼⠁` represents `1`. A space, hyphen, or slash terminates numeric mode; a Grade 1 indicator can clarify that a following ambiguous cell should be read as a letter.

The dots 3-4-5-6 can also represent the `ble` contraction in an appropriate contracted-Braille context. The physical pattern is the same, but its function depends on position and context. This is why “indicator” and “mode” are more precise terms than treating the cell as a generic shift code.

## Closing notes

Braille shows how six binary positions can form a compact, structured code. Its alphabet uses reusable patterns, while contractions, indicators, and modes allow the same limited set of cells to represent much more than 26 letters.
