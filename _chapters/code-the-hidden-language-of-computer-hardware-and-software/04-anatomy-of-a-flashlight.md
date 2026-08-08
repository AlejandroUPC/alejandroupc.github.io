---
title: Chapter 4 - Anatomy of a Flashlight
order: 4
date: 2026-08-07
tags: [books, computer-architecture, code-the-hidden-language]
---

## Chapter notes

A flashlight serves as a great and simple physical example to explain something that feels slightly more complicated and abstract: electricity.

It is mostly made of a few elements: batteries, a lightbulb, a switch, and some metal conductors and wires. Most flashlights nowadays use light-emitting diodes, aka LEDs, because they are more energy-efficient and last longer.

For the sake of this example, we will be referring to an incandescent bulb. It contains a thin tungsten filament inside a glass bulb with a vacuum or inert gas, which prevents the hot filament from oxidizing and burning out quickly. One electrical contact is attached to the metal base and the other is at the bottom:

![Basic open flashlight circuit]({{ '/assets/images/code_hidden_language_chapter_4/basic-circuit-open.svg' | relative_url }}){: .center-img }

Notice the switch? It controls whether current can flow through our circuit. Here it is open—the conductors are not touching—so no current flows. When it closes, the circuit is completed and the bulb lights up:

![Basic closed flashlight circuit]({{ '/assets/images/code_hidden_language_chapter_4/basic-circuit-closed.svg' | relative_url }}){: .center-img }

A circuit behaves like a loop. In order for current to flow, the loop has to be closed; anything that breaks it stops the flow. In this case, the switch takes care of turning the flashlight on and off.

## Charge and electron flow

Electron theory is one way of understanding electricity through the movement of electrons.

All ordinary matter is composed of atoms, which contain:

- Protons.
- Neutrons.
- Electrons.

Protons and neutrons are bound together in the nucleus. Electrons are not little planets orbiting it in fixed paths; modern atomic models describe them as a probability cloud around the nucleus.

Protons have positive charge, represented by `+`, and electrons have negative charge, represented by `-`. These signs do not represent arithmetic here; they tell us that the two charges are opposites. A neutral atom has equal numbers of protons and electrons, but electrons can sometimes move from one atom or material to another.

The word *electricity* comes from the Greek *elektron*, meaning amber: the hardened, glasslike resin of a tree. Rubbing amber with wool produces static electricity. During this process, electrons transfer from the wool to the amber, so the amber becomes negatively charged and the wool positively charged.

A similar transfer can happen when you walk across a carpet. If charge accumulates on your body and you approach a conductor, such as a metal doorknob, it may suddenly discharge as a spark. The direction of the electron transfer depends on the materials involved.

Lightning is a much larger electrical discharge. Charge separation develops inside a storm cloud and between the cloud and its surroundings; when the electric field becomes strong enough, charge moves rapidly through the air.

In our flashlight, we do not see a spark. The bulb remains lit as long as the circuit is closed and the battery can maintain a potential difference. The circuit's metal conductors already contain mobile electrons, and an electric field drives them around the loop. In metals, the electrons drift from the negative terminal toward the positive terminal. By convention, however, current is described in the opposite direction: from positive to negative.

## Batteries

Batteries use chemical reactions to separate charge and maintain an electric potential difference between their terminals. They do not create spare electrons; the electrons are already present in the battery and the conductors. Once the circuit is closed, the battery's electric field pushes those electrons through the external circuit.

### Batteries in series and parallel

Do you notice that both batteries in our first diagram point in the same direction? They are connected in **series**: the positive terminal of one touches the negative terminal of the other. Their voltages add, so two 1.5 V batteries provide approximately 3 V.

If one of two identical series batteries were reversed, their voltages would oppose each other and the net voltage would be close to zero.

Batteries can instead be connected in **parallel**, with their positive terminals connected together and their negative terminals connected together. Two identical 1.5 V batteries in parallel still provide approximately 1.5 V, but they can provide more capacity and current than a single battery. The same load would be roughly as bright as with one battery and could run longer, though not necessarily for exactly twice as long:

![Closed flashlight circuit with batteries in parallel]({{ '/assets/images/code_hidden_language_chapter_4/basic-circuit-closed-parallel.svg' | relative_url }}){: .center-img }

## Conductors and resistance

Electrons need a conductive path to travel through the circuit. Copper, silver, and gold are good conductors, while materials such as rubber are insulators. That is why wires commonly have a copper conductor covered by rubber or plastic insulation.

