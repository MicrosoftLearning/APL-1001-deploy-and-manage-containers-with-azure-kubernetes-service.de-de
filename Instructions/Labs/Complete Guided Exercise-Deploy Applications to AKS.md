---
Guided Exercise:
  title: 'Geführte Übung: Bereitstellen von Anwendungen in AKS'
---
# Geführte Übung: Bereitstellen von Anwendungen in Azure Kubernetes Service (AKS)

## Ziele

Diese geführte Übung besteht aus den folgenden Aktivitäten:

+ Übung 1: Bereitstellen von Azure Container Registry (ACR) und Azure Kubernetes Service (AKS)
+ Übung 2: Erstellen von Linux- und Windows-Containerimages und Speichern in ACR
+ Übung 3: Bereitstellen von Containerimages in AKS 
+ Übung 4: Überprüfen der Bereitstellung und Aufheben der Bereitstellung aller Ressourcen

## Übung 1: Bereitstellen von Azure Container Registry (ACR) und Azure Kubernetes Service (AKS)
In dieser Übung erstellen Sie eine Azure Container Registry-Instanz und einen AKS-Cluster.


>**Hinweis**: Für diese Übung benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free/).
> Verwenden Sie für alle Eigenschaften, die nicht angegeben sind, den Standardwert.


### Aufgabe 1: Erstellen einer Azure Container Registry-Instanz
In dieser Aufgabe erstellen Sie eine Azure Container Registry-Instanz.

1. Öffnen Sie von Ihrem Computer aus ein Webbrowserfenster, und navigieren Sie zum Azure-Portal unter https://portal.azure.com.
1. Melden Sie sich bei der entsprechenden Aufforderung mit einem Benutzerkonto an, das über die Rolle „Besitzer“ in dem Azure-Abonnement verfügt, das Sie für diese Übung verwenden werden. 
1. Melden Sie sich beim Azure-Portal an. 
1. Suchen Sie im Azure-Portal im Textfeld **Suchen** nach **Containerregistrierungen**, und wählen Sie diese Option aus.
1. Wählen Sie auf der Seite **Containerregistrierungen** die Option **+ Erstellen** aus, und geben Sie die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Abonnement|Der Name des Azure-Abonnements, das Sie in dieser Übung verwenden|
    |Ressourcengruppe|Der Name einer neuen Ressourcengruppe: **acr-01-RG**|
    |Registrierungsname|Einen gültigen, global eindeutigen Namen, der aus 5 bis 50 alphanumerischen Zeichen besteht|
    |Region|Eine beliebige Azure-Region, in der Sie eine Azure Container Registry-Instanz und einen AKS-Cluster erstellen können|
    |Verfügbarkeitszonen|**Keine**|
    |SKU|**Grundlegend**|

1. Wählen Sie auf der Seite **Containerregistrierungen** die Option **Überprüfen + erstellen** und auf der Registerkarte **Überprüfen + erstellen** die Option **Erstellen** aus.

   > **Hinweis:** Fahren Sie mit der nächsten Übung fort, ohne auf den Abschluss der Bereitstellung der Azure Container Registry-Instanz zu warten.

### Aufgabe 2: Erstellen eines virtuellen Azure-Netzwerks und eines AKS-Clusters
In dieser Aufgabe erstellen Sie ein virtuelles Azure-Netzwerk und stellen einen AKS-Cluster einschließlich eines Windows-Knotenpools in diesem bereit.

> **Hinweis:** Sie könnten zwar beim Bereitstellen eines AKS-Clusters ein virtuelles Netzwerk erstellen, das ist jedoch nicht üblich. Noch wichtiger ist, dass bei der Bereitstellung von AKS-Clustern in einem vorhandenen virtuellen Netzwerk einige zusätzliche Anforderungen gelten, mit denen Sie vertraut sein sollten.

