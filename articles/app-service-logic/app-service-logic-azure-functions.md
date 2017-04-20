<properties
   pageTitle="Verwenden von Azure mit Logik Apps | Microsoft Azure"
   description="Finden Sie unter Verwenden von Azure Funktionen mit Geschäftslogik Apps"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Verwenden von Azure mit Logik Apps

Sie können Codeausschnittbibliothek C# oder node.js mit Azure-Funktionen in einer Anwendung Logik ausführen.  [Azure-Funktionen](../azure-functions/functions-overview.md) bietet Microsoft Azure computing Server frei. Dies ist hilfreich für die folgenden Aufgaben ausführen:

* Erweiterte Formatierung oder Berechnen von Feldern in einer Anwendung Logik
* Berechnungen innerhalb eines Workflows
* Erweitern der Funktionalität des Logik-Apps mit Funktionen, die in C# oder node.js unterstützt werden

## <a name="create-a-function-for-logic-apps"></a>Erstellen Sie eine Funktion für Logik-Apps

Wir empfehlen, eine neue Funktion im Portal Azure Funktionen mithilfe der **Generischen Webhook - Knoten** oder **Webhook - generische C#** -Vorlagen zu erstellen. Diese Auto-füllt eine Vorlage akzeptiert `application/json` Logik App.  Bei Auswahl die Registerkarte **Integration** in Azure Funktionen müssen sie **Modus** **Webhook** und **Webhook Typ** der **Generischen JSON**.  Funktionen, die diese Vorlagen automatisch erkannt und in Designer Logik Apps unter aufgeführten **Azure Funktionen in meiner Region.**

Webhook Funktionen akzeptiert und übergeben es an die Methode über eine `data` Variable. Zugriff die Eigenschaften der Nutzlast mithilfe der Punktnotation wie `data.foo`.  Im folgenden Beispiel wird z. B. sieht eine einfache JavaScript-Funktion, die einen DateTime-Wert in eine Datumszeichenfolge konvertiert:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Rufen Sie eine Logik-app Azure-Funktionen

Wenn Sie im Menü **Aktionen** klicken, können Sie im Designer **Azure Funktionen in meiner Region**auswählen.  Listet die Container auf Ihr Abonnement, und ermöglicht Ihnen die Funktion, die Sie anrufen möchten.  

Nach Auswahl die Funktion werden Sie aufgefordert, ein Eingabe-Nutzlast Objekt an. Dies ist die Meldung, die die Anwendung Logik an die Funktion sendet und muss ein JSON-Objekt. Sie gegebenenfalls das Datum der **Letzten Änderung** von Salesforce-Trigger übergeben könnte Funktion Nutzlast wie folgt aussehen:

![Datum der letzten modifiziert][1]

## <a name="trigger-logic-apps-from-a-function"></a>Trigger Logik apps aus einer Funktion

Es kann auch eine Logik-app innerhalb einer Funktion ausgelöst.  Dazu erstellen Sie einfach eine Logik-app mit einem manuellen Auslöser. Weitere Informationen finden Sie unter [Logik apps als aufrufbare Endpunkte](app-service-logic-http-endpoint.md).  Erstellen Sie anschließend innerhalb der Funktion HTTP POST für die manuelle Auslösung URL mit der Nutzlast Logik-App möchten.

### <a name="create-a-function-from-the-designer"></a>Erstellen Sie eine Funktion im Designer

Sie können auch Webhook Funktion eine node.js im Designer erstellen. Zunächst wählen Sie **Azure-Funktion in meiner Region** und wählen Sie einen Container für die Funktion.  Haben Sie noch einen Container, müssen Sie die [Funktionen der Azure-Portal](https://functions.azure.com/signin)erstellen. Wählen Sie dann **Neu erstellen**.  

Generiert eine Vorlage basierend auf den Daten, die Sie berechnen möchten, geben Sie das Kontextobjekt, das an eine Funktion übergeben werden soll. Dies muss ein JSON-Objekt. Wenn der Inhalt der Datei in einem FTP-übergeben wird Kontext Nutzlast so aussehen:

![Kontext-Nutzlast][2]

>[AZURE.NOTE] Da dieses Objekt als Zeichenfolge umgewandelt wurde nicht wird der Inhalt der JSON-Nutzlast direkt hinzugefügt werden. Es werden jedoch Fehler, ist es kein JSON-Token (eine Zeichenfolge oder ein JSON-Objekt-Array). Um es als Zeichenfolge umwandeln, fügen Sie Angebote wie in der ersten Abbildung in diesem Artikel.

Der Designer generiert eine Funktionsvorlage, Inline zu erstellen. Variablen werden basierend auf dem Kontext bereits erstellt, die an die Funktion übergeben werden soll.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
