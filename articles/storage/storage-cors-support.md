<properties
    pageTitle="Cross-Ursprung Freigabe (CORS) Unterstützung | Microsoft Azure"
    description="Informationen Sie zum CORS-Unterstützung für Microsoft Azure-Speicherdienste zu aktivieren."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Cross-Ursprung Ressourcenfreigabe Unterstützung für Azure Storage Services (CORS)

Ab Version 2013-08-15 unterstützt Azure-Speicherdienste Cross-Ursprung Ressource freigeben (CORS) Blob, Tabelle, Warteschlange und Datei. CORS ist ein HTTP-Feature, eine Webanwendung unter einer Domäne auf Ressourcen in einer anderen Domäne zugreifen können. Web-Browser implementieren eine Einschränkung für die Sicherheit als [same Origin Policy](http://www.w3.org/Security/wiki/Same_Origin_Policy) , die einer Webseite Aufrufen von APIs in einer anderen Domäne verhindert. CORS bietet eine sichere Möglichkeit, eine Domäne (Ursprungsdomäne) in einer anderen Domäne APIs aufrufen. Informationen finden Sie der [Spezifikation CORS](http://www.w3.org/TR/cors/) auf CORS.

Sie können CORS einzeln aller Storage Services Regeln durch Aufrufen von [Blob-Eigenschaften festlegen](https://msdn.microsoft.com/library/hh452235.aspx), [Eigenschaften für Warteschlange festlegen](https://msdn.microsoft.com/library/hh452232.aspx)und [Eigenschaften für Tabelle festlegen](https://msdn.microsoft.com/library/hh452240.aspx). Nach dem Festlegen der CORS-Regeln für den Dienst wird eine ordnungsgemäß authentifizierte Anforderung für den Dienst aus einer anderen Domäne ausgewertet werden, um festzustellen, ob nach den Regeln zulässig ist, die Sie angegeben haben.

>[AZURE.NOTE] Beachten Sie, dass CORS nicht Authentifizierungsmechanismus. Antrag auf eine Speicherressource aktiviert CORS muss entweder eine ordnungsgemäße Authentifizierungssignatur oder für öffentliche Ressourcen erfolgen muss.

## <a name="understanding-cors-requests"></a>Grundlegendes zu CORS Anfragen

Ein Ursprungsdomäne Ersuchen CORS besteht aus zwei separaten Anfragen:

- Ein Preflight-Antrag CORS Einschränkungen vom Dienst Abfragen. Preflight-Anforderung ist erforderlich die Anforderungsmethode eine [einfache Methode](http://www.w3.org/TR/cors/), d. h. GET, HEAD und POST.

- Die eigentliche Anforderung gegen die gewünschte Ressource.

### <a name="preflight-request"></a>Preflight-Anforderung

Preflight-Anforderung Abfragen, CORS eingeschränkt, die für den Speicherdienst vom Kontoinhaber eingerichtet wurden. Web-Browser (oder andere Benutzer-Agenten) sendet eine Optionen Anforderung mit der Anforderungsheader, Methode und der Ursprung. Der Speicherdienst wertet den Vorgang basierend auf vorkonfigurierten CORS-Regeln, die angeben, welche Ursprungsdomänen Anforderungsmethoden und Anforderungsheader für eine tatsächliche Anforderung für eine Speicherressource angegeben werden können.

CORS für den Dienst aktiviert und gilt CORS, die Preflight-Anforderung entspricht, wird der Dienst antwortet mit Statuscode 200 (OK) und die erforderlichen Access-Control-Header in der Antwort enthält.

Wenn CORS für den Dienst nicht aktiviert ist oder keine CORS-Regel die Preflight-Anforderung erfüllt, antwortet der Dienst mit dem Statuscode 403 (Verboten).

OPTIONS-Anfrage erforderlichen CORS Header (Ursprung und Access-Control-Anforderungsmethode Header) enthält, reagiert der Dienst mit Statuscode 400 (Ungültige Anforderung).

Beachten Sie, dass eine Preflight-Anforderung für den Dienst (BLOB-Warteschlange und Tabelle) und nicht auf die angeforderte Ressource ausgewertet wird. Konto muss CORS als Teil des Dienstes Kontoeigenschaften in Bestellung erfolgreich aktiviert haben.

### <a name="actual-request"></a>Tatsächliche Anforderung

Preflight-Anforderung angenommen und die Antwort zurückgegeben, entsendet der Browser die tatsächliche Anforderung die Speicherressource. Der Browser Verweigern der Anforderung sofort die Preflight-Anforderung abgelehnt.

Die eigentliche Anforderung wird als normale Anforderung der Speicherdienst behandelt. Der Ursprung Kopfzeile deutet die Anforderung ist eine CORS-Anforderung, die Checks CORS Zuordnungsregeln. Wenn eine Übereinstimmung gefunden wird, werden Access-Control-Header der Antwort hinzugefügt und an den Client gesendet. Wenn eine Übereinstimmung gefunden wird, werden die CORS Access-Control-Header nicht zurückgegeben.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Aktivieren von CORS für die Azure-Speicherdienste

CORS werden Regeln auf der Dienstebene müssen Sie aktivieren oder Deaktivieren von CORS für jeden Dienst (BLOBs, Warteschlangen und Tabelle) getrennt. CORS ist für jeden Dienst deaktiviert. Um CORS zu aktivieren, müssen die entsprechenden Eigenschaften mit Version 2013-08-15 oder höher und die Diensteigenschaften CORS-Regeln hinzufügen. Aktivieren oder Deaktivieren von CORS für einen Dienst und CORS-Regeln bitte Einzelheiten Sie [Blob-Eigenschaften festlegen](https://msdn.microsoft.com/library/hh452235.aspx), [Eigenschaften für Warteschlange festlegen](https://msdn.microsoft.com/library/hh452232.aspx)und [Eigenschaften für Tabelle festlegen](https://msdn.microsoft.com/library/hh452240.aspx).

Hier ist ein Beispiel für eine einzelne CORS-Regel legen Eigenschaften Vorgang angegeben:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Jedes Element in der Regel CORS eingeschlossen wird nachstehend beschrieben:

- **AllowedOrigins**: Ursprungsdomänen, die eine Anforderung anhand der Speicherdienst über CORS zulässig sind. Die Ursprungsdomäne ist die Domäne, von der die Anforderung stammt. Beachten Sie, dass der Ursprung dieselben Groß-und dessen Ursprung, die das Alter des Benutzers an den Dienst sendet. Können Sie auch das Platzhalterzeichen ' *' alle Ursprungsdomänen Anfragen über CORS zu ermöglichen. Im obigen Beispiel können Domänen [http://www.contoso.com](http://www.contoso.com) und [http://www.fabrikam.com](http://www.fabrikam.com) Abfragen mit CORS.

- **AllowedMethods**: Methoden (HTTP-Verben), die die Ursprungsdomäne für eine CORS-Anforderung verwenden kann. Im obigen Beispiel dürfen nur PUT- und GET-Anfragen.

- **AllowedHeaders**: der Anforderungsheader, die auf die CORS Anforderung die Ursprungsdomäne angeben können. Im obigen Beispiel dürfen alle Metadaten-Header X-ms-Metadaten, X-ms-Meta-Ziel und X-ms-Meta-Abc ab. Beachten Sie, dass das Platzhalterzeichen ' *' gibt an, dass alle Header beginnend mit dem angegebenen Präfix zulässig ist.

- **ExposedHeaders**: die Antwortheader, die auf die CORS-Anforderung gesendet und vom Browser die Anforderung Emittenten ausgesetzt. Im obigen Beispiel wird der Browser angewiesen, alle Header beginnend mit X-ms-Metadaten verfügbar gemacht.

- **MaxAgeInSeconds**: die maximale Zeit, ein Browser Preflight-Optionen zwischenspeichern soll.

Azure-Speicherdienste unterstützt vorangestellter Header für die **AllowedHeaders** und **ExposedHeaders** . Um eine Kategorie von Headern zu ermöglichen, können Sie ein gemeinsames Präfix Kategorie angeben. Z. B. Angabe *X-ms-Meta** ein vorangestellter Header eine Regel fest, die alle Header entsprechen, die mit X-ms-Metadaten.

Einschränkungen CORS Regeln:

- Sie können bis zu 5 CORS Regeln pro Storage Service (Blob, Tabelle und Warteschlange).
- Die maximale Größe aller CORS Regeln Einstellungen für die Anforderung ohne XML-Tags muss 2 KB nicht überschreiten.
- Die Länge eines zulässigen Header, verfügbar gemachten Kopf- oder zulässigen Ursprung darf 256 Zeichen nicht überschreiten.
- Zulässigen Header und verfügbar gemachte Header möglicherweise entweder:
  - Literale Header, der genauen Headernamen wie **X-ms-Meta-Verarbeitung**bereitgestellt wird. Möglicherweise 64 Literale Header in der Anforderung angegeben werden.
  - Header, wobei ein Präfix des Headers, wie **X-ms-Metadaten angegeben ist**Präfix *. Die Angabe eines Präfix auf diese Weise kann oder macht alle Header mit dem angegebenen Präfix beginnt. Zwei vorangestellten Header kann in der Anforderung angegeben werden.
- Im **AllowedMethods** -Element angegebene Methoden (oder HTTP-Verben) müssen von Azure Storage Service-APIs unterstützt Methoden entsprechen. Unterstützte Methoden sind löschen, GET, HEAD, ZUSAMMENFÜHREN, POST, Optionen und PUT.

## <a name="understanding-cors-rule-evaluation-logic"></a>CORS Logik Regel Bewertung

Wenn eines Preflight- oder tatsächliche Anforderung erhält, wertet die Anforderung anhand der CORS-Regeln für den Dienst über den entsprechenden Diensteigenschaften Vorgang festgelegten. CORS-Regeln werden in der Reihenfolge ausgewertet, in denen sie im Hauptteil Anforderung des Vorgangs legen Sie Eigenschaften festgelegt wurden.

CORS-Regeln werden wie folgt ausgewertet:

1. Zunächst wird die Ursprungsdomäne der Anforderung für das **AllowedOrigins** -Element aufgelisteten Domänen verglichen. Wenn die Ursprungsdomäne in der Liste enthalten ist oder alle Domänen mit dem Platzhalterzeichen dürfen ' *', Regeln Bewertung wird fortgesetzt. Wenn die Ursprungsdomäne nicht enthalten ist, schlägt die Anforderung fehl.

2. Anschließend wird die Methode oder HTTP-Verb der Anfrage im **AllowedMethods** -Element aufgeführten Methoden verglichen. Wenn die Methode in der Liste enthalten ist, wird Regeln Bewertung; Wenn dies nicht der Fall ist, schlägt die Anforderung fehl.

3. Wenn die Anforderung in der Ursprungsdomäne und die Regel übereinstimmt, wird dieser Regel Prozess aktiviert, die Anforderung und keine weiteren Regeln ausgewertet werden. Bevor die Anforderung erfolgreich sein kann, sind jedoch alle Header in der Anforderung angegeben Header im **AllowedHeaders** -Element verglichen. Wenn der Header gesendet zulässigen Header nicht übereinstimmen, schlägt die Anforderung fehl.

Da die Regeln in der Reihenfolge sind sie im Hauptteil Anforderung verarbeitet werden, empfohlen, dass Sie die am meisten einschränkenden Regeln in Bezug auf Herkunft zunächst in der Liste angeben, damit diese zuerst ausgewertet werden. Geben Sie Regeln, die weniger restriktiv – z. B. eine Regel zu allen Ursprüngen – am Ende der Liste.

### <a name="example--cors-rules-evaluation"></a>Beispiel – Regeln CORS Bewertung

Das folgende Beispiel zeigt einen partielle Anforderungstext für einen Vorgang CORS Regeln für Storage Services. Finden Sie weitere Informationen zum Erstellen der Anforderung [Blob-Eigenschaften festlegen](https://msdn.microsoft.com/library/hh452235.aspx), [Eigenschaften für Warteschlange festlegen](https://msdn.microsoft.com/library/hh452232.aspx)und [Eigenschaften für Tabelle festlegen](https://msdn.microsoft.com/library/hh452240.aspx) .

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Nehmen Sie die folgenden CORS Anfragen an:

Anforderung||| Antwort||
---|---|---|---|---
**-Methode** |**Ursprung** |**Anfrage-Header** |**Übereinstimmung** |**Ergebnis**
**EINFÜGEN** | http://www.contoso.com |X-ms-Blob-Inhaltstyp | Erste Regel |Erfolg
**Erhalten** | http://www.contoso.com| X-ms-Blob-Inhaltstyp | Zweite Regel |Erfolg
**Erhalten** | http://www.contoso.com| X-ms-Blob-Inhaltstyp | Zweite Regel | Fehler

Die erste Anforderung entspricht der ersten Regel – die Ursprungsdomäne entspricht der zulässigen Ursprünge die Methode entspricht der zulässigen Methoden und Header entspricht der zulässigen Header – und so erfolgreich.

Die zweite Anforderung entspricht nicht die erste Regel, da die Methode nicht zulässigen Methoden übereinstimmen. Es stimmt, die zweite Regel jedoch erfolgreich überein.

Die dritte Anforderung entspricht die zweite Regel in der Ursprungsdomäne und -Methode, sodass keine weiteren Regeln ausgewertet werden. Jedoch *X-ms-Client-Anfrage-Id-Header* in der zweiten Regel darf nicht so die Anforderung, trotz fehlschlägt, die Semantik der dritten Regel Erfolg hätten.

>[AZURE.NOTE] Dieses Beispiel zeigt eine weniger restriktive Regel vor einer restriktiveren zwar im Allgemeinen am besten zuerst aufgeführt, die am meisten einschränkenden Regeln.

## <a name="understanding-how-the-vary-header-is-set"></a>Grundlegendes zum Festlegen des Vary-header

Der *Vary* -Header ist ein standard HTTP/1.1-Header aus der Anforderung-Headerfeldern, die den Browser oder Benutzer-Agent über die Kriterien beraten, die vom Server zum Verarbeiten der Anforderung ausgewählt wurden. Der *Vary* -Header wird hauptsächlich zum Zwischenspeichern von Proxys, Browsern und CDNs, die bestimmen wie die Antwort zwischengespeichert werden soll. Details finden Sie in der Spezifikation für den [Vary-Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Wenn der Browser oder eine andere Benutzer-Agent die Antwort einer Anforderung CORS zwischenspeichert, wird die Ursprungsdomäne zulässigen Ursprung zwischengespeichert. Bei eine zweite Domäne derselben Anforderung für eine Speicherressource während der Cache aktiviert ist, ruft der Benutzer-Agent die zwischengespeicherten Ursprungsdomäne. Domäne die zweite entspricht nicht zwischengespeicherten Domänen, damit die Anforderung fehlschlägt, wenn es sonst erfolgreichen. In bestimmten Fällen wird Azure Storage Vary-Header **Ursprung** anweisen der Benutzer-Agent nachfolgende CORS-Anforderung an den Dienst senden, wenn zwischengespeicherte Ursprung die anfordernde Domäne unterscheidet.

Azure-Speicher wird *Vary* -Header **Ursprung** für tatsächliche GET-HEAD-Anfragen in folgenden Fällen:

- Bei Anforderung Ursprung Regel CORS festgelegten zulässigen Ursprung übereinstimmt. Um eine genaue Übereinstimmung, CORS-Regel enthalten Platzhalter "*" Zeichen.

- Es gibt keine Regel mit den Ursprung der Anforderung, aber CORS für Storage Service aktiviert ist.

Im Fall, bei denen eine GET-HEAD-Anforderung CORS Regel übereinstimmt, die Herkunft ermöglicht, gibt die Antwort aller dürfen und Benutzer-Agent-Cache werden nachfolgende Anfragen durch alle Ursprungsdomäne während der Cache aktiviert ist.

Beachten Sie, dass Anfragen mit Methoden als GET-HEAD, Speicherdienste nicht Vary-Header festgelegt, da Antworten auf diese Methoden durch Benutzer-Agents nicht zwischengespeichert werden.

Die folgende Tabelle zeigt wie Azure-Speicher wird basierend auf den oben erwähnten Fällen GET-HEAD-Anfragen Antworten:

Anforderung|Konto festlegen und Ergebnis der regelauswertung|||Antwort|||
---|---|---|---|---|---|---|---|---
**Ursprungsheader auf Anforderung** | **Für diesen Dienst angegebene CORS-Regeln** | **Übereinstimmende Regel ist vorhanden, die Herkunft ermöglicht (*)** | **Abgleichsregel vorhanden für genaue Herkunft Übereinstimmung** | **Antwort enthält Vary-Header Ursprung festlegen** | **Antwort enthält Access Control zulässig Ursprung: "*"** | **Antwort enthält Access Control ausgesetzt Header**
Nein|Nein|Nein|Nein|Nein|Nein|Nein
Nein|Ja|Nein|Nein|Ja|Nein|Nein
Nein|Ja|Ja|Nein|Nein|Ja|Ja
Ja|Nein|Nein|Nein|Nein|Nein|Nein
Ja|Ja|Nein|Ja|Ja|Nein|Ja
Ja|Ja|Nein|Nein|Ja|Nein|Nein
Ja|Ja|Ja|Nein|Nein|Ja|Ja


## <a name="billing-for-cors-requests"></a>Abrechnung für CORS Anfragen

Erfolgreiche Preflight-Anfragen werden berechnet, wenn Sie CORS für Speicher-Services für Ihr Konto aktiviert haben (durch Aufrufen von [Blob-Eigenschaften festlegen](https://msdn.microsoft.com/library/hh452235.aspx), [Eigenschaften für Warteschlange festlegen](https://msdn.microsoft.com/library/hh452232.aspx)oder [Eigenschaften für Tabelle festgelegt](https://msdn.microsoft.com/library/hh452240.aspx)). Um Kosten zu minimieren, sollten Sie das **MaxAgeInSeconds** -Element in der CORS-Regeln auf einen hohen Wert festlegen, sodass der Benutzer-Agent die Anforderung zwischengespeichert.

Nicht erfolgreiche Preflight-Anfragen werden nicht berechnet.

## <a name="next-steps"></a>Nächste Schritte

[BLOB-Eigenschaften festlegen](https://msdn.microsoft.com/library/hh452235.aspx)

[Festlegen von Eigenschaften Service](https://msdn.microsoft.com/library/hh452232.aspx)

[Festlegen von Tabelleneigenschaften Service](https://msdn.microsoft.com/library/hh452240.aspx)

[Gemeinsame Nutzung von Spezifikation W3C ursprungsübergreifende Ressourcen](http://www.w3.org/TR/cors/)
