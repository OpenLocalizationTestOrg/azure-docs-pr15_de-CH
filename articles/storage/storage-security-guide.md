<properties
    pageTitle="Azure Storage Security Guide | Microsoft Azure"
    description="Details zu viele Methoden zum Azure-Speicher, einschließlich aber nicht beschränkt auf RBAC Storage Service Encryption, clientseitige Verschlüsselung, SMB 3.0 und Azure Datenträgerverschlüsselung sichern."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="robinsh"/>

#<a name="azure-storage-security-guide"></a>Azure Storage Security guide

##<a name="overview"></a>Übersicht

Azure-Speicher bietet umfassende Sicherheitsfunktionen zusammen ermöglicht Entwicklern, sichere Anwendung zu erstellen. Das Speicherkonto selbst kann rollenbasierte Zugriffskontrolle mit Azure Active Directory gesichert werden. Daten können zwischen einer Anwendung und Azure mithilfe von [Clientseitigen Encryption](storage-client-side-encryption.md), HTTPS oder SMB 3.0 gesichert werden. Daten können mit festgelegt werden automatisch verschlüsselt werden, beim Schreiben in Azure Storage [Storage Service Verschlüsselung (SSE)](storage-service-encryption.md). Betriebssystem und Daten von virtuellen Computern verwendete Datenträger lassen [Azure Datenträger](../security/azure-security-disk-encryption.md)Verschlüsselung verschlüsselt werden. Delegierter Zugriff auf die Datenobjekte in Azure Storage kann mit [Shared Access Signatures](storage-dotnet-shared-access-signature-part-1.md)erteilt werden.

Dieser Artikel bietet einen Überblick der Sicherheitsfeatures, die mit Azure verwendet werden können. Links zu Artikeln, die Details jedes Feature so geben Sie problemlos tun erhalten weitere Untersuchung jedes Thema.

Hier werden die in diesem Artikel behandelten Themen auf:

