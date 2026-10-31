# Glass and power-draw tests

**Glass** is a WPILib tool that shows the values your robot code
publishes, live, and graphs them. In this chapter you'll learn to use it,
then use it to run a **power-draw test** on a real mechanism, together
with someone from the mechanical team.

You need:

* the Kraken code from [Control a motor from code](../part1/07_MOTOR_CODE.md),
  deployed and working
* a laptop connected to the RoboRIO (USB-B is fine)

<!--
PHOTO CHECKLIST: save these in docs/img/ and replace each "TODO" note below.
  [ ] glass_start_tool.png        VS Code "WPILib: Start Tool" list with Glass
  [ ] glass_nt_settings.png       Glass NetworkTables Settings, client mode
  [ ] glass_log_tree.png          NetworkTables tree open at log/motor/KrakenX60Motor
  [ ] glass_plot_current.png      plot of supply and stator current while the motor runs
  [ ] power_test_setup.jpg        a mechanism on the bench, ready for a power test
-->

## Where the numbers come from

Our loggers (`LoggerFactory`) publish values to **NetworkTables**, a
shared list of named values that the robot and your laptop both see.
Glass is a window onto NetworkTables.

Every Kraken already logs these, among others:

| Name | Meaning |
| --- | --- |
| `velocity (rad_s)` | How fast it's turning |
| `output [-1,1]` | Duty cycle it's actually applying |
| `supply current (A)` | Current pulled **from the battery** |
| `stator current (A)` | Current going **through the motor windings** |
| `supply voltage (V)` | Battery voltage, as the motor sees it |
| `temperature (C)` | How hot the motor is |

And the robot as a whole logs:

| Name | Meaning |
| --- | --- |
| `log/Robot/voltage` | Battery voltage |
| `log/Robot/TotalCurrentLog/total supply current (A)` | Supply current of every motor, added up |

> **Supply vs. stator current.** When a motor is spinning slowly but
> pushing hard (like a stalled arm), the motor controller turns 12 V
> from the battery into a lower voltage at a higher current. So stator
> current can be **much bigger** than supply current. Stator current
> makes heat and torque. Supply current drains the battery and trips
> breakers. That's why
> [`CurrentLimit`](https://github.com/Team100/all26/blob/main/lib/src/main/java/org/team100/lib/config/CurrentLimit.java)
> has a limit for each. The full story is in
> [How motors work](../part1/05a_HOW_MOTORS_WORK.md#stator-current-vs-supply-current).

## Open Glass and connect

1. In VS Code, `Ctrl+Shift+P` → **WPILib: Start Tool** → **Glass**.

   > 📷 **TODO screenshot:** `docs/img/glass_start_tool.png`.

1. Connect Glass to the robot:
   1. **NetworkTables Settings** → **Client**
   1. **Team** → **100** (our team number)
   1. **Apply**

   Glass works out the robot's address for you, whether you're on USB or
   the robot network.
   * For the [simulator](../part1/08_SIMULATOR.md), type `localhost` instead of 100.
   * Still not connecting? Type the robot's address instead: `10.1.0.2`
     on the robot network, or `172.22.11.2` over USB.

   > 📷 **TODO screenshot:** `docs/img/glass_nt_settings.png`: Client
   > mode with 100 in Team.

1. The title bar should say it's connected.
   > **TODO:** check the exact wording Glass shows when it's connected.

## Find your motor's values

1. Open **NetworkTables → NetworkTables View**.
1. Open `log`, then `motor` (the name you gave in `log.name("motor")`),
   then `KrakenX60Motor`.

   > 📷 **TODO screenshot:** `docs/img/glass_log_tree.png`.

Don't see `supply current (A)`? The robot only publishes values at or
above the **log level**. Open `SmartDashboard` → `Log Level` and make
sure it's `DEBUG` (the default) or `TRACE`, not `COMP`.

![](../img/log_level.png)

> 📷 **TODO screenshot:** retake the log level picture above in Glass on
> on the robot, with `DEBUG` selected.

**Checkpoint:** enable teleop, push the stick, and watch `velocity
(rad_s)` change.

## Make a graph

1. **Plot → New Plot Window**.
1. Drag `supply current (A)` and `stator current (A)` from the
   NetworkTables view onto the plot. Add `log/Robot/voltage` too.
1. Run the motor at a few speeds with the stick.

> 📷 **TODO screenshot:** `docs/img/glass_plot_current.png`: supply
> and stator current while the motor runs.

> **Check yourself:** at a steady speed with nothing touching the motor,
> which current is bigger? Now grab the wheel with a rag (gently, at low
> power, with a mentor) so it slows down. What happens to each current,
> and to the battery voltage? Why?

Glass is also where you **choose which auton runs** on a real robot
(**SmartDashboard** → **Auton Command**), because the Driver Station
can't.
See [Work on auton](../part4/17_AUTON.md#part-6-run-it-on-a-real-robot).

The WPILib [Glass docs](https://docs.wpilib.org/en/stable/docs/software/dashboards/glass/index.html)
cover more widgets, like the field view you used in the simulator.

## Run a power-draw test

A robot has one battery and a main breaker, and every motor shares
them. The mechanical team needs to know how much current each mechanism
really takes, so they can pick gear ratios and the electrical team can
budget the battery and breakers. You measure it.

> **TODO (mentors):** confirm what mechanical wants recorded, and link
> the sheet where results go.

**Work in pairs:** one programmer runs the code and Glass, and one
person from the **mechanical team** runs the mechanism and watches it.

> 📷 **TODO photo:** `docs/img/power_test_setup.jpg`: a mechanism on
> the bench, ready for a power test.

### Before you start

* Agree on what you're testing and its **normal job** (for example, "the
  intake picking up a game piece").
* Use a **freshly charged battery**, and write down its starting
  voltage.
* Start with a **low current limit** in your `CurrentLimit`, and raise
  it only with a mentor's OK. The mechanism must be able to survive
  every test you plan.
* Clear hands, say "Enabling!", and keep a finger on **Enter**.

### Measure

Graph `supply current (A)`, `stator current (A)`, and
`log/Robot/voltage` (for several motors, add `total supply current
(A)`). For each step, hold it steady for a few seconds and read the
values off the graph:

1. **Free spin:** the mechanism moving with no load, at the speed it
   uses in a match.
1. **Normal load:** doing its real job (lifting, shooting, intaking).
1. **Worst case:** the hardest thing it does, like the start of a lift
   or a jam. Only with a mentor, only briefly, and only within the
   current limit.

### Record

| Mechanism | Step | Speed/output | Supply A | Stator A | Battery V | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| Intake | Free spin | 0.5 | | | | |
| Intake | Normal load | 0.5 | | | | |
| Intake | Worst case | 0.5 | | | | hit the current limit? |

Then, with your mechanical partner:

* Did the current hit the **limit** (it stops rising at the limit
  value)? Then the mechanism may need more current, or a different gear
  ratio.
* Did the battery voltage **sag** a lot? Below about 6.8 V the RoboRIO
  **browns out**: it turns the motors off to save itself.
* Is the motor getting hot (`temperature (C)`)?

**Checkpoint:** a filled-in table for one mechanism, which your
mechanical partner agrees matches what they saw.

Next: [Connect to the robot and test your code](10_ROBOT_NETWORK.md)
