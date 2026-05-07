---
title: Connecting to a Server with Ashita v4
---

# Connecting to a Server with Ashita v4

This guide explains how to connect to a Final Fantasy XI server using **Ashita v4**.

Ashita v4 does not currently use a launcher GUI during the beta. Instead, you create a boot profile manually and launch the game with `ashita-cli.exe`.

This guide is mainly for connecting to a **private server**, but a retail FFXI example is included near the bottom.

---

## Requirements (Please read this)

Before starting, make sure you have:

- A working Final Fantasy XI client installation. If you need help, follow the [LSB Wiki Guide on how to install retail](https://github.com/LandSandBoat/server/wiki/Client-Setup-Windows). Steps 1 -> 3.
- Ashita v4 downloaded or cloned.
- The server bootloader required by your private server, if one is needed.
- Your server account username and password.
- .NET Frameworks and Microsoft VC++ Redistributables ([Ashita v4 System Requirements](https://docs.ashitaxi.com/installation/requirements/))

Recommended Ashita install location:

```txt
C:\Ashita\
```

Avoid installing Ashita inside:

```txt
C:\Program Files\
C:\Program Files (x86)\
C:\Windows\
```

Also do **not** install Ashita inside your Final Fantasy XI game folder.

---

## 1. Download Ashita v4

Download Ashita v4 from the official repository:

```txt
https://github.com/AshitaXI/Ashita-v4beta
```

You can either download the repository as a ZIP or clone it with Git:

```bat
git clone https://github.com/AshitaXI/Ashita-v4beta.git C:\Ashita
```

After installing, your Ashita folder should contain files such as:

```txt
C:\Ashita\ashita-cli.exe
C:\Ashita\Ashita.dll
C:\Ashita\config\
C:\Ashita\scripts\
C:\Ashita\addons\
C:\Ashita\plugins\
```

---

## 2. Create a Boot Profile

Ashita v4 boot profiles are stored here:

```txt
C:\Ashita\config\boot\
```

Inside that folder, copy the private server example file:

```txt
example-privateserver.ini
```

Rename the copy to something easy to recognize, such as:

```txt
myserver.ini
```

Do **not** edit the original example file directly. Keep it as a backup/reference.

---

## 3. Configure the Server Connection

Open your new profile:

```txt
C:\Ashita\config\boot\myserver.ini
```

Find the section named:

```ini
[ashita.boot]
```

Update it to match your server.

### Example Private Server Profile

```ini
[ashita.launcher]
autoclose = 1
name = My Server

[ashita.boot]
file = .\\bootloader\\xiloader.exe
command = --server <SERVER_HOSTNAME_OR_IP>
gamemodule = ffximain.dll
script = default.txt
args =
```

Replace this:

```txt
<SERVER_HOSTNAME_OR_IP>
```

With your server address.

Example using a hostname:

```ini
command = --server play.myserver.com
```

Example using an IP address:

```ini
command = --server 127.0.0.1
```

---

## 4. Bootloader Notes

Most private servers require a bootloader instead of launching retail PlayOnline normally.

You can download `xiloader.exe` from the LandSandBoat xiloader repository:

```txt
https://github.com/LandSandBoat/xiloader
```

If your server gives you a bootloader named `xiloader.exe`, your profile may look like this instead:

```ini
[ashita.boot]
file = .\\bootloader\\xiloader.exe
command = --server <SERVER_HOSTNAME_OR_IP>
gamemodule = ffximain.dll
script = default.txt
args =
```

Make sure the `file` path points to the actual bootloader location inside your Ashita folder.

---

## 5. Optional Login Arguments

Some private server bootloaders support login arguments.

Example:

```ini
command = --server <SERVER_HOSTNAME_OR_IP> --user YourUsername --pass YourPassword
```

Only use this if your server specifically supports it.

> **Warning:** Putting your username and password in this file stores them in plain text. If you share screenshots, logs, or your Ashita folder, remove your password first.

---

## 6. Launching the Game

You can launch Ashita v4 from Command Prompt or from a desktop shortcut.

---

### Option A: Launch from Command Prompt

Open Command Prompt and run:

```bat
cd C:\Ashita
ashita-cli.exe myserver.ini
```

The profile name should be only the file name, not the full path.

Correct:

```bat
ashita-cli.exe myserver.ini
```

Incorrect:

```bat
ashita-cli.exe C:\Ashita\config\boot\myserver.ini
```

---

### Option B: Create a Desktop Shortcut

1. Open your Ashita folder:

   ```txt
   C:\Ashita\
   ```

2. Right-click:

   ```txt
   ashita-cli.exe
   ```

3. Choose:

   ```txt
   Create shortcut
   ```

4. Rename the shortcut to your server name.

   ```txt
   C:\Ashita4.3\Ashita-cli.exe myserver.ini
   ```

5. Right-click the shortcut and choose:

   ```txt
   Properties
   ```

6. In the `Target` box, add your profile name after `ashita-cli.exe`.

Example:

```txt
C:\Ashita\ashita-cli.exe myserver.ini
```

If your Ashita path has spaces, put quotes around the executable path:

```txt
"C:\Users\YourName\Desktop\Ashita v4\ashita-cli.exe" myserver.ini
```

---

## 7. Retail FFXI Example

If you are connecting to the official retail Final Fantasy XI servers instead of a private server, the boot section is different.

Example retail profile:

```ini
[ashita.boot]
file =
command = /game eAZcFcB
gamemodule = ffximain.dll
script = default.txt
args =
```

For private servers, use the private server setup above instead.

---

## 8. Troubleshooting

### Ashita opens and immediately closes

Check that the profile name is correct:

```bat
ashita-cli.exe myserver.ini
```

Also make sure the file exists here:

```txt
C:\Ashita\config\boot\myserver.ini
```

---

### The game does not connect to the server

Check the `command` line:

```ini
command = --server <SERVER_HOSTNAME_OR_IP>
```

Make sure the hostname or IP is correct.

---

### Bootloader file not found

Check the `file` line:

```ini
file = .\\bootloader\\pol.exe
```

Make sure that file actually exists in your Ashita folder.

For example, this path:

```ini
file = .\\bootloader\\pol.exe
```

Means the file should be here:

```txt
C:\Ashita\bootloader\pol.exe
```

---

### Game launches but Ashita addons/plugins do not load

Make sure your profile has a script set:

```ini
script = default.txt
```

The script should exist here:

```txt
C:\Ashita\scripts\default.txt
```

---

### Password is visible in the config

If you used `--user` and `--pass`, your login is stored in plain text.

Before sharing your config, remove this part:

```txt
--user YourUsername --pass YourPassword
```

---

## Example Complete Private Server Config

```ini
[ashita.launcher]
autoclose = 1
name = My Server

[ashita.boot]
file = .\\bootloader\\pol.exe
command = --server play.myserver.com
gamemodule = ffximain.dll
script = default.txt
args =

[ashita.input]
keyboard.blockinput = 0
keyboard.blockbindsduringinput = 1
keyboard.silentbinds = 0
keyboard.windowskeyenabled = 0
mouse.blockinput = 0
mouse.unhook = 1

[ashita.language]
playonline = 2
ashita = 2

[ashita.logging]
level = 5
crashdumps = 1

[ashita.misc]
addons.silent = 0
aliases.silent = 0
plugins.silent = 0

[ashita.resources]
offsets.use_overrides = 1
pointers.use_overrides = 1
resources.use_overrides = 1

[ashita.taskpool]
threadcount = -1

[ashita.window.startpos]
x = -1
y = -1
```

Replace:

```txt
play.myserver.com
```

With the real server hostname or IP.

---

## Useful Links

- [Ashita v4 GitHub Repository](https://github.com/AshitaXI/Ashita-v4beta)
- [Ashita v4 System Requirements](https://docs.ashitaxi.com/installation/requirements/)
- [Ashita v4 Running Guide](https://docs.ashitaxi.com/usage/running/)
- [Ashita v4 Configuration Guide](https://docs.ashitaxi.com/usage/configurations/)