-   [Management-Ebene Sicherheit](#management-plane-security) – das Speicherkonto sichern

    Die Verwaltungsebene umfasst Ressourcen zur Verwaltung Ihres Speicherkontos. In diesem Abschnitt sprechen wir über Azure-Ressourcen-Manager-Bereitstellungsmodell und wie Sie rollenbasierte Zugriffskontrolle (RBAC) Speicherkonten Zugriffskontrolle. Wir werden auch zum Verwalten von Ihrem Konto Speicherschlüssel und wie sie Regenerieren sprechen.

-   [Daten Plane Sicherheit](#data-plane-security) – Zugriff auf Ihre Daten sichern

    In diesem Abschnitt betrachten wir Zugriff auf die tatsächlichen Datenobjekte in Ihrem Speicherkonto wie Blobs, Dateien, Warteschlangen und Tabellen mit Shared Access Signatures und Zugriffsrichtlinien gespeichert. Service Level SAS und SAS Ebene werden behandelt. Wir sehen auch das Protokoll HTTPS beschränkt Zugriff auf eine bestimmte IP-Adresse (oder der IP-Adressen) zu beschränken und zum einen SAS widerrufen, ohne es abläuft.

-   [Verschlüsselung in Transit](#encryption-in-transit)

    Dieser Abschnitt erläutert die Daten zu schützen, wenn Sie in oder aus dem Azure-Speicher übertragen. Wir sprechen über die empfohlene Verwendung von HTTPS und Verschlüsselung von SMB 3.0 für Dateifreigaben Azure verwendet. Wir nehmen auch einen Blick auf die clientseitige Verschlüsselung können Sie die Daten verschlüsseln, bevor sie in einer Clientanwendung in den Speicher übertragen werden und zum Entschlüsseln der Daten nach der Übertragung aus.

-   [Verschlüsselung ruhender](#encryption-at-rest)

    Wir sprechen über Storage Service Verschlüsselung (SSE) und wie Sie aktivieren ein Speicherkonto, wodurch die Block-Blobs Seitenblobs und Anhängen Blobs automatisch verschlüsselt werden, wenn in den Azure-Speicher geschrieben. Auch werden wir Azure Datenträger Verschlüsselung und untersuchen die grundlegenden Unterschiede und Anfragen Datenträger Verschlüsselung und SSE und clientseitige Verschlüsselung. FIPS-Konformität für US-Regierung Computer werden kurz behandelt.

-   Verwenden [Speicheranalyse](#storage-analytics) Azure-Speicher aufnehmen

    Dieser Abschnitt beschreibt, wie Informationen in Protokollen Analytics Speicher für eine Anforderung. Wir betrachten eine echte Speicheranalyse Daten und Informationen zu erkennen, ob eine mit dem Schlüssel Storage-Konto mit einer Signatur Shared Access Anforderung oder anonym und ob erfolgreich war oder nicht.

-   [Browser-basierte Kunden mit CORS](#Cross-Origin-Resource-Sharing-CORS)

    Informationen zu Cross-Ursprung Ressourcenfreigabe (CORS) erörtert. Wir sprechen über domänenübergreifenden Zugriff und mit CORS Funktionen in Azure-Speicher integriert.

##<a name="management-plane-security"></a>Management-Ebene Sicherheit

Die Verwaltungsebene besteht der Vorgänge, die den Speicher selbst beeinflussen. Beispielsweise können Sie erstellen ein Speicherkonto löschen, erhalten eine Liste der Speicherkonten in einem Abonnement, Konto Speicherschlüssel abrufen oder Konto Speicherschlüssel regenerieren.

Beim Erstellen eines neuen Speicherkontos wählen Sie ein Modell zur Bereitstellung von Classic oder Ressourcen-Manager. Klassisch Ressourcen in Azure erstellen kann nur nichts Zugriff auf das Abonnement, und das Speicherkonto.

Dieses Handbuch konzentriert sich auf Ressourcen-Manager-Modell ist die empfohlenen Methoden zum Erstellen von Speicherkonten. Mit der Ressourcen-Manager-Speicherkonten und Zugriff auf das gesamte Abonnement können Sie Zugriff auf Weitere begrenzte Role-Based Access Control (RBAC) mit Management-Ebene steuern.

###<a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Schützen Ihres Speicherkontos mit Role-Based Access Control (RBAC)

Sprechen wir über was RBAC ist und wie Sie sie verwenden können. Jede Azure-Abonnement hat Azure Active Directory. Benutzer, Gruppen und Applikationen aus dem Verzeichnis können erteilt werden Zugriff auf Ressourcen in der Azure-Abonnement, mit dem Ressourcen-Manager-Bereitstellungsmodell. Dies wird als Role-Based Access Control (RBAC) bezeichnet. Um diesen Zugriff zu verwalten, können Sie der [Azure-Portal](https://portal.azure.com/) [Azure CLI-Tools](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)oder [Azure Storage Resource Provider REST-APIs](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Mit dem Ressourcen-Manager können Sie das Speicherkonto eine Ressource Gruppe und Zugriff auf die Verwaltung, bestimmte Speicherkonto mit Azure Active Directory. Beispielsweise erhalten bestimmte Benutzer Sie die Möglichkeit, Speicherschlüssel Konto zugreifen, während andere Benutzer können Informationen über das Speicherkonto anzeigen, aber nicht auf Konto Speicherschlüssel.

####<a name="granting-access"></a>Gewähren des Zugriffs

Zugriff durch Benutzer, Gruppen und Applikationen im rechten Bereich die entsprechende RBAC-Rolle zugewiesen. Um Zugriff auf das gesamte Abonnement zuweisen eine Rolle auf Abonnementebene. Die Ressourcengruppe selbst Berechtigungen erteilen, um Zugriff auf alle Ressourcen in einer Ressourcengruppe zu gewähren. Bestimmte Ressourcen, wie Speicher-Konten können auch bestimmte Rollen zuweisen.

Hier sind die wichtigsten Punkte, die Sie kennen, mit RBAC Verwaltungsvorgänge von Azure Storage-Konto zuzugreifen:

-   Beim Zuweisen des Zugriffs weisen Sie grundsätzlich eine Rolle für das Konto, das Sie zugreifen möchten. Sie können die Vorgänge zu diesem Storage-Konto verwendet, nicht jedoch die Datenobjekte im Konto Zugriffskontrolle. Sie können beispielsweise über die Berechtigung zum Abrufen der Eigenschaften des Speicherkontos (wie Redundanz) erteilen, aber nicht an einen Container oder Daten in einem Container im BLOB-Speicher.

-   Andere Benutzer Zugriff auf die Datenobjekte in das Speicherkonto können Sie Ihnen Berechtigung zum Lesen der speicherkontoschlüssel, und Benutzer können diese Schlüssel Blobs, Queues, Tabellen und Dateien zugreifen.

-   Rollen können einem bestimmten Benutzerkonto, eine Benutzergruppe oder eine bestimmte Anwendung zugewiesen werden.

-   Jede Rolle hat eine Liste der Aktionen und keine Aktionen. Virtual Machine Teilnehmerrolle hat beispielsweise eine Aktion "ListKeys", mit der das Konto Speicherschlüssel gelesen werden. Der Mitwirkende hat "Keine Maßnahmen" wie den Zugriff für Benutzer in Active Directory aktualisiert.

-   Rollen für Speicher enthalten (jedoch nicht nur) der folgenden:

    -   Besitzer – sie können alles, einschließlich Zugriff verwalten.

    -   Teilnehmer – sie können nichts Besitzer können außer Zugriff zuweisen. Benutzer mit dieser Rolle kann angezeigt und regeneriert die speicherkontoschlüssel. Mit der speicherkontoschlüssel können sie Datenobjekte zugreifen.

    -   Leser – können sie Informationen über das Speicherkonto außer Geheimnisse anzeigen. Beispielsweise wenn Sie eine Rolle mit Leseberechtigungen für das Speicherkonto einer Person zuweisen, zeigen sie die Eigenschaften des Speicherkontos, aber nicht der Speicherschlüssel Konto anzeigen oder ändern Sie die Eigenschaften.

    -   Storage-Konto Teilnehmer – verwalten sie das Speicherkonto – können sie das Abonnement Ressourcengruppen und Ressourcen lesen und erstellen und Verwalten von Abonnements Ressource Gruppe Bereitstellungen. Sie können auch die speicherkontoschlüssel, was wiederum bedeutet, dass sie die Datenebene zugreifen können zugreifen.

    -   Benutzer Zugriff Administrator können sie Benutzerzugriff auf das Speicherkonto verwalten. Beispielsweise können sie einen bestimmten Benutzer Lesezugriff erteilen.

    -   Virtual Machine Teilnehmer – können sie verwalten, virtuelle Computer aber nicht das Speicherkonto, mit dem sie verbunden sind. Diese Rolle kann Speicherschlüssel Konto auflisten, d.h. der Benutzer, den diese Rolle zuweisen, die Datenebene aktualisieren kann.

        Damit Benutzer einen virtuellen Computer erstellen müssen sie die zugehörige VHD-Datei in ein Speicherkonto erstellen. Dazu müssen sie Konto Speicherschlüssel abrufen und erstellen die VM API übergeben. Daher müssen sie diese Berechtigung, damit sie Konto Speicherschlüssel auflisten können.

- Die Möglichkeit, benutzerdefinierte Rollen definieren ist ein Feature, mit dem Sie eine Reihe von Aktionen aus einer Liste verfügbarer Aktionen erstellen, die in Azure-Ressourcen ausgeführt werden können.

- Der Benutzer hat in Azure Active Directory festgelegt werden, bevor Sie ihnen eine Rolle zuweisen können.

- Erstellen Sie einen Bericht über die gewährt/welche nach dem und inwieweit mit PowerShell oder CLI Azure gesperrt.

####<a name="resources"></a>Ressourcen

-   [Azure Active Directory rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-configure.md)

    Dieser Artikel beschreibt die Azure Active Directory rollenbasierte Zugriffskontrolle und deren Funktionsweise.

-   [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md)

    Dieser Artikel listet alle verfügbaren RBAC integrierten Rollen.

-   [Grundlegendes zum Ressourcen-Manager und klassischen Bereitstellung](../resource-manager-deployment-model.md)

    Dieser Artikel beschreibt die Ressourcenmanager Bereitstellung und klassischen Bereitstellungsmodelle und erläutert die Vorteile der Verwendung von Gruppen Ressourcenmanager und Ressourcen

-   [Azure Compute, Netzwerk und Speicheranbieter unter Azure Resource Manager](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    In diesem Artikel wird erläutert, wie Azure Compute, Netzwerk und Speicheranbieter Modell der Ressourcen-Manager.

-   [Rollenbasierte Zugriffskontrolle mit der REST-API verwalten](../active-directory/role-based-access-control-manage-access-rest.md)

    Dieser Artikel beschreibt, wie Sie die REST-API RBAC verwalten.

-   [Azure Storage Resource Provider REST-API-Referenz](https://msdn.microsoft.com/library/azure/mt163683.aspx)

    Dies ist die Referenz für die APIs mit das Speicherkonto programmgesteuert verwalten.

-   [Entwicklerhandbuch AUTH Azure-Ressourcen-Manager-API](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

    Dieser Artikel veranschaulicht die Authentifizierung mithilfe der Ressourcen-Manager-APIs.

-   [Rollenbasierte Zugriffskontrolle für Microsoft Azure aus entzünden](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Dies ist eine Verknüpfung mit einem Video auf Channel 9 2015 MS entzünden Konferenz. Diese Sitzung reden über Management und reporting-Funktionen in Azure und best Practices Absicherung von Azure Abonnements Azure Active Directory durchsuchen.

###<a name="managing-your-storage-account-keys"></a>Der Speicherschlüssel Konto verwalten

Speicherkontoschlüssel sind 512-Bit-Zeichenfolgen von Azure, die zusammen mit den speicherkontonamen wird in das Speicherkonto gespeicherten Datenobjekte zugreifen, z. B. blobs Entitäten innerhalb einer Tabelle, Warteschlangennachrichten und Dateien auf einer Freigabe Azure-Dateien erstellt. Steuern des Zugriffs auf den Speicher Konto Schlüssel steuert den Zugriff auf die Datenebene für das Storage-Konto.

Jedes Storage-Konto hat zwei Schlüssel "Key 1" und "Key 2" in der [Azure-Portal](http://portal.azure.com/) und die PowerShell-Cmdlets genannt. Diese können manuell mithilfe einer der folgenden Methoden, einschließlich aber nicht beschränkt auf [Azure-Portal](https://portal.azure.com/), PowerShell, Azure-CLI oder programmgesteuert .NET Speicher-Clientbibliothek oder REST-API von Azure Storage Services generiert.

Es gibt verschiedene Gründe der speicherkontoschlüssel regeneriert.

-   Möglicherweise müssen aus Sicherheitsgründen regelmäßig neu erstellt.

-   Sie möchten Ihr Konto Speicherschlüssel generieren, wenn jemand geschafft hack in eine Anwendung und die Schlüssel, die in einer Konfigurationsdatei gespeichert oder hartcodiert wurde Ihnen uneingeschränkten Zugriff auf das Speicherkonto.

-   Für Schlüssel ist Ihr Team verwendet eine Speicher-Explorer-Anwendung, die Speicherschlüssel Konto behält und eine der Teammitglieder. Die Anwendung wird weiterhin, Zugang zu Ihrem Speicherkonto Nachdem sie Weg sind. Dies ist tatsächlich der Hauptgrund Kontoebene Shared Access Signatures Erstellung – können Sie anstatt der Zugriffstasten in einer Konfigurationsdatei ein SAS Ebene.

####<a name="key-regeneration-plan"></a>Schlüssel-plan

Sie möchten nicht nur den Schlüssel neu erstellen, ohne eine gewisse Planung verwenden. Wenn Sie dies tun, konnte Sie Zugriff auf dieses Speicherkonto abgeschnitten größere Unterbrechung führen. Deshalb gibt es zwei Schlüssel. Gleichzeitig sollte ein Schlüssel neu erstellen.

Bevor die Schlüssel regeneriert müssen Sie eine Liste aller Anwendung, die das Speicherkonto abhängig sind, sowie andere Dienste in Azure verwenden. Beispielsweise verwenden Sie Azure Media Services, die das Speicherkonto abhängig sind, müssen Sie erneut synchronisieren Zugriffstasten Media-Dienst nach den Schlüssel neu erstellen. Wenn Sie eine Anwendung wie Speicher-Explorer verwenden, müssen Sie neue Schlüssel sowie die Anwendung bereitstellen. Beachten Sie, dass haben Sie VMs im Speicherkonto, deren VHD-Dateien gespeichert sind, sie vom Konto Speicherschlüssel Regenerieren unberührt.

Sie können die Schlüssel im Azure-Portal neu generieren. Nach Schlüssel generiert werden, nehmen sie bis zu 10 Minuten in Storage Services synchronisiert werden.

Wenn Sie bereit sind, versteht hier allgemeine Details ändern Ihre Schlüssel sollten. Dabei wird vorausgesetzt, dass Sie derzeit Schlüssel 1 und alles um Schlüssel 2 verwenden möchten.

1.  Regenerieren Sie Schlüssel 2 um sicherzustellen, dass sie sicher ist. Dies ist im Azure-Portal möglich.

2.  Ändern Sie in allen Speicherschlüssel Speicherort Applikationen Speicherschlüssel um neue Taste 2-Wert verwenden. Testen Sie und veröffentlichen Sie die Anwendung.

3.  Programme und Dienste sind und erfolgreich mit Schlüssel 1 regenerieren. Dadurch wird sichergestellt, dass jeder, denen Sie nicht ausdrücklich den neuen Schlüssel angegeben, nicht mehr auf das Speicherkonto.

Verwenden Sie derzeit Schlüssel 2, verwenden Sie dieselbe Vorgehensweise können die Schlüsselnamen umkehren.

Ein paar Tage zu migrieren, jede Anwendung auf den neuen Schlüssel ändern und veröffentlichen. Nachdem alle fertig sind, sollte dann zurück und Regenerieren den alten Schlüssel nicht mehr funktioniert.

Eine weitere Option ist Speicherschlüssel Konto in ein [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) geheim und Ihre Anwendung den Schlüssel dort abrufen. Dann beim Regenerieren des Schlüssels und Aktualisieren von Azure Key Vault müssen Anträge nicht erneut bereitgestellt werden, da sie den neuen Schlüssel aus dem Azure Schlüsseltresor automatisch übernimmt. Beachten Sie, dass die Anwendung der Schlüssel jedes Mal sie gelesen haben oder im Arbeitsspeicher zwischengespeichert werden und schlägt bei Verwendung, den Schlüssel erneut aus dem Azure Schlüsseltresor abrufen.

Mithilfe von Azure Key Vault auch hinzugefügt Sicherheit für Ihre Speicherschlüssel. Wenn Sie diese Methode verwenden, müssen Sie nie Speicher Schlüssel hartcodiert in einer Konfigurationsdatei, der Zugriff auf die Schlüssel Erlaubnis Avenue entfernt.

Ein weiterer Vorteil von Azure Key Vault ist Sie auch Zugriff auf die Schlüssel mithilfe von Azure Active Directory steuern. Dies bedeutet, dass Sie Handvoll Anwendung gewähren können, die die Schlüssel von Azure Key Vault abzurufen und andere Programme nicht auf Schlüssel zugreifen, ohne diese Berechtigung speziell werden.

Hinweis: Es empfiehlt eine der Tasten in allen Anwendung gleichzeitig verwenden. Wenn Sie teilweise 1 und 2 in anderen Schlüssel verwenden, können nicht die Schlüssel ohne einige Zugriff drehen Sie.

####<a name="resources"></a>Ressourcen

-   [Konteninformationen Azure-Speicher](storage-create-storage-account.md#regenerate-storage-access-keys)

    In diesem Artikel Überblick Speicherkonten und anzeigen, kopieren und Regenerieren Speicher Zugriffstasten erläutert.

-   [Azure Storage Resource Provider REST-API-Referenz](https://msdn.microsoft.com/library/mt163683.aspx)

    Dieser Artikel enthält Hyperlinks zu bestimmten Artikeln über Konto Speicherschlüssel abrufen und Regenerieren Konto Speicherschlüssel ein Azure-Konto mithilfe der REST-API. Hinweis: Dies ist für Ressourcenmanager Speicherkonten.

-   [Operationen für Speicherkonten](https://msdn.microsoft.com/library/ee460790.aspx)

    Dieser Artikel in Storage Service Manager REST API Reference enthält Hyperlinks zu bestimmten Artikeln abrufen und regenerieren Sie die REST-API mit speicherkontoschlüssel. Hinweis: Dies ist für Speicherkonten Classic.

-   [Verabschieden Sie sich wichtige Management-Zugriff auf Azure-Speicher Daten mit Azure AD verwalten](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

    Dieser Artikel beschreibt, wie Sie Active Directory Zugriffskontrolle auf der Azure-Speicher in Azure Key Vault. Es wird das Schlüssel auf Stundenbasis neu generieren ein Auftrags Azure Automation veranschaulicht.

##<a name="data-plane-security"></a>Datenschutz-Ebene

Datenschutz-Ebene bezieht sich auf die Methoden zum Sichern der Datenobjekte in Azure Storage-Blobs, Queues, Tabellen und Dateien gespeichert. Gesehen Methoden zum Verschlüsseln von Daten und Sicherheit bei der Übertragung der Daten haben, aber wie wir können Zugriff auf die Objekte?

Es gibt grundsätzlich zwei Methoden zum Steuern des Zugriffs auf die Datenobjekte selbst. Durch Steuern des Zugriffs auf das Konto Speicherschlüssel ist und die zweite Shared Access Signatures gewähren Zugriff auf bestimmte Objekte für eine bestimmte Zeitspanne verwendet.

Eine Ausnahme zu beachten ist, dass Sie können öffentlichen Zugriff auf die Blobs durch die Zugriffsebene für den Container, die Blobs entsprechend festlegen. Wenn Sie Zugriff für einen Container Blob oder Container festlegen, können öffentliche Lesezugriff für Blobs im Container. Dies bedeutet, dass jeder URL für ein Blob im Container es ohne Verwendung einer SAS oder Konto Speicherschlüssel in einem Browser öffnen kann.

###<a name="storage-account-keys"></a>Speicherkontoschlüssel

Speicherkontoschlüssel sind 512-Bit-Zeichenfolgen erstellt von Azure, die zusammen mit dem speicherkontonamen in das Speicherkonto gespeicherten Datenobjekte auf verwendet werden kann.

Sie können z. B. Blobs lesen, schreiben, Warteschlangen, Tabellen erstellen und Dateien ändern. Viele dieser Aktionen erfolgt mithilfe von Azure-Portal oder vielen Speicher-Explorer. Sie können auch die REST-API oder eines Speicher-Clientbibliotheken verwenden, um diese Operationen Code schreiben.

Wie im Abschnitt [Management Ebene Sicherheit](#management-plane-security), kann Zugriff auf Speicherschlüssel für ein Speicherkonto Classic mit Vollzugriff auf den Azure-Abonnement gewährt. Zugriff auf die Speicherschlüssel für ein Speicherkonto mit dem Azure-Ressourcen-Manager-Modell kann durch rollenbasierte Zugriffskontrolle (RBAC) gesteuert werden.

###<a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Zugriff auf Objekte in Ihrem Konto Shared Access Signatures mit gespeicherten Richtlinien delegieren

Eine SAS ist eine Zeichenfolge ein Sicherheitstoken URI zugeordnet werden kann, die Sie Zugriff auf Speicherobjekte delegieren und Einschränkungen wie die Berechtigungen und den Zeitpunkt Zugriff ermöglicht.

Sie können Zugriff auf Blobs, Container Warteschlangennachrichten, Dateien und Tabellen erteilen. Tabellen können tatsächlich gewähren Zugriff auf eine Reihe von Elementen in der Tabelle durch Angabe der Partition und Zeile Schlüsselbereiche, die den Benutzer Zugriff haben soll. Beispielsweise haben Sie Daten mit einer Partition geografischen Status, Sie könnte jemandem Zugriff auf die Daten für Kalifornien.

Beispielsweise können Sie geben einer Anwendung ein SAS-Token, die Einträge in einer Warteschlange schreiben kann und geben eine Arbeitskraft Rolle Anwendung ein SAS-Token zu Nachrichten aus der Warteschlange verarbeiten. Oder könnte ein Kunde ein SAS-Token können sie verwenden, um Bilder zu einem Container im BLOB-Speicher und erteilen der Berechtigung eine Web-Anwendung Bilder lesen. In beiden Fällen steht eine Trennung von Bereichen – jede Anwendung, den sie benötigen, um ihre Aufgabe Zugang gewährt werden kann. Dies ist möglich durch Shared Access Signatures.

####<a name="why-you-want-to-use-shared-access-signatures"></a>Warum möchten Sie Shared Access Signatures

Warum sollten Sie ein SAS anstatt nur Ihre Speicher-Konto-Schlüssel ist einfacher verwenden? Geben Sie Ihr Konto Speicherschlüssel ist wie die Schlüssel des Königreichs der Speicher freigeben. Vollständigen Zugriff gewährt. Jemand die Tasten und ihre Musiksammlung auf das Speicherkonto hochladen. Sie können auch Versionen infizierte Dateien ersetzen oder Ihre Daten stehlen. Entfernt ist uneingeschränkten Zugriff auf das Speicherkonto etwas nicht unterschätzt werden sollte.

Mit freigegebenen Zugriff erhalten einem Clients nur die erforderlichen Berechtigungen für eine begrenzte Zeit Sie. Beispielsweise wenn jemand einen Blob in Ihr Konto Hochladen ist, können Sie Schreibzugriff für gerade genug Zeit (je nach Größe des BLOBs, natürlich) Blob hochladen gewähren. Und wenn Sie Ihre Meinung ändern, können Sie diesen Zugriff widerrufen.

Darüber hinaus können Sie angeben, dass Anfragen mithilfe SAS auf eine bestimmte IP-Adresse oder IP-Adressbereich in Azure. Sie können auch festlegen, dass Anträge sind einem bestimmten Protokoll (HTTPS oder HTTP/HTTPS). Dies bedeutet, wenn Sie HTTPS-Datenverkehr zulassen möchten, Sie HTTPS nur das erforderliche Protokoll festlegen können und HTTP-Datenverkehr blockiert.

####<a name="definition-of-a-shared-access-signature"></a>Definition einer SAS

Eine SAS ist ein Satz von Abfrageparametern, die an die URL der Ressource angehängt

die Informationen über den Zugriff und die Zeitdauer, für die der Zugriff zulässig ist. Hier ist ein Beispiel. Dieser URI bietet Lesezugriff auf ein Blob für fünf Minuten. Beachten Sie, dass SAS Parameter Abfragen muss wie % 3A Doppelpunkt (:) oder ein Leerzeichen %20 URL codiert sein.

    http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
    ?sv=2015-04-05 (storage service version)
    &st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
    &se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
    &sr=b (resource is a blob)
    &sp=r (read access)
    &sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
    &spr=https (only allow HTTPS requests)
    &sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####<a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Wie die SAS von Azure Storage Service authentifiziert

Wenn der Speicherdienst die Anforderung empfängt, die Eingabeabfrage Parameter und erstellt eine Signatur mit demselben Verfahren wie das aufrufende Programm. Anschließend werden die zwei Signaturen verglichen. Wenn sie zustimmen, finden der Speicherdienst Storage Service-Version um sicherzustellen, dass sie gültig ist, überprüfen Sie, ob das aktuelle Datum und die Uhrzeit im angegebenen Fenster, stellen sicher, dass der angeforderte Zugriff auf Anforderung usw. entspricht.

Beispielsweise mit unserer oben genannten URL würde Wenn die URL auf eine Datei statt eines BLOBs verweist diese Anforderung fehl, da es gibt an, dass die SAS für ein Blob. Wurde einen Blob aktualisiert der REST-Befehl aufgerufen würde es fehl, da die SAS gibt an, dass nur Lesezugriff zulässig ist.

####<a name="types-of-shared-access-signatures"></a>Arten von SAS

-   Servicelevel-SAS kann verwendet werden, Zugriff auf bestimmte Ressourcen in ein Speicherkonto. Beispiele hierfür sind eine Liste von Blobs in einem Container einen Blob herunterladen, Aktualisieren einer Entität in einer Tabelle, Nachrichten an eine Warteschlange hinzufügen oder Hochladen einer Datei in einer Dateifreigabe abrufen.

-   Eine SAS Ebene kann verwendet werden, auf etwas zuzugreifen, die Service Level-SAS für verwendet werden kann. Es kann außerdem Optionen auf Ressourcen gewähren, die nicht mit einem SAS Service Level wie Container, Tabellen, Warteschlangen und Dateifreigaben erstellen. Sie können auch gleichzeitig auf mehrere Dienste angeben. Angenommen, Sie können jemandem Blobs und Dateien in Ihrem Speicherkonto zugreifen.

####<a name="creating-an-sas-uri"></a>Ein SAS-URI erstellen

1.  Einen ad-hoc-URI können bei Bedarf alle Abfrageparameter jedem definiert.

    Dies ist sehr flexibel, aber haben Sie eine logische Gruppe von Parametern, die jedem ähneln, mit einer Richtlinie gespeichert ist besser.

2.  Sie können eine gespeicherte Zugriffsrichtlinie gesamten Container, Dateifreigabe, Tabelle oder Warteschlange erstellen. Dann können Sie dies als Grundlage für SAS-URIs verwenden erstellten. Berechtigungen basierend auf gespeicherten Richtlinien können leicht aufgehoben werden. Bis zu 5 Richtlinien für jeden Container, Warteschlange, Tabelle oder Dateifreigabe definiert haben.

    Beispielsweise wollten Sie viele Blobs in einem bestimmten Container gelesen haben, können Sie eine Zugriffsrichtlinie gespeichert erstellen "Lesezugriff erteilen" und alle anderen Einstellungen jedes Mal werden. Sie können eine SAS-URI Einstellungen der Richtlinie gespeichert und das Ablaufdatum/-Uhrzeit erstellen. Der Vorteil ist, dass die Abfrage Parameter jedes Mal angeben müssen.

####<a name="revocation"></a>Sperrung

Angenommen Sie, Ihre SAS kompromittiert oder Unternehmens oder gesetzliche Vorschriften und Richtlinien ändern möchten. Wie widerrufen Sie Zugriff auf eine Ressource mit diesem SAS? Das hängt davon ab, wie SAS-URI erstellt.

Wenn Sie ad-hoc-URI verwenden, haben Sie drei Optionen. SAS-Token mit kurzen Ablaufrichtlinien ausstellen, und warten Sie SAS abläuft. Sie können umbenennen oder löschen die Ressource (vorausgesetzt, das Token auf ein einzelnes Objekt beschränkt wurde). Sie können die speicherkontoschlüssel ändern. Letzteres kann gravierende Folgen, je nachdem, wie viele Dienste diesem Storage-Konto verwenden und wahrscheinlich nicht ohne Planung wollen.

Bei Verwendung von SAS abgeleitet Zugriffsrichtlinie für ein gespeichert entfernen Zugriff durch Aufheben der Richtlinie gespeichert – Sie können einfach ändern sie bereits abgelaufen oder ganz entfernen. Dies wird sofort wirksam und erklärt alle SAS erstellt mit einer Richtlinie Zugriff gespeichert. Aktualisieren oder Entfernen der Richtlinie gespeichert können Konsequenzen für die Personen den Zugriff auf bestimmte Container Dateifreigabe, Tabelle oder Warteschlange über SAS, aber wenn die Clients wurden, damit sie ein neuen SAS anfordern, wird die alte Version ungültig, dies funktioniert einwandfrei.

Da mithilfe SAS abgeleitet Zugriffsrichtlinie für ein gespeichert Ihnen die Möglichkeit gibt, SAS sofort sperren, wird empfohlen Zugriffsrichtlinien gespeichert Wenn möglich mit.

####<a name="resources"></a>Ressourcen

Weitere Informationen zu freigegebenen Zugriff Signaturen mit gespeicherten Richtlinien, mit Beispielen finden Sie in folgenden Artikeln:

-   Dies sind die Referenzartikel.

    -   [Dienst-SAS](https://msdn.microsoft.com/library/dn140256.aspx)

        Dieser Artikel enthält Beispiele für Blobs, Warteschlangennachrichten, Bereiche und Dateien mit SAS Service Level.

    -   [Erstellen eines Dienstes SAS](https://msdn.microsoft.com/library/dn140255.aspx)

    -   [Erstellen eines Kontos SAS](https://msdn.microsoft.com/library/mt584140.aspx)

-   Lernprogramme für die Verwendung der Clientbibliothek .NET Shared Access Signatures und Zugriffsrichtlinien gespeichert sind.

    -   [Mithilfe SAS (SAS)](storage-dotnet-shared-access-signature-part-1.md)

    -   [Freigegebene Access Signaturen, Teil 2: Erstellen und Verwenden von SAS mit BLOB-Dienst](storage-dotnet-shared-access-signature-part-2.md)

        Dieser Artikel enthält eine Erläuterung der SAS-Modell Shared Access Signatures-Beispiele und Vorschläge für die besten mithilfe SAS. Ebenfalls ist die Sperrung der Berechtigung.

-   Einschränken des Zugriffs nach IP-Adresse (IP-ACLs)

    -   [Was ist ein Endpunkt Zugriffssteuerungsliste (ACLs)?](../virtual-network/virtual-networks-acl.md)

    -   [Erstellen eines Dienstes SAS](https://msdn.microsoft.com/library/azure/dn140255.aspx)

        Dies ist im Referenzartikel für Servicelevel SAS. Es enthält ein Beispiel für IP-ACLing.

    -   [Erstellen eines Kontos SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx)

        Dies ist im Referenzartikel für SAS Ebene. Es enthält ein Beispiel für IP-ACLing.

-   Authentifizierung

    -    [Authentifizierung für Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   Shared Access Signatures erste Schritte abschließen

    -   [SAS-erste Schritte](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##<a name="encryption-in-transit"></a>Verschlüsselung in Transit

###<a name="transport-level-encryption--using-https"></a>Verschlüsselung auf Transportebene – mit HTTPS

Ein weiterer Schritt sollten Sie für die Sicherheit Ihrer Daten Azure Storage wird zum Verschlüsseln der Daten zwischen dem Client und dem Azure-Speicher. Die erste Empfehlung ist, immer das [HTTPS](https://en.wikipedia.org/wiki/HTTPS) -Protokoll sorgt für sichere Kommunikation über das Internet.

REST-APIs aufrufen oder den Zugriff auf im Speicher Objekte sollten Sie immer HTTPS verwenden. **Shared Access Signatures**Zugriff auf Azure Storage Objekte delegieren verwendet werden können, gehören eine Option festlegen, dass freigegebene Access Signaturen, damit jemand mit SAS-Token senden das richtige Protokoll verwendet das HTTPS-Protokoll verwendet werden kann.

####<a name="resources"></a>Ressourcen

-   [Aktivieren Sie HTTPS für eine Anwendung in Azure App Service](../app-service-web/web-sites-configure-ssl-certificate.md)

    Dieser Artikel veranschaulicht das HTTPS für ein Azure Web App aktivieren.

###<a name="using-encryption-during-transit-with-azure-file-shares"></a>Verschlüsselung während der Übertragung mit Azure-Dateifreigaben

Azure Dateispeicher unterstützt HTTPS, wenn die REST-API verwenden ist, jedoch mehr als eine SMB-Dateifreigabe angehängt einer VM. SMB 2.1 unterstützt keine Verschlüsselung, damit Verbindungen nur innerhalb derselben Region in Azure zulässig sind. SMB 3.0 unterstützt Verschlüsselung und kann mit Windows Server 2012 R2 verwendet jedoch Windows 8, Windows 8.1 und Windows-10 Cross-Region zugreifen und sogar Zugriff auf dem Desktop.

Beachten Sie, dass während Azure Dateifreigaben mit Unix verwendet werden können, Linux SMB-Client noch Verschlüsselung nicht unterstützt, der Zugriff ist nur innerhalb einer Azure-Region. Unterstützung für Linux Verschlüsselung ist Roadmap Linux Entwickler für SMB-Funktionen verantwortlich. Wenn sie Verschlüsselung hinzufügen, müssen Sie dieselbe Fähigkeit auf einer Dateifreigabe Azure unter Linux wie für Windows.

####<a name="resources"></a>Ressourcen

-   [Verwendung von Azure Dateispeicher mit Linux](storage-how-to-use-files-linux.md)

    Dieser Artikel beschreibt, wie eine Azure-Dateifreigabe auf einem linuxsystem und Upload/Download von Dateien.

-   [Erste Schritte mit Windows Azure Dateispeicher](storage-dotnet-how-to-use-files.md)

    Dieser Artikel bietet eine Übersicht über PowerShell und .NET Azure Dateifreigaben und bereitstellen und verwenden.

-   [In Azure Dateispeicher](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    Dieser Artikel kündigt die allgemeine Verfügbarkeit von Azure Dateispeicher und enthält technische Details zur Verschlüsselung SMB 3.0.

###<a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Clientseitige Verschlüsselung Sicherung an Speicher Daten

Eine weitere Option, die gewährleistet, dass Ihre Daten sicher sind und zwischen einer Clientanwendung und Speicher ist clientseitige Verschlüsselung. Die Daten werden verschlüsselt, bevor in Azure-Speicher übertragen. Beim Abrufen der Daten aus dem Azure-Speicher entschlüsselt die Daten nach dem Empfang auf dem Client. Obwohl die Daten über das Netzwerk verschlüsselt werden, empfiehlt auch HTTPS wie Prüfung der Datenintegrität in der mindern, Netzwerkfehler auf die Integrität der Daten erstellt.

Clientseitige Verschlüsselung ist auch eine Methode zum Verschlüsseln der Daten, die Daten in verschlüsselter Form gespeichert werden. Wir werden ausführlicher im Abschnitt zur [Verschlüsselung ruhender](#encryption-at-rest)sprechen.

##<a name="encryption-at-rest"></a>Verschlüsselung ruhender

Es gibt drei Azure-Funktionen, die Verschlüsselung ruhender. Azure Datenträgerverschlüsselung zum Betriebssystem und Daten Datenträger IaaS virtuelle Computer verschlüsseln. Die anderen beiden – clientseitige Verschlüsselung und SSE-sind sowohl zur Verschlüsselung in Azure-Speicher verwendet. Wir betrachten, einem Vergleich und finden Sie unter jeder Verwendung.

Während Sie clientseitige Verschlüsselung zum Verschlüsseln der Daten während der Übertragung (die auch in ihrer verschlüsselten Form im Speicher gespeichert wird) verwenden können, empfiehlt es sich einfach HTTPS verwenden, während der Übertragung und irgendwie die Daten beim Speichern automatisch verschlüsselt. Es gibt zwei Möglichkeiten – Azure Disk Encryption und SSE. Einer dient zum Verschlüsseln von Daten auf Betriebssystem und Daten von virtuellen Computern verwendete Datenträger direkt und der zweite dient zum Verschlüsseln von Daten in Azure BLOB-Speicher geschrieben.

###<a name="storage-service-encryption-sse"></a>Storage Service Encryption (SSE)

SSE können Sie anfordern, dass der Speicherdienst automatisch Verschlüsseln von Daten beim Schreiben in Azure-Speicher. Beim Lesen der Daten aus dem Azure-Speicher wird es vom Speicherdienst entschlüsselt vor. Dadurch können Sie sichern Sie Ihre Daten ohne Code ändern oder jede Anwendung Code hinzufügen.

Dies ist eine Einstellung für die gesamte Speicherkonto. Aktivieren und deaktivieren Sie diese Funktion den Wert der Einstellung ändern. Hierzu können Sie Azure-Portal, PowerShell, Azure-CLI, der Ressourcen-REST API-Provider oder .NET Speicher-Clientbibliothek. SSE ist deaktiviert.

Zu diesem Zeitpunkt werden die Schlüssel für die Verschlüsselung von Microsoft verwaltet. Wir ursprünglich Schlüssel generieren, und die sichere Speicherung von Schlüssel sowie regelmäßige Rotation von internen Microsoft-Richtlinie definiert. In Zukunft Sie erhalten die Möglichkeit, Ihre eigenen Verschlüsselungsschlüssel verwalten, und bieten einen Migrationspfad von Microsoft verwalteten Schlüsseln auf Kunden verwaltet.

Dieses Feature steht für Standard und Premium-Speicher mit dem Ressourcen-Manager-Bereitstellungsmodell erstellt. SSE gilt nur für Blobs Seitenblobs blockieren und Blobs anfügen. Andere Arten von Daten, einschließlich Tabellen, Warteschlangen und Dateien werden nicht verschlüsselt.

Daten werden nur verschlüsselt, wenn SSE aktiviert ist und die Daten in den BLOB-Speicher geschrieben. Aktivieren oder Deaktivieren von SSE wirkt sich nicht auf vorhandene Daten aus. In anderen Worten, wenn Sie die Verschlüsselung aktivieren, es nicht zurück und bereits Daten verschlüsseln; auch wird beim Deaktivieren von SSE bereits Daten entschlüsseln.

Wenn Sie dieses Feature mit einer klassischen Speicherkonto verwenden möchten, können Sie erstellen ein neues Speicherkonto Ressourcenmanager und AzCopy verwenden, um die Daten in das neue Konto kopieren. 

###<a name="client-side-encryption"></a>Clientseitige Verschlüsselung

Bei der Verschlüsselung der Daten bei der Übertragung erwähnten clientseitigen Verschlüsselung. Dadurch können Sie programmgesteuert Verschlüsselung die Daten in einer Clientanwendung vor dem Senden über das Netzwerk in Azure-Speicher geschrieben werden und programmgesteuert entschlüsselt die Daten nach dem Abrufen von Azure-Speicher.

Dies bietet Verschlüsselung während der Übertragung, sondern bietet auch die Funktion der Verschlüsselung ruhender. Obwohl die Daten während der Übertragung mit HTTPS empfohlen zu integrierten Datenintegrität überprüft die Hilfe Netzwerkfehler auf die Integrität der Daten zu verringern.

Ein Beispiel, in dem Sie diese verwenden können ist Wenn Sie eine Anwendung, die Blobs speichert und abruft Blobs, und der Anwendung und Daten so sicher wie möglich sein soll. In diesem Fall verwenden Sie clientseitige Verschlüsselung. Der Datenverkehr zwischen dem Client und Azure BLOB-Dienst enthält verschlüsselte Ressourcen und niemand Interpretation der Daten bei der Übertragung und in Ihrem privaten Blobs wiederherstellen.

Clientseitige Verschlüsselung integriert die Java und .NET Speicher Clientbibliotheken, die wiederum sehr einfaches implementieren Azure Key Vault APIs verwenden. Der Vorgang des Ver- und Entschlüsseln der Daten verwendet den Umschlag und speichert Metadaten der Verschlüsselung jedes Objekt verwendet werden. Beispielsweise für Blobs, sondern es in den Metadaten Blob für Warteschlangen hinzugefügt es jede warteschlangennachricht.

Für die Verschlüsselung selbst können Sie generieren und Verwalten eigener Verschlüsselungsschlüssel. Können Sie auch von Azure Storage-Clientbibliothek generierten Schlüssel und Azure Key Vault Schlüssel generiert wurden. Speichern von Verschlüsselungsschlüsseln in Ihrem lokalen Schlüsselspeicher oder in ein Depot Azure Schlüssel speichern. Azure Key Vault können Sie Azure Active Directory mit Benutzern Zugriff auf vertrauliche Informationen in Azure Key Vault erteilen. Dies bedeutet, dass nicht jeder Azure Key Vault und kann für die clientseitige Verschlüsselung verwendeten Schlüssel abzurufen.

####<a name="resources"></a>Ressourcen

-   [Verschlüsseln und Entschlüsseln von Blobs in Azure Key Vault mit Microsoft Azure-Speicher](storage-encrypt-decrypt-blobs-key-vault.md)

    Dieser Artikel beschreibt wie Sie clientseitige Verschlüsselung mit Azure Schlüssel, wie die KEK erstellen und im Tresor mit PowerShell speichern.

-   [Clientseitige Verschlüsselung und Azure Key Vault für Microsoft Azure-Speicher](storage-client-side-encryption.md)

    Dieser Artikel erläutert eine clientseitige Verschlüsselung und bietet Beispiele für die Verwendung von Speicher-Clientbibliothek zum Verschlüsseln und Entschlüsseln von Ressourcen aus vier Speicherservices. Es spricht auch Azure Key Vault.

###<a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Verschlüsselung Azure Datenträger zum Verschlüsseln von virtuellen Computern verwendete Datenträger

Azure Verschlüsselung von Festplatten ist ein neues Feature, das derzeit in der Vorschau. Dieses Feature können Sie OS Festplatten und Datenträger verwendet einen IaaS virtuellen Computer verschlüsseln. Unter Windows werden die Laufwerke mit Industriestandard BitLocker-Verschlüsselung verschlüsselt. Unter Linux werden die Festplatten mit DM-Crypt-Technologie verschlüsselt. Dies ist in Azure Key Vault zu steuern und die Datenträger-Verschlüsselungsschlüssel verwalten integriert.

Azure Disk Encryption-Lösung unterstützt die folgenden drei Kunden Verschlüsselung Szenarios:

-   Aktivieren der Verschlüsselung für neue IaaS VMs erstellt von VHD-Dateien Kunden verschlüsselt und vom Kunden bereitgestellter Schlüssel in Azure Schlüssel Depot gespeichert sind.

-   Aktivieren der Verschlüsselung für neue IaaS VMs Azure Marketplace erstellt.

-   Aktivieren der Verschlüsselung für vorhandene IaaS VMs bereits in Azure ausgeführt.

>[AZURE.NOTE] Für Linux VMs bereits in Azure oder neue Linux-VMs erstellt Bilder in Azure Marketplace ist Verschlüsselung des Betriebssystem-Datenträgers derzeit nicht unterstützt. Verschlüsselung für Linux VMs-BS-Volume wird nur für VMs unterstützt, die in Azure hochgeladen und lokal verschlüsselt wurden. Diese Einschränkung gilt nur für die Betriebssystem-Datenträger; Verschlüsselung von Daten-Volumes für Linux VM wird unterstützt.

Die Lösung unterstützt die folgenden für IaaS VMs für public Preview-Version in Microsoft Azure aktiviert:

-   Integration in Azure-Tresor

-   [A, D und G Serie IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)

-   Aktivieren der Verschlüsselung für IaaS VMs erstellt [Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md) Modell

-   Alle Azure öffentliche [Bereiche](https://azure.microsoft.com/regions/)

Diese Funktion stellt sicher, dass alle Daten auf der virtuellen Festplatten ruhende in Azure Storage verschlüsselt.

####<a name="resources"></a>Ressourcen

-   [Azure Verschlüsselung Windows und Linux IaaS virtuelle Computer](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    Dieser Artikel beschreibt die Vorabversion von Azure Datenträgerverschlüsselung und enthält einen Link, um das Whitepaper herunterzuladen.

###<a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Vergleich von Azure Disk Encryption, SSE und clientseitige Verschlüsselung

####<a name="iaas-vms-and-their-vhd-files"></a>IaaS VMs und die VHD-Dateien

Bei IaaS VMs verwendet empfehlen wir Azure Datenträgerverschlüsselung. SSE VHD-Dateien verschlüsseln, die verwendet werden, die Festplatten in Azure-Speicher wieder einschalten, aber neu geschriebene Daten nur dann verschlüsselt. Wenn Sie einen virtuellen Computer erstellen und aktivieren Sie SSE auf das Speicherkonto, die VHD-Datei enthält, wird nur die Änderungen verschlüsselt werden dadurch nicht die ursprüngliche VHD-Datei.

Wenn Sie einen virtuellen Computer erstellen Bild Azure Marketplace verwenden Azure führt eine [flache Kopie](https://en.wikipedia.org/wiki/Object_copying) des Bildes, das Speicherkonto in Azure Storage und ist nicht verschlüsselt, selbst wenn Sie SSE aktiviert haben. Nachdem die VM erstellt und das Bild aktualisiert wird, startet SSE Verschlüsseln der Daten. Aus diesem Grund empfiehlt es sich mit Azure Datenträgerverschlüsselung auf VMs von Bildern in Azure Marketplace erstellt, wenn Sie vollständig verschlüsselt werden sollen.

Vor der Verschlüsselung VM in Azure lokal mitbringen, werden Sie die Verschlüsselungsschlüssel auf Azure Schlüssel Depot hochladen und weiterhin die Verschlüsselung für die VM mit lokalen verwendet wurden. Azure Datenträger-Verschlüsselung ist für dieses Szenario aktiviert.

Haben Sie VHD lokal nicht verschlüsselt, können der Galerie ein benutzerdefiniertes Bild hochladen und Bereitstellen eine VM aus. Sollten mithilfe der Ressourcen-Manager-Vorlagen können Sie bitten sie beim Hochfahren der VM Azure Datenträgerverschlüsselung aktivieren.

Wenn Sie Datenträger und auf dem virtuellen Computer bereitstellen, können Sie Azure Datenträgerverschlüsselung auf diesem Datenträger aktivieren. Es zuerst lokal, Datenträger verschlüsselt, und führen Sie dann die Service Management-Ebene wird lazy Write gegen so der Inhalt verschlüsselt wird.

####<a name="client-side-encryption"></a>Clientseitige Verschlüsselung####

Clientseitige Verschlüsselung ist die sicherste Methode, Ihre Daten vor der Übertragung verschlüsselt und verschlüsselt die Daten zu verschlüsseln. Es erfordert, dass Sie Code hinzufügen, um Ihre Anwendung mit Speicher, was Sie tun möchten. In diesen Fällen können HTTPs für Ihre Daten bei der Übertragung und SSE Sie die Daten verschlüsseln.

Clientseitige Verschlüsselung Verschlüsseln Sie Tabellenentitäten, Warteschlangennachrichten und Blobs. Mit SSE können nur Blobs verschlüsselt werden. Benötigen Sie Tabellen- und Warteschlange Daten verschlüsselt werden, sollten Sie die clientseitige Verschlüsselung verwenden.

Clientseitige Verschlüsselung wird von der Anwendung verwaltet. Dies ist der sicherste Ansatz, erfordert jedoch programmgesteuerten Änderung zur Anwendung und wichtige Prozesse eingeführt. Dieser ist verwenden, wenn Sie zusätzliche Sicherheit während der Übertragung, und die gespeicherten Daten verschlüsselt werden sollen.

Clientseitige Verschlüsselung mehr Auslastung auf dem Client ist und Sie diese in Ihre Pläne Skalierbarkeit berücksichtigt insbesondere dann, wenn Sie verschlüsseln und große Datenmengen übertragen haben.

####<a name="storage-service-encryption-sse"></a>Storage Service Encryption (SSE)

SSE wird von Azure-Speicher. Mit SSE bietet keine Sicherheit übertragen, verschlüsselt werden jedoch die Daten schreiben in Azure-Speicher. Gibt es keine Auswirkung auf die Leistung beim Verwenden dieses Features.

Sie können nur verschlüsseln Block-Blobs Anhängen Blobs und Seite SSE mit Blobs. Benötigen Sie Tabellendaten oder Warteschlangendaten verschlüsseln, sollten Sie clientseitige Verschlüsselung.

Haben Sie ein Archiv oder eine Bibliothek von VHD-Dateien, die Sie als Grundlage zum Erstellen neuen virtueller Maschinen verwenden, können Sie erstellen ein neues Speicherkonto SSE aktivieren und VHD-Dateien auf dieses Konto hochladen. Die VHD-Dateien werden von Azure Storage verschlüsselt.

Wenn Sie Azure Datenträgerverschlüsselung der Laufwerke in einer VM und SSE Speicherkonto mit den VHD-Dateien aktiviert haben, wird es funktionieren; Es führt neu geschriebene Daten zweimal verschlüsselt werden.

##<a name="storage-analytics"></a>Speicheranalyse

###<a name="using-storage-analytics-to-monitor-authorization-type"></a>Verwenden zum Überwachen der Autorisierungstyp Speicheranalyse

Jedes Storage-Konto können Sie Azure Speicheranalyse Protokollierung und Metriken speichern aktivieren. Dies ist ein hervorragendes Tool zu verwenden, wenn die Leistungsmetrik ein Speicherkonto überprüfen möchten oder ein Speicherkonto, denn Leistungsprobleme beheben müssen.

Eine weitere ist Daten finden Sie in den Protokollen Storage Analytics die von einem Benutzer beim Zugriff auf Speicher verwendete Authentifizierungsmethode. Beispielsweise mit BLOB-Speicher sehen Sie einen SAS oder Konto Speicherschlüssel verwendet oder öffentlichen Blob zugegriffen wurde.

Dies ist sehr hilfreich, wenn Sie Zugriff auf Speicher eng bewachen. Beispielsweise können Sie im BLOB-Speicher alle Container auf private festgelegt und ein SAS-Dienst in Ihre Anwendung implementieren. Anschließend können Sie die Protokolle regelmäßig, ob die Blobs erfolgt Tasten Storage Konto, was auf eine Verletzung der Sicherheit hinweisen kann, oder wenn die Blobs sind öffentlich, aber sie sollten nicht überprüfen.

####<a name="what-do-the-logs-look-like"></a>Sehen die Protokolle wie?

Nach dem Aktivieren Storage Konto Metriken und Protokollierung durch Azure-Portal Analysedaten schnell anhäufen gestartet. Protokollierung und Metriken für jeden Dienst unterscheidet; die Protokollierung wird nur geschrieben, wenn es Aktivitäten in diesem Storage-Konto während jede Minute, stündlich oder täglich je nachdem wie Sie konfigurieren die Metrik protokolliert werden.

Die Protokolle werden im Block-Blobs in einem Container mit dem Namen $logs im Speicherkonto gespeichert. Dieser Container wird automatisch erstellt, wenn Speicheranalyse aktiviert ist. Erstellte Container kann nicht, obwohl Sie dessen Inhalt löschen können gelöscht werden.

Im Container $logs ein Ordner für jeden Dienst und gibt dann Unterordner Jahr/Monat/Tag/Stunde. Stunde werden die Protokolle einfach nummeriert. Wird die Verzeichnisstruktur aussehen:

![Anzeigen der Protokolldateien](./media/storage-security-guide/image1.png)

Jede Anforderung in Azure Storage wird protokolliert. Hier ist eine Momentaufnahme einer Protokolldatei, die ersten Paar Felder angezeigt.

![Snapshot einer Protokolldatei](./media/storage-security-guide/image2.png)

Sie können sehen, dass Sie beliebige Aufrufe ein Speicherkonto verfolgen die Protokolle verwenden können.

####<a name="what-are-all-of-those-fields-for"></a>Was sind die Felder für?

Ist ein Artikel in den Ressourcen unten enthält eine Liste der viele Felder in den Protokollen und deren Verwendung. Hier ist die Liste der Felder in der Reihenfolge:

![Snapshot der Felder in einer Protokolldatei](./media/storage-security-guide/image3.png)

Wir wollen die Einträge für GetBlob und wie sie authentifiziert werden, müssen wir suchen nach Einträgen mit "Get-Blob-Vorgangstyp sowie der Anforderungsstatus (<sup>ten</sup> Spalte 4) und der Autorisierungstyp (<sup>ten</sup> Spalte 8).

In den ersten Zeilen in der obigen Liste, z. B. der Anforderungsstatus wird "Erfolg" und der Autorisierungstyp "authentifiziert". Dies bedeutet, dass die Anforderung mit Speicherschlüssel Konto überprüft wurde.

####<a name="how-are-my-blobs-being-authenticated"></a>Wie werden meine Blobs identifiziert?

Wir haben drei Fälle, denen uns interessiert.

1.  Das Blob ist öffentlich, und der Zugriff erfolgt über einen URL ohne eine SAS. In diesem Fall den Status der Anfrage ist "AnonymousSuccess" und der Autorisierungstyp ist "Anonym".

    1.0, 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**200 124; 37; **anonyme**; Mystorage...

2.  Das Blob ist privat und mit einem SAS verwendet wurde. In diesem Fall den Status der Anfrage ist "SASSuccess" und der Autorisierungstyp ist "Sas".

    1.0, 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**200 416; 64; **Sas**; Mystorage...

3.  Das Blob ist privat und Speicherschlüssel Zugriff verwendet wurde. In diesem Fall ist des Anforderungsstatus "**Erfolg**" und der Autorisierungstyp ist "**Authentifizierung**".

    1.0, 2015-11-16T18:32:24.3174537Z; GetBlob; **Erfolg**206 59; 22; **authentifiziert**; Mystorage...

Sie können Microsoft Message Analyzer anzeigen und Analysieren von Ereignisprotokollen. Er enthält Funktionen für Suche und Filter. Angenommen, Sie möchten suchen Instanzen von GetBlob auf der Verbrauch ist, d. h. um sicherzustellen, dass jemand greift das Speicherkonto fälschlicherweise nicht.

####<a name="resources"></a>Ressourcen

-   [Speicheranalyse](storage-analytics.md)

    Dieser Artikel bietet eine Übersicht über Speicheranalyse und wie Sie sie aktivieren.

-   [Storage Analytics Protokollformat](https://msdn.microsoft.com/library/azure/hh343259.aspx)

    Dieser Artikel veranschaulicht das Format Storage Analytics und werden Felder, einschließlich Authentifizierungstyp, der für die Anforderung verwendete Authentifizierungstyp angibt.

-   [Ein Speicherkonto in Azure-Portal überwachen](storage-monitor-storage-account.md)

    Dieser Artikel beschreibt, wie Metriken überwachen und Protokollieren für ein Speicherkonto konfigurieren.

-   [End-to-End-Problembehandlung mit Azure Storage Metriken und Protokollierung, AzCopy und Message Analyzer](storage-e2e-troubleshooting.md)

    In diesem Artikel zur Problembehandlung mit der Speicheranalyse spricht und zeigt, wie Microsoft Message Analyzer.

-   [Für Microsoft Message Analyzer-Betrieb](https://technet.microsoft.com/library/jj649776.aspx)

    Dieser Artikel ist der Verweis auf die Microsoft Message Analyzer und beinhaltet ein Lernprogramm, Schnellstart und Übersicht der Funktionen.

##<a name="cross-origin-resource-sharing-cors"></a>Cross-Ursprung Ressourcenfreigabe (CORS)

###<a name="cross-domain-access-of-resources"></a>Domänenübergreifender Zugriff auf Ressourcen

Bei ein Webbrowser in einer Domäne mit einer HTTP-Anforderung für eine Ressource aus einer anderen Domäne, ist dies eine Cross-Ursprung HTTP-Anforderung bezeichnet. Eine HTML-Seite von contoso.com wird beispielsweise eine Anforderung für eine JPEG-fabrikam.blob.core.windows.net gehostet. Aus Sicherheitsgründen beschränkt Browser Cross stammende HTTP-Anfragen innerhalb Skripts wie JavaScript. Dies bedeutet, dass wenn JavaScript-Code auf einer Webseite auf contoso.com, Jpeg fabrikam.blob.core.windows.net anfordert, der Browser nicht die Anforderung.

Was hat dies mit Azure-Speicher? Auch wenn Sie statische Elemente wie JSON oder XML-Dateien speichern im BLOB-Speicher verwenden ein Speicherkonto namens Fabrikam Bereich der Anlagen werden fabrikam.blob.core.windows.net und contoso.com Anwendung nicht darauf zugreifen mit JavaScript, da Domänen unterschiedlich sind. Dies gilt auch, wenn Sie eine Azure-Speicherdienste – wie Tabellenspeicher – zurück, die JSON-Daten von der JavaScript-Client verarbeitet werden.

####<a name="possible-solutions"></a>Lösungsvorschläge

Eine Möglichkeit zur Lösung des Problems ist fabrikam.blob.core.windows.net eine benutzerdefinierte Domäne wie "storage.contoso.com" zugewiesen. Das Problem ist, dass benutzerdefinierte Domäne nur ein Konto zuweisen können. Wenn die Anlagen in mehrere Storage-Konten gespeichert werden?

Eine andere Möglichkeit zur Lösung des Problems wird als Proxy für die Storage-Aufrufe der Anwendung haben. Dies bedeutet, wenn Sie einer Datei auf BLOB-Speicher Hochladen der Anwendung würde entweder lokal geschrieben und Blob-Speicher kopieren oder es aller in den Speicher lesen und Schreiben auf BLOB-Speicher. Alternativ können Sie eine dediziertes Web-Anwendung (z. B. eine Web-API) schreiben, die lädt die Dateien lokal und Blob-Speicher schreibt. In beiden Fällen müssen Sie diese Funktion berücksichtigt bei der Skalierung bestimmen muss.

####<a name="how-can-cors-help"></a>Helfen CORS?

Azure-Speicher können Sie CORS-Ursprung Ressourcenfreigabe Cross aktivieren. Für jedes Storage-Konto können Sie Domänen angeben, die die Ressourcen in diesem Storage-Konto zugreifen können. Beispielsweise können wir in unserem beschriebenen Speicherkonto fabrikam.blob.core.windows.net CORS aktivieren und konfigurieren Zugriff auf contoso.com. Dann können Web Application contoso.com Ressourcen in fabrikam.blob.core.windows.net direkt zugreifen.

Zu beachten ist, dass CORS Zugriff bietet keine Authentifizierung für alle nicht-öffentlichen Zugriff auf Speicherressourcen erforderlich ist. Dies bedeutet, dass Blobs nur zugreifen können, wenn sie öffentliche oder Sie eine SAS Geben Sie die entsprechende Berechtigung. Tabellen, Warteschlangen und Dateien keinen öffentlichen Zugriff und SAS erforderlich.

CORS ist für alle Dienste deaktiviert. CORS können mithilfe der REST-API oder der Speicher-Clientbibliothek Aufruf einer der Methoden die Richtlinien festgelegt. Wenn Sie dies tun, können Sie CORS in der Regel in XML. Hier ist ein Beispiel einer CORS-Regel, die mithilfe des Vorgangs legen Sie Eigenschaften für den BLOB-Dienst für ein Speicherkonto festgelegt wurde. Sie können diesen Vorgang mit Speicher-Clientbibliothek oder den REST-APIs für Azure-Speicher ausführen.

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Hier bedeutet jede Zeile:

-   **AllowedOrigins** Dies weist nicht übereinstimmende Domains anfordern und Empfangen von Daten vom Speicherdienst. Das bedeutet dass contoso.com und fabrikam.com Daten-BLOB-Speicher für ein bestimmtes Konto anfordern können. Zudem lassen sich diese auf einen Platzhalter (\*) zu allen Domänen auf Anfragen.

-   **AllowedMethods** Dies ist die Liste der Methoden (HTTP-Verben), die der Anforderung verwendet werden kann. In diesem Beispiel dürfen nur PUT und GET. Sie können dies festlegen, um einen Platzhalter (\*) alle Methoden ermöglichen.

-   **AllowedHeaders** Dies ist der Anforderungsheader, die bei der Anforderung die Ursprungsdomäne angeben können. In diesem Beispiel werden alle Metadaten-Header X-ms-Metadaten wobei, X-ms-Meta-Ziel und X-ms-Meta-Abc zulässig. Das Platzhalterzeichen (\*) gibt an, dass alle Header beginnend mit dem angegebenen Präfix zulässig ist.

-   **ExposedHeaders** Veranlaßt die Antwortheader der Browser die Anforderung Emittenten verfügbar gemacht werden sollen. In diesem Beispiel mit Header "X-ms - Meta-" verfügbar gemacht werden.

-   **MaxAgeInSeconds** Dies ist die maximale Zeit, ein Browser die Preflight-Optionen-Anforderung zwischenspeichern. (Weitere Informationen über die Preflight-Anforderung überprüfen Sie im ersten Artikel unten.)

####<a name="resources"></a>Ressourcen

Weitere Informationen zu CORS und Aktivierung finden Sie diese Ressourcen.

-   [Cross-Ursprung Ressourcenfreigabe Unterstützung für Azure-Speicherdienste Azure.com (CORS)](storage-cors-support.md)

    Dieser Artikel bietet eine Übersicht über CORS und Regeln für die verschiedenen Dienste.

-   [Cross-Ursprung Ressourcenfreigabe Unterstützung für Azure-Speicherdienste MSDN (CORS)](https://msdn.microsoft.com/library/azure/dn535601.aspx)

    Dies ist die Referenzdokumentation für CORS Unterstützung für Azure Storage Services. Enthält Links zu Artikeln zuweisen jedes Speicherdienst ein Beispiel und jedes Element in der Datei CORS erläutert.

-   [Microsoft Azure Storage: Einführung in CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

    Dies ist ein Link Ankündigung CORS ersten Blog Artikel zum verwenden.

##<a name="frequently-asked-questions-about-azure-storage-security"></a>Häufig gestellte Fragen zur Azure Storage security

1.  **Wie kann ich die Integrität der Blobs überprüfen, die ich in oder aus Azure Storage übertragen wird, wenn ich das HTTPS-Protokoll verwenden kann?**

    Wenn aus irgendeinem Grund mit HTTP statt HTTPS und müssen Block-Blobs arbeiten, können Sie MD5 überprüft die Integrität der übertragenen Blobs feststellen. Mit Netzwerk-Transport Layer Fehler aber nicht mit zwischengeschalteten dadurch.

    Wenn Sie HTTPS verwenden können, bietet Sicherheit auf Transportebene ist MD5 Überprüfung redundant und unnötig.
    
    Weitere Informationen entnehmen Sie bitte [Azure Blob MD5 Overview](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).

2.  **Was die US-Regierung FIPS-Konformität?**

    Die Vereinigten Staaten FIPS Federal Information Processing Standard () definiert Kryptografiealgorithmen genehmigtes US Federal Regierung Computersysteme zum Schutz sensibler Daten. Aktivieren der FIPS derzufolge Modus auf einem Windows-Server oder Desktop des Betriebssystems nur FIPS-überprüften kryptografische Algorithmen verwendet werden soll. Wenn eine Anwendung nicht konformen Algorithmen verwendet, werden die Anwendung unterbrochen. : In .NET Framework, Version 4.5.2 oder höher, die Anwendung automatisch die Kryptografiealgorithmen um FIPS-kompatible Algorithmen verwenden, wenn der Computer im FIPS-Modus.

    Microsoft bleibt jeder Kunde entscheiden, ob Sie die FIPS-Modus aktivieren. Wir glauben, dass es keinen zwingenden Grund für Kunden, die nicht unterliegen behördlichen Auflagen FIPS-Modus standardmäßig aktiviert.

    **Ressourcen**

-   [Warum sind wir nicht "FIPS-Modus" mehr empfehlen](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

    Dieser Blogartikel gibt einen Überblick über die FIPS und erläutert, warum sie standardmäßig nicht FIPS-Modus aktivieren.

-   [FIPS 140 Validierung](https://technet.microsoft.com/library/cc750357.aspx)

    Dieser Artikel enthält Informationen zu FIPS-Standard für die US-Bundesregierung wie Microsoft-Produkte und kryptografischen Modulen einhalten.

-   ["Systemkryptografie: Verwenden von FIPS-konformen Algorithmus für Verschlüsselung, hashing und Signatur" Security Settings Effekte in Windows XP und späteren Versionen von Windows](https://support.microsoft.com/kb/811833)

    Dieser Artikel spricht über die Verwendung von FIPS-Modus in älteren Windows-Computern.
