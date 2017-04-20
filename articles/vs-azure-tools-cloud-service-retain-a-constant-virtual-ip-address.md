<properties
   pageTitle="Wie eine ständige virtuelle IP-Adresse für einen Clouddienst beibehalten | Microsoft Azure"
   description="Informationen Sie zum sicherstellen, dass die virtuelle IP-Adresse (VIP) der Azure-Clouddienst ändert."
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

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Wie Sie eine ständige virtuelle IP-Adresse für einen Clouddienst beibehalten

Wenn Sie Cloud-Dienst, der in Azure gehostet wird aktualisieren, müssen Sie sicherstellen, dass die virtuelle IP-Adresse (VIP) des Dienstes ändert. Viele Domänendienste Management verwenden Domain Name System (DNS) für die Registrierung von Domänennamen. DNS funktioniert nur, wenn die VIP-Adresse unverändert. Können Sie den **Webpublishing-Assistenten** in Azure Tools sicher, dass die VIP des Cloud-Diensts ändert, wenn Sie aktualisieren. Weitere Informationen zur Verwendung von DNS domainmanagement für Cloud-Services finden Sie unter [konfigurieren einen benutzerdefinierten Domänennamen für einen Azure-Cloud-Dienst](./cloud-services/cloud-services-custom-domain-name.md).

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Cloud-Dienst veröffentlichen, ohne die VIP

Die VIP-Adresse eines Cloud-Diensts wird bei der ersten Azure in einer bestimmten Umgebung wie Produktionsumfeld Bereitstellung zugewiesen. Die VIP-Adresse ändern nicht, wenn der Bereitstellung explizit löschen oder implizit vom Bereitstellungsprozess Update gelöscht. Um die VIP beizubehalten, muss nicht die Bereitstellung löschen, und Sie müssen auch sicherstellen, dass Visual Studio die Bereitstellung nicht automatisch gelöscht. Steuern des Verhaltens von Bereitstellungseigenschaften im **Webpublishing-Assistenten**unterstützt mehrere Bereitstellungsoptionen angeben. Sie können eine neue Bereitstellung oder eine Bereitstellung, die inkrementelle oder gleichzeitig sein kann, und beide Arten von Update-Installationen VIP behalten. Definitionen der verschiedenen Typen der Bereitstellung finden Sie unter [Assistent Azure veröffentlichen](vs-azure-tools-publish-azure-application-wizard.md).  Darüber hinaus können Sie steuern, ob die vorherige Bereitstellung eines Cloud-Diensts gelöscht wird, wenn ein Fehler auftritt. Die VIP-Adresse möglicherweise unerwartet ändern, wenn Sie diese Option nicht korrekt festlegen.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Cloud-Dienst aktualisieren, ohne die VIP

1. Nach dem Cloud-Dienst einmal bereitstellen öffnen Sie das Kontextmenü für den Knoten für Ihre Azure-Projekt und wählen Sie dann **Veröffentlichen**. **Azure-Anwendung veröffentlichen** -Assistent wird angezeigt.

1. Die Liste der Abonnements wählen Sie bereitstellen möchten, und wählen Sie die Schaltfläche **Weiter** . Die **Standardeinstellungen** des Assistenten angezeigt.

1. Überprüfen Sie auf der Registerkarte **Allgemeine Eigenschaften** der Namen des Cloud-Dienst, die Sie bereitstellen, **Umgebung**, die **Buildkonfiguration**und die **Konfiguration** korrekt.

1. Überprüfen Sie auf der Registerkarte **Erweiterte Einstellungen** das Speicherkonto und die Bereitstellung Beschriftung, deaktiviert das Kontrollkästchen **Löschen auf Fehler** und das Kontrollkästchen **Bereitstellung** aktualisieren. Kontrollkästchens **Bereitstellung** aktualisieren, gewährleistet, dass die Bereitstellung nicht gelöscht und die VIP nicht verloren werden, wenn Sie die Anwendung veröffentlichen. Wenn Sie **auf das Kontrollkästchen Löschen**deaktivieren, sichergestellt, dass die VIP verloren tritt ein Fehler während der Bereitstellung.

1. Um weitere festzulegen, wie die Rollen, die aktualisiert werden sollen, wählen Sie **den Link neben das **Bereitstellung aktualisieren** ** und dann inkrementelle Aktualisierung oder gleichzeitige Aktualisierungsoption im Dialogfeld **Bereitstellung aktualisieren** . Wenn Sie inkrementelle Aktualisierung auswählen, wird jede Instanz aktualisiert, damit die Anwendung immer verfügbar ist. Wenn Sie gleichzeitigen Aktualisierung auswählen, werden alle Instanzen gleichzeitig aktualisiert. Gleichzeitige Aktualisierung ist schneller, aber der Dienst möglicherweise nicht zur Verfügung während des Aktualisierungsvorgangs.

1. Wenn Sie mit Ihren \\map="ainschtellungen"="Einstellungen"\ zufrieden sind, wählen Sie **Weiter** .

1. Auf der Seite **Zusammenfassung** des Assistenten die Konfiguration, und wählen Sie die Schaltfläche " **Veröffentlichen** ".

  >[AZURE.WARNING] Schlägt die Bereitstellung sollten Sie warum Fehler zu beheben und erneut unverzüglich um zu vermeiden Ihre Cloud-Dienst beschädigt.

## <a name="next-steps"></a>Nächste Schritte

Zur Veröffentlichung in Azure von Visual Studio finden Sie unter [Azure Veröffentlichen-Assistenten](vs-azure-tools-publish-azure-application-wizard.md).
