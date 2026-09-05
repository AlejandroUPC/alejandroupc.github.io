---
title: Chapter 8 - Relays and Gates
order: 8
date: 2026-08-23
tags: [books, computer-architecture, code-the-hidden-language]
---

## Chapter notes

A digital computer can be defined, very broadly, as Boolean algebra implemented with electrical signals. This mix of math and hardware gives us logic gates, which are the building blocks of many important parts inside a computer. In our relay circuits, a gate performs a simple Boolean operation by controlling whether an output receives current.

Previously we went through the cat example and ended up with the following Boolean expression:

$$
(M \times N \times (W + T)) + (F \times N \times (1 - W)) + B
$$

Here, $$+$$ means `OR`, $$\times$$ means `AND`, and $$1 - W$$ means `NOT W`. Because every variable can only be `0` or `1`, subtracting it from `1` gives us its opposite.

Before moving forward, let's simplify it. The first two groups share the value $$N$$. We can rearrange the terms and define two temporary variables:

$$
X = M \times (W + T)
\\
Y = F \times (1 - W)
$$

We can then write the same expression as:

$$
(N \times X) + (N \times Y) + B
$$

Now we can apply the distributive law and factor out $$N$$:

$$
(N \times (X + Y) ) + B
$$

Replacing $$X$$ and $$Y$$ with their original values gives us:

$$
(N \times ((M \times (W + T)) + (F \times (1 - W)))) + B
$$

Now we can draw an equivalent circuit, which looks slightly simpler than the one in the previous chapter:

![Simplified equivalent circuit]({{ '/assets/images/code_hidden_language_chapter_8/simplified-circ.svg' | relative_url }}){: .center-img }


Although this is slightly simpler, there is still some room for improvement. Instead of having separate switches for male and female, for example, we could use one switch: closed means one option and open means the other. We could do something similar for white and non-white.

Let's make a control panel where every variable in our circuit is a switch. If the light turns on, we found a matching cat; if it stays off, no luck yet. Here is my very questionable attempt at drawing it with ASCII:

```plaintext
  ┌─-───────────────────────────────────────────┐
  │               CAT SELECTOR                  │
  │                                             │
  │   SEX              NEUTERED                 │
  │   Female ◉───○ Male    No ○───◉ Yes         │
  │       F / M                N                │
  │                                             │
  │   COAT COLOR                                │
  │   ○ White   ○ Tan   ● Black                 │
  │     W        T        B                     │
  │                                             │
  │                    ┌──────────────────┐     │
  │                    │  ● CAT FOUND     │     │
  │                    └──────────────────┘     │
  └─-───────────────────────────────────────────┘
```

As you can see, there are two selector switches for sex and neutered, plus three choices for color. In this particular example $$B = 1$$, so we find a cat. Remember that $$B$$ is joined to the rest of the expression with `OR`, so a black cat matches regardless of sex or whether it is neutered.

In computer terminology, the control-panel switches are an input device. Those inputs pass through the circuit, which produces an output—the light bulb—when they match its logic.


Now, instead of using our fingers to control every switch, let's use relays. Besides letting a small signal control another circuit, relays give us a way to operate switches using current and electromagnetism. To build our cat circuit we first need to reproduce the Boolean operations we know so far: `AND` and `OR`.

Relays can be connected in series or in parallel to perform these operations. The book credits Claude (lol) Elwood Shannon for connecting relay circuits with Boolean algebra, while also mentioning earlier related work by the Japanese engineer Akira Nakashima.


When current energizes a relay's coil, we say the relay has been triggered. We can also keep chaining relays; when the output of one controls the next, they are said to be cascaded.


### AND with relays

First, let's connect two normally open relay contacts in series:

![AND relay circuit with both inputs open]({{ '/assets/images/code_hidden_language_chapter_8/and-relay-all-open.svg' | relative_url }}){: .center-img }

Now, for the sake of the exercise, let's energize only the first relay (the top-left one):

![AND relay circuit with the first input closed]({{ '/assets/images/code_hidden_language_chapter_8/and-relay-top-closed.svg' | relative_url }}){: .center-img }

When current is sent to both inputs, both relays close. Because their contacts are connected in series, this completes the circuit and activates the output:

![AND relay circuit with both inputs closed]({{ '/assets/images/code_hidden_language_chapter_8/and-relay-both-closed.svg' | relative_url }}){: .center-img }

