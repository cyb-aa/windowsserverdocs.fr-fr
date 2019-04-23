---
title: Service de Migration de stockage Forum aux questions (FAQ)
description: Forum aux questions sur le Service de Migration de stockage, tels que les fichiers qui sont exclus de transferts lors de la migration d’un serveur vers un autre.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 11/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: df03f722b7b36a163693f675a2eaade2fabeb82f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860910"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>Service de Migration de stockage Forum aux questions (FAQ)

Cette rubrique contient des réponses aux questions fréquemment posées (FAQ) sur l’utilisation de [Service de Migration de stockage](overview.md) pour migrer des serveurs.

## <a name="excluded-files"></a> Quels fichiers et dossiers sont exclus des transferts ?

Service de Migration de stockage ne sont pas transférer des fichiers ou dossiers que nous savons peut interférer avec l’opération de Windows. Plus précisément, voici ce que nous ne transférez ou déplacer dans le dossier PreExistingData sur la destination :

- Windows, Program Files, Program Files (x86), données de programme, les utilisateurs
- $Recycle.bin, recycler, recyclage, System Volume Information, $ $UpgDrv, $SysReset, $Windows. ~ BT, $Windows. ~ LS, Windows.old, démarrage, récupération, Documents et paramètres
- pagefile.sys, hiberfil.sys, swapfile.sys, winpepge.sys, config.sys, bootsect.bak, bootmgr, bootnxt
- Les fichiers ou les dossiers sur le serveur source qui est en conflit avec les dossiers exclus de la destination. <br>Par exemple, s’il existe un dossier N:\Windows sur la source et il est mappé à la C:\ volume sur la destination, il ne transfert, quel que soit ce qu’il contient, car il entraînerait une interférence avec le dossier de système de C:\Windows sur la destination.

## <a name="domain-migration"></a> Migrations de domaine sont pris en charge ?

Service de Migration de stockage n’autorise pas la migration entre domaines Active Directory. Les migrations entre serveurs seront toujours joints du serveur de destination au même domaine. Vous pouvez utiliser les informations d’identification de la migration à partir de différents domaines dans la forêt Active Directory. Le Service de Migration de stockage ne prend pas en charge la migration entre des groupes de travail.  

## <a name="cluster-support"></a> Les clusters sont pris en charge en tant que sources ou des destinations ?

Service de Migration de stockage n’actuellement migrer entre des Clusters dans Windows Server 2019. Nous prévoyons d’ajouter la prise en charge de cluster dans une prochaine version du Service de Migration de stockage.

## <a name="local-principals"></a> Migrent les utilisateurs locaux et de groupes locaux ?

Service de Migration de stockage n’actuellement migrer les utilisateurs locaux ou des groupes locaux dans Windows Server 2019. Nous prévoyons d’ajouter la prise en charge de l’utilisateur local et migration d’un groupe local dans une prochaine version du Service de Migration de stockage.

## <a name="domain-controller"></a> Migration de contrôleur de domaine est pris en charge ?

Service de Migration de stockage ne migre actuellement des contrôleurs de domaine dans Windows Server 2019. Pour résoudre ce problème, tant que vous avez plusieurs contrôleurs de domaine dans le domaine Active Directory, rétrograder le contrôleur de domaine avant de les migrer, puis promouvoir la destination après le basculement se termine. Nous prévoyons d’ajouter le support de migration de contrôleur de domaine dans une prochaine version du Service de Migration de stockage.

## <a name="share-attributes"></a> Quels attributs sont migrés par le Service de Migration de stockage ?

Service de stockage Migration migre tous les indicateurs, paramètres et la sécurité des partages SMB. Cette liste d’indicateurs qui migre le Service de Migration de stockage comprend :

    - État d’un partage
    - Type de disponibilité
    - Type de partage
    - Mode d’énumération de dossier *(également appelé énumération basée accès ou ABE)*
    - Mode de mise en cache
    - Mode de bail
    - Instance de SMB
    - Délai d’expiration de l’autorité de certification
    - Limite d’utilisateurs simultanés
    - Disponible en continu
    - Description           
    - Chiffrer les données
    - Communication à distance de l’identité
    - Infrastructure
    - Nom
    - Path
    - Étendue
    - Nom de l'étendue
    - Descripteur de sécurité
    - Cliché instantané
    - Spéciale
    - Temporaire

## <a name="move-db"></a> Puis-je déplacer la base de données du Service de Migration de stockage ?

Le Service de Migration de stockage utilise une base de données de moteur ESE () de stockage extensible qui est installé par défaut dans le dossier c:\programdata\microsoft\storagemigrationservice masqué. Cette base de données augmente à mesure que les travaux sont ajoutés et transferts sont terminées et consomment l’espace du lecteur significatifs après la migration des millions de fichiers si vous ne supprimez pas les travaux. Si la base de données doit déplacer, procédez comme suit :

