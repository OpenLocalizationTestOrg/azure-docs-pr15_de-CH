<properties
    pageTitle="Große Bereitstellung bereitstellen"
    description="Weitere Informationen zum Bereitstellen von großer Bereitstellungen mit dem Azure-Toolkit für Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Große Bereitstellung bereitstellen #

Wenn die Bereitstellung zu groß, um in den Standardordner Approot enthalten sein, können lokale Speicherressourcen als Stammordner der Bereitstellung für die JDK und Anwendungsserver.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Lokale Speicherressourcen als Bereitstellungsstammordner für große Installationen verwenden ##

1. Erstellen Sie eine neue lokale Speicherressourcen. Der Name der Ressource spielt keine Rolle. Ressourcen werden auf Rollenebene definiert. Am schnellsten Zugriff Konfigurationsdialogfeld lokalen Speicher, aus dem Sie eine neue Ressource für lokalen Speicher erstellen konnte, mit den folgenden Schritten wird: mit der rechten Maustaste der Rolle in der **Projekt-Explorer** -Ansicht (Knoten der Azure-Projekt erscheint die Rolle nicht), **Azure**klicken und dann auf **Lokalem Speicher**. Klicken Sie im Dialogfeld **Lokale Speicherung** auf **Hinzufügen** , um eine neue lokale Speicherung erstellen.
1. Legen Sie die gewünschte Größe auf 2048 MB (alles möglicherweise weniger dieselbe Datei Größe Probleme auftreten würde in der Approot).
1. Stellen Sie sicher, dass **Reinigen von Inhalt, wenn die Instanz der Rolle recycelt wird** aktiviert ist; Dadurch verhindern Startlogik der Bereitstellung Konflikte mit vorhandenen Dateien in der Ressource wird die Rolleninstanz wiederverwendet.
1. Sicherstellen Sie, dass die **Umgebungsvariable speichern die Ressource Pfad nach der Bereitstellung** der Zeichenfolge **DEPLOYROOT**Wert. Dialogfeld Ihre lokalen Speicher sieht etwa wie folgt.
    ![][ic667943]

Auch wenn Sie **DEPLOYROOT** als *Namen* der lokalen Ressource und Sie automatisch generierte Namen der Umgebungsvariablen (die auf **DEPLOYROOT_PATH** in diesem Fall festgelegt wird) nicht ändern, die für Ihre Anwendung funktioniert.

Weitere Informationen zum Erstellen einer lokalen Speicherressourcen kann [lokalen Speicher][]Eigenschaften gefunden.

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für Eclipse][]

[Erstellen eine Hello World-Anwendung für Azure in Eclipse][]

[Azure Toolkit installieren für Eclipse][] 

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen eine Hello World-Anwendung für Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Eigenschaften lokaler Speicher]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
