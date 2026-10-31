# Break the cameras (on purpose)

At a competition, vision **will** break: a cable gets knocked loose, a
Pi browns out, a camera points at the stadium lights. The team that
finds the cause in two minutes keeps playing. The way to get that fast
is to **break things on purpose now**, on the bench, and learn what
each problem looks like.

This chapter has two labs:

1. **Break it:** cause one failure at a time, and record exactly what it
   looks like.
2. **Measure it:** find the limits of AprilTag detection: distance,
   angle, speed, and light.

You'll finish with a **symptom → cause table** that the whole team can
use.

> [!WARNING]
> **Do this on a bench Pi or a test robot, with a mentor. Never on the
> competition robot at an event.**
>
> * **Power the Pi off before touching the camera ribbon cable.** Every
>   time. Plugging or unplugging it while powered can kill the camera
>   or the Pi.
> * Shut the Pi down properly (`sudo shutdown -h now`, wait for the
>   green LED to stop) before pulling power. Pulling power while it's
>   running can corrupt the SD card.
> * Put everything back the way you found it, and check it works, before
>   you leave.

## Before you start: know what "healthy" looks like

You can't spot "broken" until you know "working". With a Pi connected to
your laptop or the robot ([how](13_SSH_AND_CHECKS.md#connect-your-laptop)),
write down what each of these shows when everything is fine:

| Where to look | What you see when healthy |
| --- | --- |
| `http://<pi-ip>:9001` (supervisor page) | `app` **RUNNING** |
| `http://<pi-ip>:1181` (camera view) | Live picture; a tag held up gets a box drawn on it |
| `app.stderr` (via `:9001` or `ssh`) | No repeating errors |
| `vcgencmd get_throttled` (via `ssh`) | `throttled=0x0` |
| `vcgencmd measure_temp` (via `ssh`) | The temperature, under ~80 °C |
| Glass, on the robot side | The vision values updating when a tag is in view |

> 📷 **TODO screenshot:** `docs/img/pi_healthy_1181.png`: the `:1181`
> camera view with a tag detected.

> **TODO (mentors):** add which NetworkTables values in Glass show
> vision data arriving at the robot.

## Lab 1: Break it

Do these one at a time. For **each** one:

1. **Predict** what you'll see in each place in the healthy table.
1. **Break** it.
1. **Look** at every place in the table, and write down what changed.
   Include exact error messages from `app.stderr`.
1. **Fix** it, and time how long until everything is healthy again. Did
   it come back **by itself**, or did you have to do something?

| # | Break it | Where it happens on a real robot |
| --- | --- | --- |
| 1 | **Unplug the Pi's Ethernet cable** while it's running. Then plug it back in. | Cable knocked loose during a match |
| 2 | **Stop the app:** `supervisorctl stop app`. Then `supervisorctl start app`. | The vision program crashed |
| 3 | **No camera:** shut down, **power off**, unplug the camera ribbon, then power on. (Then shut down, power off, plug it back, power on.) | Ribbon cable came loose |
| 4 | **Cover the lens** with your hand, then shine a flashlight straight into it. | A robot in front of the camera; stadium lights |
| 5 | **Weak power:** with a mentor, power the Pi from a weak phone charger instead of its proper supply. Check `vcgencmd get_throttled`. | Bad power wiring on the robot |
| 6 | **Robot side gone:** leave the Pi running, and turn off (or unplug) the RoboRIO or the radio. Then bring it back. | Robot brownout or radio reboot |

> **Check yourself:** in breaks 1, 2, and 6, does the robot **know**
> vision stopped, or does it keep using the last answer it got? Why does
> that matter for a robot that's driving?

**Checkpoint:** a table like this, filled in for all six:

| # | What broke | `:9001` | `:1181` | `app.stderr` | Glass | Came back by itself? How long? |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Ethernet unplugged | | | | | |
| … | | | | | | |

## Lab 2: Measure the limits

Now find out how **good** detection really is. You'll need an AprilTag
printed at the right size, a tape measure, and a protractor (or your
phone's level app).

> **TODO (mentors):** add the tag family and print size we use.

For each test, use the `:1181` view to see whether the tag is detected.
Do each one **three times** and write down the results.

| Test | How | Measure |
| --- | --- | --- |
| **Distance** | Hold the tag facing the camera, start close, and back away slowly | The farthest distance where it's still detected **every** time |
| **Angle** | At 2 m, turn the tag away from facing the camera | The biggest angle that's still detected |
| **Speed** | At 2 m, move the tag sideways, faster each time | The speed where it stops being detected (film it, or count seconds over a measured distance) |
| **Light** | Turn off the room lights, then shine a flashlight from behind the tag, then from the side | Which conditions work, and which don't |
| **Partly hidden** | Cover a corner of the tag with a finger, then two | How much can be covered before it's lost |

> **Check yourself:**
> 1. Why do you think fast movement makes detection fail? (Hint: what
>    happens to a photo of something moving fast?)
> 2. Our code trusts far-away tags **less** (see
>    [Where am I?](../part4/16_LOCALIZATION.md)). Does your distance
>    test agree that far tags are worse? How?
> 3. If the robot will see tags from 4 m away while driving at 2 m/s,
>    will it work, based on your measurements?

**Checkpoint:** your limits table, with the averages from your three
tries.

## Share what you learned

Post both tables in Slack, and add the useful rows to the
[Troubleshooting table in Change the Pi code](14_PI_CHANGES.md#troubleshooting)
with a pull request. The next person who sees "`:1181` is blank" at a
competition will thank you.

Next: [Part 4: Where am I?](../part4/16_LOCALIZATION.md)