1. Arrêter le service « Service de Migration de stockage » sur l’ordinateur orchestrator.
2. S’approprier le `%programdata%/Microsoft/StorageMigrationService` dossier
3. Ajoutez votre compte d’utilisateur d’avoir un contrôle total sur qui partagent et tous ses fichiers et les sous-dossiers.
4. Déplacez le dossier vers un autre lecteur sur l’ordinateur orchestrator.
5. Définissez la valeur REG_SZ de Registre suivante :

    HKEY_Local_Machine\Software\Microsoft\SMS DatabasePath = *chemin d’accès au nouveau dossier de base de données sur un autre volume* . 
6. Assurez-vous que système dispose d’un contrôle total sur tous les fichiers et sous-dossiers de ce dossier
7. Supprimer vos propres autorisations de comptes.
8. Démarrez le service « Service de Migration de stockage ».

## <a name="transfer-threads"></a> Puis-je augmenter le nombre de fichiers copier simultanément ?

Le service de Proxy de Service de Migration de stockage copie 8 fichiers simultanément dans un travail donné. Ce service s’exécute sur l’orchestrateur pendant le transfert si les ordinateurs de destination sont Windows Server 2012 R2 ou Windows Server 2016, mais s’exécute également sur tous les nœuds de destination Windows Server 2019. Vous pouvez augmenter le nombre de threads de copie simultanées en ajustant le nom de valeur REG_DWORD de Registre suivant au format décimal sur chaque nœud qui exécute le Proxy de SMS :

    HKEY_Local_Machine\Software\Microsoft\SMSProxy
    FileTransferThreadCount

 La plage valide est 1 et 128 dans Windows Server 2019. 

 Après avoir modifié, vous devez redémarrer le service de Proxy de Service de Migration de stockage sur tous les ordinateurs partipating dans une migration. Nous prévoyons d’augmenter ce nombre dans une future version de Migration du Service de stockage.

## <a name="non-windows"></a> Puis-je migrer à partir de sources autres que Windows Server ?

La version du Service de Migration de stockage fournie dans Windows Server 2019 prend en charge la migration à partir de Windows Server 2003 et les systèmes d’exploitation ultérieurs. Actuellement, il ne peut pas migrer à partir de Linux, Samba, NetApp, EMC ou autres périphériques de stockage SAN et NAS. Nous prévoyons autoriser cela dans une future version de Service de Migration de stockage, en commençant par la prise en charge de Linux Samba.

## <a name="previous-versions"></a> Puis-je migrer des versions antérieures des fichiers ?

La version du Service de Migration de stockage fournie dans Windows Server 2019 ne prend pas en charge la migration les Versions précédentes (effectuées avec le service de cliché instantané de volume) de fichiers. Migre uniquement la version actuelle. 

## <a name="ntfs-refs"></a> Puis-je migrer à partir de NTFS vers REFS ?

La version du Service de Migration de stockage fournie dans Windows Server 2019 ne prend pas en charge la migration à partir de NTFS pour les systèmes de fichiers REFS. Vous pouvez migrer à partir de NTFS vers NTFS et REFS en ReFS. Il s’agit par conception, en raison des nombreuses différences dans les fonctionnalités, les métadonnées et les autres aspects ReFS ne duplique pas NTFS. ReFS est conçue comme un système de fichiers de la charge de travail application, pas un système de fichiers généraux. Pour plus d’informations, consultez [vue d’ensemble du système de fichiers résilient (ReFS)](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> Puis-je consolider plusieurs serveurs dans un seul serveur ?

La version du Service de Migration de stockage fournie dans Windows Server 2019 ne prend pas en charge la consolidation de plusieurs serveurs dans un seul serveur. Un exemple de consolidation serait effectuez la migration trois serveurs source distinct - qui peuvent avoir les mêmes noms de partage, les chemins de fichiers locaux - sur un nouveau serveur unique qui virtualisés ces chemins d’accès et les partages afin d’éviter tout chevauchement ou un conflit, puis a répondu trois les noms de serveurs précédents et l’adresse IP. Nous pouvons ajouter cette fonctionnalité dans une future version de la Migration du Service de stockage.  


## <a name="give-feedback"></a> Quelles sont mes options pour donner votre avis, signaler des bogues, ou obtenir un support technique ?

Pour envoyer des commentaires sur le Service de Migration de stockage :

- Utilisez l’outil de Hub de commentaires inclus dans Windows 10, en cliquant sur « Suggérer une fonctionnalité » et en spécifiant la catégorie de « Windows Server » et la sous-catégorie de « Migration de stockage »
- Utilisez le [Windows Server UserVoice](https://windowsserver.uservoice.com) site
- Messagerie smsfeed@microsoft.com

Pour signaler des bogues :

- Utilisez l’outil de Hub de commentaires inclus dans Windows 10, en cliquant sur « Signaler un problème » et en spécifiant la catégorie de « Windows Server » et la sous-catégorie de « Migration de stockage »
- Ouvrez une demande de support via [Support Microsoft](https://support.microsoft.com)

Pour obtenir la prise en charge :

 - Publier une question sur le [communauté technique de Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Publier sur le [Forum de Technet de Windows Server 2019](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - Ouvrez une demande de support via [Support Microsoft](https://support.microsoft.com)


## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble du Service de Migration de stockage](overview.md)
