# Install an I/O box

Connect an I/O box to a trainer for digital and analog I/O such as eye tracking. First prepare the host device (usually a trainer) as the PTP grandmaster on Ethernet. Then adopt the I/O box in ESS Control and save the connection to flash.

## Prepare the host

### 1. Update system software

Update **dlsh**, **dserv**, amd **stim2** first so the trainer has current PTP support. Follow [Update system software](update-system-software.md). Always update **dlsh** before **dserv** and **stim2** if those updates are available.

### 2. SSH into the trainer

Connect over SSH. See [SSH into a device](ssh-into-device.md).

### 3. Install PTP packages

On the trainer, run:

```bash
sudo apt update
sudo apt install linuxptp ethtool
```

`ethtool` may already be installed.

### 4. Make the trainer the PTP grandmaster

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

Then restart dserv:

```bash
sudo systemctl restart dserv
```

## Adopt the I/O box

### 1. Connect the I/O box

Plug USB-C power (either port on the I/O box works for this purpose) and Ethernet into the I/O box. 

### 2. Open Extio Boxes

Open ESS Control for the trainer (see [Connecting to the device](ess-control-quick-start.md)). In the top right, click the **ESS Control** dropdown and select **Extio Boxes**.

![ESS Control menu with Extio Boxes](assets/io-box/extio_boxes.png)

### 3. Adopt the box

The I/O box should appear under **ON THIS NETWORK** and after about 30 seconds an "adopt" option should appear. Click **adopt**.

![Adopt an I/O box on the network](assets/io-box/adopt.png)

### 4. Open the box configuration

Within a few seconds, a card should appear for the new box. Click **open** near the top of this window to open the device configuration page.

![Open the adopted I/O box](assets/io-box/open.png)

### 5. Save to flash

At the top of the page, it should notify you that changes were made. Scroll down tot the bottom and click **Save to flash** so the box remembers this connection after reboot.

![Save to flash](assets/io-box/save.png)

### 6. Confirm the box is active

Within about 30-60 seconds, the box should show as connected with (`sync ptp`). It may take up to two minutes to switch from . Inputs, outputs, and analog groups (including **eye**) should update live.

![Active I/O box with PTP synced](assets/io-box/active.png)

### 7. Check eye position

Return to **ESS Control**. The **Eye/Touch Monitor** should show live eye coordinates. Use **Recenter**, **Center**, **Gain**, and **Invert** as needed. Note: when using analog inputs for eye position, the gain settings will likely be quite low (~0.01).

![Eye/Touch Monitor with live eye position](assets/io-box/eye_position.png)

---

[← Update system software](update-system-software.md) · [Enable offline mode →](enable-offline-mode.md)
