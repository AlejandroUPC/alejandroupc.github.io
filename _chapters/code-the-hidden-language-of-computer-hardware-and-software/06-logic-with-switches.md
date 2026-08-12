---
title: Chapter 6 - Logic with Switches
order: 6
date: 2026-08-11
tags: [books, computer-architecture, code-the-hidden-language]
---

## Chapter notes

What is the truth? Aristotele thought there was some kind of logic on it, defining logic as the emans of analysing the language in the search of this truth, in his case ussing syllogism:

```text
All men are mortal;
Socrates is a man;
Hence, Socrates is mortal.
```

Syllogism assume that if two premisses are assumed to be correct, the two first sentences in the previous example, a valid conclusion can be deduced.

### Intro to boolean algebra

Fast forward (ignoring Leibinz contributions), Geroge Boole came up with Boolean algebra. He wanted to express the laws of reasoning using mathematics and use them to investigate how the human mind works.

In conventional algebra usually letters represent numbers and are called *operands*, and are usually combind with some *operators* such as $$+$$ or $$\times$$:

$$
A = 3 \times (B + 5)
$$


There are some rules (or properties?) in conventional algebra such as that addition and multiplication are commutative (order does not matter):

$$
A + B = B + A
\\
A \times B = B \times A
$$

They are also associative (when same op we can combine order without altering result):

$$
A + (B + C) = (A + B) + C
\\
A \times (B \times C) = (A \times B) \times C
$$

And finally, multiplication is distribitive over addition (multiplying a number by a sum gives the same result as adding their products together):

$$
A \times (B + C) = (A \times B) + (A \times C)
$$

In this chapter, Boolean algebra is first interpreted as working with classes rather than conventional numbers. A class is nothing but a group of things (or a set). Later, we will also interpret its values as true or false.


### Cats example

Cats can be either male or female, so we could split them in two classes $$M$$ and $$F$$ for Male and Female respectively.

We could also classifiy them as $$T$$ tan cats, $$B$$ black cats, $$W$$ white cats or $$O$$ other colors.

Also, if whether they are $$N$$ neutered or $$U$$ unnetured.

In Boolean algebra we have two main symbols $$+$$ that represents union, everything of one class combined to the other, $$B+W$$ is all cats that are black OR white.

The $$\times$$ symbol represents the interesction of twho classes, an AND, between the two classes, $$T \times F$$ is all tanned AND female cats.

**Note**: union can also be represented as $$\cup$$ and intersection as $$\cap$$, but we stay on the $$+$$ and $$\times$$.

Another interesting property is the distributiveness of $$+$$ over $$\times$$ that does not hold for conventional algebra, so we can write:


$$
W + (B \times F) = (W + B) \times (W + F)
$$

This means that white cats together with black female cats is the same set as cats that are either white or black and, at the same time, either white or female.

The symbol of universe represents the complete population of a set, e.g the union of female and male cats should be all of the cats we can always think of:

$$
M + F = 1
$$

The same way as if we take all females from the cat universe we get the males:

$$
M = 1 - F
$$

The same way that, given our categories of colors all of the cats combined are:

$$
T + W + B + O = 1
$$

The intersection of any class with the universe is the original class, so the intersection of the universe and female cats is the class of female cats.

$$
1 \times F = F
$$

The next symbol is $$0$$, which means an empty class. For example, there is no intersection between the male and female classes as they are defined in this example:

$$
M \times F = 0
$$

This has some interesting properties, such as the intersection of the empty class and female cats being the empty class:

$$
0 \times F = 0
$$

And the union is the very same group:

$$
0 + F = F
$$

The law of contradiction is also an important concept: something cannot be both itself and its complement, which can be written as $$A \times (1 - A) = 0$$. In our example, a cat cannot be both $$M$$ and $$F$$ at the same time because we defined those classes as mutually exclusive.

### Proving Socrates mortality

Now going back to the initiall syllogism from Aristoteles:

```markdown
All persons are mortal;
Socrates is a person.
```

We now define the following classes:

$$
P = all persons
\\
M = Mortal things
\\
S = all Socrates (n=1)
$$

So the first syllogism is, all personals are mortals means that all the persons and all the mortals are all the persons:

$$
P \times M = P
$$

Note that the case:

$$
P \times M = M
$$

Is wrongm because not all the mortal things are persons (trees, animals, ...).

The second sentence *Socrates is a person* can be defined as:

$$
S \times P = S
$$

