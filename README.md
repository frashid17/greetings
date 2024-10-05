

---

# Hello Fahim - Wake-up Script Automation

This project sets up a Python script to automatically run when your Mac wakes up from sleep. The script will greet you with "Hello Fahim". We achieve this using the `SleepWatcher` tool and macOS' `launchd` for background automation.

## Prerequisites

### 1. Homebrew
Make sure you have [Homebrew](https://brew.sh/) installed to manage packages on your macOS.

If Homebrew is not installed, you can install it using this command in your terminal:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. SleepWatcher
Install `SleepWatcher`, a tool that triggers scripts on sleep and wake events:

```bash
brew install sleepwatcher
```

### 3. Python
Ensure Python 3 is installed and accessible via `/usr/bin/python3`. You can install it via Homebrew if necessary:

```bash
brew install python
```

## Project Structure

```plaintext
.
├── hello_fahim.py           # Python script to be run on wakeup
├── wakeup.sh                # Shell script to trigger the Python script
└── de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist # LaunchAgent configuration for SleepWatcher
```

### Python Script (`hello_fahim.py`)

This script uses Python’s `os` module to trigger a greeting when the Mac wakes up.

**Content of `hello_fahim.py`:**

```python
import os

os.system('say "Hello Fahim"')
```

Make sure the script is executable:

```bash
chmod +x ~/Desktop/hello_fahim.py
```

### Shell Script (`wakeup.sh`)

This shell script calls the Python script when the system wakes up.

**Content of `wakeup.sh`:**

```bash
#!/bin/bash
/usr/bin/python3 /Users/fahimrashid/Desktop/hello_fahim.py
```

Make it executable:

```bash
chmod +x ~/wakeup.sh
```

### Launch Agent (`.plist` File)

The `.plist` file defines the `launchd` job for SleepWatcher. This file tells `launchd` to start SleepWatcher and monitor the system's sleep and wake events, triggering your `wakeup.sh` on wake.

**Content of `de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist`:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>de.bernhard-baehr.sleepwatcher-20compatibility-localuser</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/sbin/sleepwatcher</string>
        <string>--wakeup</string>
        <string>/Users/fahimrashid/wakeup.sh</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <false/>
</dict>
</plist>
```

Make sure to place this file in `~/Library/LaunchAgents/` and use the following command to load it:

```bash
launchctl load ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist
```

## Installation Steps

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/frashid17/greetings.git
   cd greetings
   ```

2. **Install Dependencies:**

   Make sure `sleepwatcher` and Python are installed using Homebrew:

   ```bash
   brew install sleepwatcher python
   ```

3. **Configure and Load the LaunchAgent:**

   After editing the `.plist` file with the correct paths, load it with `launchctl`:

   ```bash
   launchctl load ~/Library/LaunchAgents/de.bernhard-baehr.sleepwatcher-20compatibility-localuser.plist
   ```

4. **Test the Setup:**

   Put your Mac to sleep and wake it up. The Python script should run, and you'll hear "Hello Fahim" as the system wakes up.

## Troubleshooting

- Ensure the paths in the `.plist` file and `wakeup.sh` are correct.
- Run `launchctl list` to verify that the SleepWatcher service is loaded.
- You can test the Python script directly by running:

  ```bash
  /usr/bin/python3 ~/Desktop/hello_fahim.py
  ```


---

