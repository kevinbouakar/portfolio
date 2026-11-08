---
title: "Some Useful Registry Tweaks"
date: 2026-09-22T23:15:34+03:00
lastmod: 2026-09-22T23:15:34+03:00
draft: false
tags: []
slug: "some-useful-registry-tweaks"
---

# Windows Registry Tweaks 
> [!warning] Before you start
> - Create a restore point first (tweak 3.1 makes this possible anytime).
> - Run everything in an **admin** cmd window, then sign out or restart.
> - Undo a single value: `reg delete "<key>" /v <name> /f`

> [!tip] Apply Explorer changes without signing out
> `taskkill /f /im explorer.exe & start explorer.exe`

---

## Part 1: Video Tweaks
Source: https://www.youtube.com/watch?v=V7AuHBZsOj0

### 1.1 Context Menu Delay
Change `MenuShowDelay` from 400 to 0
```cmd
reg add "HKCU\Control Panel\Desktop" /v MenuShowDelay /t REG_SZ /d 0 /f
```

### 1.2 Minimum Drag Distance
Change `DragHeight` and `DragWidth` from 4 to 20
```cmd
reg add "HKCU\Control Panel\Desktop" /v DragHeight /t REG_SZ /d 20 /f
reg add "HKCU\Control Panel\Desktop" /v DragWidth /t REG_SZ /d 20 /f
```

### 1.3 Dragging Maximized Windows
Change `DragFromMaximize` from 1 to 0
```cmd
reg add "HKCU\Control Panel\Desktop" /v DragFromMaximize /t REG_SZ /d 0 /f
```

### 1.4 Verbose Startup / Shutdown Messages
Change `VerboseStatus` from 0 to 1
```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v VerboseStatus /t REG_DWORD /d 1 /f
```

### 1.5 Disable Web Results in Start Menu Search
Change `DisableSearchBoxSuggestions` from 0 to 1
```cmd
reg add "HKCU\Software\Policies\Microsoft\Windows\Explorer" /v DisableSearchBoxSuggestions /t REG_DWORD /d 1 /f
```

### 1.6 Remove Startup Delays
Change `StartupDelayInMSec` and `WaitForIdleState` to 0
```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Serialize" /v StartupDelayInMSec /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Serialize" /v WaitForIdleState /t REG_DWORD /d 0 /f
```

### 1.7 Taskbar Clock Seconds
Change `ShowSecondsInSystemClock` from 0 to 1
```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowSecondsInSystemClock /t REG_DWORD /d 1 /f
```

---

## Part 2: Popular Extra Tweaks

### 2.1 Show File Extensions
Change `HideFileExt` from 1 to 0 (spot fake files like `invoice.pdf.exe`)
```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v HideFileExt /t REG_DWORD /d 0 /f
```

### 2.2 Show Hidden Files
Change `Hidden` from 2 to 1
```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v Hidden /t REG_DWORD /d 1 /f
```

### 2.3 Classic Right-Click Menu
Create an empty `InprocServer32` key (also in CTT as "Right-Click Menu Previous Layout")
```cmd
reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
```
Undo:
```cmd
reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f
```

