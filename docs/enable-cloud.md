# Enable cloud trial upload

Trainer devices can automatically upload each trial outcome to a cloud server as soon as the trial completes. Cloud storage supports same-day performance monitoring, historical trends, and analysis at [ngage.systems/analysis](https://ngage.systems/analysis). It also provides API access to your trial data so you can run analyses from your preferred programming language. Contact [info@ngage.systems](mailto:info@ngage.systems) for more information on the API.

## Security and access

Each step below requires manual approval from an ngage systems administrator. Contact [info@ngage.systems](mailto:info@ngage.systems) when prompted.

## 1. Set up your workgroup database

Email [info@ngage.systems](mailto:info@ngage.systems) to request cloud storage for your workgroup. An administrator will create a database for your organization.

## 2. Enable cloud upload on a device

### New devices

During provisioning, you will be asked whether to store data from this machine in the cloud. Tap **Yes** to request access for the device.

### Already-provisioned devices

SSH into the trainer and run the enable script. To find the trainer IP address, open your workgroup page at `https://dserv.net/w/[workgroup-name]` (see [Connecting to the device](ess-control-quick-start.md)).

```bash
ssh [user]@[ip-address-of-trainer]
git clone https://github.com/ngage-systems/provision
sudo bash ./provision/enable_cloud_trial_ingest.sh
```

Wait at least five seconds, then verify the service is running:

```bash
sudo systemctl status dserv -n 100
```

You should see:

> trialsync started. API key and target server loaded. Outbox empty.

Then email [info@ngage.systems](mailto:info@ngage.systems) to grant this device access to your workgroup's database.

## 3. Create an analysis user

1. Visit [ngage.systems/analysis](https://ngage.systems/analysis) and create a user account.
2. Email [info@ngage.systems](mailto:info@ngage.systems) to grant that account access to your workgroup's database.

---

[← Connecting to the device](ess-control-quick-start.md) · [Documentation home](../README.md)
