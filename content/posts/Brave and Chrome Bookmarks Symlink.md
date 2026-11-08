---
title: "Brave and Chrome Bookmarks Symlink"
date: 2026-04-01T21:57:02+03:00
lastmod: 2026-04-01T21:57:02+03:00
draft: false
tags: []
slug: "brave-and-chrome-bookmarks-symlink"
---

I use browsers for many different reasons. One for privacy and performance like Brave. The other for data synchroniztion like Google Chrome through my Google Account.

My bookmarks got messy. Having to adjust some on Brave and deleting others on Chrome was inefficient and difficult to maintain. 

So I came up with a simple 3 liner that creates a symlink between both Brave and Google Chrome bookmarks.

How? you may ask... Well its due to the nature of how bookmarks are stored in browsers. Using a simple JSON file. 

By doing a symlink, both Brave and Chrome read and write to the same file in real-time.

```cmd
cd "$env:LOCALAPPDATA\BraveSoftware\Brave-Browser\User Data\Default"
del Bookmarks
cmd /c mklink Bookmarks "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\Bookmarks"
```

> :LiInfo: The script navigates to Brave's profile directory, deletes its existing `Bookmarks` file, and creates a symbolic link in its place that points to Chrome's `Bookmarks` file — making both browsers share the same bookmarks in real-time.


