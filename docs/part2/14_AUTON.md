# Work on auton

In this chapter you'll write your first autonomous routine ("auton"):
**drive forward 2 meters, then drive back to where you started.** You'll
test it in the simulator first, then on a real robot.

Along the way you'll learn the words everyone uses when talking about
robot motion: **pose**, **transform**, **heading**, **course**,
**waypoint**, **trajectory**, and **controller**.

Before you start, you should have finished
[Setup](../part1/01_SETUP.md), [Fork workflow](../part1/02_FORK_WORKFLOW.md), the
[simulator chapter](../part1/08_SIMULATOR.md), and
[Switch to the SystemCore](10_SYSTEMCORE.md). This chapter uses the
`2027` branch.

## Part 1: The ideas

### Pose: where am I?

A **pose** is where the robot is *and* which way it's facing. In WPILib
it's a [`Pose2d`](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/geometry/pose.html):

```java
Pose2d p = new Pose2d(4.0, 2.5, Rotation2d.kZero);
//                    x    y    heading
```

* **x, y** are in **meters**, measured on the field.
* **heading** is the way the front of the robot points, as a
  `Rotation2d`. Angles are in **radians** and counterclockwise is
  positive. `Rotation2d.kZero` faces +x; `Rotation2d.kPi` faces −x.

