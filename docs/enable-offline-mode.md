# Enable offline mode

Offline mode tells the trainer not to register with [dserv.net](https://dserv.net) or automatically update tasks on start. Use this when the device will run without online access to the mesh or workgroup services.

## 1. Update dserv

Update system software first so the device has a current dserv that supports the offline signal. Follow [Update system software](update-system-software.md). Always update **dlsh** before **stim2** and **dserv** if those updates are available.

## 2. SSH into the device

Connect over SSH. See [SSH into a device](ssh-into-device.md).

## 3. Create the offline signal file

On the device, run:

```bash
sudo touch /usr/local/dserv/local/offline
```

This creates an empty file that dserv treats as a signal to run in offline mode.

## 4. Reboot

```bash
sudo reboot
```

After reboot, the device no longer expects online registration with dserv.net or automatic task updates at start.

## Connecting after offline mode

In offline mode the device does **not** create a Lab Mesh directory, so browsing to the bare IP (for example `http://192.168.0.10`) will not work.

1. Get the device IP from your router’s client/DHCP list (or another local network tool).
2. Open ESS Control by adding port **2565** to that IP:

   `http://192.168.0.10:2565`

Replace the example IP with the address assigned by your router.

## Return to online mode

To leave offline mode, SSH in and delete the signal file:

```bash
sudo rm /usr/local/dserv/local/offline
sudo reboot
```

After reboot, the device registers with dserv.net again and resumes automatic task updates on start.

---

[← Install an I/O box](install-iobox.md) · [Documentation home](../README.md)
