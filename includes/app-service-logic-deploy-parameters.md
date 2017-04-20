Mit Azure-Ressourcen-Manager definieren Sie Parameter für Werte zu geben, wenn die Vorlage bereitgestellt wird. Die Vorlage enthält einen Abschnitt Parameter, der alle Parameterwerte enthält.
Definieren Sie Parameter für die Werte, die variieren basierend auf das Projekt, das Sie bereitstellen oder die Umgebung, der Sie bereitstellen. Definieren Sie Parameter für die Werte, die immer gleich bleiben. Jeder Parameterwert wird in der Vorlage verwendet, definieren die Ressourcen bereitgestellt werden. 

Wenn Sie Parameter definieren, verwenden Sie das Feld **AllowedValues** Werte angegeben Benutzer während der Bereitstellung bieten kann. Verwenden Sie das Feld **DefaultValue** der Parameter einen Wert zuweisen, wird kein Wert, während der Bereitstellung angegeben ist.

Jeder Parameter in der Vorlage beschrieben wird.

### <a name="logicappname"></a>logicAppName

Der Name der Logik-app erstellen.

    "logicAppName": {
        "type": "string"
    }