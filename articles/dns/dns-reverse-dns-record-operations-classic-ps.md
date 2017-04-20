<properties
   pageTitle="Verwalten von reverse DNS-Datensätze für Ihre Azure (klassisch) Dienste mit PowerShell | Microsoft Azure"
   description="Zum Verwalten von reverse-DNS-Einträge und PTR-Einträge für Azure Services mit PowerShell im klassischen Bereitstellungsmodell. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Verwalten von reverse DNS-Datensätze für Ihre Azure mithilfe von Azure PowerShell Services (klassisch)

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Überprüfung von reverse-DNS-Einträge
Um sicherzustellen, dass Dritte reverse-DNS-Einträge für DNS-Domänen zuordnen erstellen kann, ermöglicht Azure nur die Erstellung von reverse DNS-Datensatz, in dem Folgendes zutrifft:

- Der Reverse DNS FQDN den Namen Cloud-Dienst für die angegeben wurde, oder einen beliebigen Namen Cloud-Dienst innerhalb des gleichen Abonnements z.B. ist reverse DNS ist "contosoapp1.cloudapp.net".
- Reverse DNS FQDN vorwärts löst den Namen oder die IP-Adresse des Cloud-Dienst für die angegeben wurde oder mit keinem Cloud-Dienst oder IP-innerhalb des gleichen Abonnements z. B. reverse DNS ist "app1.contoso.com." einen CName-Alias für contosoapp1.cloudapp.net ist.

Überprüfungen werden nur ausgeführt, wenn die reverse DNS-Eigenschaft für einen Cloud-Dienst festgelegt oder geändert wird. Periodische erneute Validierung wird nicht ausgeführt.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Vorhandene Clouddienste reverse DNS hinzufügen
Sie können einen vorhandenen Cloud-Dienst mit dem Cmdlet "Set-AzureService" reverse DNS-Datensatz hinzufügen:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Erstellen Sie einen Cloud-Dienst reverse DNS
Sie können einen neuen Cloud-Dienst mit angegebenen mit dem Cmdlet "Set-AzureService" reverse DNS-Eigenschaft hinzufügen:

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Reverse-DNS für vorhandene Cloud-Dienste anzeigen
Sie können den Wert für einen vorhandenen Cloud-Dienst mithilfe des Cmdlets "Get-AzureService" anzeigen:

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Reverse DNS aus vorhandenen Cloud-Dienste entfernen
Sie können reverse DNS-Eigenschaft aus einem vorhandenen Cloud-Dienst mit dem Cmdlet "Set-AzureService" entfernen. Dies ist der Wert reverse DNS-Eigenschaft leer:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
