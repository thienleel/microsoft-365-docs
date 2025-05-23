---
title: Set up Microsoft 365 Copilot pay-as-you-go for IT admins
description: Enterprise and company IT administrators can use the Microsoft 365 admin center to set up the Microsoft 365 Copilot pay-as-you-go feature. Get step-by-step instructions on setting up a billing policy, connecting the billing policy to the pay-as-you-go Copilot service, and removing pay-as-you-go.
ms.author: camillepack
author: camillepack
manager: scotv
ms.date: 04/22/2025
ms.reviewer: nishanair
audience: Admin
ms.topic: get-started
ms.service: microsoft-365-copilot
ms.localizationpriority: medium
ms.collection: 
- scotvorg
- m365copilot
- magic-ai-copilot
- essentials-overview
ms.custom: [copilot-learning-hub]
appliesto:
  - ✅ Microsoft 365 Copilot
---

# Set up pay-as-you-go for Microsoft 365 Copilot services for IT admins

The Microsoft 365 Copilot pay-as-you-go service offers a consumption-based option for organizations to access Copilot services, like Microsoft 365 Copilot Chat. To learn more about the pay-as-you-go service, see [Microsoft 365 Copilot pay-as-you-go overview](overview.md).

You can set up the pay-as-you-go plan directly in the Microsoft 365 admin center. This article provides step-by-step instructions on how to set up and manage pay-as-you-go billing.

This article applies to:

- Microsoft 365 Copilot Chat

## Prerequisites

To set up pay-as-you-go, you must have the following prerequisites:

- Azure subscription and resource group:
  - You must have an owner or contributor Azure role to an Azure subscription to set up the pay-as-you-go service.
  - You must have an owner or contributor Azure role to an Azure resource group linked to the same Azure subscription to set up the pay-as-you-go service.

  To learn more, see [Use the Azure portal and Azure Resource Manager to Manage Resource Groups](/azure/azure-resource-manager/management/manage-resource-groups-portal).

- One of the following Microsoft 365 administrator roles:
  - Global administrator
  - Billing administrator
  - AI administrator

  To learn more about these roles, see [Microsoft 365 admin roles](/microsoft-365/admin/add-users/about-admin-roles).

## Step 1 - Add a billing policy

The first step is to add a billing policy.

1. In the [Microsoft 365 admin center](https://admin.microsoft.com), go to **Copilot** > **Billing & usage**.
2. On the **Billing policies** tab, select **Add a billing policy**.
3. Select the Azure subscription, resource group, and region for the pay-as-you-go setup.
4. Select the user scope for the billing policy:
    - **All Users**: All users in the tenant are included in the billing policy
    - **Specific group**: Assign a specific security group to be included in the billing policy
5. Review the details and select **Create policy** to complete the creation on billing policy.

## Step 2 - Connect a billing policy

After the billing policy is created, the next step is to connect the policy to the Copilot pay-as-you-go service.

1. In the admin center, go to the **Pay-as-you-go services** tab.
2. Select **Microsoft 365 Copilot Chat** and select the billing policy you created.

## Disable pay-as-you-go

Disabling pay-as-you-go for a Copilot service involves disconnecting the billing policies associated with the service, like Microsoft 365 Copilot Chat.

1. In the admin center, go to the **Pay-as-you-go services** tab, and select the Copilot service, like Microsoft 365 Copilot Chat.
2. Unselect the billing policy, one at a time to disconnect the billing policy.
3. Read and accept the confirmation. This step completes the disconnection.

When you turn off pay-as-you-go, it can take up to two hours for users to stop being able to use the agents. If the agent hasn't been used, then it stops being available when pay-as-you-go is turned off.

## Delete a billing policy

After you disable pay-as-you-go, you can also delete the billing policy.

1. In the admin center, select a billing policy, and select the **Delete billing policy** button.
2. Accept the confirmation dialogue. Any services connected to the billing policy are disconnected.
  
## What if pay-as-you-go is already set up in the Power Platform admin center?

If you already set up pay-as-you-go in the Power Platform admin center, the two setups can coexist. You can create another setup in the Microsoft 365 admin center without being double billed. The billing system ensures that you're only billed once for your user's usage, regardless of the setup location.

Although the setups can coexist, we recommend you turn off pay-as-you-go in the Power Platform admin center before you enable it in the Microsoft 365 admin center.

## Related articles

- [Microsoft 365 Copilot pay-as-you-go service overview](overview.md) (article)
- [Meters for Microsoft 365 Copilot pay-as-you-go](meters.md) (article)
- [View costs and billing for Microsoft 365 Copilot pay-as-you-go](view-cost.md) (article)
