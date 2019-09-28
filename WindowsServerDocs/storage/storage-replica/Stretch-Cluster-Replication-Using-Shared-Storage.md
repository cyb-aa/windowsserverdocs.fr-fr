---
title: Réplication de cluster étendu à l’aide d’un stockage partagé
ms.prod: windows-server
manager: eldenc
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 6c5b9431-ede3-4438-8cf5-a0091a8633b0
ms.openlocfilehash: 654b4aea135c360f5fc5f59fdf85627fe8dd4cc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402969"
---
# <a name="stretch-cluster-replication-using-shared-storage"></a>Réplication de cluster étendu à l’aide d’un stockage partagé

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)

Dans cet exemple d’évaluation, vous allez configurer ces ordinateurs et leur stockage dans un seul cluster étendu, où deux nœuds partagent un ensemble de stockage et deux nœuds en partagent un autre. La réplication garde ensuite les deux ensembles de stockage en miroir dans le cluster pour permettre un basculement immédiat. Ces nœuds et leur stockage doivent se trouver sur des sites physiques distincts, même si cela n’est pas obligatoire. Les procédures sont distinctes pour créer des clusters Hyper-V et de serveur de fichiers en tant qu’exemples de scénarios.  

> [!IMPORTANT]  
> Dans cette évaluation, les serveurs de différents sites doivent être en mesure de communiquer avec les autres serveurs via un réseau, mais sans avoir de connectivité physique au stockage partagé de l’autre site. Ce scénario n’utilise pas les espaces de stockage direct.  

## <a name="terms"></a>Termes  
Cette procédure pas à pas utilise l’environnement suivant comme exemple :  

-   Quatre serveurs, nommés **SR-SRV01**, **SR-SRV02**, **SR-SRV03** et **SR-SRV04** constituant un seul cluster appelé **SR-SRVCLUS**.  

-   Une paire de sites logiques qui représentent deux centres de données différents, l’un étant nommé **Redmond** et l’autre **Bellevue**.  

> [!NOTE]  
> Vous pouvez utiliser seulement deux nœuds, avec un nœud dans chaque site. Toutefois, vous ne serez pas en mesure d’effectuer le basculement intrasite avec seulement deux serveurs. Vous pouvez utiliser jusqu’à 64nœuds.

![Diagramme montrant deux nœuds à Redmond se répliquant avec deux nœuds du même cluster sur le site de Bellevue](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_StretchClusterExample.png)  

@NO__T 0FIGURE 1 :  Réplication du stockage dans un cluster Stretch @ no__t-0  

## <a name="prerequisites"></a>Prérequis  
-   Forêt de services de domaine Active Directory (exécution de Windows Server 2016 non nécessaire).  
-   2-64 serveurs exécutant Windows Server 2019 ou Windows Server 2016 Datacenter Edition. Si vous exécutez Windows Server 2019, vous pouvez à la place utiliser l’édition standard si vous ne répliquez qu’un seul volume, avec une taille maximale de 2 to. 
-   Deux ensembles de stockage partagé avec JBOD SAS (comme pour les espaces de stockage), SAN Fibre Channel, VHDX partagé ou cible iSCSI. Le stockage doit contenir un mélange de disques SSD et HDD, et doit prendre en charge la réservation persistante. Vous mettrez chaque ensemble de stockage à la disposition de deux des serveurs uniquement (asymétrique).  
-   Chaque ensemble de stockage doit autoriser la création d’au moins deux disques virtuels, un pour les données répliquées et un autre pour les journaux. Le stockage physique doit avoir la même taille de secteur sur tous les disques de données. Le stockage physique doit avoir la même taille de secteur sur tous les disques de journal.  
-   Au moins une connexion 1GbE sur chaque serveur pour la réplication synchrone, mais de préférence RDMA.   
-   Au moins 2Go de RAM et deux cœurs par serveur. Vous aurez besoin de plus de mémoire et de cœurs pour d’autres machines virtuelles.  
-   Les règles de pare-feu et de routeur appropriés pour autoriser le trafic bidirectionnel ICMP, SMB (port 445, ainsi que 5445 pour SMB Direct) et WS-MAN (port 5985) entre tous les nœuds.  
-   Un réseau entre les serveurs avec une bande passante suffisante pour contenir votre charge de travail d’écriture d’E/S et une latence moyenne de parcours circulaire égale à 5 ms pour la réplication synchrone. La réplication asynchrone ne présente pas de recommandation en matière de latence.  
-   Impossible de localiser le stockage répliqué sur le lecteur contenant le dossier du système d’exploitation Windows.

La plupart de ces exigences peuvent être déterminées à l’aide de l’applet de commande `Test-SRTopology`. Vous pouvez accéder à cet outil si vous installez un réplica de stockage ou les fonctionnalités des outils de gestion des réplicas de stockage sur au moins un serveur. Il n’est pas nécessaire de configurer de réplica de stockage pour utiliser cet outil, mais vous devez installer l’applet de commande. Vous trouverez plus d’informations dans la procédure suivante.  

## <a name="provision-operating-system-features-roles-storage-and-network"></a>Configurer le système d’exploitation, les fonctionnalités, les rôles, le stockage et le réseau  

1.  Installez Windows Server sur tous les nœuds de serveur, à l’aide des options d’installation Server Core ou Server with Desktop Experience.  
    > [!IMPORTANT]
    > À partir de ce stade, connectez-vous toujours en tant qu’utilisateur de domaine membre du groupe Administrateurs intégré sur tous les serveurs. Pensez toujours à élever vos invites CMD et PowerShell à l’avenir lors de l’exécution sur une installation de serveur graphique ou sur un ordinateur Windows10.

2.  Ajoutez des informations réseau et joignez les nœuds au domaine, puis redémarrez-les.  
    > [!NOTE]
    > À ce stade, le guide suppose que vous avez deux paires de serveurs à utiliser dans un cluster étendu. Un réseau LAN ou WAN sépare les serveurs et les serveurs appartiennent à des sites physiques ou logiques. Le guide considère que **SR-SRV01** et **SR-SRV02** se trouvent dans le site Redmond et que **SR-SRV03** et **SR-SRV04** se trouvent dans le site **Bellevue**.  

3.  Connectez le premier ensemble de boîtier de stockage JBOD partagé, VHDX partagé, cible iSCSI ou SAN Fibre Channel aux serveurs du site **Redmond**.  

4.  Connectez le deuxième ensemble de stockage aux serveurs du site **Bellevue**.  

5.  Installez si nécessaire les derniers pilotes et microprogrammes de stockage et de boîtier du fournisseur, les derniers pilotes du fournisseur HBA, le dernier microprogramme du fournisseur de BIOS/UEFI, les derniers pilotes réseau du fournisseur et les derniers pilotes de carte mère sur les quatre nœuds. Redémarrez les nœuds si besoin.  

    > [!NOTE]
    > Consultez la documentation de votre fournisseur de matériel pour la configuration du stockage partagé et du matériel réseau.  

