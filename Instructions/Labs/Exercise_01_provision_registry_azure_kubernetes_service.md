---
Exercise:
  title: 'Übung: Bereitstellen von Azure Container Registry (ACR) und Azure Kubernetes Service (AKS)'
  module: Guided Project - Deploy applications to Azure Kubernetes Service
---

# Übung: Bereitstellen von Anwendungen in Azure Kubernetes Service (AKS)

## Ziele

Dieses geführte Projekt besteht aus den folgenden Übungen:

+ **Übung 1: Bereitstellen von Azure Container Registry (ACR) und Azure Kubernetes Service (AKS)**
+ Übung 2: Erstellen von Linux- und Windows-Containerimages und Speichern in Azure Container Registry.
+ Übung 3: Bereitstellen von Containerimages in Azure Kubernetes Service
+ Übung 4: Überprüfen der Bereitstellung und Aufheben der Bereitstellung aller Ressourcen

In dieser Übung stellen Sie Azure Container Registry- und Azure Kubernetes Service-Ressourcen bereit.
## Übung 1: Bereitstellen von Azure Container Registry (ACR) und Azure Kubernetes Service (AKS)
In dieser Übung erstellen Sie eine Azure Container Registry-Instanz und einen AKS-Cluster.

>**Hinweis**: Für diese Übung benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free/).
> Verwenden Sie für alle Eigenschaften, die nicht angegeben sind, den Standardwert.

### Aufgabe 1: Erstellen einer Azure Container Registry-Instanz
In dieser Aufgabe erstellen Sie eine Azure Container Registry-Instanz.

1. Öffnen Sie von Ihrem Computer aus ein Webbrowserfenster, und navigieren Sie zum Azure-Portal unter https://portal.azure.com.
1. Melden Sie sich bei der entsprechenden Aufforderung mit einem Benutzerkonto an, das über die Rolle „Besitzer“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab verwenden werden. 
1. Melden Sie sich beim Azure-Portal an. 
1. Suchen Sie im Azure-Portal im Textfeld **Suchen** nach **Containerregistrierungen**, und wählen Sie diese Option aus.
1. Wählen Sie auf der Seite **Containerregistrierungen** die Option **+ Erstellen** aus, und geben Sie die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden|
   |Ressourcengruppe|Der Name einer neuen Ressourcengruppe: **acr-01-RG**|
   |Registrierungsname|Einen gültigen, global eindeutigen Namen, der aus 5 bis 50 alphanumerischen Zeichen besteht|
   |Location|Eine beliebige Azure-Region, in der Sie eine Azure Container Registry-Instanz und einen AKS-Cluster erstellen können|
   |Verwenden von Verfügbarkeitszonen|Disabled|
   |Tarif|**Grundlegend**|

1. Wählen Sie auf der Seite **Containerregistrierungen** die Option **Überprüfen + erstellen** und auf der Registerkarte **Überprüfen + erstellen** die Option **Erstellen** aus.

   > **Hinweis:** Fahren Sie mit der nächsten Übung fort, ohne auf den Abschluss der Bereitstellung der Azure Container Registry-Instanz zu warten.

### Aufgabe 2: Erstellen eines virtuellen Azure-Netzwerks und eines AKS-Clusters
In dieser Aufgabe erstellen Sie ein virtuelles Azure-Netzwerk und stellen ein AKS-Cluster einschließlich eines Windows-Knotenpools in diesem bereit.

> **Hinweis:** Während Sie beim Bereitstellen eines AKS-Clusters ein virtuelles Netzwerk erstellen können, erfordert die Bereitstellung von AKS-Clustern in einem vorhandenen virtuellen Netzwerk einige zusätzliche Überlegungen, mit denen Sie vertraut sein sollten.

1. Suchen Sie im Azure-Portal im Textfeld **Suchen** nach **Virtuelle Netzwerke**, und wählen Sie diese Option aus.
1. Wählen Sie auf der Seite **Virtuelle Netzwerke** die Option **+ Erstellen** aus, und geben Sie dann auf der Registerkarte **Grundeinstellungen** der Seite **Virtuelles Netzwerk erstellen** die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Abonnement|Der Name des Azure-Abonnements, das Sie in der ersten Übung dieses Labs ausgewählt haben|
   |Ressourcengruppe|Der Name einer neuen Ressourcengruppe: **aks-01-RG**|
   |Name des virtuellen Netzwerks|**vnet-01**|
   |Region|Dieselbe Azure-Region, die Sie in der ersten Übung dieses Labs ausgewählt haben|

