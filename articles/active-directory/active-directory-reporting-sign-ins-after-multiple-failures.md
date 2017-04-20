<properties
    pageTitle="Anmeldungen nach mehreren Fehlern"
    description="Ein Bericht, der Benutzer gibt an, wer angemeldet haben nach mehreren aufeinander folgenden Zeichen in Versuche fehlgeschlagen."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-after-multiple-failures"></a>Anmeldungen nach mehreren Fehlern
Dieser Bericht zeigt Benutzer angemeldet haben nach mehreren aufeinander folgenden Zeichen in Versuche fehlgeschlagen. Mögliche Ursachen:

- Benutzer haben ihr Kennwort vergessen.</li><li>Benutzer ist das Opfer einer erfolgreichen Kennwort erraten brute-force-Angriff

Ergebnisse in diesem Bericht werden die Anzahl der aufeinander folgenden fehlgeschlagenen Anmeldeversuche vor der erfolgreichen Anmeldung vorgenommen und der ersten erfolgreichen Anmelden zugeordnete Zeitstempel angezeigt.

**Einstellungen**: konfiguriert die minimale Anzahl von aufeinander folgenden fehlgeschlagenen Anmeldung Anmeldeversuche erfolgen muss, bevor er im Bericht angezeigt werden kann. Wenn Sie diese Einstellung ändern ist zu beachten, dass diese Änderung keine vorhandenen fehlgeschlagene Anmeldung ins, angezeigt in Ihr bereits vorhandener Bericht angewendet werden. Jedoch wird auf allen zukünftigen Anmeldungen angewendet werden. Dieser Bericht kann nur von lizenzierten Administratoren geändert werden.


![Anmeldungen nach mehreren Fehlern](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)
