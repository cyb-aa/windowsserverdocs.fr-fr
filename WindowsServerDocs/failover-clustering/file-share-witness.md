---
title: Déployer un témoin de partage de fichiers dans Windows Server 2019
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: Les témoins de partage de fichiers vous permettent d’utiliser un partage de fichiers pour voter dans le quorum de cluster. Cette rubrique décrit les témoins de partage de fichiers et les nouvelles fonctionnalités, notamment l’utilisation d’un lecteur USB connecté à un routeur en tant que témoin de partage de fichiers.
ms.localizationpriority: medium
ms.openlocfilehash: 9f0a0c5b48f7c382367e4b1100ff649fe73d3be9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369762"
---
# <a name="deploy-a-file-share-witness"></a>Déployer un témoin de partage de fichiers

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un témoin de partage de fichiers est un partage SMB que le cluster de basculement utilise comme vote dans le quorum de cluster. Cette rubrique fournit une vue d’ensemble de la technologie et des nouvelles fonctionnalités de Windows Server 2019, y compris l’utilisation d’un lecteur USB connecté à un routeur en tant que témoin de partage de fichiers.

Les témoins de partage de fichiers sont pratiques dans les circonstances suivantes :  

- Impossible d’utiliser un témoin de Cloud, car tous les serveurs du cluster ne disposent pas d’une connexion Internet fiable
- Un témoin de disque ne peut pas être utilisé car il n’existe aucun lecteur partagé à utiliser pour un témoin de disque. Il peut s’agir d’un cluster espaces de stockage direct, d’SQL Server Always On groupes de disponibilité (AG), d’un groupe de disponibilité de base de données Exchange (DAG), etc.  Aucun de ces types de clusters n’utilise des disques partagés.

## <a name="file-share-witness-requirements"></a>Conditions requises pour le partage de fichiers

Vous pouvez héberger un témoin de partage de fichiers sur un serveur Windows joint à un domaine, ou si votre cluster exécute Windows Server 2019, tout appareil qui peut héberger un partage de fichiers SMB 2 ou version ultérieure.

|Type de serveur de fichiers                 | Clusters pris en charge |
|---------------------------------|--------------------|
|Tout appareil avec un partage de fichiers SMB 2 | Windows Server 2019|
|Serveur Windows joint à un domaine     | Windows Server 2008 et versions ultérieures|

Si le cluster exécute Windows Server 2019, voici la configuration requise :

- Un partage de fichiers SMB *sur tout appareil qui utilise le protocole SMB 2 ou version ultérieure*, y compris :
    - Périphériques NAS (Network-Attached Storage)
    - Ordinateurs Windows joints à un groupe de travail
    - Routeurs avec stockage USB connecté localement
- Un compte local sur l’appareil pour l’authentification du cluster
- Si vous utilisez à la place Active Directory pour l’authentification du cluster avec le partage de fichiers, l’objet nom de cluster (CNO) doit disposer d’autorisations en écriture sur le partage, et le serveur doit se trouver dans la même forêt Active Directory que le cluster
- Le partage de fichiers a un minimum de 5 Mo d’espace libre

Si le cluster exécute Windows Server 2016 ou une version antérieure, voici la configuration requise :

- Partage de fichiers SMB *sur un serveur Windows Server joint à la même forêt de Active Directory que le cluster*
- L’objet nom de cluster (CNO) doit avoir des autorisations en écriture sur le partage
- Le partage de fichiers a un minimum de 5 Mo d’espace libre

Autres remarques :
- Pour utiliser un témoin de partage de fichiers hébergé par des appareils autres qu’un serveur Windows joint à un domaine, vous devez utiliser l’applet de commande PowerShell **Set-ClusterQuorum-Credential** pour définir le témoin, comme décrit plus loin dans cette rubrique.
- Pour une haute disponibilité, vous pouvez utiliser un témoin de partage de fichiers sur un cluster de basculement distinct.
- Le partage de fichiers peut être utilisé par plusieurs clusters.
- L’utilisation d’un partage système de fichiers DFS (DFS) ou d’un stockage répliqué n’est pas prise en charge avec les versions du clustering de basculement.  Cela peut entraîner une situation de Split Brain dans laquelle les serveurs en cluster s’exécutent indépendamment l’un de l’autre et peuvent entraîner une perte de données.

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>Création d’un témoin de partage de fichiers sur un routeur doté d’un périphérique USB

Chez [2018 Microsoft](https://azure.microsoft.com/ignite/), le [stockage DataOn](http://www.dataonstorage.com/) avait une espaces de stockage direct cluster dans son kiosque.  Ce cluster était connecté à un routeur Wi-Fi [Netgear](https://www.netgear.com) Nighthawk X4S en utilisant le port USB comme témoin de partage de fichiers similaire à celui-ci.

![Témoin NetGear](media/File-Share-Witness/FSW1.png)

Les étapes de création d’un témoin de partage de fichiers à l’aide d’un périphérique USB sur ce routeur particulier sont répertoriées ci-dessous.  Notez que les étapes sur les autres routeurs et les appliances NAS varient et doivent être effectuées à l’aide des instructions fournies par le fournisseur.


1. Connectez-vous au routeur avec le périphérique USB branché.

   ![Interface NetGear](media/File-Share-Witness/FSW2.png)

2. Dans la liste des options, sélectionnez ReadySHARE, où vous pouvez créer des partages.

   ![ReadySHARE NetGear](media/File-Share-Witness/FSW3.png)

3. Pour un témoin de partage de fichiers, un partage de base est tout ce qui est nécessaire.  Sélectionnez le bouton modifier pour afficher une boîte de dialogue dans laquelle le partage peut être créé sur le périphérique USB.

   ![Interface de partage NetGear](media/File-Share-Witness/FSW4.png)

4. Une fois que vous avez sélectionné le bouton appliquer, le partage est créé et peut être affiché dans la liste.

   ![Partages NetGear](media/File-Share-Witness/FSW5.png)

5. Une fois le partage créé, la création du témoin de partage de fichiers pour le cluster s’effectue avec PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Une boîte de dialogue s’affiche pour vous indiquer le compte local sur l’appareil.

Ces mêmes étapes similaires peuvent être effectuées sur d’autres routeurs avec des fonctionnalités USB, des périphériques NAS ou d’autres appareils Windows.
