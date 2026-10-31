# Connect to the robot and test your code

This chapter covers getting your laptop talking to a **real robot**
(like the demo bot), setting up the radios, and testing your code on it
without causing trouble for anyone else. In Part 2 our robots use a
**RoboRIO**; the SystemCore differences are in
[Part 5](../part5/18_SYSTEMCORE.md).

Addresses for the RoboRIO and the Pis are in
[SSH and checks](../part3/13_SSH_AND_CHECKS.md#whos-who-on-the-robot-network).

## How the pieces connect

```
Laptop ──Ethernet──▶ Access point (VH-109, "Access Point Mode", on the table)
                         ⋯ 6 GHz wireless ⋯
                     Robot radio (VH-109, "Robot Radio Mode", ON the robot)
                         ──Ethernet──▶ RoboRIO, Pi cameras (via a switch)
```

* There are **two radios**, and they look identical. The **robot radio**
  is mounted on the robot. The **access point** sits near the laptop.
  The robot radio is *not* the RoboRIO.
* The robot radio hands out addresses (`10.1.0.x`) to everything on the
  network, including your laptop.
* At competitions, the field replaces our access point. Our robot radio
  is reprogrammed at the event's radio kiosk.

| Device | Address |
| --- | --- |
| Robot radio | `10.1.0.1` |
| RoboRIO | `10.1.0.2` / `roboRIO-100-FRC.local` |
| Access point | `10.1.0.4` |
| Pi cameras | `10.1.0.30` – `10.1.0.36` |
| Your laptop | `10.1.0.x`, given out automatically |

## The quick path (radios already set up)

1. **Plug your laptop into the access point's `DS` port** with an
   Ethernet cable. Not the PoE port.
1. Set the laptop's Ethernet to **Automatic (DHCP)** (see
   [Laptop network settings](#laptop-network-settings)).
1. Turn on the robot, and wait about 90 seconds for the radios to link.
1. Check:
   ```
   ipconfig
   ping 10.1.0.4
   ping roboRIO-100-FRC.local
   ```
   * On a Mac, use `ping -c 4 …` so it stops after 4 tries (or press
     `Ctrl+C`).
   * `ipconfig` should show `10.1.0.x` on the Ethernet adapter.
   * `10.1.0.4` answering means you reached the access point.
   * `roboRIO-100-FRC.local` (or `10.1.0.2`) answering means you reached the robot
     across the wireless link.
1. Open the **FRC Driver Station** (Windows only), with team number
   `100`. **Communications** should go green.

If any step fails, go to [Troubleshooting](#troubleshooting) below.

**No radios at all?** Plug a USB-B cable from the laptop into the
RoboRIO (see [Deploy](../part1/04_DEPLOY.md)). That's enough to deploy
and drive, as long as the robot is on blocks or tethered.

## Laptop network settings

### Windows: set Ethernet to Automatic (DHCP)

Settings → **Network & internet → Ethernet** → **IP assignment → Edit →
Automatic (DHCP)**.

**Don't trust the Settings screen; check with `ipconfig /all`.** Today
Settings said DHCP was on while `ipconfig /all` said `DHCP Enabled: No`.
That was leftover from typing in a static IP earlier. To force it, open
**Command Prompt as administrator** (Start → type `cmd` → right-click →
**Run as administrator**), and use your adapter's name from
`ipconfig /all` (e.g. `Ethernet adapter Ethernet:` means `"Ethernet"`):

```
netsh interface ip set address name="Ethernet" source=dhcp
netsh interface ip set dns name="Ethernet" source=dhcp
ipconfig /renew "Ethernet"
ipconfig /all
```

You want `DHCP Enabled: Yes` and `IPv4 Address: 10.1.0.x`.

### Mac

System Settings → **Network** → your USB Ethernet adapter → **Details →
TCP/IP → Configure IPv4: Using DHCP**.

### Turn OFF internet sharing

This is what broke everything today. If the laptop is **sharing its
internet** to the Ethernet port, it takes over the adapter and fights the
robot radio, and you get a `169.254…` address or nothing at all.

* **Mac:** System Settings → General → Sharing → **Internet Sharing
  off**.
* **Windows:** turn **Mobile hotspot** off. Then press Win+R, run
  `ncpa.cpl`, right-click the **Wi-Fi** adapter → Properties →
  **Sharing**, and uncheck "Allow other network users to connect…". If
  there's a **Network Bridge** adapter in that window, delete it.

(The Pi setup instructions *do* use internet sharing on purpose, to give
a Pi internet access on the bench. Turn it back off afterwards.)

### What your IP address means

| `ipconfig` shows | Meaning |
| --- | --- |
| `10.1.0.x` | 👍 You're on the robot network |
| `169.254.x.x` | Nothing gave you an address: DHCP is off, internet sharing is on, wrong port, or the radios aren't linked |
| `192.168.69.x` or another static address you typed | Left over from configuring a radio. Switch back to DHCP |
| `192.168.2.x` (Mac) | Internet Sharing is on |

## Setting up the radios

You only do this when a radio is new, reset, or acting up. **Do it with
a mentor.** The official guide is
[Vivid-Hosting: programming your radio](https://frc-radio.vivid-hosting.net/overview/programming-your-radio-at-home).

You'll set the **robot radio** first, then the **access point**, with
exactly matching settings.

### 1. Get to the radio's config page

1. Plug the laptop into the radio's **DS** port.
1. Set the laptop's Ethernet to a **static** address:
   * a brand-new or reset radio: IP `192.168.69.2`, mask `255.255.255.0`,
     then open `http://192.168.69.1/`
   * a radio already set to team 100: IP `10.1.0.10`, mask
     `255.255.255.0`, then open `http://radio.local/` (or
     `http://10.1.0.1/` for the robot radio, `http://10.1.0.4/` for the
     access point)
1. The Vivid-Hosting config page should load.

### 2. Robot radio settings

| Field | Value |
| --- | --- |
| Mode | **Robot Radio Mode** |
| Team Number | `100` |
| SSID Suffix | the robot's name, e.g. `compbotA` (**case-sensitive**) |
| WPA Key for 6 GHz | the team key (**ask a mentor**; don't write it in this repo) |
| Enable SystemCore Mode | ❌ **off** for a RoboRIO robot (✅ on for a SystemCore robot, [Part 5](../part5/18_SYSTEMCORE.md)) |
| 2.4 GHz | optional. Gives a backup Wi-Fi network for testing |

Click **Configure** and wait for "success". **Don't unplug it while it
saves.** If you don't see **Enable SystemCore Mode**, the radio needs a
firmware update. Ours are on 2.0.1.

> **TODO (mentors):** confirm SystemCore Mode must be off for our
> RoboRIO robots.

### 3. Access point settings

Plug the laptop into the **access point's** DS port and open its
config page the same way.

| Field | Value |
| --- | --- |
| Mode | **Access Point Mode** |
| Team Number, SSID Suffix, 6 GHz key | **exactly the same** as the robot radio |
| Enable SystemCore Mode | **the same** as the robot radio |
| Channel | leave the default |

Power the access point from its **VH-117 PoE adapter**. A weaker power
supply can run the config page but not the 6 GHz radio.

### 4. Power-cycle both, then check the link

**Turn both radios off and on again after configuring.** Today the
settings were all correct, but the radios didn't link until they
restarted. Then they linked within seconds, with an "excellent" signal.

Put the radios 2–5 m apart with a clear view of each other (6 GHz is
blocked easily, e.g. by metal), wait about 90 seconds, and check:

* the radio's config/status page shows the 6 GHz link as connected
* the robot radio's backup 2.4 GHz Wi-Fi network disappears from your
  Wi-Fi list (it turns off once linked)
* set the laptop back to **Automatic (DHCP)**, plug into the access
  point, and run through [the quick path](#the-quick-path-radios-already-set-up)

Write the robot's SSID suffix on masking tape on both radios.

## Testing your code on the competition robot

The competition robot is shared, and expensive, and people are counting
on it. So:

1. **Ask first.** Check with the mentor or lead in charge of the robot
   *before* you deploy anything. Someone else may be mid-test.
1. **Know what's on it.** Note which branch/PR is currently deployed, so
   you can put it back.
1. **Test in the simulator first** ([Simulator](../part1/08_SIMULATOR.md)). The robot
   is for things the simulator can't tell you.
1. **Deploy** from your branch (WPILib → **Deploy Robot Code**, choose
   `comp`).

   > 🖥️ **Advanced:** in a terminal, run `./gradlew deploy` in the `comp` folder ([how](../part1/01_SETUP.md#advanced-run-commands-from-a-terminal)).
1. **Make it safe to enable.**
   * Robot **on blocks** (wheels off the ground) for anything new,
     especially autons.
   * For driving tests: clear floor space and a clear area around the
     robot.
   * One person's finger on **Enter** (disable) at all times. Know where
     the robot's main breaker is.
   * Say **"Enabling!"** loudly before you enable.
1. **Check the basics** before you blame your code: Driver Station lights
   green, battery above about 12.5 V, correct auton selected, no errors
   scrolling in the console. See [SSH and checks](../part3/13_SSH_AND_CHECKS.md).
1. **Put it back.** When you're done, redeploy the team's code from the
   `main` branch (or whatever was on it before), unless the mentor says
   otherwise. The same goes for the Pis if you deployed camera code
   ([Pi changes](../part3/14_PI_CHANGES.md#4-deploy-to-a-pi)).
1. **Write down what you learned** in your PR description: what you
   tested, on which robot, and what happened.

## Troubleshooting

Work from the laptop outward:

1. **Laptop:** `ipconfig` shows `169.254…`? Check
   [internet sharing](#turn-off-internet-sharing) and
   [DHCP](#windows-set-ethernet-to-automatic-dhcp) first. These were the
   causes today.
1. **Cable:** use the **DS** port, not PoE. Try the cable and port that
   work for someone else.
1. **Robot radio by itself:** plug the laptop straight into the **robot
   radio's** DS port. Get `10.1.0.x` and `ping 10.1.0.2` replies?
   Then the robot side is fine, and the problem is the wireless link.
1. **Wireless link:** both radios must have the same team number, SSID
   suffix (case-sensitive), and 6 GHz key; one in Access Point Mode, one
   in Robot Radio Mode; SystemCore Mode the same on both. Then
   power-cycle both.
1. **`roboRIO-100-FRC.local` doesn't resolve, but `10.1.0.2` works:** that's a
   name-lookup problem on the laptop, not the robot. Just use the IP.
1. **Nothing works:** go back to a USB-B cable straight to the RoboRIO
   ([Deploy](../part1/04_DEPLOY.md)).

## Good to know

* The **FRC Driver Station** only runs on **Windows**. On a Mac you can
  still view NetworkTables with **OutlineViewer**, **Glass**,
  **AdvantageScope**, or **Elastic** (connect to `10.1.0.2`). To deploy
  from a Mac, see
  [Using a Mac with an Ethernet cable](../part1/04_DEPLOY.md#using-a-mac-with-an-ethernet-cable).

Next: [Discus (easy mode)](11_DISCUS.md)
