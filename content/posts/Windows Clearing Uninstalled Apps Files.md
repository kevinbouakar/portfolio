---
title: "Windows Clearing Uninstalled Apps Files"
date: 2024-09-27T13:35:36+03:00
lastmod: 2024-09-27T13:35:36+03:00
draft: false
tags: []
slug: "windows-clearing-uninstalled-apps-files"
---

1. Press `Win + R`
2. Type in "regedit"
3. Press the Path bar & paste the following path

```
Computer\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall
```

4. The Folder with random letters in it is junk files but always double check the DisplayName. 

# 2nd Method:

1. run ```winget list | findstr <app_name>```
2. then do `winget uninstall "app_name"`


