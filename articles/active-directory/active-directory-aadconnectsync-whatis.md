<properties
    pageTitle="Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen | Microsoft Azure"
    description="Erläutert, wie Azure AD verbinden Synchronisieren von Works und anpassen."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Verbindung synchronisieren: verstehen und Synchronisation anpassen
Azure Active Directory verbinden Synchronization Services (Azure AD Connect Synchronisierung) ist eine zentrale Komponente von Azure AD verbinden. Er übernimmt alle Vorgänge, die zum Synchronisieren von Daten zwischen der lokalen Umgebung und Azure AD zusammenhängen. Azure AD Connect Sync ist der Nachfolger DirSync Azure AD synchronisieren und Forefront Identity Manager mit dem Azure Active Directory Connector konfiguriert.

Dieses Thema ist die Homepage für **Azure AD Connect Sync** (sogenannte **Synchronisierungsmodul**) sowie Links zu anderen zugehörigen Themen. Links zu Azure AD Connect finden Sie in der [lokalen Benutzeridentitäten Azure Active Directory integrieren](active-directory-aadconnect.md).

Synchronisierungsdienst besteht aus zwei Komponenten, die lokalen **Azure AD verbinden synchronisieren** und die Dienstseite in Azure AD **Azure AD Connect-Synchronisierungsdienst**aufgerufen. Der Dienst ist für DirSync Azure AD synchronisieren und Azure AD verbinden.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect Sync Themen

Thema | Was umfasst und beim Lesen
----- | -----
**Azure AD Connect Sync-Grundlagen** |
[Verständnis der Architektur](active-directory-aadconnectsync-understanding-architecture.md) | Für diejenigen, die mit dem Synchronisierungsmodul und die Architektur und die verwendeten Begriffe erfahren möchten.
[Technische Begriffe](active-directory-aadconnectsync-technical-concepts.md) | Eine Kurzversion des Themas Architektur und kurz erläutert die Begriffe verwendet.
[Topologien für Azure AD verbinden](active-directory-aadconnect-topologies.md) | Beschreibt die verschiedenen Topologien und Szenarien das Synchronisierungsmodul unterstützt.
**Konfiguration** |
[Der Installationsassistent ausführen erneut](active-directory-aadconnectsync-installation-wizard.md) | Erläutert, welche Optionen stehen bei Azure AD Connect-Installations-Assistenten erneut ausführen.
[Deklarative Bereitstellung verstehen](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| Beschreibt die Konfiguration deklarative Bereitstellung bezeichnet.
[Grundlegendes zur Bereitstellung deklarative Ausdrücken](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | Beschreibt die Syntax für die deklarative Bereitstellung verwendete Ausdruckssprache.
[Verstehen der Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md)| Beschreibt die Out-of-Box-Regeln und die Standardkonfiguration. Beschreibt die Regeln für die Out-of-Box-Szenarien zu zusammenwirken.
[Grundlegendes zu Benutzern und Kontakten](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Das vorherige Thema fort und beschreibt, wie die Konfiguration für Benutzer und Kontakte, insbesondere in einer Umgebung mit mehreren Gesamtstrukturen funktioniert.
[Wie Sie die Konfiguration ändern](active-directory-aadconnectsync-change-the-configuration.md) | Führt Sie durch die allgemeine Konfiguration Attribut zu ändern.
[Best Practices für die Konfiguration ändern](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Eingeschränkte Unterstützung und für die Änderung der Out-of-Box-Konfigurations.
[Filterung](active-directory-aadconnectsync-configure-filtering.md) | Beschreibt die verschiedenen Optionen zum Einschränken der Objekte synchronisiert Azure AD und schrittweise wie diese Optionen konfiguriert werden.
**Features und Szenarios** |
[Versehentliche Löschen sperren](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | Beschreibt die Funktion *verhindert versehentliche Löschen* und konfigurieren.
[Planer](active-directory-aadconnectsync-feature-scheduler.md) | Der integrierte Taskplaner importieren, synchronisieren und Exportieren von Daten beschrieben.
[Synchronisierung von Kennwörtern implementieren](active-directory-aadconnectsync-implement-password-synchronization.md) | Beschreibt die Funktionsweise Kennwortsynchronisation, Implementierung und Betrieb und Problembehandlung.
[Gerät Rückschreiben](active-directory-aadconnect-feature-device-writeback.md) | Beschreibt die Funktionsweise von Gerät Rückschreiben in Azure AD verbinden.
[Verzeichnis extensions](active-directory-aadconnectsync-feature-directory-extensions.md) | Beschreibt das Erweitern des Schemas Azure AD eigene benutzerdefinierte Attribute.
**Sync-Diensts** |
[Azure AD Connect Service Synchronisierungsfunktionen](active-directory-aadconnectsyncservice-features.md) | Beschreibt die Dienstseite Sync und Sync in Azure AD ändern.
[Doppeltes Attribut Stabilität](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | Beschreibt das Aktivieren und verwenden **UserPrincipalName** und **ProxyAddresses** Doppeltes Attribut Werte Stabilität.
**Vorgänge und Benutzeroberfläche** |
[Service-Synchronisierung](active-directory-aadconnectsync-service-manager-ui.md) | Synchronisierung Service Manager-UI, einschließlich [Operationen](active-directory-aadconnectsync-service-manager-ui-operations.md), [Connectors](active-directory-aadconnectsync-service-manager-ui-connectors.md) [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)und [Metaverse](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) Registerkarten beschrieben.
[Aufgaben und Aspekte](active-directory-aadconnectsync-operations.md) | Betriebliche Bedenken, wie Wiederherstellung beschrieben.
**So wird es gemacht...** |
[Azure AD-Konto zurücksetzen](active-directory-aadconnectsync-howto-azureadaccount.md) | Wie Sie die Anmeldeinformationen des Dienstkontos zum Verbinden von Azure AD Connect synchronisieren mit Azure AD zurücksetzen.
**Weitere Informationen und Referenzen** |
[Anschlüsse](active-directory-aadconnect-ports.md) | Listet die Ports dem Synchronisierungsmodul für lokale Verzeichnisse und Ihre Azure AD zwischen müssen.
[Attribute mit Azure Active Directory synchronisiert.](active-directory-aadconnectsync-attributes-synchronized.md) | Listet alle Attribute zwischen lokalen AD und Azure AD synchronisiert.
[Funktionen-Referenz](active-directory-aadconnectsync-functions-reference.md) | Listet alle Funktionen deklarative Bereitstellung.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
