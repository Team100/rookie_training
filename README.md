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
* **Two parts.** You learn the basics on a **RoboRIO**, then switch to
  the new **SystemCore** controller for the real robot.
* **Krakens only.** The competition robot uses Kraken X60 and X44
  motors, so that's what this guide uses.
* **Raspberry Pi 5 cameras** run our own Python vision code, and you can
  change and deploy it yourself.

## What you'll learn

* **Hardware:** connecting motors, controllers, and sensors
* **Programming:** Java, WPILib, and the Team 100 library
* **Software engineering:** Git, testing, debugging
* **Teamwork:** pull requests, code review, documentation

## Part 1: RoboRIO and motors

Learn the basics on a RoboRIO test board. Use the **`main`** branch of
`Team100/all26` and **2026 WPILib**.

### Getting started

1. **[Set up your laptop and your fork](SETUP.md)**: GitHub, Git, WPILib, first build
1. **[Day-to-day workflow with your fork](FORK_WORKFLOW.md)**: sync, branch, commit, push, pull request
1. **[Work from home on your own laptop](HOME_LAPTOP.md)**: Slack, GitHub, VS Code, Claude
1. **[Deploy to the RoboRIO](DEPLOY.md)**: put code on a real controller

### Motors

5. **[Your first motor](FIRST_MOTOR.md)**: wiring a Kraken, CAN IDs, spinning it with no code
1. **[Your first study](STUDY.md)**: your own small project that uses `lib`
1. **[Control a motor from code](MOTOR_CODE.md)**: duty cycle, reading speed, Xbox controller
1. **[Use the simulator](SIMULATOR.md)**: test without a robot
1. **[Glass and power-draw tests](GLASS_POWER.md)**: graph live values; measure how much current a mechanism draws

## Part 2: SystemCore and the real robot

The competition robot runs on the
[SystemCore](https://docs.wpilib.org/en/2027/). Start here once you've
finished Part 1. Use the **`2027`** branch of `Team100/all26` and
**2027 WPILib**.

10. **[Switch to the SystemCore](SYSTEMCORE.md)**: what's different, the `2027` branch, deploy, CAN ports, move your study over
1. **[Connect to the robot and test your code](ROBOT_NETWORK.md)**: radios, laptop network, competition-robot etiquette
1. **[SSH into the SystemCore and the Pis](SSH_AND_CHECKS.md)**: log in and check that things are healthy
1. **[Change the Raspberry Pi code](PI_CHANGES.md)**: edit, test, deploy, PR
1. **[Work on auton](AUTON.md)**: drive out 2 meters and back; pose, waypoints, trajectories

## Whenever you need them

* [Robot identity](IDENTITY.md): how the code knows which robot it's on (SystemCore)
* [RoboRIO → SystemCore cheat sheet](SYSTEMCORE.md#roborio--systemcore-cheat-sheet)
* [Debugging](DEBUGGING.md): errors, prints, logs, the debugger
* [Troubleshooting](TROUBLESHOOTING.md): Git, build, deploy, and robot problems
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
[fork workflow](FORK_WORKFLOW.md) you use for robot code. That's good
practice too.

**Ready?** Start with [Set up your laptop and your fork](SETUP.md).
