<properties 
    pageTitle="Wie Blitline Bild mit Verarbeitung - Azure feature Guide" 
    description="Informationen Sie zum Blitline Dienst zum Verarbeiten von Bildern in eine Azure-Anwendung verwenden." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Verwendung von Blitline mit Azure und Azure-Speicher

Dieses Handbuch erklären wie Blitline Dienste und Aufträge an Blitline übermitteln.

## <a name="what-is-blitline"></a>Was ist Blitline?

Blitline ist ein Cloud-basierten Dienst Enterprise auf Image-Verarbeitung zu einem Bruchteil des Preises, die Kosten selbst erstellen.

Image-Verarbeitung normalerweise von Grund für jede Website neu immer wieder durchgeführt worden ist. Wir nutzen, das wir ihnen einen millionenfach zu erstellt haben. Wir entschieden, dass vielleicht werden einmal tun wir nur für alle. Wir wissen, wie es schnell und effizient, und speichern alle Arbeit in der.

Weitere Informationen finden Sie unter [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Was Blitline nicht ist...

Um zu verdeutlichen, was Blitline für eignet, ist es oft leichter zu erkennen, was Blitline nicht vor voran.

- Blitline hat keine HTML-Widgets Bilder hochladen. Sie müssen Bilder öffentlich oder mit eingeschränkten Berechtigungen für Blitline zu.

- Blitline nicht verarbeiten wie täuschen Livebild

- Blitline akzeptiert keine Bildupload, Bilder können nicht zu Blitline direkt drücken. Drücken sie Azure Storage oder anderem Blitline unterstützt, und teilen Sie Blitline, wo Sie sie erhalten.

- Blitline parallele ist und keine synchrone Verarbeitung. D. h. Sie geben uns eine Postback_url und wir sagen Ihnen, wenn wir Verarbeitung.

## <a name="create-a-blitline-account"></a>Erstellen eines Kontos Blitline

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Erstellen ein Auftrags Blitline

Blitline verwendet JSON Aktionen ein Bild übernehmen möchten. Diese JSON besteht aus einigen einfachen Felder.

Das einfachste Beispiel lautet wie folgt:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Hier haben wir JSON, mit denen ein "Src" Bild "... boys.jpeg", und ändern Sie dieses Abbild auf 240 x 140.

Die ID ist etwas finden Sie auf der Registerkarte **VERBINDUNGSINFORMATIONEN** oder **Verwalten** auf Azure. Ist Ihre geheimen Bezeichner, der Aufträge auf Blitline ausführen kann.

"Speichern" identifiziert Informationen soll das Bild eingefügt, sobald wir es bearbeitet haben. In diesem Fall einfach noch nicht wir definiert. Wenn kein Lagerort definiert Blitline speichern lokal (und vorübergehend) an einem Ort eindeutig Cloud. Sie werden zu diesem Speicherort von Blitline bei der Blitline zurückgegebene JSON. Der Bezeichner "Image" ist erforderlich und zurück zu diesem Bild gespeichert.

Finden Sie weitere Informationen zu den *Funktionen* wir hier unterstützen: <http://www.blitline.com/docs/functions>

Finden Sie die Auftragsoptionen Dokumentation: <http://www.blitline.com/docs/api>

Haben Sie Ihre JSON ist müssen Sie **Buchen** sie`http://api.blitline.com/job`

JSON zurück erhalten, die etwa so aussieht:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Blitline hat Ihre Anfrage erhalten und wurde in eine Verarbeitungswarteschlange gelangen sie nach Abschluss das Bild werden aufgefordert: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Speichern Sie ein Bild in Azure Storage-Konto

Azure Storage-Konto verfügen, haben Sie einfach Blitline verarbeiteten Bilder Azure Container abzulegen. Durch Hinzufügen von "Azure_destination" definieren Sie die Position und die Berechtigungen für Blitline auf.

Hier ist ein Beispiel:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


AKTIVIERTE Werte mit eigenen auszufüllen, diese JSON und http://api.blitline.com/job senden und "Src" Bild Weichzeichnen-Filter verarbeitet und in Azure Ziel verschoben.

###<a name="please-note"></a>Bitte beachten Sie:

Die SAS enthalten den gesamten SAS-Url und Dateinamen der Zieldatei.

Beispiel:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Lesen Sie auch die neueste Ausgabe des Blitlines Azure-Speicher Dokumente [hier](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie blitline.com, um Informationen über alle anderen Funktionen:

* Blitline API-Endpunkt Dokumente <http://www.blitline.com/docs/api>
* Blitline-API-Funktionen <http://www.blitline.com/docs/functions>
* Blitline API Beispiele <http://www.blitline.com/docs/examples>
* Dritter Teil Nuget-Bibliothek <http://nuget.org/packages/Blitline.Net>
