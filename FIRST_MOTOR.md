# Your first motor (hardware)

This chapter gets one motor spinning on a **SystemCore** test board,
**without writing any code**, so you know the wiring is right before you
start programming.

> Working on an older RoboRIO board? Use the
> [2025 RoboRIO version](ROBORIO_NEO_FIRST_MOTOR.md) instead.

<!--
PHOTO CHECKLIST: take these on a real SystemCore test board, save them in
readme_img/ with these names, and replace each "TODO photo" note below.
  [ ] systemcore_test_board.jpg   whole board: battery, power, SystemCore, motor
  [ ] systemcore_power.jpg        power wiring into the SystemCore
  [ ] systemcore_can_ports.jpg    CAN ports, with can_s0 labeled
  [ ] neo_spark_max_wired.jpg     NEO wired to a SPARK MAX
  [ ] spark_max_can_power.jpg     SPARK MAX power and CAN wires
  [ ] spark_max_can_to_systemcore.jpg  CAN from the SPARK MAX into can_s0
  [ ] spark_max_led_ok.jpg        SPARK MAX blinking normally
  [ ] spark_max_usb_c.jpg         laptop USB-C into the SPARK MAX
  [ ] systemcore_screen.jpg       SystemCore screen after boot
  [ ] kraken_can_to_systemcore.jpg  Kraken alternative wiring
-->

## The test board

Ask a mentor for a SystemCore test board and a freshly charged battery.
Clear a few square feet of table, and don't pile your work on top of
something else.

> 📷 **TODO photo:** `readme_img/systemcore_test_board.jpg`: the whole
> board, labeled: battery, main breaker, power distribution, SystemCore,
> motor controller, motor.

> 📷 **TODO photo:** `readme_img/systemcore_power.jpg`: how power gets
> into the SystemCore.

## Connect the motor

We'll use a **NEO** motor with a **SPARK MAX** controller.

1. Ask a mentor for a NEO, a SPARK MAX, and their wiring.

   ![](readme_img/neo_motor.jpg)

1. Connect the NEO's power and sensor wires to the SPARK MAX.

   > 📷 **TODO photo:** `readme_img/neo_spark_max_wired.jpg`: NEO plugged
   > into the SPARK MAX, both connectors visible.

1. Connect the SPARK MAX to power and to the **CAN bus**. The CAN wires
   are yellow and green; match them to the labels.

   > 📷 **TODO photo:** `readme_img/spark_max_can_power.jpg`: SPARK MAX
   > power wires going to the power distribution, and the CAN wires.

1. Plug the CAN wires into the SystemCore's **CAN port 0** (`can_s0`).
   The SystemCore has five CAN ports, `can_s0` to `can_s4`. In code, port
   0 is `new CanBusId(0)`.

   > 📷 **TODO photo:** `readme_img/systemcore_can_ports.jpg`: close-up of
   > the SystemCore CAN ports, with `can_s0` marked.

   > 📷 **TODO photo:** `readme_img/spark_max_can_to_systemcore.jpg`: CAN
   > from the SPARK MAX going into `can_s0`.

1. Turn on the main breaker. The SPARK MAX light should blink, and after
   a minute the SystemCore screen should show that it has booted.

   > 📷 **TODO photo:** `readme_img/spark_max_led_ok.jpg`: SPARK MAX LED
   > blinking normally.

   > 📷 **TODO photo:** `readme_img/systemcore_screen.jpg`: the SystemCore
   > screen after boot (team number and IP address).

   If the SPARK MAX is dark, check its power. If the light blinks a
   strange color, look it up in the
   [SPARK MAX status LED guide](https://docs.revrobotics.com/brushless/spark-max/status-led).

## Find the CAN ID

Every device on a CAN bus has an **ID number**, like a house number on a
street. Two devices with the same ID cause trouble, and "the motor does
nothing" is very often a CAN ID problem.

You read and set the SPARK MAX's ID with the **REV Hardware Client**
through its USB-C port. The full instructions are
[here](https://docs.revrobotics.com/rev-hardware-client/ion/spark-max);
here's the quick version.

1. Find the REV Hardware Client on the laptop (Windows key, type "REV").
   If it's not there, download it
   [here](https://docs.revrobotics.com/rev-hardware-client).

   ![](readme_img/rev_hardware_client.png)

1. Plug a USB-C cable from the laptop into the SPARK MAX. Click
   **Refresh**.

   > 📷 **TODO photo:** `readme_img/spark_max_usb_c.jpg`: USB-C cable in
   > the SPARK MAX.

   ![](readme_img/rev_hardware_client_working.png)

1. **Check the CAN bus.** Open the **CAN Bus** tab ("Bus OK"), then
   **CAN Data**. You should see messages flowing.

   ![](readme_img/can_bus_flowing.png)

   > 📷 **TODO screenshot:** retake this on the SystemCore board, to show
   > what "OK" looks like there.

1. **Check the power.** Open the **SPARK MAX** tab. The voltage should be
   about 12 V.

   ![](readme_img/spark_controller_voltage.png)

1. **Write down the CAN ID** (top left; it's 7 in the picture). You can
   change it here if it clashes with another device. Also write it on
   masking tape on the motor.
1. **Update the firmware** if the client offers.

## Run the motor

1. Open the **Run** tab. Set **Mode = Percent** and the value to **0.01**
   (1%).
1. Clear hands away and click **Run Motor**.

   ![](readme_img/run_motor.png)

**Checkpoint:** the motor turns, slowly.

## Check from the SystemCore side

The REV client talks to the motor directly. Now check that the
**SystemCore** can see CAN traffic too. [SSH in](SSH_AND_CHECKS.md#systemcore)
and run:

```
ip -s link show can_s0
```

Run it twice. The **RX packets** number should go up. If it doesn't, the
CAN wires aren't reaching the SystemCore: check the port and the
yellow/green order.

## Using a Kraken instead?

Kraken and Falcon motors are CTRE devices. Wire CAN the same way, into
`can_s0`. Then use the **Phoenix Tuner X** app instead of the REV
client to check firmware and set the CAN ID. Our
[test board setup](https://github.com/Team100/all26/blob/2027/lib/doc/setup.md) has the details.

> 📷 **TODO photo:** `readme_img/kraken_can_to_systemcore.jpg`: a Kraken
> wired to the SystemCore.

> 📷 **TODO screenshot:** Phoenix Tuner X showing a Kraken's CAN ID.

Next: [Your first study](STUDY.md)
