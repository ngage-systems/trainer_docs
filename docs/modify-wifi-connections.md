# Managing WiFi Connections with nmcli

If you decide you'd like to connect your trainer to a different WiFi network than the one(s) originally provisioned, follow the instructions below. 

It should be noted that having multiple stored networks (SSIDs) can sometimes lead to unpredictable results when those networks cover overlapping areas, as the trainer can jump from one to another and become seemingly unresponsive. For this reason, we recommend only using only storing multiple SSIDs if they don't cover overlapping areas.

First SSH into a device (see [SSH into a device](ssh-into-device.md)).

## Listing connections

List all saved connections:
```bash
nmcli connection show
```

See details of one connection:
```bash
nmcli connection show "SSID_NAME"
```

## Adding networks

Add and connect immediately:
```bash
sudo nmcli device wifi connect "NEW_SSID" password "PASSWORD"
```

Add without connecting:
```bash
sudo nmcli connection add type wifi ifname wlan0 con-name "NEW_SSID" ssid "NEW_SSID"
sudo nmcli connection modify "NEW_SSID" wifi-sec.key-mgmt wpa-psk wifi-sec.psk "PASSWORD"
```

## Modifying networks

Change the password:
```bash
sudo nmcli connection modify "SSID_NAME" wifi-sec.psk "NEW_PASSWORD"
```

Set autoconnect priority (higher number = preferred when multiple in range):
```bash
sudo nmcli connection modify "SSID_NAME" connection.autoconnect-priority 10
```

Enable/disable autoconnect:
```bash
sudo nmcli connection modify "SSID_NAME" connection.autoconnect yes
sudo nmcli connection modify "SSID_NAME" connection.autoconnect no
```

## Deleting networks

```bash
sudo nmcli connection delete "SSID_NAME"
```

## Applying changes

Most `modify` commands take effect on the next connection. To apply immediately:
```bash
sudo nmcli connection up "SSID_NAME"
```

Note: running `connection up` over an active SSH session on that same connection will briefly drop the link.

