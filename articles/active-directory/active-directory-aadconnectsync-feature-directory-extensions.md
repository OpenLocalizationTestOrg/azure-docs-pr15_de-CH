<properties
   pageTitle="Azure AD Verbindung synchronisieren: Verzeichnis Extensions | Microsoft Azure"
   description="Dieses Thema beschreibt die Verzeichnisfunktion Extensions in Azure AD verbinden."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Verbindung synchronisieren: Verzeichnis Extensions
Verzeichnis Extensions können Sie das Schema in Azure AD mit eigener Attribute aus dem lokalen Active Directory erweitern. Dadurch verbrauchen Sie weiterhin lokale Attribute Branchen-apps erstellen. Diese Attribute können über [Azure AD Graph Verzeichnis Extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) oder [Microsoft Graph](https://graph.microsoft.io/)genutzt werden. Sie können Attribute mit [Azure AD Graph-Explorer](https://graphexplorer.cloudapp.net) und [Microsoft Graph-Explorer](https://graphexplorer2.azurewebsites.net/) anzeigen.

Derzeit belegt keine Office 365-Auslastung diese Attribute.

Sie konfigurieren, welche zusätzlichen Attribute Sie benutzerdefinierte Pfad im Installationsassistenten synchronisieren möchten.
![Schema-Erweiterung Assistenten](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) die Installation zeigt die folgenden Attribute gültig sind:

- Benutzer- und Objekttypen
- Attribute einwertig: String, Boolean, Integer, Binary
- Mehrwertige Attribute: String, Binary

Die Liste der Attribute wird aus dem Cache erstellt während der Installation von Azure AD Connect gelesen. Wenn Sie Active Directory-Schema zusätzliche Attribute erweitert haben, das [Schema muss aktualisiert werden,](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) bevor diese neue Attribute sind sichtbar.

Ein Objekt kann bis zu 100 Verzeichnisattribute Extensions aufweisen. Die maximale Länge beträgt 250 Zeichen. Wenn ein Attributwert länger ist, wird es durch das Synchronisierungsmodul abgeschnitten.

Während der Installation von Azure AD Connect ist eine Anwendung registriert, wenn diese Attribute verfügbar sind. Sie können diese Anwendung in Azure-Portal anzeigen.  
![Schema-Erweiterung App](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Diese Attribute sind jetzt verfügbar über Diagramm:  
![Diagramm](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Die Attribute vorangestellt Erweiterung\_{AppClientId}\_. Der AppClientId hat den gleichen Wert für alle Attribute im Azure AD-Verzeichnis.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
