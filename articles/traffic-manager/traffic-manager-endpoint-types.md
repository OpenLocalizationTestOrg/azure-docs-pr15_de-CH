<properties
    pageTitle="Traffic Manager Endpunkttypen | Microsoft Azure"
    description="Dieser Artikel beschreibt verschiedene Endpunkte mit Azure Traffic Manager verwendet werden kann"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-endpoints"></a>Traffic Manager Endpunkte

Microsoft Azure Traffic Manager können Sie steuern, Verteilung des Netzwerkverkehrs Anwendung Bereitstellungen in verschiedenen Rechenzentren ausgeführt. Jede anwendungsbereitstellung als einen Endpunkt konfigurieren in Traffic Manager. Traffic Manager eine DNS-Anforderung erhält, wählt einen Endpunkt in der DNS-Antwort zurückgegeben. Traffic Manager basiert die Wahl der aktuelle Endpunktstatus und Datenverkehr weiterleiten. Weitere Informationen finden Sie unter [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).

Es gibt drei Typen der Endpunkte von Traffic Manager unterstützt:

- **Azure Endpunkte** werden in Azure gehosteten Dienste verwendet.
- **Externer Endpunkte** werden für Dienste außerhalb Azure, entweder lokal oder mit einem anderen Hostinganbieter verwendet.
- **Verschachtelte Endpunkte** wird Traffic Manager Profile erstellen flexibler routing von Datenverkehr Schemas für größere und komplexere Installationen Vorhaben kombinieren.

Es gibt keine Beschränkung, wie Endpunkte verschiedene Typen in einem einzigen Profil Traffic Manager kombiniert werden. Jedes Profil kann eine Mischung aus Endpunkttypen enthalten.

In den folgenden Abschnitten beschreiben jede Endpunkttyp ausführlicher beschrieben.

## <a name="azure-endpoints"></a>Azure Endpunkte

Azure Endpunkte werden Azure-basierte Services in Traffic Manager. Die folgenden Azure Ressourcentypen werden unterstützt:

- "Klassische" IaaS VMs und PaaS cloud-Services.
- Webapps
- Öffentl.IP Ressourcen (mit VMs verbunden werden kann direkt oder über ein Azure-Lastenausgleich). Die Öffentl.IP müssen den DNS-Namen in Traffic Manager-Profil verwendet werden.

Öffentl.IP Ressourcen sind Ressourcen Azure-Ressourcen-Manager. Sie sind nicht in der klassischen Bereitstellungsmodell vorhanden. Sie sind nur in unterstützten Traffic Manager der Azure-Ressourcen-Manager Erfahrungen. Der andere Endpunkt werden über Ressourcen-Manager und das klassische Bereitstellungsmodell unterstützt.