1. Suchen Sie im Azure-Portal im Textfeld **Suchen** nach **Virtuelle Netzwerke**, und wählen Sie diese Option aus.
1. Wählen Sie auf der Seite **Virtuelle Netzwerke** die Option **+ Erstellen** aus, und geben Sie dann auf der Registerkarte **Grundeinstellungen** der Seite **Virtuelles Netzwerk erstellen** die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Abonnement|Der Name des Azure-Abonnements, das Sie in der ersten Übung ausgewählt haben|
    |Ressourcengruppe|Der Name einer neuen Ressourcengruppe: **aks-01-RG**|
    |Name des virtuellen Netzwerks|**vnet-01**|
    |Region|Dieselbe Azure-Region, die Sie in der ersten Übung ausgewählt haben|

1. Wählen Sie auf der Registerkarte **Grundeinstellungen** der Seite **Virtuelles Netzwerk erstellen** die Option **Weiter** aus.
1. Akzeptieren Sie auf der Registerkarte **Sicherheit** der Seite **Virtuelles Netzwerk erstellen** die Standardeinstellungen, und wählen Sie **Weiter** aus.
1. Stellen Sie auf der Registerkarte **IP-Adressen** der Seite **Virtuelles Netzwerk erstellen** sicher, dass der IP-Adressraum auf **10.0.0.0/16** festgelegt ist, löschen Sie das **Standardsubnetz**, wählen Sie **Überprüfen + erstellen** aus, und wählen Sie dann auf der Registerkarte **Überprüfen + erstellen** die Option **Erstellen** aus.

   > **Hinweis:** Das Erstellen eines virtuellen Netzwerks sollte nur wenige Sekunden dauern, sodass Sie direkt mit dem nächsten Schritt fortfahren können.

