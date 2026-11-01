# How motors work

You've spun a motor. Now let's find out what's happening inside it.
This page starts from zero: electricity, magnets, and how the two
together make something spin. By the end you'll understand the words
you keep seeing in our code: **current**, **voltage**, **stator**,
**supply**, **torque**, and why we set **current limits**.

There's no code on this page. There are "check yourself" questions, and
at the end, a hands-on exercise: taking apart a real Kraken.

## Electricity: think water in pipes

Electricity is a flow of tiny charged particles (electrons) through a
wire. The easiest way to picture it is **water in pipes**:

| Electricity | Water | Unit | On our robot |
| --- | --- | --- | --- |
| **Voltage** | Water **pressure** pushing the water | volts (V) | The battery pushes with about **12 V** |
| **Current** | How much water **flows** per second | amps (A) | A motor can pull from 1 A to hundreds of amps |
| **Resistance** | How **narrow** the pipe is | ohms (Ω) | A Kraken's coils: just **0.025 Ω**, a very wide pipe |
| **Power** | How much **work** the flow does | watts (W) | volts × amps |

The rule that ties them together is **Ohm's law**:

> **current = voltage ÷ resistance**

More pressure, more flow. Narrower pipe, less flow.

> **Check yourself:** a Kraken X60's coils have a resistance of about
> 0.025 Ω. If you connect 12 V straight across them and nothing else
> happens, how much current flows? (Hold on to that number. It's
> scary, and it comes back later.)

## Magnets: think compass

A **compass** needle is a tiny magnet. It always turns to point north
because the Earth is a giant magnet, and the needle lines itself up
with it. Every magnet has a **north** and a **south** end:

* **Opposites attract:** north pulls on south.
* **Same ends push apart:** north pushes north away.

So if you hold a magnet near a compass and walk it around in a circle,
the needle **follows** it around. Keep that picture in mind. It's
basically a motor.

## Electricity makes magnets

Here's the trick that makes motors possible: **current flowing through
a wire makes a magnet.** Wrap the wire into a coil, and you get a
stronger magnet, called an **electromagnet**. Unlike a normal magnet:

* **More current = stronger magnet.**
* **Reverse the current = swap north and south.**
* **No current = no magnet.**

> **Try it (with a mentor):** put a compass on the table, lay a wire
> across the top of it, and briefly touch the wire's ends to a battery.
> The needle jumps. The current made a magnet strong enough to beat the
> Earth's.

## Putting it together: a brushless motor

A Kraken has two main parts:

* The **stator** ("stays still"): a ring of **copper coils** fixed to
  the motor's case. These are electromagnets.
* The **rotor** ("rotates"): the part attached to the shaft, covered in
  **permanent magnets**.

The motor controller (inside the Kraken) turns the coils on and off in
a pattern that goes **around the ring**, so the "magnet" in the stator
moves in a circle. The rotor's magnets keep chasing it, like the
compass needle chasing the magnet you walk around it. That's rotation.

To switch the right coil at the right moment, the controller has to
know exactly where the rotor is. That's what the **encoder** inside the
Kraken does, and it's the same encoder your code reads speed from.

```
        stator coils (fixed, electromagnets)
          ┌──N──┐
       S ┌┘     └┐ S         the controller moves the N/S pattern
         │ rotor │           around the ring...
       N └┐ (NS) ┘ N
          └──S──┘            ...and the rotor's magnets follow it
```

## Torque comes from current

How hard the motor twists (its **torque**) depends on how strong the
stator's electromagnets are, and that depends on **current**. Double
the current through the coils, double the torque.

