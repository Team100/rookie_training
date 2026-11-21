# Deploy to the RoboRIO

Your code builds on your laptop. Before changing anything, make sure you
can **deploy** it, which means copying it to a robot controller and
running it there. For now that's a **RoboRIO** test board.

1. Ask a mentor for a RoboRIO test board, and for help powering it up.
   Wait about a minute for it to boot.

   > 📷 **TODO photo:** `docs/img/roborio_test_board.jpg`: the RoboRIO
   > test board, powered up.

1. Connect your laptop to the RoboRIO. Pick **one** cable:
   * **Windows, USB-B cable** (the square "printer" kind): the easiest.
     It needs the RoboRIO's USB driver, which comes with the
     **FRC Game Tools** ([download](https://www.ni.com/en/support/downloads/drivers/download.frc-game-tools.html)).
     Check whether yours has them: no **FRC Driver Station** in the
     Start menu means install them. (TODO: confirm which team laptops
     have them.)
   * **Mac:** USB won't work (macOS has no driver for the RoboRIO's
     USB network). Use an Ethernet cable and follow
     [Using an Ethernet cable](#using-an-ethernet-cable-mac-or-windows).
   * **Windows, Ethernet cable:** works too, see
     [Using an Ethernet cable](#using-an-ethernet-cable-mac-or-windows).

   > 📷 **TODO photo:** `docs/img/roborio_usb_b.jpg`: the USB-B cable
   > plugged into the RoboRIO.

1. Make sure the project knows our team number:
   `Ctrl+Shift+P` → **WPILib: Set Team Number** → `100`.

   > 🖥️ **Advanced:** open `.wpilib/wpilib_preferences.json` in your
   > project and check it says `"teamNumber": 100`.

1. Click the **W** button in the top right of VS Code (or press
   `Ctrl+Shift+P`) and choose **WPILib: Deploy Robot Code**.

   ![deploy](../img/wpilib_deploy.png)

   > 🖥️ **Advanced:** in a terminal, run `./gradlew deploy` ([how](01_SETUP.md#advanced-run-commands-from-a-terminal)).

1. Watch the terminal. You want `BUILD SUCCESSFUL`, and then the robot
   console output.

   ![console](../img/rio_log.png)

## Watch the console

Open the **FRC Driver Station** and click the gear icon → **View
Console** to see what the robot code prints.

![](../img/view_console.png)

If you deployed `comp` to a test board, expect **lots of errors**. The
competition code is looking for motors and cameras that your test board
doesn't have. That's fine, and it's why you'll make your own
[study](07_STUDY.md) soon.

## If the deploy fails

* **Can't find the robot over USB (Windows):** check the cable (some
  USB cables only charge), and wait for the RoboRIO to finish booting.
  Try `ping 172.22.11.2` (the RoboRIO's USB address). No answer? Open
  **Device Manager → Network adapters**: there should be an NI/RoboRIO
  USB adapter. Missing, or a yellow ⚠? Install the
  [FRC Game Tools](https://www.ni.com/en/support/downloads/drivers/download.frc-game-tools.html)
  and plug the cable in again. Or switch to
  [an Ethernet cable](#using-an-ethernet-cable-mac-or-windows).
* **Can't find the robot over Ethernet:** work through
  [Using an Ethernet cable](#using-an-ethernet-cable-mac-or-windows).
* **"Team number not found":** you skipped the **Set Team Number** step.
* **Deploy works but the code crashes:** read the console from the top.
  The first error is the one that matters. See
  [Debugging](../reference/DEBUGGING.md).

## Using an Ethernet cable (Mac or Windows)

Plug an **Ethernet cable** from your laptop (a USB or Thunderbolt
Ethernet adapter is fine) into the RoboRIO's Ethernet port. If the
deploy can't find the robot, work through these steps **in order**.
Each one checks one link in the chain, so you know exactly where it
breaks. Each step has a **Mac** part and a **Windows** part: do the one
for your laptop.

> The **FRC Driver Station only runs on Windows**, so a Mac can deploy
> but can't enable the robot. For anything that has to *move*, a team
> Windows laptop runs the Driver Station (it can be plugged in at the
> same time, over USB).

### What's an IP address?

Every device on a network has an **IP address**, like a phone number,
e.g. `10.1.0.2`. To call each other, two devices must be on the same
**network**: their numbers have to start the same way (like the same
area code). `10.1.0.2` and `10.1.0.50` can talk; `10.1.0.2` and
`169.254.3.7` can't.

Devices also have **names**, like `roboRIO-100-FRC.local`, so you don't
have to remember numbers. Your laptop looks the name up to find the
number (the `.local` part means "ask the devices on this cable").

**`ping`** is how you check whether two devices can talk: it sends a
tiny "hello?" message and waits for an answer.

> On a Mac, `ping` **never stops by itself**. That's why the Mac `ping`
> commands on this page have **`-c 4`**: send 4 tries, then stop. If
> you forget it, press **`Ctrl+C`** to stop. On Windows, leave out
> `-c 4`: `ping` stops after 4 tries on its own.

### Step 1: Is the cable connected?

1. Turn **Wi-Fi off** for now, so the laptop only has one network to
   think about. Turn off any **VPN** too.
1. Check that the laptop sees the cable:
   * **Mac:** open **System Settings → Network**. Find your Ethernet
     adapter (it may be called **Ethernet**, **USB 10/100/1000 LAN**,
     **Thunderbolt Ethernet**, or a chip name like **AX88179A**). It
     should have a **green dot** and say **Connected**.

     > 📷 **TODO screenshot:** `docs/img/mac_network_list.png`: System
     > Settings → Network, with the Ethernet adapter's green dot and
     > "Connected" circled, and Wi-Fi off.
   * **Windows:** open **Settings → Network & internet**. **Ethernet**
     (maybe **Ethernet 2**) should say **Connected** or
     **No internet** (that's fine: the RoboRIO has no internet). If it
     says **Not connected** or **Network cable unplugged**, the cable
     isn't working.

     > 📷 **TODO screenshot:** `docs/img/win_network_ethernet.png`:
     > Settings → Network & internet, with Ethernet showing
     > "No internet".
1. The lights next to the RoboRIO's Ethernet port should be on or
   blinking.

Not connected, or no lights? Try another cable or adapter, and make
sure the RoboRIO is powered and has finished booting (about a minute).

### Step 2: Ping the RoboRIO by name

Open a terminal (**Mac:** the **Terminal** app; **Windows:**
**PowerShell** or **Command Prompt**) and run:

* **Mac:** `ping -c 4 roboRIO-100-FRC.local`
* **Windows:** `ping roboRIO-100-FRC.local`

> 📷 **TODO screenshot:** `docs/img/mac_ping_ok.png`: Terminal showing
> `ping` getting `64 bytes from ...` answers.

* **Lines like `64 bytes from 10.1.0.2: ... time=1.2 ms`** (Windows:
  **`Reply from 10.1.0.2: ...`**): it answers! Note the number in that
  line: that's the RoboRIO's IP address. Skip to [Step 5](#step-5-deploy).
* **`cannot resolve`**, **`could not find host`**, **`Request timeout`**
  or **`Request timed out`**: the laptop can't find it by name. Go to
  Step 3. (Windows is often bad at `.local` names, so this is common
  there.)

### Step 3: Check your laptop's IP address

* **Mac:** in **System Settings → Network → (your Ethernet adapter) →
  Details → TCP/IP**, look at **IP address**. Or, in Terminal:
  `ifconfig | grep "inet "` (ignore `127.0.0.1`; that's the Mac talking
  to itself).

  > 📷 **TODO screenshot:** `docs/img/mac_network_details.png`: the
  > Ethernet adapter's page, with the **Details…** button circled.

  > 📷 **TODO screenshot:** `docs/img/mac_tcpip_selfassigned.png`: the
  > TCP/IP tab showing a `169.254.x.x` self-assigned address.
* **Windows:** in PowerShell or Command Prompt, run `ipconfig`. Find
  the block for **Ethernet adapter Ethernet** (not Wi-Fi) and look at
  **IPv4 Address** or **Autoconfiguration IPv4 Address**.

What you see:

* **`169.254.something`:** the laptop couldn't get an address from
  anyone, so it made one up ("self-assigned"). The RoboRIO usually does
  the same on a direct cable, and then the name from Step 2 should
  work. Wait a minute after plugging in, and try Step 2 again. Still
  nothing? Go to Step 4.
* **`10.1.0.something`:** you're on the robot's network. Try
  `ping -c 4 10.1.0.2` (Windows: `ping 10.1.0.2`).
* **Nothing at all for Ethernet:** go back to Step 1.

### Step 4: Give your laptop a fixed address

Many of our RoboRIOs are set to the fixed address **`10.1.0.2`**. Your
laptop needs an address on the same network. Type in **exactly** these
values:

| Setting | Value |
| --- | --- |
| IP address | `10.1.0.50` |
| Subnet mask | `255.255.0.0` |
| Router / Gateway | leave **empty** |
| DNS | leave **empty** |

Two laptops on the same robot? Each needs its own number: use
`10.1.0.50`, `10.1.0.51`, and so on. (Don't use `10.1.0.2`; that's the
RoboRIO.)

**Mac:**

1. **System Settings → Network → (your Ethernet adapter) → Details →
   TCP/IP.**
1. **Configure IPv4: Manually.**
1. **IP address:** `10.1.0.50`. **Subnet mask:** `255.255.0.0`.
   Leave **Router** empty. Click **OK**.

   ![Mac TCP/IP settings: Manually, 10.1.0.50, 255.255.0.0, Router empty, OK](../img/mac_tcpip_manual.png)
1. In Terminal: `ping -c 4 10.1.0.2`.

**Windows 11:**

1. **Settings → Network & internet → Ethernet.**
1. Next to **IP assignment**, click **Edit**.
1. Change **Automatic (DHCP)** to **Manual**, and turn **IPv4** on.
1. **IP address:** `10.1.0.50`. **Subnet mask:** `255.255.0.0`. Leave
   **Gateway**, **Preferred DNS** and **Alternate DNS** empty. Click
   **Save**.
   * Asks for **Subnet prefix length** instead of a subnet mask? Type
     `16` (that's the same thing as `255.255.0.0`).

   > 📷 **TODO screenshot:** `docs/img/win_ip_manual.png`: the Edit IP
   > settings box with Manual, IPv4 on, 10.1.0.50, 255.255.0.0,
   > Gateway empty, Save.
1. In PowerShell: `ping 10.1.0.2`.

> 🖥️ **Advanced (any Windows):** press `Win+R`, type `ncpa.cpl`, Enter.
> Right-click **Ethernet → Properties → Internet Protocol Version 4
> (TCP/IPv4) → Properties → Use the following IP address**, and type
> the same values. Leave **Default gateway** and the DNS boxes empty.

> **TODO:** check which of our RoboRIO test boards use a fixed
> `10.1.0.2` and which use automatic addresses, and write it on each
> board.

**When you're done**, put it back to automatic, or your laptop won't
work on other networks (like school or home Ethernet):

* **Mac:** **Configure IPv4 → Using DHCP**.
* **Windows:** **IP assignment → Edit → Automatic (DHCP)** (or, in
  `ncpa.cpl`, **Obtain an IP address automatically**).

### Step 5: Deploy

Once `ping` works, deploy from the terminal so you can see everything
([how](01_SETUP.md#advanced-run-commands-from-a-terminal)):

```
./gradlew deploy
```

* It lists every address it tries. Is the one that answered your `ping`
  in the list?
* `ping` works but deploy still fails? Then it's not a network problem.
  Check that `.wpilib/wpilib_preferences.json` says `"teamNumber": 100`,
  and read the **first** `error:` line. Post that line in Slack.

**Checkpoint:** `BUILD SUCCESSFUL`, and the deploy says which address
it used.

Next: [Your first motor](05_FIRST_MOTOR.md)
