---
title: Déployer un deux nœuds stockage sofs avec espaces Direct pour le stockage UPD dans Azure
description: Découvrez comment utiliser des espaces de stockage Direct avec RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1099f21d-5f07-475a-92dd-ad08bc155da1
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
manager: scottman
ms.openlocfilehash: 8af3a389ec726bbb5ebd62db57d9b3a9861ac63f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890960"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>Déployer un serveur de fichiers de montée en puissance espaces de stockage Direct de deux nœuds pour le stockage UPD dans Azure

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Services Bureau à distance (RDS) nécessite un serveur de fichiers joints au domaine pour les disques de profil utilisateur (UPD). Pour déployer un serveur de fichiers de montée en puissance joints au domaine de haute disponibilité (SOFS) dans Azure, utiliser les espaces de stockage Direct avec Windows Server 2016. Si vous n’êtes pas familiarisé avec les UPD ou des Services Bureau à distance, consultez [Bienvenue aux Services Bureau à distance](welcome-to-rds.md).

> [!NOTE] 
> Microsoft vient d’être publié un [modèle Azure pour déployer un serveur de fichiers de montée en puissance espaces de stockage Direct](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/)! Vous pouvez utiliser le modèle pour créer votre déploiement, ou utilisez les étapes de cet article. 

Nous vous recommandons de déployer votre SOFS avec des machines virtuelles de série DS et les disques de données de stockage premium, où il y a le même nombre et taille des disques de données sur chaque machine virtuelle. Vous devez au minimum deux comptes de stockage. 

Pour les déploiements de petite taille, nous recommandons un cluster à 2 noeuds avec un témoin de cloud, où le volume est mis en miroir avec 2 copies. Augmenter les petits déploiements en leur ajoutant des disques de données. Augmenter les déploiements plus importants en leur ajoutant des nœuds (machines virtuelles). 

Ces instructions concernent un déploiement de 2 nœuds. Le tableau suivant montre les tailles de machine virtuelle et disque vous aurez besoin stocker les disques de profil utilisateur pour le nombre d’utilisateurs dans votre entreprise. 

| Utilisateurs | Total (Go) | VM | # Disques | Type de disque | Taille du disque (Go) | Configuration   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2x(DS1 + 2 P20)  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2x(DS1 + 2 P30)  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2x(DS2 + 3 P30)  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2x(DS3 + 5 P30)  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2x(DS4 + 13 P30) |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2x(DS5 + 25 P30) | 

Utilisez les étapes suivantes pour créer un contrôleur de domaine (nous avons appelé la nôtre « mon-dc » ci-dessous) et deux nœuds de machines virtuelles (« my fsn1 » et « my fsn2 ») et de configurer les machines virtuelles à un nœud de 2 stockage Direct sofs avec espaces de.

