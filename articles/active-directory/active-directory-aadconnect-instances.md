<properties
    pageTitle="Azure AD Connect: Dienstinstanzen synchronisieren | Microsoft Azure"
    description="Diese Seite beschreibt Besonderheiten bei Azure Instanzen."
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
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Besonderheiten bei Instanzen
Azure AD Connect wird am häufigsten mit der weltweit Instanz von Azure AD und Office 365. Aber auch andere Instanzen vorhanden sind und diese müssen verschiedene URLs und andere besondere Hinweise.

## <a name="microsoft-cloud-germany"></a>Microsoft-Cloud-Deutschland
[Microsoft-Cloud Deutschland](http://www.microsoft.de/cloud-deutschland) ist eine souveräne Cloud von einem deutschen betrieben.

Diese Cloud ist derzeit in der Vorschau. Viele der Szenarios, wie gewohnt können Domänen überprüfen, vom Operator vorgenommen werden. Wenden Sie sich an Ihren Microsoft-Vertriebsmitarbeiter für Weitere Informationen in der Vorschau an.

URLs in Proxyserver öffnen |
--- |
\*. microsoftonline.de |
\*. Windows |
+ Zertifikatssperrlisten |

Beim Anmelden Azure AD-Verzeichnis verwenden Sie ein Konto in der Domäne onmicrosoft.de.

Funktionen derzeit nicht in der Microsoft-Cloud-Deutschland:

- Azure AD Connect Health ist nicht verfügbar.
- Automatische Updates ist nicht verfügbar.
- Kennwort Rückschreiben ist nicht verfügbar.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government cloud
[Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/) ist eine Cloud für die US-Regierung.

Diese Cloud wird von früheren Versionen von DirSync unterstützt. Generierung der Cloud wird von Azure AD Connect nächsten Build 1.1.180 unterstützt. Diese ist nur in den USA basierend Endpunkte verwenden und eine andere Liste von URLs in Ihren Proxy-Server öffnen.

URLs in Proxyserver öffnen |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
+ Zertifikatssperrlisten |

Azure AD Connect werden nicht automatisch erkennen können, dass Ihre Azure AD-Verzeichnis in der Cloud Regierung befindet. Stattdessen müssen Sie Aktionen bei der Installation von Azure AD verbinden.

1. Starten Sie auf Azure AD verbinden.
2. Als die erste Seite angezeigt, wo Sie den EULA akzeptieren sollen, nicht weiter zu behalten der Installations-Assistent ausgeführt.
3. Starten Sie Regedit, und ändern Sie den Registrierungsschlüssel `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` auf den Wert `2`.
4. Stimmen Sie dem EULA zu Azure AD Connect-Installationsassistenten und gehen. Während der Installation müssen Sie den Installationspfad **Konfiguration** verwenden (und nicht Installation). Fahren Sie die Installation wie gewohnt.

Funktionen derzeit nicht in der Microsoft Azure Government Cloud:

- Azure AD Connect Health ist nicht verfügbar.
- Automatische Updates ist nicht verfügbar.
- Kennwort Rückschreiben ist nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
