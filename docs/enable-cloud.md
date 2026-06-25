# Enable cloud trial upload

Trainer devices can automatically upload each trial outcome to a cloud server as soon as the trial completes. Cloud storage supports same-day performance monitoring, historical trends, and analysis at [ngage.systems/analysis](https://ngage.systems/analysis). It also provides API access to your trial data so you can run analyses from your preferred programming language. Contact [info@ngage.systems](mailto:info@ngage.systems) for more information on the API.

## Security and access

Each step below requires manual approval from an ngage systems administrator for workgroup and device access. Analysis user access is usually granted automatically; contact [info@ngage.systems](mailto:info@ngage.systems) if you need help.

## 1. Set up your workgroup database

Email [info@ngage.systems](mailto:info@ngage.systems) to request cloud storage for your workgroup. An administrator will create a database for your organization.

## 2. Enable cloud upload on a device

### New devices

During provisioning, you will be asked whether to store data from this machine in the cloud. Tap **Yes** to request access for the device.

### Already-provisioned devices

SSH into the trainer and run the enable script (see [SSH into a device](ssh-into-device.md)). The default username is `lab` if you did not change it during provisioning.

```bash
ssh lab@[ip-address-of-trainer]
git clone https://github.com/ngage-systems/provision
sudo bash ./provision/enable_cloud_trial_ingest.sh
```

Wait at least five seconds, then verify the service is running:

```bash
sudo systemctl status dserv -n 100
```

You should see:

> trialsync started. API key and target server loaded. Outbox empty.

An administrator is automatically notified when a new device requests access and will typically grant access to your workgroup's database within about 60 minutes.

## 3. Create an analysis user

1. Visit [ngage.systems/analysis](https://ngage.systems/analysis) and create a user account.
2. An administrator is automatically notified when a new user requests access and will typically grant access to your workgroup's database within about 60 minutes. If you still cannot log in after about an hour, email [info@ngage.systems](mailto:info@ngage.systems) with your account email and workgroup name.

## 4. Log in to the analysis site

Once your account has been approved, you will be able to sign in at [ngage.systems/analysis](https://ngage.systems/analysis) to view trial data and run analyses.

---

[← Connecting to the device](ess-control-quick-start.md) · [Documentation home](../README.md)
