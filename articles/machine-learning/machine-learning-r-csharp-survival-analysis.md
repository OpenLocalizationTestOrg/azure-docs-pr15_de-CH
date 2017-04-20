<properties 
    pageTitle="Analyse mit Azure-Computer | Microsoft Azure" 
    description="Überleben Analyse Ereignis eintreten Wahrscheinlichkeit" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>Analyse 

In vielen Fällen ist das Ergebnis geprüft Zeit ein Ereignis von Interesse. In anderen Worten, die Frage "dieses Ereignisses?" gefragt. Beispielsweise sollten Sie Situationen, die die Daten, die verstrichene Zeit (Tage, Jahre, Kilometer usw. beschreiben) bis Ereignis (Krankheit Rückfall, Promotion empfangen Bremsbelag Fehler) auftritt. Jede Instanz Daten steht für ein bestimmtes Objekt (Patienten ein Student, ein Auto usw.).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Dieser [Webdienst]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) beantwortet die Frage "welche Wahrscheinlichkeit Ereignis Zeit n für Objekt X? treten" Durch die Bereitstellung eines Analysemodells überleben, ermöglicht dieser Webdienst Daten zu trainieren Sie das Modell testen. Das Thema des Experiments ist Modell die verstrichene Zeitdauer bis Ereignis auftritt. 

>Dieser Webdienst kann beispielsweise von Benutzer möglicherweise über eine mobile Anwendung auf einer Website oder auch auf einem lokalen Computer verwendet werden. Aber die Web Service dient auch als Beispiel wie Azure Machine Learning Webdienste auf R-Code verwendet werden kann. Mit nur wenigen Codezeilen R und klickt auf eine Schaltfläche in Azure Machine Learning Studio werden Experiment mit R erstellt und als Webdienst veröffentlicht. Der Webdienst kann Azure Marketplace veröffentlicht und Benutzer und Geräte weltweit ohne Installation Infrastruktur vom Autor des Webdienstes verwendet werden.  

##<a name="consumption-of-web-service"></a>Verbrauch des Webdienstes

Das Eingabedaten Schema des Webdienstes ist in der folgenden Tabelle dargestellt. Sechs Informationseinheiten als Eingabe benötigt: Trainingsdaten Testdaten, Zeit an den Index "Zeit" dimension, Index "Ereignis" Dimension und die Variablentypen (fortlaufende oder). Die Trainingsdaten werden mit einer Zeichenfolge dargestellt, die Zeilen werden durch Komma getrennt und die Spalten werden durch Semikolon getrennt. Die Anzahl der Daten ist flexibel. Alle Elemente in der Eingabezeichenfolge müssen numerisch sein. In den Trainingsdaten deutet die Dimension "Time" die Anzahl der Einheiten (Tage, Jahre, Kilometer usw.) verstrichen Ausgangspunkt der Studie (ein Patient Medikament Programme, Schüler ab PhD Studie, ein Auto zu werden usw.) bis Ereignis (der Patient nach Drogenkonsum Promotion Bremsbelag Störung Auto erhalten Schüler etc..) auftritt. Die Dimension "Ereignis" gibt an, ob das Ereignis am Ende der Studie. Der Wert "Event = 1" bedeutet Ereignisses Interesse am angegebenen Dimension "Time"; "Ereignis = 0" bedeutet, dass das Ereignis nicht von der Dimension "Time" angegebenen Zeit aufgetreten ist.

- Trainingdata - eine Zeichenfolge. Zeilen werden durch Komma getrennt und Spalten werden durch Semikolon getrennt. Jede Zeile enthält die Dimension "Time", "Ereignis" Dimension und unabhängigen Variablen.
- Testingdata - eine Datenzeile, die unabhängigen Variablen für ein bestimmtes Objekt enthält.
- Time_of_interest - die verstrichene Zeit Zinsen n.
- Index_time - Spaltenindex der Dimension "Time" (beginnend mit 1).
- Index_event - Spaltenindex der Dimension "Ereignis" (beginnend mit 1).
- Variable_types - Zeichenfolge mit Semikolons als Trennzeichen darin. 0 kontinuierliche Variablen und 1 Faktor Variablen darstellt.


Die Ausgabe ist die Wahrscheinlichkeit eines Ereignisses durch eine bestimmte Zeit. 

>Dieser Dienst ist auf dem Markt Azure ein OData-Dienst; Diese können durch POST oder GET-Methoden aufgerufen werden. 

Es gibt verschiedene Arten der Nutzung des Diensts automatisch (eine Beispiel-app ist [hier](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>C#-Code für Webdiensten gestartet:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




Die Interpretation dieser Prüfung lautet wie folgt. Angenommen das Ziel der Daten ist die Zeitdauer bis zur Rückgabe Drogenkonsum Patienten modellieren, eines der beiden Programme erhalten. Die Ausgabe des Web Service lautet: 35 Jahren Patienten mit früheren drug Behandlung 2 Mal unter Anwendung lange stationäre Behandlung und mit Heroin und kokain, die Wahrscheinlichkeit zu den Drogenkonsum % 95,64 Tag 500.

##<a name="creation-of-web-service"></a>Erstellen des Webdiensts

>Dieser Webdienst wurde mit Azure Machine Learning erstellt. Eine kostenlose Testversion sowie einführende Videos Experimente und [publishing-Webdienste](machine-learning-publish-a-machine-learning-web-service.md)finden Sie unter [azure.com/ml](http://azure.com/ml). Unten ist ein Screenshot des Versuchs, die Web Service und Beispielcode für jedes der Module innerhalb des Experiments erstellt.

Aus in Azure Machine Learning ein neues leeres Experiment erstellt wurde und zwei [R-Skript ausführen] [ execute-r-script] Module in den Arbeitsbereich gezogen wurden. Das Schema wurde mit einem einfachen [R-Skript ausführen][execute-r-script], definiert das Schema Eingabedaten für den Webdienst. Dieses Modul verknüpft ist dann das zweite [R-Skript ausführen] [ execute-r-script] Modul arbeiten größere. Dieses Modul macht Daten vorverarbeiten, Gebäude und Vorhersagen. Präprozessorlauf Daten im Schritt eingegebenen Daten als eine lange Zeichenfolge umgewandelt und in einem Datenrahmen konvertiert. Im Modell Gebäudes Schritt ist ein externes R-Paket "survival_2.37-7.zip" Erstinstallation für Analyse durchführen. Anschließend wird die Funktion "Coxph" nach einer Reihe Datenverarbeitung Aufgaben ausgeführt. Die Details der Funktion "Coxph" Analyse können der R-Dokumentation gelesen werden. Im Schritt Vorhersage testen Instanz ausgebildeten Modell mit der Funktion "Surfit" angegeben und Überleben Kurve für diese Tests Instanz als "Kurven" Variable erstellt wird. Schließlich wird die Wahrscheinlichkeit von dem Zeitpunkt abgerufen. 

###<a name="experiment-flow"></a>Flow Experiment:

![Experimentieren Sie Fluss][1]

####<a name="module-1"></a>Modul 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Modul 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Grenzen

Dieser Webdienst kann nur numerische Werte als Funktion Variablen (Spalten) enthalten. Die Spalte "Ereignis" kann nur Wert 0 oder 1 annehmen. Die Spalte "Zeit" muss eine positive ganze Zahl sein.

##<a name="faq"></a>Häufig gestellte Fragen
Häufig gestellte Fragen auf der Webdienst oder Veröffentlichung Azure Marketplace finden Sie [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
