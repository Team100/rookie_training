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
* **SystemCore, not RoboRIO.** The 2027 robot controller is the
  [SystemCore](https://docs.wpilib.org/en/2027/). You can `ssh` into it
  just like a Raspberry Pi.
* **Raspberry Pi 5 cameras** run our own Python vision code, and you can
  change and deploy it yourself.

The SystemCore code lives on the **`2027` branch** of `Team100/all26`.
Everything in this guide uses that branch.

## What you'll learn

* **Hardware:** connecting motors, controllers, and sensors
* **Programming:** Java, WPILib, and the Team 100 library
* **Software engineering:** Git, testing, debugging
* **Teamwork:** pull requests, code review, documentation

## Learning path

### Getting started

1. **[Set up your laptop and your fork](SETUP.md)**: GitHub, Git, WPILib, first build
1. **[Day-to-day workflow with your fork](FORK_WORKFLOW.md)**: sync, branch, commit, push, pull request
1. **[Deploy to the SystemCore](DEPLOY.md)**: put code on a real controller

### Motors

4. **[Your first motor](FIRST_MOTOR.md)**: wiring, CAN IDs, spinning a motor with no code
1. **[Your first study](STUDY.md)**: your own small project that uses `lib`
1. **[Control a motor from code](MOTOR_CODE.md)**: duty cycle, reading speed, Xbox controller
1. **[Use the simulator](SIMULATOR.md)**: test without a robot

### The whole robot

8. **[Connect to the robot and test your code](ROBOT_NETWORK.md)**: radios, laptop network, competition-robot etiquette
1. **[SSH into the SystemCore and the Pis](SSH_AND_CHECKS.md)**: log in and check that things are healthy
1. **[Change the Raspberry Pi code](PI_CHANGES.md)**: edit, test, deploy, PR
1. **[Work on auton](AUTON.md)**: drive out 2 meters and back; pose, waypoints, trajectories

### Whenever you need them

* [Robot identity](IDENTITY.md): how the code knows which robot it's on
* [First motor on a RoboRIO (2025)](ROBORIO_NEO_FIRST_MOTOR.md): for older test boards
* [Debugging](DEBUGGING.md): errors, prints, logs, the debugger
* [Troubleshooting](TROUBLESHOOTING.md): Git, build, and deploy problems
* [Coding conventions](https://github.com/Team100/all26/blob/2027/lib/doc/getting_started/Conventions.md)

## RoboRIO → SystemCore cheat sheet

Older docs, forum posts, and videos talk about the RoboRIO. Translate like
this:

| RoboRIO (old) | SystemCore (new) |
| --- | --- |
| USB-B cable to the RoboRIO | USB-C cable to the SystemCore |
| USB address `172.22.11.2` | `172.26.0.1` (Windows), `172.27.0.1` (Mac/Linux) |
| `roboRIO-100-FRC.local` | `robot.local` |
| Robot address `10.1.0.2` | Still `10.1.0.2` on the robot network |
| ssh user `lvuser` / `admin` | ssh user `systemcore`, password `systemcore` |
| Deploy folder `/home/lvuser/deploy` | `/home/systemcore/deploy` |
| Serial number in the console | See [Robot identity](IDENTITY.md) |
| CAN bus | Five CAN ports, `can_s0` … `can_s4`; in code, `new CanBusId(0)` … |
| `edu.wpi.first.…` packages | `org.wpilib.…` packages |
| "2026 WPILib VS Code" | "2027 WPILib VS Code" |

The SystemCore also has a web dashboard at http://robot.local, and its
screen shows its current IP address.

## Reference

* [Network addresses](https://github.com/Team100/all26/blob/2027/lib/doc/network.md)
* [Test board setup (Kraken, Vortex, gyro)](https://github.com/Team100/all26/blob/2027/lib/doc/setup.md)
* [Library tutorial](https://github.com/Team100/all26/blob/2027/lib/doc/README.md)
* [Raspberry Pi docs](https://github.com/Team100/all26/blob/2027/raspberry_pi/doc/README.md)
* [WPILib 2027 docs](https://docs.wpilib.org/en/2027/)

## Help improve this guide

This guide is never finished. If something is wrong, unclear, or
missing, or if a screenshot still shows a RoboRIO, fix it! Fork this
repo, edit the page, and send a pull request, the same
[fork workflow](FORK_WORKFLOW.md) you use for robot code. That's good
practice too.

**Ready?** Start with [Set up your laptop and your fork](SETUP.md).
