---
title: Problème d’utilisation élevée du processeur sur le serveur SMB
description: Explique comment résoudre le problème d’utilisation élevée du processeur sur le serveur SMB.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 8a14952ca90b6ee3bea901fd944d7c9843492fd4
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654340"
---
# <a name="high-cpu-usage-issue-on-the-smb-server"></a>Problème d’utilisation élevée du processeur sur le serveur SMB

Cet article explique comment résoudre le problème d’utilisation élevée du processeur sur le serveur SMB.

## <a name="high-cpu-usage-because-of-storage-performance-issues"></a>Utilisation élevée du processeur en raison de problèmes de performances de stockage

Les problèmes de performances de stockage peuvent entraîner une utilisation élevée du processeur sur les serveurs SMB. Avant de résoudre le problème, assurez-vous que le correctif cumulatif le plus récent est installé sur le serveur SMB pour éliminer les problèmes connus dans srv2. sys.

Dans la plupart des cas, vous remarquerez le problème d’une utilisation élevée du processeur dans le processus système. Avant de continuer, utilisez Process Explorer pour vous assurer que srv2. sys ou NTFS. sys consomme des ressources processeur excessives.

### <a name="storage-area-network-san-scenario"></a>Scénario de réseau de zone de stockage (SAN)

Dans les niveaux agrégés, les performances globales du SAN peuvent sembler très précises. Toutefois, lorsque vous utilisez des problèmes SMB, le temps de réponse de la requête individuelle est ce qui est le plus important.

En règle générale, ce problème peut être dû à une forme de mise en file d’attente de commandes dans le SAN. Vous pouvez utiliser **Perfmon** pour capturer un suivi **Microsoft-Windows-Storport** et l’analyser pour déterminer avec précision la réactivité du stockage.

### <a name="disk-io-latency"></a>Latence d’e/s disque

La latence d’e/s du disque est une mesure du délai entre la création et la fin d’une demande d’e/s disque.

La latence d’e/s mesurée dans Perfmon comprend tout le temps passé dans les couches matérielles plus le temps passé dans la file d’attente des pilotes de port Microsoft (Storport. sys pour SCSI). Si les processus en cours d’exécution génèrent une grande file d’attente StorPort, la latence mesurée augmente. Cela est dû au fait que les e/s doivent attendre avant d’être distribuées aux couches matérielles.

Dans Perfmon, les compteurs suivants montrent la latence du disque physique :

- « Objet de performance disque physique »-\> « moyenne disque s/lecture » : indique la latence de lecture moyenne.

- « Objet de performance disque physique »-\> « moyenne disque s/écriture » : indique la latence d’écriture moyenne.

- « Objet de performance disque physique »-\> « moyenne disque s/transfert » – affiche les moyennes combinées pour les lectures et les écritures.

L’instance « total\_» est une moyenne des latences pour tous les disques physiques de l’ordinateur. Chaque autre instance représente un disque physique individuel.

> [!NOTE]
> Ne confondez pas ces compteurs avec une moyenne de transferts disque/s. Il s’agit de compteurs complètement différents.

### <a name="windows-storage-stack-follows"></a>La pile de stockage Windows suit

Cette section fournit une brève explication sur la pile de stockage Windows.

Quand une application crée une demande d’e/s, elle envoie la demande au sous-système d’e/s de Windows en haut de la pile. Les e/s circulent ensuite tout au long de la pile vers le sous-système « disque » matériel. Ensuite, la réponse se déplace tout au long de la procédure de sauvegarde. Au cours de ce processus, chaque couche exécute sa fonction, puis transmet les e/s à la couche suivante.

![Déroulement de la pile](media/high-cpu-usage-issue-on-smb-server-1.png)

Perfmon ne crée pas de données de performances par seconde. Au lieu de cela, il consomme des données fournies par d’autres sous-systèmes dans Windows.

Pour l’objet de performance « disque physique », les données sont capturées au niveau « gestionnaire de partition » dans la pile de stockage.

