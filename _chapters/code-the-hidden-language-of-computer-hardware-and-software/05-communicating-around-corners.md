---
title: Chapter 5 - Communicating Around Corners
order: 5
date: 2026-08-08
tags: [books, computer-architecture, code-the-hidden-language]
---

## Chapter notes

In the first chapters, we introduced Morse code and the possibility of communicating visually using flashlights. Now imagine we want to communicate with the neighbor next to us, whose window faces the opposite direction. We can no longer see each other's windows, so the flashlight approach is no longer useful.


Maybe we could use mirrors? Yeah, but what happens after dark?

Could we use wires, batteries, switches, and lightbulbs? Instead of sending light through the air, we could send an electrical signal through a cable and use it to light the bulb at the destination.

Imagine we manage to set up wires and batteries across the fence. In each room, we have a lightbulb that can be turned on and off by the sender, along with our own switch to control the lightbulb in the receiver's room:

![Simple open circuit between neighboring rooms]({{ '/assets/images/code_hidden_language_chapter_5/neighbor-simple-open.svg' | relative_url }}){: .center-img }

Great. Now our neighbor needs to build the exact same setup so we can both communicate bidirectionally:

![Bidirectional open circuit between neighboring rooms]({{ '/assets/images/code_hidden_language_chapter_5/neighbor-bidir-open.svg' | relative_url }}){: .center-img }

That's great, but as with most things in engineering, there is room for improvement. Some of the infrastructure seems to be duplicated, so we could remove one wire and have both circuits share the negative connection:

![Simplified bidirectional circuit with a shared common wire]({{ '/assets/images/code_hidden_language_chapter_5/neighbor-simplified-open.svg' | relative_url }}){: .center-img }

A dot shows that the wires meeting at that junction are electrically connected. Wires that cross without a dot are not connected.

This shared connection is called the common. It forms one electrical node connecting both batteries' negative terminals and one terminal of each lightbulb. After this change, let's visually make sure the circuits work as expected by testing all switch combinations except when both are off.

First, we close our switch to let the electricity flow:

![Bidirectional circuit check with the first switch closed]({{ '/assets/images/code_hidden_language_chapter_5/circuit-bi-check-1.svg' | relative_url }}){: .center-img }

As you can see, when our switch closes, conventional current flows from the positive terminal through the switch and the lightbulb. Could it continue toward the other battery? Not while the other switch is open: that branch still has effectively $$\infty$$ resistance and does not form a complete circuit, so the current returns through the common to its battery's negative terminal.

Now we close our neighbor's circuit:

![Bidirectional circuit check with the second switch closed]({{ '/assets/images/code_hidden_language_chapter_5/circuit-bi-check-2.svg' | relative_url }}){: .center-img }

The same thing happens in the opposite direction.

Things get interesting when both circuits are closed. Although sending and receiving at the same time would be difficult, our system is capable of doing both:

![Bidirectional circuit check with both switches closed]({{ '/assets/images/code_hidden_language_chapter_5/circuit-bi-check-3.svg' | relative_url }}){: .center-img }

When both identical circuits are closed, conventional current circulates clockwise around the outside of the diagram: from left to right through the upper branch and from right to left through the lower branch. At each end of the common, the current arriving through one outer branch continues through the other, so no net current flows through the common itself. Both bulbs remain lit.


So we have learned two things here:

1. We were able to replace a $$\frac{1}{4}$$ of the cables by using a common one for the negatives, this is no big deal for these examples but for bigger cables/projects that could mean more money.

2. Once we identify the common part of the circuit, we can remove the wire that represents it and use a connection to Earth (yes, the planet), which is a reasonably good conductor. We call it ground in the US and earth in the UK. This is usually done by driving a copper rod 8 feet long and half an inch in diameter deep into the ground.


This is what our simple one-way diagram would look like with ground (see the new symbol extending from the negative terminal):

![Simple one-way circuit using the ground as a common return]({{ '/assets/images/code_hidden_language_chapter_5/simple-ground.svg' | relative_url }}){: .center-img }


Now what happens when we close the circuit?

![Closed one-way circuit using the ground as a common return]({{ '/assets/images/code_hidden_language_chapter_5/simple-ground-closed.svg' | relative_url }}){: .center-img }

Electrons travel from the Earth through the lightbulb and into the battery, while other electrons leave the battery and enter the Earth. The book describes the Earth as a giant repository, or ocean, of electrons that circuits around the world can draw from and return electrons to.

Also note that the Earth has electrical resistance. With a low voltage, such as 1.5 V, too little current may flow for the circuit to work.

There's another simplification that avoids drawing the ground connection every time we show the voltage source. Instead of drawing the battery and ground, we can do the following:


![Simplified grounded circuit]({{ '/assets/images/code_hidden_language_chapter_5/circuit-ground-simp.svg' | relative_url }}){: .center-img }

We now have a capital $$V$$ that stands for voltage. It represents the wire connected to the battery's positive terminal, with its negative terminal connected to ground.

The book also suggests reading the $$V$$ as *vacuum*: imagine it sucking electrons from the Earth. Ground is also called zero potential because it is chosen as the circuit's 0 V reference point (remember the brick example).


It is also worth mentioning that, as we said earlier, our circuits no longer look like circles after all these changes. However, they are the same circuits we drew at the beginning of this post.

One of the breakthroughs of this chapter is that we are no longer limited to communicating in Morse code only as far as a flashlight can reach, or by other visual constraints. Now we can communicate as far as the wires go—or can we? Even though copper is a very good conductor, it still has resistance. A longer wire has more resistance, so less current flows for the same voltage.

To compensate, we can use a thicker wire, which has less resistance, or increase the voltage while remaining within the safe ratings of the circuit's components.

By using a local switch to control a lightbulb in another house, we have built a simple electrical telegraph and removed the need for a direct line of sight.
