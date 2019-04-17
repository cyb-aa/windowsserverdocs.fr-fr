---
title: "Réplication du stockage de cluster à cluster"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
ms.assetid: 834e8542-a67a-4ba0-9841-8a57727ef876
author: nedpyle
ms.date: 10/11/2016
description: "Comment utiliser le réplica de stockage pour répliquer les volumes d'un cluster à un autre exécutant Windows Server2016 Datacenter Edition."
ms.openlocfilehash: 0681f6495390fda9b0c4564bf5516a48652b2c71
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-to-cluster-storage-replication"></a>Réplication du stockage de cluster à cluster

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016

La réplication de cluster à cluster est désormais disponible dans Windows Server2016 Datacenter Edition, ainsi que la réplication de clusters à l’aide des espaces de stockage direct (c’est-à-dire un stockage en attachement direct sans partage). La gestion et la configuration sont similaires à la réplication de serveur à serveur.  

Vous allez configurer ces ordinateurs et ce stockage dans une configuration de cluster à cluster, où un seul cluster réplique son propre ensemble de stockage avec un autre cluster et son ensemble de stockage. Ces nœuds et leur stockage doivent se trouver sur des sites physiques distincts, même si cela n’est pas obligatoire.  

Dans Windows Server2016 Datacenter Edition, aucun outil graphique ne peut configurer un réplica de stockage pour une réplication de cluster à cluster, mais Azure Site Recovery comblera bientôt cette lacune.

> [!IMPORTANT]
> Dans ce test, quatre serveurs sont pris comme exemple. Vous pouvez utiliser autant de serveurs que Microsoft l'autorise dans chaque cluster, soit actuellement 16 pour un cluster d'espaces de stockage direct et 64 pour un cluster de stockage partagé.  
>   
> Ce guide ne couvre pas la configuration des espaces de stockage direct. Pour plus d’informations sur la configuration des espaces de stockage direct, consultez [Espaces de stockage direct dans WindowsServer2016](../storage-spaces/storage-spaces-direct-overview.md).  

Cette procédure pas à pas utilise l’environnement suivant comme exemple:  

-   Deux serveurs membres, nommés **SR-SRV01** et **SR-SRV02** qui sont intégrés ultérieurement dans un cluster nommé **SR-SRVCLUSA**.  

-   Deux serveurs membres, nommés **SR-SRV03** et **SR-SRV04** qui sont intégrés ultérieurement dans un cluster nommé **SR-SRVCLUSB**.  

-   Une paire de sites logiques qui représentent deux centres de données différents, l’un nommé **Redmond** et l’autre **Bellevue**.  

![Diagramme montrant un exemple d’environnement avec un cluster du site de Redmond qui se réplique sur un cluster du site de Bellevue](./media/Cluster-to-Cluster-Storage-Replication/SR_ClustertoCluster.png)  

**FIGURE1: Réplication de cluster à cluster**  

## <a name="prerequisites"></a>Conditions préalables  

* Forêt de services de domaine Active Directory (exécution de Windows Server2016 non nécessaire).  
* Au moins quatre serveurs (deux serveurs dans deux clusters) avec Windows Server2016 Datacenter Edition installé. Prend en charge jusqu’à deux clusters de 64nœuds.  
* Deux ensembles de stockage avec JBOD SAS, SAN Fibre Channel, VHDX partagé, espaces de stockage direct ou cible iSCSI. Le stockage doit contenir un mélange de disques durs et SSD. Nous allons rendre chaque ensemble de stockage disponible uniquement pour chacun des clusters, sans aucun accès partagé entre les clusters.  
* Chaque ensemble de stockage doit autoriser la création d’au moins deux disques virtuels, un pour les données répliquées et un autre pour les journaux. Le stockage physique doit avoir la même taille de secteur sur tous les disques de données. Le stockage physique doit avoir la même taille de secteur sur tous les disques de journal.  
* Au moins une connexion Ethernet/TCP sur chaque serveur pour la réplication synchrone, mais de préférence RDMA.   
* Règles de pare-feu et de routeur appropriées pour autoriser le trafic bidirectionnel ICMP, SMB (port445, ainsi que5445 pour SMB Direct) et WS-MAN (port 5985) entre tous les nœuds.  
* Un réseau entre les serveurs avec une bande passante suffisante pour contenir votre charge de travail d’écriture d’E/S et une latence moyenne de parcours circulaire égale à 5ms pour la réplication synchrone. La réplication asynchrone ne présente pas de recommandation en matière de latence.  
* Impossible de localiser le stockage répliqué sur le lecteur contenant le dossier du système d’exploitation Windows.

