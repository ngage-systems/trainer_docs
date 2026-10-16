# Swap the default Wi-Fi adapter

Older trainers use the Raspberry Pi's built-in Wi-Fi. Newer trainers come with a USB Wi-Fi adapter. This guide switches which one is the default. Saved Wi-Fi networks are reused. The script does not rewrite those profiles.

**If something goes wrong, the trainer can become unreachable over Wi-Fi.** Have an Ethernet cable ready before you start, so you can plug in and reach the device if Wi-Fi does not come back after the reboot.

## Recommended hardware

Newer trainers ship with this adapter and hub. Use the same parts on an older trainer.

- **Wi-Fi adapter:** [EDUP EP-AX1672](https://www.amazon.com/dp/B0CZ82RM5L)
- **USB hub:** [Anker 4-port hub](https://www.amazon.com/dp/B00XMD7KPU?th=1)

Plug the adapter into the hub. There is not enough room to seat the adapter in a trainer USB port, and the hub's extra ports are there for other devices.

The white USB 2 ports give slightly better performance than the blue USB 3 ports. Either works fine. Plug the hub into a white USB 2 port when one is free.

## 1. Plug in the USB adapter

Newer trainers already have a USB Wi-Fi adapter. On an older trainer, plug one in before you choose USB. The script refuses that choice if no USB adapter is visible.

You can leave the adapter unplugged if you are only switching back to the built-in radio.

## 2. SSH into the device

Connect over SSH. See [SSH into a device](ssh-into-device.md).

If that session is already on Wi-Fi, the script warns you. If Wi-Fi does not come back after the reboot, remote access over Wi-Fi is lost. Use a wired connection for this step when you have one.

## 3. Download and run the script

On the device, run:

```bash
curl -fsSLO https://raw.githubusercontent.com/ngage-systems/provision/main/swap_wifi_device.sh
sudo bash swap_wifi_device.sh
```

The script lists the adapters it sees, the current default, and the saved networks. For example:

```
Wi-Fi adapters:
  wlan0 (brcmfmac) built-in
  wlan1 (mt7921u) USB

Configured default: built-in

Saved Wi-Fi networks (these profiles will be reused):
  EXAMPLE-NETWORK
```

Names and drivers depend on the hardware. `brcmfmac` is the built-in radio. A USB adapter shows a different driver, such as `mt7921u`.

## 4. Choose the default

```
Make which adapter the default?
  1) USB (wlan1) — disable the built-in radio
     The USB adapter is tested first. config.txt is unchanged if the test fails.
  2) Built-in — re-enable the built-in radio [current]
Choice [1/2]:
```

- **1** tests the USB adapter, then disables the built-in radio so saved networks attach to it.
- **2** re-enables the built-in radio. There is no USB test.

The current default is marked `[current]`.

If a saved network is tied to one adapter (a specific interface or MAC address), the script warns that it may not connect after the switch. Untie it, then run the script again:

```bash
sudo nmcli connection modify "SSID_NAME" connection.interface-name "" 802-11-wireless.mac-address ""
```

Replace `SSID_NAME` with the profile name from the warning.

## 5. USB test

Choosing **1** while the built-in radio is still running tests the USB adapter before `config.txt` is changed. The script brings up a temporary copy of a saved network on the USB adapter and leaves the current connection in place, then pings that adapter's gateway.

```
Testing EXAMPLE-NETWORK on wlan1 without moving the current connection.
Bringing up EXAMPLE-NETWORK on wlan1. The current connection is left in place.
USB adapter wlan1 reached its gateway. The temporary profile was removed.
```

`EXAMPLE-NETWORK` is whatever saved profile the script tests. The name on your trainer will differ.

If the ping succeeds, the temporary profile is removed and the script continues. If it fails, `config.txt` is left unchanged and the SSH session on the built-in radio stays up.

## 6. Reboot

After a successful USB test, the script backs up `/boot/firmware/config.txt` (for example to `config.txt.bak-20261016-110227`) and adds `dtoverlay=disable-wifi`. Choosing the built-in radio removes that line instead. The change takes effect on the next boot.

```
The change takes effect on the next boot.
Reboot now? [y/N]:
```

Enter `y` to reboot now. The SSH session drops when the device reboots.

If you enter `n`, reboot later when you want the change to take effect:

```bash
sudo reboot
```

## After reboot

Confirm the trainer appears on your workgroup page. See [Connecting to the device](ess-control-quick-start.md).

To switch again, SSH in and run the script. Choose **1** for the USB adapter or **2** for the built-in radio.

If the trainer does not come back on Wi-Fi, connect another way (a wired network, or a keyboard on the trainer) and run the script again. Choose **2** to restore the built-in radio.

---

[← Managing Wi-Fi connections](modify-wifi-connections.md) · [Documentation home](../README.md)
