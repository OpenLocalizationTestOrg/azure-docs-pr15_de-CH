<properties
   pageTitle="FQDN für einen virtuellen Computer in Azure-Portal erstellen | Microsoft Azure"
   description="So erstellen Sie einen vollqualifizierten Domänennamen oder FQDN für einen Ressourcen-Manager basierten virtuellen Computer in Azure-Portal."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Erstellen Sie einen vollqualifizierten Domänennamen in Azure-portal
Beim Erstellen einer virtuellen Maschine (VM) in der [Azure-Portal](https://portal.azure.com) mit dem Ressourcen-Manager-Bereitstellungsmodell wird automatisch eine öffentliche IP-Adressressource für den virtuellen Computer erstellt. Diese Adresse wird Remotezugriff auf den virtuellen Computer verwenden. Obwohl das Portal kein [vollqualifizierter Domänenname](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)oder FQDN, standardmäßig erstellt wird, können Sie eine erstellte VM hinzufügen. Dieser Artikel beschreibt die Schritte zum Erstellen einer DNS-Namen oder FQDN.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Sie können jetzt eine Remoteverbindung mit dem DNS-Namen wie mit VM `ssh adminuser@testdnslabel.centralus.cloudapp.azure.com`.

## <a name="next-steps"></a>Nächste Schritte
VM einen öffentlichen IP-Adresse und DNS-Namen besitzt, können Sie allgemeine Anwendungsframeworks oder Dienstleistungen wie Nginx, MongoDB, Andockfenster bereitstellen usw..

Außerdem erfahren Sie mehr über die Tipps auf der Azure-Bereitstellung [mit dem Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md) .