Ordinary conductors still have some resistance and lose electrical energy as heat. A longer wire has more resistance, while a thicker wire has less. For a uniform conductor:

$$
R = \rho \frac{L}{A}
$$

Here, $$\rho$$ is the material's resistivity, $$L$$ its length, and $$A$$ its cross-sectional area. Superconductors are a special exception: below a critical temperature, they can have zero DC electrical resistance.

## Voltage, current, and resistance

### Voltage

Voltage, measured in volts, is the electric potential difference between two points: the amount of potential energy per unit of charge.

$$
1\,\mathrm{V} = 1\,\mathrm{J}/\mathrm{C}
$$

A battery can maintain a voltage even when it is not connected to a closed circuit. You can kinda think of a brick: lifting it gives it gravitational potential energy even while it is standing still. In the common water-pipe analogy, voltage is like a pressure difference that can push water, but voltage itself is not something that flows.

### Current

Current is the rate at which electric charge passes a point in a circuit:

$$
I = \frac{dQ}{dt}
$$

It is measured in amperes, aka amps. One ampere is one coulomb of charge per second, equivalent to approximately $$6.24 \times 10^{18}$$ elementary charges passing a point each second. In the water analogy, current is like the amount of water flowing through the pipe per second.

### Resistance

Resistance is the opposition a component offers to electric current, and it is measured in ohms, represented by the uppercase Greek letter $$\Omega$$. In the water analogy, a narrower pipe provides more resistance to the flow than a wider one.

## Ohm's law

For an ohmic component under constant physical conditions, current is directly proportional to voltage and inversely proportional to resistance:

$$
I = \frac{V}{R}
$$

The book sometimes uses `E` for voltage, where `E` refers to **electromotive force**, or emf. I will use `V`.

Let's examine the law with its three variables in different scenarios.

Imagine first having just a 1.5 V battery sitting around:

![Battery Simple]({{ '/assets/images/code_hidden_language_chapter_4/battery-simple.svg' | relative_url }}){: .center-img }

The two terminals are separated, so this is an open circuit. We can model its resistance as approaching infinity, which makes the current approach zero:

$$
R_{\mathrm{open}} \to \infty
\qquad
I \to 0
$$

There is no sustained current through the external circuit.

Let's now add a wire directly from the positive terminal to the negative terminal:

![Battery Wire]({{ '/assets/images/code_hidden_language_chapter_4/battery-wire.svg' | relative_url }}){: .center-img }

This is known as a short circuit. Its resistance is very low, but it is never exactly zero: the battery has internal resistance and so does the wire. The resulting current is therefore very high but finite:

$$
I = \frac{V}{R_{\mathrm{internal}} + R_{\mathrm{wire}}}
$$

That large current can rapidly heat the battery and wire, damage them, or start a fire.

Most useful circuits live somewhere between an open circuit and a short circuit, and they include a load such as a resistor or lightbulb:

![Battery Resistor]({{ '/assets/images/code_hidden_language_chapter_4/battery-resistor.svg' | relative_url }}){: .center-img }

An incandescent bulb's filament is deliberately thin and resistive. Electrical energy dissipated in the filament heats it until it glows. Its resistance actually changes as its temperature rises, but we can simplify the example by treating it as a fixed $$4\,\Omega$$ load.

If we connect two 1.5 V batteries in series, they provide 3 V. The current through our simplified bulb is:

$$
I = \frac{3\,\mathrm{V}}{4\,\Omega} = 0.75\,\mathrm{A}
$$

There will be 0.75 A flowing through the circuit, equivalent to approximately $$4.68 \times 10^{18}$$ electrons passing a point each second.

## Power

Power is the rate at which electrical energy is transferred, measured in watts. It is calculated from voltage and current:

$$
P = V \cdot I
$$

Using the previous values, the circuit's power is:

$$
P = (3\,\mathrm{V})(0.75\,\mathrm{A}) = 2.25\,\mathrm{W}
$$

For a resistor, the same relationship can also be written as $$P = I^2R$$ or $$P = V^2/R$$. This electrical power becomes heat and, in an incandescent filament, light.

Electricity bills charge for energy, usually measured in kilowatt-hours (kWh), rather than power alone. Using an LED instead of an incandescent bulb reduces the energy needed to produce a similar amount of light.

## Closing the loop

The switch has the important responsibility of deciding whether current can flow. A closed switch completes the circuit and lets current flow; an open switch breaks the circuit and stops it. With that final piece, we have accounted for every basic part of our flashlight.
