<properties 
   pageTitle="Ändern der Daten 0 auf ein StorSimple | Microsoft Azure"
   description="Erfahren Sie, wie mit Windows PowerShell für StorSimple die Daten 0-Schnittstelle auf dem StorSimple-Gerät konfigurieren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-device"></a>Ändern der Daten 0 Netzwerk-Schnittstelle auf dem StorSimple-Gerät

## <a name="overview"></a>Übersicht

Das Microsoft Azure StorSimple Gerät hat sechs Netzwerkschnittstellen aus Daten 5 0. Die Daten 0 Schnittstelle immer über die Windows PowerShell-Schnittstelle oder die serielle Konsole konfiguriert und automatisch Cloud aktiviert. Beachten Sie, dass Sie Daten 0 Netzwerkschnittstelle über klassischen Azure-Portal konfigurieren können. 

Schnittstelle durch den Setup-Assistenten während der Konfiguration Daten 0 anfängliche Bereitstellung von Gerät StorSimple. Wenn das Gerät in einen Betriebsmodus ist möglicherweise müssen Daten 0 Settings. Dieses Lernprogramm bietet zwei Methoden zum Ändern von Daten 0 Einstellungen sowohl über Windows PowerShell für StorSimple Netzwerk.

Nach dem Lesen dieses Lernprogramm können, Sie:

- Ändern von Daten 0 Netzwerk-Einstellung über den Setup-Assistenten
- Daten 0 Netzwerk ändern über die `Set-HcsNetInterface` Cmdlets



## <a name="modify-data-0-network-settings-through-setup-wizard"></a>Ändern Sie Daten 0 Netzwerk Setup-Assistenten
Sie können Daten 0 Network Settings durch Verbinden der Windows PowerShell-Schnittstelle des Geräts StorSimple und ein Setup-Assistent-Sitzung neu konfigurieren. Gehen Sie zum Ändern von Daten 0 Einstellungen:

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>Um Daten 0 Network Setup-Assistenten ändern

1. Wählen Sie im Menü seriellen Konsole Option 1 **Vollzugriff einloggen**. Nach der Aufforderung das **Administratorkennwort Gerät**bereitstellen. Das Standardkennwort lautet `Password1`.

2. In der Befehlszeile eingeben:

    `Invoke-HcsSetupWizard`

3. Setup-Assistent werden Daten 0 konfigurieren Schnittstelle Ihres Geräts. Neue Werte für die IP-Adresse, Gateway und Netmask angeben.

> [AZURE.NOTE] Feste Controller IPs Seite **Konfigurieren** StorSimple Gerät im klassischen Azure-Portal neu konfiguriert werden müssen. Weitere Informationen finden Sie im [Netzwerkschnittstellen ändern](storsimple-modify-device-config.md#modify-network-interfaces).


## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>Ändern Sie Daten 0 Netzwerk über Set-HcsNetInterface-cmdlet
Eine alternative Möglichkeit, Daten 0 konfigurieren Netzwerkschnittstelle durch wird die `Set-HcsNetInterface` Cmdlet. StorSimple-Gerät aus Windows PowerShell wird das Cmdlet ausgeführt. Wenn Sie dieses Verfahren verwenden, kann IPs feste Controller auch hier konfiguriert werden. Gehen Sie zum Ändern der Daten 0 Einstellungen: 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>Um Daten 0 Netzwerk durch das Cmdlet "Set-HcsNetInterface" ändern

1. Wählen Sie im Menü seriellen Konsole Option 1 **Vollzugriff einloggen**. Nach der Aufforderung das Administratorkennwort Gerät bereitstellen. Das Standardkennwort lautet `Password1`.

2. In der Befehlszeile eingeben:

    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
    
    Geben Sie in spitzen Klammern folgende Daten 0:
                                            
    - IPv4-Adresse
    
    - IPv4-gateway
    
    - IPv4-Subnetzmaske
    
    - Feste IPv4-Adresse für Controller 0

    - Feste IPv4-Adresse für Controller 1

    Weitere Informationen zur Verwendung dieses Cmdlets finden Sie im [Windows PowerShell-StorSimple-Cmdlet zu Referenzzwecken](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie Netzwerkschnittstellen Daten 0 können Sie die [Seite im klassischen Azure-Portal konfigurieren](storsimple-modify-device-config.md). 

- Wenn Probleme auftreten, wenn Ihre Netzwerkschnittstellen konfigurieren, finden Sie [Fehlerbehebung Fragen](storsimple-troubleshoot-deployment.md).

