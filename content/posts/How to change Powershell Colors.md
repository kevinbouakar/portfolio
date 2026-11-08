---
title: "How to change Powershell Colors"
date: 2026-03-28T22:34:18+02:00
lastmod: 2026-03-28T22:34:18+02:00
draft: false
tags: []
slug: "how-to-change-powershell-colors"
---

# 1. How To:

1. Open Powershell
2. Run the following command:

```powershell
Get-PSReadLineOption
```

3.  To change colors simply specify the part you would like to change.

```powershell
Set-PSReadLineOption -Colors @{ Command = 'Yellow'}
```


4. Here is the color scheme that I am using currently:

```powershell
    Set-PSReadLineOption -Colors @{
      Command            = 'Yellow'
      Number             = 'DarkGray'
      Member             = 'DarkGray'
      Operator           = 'Blue'
      Comment            = 'DarkGreen'
      Type               = 'DarkGray'
      Variable           = 'DarkGreen'
      Parameter          = 'Red'
      ContinuationPrompt = 'DarkGray'
      Default            = 'White'
    }
```

# 2. Reference:

:FiLink2:: https://4sysops.com/archives/change-powershell-console-syntax-highlighting-colors-of-psreadline/
