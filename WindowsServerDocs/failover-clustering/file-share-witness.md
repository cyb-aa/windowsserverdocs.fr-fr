---
title: Déployer un témoin de partage de fichiers dans Windows Server 2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: Témoins de partage de fichiers vous permettent d’utiliser un partage de fichiers vote de quorum du cluster. Cette rubrique décrit les témoins de partage de fichiers et les nouvelles fonctionnalités, notamment l’utilisation d’un lecteur USB connecté à un routeur en tant qu’un témoin de partage de fichiers.
ms.localizationpriority: medium
ms.openlocfilehash: 47371be946c08cac2f271138d701922fc340a89d
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453043"
---
# <a name="deploy-a-file-share-witness"></a>Déployer un témoin de partage de fichiers

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un témoin de partage de fichiers est un partage SMB qui utilise de Cluster de basculement comme un vote dans le quorum du cluster. Cette rubrique fournit une vue d’ensemble de la technologie et les nouvelles fonctionnalités de Windows Server 2019, y compris à l’aide d’un lecteur USB connecté à un routeur en tant qu’un témoin de partage de fichiers.

Les témoins de partage de fichier sont pratiques dans les circonstances suivantes :  

- Impossible d’utiliser un témoin cloud car pas tous les serveurs du cluster ont une connexion Internet fiable
- Un témoin de disque ne peut pas être utilisé car il ne sont pas tous les lecteurs à utiliser pour un témoin de disque partagés. Cela peut être un cluster espaces de stockage Direct, SQL Server toujours sur les groupes de disponibilité, Exchange de base de données groupe de disponibilité (DAG), etc.  Aucun de ces types de clusters utiliser des disques partagés.

## <a name="file-share-witness-requirements"></a>Exigences de témoin de partage de fichiers

Vous pouvez héberger un témoin de partage de fichiers sur un serveur Windows joint au domaine, ou si votre cluster exécute Windows Server 2019, n’importe quel appareil hôte un SMB 2 fichier ou version ultérieure partager.

|Type de serveur de fichiers                 | Clusters pris en charge |
|---------------------------------|--------------------|
|N’importe quel partage w/un appareil SMB 2 fichier | Windows Server 2019|
|Joint au domaine de Windows Server     | Windows Server 2008 et versions ultérieur|

Si le cluster exécute Windows Server 2019, voici la configuration requise :

- Un partage de fichiers SMB *sur n’importe quel appareil qui utilise le SMB 2 ou ultérieure protocole*, y compris :
    - Périphériques de stockage connecté au réseau (NAS)
    - Ordinateurs Windows joints à un groupe de travail
    - Routeurs avec le stockage USB connecté localement
- Un compte local sur l’appareil pour l’authentification du cluster
- Si vous utilisez à la place de Active Directory pour authentifier le cluster avec le partage de fichiers, l’objet nom de Cluster (CNO) doit avoir des autorisations en écriture sur le partage et le serveur doit être dans la même forêt Active Directory que le cluster
- Le partage de fichiers a un minimum de 5 Mo d’espace libre

Si le cluster est en cours d’exécution Windows Server 2016 ou antérieure, voici la configuration requise :

- Partage de fichiers SMB *sur un serveur Windows joint à la même forêt Active Directory que le cluster*
- L’objet nom de Cluster (CNO) doit avoir des autorisations en écriture sur le partage
- Le partage de fichiers a un minimum de 5 Mo d’espace libre

Autres remarques :
- Pour utiliser un témoin de partage de fichiers hébergé par les périphériques autres qu’un serveur Windows joint au domaine, vous devez utilisez actuellement la **Set-ClusterQuorum-informations d’identification** applet de commande PowerShell pour définir le serveur témoin, comme décrit plus loin dans cette rubrique.
- Pour la haute disponibilité, vous pouvez utiliser un témoin de partage de fichiers sur un Cluster de basculement distinct
- Le partage de fichiers peut être utilisé par plusieurs clusters
- L’utilisation d’un partage de système de fichiers distribués (DFS, Distributed File System) ou un stockage répliqué n’est pas pris en charge avec les versions de cluster de basculement.  Cela peut entraîner une situation de cerveau fractionnement où les serveurs en cluster sont exécutent indépendamment les uns des autres et peut entraîner la perte de données.

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>Création d’un témoin de partage de fichiers sur un routeur avec un périphérique USB

À [Microsoft Ignite 2018](https://azure.microsoft.com/ignite/), [DataOn stockage](http://www.dataonstorage.com/) avait un Cluster à espaces de stockage Direct dans leur zone plein écran.  Ce cluster a été connecté à un [NetGear](https://www.netgear.com) semblable à celle-ci de témoin de partage de Nighthawk X4S Wi-Fi routeur à l’aide du port USB en tant que fichier.

![NetGear témoin](media/File-Share-Witness/FSW1.png)

Les étapes de création d’un témoin de partage de fichiers à l’aide d’un périphérique USB sur ce routeur particulier sont répertoriées ci-dessous.  Notez que les étapes sur d’autres routeurs et les appliances NAS peuvent varier et doivent être effectuées à l’aide du fournisseur fourni directions.


1. Ouvrez une session dans le routeur avec le périphérique USB connecté.

   ![Interface de NetGear](media/File-Share-Witness/FSW2.png)

2. Dans la liste des options, sélectionnez ReadySHARE qui est où les partages peuvent être créés.

   ![NetGear ReadySHARE](media/File-Share-Witness/FSW3.png)

3. Pour un témoin de partage de fichiers, un partage de base est tout ce qui est nécessaire.  En sélectionnant le bouton Modifier s’affiche une boîte de dialogue où le partage peut être créé sur le périphérique USB.

   ![Interface de partage de NetGear](media/File-Share-Witness/FSW4.png)

4. Une fois que vous sélectionnez le bouton Appliquer, le partage est créé et peut être consulté dans la liste.

   ![Partages NetGear](media/File-Share-Witness/FSW5.png)

5. Une fois que le partage a été créé, création le témoin de partage de fichier pour le Cluster s’effectue avec PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Cela affiche une boîte de dialogue pour entrer le compte local sur l’appareil.

Ces mêmes étapes similaires peuvent être effectuées sur les autres routeurs avec les fonctionnalités USB, les périphériques NAS ou autres appareils Windows.
