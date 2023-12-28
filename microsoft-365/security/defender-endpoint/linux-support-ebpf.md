---
title: Use eBPF-based sensor for Microsoft Defender for Endpoint on Linux
description: eBPF-based sensor deployment in Microsoft Defender for Endpoint on Linux.
ms.service: defender-endpoint
ms.author: dansimp
author: dansimp
ms.localizationpriority: medium
manager: dansimp
audience: ITPro
ms.collection:
- m365-security
- tier3
- mde-linux
ms.topic: conceptual
ms.subservice: linux
search.appverid: met150
ms.date: 12/11/2023
---

# Use eBPF-based sensor for Microsoft Defender for Endpoint on Linux

[!INCLUDE [Microsoft Defender XDR rebranding](../../includes/microsoft-defender.md)]

**Applies to:**

- [Microsoft Defender for Endpoint on Linux](microsoft-defender-endpoint-linux.md)
- [Microsoft Defender for Endpoint Plan 1](https://go.microsoft.com/fwlink/p/?linkid=2154037)
- [Microsoft Defender for Endpoint Plan 2](https://go.microsoft.com/fwlink/p/?linkid=2154037)

The extended Berkeley Packet Filter (eBPF) for Microsoft Defender for Endpoint on Linux provides supplementary event data for Linux operating systems. eBPF can be used as an alternative technology to auditd because eBPF helps address several classes of issues seen with the auditd event provider and is beneficial in the areas of performance and system stability.

Key benefits include:

- Reduced system-wide auditd-related log noise
- Optimized system-wide event rules otherwise causing conflict between applications
- Reduced overhead for file event (file read/open) monitoring
- Improved event rate throughput and reduced memory footprint
- Optimized performance for specific configurations

## How eBPF works

With eBPF, events previously obtained from the auditd event provider now flow from the eBPF sensor. This helps with system stability, improves CPU and memory utilization, and reduces disk usage. Also, when eBPF is enabled, all auditd-related custom rules are eliminated, which helps reduce the possibility of conflicts between applications.

In addition, the eBPF sensor uses capabilities of the Linux kernel without requiring the use of a kernel module that helps increase system stability.

> [!NOTE]
> eBPF is used in conjunction with auditd, whereas auditd is used only for user login events and captures these events without any custom rules and flow them automatically. Be aware that auditd will be gradually removed in future versions.

## System prerequisites

The eBPF sensor for Microsoft Defender for Endpoint on Linux is supported on the following minimum distribution and kernel versions:

| Linux Distribution | Distribution version | Kernel version |
|--------------------|----------------------|----------------|
| Ubuntu             | 16.04                | 4.15.0         |
| Fedora             | 33                   | 5.8.15         |
| CentOS             | 7.6                  | 3.10.0-957.12  |
| SLES               | 15                   | 5.3.18-18.47   |
| RHEL               | 7.6                  | 3.10.0-957.12  |
| Debian             | 9.0                  | 4.19.0         |
| Oracle Linux RHCK  | 7.9                  | 3.10.0-1160    |
| Oracle Linux UEK   | 7.9                  | 5.4            |

> [!NOTE]
> Oracle Linux 8.8 with kernel version 5.15.0-0.30.20.el8uek.x86_64, 5.15.0-0.30.20.1.el8uek.x86_64 will result in kernel hang when eBPF is enabled as supplementary subsystem    provider. This kernel version should not be used for eBPF mode. Refer to Troubleshooting and Diagnostics section for mitigation steps.

## Use eBPF

The eBPF sensor will be automatically enabled for all customers by default for agent versions “101.23082.0006” and above. Customers need to update to the above-mentioned supported versions to experience the feature. When the eBPF sensor is enabled on an endpoint, Defender for Endpoint on Linux updates supplementary_events_subsystem to ebpf.

:::image type="content" source="../../media/defender-endpoint/ebpf-subsystem-linux.png" alt-text="ebpf subsystem highlight in the mdatp health command" lightbox="../../media/defender-endpoint/ebpf-subsystem-linux.png":::

In case you want to manually disable eBPF then you can run the following command:

```bash
sudo mdatp config ebpf-supplementary-event-provider --value [enabled/disabled]
```
> [!IMPORTANT]
> If you disable eBPF, the supplementary event provider switches back to auditd.
> In the event eBPF doesn't become enabled or is not supported on any specific kernel, it will automatically switch back to auditd and retain all auditd custom rules. 

## Immutable mode of Auditd
For customers using auditd in immutable mode, a reboot is required post enablement of eBPF in order to clear the audit rules added by Microsoft Defender for Endpoint. This is a limitation in immutable mode of auditd which freezes the rules file and prohibits editing/overwriting. This is resolved with the reboot.
Post reboot, run the below command to check if audit rules got cleared.
```bash
% sudo auditctl -l
```
The output of above command should show no rules or any user added rules. In case the rules did not get removed, then perform the following steps to clear the audit rules file
  1. Switch to ebpf mode
  2. Remove the file /etc/audit/rules.d/mdatp.rules
  3. Reboot the machine

### Troubleshooting and Diagnostics

You can check the agent health status by running the **mdatp** health command. Make sure that the eBPF sensor for Defender for Endpoint on Linux is supported by checking the current kernel version by using the following command line:

```bash
uname -a
```

Using Oracle Linux 8.8 with kernel version **5.15.0-0.30.20.el8uek.x86_64, 5.15.0-0.30.20.1.el8uek.x86_64** might result into kernel hang issues. Following steps can be taken to mitigate this issue:    
 
1. Use a kernel version higher or lower than **5.15.0-0.30.20.el8uek.x86_64, 5.15.0-0.30.20.1.el8uek.x86_64** on Oracle Linux 8.8,  if you want to use eBPF as supplementary subsystem provider. Please note, min kernel version for Oracle Linux is RHCK 3.10.0 and Oracle Linux UEK is 5.4. 
2. Switch to auditd mode if customer needs to use the same kernel version
```bash
sudo mdatp config  ebpf-supplementary-event-provider  --value disabled
```
The following two sets of data help analyze potential issues and determine the most effective resolution options.

1. Collect a diagnostic package from the client analyzer tool by using the following instructions: [Troubleshoot performance issues for Microsoft Defender for Endpoint on Linux](linux-support-perf.md).

2. Collect a debug diagnostic package when Defender for Endpoint is utilizing high resources by using the following instructions: [Microsoft Defender for Endpoint on Linux resources](linux-resources.md#collect-diagnostic-information).


## See also

- [Troubleshoot performance issues for Microsoft Defender for Endpoint on Linux](linux-support-perf.md)
- [Microsoft Defender for Endpoint on Linux resources](linux-resources.md#collect-diagnostic-information)

