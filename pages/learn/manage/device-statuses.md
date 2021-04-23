---
title: Device statuses
excerpt: Determine the status and connectivity of a device
---

# Device statuses

Device statuses are displayed on the devices page, and the device summary page.  An overview of the devices statuses of an application is shown on the applications page.  Each device can have one of the following statuses:

| Status                     | Description                                                                                                                                                     |
|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Online**                 | The device is online, connected to the VPN, and has recent API communications.                                                                                  |
| **Online (VPN&#160;Only)**<sup>1</sup>      | The device is online, is connected to the VPN, but has **no recent API communications**.                                                       |
| **Online (Heartbeat&#160;Only)**<sup>2</sup>| The device is online, has recent API communications, but is **not connected to the VPN**.                                                      |
| **Offline**                | The device is offline.                                                                                                                                          |
| **Configuring**            | The device is applying OS configuration.                                                                                                                        |
| **Updating**               | The device is updating to a new application release.                                                                                                            |
| **Post Provisioning**      | The device has been [provisioned][device-provisioning] but has not yet come online.                                                                             |
| **Inactive**               | The device has been [deactivated][deactivated] or has been [preregistered][preregistered] but has not yet connected to the {{ $names.cloud.lower }} API.        |
| **Frozen**                 | The device has been frozen because it's outside the organization's allowance, or is in a paid [application type][application type] on a free tier organization. |

Some statuses may indicate partial connectivity issues:

<sup>1</sup> `Online (VPN Only)` indicates that the device is unable to communicate with the {{ $names.cloud.lower }} API.

<sup>2</sup> `Online (Heartbeat Only)` indicates that either: i) device is unable to connect to the {{ $names.cloud.lower }} VPN (e.g. a firewall is blocking VPN traffic), or ii) the device has very recently been powered off or lost network connectivity,
in which case it will remain in the `Online (Heartbeat Only)` state for an extended time based on the device [API polling interval][poll-interval].

## Examples

![Device statuses](/img/common/main_dashboard/device_status.png)

<img src="/img/common/main_dashboard/application_device_status.png" alt="Application device statuses overview" width="40%" >

[deactivated]: /learn/manage/billing/#inactive-devices
[host-os-updates]: /reference/OS/updates/self-service/
[poll-interval]: /learn/manage/configuration/#variable-list
[device-provisioning]: /learn/welcome/primer/#device-provisioning
[preregistered]: /learn/more/masterclasses/advanced-cli/#52-preregistering-a-device
[application type]: /learn/manage/app-types
