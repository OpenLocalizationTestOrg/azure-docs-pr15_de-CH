<properties
   pageTitle="Übersicht der Zugriffskontrolle im Datenspeicher See | Microsoft Azure"
   description="Verstehen, wie Zugriffskontrolle in Azure See Datenspeicher"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Zugriffskontrolle in Azure See Datenspeicher

Datenspeicher See implementiert ein Zugriffssteuerungsmodell bietet und wiederum über das POSIX-Modell abgeleitet wird. Dieser Artikel beschreibt die Grundlagen der Zugriffssteuerungsmodell See Datenspeicher. Über die bietet Zugriff Steuerelement Modell finden Sie [Berechtigungen bietet](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Zugriffssteuerungslisten für Dateien und Ordner

Es gibt zwei Arten von Access Zugriffssteuerungslisten (ACLs) - **Access ACLs** und **Standard-ACLs**.

* **Access ACLs** – diese Steuerelement Zugriff auf ein Objekt. Dateien und Ordner haben Zugriff auf ACLs.

* **Standard-ACLs** – "Vorlage" ACLs einem Ordner zugeordnet, die bestimmen, Access ACLs für alle untergeordneten Elemente in diesem Ordner erstellt. Dateien haben keine Standard-ACLs.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Access ACLs und Standard-ACLs weisen dieselbe Struktur auf.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] Ändern die Standard-ACL in einem übergeordneten Element wirkt sich nicht auf Access ACL oder Standard-ACL von untergeordneten Elementen, die bereits vorhanden sind.

## <a name="users-and-identities"></a>Benutzer und Identitäten

Jede Datei und jeder Ordner verfügt über unterschiedliche Berechtigungen für diese Identitäten:

* Der besitzende Benutzer der Datei
* Der Eigner-Gruppe
* Benutzer
* Benannte Gruppen
* Alle anderen Benutzer

Die Identitäten der Benutzer und Gruppen sind Azure Active Directory (AAD) Identitäten soweit ein "Benutzer" im Kontext des Sees Datenspeicher entweder könnte ein AAD Benutzer oder eine Sicherheitsgruppe AAD.

## <a name="permissions"></a>Berechtigungen

Die Berechtigungen für ein Objekt Dateisystem sind **Lesen**, **Schreiben**und **Ausführen** und sie können für Dateien und Ordner wie in der folgenden Tabelle gezeigt verwendet werden.

|            |    Datei     |   Ordner |
|------------|-------------|----------|
| **Read (R)** | Kann den Inhalt einer Datei lesen | Erfordert **Lesen** und **Ausführen** , um den Inhalt des Ordners anzuzeigen.|
| **Schreiben (W)** | Schreiben oder an eine Datei anhängen | **Schreiben und Ausführen** zum Erstellen von untergeordneten Elementen in einem Ordner erfordert. |
| **Ausführen (X)** | Nicht bedeutet im Kontext des Sees Datenspeicher nichts | Zum Durchlaufen der untergeordneten Elemente eines Ordners erforderlich. |

### <a name="short-forms-for-permissions"></a>Kurzformen für Berechtigungen

**RWX**wird an **Lesen + Schreiben und Ausführen**. Kleinere numerische Form existiert in der **Lesen = 4**, **Schreiben 2 =**, und **Ausführen = 1** und die Summe die Berechtigungen. Im folgenden sind einige Beispiele.

| Numerische form | Kurzform |      Bedeutung     |
|--------------|------------|------------------------|
| 7            | RWX        | Lesen + Schreiben und ausführen |
| 5            | R-X        | Lesen und ausführen         |
| 4            | R--        | Lesen                   |
| 0            | ---        | Keine Berechtigungen         |


### <a name="permissions-do-not-inherit"></a>Berechtigungen erben

Im Datenspeicher See verwendet POSIX-Modell werden Berechtigungen für ein Element auf das Element selbst gespeichert. Berechtigungen für ein Element können also von den übergeordneten Elementen geerbt werden.

## <a name="common-scenarios-related-to-permissions"></a>Allgemeine Szenarien im Zusammenhang mit Berechtigungen

Hier sind einige häufige Szenarien zu verstehen, welche Berechtigungen für bestimmte Operationen für einen See Datenspeicher benötigt werden.

### <a name="permissions-needed-to-read-a-file"></a>Berechtigungen für eine Datei lesen

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Zu lesende - Datei benötigt der Aufrufer die Berechtigung **Lesen**
* Der Aufrufer benötigt für alle Ordner in der Ordnerstruktur mit der Datei - **Execute** -Berechtigungen

