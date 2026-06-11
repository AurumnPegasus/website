---
title: "First Integrated Circuits"
date: 2026-05-19
draft: true
tags:
  - semiconductors
  - chipwar
---

I started going deeper into how chip-economics work when I realised I should probably get to know its history better. My first easy-read was Chip War to understand politics around chip production. While I have just completed a few chapters of it, it took me a few weeks to get through it. It took time since I went deep into every invention the book mentions, and what motivated it.

I am writing this blog to document my journey, and explain to others how the first ICs were made. 

## Edison's Lightbulb

It all started with a lightbulb. A bulb works on the simple idea that when you heat up anything to really high temperature, it produces light. A modern bulb uses tungsten filament (insanely high melting point) which is a thin, coiled around piece of wire to get higher resistance. By doing this, you see light. An important factor of the lightbulb is that the glass-tube it is in has to be a vacuum (nowdays inert gas). This is because if there are even trace amount of Oxygen in the tube, the filament will oxidise rapidly at high temps and burn out. 

![Lightbulb Diagram](/images/lightbulb_simple_with_wires.svg)

Nothing new till now, everyone knows how lightbulb works. One day, Edison (or someone else, knowing Edison), noticed that after some time, the glass darkened everywhere *except* one sharp, clean stripe cast by the positive leg of the filament. That meant something electrically charged was flying off the
negative side and getting blocked by the positive leg on its way to the glass. 

Edison poked at this. He sealed a small metal plate inside the bulb, near the filament, and found that a current flowed across the empty vacuum to the plate, but only when the plate was connected to the positive side. Flip it, and nothing. Electricity was jumping through empty space, and only in one direction.

## Fleming Valve

Edison patented his strange one-way current and promptly moved on. The effect sat on the shelf for about twenty years. Then, in 1904, John Ambrose Fleming dusted it off and turned it into the first true electronic device: the thermionic diode, better known as the Fleming valve.

The purpose of a diode is simple: act as a one-way valve for current. The construction is basically Edison's experimental bulb: a filament, a metal plate near it, all sealed in a vacuum. The filament is heated until it glows, boiling off a cloud of electrons (thermionic emission again).

Now the trick:
- If the plate is *positive* relative to the filament, it pulls that electron cloud across the vacuum gap. Current flows.
- If the plate is *negative*, it pushes the electrons back. Since the plate is cold, there is no electron cloud around it so nothing to send the other way. No current flows. 

Only the hot side can emit. That single asymmetry is what makes it one-way.

![Fleming Valve Diagram](/images/diode.png)

The cleanest demonstration of this is via the AC -> DC half-wave rectifier. Connect the bulb to the AC source, and a plate near the filament (at 0V). When the filament near the plate (near-side) is -vely charged, and far from the plate (far-side) is positively charged, current flows. It happens because there is an electron cloud which forms, which goes to both the far side and the plate, since there is a PD in both cases. On the other hand when near side is positive, there is no electron cloud at the plate (where the PD exists), hence no current.

The big loop is where the action is. It runs from the filament, *across the vacuum gap*, to the plate, then out through the AC source and the load, and back to the filament.  Notice the AC source isn't connected along the filament. It sits between the plate and the filament. So it doesn't tilt the filament's internal gradient at all. Instead, it lifts and drops the entire filament relative to the plate, by swings far bigger than that few-volt gradient. The filament's two ends always differ by their few volts, but the AC throws both of them together far above or far below the plate.

That's all a rectifier needs. During the half-cycle when the plate sits above the filament, the plate pulls the electron cloud across the gap and current
flows around the big loop, through the load. During the other half-cycle, the plate sits below the filament and shoves the cloud back, and since the plate is cold, it has no cloud of its own to send the other way. The big loop is simply broken for that half-cycle. Feed in AC, and only one half of every wave makes it through: one-directional current. Half the wave survives, hence, half-wave rectifier.

This mental image is useful into how the diode's circuit diagram symbol was created as well (a triangle pointing into a line). The line represents the filament, and the triangle represents the plate. Electrons move from the filament to the relatively positively charged plate. (Current moves in the opposite direction from the electron flow). Hence, the current can only flow in the direction of the triangle (from plate to the filament, because electrons flow in opposite direction).

By the way, its possible to create a Full-Wave Rectifier using multiple of these diodes as well. I will not go into details for it here

## Lee De Forest's Vacuum Tube

The next upgrade was pretty simple, instead of a diode, why not add a third electrode? It was of the shape of a sparse zigzag of wire, called a **grid** (later a mesh), sitting between the filament and the plate. Lee De Forest took Fleming's valve and created a Triode. 

