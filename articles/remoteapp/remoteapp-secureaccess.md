
<properties 
    pageTitle="Sichern des Zugriffs auf Azure RemoteApp und darüber hinaus | Microsoft Azure"
    description="Erfahren Sie, wie sicher auf Azure RemoteApp mit Zugangskontrolle in Azure Active Directory"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Sichern des Zugriffs auf Azure RemoteApp und darüber hinaus

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

In diesem Artikel geben wir einen Überblick darüber, wie Zugriff Kanal des Endbenutzers über Azure RemoteApp bis eine sichere Ressource eine SQL-Datenbank oder einer anderen Anwendung Backend-Administrator einrichten kann. Soll sicherstellen, dass nur autorisierte Benutzer die gewünschten Anforderungen Remoteanwendungen zugreifen können und sichere Back-End nur von Azure RemoteApp kontrollierten und nicht von anderen Speicherorten zugegriffen werden kann.

Es gibt 3 Hauptbereiche, die, denen der Administrator muss:

![Azure RemoteApp Zugangskontrolle Aspekte](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Lesen von Informationen und Antworten auf diese Fragen.

## <a name="who-can-access-the-collection"></a>Die die Auflistung zugreifen können?
Der Administrator wählt der Benutzer die Remoteanwendungen in der Auflistung zugreifen können. Sie können Active Directory Azure (Azure AD) Arbeit oder Schule (früher, "Organisationseinheiten Konten") oder Microsoft Konten (z. B. @outlook.com). Die meisten Unternehmensszenarios verwenden Azure AD-Konten. Sie können Sie bedingte Access Features später erläutert und auch die einzige Option für Domäne Sammlungen. Der Rest dieses Artikels wird vorausgesetzt, dass mithilfe von Azure AD-Konten Azure RemoteApp.

**Was wir erreicht haben:**

Azure AD-Konten können zum Azure RemoteApp Zugriffskontrolle wir zwei Dinge:

1.  Wissen wir immer, die wir veröffentlicht haben und Zugriff auf die Anwendung zugreifen können Back-Ends Anwendungsbereiche herstellen.
2.  Wir steuern das zugrunde liegenden Azure AD wir erstellen bzw. löschen Benutzerkonten Kennwortrichtlinien festlegen, mehrstufige Authentifizierung usw.. 

## <a name="how-is-the-collection-accessed-from-where"></a>Wie wird auf die Auflistung zugegriffen? Von wo?
Häufig Administratoren Richtlinien für den Zugriff auf einen öffentlichen Internetzugriff Umgebung wie Azure RemoteApp definieren möchten. Sie möchten z. B. sicherstellen, dass Benutzer den Zugriff auf die Umgebung von außerhalb des Unternehmensnetzwerks mehrstufige Authentifizierung (MFA) verwenden. oder vielleicht sie vollständig blockiert werden soll.

Azure AD Premium verfügbaren Funktionen können Azure RemoteApp Administratoren bedingte Richtlinien für ihre Azure RemoteApp-Umgebung festgelegt. Sie können auch umfangreiche Berichts- und Warnmerkmale überwachen, wie die Umgebung zugegriffen wird.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Zugangskontrolle für Azure RemoteApp einrichten
Wir werden ein Beispielszenario – Azure RemoteApp durchlaufen Administrator Zugriff auf die Umgebung, wenn Benutzer außerhalb des Unternehmensnetzwerks möchte.

>[AZURE.NOTE] Aktualisierung von Azure AD auf die Premium-Stufe und erstellen mindestens eine Azure RemoteApp-Auflistung angenommen.

1.  Klicken Sie auf die Registerkarte **Active Directory** in Azure-Portal. Klicken Sie auf das Verzeichnis, das Sie konfigurieren möchten.

    Denken Sie daran: Zugangskontrolle ist eine Eigenschaft des Verzeichnisses und Azure RemoteApp nicht so die Konfiguration auf Verzeichnisebene erfolgt. Dies bedeutet auch, Sie müssen Directory Administrator zu gewährleisten.

2.  Klicken Sie auf **Programme**und dann auf **Microsoft Azure RemoteApp** Zugangskontrolle einzurichten. Beachten Sie, dass Sie Zugangskontrolle für jede Anwendung "Software als Dienst" in Ihrem Verzeichnis separat einrichten können.
![Zugangskontrolle einrichten für Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
 

3.  **Zugriffsregeln können** auf der Registerkarte **Konfigurieren** auf ON festgelegt.
![Zugriffsregeln für Azure RemoteApp aktivieren](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
 

4.  Jetzt können Sie verschiedene Regeln konfigurieren und wählen, die sie anwenden:

    1. Wählen Sie vollständig verhindern den Zugriff auf Azure RemoteApp außerhalb der angegebenen Netzwerk **Zugriff nicht bei der Arbeit** .
    2. Klicken Sie auf die Option folgende IP-Adressbereiche definieren, die "Unheil" bilden. Alles außerhalb wird abgelehnt.

5.  Testen Sie die Konfiguration mit dem Azure RemoteApp-Client eine IP-Adresse außerhalb des angegebenen Bereichs. Nachdem Sie Ihre Azure AD-Anmeldeinformationen anmelden sollte eine Meldung angezeigt werden:

![Zugriff verweigert auf Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)
 

### <a name="future-conditional-access-features"></a>Zukünftige bedingten Zugriff 
Azure Active Directory-Team arbeitet an neuen Funktionen in bedingten Zugriff. Administratoren können neue Regeln über Netzwerk Speicherort Regeln erstellen. Eine öffentliche Vorschau der neuen Funktionalität sollte bald verfügbar sein.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Zugriff auf Azure RemoteApp überwachen
Ein großartiges Feature neben Zugangskontrolle ist Azure Active Directory Premium Funktion. Sie können Berichte überwachen Sie, wer Zugriff auf Ihre Umgebung und verdächtige Aktivitäten zu erkennen.

Beispielsweise die Namen von Azure RemoteApp wie viele Zugriffe sie es Taten Benutzern angezeigt und wann.

1.  In Azure-Portal auf **Active Directory**und klicken Sie dann auf das Verzeichnis.

2.  Gehen Sie auf die Registerkarte **Berichte** .

3.  Wählen Sie aus der Liste der Berichte **Anwendung** unter **Integrated Applications**.

    Sie sehen einige aggregierten Statistiken für Azure RemoteApp. 
![Aggregierte Statistiken von Azure RemoteApp Zugriff](./media/remoteapp-secureaccess/ra-accessstats.png)
 
5.  Klicken Sie auf die Anwendung Informationen über Benutzer Azure RemoteApp.
![Benutzer Zugriff auf Statistiken für Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)
 
### <a name="summary"></a>Zusammenfassung
Mit Azure Active Directory können Sie Zugriffsregeln Azure RemoteApp (und andere Software als einen Dienst Applikationen über Azure AD) einrichten. Regeln sind derzeit auf Standort Netzwerkrichtlinien jedoch andere Aspekte des Enterprise-Management in Zukunft erweitert werden.

Azure AD Premium bietet reporting und Überwachungsfunktionen, die dem Steuerelement den Admin erweitert über ihre Azure RemoteApp-Umgebung.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Wie sicherstellen kann ich, dass meine sichere Ressource nur über meine Umgebung Azure RemoteApp wird?
In vorherigen Abschnitten dieses Artikels konzentriert Absicherung von Azure RemoteApp-Umgebung. Wir haben bewerkstelligt, die Benutzern Zugriff auswählen und Einrichten von Zugriffsregeln zu steuern, wie sie den Dienst verwenden.

Ein häufiges Szenario für die Bereitstellung von Azure RemoteApp ist, dass die Remoteanwendungen Kommunikation mit Back-End-Ressource, z. B. eine SQL-Datenbank. Diese Ressource gehostet wird entweder lokal (z. B. eines Firmennetzwerks) oder in der Cloud (z. B. in Azure IaaS). Oft möchten Administratoren sicherstellen, dass die Back-End-Ressource nur von Clientanwendungen über Azure RemoteApp bereitgestellt und nicht von einer Anwendung auf dem PC eines Benutzers ausführen und über Internet zugreifen zugegriffen werden kann. Azure RemoteApp wird häufig als Umgebung zentral verwaltet und gesichert und damit der einzige Pfad, über den Benutzer mit Back-End-Ressourcen interagieren soll.

Die Lösung ist der Azure RemoteApp-Umgebung und sichere Ressource in der gleichen Azure Virtual Network (VNET) platziert. Wenn die Ressource an einem anderen Standort, können Sie eine Standort-zu-Standort-VPN-Verbindung z. B. in einem VNet in der Azure-Rechenzentrum und die Umgebung des Kunden vor Ort einrichten.

Azure RemoteApp unterstützt zwei Arten der Auflistung Bereitstellung, eigene VNET bereitstellen können:

-   Nicht-Domäne: die Anwendung "Sichtfeld" anderen Ressourcen die VNET haben. Dies kann z. B. verwendet werden, Applikationen mit einer SQL-Datenbank herstellen, die SQL-Authentifizierung (Applications Benutzerauthentifizierung direkt gegen die Datenbank)

-   Domäne: von Azure RemoteApp verwendete virtuelle Computer auf einem Domänencontroller in der VNET angehören. Dies ist nützlich, wenn der Windows-Domänencontroller authentifizieren, um auf Back-End-Ressource zugreifen muss.
![Domäne Auflistung in Azure RemoteApp](./media/remoteapp-secureaccess/ra-domainjoined.png)
 
### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Zum Erstellen einer sicheren Verbindungs zwischen Azure und meinem lokalen Umgebung
Es stehen verschiedene Konfigurationsoptionen für die Verbindung Ihrer Azure und lokalen Umgebung. Hier ist ein Überblick über die Optionen verfügbar.

Azure RemoteApp müssen Sie zuerst das VNet konfigurieren und dann bei der Erstellung der Auflistung verwenden. 

## <a name="the-complete-solution"></a>Die vollständige Lösung
Das Diagramm unten zeigt die vollständige Lösung, haben wir einen sicheren Zugriff Kanal des Endbenutzers über Azure RemoteApp (ARA) in die Back-End-Ressourcen aufgebaut.
![Sichern von Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) In Phase 1 wir den Benutzer und erstellt Regeln, die bestimmen, wie ARA zugegriffen werden kann. Im folgenden Beispiel wir nur Zugriff für Benutzer im Unternehmensnetzwerk. Nicht kompatible Benutzer können überhaupt auf die ARA-Umgebung nicht.
In "Phase 2" haben wir die Back-End-Ressource nur über die VNet-VPN-Konfiguration ausgesetzt, die wir steuern. Azure RemoteApp wurde in derselben VNet platziert. Das Endergebnis ist, dass die Ressource nur über die ARA-Umgebung zugegriffen werden kann.


