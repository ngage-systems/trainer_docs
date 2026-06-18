# Interacting with Dserv/ESS via VS Code Tasks

When working with the `dserv`/`ess` laboratory control systems through VS Code in a web environment, running commands directly in the terminal occasionally fails with environment provider issues (e.g., `ENOPRO: No file system provider found for resource 'file://...'`).

To work around this limitation and safely interact with the running system, the best approach is to define your commands as background tasks within `.vscode/tasks.json` and capture their standard output into a dedicated workspace file (e.g., `/home/lab/systems/.agent-last-output.txt`).

## The `tasks.json` Wrapper Pattern

Each command you need to run should be created as a `shell` task that redirects both stdout and stderr (`> file.txt 2>&1`). If a command might hang (such as `ess::load_system`), wrap it with `timeout 30`.

```jsonc
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "ESS: Load System",
      "type": "shell",
      "command": "timeout 30 essctrl -c \"ess::load_system detection visual squares\" > /home/lab/systems/.agent-last-output.txt 2>&1"
    },
    {
      "label": "ESS: Check Loading Progress",
      "type": "shell",
      "command": "timeout 5 essctrl -c \"return [dservGet ess/loading_progress]\" > /home/lab/systems/.agent-last-output.txt 2>&1"
    },
    {
      "label": "ESS: Debug Load System",
      "type": "shell",
      "command": "timeout 30 essctrl -c \"send ess {set e [catch {::ess::load_system detection visual squares} msg]; list catch=$e msg=\\$msg ei=\\$::errorInfo}\" > /home/lab/systems/.agent-last-output.txt 2>&1"
    }
  ]
}
```

### Workflow for Agents

1. **Modify `tasks.json`**: Update or add a task with the specific TCL, `essctrl`, or `dservctl` command you wish to execute.
2. **Execute via API**: Run the task using your VS Code task runner capabilities.
3. **Read Output**: Read the contents of `/home/lab/systems/.agent-last-output.txt` to inspect the results.

### Common Diagnostic Commands

* **Checking the active system state**:
  `dservctl -c "send ess {return [namespace eval ::ess {array get current}]}"`
* **Checking active trial status**:
  `dservctl get ess/action_state`
* **Starting the system**:
  `essctrl -c "ess::start"`
* **Querying internal variables** (e.g. checking a dynlist to verify data formatting):
  `dservctl -c "send ess {catch {return [dl_tcllist [dl_get stimdg:stim_color 0]]} msg; return $msg}"`
* **Checking logs for subprocess errors**:
  `dservctl logs` or `sudo journalctl -u dserv -n 100`

This pattern provides a highly reliable conduit for iterating on the `ESS` codebase, bypassing terminal environment idiosyncrasies.
