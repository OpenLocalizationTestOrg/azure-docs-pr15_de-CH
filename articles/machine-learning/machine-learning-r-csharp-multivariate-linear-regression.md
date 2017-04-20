<properties 
    pageTitle="Lineare Regression multivariaten | Microsoft Azure" 
    description="Multivariate lineare Regression" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Multivariate lineare Regression   
 

 
Angenommen Sie, Sie haben ein Dataset und schnell eine abhängige Variable (y) vorhersagen möchten einzelnen (i) unabhängigen Variablen. Linearer Regression ist ein beliebtes statistische Verfahren für solche Vorhersagen verwendet. Hier wird die abhängige Variable y fortlaufenden Wert sein.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Ein einfaches Szenario könnte, der Forscher versucht das Gewicht einer Person (y) entsprechend ihrer Höhe (X) vorherzusagen. Ein erweitertes Szenario könnte, Forscher zusätzliche Informationen für die einzelnen (z. B. Gewicht, Geschlecht, Rasse) und versucht, das Gewicht der einzelnen vorherzusagen. Dieser [Webdienst]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) passt das lineare Regressionsmodell auf die Daten und gibt den vorhergesagten Wert (y) aller Messwerte in den Daten.

>Dieser Webdienst kann beispielsweise von Benutzer möglicherweise über eine mobile Anwendung auf einer Website oder auch auf einem lokalen Computer verwendet werden. Aber die Web Service dient auch als Beispiel wie Azure Machine Learning Webdienste auf R-Code verwendet werden kann. Mit nur wenigen Codezeilen R und klickt auf eine Schaltfläche in Azure Machine Learning Studio werden Experiment mit R erstellt und als Webdienst veröffentlicht. Der Webdienst kann Azure Marketplace veröffentlicht und Benutzer und Geräte weltweit ohne Installation Infrastruktur vom Autor des Webdienstes verwendet werden.  

##<a name="consumption-of-web-service"></a>Verbrauch des Webdienstes  
Dieser Webdienst gibt die vorhergesagten Werte der abhängigen Variablen anhand der unabhängigen Variablen für alle Bemerkungen. Der Webdienst erwartet die Endbenutzer Daten als Zeichenfolge eingeben, wobei Zeilen werden durch Kommas (,) getrennt und Spalten werden durch ein Semikolon (;) getrennt. Der Webdienst erwartet 1 Zeile jeweils und die erste Spalte die abhängige Variable erwartet. Ein Beispieldataset könnte wie folgt aussehen:

![Beispieldaten][1]

Messwerte ohne abhängige Variable sollte als "NV" y eingegeben werden. Die Daten für das obige Dataset die folgende Zeichenfolge wäre: "10 5 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1, NA; 3; 4". Die Ausgabe ist der vorhergesagte Wert für jede Zeile basierend auf der unabhängigen Variablen. 

>Dieser Dienst ist auf dem Markt Azure ein OData-Dienst; Diese können durch POST oder GET-Methoden aufgerufen werden. 

Es gibt verschiedene Arten der Nutzung des Diensts automatisch (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-Code für Webdiensten gestartet:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




##<a name="creation-of-web-service"></a>Erstellen des Webdiensts  
>Dieser Webdienst wurde mit Azure Machine Learning erstellt. Eine kostenlose Testversion sowie einführende Videos Experimente und [publishing-Webdienste](machine-learning-publish-a-machine-learning-web-service.md)finden Sie unter [azure.com/ml](http://azure.com/ml). Unten ist ein Screenshot des Versuchs, die Web Service und Beispielcode für jedes der Module innerhalb des Experiments erstellt.


Aus in Azure Machine Learning ein neues leeres Experiment erstellt wurde und zwei [R-Skript ausführen] [ execute-r-script] Module in den Arbeitsbereich gezogen wurden. Dieser wird einem Experiment Azure maschinelles Lernen mit einem zugrunde liegenden R-Skript. 2 Teile dieses Experiment: Schemadefinition Modell + bewerten. Das erste Modul definiert die erwartete Struktur des Eingabedatasets, wobei die erste Variable ist die abhängige Variable und die restlichen Variablen unabhängig sind. Das zweite Modul passt ein generische lineare Regressionsmodell für die Eingabedaten.  
  
![Experiment Fluss][3]

####<a name="module-1"></a>Modul 1:
 
####<a name="schema-definition"></a>Schemadefinition  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Modul 2:
####<a name="lm-modeling"></a>LM-Modellierung   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Grenzen
Dies ist ein sehr einfaches Beispiel eines Webdiensts mehrere lineare Regression. Wie aus den oben stehenden Beispielcode ersichtlich, wird keine Fehler abgefangen und der Dienst geht alles eine kontinuierliche Variable (keine kategorischen Funktionen zulässig), als der Dienst nur Eingaben numerische Werte zum Zeitpunkt der Erstellung dieser Webdienst. Außerdem behandelt der Dienst derzeit beschränkt Datengröße fällig Anforderung/Antwort Art des Web Service-Aufruf und die Tatsache, dass das Modell immer entspricht wird der Webdienst aufgerufen wird. 

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen auf der Webdienst oder Veröffentlichung Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
