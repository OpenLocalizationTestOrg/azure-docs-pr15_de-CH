<properties
   pageTitle="Reverse DNS-Datensätze für Ihre Azure-Dienste Azure CLI verwalten | Microsoft Azure"
   description="Verwalten von reverse-DNS-Einträge und PTR-Einträge für Azure Services mithilfe der Azure-CLI in Ressourcen-Manager"
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-the-azure-cli"></a>Verwalten von reverse DNS-Datensätze für Ihre Azure-Dienste mithilfe der Azure-CLI

[AZURE.INCLUDE [DNS-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](dns-reverse-dns-record-operations-classic-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Überprüfung von reverse-DNS-Einträge
Um sicherzustellen, dass Dritte reverse-DNS-Einträge für DNS-Domänen zuordnen erstellen kann, ermöglicht Azure nur die Erstellung von reverse DNS-Datensatz, in dem Folgendes zutrifft:

- "ReverseFqdn" ist gleich "Fqdn" für die öffentliche IP-Adressressource für die angegeben wurde, oder "Fqdn" für alle öffentlichen IP-Adresse innerhalb des gleichen Abonnements z. B. ist "ReverseFqdn" "contosoapp1.northus.cloudapp.azure.com".

- "ReverseFqdn" behebt vorwärts auf den Namen oder die IP-öffentliche IP-Adresse für die angegeben wurde oder öffentliche IP-Adresse "Fqdn" oder IP innerhalb des gleichen Abonnements z. B. "ReverseFqdn" "app1.contoso.com" ist also einen CName-Alias für "contosoapp1.northus.cloudapp.azure.com"

Überprüfungen werden nur ausgeführt, wenn die reverse DNS-Eigenschaft für eine öffentliche IP-Adresse festgelegt oder geändert wird. Periodische erneute Validierung wird nicht ausgeführt.

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>Öffentliche Adressen reverse DNS hinzufügen
Sie können eine vorhandene öffentliche IP-Adresse mithilfe der Azure-Netzwerk öffentliche Ip-reverse DNS hinzufügen:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -f contosoapp1.westus.cloudapp.azure.com.

Möchten Sie reverse-DNS-eine vorhandene öffentliche IP-Adresse hinzufügen, die bereits einen DNS-Namen hat, müssen Sie einen DNS-Namen angeben. Mithilfe der öffentlichen Ip-Azure Netzwerk erreichen können:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Erstellen Sie eine öffentliche IP-Adresse mit reverse DNS
Sie können hinzufügen, erstellen Sie eine neue öffentliche IP-Adresse mit der reverse DNS-Eigenschaft mithilfe der Azure-Netzwerk Public-Ip angegeben:

    azure network public-ip create -n PublicIp3 -g NRP-DemoRG-PS -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Reverse DNS vorhandene öffentliche IP-Adressen anzeigen
Den konfigurierten Wert können vorhandene öffentliche IP-Adresse mithilfe des öffentlichen Ip-Netzwerk in Azure angezeigt werden:

    azure network public-ip show -n PublicIp3 -g NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Öffentliche Adressen reverse DNS entfernen
Sie können reverse DNS-Eigenschaft eine vorhandene öffentliche IP-Adresse mit öffentlichen Ip-Azure-Netzwerk entfernen. Dies ist der Wert ReverseFqdn-Eigenschaft leer:

    azure network public-ip set -n PublicIp3 -g NRP-DemoRG-PS –f “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]
