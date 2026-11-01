# 🏆 Park on the X, again (SystemCore)

Same challenge as [Part 4](../part4/21_PARK_ON_THE_X.md), new stack: the
demo bot now runs on the **SystemCore**, with the `2027` branch and
2027 WPILib.

**The rules don't change.** Same course, same card, same random starts,
same scoring (total cm error over 3 runs, lower is better). Only the
robot's insides are different.

> **TODO (mentors):** which demo bot is converted to the SystemCore, and
> whether its gyro and camera are set up the same way as in Part 4.

## Step 1: Move your auton over

Bring your Park on the X code from `main` to the `2027` branch, the same
way you moved your [shooter study](25_DEMO_BOT_SYSTEMCORE.md#step-1-move-your-shooter-study-to-2027).

**Checkpoint:** it builds, and it runs in the **simulator** on `2027`
before it touches the robot.

> **Check yourself:** what did you have to change? Make a list. Was any
> of it about the auton itself, or was it all about the new stack?

## Step 2: Check the pieces

Before you run the challenge, check each piece on its own. A problem is
much easier to find one piece at a time.

| Check | How | Good answer |
| --- | --- | --- |
| Pi cameras reach the SystemCore | `ping -c 3 10.1.0.2` from each Pi | 3 replies |
| The robot sees the tag | the tag's values change in Glass when you cover the camera | yes |
| Pose looks right | put the robot on the X by hand; read its pose in Glass | within 10 cm of where the card says |
| Odometry works | push the robot 1 m by hand | pose moves about 1 m |

## Step 3: Run the challenge

Exactly the [Part 4 rules](../part4/21_PARK_ON_THE_X.md#scoring). Fill in
the "two kinds of miss" table for every run and post it, with your
score and a video, to **`#rookie_training`**.

### Records (SystemCore)

| Mode | Total error (3 runs) | Robot | Who | Date | Slack proof |
| --- | --- | --- | --- | --- | --- |
| Easy | — | | | | |
| Hard | — | | | | |

> **Check yourself:** compare with your Part 4 score.
> 1. Better, worse, or the same?
> 2. Look at your "two kinds of miss" tables. Did the **localization**
>    misses change, or the **tracking** misses?
> 3. If something got worse, was it the new controller, the new code, or
>    something else (a different robot, a different floor, a battery)?
>    How could you tell?

Next: [Part 6: Spot the balls](../part6/27_OBJECT_DETECTION.md)
