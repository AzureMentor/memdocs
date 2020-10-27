---
# required metadata

title: Windows 10 Antivirus policy settings for Windows Security experience for Intune | Microsoft Docs
description: Endpoint security Antivirus policy settings for the Windows Security app in Microsoft Intune 
keywords:
author: brenduns
ms.author: brenduns
manager: dougeby
ms.date: 10/23/2020
ms.topic: reference
ms.service: microsoft-intune
ms.subservice: protect
ms.localizationpriority: medium
ms.technology:

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.reviewer: mattsha

---

# Settings for the Windows Security experience profile in Microsoft Intune

View the Antivirus policy settings you can configure for the **Windows Security Experience** profile for Windows 10 in Microsoft Intune as part of an [Endpoint security policy](../protect/endpoint-security-policy.md).

**Windows Security**

- **Enable tamper protection to prevent Microsoft Defender being disabled**  
  [Prevent changes to security settings with Tamper Protection](https://go.microsoft.com/fwlink/?linkid=2066083)

  - **Not configured** (*default*) - When the *Enable* or *Disable* state exists on a client, deploying *Not configured* has no impact on the setting.
  - **Enable** - Enable the Tamper Protection restriction. To change the state from either enabled or disabled, deploy the opposite setting to have effect.
  - **Disable** - Disable the Tamper Protection restrictions. To change the state from either enabled or disabled, deploy the opposite setting to have effect.

- **Hide the Virus and threat protection area in the Windows Security app**  
  CSP: [DisableVirusUI](https://go.microsoft.com/fwlink/?linkid=873662)

  - **Not configured** (*default*) - The setting returns to the client default, which is to allow user access and notifications.
  - **Yes** - The virus and threat protection area in the Windows Security app is hidden from end-users. Virus and threat protection-related notifications are suppressed.
  - **No** - Behavior is the same as *Not configured*.

  - **Hide the Ransomware data recovery option in the Windows Security app**  
    CSP: [HideRansomwareDataRecovery](https://go.microsoft.com/fwlink/?linkid=873664)

  - **Not configured** (*default*) - The setting returns to the client default, which is to allow user access and notifications.
  - **Yes** - The ransomware data recovery area in the Windows Security app is hidden from end-users. Ransomware related notifications are suppressed.
  - **No** - Behavior is the same as *Not configured*.

- **Hide the Account protection area in the Windows Security app**  
  CSP: [DisableAccountProtectionUI](https://go.microsoft.com/fwlink/?linkid=873666)

  - **Not configured** (*default*) - The setting returns to the client default, which is to allow user access and notifications.
  - **Yes** - The account protection area in the Windows Security app is hidden from end-users. Account protection-related notifications are suppressed.
  - **No** - Behavior is the same as *Not configured*.

- **Hide the Firewall and network protection area in the Windows Security app**  
  CSP: [DisableNetworkUI](https://go.microsoft.com/fwlink/?linkid=873668)

  - **Not configured** (*default*) - The setting returns to the client default, which is to allow user access and notifications.
  - **Yes** - The firewall and network protection area in the Windows Security are hidden from end-users. Firewall and network protection-related notifications are suppressed.
  - **No** - Behavior is the same as *Not configured*.

- **Hide the App and browser control area in the Windows Security app**  
  CSP: [DisableAppBrowserUI](https://go.microsoft.com/fwlink/?linkid=873669)

  - **Not configured** (*default*) - The setting returns to the client default, which is to allow user access and notifications.
  - **Yes** - The app and browser control area in the Windows Security is hidden from end-users. App and browser control related notifications are suppressed.
  - **No** - Behavior is the same as *Not configured*.

- **Hide the Device security area in the Windows Security app**  
  CSP: [DisableDeviceSecurityUI](https://go.microsoft.com/fwlink/?linkid=873670)

  - **Not configured** (*default*) - The setting returns to the client default, which is to allow user access and notifications.
  - **Yes** - The hardware protection area in the Windows Security app is hidden from end-users. Hardware protection-related notifications will be suppressed.
  - **No** - Behavior is the same as *Not configured*.
  
- **Hide the Device performance and health area in the Windows Security app**  
  CSP: [DisableHealthUI](https://go.microsoft.com/fwlink/?linkid=873671)

  - **Not configured** (*default*) - The setting returns to the client default, which is to allow user access and notifications.
  - **Yes** - The device performance and health area in the Windows Security app are hidden from end-users. Device performance and health-related notifications ware suppressed
  - **No** - Behavior is the same as *Not configured*.

- **Hide the Family options area in the Windows Security app**  
  CSP: [DisableFamilyUI](https://go.microsoft.com/fwlink/?linkid=873673)

  - **Not configured** (*default*) - The setting returns to the client default, which is to allow user access and notifications.
  - **Yes** - The family options area in the Windows Security app is hidden from end-users. Also, notifications related to family options are suppressed.
  - **No** - Behavior is the same as *Not configured*.

- **Windows Security app notifications**  
  CSP: [DisableNotifications](https://go.microsoft.com/fwlink/?linkid=873675)

  Use this setting to block Windows Security notifications to your users for all of the preceding feature settings. Alternatively, you can manage the Windows Security app notifications per feature by using the proceeding settings.

  - **Not configured** (*default*) - All Windows Security app notifications that are not controlled by another setting are allowed.
  - **Block non-critical notification** - Notifications such as scan completions are blocked.
  - **Block all notifications** - Critical and non-critical notifications are blocked for all Windows Security features.

- **Hide the Windows Security icon from the notification area**  
  CSP: [HideWindowsSecurityNotificationAreaControl](https://go.microsoft.com/fwlink/?linkid=2114313&clcid=0x409)

  For this setting to take effect, the user needs to either sign out and back in, or reboot the computer.
  - **Not configured** (*default*) - The setting returns the client to the default, which is to show the icon.
  - **Yes** - Hide the Windows Security icon from the users system tray.
  - **No** - Behavior is the same as *Not configured*.
  
- **Disable the Clear TPM option in the Windows Security app**  
  CSP: [DisableClearTpmButton](https://go.microsoft.com/fwlink/?linkid=2114125&clcid=0x409)

  - **Not configured** (*default*) - The setting returns to the client default, which allows access to the button.
  - **Yes** - Disable access to the clear TPM button in the Windows Security app.
  - **No** - Behavior is the same as *Not configured*.

- **Prompt users to update TPM firmware if vulnerability is discovered**  
  CSP: [DisableTpmFirmwareUpdateWarning](https://go.microsoft.com/fwlink/?linkid=2114212&clcid=0x409)

  - **Not configured** (*default*) - The setting returns to the client default, which is to not prompt users.
  - **Yes** - Allow Windows to prompt end-users when a potential vulnerability is found in their TPM firmware. They are then encouraged to run firmware updates to resolve the vulnerability.
  - **No** - Behavior is the same as *Not configured*.

- **Organization's support contact information**  
  CSP: [EnableCustomizedToasts](https://go.microsoft.com/fwlink/?linkid=873676)

  Declare where you would like your IT organization information displayed in the Windows Security app and notifications.
  - **Not configured** (*default*)
  - **Display in app and in notifications**
  - **Display only in app**
  - **Display only in notifications**