Bei Azure Endpunkte erkennt Traffic Manager 'Klassisch' IaaS VM, Cloud-Dienst oder Web App beendet und gestartet ist. Der Endpunktstatus Status wider. Einzelheiten finden Sie unter [Traffic Manager Endpunkt überwachen](traffic-manager-monitoring.md#endpoint-and-profile-status) . Wenn der zugrunde liegende angehalten führt Traffic Manager keine Gesundheitskontrolle Endpunkt oder direkten Verkehr an den Endpunkt. Keine Traffic Manager Fakturierung Ereignisse für die Instanz beendet. Beim Neustart des Dienstes ist Rechnung wieder aufgenommen, und der Endpunkt Verkehr erhalten. Diese Erkennung gilt nicht für Öffentl.IP-Endpunkte.

## <a name="external-endpoints"></a>Externer Endpunkte

Externer Endpunkte werden Services von Azure. Beispielsweise ein Dienst lokal gehostet oder einen anderen Anbieter. Externer Endpunkte können einzeln oder kombiniert mit Azure Endpunkten in demselben Traffic Manager-Profil. Kombinieren von Azure Endpunkte mit externer Endpunkte kann verschiedene Szenarios:

- In einer aktiv / aktiv- oder Aktiv / Passiv-Failover Muster Verwendung Azure höhere Redundanz für eine vorhandene Anwendung lokal.
- Verringerung der Latenz der Anwendung für Benutzer auf der ganzen Welt erweitern Sie vorhandenen lokalen Anwendung zusätzlichen Standorten in Azure Weitere Informationen finden Sie unter [Traffic Manager "Leistung" Streckenführung](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Mit Azure zusätzliche Kapazität für eine vorhandene lokale Anwendung kontinuierlich oder als "Burst-Cloud" Lösung zu einem Anstieg der Nachfrage.

In bestimmten Fällen ist für externe Endpunkte verwenden auf Azure Services (Beispiele finden Sie unter [häufig gestellte Fragen](#faq)). In diesem Fall werden bei der Azure Endpunkte Rate, nicht die externe Endpunkte Gesundheitskontrolle belastet. Aber im Gegensatz zu Azure Endpunkte beenden oder Löschen des zugrunde liegenden Dienstes HealthCheck Fakturierung fortgesetzt deaktivieren oder löschen Sie den Endpunkt in Traffic Manager.

## <a name="nested-endpoints"></a>Geschachtelte Endpunkte

Geschachtelte Endpunkte Verbinden mehrerer Traffic Manager Profile erstellen flexibler Datenverkehrs-routing und Unterstützung für die größeren, komplexen Installationen. Verschachtelte Endpunkte wird Profil 'Kind' Profil 'Parent' als Endpunkt hinzugefügt. Die untergeordneten und übergeordneten Profile können andere Endpunkte eines beliebigen Typs, einschließlich andere geschachtelte Profile enthalten. Weitere Informationen finden Sie unter [geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps als Endpunkte

Einige zusätzliche Aspekte sollten bei Web Apps als Endpunkte in Traffic Manager zu konfigurieren:

1. Nur Web Apps 'Standard' SKU oder höher sind für die Verwendung mit Traffic Manager. Versuche, einen Web App eine niedrigere SKU hinzufügen fehl. Herabstufung der SKUs einer vorhandenen Web App führt in Traffic Manager senden von Datenverkehr nicht an Web App.

2. Wenn ein Endpunkt eine HTTP-Anforderung empfängt, verwendet 'Host'-Header in der Anforderung bestimmt die Web App Anfrage sollte. Der Host-Header enthält den DNS-Namen verwendet, z. B. 'contosoapp.azurewebsites.net' angefordert. Um einen anderen DNS-Namen mit Ihrer Anwendung verwenden, muss der DNS-Namen als einen benutzerdefinierten Domänennamen für die Anwendung registriert. Wenn Web App Endpunkt als Azure Endpunkt hinzufügen, wird automatisch Traffic Manager Profil DNS-Namen für die Anwendung registriert. Diese Registrierung wird automatisch entfernt, wenn der Endpunkt gelöscht wird.

3. Jedes Profil Traffic Manager können höchstens einen Web App Endpunkt aus jeder Region jeweils Azure. Um diese Einschränkung zu umgehen, können Sie eine Web-Anwendung als externen Endpunkt konfigurieren. Weitere Informationen finden Sie unter [häufig gestellte Fragen](#faq).

## <a name="enabling-and-disabling-endpoints"></a>Aktivieren und Deaktivieren von Endpunkten

Deaktivieren eines Endpunkts in Traffic Manager kann hilfreich sein, vorübergehend Datenverkehr von einem Endpunkt entfernen im Wartungsmodus oder bereitgestellt wird. Sobald der Endpunkt wieder ausgeführt wird, können sie erneut aktiviert werden.

Endpunkte können aktiviert und deaktiviert das Portal Traffic Manager PowerShell, CLI oder REST-API, die im Ressourcen-Manager und das klassische Bereitstellungsmodell unterstützt werden.

>[AZURE.NOTE] Deaktivieren eines Endpunkts Azure hat nichts mit der Bereitstellung in Azure. Ein Azure Service (z. B. bleibt ein VM oder Web App ausgeführt wird und auch wenn in Traffic Manager deaktiviert empfangen. Datenverkehr kann direkt an die Dienstinstanz nicht über den Traffic Manager DNS-Profilnamen adressiert werden. Weitere Informationen finden Sie unter [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).

Die aktuelle Berechtigung jeder Endpunkt empfangen hängt die folgenden Faktoren:

- Der Profilstatus (aktiviert/deaktiviert)
- Der Endpunktstatus (aktiviert/deaktiviert)
- Die Ergebnisse der Gesundheit für diesen Endpunkt überprüft

Weitere Informationen finden Sie unter [Traffic Manager Endpunkt monitoring](traffic-manager-monitoring.md#endpoint-and-profile-status).

>[AZURE.NOTE] Da Traffic Manager auf DNS arbeitet, kann vorhandene Verbindungen an jedem Endpunkt beeinflussen. Wenn ein Endpunkt verfügbar ist, weist Traffic Manager neue Verbindungen zu einem anderen Endpunkt. Jedoch weiterhin der Host hinter den Endpunkt deaktiviert oder fehlerhaft empfangen Datenverkehr über vorhandene Verbindungen, bis diese beendet sind. Programme sollten die Dauer zum Zulassen von Datenverkehr aus vorhandenen Verbindungen einschränken.

Wenn alle Endpunkte in einem Profil deaktiviert sind oder des Profils deaktiviert ist, sendet Traffic Manager eine "NXDOMAIN" auf eine neue DNS-Abfrage.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kann ich Traffic Manager mit Endpunkten mehrere Abonnements verwenden?

Mit Endpunkten mehrere Abonnements ist nicht möglich mit Azure Web Apps. Azure Web Apps müssen alle benutzerdefinierten Domänennamen mit Web Apps nur innerhalb eines Abonnements verwendet wird. Es ist nicht möglich Web Apps mehrere Abonnements mit dem gleichen Domänennamen verwenden.

Für andere Endpunkt kann Traffic Manager mit Endpunkten über mehrere Abonnements verwenden. Wie Traffic Manager konfigurieren, hängt davon ab, ob Sie das klassische Bereitstellungsmodell oder der Ressourcenmanager Erfahrung verwenden.

- Im Ressourcen-Manager Endpunkte von Abonnements zu Traffic Manager möglich so Traffic Manager-Profil konfigurieren Person Lesezugriff auf den Endpunkt verfügt. Diese Berechtigungen können [Ressourcenmanager Azure rollenbasierte Zugriffskontrolle (RBAC)](../active-directory/role-based-access-control-configure.md)erteilt werden.
- In der klassischen Bereitstellung Modell Schnittstelle erfordert Traffic Manager die Cloud-Services oder Web Apps als Azure Endpunkte konfiguriert das Abonnement Traffic Manager-Profil befinden. Cloud-Endpunkte in andere Abonnements können als "externe" Endpunkte Traffic Manager hinzugefügt werden. Diese externe Endpunkte werden als Azure Endpunkte externe Rate berechnet.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kann ich mit der Cloud-Dienst "Staging" Traffic Manager verwenden?

Ja. Cloud-Dienst die staging-Steckplätze kann in Traffic Manager als externer Endpunkte konfiguriert werden. Gesundheitskontrolle sind noch berechnet der Azure-Endpunkte. Da externe Endpunkttyp verwendet wird, sind Änderungen an den zugrunde liegenden Dienst nicht automatisch übernommen. Mit externer Endpunkte kann Traffic Manager erkennen, Cloud-Dienst angehalten oder gelöscht wird. Daher fortgesetzt Traffic Manager Fakturierung Gesundheitskontrolle der Endpunkt deaktiviert oder gelöscht wird.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Unterstützt Traffic Manager IPv6-Endpunkte?

Traffic Manager bietet derzeit keine IPv6-addressible Namenserver. Traffic Manager kann jedoch weiterhin von IPv6-Clients mit der IPv6-Endpunkte verwendet werden. Ein Client macht DNS-Anfragen direkt an Traffic Manager. Stattdessen verwendet der Client einen rekursive DNS-Dienst. Ein reine IPv6-Client sendet Anfragen an die rekursive DNS-Dienst über IPv6. Dann sollte rekursive Service mit IPv4 Traffic Manager Namenserver zu kontaktieren.

Traffic Manager antwortet mit der DNS-Name des Endpunkts. Zur Unterstützung von IPv6-Endpunkt muss eine IPv6-Adresse Endpunkt DNS-Namen auf DNS AAAA-Eintrag vorhanden sein. Traffic Manager Gesundheitskontrolle unterstützt nur IPv4-Adressen. Der Dienst muss einen IPv4-Endpunkt auf denselben DNS-Namen verfügbar machen.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Kann ich mit mehreren Web App in derselben Region Traffic Manager verwenden?

Traffic Manager wird meist direkten Verkehr Anwendung in verschiedenen Regionen bereitgestellt. Jedoch können auch Hiermit eine Anwendung mehr als eine Bereitstellung in derselben Region hat. Traffic Manager Azure Endpunkte lässt nicht mehrere Web App Endpunkte aus dem gleichen Azure dasselbe Profil Traffic Manager hinzugefügt werden.

Die folgenden Abschnitte zur Umgehung dieser Einschränkung:

1. Überprüfen Sie, ob Ihre Endgeräte in anderen Web app "Maßeinheiten". Ein Domänenname muss einen einzigen Standort in bestimmten Mengen-Einheit zuordnen. Daher kann nicht zwei Web-Apps in der gleichen Skala Traffic Manager-Profil verwenden.
2. Jede Web App DomainName personalisiertes als benutzerdefinierte Hostnamen hinzufügen. Jede Web App müssen unterschiedliche Mengen-Einheit. Alle Web-Apps müssen dieselbe Abonnement gehören.
3. Hinzufügen einer (und einzigen) Web App-Endpunkt zu Ihrem Profil Traffic Manager als Azure Endpunkt.
4. Jeder zusätzliche Web App Endpunkt der Traffic Manager Profil fügen Sie als externe Endpunkt hinzu. Mit dem Ressourcen-Manager-Bereitstellungsmodell können nur externe Endpunkte hinzugefügt werden.
5. Erstellen von DNS CNAME-Eintrag in der personalisiertes Domäne, die auf Ihren Profilnamen DNS-Traffic Manager zeigt (<>.... trafficmanager.net).
6. Zugriff auf die Website mit den personalisiertes Domänennamen nicht Traffic Manager Profil DNS-Namen.

## <a name="next-steps"></a>Nächste Schritte

- Funktionsweise [Traffic Manager](traffic-manager-how-traffic-manager-works.md).
- Informationen Sie zu Traffic Manager [Endpunkt überwachen und automatisches Failover](traffic-manager-monitoring.md).
- Lernen Sie Traffic Manager [Verteilermethoden Datenverkehr](traffic-manager-routing-methods.md).
