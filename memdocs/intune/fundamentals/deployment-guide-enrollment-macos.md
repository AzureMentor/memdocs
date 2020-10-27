---
# required metadata

title: macOS device enrollment guide for  Microsoft Intune - Azure | Microsoft Docs
description: Enroll macOS devices using device enrollment, automated device enrollment (DEP), and Apple Configurator enrollment options in Microsoft Intune. Decide which enrollment method to use, and get an overview of the administrator and end user tasks to enroll devices.
keywords:
author: MandiOhlinger
ms.author: mandia
manager: dougeby
ms.date: 10/26/2020
ms.topic: conceptual
ms.service: microsoft-intune
ms.subservice: enrollment
ms.localizationpriority: high
ms.technology:

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: auherrin, dregan, annovich
ms.suite: ems
search.appverid: MET150
#ms.tgt_pltfrm:
ms.custom:
ms.collection: M365-identity-device-management
---

# Deployment guide: Enroll macOS devices in Microsoft Intune

Personal and organization-owned devices can be enrolled in Intune. On macOS devices, the Company Portal app or the Apple Setup Assistant authenticates users, and starts the enrollment. Once they're enrolled, they receive the policies and profiles you create.

You have the following options when enrolling macOS devices:

- [BYOD: Device enrollment](#byod-device-enrollment)
- [Automated device enrollment (ADE)](#automated-device-enrollment-ade-supervised)

> [!NOTE]
> Apple Configurator is available for iOS/iPadOS devices. It's not available for macOS devices.

This article:

- Describes your Company Portal app options for each enrollment method.
- Provides recommendations on the macOS enrollment method to use.
- Includes an overview of the administrator and user tasks for each enrollment type.

For more specific information, see [Enroll macOS devices](../enrollment/macos-enroll.md).

> [!TIP]
> [!INCLUDE [tips-guidance-plan-deploy-guides](../includes/tips-guidance-plan-deploy-guides.md)]

## Before you begin

For an overview, including any Intune-specific prerequisites, see [Deployment guidance: Enroll devices in Microsoft Intune](deployment-guide-enrollment.md).

## BYOD: Device enrollment

Use for personal or bring your own devices (BYOD). Not a traditional "enrollment" method, as it uses an app configuration profile. This option manages apps on the device. Devices aren't enrolled.

---
| Feature | Use this enrollment option when |
| --- | --- |
| Devices are personal or BYOD. | ✔️ |
| Need to enroll a small number of devices, or a large number of devices (bulk enrollment). | ✔️ |
| You have new or existing devices. | ✔️ |
| Devices are associated with a single user. | ✔️ |
| You use the device enrollment manager (DEM) account. | ✔️ <br/><br/> Be aware of impact and any limitations using DEM account. |
| Devices are managed by another MDM provider. | ❌ <br/><br/> When a device enrolls, MDM providers install certificates and other files. These files must be removed. The quickest way may be to unenroll, or factory reset the devices. If you don't want to factory reset, then contact the MDM provider. |
| Devices are owned by the organization or school. | ❌ <br/><br/> Not recommended for organization-owned devices. Organization-owned devices should be enrolled using Automated Device Enrollment or Apple Configurator. <br/><br/> You can add the MacBook serial numbers to the corporate device identifiers to mark the devices as corporate. But, by default, devices are marked personal. |
| Devices are user-less, such as kiosk, dedicated, or shared. | ❌ <br/><br/> These devices are organization-owned. User-less devices should be enrolled using Automated Device Enrollment or Apple Configurator. |

---

### Device enrollment administrator tasks

This task list provides an overview.

- Be sure your devices are [supported](supported-devices-browsers.md).
- Be sure the [Apple MDM push certificate](../enrollment/apple-mdm-push-certificate-get.md) is added to Endpoint Manager, and is active. This certificate is required to enroll macOS devices. For more information, see [Get an Apple MDM push certificate](../enrollment/apple-mdm-push-certificate-get.md).
- There isn't a Company Portal app for macOS devices in the Apple App Store, or through VPP. Users must [manually download and run the Company Portal app installer package](https://go.microsoft.com/fwlink/?linkid=853070). They sign in with their organization account (`user@contoso.com`), and then step through the enrollment. Once they enroll, they must approve the enrollment profile.

  When they approve, the device is added to your organization Azure AD. Then, it's available to Intune to receive your policies and profiles.

  Be sure to communicate this information with your users.

### Device enrollment end user tasks

Your users must do the following steps. For more specific information on the end user steps, see [Enroll your macOS device using the Company Portal app](../user-help/enroll-your-device-in-intune-macos-cp.md).

1. Download and run [the Company Portal app installer package](https://go.microsoft.com/fwlink/?linkid=853070).
2. Open the Company Portal app, and sign in with their organization account (`user@contoso.com`). Once they sign in, they must approve the enrollment profile (System preferences). When users approve, the device is enrolled, and considered managed. If they don't approve, then they're not enrolled, and won't receive your policy and profiles.

For more specific information on the end user steps, see [Enroll your macOS device using the Company Portal app](../user-help/enroll-your-device-in-intune-macos-cp.md).

[!INCLUDE [users-dont-like-enroll](../includes/users-dont-like-enroll.md)]

## Automated Device Enrollment (ADE) (supervised)

Previously called Apple Device Enrollment Program (DEP). Use on devices owned by your organization. This option configures settings using Apple Business Manager (ABM) or Apple School Manager (ASM). It enrolls a large number of devices, without you ever touching the devices. These devices are purchased from Apple, have your preconfigured settings, and can be shipped directly to users or schools. You create an enrollment profile in the [Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431), and push this profile to the devices.

For more specific information on this enrollment type, see [Automatically enroll macOS devices with the Apple Business Manager or Apple School Manager](../enrollment/device-enrollment-program-enroll-macos.md).

---
| Feature | Use this enrollment option when |
| --- | --- |
| Devices are owned by the organization or school. | ✔️ |
| You have new devices. | ✔️ |
| Need to enroll a small number of devices, or a large number of devices (bulk enrollment). | ✔️ |
| Devices are associated with a single user. | ✔️ |
| Devices are user-less, such as kiosk or dedicated device. | ✔️ |
| Devices are personal or BYOD. | ❌ <br/><br/> Not recommended. BYOD or personal devices should be enrolled using Device enrollment. |
| You have existing devices. | ❌ <br/><br/>Existing devices should be enrolled using Apple Configurator. |
| Devices are managed by another MDM provider. | ❌ <br/><br/> To be fully managed by Intune, users must unenroll from the current MDM provider, and then enroll in Intune. Or, you can use Device enrollment to manage specifics apps on the device. Since these devices are organization-owned, it's recommended to enroll in Intune. |
| You use the device enrollment manager (DEM) account. | ❌ <br/><br/> The DEM account isn't supported. |

---

### ADE administrator tasks

This task list provides an overview. For more specific information, see [Automatically enroll macOS devices with the Apple Business Manager or Apple School Manager](../enrollment/device-enrollment-program-enroll-macos.md).

- Be sure your devices are [supported](supported-devices-browsers.md).
- Need access to the [Apple Business Manager (ABM) portal](https://business.apple.com/), or the [Apple School Manager (ASM) portal](https://school.apple.com/).
- Be sure the Apple token (.p7m) is active. For more specific information, see [Get an Apple ADE token](../enrollment/device-enrollment-program-enroll-macos.md#get-an-apple-ade-token).
- Be sure the [Apple MDM push certificate](../enrollment/apple-mdm-push-certificate-get.md) is added to Endpoint Manager, and is active. This certificate is required to enroll macOS devices. For more information, see [Get an Apple MDM push certificate](../enrollment/apple-mdm-push-certificate-get.md).
- Decide how users will authenticate on their devices: the **Company Portal** app, or **Setup Assistant**. Make this decision before you create the enrollment profile. Using the Company Portal app is considered modern authentication. We recommended using the Company Portal app.

  For all organization-owned macOS devices, **Setup Assistant** is always and automatically used, even if you don't see "Setup Assistant" text in Endpoint Manager. Setup Assistant authenticates the user, and enrolls the device. 

  If you want to continue to use **Setup Assistant** for authentication, then you don't need the Company Portal app. If you want to use the Company Portal app for authentication, instead of using Setup Assistant, then you can. **After** the device is enrolled, you can install the Company Portal app.

  To install the Company Portal app on devices, see [add the Company Portal app](../apps/apps-company-portal-macos.md). Set the Company Portal app as a required app.

  Once installed, users open the Company Portal app, and sign in with their organization account (`user@contoso.com`). When they sign-in, they're authenticated, and ready to receive your policies and profiles.

- In the [Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431), create an enrollment profile. Choose to **Enroll with user affinity** (associate a user to the device), or **Enroll without user affinity** (user-less devices or shared devices).

  - **Enroll with user affinity**: Setup Assistant authenticates the user, and enrolls the device in Intune. Also choose if users can delete the management profile, called **Locked enrollment**.

  - **Enroll without user affinity**: Setup Assistant authenticates the user, and enrolls the user in Intune. Also choose if users can delete the management profile, called **Locked enrollment**. The Company Portal app isn't used, needed, or supported on enrollments without user affinity.

### ADE end user tasks

These tasks depend on how administrators tell users to install the Company Portal app. Typically, the less end users must do to enroll, the higher chance they'll want to enroll.

For more specific information on the end user steps, see [Enroll your macOS device using the Company Portal app](../user-help/enroll-your-device-in-intune-macos-cp.md).

- **Enroll with user affinity**:

  1. When the device is turned on, the Apple Setup Assistant runs. Users enter their Apple ID (`user@iCloud.com` or `user@gmail.com`).
  2. The Setup Assistant prompts the user for information, and enrolls the device in Intune. The device isn't registered in Azure AD.

      If you're using Setup Assistant for authentication, then stop here.

  3. Optional. If you're using the Company Portal app for authentication (instead of Setup Assistant), then the Company Portal app [installs using the option](../apps/apps-company-portal-macos.md) you configured.

      Users open the Company Portal app, and sign in with their organization credentials (`user@contoso.com`). After they sign in, users are authenticated, and can access organization resources.

      Remember, installing the Company Portal app is optional. If you want your users to authenticate using Company Portal app, instead of using the Setup Assistant, then [add the Company Portal app](../apps/apps-company-portal-macos.md).

- **Enroll without user affinity**: No actions. Be sure your users don't install the Company Portal app.

[!INCLUDE [users-dont-like-enroll](../includes/users-dont-like-enroll.md)]

## Next steps

- [MAM-WE](deployment-guide-enrollment-mamwe.md)
- [Android enrollment guide](deployment-guide-enrollment-android.md)
- [iOS/iPadOS enrollment guide](deployment-guide-enrollment-ios-ipados.md)
- [Windows enrollment guide](deployment-guide-enrollment-windows.md)
