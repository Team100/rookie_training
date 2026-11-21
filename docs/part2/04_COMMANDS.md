# [Optional] Commands vs. `if()`: the gripper bot

In [Control a motor from code](../part1/08_MOTOR_CODE.md#step-12-build-a-grabber)
you built a grabber with `if()` statements in `teleopPeriodic()`. That
works, and it's the best way to *learn*, because you can see everything
that happens each cycle.

But the team's real robot code doesn't look like that. It uses
**commands**. In this chapter you'll build a two-motor **gripper** both
ways, add a feature that's hard with `if()`, and see for yourself why
commands exist.

This chapter is **optional**, but if you want to work on the real
robot code, do it: everything there is commands.

You need:

* the grabber from [1.8, Step 12](../part1/08_MOTOR_CODE.md#step-12-build-a-grabber),
  or at least Steps 9 to 11 (position control)
* the [shooter bot](03_DEMO_BOT.md), where Step 6's hints already
  mentioned commands

Make a **new study** for this chapter, with `lib` included, like
[Your first study](../part1/07_STUDY.md). Kraken or REV motors both
work; use whichever the gripper bot has.

## Safety first

A gripper is a **pinch point**: it's built to close on things, and it
can't tell a ball from a finger.

* Keep the current limit **low** (10 amps) while you're testing.
* Hands **out** of the jaws before you say "Enabling!".
* One person drives; everyone else watches the jaws, not the laptop.

## The gripper bot

> **TODO (mentors):** which bot this is, a photo, and the two motors'
> CAN IDs and types.
> 📷 **TODO photo:** `docs/img/gripper_bot.jpg`: the gripper bot, open
> and closed, with the two motors labeled.

| Motor | Kind | CAN ID |
| --- | --- | --- |
| Left jaw | TODO | TODO |
| Right jaw | TODO | TODO |

## Step 1: The `if()` gripper, plus one new trick

**Goal:** the same gripper as in Part 1, with one new button:

* **A**: close. **B**: open.
* **X**: **grab and release**: close, wait **2 seconds**, then open by
  itself.

Write it the way you know: fields, and `if()`s in `teleopPeriodic()`.

**Checkpoint:** all three buttons work. Then try these, and **write
down what happens**:

1. Press X, then press **B** in the middle of the 2 seconds.
1. Press X, then press **A** in the middle.
1. Press X twice, quickly.

> **Check yourself:** how did you do "wait 2 seconds"? You can't stop
> and wait inside `teleopPeriodic()`: what would happen to everything
> else the robot does in those 2 seconds?

> **Check yourself:** count the fields and `if()`s you needed just for
> X. Now imagine the gripper also needs to work in **auton**: "drive to
> the ball, grab it, drive back". Which of your code could auton reuse?

<details>
<summary>Hint 1: waiting without stopping</summary>

Don't wait; **remember**. When X is pressed, remember the time (try
`Timer.getFPGATimestamp()`, in seconds). Every cycle, check: has it
been 2 seconds since then? If so, open.
</details>

<details>
<summary>Hint 2: what is it doing right now?</summary>

You need a variable that says what the gripper is doing: "idle",
"grabbing", "releasing". Each button, and the clock, moves it from one
to another. Draw it as boxes and arrows first.
</details>

## Commands, in plain words

Here's the other way to think about it. Instead of checking every
button every cycle, you **describe the robot once**, at startup:

| Word | What it means | For the gripper |
| --- | --- | --- |
| **Subsystem** | A class that **owns** some hardware. | `Gripper`, which owns both jaw motors. |
| **Command** | Something a subsystem can **do**. It starts, runs every cycle, and may end. | "close", "open", "grab and release" |
| **Requirement** | Which subsystem a command uses. **Only one command can use a subsystem at a time**: starting a new one **interrupts** the old one. | "open" and "close" both require `Gripper`, so they can never fight. |
| **Trigger** (or **binding**) | "When *this* happens, run *that* command." | "While A is held, run close." |
| **Default command** | What a subsystem does when nothing else is using it. | "Hold still." |
| **Scheduler** | The part that runs every command, every cycle. | You already call it: it's the `CommandScheduler` line in your `robotPeriodic()`. |

The `if()` version and the command version both end up calling the
same motor methods every 20 ms. The difference is **who decides**:
your `if()`s, or the scheduler, following rules you set up once.

Read [WPILib: What is "command-based" programming?](https://docs.wpilib.org/en/stable/docs/software/commandbased/what-is-command-based.html)
before you go on.

## Step 2: A `Gripper` subsystem

**Goal:** a new class, `Gripper`, that **owns** both motors. `Robot`
creates one `Gripper` and has **no motor fields** of its own.

Put the OPEN and CLOSED angles inside `Gripper` too: they're facts
about the gripper, and nothing else needs them.

**Look in:** [WPILib: Subsystems](https://docs.wpilib.org/en/stable/docs/software/commandbased/subsystems.html).
Which class should `Gripper` extend?

**Checkpoint:** it builds, and the motors' "created" messages still
print when the code starts.

<details>
<summary>Hint 1</summary>

`public class Gripper extends SubsystemBase`. Move the motor creation
from `Robot`'s constructor into `Gripper`'s. What does `Gripper`'s
constructor need to be given, so it can make its motors' logs?
</details>

## Step 3: `open()` and `close()` commands

**Goal:** `Gripper` has two methods, `open()` and `close()`, that each
**return a `Command`**. Each command sends its target to both motors
every cycle.

**Look in:** [WPILib: Commands](https://docs.wpilib.org/en/stable/docs/software/commandbased/commands.html).
`SubsystemBase` has small methods that make a command for you **and**
set its requirement automatically. Find them.

**Checkpoint:** it builds. (Nothing moves yet: nobody runs the
commands.)

> **Check yourself:** why does `close()` **return** a command, instead
> of closing the gripper right away?

<details>
<summary>Hint 1</summary>

Look for `run(...)` on `SubsystemBase`. It takes a little piece of code
(a **lambda**, like `() -> doSomething()`) and makes a command that
runs it every cycle, requiring this subsystem.
</details>

<details>
<summary>Hint 2</summary>

Write a private method that sends one target to both motors. Then
`close()` is just `run(...)` calling it with CLOSED.
</details>

## Step 4: Bind the buttons

**Goal:** A closes and B opens, like before, but `teleopPeriodic()` is
**empty**. The buttons are connected **once**, in `Robot`'s
constructor.

**Look in:** [WPILib: Binding commands to triggers](https://docs.wpilib.org/en/stable/docs/software/commandbased/binding-commands-to-triggers.html).
A `Trigger` can be made from anything that answers true or false. Your
`DriverXboxControl` has `a()` and `b()`.

**Checkpoint:** A and B work, and there are no `if()`s for buttons
anywhere.

Now try both kinds of binding, and write down what happens when you
**let go** of the button:

* `whileTrue(...)`
* `onTrue(...)`

> **Check yourself:** with `onTrue`, the `close` command never ends by
> itself. So how does pressing B stop it? (Look back at the
> **Requirement** row in the table.)

> **Check yourself:** what does the gripper do right after you enable,
> before you press anything? Is that what you want?

<details>
<summary>Hint 1</summary>

`new Trigger(m_driver::a).onTrue(m_gripper.close());`. The `::` means
"this method, to call later", not "call it now".
</details>

## Step 5: A default command

**Goal:** when no button command is running, the gripper does
something sensible on purpose, not by accident.

Decide first: when nothing is pressed, should it **open**, **hold where
it is**, or **stop pushing**? Think about the robot carrying a ball.
Write your choice and your reason in a comment.

**Look in:** the Subsystems page again, for **default commands**.

**Checkpoint:** enable without touching anything: the gripper does what
you chose. Then switch A and B back to `whileTrue` and check that
letting go returns to your default.

> **Check yourself:** which binding, `whileTrue` or `onTrue`, makes
> sense for your default? Why?

## Step 6: Grab and release, the command way

**Goal:** X does "close, wait 2 seconds, open", built from commands,
with **no new fields**.

**Look in:** [WPILib: Command compositions](https://docs.wpilib.org/en/stable/docs/software/commandbased/command-compositions.html).
You want one thing, **then** another, **then** another.

Build it, and press X. It probably doesn't work yet. Before you open
the hints, work out why.

**Checkpoint:** X grabs, waits, and releases. Then repeat the three
tests from Step 1 (B during X, A during X, X twice) and compare with
what you wrote down.

> **Check yourself:** what does B do to the X command halfway through?
> Did you write any code to make that happen?

<details>
<summary>Hint 1: why it gets stuck</summary>

A **sequence** starts the next command only when the current one
**ends**. Your `close()` runs every cycle, forever. When does it end?
</details>

<details>
<summary>Hint 2: making it end</summary>

Give `Gripper` a method that says whether the jaws are **at** their
target: read both encoders and compare with the target, with a small
tolerance. Then `close().until(m_gripper::atTarget)` ends when it gets
there. Another way: `withTimeout(...)`.

Careful: if the gripper closes on a ball, it **never reaches** CLOSED.
Which ending is safer, then?
</details>

<details>
<summary>Hint 3: the pieces</summary>

`Commands.sequence(...)`, `Commands.waitSeconds(...)`, and your own
`close()` and `open()`.
</details>

## Step 7: Compare

**Goal:** decide for yourself which way is better, and when.

Fill in this table, from your two versions:

| | `if()` version (Step 1) | Command version (Steps 2 to 6) |
| --- | --- | --- |
| Lines in `teleopPeriodic()` | | |
| Extra fields just for X | | |
| What B does during X | | |
| Could auton reuse "grab and release"? | | |
| Easier to see what happens each cycle? | | |
| Easier to add a fourth button? | | |

**Checkpoint:** post your filled-in table to `#rookie_training`, with
one sentence: **when would you still use `if()`?**

> **Check yourself:** there's still an `if`-like question hiding in the
> command version: "is it at the target?" Where did it go?

## Send it in

Commit, push, and open a PR ([how](../part1/02_FORK_WORKFLOW.md)). In
the description, include your table from Step 7. Ask a mentor to
compare your `Gripper` with a subsystem in the team's robot code: what's
the same, and what's different?

Next: [Part 3: Where am I? (localization)](../part3/01_LOCALIZATION.md)
