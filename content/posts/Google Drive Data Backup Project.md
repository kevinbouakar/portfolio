---
title: "Google Drive Data Backup Project"
date: 2026-09-22T22:01:02+03:00
lastmod: 2026-09-22T22:01:02+03:00
draft: false
tags: []
slug: "google-drive-data-backup-project"
---

Over the past 8 months, I have suffered 3 major data losses that affected my digital life.

It hurt me a lot. I was devastated. Years of memories and data all wiped within seconds.

This alone led to the creation of this simple backup automation project to protect my future and data.

In order to replicate the same, you will need the following tools:

- Google Drive Desktop App
- Robocopy
- Windows Task Scheduler

You will need to setup where you want your files to go.

First, install Google Drive Desktop App then configure it to **mirror files locally**. This ensures your backups exist both on your machine and in the cloud.

Locate your Google Drive directory (usually inside your user folder; by default it's called `My Drive`, mine is renamed to `GoogleDrive`), then create the following structure:

```plaintext
GoogleDrive\BACKUP\Laptop
```

This will act as your main backup destination. For me, it's backing up all Laptop files I need. You can rename it to whatever you want because `BACKUP` is the root folder for your files.

Place the backup script in a fixed location on your system.

Make sure all paths inside the script match your system (user folder, custom directories like `Github`, `Obsidian`, etc.).

Before automating anything, run the script manually:

```powershell
powershell -ExecutionPolicy Bypass -File "C:\Users\kev\Documents\Programming\Powershell\driveBackup.ps1"
```

The script does the following, in order:

1. Creates the destination folders so robocopy always has somewhere to write.
2. Pauses Google Drive (stops `GoogleDriveFS`) so it doesn't try to sync files while they're being copied.
3. Uses robocopy to copy certain paths into your backup folder.
4. Restarts Google Drive so it uploads the fresh copy.

Here's the robocopy part:

```powershell
$BASE = "$env:USERPROFILE\GoogleDrive\BACKUP\Laptop"
$DEST = $BASE

# Must be an array: a single string gets passed as ONE argument and robocopy rejects it
$roboargs = @("/MIR", "/Z", "/R:2", "/W:2", "/MT:8", "/NFL", "/NDL")

robocopy "$env:USERPROFILE\Documents" "$DEST\Documents" $roboargs
robocopy "$env:USERPROFILE\Pictures" "$DEST\Pictures" $roboargs
robocopy "$env:USERPROFILE\Music" "$DEST\Music"  $roboargs
robocopy "$env:USERPROFILE\Github" "$DEST\Github" $roboargs
robocopy "$env:USERPROFILE\Obsidian" "$DEST\Obsidian" $roboargs
robocopy "$env:USERPROFILE\.ssh" "$DEST\SSH"  $roboargs
robocopy "$env:USERPROFILE\Github\kevinbouakar\winfiles" "$DEST\Configs" $roboargs
robocopy "$env:USERPROFILE\Cursors"  "$DEST\Cursors"  $roboargs
robocopy "$env:USERPROFILE\Fonts"  "$DEST\Fonts"  $roboargs
robocopy "$env:USERPROFILE\Screensavers" "$DEST\Screensavers" $roboargs

Write-Host "Backup completed successfully."
```

> ⚠️ This is not the actual script that I use, but **PART** of it! The full version also checks robocopy's exit codes (0-7 means success, 8 or higher means something failed) so it only reports success when every copy actually worked.

Before you rely on this, know two things:

- **`/MIR` mirrors deletions too.** If a source folder gets wiped, `/MIR` deletes the matching backup, and Google Drive syncs that deletion to the cloud. Drive keeps deleted files in its trash for about 30 days, so check there first if something disappears, and consider keeping occasional versioned snapshots as well.
- **Be careful with secrets.** Copying `.ssh` sends your private keys to the cloud unencrypted. Make sure your keys have a passphrase, or encrypt them before backing them up (which is why my password vault only goes up as an encrypted KeePassXC database).

Now for the final part, you have to schedule the script to run each time depending on your preferences.

```powershell
$Action = New-ScheduledTaskAction `
    -Execute "powershell.exe" `
    -Argument '-NoProfile -ExecutionPolicy Bypass -File "C:\Users\kev\Documents\Programming\Powershell\driveBackup.ps1"'

$Trigger = New-ScheduledTaskTrigger -Daily -At 02:00AM

# Runs the task as soon as possible if the laptop was asleep or off at 2 AM
$Settings = New-ScheduledTaskSettingsSet -StartWhenAvailable

Register-ScheduledTask `
    -TaskName "GoogleDriveBackup" `
    -Action $Action `
    -Trigger $Trigger `
    -Settings $Settings `
    -Force
```

You can modify the `$Trigger` to run the task daily or weekly or at a different time of the day...