### <a name="permissions-needed-to-append-to-a-file"></a>Berechtigungen für eine Datei hinzufügen

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Die Datei angefügt, benötigt der Aufrufer Berechtigungen **Schreiben**
* Der Aufrufer benötigt für alle Ordner mit der Datei - **Execute** -Berechtigungen

### <a name="permissions-needed-to-delete-a-file"></a>Berechtigungen für eine Datei zu löschen

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Für den übergeordneten Ordner - benötigt der Aufrufer Berechtigungen **Schreiben + Ausführen**
* Der Aufrufer benötigt für alle Ordner im Pfad der Datei - **Execute** -Berechtigungen

>[AZURE.NOTE] Schreiben Sie die Berechtigungen für die Datei ist nicht erforderlich, die Datei als die beiden oben genannten Umstände zutrifft.

### <a name="permissions-needed-to-enumerate-a-folder"></a>Berechtigungen für einen Ordner auflisten

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Für den Ordner auflisten - benötigt der Aufrufer **Lesen + Execute** -Berechtigungen
* Der Aufrufer benötigt für alle übergeordneten Ordner - **Execute** -Berechtigungen

## <a name="viewing-permissions-in-the-azure-portal"></a>Anzeigen von Berechtigungen im Azure-portal

Datenspeicher See Konto **Daten-Explorer** Blatt klicken Sie auf **Zugriff** auf die ACLs für eine Datei oder einen Ordner finden Sie unter. Klicken Sie auf Zugriff auf die ACLs für **Katalog** -Ordner unter dem Konto des **Mydatastore** finden im folgenden Screenshot.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Klicken Sie danach Blatt **Zugriff** auf **Einfache Ansicht** einfacher anzuzeigen.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Klicken Sie auf **Erweiterte Ansicht** um die erweiterte Ansicht anzuzeigen.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>Der super-user

Super-User ist die Rechte aller Benutzer im Datenspeicher See. Super-User:

* RWX berechtigt, **Alle** Dateien und Ordner
* Sie können die Berechtigungen für alle Dateien oder Ordner.
* die besitzende Benutzer oder besitzen alle Dateien oder Ordner können.

In Azure hat ein Datenspeicher See Konto mehrere Azure-Rollen:

* Besitzer
* Beteiligte Personen
* Leser
* Usw.

Die Rolle **Eigentümer** für einen Datenspeicher See Konto ist automatisch ein Super-User für das Konto. Finden Sie mehr über Azure Rolle basiert Zugang Steuerelement (RBAC) [rollenbasierter Zugriff steuern](../active-directory/role-based-access-control-configure.md).

## <a name="the-owning-user"></a>Der besitzende Benutzer

Der Benutzer, der das Element erstellt wird der besitzenden Benutzer des Elements. Besitzer Benutzer kann:

* Ändern Sie die Berechtigungen einer Datei gehört
* Ändern des Eigner-Gruppe einer Datei gehört, als der besitzende Benutzer auch der Zielgruppe angehört.

>[AZURE.NOTE] Die besitzende Benutzer **nicht** ändern den besitzenden Benutzer Besitzer Dateinamen. Nur Super-Benutzer können den besitzenden Benutzer eine Datei oder einen Ordner ändern.

## <a name="the-owning-group"></a>Der Eigner-Gruppe

In den ACLs POSIX ist "primären Gruppe" zugeordnet jeder Benutzer. Beispielsweise kann Benutzer "Alice" der Finanzgruppe "" angehören. Alice kann zu mehreren Gruppen gehören, aber eine Gruppe immer als ihre primäre Gruppe festgelegt. In POSIX Wenn Alice eine Datei erstellt wird die Eigentümergruppe der Datei ihre primäre Gruppe fest, in diesem Fall "Finance".
 
Beim Erstellen ein neuen Dateisystem Elements mit See Datenspeicher die Eigentümergruppe ein Wert zugewiesen. 

* **Fall 1** : das Stammverzeichnis "/". Dieser Ordner wird erstellt, wenn ein Datenspeicher See-Konto erstellt. In diesem Fall ist der Eigner-Gruppe für den Benutzer festgelegt, die das Konto erstellt.
* **Fall 2** (alle anderen Fall) - ein neues Element erstellt wird, wird die Eigentümergruppe aus dem übergeordneten Ordner kopiert.

Die Eigentümergruppe kann durch geändert werden:
* Super-Benutzer
* Der besitzende Benutzer ist der besitzende Benutzer auch Mitglied der Zielgruppe.

## <a name="access-check-algorithm"></a>Zugriff Kontrollkästchen Algorithmus

Die folgende Abbildung stellt Zugriff Kontrollkästchen Algorithmus für Datenspeicher See Konten.

