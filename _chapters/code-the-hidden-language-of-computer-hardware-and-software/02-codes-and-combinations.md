---
title: Chapter 2 - Codes and Combinations
order: 2
date: 2026-08-06
tags: [books, computer-architecture, code-the-hidden-language]
---

# Chapter Notes


Morse code was developed alongside the electric telegraph through the collaboration of Samuel Morse and Alfred Vail. The table below shows International Morse code, which evolved from the earlier American Morse system:


| Letter | Morse code | Letter | Morse code |
|:------:|:----------:|:------:|:----------:|
| A | •— | N | —• |
| B | —••• | O | ——— |
| C | —•—• | P | •——• |
| D | —•• | Q | ——•— |
| E | • | R | •—• |
| F | ••—• | S | ••• |
| G | ——• | T | — |
| H | •••• | U | ••— |
| I | •• | V | •••— |
| J | •——— | W | •—— |
| K | —•— | X | —••— |
| L | •—•• | Y | —•—— |
| M | —— | Z | ——•• |


This table above represents a mapping from alphabetical letter to the code, which is easy, but the otehr way, mapping a code to a letter can be initially hard until you figure out cadence/duration of the symbols the other interlocutor is sending.

Let's take a look on other ways, other than alphabetically ordered tables, can wce organize the codes; for example the number of symbles that it represent:


| Morse Code | Letter |
|:----------:|:------:|
|     •      |    E   | 
|     —      |    T   |


With one symbol, we can represent two letters, that's great, let's keep escalating to two symbols now:

| Morde Code | Letter |
|:----------:|:------:|
|     ••     |   I    |
|     •—     |   A    |
|     —•     |   N    |
|     ——     |   M    |


With two characters we can represent four letters, spoiler, with three we can produce 8 and so on:


| # of symbols | symbols space |
|:------------:|:-------------:|
|      1       |       2       |
|      2       |       4       |
|      3       |       8       |
|      4       |       16      |


And so on, so you see the trend? With exactly `n` symbols, the symbol space contains `2^n` possible sequences. If we include every length from one through four, there are `2 + 4 + 8 + 16 = 30` possible positions in the tree. (Note shortest letters are because they are more common, so they can be typed faster).

Morse is a variable-length code and it is not prefix-free. For example, `E` is `•`, while `I` is `••`. The pause between letters is what tells the receiver whether the first dot is a complete `E` or the beginning of another letter.

Let's see here how we can build a binary tree (huh sounds familiar) with four symbols:

![Four-symbol Morse code tree]({{ '/assets/images/code_hidden_language_chapter_2/4-symbol--tree.svg' | relative_url }}){: .center-img }

This is quite cool, it let's us also understand how common some of the letters are, like the vowels, `E/I/A` and then `O/U`, they are quite common so they can all be represneted within three symbols.


Also, deeper in the tree you can see some extended or accented letters (umlauts and so on).
