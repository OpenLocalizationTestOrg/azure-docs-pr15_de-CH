<properties 
   pageTitle="Eine bereitgestellte StorSimple Geräte | Microsoft Azure"
   description="Beschreibt die diagnose und Behebung von auftretenden Fehlern auf ein StorSimple Gerät derzeit bereitgestellt und betriebsbereit."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Eine betriebliche StorSimple Geräte

## <a name="overview"></a>Übersicht

Dieser Artikel enthält nützliche Hinweise zur Problembehandlung für Konfiguration Probleme, die auftreten können, nachdem das Gerät StorSimple bereitgestellt und betriebsbereit ist. Es beschreibt Probleme, Ursachen und empfohlenen Schritte können Sie die Probleme, die auftreten können, wenn Sie Microsoft Azure StorSimple ausführen. Diese Informationen gelten für StorSimple lokalen physischen Gerät und StorSimple virtuelles Gerät.

Am Ende dieses Artikels finden Sie eine Liste der Fehlercodes, die Microsoft Azure StorSimple Betrieb auftreten können, sowie Schritte nehmen Sie die Fehler zu beheben. 

## <a name="setup-wizard-process-for-operational-devices"></a>Assistenten für Installation Betriebseinrichtungen

Verwenden Sie den Setupassistenten ([Invoke-HcsSetupWizard][1]) Überprüfen der Gerätekonfiguration und erforderlichenfalls Maßnahmen.

Beim Ausführen des Setup-Assistenten auf eine zuvor konfigurierte und betriebsbereite Gerät unterscheidet der Prozessablauf. Ändern Sie die folgenden Einträge:

- IP-Adresse, Subnetzmaske und gateway
- Primäre DNS-server
- Primäre NTP-server
- Optionale Web-Proxy-Konfiguration

Der Setup-Assistent führt keine Maßnahmen Kennwort Auflistung und Gerät Registrierung.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Fehler bei der nachfolgende Durchläufe des Setup-Assistenten

Die folgende Tabelle beschreibt Fehler, die beim Ausführen von Setup-Assistenten auf einem Gerät betriebsbereit, mögliche Ursachen für Fehler auftreten und Maßnahmen zur Problembehebung empfohlenen. 

| Nein. | Fehlermeldung oder Bedingung | Mögliche Ursachen | Empfohlene Aktion |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | Fehler 350032: Dieses Gerät ist bereits deaktiviert. | Dieser Fehler wird angezeigt, wenn Sie den Setup-Assistenten auf einem Gerät ausgeführt, die deaktiviert ist. | [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) weiter. Ein deaktiviertes Gerät kann nicht aktiviert werden. Ein Factory-Reset kann erforderlich sein, bevor das Gerät wieder aktiviert werden kann. |
|  2  | Rufen Sie HcsSetupWizard: ERROR_INVALID_FUNCTION (Ausnahme von HRESULT: 0 x 80070001) | Das DNS-Serverupdate fehlschlägt. DNS-globale Einstellungen und über alle Netzwerk-Schnittstellen angewendet werden. | Aktiviert die Schnittstelle und die DNS-Einstellungen erneut anwenden. Dies kann im Netzwerk für andere Schnittstellen aktivierten beeinträchtigen, da diese globale Einstellungen. |
|  3  | Das Gerät im StorSimple-ServicePortal online zu sein scheint, aber beim Vervollständigen der Mindestkonfiguration und speichern Sie die Konfiguration der Vorgang fehlschlägt. | Der Webproxy wurde während der ursprünglichen Installation nicht konfiguriert, obwohl tatsächliche Proxyserver gab. | Verwenden Sie das [Cmdlet Test-HcsmConnection] [ 2] den Fehler gefunden. [Microsoft-Support kontaktieren](storsimple-contact-microsoft-support.md) , wenn Sie das Problem beheben können. |
|  4  | Rufen Sie HcsSetupWizard: Wert fällt nicht innerhalb des erwarteten Bereichs. | Eine falsche Subnetzmaske erzeugt dieser Fehler. Mögliche Ursachen sind: <ul><li> Die Subnetzmaske ist leer oder fehlt.</li><li>Das IPv6-Präfix Format ist falsch.</li><li>Die Schnittstelle ist Cloud aktiviert aber das Gateway ist nicht vorhanden oder falsch.</li></ul>Beachten Sie, dass 0 automatisch Cloud-aktiviert, wenn über den Setup-Assistenten konfiguriert werden. | Bestimmen Sie das Problem, Subnetzmaske 0.0.0.0 oder 256.256.256.256, und sehen Sie sich die Ausgabe. Geben Sie Werte für die Subnetzmaske, Gateway und IPv6-Präfix Bedarf. |
 
## <a name="error-codes"></a>Fehlercodes

Fehler werden in der richtigen Reihenfolge aufgelistet.

|Fehlernummer|Fehlermeldung oder eine Beschreibung|Empfohlene Benutzeraktion|
|:---|:---|:---|
|10502|Fehler beim Zugriff auf das Speicherkonto.|Warten Sie einige Minuten, und versuchen Sie es erneut. Wenn der Fehler weiterhin auftritt, bitte Kontakt Microsoft Support weiter.|
|40017|Der Sicherungsvorgang ist fehlgeschlagen, da in der backup-Richtlinie angegebenen Datenträger auf dem Gerät nicht gefunden wurde.|Die Sicherung Wiederholen-Vorgang, wenn der Fehler weiterhin auftritt, wenden Sie sich an Microsoft Support. für die nächsten Schritte.|
|40018|Der Sicherungsvorgang ist fehlgeschlagen, da keine backup-Richtlinie angegebenen Volumes auf dem Gerät gefunden wurden. |Die Sicherung Wiederholen-Vorgang, wenn der Fehler weiterhin auftritt, wenden Sie sich an Microsoft Support. für die nächsten Schritte.|
|390061|Das System ist ausgelastet oder nicht verfügbar.|Warten Sie einige Minuten, und versuchen Sie es erneut. Wenn der Fehler weiterhin auftritt, bitte Kontakt Microsoft Support weiter.|
|390143|Fehler bei 390143. (Fehler).|Wenn der Fehler weiterhin auftritt, wenden Sie Microsoft Support für nächste Schritte.|

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie nicht zur Behebung des Problems [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md) , um Hilfe. 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