So yeah, once both relays are triggered, current finally reaches the bulb. The bulb stays off when both inputs are `0` or when only one input is `1`; it turns on only when both inputs are `1`. Does this ring a bell? The relays are behaving like an `AND` gate.

The `AND` gate is represented like this:

![AND logic gate]({{ '/assets/images/code_hidden_language_chapter_8/and-gate.svg' | relative_url }}){: .center-img }

So the previous circuit could have been written as:

![Relay circuit represented with an AND gate]({{ '/assets/images/code_hidden_language_chapter_8/circit-and.svg' | relative_url }}){: .center-img }


Also note that the inputs and output do not always need to be switches and bulbs. Gates can be combined, and they can even have more than two inputs. A three-input `AND`, for example, outputs `1` only when all three inputs are `1`.

Here, `1` means current is present and `0` means no current is present. The output only has current when both inputs do:

| A | B | Output |
|:-------:|:-------:|:------:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |



### OR with relays

Now let's connect the normally open contacts in parallel (spoiler: this gives us `OR`):

![OR relay circuit with both inputs open]({{ '/assets/images/code_hidden_language_chapter_8/relay-or-open.svg' | relative_url }}){: .center-img }

If we trigger only the top relay, one complete path already exists, so the bulb lights:

![OR relay circuit with the first input closed]({{ '/assets/images/code_hidden_language_chapter_8/or-relay-top-closed.svg' | relative_url }}){: .center-img }


This also works with only the bottom relay or with both relays. The circuit is behaving like `OR`, whose logic-gate symbol is:

![OR logic gate]({{ '/assets/images/code_hidden_language_chapter_8/or-logic-gate.svg' | relative_url }}){: .center-img }


Like `AND`, an `OR` gate can have multiple inputs. Its output is `1` when at least one input is `1`.

The table shows the parallel paths clearly: current at either input is enough for current to reach the output.

| A | B | Output |
|:-------:|:-------:|:------:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |


### The inverter

The relays we have been drawing use a moving common contact that can connect to one of two fixed contacts—a double-throw arrangement. At rest, the common contact touches the **normally closed** contact. When current energizes the coil, the magnetic field moves it to the **normally open** contact. So yeah, “normally” just describes the relay while its coil is not energized.

We can use the normally closed contact to invert a signal. In this first illustration the input has no current, but the resting contact completes the output circuit and the bulb still lights:

![Inverted relay with an open input and active output]({{ '/assets/images/code_hidden_language_chapter_8/inverted-open.svg' | relative_url }}){: .center-img }

When the input is enabled, the relay moves away from the normally closed contact, breaking the output circuit:

![Inverted relay with an energized input and inactive output]({{ '/assets/images/code_hidden_language_chapter_8/inverter-closed.svg' | relative_url }}){: .center-img }


This is an inverter, and it performs the Boolean `NOT` operation. It can feel strange that the output has current when the input does not, but remember that the input only controls the relay. The output circuit has its own power source.

![Inverter logic gate]({{ '/assets/images/code_hidden_language_chapter_8/inverter.svg' | relative_url }}){: .center-img }

| Input | Output |
|:-----:|:------:|
| 0 | 1 |
| 1 | 0 |


### Building the cat-selection circuit

Going back to our control panel, we can now use a single selector for `M/F`:

![Male and female control-panel switch using an inverter]({{ '/assets/images/code_hidden_language_chapter_8/m-f-controlp.svg' | relative_url }}){: .center-img }


The same selector signal goes down two paths: one path uses the signal directly and the other passes it through an inverter. If we call the direct signal $$F$$, then $$M = 1 - F$$. When $$F = 1$$, $$M = 0$$; when $$F = 0$$, $$M = 1$$. There is no way for both values to be `1`, which is exactly what we want from a two-position selector.


We could do the same for neutered/not neutered. For color, we can use a three-position selector for $$B$$, $$W$$, and $$T$$ so that only one color is selected at a time.


Now, how do we convert the expression:


$$
(N \times ((M \times (W + T)) + (F \times (1 - W)))) + B
$$


The trick is to translate each Boolean operation into a gate: every $$+$$ becomes an `OR`, every $$\times$$ becomes an `AND`, and $$1 - W$$ becomes `NOT W`:

![Complete circuit built from logic gates]({{ '/assets/images/code_hidden_language_chapter_8/very-ugly-gaates-circuit-lol.svg' | relative_url }}){: .center-img }

**This is probably the ugliest diagram I have ever posted from this book, and it definitely needs the legend, but oh well.**


