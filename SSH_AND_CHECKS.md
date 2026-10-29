# SSH into the SystemCore and the Pis

The SystemCore and the Raspberry Pis are both small Linux computers. When
something on the robot isn't working, the fastest way to find out why is
to log in and look. This page shows you how to log in and what to check.

`ssh` is built into Windows, Mac, and Linux. On Windows, open **Command
Prompt** or **PowerShell**.

## Who's who on the robot network

| Device | Address | ssh login | Password |
| --- | --- | --- | --- |
| Robot radio | `10.1.0.1` | — | — |
| SystemCore | `10.1.0.2` or `robot.local` | `systemcore` | `systemcore` |
| Driver Station laptop | `10.1.0.5` | — | — |
| Raspberry Pi cameras | `10.1.0.30` – `10.1.0.36` | `pi` | `raspberry` |

Each Pi's IP is written on its case. See [network.md](https://github.com/Team100/all26/blob/2027/lib/doc/network.md)
for the full scheme.

## Connect your laptop

* **To the whole robot:** plug into the access point's (or robot
  radio's) **DS** port, with Ethernet set to **Automatic (DHCP)**. You
  should get a `10.1.0.x` address. If you get `169.254…`, see
  [Connect to the robot](ROBOT_NETWORK.md#troubleshooting).
* **To just the SystemCore:** a USB-C cable works without changing any
  settings. The SystemCore is then at `172.26.0.1` (Windows) or
  `172.27.0.1` (Mac/Linux); older SystemCore images use `172.28.0.1` /
  `172.29.0.1`.
* **To a Pi on the bench, without a robot:** set your laptop to
  `10.1.0.2` so it pretends to be the SystemCore
  ([how](https://github.com/Team100/all26/blob/2027/lib/doc/network.md#impersonating-the-roborio)).

First check that you can reach it at all:

```
ping 10.1.0.2
ping 10.1.0.31
```

If `ping` fails, `ssh` will fail too. Fix the cable and IP first.

## SystemCore

```
ssh systemcore@robot.local
```

(or `ssh systemcore@10.1.0.2`, or the USB address). The first time, type
`yes` to trust the host. The password is `systemcore`. **Never change
it,** because deploys from VS Code stop working if you do.

### Things to check

| Question | Command | Good answer |
| --- | --- | --- |
| Which SystemCore is this? | `cat /sys/firmware/devicetree/base/serial-number; echo` | A serial that matches an entry in [Identity.java](https://github.com/Team100/all26/blob/2027/lib/src/main/java/org/team100/lib/config/Identity.java) |
| What are its addresses? | `ip -brief addr` | `10.1.0.2` on the Ethernet interface |
| Can it see the radio? | `ping -c 3 10.1.0.1` | 3 replies |
| Can it see the cameras? | `ping -c 3 10.1.0.31` (each Pi) | 3 replies |
| Are the CAN buses up? | `ip -brief link \| grep can` | `can_s0` … `can_s4` say `UP` |
| Is CAN traffic flowing? | `ip -s link show can_s0` | RX packets going up when run twice; few errors |
| Is our code deployed? | `ls -l /home/systemcore/deploy` | Files with a recent date |
| Is the disk full? | `df -h /` | `Use%` well under 90% |
| How long since reboot? | `uptime` | Rebooting by itself means a power problem |
| USB drive plugged in? | `ls /U` | Your files (USB drives mount at `/U`, `/V`, …) |

Robot program output (prints, stack traces) is easiest to read in the
**Driver Station console** or the **web dashboard** at http://robot.local.

Type `exit` to log out.

## Raspberry Pi

```
ssh pi@10.1.0.31
```

The password is `raspberry`.

### Things to check

| Question | Command | Good answer |
| --- | --- | --- |
| Which Pi is this? | `grep Serial /proc/cpuinfo` | Last 16 characters match an entry in [identity.py](https://github.com/Team100/all26/blob/2027/raspberry_pi/app/config/identity.py) |
| What's its address? | `hostname -I` | The address written on the case |
| Can it see the SystemCore? | `ping -c 3 10.1.0.2` | 3 replies. Without this, no vision data reaches the robot |
| Is our app running? | `supervisorctl status` | `app  RUNNING` |
| What is the app saying? | `tail -n 50 app.stdout`<br>`tail -n 50 app.stderr` | No repeating errors in `stderr` |
| Is a camera attached? | see below | Your camera model is listed |
| Too hot or underpowered? | `vcgencmd measure_temp`<br>`vcgencmd get_throttled` | Under ~80'C; `throttled=0x0` |
| Is the disk full? | `df -h /` | `Use%` well under 90% |

You can also see the app status and logs in a browser at
`http://10.1.0.31:9001`, and the annotated camera view at
`http://10.1.0.31:1181`.

#### Is a camera attached?

The camera can only be used by one program at a time, so stop our app
first, check, then start it again:

```
supervisorctl stop app
rpicam-hello --list-cameras
supervisorctl start app
```

If `rpicam-hello` isn't installed, use Python instead:

```
python3 -c "from picamera2 import Picamera2; print(Picamera2.global_camera_info())"
```

An empty list (`[]`) or "No cameras available" means the ribbon cable
is loose or in backwards. **Power off before touching the cable.**

### Turning a Pi off

Don't just pull the power, because that can corrupt the SD card. Run:

```
sudo shutdown -h now
```

wait for the green LED to stop blinking, then unplug. (Or use the
`shutdown` task at `:9001`; see [use.md](https://github.com/Team100/all26/blob/2027/raspberry_pi/doc/use.md).)

## Common problems

* **`REMOTE HOST IDENTIFICATION HAS CHANGED`**: the device was reimaged,
  or a different Pi now has that IP. This is expected on our robots. Clear
  the old key and try again:
  ```
  ssh-keygen -R 10.1.0.31
  ```
* **`Connection timed out`**: wrong IP, wrong laptop network settings, or
  the device is off. Go back to `ping`.
* **`Connection refused`**: the device is still booting. Wait 30
  seconds.
* **`Permission denied`**: wrong user or password. It's `systemcore@` for
  the SystemCore and `pi@` for the Pis.
* **`robot.local` not found**: name lookup doesn't always work. Use the IP
  address instead.

## Rules

* **Look, don't change.** Don't edit files on the SystemCore or a Pi by
  hand. The next deploy overwrites them, and nobody else will know what
  you did. Change the code in your fork and deploy it
  ([Pi changes](PI_CHANGES.md)).
* **Don't `sudo apt upgrade` a robot at an event.** Updates can break
  things right before a match.
* Tell the drive team before you restart anything on a robot that someone
  else is using.

Next: [Change the Raspberry Pi code and deploy it](PI_CHANGES.md)