Where is (0, 0)? WPILib puts the origin in a corner of the field, with
+x pointing away from the blue driver station and +y to the left. Read
the [WPILib coordinate system page](https://docs.wpilib.org/en/stable/docs/software/basic-programming/coordinate-system.html)
and find the picture.

> **Check yourself:** a robot at `new Pose2d(4, 2.5, Rotation2d.kPi)`
> drives "forward". Does x get bigger or smaller?

Our code keeps track of the robot's current pose using wheel odometry,
the gyro, and AprilTags seen by the Pi cameras. You get it with
`machinery.m_drive.getState().pose()`.

### Transform: move relative to me

A **transform** (`Transform2d`) is a move described *from the robot's
point of view*: "2 meters ahead, 0 to the left, no turn."

```java
Transform2d twoAhead = new Transform2d(2, 0, Rotation2d.kZero);
Pose2d there = here.plus(twoAhead);
```

`here.plus(twoAhead)` gives the pose 2 m in front of the robot,
**whichever way it's facing**. This is how you write an auton that works
from any starting spot.

> **Check yourself:** if `here` is facing −x (`Rotation2d.kPi`), what is
> `there.getX()` compared to `here.getX()`?

### Heading vs course

Our robots use swerve drive, which means they can drive in one direction
while facing another.

* **Heading:** which way the robot *faces*. This is part of the pose.
* **Course:** which way the robot *travels*.

When you drive forward, heading and course are the same. When you drive
*backward* without turning around, the heading stays the same and the
course points the opposite way. This matters for the return trip.

In our library, a course is a
[`DirectionSE2`](https://github.com/Team100/all26/blob/2027/lib/src/main/java/org/team100/lib/geometry/se2/DirectionSE2.java).
The easy one to use is `DirectionSE2.irrotational(angle)`, which means
"travel toward `angle` without spinning."

### Waypoint: a place to pass through

A **waypoint**
([`WaypointSE2`](https://github.com/Team100/all26/blob/2027/lib/src/main/java/org/team100/lib/geometry/se2/WaypointSE2.java))
is a pose plus the course the robot should have there, plus a "scale":

```java
new WaypointSE2(pose, course, 1)
```

* `pose`: where to be and which way to face
* `course`: which way to be travelling at that moment
* `scale`: how strongly the course bends the path. Use `1` for now. Bigger
  numbers make wider curves, as in
  [`UTurnWithRotation`](https://github.com/Team100/all26/blob/2027/comp/src/main/java/org/team100/frc2026/auton/UTurnWithRotation.java).

### Trajectory: the path plus a schedule

You give the planner a list of waypoints, and it makes a
**trajectory**: a smooth path through them, plus *when* the robot should
be at each point along it, respecting how fast the robot can go and
accelerate.

```java
TrajectorySE2 t = planner.restToRest(waypoints);
```

`restToRest` means the robot is stopped at the start and the end.
Read the [trajectory README](https://github.com/Team100/all26/blob/2027/lib/src/main/java/org/team100/lib/trajectory/README.md)
for how it works inside.

### Controller: stay on the trajectory

Wheels slip and batteries sag, so the robot never follows the plan
perfectly. A **controller** compares where the robot *is* to where the
trajectory says it *should be*, and corrects. It is set up for you in
[`Autons.java`](https://github.com/Team100/all26/blob/2027/comp/src/main/java/org/team100/frc2026/auton/Autons.java)
(look for `FullStateControllerSE2`). Read the comments on each number.
You don't need to change them yet.

### Commands: doing things in order

Each drive step is a **command**, a thing the robot does until it's
finished. WPILib can chain commands:

```java
command = sequence(stepOne, waitSeconds(1), stepTwo);
```

Read [WPILib command compositions](https://docs.wpilib.org/en/stable/docs/software/commandbased/command-compositions.html).

## Part 2: Read the example

Open
[`comp/.../auton/AheadTwoMeters.java`](https://github.com/Team100/all26/blob/2027/comp/src/main/java/org/team100/frc2026/auton/AheadTwoMeters.java).
It already does the first half of your job. Read it slowly and answer
these questions (write the answers down, or tell a mentor):

1. In `t1`, what is `p1`? Where does it come from? (Hint: look at
   `initialize()` in
   [`DriveWithTrajectoryFunction`](https://github.com/Team100/all26/blob/2027/lib/src/main/java/org/team100/lib/subsystems/se2/commands/DriveWithTrajectoryFunction.java).)
1. What does `DirectionSE2.irrotational(p1.getRotation())` mean in words?
1. Why is the course the same at both waypoints?
1. What would change if the `Transform2d` were `(0, 2, Rotation2d.kZero)`?
1. What does `n1.until(n1::isDone)` do?
1. The comment says "Note slow constraints here." Find `slow()` in
   `TimingConstraintFactory`. Why is slow a good idea for a training
   auton?

Then look at
[`LeftSequenceExample.java`](https://github.com/Team100/all26/blob/2027/comp/src/main/java/org/team100/frc2026/auton/LeftSequenceExample.java),
which runs **two** trajectories in a row. That's the shape you need.

## Part 3: Plan "There and Back"

You need two trajectories:

* **Out:** from the start, 2 m ahead. (You've seen this.)
* **Back:** from wherever you ended up, to **the start pose**.

Think about this before you write code. For the return trip you could
do either of these:

* **(a)** "2 meters behind wherever I am now" (a transform), or
* **(b)** "the exact pose where I started" (a remembered pose).

> **Check yourself:** suppose the robot actually stops 1.95 m out, not
> 2.00 m. Where does (a) leave it? Where does (b) leave it? Which one is
> "back to where it started"?

To do (b), the code has to **remember the start pose** when the out-trip
begins, and use it when the back-trip begins.

And for the course on the way back: the robot is still facing the
same way as before, but it's travelling the opposite way. What angle is
"opposite of the robot's heading"? (Look at how `UTurnWithRotation`
computes `d2`.)

## Part 4: Write it

1. Sync your fork and make a branch, e.g. `alice/there-and-back`
   ([how](../part1/02_FORK_WORKFLOW.md)).
1. Open the `comp` folder in VS Code.
1. In `comp/src/main/java/org/team100/frc2026/auton/`, create
   `ThereAndBack.java`. Start by copying `AheadTwoMeters.java`, and
   rename the class and `name()` (to "There And Back").
1. Change it so that:
   * there's a field to remember the start pose
   * there are two trajectory functions, `out` and `back`
   * `out` remembers its starting pose, then goes 2 m ahead
   * `back` goes from its starting pose to the remembered pose, with the
     reversed course
   * the command runs `out` and then `back` in a `sequence`
   * `trajectoryFns()` returns both, so the dashboard can draw them
1. Register it in `Autons.java`, next to `AheadTwoMeters`.
1. Build (WPILib → **Build Robot Code**). Fix errors until it's
   `BUILD SUCCESSFUL`.

Stuck? Use the hints at the bottom. Try each one before opening the next.

## Part 5: Test in the simulator

1. WPILib → **Simulate Robot Code**, pick `comp`, choose **Sim GUI** only.
1. Open the field view (see the
   [simulator chapter](../part1/08_SIMULATOR.md)).
1. Find the **Auton Command** chooser (`NetworkTables → SmartDashboard →
   Auton Command`) and pick **There And Back**. While the robot is
   disabled, the field should draw both trajectories.
1. Click **Autonomous** in the Robot State widget.

**Checkpoint:** the robot drives 2 m, stops, drives back, and ends up
where it started, facing the same way.

Try these experiments, and predict the result *before* you run each one:

* Drive the robot somewhere else in teleop and turn it, then run the
  auton again. Does it still go "2 m ahead" of where it's facing?
* Add `waitSeconds(1)` between the two steps.
* Change the out-trip to go 2 m to the robot's left instead.

## Part 6: Run it on a real robot

Only with a mentor, the first few times.

1. Find **at least 3 meters of clear floor** in front of the robot, and
   a little behind it.
1. Deploy (WPILib → **Deploy Robot Code**).
1. **Pick the auton in Glass.** The Driver Station can't choose which
   auton runs. If nobody picks one, the robot runs `NONE` and just sits
   there when you enable.
   1. Open Glass (`Ctrl+Shift+P` → **WPILib: Start Tool** → **Glass**).
   1. Connect: **NetworkTables Settings** → **Client**, **Team** →
      **100**, **Apply**. (More in
      [Glass](../part1/09_GLASS_POWER.md#open-glass-and-connect).)
   1. Choose: **SmartDashboard** → **Auton Command** → **There And
      Back**.

   > 📷 **TODO screenshot:** `docs/img/glass_auton_chooser.png`: the
   > Auton Command chooser in Glass with an auton selected.

1. In the Driver Station, choose **Autonomous**.
1. Everyone stands clear. Say **"Enabling!"** loudly. One person keeps a
   finger on **Enter** (disable) the whole time.
1. Enable.

Measure where the robot ends up compared to a tape mark at the start.
How far off is it? That number is a great conversation starter about
odometry and the controller tolerances in `Autons.java`.

## Part 7: Send it in

Commit, push to your fork, and open a PR against `Team100/all26`,
branch `2027`. Say in the description what you tested (sim? which robot?)
and how far from the start mark it stopped.

## Going further

* **Turn around on the way out** so the robot faces home, and drive back
  forward. (Compare `AheadTwoMetersWithRotation`.)
* **A square:** four trajectories, ending at the start.
* **Absolute poses:** start from one of the
  [`StartingPositions`](https://github.com/Team100/all26/blob/2027/lib/src/main/java/org/team100/frc2026/auton/StartingPositions.java)
  and drive to a fixed spot on the field, like `LeftSequenceExample` does.
  When is absolute better than relative?

## Hints

<details>
<summary>Hint 1: remembering the start</summary>

Add a field, `private Pose2d m_start;`. The first line of `out(Pose2d p)`
can save `p` into it. `back` can read it.
</details>

<details>
<summary>Hint 2: two steps</summary>

Make two `DriveWithTrajectoryFunction`s, one with `this::out` and one
with `this::back`, and then:

```java
command = sequence(
        outStep.until(outStep::isDone),
        backStep.until(backStep::isDone));
```

You'll need `import static org.wpilib.command2.Commands.sequence;`
(copy the import from `LeftSequenceExample`).
</details>

<details>
<summary>Hint 3: the reversed course</summary>

The robot faces `p.getRotation()` but travels the other way, which is
`p.getRotation().plus(Rotation2d.kPi)`. Wrap that in
`DirectionSE2.irrotational(...)`.
</details>

<details>
<summary>Hint 4: the back trajectory (try everything else first!)</summary>

```java
TrajectorySE2 back(Pose2d p) {
    DirectionSE2 reverse = DirectionSE2.irrotational(
            p.getRotation().plus(Rotation2d.kPi));
    WaypointSE2 w1 = new WaypointSE2(p, reverse, 1);
    WaypointSE2 w2 = new WaypointSE2(m_start, reverse, 1);
    return planner.restToRest(List.of(w1, w2));
}
```

Before you use this, explain to someone what every line does.
</details>

Next: back to the [guide](../../README.md).
