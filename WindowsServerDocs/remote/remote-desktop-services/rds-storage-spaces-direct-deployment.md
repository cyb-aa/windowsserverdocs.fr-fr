---
title: Déployer un serveur SOFS avec des espaces de stockage direct à deux nœuds pour le stockage UPD dans Azure
description: Découvrez comment utiliser les espaces de stockage direct avec les services Bureau à distance.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 1099f21d-5f07-475a-92dd-ad08bc155da1
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
manager: scottman
ms.openlocfilehash: 2386a231edf80fa611daf71c171bc0de3a7b497e
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855542"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>Déployer un serveur de fichiers scale-out avec des espaces de stockage direct à deux nœuds pour le stockage UPD dans Azure

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Les services Bureau à distance nécessitent un serveur de fichiers appartenant à un domaine pour les UPD (disques de profil utilisateur). Pour déployer sur Azure un SOFS (serveur de fichiers scale-out) à haute disponibilité appartenant à un domaine, utilisez les espaces de stockage direct avec Windows Server 2016. Si vous n’avez pas l’habitude d’utiliser les UPD ou les services Bureau à distance, consultez [Bienvenue dans les services Bureau à distance](welcome-to-rds.md).

> [!NOTE] 
> Microsoft vient de publier un [modèle Azure permettant de déployer un serveur de fichiers scale-out avec espaces de stockage direct](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/) ! Vous pouvez utiliser le modèle pour créer votre déploiement, ou suivre les étapes décrites dans cet article. 

Nous vous recommandons de déployer votre SOFS avec des machines virtuelles de la série DS et des disques de données de stockage Premium, car le nombre et la taille des disques de données sont identiques sur chaque machine virtuelle. Vous avez besoin au minimum de deux comptes de stockage. 

Pour les petits déploiements, nous recommandons un cluster à 2 nœuds avec témoin cloud, où le volume est en miroir avec 2 copies. Augmentez la taille des petits déploiements en ajoutant des disques de données. Augmentez la taille des grands déploiements en ajoutant des nœuds (machines virtuelles). 

Ces instructions concernent un déploiement à 2 nœuds. Le tableau suivant montre les types de machine virtuelle et tailles de disque dont vous avez besoin pour stocker les UPD en fonction du nombre d’utilisateurs de votre entreprise. 

| Utilisateurs | Total (Go) | VM | Nombre de disques | Type de disque | Taille du disque (Go) | Configuration   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2x(DS1 + 2 P20)  |
| 250   | 1 250       | DS1 | 2       | P30       | 1024           | 2x(DS1 + 2 P30)  |
| 500   | 2 500       | DS2 | 3       | P30       | 1024           | 2x(DS2 + 3 P30)  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2x(DS3 + 5 P30)  |
| 2 500  | 12 500      | DS4 | 13      | P30       | 1024           | 2x(DS4 + 13 P30) |
| 5000  | 25 000      | DS5 | 25      | P30       | 1024           | 2x(DS5 + 25 P30) | 

Suivez les étapes ci-après pour créer un contrôleur de domaine (nous l’avons appelé « my-dc » ci-dessous) et deux machines virtuelles servant de nœuds (« my-fsn1 » et « my-fsn2 »). Configurez ensuite les machines virtuelles pour créer un SOFS avec espaces de stockage direct à 2 nœuds.

