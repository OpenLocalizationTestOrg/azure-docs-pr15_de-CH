**Objective-C**: 

1. Öffnen Sie auf Ihrem Mac _QSTodoListViewController.m_ in Xcode, und fügen Sie die folgende Methode hinzu. Ändern Sie _Google_ _Microsoftaccount_ _twitter_, _Facebook_und _Windowsazureactivedirectory_ , wenn Sie Google nicht als der Identitätsanbieter verwenden. Wenn Sie Facebook, [Sie müssen weiße Facebook Domänen in Ihrer Anwendung](https://developers.facebook.com/docs/ios/ios9#whitelist)verwenden.

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. Ersetzen Sie `[self refresh]` in `viewDidLoad` in _QSTodoListViewController.m_ mit den folgenden:

            [self loginAndGetData];

3. Drücken Sie die Anwendung starten und dann _Ausführen_ . Wenn Sie angemeldet sind, sollten Sie möglicherweise die Aufgabenliste anzeigen und aktualisieren.

**Swift**:

1. Öffnen Sie auf Ihrem Mac _ToDoTableViewController.swift_ in Xcode, und fügen Sie die folgende Methode hinzu. Ändern Sie _Google_ _Microsoftaccount_ _twitter_, _Facebook_und _Windowsazureactivedirectory_ , wenn Sie Google nicht als der Identitätsanbieter verwenden. Wenn Sie Facebook, [Sie müssen weiße Facebook Domänen in Ihrer Anwendung](https://developers.facebook.com/docs/ios/ios9#whitelist)verwenden.
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. Entfernen Sie die Zeilen `self.refreshControl?.beginRefreshing()` und `self.onRefresh(self.refreshControl)` am Ende des `viewDidLoad()` in _ToDoTableViewController.swift_. Fügen Sie einen Aufruf an `loginAndGetData()` an ihrer Stelle:

            loginAndGetData()

3. Drücken Sie die Anwendung starten und dann _Ausführen_ . Wenn Sie angemeldet sind, sollten Sie möglicherweise die Aufgabenliste anzeigen und aktualisieren.
