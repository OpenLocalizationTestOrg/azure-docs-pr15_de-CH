<properties
    pageTitle=" Ein Azure Media Services-Konto mit der Azure-Portal erstellen | Microsoft Azure"
    description="Dieses Lernprogramm führt Sie schrittweise ein Azure Media Services-Konto mit der Azure-Portal erstellen."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Erstellen Sie ein Azure-Portal mit Azure Media Services-Konto

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 

Azure-Portal ermöglicht das Erstellen eines Kontos Azure Media Services (AMS). Ihr Konto können Sie Media Services zugreifen, mit denen Sie speichern, verschlüsseln, codieren, verwalten und Streaming Media-Inhalte in Azure. Zeitpunkt ein Media Services-Konto erstellen Sie ein Speicherkonto zugeordneten (auch vorhandene verwendet) in derselben geographischen Region Media Services-Konto.

Dieser Artikel beschreibt einige allgemeine Konzepte und zeigt, wie ein Media Services-Konto mit der Azure-Portal erstellen.

## <a name="concepts"></a>Konzepte

Zugriff auf Media Services erfordert zwei zugeordnete Konten:

- Media Services-Konto. Ihr Konto erhalten Sie Zugriff auf eine Reihe von Cloud-basierten Media Services, die in Azure verfügbar sind. Media Services-Konto werden keine tatsächlichen Medieninhalte gespeichert. Stattdessen werden Metadaten zu der Medieninhalte Verarbeitungsaufträge Media in Ihrem Konto gespeichert. Bei der Erstellung des Kontos, wählen Sie eine verfügbare Region Media Services. Die gewählte Region ist ein Rechenzentrum, in dem die Metadaten Datensätze für Ihr Konto gespeichert.

    Folgende Bereiche verfügbaren Media Services (AMS): Nordeuropa Westeuropa Westen der USA, USA, Osten, Südostasien, Ostasien, Japan, Japan OST. Media Services werden keine Gruppen verwendet.
    
    AMS ist jetzt auch in folgenden Rechenzentren: Süden Brasiliens, Indien West Indien Süd und Indien Central. Jetzt können Sie Azure-Portal zu Media Dienstkonten beschriebene Aufgaben ausführen. Live-Codierung ist jedoch in Rechenzentren nicht aktiviert. Darüber hinaus stehen nicht alle Arten von Codierung reservierte Einheiten in Rechenzentren.
    
    - Brasilien Süd: Standard und Codierung reservierte Basiseinheiten sind nur verfügbar.
    - West, Indien Indien Süd: 

- Ein Azure Storage-Konto. Speicherkonten müssen in derselben geographischen Region Media Services-Konto befinden. Wenn ein Media Services-Konto erstellen, Sie können entweder ein vorhandenes Speicherkonto in derselben Region oder erstellen ein neues Speicherkonto in derselben Region. Media Services-Konto löschen, werden die Blobs in Ihrem Speicherkonto verknüpfte nicht gelöscht.

## <a name="create-an-ams-account"></a>Ein AMS-Konto erstellen

Die Schritte in diesem Abschnitt zeigen, wie ein AMS-Konto erstellen.

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com/).
2. Klicken Sie auf **+ Neuer** > **Web + Mobile** > **Media Services**.

    ![Media Services erstellen](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Geben Sie die gewünschten Werte, in **MEDIA SERVICES-Konto erstellen** .

    ![Media Services erstellen](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Geben Sie den Namen des neuen AMS **Kontonamen**. Ein Kontoname Media Services ist alle Kleinbuchstaben Zahlen oder Buchstaben ohne Leerzeichen und 3 bis 24 Zeichen lang ist.
    2. Wählen Sie im Abonnement unter verschiedenen Azure-Abonnements, denen Sie Zugriff haben.
    
    2. **Ressourcengruppe**wählen Sie die neue oder vorhandene Ressource.  Eine Ressourcengruppe ist eine Sammlung von Ressourcen, die Lebenszyklus, Berechtigungen und Richtlinien. Weitere Informationen [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Wählen Sie **Speicherort**geografische Region, mit der die Medien- und Metadaten für Ihr Media Services-Konto gespeichert. Dieser Bereich wird zum Verarbeiten und Streamen von Medien verwendet. Die verfügbaren Media Services Bereiche werden im Dropdown-Listenfeld angezeigt. 
    
    3. Wählen Sie **Konto**ein Speicherkonto BLOB-Speicher der Medieninhalte von der Media Services-Konto. Auswählen eines vorhandenen Speicherkontos in derselben geographischen Region als Media Services-Konto oder ein Speicherkonto erstellen. Ein neues Speicherkonto wird in derselben Region erstellt. Die Regeln für Namen Speicher entsprechen Media Services-Konten.

        Erfahren Sie mehr über Speicher [hier](storage-introduction.md).

    4. **Pin Dashboard** die Fortschritte der Bereitstellung Konto auswählen
    
7. Klicken Sie am unteren Rand des Formulars **Erstellen** .

    Sobald das Konto erfolgreich erstellt wurde, wird der Status **ausgeführt**. 

    ![Media Services settings](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Das AMS-Konto verwalten (z. B. Hochladen Videos Codieren von Ressourcen, Überwachung des Projektstatus) **Settings** Fenster.

## <a name="manage-keys"></a>Verwalten von Schlüsseln

Sie benötigen den Kontonamen und Primärschlüsselinformationen zum programmgesteuerten Zugriff auf das Media Services-Konto.

1. Wählen Sie Ihr Konto in Azure-Portal. 

    Das Fenster **Einstellungen** auf der rechten Seite. 

2. Wählen Sie im **Einstellungsfenster** **Schlüssel**. 

    Fenster **Verwalten Schlüssel** wird der Name und die primären und sekundären Schlüssel angezeigt. 
3. Drücken Sie die Schaltfläche Kopieren, um die Werte zu kopieren.
    
    ![Media Services-Schlüssel](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt Dateien in Ihrem AMS-Konto hochladen. Weitere Informationen finden Sie unter [Dateien hochladen](media-services-portal-upload-files.md).

## <a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