1. Créez un abonnement [Microsoft Azure](https://azure.microsoft.com).
2. Connectez-vous au [portail Azure](https://ms.portal.azure.com).
3. Créez un [compte de stockage Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account) dans Azure Resource Manager. Créez-le dans un nouveau groupe de ressources, et utilisez les configurations suivantes :
   - Modèle de déploiement : Resource Manager
   - Type de compte de stockage : Universel
   - Niveau de performance : Premium
   - Option de réplication : LRS
4. Configurez une forêt Active Directory en utilisant un modèle de démarrage rapide ou en déployant la forêt manuellement. 
   - Effectuez le déploiement à l’aide d’un modèle de démarrage rapide Azure :
      - [Créer une machine virtuelle Azure avec une nouvelle forêt Active Directory](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [Créer un domaine Active Directory avec 2 contrôleurs de domaine](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) (pour la haute disponibilité)
   - [Déployez la forêt](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/) manuellement avec les configurations suivantes :
      - Créez le réseau virtuel dans le même groupe de ressources que le compte de stockage.
      - Taille recommandée : DS2 (augmentez la taille si le contrôleur de domaine doit héberger un plus grand nombre d’objets de domaine)
      - Utilisez un réseau virtuel généré automatiquement.
      - Suivez les étapes pour installer AD DS.
5. Configurez les nœuds de cluster de serveurs de fichiers. Pour ce faire, vous pouvez déployer le [modèle Azure de cluster SOFS avec espaces de stockage direct Windows Server 2016](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/) ou suivre les étapes 6 à 11 afin d’effectuer le déploiement manuellement.
6. Pour configurer manuellement les nœuds de cluster de serveurs de fichiers :
   1. Créez le premier nœud : 
      1. Créez une machine virtuelle à l’aide de l’image Windows Server 2016. (Cliquez sur **Nouveau > Machines virtuelles > Windows Server 2016.** Sélectionnez **Resource Manager**, puis cliquez sur **Créer**.)
      2. Définissez la configuration de base comme suit :
         - Nom : my-fsn1
         - Type de disque de machine virtuelle : SSD
         - Utilisez le groupe de ressources que vous avez créé à l’étape 3. 
      3. Taille : DS1, DS2, DS3, DS4 ou DS5 en fonction des besoins de l’utilisateur (consultez le tableau au début de ces instructions). Vérifiez que la prise en charge de disques Premium est sélectionnée.
      4. Paramètres : 
         - Compte de stockage : choisissez le compte de stockage que vous avez créé à l’étape 3.
         - Haute disponibilité : créez un groupe à haute disponibilité. (Cliquez sur **Haute disponibilité > Créer**, puis entrez un nom (par exemple s2d-cluster). Utilisez les valeurs par défaut pour **Domaines de mise à jour** et **Domaines d’erreur**.)
   2. Créez le second nœud. Répétez l’étape ci-dessus en apportant les changements suivants :
      - Nom : my-fsn2
      - Haute disponibilité : sélectionnez le groupe à haute disponibilité que vous avez créé ci-dessus.  
7. [Attachez des disques de données](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/) aux machines virtuelles servant de nœuds de cluster en fonction des besoins des utilisateurs (consultez le tableau ci-dessus). Une fois les disques de données créés et attachés à la machine virtuelle, affectez à l’option **Mise en cache de l’hôte** la valeur **Aucune**.
8. Définissez des adresses IP **statiques** pour toutes les machines virtuelles. 
   1. Dans le groupe de ressources, sélectionnez une machine virtuelle, puis cliquez sur **Interfaces réseau** (sous **paramètres**). Sélectionnez l’interface réseau listée, puis cliquez sur **Configurations IP**. Sélectionnez la configuration IP listée, sélectionnez **statique**, puis cliquez sur **Enregistrer**.
   2. Notez l’adresse IP privée du contrôleur de domaine (my-dc pour notre exemple) (10.x.x.x).
9. Définissez le serveur my-dc en tant qu’adresse de serveur DNS principal sur les cartes réseau des machines virtuelles servant de nœuds de cluster. Sélectionnez la machine virtuelle, puis cliquez sur **Interfaces réseau > Serveurs DNS > DNS personnalisé**. Entrez l’adresse IP privée notée ci-dessus, puis cliquez sur **Enregistrer**.
10. Créez un [compte de stockage Azure servant de témoin cloud](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness). (Si vous utilisez les instructions liées, arrêtez-vous à l’étape décrivant la configuration du témoin cloud via l’interface graphique utilisateur du Gestionnaire du cluster de basculement. Nous allons effectuer cette étape ci-dessous.)
11. Configurez le serveur de fichiers avec espaces de stockage direct. Connectez-vous à une machine virtuelle de nœud, puis exécutez les applets de commande Windows PowerShell suivantes.
    1. Installez la fonctionnalité de clustering de basculement et la fonctionnalité de serveur de fichiers sur les deux machines virtuelles servant de nœuds de cluster de serveurs de fichiers :

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. Validez les machines virtuelles servant de nœuds de cluster, puis créez un cluster SOFS à 2 nœuds :

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. Configurez le témoin cloud. Utilisez le nom et la clé d’accès du compte de stockage de votre témoin cloud.

       ```powershell
       Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> 
       ```
    4. Activez les espaces de stockage direct.

       ```powershell
       Enable-ClusterS2D 
       ```
      
    5. Créez un volume de disque virtuel.

       ```powershell
       New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 120GB 
       ```
       Pour voir les informations relatives au volume partagé de cluster sur le cluster SOFS, exécutez l’applet de commande suivante :

       ```powershell
       Get-ClusterSharedVolume
       ```
   
    6. Créez le SOFS (serveur de fichiers scale-out) :

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. Créez un partage de fichiers SMB sur le cluster SOFS.

       ```powershell
       New-Item -Path C:\ClusterStorage\VDisk01\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\VDisk01\Data
       ```

Vous disposez maintenant d’un partage sur `\\my-sofs1\UpdStorage`, que vous pouvez utiliser pour le stockage UPD quand vous [activez UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx) pour vos utilisateurs. 
