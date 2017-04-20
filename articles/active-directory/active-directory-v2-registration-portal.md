<properties
    pageTitle="App-Registrierung Portal Hilfethemen | Microsoft Azure"
    description="Eine Beschreibung der verschiedenen Features in Microsoft app-registrierungsportal."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="app-registration-reference"></a>App-Registrierung Verweis
Dieses Dokument enthält Kontext und eine Beschreibung der verschiedenen Features in Microsoft App-Registrierungsportal [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)gefunden.

## <a name="my-applications"></a>Meine Programme
Diese Liste enthält alle Ihre Anwendung für die Verwendung mit dem Azure AD v2. 0-Endpunkt registriert.  Diese Programme können sich Benutzer sowohl persönliche Konten von Microsoft-Konto und arbeiten Schule Konten Azure Active Directory.  Erfahren Sie mehr über Azure AD v2. 0-Endpunkt finden Sie unsere [Übersicht v2. 0](active-directory-appmodel-v2-overview.md).  Anwendung können auch zur Integration mit Microsoft-Konto Authentifizierung Endpunkt `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK-Applikationen
Diese Liste enthält alle Ihre Anwendung ausschließlich mit Microsoft-Konto registriert.  Sie sind nicht für die Verwendung mit Active Directory Azure immer aktiviert.  Wo Sie Programme finden, die zuvor mit der MSA-Entwicklerportal auf registriert hat `https://account.live.com/developers/applications`.  Alle Funktionen, die Sie zuvor durchgeführt `https://account.live.com/developers/applications` können nun in das neue Portal ausgeführt `https://apps.dev.microsoft.com`.  Wenn Sie Fragen zu Ihrem Microsoft-Konto haben, kontaktieren Sie uns.

## <a name="application-secrets"></a>Anwendung Geheimnisse
Anwendung Geheimnisse sind Anmeldeinformationen, mit die die Anwendung zuverlässig [Clientauthentifizierung](http://tools.ietf.org/html/rfc6749#section-2.3) mit Azure ausführen können.  OAuth & OpenID Herstellen einer Anwendung Geheimnisse wird gemeinhin als ein `client_secret`.  V2. 0-Protokoll, jede Anwendung, die ein Sicherheitstoken adressierbare Webspeicherort empfängt (mit einer `https` Schema) verwenden einen geheimen Schlüssel selbst bei Azure AD auf Rückzahlung dieser Sicherheitstoken identifizieren.  Außerdem native Client, empfängt Token auf einem Gerät mit einem geheimen Schlüssel Client-Authentifizierung, gegen die Speicherung der Geheimnisse in unsicheren Umgebung verboten.

Jede Anwendung kann zwei gültige Anwendung Geheimnisse jederzeit enthalten.  Zwei Kennwörter verwalten, können Sie Fähigkeit periodische Key Rollover über die gesamte Umgebung der Anwendung ausführen.  Nach der Migration der Gesamtheit der Anwendung einen neuen geheimen möglicherweise den alten Schlüssel löschen und eine neue Bereitstellung.

Zu diesem Zeitpunkt sind nur zwei Arten von Anwendung Geheimnisse im Portal Registrierung app zulässig.  Wählen **Neues Kennwort generieren** generiert und speichert einen gemeinsamen geheimen Schlüssel im Speicher Daten in Ihrer Anwendung verwenden können.  **Neues Schlüsselpaar generieren** auswählen erstellt ein neues öffentliches/privates Schlüsselpaar, das heruntergeladen und für die Clientauthentifizierung Azure AD verwendet werden kann.

## <a name="profile"></a>Profil
Profilabschnitt der Portal-Registrierung app kann verwendet werden, die Anmeldeseite der Anwendung anpassen.  Zu diesem Zeitpunkt können Sie die Anmeldung Seite Anwendung Logo, Dienst-URL und Datenschutz ändern.  Das Logo muss ein transparentes Bild eines 48 x 48 oder 50 x 50 Pixel in eine GIF-, PNG- oder JPEG-Datei 15 KB oder kleiner.  Versuchen Sie, die Werte ändern und Anzeigen der resultierenden Zeichens auf.

## <a name="live-sdk-support"></a>Live SDK-Unterstützung
Wenn "Live SDK Support" aktivieren, werden erstellen Anwendung vertrauliche Daten in Azure AD bereitgestellt und Microsoft Account Datenspeicher.  Dadurch wird die Anwendung direkt mit dem Dienst Microsoft Account (dann) integrieren.  Wenn Sie eine Anwendung mithilfe von Microsoft Account direkt (anstelle von Azure AD v2. 0-Endpunkt) erstellen möchten, sollten Sie sicherstellen, dass Live SDK unterstützt wird.

Live SDK-Unterstützung deaktiviert wird sichergestellt, dass der geheimen Schlüssel nur in Azure AD-Daten geschrieben wird gespeichert.  Azure AD-Daten Speicher enthält Unternehmensklasse Vorschriften, die bestimmte Normen wie FISMA zugelassen.  Live SDK-Unterstützung aktivieren, kann die Anwendung nicht Einhaltung dieser Standards erreichen.

Wenn Sie nur den Azure AD v2. 0-Endpunkt verwenden möchten, können Sie problemlos Live SDK-Unterstützung deaktivieren.

