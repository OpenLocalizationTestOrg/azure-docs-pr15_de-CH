<properties
    pageTitle="Verwendung die Servicemanagement-API (Python) - Leistungsmerkmale"
    description="Informationen Sie zum gemeinsamen Dienstverwaltungsaufgaben Python programmgesteuert ausführen."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>Verwendung von Python Servicemanagement

> [AZURE.NOTE] Service Management-API wird mit der neuen Ressource Management API derzeit in einer Vorabversion ersetzt.  Anzeigen Sie der [Azure-Ressourcenmanagement-Dokumentation](http://azure-sdk-for-python.readthedocs.org/) zur Verwendung der neuen Ressource-API aus Python

Dieses Handbuch veranschaulicht die allgemeine Dienstverwaltungsaufgaben Python programmgesteuert ausführen. **ServiceManagementService** -Klasse in [Azure SDK für Python](https://github.com/Azure/azure-sdk-for-python) unterstützt den programmgesteuerten Zugriff auf den Großteil der Service Management-Funktionen, die in [Azure-Verwaltungsportal] [ management-portal] (z. B. **erstellen, aktualisieren und Löschen von Clouddiensten, Bereitstellung, Daten-Management-Services und virtuelle Computer**). Dies ist nützlich in erstellen, die programmgesteuerten Zugriff auf Service-Management.

## <a name="WhatIs"> </a>Was wird Servicemanagement
Die Dienst-API ermöglicht programmgesteuerten Zugriff auf viele Service Management Funktionen über das [klassische Azure-Portal][management-portal]. Azure SDK Python können Sie Ihre Clouddienste und Speicherkonten verwalten.

Um die Dienst-API zu verwenden, müssen Sie [ein Azure-Konto erstellen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>(Konzepte)
Azure SDK Python umschließt [Azure Service Management API][svc-mgmt-rest-api], also eine REST-API. Alle API-Vorgänge erfolgen über SSL und authentifizieren sich gegenseitig mit x. 509 v3-Zertifikate. Der Verwaltungsdienst kann in einen Dienst in Azure oder direkt über das Internet von jeder Anwendung, die eine HTTPS-Anforderung senden und empfangen eine HTTPS-Antwort aufgerufen werden.

## <a name="Installation"> </a>Installation

In diesem Artikel beschriebenen Features stehen in der `azure-servicemanagement-legacy` Paket mit Pip installieren können. Weitere Informationen zur Installation (z. B. Wenn Sie Python sind) finden Sie in diesem Artikel: [Installation von Python und Azure SDK](../python-how-to-install.md)

## <a name="Connect"> </a>Wie: Verbinden mit Service-Management
Verbindung mit dem Service Management-Endpunkt benötigen Sie Ihre Azure-Abonnement-ID und eine gültige Zeugnisse. Sie erhalten Ihre Abonnenten-ID über das [klassische Azure-Portal][management-portal].

> [AZURE.NOTE] Seit Azure SDK für Python-v0.8.0 ist es möglich, Zertifikate mit OpenSSL unter Windows verwenden.  Erfordert Python 2.7.4 oder höher. Wir empfehlen Benutzern, OpenSSL PFX, da die Unterstützung für PFX anstelle Zertifikate werden in Zukunft wahrscheinlich entfernt.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Zertifikate auf Windows/Mac/Linux (OpenSSL) Management
[OpenSSL](http://www.openssl.org/) können Sie Ihre Management-Zertifikat erstellen.  Tatsächlich zwei Zertifikate für den Server erstellen müssen (ein `.cer` Datei) und eine für den Client (ein `.pem` Datei). Erstellen der `.pem` Datei, auszuführen:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Erstellen der `.cer` Zertifikat, ausführen:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Weitere Informationen zu Azure Zertifikaten finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](./cloud-services-certs-create.md). Eine vollständige Beschreibung der OpenSSL-Parameter finden Sie unter [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Nachdem diese Dateien erstellt haben, müssen Sie Hochladen der `.cer` Datei Azure über die Aktion "Hochladen" der Registerkarte "Settings" [Azure-Verwaltungsportal][management-portal], und müssen Notieren Sie den Speicherort der `.pem` Datei.

Nachdem Sie Ihre Abonnenten-ID erhalten haben, ein Zertifikat erstellt und Übertragen der `.cer` Datei auf Azure herstellen Azure Management-Endpunkt die Abonnement-Id und den Pfad der `.pem` Datei **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

Im vorhergehenden Beispiel `sms` ist ein **ServiceManagementService** -Objekt. **ServiceManagementService** -Klasse ist die primäre Klasse zum Verwalten von Azure Services verwendet.

### <a name="management-certificates-on-windows-makecert"></a>Verwaltungszertifikate unter Windows (MakeCert)

Erstellen eines selbstsignierten Management Zertifikats auf Ihrem Computer `makecert.exe`.  **Visual Studio-Eingabeaufforderungsfenster** als **Administrator** öffnen Sie und verwenden Sie den folgenden Befehl, *AzureCertificate* der Name des Zertifikats, das Sie verwenden möchten.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Der Befehl erstellt das `.cer` Datei und im **persönlichen** Zertifikatspeicher installiert. Weitere Informationen finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](./cloud-services-certs-create.md).

Nachdem Sie das Zertifikat erstellt haben, müssen Sie Hochladen der `.cer` Datei in Azure über die Aktion "Hochladen" der Registerkarte "Settings" [Azure-Verwaltungsportal][management-portal].

Wenn Sie Ihre Abonnenten-ID erhalten haben, ein Zertifikat erstellt und Übertragen der `.cer` Datei in Azure, Sie können Azure Management-Endpunkt durch die Abonnement-Id und den Speicherort des Zertifikats in Ihrem **persönlichen** Zertifikatspeicher an **ServiceManagementService** (in diesem Fall ersetzen *AzureCertificate* mit dem Namen des Zertifikats) übergeben:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

Im vorhergehenden Beispiel `sms` ist ein **ServiceManagementService** -Objekt. **ServiceManagementService** -Klasse ist die primäre Klasse zum Verwalten von Azure Services verwendet.

## <a name="ListAvailableLocations"> </a>Wie: Liste der verfügbaren Standorte

Um die Standorte aufzulisten, die für Hosting-Services verfügbar sind, verwenden die **Liste\_Speicherorte** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Beim Erstellen einer Cloud-Dienst oder Speicherdienst müssen Sie einen gültigen Pfad angeben. Die **Liste\_Speicherorte** Methode gibt immer eine aktuelle Liste der derzeit verfügbaren Speicherorte. Derzeit sind die verfügbaren Standorte:

- Westeuropa
- Nordeuropa
- Südostasien
- Ostasien
- USA
- Norden der USA – zentral
- Südlichen zentralen USA
- Westen der USA
- Osten der USA
- Japan OST
- Japan West
- Brasilien Süd
- Australien OST
- Australien Südost

## <a name="CreateCloudService"> </a>Wie: einen Clouddienst erstellen

Wenn Sie eine Anwendung erstellen und in Azure ausführen, werden Code und Konfiguration zusammen eine Azure [cloudservice][] (ein *gehosteter Dienst* in früheren Versionen Azure genannt) genannt. Die **Erstellen\_gehostet\_Service** -Methode können Sie einen neuen gehosteten Dienst erstellen, indem ein gehosteter Dienst (die in Azure eindeutig sein muss), einer Bezeichnung (automatisch base64-codiert), eine Beschreibung und einen Speicherort.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Listen Sie die gehostete Dienste für Ihr Abonnement mit der **Liste\_gehostet\_Services** Methode:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Möchten Sie Informationen über einen bestimmten gehosteten Dienst, Sie dazu zu gehosteter Dienst übergeben der **abrufen\_gehostet\_Service\_Eigenschaften** Methode:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Nach dem Erstellen eines Cloud-Diensts können Sie Code bereitstellen, mit dem **Erstellen\_Bereitstellung** Methode.

## <a name="DeleteCloudService"> </a>Wie: einen Cloud-Dienst löschen

Löschen einen Cloud-Dienst übergeben den Namen, den **Löschen\_gehostet\_Service** Methode:

    sms.delete_hosted_service('myhostedservice')

Vor dem Löschen eines Dienstes müssen alle Installationen für den Dienst zuerst gelöscht. (Siehe [wie: löschen eine Bereitstellung](#DeleteDeployment) Weitere Informationen.)

## <a name="DeleteDeployment"> </a>Wie: löschen eine Bereitstellung

Um eine Bereitstellung zu löschen, verwenden Sie die **Löschen\_Bereitstellung** Methode. Das folgende Beispiel zeigt eine Bereitstellung mit dem Namen löschen `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Wie: Erstellen eines

[Storage-Service](../storage/storage-create-storage-account.md) erhalten Sie Zugriff auf Azure- [Blobs](../storage/storage-python-how-to-use-blob-storage.md), [Tabellen](../storage/storage-python-how-to-use-table-storage.md)und [Warteschlangen](../storage/storage-python-how-to-use-queue-storage.md). Erstellen eines benötigen Sie einen Namen für den Dienst (zwischen 3 und 24 Kleinbuchstaben und in Azure eindeutig), eine Beschreibung, eine Bezeichnung (bis zu 100 Zeichen automatisch base64-codiert) und einen Speicherort an. Im folgenden Codebeispiel wird das Erstellen eines durch Angabe einer Adresse.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Beachten Sie im vorherigen Beispiel, den Status der **Erstellen\_Speicher\_Konto** Operation übergeben das zurückgegebene Ergebnis abgerufen werden **Erstellen\_Speicher\_Konto** , die **erhalten\_Vorgang\_Status** Methode.  

Speicherkonten und Eigenschaften durch Auflisten der **Liste\_Speicher\_Konten** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Wie: Löschen eines

Löschen eines übergeben Storage Service Name, um die **Löschen\_Speicher\_Konto** Methode. Löschen eines löscht alle Daten (Blobs, Tabellen und Warteschlangen).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Wie: Liste der verfügbaren Betriebssysteme

Um die Betriebssysteme aufzulisten, die für Hosting-Services verfügbar sind, verwenden die **Liste\_Betrieb\_Systeme** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Alternativ können Sie die **Liste\_Betrieb\_System\_Familien** Methode, welche Betriebssysteme von Familie:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Wie: ein Betriebssystemabbild erstellen

Um ein Betriebssystemabbild Bildspeicher hinzuzufügen, verwenden Sie die **Hinzufügen\_os\_Bild** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Um die Betriebssystemabbilder aufzulisten, die verfügbar sind, verwenden die **Liste\_os\_Bilder** Methode. Es enthält alle Plattform-Images und Benutzerbilder:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Wie: ein Betriebssystemabbild löschen

Um ein Bild zu löschen, verwenden Sie die **Löschen\_os\_Bild** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Wie: erstellen eine virtuellen Maschine

Um einen virtuellen Computer erstellen, müssen Sie zunächst einen [Cloud-Dienst](#CreateCloudService)erstellen.  Erstellen der virtuellen Bereitstellung mithilfe der **Erstellen\_virtuellen\_Computer\_Bereitstellung** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Wie: löschen eine virtuellen Maschine

Um einen virtuellen Computer löschen, zuerst Löschen der Bereitstellung mit der **Löschen\_Bereitstellung** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Cloud-Dienst kann dann gelöscht werden mit der **Löschen\_gehostet\_Service** Methode:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Gewusst wie: Erstellen eines virtuellen Computers von aufgezeichneten VM Image

Um eine VM-Aufnahme rufen Sie zuerst die **erfassen\_Vm\_Bild** Methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Anschließend verwenden, um sicherzustellen, dass Sie erfolgreich Aufnahme der **Liste\_Vm\_Bilder** -api und das Bild in den Suchergebnissen angezeigt werden soll:

    images = sms.list_vm_images()

Verwenden Sie schließlich das aufgenommene Bild mit virtuellen Computer erstellen die **Erstellen\_virtuellen\_Computer\_Bereitstellung** Methode stattdessen übergeben werden, aber dieses Mal in der Vm_image_name

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Weitere Informationen zum virtuellen Linux-Maschine aufnehmen, finden Sie [wie virtuellen Linux-Maschine.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Weitere Informationen zum Erfassen von einem Windows-Computer finden Sie unter [einen virtuellen Windows-Maschine erfassen.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Nächste Schritte

Die Grundlagen der Servicemanagement bearbeitet haben, können die [vollständige API-Referenzdokumentation für Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) und komplexe Aufgaben einfach zum Verwalten der Python-Anwendung.

Weitere Informationen finden Sie unter [Python Developer Center](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[Cloud-Dienst]:/services/cloud-services/

