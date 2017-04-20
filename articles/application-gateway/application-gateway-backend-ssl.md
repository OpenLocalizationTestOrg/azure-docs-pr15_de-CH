<properties
   pageTitle="Aktivieren der SSL-Richtlinien und Ende SSL auf Application Gateway | Microsoft Azure"
   description="Diese Seite Überblick Application Gateway Ende SSL unterstützen."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Aktivieren der SSL-Richtlinien und Ende SSL auf Application Gateway

## <a name="overview"></a>Übersicht

Application Gateway unterstützt SSL-Beendigung und nach der Datenfluss in der Regel auf die Back-End-Server unverschlüsselt. Dadurch können Server aus kostspielige Verschlüsselung/Entschlüsselung Overhead entlastet werden. Jedoch steht für einige Kunden unverschlüsselte Kommunikation mit den Back-End-Servern nicht zulässig. Sicherheits-Compliance-Gründen möglicherweise oder akzeptiert die Anwendung nur sichere Verbindung. Für diese Anwendung unterstützt Application Gateway jetzt Ende SSL-Verschlüsselung.

Gegen Ende SSL ermöglicht sicheren Übertragung von sensiblen Daten an den Back-End-verschlüsselte noch nutzen die Vorteile der Layer 7 Lastenausgleichsfunktionen das Application Gateway, wie cookieaffinität, URL-basierten routing, unterstützt routing basierend auf Websites oder X einfügen-weitergeleitete-* Header.

Konfiguriert mit Ende SSL-Kommunikation Application Gateway beendet die SSL-Sitzungen am Gateway als Benutzerdatenverkehr entschlüsselt. Es gilt die konfigurierten Regeln eine entsprechenden Back-End-Pool Instanz Datenverkehr zu aktivieren. Application-Gateway eine neue SSL-Verbindung zum Back-End-Server initiiert und erneut verschlüsselt Daten mit öffentliches Schlüsselzertifikat Back-End-Server vor dem Senden der Anforderung an den Back-End. Gegen Ende SSL aktivierte Protokoll Einstellung in BackendHTTPSetting auf Https dann ein Back-End-Pool angewendet wird. Jeder Datenbankserver im Back-End-Pool mit Ende SSL aktiviert muss mit einem Zertifikat, sichere Kommunikation konfiguriert.

![Ssl Ende-Szenario][1]

In diesem Beispiel werden Anfragen mit TLS1.2 Back-End-Server in Pool1 mit SSL Ende weitergeleitet.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Ende SSL und mithilfe von Zertifikaten

Application Gateway kommuniziert nur mit bekannten Backend-Instanzen mit weißen Liste ihres Zertifikats mit Application Gateway. Um mithilfe von Zertifikaten zu aktivieren, müssen Sie den öffentlichen Schlüssel des Back-End-Serverzertifikate Application Gateway (nicht das Stammzertifikat) hochladen. Nur Verbindungen zu bekannten und White Backends sind zulässig. Verbleibende Downloadzeit führt eine Gateway-Fehler. Selbstsignierte Zertifikate werden nicht empfohlen für Produktionsarbeitslasten Testzwecken. Die Bescheinigung muss auch White Application Gateway wie oben beschrieben verwendet werden kann.

## <a name="application-gateway-ssl-policy"></a>Application Gateway SSL-Richtlinien

Application Gateway unterstützt Benutzer konfigurierbare SSL Aushandlungsrichtlinien, die Kunden mehr Kontrolle über eine SSL-Verbindung am Anwendungsgateway ermöglichen.

1. SSL 2.0 und 3.0 für alle Anwendungsgateways standardmäßig deaktiviert. Sie sind nicht konfiguriert.
2. Deaktivieren Sie eines der folgenden Protokolle 3 - TLSv1 option SSL Policy Definition gibt\_0, TLSv1\_1 TLSv1\_2.
3. Wenn keine SSL-Richtlinie definiert drei (TLSv1\_0, TLSv1\_1, TLSv1_2) sind aktiviert.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Kennenlernen Ende SSL und SSL-Richtlinien, zum [Ende SSL Application Gateway aktivieren](application-gateway-end-to-end-ssl-powershell.md) ein Gateway mit Senden von Datenverkehr an Downloadzeit in verschlüsselter Form erstellt.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png