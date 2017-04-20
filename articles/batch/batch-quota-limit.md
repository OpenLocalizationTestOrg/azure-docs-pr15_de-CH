<properties
    pageTitle="Batch-Service-Vorgaben und Grenzwerte | Microsoft Azure"
    description="Lernen Sie Standardkontingente Azure Batch, Grenzen und Integritätsregeln und erhöht das Kontingent anfordern"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Kontingente und Grenzwerte für Azure Batch-Dienst

Wie mit anderen Diensten Azure begrenzt auf bestimmte Ressourcen der Stapel zugeordnet ist. Diese Grenzwerte sind Standardkontingente von Azure-Abonnement oder Ebene angewendet. Dieser Artikel beschreibt die Standardwerte und Anforderung Kontingent erhöht.

Möchten Sie Produktions-Workloads in Batch ausführen, müssen Sie eine oder mehrere Kontingente oberhalb der Standardwert erhöhen. Ggf. eine erhöhen können Sie eine online- [Kundensupport-Anforderung](#increase-a-quota) kostenlos öffnen.

>[AZURE.NOTE] Ein Kontingent ist ein Kreditlimit Kapazität-Angebot. Wenden Sie sich an den Azure-Support haben große Kapazitätsbedarf.

## <a name="subscription-quotas"></a>Abonnement Kontingente
**Ressource**|**Standardlimit**|**Maximale Anzahl**
---|---|---
Batch-Konten pro Region pro Abonnement | 1 | 50

## <a name="batch-account-quotas"></a>Batch-Konto Kontingente
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Andere Grenzwerte
**Ressource**|**Maximale Anzahl**
---|---
[Gleichzeitige Aufgaben](batch-parallel-node-tasks.md) pro Compute-Knoten | 4 X Anzahl der Kerne Knoten
[Applikationen](batch-application-packages.md) pro Batch-Konto        | 20
Anwendungspakete pro Anwendung  | 40
Größe des Pakets (jeder)       | Ca. 195GB<sup>1</sup>

<sup>1</sup> Azure Storage Limit für die maximale BLOB-Größe

## <a name="view-batch-quotas"></a>Anzeigen von Batch-Vorgaben

Die Stapelverarbeitung Konto Kontingente in [Azure-Portal]anzeigen[portal].

1. Wählen Sie **Batch-Konten** im Portal, und wählen Sie Batch-Konto, das, dem Sie interessiert.

2. Wählen Sie **Eigenschaften** im Menü auf das Batch-Konto

3. Zeigt Eigenschaften Blade Batch-Konto aktuell zugewiesenen **Kontingente**

    ![Batch-Konto Kontingente][account_quotas]

## <a name="increase-a-quota"></a>Eine Quote

Führen Sie Schritte Kontingent anfordern Erhöhen der [Azure-Portal][portal].

1. Wählen Sie die Kachel **Hilfe und Support** für Ihr Portal Dashboard oder Fragezeichen (**?**) in der oberen rechten Ecke des Portals.

2. Wählen Sie **neue support-Anfragen** > **Grundlagen**.

3. **Grundlagen** -Blade:

    ein. **Ausstellen von Typ** > **Kontingent**

    b. Wählen Sie Ihr Abonnement.

    c. **Kontingenttyp** > **Stapel**

    d. **Supportplan** > **Kontingent Unterstützung -**

    Klicken Sie auf **Weiter**.

4. Auf das **Problem** :

    ein. Wählen Sie einen **Schweregrad** nach [geschäftlichen][support_sev].

    b. Geben Sie **Details**jeden Kontingents ändern möchten, Batch-Kontonamen und die neue Grenze.

    Klicken Sie auf **Weiter**.

5. Auf die **Kontaktinformationen** :

    ein. Wählen Sie die **bevorzugte Methode erhalten**.

    b. Überprüfen und die erforderlichen Kontaktinformationen eingeben.

    Klicken Sie auf **Erstellen** , um die Anfrage abzusenden.

Sobald Sie Ihre Support-Anfrage gesendet haben, wird Azure Sie Kundendienst. Beachten Sie, dass die Anforderung bis zu 2 Werktage dauern kann.

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen Sie ein Azure-Portal mit Azure Batch-Konto](batch-account-create-portal.md)

* [Azure Batch-Features (Übersicht)](batch-api-basics.md)

* [Azure-Abonnement und Service Grenzen, Kontingente und Integritätsregeln](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
