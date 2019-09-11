---
title: Réplication de stockage de serveur à serveur
description: Comment configurer et utiliser le réplica de stockage pour la réplication de serveur à serveur dans Windows Server, y compris le centre d’administration Windows et PowerShell.
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 61881b52-ee6a-4c8e-85d3-702ab8a2bd8c
ms.openlocfilehash: fccdb8547ff27083ce943892842c2e2d05e5ace8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865289"
---
# <a name="server-to-server-storage-replication-with-storage-replica"></a>Réplication de stockage de serveur à serveur avec le réplica de stockage

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)

Vous pouvez utiliser le réplica de stockage pour configurer la synchronisation des données de deux serveurs, afin que chacune possède une copie identique du même volume. Cette rubrique fournit des informations générales sur cette configuration de réplication de serveur à serveur, ainsi que sur la configuration et la gestion de l’environnement.

Pour gérer le réplica de stockage, vous pouvez utiliser le [Centre d’administration Windows](../../manage/windows-admin-center/overview.md) ou PowerShell.

Voici une vidéo de présentation de l’utilisation du réplica de stockage dans le centre d’administration Windows.
> [!video https://www.microsoft.com/videoplayer/embed/3aa09fd4-867b-45e9-953e-064008468c4b?autoplay=false]


## <a name="prerequisites"></a>Prérequis  

* Active Directory Domain Services forêt (n’a pas besoin d’exécuter Windows Server 2016).  
* Deux serveurs exécutant Windows Server 2019 ou Windows Server 2016, Datacenter Edition. Si vous exécutez Windows Server 2019, vous pouvez à la place utiliser l’édition standard si vous ne répliquez qu’un seul volume, avec une taille maximale de 2 to.  
* Deux ensembles de stockage, utilisant des JBOD SAS, un réseau SAN Fibre Channel, une cible iSCSI ou un stockage SCSI/SATA local. Le stockage doit contenir un mélange de disques durs HDD et SSD. Nous allons rendre chaque ensemble de stockage disponible uniquement pour chacun des serveurs, sans aucun accès partagé.  
* Chaque ensemble de stockage doit autoriser la création d’au moins deux disques virtuels, un pour les données répliquées et un autre pour les journaux. Le stockage physique doit avoir la même taille de secteur sur tous les disques de données. Le stockage physique doit avoir la même taille de secteur sur tous les disques de journal.  
* Au moins une connexion Ethernet/TCP sur chaque serveur pour la réplication synchrone, mais de préférence RDMA.   
* Les règles de pare-feu et de routeur appropriés pour autoriser le trafic bidirectionnel ICMP, SMB (port 445, ainsi que 5445 pour SMB Direct) et WS-MAN (port 5985) entre tous les nœuds.  
* Un réseau entre les serveurs avec une bande passante suffisante pour contenir votre charge de travail d’écriture d’E/S et une latence moyenne de parcours circulaire égale à 5 ms pour la réplication synchrone. La réplication asynchrone n’a pas de recommandation de latence.<br>
Si vous effectuez une réplication entre des serveurs locaux et des machines virtuelles Azure, vous devez créer une liaison réseau entre les serveurs locaux et les machines virtuelles Azure. Pour ce faire, utilisez [Express route](#add-azure-vm-expressroute), une [connexion de passerelle VPN de site à site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)ou installez le logiciel VPN sur vos machines virtuelles Azure pour les connecter à votre réseau local.
* Impossible de localiser le stockage répliqué sur le lecteur contenant le dossier du système d’exploitation Windows.

> [!IMPORTANT]
>  Dans ce scénario, chaque serveur doit être dans un site physique ou logique différent. Chaque serveur doit être en mesure de communiquer avec les autres par le biais d’un réseau.  


Beaucoup de ces exigences peuvent être déterminées à l’aide de `Test-SRTopology cmdlet`. Vous pouvez accéder à cet outil si vous installez un réplica de stockage ou les fonctionnalités des outils de gestion des réplicas de stockage sur au moins un serveur. Il n’est pas nécessaire de configurer de réplica de stockage pour utiliser cet outil, mais vous devez installer l’applet de commande. Vous trouverez plus d’informations dans la procédure ci-dessous.  

## <a name="windows-admin-center-requirements"></a>Configuration requise pour le centre d’administration Windows

Pour utiliser ensemble le réplica de stockage et le centre d’administration Windows, vous avez besoin des éléments suivants :

| System                        | Système d’exploitation                                            | Requis pour     |
|-------------------------------|-------------------------------------------------------------|------------------|
| Deux serveurs <br>(n’importe quelle combinaison de matériel local, de machines virtuelles et de machines virtuelles Cloud, y compris les machines virtuelles Azure)| Windows Server 2019, Windows Server 2016 ou Windows Server (canal semi-annuel) | Réplica de stockage  |
| Un PC                     | Windows 10                                                  | Windows Admin Center |

> [!NOTE]
> Pour le moment, vous ne pouvez pas utiliser le centre d’administration Windows sur un serveur pour gérer le réplica de stockage.

## <a name="terms"></a>Termes  
Cette procédure pas à pas utilise l’environnement suivant comme exemple :  

-   Deux serveurs, nommés **SR-SRV05** et **SR-SRV06**.  

-   Une paire de sites logiques qui représentent deux centres de données différents, l’un étant nommé **Redmond** et l’autre **Bellevue**.  

![Diagramme montrant un serveur du bâtiment 5 en cours de réplication avec un serveur du bâtiment 9](media/Server-to-Server-Storage-Replication/Storage_SR_ServertoServer.png)  

**Figure 1 : Réplication de serveur à serveur**  

## <a name="step-1-install-and-configure-windows-admin-center-on-your-pc"></a>Étape 1 : Installer et configurer le centre d’administration Windows sur votre PC

Si vous utilisez le centre d’administration Windows pour gérer le réplica de stockage, suivez les étapes ci-dessous pour préparer votre ordinateur à la gestion du réplica de stockage.
1. Téléchargez et installez le [Centre d’administration Windows](../../manage/windows-admin-center/overview.md).
2. Téléchargez et installez le [Outils d’administration de serveur distant](https://www.microsoft.com/download/details.aspx?id=45520).
    - Si vous utilisez Windows 10, version 1809 ou ultérieure, installez le «RSAT : Module de réplica de stockage pour Windows PowerShell» de fonctionnalités à la demande.
3. Ouvrez une session PowerShell en tant qu’administrateur en sélectionnant le bouton **Démarrer** , en tapant **PowerShell**, en cliquant avec le bouton droit sur **Windows PowerShell,** puis en sélectionnant **exécuter en tant qu’administrateur**.
4. Entrez la commande suivante pour activer le protocole WS-Management sur l’ordinateur local et configurer la configuration par défaut pour la gestion à distance sur le client.

    ```PowerShell
    winrm quickconfig
    ```

5. Tapez **Y** pour activer les services WinRM et activer l’exception de pare-feu WinRM.

## <a name="provision-os"></a>Étape 2 : Configurer le système d’exploitation, les fonctionnalités, les rôles, le stockage et le réseau

1.  Installez Windows Server sur les deux nœuds de serveur avec le type d’installation Windows Server **(expérience utilisateur)** . 
 
    Pour utiliser une machine virtuelle Azure connectée à votre réseau via un ExpressRoute, consultez [Ajout d’une machine virtuelle Azure connectée à votre réseau via ExpressRoute](#add-azure-vm-expressroute).

3.  Ajoutez des informations réseau, joignez les serveurs au même domaine que votre PC de gestion Windows 10 (si vous en utilisez un), puis redémarrez les serveurs.  

    > [!NOTE]
    > À partir de ce stade, connectez-vous toujours en tant qu’utilisateur de domaine membre du groupe Administrateurs intégré sur tous les serveurs. Pensez toujours à élever vos invites CMD et PowerShell à l’avenir lors de l’exécution sur une installation de serveur graphique ou sur un ordinateur Windows10.  

3.  Connectez le premier ensemble de boîtier de stockage JBOD, de cible iSCSI, de réseau SAN FC ou de stockage sur disque fixe local (DAS) au serveur dans le site **Redmond**.  

4.  Connectez le deuxième ensemble de stockage au serveur dans le site **Bellevue**.  

5.  Installez comme nécessaire les derniers pilotes et microprogrammes de stockage et de boîtier du fournisseur, les derniers pilotes du fournisseur HBA, le dernier microprogramme du fournisseur de BIOS/UEFI, les derniers pilotes réseau du fournisseur et les derniers pilotes de carte mère sur les deux nœuds. Redémarrez les nœuds si besoin.  

    > [!NOTE]
    > Consultez la documentation de votre fournisseur de matériel pour la configuration du stockage partagé et du matériel réseau.  

6.  Vérifiez que les paramètres BIOS/UEFI des serveurs permettent de hautes performances, par exemple avec la désactivation de C-State, la définition de la vitesse de QPI, l’activation de NUMA et la définition de la fréquence mémoire la plus élevée. Assurez-vous que la gestion de l’alimentation dans Windows Server est définie sur hautes performances. Redémarrez si nécessaire.  

7.  Configurez les rôles comme suit :  

    -   **Méthode du centre d’administration Windows**
        1. Dans le centre d’administration Windows, accédez à Gestionnaire de serveur, puis sélectionnez l’un des serveurs.
        2. Accédez à **rôles & fonctionnalités**.
        3. Sélectionnez **fonctionnalités** > de**réplica de stockage**, puis cliquez sur **installer**.
        4. Répétez l’opération sur l’autre serveur.
    -   **Méthode Gestionnaire de serveur**  

        1.  Exécutez **ServerManager.exe** et créez un groupe de serveurs, en ajoutant tous les nœuds de serveur.  

        2.  Installez les rôles et fonctionnalités **Serveur de fichiers** et **Réplica de stockage** sur chacun des nœuds et redémarrez-les.  

    -   **Méthode Windows PowerShell**  

        Sur SR-SRV06 ou un ordinateur de gestion à distance, exécutez la commande suivante dans une console Windows PowerShell pour installer les fonctionnalités et rôles requis et redémarrez-les :  

        ```powershell  
        $Servers = 'SR-SRV05','SR-SRV06'  

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,FS-FileServer -IncludeManagementTools -restart }  
        ```  

        Pour plus d’informations sur ces étapes, voir [Installer ou désinstaller des rôles, services de rôle ou fonctionnalités](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md).  

8.  Configurez le stockage comme suit :  

    > [!IMPORTANT]  
    > -   Vous devez créer deux volumes sur chacun des boîtiers : un pour les données et l’autre pour les journaux.  
    > -   Les disques de données et de journaux doivent être initialisés en tant que GPT, et non MBR.  
    > -   Les deux volumes de données doivent être de la même taille.  
    > -   Les deux volumes de journaux doivent être de la même taille.  
    > -   Tous les disques de données répliqués doivent avoir la même taille de secteur.  
    > -   Tous les disques de journaux doivent avoir la même taille de secteur.  
    > -   Les volumes de journaux doivent utiliser un stockage flash, comme les disques SSD. Microsoft recommande que le stockage de journaux soit plus rapide que le stockage de données. Les volumes de journaux ne doivent jamais être utilisés pour d’autres charges de travail.
    > -   Les disques de données peuvent utiliser des disques HDD, des disques SSD ou une combinaison hiérarchisée et utiliser des espaces de parité ou en miroir, ou RAID1 ou10, ou RAID5 ou RAID50.  
    > -   Le journal doit avoir une taille par défaut de 9 Go, qui peut être supérieure ou inférieure selon la configuration requise.  
    > -   Le rôle serveur de fichiers est uniquement nécessaire pour que Test-SRTopology fonctionne, car elle ouvre les ports de pare-feu nécessaires au test.
    
    - **Pour les boîtiers JBOD :**  

        1.  Assurez-vous que chaque serveur peut voir uniquement les boîtiers de stockage de ce site et que les connexions SAS sont correctement configurées.  

        2.  Configurez le stockage à l’aide d’espaces de stockage en suivant les **étapes1 à3** indiquées dans [Déployer des espaces de stockage sur un serveur autonome](../storage-spaces/deploy-standalone-storage-spaces.md) à l’aide de Windows PowerShell ou du Gestionnaire de serveur.  

    - **Pour le stockage iSCSI :**  

        1.  Assurez-vous que chaque cluster peut voir uniquement les boîtiers de stockage de ce site. Vous devez utiliser plusieurs cartes réseau si vous utilisez iSCSI.    

        2.  Configurez le stockage à l’aide de la documentation de votre fournisseur. Si vous utilisez le ciblage iSCSI basé sur Windows, voir [Stockage par blocs de cibles iSCSI, procédure](../iscsi/iscsi-target-server.md).  

    - **Pour le stockage SAN FC :**  

        1.  Assurez-vous que chaque cluster peut voir uniquement les boîtiers de stockage de ce site et que vous avez correctement segmenté les hôtes.   

        2.  Configurez le stockage à l’aide de la documentation de votre fournisseur.  

    - **Pour le stockage sur disque fixe local :**  

        -   Assurez-vous que le stockage ne contient pas un volume système, un fichier d’échange ou des fichiers de vidage.  

        -   Configurez le stockage à l’aide de la documentation de votre fournisseur.  

9. Démarrez Windows PowerShell et utilisez l’applet de commande **Test-SRTopology** pour déterminer si toutes les conditions des réplicas de stockage sont satisfaites. Vous pouvez utiliser l’applet de commande dans un mode d’exigences uniquement pour un test rapide ainsi qu’un mode d’évaluation de performances à exécution longue.  

    Par exemple, pour valider les nœuds proposés qui contiennent chacun un volume **F:** et **G:** volume et exécuter le test pendant 30 minutes :  
        
    ```PowerShell
    MD c:\temp  

    Test-SRTopology -SourceComputerName SR-SRV05 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV06 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp  
    ```

    > [!IMPORTANT]
      > Quand vous utilisez un serveur de test sans aucune écriture de charge d’E/S sur le volume source spécifié pendant la période d’évaluation, envisagez d’ajouter une charge de travail, ou le rapport généré ne sera pas utile. Vous devez effectuer des tests avec des charges de travail de type production afin de voir des nombres réels et les tailles de journal recommandées. Vous pouvez également simplement copier certains fichiers dans le volume source pendant le test ou télécharger et exécuter [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223) pour générer des E/S d’écriture. Par exemple, un échantillon avec une faible charge de travail d’E/S d’écriture pendant dix minutes sur le volume D: :  
      >
      > `Diskspd.exe -c1g -d600 -W5 -C5 -b8k -t2 -o2 -r -w5 -i100 -j100 d:\test` 

10. Examinez le rapport **rapport testsrtopologyreport. html** illustré à la figure 2 pour vous assurer que vous répondez aux exigences du réplica de stockage.  

    ![Écran affichant le rapport de topologie](media/Server-to-Server-Storage-Replication/SRTestSRTopologyReport.png)

    **Figure 2 : Rapport de topologie de réplication du stockage**

## <a name="step-3-set-up-server-to-server-replication"></a>Étape 3 : Configurer la réplication de serveur à serveur
### <a name="using-windows-admin-center"></a>Utilisation du centre d’administration Windows

1. Ajoutez le serveur source.
    1. Sélectionnez le bouton **Ajouter** .
    2. Sélectionnez **Ajouter une connexion au serveur**.
    3. Tapez le nom du serveur, puis sélectionnez **Envoyer**.
2. Sur la page **toutes les connexions** , sélectionnez le serveur source.
3. Sélectionnez **réplica de stockage** dans le panneau Outils.
4. Sélectionnez **nouveau** pour créer un partenariat.
5. Fournissez les détails du partenariat, puis sélectionnez **créer**. <br>
   ![Nouvel écran de partenariat présentant les détails du partenariat, par exemple une taille de journal de 8 Go.](media/Storage-Replica-UI/Honolulu_SR_Create_Partnership.png)

    **Figure 3: Création d’un partenariat**

> [!NOTE]
> La suppression du partenariat du réplica de stockage dans le centre d’administration Windows ne supprime pas le nom du groupe de réplication.

### <a name="using-windows-powershell"></a>Utilisation de Windows PowerShell

Vous allez maintenant configurer la réplication de serveur à serveur à l’aide de Windows PowerShell. Vous devez effectuer toutes les étapes ci-dessous sur les nœuds directement ou à partir d’un ordinateur de gestion à distance qui contient le Outils d’administration de serveur distant de Windows Server.  

1. Veillez à utiliser une console Powershell avec élévation de privilèges en tant qu’administrateur.  
2. Configurez la réplication de serveur à serveur, en spécifiant les disques de la source et de la destination, les journaux de la source et de la destination, les nœuds de la source et de la destination, et la taille du journal.  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv05 -SourceRGName rg01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName sr-srv06 -DestinationRGName rg02 -DestinationVolumeName f: -DestinationLogVolumeName g:  
    ```  

   Sortie :
   ```PowerShell
   DestinationComputerName : SR-SRV06
   DestinationRGName       : rg02
   SourceComputerName      : SR-SRV05
   PSComputerName          :
   ```

    > [!IMPORTANT]
    > La taille du journal par défaut est de 8 Go. En fonction des résultats de l’applet de commande `Test-SRTopology`, vous pouvez décider d’utiliser -LogSizeInBytes avec une valeur supérieure ou inférieure.  

2.  Pour obtenir l’état de réplication source et de destination, utilisez `Get-SRGroup` et `Get-SRPartnership` comme suit :  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```
    Sortie :

    ```PowerShell
    CurrentLsn             : 0
    DataVolume             : F:\
    LastInSyncTime         :
    LastKnownPrimaryLsn    : 1
    LastOutOfSyncTime      :
    NumOfBytesRecovered    : 37731958784
    NumOfBytesRemaining    : 30851203072
    PartitionId            : c3999f10-dbc9-4a8e-8f9c-dd2ee6ef3e9f
    PartitionSize          : 68583161856
    ReplicationMode        : synchronous
    ReplicationStatus      : InitialBlockCopy
    PSComputerName         :
    ```

3.  Déterminez la progression de la réplication comme suit :  

    1.  Sur le serveur source, exécutez la commande suivante et examinez les événements 5015, 5002, 5004, 1237, 5001 et 2200:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  Sur le serveur de destination, exécutez la commande suivante pour afficher les événements de réplica de stockage qui indiquent la création du partenariat. Cet événement indique le nombre d’octets copiés et la durée de l’opération. Exemple :  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  
        ```
        Voici quelques exemples de sortie :
        ```
        TimeCreated  : 4/8/2016 4:12:37 PM  
        ProviderName : Microsoft-Windows-StorageReplica  
        Id           : 1215  
        Message      : Block copy completed for replica.  

        ReplicationGroupName: rg02  
        ReplicationGroupId: {616F1E00-5A68-4447-830F-B0B0EFBD359C}  
        ReplicaName: f:\  
        ReplicaId: {00000000-0000-0000-0000-000000000000}  
        End LSN in bitmap:   
        LogGeneration: {00000000-0000-0000-0000-000000000000}  
        LogFileId: 0  
        CLSFLsn: 0xFFFFFFFF  
        Number of Bytes Recovered: 68583161856  
        Elapsed Time (ms): 117  
        ```  

        > [!NOTE]
        > Le réplica de stockage démonte les volumes de destination et leurs points de montage ou lettres de lecteur. C’est normal.  

    3.  Alternativement, le groupe de serveurs de destination pour le réplica indique le nombre d’octets restants à copier à tout moment, et peut être obtenu via PowerShell. Exemple :  

        ```PowerShell  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        Un exemple de progression (qui ne s’interrompt pas) :  

        ```PowerShell  
        while($true) {  

         $v = (Get-SRGroup -Name "RG02").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

    4.  Sur le serveur de destination, exécutez la commande suivante et examinez les événements 5009, 1237, 5001, 5015, 5005 et 2200 pour comprendre la progression du traitement. Il ne doit y avoir aucun avertissement d’erreur dans cette séquence. Il y aura un grand nombre d’événements 1237; ils indiquent la progression.  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

## <a name="step-4-manage-replication"></a>Étape 4 : Gérer la réplication

Maintenant, vous allez gérer et faire fonctionner votre infrastructure répliquée de serveur à serveur. Vous pouvez effectuer toutes les étapes ci-dessous sur les nœuds directement ou à partir d’un ordinateur de gestion à distance qui contient le Outils d’administration de serveur distant de Windows Server.  

1.  Utilisez `Get-SRPartnership` et `Get-SRGroup` pour déterminer la source et la destination actuelles de la réplication et leur état.  

2.  Pour mesurer les performances de la réplication, utilisez l’applet de commande `Get-Counter` sur les nœuds source et de destination. Les noms des compteurs sont :  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Nombre de suspensions du vidage  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Nombre d’E/S de vidage en attente  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Nombre de demandes associées à la dernière écriture de journal  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Moy. Longueur de file d’attente de vidage  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Longueur de file d’attente de vidage actuelle  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Nombre de demandes d’écriture d’application  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Moy. Nombre de demandes par écriture de journal  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Moy. Latence d’écriture d’application  

    -   \Statistiques d’E/S du réplica du système de stockage(*)\Moy. Latence de lecture d’application  

    -   \Statistiques du réplica du système de stockage(*)\RPO cible  

    -   \Statistiques du réplica du système de stockage(*)\RPO actuel  

    -   \Statistiques du réplica du système de stockage(*)\Moy. Longueur de la file d’attente du journal  

    -   \Statistiques du réplica du système de stockage(*)\Longueur de file d’attente de journal actuelle  

    -   \Statistiques du réplica du système de stockage(*)\Total des octets reçus  

    -   \Statistiques du réplica du système de stockage(*)\Total des octets envoyés  

    -   \Statistiques du réplica du système de stockage(*)\Moy. Latence d’envoi du réseau  

    -   \Statistiques du réplica du système de stockage(*)\État de réplication  

    -   \Statistiques du réplica du système de stockage(*)\Moy. Latence de boucle de message  

    -   \Statistiques du réplica du système de stockage(*)\Temps écoulé pour la dernière récupération  

    -   \Statistiques du réplica du système de stockage(*)\Nombre de transactions de récupération vidées  

    -   \Statistiques du réplica du système de stockage(*)\Nombre de transactions de récupération  

    -   \Statistiques du réplica du système de stockage(*)\	Nombre de transactions de réplication vidées  

    -   \Statistiques du réplica du système de stockage(*)\	Nombre de transactions de réplication  

    -   \Statistiques du réplica du système de stockage(*)\Numéro séquentiel dans le journal max.  

    -   \Statistiques du réplica du système de stockage(*)\Nombre de messages reçus  

    -   \Statistiques du réplica du système de stockage(*)\Nombre de messages envoyés  

    Pour plus d’informations sur les compteurs de performances dans Windows PowerShell, consultez [Get-Counter](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Diagnostics/Get-Counter).  

3.  Pour déplacer la direction de réplication d’un site, utilisez l’applet de commande `Set-SRPartnership`.  

    ```PowerShell  
    Set-SRPartnership -NewSourceComputerName sr-srv06 -SourceRGName rg02 -DestinationComputerName sr-srv05 -DestinationRGName rg01  
    ```  

    > [!WARNING]  
    > Windows Server empêche le basculement de rôle lorsque la synchronisation initiale est en cours, car cela peut entraîner une perte de données si vous tentez de basculer avant d’autoriser la réplication initiale. Ne forcez pas les directions de basculement tant que la synchronisation initiale n’est pas terminée.  

    Vérifiez les journaux d’événements pour voir la direction du changement de réplication et la survenue du mode de récupération, puis rapprochez. Les E/S d’écriture peuvent alors écrire sur le stockage du nouveau serveur source. Modifier la direction de réplication bloquera les E/S d’écriture sur l’ordinateur source précédent.  

4.  Pour supprimer la réplication, utilisez `Get-SRGroup`, `Get-SRPartnership`, `Remove-SRGroup` et `Remove-SRPartnership` sur chaque nœud. Veillez à exécuter l’applet de commande `Remove-SRPartnership` sur la source actuelle de la réplication uniquement, pas sur le serveur de destination. Exécutez `Remove-Group` sur les deux serveurs. Par exemple, pour supprimer toute la réplication des deux serveurs :  

    ```powershell
    Get-SRPartnership  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

## <a name="replacing-dfs-replication-with-storage-replica"></a>Remplacement de la réplication DFS par le réplica de stockage  
De nombreux clients Microsoft déploient la réplication DFS en tant que solution de récupération d’urgence pour les données utilisateur non structurées telles que les dossiers de base et les partages de service. La réplication DFS est incluse dans Windows Server 2003 R2 et tous les systèmes d’exploitation ultérieurs. Elle fonctionne sur les réseaux à bande passante faible, ce qui la rend intéressante pour les environnements à latence élevée et peu sujets aux modifications dotés d’un grand nombre de nœuds. Toutefois, la réplication DFS a des limitations importantes comme une solution de réplication de données :  
* Il ne réplique pas les fichiers en cours d’utilisation ou ouverts.  
* Elle n’est pas répliquée de façon synchrone.  
* Sa latence de réplication asynchrone peut représenter un nombre élevé de minutes, heures, voire jours.  
* Elle s’appuie sur une base de données pouvant nécessiter de longues vérifications de cohérence après une coupure de courant.  
* Elle est généralement configurée en tant que multi-master, ce qui permet d’effectuer des modifications dans les deux sens, en remplaçant éventuellement les données les plus récentes.  

Le réplica de stockage ne présente aucune de ces limitations. Il en a, toutefois, plusieurs qui le rendent moins intéressant dans certains environnements :  

* Il autorise uniquement la réplication un à un entre les volumes. Il est possible de répliquer différents volumes entre plusieurs serveurs.  
* Bien qu’il prenne en charge la réplication asynchrone, il n’est pas conçu pour les réseaux à faible bande passante et à latence élevée.  
* Elle n’autorise pas l’accès des utilisateurs aux données protégées sur la destination pendant que la réplication est en cours.  

Si ces limitations ne sont pas rédhibitoires, le réplica de stockage vous permet de remplacer les serveurs de réplication DFS par cette technologie plus récente.   
Le processus est généralement le suivant :  

1. Installez Windows Server sur deux serveurs et configurez votre stockage. Cela peut impliquer la mise à niveau d’un ensemble existant de serveurs ou une nouvelle installation.  
2. Assurez-vous que toutes les données à répliquer existent sur un ou plusieurs volumes de données et non sur le lecteur C:.   
   a.  Vous pouvez également amorcer les données sur l’autre serveur pour gagner du temps, à l’aide d’une sauvegarde ou de copies de fichiers, ainsi qu’utiliser le stockage alloué dynamiquement. Faire correspondre parfaitement la sécurité de type métadonnées est inutile, contrairement à la réplication DFS.  
3. Partagez les données sur votre serveur source et rendez-les accessibles par le biais d’un espace de noms DFS. Ceci est important pour vous assurer que les utilisateurs peuvent toujours y accéder si le nom du serveur est remplacé par un nom situé dans un site d’incident.  
   a.  Vous pouvez créer des partages correspondants sur le serveur de destination, qui sont indisponibles en temps de fonctionnement normal.   
   b.  N’ajoutez pas le serveur de destination à l’espace de noms des espaces de noms DFS, ou si vous le faites, assurez-vous que toutes ses cibles de dossier sont désactivées.  
4. Activez la réplication du réplica de stockage et procédez à la synchronisation initiale. La réplication peut être synchrone ou asynchrone.   
   a.  Toutefois, une réplication synchrone est recommandée pour garantir la cohérence des données d’E/S sur le serveur de destination.   
   b.  Nous recommandons fortement l’activation de clichés instantanés de volume et des captures instantanées régulières avec VSSADMIN ou d’autres outils de votre choix. Ainsi, les applications vident bien leurs fichiers de données sur le disque de manière cohérente. En cas d’incident, vous pouvez récupérer des fichiers à partir des captures instantanées sur le serveur de destination qui peuvent avoir été partiellement répliquées de manière asynchrone. Les captures instantanées sont répliquées de concert avec les fichiers.  
5. Agissez normalement jusqu’à ce qu’un incident se produise.  
6. Faites basculer le serveur de destination pour qu’il devienne la nouvelle source, qui expose ses volumes répliqués aux utilisateurs.  
7. Si vous utilisez la réplication synchrone, aucune restauration des données n’est nécessaire, sauf si l’utilisateur utilisait une application qui écrivait des données sans protéger la transaction (quelle que soit la réplication) lors de la perte du serveur source. Si vous utilisez la réplication asynchrone, le besoin d’un montage de capture instantanée VSS est plus élevé, mais envisagez d’utiliser VSS dans tous les cas pour des captures instantanées cohérentes d’application.  
8. Ajoutez le serveur et ses partages en tant que cible de dossier d’espaces de noms DFS.   
9. Les utilisateurs peuvent ensuite accéder à leurs données.  

   > [!NOTE]
   > La planification de la récupération d’urgence est un sujet complexe qui nécessite de donner une attention particulière aux détails. La création de Runbook et les performances des exercices de basculement en direct annuels sont fortement recommandées. En cas d’incident réel, le chaos règne et le personnel expérimenté peut être en permanence indisponible.  

## <a name="add-azure-vm-expressroute"></a>Ajout d’une machine virtuelle Azure connectée à votre réseau via ExpressRoute

1. [Créez un ExpressRoute dans le portail Azure](https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager).<br>Une fois le ExpressRoute approuvé, un groupe de ressources est ajouté à l’abonnement. Accédez à **groupes de ressources** pour afficher ce nouveau groupe. Notez le nom du réseau virtuel.
![Portail Azure de l’indication du groupe de ressources ajouté avec ExpressRoute](media/Server-to-Server-Storage-Replication/express-route-resource-group.png)
    
    **Figure 4 : Ressources associées à un ExpressRoute-prenez note du nom du réseau virtuel**
1. [Créez un groupe de ressources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).
1. [Ajoutez un groupe de sécurité réseau](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal). Lors de sa création, sélectionnez l’ID d’abonnement associé au ExpressRoute que vous avez créé, puis sélectionnez le groupe de ressources que vous venez de créer.
<br><br>Ajoutez les règles de sécurité entrantes et sortantes dont vous avez besoin au groupe de sécurité réseau. Par exemple, vous souhaiterez peut-être autoriser Bureau à distance accès à la machine virtuelle.
1. [Créez une machine virtuelle Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) avec les paramètres suivants (illustrée à la figure 5) :
    - **Adresse IP publique** : Aucun
    - **Réseau virtuel**: Sélectionnez le réseau virtuel que vous avez noté dans le groupe de ressources ajouté avec ExpressRoute.
    - **Groupe de sécurité réseau (pare-feu)** : Sélectionnez le groupe de sécurité réseau que vous avez créé précédemment.
    ![Créer une machine virtuelle présentant les paramètres](media/Server-to-Server-Storage-Replication/azure-vm-express-route.png)
    **réseau ExpressRoute figure 5 : Création d’une machine virtuelle lors de la sélection des paramètres réseau ExpressRoute**
1. Une fois la machine virtuelle créée, [consultez étape 2 : Approvisionner le système d’exploitation, les fonctionnalités, les rôles](#provision-os), le stockage et le réseau.


## <a name="related-topics"></a>Rubriques connexes  
- [Vue d’ensemble du réplica de stockage](storage-replica-overview.md)  
- [Réplication de cluster étendu à l’aide d’un stockage partagé](stretch-cluster-replication-using-shared-storage.md)  
- [Réplication de stockage de cluster à cluster](cluster-to-cluster-storage-replication.md)
- [Réplica de stockage : Problèmes connus](storage-replica-known-issues.md)  
- [Réplica de stockage : Forum Aux Questions](storage-replica-frequently-asked-questions.md)
- [espaces de stockage direct dans Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
