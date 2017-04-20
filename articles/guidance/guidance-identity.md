<properties
   pageTitle="Verwalten von Identitäten in Azure | Microsoft Azure"
   description="Erläutert und vergleicht die verschiedenen Methoden zum Verwalten von Identitäten im Hybrid-Systemen, die lokal und Cloud Berandung mit Azure umfassen."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Verwalten von Identitäten in Azure

In den meisten Windows-basierten Enterprise-Systeme verwenden Sie Active Directory (AD) Identität Verwaltungsdienste Anwendung bereitstellen. Anzeige in einer lokalen Umgebung funktioniert, aber wenn Sie die Netzwerkinfrastruktur für die Cloud erweitern müssen einige wichtigen Informationen bezüglich Identität verwalten zu. Sollten Sie Ihre lokalen Domänen VMs in der Cloud integrieren erweitern? Sollten Sie neue Domänen in der Cloud erstellen und wenn Ja, wie? Sollten Sie eine eigene Gesamtstruktur in der Cloud implementieren oder sollten Sie von Azure Active Directory (AAD) verwenden?

Dieser Artikel beschreibt einige häufig verwendete Optionen in diesem Szenario vorhandene Herausforderungen und können, die Sie ermitteln, welche Lösung am besten, Ihren Bedürfnissen entsprechend Ihren Bedürfnissen.

## <a name="using-azure-active-directory"></a>Verwenden von Azure Active Directory

Können AAD AD-Domäne in der Cloud erstellen und verknüpfen Sie es mit einem lokalen Active Directory-Domäne. AAD können Sie einmaliges Anmelden (SSO) konfigurieren für Benutzer Zugriff über die Cloud Applications.

[! [0]][0]

AAD ist einfach eine Sicherheitsdomäne in der Cloud zu implementieren. Es wird von vielen Microsoft-Programmen wie Microsoft Office 365 verwendet. 

Vorteile von AAD:

- Besteht keine Active Directory-Infrastruktur in der Cloud zu verwalten. AAD ist vollständig verwaltet und von Microsoft verwaltet.

- AAD bietet die gleichen Identitätsinformationen lokal verfügbar ist.

- Authentifizierung geschieht in Azure müssen für externen Applikationen und Benutzer der lokalen Domäne an.

Punkte AAD verwenden:

- Identitätsdienste sind auf Benutzer und Gruppen. Es gibt keine Möglichkeit, Service und Computerkonten zu authentifizieren.

- Konfigurieren Sie Verbindung mit lokalen Verzeichnisses AAD synchronisiert zu halten. 

- Sie sind verantwortlich für die Veröffentlichung von Applikationen, die Benutzer in die Cloud AAD zugreifen können.

