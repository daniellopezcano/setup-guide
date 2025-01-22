## What is `screen`?
`screen` is a terminal multiplexer that allows you to manage multiple shell sessions from a single terminal window. It is particularly useful for maintaining long-running processes on remote servers, such as Jupyter notebooks or other computational tasks, without needing an active terminal connection.
### Why Use `screen`?
- **Persistent Sessions**: Detach and reattach sessions without interrupting running processes.
- **Multi-tasking**: Run multiple tasks in separate screen windows and switch between them.
- **Remote Server Workflows**: Keep processes running even when your SSH connection is interrupted.
- **Logging**: Save session output for debugging or later reference.
---
## Installation
Most Linux distributions come with `screen` pre-installed. To check if it is installed, run:
```
screen --version
```
If it is not installed, you can install it using:
- **Debian/Ubuntu**:
    ```
    sudo apt install screen
    ```
---
## Basic Commands

|**Command**|**Description**|
|---|---|
|`screen -S session_name`|Start a new `screen` session with a custom name.|
|`Ctrl-a d`|Detach from the session, leaving it running in the background.|
|`screen -ls`|List all active `screen` sessions.|
|`screen -r session_name`|Reattach to a specific detached session by name.|
|`screen -r`|Reattach to the only running session (if only one exists).|
|`Ctrl-d` or `exit`|Terminate the current `screen` session.|
|`Ctrl-a S`|Split the screen horizontally into multiple regions.|
|`Ctrl-a|`|
|`Ctrl-a Tab`|Switch between split regions.|
|`Ctrl-a X`|Close the active region.|
|`Ctrl-a c`|Create a new window within the session.|
|`Ctrl-a n` / `Ctrl-a p`|Move to the next/previous window.|
|`Ctrl-a w`|List all windows in the current session.|
|`Ctrl-a H`|Start logging session output to `screenlog.0` in the current directory.|
|`Ctrl-a :multiuser on`|Enable multiuser mode for sharing the session.|
|`Ctrl-a :acladd username`|Grant another user access to the session (on the same system).|

---
## Common Use Cases
### Running Jupyter Notebooks Remotely
1. Start a new session:
    ```
    screen -S jupyter
    ```
2. Launch Jupyter in the session:
    ```
    jupyter notebook --no-browser --port=8888
    ```
3. Detach the session:
    ```
    Ctrl-a d
    ```
4. Reattach later if needed:
    ```
    screen -r jupyter
    ```

### Long-Running Scripts
1. Start a new session:
    ```
    screen -S long_task
    ```
2. Run your script:
    ```
    python long_script.py
    ```
3. Detach and reattach as needed.
---
## Best Practices
- **Name Your Sessions**: Use descriptive names for easy identification.
- **Log Outputs**: Enable logging for debugging or reference.