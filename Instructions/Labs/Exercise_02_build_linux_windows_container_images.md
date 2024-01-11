---
Exercise:
  title: 'Übung: Erstellen von Linux- und Windows-Containerimages und Speichern in Azure Container Registry'
  module: Guided Project - Deploy applications to Azure Kubernetes Service
---
# Übung 2: Bereitstellen von Anwendungen in Azure Kubernetes Service (AKS)

## Ziele

Dieses geführte Projekt besteht aus den folgenden Übungen:

+ Übung 1: Bereitstellen von Azure Container Registry (ACR) und Azure Kubernetes Service (AKS).
+ **Übung 2: Erstellen von Linux- und Windows-Containerimages und Speichern in Azure Container Registry.**
+ Übung 3: Bereitstellen von Containerimages in Azure Kubernetes Service
+ Übung 4: Überprüfen der Bereitstellung und Aufheben der Bereitstellung aller Ressourcen.

In dieser Übung erstellen Sie Linux- und Windows-Containerimages und speichern sie in Azure Container Registry.

## Übung 2: Erstellen von Linux- und Windows-Containerimages und Speichern in ACR
In dieser Übung erstellen Sie Linux- und Windows-basierte Docker-Images und pushen sie in die Azure Container Registry, die Sie zuvor in diesem Lab erstellt haben.


>**Hinweis**: Für diese Übung benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free/).
> Verwenden Sie für alle Eigenschaften, die nicht angegeben sind, den Standardwert.


### Aufgabe 1: Erstellen eines Linux-Containerimages und Speichern in ACR
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