![Datenspeicher See ACLs Algorithmus](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Die Maske und "effektive Berechtigungen"

Die **Maske** ist ein RWX Wert, mit dem beim Algorithmus überprüft Access für **benannte Benutzer** **besitzende Gruppe**und der **Gruppen** beschränken. Hier werden die Schlüsselkonzepte für die Maske. 

* Die Maske "effektive Berechtigungen" erstellt, d. h. ändert die Berechtigungen bei Zugriff überprüfen.
* Die Maske kann Eigentümer und Super-Benutzer direkt bearbeitet werden.
* Die Maske kann die effektive Berechtigung zu entfernen. Die Maske **kann nicht** hinzufügen die effektive Berechtigung Berechtigungen. 

Lassen Sie uns einige Beispiele. Unten wird die Maske **RWX**festgelegt, was bedeutet, dass die Maske keine Berechtigungen entfernt. Beachten Sie, dass die effektiven Berechtigungen für benannte Benutzer Eigentümergruppe und benannte Gruppe bei der Zugriff nicht geändert werden.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

Im folgenden Beispiel wird die Maske **R X**festgelegt. So es für **Benutzer namens** **Gruppe besitzen**und gleichzeitig auf **benannte Gruppe** **deaktiviert die Schreibberechtigung** prüfen.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Zu Referenzzwecken ist hier das Format für eine Datei oder einen Ordner in der Azure-Portal.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] Für ein neues Konto See Datenspeicher sind RWX Maske für Access ACL und Standard-ACL des Stammordners ("/") übernommen.

## <a name="permissions-on-new-files-and-folders"></a>Berechtigungen für Dateien und Ordner

Wenn eine neue Datei oder einen Ordner in einem vorhandenen Ordner erstellt wird, bestimmt die Standard-ACL für den übergeordneten Ordner:

* Standard-ACL und Access ACL eines untergeordneten Ordners
* Einer untergeordnete Datei Zugriff ACL (Dateien haben keine Standard-ACL)

### <a name="a-child-file-or-folders-access-acl"></a>Eine untergeordnete Datei oder des Ordners Access ACL

Beim Erstellen eine untergeordnete Datei oder einen Ordner wird Standard-ACL des übergeordneten Elements der untergeordneten Datei oder des Ordners Access ACL kopiert. Auch wenn **andere** Benutzer RWX Berechtigungen im übergeordneten Standard-ACL verfügt, vollständig aus des untergeordneten Elements Zugriff ACL entfernt.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

In den meisten Fällen lediglich die oben genannten Informationen müssen Sie erfahren, wie ein untergeordnetes Element Access ACL bestimmt wird. Allerdings kennen POSIX-Systemen und detaillierte erkennen wie diese Transformation erreicht wird Abschnitt [Umask Rolle die ACL für neue Dateien und Ordner](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) in diesem Artikel.
 

### <a name="a-child-folders-default-acl"></a>Eines untergeordneter Ordners Standard-ACL

Beim Erstellen eines untergeordneten Ordners in einem übergeordneten Ordner wird Standard-ACL des übergeordneten Ordners, des Ordners des Standard-ACL ist, kopiert.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Erweiterte Themen für das Verständnis von ACLs im Datenspeicher See

Es folgen ein paar Erweiterte Themen erfahren Sie wie ACLs für Datenspeicher See Dateien oder Ordner festgelegt werden.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>Die Umask Rolle die ACL für neue Dateien und Ordner

In POSIX-kompatiblen System ist das allgemeine Konzept, Umask 9 Bitwert für den übergeordneten Ordner die Berechtigung **besitzen Benutzer** **Gruppe besitzt**und **andere** neue untergeordnete Datei oder des Ordners Access ACL transformiert. Die Bits einer Umask identifizieren die Bits in des untergeordnete Elements Zugriff ACL deaktivieren. Deshalb verwendet selektiv verhindert die Weitergabe von Berechtigungen für Benutzer, Gruppe, Besitzer und andere.
  
In einem System bietet ist die Umask normalerweise eine standortweite Konfigurationsoption, die von Administratoren gesteuert. See Datenspeicher verwendet ein **Konto Wide Umask** , die geändert werden können. Die folgende Tabelle zeigt See Datenspeichers Umask.

| Benutzergruppe  | Einstellung | Auswirkung auf neues untergeordnetes Element Access ACL |
|------------ |---------|---------------------------------------|
| Besitzer (Benutzer) | ---     | Keine Auswirkung                             |
| Übergeordnete Gruppe| ---     | Keine Auswirkung                             |
| Andere       | RWX     | Entfernen Sie lesen + schreiben + ausführen         | 