The concept was that the grid acted as a sort of amplifier / gate. If the grid was at a slight negative voltage, the grid would repel the electron cloud from the filament, and lesser electrons could pass on through to the plate. Alternatively, if the grid was at a slight positive voltage, a lot more electrons would pass through the grid and on to the plate. If you can envision this, it was essentially a small PD inducing a much larger current in a different circuit! This was an amplifier. 

And one more thing hides inside the amplifier. If a small grid voltage can smoothly throttle the current, then a slightly bigger one can slam it shut entirely: drive the grid negative enough and *no* current flows; relax it and the current floods through. The triode wasn't just an amplifier, it was an electrically
controlled switch. 

People later optimised the triode too. The best configuration of the triode was a cylinder. A cyliderical mesh encircled the mesh, which itself was covered by a cylinderical plate. This configuration allowed for a much larger current amplification factor than a planar mesh.

![Electromechanical Relay](/images/electromechanical_relay.png)

It seems pretty important even at this point of the story, but to give more context one must understand what it was actually replacing. Before Vacuum Tube (Triode), there existed Electromechanical Relay. The relay is old, older than a lightbulb, and credited to Joseph Henry in 1835. It solved the same problem of amplification via simple electromagnetism. It gave the input current a simple job, flow through a coil of copper wire wrapped around some iron core, turning it into a magnet. The magnet then pulls down a small iron lever, and the tip of the lever, when pulled, closes a compeltely seperate circuit with its own, fresh, full strength battery. Pretty simple, a small input current leads to a stronger current in a different circuit. 

But the relay has an obvious weakness: it's a machine. A physical lever has to swing through the air, which takes milliseconds. The contacts spark, the moving parts wear out, and a room full of them clatters like a typewriter factory.

Which is what makes de Forest's tube the real beginning of electronics. The triode does exactly the relay's job: weak signal in, strong current controlled, but nothing inside it moves except electrons. It has no mechanical parts which are often prone to failure and wear out. Where the relay needs milliseconds to swing its arm, the grid's electric field redirects the electron stream in nanoseconds

## William Shockley's Field Effect Transistor (FET)

The jump from electromechanical relay to the vacuum tube was huge (almost a century huge). For about forty years, the vacuum tube ran the world. Radio, long-distance telephony, radar, and eventually the first electronic computers (ENIAC) were all powered by vacuum tubes. There were still multiple problems, since the vacuum tube was essentially powered by a light bulb. It was hot, the filament had to glow, by design, so every tube wasted watts just existing. It was fragile, glass bulbs, thin wires and like every lightbulb, it burned out. This required frequent replacement of vacuum tubes from the main circuitry.

A new solution was needed, one which did not depend on so many components. William Shockley was tasked with finding a solid-state solution, without so many mechanical components prone to failure. This is where the FET (Field Effect Transistor) was born. Semiconductors were all the hype even then, a family of misfit elements which were somewhere between an insulator and a conductor. Doping a semiconductor to make it behave like a conductor was also a known thing.

Shockley used a thin n-type doped semiconductor, and connected its two short sides to a battery. In this configuration, the semiconductor acts as a normal (but worse) conductor. Then, he introduced a metal plate which hovered slightly over the semiconductor's long side, seperated by an insulator. When this side was connected with a PD, it created a Field Effect that either pulled or repelled electrons from the semiconductor. This was the Field Effect Transistor. The idea was that when connected across a positive PD, the electrons would be pulled towards the top (forming a thin line of electrons) where conduction would be stronger. And when it had negative PD, then the field would push the electrons away, starving this channel. If the slab was thin enough, a negative PD would stop any electron movement across the semiconductor. 

I repeat, if a positive, much weaker PD across the plate, then the much stronger current would flow. If it was much weaker negative PD, then almost no current would flow. He had invented a solid state replacement of the vacuum tube.

Shockley then ran the experiment, it was supposed to work theoretically. But, when he made the connections and set the voltage, he observed "nothing measurable" across the semiconductor slab. He couldn't explain it. Later, his colleagues explained it as a problem with "Surface States". What happens is that Si in the semiconductor sits in a configuration of tetrahedron in the lattice. But, the topmost layer of this lattice are only connected to 3 Si atoms, not 4. This is the trap. When electrons are pulled upwards when the voltage is applied, the first few electrons gets trapped by these topmost layer of Si atoms. These immovable charges create a negative field of their own, which reduces the effect of the positive PD applied to the plate, and repels the electrons coming to the top layer. Hence, there is no electron channel created at the top layer, leading to no measurable difference in current conducted across the slab.

## Bardeen and Brattain
