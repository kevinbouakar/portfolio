---
title: "Windows Partition Formatting"
date: 2025-09-17T08:31:44+03:00
lastmod: 2025-09-17T08:31:44+03:00
draft: false
tags: []
slug: "windows-partition-formatting"
---

1. In the Prompt type DISKPART, when it opens, DISKPART will appear on the left.  
2. Type LIST DISK, this command will list the hard drives installed on the machine, pay close attention not to choose the wrong hard drive.  
3. Type SELECT DISK “X”, in place of the X put the number for the HD you want to format, put without quotes.  
4. Now just type the commands below and press Enter at each command.  
5. Commands:  
```cmd
CLEAN  
CREATE PARTITION PRIMARY  
SELECT PARTITION 1  
ACTIVE  
FORMAT FS=NTFS QUICK  
ASSIGN  
EXIT
```