1. Créer un [abonnement Microsoft Azure](https://azure.microsoft.com).
2. Connectez-vous à la [Azure portal](https://ms.portal.azure.com).
3. Créer un [compte de stockage Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account) dans Azure Resource Manager. Créer dans un groupe de ressources et utiliser les configurations suivantes :
   - Modèle de déploiement : Resource Manager
   - Type de compte de stockage : Usage général
   - Niveau de performances : Premium
   - Option de réplication : LRS
4. Configurer une forêt Active Directory en utilisant un modèle de démarrage rapide ou déploiement manuel de la forêt. 
   - Déployer à l’aide d’un modèle de démarrage rapide Azure :
      - [Créer une machine virtuelle Azure avec une nouvelle forêt AD](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [Créer un nouveau domaine Active Directory avec 2 contrôleurs de domaine](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) (pour la haute disponibilité)
   - Manuellement [déployer la forêt](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/) avec les configurations suivantes :
      - Créez le réseau virtuel dans le même groupe de ressources que le compte de stockage.
      - Taille recommandée : DS2 (augmenter la taille si le contrôleur de domaine héberge plusieurs objets de domaine)
      - Utiliser un réseau virtuel généré automatiquement.
      - Suivez les étapes pour installer les services AD DS.
5. Configurer les nœuds de cluster de serveurs de fichiers. Vous pouvez le faire en déployant le [cluster Windows Server 2016 stockage sofs avec espaces Direct modèle Azure](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/) ou en suivant les étapes 6 à 11 pour déployer manuellement.
5. Pour configurer manuellement les nœuds de cluster de serveurs de fichiers :
   1. Créer le premier nœud : 
      1. Créer une machine virtuelle à l’aide de l’image Windows Server 2016. (Cliquez sur **Nouveau > Machines virtuelles > Windows Server 2016.** Sélectionnez **Resource Manager**, puis cliquez sur **créer**.)
      2. Définissez la configuration de base comme suit :
         - Nom : my-fsn1
         - Type de disque SSD de machine virtuelle
         - Utiliser un groupe de ressources existant, celui que vous avez créé à l’étape 3. 
      3. Taille : DS1, DS2, DS3, DS4 ou DS5 en fonction de votre utilisateur a besoin (voir tableau au début de ces instructions). Vérifiez la prise en charge des disques premium est sélectionné.
      4. Paramètres : 
         - Compte de stockage : Choisissez le compte de stockage que vous avez créé à l’étape 3.
         - Haute disponibilité - créer un nouveau groupe à haute disponibilité. (Cliquez sur **haute disponibilité > créer un nouveau**, puis entrez un nom (par exemple, cluster s2d). Utilisez les valeurs par défaut **mettre à jour des domaines** et **domaines d’erreur**.)
   2. Créez le second nœud. Répétez l’étape précédente avec les modifications suivantes :
      - Nom : my-fsn2
      - Haute disponibilité : sélectionnez que la groupe à haute disponibilité vous créé ci-dessus.  
6. [Attacher des disques de données](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/) au nœud de cluster des machines virtuelles en fonction de votre utilisateur a besoin (comme indiqué dans le tableau ci-dessus). Une fois que les données des disques sont créés et attachés à la machine virtuelle, définissez **héberger la mise en cache** à **aucun**.
7. Définir des adresses IP pour toutes les machines virtuelles à **statique**. 
   1. Dans le groupe de ressources, sélectionnez une machine virtuelle, puis cliquez sur **interfaces réseau** (sous **paramètres**). Sélectionnez l’interface réseau répertoriée, puis cliquez sur **Configurations IP**. Sélectionnez la configuration IP répertoriées, sélectionnez **statique**, puis cliquez sur **enregistrer**.
   2. Notez le domaine contrôleur (my-dc pour notre exemple) adresse IP privée (10.x.x.x).
8. Définir l’adresse du serveur DNS principal sur les cartes réseau du nœud de cluster des machines virtuelles au serveur mon contrôleur de domaine. Sélectionnez la machine virtuelle, puis cliquez sur **Interfaces réseau > serveurs DNS > DNS personnalisé**. Entrez l’adresse IP privée que vous avez notée ci-dessus, puis cliquez sur **enregistrer**.
9. Créer un [compte de stockage Azure pour être votre témoin de cloud](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness). (Si vous utilisez les instructions liées, arrêter quand vous accédez à « Configuration Cloud témoin avec basculement Cluster Manager interface graphique utilisateur » ; nous allons faire cette étape ci-dessous).
10. Configurer le serveur de fichiers espaces de stockage Direct. Se connecter à un machine virtuelle du nœud et exécutez les applets de commande Windows PowerShell suivante.
   1. Installer la fonctionnalité de Clustering de basculement et la fonctionnalité de serveur de fichiers sur le nœud de cluster de serveur de deux fichiers machines virtuelles :

      ```powershell
      $nodes = ("my-fsn1", "my-fsn2")
      icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
      icm $nodes {Install-WindowsFeature FS-FileServer} 
      ```
   2. Valider des machines virtuelles de nœud de cluster et créer le cluster SOFS à 2 nœuds :

      ```powershell
      Test-Cluster -node $nodes
      New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
      ``` 
   3. Configurer le témoin de cloud. Utilisez votre témoin compte accès et le nom clé de stockage cloud.

      ```powershell
      Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> 
      ```
   4. Activer les espaces de stockage Direct.

      ```powershell
      Enable-ClusterS2D 
      ```
      
   5. Créer un volume de disque virtuel.

      ```powershell
      New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 120GB 
      ```
      Pour afficher des informations sur le volume partagé de cluster sur le cluster SOFS, exécutez l’applet de commande suivante :

      ```powershell
      Get-ClusterSharedVolume
      ```
   
   6. Créer le serveur de fichiers de montée en puissance (parallèle SOFS) :

      ```powershell
      Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
      ```

   7. Créer un nouveau partage de fichiers SMB sur le cluster SOFS.

      ```powershell
      New-Item -Path C:\ClusterStorage\Volume1\Data -ItemType Directory
      New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\Volume1\Data
      ```

Vous disposez maintenant d’un partage à &#92;\my-sofs1\UpdStorage, que vous pouvez utiliser pour le stockage UPD lorsque vous [activer UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx) pour vos utilisateurs. 