### 2.4 Open File Explorer to "This PC"
Change `LaunchTo` from 2 to 1 (also in CTT's Home & Gallery tweak)
```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v LaunchTo /t REG_DWORD /d 1 /f
```

### 2.5 "End Task" in Taskbar Right-Click
Change `TaskbarEndTask` from 0 to 1 (also in CTT, Windows 11 23H2+)
```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\TaskbarDeveloperSettings" /v TaskbarEndTask /t REG_DWORD /d 1 /f
```

### 2.6 Disable Sticky Keys Popup
Change `Flags` from 510 to 506
```cmd
reg add "HKCU\Control Panel\Accessibility\StickyKeys" /v Flags /t REG_SZ /d 506 /f
```

### 2.7 Disable Mouse Acceleration
Change `MouseSpeed`, `MouseThreshold1` and `MouseThreshold2` to 0
```cmd
reg add "HKCU\Control Panel\Mouse" /v MouseSpeed /t REG_SZ /d 0 /f
reg add "HKCU\Control Panel\Mouse" /v MouseThreshold1 /t REG_SZ /d 0 /f
reg add "HKCU\Control Panel\Mouse" /v MouseThreshold2 /t REG_SZ /d 0 /f
```

### 2.8 Disable Lock Screen
Change `NoLockScreen` from 0 to 1 (Pro/Enterprise)
```cmd
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\Personalization" /v NoLockScreen /t REG_DWORD /d 1 /f
```

---

## Part 3: CTT (WinUtil) Tweaks
Extracted from my `ctt.json` selection, using WinUtil's tweak definitions.

### 3.1 Allow Restore Points Anytime
Change `SystemRestorePointCreationFrequency` from 1440 to 0 (no 24h limit)
```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SystemRestore" /v SystemRestorePointCreationFrequency /t REG_DWORD /d 0 /f
```

### 3.2 Block Razer / OEM Software Auto-Install
Change `SearchOrderConfig` from 1 to 0 and `DisableCoInstallers` from 0 to 1
> [!note] This also stops Windows Update from auto-downloading device drivers.

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\DriverSearching" /v SearchOrderConfig /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Device Installer" /v DisableCoInstallers /t REG_DWORD /d 1 /f
```

### 3.3 Disable Activity History
Set `EnableActivityFeed`, `PublishUserActivities` and `UploadUserActivities` to 0
```cmd
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\System" /v EnableActivityFeed /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\System" /v PublishUserActivities /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\System" /v UploadUserActivities /t REG_DWORD /d 0 /f
```

### 3.4 Disable Storage Sense
Change `01` from 1 to 0
```cmd
reg add "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy" /v 01 /t REG_DWORD /d 0 /f
```

### 3.5 Hide Home and Gallery in File Explorer
Set `System.IsPinnedToNameSpaceTree` to 0 for both
```cmd
reg add "HKCU\Software\Classes\CLSID\{f874310e-b6b7-47dc-bc84-b9e6b38f5903}" /v System.IsPinnedToNameSpaceTree /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Classes\CLSID\{e88865ea-0e1c-4e20-9aa6-edcd0212c87c}" /v System.IsPinnedToNameSpaceTree /t REG_DWORD /d 0 /f
```

### 3.6 Disable GameDVR
Turn off background game recording
```cmd
reg add "HKCU\System\GameConfigStore" /v GameDVR_Enabled /t REG_DWORD /d 0 /f
reg add "HKCU\System\GameConfigStore" /v GameDVR_FSEBehavior /t REG_DWORD /d 2 /f
reg add "HKCU\System\GameConfigStore" /v GameDVR_HonorUserFSEBehaviorMode /t REG_DWORD /d 1 /f
reg add "HKCU\System\GameConfigStore" /v GameDVR_EFSEFeatureFlags /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\GameDVR" /v AllowGameDVR /t REG_DWORD /d 0 /f
```

### 3.7 Disable Telemetry
```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\AdvertisingInfo" /v Enabled /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Privacy" /v TailoredExperiencesWithDiagnosticDataEnabled /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Speech_OneCore\Settings\OnlineSpeechPrivacy" /v HasAccepted /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Input\TIPC" /v Enabled /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\InputPersonalization" /v RestrictImplicitInkCollection /t REG_DWORD /d 1 /f
reg add "HKCU\Software\Microsoft\InputPersonalization" /v RestrictImplicitTextCollection /t REG_DWORD /d 1 /f
reg add "HKCU\Software\Microsoft\InputPersonalization\TrainedDataStore" /v HarvestContacts /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Personalization\Settings" /v AcceptedPrivacyPolicy /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" /v AllowTelemetry /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v Start_TrackProgs /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\System" /v PublishUserActivities /t REG_DWORD /d 0 /f
reg add "HKCU\Software\Microsoft\Siuf\Rules" /v NumberOfSIUFInPeriod /t REG_DWORD /d 0 /f
setx POWERSHELL_TELEMETRY_OPTOUT 1 /M
```

### 3.8 Disable Location Tracking
```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\location" /v Value /t REG_SZ /d Deny /f
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Sensor\Overrides\{BFA794E4-F964-4FDB-90F6-51056BFE4B44}" /v SensorPermissionState /t REG_DWORD /d 0 /f
reg add "HKLM\SYSTEM\Maps" /v AutoUpdateEnabled /t REG_DWORD /d 0 /f
```

### 3.9 Disable Recall
Change `DisableAIDataAnalysis` to 1 and `AllowRecallEnablement` to 0
```cmd
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsAI" /v DisableAIDataAnalysis /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsAI" /v AllowRecallEnablement /t REG_DWORD /d 0 /f
```

### 3.10 Set Hardware Clock to UTC (Dual-Boot Fix)
Change `RealTimeIsUniversal` from 0 to 1 (stops the clock jumping after booting Linux)
```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /t REG_QWORD /d 1 /f
```

### 3.11 Debloat Microsoft Edge
```cmd
reg add "HKLM\SOFTWARE\Policies\Microsoft\EdgeUpdate" /v CreateDesktopShortcutDefault /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v PersonalizationReportingEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge\ExtensionInstallBlocklist" /v 1 /t REG_SZ /d ofefcgjbeghpigppfmkologfjadafddi /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v ShowRecommendationsEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v HideFirstRunExperience /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v UserFeedbackAllowed /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v ConfigureDoNotTrack /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v AlternateErrorPagesEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v EdgeCollectionsEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v EdgeShoppingAssistantEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v MicrosoftEdgeInsiderPromotionEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v ShowMicrosoftRewards /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v WebWidgetAllowed /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v DiagnosticData /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v EdgeAssetDeliveryServiceEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v WalletDonationEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Edge" /v DefaultBrowserSettingsCampaignEnabled /t REG_DWORD /d 0 /f
```

### 3.12 Debloat Brave
```cmd
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v BraveRewardsDisabled /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v BraveWalletDisabled /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v BraveVPNDisabled /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v BraveAIChatEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v BraveStatsPingEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v BraveNewsDisabled /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v BraveTalkDisabled /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v TorDisabled /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v BraveP3AEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v UrlKeyedAnonymizedDataCollectionEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v SafeBrowsingExtendedReportingEnabled /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\BraveSoftware\Brave" /v MetricsReportingEnabled /t REG_DWORD /d 0 /f
```

---

> [!info] Not included from the CTT file
> - **Script-only tweaks** (no registry values): Adobe debloat, Adobe URL block, Copilot removal, delete temp files, services to manual, PowerShell 7 install, Explorer folder auto-discovery.
> - **Disable Edge** (`DisallowRun msedge.exe`): skipped because it can break apps that rely on Edge.
> - **Smart App Control off** (`VerifiedAndReputablePolicyState` = 0): CTT bundles this with Disable Recall, but it lowers security and can't be turned back on without reinstalling Windows.