Our code knows this number for each motor. Open
[`KrakenX60Motor.java`](https://github.com/Team100/all26/blob/main/lib/src/main/java/org/team100/lib/motor/ctre/KrakenX60Motor.java)
and find `kT()`: about **0.019 newton-meters per amp**. So 100 A of
coil current gives about 1.9 N·m of torque.

## Why it doesn't melt every time: back-EMF

Remember your scary number from the Ohm's law question: 12 V ÷ 0.025 Ω
is about **480 A**. Why doesn't every Kraken pull 480 A all the time?

Because a **spinning** motor is also a **generator**. As the rotor's
magnets sweep past the coils, they push current *backwards*, against
the battery. This push-back is called **back-EMF**, and it gets bigger
the faster the motor spins.

* **Spinning fast, nothing to push:** back-EMF almost cancels the
  battery. Very little current flows. (At its top speed, about 5800
  RPM, a Kraken X60 pulls just a couple of amps.)
* **Stalled** (stuck, not turning): **no** back-EMF. Nothing holds the
  current back except that very wide pipe. Current shoots up toward
  that scary number.

That's the most important idea on this page: **a stalled motor is the
dangerous one.** A motor pushing against a wall, a jammed intake, an arm
pressed against a hard stop: no spin, no back-EMF, huge current.

## Heat

Every bit of current flowing through the coils warms them up. The heat
grows with the **square** of the current:

> **heat (watts) = current × current × resistance**

Double the current and you get **four times** the heat. At 100 A, a
Kraken's coils make about 100 × 100 × 0.025 = **250 W** of heat, more
than a bright old-style light bulb, inside a small metal can.

The copper wire is coated with a thin layer of insulation (enamel).
Get it hot enough and the insulation melts, the coils short together,
and the motor is ruined. You usually find out because of the smell and
the smoke. People joke that electronics run on "magic smoke": once you
let it out, they stop working.

## Stator current vs. supply current

Here's the part that confuses everyone, including adults. A motor has
**two** currents:

* **Supply current:** what flows **from the battery** into the motor
  controller.
* **Stator current:** what flows **through the coils** (the stator).

How can they be different? The motor controller works like a **gearbox
for electricity**. A gearbox trades speed for force: the output turns
slower but pushes harder. The controller does the same with
electricity. It switches the battery on and off thousands of times a
second (that's what **duty cycle** means: the fraction of time it's
on), and it can turn "12 V and a little current" from the battery into
"low voltage and a **lot** of current" in the coils.

Roughly:

> **supply current ≈ duty cycle × stator current**

Example: a Kraken stalled at 10% duty cycle. The coils see about 10% of
12 V, so about 1.2 V. Ohm's law: 1.2 V ÷ 0.025 Ω = **48 A** in the
coils. But the battery only supplies about 10% of that: **about 5 A**.

| Situation | Stator current (coils) | Supply current (battery) |
| --- | --- | --- |
| Spinning freely, fast | Low | Low |
| Speeding up hard | High | High |
| **Stalled, low duty cycle** | **High** | **Low** |

That last row is why we need **both** limits in
[`CurrentLimit`](https://github.com/Team100/all26/blob/main/lib/src/main/java/org/team100/lib/config/CurrentLimit.java):

* The **stator limit** caps the current in the coils. It caps the
  motor's **force** and its **heat**, so it protects the **motor** and
  your mechanism (and fingers).
* The **supply limit** caps what's taken from the battery. It protects
  the **battery** and the **breakers**, and keeps the robot from
  "browning out".

And here's the sneaky part: in that stalled example, the battery only
supplies 5 A, so **the 40 A breaker never trips**, while the motor
quietly heats its coils with about 58 W. Only the **stator limit**
protects the motor from that.

> **Check yourself:**
> 1. An arm is holding a heavy game piece perfectly still. Which current
>    is high, stator or supply? Why?
> 2. A shooter wheel is spinning freely at full speed. Is either current
>    high?
> 3. Why do we use `new CurrentLimit(10, 10)` on a test board?

## Words to know

| Word | Meaning |
| --- | --- |
| Voltage (V) | Electrical "pressure" |
| Current (A) | Electrical "flow" |
| Resistance (Ω) | How hard it is for current to flow |
| Stator | The fixed part with the copper coils |
| Rotor | The spinning part with the permanent magnets |
| Encoder | Sensor that measures the rotor's position and speed |
| Torque | Twisting force, in newton-meters (N·m) |
| Back-EMF | Voltage a spinning motor makes, pushing back against the battery |
| Stall | The motor is powered but can't turn |
| Duty cycle | The fraction of time the controller connects the battery, −1 to 1 |
| Stator current | Current through the coils: makes torque and heat |
| Supply current | Current from the battery |

## Exercise: take apart a Kraken

We have an **old Kraken that's already retired**. Taking it apart is
the best way to see everything on this page for real.

> [!WARNING]
> **Only with a mentor, and only the retired Kraken.** Never open a
> working motor: it may not go back together, and it won't be
> trustworthy on a robot again.
>
> * **The rotor magnets are very strong.** The rotor can snap back
>   into the stator hard enough to pinch fingers. Pull it out slowly
>   and keep fingers off the edges.
> * Keep the magnets away from phones, laptops, and bank cards.
> * Wear **safety glasses**.
> * Take a **photo before every step**, so you know how it came apart.

> **TODO (mentors):** list the tools needed (hex keys, Torx, retaining
> ring pliers, ...) and the order the Kraken comes apart. Add photos of
> each stage to `docs/img/`.

As you go, find and photograph each of these:

1. **The motor controller board** (the Talon FX). Find where the **red
   and black power wires** come in, and where the **CAN** wires
   (yellow and green) come in.
1. **Three thick wires** going from the board to the coils. Why three?
   (Hint: the controller makes the magnet go "around the ring" by
   switching between three groups of coils.)
1. **The encoder.** Look for a small chip near the end of the shaft,
   and a small magnet on the shaft itself. The chip senses the magnet
   to measure where the rotor is.
1. **The stator:** the copper coils. Count how many coils there are.
   Look closely: can you see the thin enamel coating on the wire? Do
   any coils look darkened or burnt?
1. **The rotor:** the magnets on the shaft. Hold a compass near it and
   turn the rotor slowly. What does the needle do? Can you count how
   many north/south poles go by in one turn?
1. **The bearings:** what lets the rotor spin smoothly.

**Checkpoint:** a set of labeled photos of every part above, posted in
Slack, and you can explain to a mentor how the parts work together to
make the shaft spin.

Want to see what too much stator current really does? With a mentor's
approval, try the [advanced experiment: smoke a motor](../advanced/SMOKE_A_MOTOR.md).

Next: [Your first study](07_STUDY.md)
