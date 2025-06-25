# WhatsApp Switcher Automation

A Python utility to bulk-send personalized WhatsApp messages by automating either WhatsApp Web or the native WhatsApp Desktop app on Windows.

---

## Table of Contents

1. [Overview](#overview)  
2. [Requirements](#requirements)  
3. [Installation](#installation)  
4. [Usage](#usage)  
5. [Configuration](#configuration)  
6. [Pros](#pros)  
7. [Cons](#cons)  
8. [Limitations](#limitations)  
9. [Contributing](#contributing)  
10. [License](#license)  

---

## Overview

This script loops through a hard-coded list of phone numbers and names and sends a simple “Hello `<Name>`! This is your message #X of Y.” via WhatsApp. It supports:

- **WhatsApp Desktop** (via the `whatsapp://send?phone=…` URI)  
- **WhatsApp Web** (opens or reuses a browser tab)

Mode can be set to:

- `desktop` – use Desktop app only  
- `web` – use WhatsApp Web only  
- `auto` – try Desktop first, fall back to Web  

---

## Requirements

- **OS:** Windows 10 or newer  
- **Python:** 3.6+  
- **Dependencies:**
  - pygetwindow  
  - pyautogui  
  - psutil  
  - pywin32  

---

## Installation

1. Clone this repo or copy `wp.py` into your project directory.  
2. Install dependencies:
   ```bash
   pip install pygetwindow pyautogui psutil pywin32
   ```
3. Ensure you are already logged into WhatsApp Web/Desktop on your machine.

---

## Usage

```bash
python wp.py
```

By default, the script runs in `auto` mode. To change mode or recipients, edit the bottom of the file:

```python
if __name__ == "__main__":
    # mode: "desktop", "web", or "auto"
    send_whatsapp(mode="auto")
```

---

## Configuration

All configuration is done by editing `wp.py` directly:

- **Recipient list:**  
  ```python
  contacts = {
      "201234567890": "Alice",
      "201098765432": "Bob",
      # …
  }
  ```
- **Message template:**  
  Inside `send_whatsapp()`, the message is built as:
  ```python
  f"Hello {name}! This is message #{i+1} of {total}."
  ```
- **Delays:**  
  - Desktop send delay: `time.sleep(5)`  
  - Web send delay: `time.sleep(2)`  

---

## Pros

- **Dual-mode support**  
  Can target both the Desktop app and Web client, with automatic fallback.  
- **Window reuse**  
  Finds and focuses an existing WhatsApp window/tab, avoiding spawning multiples.  
- **Simple code structure**  
  Separate functions for each mode make it easy to extend or refactor.  
- **Progress output**  
  Prints out which contact is being messaged and overall progress.

---

## Cons

- **Hard-coded data**  
  Contacts, mode, and message template live in the script—no CLI or external config file.  
- **Fixed timing**  
  Uses static `sleep()` values that may be too short/long depending on your machine and network.  
- **Limited browser detection**  
  Only recognizes a small set of executables (`chrome.exe`, `msedge.exe`, etc.).  
- **Argparse unused**  
  Although imported, no command-line interface is actually wired up.

---

## Limitations

1. **Windows-only**  
   Relies on Windows-specific APIs (`win32process`, `os.startfile`, etc.).  
2. **GUI automation**  
   Uses `pyautogui` to manipulate the UI; will fail in headless or locked-session environments.  
3. **No login handling**  
   Assumes the user is already authenticated in WhatsApp Web/Desktop.  
4. **No error recovery**  
   If a send operation fails (e.g. window not found or network hiccup), script does not retry or skip gracefully.

---

## Contributing

1. Fork the repo  
2. Create a feature branch (`git checkout -b feature-name`)  
3. Commit your changes (`git commit -m "Add feature"`)  
4. Push to your branch (`git push origin feature-name`)  
5. Open a Pull Request  

Please open issues for any bugs or feature requests!

---

## License

This project is released under the MIT License. Feel free to use, modify, and distribute!