1. Wählen Sie auf der Registerkarte **Grundeinstellungen** der Seite **Virtuelles Netzwerk erstellen** die Option **Weiter** aus.
1. Akzeptieren Sie auf der Registerkarte **Sicherheit** der Seite **Virtuelles Netzwerk erstellen** die Standardeinstellungen, und wählen Sie **Weiter** aus.
1. Stellen Sie auf der Registerkarte **IP-Adressen** der Seite **Virtuelles Netzwerk erstellen** sicher, dass der IP-Adressraum auf **10.0.0.0/16** festgelegt ist, löschen Sie das **Standardsubnetz**, wählen Sie **Überprüfen + erstellen** aus, und wählen Sie dann auf der Registerkarte **Überprüfen + erstellen** die Option **Erstellen** aus.

   > **Hinweis:** Das Erstellen eines virtuellen Netzwerks sollte nur wenige Sekunden dauern, sodass Sie direkt mit dem nächsten Schritt fortfahren können.

1. Suchen Sie im Azure-Portal im Textfeld **Suchen** nach **Kubernetes-Dienste**, und wählen Sie diese Option aus.
1. Wählen Sie auf der Seite **Kubernetes-Dienste** die Option **+ Erstellen** aus, wählen Sie in der Dropdownliste die Option **Kubernetes-Cluster erstellen** aus, und geben Sie dann auf der Registerkarte **Grundeinstellungen** der Seite **Kubernetes-Cluster erstellen** die folgenden Einstellungen an und wählen sie **Weiter** aus:

   |Einstellung|Wert|
   |---|---|
   |Abonnement|Der Name des Azure-Abonnements, das Sie in der ersten Übung dieses Labs ausgewählt haben|
   |Ressourcengruppe|**aks-01-RG**|
   |Voreingestellte Clusterkonfiguration|**Dev/Test**|
   |Kubernetes-Clustername|**aks-01**|
   |Region|Dieselbe Azure-Region, die Sie in der ersten Übung dieses Labs ausgewählt haben|
   |Verfügbarkeitszonen|**Keine**|
   |AKS-Tarif|**Free**|
   |Kubernetes-Version|Akzeptieren Sie den Standardwert.|
   |automatische Upgrade|Deaktiviert|
   |Kanaltyp für Knotensicherheit|**Keine**|
   |Authentifizierung und Autorisierung|**Lokale Konten mit Kubernetes RBAC**|