6.  Vérifiez que les paramètres BIOS/UEFI des serveurs permettent de hautes performances, par exemple avec la désactivation de C-State, la définition de la vitesse de QPI, l’activation de NUMA et la définition de la fréquence mémoire la plus élevée. Vérifiez que la gestion de l’alimentation dans Windows Server est définie pour favoriser de hautes performances. Redémarrez si nécessaire.  

7.  Configurez les rôles comme suit :  

    -   **Méthode graphique**  

        Exécutez **ServerManager.exe** et ajoutez tous les nœuds de serveur en cliquant sur **Gérer** et **Ajouter des serveurs**.  

        > [!IMPORTANT]
        > Installez les rôles et fonctionnalités **Clustering de basculement** et **Réplica de stockage** sur chacun des nœuds et redémarrez-les. Si vous envisagez d’utiliser d’autres rôles tels que Hyper-V, Serveur de fichiers, etc., vous pouvez aussi les installer maintenant.  

    -   **Utilisation de la méthode Windows PowerShell**  

        Sur **SR-SRV04** ou un ordinateur de gestion à distance, exécutez la commande suivante dans une console Windows PowerShell pour installer les fonctionnalités et rôles requis pour un cluster étendu sur les quatre nœuds et redémarrez-les:  

        ```PowerShell  
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'  

        $Servers | foreach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }  

        ```  

        Pour plus d’informations sur ces étapes, voir [Installer ou désinstaller des rôles, services de rôle ou fonctionnalités](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md).  


8. Configurez le stockage comme suit :  

    > [!IMPORTANT]  
    > -   Vous devez créer deux volumes sur chacun des boîtiers : un pour les données et l’autre pour les journaux.  
    > -   Les disques de données et de journaux doivent être initialisés en tant que GPT, et non MBR.  
    > -   Les deux volumes de données doivent être de la même taille.  
    > -   Les deux volumes de journaux doivent être de la même taille.  
    > -   Tous les disques de données répliqués doivent avoir la même taille de secteur.  
    > -   Tous les disques de journaux doivent avoir la même taille de secteur.  
    > -   Les volumes de journaux doivent utiliser un stockage flash et des paramètres de résilience hautes performances. Microsoft recommande que le stockage de journaux soit aussi rapide que le stockage de données. Les volumes de journaux ne doivent jamais être utilisés pour d’autres charges de travail. 
    > -   Les disques de données peuvent utiliser des disques HDD, des disques SSD ou une combinaison hiérarchisée et utiliser des espaces de parité ou en miroir, ou RAID1 ou10, ou RAID5 ou RAID50.  
    > -  Le journal doit avoir une taille par défaut de 9Go, qui peut être supérieure ou inférieure selon la configuration requise.  
    > - Les volumes doivent être formatés en NTFS ou ReFS.
    > - Le rôle serveur de fichiers est uniquement nécessaire pour que Test-SRTopology fonctionne, car elle ouvre les ports de pare-feu nécessaires au test.  

    -   **Pour les boîtiers JBOD :**  

        1.  Vérifiez que chaque ensemble de nœuds de serveur associés peut voir uniquement les boîtiers de stockage de ce site (autrement dit, un stockage asymétrique) et que les connexions SAS sont correctement configurées.  

        2.  Configurez le stockage à l’aide d’espaces de stockage en suivant les **étapes1 à3** indiquées dans [Déployer des espaces de stockage sur un serveur autonome](../storage-spaces/deploy-standalone-storage-spaces.md) à l’aide de Windows PowerShell ou du Gestionnaire de serveur.  

    -   **Pour le stockage iSCSI :**  

        1.  Vérifiez que chaque ensemble de nœuds de serveur associés peut voir uniquement les boîtiers de stockage de ce site (autrement dit, un stockage asymétrique). Vous devez utiliser plusieurs cartes réseau si vous utilisez iSCSI.  

        2.  Configurez le stockage à l’aide de la documentation de votre fournisseur. Si vous utilisez le ciblage iSCSI basé sur Windows, voir [Stockage par blocs de cibles iSCSI, procédure](../iscsi/iscsi-target-server.md).  

    -   **Pour le stockage SAN FC :**  

        1.  Vérifiez que chaque ensemble de nœuds de serveur associés peut voir uniquement les boîtiers de stockage de ce site (autrement dit, un stockage asymétrique) et que vous avez correctement segmenté les hôtes.  

        2.  Configurez le stockage à l’aide de la documentation de votre fournisseur.  

## <a name="configure-a-hyper-v-failover-cluster-or-a-file-server-for-a-general-use-cluster"></a>Configurer un cluster de basculement Hyper-V ou un serveur de fichiers pour un cluster d’utilisation générale

