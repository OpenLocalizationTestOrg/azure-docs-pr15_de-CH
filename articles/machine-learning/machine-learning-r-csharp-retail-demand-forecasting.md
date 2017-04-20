<properties 
    pageTitle="Planung - ETS + STL | Microsoft Azure" 
    description="Planung - ETS + STL" 
    services="machine-learning" 
    documentationCenter="" 
    authors="xueshanz" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="yijichen"/> 

#<a name="forecasting---ets--stl"></a>Planung - ETS + STL  

Dieser [Webdienst]( https://datamarket.azure.com/dataset/aml_labs/demand_forecast) implementiert saisonale Trend Zerlegung (STL) und exponentielle Glättung ETS Modelle anhand der vom Benutzer bereitgestellten Verlaufsdaten Vorhersagen zu erstellen. Steigt die Nachfrage für ein bestimmtes Produkt dieses Jahr? Kann ich meine Produktverkäufe für die Weihnachtszeit Vorhersagen, damit ich mein Inventar effektiv planen? Vorhersagemodelle sind dafür prädestiniert, solche Fragen. Angesichts der letzten Daten, prüfen diese Modelle versteckte Trends und saisonbedingten zukünftige Entwicklungen 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
 
>Dieser Webdienst kann beispielsweise von Benutzer möglicherweise über eine mobile Anwendung auf einer Website oder auch auf einem lokalen Computer verwendet werden. Aber die Web Service dient auch als Beispiel wie Azure Machine Learning Webdienste auf R-Code verwendet werden kann. Mit nur wenigen Codezeilen R und klickt auf eine Schaltfläche in Azure Machine Learning Studio werden Experiment mit R erstellt und als Webdienst veröffentlicht. Der Webdienst kann Azure Marketplace veröffentlicht und Benutzer und Geräte weltweit ohne Installation Infrastruktur vom Autor des Webdienstes verwendet werden.  
 
##<a name="consumption-of-web-service"></a>Verbrauch des Webdienstes 

Dieser Dienst 4 Argumente akzeptiert und die Prognosen berechnet.
Die Eingabeargumente sind:

* Häufigkeit – Gibt die Häufigkeit der Rohdaten (täglich/wöchentlich/monatliche/vierteljährliche/jährlich).
* Horizon - Planung Zukunft Zeitrahmen.
* Datum - Zeit in der neuen Serie Zeitdaten hinzufügen.
* Wert - die neue Zeit Serie Datenwerte hinzufügen.

Die Ausgabe des Dienstes ist die prognostizierten Werte.
 
Beispieleingabe möglich: 

* Häufigkeit – 12
* Horizon - 12
* Datum - 15/1/2012 2/15/2012 3/15/2012; 4/15/2012; 5/15/2012; 6/15/2012, 7. 15 2012; 8 / 15-2012 15/9/2012 15/10/2012; 15/11/2012; 12/15/2012; 15/1/2013 2/15/2013 15 3 2013; 4/15/2013; 5/15/2013; 6/15/2013; 7/15/2013; 8 / 15-2013 15/9/2013 15/10/2013; 15/11/2013; 12/15/2013. 1/15/2014 2/15/2014 15/3/2014; 4/15/2014; 5/15/2014; 6/15/2014; 15/7/2014; 8 / 15-2014; 15/9/2014
* Wert: 3.479; 3,68; 3.832; 3.941; 3.797; 3.586; 3.508; 3,731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3,65; 3.709; 3.682; 3.511; 3.429 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

>Dieser Dienst ist auf dem Markt Azure ein OData-Dienst; Diese können durch POST oder GET-Methoden aufgerufen werden. 

Es gibt verschiedene Arten der Nutzung des Diensts automatisch (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-Code für Webdiensten gestartet:

    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };         var json = JsonConvert.SerializeObject(input);
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

Aus in Azure Machine Learning wurde ein neues leeres Experiment erstellt. Eingabedaten Beispiel wurde mit einem vordefinierten Schema hochgeladen. Ein [R-Skript ausführen] , das Schema verknüpft ist[ execute-r-script] Modul generiert die STL und ETS Prognosemodelle mittels Stl, 'Ets' und 'Planung' Funktionen von r 

###<a name="experiment-flow"></a>Flow Experiment:

![Experiment Fluss][2]

####<a name="module-1"></a>Modul 1:
 
    # Add in the CSV file with the data in the format shown below 
![Beispieldaten][3]   

####<a name="module-2"></a>Modul 2:

    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- stl(train_ts,  s.window="periodic")
    train_model <- forecast(fit1, h = data$horizon, method = 'ets')
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

##<a name="limitations"></a>Grenzen 

Dies ist ein sehr einfaches Beispiel für ETS + STL Prognosen. Aus den oben stehenden Beispielcode ersichtlich ist, erfolgt keine Fehler abfangen und der Dienst geht davon aus, dass alle Variablen continuous-Positive Werte und die Häufigkeit eine ganze Zahl größer als 1 sollte. Vektoren Datum und Wert sollte identisch sein und die Länge der Zeitreihe sollte größer sein als 2 * Häufigkeit. Variable für das Datum sollte das Format ' tt/mm/jjjj' entsprechen.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen auf der Webdienst oder Veröffentlichung Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img1.png
[2]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img2.png
[3]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
