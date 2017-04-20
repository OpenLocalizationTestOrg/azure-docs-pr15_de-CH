<properties
   pageTitle="Verwalten von Rollen in der Azure-Cloud-Dienste mit Visual Studio | Microsoft Azure"
   description="Informationen Sie zum Azure-Cloud-Dienstprojekt neue Rollen hinzufügen oder vorhandene Rollen entfernen, mit Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Verwalten von Azure Cloud Services Projekte mit Visual Studio

Nachdem Sie Ihre Azure-Cloud-Dienstprojekt erstellt haben, können Sie neue Rollen hinzufügen oder vorhandene Rollen entfernen. Auch können Sie ein vorhandenes Projekt importieren und Konvertieren in eine Rolle. Sie können z. B. eine ASP.NET Web-Anwendung importieren und als Web-Rolle festlegen.

## <a name="adding-or-removing-roles"></a>Hinzufügen oder Entfernen von Rollen

**Hinzufügen eine Rolle**

Öffnen Sie im **Projektmappen-Explorer**im Kontextmenü für den Knoten **Rollen** im Cloud-Dienstprojekt zu, und wählen Sie **Hinzufügen**. Sie können eine vorhandene Web oder workerrolle aus der aktuellen Projektmappe auswählen oder erstellen ein neues Projekt für Web oder Worker-Rolle. Oder wählen Sie ein entsprechendes Projekt wie ein ASP.NET Webanwendungsprojekt und eine Rolle Projekt zuordnen.

**Eine Rolle Assoziation**

Öffnen Sie in der Cloud-Dienst-Projekt im Projektmappen-Explorer den Knoten **Rollen** im Kontextmenü für die Rolle zu entfernen und **zu entfernen**.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Entfernen und Hinzufügen von Rollen in der Cloud-Dienst

Entfernen Sie eine Rolle aus der Cloud-Dienstprojekt aber später die Rolle zum Projekt hinzufügen werden nur die Rolle Deklaration und Hauptattribute wie Endpunkte und Diagnose hinzugefügt. Zusätzliche Ressourcen keine Verweise werden in die Datei ServiceDefinition.csdef oder ServiceConfiguration.cscfg-Datei hinzugefügt. Wenn Sie diese Informationen hinzufügen möchten, müssen Sie manuell in diese Dateien.

Beispielsweise könnte eine Web Service-Rolle entfernen und später die Funktion in der Projektmappe hinzugefügt. Wenn Sie dies tun, tritt ein Fehler auf. Zur Vermeidung dieses Fehlers müssen Sie Hinzufügen der `<LocalResources>` Element im folgenden XML-Code in der Datei ServiceDefinition.csdef angezeigt. Verwenden Sie die Web Service-Rolle, die in das Projekt als Teil des Name-Attributs für hinzugefügt der **<LocalStorage>** Element. In diesem Beispiel ist der Name der Rolle Web Service **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Nächste Schritte

Enthält Informationen zum Konfigurieren von Rollen in Visual Studio anhand von [Rollen für eine Azure-Cloud-Dienst mit Visual Studio konfigurieren](vs-azure-tools-configure-roles-for-cloud-service.md)