Après avoir configuré vos nœuds de serveur, l’étape suivante consiste à créer l’un des types suivants de clusters :  
*  [Cluster de basculement Hyper-V](#BKMK_HyperV)  
*  [Serveur de fichiers pour un cluster d’utilisation générale](#BKMK_FileServer)  

### <a name="BKMK_HyperV"></a>Configurer un cluster de basculement Hyper-V  

>[!NOTE]
> Ignorez cette section et passez à la section [Configurer un cluster Serveur de fichiers pour une utilisation générale](#BKMK_FileServer) si vous souhaitez créer un cluster de serveurs de fichiers et non un cluster Hyper-V.  

Vous allez maintenant créer un cluster de basculement normal. Après la configuration, la validation et le test, vous allez l’étirer à l’aide d’un réplica de stockage. Vous pouvez effectuer toutes les étapes ci-dessous sur les nœuds de cluster directement ou à partir d’un ordinateur de gestion à distance qui contient le Outils d’administration de serveur distant de Windows Server.  

#### <a name="graphical-method"></a>Méthode graphique  

1. Exécutez **cluadmin.msc**.  

2. Validez le cluster proposé et analysez les résultats pour vérifier que vous pouvez continuer.  

   > [!NOTE]  
   > Vous devez vous attendre à des erreurs de stockage suite à la validation du cluster, en raison de l’utilisation du stockage asymétrique.  

3. Créez le cluster de calcul Hyper-V. Vérifiez que le nom du cluster comporte au maximum 15 caractères. L’exemple utilisé ci-dessous est SR-SRVCLUS. Si les nœuds se trouvent dans des sous-réseaux différents, vous devez créer une adresse IP pour le nom de cluster pour chaque sous-réseau et utiliser la dépendance « ou ».  Pour plus d’informations, consultez [Configuration des adresses IP et des dépendances pour les clusters de sous-réseaux multiples – partie III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).  

4. Configurez un témoin de partage de fichiers ou un témoin de cloud pour fournir un quorum en cas de perte du site.  

   > [!NOTE]  
   > WIndows Server comprend désormais une option pour le témoin Cloud (Azure). Vous pouvez choisir cette option de quorum au lieu du témoin de partage de fichiers.  

   > [!WARNING]  
   > Pour plus d’informations sur la configuration de quorum, voir [Configurer et gérer le quorum dans un cluster de basculement Windows Server2012](https://technet.microsoft.com/library/jj612870.aspx). Pour plus d’informations sur l’applet de commande `Set-ClusterQuorum`, consultez la page [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).  

5. Passez en revue [Recommandations de réseau pour un cluster Hyper-V dans Windows Server 2012](https://technet.microsoft.com/library/dn550728.aspx) et vérifiez que vous avez configuré de façon optimale la mise en réseau de cluster.  

6. Ajoutez un disque dans le site Redmond au volume partagé de cluster. Pour ce faire, cliquez avec le bouton droit sur un disque source dans le nœud **Disques** de la section **Stockage**, puis cliquez sur **Ajouter aux volumes partagés de cluster**.  

7. En utilisant le guide [Déployer un cluster Hyper-V](https://technet.microsoft.com/library/jj863389.aspx), suivez les étapes 7 à 10 dans le site **Redmond** pour créer une machine virtuelle de test uniquement pour vérifier que le cluster fonctionne normalement dans les deux nœuds partageant le stockage dans le premier site de test.  

8. Si vous créez un cluster étendu à deux nœuds, vous devez ajouter tout le stockage avant de continuer. Pour ce faire, ouvrez une session PowerShell avec des autorisations administratives sur les nœuds de cluster et exécutez la commande suivante : `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Il s’agit d’un comportement lié à la conception dans Windows Server2016.

9. Démarrez Windows PowerShell et utilisez l’applet de commande `Test-SRTopology` pour déterminer si vous répondez à toutes les conditions des réplicas de stockage.  

    Par exemple, pour valider deux des nœuds de cluster étendu proposés ayant chacun un volume **D:** et **E:** , et exécuter le test pendant 30 minutes :
   1. Déplacez la totalité du stockage disponible sur **SR-SRV01**.
   2. Cliquez sur **Créer un rôle vide** dans la section **Rôles** du Gestionnaire du cluster de basculement.
   3. Ajoutez le stockage en ligne à ce rôle vide nommé **Nouveau rôle**.
   4. Déplacez la totalité du stockage disponible sur **SR-SRV03**.
   5. Cliquez sur **Créer un rôle vide** dans la section **Rôles** du Gestionnaire du cluster de basculement.
   6. Déplacez le **Nouveau rôle (2)** vide sur **SR-SRV03**.
   7. Ajoutez le stockage en ligne à ce rôle vide nommé **Nouveau rôle (2)** .
   8. Vous avez maintenant monté tout votre stockage avec des lettres de lecteur et pouvez évaluer le cluster avec `Test-SRTopology`.

       Exemple :

           MD c:\temp  

           Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName D: -SourceLogVolumeName E: -DestinationComputerName SR-SRV03 -DestinationVolumeName D: -DestinationLogVolumeName E: -DurationInMinutes 30 -ResultPath c:\temp        

      > [!IMPORTANT]
      > Quand vous utilisez un serveur de test sans aucune charge d’E/S d’écriture sur le volume source spécifié pendant la période d’évaluation, envisagez d’ajouter une charge de travail ou Test-SRTopology ne génère pas de rapport utile. Vous devez effectuer des tests avec des charges de travail de type production afin de voir des nombres réels et les tailles de journal recommandées. Vous pouvez également simplement copier certains fichiers dans le volume source pendant le test ou télécharger et exécuter DISKSPD pour générer des E/S d’écriture. Par exemple, un échantillon avec une faible charge de travail d’E/S d’écriture pendant dix minutes sur le volume D: :   
       `Diskspd.exe -c1g -d600 -W5 -C5 -b4k -t2 -o2 -r -w5 -i100 d:\test.dat`  

10. Examinez le rapport **TestSrTopologyReport-&lt;date&gt;.html** pour vérifier que vous répondez aux conditions des réplicas de stockage et pour noter les recommandations en matière de prévision de l’heure de synchronisation initiale.  

      ![Écran montrant le rapport de réplication](./media/Stretch-Cluster-Replication-Using-Shared-Storage/SRTestSRTopologyReport.png)

11. Renvoyez les disques au stockage disponible et supprimez les rôles vides temporaires.

12. Une fois que vous êtes satisfait, supprimez la machine virtuelle de test. Ajoutez toutes les machines virtuelles de test réel nécessaires pour une évaluation supplémentaire sur un nœud source proposé.  

13. Configurez la reconnaissance des sites de cluster étendu pour que les serveurs **SR-SRV01** et **SR-SRV02** figurent dans le site **Redmond**, **SR-SRV03** et **SR-SRV04** dans le site **Bellevue**, et que **Redmond** soit préféré pour la propriété des nœuds du stockage source et des machines virtuelles:  

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  
   
    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  
   
    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"
    ```

    > [!NOTE]
    > Il n’est pas possible de configurer la reconnaissance des sites à l’aide du Gestionnaire du cluster de basculement dans Windows Server2016.  

14. **(Facultatif)** Configurez la mise en réseau de cluster et Active Directory pour un basculement plus rapide du site DNS. Vous pouvez utiliser la mise en réseau SDN (Software-Defined Networking) Hyper-V, des réseaux VLAN étirés, des périphériques d’abstraction de réseau, le TTL du DNS abaissé et d’autres techniques courantes.

    Pour plus d’informations, consultez la session Microsoft de l’allumage : L' [étirement des clusters de basculement et l’utilisation du réplica de stockage dans Windows Server vNext](http://channel9.msdn.com/Events/Ignite/2015/BRK3487) et le billet de blog [activer les notifications de modifications entre les sites-comment et pourquoi ?](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx) .  

15. **(Facultatif)** Configurez la résilience d’ordinateur virtuel afin que les invités ne soient pas en pause longtemps pendant les défaillances de nœud. Au lieu de cela, ils basculent vers le nouveau stockage source de réplication dans les 10 secondes.  

    ```PowerShell  
    (Get-Cluster).ResiliencyDefaultPeriod=10  
    ```  

    > [!NOTE]
    >  Il n’est pas possible de configurer la résilience des machines virtuelles à l’aide du Gestionnaire du cluster de basculement dans Windows Server 2016.  

#### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  

1. Testez le cluster proposé et analysez les résultats pour vous assurer que vous pouvez continuer:  

   ```PowerShell  
   Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04  
   ```  

   > [!NOTE]
   >  Vous devez vous attendre à des erreurs de stockage suite à la validation du cluster, en raison de l’utilisation du stockage asymétrique.  

2. Créez le serveur de fichiers pour le cluster de stockage à usage général (vous devez spécifier votre propre adresse IP statique que le cluster utilisera). Vérifiez que le nom du cluster comporte au maximum 15 caractères.  Si les nœuds se trouvent dans des sous-réseaux différents, vous devez créer une adresse IP pour le site supplémentaire à l’aide de la dépendance « ou ». Pour plus d’informations, consultez [Configuration des adresses IP et des dépendances pour les clusters de sous-réseaux multiples – partie III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).
   ```PowerShell  
   New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here>  
   Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”
   Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
   ```  

3. Configurez un témoin de partage de fichiers ou un témoin de cloud (Azure) dans le cluster qui pointe vers un partage hébergé sur le contrôleur de domaine ou un autre serveur indépendant. Exemple :  

   ```PowerShell  
   Set-ClusterQuorum -FileShareWitness \\someserver\someshare  
   ```  

   > [!NOTE]
   > WIndows Server comprend désormais une option pour le témoin Cloud (Azure). Vous pouvez choisir cette option de quorum au lieu du témoin de partage de fichiers.  
    
   Pour plus d’informations sur la configuration de quorum, voir [Configurer et gérer le quorum dans un cluster de basculement Windows Server2012](https://technet.microsoft.com/library/jj612870.aspx). Pour plus d’informations sur l’applet de commande `Set-ClusterQuorum`, consultez la page [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).  

4. Passez en revue [Recommandations de réseau pour un cluster Hyper-V dans Windows Server 2012](https://technet.microsoft.com/library/dn550728.aspx) et vérifiez que vous avez configuré de façon optimale la mise en réseau de cluster.  

5. Si vous créez un cluster étendu à deux nœuds, vous devez ajouter tout le stockage avant de continuer. Pour ce faire, ouvrez une session PowerShell avec des autorisations administratives sur les nœuds de cluster et exécutez la commande suivante : `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Il s’agit d’un comportement lié à la conception dans Windows Server2016.

6. En utilisant le guide [Déployer un cluster Hyper-V](https://technet.microsoft.com/library/jj863389.aspx), suivez les étapes 7 à 10 dans le site **Redmond** pour créer une machine virtuelle de test uniquement pour vérifier que le cluster fonctionne normalement dans les deux nœuds partageant le stockage dans le premier site de test.  

7. Une fois que vous êtes satisfait, supprimez la machine virtuelle de test. Ajoutez toutes les machines virtuelles de test réel nécessaires pour une évaluation supplémentaire sur un nœud source proposé.  

8. Configurez la reconnaissance des sites de cluster étendu pour que les serveurs **SR-SRV01** et **SR-SRV02** figurent dans le site **Redmond**, **SR-SRV03** et **SR-SRV04** dans le site **Bellevue**, et que **Redmond** soit préféré pour la propriété des nœuds du stockage source et des machines virtuelles:  

   ```PowerShell  
   New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

   New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

   Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
   Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
   Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
   Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

   (Get-Cluster).PreferredSite="Seattle"  
   ```  

9. **(Facultatif)** Configurez la mise en réseau de cluster et Active Directory pour un basculement plus rapide du site DNS. Vous pouvez utiliser la mise en réseau SDN (Software-Defined Networking) Hyper-V, des réseaux VLAN étirés, des périphériques d’abstraction de réseau, le TTL du DNS abaissé et d’autres techniques courantes.  

   Pour plus d’informations, consultez la session Microsoft de l’allumage : [Étirement des clusters de basculement et utilisation du réplica de stockage dans Windows Server vNext](http://channel9.msdn.com/Events/Ignite/2015/BRK3487) et [activation des notifications de changement entre les sites-comment et pourquoi](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx).  

10. **(Facultatif)** Configurez la résilience de machine virtuelle afin que les invités ne soient pas en pause longtemps pendant les défaillances de nœud. Au lieu de cela, ils basculent vers le nouveau stockage source de réplication dans les 10 secondes.  

    ```PowerShell  
    (Get-Cluster).ResiliencyDefaultPeriod=10  
    ```  

    > [!NOTE]  
    > Le Gestionnaire du cluster de basculement de Windows Server2016 n’offre pas d’option de résilience de machines virtuelles.  



### <a name="BKMK_FileServer"></a>Configurer un serveur de fichiers pour un cluster d’utilisation générale  

>[!NOTE]
> Ignorez cette section si vous avez déjà configuré un cluster de basculement Hyper-V comme décrit dans [Configurer un cluster de basculement Hyper-V](#BKMK_HyperV).  

Vous allez maintenant créer un cluster de basculement normal. Après la configuration, la validation et le test, vous allez l’étirer à l’aide d’un réplica de stockage. Vous pouvez effectuer toutes les étapes ci-dessous sur les nœuds de cluster directement ou à partir d’un ordinateur de gestion à distance qui contient le Outils d’administration de serveur distant de Windows Server.  

#### <a name="graphical-method"></a>Méthode graphique  

1. Exécutez cluadmin.msc.  

2. Validez le cluster proposé et analysez les résultats pour vérifier que vous pouvez continuer.  
   >[!NOTE]
   >Vous devez vous attendre à des erreurs de stockage suite à la validation du cluster, en raison de l’utilisation du stockage asymétrique.   
3. Créez le cluster de stockage Serveur de fichiers pour une utilisation générale. Vérifiez que le nom du cluster comporte au maximum 15 caractères. L’exemple utilisé ci-dessous est SR-SRVCLUS.  Si les nœuds se trouvent dans des sous-réseaux différents, vous devez créer une adresse IP pour le nom de cluster pour chaque sous-réseau et utiliser la dépendance « ou ».  Pour plus d’informations, consultez [Configuration des adresses IP et des dépendances pour les clusters de sous-réseaux multiples – partie III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).  

4. Configurez un témoin de partage de fichiers ou un témoin de cloud pour fournir un quorum en cas de perte du site.  
   >[!NOTE]
   > WIndows Server comprend désormais une option pour le témoin Cloud (Azure). Vous pouvez choisir cette option de quorum au lieu du témoin de partage de fichiers.                                                                                                                                                                             
   >[!NOTE]
   >  Pour plus d’informations sur la configuration de quorum, voir [Configurer et gérer le quorum dans un cluster de basculement Windows Server2012](https://technet.microsoft.com/library/jj612870.aspx). Pour plus d’informations sur l’applet de commande Set-ClusterQuorum, voir [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum). 

5. Si vous créez un cluster étendu à deux nœuds, vous devez ajouter tout le stockage avant de continuer. Pour ce faire, ouvrez une session PowerShell avec des autorisations administratives sur les nœuds de cluster et exécutez la commande suivante : `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

   Il s’agit d’un comportement lié à la conception dans Windows Server2016.

6. Vérifiez que vous avez configuré de façon optimale la mise en réseau de cluster.  
    >[!NOTE]
    > Le rôle de serveur de fichiers doit être installé sur tous les nœuds avant de passer à l’étape suivante.   |  

7. Sous **Rôles**, cliquez sur **Configurer un rôle**. Passez en revue la section **Avant de commencer** et cliquez sur **Suivant**.  

8. Sélectionnez **Serveur de fichiers**, puis cliquez sur **Suivant**.  

9. Laissez l’option **Serveur de fichiers pour une utilisation générale** sélectionnée et cliquez sur **Suivant**.  

10. Indiquez un nom **Point d’accès client** (15 caractères au maximum) et cliquez sur **Suivant**.  

11. Sélectionnez un disque comme étant votre volume de données et cliquez sur **Suivant**.  

12. Vérifiez vos paramètres, puis cliquez sur **Suivant**. Cliquez sur **Terminer**.  

13. Cliquez avec le bouton droit sur le nouveau rôle de serveur de fichiers, puis cliquez sur **Ajouter le partage de fichiers**. Exécutez l’Assistant pour configurer les partages.  

14. Facultatif : Ajoutez un autre rôle de serveur de fichiers qui utilise l’autre stockage dans ce site.  

15. Configurez la reconnaissance des sites de cluster étendu pour que les serveurs SR-SRV01 et SR-SRV02 figurent dans le site Redmond, SR-SRV03 et SR-SRV04 dans le site Bellevue, et que Redmond soit préféré pour la propriété des nœuds du stockage source et des machines virtuelles :  

    ```PowerShell  
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"  
    ```  

      >[!NOTE]
      > Il n’est pas possible de configurer la reconnaissance des sites à l’aide du Gestionnaire du cluster de basculement dans Windows Server2016.  

16. (Facultatif) Configurez la mise en réseau de cluster et Active Directory pour un basculement plus rapide du site DNS. Vous pouvez utiliser des réseaux VLAN étirés, des périphériques d’abstraction de réseau, le TTL du DNS abaissé et d’autres techniques courantes.  

Pour plus d’informations, consultez la session Microsoft Ignite : [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](http://channel9.msdn.com/events/ignite/2015/brk3487) (Extension des clusters de basculement et utilisation de réplicas de stockage dans Windows Server vNext) et le billet de blog [Enable Change Notifications between Sites - How and Why?](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx) (Permettre les notifications de modification entre les sites : comment et pourquoi ?).    

#### <a name="powershell-method"></a>Méthode PowerShell

1. Testez le cluster proposé et analysez les résultats pour vous assurer que vous pouvez continuer:    

    ```PowerShell
    Test-Cluster SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04
    ```

    > [!NOTE]
    >  Vous devez vous attendre à des erreurs de stockage suite à la validation du cluster, en raison de l’utilisation du stockage asymétrique.   

2.  Créez le cluster de calcul Hyper-V (vous devez spécifier votre propre adresseIP statique que le cluster utilisera). Vérifiez que le nom du cluster comporte au maximum 15 caractères.  Si les nœuds se trouvent dans des sous-réseaux différents, vous devez créer une adresse IP pour le site supplémentaire à l’aide de la dépendance « ou ». Pour plus d’informations, consultez [Configuration des adresses IP et des dépendances pour les clusters de sous-réseaux multiples – partie III](https://techcommunity.microsoft.com/t5/Failover-Clustering/Configuring-IP-Addresses-and-Dependencies-for-Multi-Subnet/ba-p/371698).  

    ```PowerShell
    New-Cluster -Name SR-SRVCLUS -Node SR-SRV01, SR-SRV02, SR-SRV03, SR-SRV04 -StaticAddress <your IP here> 

    Add-ClusterResource -Name NewIPAddress -ResourceType “IP Address” -Group “Cluster Group”

    Set-ClusterResourceDependency -Resource “Cluster Name” -Dependency “[Cluster IP Address] or [NewIPAddress]”
    ```


3. Configurez un témoin de partage de fichiers ou un témoin de cloud (Azure) dans le cluster qui pointe vers un partage hébergé sur le contrôleur de domaine ou un autre serveur indépendant. Exemple :  

    ```PowerShell
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare
    ```

    >[!NOTE]
    > Windows Server comprend désormais une option pour le témoin Cloud à l’aide d’Azure. Vous pouvez choisir cette option de quorum au lieu du témoin de partage de fichiers.  

   Pour plus d’informations sur la configuration du quorum, consultez [fonctionnement du cluster et du quorum de pool](../storage-spaces/understand-quorum.md). Pour plus d’informations sur l’applet de commande Set-ClusterQuorum, voir [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum).

4.  Si vous créez un cluster étendu à deux nœuds, vous devez ajouter tout le stockage avant de continuer. Pour ce faire, ouvrez une session PowerShell avec des autorisations administratives sur les nœuds de cluster et exécutez la commande suivante : `Get-ClusterAvailableDisk -All | Add-ClusterDisk`.

    Il s’agit d’un comportement lié à la conception dans Windows Server2016.

5. Vérifiez que vous avez configuré de façon optimale la mise en réseau de cluster.  

6.  Configurez un rôle de serveur de fichiers. Exemple :

    ```PowerShell  
    Get-ClusterResource  
    Add-ClusterFileServerRole -Name SR-CLU-FS2 -Storage "Cluster Disk 4"  

    MD e:\share01  

    New-SmbShare -Name Share01 -Path f:\share01 -ContinuouslyAvailable $false  
    ```

7. Configurez la reconnaissance des sites de cluster étendu pour que les serveurs SR-SRV01 et SR-SRV02 figurent dans le site Redmond, SR-SRV03 et SR-SRV04 dans le site Bellevue, et que Redmond soit préféré pour la propriété des nœuds du stockage source et des machines virtuelles :  

    ```PowerShell
    New-ClusterFaultDomain -Name Seattle -Type Site -Description "Primary" -Location "Seattle Datacenter"  

    New-ClusterFaultDomain -Name Bellevue -Type Site -Description "Secondary" -Location "Bellevue Datacenter"  

    Set-ClusterFaultDomain -Name sr-srv01 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv02 -Parent Seattle  
    Set-ClusterFaultDomain -Name sr-srv03 -Parent Bellevue  
    Set-ClusterFaultDomain -Name sr-srv04 -Parent Bellevue  

    (Get-Cluster).PreferredSite="Seattle"  
    ```

8.  (Facultatif) Configurez la mise en réseau de cluster et Active Directory pour un basculement plus rapide du site DNS. Vous pouvez utiliser des réseaux VLAN étirés, des périphériques d’abstraction de réseau, le TTL du DNS abaissé et d’autres techniques courantes.  
    
    Pour plus d’informations, consultez la session Microsoft Ignite : [Stretching Failover Clusters and Using Storage Replica in Windows Server vNext](http://channel9.msdn.com/events/ignite/2015/brk3487) (Extension des clusters de basculement et utilisation de réplicas de stockage dans Windows Server vNext) et le billet de blog [Enable Change Notifications between Sites - How and Why?](http://blogs.technet.com/b/qzaidi/archive/2010/09/23/enable-change-notifications-between-sites-how-and-why.aspx) (Permettre les notifications de modification entre les sites : comment et pourquoi ?).

### <a name="configure-a-stretch-cluster"></a>Configurer un cluster étendu  
Vous allez maintenant configurer le cluster étendu à l’aide du Gestionnaire du cluster de basculement ou de Windows PowerShell. Vous pouvez effectuer toutes les étapes ci-dessous sur les nœuds de cluster directement ou à partir d’un ordinateur de gestion à distance qui contient le Outils d’administration de serveur distant de Windows Server.  

#### <a name="failover-cluster-manager-method"></a>Méthode du Gestionnaire du cluster de basculement  

1.  Pour les charges de travail Hyper-V, sur un nœud où vous avez les données que vous souhaitez répliquer, ajoutez le disque de données source à partir de vos disques disponibles aux volumes partagés de cluster si vous n’avez pas déjà effectué cette configuration. N’ajoutez pas tous les disques; ajoutez simplement le disque unique. À ce stade, la moitié les disques s’affichent hors connexion, car il s’agit d’un stockage asymétrique.  
Si vous répliquez une charge de travail de ressource de disque physique comme Serveur de fichiers pour une utilisation générale, vous disposez déjà d’un disque associé au rôle prêt à l’emploi.  

    ![Écran montrant le Gestionnaire du cluster de basculement](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_OnlineDisks2.png)  

2.  Cliquez avec le bouton droit sur le disque CSV ou associé au rôle, cliquez sur **Réplication**, puis sur **Activer**.  

3.  Sélectionnez le volume de données de destination approprié et cliquez sur **Suivant**. Les disques de destination indiqués auront un volume de la même taille que le disque source sélectionné. Lors du déplacement entre ces boîtes de dialogue de l’Assistant, le stockage disponible se déplace automatiquement et est mis en ligne en arrière-plan si nécessaire.  

    ![Écran montrant la page Sélectionner le disque de destination de l’Assistant Configurer la réplication de stockage](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_SelectDestinationDataDisk2.png)  

4.  Sélectionnez le disque du journal source approprié et cliquez sur **Suivant**. Le volume du journal source doit résider sur un disque qui utilise un disque SSD ou un support aussi rapide, et non des disques en rotation.  

5.  Sélectionnez le volume du journal de destination approprié et cliquez sur Suivant. Les disques du journal de destination indiqués auront un volume de la même taille que le volume des disques du journal source sélectionné.  

6.  Laissez la valeur **Remplacer le volume de destination** pour **Overwrite Volume** (Remplacer le volume) si le volume de destination ne contient pas une copie antérieure des données du serveur source. Si la destination contient des données similaires, provenant d’une sauvegarde récente ou d’une réplication précédente, sélectionnez **Disque de destination amorcé**, puis cliquez sur **Suivant**.  

7.  Laissez la valeur **Réplication synchrone** pour **Mode de réplication** si vous envisagez d’utiliser une réplication sans objectif de point de récupération. Remplacez-la par **Réplication asynchrone** si vous envisagez d’étendre votre cluster sur des réseaux à latence plus élevée ou si vous avez besoin de latence d’E/S plus faible sur les nœuds du site principal.  
8.  Laissez la valeur **Performances maximales** pour **Groupe de cohérence** si vous ne prévoyez pas d’utiliser de demande d’écriture ultérieurement avec des paires de disques supplémentaires dans le groupe de réplication. Si vous prévoyez d’ajouter d’autres disques à ce groupe de réplication et que vous avez besoin d’une demande d’écriture garantie, sélectionnez **Activer la demande d’écriture**, puis cliquez sur **Suivant**.  

9.  Cliquez sur **Suivant** pour configurer la réplication et la formation de cluster étendu.  

    ![Écran montrant la page de confirmation de sélection de l’Assistant Configurer la réplication de stockage](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ConfigureSR2.png)  

10. Sur l’écran récapitulatif, notez les résultats de la boîte de dialogue de fin. Vous pouvez afficher le rapport dans un navigateur web.  

11. À ce stade, vous avez configuré un partenariat de réplica de stockage entre les deux moitiés du cluster, mais la réplication est en cours. Il existe plusieurs façons d’afficher l’état de la réplication via un outil graphique.  

    1.  Utilisez la colonne **Rôle de réplication** et l’onglet **Réplication**. Quand la synchronisation initiale est terminée, les disques source et de destination affichent **Réplication continue** pour l’état de réplication.   

        ![Écran montrant l’onglet Réplication d’un disque dans le Gestionnaire du cluster de basculement](./media/Stretch-Cluster-Replication-Using-Shared-Storage/Storage_SR_ReplicationDetails2.png)  

    2.  Démarrez **eventvwr.exe**.  

        1.  Sur le serveur source, accédez à **Applications et services\Microsoft\Windows\StorageReplica\Admin** et examinez les événements 5015, 5002, 5004, 1237, 5001 et 2200.  

        2.  Sur le serveur de destination, accédez à **Applications et services\Microsoft\Windows\StorageReplica\Operational** et attendez l’événement 1215. Cet événement indique le nombre d’octets copiés et la durée de l’opération. Exemple :  

            ```  
            Log Name:      Microsoft-Windows-StorageReplica/Operational  
            Source:        Microsoft-Windows-StorageReplica  
            Date:          4/6/2016 4:52:23 PM  
            Event ID:      1215  
            Task Category: (1)  
            Level:         Information  
            Keywords:      (1)  
            User:          SYSTEM  
            Computer:      SR-SRV03.Threshold.nttest.microsoft.com  
            Description:  
            Block copy completed for replica.  

            ReplicationGroupName: Replication 2  
            ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}  
            ReplicaName: \\?\Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}\  
            ReplicaId: {00000000-0000-0000-0000-000000000000}  
            End LSN in bitmap:   
            LogGeneration: {00000000-0000-0000-0000-000000000000}  
            LogFileId: 0  
            CLSFLsn: 0xFFFFFFFF  
            Number of Bytes Recovered: 68583161856  
            Elapsed Time (ms): 140  
            ```  

        3.  Sur le serveur de destination, accédez à **Applications et services\Microsoft\Windows\StorageReplica\Admin** et examinez les événements 5009, 1237, 5001, 5015, 5005 et 2200 pour comprendre la progression du traitement. Il ne doit y avoir aucun avertissement d’erreur dans cette séquence. Il y aura un grand nombre d’événements 1237; ils indiquent la progression.  

            > [!WARNING]
            > L’utilisation du processeur et de la mémoire est susceptible d’être supérieure à la normale tant que la synchronisation initiale n’est pas terminée.  

#### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  

1.  Vérifiez que votre console Powershell s’exécute avec un compte d’administrateur aux droits élevés.  
2.  Ajoutez le stockage de données source uniquement au cluster en tant que volume partagé de cluster. Pour obtenir la taille, la partition et la disposition du volume des disques disponibles, utilisez les commandes suivantes:  

    ```PowerShell  
    Move-ClusterGroup -Name "available storage" -Node sr-srv01  

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }  
    $DiskResources | foreach {  
        $resource = $_  
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid  

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |  
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining  
    } | FT -AutoSize  

    Move-ClusterGroup -Name "available storage" -Node sr-srv03  

    $DiskResources = Get-ClusterResource | Where-Object { $_.ResourceType -eq 'Physical Disk' -and $_.State -eq 'Online' }  
    $DiskResources | foreach {  
        $resource = $_  
        $DiskGuidValue = $resource | Get-ClusterParameter DiskIdGuid  

        Get-Disk | where { $_.Guid -eq $DiskGuidValue.Value } | Get-Partition | Get-Volume |  
            Select @{N="Name"; E={$resource.Name}}, @{N="Status"; E={$resource.State}}, DriveLetter, FileSystemLabel, Size, SizeRemaining  
    } | FT -AutoSize  
    ```  

4.  Définissez le volume partagé de cluster comme disque correct avec :  

    ```PowerShell  
    Add-ClusterSharedVolume -Name "Cluster Disk 4"  
    Get-ClusterSharedVolume  
    Move-ClusterSharedVolume -Name "Cluster Disk 4" -Node sr-srv01  
    ```  

5.  Configurez le cluster étendu, en spécifiant les éléments suivants :  

    -   Nœuds source et de destination (où la source de données est un disque de volume partagé de cluster contrairement à tous les autres disques).  

    -   Noms des groupes de réplication source et de destination.  

    -   Disques source et de destination, où les tailles des partitions correspondent.  

    -   Volumes de journaux source et de destination, où l’espace libre est suffisant pour contenir la taille du journal sur les deux disques et le stockage est un disque SSD ou un support aussi rapide.  

    -   Volumes de journaux source et de destination, où l’espace libre est suffisant pour contenir la taille du journal sur les deux disques et le stockage est un disque SSD ou un support aussi rapide.  

    -   Taille du journal.  

    -   Le volume du journal source doit résider sur un disque qui utilise un disque SSD ou un support aussi rapide, et non des disques en rotation.  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv01 -SourceRGName rg01 -SourceVolumeName "C:\ClusterStorage\Volume1" -SourceLogVolumeName e: -DestinationComputerName sr-srv03 -DestinationRGName rg02 -DestinationVolumeName d: -DestinationLogVolumeName e:  
    ```  

    > [!NOTE]  
    > Vous pouvez également utiliser `New-SRGroup` sur un nœud dans chaque site et `New-SRPartnership` pour créer la réplication par phases, plutôt que tout à la fois.  

6.  Déterminez la progression de la réplication.  

    1.  Sur le serveur source, exécutez la commande suivante et examinez les événements 5015, 5002, 5004, 1237, 5001 et 2200:  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  Sur le serveur de destination, exécutez la commande suivante pour afficher les événements de réplica de stockage qui indiquent la création du partenariat. Cet événement indique le nombre d’octets copiés et la durée de l’opération. Exemple :  

            Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  

            TimeCreated  : 4/6/2016 4:52:23 PM  
            ProviderName : Microsoft-Windows-StorageReplica  
            Id           : 1215  
            Message      : Block copy completed for replica.  

               ReplicationGroupName: Replication 2  
               ReplicationGroupId: {c6683340-0eea-4abc-ab95-c7d0026bc054}  
               ReplicaName: ?Volume{43a5aa94-317f-47cb-a335-2a5d543ad536}  
               ReplicaId: {00000000-0000-0000-0000-000000000000}  
               End LSN in bitmap:   
               LogGeneration: {00000000-0000-0000-0000-000000000000}  
               LogFileId: 0  
               CLSFLsn: 0xFFFFFFFF  
               Number of Bytes Recovered: 68583161856  
               Elapsed Time (ms): 140  

    3.  Sur le serveur de destination, exécutez la commande suivante et examinez les événements 5009, 1237, 5001, 5015, 5005 et 2200 pour comprendre la progression du traitement. Il ne doit y avoir aucun avertissement d’erreur dans cette séquence. Il y aura un grand nombre d’événements 1237; ils indiquent la progression.  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

    4.  Le groupe de serveurs de destination pour le réplica peut aussi indiquer le nombre d’octets restants à copier à tout moment, et peut être interrogé via PowerShell. Exemple :  

        ```PowerShell  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        Un exemple de progression (qui ne s’interrompt pas) :  

        ```PowerShell  
        while($true) {  

         $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

7.  Pour obtenir l’état de réplication source et de destination au sein du cluster étendu, utilisez `Get-SRGroup` et `Get-SRPartnership` pour afficher l’état configuré de réplication dans le cluster étendu.  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```  

### <a name="manage-stretched-cluster-replication"></a>Gérer la réplication de cluster étendu  
Vous allez maintenant gérer et faire fonctionner votre cluster étendu. Vous pouvez effectuer toutes les étapes ci-dessous sur les nœuds de cluster directement ou à partir d’un ordinateur de gestion à distance qui contient le Outils d’administration de serveur distant de Windows Server.  

#### <a name="graphical-tools-method"></a>Méthode des outils graphiques  

1.  Utilisez le Gestionnaire du cluster de basculement pour déterminer la source et la destination actuelles de la réplication et leur état.  

2.  Pour mesurer les performances de la réplication, exécutez **Perfmon.exe** sur les deux nœuds source et de destination.  

    1.  Sur le nœud de destination:  

        1.  Ajoutez les objets **Statistiques du réplica du système de stockage** avec tous leurs compteurs de performance pour le volume de données.  

        2.  Examinez les résultats.  

    2.  Sur le nœud source:  

        1.  Ajoutez les objets **Statistiques du réplica du système de stockage** et **Statistiques d’E/S du réplica du système de stockage** avec tous leurs compteurs de performance pour le volume de données (ce dernier est uniquement disponible avec des données sur le serveur source actuel).  

        2.  Examinez les résultats.  

3.  Pour modifier la source et la destination de réplication au sein du cluster étendu, utilisez les méthodes suivantes:  

    1.  Pour déplacer la réplication source entre les nœuds du même site : cliquez avec le bouton droit sur le volume partagé de cluster source, cliquez sur **Move Storage** (Déplacer le stockage), sur **Sélectionnez un nœud**, puis sélectionnez un nœud dans le même site. Si vous utilisez un stockage non-CSV pour un disque affecté au rôle, vous déplacez le rôle.  

    2.  Pour déplacer la réplication source entre deux sites: cliquez avec le bouton droit sur le volume partagé de cluster source, cliquez sur **Move Storage** (Déplacer le stockage), sur **Sélectionnez un nœud**, puis sélectionnez un nœud dans un autre site. Si vous avez configuré un site préféré, vous pouvez utiliser le meilleur nœud possible pour toujours déplacer le stockage source vers un nœud dans le site préféré. Si vous utilisez un stockage non-CSV pour un disque affecté au rôle, vous déplacez le rôle.  

    3.  Pour effectuer un basculement planifié du sens de la réplication entre deux sites: arrêtez les deux nœuds dans un site en utilisant **ServerManager.exe** ou **SConfig**.  

    4.  Pour effectuer un basculement non planifié du sens de la réplication entre deux sites: coupez l’alimentation des deux nœuds dans un site.  

        > [!NOTE]
        > Dans Windows Server2016, vous devrez peut-être utiliser le Gestionnaire du cluster de basculement ou Move-ClusterGroup pour ramener manuellement les disques de destination sur l’autre site une fois les nœuds remis en ligne.  

        > [!NOTE]
        > Le réplica de stockage démonte les volumes de destination. Cela est normal.  

4.  Pour modifier la taille du journal par défaut de 8 Go, cliquez avec le bouton droit sur les disques du journal source et de destination, cliquez sur l’onglet **Journal de réplication** , puis modifiez la taille des deux disques pour les faire correspondre.  

    > [!NOTE]  
    > La taille du journal par défaut est de 8 Go. En fonction des résultats de l’applet de commande `Test-SRTopology`, vous pouvez décider d’utiliser `-LogSizeInBytes` avec une valeur supérieure ou inférieure.  

5.  Pour ajouter une autre paire de disques répliqués au groupe de réplication existant, vous devez vérifier qu’il existe au moins un disque supplémentaire dans l’espace de stockage disponible. Vous pouvez alors cliquer avec le bouton droit sur le disque source, puis sélectionner **Add replication partnership** (Ajouter un partenariat de réplication).  
    > [!NOTE]  
    > Cette nécessité d’un disque supplémentaire « factice » dans l’espace de stockage disponible est dû à une régression et n’est pas intentionnelle. Le Gestionnaire du cluster de basculement prenait auparavant en charge l’ajout d’autres disques normalement et assurera à nouveau cette prise en charge dans une version ultérieure.  


6.  Pour supprimer la réplication existante :  

    1.  Démarrez **cluadmin.msc**.  

    2.  Cliquez avec le bouton droit sur le disque de volume partagé de cluster source et cliquez sur **Réplication**, puis sur **Supprimer**. Acceptez la demande de confirmation d’avertissement.  

    3.  Si vous le souhaitez, supprimez le stockage du volume partagé de cluster pour le renvoyer au stockage disponible pour d’autres tests.  

        > [!NOTE]  
        > Vous devrez peut-être utiliser **DiskMgmt.msc** ou **ServerManager.exe** pour rajouter des lettres de lecteur aux volumes renvoyés à l’espace de stockage disponible.  

#### <a name="windows-powershell-method"></a>Méthode basée sur Windows PowerShell  

1.  Utilisez **Get-SRGroup** et **(Get-SRGroup).Replicas** pour déterminer la source et la destination actuelles de la réplication et leur état.  

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

3.  Pour modifier la source et la destination de réplication au sein du cluster étendu, utilisez les méthodes suivantes:  

    1.  Pour déplacer la source de réplication entre deux nœuds dans le site **Redmond**, déplacez la ressource CSV à l’aide de l’applet de commande Move-ClusterSharedVolume.  

        ```PowerShell  
        Get-ClusterSharedVolume | fl *  
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv02  
        ```  

    2.  Pour déplacer le sens de la réplication d’un site vers un autre site «planifié», déplacez la ressource CSV à l’aide de l’applet de commande **Move-ClusterSharedVolume**.  

        ```PowerShell 
        Get-ClusterSharedVolume | fl *  
        Move-ClusterSharedVolume -Name "cluster disk 4" -Node sr-srv04  
        ```  

        Vous déplacez ainsi également les journaux et les données de manière appropriée pour l’autre site et les nœuds.  

    3.  Pour effectuer un basculement non planifié du sens de la réplication entre deux sites: coupez l’alimentation des deux nœuds dans un site.  

        > [!NOTE]  
        > Le réplica de stockage démonte les volumes de destination. Cela est normal.  

4.  Pour modifier la taille du journal par défaut de 8 Go, utilisez **Set-SRGroup** sur les groupes de réplicas de stockage source et de destination.   Par exemple, pour définir une taille de 2Go pour tous les journaux:  

    ```PowerShell  
    Get-SRGroup | Set-SRGroup -LogSizeInBytes 2GB  
    Get-SRGroup  
    ```  

5.  Pour ajouter une autre paire de disques répliqués au groupe de réplication existant, vous devez vérifier qu’il existe au moins un disque supplémentaire dans l’espace de stockage disponible. Vous pouvez alors cliquer avec le bouton droit sur le disque source, puis sélectionner Add replication partnership (Ajouter un partenariat de réplication).  
       >[!NOTE]
       >Cette nécessité d’un disque supplémentaire « factice » dans l’espace de stockage disponible est dû à une régression et n’est pas intentionnelle. Le Gestionnaire du cluster de basculement prenait auparavant en charge l’ajout d’autres disques normalement et assurera à nouveau cette prise en charge dans une version ultérieure.  

    Utilisez l’applet de commande **Set-SRPartnership** avec les paramètres **-SourceAddVolumePartnership** et **-DestinationAddVolumePartnership**.  
6.  Pour supprimer la réplication, utilisez `Get-SRGroup`, Get-`SRPartnership`, `Remove-SRGroup` et `Remove-SRPartnership` sur n’importe quel nœud.  

    ```PowerShell  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

    > [!NOTE]
    > Si vous utilisez un ordinateur de gestion à distance, vous devez indiquer le nom du cluster à ces applets de commande et fournir les deux noms des groupes de réplication.  

### <a name="related-topics"></a>Rubriques connexes  
- [Vue d’ensemble du réplica de stockage](storage-replica-overview.md)  
- [Réplication de stockage de serveur à serveur](server-to-server-storage-replication.md)  
- [Réplication de stockage de cluster à cluster](cluster-to-cluster-storage-replication.md)  
- [Réplica de stockage : Problèmes connus](storage-replica-known-issues.md) 
- [Réplica de stockage : Forum Aux Questions](storage-replica-frequently-asked-questions.md)  

## <a name="see-also"></a>Voir aussi  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [espaces de stockage direct dans Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)
