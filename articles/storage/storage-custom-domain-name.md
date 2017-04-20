<properties
    pageTitle="Ein Domänenname für den BLOB-Speicher Endpunkt konfigurieren | Microsoft Azure"
    description="Erfahren Sie, wie BLOB-Speicher Endpunkt für eine Firma Azure-Speicher in Azure-Verwaltungsportal benutzerdefinierte Domäne zuzuordnen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Konfigurieren Sie einen benutzerdefinierten Domänennamen für den BLOB-Speicher-Endpunkt

## <a name="overview"></a>Übersicht

Sie können eine benutzerdefinierte Domäne für den Zugriff auf BLOB-Daten in Ihrem Konto Azure-Speicher. Der Standardendpunkt für BLOB-Speicher ist `<storage-account-name>.blob.core.windows.net`. Wenn Sie eine benutzerdefinierte Domäne und Unterdomäne wie **www.contoso.com** BLOB-Endpunkt für das Speicherkonto und die Benutzer können auch Zugriff auf BLOB-Daten in das Speicherkonto, Domäne zuordnen.

>[AZURE.IMPORTANT] Azure-Speicher unterstützt noch keine HTTPS mit benutzerdefinierten Domänen. Wir sind uns bewusst, dass Kunden diese Funktion, und wird in einer zukünftigen Version verfügbar sein.

Gibt es zwei Arten der BLOB-Endpunkt für das Speicherkonto Ihrer benutzerdefinierten Domäne auf. Am einfachsten ist ein CNAME-Eintrag Zuordnen der benutzerdefinierten Domäne und Unterdomäne BLOB-Endpunkt zu erstellen. Ein CNAME-Eintrag ist ein DNS-Feature, die eine Quelldomäne zu einer Zieldomäne zuordnet. In diesem Fall ist die Quelldomäne eigene Domäne und Unterdomäne - Beachten Sie, dass die Domäne immer erforderlich ist. Die Zieldomäne ist die Blob-Endpunkt.