Keep in mind that this already uses a lot of gates, and each gate contains one or more relays. A computer contains much, much more than this.


### NOR with relays

The previous `AND` and `OR` circuits used normally open contacts: triggering a relay closed a path. We can also use normally closed contacts, which carry current while the relay is not triggered and open when it is triggered.

![NOR gate relay circuit]({{ '/assets/images/code_hidden_language_chapter_8/nor-circuit.svg' | relative_url }}){: .center-img }

Although `NOR` may sound similar to `OR`, these normally closed contacts are connected in series like an `AND`. Current only reaches the output while both inputs are `0`; triggering either relay opens its contact and breaks the path.

| A | B | Output |
|:-------:|:-------:|:------:|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

The name `NOR` means “NOT OR”: its behavior is the exact opposite of an `OR` gate. Its logic-gate symbol is:

![NOR logic gate]({{ '/assets/images/code_hidden_language_chapter_8/nor-logic-gate.svg' | relative_url }}){: .center-img }

We could also build the same operation by placing an inverter after an `OR` gate.


### NAND with relays

We can build a similar circuit with normally closed contacts in parallel. Current keeps flowing as long as at least one path remains closed:

![NAND gate relay circuit]({{ '/assets/images/code_hidden_language_chapter_8/nand-circuit.svg' | relative_url }}){: .center-img }

This produces the opposite of `AND`: current reaches the output unless both relays are triggered. `NAND` means “NOT AND,” and its gate looks like this:

![NAND logic gate]({{ '/assets/images/code_hidden_language_chapter_8/nand-logic-gate.svg' | relative_url }}){: .center-img }

With normally closed contacts in parallel, the output has current while either path remains closed. Only when both inputs are `1` do both contacts open and stop the current:

| A | B | Output |
|:-------:|:-------:|:------:|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |


### The buffer

This might be the simplest one. A buffer passes its input value through unchanged:

![Buffer relay circuit]({{ '/assets/images/code_hidden_language_chapter_8/buffer-circuit.svg' | relative_url }}){: .center-img }

Its logic-gate symbol looks like an inverter without the circle:

![Buffer logic gate]({{ '/assets/images/code_hidden_language_chapter_8/buffer-logic-gate.svg' | relative_url }}){: .center-img }

The buffer does not change the logical value: when current enters, current leaves; when no current enters, no current leaves.

| Input | Output |
|:-----:|:------:|
| 0 | 0 |
| 1 | 1 |


Its most common job is to restore a signal and provide enough drive for whatever comes next, similar to what we needed for the telegraph. This is especially useful when a circuit fans out and one signal has to control several inputs.

A buffer also introduces a tiny propagation delay. Usually that delay is something to account for, although a chain of buffers can sometimes be used when a deliberate delay is needed.


### Deriving the “other color” signal

When we designed the color selector, we omitted an explicit switch for “other.” We want this signal to be `1` only when the cat is not black, not white, and not tan:

$$
Other = \overline{B} \times \overline{W} \times \overline{T}
$$

We can create it using three inverters followed by a three-input `AND` gate. Sometimes the separate inverter symbols are skipped and their circles are drawn directly on the inputs of the `AND` gate:

![Logic gate for detecting a cat with another color]({{ '/assets/images/code_hidden_language_chapter_8/other-color-logic-gate.svg' | relative_url }}){: .center-img }

The `Other` output is `1` only when none of the three known colors is selected:

| Black | White | Tan | Other |
|:-----:|:-----:|:---:|:-----:|
| 0 | 0 | 0 | 1 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 0 |

Selecting any known color makes `Other` equal to `0`, as expected. Because our selector allows only one color at a time, rows containing more than one `1` would not normally happen, but the table still shows how the logic behaves for every possible input.

### Universal gates and De Morgan's laws

If we had to pick only one of the six gate types seen so far, we should pick either `NAND` or `NOR`. Using multiple copies of either type, we can reproduce all the other gates.

Let's start with an inverter made from `NAND`. A `NAND` has two inputs, so we split one signal and connect the exact same value to both. The split does not change or copy the logic into something new; it simply sends the same `0` or `1` to both input pins:

![Inverter created by connecting one signal to both inputs of a NAND gate]({{ '/assets/images/code_hidden_language_chapter_8/inverter-from-nand.svg' | relative_url }}){: .center-img }

Because both NAND inputs receive the same signal, only the `0, 0` and `1, 1` input combinations are possible. The output is therefore the opposite of the original input:

