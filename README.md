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
* **Seven parts.** Basics, real robots, localization, and auton on a
  **RoboRIO**; then the new **SystemCore** controller, cameras and
  vision, finding balls with a neural network, and a final challenge
  that uses everything.
* **Krakens first.** The competition robot uses Kraken X60 and X44
  motors, so the wiring chapters use them. The motor-code chapter also
  covers REV motors on SPARK controllers, like the demo bot's.
* **Raspberry Pi 5 cameras** run our own Python vision code, and you can
  change and deploy it yourself.

## What you'll learn

* **Hardware:** connecting motors, controllers, and sensors
* **Programming:** Java, WPILib, and the Team 100 library
* **Software engineering:** Git, testing, debugging
* **Teamwork:** pull requests, code review, documentation

## ⚠️ Use the right VS Code

Your laptop may have **several** VS Codes: a plain one, and one for each
WPILib year. They look the same, and Windows search, Spotlight, and the
Dock often open the wrong one. Using the wrong one gives you **hundreds
of build errors** in code that's fine.

| Parts | Branch | Use | Windows: in File Explorer | Mac: in Finder (**Go → Go to Folder…**) |
| --- | --- | --- | --- | --- |
| 1 to 3 | `main` | **2026 WPILib VS Code** | `C:\Users\Public\wpilib\2026\vscode\Code.exe` | `~/wpilib/2026/vscode` → **VSCode FRC 2026** |
| 4 to 7 | `2027` | **2027 WPILib VS Code** | `C:\Users\Public\wpilib\2027…\vscode\Code.exe` | `~/wpilib/2027/vscode` |