Lorsque nous mesurons les compteurs mentionnés dans la section précédente, nous mesurons tout le temps passé par la demande sous le niveau « gestionnaire de partition ». Lorsque la requête d’e/s est envoyée par le gestionnaire de partition en aval de la pile, nous l’indiquons. À son retour, nous l’indiquons à nouveau et calculons la différence de temps. La différence de temps est la latence.

En procédant ainsi, nous comptons sur le temps passé dans les composants suivants :

- Pilote de classe : gère le type d’appareil, tels que les disques, les bandes, etc.

- Pilote de port : gère le protocole de transport, tel que SCSI, FC, SATA, et ainsi de suite.

- Pilote miniport de l’appareil : il s’agit du pilote de périphérique de la carte de stockage. Il est fourni par le fabricant des appareils, tels que le contrôleur RAID et le HBA FC.

- Sous-système de disque-cela inclut tout ce qui se trouve sous le pilote miniport de l’appareil. Cela peut être aussi simple qu’un câble qui est connecté à un seul disque dur physique, ou aussi complexe qu’un réseau de zone de stockage. Si le problème est dû à ce composant, vous pouvez contacter le fournisseur du matériel pour plus d’informations sur la résolution des problèmes.

### <a name="disk-queuing"></a>Mise en file d’attente des disques

Un sous-système de disque peut accepter une quantité limitée d’e/s à un moment donné. L’e/s excédentaire est mise en file d’attente jusqu’à ce que le disque puisse réaccepter les e/s. Le temps passé par les e/s dans les files d’attente sous le niveau « gestionnaire de partition » est comptabilisé dans les mesures de latence de disque physique Perfmon. À mesure que la taille des files d’attente augmente et que les e/s doivent attendre plus longtemps, la latence mesurée augmente également.

Il existe plusieurs files d’attente sous le niveau « gestionnaire de partition », comme suit :

- File d’attente de pilotes de port Microsoft-file d’attente SCSIport ou Storport

- File d’attente de pilotes de périphérique fournie par le fabricant-pilote de périphérique OEM

- Files d’attente matérielles, telles que la file d’attente du contrôleur de disque, la file d’attente des commutateurs SAN, la file du contrôleur de groupe et la file d’attente

Nous avons également pris en compte la durée pendant laquelle le disque dur passe activement à traiter les e/s et le temps de trajet utilisé pour que la demande retourne le niveau « gestionnaire de partition » pour être marqué comme terminé.

Enfin, nous devons prêter une attention particulière à la file d’attente du pilote de port (pour SCSI Storport. sys). Le pilote de port est le dernier composant Microsoft à toucher une e/s avant de le transférer au pilote de miniport de périphérique fourni par le fabricant.

Si le pilote miniport de périphérique ne peut pas accepter d’autres e/s car sa file d’attente ou les files d’attente matérielles en dessous sont saturées, nous allons commencer à accumuler les e/s dans la file d’attente du pilote de port. La taille de la file d’attente du pilote de port Microsoft est limitée uniquement par la mémoire système disponible (RAM) et peut devenir très volumineuse. Cela génère une latence mesurée importante.

## <a name="high-cpu-caused-by-enumerating-folders"></a>UC élevée provoquée par l’énumération de dossiers 

Pour résoudre ce problème, désactivez la fonctionnalité d’énumération basée sur l’accès (ABE).

Pour déterminer les partages SMB sur lesquels ABE est activé, exécutez la commande PowerShell suivante :

```PowerShell
Get-SmbShare | Select Name, FolderEnumerationMode
```

Unrestricted = ABE désactivé. <br />
AccessBase = ABE activé.


Vous pouvez activer ABE dans **Gestionnaire de serveur**. Naviguez vers **services de fichiers et de stockage** > les **partages**, cliquez avec le bouton droit sur le partage, sélectionnez **Propriétés**, accédez à **paramètres** , puis sélectionnez **activer l’énumération basée sur l’accès**.

![Options d’interface utilisateur](media/high-cpu-usage-issue-on-smb-server-2.png)

En outre, vous pouvez réduire **ABELevel** à un niveau inférieur (1 ou 2) pour améliorer les performances.

Vous pouvez vérifier les performances du disque lorsque l’énumération est lente en ouvrant le dossier localement par le biais d’une console ou d’une session RDP.