Ausführliche Informationen finden Sie unter [Implementieren von Azure Active Directory][implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>Mithilfe von Active Directory in der Cloud verknüpft zu einer lokalen Gesamtstruktur

AD-Verzeichnisdienste (AD DS) lokalen host, aber im Hybrid-Szenario Elemente einer Anwendung in Azure befinden möglich effizienter, diese Funktionalität und AD-Repository in die Cloud zu replizieren. Dieser Ansatz kann verringern die Verzögerung durch das Senden von Authentifizierung und lokale Autorisierungsanfragen aus der Cloud zurück in AD DS lokal ausgeführt. 

[! [1]][1]

Dieser Ansatz erfordert, dass Ihre Domäne in der Cloud erstellen und an die lokale Gesamtstruktur beitreten. Erstellen virtueller Computer zum Hosten der AD DS-Dienste.

Vorteile der Verwendung einer separaten Domäne in der Cloud:

- Bietet die Möglichkeit, Benutzer-, Service und Konten lokale Authentifizierung und in der Cloud.

- Bietet Zugriff auf die gleichen Identitätsinformationen lokal verfügbar ist.

- Es ist nicht erforderlich zu einer separaten Active Directory-Gesamtstruktur. die Domäne in die Cloud kann der lokalen Gesamtstruktur angehören.

- Sie können für lokale Gruppenrichtlinienobjekte in der Domäne in der Cloud definierten Gruppenrichtlinien anwenden.

Aspekte der Verwendung einer separaten Domäne in der Cloud:

- Erfordert das Erstellen und Verwalten von AD DS-Server und Domäne in der Cloud.

- Möglicherweise Synchronisierung Latenz zwischen Domänenserver in der Cloud und Server vor Ort.

Informationen zum Konfigurieren dieser Architektur finden Sie unter [Erweitern Active Directory Directory Services (ADDS) in Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Mithilfe von Active Directory mit einer separaten Gesamtstruktur

Eine Organisation, die lokale Active Directory (AD) führt möglicherweise eine Gesamtstruktur mit zahlreichen Domänen. Können Sie Domänen zwischen Funktionsbereiche isolieren, die möglicherweise aus Sicherheitsgründen getrennt werden müssen, aber Informationen zwischen Domänen durch Einrichten von Vertrauensstellungen teilen.

[! [2]][2]

Eine Organisation, die separate Domänen nutzt profitieren Azure durch eine oder mehrere dieser Domänen in einer separaten Gesamtstruktur in die Cloud verschieben. Alternativ möchte eine Organisation alle Cloudressourcen logisch abgetrennter diese statt lokal halten und Informationen zu Cloudressourcen im eigenen Verzeichnis als Bestandteil einer Gesamtstruktur auch statt in der Cloud speichern.

Vorteile der Verwendung einer separaten Gesamtstruktur in der Cloud:

- Implementieren von lokalen Identitäten und trennen Azure nur Identitäten.

- Besteht keine Notwendigkeit zur lokalen Replikation AD-Gesamtstruktur in Azure Auswirkungen Netzwerklatenz.

Hinweise:

- Authentifizierung für lokalen Identitäten in der Cloud führt zusätzliche Netzwerk *Hops* zum lokalen AD-Servern.

- Müssen Sie Ihre eigenen AD DS-Server und Gesamtstruktur in der Cloud, und die geeigneten Vertrauensstellungen zwischen Gesamtstrukturen einrichten.

[Erstellen einer Ressourcengesamtstruktur Active Directory Directory Services (ADDS) in Azure] Dokument[ adds-forest-in-azure] Implementieren dieser Methode ausführlicher beschrieben.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Verwenden Active Directory Federation Services (ADFS) mit Azure

ADFS können lokal, jedoch im Hybrid-Szenario Anwendung in Azure befinden möglich effizienter, diese Funktionalität in der Cloud implementieren, wie unten dargestellt.

[! [3]][3]

Diese Architektur ist besonders für:

- Projektmappen, die verbundenen Autorisierung gemacht ASP.NET-Webanwendungen zu Partnerorganisationen nutzen.

- Systeme, die von außerhalb der Organisation Firewalls mit Webbrowsern unterstützt.

- Systeme, die Benutzern Zugriff auf Webapplikationen aus autorisierten externen Geräte wie remote-Computern, Notebooks und andere mobile Geräte ermöglichen. 

Vorteile von ADFS Azure:

- Sie können Ansprüche unterstützende Anwendung nutzen.

- Es bietet die Möglichkeit, externe Partner für die Authentifizierung zu vertrauen.

- Es bietet Kompatibilität mit großen Authentifizierungsprotokolle.

Mit ADFS Azure Aspekte:

- Sie müssen Ihre eigenen fügt ADFS und ADFS Web Application Proxy Server in der Cloud zu implementieren.

- Diese Architektur kann leichter zu konfigurieren.

Ausführliche Informationen finden Sie unter [Implementieren von Active Directory Federation Services (ADFS) in Azure][adfs-in-azure].

## <a name="next-steps"></a>Nächste Schritte

Die nachstehenden erläutert in diesem Artikel beschriebenen Architekturen implementieren.

- [Implementieren von Azure Active Directory][implementing-aad]
- [Erweitern Sie Active Directory-Verzeichnisdienst (ADDS) in Azure][extending-adds]
- [Erstellen einer Ressourcengesamtstruktur Active Directory Directory Services (ADDS) in Azure][adds-forest-in-azure]
- [Implementieren von Active Directory Federation Services (ADFS) in Azure][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Architektur der Cloud Identität mit Active Directory Azure"
[1]: ./media/guidance-identity/figure2.png "Sichere Hybrid-Netzwerkarchitektur mit Active Directory"
[2]: ./media/guidance-identity/figure3.png "Sichere Hybrid-Netzwerkarchitektur mit separaten Active Directory-Domänen und Gesamtstrukturen"
[3]: ./media/guidance-identity/figure4.png "Sichere Hybrid-Netzwerkarchitektur mit ADFS"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md