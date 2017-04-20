<properties
    pageTitle="Azure AD Connect: Unterstützte Topologien | Microsoft Azure"
    description="In diesem Thema werden die unterstützte und nicht unterstützte Topologien für Azure AD verbinden"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Topologien für Azure AD verbinden
Ziel dieses Themas ist anderen lokalen und Azure AD Topologien mit Azure AD Connect Sync Schlüssel Integration-Lösung. Unterstützte und nicht unterstützte Konfigurationen beschrieben.

Legende für Bilder im Dokument:

Beschreibung | Symbol
-----|-----
Lokale Active Directory-Gesamtstruktur| ![ANZEIGE](./media/active-directory-aadconnect-topologies/LegendAD1.png)
Active Directory mit gefilterten importieren| ![ANZEIGE](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Azure AD Connect Synchronisierungsserver| ![Synchronisieren](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure AD Sync Server "Staging Verbindungsmodus"| ![Synchronisieren](./media/active-directory-aadconnect-topologies/LegendSync2.png)
GALSync mit FIM2010 oder MIM2016| ![Synchronisieren](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Azure AD Connect Synchronisierungsserver detaillierte| ![Synchronisieren](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure AD-Verzeichnis |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
Nicht unterstütztes Szenario | ![Nicht unterstützt](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>Einzelne Gesamtstruktur, einzelne Azure AD-Mandanten
![Einzelne Gesamtstruktur einzelnen Mandanten](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

Die häufigste Topologie ist eine einzige Gesamtstruktur lokal mit einer oder mehreren Domänen und einzelne Azure AD Mieter. Azure AD-Authentifizierung wird Synchronisierung von Kennwörtern verwendet. Die express-Installation von Azure AD Connect unterstützt diese Topologie.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Gesamtstruktur mehrere Synchronisierungsserver ein Azure AD-Mandanten
![Einzelne Gesamtstruktur nicht unterstützte gefiltert](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Mehrere Azure AD Connect Sync Server mit derselben Azure AD-Mandanten außer einem [Stagingserver](#staging-server)verbunden wird nicht unterstützt. Es wird nicht unterstützt, auch wenn diese sich gegenseitig ausschließenden Objektgruppe Synchronisierung konfiguriert. Sie können dies haben Sie alle Domänen in der Gesamtstruktur von einem einzelnen Server oder Last auf mehrere Server verteilen erreichen.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Mehrere Gesamtstrukturen, einzelne Azure AD-Mandanten
![Mehrere Gesamtstruktur einzelnen Mandanten](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Viele Unternehmen haben Umgebungen mit mehreren lokalen Active Directory-Gesamtstrukturen. Es gibt verschiedene Gründe für mehrere lokale Active Directory-Gesamtstruktur. Beispiele sind Entwürfe mit Ressource und nach einer Fusion oder Übernahme.

Wenn Sie mehrere Gesamtstrukturen, allen Gesamtstrukturen haben muß einzelne Azure AD-Verbindung erreichbar Sync-Server. Sie müssen keinen Server zu einer Domäne hinzufügen. Ggf. alle Wald, kann der Server in einem DMZ-Netzwerk befinden.

Der Installationsassistent Azure AD Connect bietet mehrere Optionen zur Konsolidierung von Benutzern in mehreren Gesamtstrukturen dargestellt. Ziel ist es, dass ein Benutzer nur einmal in Azure AD angezeigt wird. Es gibt einige allgemeinen Topologien in benutzerdefinierten Installationspfad im Installations-Assistenten konfigurieren können. Die Option entsprechenden Ihrer Topologie auf der Seite **eindeutig identifizieren Benutzer**darstellt. Die Konsolidierung wird nur für Benutzer konfiguriert. Doppelte Gruppen werden nicht in der Standardkonfiguration konsolidiert.

Im nächsten Abschnitt werden allgemein übliche Topologien erläutert: [Separate Topologien](#multiple-forests-separate-topologies)und [Full-Mesh](#multiple-forests-full-mesh-with-optional-galsync) [- Ressource](#multiple-forests-account-resource-forest).

Die Standardkonfiguration Azure AD Connect synchronisiert wird vorausgesetzt:

1. Benutzer haben nur ein aktiviertes Konto und die Gesamtstruktur dieses Konto befindet zur Authentifizierung des Benutzers verwendet. Diese Annahme ist für beide Kennwort synchronisieren und Föderation. UserPrincipalName und SourceAnchor-ImmutableID stammen aus dieser Gesamtstruktur.
2. Benutzer haben nur ein Postfach.
3. Die Gesamtstruktur, die das Postfach eines Benutzers hat die besten Daten für Attribute in der Exchange globale Adressliste (GAL) angezeigt. Ist kein Postfach für den Benutzer, kann jeder Gesamtstruktur verwendet werden, um diese Attributwerte beitragen.
4. Haben Sie ein verknüpftes Postfach, dann gibt auch ein anderes Konto in einer anderen Gesamtstruktur für die Anmeldung verwendet.

Wenn Ihre Umgebung diese Annahmen nicht übereinstimmen, geschieht Folgendes:

- Haben Sie mehr als ein aktives Konto oder mehrere Postfächer wählt das Synchronisierungsmodul und ignoriert die anderen.
- Ein verknüpftes Postfach mit kein aktives Konto wird nicht in Azure AD exportiert. Das Benutzerkonto ist nicht als Mitglied einer Gruppe dargestellt. Ein verknüpftes Postfach DirSync würde immer als normales Postfach dargestellt. Diese Änderung ist absichtlich ein anderes Verhalten unterstützen Szenarien mit mehreren Gesamtstrukturen.

Weitere Details finden Verständnis [der Standard-Konfiguration](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Mehrere Gesamtstrukturen, mehrere Synchronisierungsserver ein Azure AD-Mandanten
![Mehrere Gesamtstruktur mehrere Sync nicht unterstützt](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Wird nicht um mehrere Azure AD verbinden Synchronisierungsserver an einem Azure AD-Mandanten. Die Ausnahme ist die Verwendung von einem [Stagingserver](#staging-server).

### <a name="multiple-forests--separate-topologies"></a>Mehrere Gesamtstrukturen – separate Topologien
**Benutzer werden über alle Verzeichnisse nur einmal dargestellt.**

![Mehrere Benutzer einmal](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Mehrere verschiedene Topologien](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

In dieser Umgebung alle Gesamtstrukturen lokal als getrennte Einheiten behandelt und kein Benutzer in jeder Gesamtstruktur vorhanden wäre.
Jede Gesamtstruktur verfügt über eigene Organisation und gibt keine GALSync zwischen den Gesamtstrukturen. Diese Topologie könnte die Situation nach einer Fusion oder Übernahme oder in einer Organisation, in dem jede Unternehmenseinheit arbeitet, voneinander isoliert. Diese Gesamtstrukturen in derselben Organisation in Azure AD und eine einheitliche GAL angezeigt.
In diesem Bild jedes Objekt in jeder Gesamtstruktur dargestellt im Metaverse und Ziel Azure AD-Mandanten aggregiert.

### <a name="multiple-forests--match-users"></a>Mehrere Gesamtstrukturen – Übereinstimmung Benutzer
**Benutzeridentitäten vorhanden über mehrere Verzeichnisse**

Für diese Szenarien ist, Verteilerlisten und Sicherheitsgruppen enthalten eine Mischung von Benutzern, Kontakten und FSPs (Fremde Sicherheitsprinzipale)

FSPs werden in fügt zum Mitglieder aus anderen Gesamtstrukturen in einer Sicherheitsgruppe darstellen. Alle FSPs werden mit dem eigentlichen Objekt in Azure AD.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Mehrere Gesamtstrukturen – full-Mesh mit optionalen GALSync
**Benutzeridentitäten über mehrere Verzeichnisse vorhanden sind. Entsprechend: e-Mail-Attribut**

![Mehrere Gesamtstruktur Benutzer E-Mail](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Mehrere Gesamtstruktur Full-Mesh](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Eine full-Mesh-Topologie ermöglicht Benutzern und Ressourcen in jeder Gesamtstruktur befinden und häufig wäre bidirektionale Vertrauensstellungen zwischen den Gesamtstrukturen.

Wenn Exchange in mehreren Gesamtstrukturen vorhanden ist, kann es optional eine lokale GALSync-Lösung. Jeder Benutzer wäre in anderen Gesamtstrukturen als Kontakt dargestellt. GALSync wird häufig mit Forefront Identity Manager 2010 oder Microsoft Identity Manager 2016 implementiert. Azure AD Connect kann für lokale GALSync verwendet werden.

In diesem Szenario sind Identitätsobjekte über das Attribut Mail verbunden. Ein Benutzer mit einem Postfach in einer Gesamtstruktur wird mit den Kontakten in anderen Gesamtstrukturen verknüpft.

### <a name="multiple-forests--account-resource-forest"></a>Mehrere Gesamtstrukturen – Konto Ressourcengesamtstruktur
**Benutzeridentitäten über mehrere Verzeichnisse vorhanden sind. Entsprechend: ObjectSID und MsExchMasterAccountSID Attribute**

![Mehrere Gesamtstruktur Benutzer ObjectSID](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Mehrere Gesamtstruktur-AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

In einer Konto-Topologie müssen Sie eine oder mehrere Kontengesamtstrukturen mit aktiven Benutzerkonten. Sie können eine oder mehrere Ressourcengesamtstrukturen mit deaktivierten Konten.

In diesem Szenario vertraut (mindestens) **Ressourcengesamtstruktur** alle **Kontengesamtstrukturen**. Die Ressourcengesamtstruktur enthält normalerweise ein erweitertes Active Directory-Schema Exchange mit Lync. Alle Exchange und Lync sowie andere gemeinsame Dienste befinden sich in dieser Gesamtstruktur. Benutzer haben ein deaktiviertes Benutzerkonto in dieser Gesamtstruktur und der Gesamtstruktur Postfach verknüpft ist.

## <a name="office-365-and-topology-considerations"></a>Office 365 und Topologie Aspekte
Einige Office 365-Workloads müssen Einschränkungen unterstützten Topologien. Wenn Sie diese verwenden möchten, lesen Sie das Thema unterstützte Topologien für die Arbeitslast.

Arbeitslast |  
--------- | ---------
Exchange Online | Ist mehr als eine Exchange-Organisation lokal (d. h. Exchange bereitgestellt wurde, mehrere Gesamtstrukturen), müssen Sie Exchange 2013 SP1 verwenden oder höher. Details finden Sie hier: [Hybrid-Bereitstellung mit mehreren Active Directory-Gesamtstrukturen](https://technet.microsoft.com/library/jj873754.aspx)
Skype für Unternehmen | Verwenden Sie mehrere Gesamtstrukturen lokal, wird nur die Konto-Topologie unterstützt. Details zu unterstützte Topologien Sie hier finden: [Umweltvorschriften Skype for Business Server 2015](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>Staging-server
![Staging-Server](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect unterstützt eine zweite Installation im **Staging-Modus**. Server in diesem Modus liest Daten aus allen verbundenen Verzeichnissen aber nicht alles an verbundene Verzeichnisse schreiben. Es normal Synchronisierungszyklus und hat daher eine aktualisierte Kopie der Daten. In einem Notfall, in dem der primäre Server ausfällt, kann Stagingserver Failover. Hierzu in Azure AD Connect-Assistenten. Dieser zweite Server vorzugsweise in einem anderen Rechenzentrum befinden, da keine Infrastruktur mit dem primären Server freigegeben ist. Sie müssen jede konfigurationsänderung auf dem primären Server auf den zweiten Server manuell kopieren.

Ein staging-Server kann auch eine neue Konfiguration und die Auswirkung auf die Daten verwendet werden. Sie können eine Vorschau die Änderungen und die Konfiguration. Wenn Sie die neue Konfiguration zufrieden sind, können Sie Stagingserver active Server machen und setzen den alten active Server staging-Modus.

Diese Methode kann auch verwendet werden, active Sync-Server ersetzt. Bereiten Sie des neuen Servers vor und in der Stagingdatenbank Modus. Sicherzustellen, dass sie in gutem Zustand deaktivieren staging-Modus (aktivieren), und der aktive Server Herunterfahren.

Es ist möglich, mehrere Stagingserver haben, wenn Sie mehrere Sicherungskopien in verschiedenen Rechenzentren haben.

## <a name="multiple-azure-ad-tenants"></a>Mehrere Azure AD-Mandanten
Microsoft empfiehlt, einen einzelnen Mandanten in Azure AD für eine Organisation.
Vor mehreren Azure AD-Mandanten verwenden, Themen dieser Szenarien können Sie einen einzelnen Mandanten verwenden.

Thema |  
--------- | ---------
Delegierung mit Verwaltungseinheiten | [Management von Verwaltungseinheiten in Azure AD](active-directory-administrative-units-management.md)

![Multi-Gesamtstruktur Multi-tenant](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Gibt es eine 1:1-Beziehung zwischen einer Azure AD Connect Synchronisierungsserver und Azure AD-Mandanten. Für jeden Mandanten Azure AD benötigen Sie ein Azure AD Connect Sync-Serverinstallation. Azure AD Mieter sind entwurfsbedingt isoliert und Benutzer in einer nicht sehen Benutzer in anderen Mandanten. Wenn diese Trennung soll dann dies unterstützt, aber Sie andernfalls die einzelnen verwenden Azure AD Tenant-Modells.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Jedes Objekt nur einmal in Azure AD-Mandanten
![Einzelne Gesamtstruktur gefiltert](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

In dieser Topologie ist jeder Azure AD-Mandanten ein Azure AD Connect Sync-Server verbunden. Azure AD Connect Synchronisierungsserver müssen zum Filtern haben jede Objektgruppe auf gegenseitig konfiguriert werden. Beispielsweise können Sie jeder Server mit einer bestimmten Domäne oder Organisationseinheit festlegen. Eine Domäne kann nur in einem einzigen registriert Azure AD-Mandanten. Die UPNs der Benutzer in der lokalen AD sowie separate Namespaces verwenden muss. In der Abbildung oben drei separate UPN werden Suffixe z. B. in den Räumen registriert AD: contoso.com und fabrikam.com wingtiptoys.com. Die Benutzer in den einzelnen lokalen AD-Domäne einen anderen Namespace verwenden.

Gibt es keine GALsync zwischen Azure AD Mieter Instanzen. Das Adressbuch in Exchange Online und Skype für Unternehmen nur zeigt denselben Mandanten.

Diese Topologie weist die folgenden Einschränkungen sonst Szenarien unterstützt:

- Nur einer der Azure AD-Mandanten können Exchange Hybrid mit lokalen Active Directory.
- Windows 10 Geräte kann nur mit einer Azure AD-Mandanten.

Die für gegenseitig Objektgruppe gilt auch für das Rückschreiben. Einige Features Rückschreiben werden mit dieser Topologie nicht unterstützt, da diese Funktionen übernehmen eine einzelne Konfiguration lokal:

-   Gruppenrückschreiben mit Standardkonfiguration
-   Gerät Rückschreiben

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Jedes Objekt mehrmals in Azure AD-Mandanten
![Einzelne Gesamtstruktur Multi-Tenant nicht unterstützt](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Einzelne Gesamtstruktur mehrere Connectors nicht unterstützt](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- Es wird nicht unterstützt, um das gleiche Benutzerkonto an mehrere Azure AD-Mandanten zu synchronisieren.
- Es ist nicht unterstützte Konfiguration ändern zu Benutzern in Azure Anzeige in anderen Azure AD-Mandanten als Kontakte angezeigt.
- Nicht unterstützt Azure AD Connect Synchronisierung mehrere Azure AD-Mandanten Verbindung ändern.

### <a name="galsync-by-using-writeback"></a>GALsync mithilfe von Rückschreiben
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD Mieter sind entwurfsbedingt isoliert.

- Es wird nicht unterstützt, zum Ändern der Konfiguration von Azure AD Connect Synchronisierung zum Lesen von Daten aus einem anderen Azure AD-Mandanten.
- Nicht unterstützt, um Benutzer als Kontakte zum Exportieren einer anderen lokalen AD mit Azure AD-Verbindung synchronisieren.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync mit dem lokalen server
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

Verwenden Sie FIM2010-MIM2016 lokale GALsync Benutzer zwischen zwei Exchange-Organisationen wird unterstützt. Die Benutzer in einer Organisation wird als fremde Benutzer/Kontakte in der Organisation. Diese anderen lokalen anzeigen können dann Azure AD Mieter synchronisiert werden.

## <a name="next-steps"></a>Nächste Schritte
Azure AD Connect für diese Szenarien installieren finden Sie unter [benutzerdefinierte Installation von Azure AD verbinden](./connect/active-directory-aadconnect-get-started-custom.md).

Erfahren Sie mehr über die Konfiguration [Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
