---
title: Chapter 7 - Telegraphs and Relays
order: 7
date: 2026-08-12
tags: [books, computer-architecture, code-the-hidden-language]
---

## Chapter notes

Being able to communicate instantly across the world is relatively recent in human history. In the early 1800s, communication could be fast over short distances or travel long distances slowly, but not both.

Fast communication was limited by how far your voice could be heard or how far a visual signal could be seen. Letters could cover much greater distances, but they took a long time to arrive.

The telegraph---a word that can be translated as *far writing*---was based on a fairly simple idea: doing something at one end of a wire causes something to happen at the other. This is similar to the light-bulb examples from previous chapters, although practical electric light bulbs did not exist yet. What made the telegraph possible was the phenomenon of *electromagnetism*.

Leaving the history aside, the basic idea is that wrapping insulated wire around a soft iron bar creates an electromagnet. When current flows through the wire, its magnetic field magnetizes the iron core. When the current stops, the core loses most of its magnetism.

![Simple electromagnet]({{ '/assets/images/code_hidden_language_chapter_7/simple-electro.svg' | relative_url }}){: .center-img }


The long coil of thin wire also provides electrical resistance, limiting the current so the circuit does not behave like a direct short circuit. This electromagnet is the foundation of the telegraph: operating a switch at one end of the wire causes a mechanical action at the other end.

In Morse's first public long-distance demonstration in 1844, an operator sent a message by controlling the circuit. Long signals represented dashes and short signals represented dots. The receiver used an electromagnet to move a stylus, producing a paper copy of the message.

Operators soon realized that they could understand a message by listening to the receiver instead of reading the paper, which was much faster. This led to the telegraph sounder: an armature pulled by an electromagnet when current arrived. It made a *click* when pulled and a *clack* when released. A short interval between the click and clack represented a dot, while a longer interval represented a dash.

There was still a problem: the longer the wire, the greater its resistance and the weaker the received signal became. One solution was to place stations along the route and have an operator receive and retransmit each message. In other words, a person acted as a repeater.

But why not automate that step? Instead of asking a person to operate another key, the weak incoming signal could energize an electromagnet that closed a fresh circuit. That new circuit would then retransmit the same signal at full strength.

Morse's system used this idea in an electromagnetic *relay*, also called a *repeater*. The incoming signal controls one circuit, while the relay contacts switch a separate output circuit.

With no current at the input, the relay remains open:

![Relay with an open circuit]({{ '/assets/images/code_hidden_language_chapter_7/relay-open.svg' | relative_url }}){: .center-img }

When current arrives at the input, the coil becomes an electromagnet and pulls the armature down. This closes the output contacts, allowing current from the separate output supply to flow through the output circuit. The contacts remain closed for as long as the input coil is energized:

![Relay with a closed circuit]({{ '/assets/images/code_hidden_language_chapter_7/relay-closed.svg' | relative_url }}){: .center-img }

Relays are fascinating devices. With enough of them, you can build logical switching networks and, ultimately, a computer.
