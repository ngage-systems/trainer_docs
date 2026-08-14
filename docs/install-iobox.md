# Install an I/O box

Connect an I/O box to a trainer for digital and analog I/O such as eye tracking. The trainer must be the PTP grandmaster on Ethernet; then you adopt the box in ESS Control and save the connection to flash.

## 1. Update system software

Update **dlsh**, **dserv**, amd **stim2** first so the trainer has current PTP support. Follow [Update system software](update-system-software.md). Always update **dlsh** before **dserv** and **stim2** if those updates are available.

## 2. SSH into the trainer

Connect over SSH. See [SSH into a device](ssh-into-device.md).

## 3. Install PTP packages

On the trainer, run:

```bash
sudo apt update
sudo apt install linuxptp ethtool
```

`ethtool` may already be installed.

## 4. Make the trainer the PTP grandmaster

```bash
sudo dserv-ptp-setup grandmaster eth0
```

When it finishes you should see:

- `portState MASTER`
- `dserv-ptp4l@eth0.service` enabled / active
- `dserv-phc2sys@eth0.service` enabled / active

Verify the role anytime with:

```bash
sudo dserv-ptp-setup
```

A grandmaster is its own time reference, so `offsetFromMaster 0.0` is expected.

## 5. Connect the I/O box

Plug USB-C power (either port on the I/O box works for this purpose) and Ethernet into the I/O box. 

## 6. Open Extio Boxes

Open ESS Control for the trainer (see [Connecting to the device](ess-control-quick-start.md)). In the top right, click the **ESS Control** dropdown and select **Extio Boxes**.

![ESS Control menu with Extio Boxes](assets/io-box/extio_boxes.png)

## 7. Adopt the box

When the box appears under **ON THIS NETWORK**, click **adopt**.

![Adopt an I/O box on the network](assets/io-box/adopt.png)

## 8. Open the box configuration

Within a few seconds, a card should appear for the new box. Click **open** near the top of this window.

![Open the adopted I/O box](assets/io-box/open.png)

## 9. Save to flash

Scroll down and click **Save to flash** so the box remembers this connection after reboot.

![Save to flash](assets/io-box/save.png)

## 10. Confirm the box is active

Within about 30 seconds, the box should show as connected with PTP synced (`PTP 1/1`). Inputs, outputs, and analog groups (including **eye**) should update live.

![Active I/O box with PTP synced](assets/io-box/active.png)

## 11. Check eye position

Return to **ESS Control**. The **Eye/Touch Monitor** should show live eye coordinates. Use **Recenter**, **Center**, **Gain**, and **Invert** as needed.

![Eye/Touch Monitor with live eye position](assets/io-box/eye_position.png)

---

[← Update system software](update-system-software.md) · [Enable offline mode →](enable-offline-mode.md)