1. Führen Sie auf der Registerkarte **Knotenpools** der Seite **Kubernetes-Cluster erstellen** die folgenden Aufgaben aus:

   - Wählen Sie im Abschnitt **Knotenpools** den Link **Agentpool** aus.
   - Wählen Sie auf der Seite **Knotenpool aktualisieren** im Abschnitt **Knotengröße** den Link **Größe auswählen** aus.
   - Wählen Sie auf der Seite **Vm-Größe**in der Liste der VM-Größen **D2s_v3** aus, und klicken Sie dann auf **Auswählen**.
   - Legen Sie auf der Seite **Knotenpool aktualisieren** die **Scale-Methode** auf **manuell** fest und die **Knotenanzahl** auf **2**.
   - Wählen Sie auf der Seite **Knotenpool aktualisieren** **Aktualisieren** aus.

   > **Hinweis:** Möglicherweise müssen Sie die vCPU-Kontingente erhöhen oder die VM-SKU ändern, um die Werte für Knotengröße und Knotenanzahl anzupassen. Informationen zum Erhöhen von vCPU-Kontingenten finden Sie im Microsoft Learn-Artikel [Erhöhen von vCPU-Kontingenten für die VM-Familie](https://learn.microsoft.com/en-us/azure/quotas/per-vm-quota-requests).

   > **Hinweis:** Sie fügen dem Cluster einen Windows-Knotenpool hinzu. Hierfür muss die Netzwerkkonfiguration von **Kubenet** (Standardeinstellung) in **Azure CNI** geändert werden. Die Kubenet-Netzwerkkonfiguration unterstützt keine Windows-Knotenpools.

1. Wählen Sie wieder auf der Registerkarte **Knotenpools** der Seite **Kubernetes-Cluster erstellen** die Option **Weiter** aus.
1. Stellen Sie auf der Registerkarte **Networking** der Seite **Kubernetes-Cluster erstellen** sicher, dass die **Azure CNI-** Option ausgewählt ist, aktivieren Sie das Kontrollkästchen **Ihr eigenes virtuelles Netzwerk** in der Dropdownliste **virtuelles Netzwerk**,  wählen Sie **vnet-01-** aus, und wählen Sie unter dem **Textfeld** Cluster-Subnetz  die Option **Subnetzkonfiguration verwalten**aus.
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
   |Virtuelles Netzwerk|**vnet-01**|
   |Clustersubnetz|**aks-subnet (10.0.0.0/20)**|
   |Kubernetes-Dienstadressbereich|**172.16.0.0/22**|
   |Kubernetes-DNS-Dienst – IP-Adresse|**172.16.3.254**|
   |DNS-Namenspräfix|**aks-01-dns**|
   |Netzwerkrichtlinie|**Keine**|

1. Wählen Sie auf der Registerkarte **Netzwerk** der Seite **Kubernetes-Cluster erstellen** die Option **Zurück** aus.
1. Wählen Sie wieder auf der Registerkarte **Knotenpools** der Seite **Kubernetes-Cluster erstellen** die Option **+ Knotenpool hinzufügen** aus.
1. Geben Sie auf der Seite **Knotenpool hinzufügen** die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Knotenpoolname|**w1pool**|
   |Mode|**Benutzer**|
   |Betriebssystemtyp|**Windows 2022**|
   |Verfügbarkeitszone|**Keine**|
   |Azure Spot-Instanzen aktivieren|Deaktiviert|
   |Knotengröße|**D2s_v3**|
   |Skalierungsmethode|**Manuell**|
   |Anzahl der Knoten|**2**|
   |Max. Pods pro Knoten|**30**|
   |Öffentliche IP-Adresse pro Knoten aktivieren|Deaktiviert|

   > **Hinweis:** Möglicherweise müssen Sie hier auch die vCPU-Kontingente erhöhen oder die VM-SKU ändern, um die Werte für Knotengröße und Knotenanzahl anzupassen.

1. Wählen Sie auf der Seite **Knotenpool hinzufügen** die Option **Hinzufügen** aus.
1. Wählen Sie wieder auf der Registerkarte **Knotenpools** der Seite **Kubernetes-Cluster erstellen** die Option **Weiter** aus.
1. Wählen Sie auf der Registerkarte **Networking** der Seite **Kubernetes-Cluster erstellen** **Weiter** aus.
1. Wählen Sie auf der Registerkarte **Integration** der Seite **Kubernetes-Cluster erstellen** in der Dropdownliste **Containerregistrierung** den Eintrag aus, der die Azure Container Registry-Instanz darstellt, die Sie in der vorherigen Übung erstellt haben, stellen Sie sicher, dass die Option**Azure Policy** deaktiviert ist und wählen Sie **Weiter**.
1. Deaktivieren Sie auf der **Registerkarte "Überwachung** " der **Clusterseite** "Kubernetes erstellen" das **Kontrollkästchen "Prometheus-Metriken** aktivieren", deaktivieren Sie das **Kontrollkästchen "Empfohlene Benachrichtigungsregeln** aktivieren", und wählen Sie dann **"Überprüfen+ Erstellen **" aus.
1. Wählen Sie auf der Registerkarte **Überprüfen + erstellen** der Seite **Kubernetes-Cluster erstellen** die Option **Erstellen** aus.

   > **Hinweis:** Fahren Sie mit der nächsten Übung fort, ohne zu warten, bis die Bereitstellung des AKS-Clusters abgeschlossen ist. Die Bereitstellung dauert etwa fünf Minuten.