| Input | Input 1 | Input 2 | Output |
|:-----:|:-------:|:-------:|:------:|
| 0 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 |

So yeah, `NAND(A, A)` is the same as `NOT A`. This ability to create negation is the important bit.

De Morgan's laws show us how negation interacts with `AND` and `OR`:

$$
\overline{A} \times \overline{B} = \overline{A + B}
\\
\overline{A} + \overline{B} = \overline{A \times B}
$$

When a negation moves from every input to the output, the gate changes: `AND` becomes `OR`, or `OR` becomes `AND`.

The first law says that an `AND` gate with both inputs inverted behaves exactly like a `NOR` gate. In normal words, “it is not raining and it is not snowing” means the same as “it is not true that it is raining or snowing.” The output is `1` only when both original inputs are `0`:

![An AND gate with inverted inputs behaves like a NOR gate]({{ '/assets/images/code_hidden_language_chapter_8/and-negated-in-same-as-nor.svg' | relative_url }}){: .center-img }

| A | B | NOT A | NOT B | AND output | NOR output |
|:-:|:-:|:-----:|:-----:|:----------:|:----------:|
| 0 | 0 | 1 | 1 | 1 | 1 |
| 0 | 1 | 1 | 0 | 0 | 0 |
| 1 | 0 | 0 | 1 | 0 | 0 |
| 1 | 1 | 0 | 0 | 0 | 0 |


The second law says that an `OR` gate with both inputs inverted behaves exactly like a `NAND` gate. “Either I am not big or I am not strong” means the same as “it is not true that I am both big and strong”:

![An OR gate with inverted inputs behaves like a NAND gate]({{ '/assets/images/code_hidden_language_chapter_8/or-negated-in-is-nand.svg' | relative_url }}){: .center-img }

| A | B | NOT A | NOT B | OR output | NAND output |
|:-:|:-:|:-----:|:-----:|:---------:|:-----------:|
| 0 | 0 | 1 | 1 | 1 | 1 |
| 0 | 1 | 1 | 0 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 | 1 |
| 1 | 1 | 0 | 0 | 0 | 0 |


The important idea here is that we are choosing one **type** of gate, not one single physical gate. We can use as many copies of that type as we need. So yeah, a computer could theoretically be built using only `NAND` gates, or only `NOR` gates, without manufacturing a different gate for every operation.

Why can we not do the same using only `AND` or only `OR`? Because they are missing negation. They can combine signals, but neither one can produce `NOT A`. Even if we split one signal and connect it to both inputs, nothing changes:

$$
A \times A = A
$$

$$
A + A = A
$$

With `NAND` and `NOR`, however, the result is negated. Connecting the same signal to both inputs makes either gate behave like an inverter:

| A | AND(A, A) | OR(A, A) | NAND(A, A) | NOR(A, A) |
|:-:|:---------:|:--------:|:----------:|:---------:|
| 0 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 0 | 0 |

This little difference is what makes `NAND` and `NOR` universal. Once we can create `NOT`, we can add or remove inversions and use De Morgan's laws to recover the other operations.

Using only `NAND`, for example:

```text
NOT A   = NAND(A, A)

X       = NAND(A, B)
A AND B = NAND(X, X)

A OR B  = NAND(NAND(A, A), NAND(B, B))
```

The first construction joins both inputs to make `NOT`. The second negates a `NAND` output again, so the two negations cancel and leave us with `AND`. The third first negates `A` and `B`, then uses De Morgan's law to produce `OR`.

We can do the same thing using only `NOR`:

```text
NOT A   = NOR(A, A)

X       = NOR(A, B)
A OR B  = NOR(X, X)

A AND B = NOR(NOR(A, A), NOR(B, B))
```

Once we can make `NOT`, `AND`, and `OR`, the remaining gates follow directly: `NAND` is `NOT AND`, `NOR` is `NOT OR`, and a buffer can be made with two inverters because `NOT(NOT A) = A`. So these constructions really do give us all six gate types from only one.

This also means that `AND` together with `NOT` is universal, and `OR` together with `NOT` is universal too, but then we need two different gate types. `NAND` already packs `AND` and negation into one type, while `NOR` packs `OR` and negation into one type.

And that is really the whole point: being universal does not mean that one physical gate somehow does everything by itself. It means we can keep combining copies of the same gate type until we reproduce every other operation. Real circuits often mix gate types because it is simpler, but if we had to manufacture only one, `NAND` or `NOR` would be enough.
