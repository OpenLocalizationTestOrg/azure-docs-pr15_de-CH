<properties
   pageTitle="Erstellen Sie eine Arbeits- oder Schulcomputer Identität in AAD | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer Identität Arbeits- oder Schulcomputer in Azure Active Directory mit der virtuellen Windows-Maschinen."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-windows-vms"></a>Erstellen einer Identität Arbeit oder Schule in Azure Active Directory mit Windows-VMs

Persönliches Azure-Konto erstellt oder persönliches MSDN-Abonnement verfügen und erstellt ein Azure-Konto Guthaben MSDN Azure – nutzen verwendet Sie eine *Microsoft-Konto* -Identität um zu erstellen. Viele hervorragende Funktionen von Azure - [Gruppe Ressourcenvorlagen](../azure-resource-manager/resource-group-overview.md) beispielsweise ist erfordern ein Geschäfts- oder Konto (Identität von Azure Active Directory verwaltet) arbeiten. Sie können gehen eine neue Arbeit oder schulkonto erstellen, weil Glück eines der besten Dinge über Ihr persönliches Azure-Konto ist, dass es mit der Standarddomäne Azure Active Directory, mit denen Sie neue Arbeit oder schulkonto mit Azure können, die sie benötigen.

Allerdings Änderungen ermöglichen Ihr Abonnement mit Azure Konto verwalten die `azure login` beschriebene interaktive Anmeldung [hier](../xplat-cli-connect.md). Sie können diesen Mechanismus verwenden, oder die Anweisungen folgen. Sie können auch [eine Arbeit oder Schule Identität in Azure Active Directory mit Linux VMs](virtual-machines-linux-create-aad-work-id.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
