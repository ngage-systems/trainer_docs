# Re-provision a trainer

Re-provisioning boots the trainer from eMMC so the original setup flow runs again. Use this when you need to re-run provisioning (for example to change workgroup, Wi-Fi, hostname, or credentials).

**This erases all local changes on the trainer.** That includes task edits that have not been saved in the cloud, and data that are stored only on this trainer. Data already stored in the cloud are unaffected.

## 1. SSH into the device

Connect over SSH. See [SSH into a device](ssh-into-device.md).

## 2. Open raspi-config

On the device, run:

```bash
sudo raspi-config
```

This is a text menu. Use the arrow keys to move and **Enter** to select.

## 3. Set boot order

1. Choose **Advanced Options**.
2. Choose **Boot Order**.
3. Choose **Option 1**.

Option 1 boots from eMMC (shown as **SD Card Boot** on some Pi models). That starts the provisioning flow on the next boot.

## 4. Finish

Select **Finish** to leave raspi-config.

## 5. Reboot

When you select **Finish**, raspi-config should ask whether you want to reboot. Choose **Yes**.

If it does not ask, run:

```bash
sudo reboot
```

The SSH session will drop when the device reboots.

## 6. Follow on-screen instructions

After reboot, complete the prompts on the trainer display.

When provisioning finishes and the device reboots again, confirm it appears on your workgroup page. See [Connecting to the device](ess-control-quick-start.md).

The first boot after provisioning may show a login screen on the trainer. That is expected; you do not need to log in. Load a task such as **Search** from ESS Control, and **stim2** should take over the screen.

---

[← SSH into a device](ssh-into-device.md) · [Enable cloud trial upload →](enable-cloud.md)