La plupart de ces exigences peuvent être déterminées à l’aide de l’applet de commande `Test-SRTopology`. Vous pouvez accéder à cet outil si vous installez les fonctionnalités de réplica de stockage ou des outils de gestion des réplicas de stockage sur au moins un serveur. Il n’est pas nécessaire de configurer des réplicas de stockage pour utiliser cet outil, uniquement pour installer l’applet de commande. Vous trouverez plus d’informations dans la procédure ci-dessous.  

## <a name="step-1-provision-operating-system-features-roles-storage-and-network"></a>Étape1: configurer le système d’exploitation, les fonctionnalités, les rôles, le stockage et le réseau

1.  Installez Windows Server2016 sur les quatrenœuds de serveur avec l’option d’installation **(Expérience utilisateur)** de Windows Server2016 Datacenter. Ne choisissez pas la version Standard Edition si elle est disponible, car elle ne contient pas les réplicas de stockage.  

2.  Ajoutez des informations réseau et joignez-les au domaine, puis redémarrez-les.  

    > [!IMPORTANT]  
    > À partir de ce stade, connectez-vous toujours en tant qu’utilisateur de domaine membre du groupe Administrateurs intégré sur tous les serveurs. Pensez toujours à élever vos invites CMD et Windows PowerShell à l’avenir lors de l’exécution sur une installation de serveur graphique ou sur un ordinateur Windows 10.  

3.  Connectez le premier ensemble de boîtiers de stockage JBOD, la cible iSCSI, le SAN FC ou le stockage sur disque fixe local (DAS) au serveur du site **Redmond**.  

4.  Connectez le deuxième ensemble de stockage au serveur du site **Bellevue**.  

5.  Installez comme nécessaire les derniers pilotes et microprogrammes de stockage et de boîtier du fournisseur, les derniers pilotes du fournisseur HBA, le dernier microprogramme du fournisseur de BIOS/UEFI, les derniers pilotes réseau du fournisseur et les derniers pilotes de carte mère sur les quatre nœuds. Redémarrez les nœuds si nécessaire.  

    > [!NOTE]  
    > Consultez la documentation de votre fournisseur de matériel pour la configuration du stockage partagé et du matériel réseau.  

6.  Vérifiez que les paramètres BIOS/UEFI des serveurs permettent de hautes performances, par exemple avec la désactivation de C-State, la définition de la vitesse de QPI, l’activation de NUMA et la définition de la fréquence mémoire la plus élevée. Vérifiez que la gestion de l’alimentation dans Windows Server est définie pour favoriser de hautes performances. Redémarrez si nécessaire.  

