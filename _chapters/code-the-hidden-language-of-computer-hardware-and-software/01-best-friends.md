---
title: Chapter 1 - Best Friends
order: 1
date: 2026-08-06
tags: [books, computer-architecture, code-the-hidden-language]
---

## Chapter notes

Imagine you got a very good friend which happeens to be your neighbor, and you love to spend all day long talking, but when night comes you want to still talk. You don't have yet a phone, so you need to think of a way to communicate to eachother.

You happen to also have both windows lookign at each other across the street, so why not using a flashlight? When it's dark its easy to see it. 

How can we make a flashlight speak? How can we make it produce letters/words information as talking would? Well you already know how to writter letters, so you could build words by reproducing the motions of the pen in paper but with the flashlight. An `I` is just a vertical stroke, an `R` might be a bit more complex... Specially because in paper the traces in the pencil remain visible, with the light it does not.

Maybe do we need a method, easier than the previous method, to represent letters? What about mapping each letter to a nunmber of blinks?


| # of blinks | letter |
|-------------|--------|
|     1       |    A   |
|     2       |    B   |
|    ...      |   ...  |
|     26      |    Z   |


So yeah, some words might be easier to write, specially if the are starting in the beginnig of the alphabet, but if you had to write the word `ZOO` might take a while.

Maybe we can add more types of symbols so the combination of them can bring us faster to other words? And here is when morse comes into play.

Morse code adds two types of boinks, short blink or dot (represented as •) or dash (represented as —). Now the `How are you?` instead of `131` blinks would take `32`. The letters use 26 dots and dashes, and the question mark adds another 6.

Here is how the combination of these two symbols we represent the entire alphabet (note Morse code can also add more symbols other than words/letters). The full mapping is in [Chapter 2 - Codes and Combinations]({{ '/books/code-the-hidden-language-of-computer-hardware-and-software/02-codes-and-combinations/' | relative_url }}).

Morse code is not directly related to computers, but it lets us understand the concept of creaeting a code (an abstraction of natural language in this case) to communicate, similar to other codes that computer use. Code means a way to communicate/transmit this information.

If we think back to the flashlight example, how are se supposed to be sending dots and dashes by just flashing it? Well we could do a very fast flash for a dot, and slightly longer for a dash... But what about separating words? Well, we could do then maybe a longer of a pause that we do between letters. See, designing a code is definitely not easy task and lots of requierements/edge cases need to be take into consdieration.

In standard International Morse code, a dot lasts one unit and a dash lasts three units. The gap between symbols in the same letter lasts one unit, the gap between letters lasts three units, and the gap between words lasts seven units.

Another problems can happen here, how long is a dot and a dash? A fast sender might do a dash the same duration as a dot from a slower sender? This is something both communications have to agree/figure out; what is important that the duration is fixed and its easy to distinct between symbols.


## Closing notes

Nothing special, initial introduction of how to represent language into alternative ways, using codes, in this case Morse that is a variable-length, two-symbol code using dots and dashes, plus timing gaps to separate symbols, letters, and words.
