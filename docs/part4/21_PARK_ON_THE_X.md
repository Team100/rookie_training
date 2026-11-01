# 🏆 Park on the X

So far your auton always started from a taped spot, so the robot knew
where it began. Real matches aren't that tidy: robots get bumped, wheels
slip, and drivers line up a little wrong.

In this challenge the robot starts **somewhere random** and has to drive
itself to an **X on the floor**. It can only win if it uses **both**
things you learned in this part:

* **localization** ([Where am I?](19_LOCALIZATION.md)): look at an
  AprilTag and work out where it is, and
* **auton** ([Work on auton](20_AUTON.md)): plan a path to the X and
  follow it.

Use one of the **demo bots ("A" or "B")**, on the **RoboRIO**, with the
`main` branch, the same as the rest of Part 4.

> **TODO (mentors):** the demo bots need a **gyro** and a **forward
> camera (Pi + USB camera)** feeding localization for hard mode, plus the
> tag family and size we print. Add the setup here.

> [!CAUTION]
> The robot drives by itself. Clear the floor, keep everyone **behind the
> start line**, and keep one person on the Driver Station with a finger
> on **Space** (e-stop). Start with the drive speed turned **down**.

## Why it matters

In a match, auton starts the instant the field says go. If the robot
*thinks* it's 20 cm to the left of where it really is, every path after
that is 20 cm off too, and it misses the game piece, the goal, or both.
Robots that win auton know where they are **before** they move.

## The course

A mentor sets up:

1. **One or two AprilTags on a wall**, at robot-camera height.
1. **An X of tape on the floor**, somewhere 2 to 5 m from the tags.
1. A card that says where the X is **relative to the tags**, measured
   with a tape measure. For example: *"The X is 3.2 m in front of tag 1
   and 0.8 m to its left. Park facing the tag."*

That card is all you get.

> 📷 **TODO photo:** docs/img/park_on_the_x_course.jpg: the wall tags,
> the X, and a start area, with distances labeled.

## Easy mode: back to the tape

The robot starts on a **taped start spot**, facing a known way. A
mentor tells you where the X is **from the start**.

This is just your [auton chapter](20_AUTON.md) with a target: plan a
path from the start to the X, run it, and measure.

**Checkpoint:** 3 runs in a row end within **20 cm** of the X.

> **Check yourself:** the mentor moves the X twice as far away. Does
> your error get bigger? Why? (Think about
> [odometry drift](19_LOCALIZATION.md#clue-1-the-wheels-odometry).)

## Hard mode: park on the X

Now the start is **random**. Before each run, a mentor puts the robot
somewhere in the start area, **pointing a random way** (but able to see
a tag). The robot doesn't know where it is, and neither do you.

**Goal:** when auton starts, your robot:

1. looks at the tag and works out its **own pose**,
1. plans a path from **wherever it is** to the X,
1. drives there and stops, facing the right way.

<details>
<summary>Hint 1: where is the X, in field coordinates?</summary>

The card gives the X relative to a tag. The robot knows where every tag
is on the field (that's how vision works out the robot's pose). So:
**X pose = tag pose + the offset on the card**. Work it out on paper
first, with a drawing.
</details>

<details>
<summary>Hint 2: when do you plan the path?</summary>

In the auton chapter you planned the path when the command *started*,
from the robot's pose at that moment. That's exactly what you want here,
as long as the pose is already right. What happens if auton starts
before the camera has seen a tag?
</details>

<details>
<summary>Hint 3: the demo bot can't slide sideways</summary>

The demo bot is a **tank drive**: it can drive forward, back, and turn,
but not sideways. A path that needs it to slide won't work. Think about
turning toward the X first, or a path that curves in, and then turning
to the final heading.
</details>

**Checkpoint:** from 3 different random starts, the robot ends within
**20 cm** of the X every time, without anyone touching it.

## Scoring

Measure the error from the **center of the robot** to the **center of
the X**, in **cm**. Write down the heading error in degrees too.

* **Score = total distance error, in cm, over 3 runs** from 3 different
  random starts. **Lower is better.**
* Heading error breaks ties.
* **15 seconds** per run. If the robot hasn't stopped by then, that run
  scores **200 cm**.
* Anyone touching the robot (or an e-stop) also scores 200 cm.

Use the [measuring steps](20_AUTON.md#how-to-measure) from the auton
chapter: tape marks, tape measure, no eyeballing.

### Two kinds of miss

For every run, also write down where the robot **thinks** it ended
(its pose in Glass) and compare it with the tape measure.

| What you see | What's wrong |
| --- | --- |
| The robot **thinks** it's on the X, but it isn't | **Localization:** it doesn't know where it is. Check the camera ([chapter 16](../part3/16_NEW_CAMERA.md)) |
| The robot **knows** it missed the X | **Tracking:** it knows where it is, but didn't drive there well. Check the path and the controller |

This is the same split as in [Work out the error](20_AUTON.md#work-out-the-error).

Post your score, a video, and that table for each run to
**`#rookie_training`**.

### Records

| Mode | Total error (3 runs) | Robot | Who | Date | Slack proof |
| --- | --- | --- | --- | --- | --- |
| Easy | — | | | | |
| Hard | — | | | | |

<details>
<summary>⭐ Stretch: park and shoot</summary>

Put the [bucket](../part3/18_VISION_SHOOTER_CHALLENGE.md) in front of the
X. Park on the X, then shoot 5 balls into it, all in auton.
**Score = balls in ÷ seconds** for the whole run, same as the Shooter
Challenge.
</details>

> **Check yourself:** keep your score. In Part 5 you'll run this exact
> challenge again on the SystemCore. Will it be better or worse? Why?

Next: [Part 5: Switch to the SystemCore](../part5/22_SYSTEMCORE.md)
