---
title: Déployer un témoin de partage de fichiers dans Windows Server 2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: Témoins de partage de fichiers vous permettent d’utiliser un partage de fichiers vote quorum du cluster. Cette rubrique décrit les témoins de partage de fichier et les nouvelles fonctionnalités, y compris à l’aide d’un lecteur USB connecté à un routeur comme un témoin de partage de fichiers.
ms.localizationpriority: medium
ms.openlocfilehash: 1888142f96208800a0417c9caeea89e8a0472e88
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150889"
---
# Déployer un témoin de partage de fichiers

> S’applique à: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un témoin de partage de fichiers est un partage SMB qui utilise de Cluster de basculement comme un vote dans le quorum du cluster. Cette rubrique fournit une vue d’ensemble de la technologie et les nouvelles fonctionnalités de Windows Server 2019, y compris à l’aide d’un lecteur USB connecté à un routeur comme un témoin de partage de fichiers.

Témoins de partage de fichiers sont utiles dans les circonstances suivantes:  

- Un témoin de cloud ne peuvent pas être utilisé dans la mesure où pas tous les serveurs du cluster ont une connexion Internet fiable
- Un témoin de disque ne peuvent pas être utilisé dans la mesure où il ne sont pas des lecteurs partagés à utiliser pour un témoin de disque. Il peut s’agir d’un cluster espaces de stockage Direct, SQL Server toujours sur la disponibilité groupes (AG), Exchange de base de données disponibilité groupe (DAG), etc..  Aucune de ces types de clusters l’utilisation des disques partagés.

## Exigences de témoin de partage de fichiers

Vous pouvez héberger un témoin de partage de fichiers sur un serveur Windows joints au domaine, ou si votre cluster exécute Windows Server 2019, n’importe quel appareil hôte un 2 SMB fichier ou version ultérieure partager.

|Type de serveur de fichiers                 | Clusters pris en charge |
|---------------------------------|--------------------|
|N’importe quel partage w/un appareil SMB 2 fichier | Windows Server2019|
|Joint au domaine de Windows Server     | Windows Server 2008 et versions ultérieur|

Si le cluster exécute Windows Server 2019, voici la configuration requise:

- Un fichier SMB partager *sur n’importe quel appareil qui utilise le protocole ultérieure ou SMB 2*, y compris:
    - Dispositifs de stockage connecté au réseau (NAS)
    - Les ordinateurs Windows joint à un groupe de travail
    - Routeurs avec un stockage USB connectés localement
- Un compte local sur l’appareil pour l’authentification du cluster
- Si vous utilisez à la place des Active Directory pour l’authentification du cluster avec le partage de fichiers, l’objet de nom de Cluster (CNO) doit avoir un accès en écriture sur le partage, et le serveur doit être dans la même forêt Active Directory en tant que le cluster
- Le partage de fichiers dispose d’au moins 5 Mo d’espace libre

Si le cluster s’exécute Windows Server 2016 ou version antérieure, voici la configuration requise:

- Partage de fichiers SMB *sur un serveur Windows joint à la même forêt Active Directory en tant que le cluster*
- L’objet de nom de Cluster (CNO) doit avoir un accès en écriture sur le partage
- Le partage de fichiers dispose d’au moins 5 Mo d’espace libre

Autres remarques:
- Pour utiliser un témoin de partage de fichier hébergé par les appareils autres que d’un ordinateur joint au domaine Windows server, vous devez utilisez actuellement le **Set-ClusterQuorum-informations d’identification** applet de commande PowerShell pour définir le rappel, comme décrit plus loin dans cette rubrique.
- De la haute disponibilité, vous pouvez utiliser un témoin de partage de fichiers sur un Cluster de basculement distinct
- Le partage de fichiers peut être utilisé par plusieurs clusters
- L’utilisation d’un partage de système de fichiers distribués (DFS) ou un stockage répliqué n’est pas pris en charge avec n’importe quelle version de clustering de basculement.  Il peuvent provoquer une situation de cerveau fractionné, où les serveurs en cluster sont en cours d’exécution indépendamment des autres et peuvent entraîner la perte de données.

## Création d’un témoin de partage de fichiers sur un routeur avec un périphérique USB

Au [Microsoft Ignite 2018](https://azure.microsoft.com/ignite/), [Stockage DataOn](http://www.dataonstorage.com/) avait un Cluster d’espaces de stockage Direct dans leur zone plein écran.  Ce cluster a été connecté à un routeur de Wi-Fi [NetGear](https://www.netgear.com) Nighthawk X4S en utilisant le port USB comme un témoin de partage de fichier comme suit.

![Témoin NetGear](media\File-Share-Witness\FSW1.png)

Les étapes de création d’un témoin de partage de fichiers à l’aide d’un périphérique USB sur ce routeur particulier sont indiqués ci-dessous.  Notez que les étapes dans d’autres routeurs et des appliances NAS peuvent varier et doivent être effectuées à l’aide du fournisseur fourni par un itinéraire.


1. Ouvrez une session dans le routeur avec le périphérique USB branché.

   ![Interface NetGear](media\File-Share-Witness\FSW2.png)

2. Dans la liste des options, sélectionnez ReadySHARE qui est l’emplacement de création des partages.

   ![NetGear ReadySHARE](media\File-Share-Witness\FSW3.png)

3. Pour un témoin de partage de fichiers, un partage de base est tout ce qui est nécessaire.  En sélectionnant le bouton Modifier avec s’affiche une boîte de dialogue où le partage peut être créé sur un périphérique USB.

   ![Interface de partage NetGear](media\File-Share-Witness\FSW4.png)

4. Une fois que vous sélectionnez le bouton Appliquer, le partage est créé et s’affiche dans la liste.

   ![Partages NetGear](media\File-Share-Witness\FSW5.png)

5. Une fois que le partage a été créé, la création du témoin de partage de fichiers pour le Cluster s’effectue avec PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Cela permet d’afficher une boîte de dialogue pour entrer le compte local sur l’appareil.

Ces mêmes étapes similaires peuvent être effectuées sur les autres routeurs avec les fonctionnalités USB, périphériques NAS ou d’autres appareils Windows.
