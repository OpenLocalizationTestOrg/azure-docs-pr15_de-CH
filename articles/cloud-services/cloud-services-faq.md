<properties
    pageTitle="Cloud-Dienste häufig gestellte Fragen | Microsoft Azure"
    description="Häufig gestellte Fragen zum Cloud-Dienste."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Cloud Services FAQ
Dieser Artikel beantwortet einige häufig gestellten Fragen zu Microsoft Azure Cloud Services. Besuchen Sie auch [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) allgemeine Azure Preise und Support-Informationen. Sie auch finden [Cloud Services VM Seite](cloud-services-sizes-specs.md) Informationen.

## <a name="certificates"></a>Zertifikate

### <a name="where-should-i-install-my-certificate"></a>Wo sollte ich mein Zertifikat installieren?

- **Meine**  
Anwendungszertifikat mit dem privaten Schlüssel (\*PFX, \*p12).

- **ZERTIFIZIERUNGSSTELLE**  
Alle Zwischenzertifikate gehen in diesem Speicher (Richtlinie und Sub-CAs).

- **STAMM**  
Die Stamm-CA speichern Ihre wichtigsten Stamm-Zertifizierungsstellenzertifikat hier gehen sollte.

### <a name="i-cant-remove-expired-certificate"></a>Abgelaufenes Zertifikat kann nicht entfernt werden.

Azure verhindert, dass Sie ein Zertifikat entfernen, während es verwendet wird. Sie müssen die Bereitstellung, die das Zertifikat verwendet löschen oder Aktualisieren der Bereitstellung mit einer anderen oder erneuerten Zertifikats.

### <a name="delete-an-expired-certificate"></a>Ein abgelaufenes Zertifikat löschen

Solange das Zertifikat nicht verwendet wird, können [Entfernen AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell-Cmdlet Sie ein Zertifikat entfernen.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Zertifikate mit dem Namen Windows Azure Service Management Extensions sind abgelaufen werden.

Diese Zertifikate werden erstellt, eine Erweiterung der Cloud-Dienst wie Remotedesktop-Erweiterung hinzugefügt wird. Diese Zertifikate werden nur zum Verschlüsseln und Entschlüsseln von privaten Konfiguration der Erweiterung. Es spielt keine Rolle, wenn Zertifikate ablaufen. Das Ablaufdatum wird nicht überprüft.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Zertifikate, die ich gelöscht werden angezeigt

Diese wahrscheinlich wegen einem Tool werden angezeigt, die Sie wie Visual Studio verwenden. Bei jedem mit einem Tool, das ein Zertifikat verwendet schließen, wird es wieder in Azure hochgeladen.

### <a name="my-certificates-keep-disappearing"></a>Meine Zertifikate verschwinden immer

Bei Freigabe die virtuellen Instanz werden alle lokale Änderungen verloren. Verwenden Sie eine [Startaufgabe](cloud-services-startup-tasks.md) Zertifikate für den virtuellen Computer jedes Mal installieren, gestartet wird.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Mein Management-Zertifikate im Portal nicht auffindbar

[Verwaltungszertifikate](..\azure-api-management-certs.md) stehen nur in der Azure-Verwaltungsportal. Aktuelle Azure-Portal verwendet keine Verwaltungszertifikate. 

### <a name="how-can-i-disable-a-management-certificate"></a>Wie kann ein Verwaltungszertifikat deaktivieren?

[Verwaltungszertifikate](..\azure-api-management-certs.md) kann nicht deaktiviert werden. Löschen sie mithilfe der Azure-Verwaltungsportal als nicht mehr verwendet werden soll.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Erstellen ein SSL-Zertifikat für eine bestimmte IP-Adresse

[Erstellen einer Zertifikat-Lernprogramm](cloud-services-certs-create.md)befolgen. Verwenden Sie die IP-Adresse als den DNS-Namen.

## <a name="security"></a>Sicherheit

### <a name="disable-ssl-30"></a>Deaktivieren Sie SSL 3.0

Deaktivieren Sie SSL 3.0 und TLS Sicherheit auf diesem Blogbeitrag erstellen Startaufgabe dokumentiert: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Skalieren eines Cloud-Diensts

### <a name="i-cannot-scale-beyond-x-instances"></a>Kann nicht über X skaliert Instanzen

Azure-Abonnement ist begrenzt auf die Anzahl der Kerne, die Sie verwenden können. Skalierung funktioniert nicht, wenn Sie alle verfügbaren Kerne verwendet haben. Haben Sie ein Limit von 100 Kerne bedeutet dies beispielsweise konnte man 100 A1 Größe virtuellen Instanzen für den Clouddienst oder 50 A2 Größe Instanzen virtueller Computer.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>IP in einer Multi-VIP-Cloud-Dienst kann nicht reserviert werden.

Vergewissern Sie sich, dass die virtuellen Computer-Instanz, die die IP-Adresse reservieren eingeschaltet ist. Stellen Sie dann sicher, dass Sie reservierte IP-Adressen für Mühe Staging- und Bereitstellung verwenden. **Nicht** Einstellungen die während die Bereitstellung aktualisieren.

