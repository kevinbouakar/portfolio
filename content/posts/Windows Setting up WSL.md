---
title: "Windows Setting up WSL"
date: 2025-01-03T11:42:16+02:00
lastmod: 2025-01-03T11:42:16+02:00
draft: false
tags: []
slug: "windows-setting-up-wsl"
---

# 1. Installation:

In order to run Linux Distros through Windows, you must first enable the WSL feature. 
- Open Powershell as ADMINISTRATOR
- Type the following command:
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

After that is done, you must be on Windows 10 Build 18362 or higher to install WSL.
Check your version of windows by running CMD Prompt as ADMINISTRATOR & type the following command: `winver` 

A small window will pop up showing you the version of Windows running on your machine.

Next, install the distro of your choice using the `curl` command. 
Use the following [link](https://learn.microsoft.com/en-us/windows/wsl/install-manual#downloading-distributions) to check which distributions you can install.
After its downloaded, double click on the file you install and everything should be good to go.

> :RiErrorWarningFill: If you receive the following error while installing your distribution:
> "Error: øx80Ø70422 The service cannot be started, either because it is disabled or because it has no enabled devices associated With It.". Then Open **CMD Prompt** as ADMINISTRATOR and run the following command: `sc config LxssManager start=auto`. 

After that simply run `wsl -d <Distro>`  in your Terminal and you should be good to go.
 
# 2. References:

1. :LiLink2: https://learn.microsoft.com/en-us/windows/wsl/install-manual
2. :LiLink2: https://www.thewindowsclub.com/wslregisterdistribution-failed-with-error-0x80070422