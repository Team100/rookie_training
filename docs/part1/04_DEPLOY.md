# Deploy to the RoboRIO

Your code builds on your laptop. Before changing anything, make sure you
can **deploy** it, which means copying it to a robot controller and
running it there. For now that's a **RoboRIO** test board.

1. Ask a mentor for a RoboRIO test board, and for help powering it up.
   Wait about a minute for it to boot.

   > 📷 **TODO photo:** `docs/img/roborio_test_board.jpg`: the RoboRIO
   > test board, powered up.

1. Connect your laptop to the RoboRIO with a **USB-B** cable (the square
   "printer" kind).

   > 📷 **TODO photo:** `docs/img/roborio_usb_b.jpg`: the USB-B cable
   > plugged into the RoboRIO.

1. Make sure the project knows our team number:
   `Ctrl+Shift+P` → **WPILib: Set Team Number** → `100`.
1. Click the **W** button in the top right of VS Code (or press
   `Ctrl+Shift+P`) and choose **WPILib: Deploy Robot Code**.

   ![deploy](../img/wpilib_deploy.png)

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
[study](06_STUDY.md) soon.

## If the deploy fails

* **Can't find the robot:** check the cable, and wait for the RoboRIO to
  finish booting. Try `ping roboRIO-100-FRC.local` or
  `ping 172.22.11.2` (the RoboRIO's USB address).
* **"Team number not found":** you skipped the **Set Team Number** step.
* **Deploy works but the code crashes:** read the console from the top.
  The first error is the one that matters. See
  [Debugging](../reference/DEBUGGING.md).

Next: [Your first motor](05_FIRST_MOTOR.md)
