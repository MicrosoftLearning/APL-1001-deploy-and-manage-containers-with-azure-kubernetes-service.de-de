---
Exercise:
  title: 'Übung: Bereitstellen von Containerimages in Azure Kubernetes Service'
  module: Guided Project - Deploy applications to Azure Kubernetes Service
---
# Übung 3: Bereitstellen von Anwendungen in Azure Kubernetes Service (AKS)

## Ziele

Dieses geführte Projekt besteht aus den folgenden Übungen:

+ Übung 1: Bereitstellen von Azure Container Registry (ACR) und Azure Kubernetes Service (AKS).
+ Übung 2: Erstellen von Linux- und Windows-Containerimages und Speichern in Azure Container Registry.
+ **Übung 3: Bereitstellen von Containerimages in Azure Kubernetes Service**
+ Übung 4: Überprüfen der Bereitstellung und Aufheben der Bereitstellung aller Ressourcen

In dieser Übung stellen Sie Containerimages in Azure Kubernetes Service bereit.

## Übung 3: Bereitstellen von Containerimages in AKS 
In dieser Übung stellen Sie zwei Containerimages, die Sie zuvor in dieser Übung erstellt haben, im AKS-Cluster bereit.
>**Hinweis**: Für diese Übung benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free/).
> Verwenden Sie für alle Eigenschaften, die nicht angegeben sind, den Standardwert.
> **Hinweis:** Bevor Sie mit dieser Übung fortfahren, vergewissern Sie sich, dass die Bereitstellung des AKS-Clusters erfolgreich abgeschlossen wurde.

### Aufgabe 1: Erstellen benutzerdefinierter AKS-Namespaces
In dieser Aufgabe erstellen Sie zwei Namespaces in dem AKS-Cluster, den Sie zuvor in diesem Lab erstellt haben.

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