Now we can start playing, we know from the first equation $$P = P \times M$$, so we can substitute from the above:

$$
S \times (P \times M) = S
$$

And by applying association:

$$
(S \times P) \times M = S
$$

And we know from the first Socrates expression that $$S \times P = S$$, so we simplify using substitution:

$$
S \times M = S
$$

Now we can read that the intersection of the class Socrates, and all of the class of mortal things S is S, so Socrates is mortal.

If we ever came across $$S \times M = 0$$, this would mean that Socrates is not mortal in this model.



### Advanced cats example

Now imagine a slightly more complex example where we go into a shop and we ask for a amle cat, neutrered, either white or tan; or a female cate, neutered of any color but white; or any cat as long as its black.

Could we write this suing Boolean algebra? Yes we do:

A male cat, neutered that is white or tan can be expressed as $$ M \times N \times (W + T) $$.

A female cat, neutered that is not white can be written as $$F \times N \times (1 - W)$$.

And finally, any black cat as $$ B $$.

Combining all of this into:

$$
(M \times N \times (W + T)) + (F \times N \times (1 - W)) + B
$$

**Note**: Already done but $$\times$$ can be expressed as `AND` and $$+$$ as `OR`.

### Boolean tests

A variation of Boolean algebra where the letters refer to properties or characterstics of attributes of cats, and might be assigned to $$1$$ representing yes or $$0$$ representing no.

Now imagine the first cat we are shown is an unneutered tan male, we can substitute each attribute in our formula as:

$$
(1 \times 0 \times (0 + 1)) + (0 \times 0 \times (1 - 0)) + 0
$$

Now we need to resolve this to find the final answer and solve above we need to understand how to resolve $$\times$$ with the values $$1$$ and $$0$$:

$$
0 \times 0 = 0
\\
0 \times 1 = 0
\\
1 \times 0 = 0
\\
1 \times 1 = 1
$$

So we can see that `AND` is only true if both values are $$1$$.


Similar for `OR`:

$$
0 + 0 = 0
\\
0 + 1 = 1
\\
1 + 0 = 1
\\
1 + 1 = 1
$$

So on this case, if any value is $$1$$ the outcome is $$1$$.

Both of those could be represed in its own tables:

|AND|0|1|
|:-:|:-:|:-:|
|0|0|0|
|1|0|1|


|OR|0|1|
|:-:|:-:|:-:|
|0|0|1|
|1|1|1|



So finally, we can know if this cat belongs to the mix of attributes:

$$
(1 \times 0 \times (0 + 1)) + (0 \times 0 \times (1 - 0)) + 0 =  0 + 0 + 0 = 0
$$

Its a $$0$$ so it does not, they bring the next one which is a neutred white female, so we replace the values again:

$$
( 0 \times 1 \times (1+ 0)) + (1 \times 1 \times (1 - 1)) + 0 = 0
$$

So again this does not the cat we are looking, the next one comes a neutered gray female:

$$
( 0 \times 1 \times (0 + 0)) + (1 \times 1 \times (1 - 0)) + 0 = (0) + (1) + (0) = 1
$$

That's it! Thats our cat.

### Joining the dots

Now, imagine we can move all of this boolean logic and join it what we learnt with our circuits so far, and interprets the $$1$$ and $$0$$ as voltage/ no voltage and all of the characteristics of the boolean tests could be modeled as switches.

The cool thing, and taking the switches as examples we see that two in series they behave as a AND:

![Two switches connected in series]({{ '/assets/images/code_hidden_language_chapter_6/line-switches.svg' | relative_url }}){: .center-img }


We can see that switches in series implement `AND`: the lightbulb lights only when both switches are closed.


In a similar fashion, switches in parallel implement `OR`: the lightbulb lights when either or both switches are closed.

![Two switches connected in parallel]({{ '/assets/images/code_hidden_language_chapter_6/para-switches.svg' | relative_url }}){: .center-img }

Now, can we build $$(M \times N \times (W + T)) + (F \times N \times (1 - W)) + B$$ into a circuit? We do indeed can:

![Full Boolean cat-selection circuit]({{ '/assets/images/code_hidden_language_chapter_6/full-cat-circuit.svg' | relative_url }}){: .center-img }

**Note**: We are negating $$W$$, which we had in our equation as $$1 - W$$. In the diagram we used `!`, but it is usually represented as $$\overline{W}$$ and read as `NOT W`. The `!W` switch is closed when $$W$$ is false and open when $$W$$ is true.
