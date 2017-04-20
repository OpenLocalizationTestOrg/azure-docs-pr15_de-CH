<properties
    pageTitle="Azure Active Directory hybride Identität Vorüberlegungen - definieren Sie eine hybride Identität Annahme Strategie | Microsoft Azure"
    description="Mit bedingten Zugriffskontrolle überprüft Azure Active Directory die besonderen Umständen, wenn Benutzer authentifizieren, bevor Zugriff auf die Anwendung auswählen. Die Suchkriterien erfüllt, wird der Benutzer authentifiziert und Zugriff auf die Anwendung."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-a-hybrid-identity-adoption-strategy"></a>Definieren Sie eine hybride Identität Annahme Strategie

In dieser Aufgabe definieren Sie hybride Identität Annahme Strategie für Ihre hybrididentitätslösung den Bedürfnissen in behandelten:

- [Ermitteln der Geschäftsbedürfnisse](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [Directory Synchronization Anforderungen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [Mehrstufige Authentifizierung ermitteln](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Definieren der Strategie muss
Die ersten Aufgabe Adressen bestimmen das Unternehmen Unternehmen muss.  Dies kann sehr breit und unterlaufen kann auftreten, wenn Sie nicht vorsichtig sind.  Am Anfang ganz einfach, aber stets einen Entwurf planen, die angepasst und Änderung in der Zukunft erleichtern.  Unabhängig davon, ob es ein einfaches Design oder eine äußerst komplex ist ist Active Directory Azure Microsoft Identity-Plattform, die Office 365, Microsoft Online Services und Cloud bewusst Programme unterstützt.

## <a name="define-an-integration-strategy"></a>Definieren einer Integrationsstrategie
Microsoft hat drei wichtigsten Integrationsszenarios cloudidentitäten, synchronisierte Identitäten und Identitätsverbund.  Sie sollten auf eine dieser Strategien Integration.  Die Strategie kann variieren und die Entscheidung bei der Auswahl gehören, welche Art von Benutzeroberfläche Sie bereitstellen möchten, müssen Sie einige der vorhandenen Infrastruktur bereits platzieren und was ist die kostengünstigste.  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

Die Szenarien in der obigen Abbildung definiert sind:

- **Cloud-Identitäten**: Identitäten, die ausschließlich in der Cloud vorhanden sind.  Bei Azure AD würden sie insbesondere in Azure AD-Verzeichnis befinden.
- **Synchronisiert**: Identitäten, die lokal vorhanden sind und in der Cloud.  Mit Azure AD verbinden, diese Benutzer erstellt oder vorhandene Azure AD-Konten verknüpft.  Kennworthash des Benutzers wird aus der lokalen Umgebung in der Cloud in der Kennworthash synchronisiert.  Synchronisiert mit wird eine Einschränkung, wenn ein Benutzer in der lokalen Umgebung deaktiviert, bis zu 3 Stunden, Kontodaten in Azure AD angezeigt nutzen können.  Das Synchronisierungsintervall Zeit liegt.
- **Verbundene**: diese Identitäten sowohl lokal vorhanden und in der Cloud.  Mit Azure AD verbinden, diese Benutzer erstellt oder vorhandene Azure AD-Konten verknüpft.  
 
>[AZURE.NOTE]
Weitere Informationen über die Synchronisierung Leseoptionen [Ihrer lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

In der folgenden Tabelle helfen bei der vor- und Nachteile der folgenden Strategien:

| Strategie         | Vorteile                                                                                                                                                                                                                                                  | Nachteile                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Cloudidentitäten** | Vereinfachte Verwaltung für Kleinunternehmen. <br> Installieren Sie lokal-keine zusätzlichen Hardware erforderlich<br>Problemlos deaktiviert werden, wenn der Benutzer das Unternehmen verlässt                                                                                                   | Benutzer müssen sich anmelden beim Zugriff auf Arbeitslasten in der cloud <br> Kennwörter oder nicht für Cloud und lokalen Identitäten                                                                                                                                                                                                                     |
| **Synchronisiert**     | Lokale Kennwort authentifiziert sowohl lokal und cloud-Verzeichnisse <br>Vereinfachte Verwaltung für kleine, mittlere und große Unternehmen <br>Benutzer können einmaliges Anmelden (SSO) für einige Ressourcen verfügen. <br> Microsoft bevorzugte Methode für die Synchronisierung <br> Einfacher zu verwalten | Einige Kunden möglicherweise nicht die Verzeichnisse mit der Cloud durch bestimmte Unternehmen Polizei synchronisieren                                                                                                                                                                                                                                                  |
| **Verbund**        | Benutzer können einmaliges Anmelden (SSO) <br>Wenn Benutzer beendet ist, bleibt das Konto kann sofort deaktiviert und Zugriff widerrufen<br> Unterstützt erweiterte Szenarios, nicht möglich, mit erreicht synchronisiert                                           | Weitere Schritte zur Einrichtung und Konfiguration <br> Höhere Wartung <br> Erfordert eventuell zusätzliche Hardware für die STS-Infrastruktur <br> Erfordert eventuell zusätzliche Hardware den Verbundserver zu installieren. Wenn AD FS verwendet wird, ist Weitere Software erforderlich <br> Erfordert umfangreiche Setup für SSO <br> Kritische Fehlerquelle unten ist der Verbundserver, nicht Benutzer authentifizieren können |

### <a name="client-experience"></a>Clientumgebung
Die Strategie, mit denen Sie bestimmen die Benutzer anmelden.  Die folgenden Tabellen enthalten Informationen über die Benutzer ihrer anmelden zu erwarten.  Beachten Sie, dass nicht alle Identitätsverbund Provider SSO in allen Szenarios unterstützen.

**Domäne hinzugefügt und die private Netzwerk-Applikationen**:
 

|                              | Synchronisierte Identität      | Federated Identity                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webbrowser                 | Formularbasierte Authentifizierung | Einmaliges, manchmal müssen Organisations-ID |
| Outlook                      | Anmeldeinformationen anfordern     | Anmeldeinformationen anfordern                                       |
| Skype for Business (Lync)    | Anmeldeinformationen anfordern     | Single Sign-On für Lync aufgefordert, Anmeldeinformationen für Exchange   |
| SkyDrive Pro                 | Anmeldeinformationen anfordern     | Einmaliges Anmelden                                               |
| Office Pro Plus Abonnement | Anmeldeinformationen anfordern     | Einmaliges Anmelden                                               |

**Externe oder nicht vertrauenswürdigen Quellen**:

|                              | Synchronisierte Identität      | Federated Identity                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webbrowser                 | Formularbasierte Authentifizierung |  Formularbasierte Authentifizierung |
| Outlook, Skype for Business (Lync) Skydrive Pro Office-Abonnement| Anmeldeinformationen anfordern     | Anmeldeinformationen anfordern                                       |
| Exchange ActiveSync    | Anmeldeinformationen anfordern     | Single Sign-On für Lync aufgefordert, Anmeldeinformationen für Exchange   |
| Mobiler apps                 | Anmeldeinformationen anfordern     | Anmeldeinformationen anfordern                                               |

Wenn Sie Aufgabe 1 festgestellt haben, dass Sie eine 3rd party IdP bzw. wird eine Föderation mit Azure zu verwenden, müssen Sie die folgenden Funktionen unterstützt:
- Alle SAML 2.0 ist für die SP-Lite Profil Azure AD-Authentifizierung unterstützen und zugeordnete Anwendung
- Passive Authentifizierung ermöglicht die Authentifizierung für OWA, SPO usw. unterstützt.
- Exchange Online-Clients können über die SAML 2.0 erweiterten Client Profile (ECP) unterstützt werden

Sie müssen auch wissen, welche Funktionen nicht verfügbar:

- WS-Trust/Verbund unterstützen bricht alle aktiven clients
 - Dies bedeutet kein Lync-Client, OneDrive Client, Office-Abonnement, Office Mobile vor Office 2016
- Übergang von Office passive Authentifizierung können reine SAML 2.0 vertriebenen unterstützen, jedoch unterstützt werden pro Client client


>[AZURE.NOTE]
Die aktuellste Liste finden Sie in Artikel http://aka.ms/ssoproviders.

## <a name="define-synchronization-strategy"></a>Definieren der Strategie für die Synchronisierung
Dabei definieren Sie die Tools zum Synchronisieren der Organisation lokalen Daten in der Cloud und welche Topologie Sie verwenden.  Da die meisten Unternehmen Active Directory verwenden, mit Azure AD Connect oben Fragen detaillierter Informationen.  Umgebungen, die nicht Active Directory gibt Informationen über FIM 2010 R2 oder MIM 2016 diese Strategie planen.  Zukünftige Versionen von Azure AD Connect unterstützt LDAP-Verzeichnissen so je nach Zeitplan jedoch diese Informationen möglicherweise helfen.

###<a name="synchronization-tools"></a>Synchronisierung
In den Jahren verschiedene Synchronisierungsprogramme vorhanden und für verschiedene Szenarios verwendet.  Azure AD Connect ist derzeit die Wahl bei allen unterstützten Szenarien.  AAD Sync DirSync weiterhin um und sind auch möglicherweise in Ihrer Umgebung jetzt. 

>[AZURE.NOTE]
Informationen über die unterstützten Funktionen der einzelnen Tools Artikel [Directory Integration Vergleich](active-directory-hybrid-identity-design-considerations-tools-comparison.md) .  

### <a name="supported-topologies"></a>Unterstützte Topologien
Beim Definieren einer Synchronisierungsstrategie muss die Topologie, die verwendet wird, bestimmt. Je nach der in Schritt ermittelte Informationen bestimmen 2 welche Topologie die richtige ist. Die Gesamtstruktur einzelne Azure AD Topology wird am häufigsten und besteht aus einer Active Directory-Gesamtstruktur und eine Instanz von Azure AD.  Dies wird in den meisten Szenarien verwendet und am erwarteten Verwendung von Azure AD verbinden Expressinstallation wie in der folgenden Abbildung dargestellt.
 
![](./media/hybrid-id-design-considerations/single-forest.png)Einzelne Gesamtstruktur Szenario ist es häufig für große und kleine Organisationen mehrere Gesamtstrukturen, wie in Abbildung 5 dargestellt.

>[AZURE.NOTE]
Weitere Informationen über die verschiedenen lokalen und Azure AD Topologien mit Azure AD Connect Artikel synchronisieren die [Topologien für Azure AD verbinden](active-directory-aadconnect-topologies.md).


![](./media/hybrid-id-design-considerations/multi-forest.png) 

Szenario mit mehreren Gesamtstrukturen

Wenn dies der Fall und multi-Gesamtstrukturen Single Azure AD Topology anzusehen, wenn Folgendes zutrifft:

- Benutzer haben nur 1 Identität in allen Gesamtstrukturen – eindeutig identifiziert Benutzer Abschnitt werden diese ausführlicher beschrieben.
- Die Benutzerauthentifizierung für die Gesamtstruktur, in der ihre Identität befindet
- Benutzerprinzipalnamen und Quellanker (unveränderliche Id) kommen aus dieser Gesamtstruktur
- Alle Gesamtstrukturen Azure AD Connect erreichbar – Dies bedeutet, dass es nicht hinzugefügt und kann in einer DMZ platziert werden, wenn diese Dies erleichtert werden muss.
- Benutzer haben nur ein Postfach
- Gesamtstruktur, die Postfach des Benutzers hat die besten Daten für Attribute in der Exchange globale Adressliste (GAL) angezeigt
- Ist kein Postfach für den Benutzer, kann dann jeder Gesamtstruktur verwendet werden, diese Werte
- Wenn Sie ein verknüpftes Postfach haben, dann gibt auch ein anderes Konto in einer anderen Gesamtstruktur angemeldet.

>[AZURE.NOTE]
Objekte, die sowohl lokal vorhanden und in der Cloud sind "verbunden" über einen eindeutigen Bezeichner. Im Rahmen der Verzeichnissynchronisation wird diese eindeutige ID der SourceAnchor genannt. Im Kontext von Single Sign-On ist dies die ImmutableId genannt. [Entwurfskonzepte für Azure AD verbinden](active-directory-aadconnect-design-concepts.md#sourceanchor) für Weitere Hinweise zur Verwendung des SourceAnchor.

Sind nicht true und Sie haben mehr als ein aktives Konto oder mehrere Postfächer wird Azure AD Verbinden wählen und ignoriert die anderen.  Wenn Sie Postfächer jedoch kein anderes Konto verknüpft haben, diese Konten nicht in Azure AD exportiert und dieser Benutzer nicht Mitglied einer Gruppe.  Dies unterscheidet wie mit DirSync war und ist beabsichtigt, um eine bessere Unterstützung für diese Szenarien mit mehreren Gesamtstrukturen. Die Abbildung zeigt ein Szenario mit mehreren Gesamtstrukturen.
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**Gesamtstrukturen mehrere Azure AD-Szenario**

Es wird empfohlen, nur ein einzelnes Verzeichnis in Azure AD für eine Organisation jedoch ist es eine 1:1-Beziehung zwischen einer Azure AD Connect Synchronisierungsserver und Azure AD-Verzeichnis gespeichert.  Für jede Instanz von Azure AD möchten Installation von Azure AD verbinden.  Auch Azure AD beabsichtigt ist isoliert und Benutzer in einer Instanz von Azure AD werden keine Benutzer in einer anderen Instanz.

Es ist möglich und unterstützt mehrere Azure AD-Verzeichnissen eine lokale Instanz von Active Directory herstellen, wie in der folgenden Abbildung dargestellt:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**Einzelne Gesamtstruktur Filterszenarios**

Dazu Folgendes muss zutreffen:

- Azure AD Connect Synchronisierungsserver müssen zum Filtern haben sie sich gegenseitig ausschließende Objektgruppe konfiguriert werden.  Dies, z. B. Bereiche jeder Server mit einer bestimmten Domäne oder Organisationseinheit.
- Eine DNS-Domäne kann nur in einem einzigen registriert Azure AD-Verzeichnis die UPNs der Benutzer in der lokalen AD muss separate Namespaces verwenden
- Benutzer in einer Instanz von Azure AD werden nur Benutzer aus ihrer Instanz sehen.  Sie werden können Benutzer in anderen Instanzen nicht
- Nur eines der Verzeichnisse Azure AD Exchange Hybrid mit lokalen kann AD
- Gegenseitigen Ausschließlichkeit gilt auch für zurückschreiben.  Dadurch werden einige Write-Back-Funktionen mit dieser Topologie nicht unterstützt, da diese eine einzige lokale Konfiguration übernehmen.  Dazu gehören:
 - Gruppe Rückschreib mit Standardkonfiguration
 - Write-Back-Gerät


Beachten Sie, dass Folgendes nicht unterstützt und sollte nicht als Implementierung:

- Mehrere Azure AD Connect Sync Server in Azure AD-Verzeichnis verbinden, selbst wenn sie gegenseitig Satz des Objekts Synchronisierung konfiguriert wird nicht unterstützt
- Es wird nicht unterstützt, um das gleiche Benutzerkonto an mehreren Azure AD-Verzeichnissen zu synchronisieren. 
- Es wird auch nicht unterstützt eine Konfiguration ändern, um Benutzern in einem Azure AD als Kontakte in einem anderen Azure AD-Verzeichnis angezeigt werden. 
- Es ist auch nicht unterstützte Azure AD Connect Sync Verbindung zu mehreren Azure AD-Verzeichnissen zu ändern.
- Azure AD-Verzeichnissen sind entwurfsbedingt isoliert. Es wird zum Ändern der Konfiguration von Azure AD Connect Synchronisierung zum Lesen von Daten aus einem anderen Azure AD-Verzeichnis in ein Versuch, eine gemeinsame und einheitliche GAL zwischen den Verzeichnissen erstellen nicht unterstützt. Wird auch zum Exportieren von Benutzern als Kontakte nicht einem anderen lokalen AD mit Azure AD-Verbindung synchronisieren.


>[AZURE.NOTE]
Ihrer Organisation Computer im Netzwerk auf das Internet beschränkt, listet dieser Artikel die Endpunkte (FQDNs IPv4- und IPv6-Adressbereichen), in sollen die ausgehende können Listen und Internet Explorer vertrauenswürdigen Sites Client Computer Computer können erfolgreich Office 365 sicherzustellen. Weitere Informationen finden Sie in [Office 365 URLs und IP-Adressbereiche](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

## <a name="define-multi-factor-authentication-strategy"></a>Mehrstufige Authentifizierungsstrategie definieren
Dabei definieren Sie die mehrstufige Authentifizierungsstrategie verwenden.  Azure mehrstufige Authentifizierung wird in zwei verschiedenen Versionen.  Eine ist eine cloudbasierte und anderen lokalen verwenden Azure MFA-Server ist.  Basierend auf der Auswertung Sie Sie ermitteln können die Lösung für Ihre Strategie richtig ist.  Anhand der folgenden Tabelle bestimmen die optimale Entwurfsoption Ihres Unternehmens sicherheitsanforderung erfüllen:

Mehrstufige Entwurfsoptionen:

| Sichere Anlage                                               | MFA in der cloud | Lokale MFA |
|---------------------------------------------------------------|------------------|----------------|
| Microsoft-apps                                                | Ja              | Ja            |
| SaaS-apps in der Galerie                                  | Ja              | Ja            |
| IIS-Anwendung über Azure AD App Proxy veröffentlicht         | Ja              | Ja            |
| IIS-Anwendung nicht über Azure AD App Proxy veröffentlicht | Nein               | Ja            |
| RAS-VPN-RDG                                     | Nein               | Ja            |

Obwohl Sie eine Lösung für Ihre Strategie ausgeglichen können, müssen Sie verwenden die Auswertung ab, wo sich die Benutzer befinden.  Dadurch kann die Projektmappe ändern.  Verwenden der Tabelle dies festzustellen:

| Speicherort                                                       | Bevorzugtes Design-option                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure Active Directory                                              | Multi-FactorAuthentication in der cloud |
| Azure AD und lokalen Föderation mit AD FS mit AD             | Beide                                    |
| Azure AD und lokale AD mit Azure AD Connect ohne Kennwort synchronisieren | Beide                                    |
| Azure AD und lokale Kennwort Sync Azure AD Verbinden mit  | Beide                                    |
| Lokale Anzeige                                                      | Mehrstufige Authentifizierungsserver      |

>[AZURE.NOTE]
Sie sollten auch sicherstellen, dass die kombinierten Authentifizierung Designoption Sie die Features unterstützt, die für den Entwurf.  Weitere Informationen finden Sie in [Wählen Sie die kombinierte Lösung für Sie](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).

## <a name="multi-factor-auth-provider"></a>Mehrstufige Authentifizierungsanbieter
Mehrstufige Authentifizierung ist standardmäßig für globale Administratoren Mieter Azure Active Directory verfügbar. Allerdings möchten Sie mehrstufige Authentifizierung aller Benutzer erweitern und Ihre globalen Administratoren zu nutzen Funktionen wie Verwaltungsportal, benutzerdefinierte Grußformeln und Berichte, müssen Sie erwerben und mehrstufige Authentifizierungsanbieter konfigurieren.

>[AZURE.NOTE]
Sie sollten auch sicherstellen, dass die kombinierten Authentifizierung Designoption Sie die Features unterstützt, die für den Entwurf. 

##<a name="next-steps"></a>Nächste Schritte
[Bestimmen des Datenschutzes](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Siehe auch
[Design Considerations (Übersicht)](active-directory-hybrid-identity-design-considerations-overview.md)
