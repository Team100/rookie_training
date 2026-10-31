# Team 100 Rookie Guide (2026–27)

Welcome to [Team 100](https://www.team100.org/)! This is the starting point
for new programmers, from "never programmed" to contributing team member.
Follow the chapters in order. Each one ends with a **checkpoint** so you
know it worked before you move on.

This repository is **just the training material**. The robot code itself
lives in [`Team100/all26`](https://github.com/Team100/all26), and the
chapters link to it.

## Using AI here

**AI assistants are welcome in this repo.** Use Claude, Copilot, ChatGPT,
or whatever you like to explain a chapter, quiz you, translate jargon,
or help you improve these pages.

The robot code repo, `all26`, is different. AI tools there act as
**mentors only**: they ask questions and explain, but you type the
code (see its `AGENTS.md`). So when a chapter has you writing code in
`all26`, expect the AI to coach you, not do it for you. That's on
purpose: the point is that *you* can do it.

What's new compared to the 2025 guide:

* **Forks, not branches on the team repo.** You never push to
  `Team100/all26` directly. You push to your own copy (your *fork*) and
  open a pull request.
* **Five parts.** Basics, real robots, vision, and auton on a
  **RoboRIO**; then the new **SystemCore** controller in Part 5.
* **Krakens only.** The competition robot uses Kraken X60 and X44
  motors, so that's what this guide uses.
* **Raspberry Pi 5 cameras** run our own Python vision code, and you can
  change and deploy it yourself.

## What you'll learn

* **Hardware:** connecting motors, controllers, and sensors
* **Programming:** Java, WPILib, and the Team 100 library
* **Software engineering:** Git, testing, debugging
* **Teamwork:** pull requests, code review, documentation

## Part 1: The basics

Learn the basics on a RoboRIO test board. Parts 1 to 4 use the
**`main`** branch of `Team100/all26` and **2026 WPILib**.

### Getting started

1. **[Set up your laptop and your fork](docs/part1/01_SETUP.md)**: GitHub, Git, WPILib, first build
1. **[Day-to-day workflow with your fork](docs/part1/02_FORK_WORKFLOW.md)**: sync, branch, commit, push, pull request
1. **[Work from home on your own laptop](docs/part1/03_HOME_LAPTOP.md)**: Slack, GitHub, VS Code, Claude
1. **[Deploy to the RoboRIO](docs/part1/04_DEPLOY.md)**: put code on a real controller

### Motors

5. **[Your first motor](docs/part1/05_FIRST_MOTOR.md)**: wiring a Kraken, CAN IDs, spinning it with no code
   * then **[How motors work](docs/part1/05a_HOW_MOTORS_WORK.md)**: electricity, magnets, stator vs. supply current; take apart a Kraken
1. **[Your first study](docs/part1/06_STUDY.md)**: your own small project that uses `lib`
1. **[Control a motor from code](docs/part1/07_MOTOR_CODE.md)**: duty cycle, reading speed, Xbox controller
1. **[Use the simulator](docs/part1/08_SIMULATOR.md)**: test without a robot

### 🎉 Finished Part 1?

**Post a photo of your group succeeding to `#rookie_training` in
Slack!** Here's the
[latest group to finish](https://team100robotics.slack.com/archives/C07JCSXEMU3/p1793483605772369)
(team Slack only).

When your group finishes, post your photo, then **update this link** to
point at it: copy the link to your Slack message (**⋮ → Copy link**),
edit this README, and send a pull request. It's a great first PR to this
guide.

## Part 2: Real robot work

Real mechanisms, on real robots. Start with **easy mode**, then try
**hard mode**.

9. **[Glass and power-draw tests](docs/part2/09_GLASS_POWER.md)**: graph live values; measure how much current a mechanism draws
1. **[Connect to the robot and test your code](docs/part2/10_ROBOT_NETWORK.md)**: radios, laptop network, robot etiquette
1. **Easy mode: [the Discus](docs/part2/11_DISCUS.md)**: write a study that spins a heavy disc, measure it, and make it hold a speed
1. **Hard mode: [the shooter bot](docs/part2/12_DEMO_BOT.md)**: write a study that spins up the shooter and feeds balls, safely

## Part 3: Raspberry Pis and vision

13. **[SSH into the RoboRIO and the Pis](docs/part3/13_SSH_AND_CHECKS.md)**: log in and check that things are healthy
1. **[Change the Raspberry Pi code](docs/part3/14_PI_CHANGES.md)**: edit, test, deploy, PR
1. **[How our vision works](docs/part3/15_VISION.md)**: cameras, AprilTags, and why vision answers are always a little late

## Part 4: Localization and auton

16. **[Where am I?](docs/part4/16_LOCALIZATION.md)**: odometry, vision, and how the robot combines them
1. **[Work on auton](docs/part4/17_AUTON.md)**: drive out 2 meters and back; pose, waypoints, trajectories

## Part 5: SystemCore, all together

The competition robot runs on the new
[SystemCore](https://docs.wpilib.org/en/2027/). Part 5 uses the
**`2027`** branch of `Team100/all26` and **2027 WPILib**.

18. **[Switch to the SystemCore](docs/part5/18_SYSTEMCORE.md)**: what's different, the `2027` branch, deploy, CAN ports, SSH, network
1. **[Robot identity](docs/part5/IDENTITY.md)**: how the code knows which robot it's on
1. **[Put it together on the demo bot](docs/part5/19_DEMO_BOT_SYSTEMCORE.md)**: move your shooter study to the SystemCore and bring up the whole robot

## Advanced (mentor approval required)

* [Smoke a motor](docs/advanced/SMOKE_A_MOTOR.md): stall a scrap motor
  until it overheats, and see what the stator limit protects you from

## Whenever you need them

* [RoboRIO → SystemCore cheat sheet](docs/part5/18_SYSTEMCORE.md#roborio--systemcore-cheat-sheet)
* [Debugging](docs/reference/DEBUGGING.md): errors, prints, logs, the debugger
* [Troubleshooting](docs/reference/TROUBLESHOOTING.md): Git, build, deploy, and robot problems
* [Coding conventions](https://github.com/Team100/all26/blob/2027/lib/doc/getting_started/Conventions.md)

## Reference

* [Network addresses](https://github.com/Team100/all26/blob/2027/lib/doc/network.md)
* [Test board setup (Kraken, Vortex, gyro)](https://github.com/Team100/all26/blob/main/lib/doc/setup.md)
* [Library tutorial](https://github.com/Team100/all26/blob/2027/lib/doc/README.md)
* [Raspberry Pi docs](https://github.com/Team100/all26/blob/2027/raspberry_pi/doc/README.md)
* WPILib docs: [2026, for the RoboRIO](https://docs.wpilib.org/en/stable/) and
  [2027, for the SystemCore](https://docs.wpilib.org/en/2027/)

## Help improve this guide

This guide is never finished. If something is wrong, unclear, or
missing, or a screenshot is out of date, fix it! Fork this
repo, edit the page, and send a pull request, the same
[fork workflow](docs/part1/02_FORK_WORKFLOW.md) you use for robot code. That's good
practice too.

**Ready?** Start with [Set up your laptop and your fork](docs/part1/01_SETUP.md).
