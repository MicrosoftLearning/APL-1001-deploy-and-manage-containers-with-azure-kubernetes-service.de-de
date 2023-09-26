---
Exercise:
  title: 'Übung: Überprüfen der Bereitstellung und Aufheben der Bereitstellung aller Ressourcen'
  module: Guided Project - Deploy applications to Azure Kubernetes Service
---
# Übung 4: Bereitstellen von Anwendungen in Azure Kubernetes Service (AKS)

## Ziele

Dieses geführte Projekt besteht aus den folgenden Übungen:

+ Übung 1: Bereitstellen von Azure Container Registry (ACR) und Azure Kubernetes Service (AKS)
+ Übung 2: Erstellen von Linux- und Windows-Containerimages und Speichern in ACR
+ Übung 3: Bereitstellen von Containerimages in AKS
+ **Übung 4: Überprüfen der Bereitstellung und Aufheben der Bereitstellung aller Ressourcen**

In dieser Übung überprüfen Sie die Bereitstellung der Dienste in den vorherigen Übungen und heben die Bereitstellung aller Ressourcen auf.

# Übung 4: Überprüfen der Bereitstellung und Aufheben der Bereitstellung aller Ressourcen
In dieser Übung überprüfen Sie die Ergebnisse der Bereitstellungen und heben die Bereitstellung aller Ressourcen auf.

>**Hinweis**: Für diese Übung benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free/).
> Verwenden Sie für alle Eigenschaften, die nicht angegeben sind, den Standardwert.

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
In dieser Aufgabe löschen Sie alle Ressourcen, die in diesem Lab bereitgestellt wurden.

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
