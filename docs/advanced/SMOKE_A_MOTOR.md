# ADVANCED: Smoke a motor

> [!CAUTION]
> **Mentor approval required. Do not start this without a mentor's
> sign-off below, and a mentor present the whole time.** This
> experiment deliberately overheats a motor until it's destroyed. It
> involves high current, hot metal, and smoke.

In [How motors work](../part1/05a_HOW_MOTORS_WORK.md) you learned that a
**stalled** motor has no back-EMF, so stator current gets huge, and heat
grows with current × current. In this experiment you'll prove it: you'll
stall a motor, measure its currents and temperature, and see what
happens when nothing limits the stator current. You'll also see why the
**breaker doesn't save the motor**, and the **stator limit** does.

## Before you start

You need to have finished:

* [How motors work](../part1/05a_HOW_MOTORS_WORK.md)
* [Control a motor from code](../part1/07_MOTOR_CODE.md): a study that
  sets a duty cycle and a `CurrentLimit`
* [Glass and power-draw tests](../part2/09_GLASS_POWER.md): graphing
  supply current, stator current, and temperature

And you need a motor that a mentor has declared **scrap**. Write
"SCRAP: SMOKE TEST" on it in big letters, so nobody ever puts it on a
robot.

## Mentor sign-off

A mentor fills this in (in your PR description or on paper) **before**
anything is powered:

* [ ] The motor is scrap and labeled.
* [ ] The location is ventilated (outdoors, or by an open door), and
      nothing flammable is near the motor.
* [ ] A **fire extinguisher** (ABC or CO2) is within reach, and
      someone knows how to use it.
* [ ] The motor is **clamped** to the bench and its shaft is locked so
      it can't turn. Nobody holds it by hand.
* [ ] Everyone is wearing **safety glasses** and standing at least an
      arm's length back.
* [ ] One person keeps a finger on **Enter** (disable) the whole time,
      and everyone knows where the **main breaker** is.
* [ ] The mentor has reviewed the student's code and predictions.
* [ ] This is a **test board**, never a competition robot.

Mentor: _______________ Date: _______________

> **TODO (mentors):** pick the location, the clamp setup for locking
> the shaft, and how the scrap motor gets disposed of afterwards.

## Step 1: Predict

Use what you learned. The coils of a Kraken X60 have a resistance of
about 0.025 Ω (it's `R()` in
[`KrakenX60Motor`](https://github.com/Team100/all26/blob/main/lib/src/main/java/org/team100/lib/motor/ctre/KrakenX60Motor.java)).
With the shaft locked, there's no back-EMF, so for each duty cycle work
out:

* the voltage on the coils (duty cycle × 12 V)
* the **stator** current (Ohm's law)
* the **supply** current (about duty cycle × stator current)
* the **heat** in the coils (current × current × resistance)

| Duty cycle | Coil voltage | Stator A | Supply A | Heat W |
| --- | --- | --- | --- | --- |
| 0.02 | | | | |
| 0.05 | | | | |
| 0.10 | | | | |
| 0.15 | | | | |

> **Check yourself:** at which of these would the 40 A breaker in the
> motor's PDP slot trip? At which would the coils get hotter than a
> soldering iron?

## Step 2: Set up the code and the graphs

Start from your study from [Control a motor from code](../part1/07_MOTOR_CODE.md).

1. Make the duty cycle easy to change in steps, for example with
   buttons on the Xbox controller, so you can go from 0.02 to 0.05 to
   0.10 without redeploying.
1. Start with a **safe** stator limit: `new CurrentLimit(10, 40)` (stator
   10 A, supply 40 A).
1. In Glass, graph **stator current**, **supply current**,
   **temperature (C)**, and the battery **voltage** (see
   [Glass](../part2/09_GLASS_POWER.md#make-a-graph)).

**Checkpoint:** a mentor has read your code, and the graphs show live
values with the motor disabled.

## Step 3: The control run (limit ON)

With the stator limit at **10 A**, step through your duty cycles,
holding each one for about 10 seconds.

* What does the stator current do as you raise the duty cycle? Where
  does it stop rising?
* What does the temperature do?

Write down what you see. This is what the stator limit does for every
motor on the robot, every match.

## Step 4: The smoke run (limit OFF)

Now raise the stator limit a lot (a mentor picks the number, for
example `new CurrentLimit(150, 40)`), deploy, and do the steps again.
At the highest duty cycle, **hold it** and watch the temperature climb.

Stop (press **Enter**) when **any** of these happens:

* you see or smell smoke
* the motor stops on its own (the controller has a built-in heat
  protection; note the temperature when it happens)
* the mentor says stop

Then: **don't touch the motor**. It can be well over 100 °C. Don't
breathe the smoke. Let it cool for at least 30 minutes before anyone
handles it.

> **TODO (mentors):** Krakens have built-in thermal protection, so the
> controller may shut itself off before the coils smoke. Record what
> actually happened, and what temperature it reached, so the next group
> knows what to expect.

## Step 5: Explain it

Compare your graphs to your predictions, and answer these in your PR or
in Slack:

1. How close were your predicted stator currents to the measured ones?
   What might explain the difference? (Hint: wires and connectors have
   resistance too, and resistance goes **up** as copper gets hot.)
1. Did the **breaker** trip? Look at the supply current. Why not?
1. In Step 3, what stopped the motor from overheating?
1. Name two situations on a real robot where a motor is stalled at a
   low duty cycle for a long time. (Think: arms, climbers, intakes.)
   What would happen without a stator limit?

If the motor smoked, take it apart (see
[the take-apart exercise](../part1/05a_HOW_MOTORS_WORK.md#exercise-take-apart-a-kraken))
and photograph the coils. Compare them to a healthy motor's coils.

**Checkpoint:** your graphs, predictions table, and answers are posted,
and a mentor has signed off on the explanation.
