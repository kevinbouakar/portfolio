---
title: "Compiling Code"
date: 2026-03-28T21:41:58+02:00
lastmod: 2026-03-28T21:41:58+02:00
draft: false
tags: []
slug: "compiling-code"
---

# 1. Java:

In order to compile Java files, you must meet the following conditions.

1. Make sure you are inside the folder behind your files aka one directory behind your Java files.
```powershell
..\Java\
├───\Basics
│       methods.class
│       methods.java
│       practice.class
│       practice.java
│
├───\Lab2
│       lab2.class
│       lab2.java
│       SavingsAccount.class
│       SavingsAccount.java
│
└───\OOP
```

Here i would like to compile both `lab2.java` & `SavingsAccount.java`

To do so, run the following:

```java
javac Lab2\*.java && java Lab2\lab2.java
javac Lab2\*.java && java Lab2\SavingsAccount.java
```

# 2. C Language:

 To compile and run C code, you must first have `gcc` installed. 
 Check gcc version by running `gcc --version`

Then, run the following cmd for supposed `example.c` file.

```c
gcc example.c -o example.exe && .\example.exe
```