Then **pin it** (Windows: right-click → **Pin to taskbar**; Mac:
right-click the Dock icon → **Options** → **Keep in Dock**), and always
start it from there. If VS Code ever offers to **import** or **update** a
project, say **no**: you're in the wrong one. More in the
[Build errors FAQ](docs/reference/BUILD_ERRORS.md#wrong-vs-code-or-wrong-year).

## Part 1: The basics

Learn the basics on a RoboRIO test board. Parts 1 to 3 use the
**`main`** branch of `Team100/all26` and **2026 WPILib**.

### Getting started

* **1.1** **[Set up your laptop and your fork](docs/part1/01_SETUP.md)**: GitHub, Git, WPILib, first build
* **1.2** **[Day-to-day workflow with your fork](docs/part1/02_FORK_WORKFLOW.md)**: sync, branch, commit, push, pull request
* **1.3** **[Work from home on your own laptop](docs/part1/03_HOME_LAPTOP.md)**: Slack, GitHub, VS Code, Claude
* **1.4** **[Deploy to the RoboRIO](docs/part1/04_DEPLOY.md)**: put code on a real controller

### Motors

* **1.5** **[Your first motor](docs/part1/05_FIRST_MOTOR.md)**: wiring a Kraken, CAN IDs, spinning it with no code
* **1.6** **[How motors work](docs/part1/06_HOW_MOTORS_WORK.md)**: electricity, magnets, stator vs. supply current; take apart a Kraken
* **1.7** **[Your first study](docs/part1/07_STUDY.md)**: your own small project that uses `lib`
* **1.8** **[Control a motor from code](docs/part1/08_MOTOR_CODE.md)**: duty cycle, reading speed, Xbox controller, velocity control (direct vs. profiled), turning to an angle; build a two-motor grabber
* **1.9** **[Use the simulator](docs/part1/09_SIMULATOR.md)**: test without a robot

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

Real mechanisms, tethered to your laptop with a cable. Start with
**easy mode**, then try **hard mode**.

* **2.1** **[Glass and power-draw tests](docs/part2/01_GLASS_POWER.md)**: graph live values; the rag test
* **2.2** **Easy mode: [the Discus](docs/part2/02_DISCUS.md)**: spin a heavy disc, measure it, make it hold a speed; 🏆 Discus Challenge
* **2.3** **Hard mode: [the shooter bot](docs/part2/03_DEMO_BOT.md)**: spin up the shooter and feed balls, safely; 🏆 Shooter Challenge
* **2.4** **[Optional] [Commands: build a gripper the Team 100 way](docs/part2/04_COMMANDS.md)**: a tour of `lib`'s building blocks, then your own two-motor gripper subsystem and commands

## Part 3: Localization and auton

* **3.1** **[Where am I?](docs/part3/01_LOCALIZATION.md)**: how the wheels tell the robot where it is, and how wrong they get; hands-on experiments with graphs
* **3.2** **[Work on auton](docs/part3/02_AUTON.md)**: simulate a circle, change the speed, run mechanisms during auton, then measure real error
* **3.3** **🏆 [Park on the X](docs/part3/03_PARK_ON_THE_X.md)**: from a taped start, the demo bot drives itself to a spot, using only its wheels

## Part 4: SystemCore, all together, on the real robot

The competition robot runs on the new
[SystemCore](https://docs.wpilib.org/en/2027/). Parts 4 to 7 use the
**`2027`** branch of `Team100/all26` and **2027 WPILib**.

* **4.1** **[Switch to the SystemCore](docs/part4/01_SYSTEMCORE.md)**: what's different, the `2027` branch, deploy, CAN ports, SSH
* **4.2** **[Connect to the robot (radios)](docs/part4/02_ROBOT_NETWORK.md)**: radios, laptop network, robot etiquette
* **4.3** **[Robot identity](docs/part4/03_IDENTITY.md)**: how the code knows which robot it's on
* **4.4** **[Put it together on the demo bot](docs/part4/04_DEMO_BOT_SYSTEMCORE.md)**: move your shooter study to the SystemCore and bring up the whole robot
* **4.5** **🏆 [Park on the X, again](docs/part4/05_PARK_ON_THE_X_SYSTEMCORE.md)**: the Part 3 challenge on the new stack; did it get better?

## Part 5: Raspberry Pis and vision

* **5.1** **[SSH into the Pis](docs/part5/01_SSH_AND_CHECKS.md)**: log in and check that things are healthy
* **5.2** **[Change the Raspberry Pi code](docs/part5/02_PI_CHANGES.md)**: edit, test, deploy, PR
* **5.3** **[How our vision works](docs/part5/03_VISION.md)**: cameras, AprilTags, and why vision answers are always a little late
* **5.4** **[Set up a new camera](docs/part5/04_NEW_CAMERA.md)**: measure it, calibrate it, and measure again: before vs. after
* **5.5** **[Break the cameras (on purpose)](docs/part5/05_BREAK_THE_CAMERAS.md)**: the breaker vs. finder game, and measuring detection limits
   * [Breaker cards](docs/part5/BREAKER_CARDS.md) (finders: don't peek!) · [Finder's guide](docs/part5/FINDER_GUIDE.md)
* **5.6** **🏆 [The Vision Shooter Challenge](docs/part5/06_VISION_SHOOTER_CHALLENGE.md)**: the camera finds the bucket (8–12 m), and the shooter adjusts by itself
* **5.7** **🏆 [Park on the X, from anywhere](docs/part5/07_PARK_FROM_ANYWHERE.md)**: from a random start, the demo bot looks at an AprilTag, finds itself, and parks

## Part 6: Spot the balls 🚧

* **6.1** **[Object detection](docs/part6/01_OBJECT_DETECTION.md)**: see what our YOLO model finds, break it, retrain it; 🏆 Ball Spotter Challenge

## Part 7: Put it all together 🚧

* **7.1** **🏆 [Fetch and shoot](docs/part7/01_FETCH_AND_SHOOT.md)**: the demo bot finds the yellow balls, picks them up, drives to the X, and shoots them into the hopper, all by itself

## Advanced (mentor approval required)

* [Smoke a motor](docs/advanced/SMOKE_A_MOTOR.md): stall a scrap motor
  until it overheats, and see what the stator limit protects you from

## Whenever you need them

* [RoboRIO → SystemCore cheat sheet](docs/part4/01_SYSTEMCORE.md#roborio--systemcore-cheat-sheet)
* **[Build errors FAQ](docs/reference/BUILD_ERRORS.md)**: not compiling? Seeing errors? Start here
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
