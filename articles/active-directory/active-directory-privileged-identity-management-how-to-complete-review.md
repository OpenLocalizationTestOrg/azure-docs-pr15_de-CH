<properties
   pageTitle="Eine Access-Prüfung durchführen | Microsoft Azure"
   description="Nach dem Starten einer Überprüfung Zugriff in Azure AD privilegierten Identitätsmanagement erfahren Sie abschließen und die Ergebnisse anzeigen"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Durchführen eine Überprüfung Zugriff in Azure AD privilegierten Identitätsmanagement


Privilegierte Rollenadministratoren können Systemzugriff einmal eine [Sicherheitsanalyse gestartet wurde,](active-directory-privileged-identity-management-how-to-start-security-review.md)überprüfen. Azure AD Berechtigungen Identitätsmanagement (PIM) senden automatisch fordert Benutzer den Zugriff überprüfen. Wenn ein Benutzer keine e-Mail erhalten, können Sie sie wie [Führen Sie eine](active-directory-privileged-identity-management-how-to-perform-security-review.md)die Anweisungen senden.

Sicherheit überprüfen abgelaufen ist oder alle Benutzer haben ihre selbst überprüfen, führen Sie die Schritte in diesem Artikel zu verwalten die Überprüfung der Ergebnisse.

## <a name="manage-security-reviews"></a>Sicherheit-Berichte verwalten

1. Zum [Azure-Portal](https://portal.azure.com/) und **Azure AD privilegierten Identitätsmanagement** Dashboard wählen.
2. Wählen Sie **Access überprüft** das Dashboard.
3. Wählen Sie Zugriff überprüfen, die Sie verwalten möchten.

Auf der Access-Überprüfung Details sind ein Optionen für die Verwaltung dieser Überprüfung.

![PIM Überprüfung Tasten - screenshot][1]

### <a name="remind"></a>Erinnern

Wenn eine Access-Überprüfung festgelegt ist, damit die Benutzer selbst überprüfen, sendet **erinnern** -Schaltfläche eine Benachrichtigung. 

### <a name="stop"></a>Beenden

Alle-Bewertungen haben ein Enddatum, jedoch können Sie die Schaltfläche **Beenden** vorzeitig beendet. Benutzer bis zu diesem Zeitpunkt noch nicht überprüft wurde, wird nicht sie können nach der Überprüfung beenden. Sie können keine Überprüfung neu starten, nachdem er beendet wurde.

### <a name="apply"></a>Anwenden

Nach Abschluss eine Überprüfung Access das Enddatum erreicht oder manuell beendet implementiert entweder die Schaltfläche **Übernehmen** das Ergebnis der Überprüfung. Bei der Überprüfung des Benutzers Zugriff verweigert wurde, ist der Schritt, der die Funktion Zuordnung entfernen.  

### <a name="export"></a>Exportieren

Wenn Sie die Ergebnisse der Sicherheitsanalyse manuell anwenden möchten, können Sie die Überprüfung exportieren. Die Schaltfläche **Exportieren** wird eine CSV-Datei downloaden. Verwalten Sie die Ergebnisse in Excel oder anderen Programmen, die CSV-Dateien öffnen.

### <a name="delete"></a>Löschen

Wenn Sie keine weitere Prüfung interessiert sind, löschen Sie sie. Die Schaltfläche **Löschen** entfernt die Überprüfung aus der PIM-Anwendung.

> [AZURE.IMPORTANT] Sie wird keine Warnung vor löschen, werden diese überprüfen möchten.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
