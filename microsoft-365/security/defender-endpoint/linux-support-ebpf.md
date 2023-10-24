---
title: Use eBPF-based sensor for Microsoft Defender for Endpoint on Linux
description: eBPF-based sensor deployment in Microsoft Defender for Endpoint on Linux.
ms.service: microsoft-365-security
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
ms.subservice: mde
search.appverid: met150
ms.date: 07/19/2023
---

# Use eBPF-based sensor for Microsoft Defender for Endpoint on Linux

[!INCLUDE [Microsoft 365 Defender rebranding](../../includes/microsoft-defender.md)]

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

With eBPF, events previously obtained from the auditd event provider now flow from the eBPF sensor. This helps with system stability and improving CPU and memory utilization and reduces disk usage. Also, when eBPF is enabled, all auditd-related custom rules are eliminated which help reduce the possibility of conflicts between applications.

In addition, the eBPF sensor uses capabilities of the Linux kernel without requiring the use of a kernel module that helps increase system stability.

> [!NOTE]
> eBPF will be used in conjunction with auditd. Auditd will be used to capture user login events only and flow them automatically. Be aware that auditd will be gradually removed in future versions.


## System prerequisites

The eBPF sensor for Microsoft Defender for Endpoint on Linux is supported on the following minimum distribution and kernel versions:

| Linux Distribution | Distribution version | Kernel version |
|--------------------|----------------------|----------------|
| Ubuntu             | 16.04                | 4.15.0         |
| Fedora             | 33                   | 5.8.15         |
| CentOS             | 7.6                  | 3.10.0-957     |
| SLES               | 15                   | 5.3.18-18.47   |
| RHEL               | 7.6                  | 3.10.0-957     |
| Debian             | 9.0                  | 4.19.0         |
| Oracle Linux RHCK  | 7.9                  | 3.10.0-1160    |
| Oracle Linux UEK   | 7.9                  | 5.4            |

## Use eBPF

The eBPF sensor will be automatically enabled for all customers on agent versions “101.23082.0006” and above. When the eBPF sensor is enabled on an endpoint, Defender for Endpoint on Linux updates supplementary_events_subsystem to ebpf.

:::image type="content" source="../../media/defender-endpoint/ebpf-subsystem-linux.png" alt-text="ebpf subsystem highlight in the mdatp health command" lightbox="../../media/defender-endpoint/ebpf-subsystem-linux.png":::

If you want to manually disable eBPF, you can run the following command:

```bash
sudo mdatp config ebpf-supplementary-event-provider --value [enabled/disabled]
```

> [!IMPORTANT]
> If you disable eBPF, the supplementary event provider switches back to auditd. </br>
> In the event eBPF doesn't become enabled or is not supported on any specific kernel, it will automatically switch back to auditd and retain all auditd custom rules.

### Immutable mode of Auditd

After enabling eBPF, customers using auditd in immutable mode must reboot their system in order to clear the audit rules added by Microsoft Defender for Endpoint. This is because immutable mode of auditd freezes the rules file and prevents it from being edited or overwritten. To verify that the audit rules have been cleared, run the following command after the reboot:

```bash
% sudo auditctl -l
```
The output of the command should show no rules or any user added rules. If the rules weren't removed, follow these steps to clear the audit rules file:

  1. Switch to ebpf mode
  2. Remove the file /etc/audit/rules.d/mdatp.rules
  3. Reboot the machine

### Troubleshooting and diagnostics

You can check the agent health status by running the **mdatp** health command. Make sure that the eBPF sensor for Defender for Endpoint on Linux is supported by checking the current kernel version by using the following command line:

```bash
uname -a
```

The following two sets of data help analyze potential issues and determine the most effective resolution options.

1. Collect a diagnostic package from the client analyzer tool by using the following instructions: [Troubleshoot performance issues for Microsoft Defender for Endpoint on Linux](linux-support-perf.md).

2. Collect a debug diagnostic package when Defender for Endpoint is utilizing high resources by using the following instructions: [Microsoft Defender for Endpoint on Linux resources](linux-resources.md#collect-diagnostic-information).

## See also

- [Troubleshoot performance issues for Microsoft Defender for Endpoint on Linux](linux-support-perf.md)
- [Microsoft Defender for Endpoint on Linux resources](linux-resources.md#collect-diagnostic-information)