Die folgende Abbildung zeigt diese Umask in Aktion. Der Effekt ist **Lesen + Schreiben und Ausführen** **anderer** Benutzer entfernen. Da die Umask Bits **besitzen Benutzer** und **Gruppe besitzt**nicht angeben, werden diese Berechtigungen nicht transformiert.

![Datenspeicher See ACLs](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>Das sticky bit

Das sticky Bit ist ein erweitertes Feature eines Dateisystems POSIX. Im Kontext des Datenspeichers See dürfte das sticky Bit benötigt werden.

Die Tabelle zeigt, wie das sticky Bit im Datenspeicher See funktioniert.

| Benutzergruppe         | Datei    | Ordner |
|--------------------|---------|-------------------------|
| Sticky Bit **aus** | Keine Auswirkung   | Keine Auswirkung           |
| Sticky Bit **auf**  | Keine Auswirkung   | **Super-Benutzer** und **Benutzer Besitzer** eines untergeordneten Elements löschen oder umbenennen, untergeordnetes Element verhindert.               |

Das sticky Bit wird nicht im Azure-Portal angezeigt.

## <a name="common-questions-for-acls-in-data-lake-store"></a>Häufig gestellte Fragen für ACLs im Datenspeicher See

Hier sind einige Fragen, die sich oft bei ACLs im Datenspeicher See.

### <a name="do-i-have-to-enable-support-for-acls"></a>Muss Unterstützung für ACLs aktivieren?

Nein. Zugangskontrolle über ACLs steht für ein Konto See Datenspeicher.

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Welche Berechtigungen sind erforderlich, um rekursiv löschen einen Ordner und seinen Inhalt?

* Der Ordner müssen **Schreiben + Ausführen**.
* Erfordert die Ordner gelöscht werden, und jeder, **Lesen + Schreiben und Ausführen**.
>[AZURE.NOTE] Löschen von Dateien in Ordnern müssen nicht Schreibzugriff auf diese Dateien. Auch das Stammverzeichnis "/" kann **nicht** gelöscht werden.

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>Der als Besitzer einer Datei oder eines Ordners festlegen?

Der Ersteller einer Datei oder eines Ordners wird der Besitzer.

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Die besitzende Gruppe einer Datei oder eines Ordners zur Erstellung festgelegt ist?

Er wird aus der Eigner-Gruppe des übergeordneten Ordners kopiert die neuen Dateien oder Ordnern erstellt wird.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Ich bin der besitzenden Benutzer einer Datei, aber ich habe RWX Berechtigungen benötigte. Was soll ich tun?

Der besitzende Benutzer kann die Berechtigungen der Datei selbst RWX Berechtigungen geben, einfach ändern.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Unterstützt See Datenspeicher Vererbung von Zugriffssteuerungslisten?

Nein.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Was ist der Unterschied zwischen Maske und Umask?

| Maske | umask|
|------|------|
| Die **Mask** -Eigenschaft ist auf alle Dateien und Ordner. | Die **Umask** ist eine Eigenschaft des Kontos See Datenspeicher. So gibt es nur einen einzigen Umask im Datenspeicher See.    |
| Die Mask-Eigenschaft auf eine Datei oder einen Ordner kann vom besitzenden Benutzer oder Eigner-Gruppe einer Datei oder einem Super-User geändert werden. | Die Umask-Eigenschaft kann von jedem Benutzer auch einen super User geändert werden. Es ist eine unveränderliche Konstante Wert.|
| Die Mask-Eigenschaft ist, werden während der Algorithmus überprüft Access zur Laufzeit bestimmen, ob ein Benutzer das Recht zum Ausführen einer Operation für eine Datei oder einen Ordner. Rolle der Maske ist bei Zugriff Kontrollkästchen "effektive Berechtigungen" erstellen. | Die Umask wird nicht während überprüft Access verwendet. Die Umask wird die ACL des neuen untergeordneten Elemente eines Ordners bestimmt. |
| Die Maske ist ein 3-Bit RWX Wert, der benannte Benutzer, Gruppe und besitzenden Benutzer zum Zeitpunkt der Zugriffstest gilt.| Die Umask Wert 9 Bit, die für die besitzende Benutzer Eigentümergruppe und andere ein neues untergeordnetes Element gilt.| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Wo erfahre ich mehr über POSIX-Modell?

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.HTML](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [BIETET Berechtigung Guide](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [POSIX-FAQ](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [POSIX ACL für Linux](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [ACL unter Linux mit Zugriffssteuerungslisten](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Siehe auch

* [Übersicht über Azure See Datenspeicher](data-lake-store-overview.md)

* [Erste Schritte mit Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





