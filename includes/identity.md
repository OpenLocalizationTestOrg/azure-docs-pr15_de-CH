Verwalten von Identitäten ist ebenso in der öffentlichen Cloud auf lokal. Azure unterstützt dafür, mehrere unterschiedliche Identität Technologie. Sie gehören:

- Sie können Windows Server Active Directory (nur Anzeige häufig genannt) in der Cloud mit Azure virtuelle Computer erstellte virtuelle Maschinen ausführen. Dieser Ansatz ist sinnvoll, wenn Azure verwendeten lokalen Datencenter in die Cloud erweitern.


- Azure Active Directory können zu Ihrem Benutzer SSO- [Software als Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) Anwendung. Microsoft Office 365 verwendet diese Technologie beispielsweise und Programme auf Azure oder andere Cloudplattformen können es auch verwenden.


- In der Cloud oder lokal ausgeführt können Benutzer mithilfe von Facebook, Google, Microsoft und anderen Identitäten anmelden Azure Active Directory Access Control.


Dieser Artikel beschreibt alle drei Optionen.

## <a name="table-of-contents"></a>Inhaltsverzeichnis

- [Windows Server Active Directory auf virtuellen Maschinen ausgeführt](#adinvm)

- [Verwenden von Azure Active Directory](#ad)

- [Mithilfe von Azure Active Directory Access Control](#ac)


## <a name="adinvm"></a>Windows Server Active Directory auf virtuellen Maschinen ausgeführt

Unter Windows Server Active Directory in Azure virtuellen Maschinen ist wie lokal ausgeführt. [Abbildung 1](#fig1) zeigt z. B. wie diese aussieht.

![Azure Active Directory in Virtual Machine](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Abbildung 1: Windows Server Active Directory führen in Azure virtuelle Computer in einer Organisation lokalen Rechenzentrum Azure virtuelles Netzwerk verbunden.

In dem hier gezeigten Beispiel wird Windows Server AD erstellt mithilfe von Azure Virtual Machines-Plattform IaaS VMs ausgeführt. In einem virtuellen Netzwerk an einem lokalen Rechenzentrum Azure virtuelles Netzwerk angeschlossen werden die VMs und einige andere gruppiert. Das virtuelle Netzwerk bietet eine Gruppe von Cloud virtuelle Computer, die Interaktion mit dem lokalen Netzwerk Verbindung virtuelles privates Netzwerk (VPN). Tun können Aussehen nur ein anderes Subnetz in der lokalen Datencenter diese Azure virtuellen Computer. Wie die Abbildung zeigt ausführen zwei die VMs Windows Server Active Directory-Domänencontroller. Die anderen virtuellen Computer im virtuellen Netzwerk können Clientanwendungen wie SharePoint oder verwendeten auf andere Weise, z. B. für Entwicklung und testing ausgeführt werden. Lokalen Datencenter zwei Windows Server Active Directory-Domänencontroller ausgeführt.

Es gibt mehrere Optionen für die Domänencontroller in der Cloud mit lokal ausgeführt:

- Alle in einer einzelnen Active Directory-Domäne.

- Erstellen Sie separate Active Directory-Domänen lokal und in der Cloud, die zu derselben Gesamtstruktur gehören.

- Erstellen Sie separate Active Directory-Gesamtstrukturen in der Cloud und lokalen und verbinden Sie die Gesamtstrukturen gesamtstrukturübergreifende Vertrauensstellungen oder Windows Server Active Directory Federation Services (AD FS), die auch in virtuellen Maschinen Azure ausführen können.

Was entschieden, ein Administrator sicherstellen, dass Cloud-Domänencontroller nur bei Bedarf Authentifizierungsanfragen von lokale Benutzer gehen die Verknüpfung in die Cloud zu lokalen Netzwerken langsamer ist. Einen anderen Faktor verbinden Cloud und lokalen Domänencontroller ist von der Replikation erzeugte Verkehr. Domänencontroller in der Cloud sind normalerweise ihre eigenen AD-Standort, wie häufig planen Administratoren ermöglicht Replikation erfolgt. Azure Kosten für Datenverkehr aus einem Azure-Rechenzentrum zwar nicht Bytes gesendet, die der Administrator Replikation Optionen auswirken. Es ist auch erwähnenswert, dass Azure bereit (DNS = Domain Name System) unterstützt diesen Dienst Funktionen erforderlich sind (z. B. Unterstützung für dynamisches DNS und SRV-Einträge) von Active Directory fehlt. Aus diesem Grund erfordert Windows Server AD auf Azure Sie eigene DNS-Server in der Cloud.

Unter Windows Server Active Directory in Azure VMs kann in verschiedenen Situationen sinnvoll. Es folgen einige Beispiele:

- Wenn mithilfe von Azure Virtual Machines als Erweiterung des eigenen Rechenzentrum können Sie Applikationen in der Cloud, die lokalen Domänencontroller Dinge wie integrierte Windows-Authentifizierung oder LDAP-Abfragen ausführen. SharePoint, z. B. arbeitet häufig mit Active Directory und so während eine SharePoint-Farm auf Azure mit einer lokalen Verzeichnis ausgeführt werden kann, Einrichten von Domänencontrollern in der Cloud verbessert Leistung. (Es ist zu beachten, dass dies nicht unbedingt erforderlich, jedoch viele Programme können erfolgreich in der Cloud nur für lokale Domänencontroller)

- Angenommen Sie, ferne Zweigstelle verfügt nicht über die Ressourcen, um einen eigenen Domänencontroller. Heute muss der Benutzerauthentifizierung auf Domänencontroller auf der anderen Seite der Welt - Logins sind langsam. Active Directory in einem näher Microsoft-Datencenter auf Windows Azure ausgeführte kann dies beschleunigen ohne weitere Server in der Zweigstelle.

- Eine Organisation, die Azure für Disaster Recovery verwendet möglicherweise eine kleine Gruppe von aktiven VMs in der Cloud, einschließlich Domänencontroller verwalten. Sie können dann hier ggf. Fehler an anderer Stelle übernehmen erweitern vorbereitet werden.

Es bestehen auch andere. Z. B. sind Sie nicht erforderlich Windows Server Active Directory in der Cloud ein lokalen Datencenter. Wenn Sie eine SharePoint-Farm ausführen, die eine bestimmte Gruppe von Benutzern bereitgestellt, beispielsweise alle ausschließlich mit cloudbasierten anmelden würde, Sie erstellen eine eigenständige Gesamtstruktur auf Azure. Die Verwendung dieser Technologie hängt von Ihren Zielen. (Für eine ausführlichere Anleitung auf Windows Server Active Directory Azure [finden Sie hier](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Verwenden von Azure Active Directory

SaaS-Applikationen häufiger werden, lösen diese Frage: Welche Verzeichnisdienst verwenden diesen cloudbasierten? Microsofts Antwort auf diese Frage ist Azure Active Directory.

Es gibt zwei Hauptoptionen für diesen Directory Service in der Cloud:

- Personen und Organisationen, die nur SaaS Anwendung können Azure Active Directory als ihre einzige Verzeichnisdienst abhängig.

- Organisationen, die Windows Server Active Directory ausführen können Azure Active Directory ihrer lokalen Verzeichnis herstellen und dann zu ihrer Benutzer einmaliges SaaS-Applikationen.


[Abbildung 2](#fig2) zeigt die erste der beiden Optionen, Azure Active Directory ist, die erforderlich ist.

![Azure Active Directory in Virtual Machine](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Abbildung 2: Azure Active Directory verleiht der Organisation Benutzer einmaliges SaaS-Applikationen, einschließlich Office 365.

Wie die Abbildung zeigt, ist Azure AD ein Multi-Tenant-Dienst. Dies bedeutet, dass gleichzeitig viele verschiedene Organisationen Speichern von Verzeichnisinformationen zu Benutzern jeweils von ihnen unterstützen können. In diesem Beispiel versucht ein Benutzer in der Organisation ein SaaS-Anwendung zugreifen. Diese Anwendung kann Teil von Office 365 wie SharePoint Online oder möglicherweise etwas - fremde Applikationen können auch diese Technologie. Da Azure AD SAML 2.0-Protokoll unterstützt, von einer Anwendung erforderlich ist Interaktion mit diesen Industriestandard. (Tatsächlich können, Azure AD verwenden, in Datencenters, nicht nur ein Azure-Rechenzentrum ausgeführt.)

Der Prozess beginnt, wenn der Benutzer greift auf eine SaaS-Anwendung (Schritt 1). Um diese Anwendung zu verwenden, muss der Benutzer ein Token ausgestellt von Azure AD angeben.

Dieses Token enthält Informationen, die den Benutzer identifiziert und von Azure AD digital signiert. Zum Abrufen eines Tokens, authentifiziert den Benutzer selbst Azure AD einen Benutzernamen und ein Kennwort (Schritt 2). Dann Azure AD gibt das Token zurück, er muss (Schritt 3).

Dieses Token wird dann an die SaaS-Anwendung (Schritt 4) gesendet, überprüft das Token Signatur verwendet die Inhalte (Schritt 5). Normalerweise wird die Identitätsinformationen Anwendung Token um zu entscheiden, welche Informationen der Benutzer Zugriff und vielleicht enthält.

Wenn die Anwendung Informationen über den Benutzer als was im Token enthalten ist, können sie dies direkt von Azure AD mit Azure AD Graph-API (Schritt 6) anfordern. In der ersten Version von Azure AD Verzeichnisschema ist einfach: Es enthält nur Benutzer, Gruppen und Beziehungen zwischen ihnen. Diese Informationen können Applikationen Informationen zwischen Benutzern. Angenommen Sie, eine Anwendung muss wissen, Manager des Benutzers zu entscheiden, ob er Zugriff auf einige Datenmenge ist. Sie finden diese Abfragen Azure AD über Graph-API.

Graph-API verwendet eine gewöhnliche RESTful Protokoll macht es einfach mit den meisten Kunden, einschließlich mobiler Geräte. Die API unterstützt auch Erweiterungen durch OData hinzufügen Dinge wie eine Abfragesprache lassen Kunden Zugriff auf Daten in sinnvoller Weise definiert. (Weitere Informationen zu OData Siehe [Einführung OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Da die Graph-API Beziehungen zwischen Benutzern Informationen verwendet werden kann, können Applikationen soziale Graphen zu verstehen, die in Azure AD-Schema für eine bestimmte Organisation eingebettet ist (weshalb man die Graph-API). Und selbst Azure AD Graph-API Anfragen Authentifizierung verwendet eine Anwendung OAuth 2.0.

Wenn eine Organisation Windows Server Active Directory nicht verwenden, hat keine lokalen Server oder Domänen - und basiert ausschließlich auf Cloudanwendungen, die Azure AD verwenden, würde mit dieser Cloud-Verzeichnis des Unternehmens Benutzer SSO-alle geben. Noch während dieses Szenario häufiger jeden Tag wird, lokalen die meisten Unternehmen verwenden weiterhin Domänen mit Windows Server Active Directory erstellt. Azure AD hat eine nützliche Rolle hier, wie in [Abbildung 3](#fig3) dargestellt.

![Azure Active Directory in virtuellen](./media/identity/identity_03_AD.png)
<a id="fig3"></a>Abbildung 3: eine Organisation kann Windows Server Active Directory Azure Active Directory zu seiner Benutzer einmaliges SaaS-Applikationen Föderation.

In diesem Szenario ein Benutzer bei Organisation B eine SaaS-Anwendung zugreifen möchte. Bevor sie dies müssen Organisationsadministratoren Verzeichnis eine Föderation mit Azure mit AD FS als Abbildung Beziehung. Außerdem müssen die Administratoren Datensynchronisation zwischen der Organisation lokalen Windows Server Active Directory und Azure AD konfigurieren. Dies übernimmt die Benutzer- und Gruppeninformationen aus dem lokalen Verzeichnis Azure AD. Beachten Sie dies ermöglicht:, Organisation erweitert seine lokalen Verzeichnis in der Cloud. Windows Server AD und Azure AD so kombinieren bietet Unternehmen einen Verzeichnisdienst, der trotz einer Präsenz sowohl lokal und in der Cloud als eine Einheit verwaltet werden.

Um Azure AD verwenden, meldet sich der Benutzer zuerst ihre lokale Active Directory-Domäne wie gewohnt (Schritt 1). Wenn sie Zugriff auf die SaaS-Anwendung (Schritt 2) versucht, führt der Föderation Prozess Azure AD ausstellenden sie einen Token für diese Anwendung (Schritt 3). (Weitere Informationen zur Funktionsweise der Föderation finden Sie [anspruchsbasierte Identität für Windows: Technologies und Szenarios](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Vor diesem Token enthält Informationen, die den Benutzer identifiziert und von Azure AD digital signiert. Dieses Token wird dann an die SaaS-Anwendung (Schritt 4) gesendet, überprüft das Token Signatur verwendet die Inhalte (Schritt 5). Im vorherigen Szenario Anwendung die Graph-API Weitere Informationen zu diesem Benutzer wenn können SaaS ist erforderlich (Schritt 6).

Azure AD ist heute kein vollständiger Ersatz für lokalen Windows Server Active Directory. Wie bereits erwähnt cloudverzeichnis ist ein viel einfacheres Schema und es fehlt auch z. B. Gruppenrichtlinien, die Möglichkeit, Informationen über Computer speichern und für LDAP unterstützen. (Tatsächlich, einem Windows-Computer nicht konfiguriert werden Benutzer mit nur Azure AD anmelden können - dies nicht unterstützt.) Stattdessen die Zielsetzung von Azure AD gehören Benutzer Zugriff Anwendung in der Cloud lassen, ohne eine separate Anmeldung und Freigeben von lokalen Verzeichnisadministratoren manuell synchronisieren ihrer lokalen Verzeichnis mit jeder SaaS-Anwendung, die ihre Organisation verwendet. Im Laufe der Zeit erwarten Sie jedoch dieses Cloud-Verzeichnisdienst an zahlreichen Szenarien.

## <a name="ac"></a>Mithilfe von Azure Active Directory Access Control

Cloud-basierte Identität Technologies können zu einer Vielzahl von Problemen verwendet werden. Azure Active Directory kann eine Organisation Benutzer SSO auf SaaS, verfügbar. Aber Identität Technologies in der Cloud können auch auf andere Weise verwendet werden.

Angenommen beispielsweise, eine Anwendung möchte der Benutzer mehrere *Identitätsanbieter (IDP)*ausgestellte Token anmelden. Viele andere Identitätsprovider sind heute Facebook, Google, Microsoft und anderen und Applikationen häufig Benutzer anmelden eine dieser Identitäten. Sollte eine Anwendung warum eine eigene Liste von Benutzern und Kennwörtern beizubehalten, wenn stattdessen auf Identitäten zurückgreifen können, die bereits vorhanden? Vorhandene Identitäten akzeptieren macht das Leben einfacher für Benutzer einen Benutzernamen und ein Kennwort merken und für die Benutzer die Anwendung erstellen, die keine eigene Listen von Benutzernamen und Kennwörtern.

Aber während jeder Identitätsanbieter eine Art Token ausstellt, diese Tokens nicht standard - jeder IdP ein eigenes Format hat. Die Informationen in diesen Token nicht darüber hinaus auch standard. Eine Anwendung, die Token ausgestellt, sagen, Facebook, Google und Microsoft übernehmen möchte steht vor der Herausforderung eindeutigen Code schreiben jeder dieser unterschiedlichen Formaten.

Aber warum? Warum nicht erstellen stattdessen Vermittler, der ein einzelnes Tokens Format eine allgemeine Darstellung von Identitätsinformationen erstellen kann? Dieser Ansatz würde machen das Leben einfacher für Clientanwendungen erstellen, da sie jetzt nur eines behandeln müssen Entwickler für token. Azure Active Directory Access Control wird genau, über Vermittler in der Cloud arbeiten mit verschiedenen Token. [Abbildung 4](#fig4) zeigt, wie es funktioniert

![Azure Active Directory in virtuellen](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>Abbildung 4: Azure Active Directory Access Control erleichtert Applikationen Identitätstoken andere Identitätsprovider ausgestellte anzunehmen.

Der Prozess beginnt, wenn ein Benutzer versucht, die Anwendung über einen Browser zugreifen. Die Anwendung leitet sie IdP ihrer Wahl (und auch der Anwendung vertraut). Sie authentifiziert sich diese IdP, z. B. durch Eingabe eines Benutzernamens und Kennworts (Schritt 1) und der IdP gibt einen Token mit Informationen über ihre (Schritt 2).

Wie die Abbildung zeigt unterstützt Access Control anderen cloudbasierten vertriebenen, einschließlich von Google, Yahoo, Facebook, Microsoft (früher Windows Live ID) und OpenID-Anbieter erstellt. Unterstützt auch Identitäten mit Active Directory Azure erstellt und mit AD FS Windows Server Active Directory. Ziel ist, auf die am häufigsten verwendeten Identitäten, ob sie von Vertriebenen in der Cloud oder lokalen ausgestellt sind.

Hat der Browser des Benutzers eine IdP Token von ihrem gewählten IdP, sendet dieses token an Access Control (Schritt 3). Zugriffskontrolle überprüft das Token stellt sicher, dass es wirklich von diesem IdP ausgestellt wurde, und erstellt ein neues Token nach Regeln, die für diese Anwendung definiert. Wie Active Directory Azure Access Control ist ein Multi-Tenant-Dienst Mieter sind jedoch Applikationen als Kunden. Jede Anwendung erhalten einen eigenen Namespace wie in der Abbildung dargestellt und kann verschiedene Regeln zur Autorisierung und mehr.

Diese Regeln können jede Anwendung Administrator definieren wie Token aus verschiedenen vertriebenen in ein Token Zugriffsteuerung umgewandelt werden. Beispielsweise verwenden verschiedene vertriebenen Arten für die Darstellung von Benutzernamen, verwandeln Zugriffskontrolle Regeln in einen allgemeinen Typ Benutzername. Zugriffskontrolle schickt dieses neue Token an den Browser (Schritt 4), der für die Anwendung (Schritt 5) sendet. Hat es das Token Zugriffskontrolle, überprüft die Anwendung dieses Token wirklich von Access Control ausgestellt wurde, dann verwendet die Informationen enthält (Schritt 6).

Während dieser Vorgang etwas kompliziert scheint, macht es Leben erheblich einfacher für den Ersteller der Anwendung. Statt unterschiedliche Token mit anderen Informationen zu behandeln, kann die Anwendung mehrere Identitätsanbieter beim Empfang immer noch eines einzelnen Tokens vertraute Angaben ausgestellte Identitäten akzeptieren. Auch als jede Anwendung verschiedene vertriebenen vertrauen konfiguriert werden muss diese Vertrauensstellungen bestehen stattdessen durch Access Control - eine Anwendung muss nur vertrauen.

Es ist erwähnenswert, dass nichts Zugriffskontrolle Windows hängt-ebenso wie eine Linux-Anwendung, die akzeptiert nur Google und Facebook Identitäten verwendet werden. Und obwohl Zugriffskontrolle Azure Active Directory-Familie gehört, Sie können es als einen völlig unterschiedlichen Service wie im vorherigen Abschnitt beschrieben wurde. Bei beiden mit Arbeit Probleme sie anders (Obwohl Microsoft hat zwei irgendwann integrieren erwartet).

Arbeiten mit Identität ist in nahezu jeder Anwendung wichtig. Die Zugriffskontrolle soll Entwickler Programme erstellen, die Identitäten von unterschiedlichen Identitätsanbieter akzeptieren erleichtern. Mit diesem Dienst in der Cloud, hat Microsoft es jeder Anwendung auf jeder Plattform ausgeführt zur Verfügung.

##<a name="about-the-author"></a>Informationen über den Autor

David Chappell ist Principal der Chappell & Associates [www.davidchappell.com](http://www.davidchappell.com) in San Francisco, Kalifornien.
