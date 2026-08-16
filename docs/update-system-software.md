# Update system software (dserv / stim2 / dlsh)

Use Lab Mesh to install available updates for **dserv**, **stim2**, and **dlsh** on a trainer.

Old method:

## 1. Find the device IP

Open your workgroup page on [dserv.net](https://dserv.net) (see [Connecting to the device](ess-control-quick-start.md)). Note the IP address of the device you want to update.

## 2. Open Lab Mesh

In the browser, go to that IP **without** the port.

For example, if the dserv.net link opens:

`http://192.168.0.10:2565`

browse to:

`http://192.168.0.10`

That page is the **Lab Mesh** directory. It lists all devices on your mesh.

## 3. Open the update panel

Find the device you want to update. On the right, click the **gear** icon to open the side panel.

The panel lists software components (**dserv**, **stim2**, **dlsh**). Components with an available update show a blue **Update** button.

## 4. Apply updates

Update each component that has a blue **Update** button. Always start with **dlsh**, then update **stim2** and **dserv**.

---

[← Enable cloud trial upload](enable-cloud.md) · [Install an I/O box →](install-iobox.md)