7.  Configurez les rôles comme suit:  

    -   **Méthode graphique**  

        1.  Exécutez **ServerManager.exe** et créez un groupe de serveurs, en ajoutant tous les nœuds de serveur.  

        2.  Installez les rôles et fonctionnalités **Serveur de fichiers** et **Réplica de stockage** sur chacun des nœuds et redémarrez-les.  

    -   **Méthode WindowsPowerShell**  

        Sur SR-SRV04 ou un ordinateur de gestion à distance, exécutez la commande suivante dans une console Windows PowerShell pour installer les fonctionnalités et rôles requis pour un cluster étendu sur les quatre nœuds et redémarrez-les :  

        ```PowerShell
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'  

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }  
        ```  

        Pour plus d’informations sur ces étapes, consultez [Installer ou désinstaller des rôles, services de rôle ou fonctionnalités](http://technet.microsoft.com/library/hh831809.aspx)  

9. Configurez le stockage comme suit:  

    > [!IMPORTANT]  
    > -   Vous devez créer deux volumes sur chacun des boîtiers: un pour les données et l’autre pour les journaux.  
    > -   Les disques de données et de journaux doivent être initialisés en tant que **GPT**, et non **MBR**.  
    > -   Les deux volumes de données doivent être de la même taille.  
    > -   Les deux volumes de journaux doivent être de la même taille.  
    > -   Tous les disques de données répliqués doivent avoir la même taille de secteur.  
    > -   Tous les disques de journaux doivent avoir la même taille de secteur.  
    > -   Les volumes de journaux doivent utiliser un stockage flash, comme les disques SSD.  Microsoft recommande que le stockage de journaux soit plus rapide que le stockage de données. Les volumes de journaux ne doivent jamais être utilisés pour d’autres charges de travail.
    > -   Les disques de données peuvent utiliser des disques HDD, des disques SSD ou une combinaison hiérarchisée et utiliser des espaces de parité ou en miroir, ou RAID1 ou10, ou RAID5 ou RAID50.  
    > -   Le journal doit avoir une taille par défaut de 9Go, qui peut être supérieure ou inférieure selon la configuration requise.  

    -   **Pour les boîtiers JBOD:**  

        1.  Assurez-vous que chaque cluster peut voir uniquement les boîtiers de stockage de ce site et que les connexions SAS sont correctement configurées.  

        2.  Approvisionnez le stockage à l’aide d’espaces de stockage en suivant **les étapes 1 à 3** fournies dans [Déployer des espaces de stockage sur un serveur autonome](http://technet.microsoft.com/library/jj822938.aspx) à l’aide de Windows PowerShell ou le Gestionnaire de serveur.  

    -   **Pour le stockage d’une cible iSCSI:**  

        1.  Assurez-vous que chaque cluster peut voir uniquement les boîtiers de stockage de ce site. Vous devez utiliser plusieurs cartes réseau si vous utilisez iSCSI.  

        2.  Configurez le stockage à l’aide de la documentation de votre fournisseur. Si vous utilisez le ciblage iSCSI basé sur Windows, voir [Stockage par blocs de cibles iSCSI, procédure](http://technet.microsoft.com/library/hh848268.aspx).  

    -   **Pour le stockage SAN FC:**  

        1.  Assurez-vous que chaque cluster peut voir uniquement les boîtiers de stockage de ce site et que vous avez correctement segmenté les hôtes.  

        2.  Provisionner le stockage à l’aide de la documentation de votre fournisseur.  

    -   **Pour les espaces de stockage direct:**  

        1.  Vérifiez que chaque cluster peut voir uniquement les boîtiers de stockage de ce site en déployant uniquement des espaces de stockage direct. (https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct) 

        2.  Assurez-vous que les volumes de journaux SR se trouvent toujours sur le périphérique de stockage flash le plus rapide et les volumes de données sur un périphérique de stockage à haute capacité plus lent.

10. Démarrez WindowsPowerShell et utilisez l’applet de commande `Test-SRTopology` pour déterminer si vous répondez à toutes les conditions des réplicas de stockage. Vous pouvez utiliser l’applet de commande dans un mode d’exigences uniquement pour un test rapide ainsi qu’un mode d’évaluation de performances à exécution longue.  
Par exemple,  

   ```PowerShell
   MD c:\temp

   Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV03 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp        
   ```

      > [!IMPORTANT]
      > Lorsque vous utilisez un serveur de test sans aucune écriture de charge d’E/S sur le volume source spécifié pendant la période d’évaluation, envisagez d’ajouter une charge de travail, ou le rapport généré ne sera pas utile. Vous devez effectuer des tests avec des charges de travail de type production afin de voir des nombres réels et les tailles de journal recommandées. Vous pouvez également simplement copier certains fichiers dans le volume source pendant le test ou télécharger et exécuter [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) pour générer des E/S d’écriture. Par exemple, un échantillon avec une faible charge de travail d’E/S pendant cinq minutes pour le volume D::  
      > `Diskspd.exe -c1g -d300 -W5 -C5 -b8k -t2 -o2 -r -w5 -h d:\test.dat`  

11. Examinez le rapport **TestSrTopologyReport.html** pour vous assurer que les exigences de réplica de stockage sont satisfaites.  

    ![Capture d’écran montrant les résultats du rapport de topologie de réplication](./media/Cluster-to-Cluster-Storage-Replication/SRTestSRTopologyReport.png)      

## <a name="step-2-configure-two-scale-out-file-server-failover-clusters"></a>Étape2: configurer deux clusters de basculement de serveur de fichiers avec montée en charge  
Vous allez maintenant créer deux clusters de basculement normaux. Après la configuration, la validation et le test, vous allez les répliquer à l’aide d’un réplica de stockage. Vous pouvez effectuer toutes les étapes ci-dessous sur les nœuds de cluster directement ou à partir d’un ordinateur distant contenant les outils d’administration de serveur distant de Windows Server2016.  

### <a name="graphical-method"></a>Méthode graphique  

1.  Exécutez **cluadmin.msc** sur un nœud dans chaque site.  

2.  Validez le cluster proposé et analysez les résultats pour vous assurer que vous pouvez continuer. L’exemple ci-dessous contient **SR-SRVCLUSA** et **SR-SRVCLUSB**.  

3.  Créez deux clusters. Assurez-vous que les noms de cluster font 15 caractères ou moins.  

4.  Configurez un témoin de partage de fichiers ou un témoin cloud.  

    > [!NOTE]  
    > Windows Server2016 inclut désormais une option de témoin cloud (Azure). Vous pouvez choisir cette option de quorum au lieu du témoin de partage de fichiers.  

    > [!WARNING]  
    > Pour plus d’informations sur la configuration du quorum, consultez la section **Configuration du témoin** dans [Configurer et gérer le quorum dans un cluster de basculement Windows Server 2012](http://technet.microsoft.com/library/jj612870.aspx). Pour plus d’informations sur l’applet de commande `Set-ClusterQuorum`, consultez la page [Set-ClusterQuorum](http://technet.microsoft.com/library/hh847275.aspx).  

5.  Ajoutez un disque dans le site **Redmond** sur le cluster CSV. Pour ce faire, faites un clic droit sur un disque source dans le nœud **Disques** de la section **Stockage**, puis cliquez sur **Ajouter aux volumes partagés du cluster**.  

6.  Créez les serveurs de fichiers avec montée en charge sur les deux clusters en suivant les instructions de [Configurer un serveur de fichiers avec montée en charge](http://technet.microsoft.com/library/hh831718.aspx)  

### <a name="windows-powershell-method"></a>Méthode WindowsPowerShell  

1.  Testez le cluster proposé et analysez les résultats pour vous assurer que vous pouvez continuer:  

    ```PowerShell
    Test-Cluster SR-SRV01,SR-SRV02  
    Test-Cluster SR-SRV03,SR-SRV04  
    ```  

2.  Créez des clusters (vous devez spécifier vos propres adresses IP statiques pour les clusters). Assurez-vous que chaque nom de cluster fait 15 caractères ou moins:  

    ```PowerShell
    New-Cluster -Name SR-SRVCLUSA -Node SR-SRV01,SR-SRV02 -StaticAddress <your IP here>  
    New-Cluster -Name SR-SRVCLUSB -Node SR-SRV03,SR-SRV04 -StaticAddress <your IP here>  
    ```  

3.  Configurez un témoin de partage de fichiers ou un témoin cloud (Azure) dans chaque cluster qui pointe vers un partage hébergé sur le contrôleur de domaine ou un autre serveur indépendant. Par exemple:  

    ```PowerShell  
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare  
    ```  

    > [!NOTE]  
    > Windows Server2016 inclut désormais une option de témoin cloud (Azure). Vous pouvez choisir cette option de quorum au lieu du témoin de partage de fichiers.  

    > [!WARNING]  
    > Pour plus d’informations sur la configuration du quorum, consultez la section **Configuration du témoin** dans [Configurer et gérer le quorum dans un cluster de basculement Windows Server 2012](http://technet.microsoft.com/library/jj612870.aspx). Pour plus d’informations sur l’applet de commande `Set-ClusterQuorum`, consultez la page [Set-ClusterQuorum](http://technet.microsoft.com/library/hh847275.aspx).  

4.  Créez les serveurs de fichiers avec montée en charge sur les deux clusters en suivant les instructions de [Configurer un serveur de fichiers avec montée en charge](https://technet.microsoft.com/library/hh831718.aspx)  

## <a name="step-3-set-up-cluster-to-cluster-replication-using-windows-powershell"></a>Étape3: configurer la réplication de cluster à cluster à l’aide de Windows PowerShell  
Vous allez maintenant configurer la réplication de cluster à cluster à l’aide de Windows PowerShell. Vous pouvez effectuer toutes les étapes ci-dessous sur les nœuds directement ou sur un ordinateur distant contenant les outils d’administration de serveur distant de Windows Server2016.  

1.  Accordez au premier cluster un accès complet à l’autre cluster en exécutant l’applet de commande **Grant-ClusterAccess** sur n’importe quel nœud du premier cluster, ou à distance.  

    ```PowerShell
    Grant-SRAccess -ComputerName SR-SRV01 -Cluster SR-SRVCLUSB  
    ```  

2.  Accordez au second cluster un accès complet à l’autre cluster en exécutant l’applet de commande **Grant-ClusterAccess** sur n’importe quel nœud du second cluster, ou à distance.  

    ```PowerShell
    Grant-SRAccess -ComputerName SR-SRV03 -Cluster SR-SRVCLUSA  
    ```  

3.  Configurez la réplication de cluster à cluster, en spécifiant les disques de la source et de la destination, les journaux de la source et de la destination, les noms de cluster de la source et de la destination, et la taille du journal. Vous pouvez exécuter cette commande localement sur le serveur ou à l’aide d’un ordinateur de gestion à distance.  

    ```powershell  
    New-SRPartnership -SourceComputerName SR-SRVCLUSA -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\Volume2 -SourceLogVolumeName f: -DestinationComputerName SR-SRVCLUSB -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\Volume2 -DestinationLogVolumeName f:  
    ```  

    > [!WARNING]  
    > La taille du journal par défaut est de 8 Go. En fonction des résultats de l’applet de commande **Test-SRTopology**, vous pouvez décider d’utiliser **- LogSizeInBytes** avec une valeur supérieure ou inférieure.  

4.  Pour obtenir l’état de réplication de la source et de la destination, utilisez **Get-SRGroup** et **Get-SRPartnership**, comme suit:  

    ```powershell
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```  

5.  Déterminez la progression de la réplication comme suit:  

    1.  Sur le serveur source, exécutez la commande suivante et examinez les événements 5015, 5002, 5004, 1237, 5001 et 2200:
        
        ```PowerShell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20
        ```
    2.  Sur le serveur de destination, exécutez la commande suivante pour afficher les événements de réplica de stockage qui indiquent la création du partenariat. Cet événement indique le nombre d’octets copiés et la durée de l’opération. Exemple :  
        
        ```powershell
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | Format-List
        ```
        Voici un exemple de sortie:
        
        ```
        TimeCreated  : 4/8/2016 4:12:37 PM  
        ProviderName : Microsoft-Windows-StorageReplica  
        Id           : 1215  
        Message      : Block copy completed for replica.  
            ReplicationGroupName: rg02  
            ReplicationGroupId:  
            {616F1E00-5A68-4447-830F-B0B0EFBD359C}  
            ReplicaName: f:\  
            ReplicaId: {00000000-0000-0000-0000-000000000000}  
            End LSN in bitmap:  
            LogGeneration: {00000000-0000-0000-0000-000000000000}  
            LogFileId: 0  
            CLSFLsn: 0xFFFFFFFF  
            Number of Bytes Recovered: 68583161856  
            Elapsed Time (seconds): 117  
        ```
    3. Alternativement, le groupe de serveurs de destination pour le réplica indique le nombre d’octets restants à copier à tout moment, et peut être obtenu via PowerShell. Par exemple:

       ```PowerShell
       (Get-SRGroup).Replicas | Select-Object numofbytesremaining
       ```

       Un exemple de progression (qui ne s’interrompt pas):  

       ```PowerShell
         while($true) {  
         $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }
        ```

6. Sur le serveur de destination dans le cluster de destination, exécutez la commande suivante et examinez les événements 5009, 1237, 5001, 5015, 5005 et 2200 pour comprendre la progression du traitement. Il ne doit y avoir aucun avertissement ou erreur dans cette séquence. Il y aura un grand nombre d’événements 1237; ils indiquent la progression.  
    
   ```PowerShell
   Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
   ```
   > [!NOTE]  
        > Le disque de cluster de destination s’affiche toujours comme étant **En ligne (aucun accès)** lors de la réplication.  

## <a name="step-4-manage-replication"></a>Étape4: gérer la réplication

Vous gérez et opérez maintenant votre réplication cluster à cluster. Vous pouvez effectuer toutes les étapes ci-dessous sur les nœuds de cluster directement ou sur un ordinateur distant contenant les outils d’administration de serveur distant de Windows Server2016.  

1.  Utilisez **Get-ClusterGroup** ou **Gestionnaire du cluster de basculement** pour déterminer la source et la destination de réplication et leur état actuel.  

2.  Pour mesurer les performances de la réplication, utilisez l’applet de commande **Get-Counter** sur les nœuds source et de destination. Les noms des compteurs sont:  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Nombre de suspensions du vidage  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Nombre d’E/S de vidage en attente  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Nombre de demandes associées à la dernière écriture de journal  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Longueur moyenne de file d’attente de vidage  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Longueur de file d’attente de vidage actuelle  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Nombre de demandes d’écriture d’application  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Nombremoy. de demandes par écriture de journal  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Latence moyenne d’écriture d’application  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Latence moyenne de lecture d’application  

    -   \Statistiques du réplica du système de stockage(*)\RPO cible  

    -   \Statistiques du réplica du système de stockage(*)\RPO actuel  

    -   \Statistiques du réplica du système de stockage(*)\Longueur moyenne de file d’attente de journal  

    -   \Statistiques du réplica du système de stockage(*)\Longueur de file d’attente de journal actuelle  

    -   \Statistiques du réplica du système de stockage(*)\Total des octets reçus  

    -   \Statistiques du réplica du système de stockage(*)\Total des octets envoyés  

    -   \Statistiques du réplica du système de stockage(*)\Latence moyenne d’envoi réseau  

    -   \Statistiques du réplica du système de stockage(*)\État de réplication  

    -   \Statistiques du réplica du système de stockage(*)\Latencemoy. de boucle de message  

    -   \Statistiques du réplica du système de stockage(*)\Temps écoulé pour la dernière récupération  

    -   \Statistiques du réplica du système de stockage(*)\Nombre de transactions de récupération vidées  

    -   \Statistiques du réplica du système de stockage(*)\Nombre de transactions de récupération  

    -   \Statistiques du réplica du système de stockage(*)\	Nombre de transactions de réplication vidées  

    -   \Statistiques du réplica du système de stockage(*)\	Nombre de transactions de réplication  

    -   \Statistiques du réplica du système de stockage(*)\Numéro séquentiel dans le journal max.  

    -   \Statistiques du réplica du système de stockage(*)\Nombre de messages reçus  

    -   \Statistiques du réplica du système de stockage(*)\Nombre de messages envoyés  

    Pour plus d’informations sur les compteurs de performances dans Windows PowerShell, consultez [Get-Counter](https://technet.microsoft.com/library/hh849685.aspx).  

3.  Pour déplacer la direction de réplication d’un site, utilisez l’applet de commande **Set-SRPartnership**.  

    ```PowerShell
    Set-SRPartnership -NewSourceComputerName SR-SRVCLUSB -SourceRGName rg02 -DestinationComputerName SR-SRVCLUSA -DestinationRGName rg01  
    ```  

    > [!NOTE]  
    > Windows Server2016 empêche le basculement de rôle lorsque la synchronisation initiale est en cours, ce qui peut entraîner une perte de données si vous déclenchez un basculement avant la fin de la réplication initiale. Ne forcez pas les directions de basculement tant que la synchronisation initiale n’est pas terminée.

    Vérifiez les journaux d’événements pour voir la direction du changement de réplication et la survenue du mode de récupération, puis rapprochez. Les E/S d’écriture peuvent alors écrire sur le stockage du nouveau serveur source. Modifier la direction de réplication bloquera les E/S d’écriture sur l’ordinateur source précédent.  

    > [!NOTE]  
    > Le disque de cluster de destination s’affiche toujours comme étant **En ligne (aucun accès)** lors de la réplication.  

4.  Pour modifier la taille du journal par défaut de 8Go dans Windows Server2016, utilisez **Set-SRGroup** sur les groupes de réplicas de stockage source et cible.  

    > [!IMPORTANT]  
    > La taille du journal par défaut est de 8Go. En fonction des résultats de l’applet de commande **Test-SRTopology**, vous pouvez décider d’utiliser -LogSizeInBytes avec une valeur supérieure ou inférieure.  

5.  Pour supprimer la réplication, utilisez **Get-SRGroup**, **Get-SRPartnership**, **Remove-SRGroup** et **Remove-SRPartnership** dans chaque cluster.  

    ```powershell
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

    > [!NOTE]  
    > Le réplica de stockage démonte les volumes de destination. Cela est normal.

## <a name="see-also"></a>Articles associés

-   [Vue d’ensemble du réplica de stockage](storage-replica-overview.md) 
-   [Réplication de cluster étendu à l’aide d’un stockage partagé](stretch-cluster-replication-using-shared-storage.md)  
-   [Réplication de stockage de serveur à serveur](server-to-server-storage-replication.md)  
-   [Réplica de stockage: Problèmes connus](storage-replica-known-issues.md)  
-   [Réplica de stockage: Forum Aux Questions](storage-replica-frequently-asked-questions.md)  
-   [Espaces de stockage direct dans Windows Server2016](../storage-spaces/storage-spaces-direct-overview.md)  
