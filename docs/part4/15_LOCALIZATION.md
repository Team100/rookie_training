# Where am I? (localization)

Before a robot can drive itself to a spot, it has to know **where it
is**. Working that out is called **localization**. Our robot combines
two kinds of clues: its **wheels** and its **cameras**.

## Pose: where and which way

A robot's **pose** is three numbers: **x** and **y** (where it is on the
field, in meters) and **θ**, theta (which way it's facing). The
[auton chapter](16_AUTON.md#pose-where-am-i) explains the field's
coordinates in detail; skim that section now.

## Clue 1: the wheels (odometry)

If you know how far each wheel turned, you can work out how far the
robot moved. Add up all those little moves, and you know where you are.
That's **odometry**, like counting your steps with your eyes closed.

It's fast and smooth, but it **drifts**: wheels slip, carpet squishes,
and every small mistake adds up. After a few laps, the robot can be
quite wrong about where it is.

A **gyro** (a sensor that measures turning) helps with which way the
robot is facing.

> **Try it:** with the demo bot on a clear floor, put tape at its start,
> drive a square, and come back to the tape. Compare where the robot
> **thinks** it is (in Glass, or the field view) with where it really
> is. How far off is it? Drive the square three more times. Does the
> error grow?

## Clue 2: the cameras (vision)

From [How our vision works](../part3/14_VISION.md): when a camera sees
an AprilTag, and the robot knows where that tag is on the field, it can
work out where the robot must be. That doesn't drift, but it's
**jumpy** (every picture is a little different), **late** (about a tenth
of a second old), and only works when a tag is in view.

## Putting them together

So we use both:

* **Odometry** for smooth, instant updates, every cycle.
* **Vision** to **nudge** the estimate back toward the truth whenever a
  tag is seen. Far-away tags are less trustworthy, so they nudge less.
* Because a vision answer is late, the robot applies it to where it
  **was** when the picture was taken, then replays the wheel moves
  since then.

The team's notes on this are in the
[localization README](https://github.com/Team100/all26/blob/main/lib/src/main/java/org/team100/lib/localization/README.md),
and the math is in
[MATH.md](https://github.com/Team100/all26/blob/main/lib/src/main/java/org/team100/lib/localization/MATH.md).
Read the README; the math is optional.

> **Check yourself:**
> 1. Why not just use vision, and skip odometry?
> 2. Why not just use odometry, and skip vision?
> 3. A camera sees a tag 8 m away. Should that change the robot's pose a
>    lot or a little? Why?

**Checkpoint:** you can explain to a mentor, without notes, how the
robot combines its wheels and its cameras, and why.

> **TODO (mentors):** add an exercise on the demo bot or comp robot
> that shows the pose jumping when a tag comes into view.

Next: [Work on auton](16_AUTON.md)
