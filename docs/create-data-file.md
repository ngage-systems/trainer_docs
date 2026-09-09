# Create a data file

Open a project in ESS Control, save a task config, collect trials into a data file, and download the file from Data Manager. See [Connecting to the device](ess-control-quick-start.md) if ESS Control is not already open. After download, you can open the `.trials.dgz` file with [dgread](https://github.com/SheinbergLab/dgread) in Python, MATLAB, or R.

## 1. Open or create a project

Make sure you are in a project. If not, select **Project** → **New Project**, give it a name, and click **Create**.

## 2. Choose the task

Choose the **System**, **Protocol**, and **Variant** you want to use, along with the settings for that task.

## 3. Save the config

At the bottom of that settings pane, click **New** to save the config. Give it a name and click **Save** at the bottom.

## 4. Open the data file

On the **Configs** tab, click **Open** to load that config and open a data file.

## 5. Start the task

Click **Go** to start the task.

## 6. Collect trials

Collect trials as desired.

## 7. Close the data file if needed

If the block was stopped in the middle, click **Close** to close the data file.

## 8. Open Data Manager

In the top right, click the **ESS Control** dropdown and select **Data Manager**.

## 9. Download the file

The data file that was just collected should appear with status **ok**. Select it, then click **Download** → **Download as zip**.

Status **obs_only** means post-processing of the data file failed.

## Analyze the file

Unzip the download and open the `.trials.dgz` file inside. [dgread](https://github.com/SheinbergLab/dgread) reads `.dg` / `.dgz` files in **Python**, **MATLAB**, and **R**. The examples below use Python; MATLAB and R open the same file with `dg_read` and `read.dgz`.

### Python

```bash
pip install dgread numpy
```

Open the trials file. The result is a dict of columns, one row per trial:

```python
import dgread
import numpy as np

d = dgread.dgread("file.trials.dgz")
print(d.keys())
```

Some cells are nested. Unwrap until you have the value:

```python
while isinstance(x, (list, tuple)):
    x = x[0]
```

For timing, use protocol times such as `stim_on`, `target_on`, and `resp_time`.

### MATLAB

```matlab
data = dg_read('file.trials.dgz');
```

### R

```r
library(dgread)
data <- read.dgz('file.trials.dgz')
```

Install steps for MATLAB and R are in the [dgread README](https://github.com/SheinbergLab/dgread).

To extract photos from a camera session, see [Enable the trainer camera](enable-camera.md).

---

[← Connecting to the device](ess-control-quick-start.md) · [SSH into a device →](ssh-into-device.md)
