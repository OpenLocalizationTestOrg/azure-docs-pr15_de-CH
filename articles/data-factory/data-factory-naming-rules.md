<properties 
    pageTitle="Data Factory - Benennungsregeln | Microsoft Azure" 
    description="Beschreibt die Benennungsregeln für Data Factory Entitäten." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - Benennungsregeln 
Die folgende Tabelle enthält die Regeln für Data Factory Artefakte.



Name | Eindeutigkeit des Benutzernamens | Überprüfung wird
:--- | :-------------- | :----------------
Data Factory | Für Microsoft Azure eindeutig. Namen Groß-und Kleinschreibung, d. h. MyDF und Mydf finden Sie die gleichen Daten Factory. |<ul><li>Jede Factory Data ist genau ein Azure-Abonnement gebunden.</li><li>Objektnamen müssen mit einem Buchstaben oder einer Zahl beginnen und darf nur Buchstaben, Zahlen und der Bindestrich (-).</li><li>Jeder Bindestrich (-) muss unmittelbar davor und dahinter einen Buchstaben oder eine Zahl. Aufeinander folgende Bindestriche sind Container-Namen nicht zulässig.</li><li>Namen kann 3 63 Zeichen lang sein.</li></ul>
Verknüpfte Services/Tabellen/Pipelines | In einer Factory Daten mit eindeutig. Namen Groß-und Kleinschreibung. | <ul><li>Maximale Anzahl von Zeichen in einem Tabellennamen: 260.</li><li>Objektnamen müssen mit einer Zahl Buchstaben oder einem Unterstrich (_) beginnen.</li><li>Folgende Zeichen sind nicht zulässig: ".", "+" und "?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul>
Ressourcengruppe | Für Microsoft Azure eindeutig. Namen Groß-und Kleinschreibung. | <ul><li>Maximale Anzahl Zeichen: 1000.</li><li>Namen kann Buchstaben, Ziffern und die folgenden Zeichen enthalten: "-", "_",""und".".</li></ul>