Der Prozess der BLOB-Endpunkt der benutzerdefinierten Domäne zuordnen kann, führen in einem kurzen Ausfallzeit für die Domäne beim Registrieren der Domäne in der [Azure-Verwaltungsportal](https://manage.windowsazure.com). Wenn Ihre benutzerdefinierte Domain zurzeit eine Anwendung mit einer Vereinbarung zum Servicelevel (SLA), die erfordert, dass keine Ausfallzeiten werden unterstützt, dann können Azure **Asverify** Subdomäne Sie intermediate Registrierung bereitstellen, damit Benutzer Zugriff auf Ihre Domäne beim DNS Zuordnung erfolgt ist.

Die folgende Tabelle zeigt Beispiel URLs für den Zugriff auf BLOB-Daten in ein Speicherkonto namens **Mystorageaccount**. Benutzerdefinierten Bereich für das Speicherkonto **www.contoso.com**wird registriert:

Ressourcentyp|URL-Formate
---|---
Konto|**Standard URL:** http://mystorageaccount.blob.core.windows.net<p>**Benutzerdefinierte Domäne URL:** http://www.contoso.com</td>
BLOB|**Standard URL:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**Benutzerdefinierte Domäne URL:** http://www.contoso.com/mycontainer/myblob
Stammcontainer|**Standard-URL:** http://mystorageaccount.blob.core.windows.net/myblob oder http://mystorageaccount.blob.core.windows.net/$ Root/Myblob<p>**Benutzerdefinierte Domäne URL:** http://www.contoso.com/myblob oder http://www.contoso.com/$ Root/Myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Eine benutzerdefinierte Domäne für das Speicherkonto registrieren

Können Sie Ihre benutzerdefinierte Domain registrieren, haben Sie keine Probleme mit der Domäne Benutzer kurz verfügbar oder Ihre benutzerdefinierte Domain nicht gerade eine Anwendung hostet.

Wenn Ihre benutzerdefinierte Domain derzeit eine Anwendung unterstützt, die Ausfallzeiten haben kann, verwenden Sie das Verfahren <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">eine benutzerdefinierte Domäne für das Speicherkonto mit zwischengeschalteten Asverify Domäne registrieren</a>.

Konfigurieren Sie einen benutzerdefinierten Domänennamen müssen Sie einen neuen CNAME-Datensatz mit Ihrer Registrierungsstelle erstellen. CNAME-Eintrag gibt einen Alias für einen Domänennamen an. In diesem Fall wird die Adresse der benutzerdefinierten Domäne an den Endpunkt BLOB-Speicher für das Speicherkonto zugeordnet.

Jeder Registrar hat eine Methode ähnliche, aber andere festlegen einen CNAME-Eintrag, aber das Konzept ist dasselbe. Beachten Sie, dass viele grundlegende Domäne Registrierung Pakete keinen DNS-Konfiguration müssen Sie möglicherweise Ihre Domäne Registrierungspaket aktualisieren, bevor der CNAME-Eintrag zu erstellen.

1.  Navigieren Sie in [Azure-Verwaltungsportal](https://manage.windowsazure.com)auf der Registerkarte **Speicher** .

2.  Klicken Sie in der Registerkarte **Speicher** Speicherkonto für die gewünschte benutzerdefinierte Domäne.

3.  Klicken Sie auf die Registerkarte **Konfigurieren** .

4.  Klicken Sie am unteren Bildschirmrand auf **Domäne verwalten** , um das Dialogfeld **Benutzerdefinierte Domäne verwalten** . In den Text am oberen Rand des Dialogfeldes sehen Sie Informationen für den CNAME-Eintrag erstellen. Ignorieren Sie für dieses Verfahren die Subdomäne **Asverify** auf Text.

5.  Melden Sie sich bei DNS der Website Ihrer Registrierungsstelle und zur Seite zum Verwalten von DNS. Dieser Abschnitt wie **Domänennamen**, **DNS**oder **Name Server Management**möglicherweise.

6.  Suchen Sie den Abschnitt für die Verwaltung von CNAMEs. Möglicherweise zu einer Seite erweitert die Worte **CNAME** **Alias**oder **Unterdomänen**.

7.  Erstellen Sie einen neuen CNAME-Eintrag und einen Alias Subdomäne **Fotos**oder **Www** . Geben Sie einen Hostnamen, die Ihr Dienstendpunkt Blob im Format **mystorageaccount.blob.core.windows.net** (wobei **Mystorageaccount** der Name Ihres Speicherkontos ist). Hostnamen verwenden wird im Text des Dialogfelds **Benutzerdefinierte Domäne verwalten** für Sie bereitgestellt.

8.  Nach der Erstellung der CNAME-Eintrag zum Dialogfeld **Benutzerdefinierte Domäne verwalten** zurückzukehren Sie, und geben Sie den Namen der benutzerdefinierten Domäne, einschließlich der Subdomäne im **Benutzerdefinierten Domänennamen** . Wenn die Domäne **contoso.com** und Ihre Subdomain **Www**, geben Sie **www.contoso.com**; Wenn Ihre Subdomain **Fotos**, geben Sie **photos.contoso.com**. Beachten Sie, dass die Domäne erforderlich.

9. Klicken Sie auf die Schaltfläche **Registrieren** registrieren Ihre benutzerdefinierte Domain.

    Wenn die Registrierung erfolgreich ist, sehen Sie die Nachricht, die **Ihre benutzerdefinierte Domain aktiv ist**. Benutzer können jetzt anzeigen BLOB-Daten für Ihre benutzerdefinierte Domain so die entsprechenden Berechtigungen.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Registrieren Sie eine benutzerdefinierte Domäne für das Speicherkonto mit zwischengeschalteten Asverify Domäne

Gehen Sie zum Registrieren der benutzerdefinierten Domäne Ihre benutzerdefinierte Domain zurzeit eine Anwendung mit einer SLA unterstützt, die erfordert es sollten keine Ausfallzeiten. CNAME zeigt von Asverify erstellen. &lt;Sub&gt;. &lt;Customdomain&gt; , Asverify. &lt;Storageaccount&gt;. blob.core.windows.net, können Sie vorab mit Azure registrieren. Dann können Sie einen zweiten CNAME aus &lt;Sub&gt;. &lt;Customdomain&gt; , &lt;Storageaccount&gt;. blob.core.windows.net Zeitpunkt Datenverkehr an Ihre benutzerdefinierte Domain zu Blob-Endpunkt geleitet.

Die Subdomäne Asverify ist eine spezielle Domäne von Azure erkannt. Vorangestellt **Asverify** eigene Subdomäne können Sie Azure Ihre benutzerdefinierte Domain erkennen, ohne den DNS-Eintrag für die Domäne zu ändern. Nachdem Sie den DNS-Eintrag für die Domäne ändern, wird dieser Endpunkt Blob ohne Ausfallzeiten zugeordnet.

1.  Navigieren Sie in [Azure-Verwaltungsportal](https://manage.windowsazure.com)auf der Registerkarte **Speicher** .

2.  Klicken Sie in der Registerkarte **Speicher** Speicherkonto für die gewünschte benutzerdefinierte Domäne.

3.  Klicken Sie auf die Registerkarte **Konfigurieren** .

4.  Klicken Sie am unteren Bildschirmrand auf **Domäne verwalten** , um das Dialogfeld **Benutzerdefinierte Domäne verwalten** . Der Text am oberen Rand des Dialogfeldes finden Sie Informationen zum **Asverify** Subdomäne mit CNAME-Eintrag erstellen.

5.  Melden Sie sich bei DNS der Website Ihrer Registrierungsstelle und zur Seite zum Verwalten von DNS. Dieser Abschnitt wie **Domänennamen**, **DNS**oder **Name Server Management**möglicherweise.

6.  Suchen Sie den Abschnitt für die Verwaltung von CNAMEs. Möglicherweise zu einer Seite erweitert die Worte **CNAME** **Alias**oder **Unterdomänen**.

7.  Erstellen Sie einen neuen CNAME-Eintrag und einen Subdomäne Alias, der Unterdomäne Asverify enthält. Die angegebene Domäne werden beispielsweise im Format **asverify.www** oder **asverify.photos**. Geben Sie einen Hostnamen, die Ihr Dienstendpunkt Blob im Format **asverify.mystorageaccount.blob.core.windows.net** (wobei **Mystorageaccount** der Name Ihres Speicherkontos ist). Hostnamen verwenden wird im Text des Dialogfelds **Benutzerdefinierte Domäne verwalten** für Sie bereitgestellt.

8.  Nach der Erstellung der CNAME-Eintrag zurück zum Dialogfeld **Benutzerdefinierte Domäne verwalten** , und geben Sie den Namen der benutzerdefinierten Domäne im Feld **Benutzerdefinierte Domänennamen** . Wenn die Domäne **contoso.com** und Ihre Subdomain **Www**, geben Sie **www.contoso.com**; Wenn Ihre Subdomain **Fotos**, geben Sie **photos.contoso.com**. Beachten Sie, dass die Domäne erforderlich.

9.  Aktivieren Sie das Kontrollkästchen sagt **Erweitert: die Domäne "Asverify" verwenden, um Meine benutzerdefinierte Domäne vormerken**.

10. Klicken Sie auf die Schaltfläche **Registrieren** , um Ihre benutzerdefinierte Domain vormerken.

    Wenn die Voranmeldung erfolgreich ist, sehen Sie die Nachricht, die **Ihre benutzerdefinierte Domain aktiv ist**.

11. An dieser Stelle Ihre benutzerdefinierte Domain wurde von Azure überprüft jedoch Datenverkehr Ihrer Domäne noch nicht an das Speicherkonto verschoben. Zum Abschluss der DNS der Website Ihrer Registrierungsstelle zurück, und erstellen Sie anderen CNAME-Eintrag, der der Domäne der Blob-Endpunkt zugeordnet ist. Beispielsweise die Domäne als **Www** oder **Fotos**und Hostname **mystorageaccount.blob.core.windows.net** (wobei **Mystorageaccount** der Name Ihres Speicherkontos ist). Mit diesem Schritt ist die Registrierung Ihrer benutzerdefinierten Domäne abgeschlossen.

12. Schließlich können Sie mit **Asverify**erstellten CNAME-Eintrag löschen, nur ein Zwischenschritt erforderlich war.

Benutzer können jetzt anzeigen BLOB-Daten für Ihre benutzerdefinierte Domain so die entsprechenden Berechtigungen.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Überprüfen Sie die benutzerdefinierte Domäne verweist auf die Blob-Endpunkt

Um sicherzustellen, dass Ihre benutzerdefinierte Domain tatsächlich die Blob-Endpunkt zugeordnet ist, erstellen Sie einen Blob in einem öffentlichen Container innerhalb Ihres Speicherkontos. Anschließend in einem Webbrowser verwenden Sie einen URI im folgenden Format auf das Blob:

-   http://<*subdomain.customdomain*>/<*Mycontainer*>/<*Myblob*>

Die folgende URI können Sie z. B. eine Web Form über eine benutzerdefinierte **photos.contoso.com** -Domäne zugreifen, die ein Blob in Ihrem **Myforms** Container zugeordnet ist:

-   http://Photos.contoso.com/MyForms/ApplicationForm.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Registrierung einer benutzerdefinierten Domäne Ihres Speicherkontos

Gehen Sie folgendermaßen vor, um die Registrierung einer benutzerdefinierten Domäne: 

1. Das [klassische Azure-Portal](https://manage.windowsazure.com)anmelden. 

2. Klicken Sie im Navigationsbereich auf **Speicher**. 

3. Klicken Sie auf **Speicher** Speicherkonto Dashboard angezeigt. 

5. Klicken Sie auf der Multifunktionsleiste auf **Domäne verwalten**. 

6. Klicken Sie im Dialogfeld **Benutzerdefinierte Domäne verwalten** auf **Registrierung**. 


## <a name="additional-resources"></a>Zusätzliche Ressourcen

-   [Wie benutzerdefinierte Domäne Content Delivery Network (CDN) Endpunkt zugeordnet](../cdn/cdn-map-content-to-custom-domain.md)