1. Suchen Sie im Azure-Portal im Textfeld **Suchen** nach **Kubernetes-Dienste**, und wählen Sie diese Option aus.
1. Wählen Sie auf der Seite **Kubernetes-Dienste** die Option **+ Erstellen** aus, wählen Sie in der Dropdownliste die Option **Kubernetes-Cluster erstellen** aus, und geben Sie dann auf der Registerkarte **Grundeinstellungen** der Seite **Kubernetes-Cluster erstellen** die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Abonnement|Der Name des Azure-Abonnements, das Sie in der ersten Übung ausgewählt haben|
    |Ressourcengruppe|**aks-01-RG**|
    |Voreingestellte Clusterkonfiguration|**Dev/Test**|
    |Kubernetes-Clustername|**aks-01**|
    |Region|Dieselbe Azure-Region, die Sie in der ersten Übung ausgewählt haben|
    |Verfügbarkeitszonen|**Keine**|
    |AKS-Tarif|**Free**|
    |Kubernetes-Version|Akzeptieren Sie den Standardwert.|
    |automatische Upgrade|Deaktiviert|
    |Knotengröße|**Standard B4ms**|
    |Skalierungsmethode|**Manuell**|
    |Anzahl der Knoten|**2**|

   > **Hinweis:** Möglicherweise müssen Sie die vCPU-Kontingente erhöhen oder die VM-SKU ändern, um die Werte für Knotengröße und Knotenanzahl anzupassen. Informationen zum Erhöhen von vCPU-Kontingenten finden Sie im Microsoft Learn-Artikel [Erhöhen von vCPU-Kontingenten für die VM-Familie](https://learn.microsoft.com/en-us/azure/quotas/per-vm-quota-requests).

1. Wählen Sie auf der Registerkarte **Grundeinstellungen** der Seite **Kubernetes-Cluster erstellen** die Option **Weiter: Knotenpools >** aus.
1. Wählen Sie auf der Registerkarte **Knotenpools** der Seite **Kubernetes-Cluster erstellen** die Option **Weiter: Zugriff >** aus.
1. Wählen Sie auf der Registerkarte **Zugriff** der Seite **Kubernetes-Cluster erstellen** die Option **Weiter: Netzwerke >** aus.
1. Wählen Sie auf der Registerkarte **Netzwerke** der Seite **Kubernetes-Cluster erstellen** die Option **Azure CNI** aus, wählen Sie in der Dropdownliste **Virtuelles Netzwerk** den Eintrag **vnet-01** und unterhalb des Textfelds **Clustersubnetz** die Option **Verwaltete Subnetzkonfiguration** aus.
1. Wählen Sie auf der Seite **vnet-01 \| Subnetze** die Option **+ Subnetz** aus.
1. Geben Sie auf der Seite **Subnetze hinzufügen** die folgenden Einstellungen an, und wählen Sie **Speichern** aus:

    |Einstellung|Wert|
    |---|---|
    |Name|**aks-subnet**|
    |Subnetzadressbereich|**10.0.0.0/20**|

1. Wählen Sie auf der Seite **vnet-01 \| Subnetze** in der Breadcrumbnavigation oben links auf der Seite **Kubernetes-Cluster erstellen** aus. 
1. Geben Sie auf der Registerkarte **Netzwerke** der Seite **Kubernetes-Cluster erstellen** die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Clustersubnetz|**aks-subnet (10.0.0.0/20)**|
    |Kubernetes-Dienstadressbereich|**172.16.0.0/22**|
    |Kubernetes-DNS-Dienst – IP-Adresse|**172.16.3.254**|
    |DNS-Namenspräfix|**aks-01-dns**|
    |Privaten Cluster aktivieren|Deaktiviert|
    |Autorisierte IP-Adressbereiche festlegen|Deaktiviert|
    |Netzwerkrichtlinie|**Keine**|

1. Wählen Sie auf der Registerkarte **Netzwerke** der Seite **Kubernetes-Cluster erstellen** die Registerkarte **Knotenpools** aus.

   > **Hinweis:** Sie fügen dem Cluster einen Windows-Knotenpool hinzu. Hierfür muss die Netzwerkkonfiguration von **Kubenet** (Standardeinstellung) in **Azure CNI** geändert werden. Die Kubenet-Netzwerkkonfiguration unterstützt keine Windows-Knotenpools.

1. Wählen Sie auf der Registerkarte **Knotenpools** der Seite **Kubernetes-Cluster erstellen** die Option **+ Knotenpool hinzufügen** aus.
1. Geben Sie auf der Seite **Knotenpool hinzufügen** die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Knotenpoolname|**w1pool**|
    |Mode|**Benutzer**|
    |Betriebssystemtyp|**Windows**|
    |Verfügbarkeitszone|**Keine**|
    |Azure Spot-Instanzen aktivieren|Deaktiviert|
    |Knotengröße|**Standard B4s_v2**|
    |Skalierungsmethode|**Manuell**|
    |Anzahl der Knoten|**2**|
    |Max. Pods pro Knoten|**30**|
    |Öffentliche IP-Adresse pro Knoten aktivieren|Deaktiviert|

   > **Hinweis:** Möglicherweise müssen Sie die vCPU-Kontingente erhöhen oder die VM-SKU ändern, um die Werte für Knotengröße und Knotenanzahl anzupassen. Informationen zum Erhöhen von vCPU-Kontingenten finden Sie im Microsoft Learn-Artikel [Erhöhen von vCPU-Kontingenten für die VM-Familie](https://learn.microsoft.com/en-us/azure/quotas/per-vm-quota-requests).

1. Wählen Sie auf der Seite **Knotenpool hinzufügen** die Option **Hinzufügen** aus.
1. Wählen Sie auf der Registerkarte **Knotenpools** der Seite **Kubernetes-Cluster erstellen** die Registerkarte **Integrationen** aus.
1. Wählen Sie auf der Registerkarte **Integration** der Seite **Kubernetes-Cluster erstellen** in der Dropdownliste **Containerregistrierung** den Eintrag aus, der die Azure Container Registry-Instanz darstellt, die Sie in der vorherigen Übung erstellt haben, deaktivieren Sie das Kontrollkästchen **Empfohlene Warnungsregeln aktivieren**, stellen Sie sicher, dass die Option **Azure Policy** deaktiviert ist, und wählen Sie **Überprüfen + erstellen** aus.
1. Wählen Sie auf der Registerkarte **Überprüfen + erstellen** der Seite **Kubernetes-Cluster erstellen** die Option **Erstellen** aus.

   > **Hinweis:** Fahren Sie mit der nächsten Übung fort, ohne zu warten, bis die Bereitstellung des AKS-Clusters abgeschlossen ist. Die Bereitstellung dauert etwa fünf Minuten.

## Übung 2: Erstellen von Linux- und Windows-Containerimages und Speichern in ACR
In dieser Übung erstellen Sie Linux- und Windows-basierte Docker-Images und pushen sie in die Azure Container Registry-Instanz, die Sie zuvor in dieser Übung erstellt haben.

### Aufgabe 1: Erstellen eines Linux-Containerimages und Speichern in ACR
In dieser Aufgabe verwenden Sie eine ACR-Aufgabe, um ein Linux-Containerimage zu erstellen und es automatisch in ACR zu pushen.

1. Wählen Sie im Azure-Portal das Symbol **Cloud Shell** aus.
1. Wählen Sie bei Aufforderung zur Auswahl von **Bash** oder **PowerShell** die Option **Bash** aus. 
1. Wenn Sie dazu aufgefordert werden, wählen Sie **Speicher erstellen** aus, und warten Sie, bis der Azure Cloud Shell-Bereich angezeigt wird. 
1. Stellen Sie sicher, dass **Bash** im Dropdownmenü oben links im Cloud Shell-Bereich angezeigt wird.
1. Erstellen Sie in der Bash-Sitzung in Cloud Shell ein Verzeichnis, das die Dockerfile-Datei für das Linux-Image hostet, und wechseln Sie aus dem aktuellen Verzeichnis dahin, indem Sie die folgenden Befehle ausführen:

   ```bash
   mkdir ~/image-l01
   cd ~/image-l01
   ```

1. Verwenden Sie in der Bash-Sitzung von Azure Cloud Shell den integrierten Editor, um eine Datei mit dem Namen „server.js“ im Verzeichnis „image-l01“ zu erstellen und den folgenden Inhalt dahin zu kopieren:

   ```js
   const http = require('http')
   const port = 80
   const server = http.createServer((request, response) => {
     response.writeHead(200, {'Content-Type': 'text/plain'})
     response.write('Hello World from Node\n')
     response.end('Version: ' + process.env.NODE_VERSION + '\n')
   })
   server.listen(port)
   console.log(`Server running at http://localhost: ${port}`)
   ```

   > **Hinweis:** Nach der Ausführung zeigt der resultierende Node.js-Code die Meldung **Hello World from Node** an.

1. Verwenden Sie in der Bash-Sitzung von Azure Cloud Shell den integrierten Editor erneut, um eine Datei mit dem Namen „package.json“ im Verzeichnis „image-l01“ zu erstellen und den folgenden Inhalt dahin zu kopieren:

   ```json
   {
     "name": "helloworld",
     "version": "1.0.0",
     "description": "Sample app for ACR Build",
     "main": "server.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "start": "node server.js"
     },
     "license": "MIT"
   }
   ```

1. Speichern Sie die Änderungen an der Datei, und schließen Sie sie, um zur Bash-Eingabeaufforderung zurückzukehren.
1. Verwenden Sie in der Bash-Sitzung von Azure Cloud Shell den integrierten Editor erneut, um eine Datei mit dem Namen „Dockerfile“ im Verzeichnis „image-l01“ zu erstellen und den folgenden Inhalt dahin zu kopieren:

   ```Dockerfile
   FROM node:20.2-alpine
   COPY . /src
   RUN cd /src && npm install
   EXPOSE 80
   CMD ["node", "/src/server.js"]
   ```

1. Speichern Sie die Änderungen an der Datei, und schließen Sie sie, um zur Bash-Eingabeaufforderung zurückzukehren.
1. Identifizieren Sie in der Bash-Sitzung von Azure Cloud Shell den Namen Ihrer Azure Container Registry, die Sie zuvor in dieser Übung erstellt haben, und speichern Sie ihn in einer Variablen namens **$ACRNAME**, indem Sie die folgenden Befehle ausführen:

   ```azurecli
   ACR_RGNAME='acr-01-RG'
   ACR_NAME=$(az acr list --resource-group $ACR_RGNAME --query "[].name" --output tsv)
   ```

1. Erstellen Sie in der Bash-Sitzung von Azure Cloud Shell ein Docker-Image, das auf der im aktuellen Verzeichnis gespeicherten Dockerfile-Datei basiert, und pushen Sie es automatisch an die Azure Container Registry, deren Name in der Variablen **$ACRNAME** gespeichert ist, indem Sie den folgenden Befehl ausführen (stellen Sie sicher, dass Sie den nachgestellten Punkt einschließen):

   ```azurecli
   az acr build --registry $ACR_NAME --image hellofromnode:v1.0 .
   ```

   > **Hinweis:** Verfolgen Sie den Fortschritt der Erstellung, und stellen Sie sicher, dass sie erfolgreich abgeschlossen wird. Das sollte weniger als eine Minute dauern.

### Aufgabe 2: Erstellen eines Windows-Containerimages und Speichern in ACR
In dieser Aufgabe verwenden Sie eine ACR-Aufgabe, um ein Windows-Containerimage zu erstellen und es automatisch in ACR zu pushen.

1. Erstellen Sie in der Bash-Sitzung in Cloud Shell ein Verzeichnis, das die Dockerfile-Datei für das Windows-Image hostet, und wechseln Sie aus dem aktuellen Verzeichnis dahin, indem Sie die folgenden Befehle ausführen:

   ```bash
   mkdir ~/image-w01
   cd ~/image-w01
   ```

1. Klonen Sie in der Bash-Sitzung von Azure Cloud Shell ein öffentliches GitHub-Repository, das die Dateien hostet, die Sie zum Erstellen des Windows-Images verwenden, und wechseln Sie das aktuelle Verzeichnis in das geklonte Repository, indem Sie die folgenden Befehle ausführen:

   ```git
   git clone https://github.com/Azure-Samples/dotnetcore-docs-hello-world.git
   cd ~/image-w01/dotnetcore-docs-hello-world
   ```

   > **Hinweis:** Das Repository enthält den Quellcode für eine .NET 7-Web-App, die die Meldung **Hello World from .NET 7** anzeigt.

1. Erstellen Sie in der Bash-Sitzung von Azure Cloud Shell ein Docker-Image, das auf der im aktuellen Verzeichnis gespeicherten Dockerfile-Datei basiert, und pushen Sie es automatisch an die Azure Container Registry, deren Name in der Variablen **$ACRNAME** gespeichert ist, indem Sie den folgenden Befehl ausführen (stellen Sie sicher, dass Sie den nachgestellten Punkt einschließen):

   ```azurecli
   az acr build --registry $ACR_NAME --image hellofromdotnet:v1.0 --platform windows --file Dockerfile.windows .
   ```

   > **Hinweis:** Der Name der Dockerfile-Datei, die den Windows-Imagebuild definiert, lautet **Dockerfile.windows**.

   > **Hinweis:** Verfolgen Sie den Fortschritt der Erstellung, und stellen Sie sicher, dass sie erfolgreich abgeschlossen wird. Das sollte weniger als 3 Minuten dauern.

1. Schließen Sie den Azure Cloud Shell-Bereich.
1. Navigieren Sie im Azure-Portal zur Seite **Containerregistrierungen**, und wählen Sie den Eintrag aus, der die Containerregistrierung darstellt, in die Sie beide Images gepusht haben.
1. Wählen Sie auf der Seite „Containerregistrierung“ im vertikalen Hubmenü **Repositorys** aus, und vergewissern Sie sich, dass **hellofromnode** und **hellofromdotnet** in der Liste der Repositorys angezeigt werden.

## Übung 3: Bereitstellen von Containerimages in AKS 
In dieser Übung stellen Sie zwei Containerimages, die Sie zuvor in dieser Übung erstellt haben, im AKS-Cluster bereit.

> **Hinweis:** Bevor Sie mit dieser Übung fortfahren, vergewissern Sie sich, dass die Bereitstellung des AKS-Clusters erfolgreich abgeschlossen wurde.

### Aufgabe 1: Erstellen benutzerdefinierter AKS-Namespaces
In dieser Aufgabe erstellen Sie zwei Namespaces in dem AKS-Cluster, den Sie zuvor in dieser Übung erstellt haben.

1. Suchen Sie im Azure-Portal im Textfeld **Suchen** nach **Kubernetes-Dienste**, und wählen Sie diese Option aus.
1. Wählen Sie auf der Seite **Kubernetes-Dienste** die Option **aks-01** aus.
1. Wählen Sie auf der Seite **aks-01** im vertikalen Hubmenü die Option **Namespaces** aus.
1. Wählen Sie auf der Seite **aks-01 \| Namespaces** die Option **+ Erstellen** aus, und wählen Sie im Dropdownmenü **Namespace** aus.
1. Geben Sie im Bereich **Namespace erstellen** im Textfeld **Name** den Namen **dev-node** ein, und wählen Sie **Erstellen** aus.
1. Wählen Sie auf der Seite **aks-01 \| Namespaces** die Option **+ Erstellen** aus, und wählen Sie im Dropdownmenü **Namespace** aus.
1. Geben Sie im Bereich **Namespace erstellen** im Textfeld **Name** den Namen **dev-dotnet** ein, und wählen Sie **Erstellen** aus.

### Aufgabe 2: Erstellen eines Kubernetes-Manifests zum Bereitstellen des Linux-Images
In dieser Aufgabe erstellen Sie ein Kubernetes-Manifest zum Bereitstellen des Linux-Images im Linux-Knotenpool.

1. Wählen Sie im Azure-Portal das Symbol **Cloud Shell** aus.
1. Stellen Sie sicher, dass **Bash** im Dropdownmenü oben links im Cloud Shell-Bereich angezeigt wird.
1. Erstellen Sie in der Bash-Sitzung in Azure Cloud Shell ein Verzeichnis, das das Bereitstellungsmanifest für die Bereitstellungspods hostet, die auf dem Linux-Image basieren, und ändern Sie das aktuelle Verzeichnis in dieses, indem Sie die folgenden Befehle ausführen:

   ```bash
   mkdir ~/aks-l01
   cd ~/aks-l01
   ```

1. Verwenden Sie in der Bash-Sitzung von Azure Cloud Shell den integrierten Editor, um eine Datei mit dem Namen „aks-deployment-l01.yaml“ im Verzeichnis „aks-l01“ zu erstellen und den folgenden Inhalt in diese zu kopieren:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: hellofromnode-deployment
     labels:
       environment: dev
       app: hellofromnode
   spec:
     replicas: 1
     template:
       metadata:
         name: hellofromnode
         labels:
           app: hellofromnode
       spec:
         nodeSelector:
           "kubernetes.io/os": linux
         containers:
         - name: hellofromnode
           image: ACR_NAME.azurecr.io/hellofromnode:v1.0
           resources:
             limits:
               cpu: 1
               memory: 800M
           ports:
             - containerPort: 80
     selector:
       matchLabels:
         app: hellofromnode
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: hellofromnode-service
   spec:
     type: LoadBalancer
     ports:
     - protocol: TCP
       port: 80
     selector:
       app: hellofromnode
   ```

   > **Hinweis:** Die Bereitstellung erstellt Pods basierend auf dem Linux-Containerimage für den Linux-Knotenpool im AKS-Cluster. Darüber hinaus enthält das Manifest einen Dienst, der über eine öffentliche IP-Adresse an Port 80 einen Lastenausgleichszugriff auf die Pods in der Bereitstellung bereitstellt.

1. Speichern Sie die Änderungen an der Datei, und schließen Sie sie, um zur Bash-Eingabeaufforderung zurückzukehren.
1. Ersetzen Sie in der Bash-Sitzung von Azure Cloud Shell den Platzhalter **ACR_NAME** in der Datei „aks-deployment-l01.yaml“, indem Sie die folgenden Befehle ausführen:

   ```azurecli
   ACR_RGNAME='acr-01-RG'
   ACR_NAME=$(az acr list --resource-group $ACR_RGNAME --query "[].name" --output tsv)
   sed -i "s/ACR_NAME/$ACR_NAME/" ./aks-deployment-l01.yaml
   ```

### Aufgabe 3: Erstellen eines Kubernetes-Manifests zum Bereitstellen des Windows-Images
In dieser Aufgabe erstellen Sie ein Kubernetes-Manifest zum Bereitstellen des Windows-Images im Windows-Knotenpool.

1. Erstellen Sie in der Bash-Sitzung in Azure Cloud Shell ein Verzeichnis, das das Bereitstellungsmanifest für die Bereitstellungspods hostet, die auf dem Windows-Image basieren, und ändern Sie das aktuelle Verzeichnis in dieses, indem Sie die folgenden Befehle ausführen:

   ```bash
   mkdir ~/aks-w01
   cd ~/aks-w01
   ```

1. Verwenden Sie in der Bash-Sitzung von Azure Cloud Shell den integrierten Editor, um eine Datei mit dem Namen „aks-deployment-w01.yaml“ im Verzeichnis „aks-w01“ zu erstellen und den folgenden Inhalt in diese zu kopieren:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: hellofromdotnet-deployment
     labels:
       environment: dev
       app: hellofromdotnet
   spec:
     replicas: 1
     template:
       metadata:
         name: hellofromdotnet
         labels:
           app: hellofromdotnet
       spec:
         nodeSelector:
           "kubernetes.io/os": windows
         containers:
         - name: hellofromdotnet
           image: ACR_NAME.azurecr.io/hellofromdotnet:v1.0
           resources:
             limits:
               cpu: 1
               memory: 800M
           ports:
             - containerPort: 80
     selector:
       matchLabels:
         app: hellofromdotnet
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: hellofromdotnet-service
   spec:
     type: LoadBalancer
     ports:
     - protocol: TCP
       port: 80
     selector:
       app: hellofromdotnet
   ```

   > **Hinweis:** Die Bereitstellung erstellt Pods basierend auf dem Windows-Containerimage für den Windows-Knotenpool im AKS-Cluster. Darüber hinaus enthält das Manifest einen Dienst, der über eine öffentliche IP-Adresse an Port 80 einen Lastenausgleichszugriff auf die Pods in der Bereitstellung bereitstellt.

1. Speichern Sie die Änderungen an der Datei, und schließen Sie sie, um zur Bash-Eingabeaufforderung zurückzukehren.
1. Ersetzen Sie in der Bash-Sitzung von Azure Cloud Shell den Platzhalter **ACR_NAME** in der Datei „aks-deployment-l01.yaml“, indem Sie den folgenden Befehl ausführen:

   ```azurecli
   sed -i "s/ACR_NAME/$ACR_NAME/" ./aks-deployment-w01.yaml
   ```

### Aufgabe 4: Ausführen von AKS-Bereitstellungen mithilfe von YAML-Manifestdateien
In dieser Aufgabe stellen Sie beide Containerimages in den jeweiligen Namespaces und Knotenpools im AKS-Zielcluster bereit.

1. Stellen Sie in der Bash-Sitzung von Azure Cloud Shell eine Verbindung mit dem AKS-Cluster her, indem Sie die folgenden Befehle ausführen:

   ```azurecli
   AKSRG='aks-01-RG'
   AKSNAME='aks-01'
   az aks get-credentials --resource-group $AKSRG --name $AKSNAME
   ```

1. Überprüfen Sie in der Bash-Sitzung von Azure Cloud Shell, ob die Verbindung erfolgreich war, indem Sie den folgenden Befehl ausführen:

   ```kubectl
   kubectl get nodes
   ```

   > **Hinweis:** Die Ausgabe des Befehls sollte eine Auflistung aller AKS-Knoten (in diesem Fall vier) enthalten.

1. Erstellen Sie in der Bash-Sitzung von Azure Cloud Shell die erste Bereitstellung, die in der entsprechenden YAML-Manifestdatei im Namespace **dev-node** definiert ist, indem Sie die folgenden Befehle ausführen:

   ```kubectl
   cd ~/aks-l01
   kubectl apply -f aks-deployment-l01.yaml -n=dev-node
   ```

   > **Hinweis:** Fahren Sie mit dem nächsten Schritt fort, ohne auf den Abschluss der Bereitstellung zu warten. Die Bereitstellung aller Ressourcen kann einige Minuten dauern.

1. Erstellen Sie in der Bash-Sitzung von Azure Cloud Shell die zweite Bereitstellung, die in der entsprechenden YAML-Manifestdatei definiert ist, indem Sie den folgenden Befehl ausführen:

   ```kubectl
   cd ~/aks-w01
   kubectl apply -f aks-deployment-w01.yaml -n=dev-dotnet
   ```

   > **Hinweis:** Fahren Sie mit dem nächsten Schritt fort, ohne auf den Abschluss der Bereitstellung zu warten. Die Bereitstellung aller Ressourcen kann einige Minuten dauern.

# Übung 4: Überprüfen der Bereitstellung und Aufheben der Bereitstellung aller Ressourcen
In dieser Übung überprüfen Sie die Ergebnisse der Bereitstellungen und heben die Bereitstellung aller Ressourcen auf.

### Aufgabe 1: Überprüfen der AKS-Bereitstellungen und -Dienste
In dieser Aufgabe überprüfen Sie die Ergebnisse beider Bereitstellungen, einschließlich der Bereitstellungen und Dienstobjekte.

1. Zeigen Sie in der Bash-Sitzung von Azure Cloud Shell den Status beider Bereitstellungen an, indem Sie die folgenden Befehle ausführen:

   ```kubectl
   kubectl get deployments -n=dev-node
   kubectl get deployments -n=dev-dotnet
   ```

   > **Hinweis:** Bevor Sie mit dem nächsten Schritt fortfahren, überprüfen Sie, ob beide Bereitstellungen den Status „ready“ aufweisen. Warten Sie andernfalls eine weitere Minute, führen Sie die beiden oben genannten Befehle erneut aus, und überprüfen Sie den Bereitstellungsstatus erneut.

1. Zeigen Sie in der Bash-Sitzung von Azure Cloud Shell den Status der beiden Dienste an, die in den Manifestdateien enthalten waren, indem Sie die folgenden Befehle ausführen:

   ```kubectl
   kubectl get services -n=dev-node
   kubectl get services -n=dev-dotnet
   ```

1. Vergewissern Sie sich, dass die Auflistung für jeden Dienst einen Wert in der Spalte **EXTERNAL-IP** enthält. 
1. Verwenden Sie einen Webbrowser, um zu den IP-Adressen zu navigieren, die Sie im vorherigen Schritt ermittelt haben, und überprüfen Sie, ob auf den resultierenden Webseiten die Nachrichten **Hello World from Node** und **Hello World from .Net 7** angezeigt werden.

### Aufgabe 2: Löschen aller Ressourcen
In dieser Aufgabe löschen Sie alle Ressourcen, die in dieser Übung bereitgestellt wurden.

1. Zeigen Sie in der Bash-Sitzung von Azure Cloud Shell die Auflistung der Ressourcen in den beiden Ressourcengruppen an, die in dieser Übung bereitgestellt wurden, indem Sie die folgenden Befehle ausführen:

   ```azurecli
   az resource list --resource-group 'acr-01-RG' --query "[].name" --output tsv
   az resource list --resource-group 'aks-01-RG' --query "[].name" --output tsv
   ```

   > **Hinweis:** Vergewissern Sie sich, dass dies die Ressourcen sind, die Sie löschen möchten. Falls ja, fahren Sie mit dem nächsten Schritt fort.

1. Löschen Sie in der Bash-Sitzung von Azure Cloud Shell alle in dieser Übung bereitgestellten Ressourcen, indem Sie die folgenden Befehle ausführen:

   ```azurecli
   az group delete --name 'acr-01-RG' --no-wait --yes
   az group delete --name 'aks-01-RG' --no-wait --yes
   ```

   > **Hinweis:** Der Befehl wird asynchron ausgeführt (wie durch den Parameter „--nowait“ erzwungen). Selbst wenn beide Befehle sofort einen Rückgabewert an die Eingabeaufforderung der Bash-Shell senden, dauert es einige Minuten, bis die Ressourcengruppen und ihre Ressourcen tatsächlich entfernt werden.

1. Schließen Sie den Azure Cloud Shell-Bereich.
