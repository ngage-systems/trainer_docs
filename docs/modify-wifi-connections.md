# Managing WiFi Connections with nmcli

If you decide you'd like to connect your trainer to a different WiFi network than the one(s) originally provisioned, follow the instructions below. 

It should be noted that having multiple stored networks (SSIDs) can sometimes lead to unpredictable results when those networks cover overlapping areas, as the trainer can jump from one to another and become seemingly unresponsive. For this reason, we recommend only storing multiple SSIDs if they don't cover overlapping areas.

First, SSH into a device (see [SSH into a device](ssh-into-device.md)).

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

## Enterprise WiFi (username and password)

Use these steps when the network is **WPA2-Enterprise / 802.1X** and requires a username and password, not a single shared WiFi password. Run the commands over SSH with `sudo`, as elsewhere in this guide. Your campus IT docs usually specify the EAP method, identity format, and domain — substitute those values in the examples below.

### eduroam

#### 1. Create the profile

```bash
sudo nmcli connection add \
  type wifi con-name eduroam ifname wlan0 ssid "eduroam" \
  wifi-sec.key-mgmt wpa-eap \
  802-1x.eap peap \
  802-1x.phase2-auth mschapv2 \
  802-1x.identity "user@university.edu" \
  802-1x.password "yourpassword" \
  802-1x.system-ca-certs yes \
  802-1x.domain-suffix-match "university.edu" \
  connection.autoconnect yes
```

- `802-1x.identity` — usually your full email or username, depending on your institution.
- `802-1x.domain-suffix-match` — the RADIUS server domain from IT docs; validates the server certificate.

#### 2. Bring it up

```bash
sudo nmcli connection up eduroam
```

Note: running `connection up` over an active SSH session on that same connection will briefly drop the link.

#### 3. Verify association and IP

```bash
iw dev wlan0 link
ip addr show wlan0
```

Look for a connected SSID, a non-empty `link/ether` address, and an IP assigned to `wlan0`.

#### 4. Verify traffic uses wlan0

```bash
ping -I wlan0 -c 4 1.1.1.1
curl --interface wlan0 -s https://ifconfig.me
```

### Other campus enterprise networks

Many university secure networks (for example `University-WiFi` or `Campus-Secure`) use the same username-and-password flow as eduroam but with a different SSID, identity format, and domain. Check your campus IT docs for the exact settings.

#### PEAP + MSCHAPv2

Most common for campus secure WiFi:

```bash
sudo nmcli connection add \
  type wifi con-name "Campus-Secure" ifname wlan0 ssid "Campus-Secure" \
  wifi-sec.key-mgmt wpa-eap \
  802-1x.eap peap \
  802-1x.phase2-auth mschapv2 \
  802-1x.identity "USERNAME_OR_EMAIL" \
  802-1x.password "yourpassword" \
  802-1x.system-ca-certs yes \
  802-1x.domain-suffix-match "campus.example.edu" \
  connection.autoconnect yes
```

| Field | Typical source |
|---|---|
| `con-name`, `ssid` | SSID name (e.g. `University-WiFi`, `Campus-Secure`) |
| `802-1x.identity` | Username or `user@domain.edu` per IT |
| `802-1x.domain-suffix-match` | RADIUS/server domain from IT docs |
| `802-1x.password` | Your account password |

#### EAP-TTLS

Some campuses specify TTLS instead of PEAP:

```bash
sudo nmcli connection add \
  type wifi con-name "Campus-Secure" ifname wlan0 ssid "Campus-Secure" \
  wifi-sec.key-mgmt wpa-eap \
  802-1x.eap ttls \
  802-1x.phase2-auth mschapv2 \
  802-1x.identity "USERNAME_OR_EMAIL" \
  802-1x.password "yourpassword" \
  802-1x.anonymous-identity "anonymous@campus.example.edu" \
  802-1x.system-ca-certs yes \
  connection.autoconnect yes
```

TTLS setups vary — `phase2-auth` may be `pap` on some campuses. Use your IT docs rather than guessing.

After creating the profile, follow eduroam steps 2–4 to bring it up and verify the connection.

If IT docs mention certificate-based auth (EAP-TLS), manual `nmcli` setup is uncommon; contact support or import a profile from a machine that already has the network configured.

## Modifying networks

Change the password:
```bash
sudo nmcli connection modify "SSID_NAME" wifi-sec.psk "NEW_PASSWORD"
```

Change enterprise (802.1X) credentials:
```bash
sudo nmcli connection modify "SSID_NAME" 802-1x.identity "NEW_IDENTITY"
sudo nmcli connection modify "SSID_NAME" 802-1x.password "NEW_PASSWORD"
```

Then run `sudo nmcli connection up "SSID_NAME"` to apply (see [Applying changes](#applying-changes)).

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

