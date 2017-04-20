<properties
    pageTitle="Einen virtueller Computer mit einem Kennwort in Azure Stack Schlüssel Depot gespeichert bereitstellen | Microsoft Azure"
    description="Weitere Informationen zum Bereitstellen einer VM mit einem Kennwort in Azure Stack Schlüssel Depot gespeichert"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>Bereitstellen einer VM im Schlüsseltresor gespeicherte Kennwort abrufen

Wenn Sie während der Bereitstellung einen sicheren Wert (z. B. ein Kennwort) als Parameter übergeben möchten, können Wert in einen Tresor Azure Stapel Schlüssel geheim speichern und Referenzwert in anderen Azure-Ressourcen-Manager-Vorlagen. Sie einbeziehen lediglich einen Verweis auf den Schlüssel in Ihre Vorlage, damit der Schlüssel niemals offen gelegt. Sie müssen nicht den Wert für den Schlüssel jedes Mal manuell Ressourcen bereitstellen. Sie angeben, welche Benutzer oder Hauptbenutzer Service den geheimen Schlüssel zugreifen können.

## <a name="reference-a-secret-with-static-id"></a>Einen Schlüssel mit static ID verweisen

Sie referenzieren den Schlüssel innerhalb einer Parameterdatei, die Werte an die Vorlage übergeben. Den geheime Schlüssel wird den Ressourcenbezeichner der wichtigsten Depot und den Namen des geheimen Schlüssels verweisen. In diesem Beispiel muss geheime Schlüssel Depot vorhanden sein. Sie verwenden einen statischen Wert für die Ressourcen-ID.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]Der Parameter, der den Schlüssel akzeptiert sollte *Securestring*.

## <a name="next-steps"></a>Nächste Schritte
[Eine Beispiel-app mit Schlüssel bereitstellen](azure-stack-kv-sample-app.md)

[Bereitstellen einer VM mit einem Schlüssel Depot](azure-stack-kv-push-secret-into-vm.md)

