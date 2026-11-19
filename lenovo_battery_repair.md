# Lenovo ThinkPad L15 Gen 2 – Battery Replacement & Troubleshooting Guide

This guide covers how to check battery health and safely replace the internal 45 Wh battery on a **Lenovo ThinkPad L15 Gen 2 (Intel)** (11th Gen Intel Core i7-1165G7 configuration).

---

## 1. Feasibility & Overview

* **Difficulty:** Beginner-friendly (10–15 minutes).
* **Adhesive:** None. The battery is fastened with standard screws and a cable connector—not glued down.
* **Screws:** The bottom chassis cover uses captive screws (they loosen but stay attached to prevent loss).

---

## 2. Phase 1: Health Diagnostic (Before Purchasing)

Check the battery's chemical wear in Windows:

1. Right-click the **Start** button and select **Windows PowerShell** or **Command Prompt**.
2. Run the following command:
   ```cmd
   powercfg /batteryreport /output "%USERPROFILE%\Desktop\battery_report.html"
   ```
3. Open `battery_report.html` from your desktop.
4. Under the **Installed batteries** section, compare:
   * **Design Capacity:** (~45,000 mWh)
   * **Full Charge Capacity:** (Current actual capacity)
   * *Rule of thumb:* If **Full Charge Capacity** is below 60–70% of the **Design Capacity**, replacing the battery will restore noticeable runtime.

---

## 3. Required Tools & Replacement Parts

### Tools
* Small Phillips screwdriver (size **#0** or **#1**).
* Thin plastic pry tool (spudger, guitar pick, or an expired credit/gift card).

### Replacement Battery
* **Capacity / Spec:** 3-cell Li-Polymer, 45 Wh, 11.52V.
* **Common OEM FRU / Part Numbers:**
  * `L20M3P71`
  * `L20L3P71`
  * `5B10W13930`
  *(You can also verify by looking at the label on your existing battery or by entering your laptop's serial number on [Lenovo PC Support](https://support.lenovo.com) under the "Parts" section).*

---

## 4. Step-by-Step Replacement Procedure

### Step 1: Disable Built-in Battery in BIOS (Safety Step)
Lenovo includes an electronic disconnect to prevent accidental shorts while servicing:
1. Disconnect the AC charger and shut down the laptop completely.
2. Turn on the laptop and immediately tap **F1** repeatedly until the BIOS setup menu opens.
3. Use the arrow keys to navigate to **Config** > **Power**.
4. Select **Disable Built-in Battery** and press **Enter**.
5. Confirm by selecting **Yes**.
6. The computer will automatically shut off. The internal battery is now disconnected from the board and the power button will not turn the machine back on until AC power is restored.

---

### Step 2: Remove the Base Cover
1. Place the laptop face down on a soft, non-scratch surface (e.g., a microfiber cloth or towel).
2. Loosen all screws on the bottom cover using your Phillips screwdriver. (Remember: they are captive and do not need to be pulled out completely).
3. Insert your plastic pry tool into the seam between the bottom cover and chassis base.
4. Slide the tool gently along the edges to disengage the internal retaining clips.
5. Lift off the base cover.

---

### Step 3: Replace the Battery
1. Locate the battery pack along the lower section of the chassis.
2. Disconnect the battery harness from the system board by pulling the connector straight back toward the battery (do not pull upward on the wires).
3. Unscrew the mounting screws securing the battery to the chassis.
4. Lift the old battery out of the laptop.
5. Place the new battery into position and align the screw mounts.
6. Reinstall and tighten the mounting screws until snug (avoid over-tightening).
7. Plug the battery cable securely into the motherboard connector.

---

### Step 4: Reassemble & Re-enable Power
1. Realign the base cover onto the chassis and press along the perimeter until all clips snap firmly into place.
2. Tighten the captive Phillips screws.
3. Plug in the **65W USB-C AC power adapter** (this wakes the laptop from the BIOS battery-disabled state).
4. Turn on the laptop.
5. Allow the battery to charge fully to 100% uninterrupted to help calibrate the new battery controller.
