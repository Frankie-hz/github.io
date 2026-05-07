---
title: Installing and Updating the FFXI Client
---

# Installing and Updating the FFXI Client

This guide walks through installing the official Windows Final Fantasy XI client and updating it through PlayOnline so it is ready to use with Ashita v4.

For the official client download, use Square Enix's Final Fantasy XI Windows installer page:

[Download the official Final Fantasy XI Windows client](https://www.playonline.com/ff11us/download/media/install_win.html)

---

## 1. Download the Official Client

On the Square Enix download page, download every file listed for the Ultimate Collection Seekers Edition installer.

Keep all of the downloaded files in the same folder. The installer is split into multiple parts, so the setup will not extract correctly if any part is missing or moved somewhere else.

After downloading everything, run:

```txt
FFXIFullSetup_US.part1.exe
```

Choose a folder to extract the installer files into. When extraction finishes, you should have a folder named something like:

```txt
FFXIFullSetup_US
```

---

## 2. Install PlayOnline and Final Fantasy XI

Open the extracted setup folder and run:

```txt
FFXISetup.exe
```

If this is a fresh install, select all available components when the installer asks what to install. This usually includes:

- DirectX runtime components
- PlayOnline Viewer
- Final Fantasy XI
- Final Fantasy XI expansions

Let each installer finish before moving to the next one.

When the install is complete, restart your computer. This helps Windows finish registering the PlayOnline and Final Fantasy XI install paths.

---

## 3. Check Windows Components

Final Fantasy XI is an older Windows game, so a few legacy components may be needed.

Make sure DirectPlay is enabled:

1. Open the Windows Start menu.
2. Search for:

   ```txt
   Turn Windows features on or off
   ```

3. Expand:

   ```txt
   Legacy Components
   ```

4. Enable:

   ```txt
   DirectPlay
   ```

5. Click OK and let Windows install the feature.

If PlayOnline, Ashita, Windower, or other tools complain about missing runtimes, install the x86 versions of the required Microsoft Visual C++ runtimes. Even on 64-bit Windows, older FFXI tools often need the x86 runtime packages.

---

## 4. Update PlayOnline

Open PlayOnline Viewer.

If PlayOnline asks to update itself, let it update. It may close and restart during this process.

After PlayOnline finishes updating, continue to the file check steps below.

---

## 5. Create a Temporary PlayOnline Profile

When PlayOnline asks whether you are a new user or existing user, choose the option for existing PlayOnline members.

For the temporary profile fields, you can use placeholder values such as:

```txt
ABCD1234
```

Use that placeholder for the account ID, username, and password fields.

This temporary profile is only used to reach the PlayOnline file check screen. You do not need a valid retail PlayOnline account just to run Check Files.

---

## 6. Run Check Files

In PlayOnline Viewer, click:

```txt
Check Files
```

On the Check Files screen, open the dropdown that may initially say:

```txt
PlayOnline Viewer
```

Change it to:

```txt
FINAL FANTASY XI
```

Then click the Check Files button.

PlayOnline will scan your Final Fantasy XI install. This can take a while, especially on a fresh install.

When the scan finishes, PlayOnline may report errors or mismatched files. In this context, that usually means your local files do not match the current retail client version.

Choose the option to repair or fix the files.

The first full update can take several hours depending on your internet connection and drive speed. Let it finish completely.

---

## 7. If FINAL FANTASY XI Does Not Appear

If the Check Files dropdown does not show `FINAL FANTASY XI`, close PlayOnline and confirm that Final Fantasy XI was installed into the normal Square Enix folder.

Common install paths are:

```txt
C:\Program Files (x86)\PlayOnline\SquareEnix\FINAL FANTASY XI
C:\Program Files\PlayOnline\SquareEnix\FINAL FANTASY XI
```

Some private server communities provide a small client patch that resets the local version information so PlayOnline can detect and update Final Fantasy XI correctly.

Only use a patch from a server or community you trust. After applying it, reopen PlayOnline Viewer and run Check Files again.

---

## 8. Copy the PlayOnline Data Folder

After Final Fantasy XI is fully updated, copy PlayOnline Viewer's `data` folder into the Final Fantasy XI folder.

Copy this folder:

```txt
C:\Program Files (x86)\PlayOnline\SquareEnix\PlayOnlineViewer\data
```

Paste it into:

```txt
C:\Program Files (x86)\PlayOnline\SquareEnix\FINAL FANTASY XI
```

When finished, you should have:

```txt
C:\Program Files (x86)\PlayOnline\SquareEnix\FINAL FANTASY XI\data
```

If you installed Final Fantasy XI somewhere else, use your actual install folder instead.

---

## 9. Updating an Existing Client

Use this section when your Final Fantasy XI client is already installed but needs to be updated to the current client version.

Close Final Fantasy XI, Ashita, Windower, and PlayOnline Viewer.

Open your Final Fantasy XI install folder:

```txt
C:\Program Files (x86)\PlayOnline\SquareEnix\FINAL FANTASY XI
```

Find this file:

```txt
VTABLE.DAT
```

Delete `VTABLE.DAT`.

Then open PlayOnline Viewer and run Check Files again:

1. Click `Check Files`.
2. Select `FINAL FANTASY XI` from the dropdown.
3. Click `Check Files`.
4. When PlayOnline finds files to repair, choose the repair option.
5. Let the update finish.
6. Run Check Files again afterward to confirm Final Fantasy XI shows a current version.

After the update is complete, close PlayOnline Viewer and launch the game with Ashita v4.

---

## 10. Notes for Modified DAT Files

If you use HD DAT files or other manual file replacements, a PlayOnline update may overwrite them.

Before updating, back up any custom DATs you installed manually.

If possible, manage DAT replacements with a tool such as XIPivot so client updates are easier to handle.

---

## 11. Next Step

Once the client is installed and updated, continue here:

[Connecting to a Server with Ashita v4](ashita-v4-server-connection.html)

---

## Useful Links

- [LSB Wiki Guide on how to install retail](https://github.com/LandSandBoat/server/wiki/Client-Setup-Windows)
- [Official Final Fantasy XI Windows Client Download](https://www.playonline.com/ff11us/download/media/install_win.html)
