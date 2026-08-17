# SSH into a device

SSH lets you run commands on the device from your laptop or desktop. You typically don't need it for day-to-day training, but it is useful when you need direct access to the computer inside the trainer.

## Why use SSH?

- **Check system status** — Inspect `dserv`, the service that runs the trainer software and connects the device to ESS Control:

  ```bash
  sudo systemctl status dserv
  ```

- **Re-provision the device** — Switch the boot drive to the eMMC to run the provisioning flow again. See [Re-provision a trainer](reprovision-trainer.md).

- **Create and modify tasks** — Edit task code on the device remotely using VS Code with **Remote - SSH**, or other editors and AI coding tools that support SSH.

## What you need

- **IP address** — Find it on your workgroup page at `https://dserv.net/w/[workgroup-name]` (see [Connecting to the device](ess-control-quick-start.md) for screenshots and steps).
- **Username and password** — Set during device provisioning. Default username is `lab` if you did not change it.

## Connect via SSH

Replace `lab` with your provisioning username and the IP address with your device's address.

### macOS

Open **Terminal** (`Applications` → `Utilities` → `Terminal`) and run:

```bash
ssh lab@192.168.1.42
```

### Linux

Open your distro's terminal emulator and run:

```bash
ssh lab@192.168.1.42
```

### Windows

Open **PowerShell** or **Command Prompt** and run:

```bash
ssh lab@192.168.1.42
```

OpenSSH is included on Windows 10 and 11. If `ssh` is not recognized, install the **OpenSSH Client** optional feature under **Settings** → **Apps** → **Optional features**.

## First connection

On first login, accept the host key fingerprint when prompted. Enter the password you chose during provisioning (input is hidden while typing).

## Troubleshooting

- Confirm the device appears on the workgroup page and that your computer is on the same Wi-Fi or network.
- Double-check the username (`lab` by default) and password.
- On Windows, verify the OpenSSH **Client** optional feature is installed.

---

[← Connecting to the device](ess-control-quick-start.md) · [Re-provision a trainer →](reprovision-trainer.md)
