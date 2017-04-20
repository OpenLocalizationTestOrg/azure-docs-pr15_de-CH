Fortlaufende Entwicklung möglicherweise Android Studio installierte Android SDK-Version nicht die Version im Code überein. Android SDK verwiesen in diesem Lernprogramm ist Version 23 spätestens verfasst. Die Versionsnummer erhöht werden neue Versionen des SDK und empfiehlt die neueste verfügbare Version.

Versionskonflikt zwei Symptome sind:

1. Beim Erstellen oder das Projekt neu erstellen, erhalten Sie Gradle Fehlermeldungen wie "**Fehler beim Ziel Google dazu dass**".

2. Android Standardobjekte im Code sollte auf der Grundlage `import` Anweisungen möglicherweise Fehlermeldungen generiert.

Diese erscheint möglicherweise Version von Android SDK installiert Android Studio SDK Ziel heruntergeladenen Projekt nicht überein.  Überprüfen Sie die Version ändern:


1. Klicken Sie auf **Extras**, Android Studio => **Android** => **SDK Manager**. Die neueste Version der SDK-Plattform nicht installiert haben, klicken Sie auf zum Installieren. Notieren Sie sich die Versionsnummer.

2. Öffnen Sie die Datei auf der Registerkarte Projekt-Explorer unter **Gradle Skripts** **build.gradle (Modeule: app)**. Stellen Sie sicher, dass **CompileSdkVersion** und **BuildToolsVersion** die SDK-Version installiert werden. Tags können wie folgt aussehen:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. Android Studio Projekt-Explorer mit der rechten Maustaste des Projektknoten **Eigenschaften**und wählen Sie in der linken Spalte **Android**. Sicherstellen Sie, dass das **Projektziel erstellen** auf die entsprechende SDK-Version als **TargetSdkVersion**festgelegt ist.

4. Android Studio Manifestdatei entfällt Ziel SDK und SDK-Mindestversion Eclipse anders an.
