---
title: Service de Migration de stockage Forum aux questions (FAQ)
description: Forum aux questions sur le Service de Migration de stockage, tels que les fichiers qui sont exclus de transferts lors de la migration d’un serveur vers un autre.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 06/04/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 8f0f16f14ccf9099af8ff8bb8b27209c75c87cfc
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284468"
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

## <a name="non-windows"></a> Puis-je migrer à partir de sources autres que Windows Server ?

La version du Service de Migration de stockage fournie dans Windows Server 2019 prend en charge la migration à partir de Windows Server 2003 et les systèmes d’exploitation ultérieurs. Actuellement, il ne peut pas migrer à partir de Linux, Samba, NetApp, EMC ou autres périphériques de stockage SAN et NAS. Nous prévoyons autoriser cela dans une future version de Service de Migration de stockage, en commençant par la prise en charge de Linux Samba.

## <a name="previous-versions"></a> Puis-je migrer des versions antérieures des fichiers ?

La version du Service de Migration de stockage fournie dans Windows Server 2019 ne prend pas en charge la migration les Versions précédentes (effectuées avec le service de cliché instantané de volume) de fichiers. Migre uniquement la version actuelle. 

## <a name="ntfs-refs"></a> Puis-je migrer à partir de NTFS vers REFS ?

La version du Service de Migration de stockage fournie dans Windows Server 2019 ne prend pas en charge la migration à partir de NTFS pour les systèmes de fichiers REFS. Vous pouvez migrer à partir de NTFS vers NTFS et REFS en ReFS. Il s’agit par conception, en raison des nombreuses différences dans les fonctionnalités, les métadonnées et les autres aspects ReFS ne duplique pas NTFS. ReFS est conçue comme un système de fichiers de la charge de travail application, pas un système de fichiers généraux. Pour plus d’informations, consultez [vue d’ensemble du système de fichiers résilient (ReFS)](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> Puis-je consolider plusieurs serveurs dans un seul serveur ?

La version du Service de Migration de stockage fournie dans Windows Server 2019 ne prend pas en charge la consolidation de plusieurs serveurs dans un seul serveur. Un exemple de consolidation serait effectuez la migration trois serveurs source distinct - qui peuvent avoir les mêmes noms de partage, les chemins de fichiers locaux - sur un nouveau serveur unique qui virtualisés ces chemins d’accès et les partages afin d’éviter tout chevauchement ou un conflit, puis a répondu trois les noms de serveurs précédents et l’adresse IP. Nous pouvons ajouter cette fonctionnalité dans une future version de la Migration du Service de stockage.  

## <a name="optimize"></a> Optimisation des performances de l’inventaire et de transfert

Le Service de Migration de stockage contient un moteur de copie appelé service de Proxy de Service de Migration de stockage qui nous avons conçu pour être à la fois rapide mais aussi d’intégrer n’ayant pas de données idéal haute fidélité dans de nombreux outils de copie de fichier et de lecture multithread. Alors que la configuration par défaut sera optimale pour de nombreux clients, il existe des moyens d’améliorer les performances de SMS lors de l’inventaire et de transfert.

- **Utilisez Windows Server 2019 pour le système d’exploitation de destination.** Windows Server 2019 contient le service de Proxy de Service de Migration de stockage. Lorsque vous installez cette fonctionnalité et migrez vers Windows Server 2019 destinations, tous les transferts de fonctionnent comme une visibilité directe entre la source et de destination. Ce service s’exécute sur l’orchestrateur pendant le transfert si les ordinateurs de destination sont Windows Server 2012 R2 ou Windows Server 2016, ce qui signifie que les transferts de double-saut et seront considérablement ralenties. S’il existe plusieurs travaux en cours d’exécution avec Windows Server 2012 R2 ou Windows Server 2016 destinations, l’orchestrateur devient un goulot d’étranglement. 

- **Modifier les threads de transfert par défaut.** Le service de Proxy de Service de Migration de stockage copie 8 fichiers simultanément dans un travail donné. Vous pouvez augmenter le nombre de threads de copie simultanées en ajustant le nom de valeur REG_DWORD de Registre suivant au format décimal sur chaque nœud qui exécute le Proxy de SMS :

    HKEY_Local_Machine\Software\Microsoft\SMSProxy   FileTransferThreadCount

   La plage valide est 1 et 128 dans Windows Server 2019. Après avoir modifié, vous devez redémarrer le service de Proxy de Service de Migration de stockage sur tous les ordinateurs participant à une migration. Soyez prudent avec ce paramètre ; définition la plus élevée peut nécessiter davantage de noyaux, les performances de stockage et la bande passante réseau. Trop élevée peut entraîner une dégradation des performances par rapport aux paramètres par défaut. La possibilité de manière heuristique modifier les paramètres de thread en fonction du processeur, mémoire, réseau et de stockage est prévue pour une version ultérieure de SMS.

- **Ajoutez les cœurs et la mémoire.**  Nous recommandons vivement que les ordinateurs source et destination orchestrator ont au moins deux cœurs de processeur ou de deux processeurs virtuels, et bien plus encore pouvant aider considérablement le stock et le transfert des performances, en particulier lorsqu’elles sont combinées avec FileTransferThreadCount (ci-dessus). Lors du transfert de fichiers qui sont plus grands que les formats Office habituels (gigaoctets ou plus) les performances de transfert bénéficient de plus de mémoire que le minimum de 2 Go par défaut.

- **Créer plusieurs travaux.** Lorsque vous créez un travail avec plusieurs sources de serveur, chaque serveur est contacté en série pour le stock, transfert et le basculement. Cela signifie que chaque serveur doit terminer sa phase avant le démarrage d’un autre serveur. Pour exécuter davantage de serveurs en parallèle, créez simplement plusieurs tâches, avec chaque tâche contenant uniquement un des serveurs. SMS prend en charge jusqu'à 100 s’exécutant simultanément des travaux, ce qui signifie qu’un seul orchestrator pouvez paralléliser de nombreux ordinateurs de destination Windows Server 2019. Nous ne recommandons pas l’exécution de plusieurs travaux parallèles si vos ordinateurs de destination sont Windows Server 2016 ou Windows Server 2012 R2 sans que le service de proxy SMS en cours d’exécution sur la destination, l’orchestrateur doit effectuer tous les transferts de lui-même et peut devenir un goulot d’étranglement. La possibilité pour les serveurs à s’exécuter en parallèle à l’intérieur d’un seul travail est une fonctionnalité que nous prévoyons d’ajouter dans une version plus récente de SMS.

- **Utiliser SMB 3 avec les réseaux RDMA.** Si le transfert à partir d’un Windows Server 2012 ou ultérieur ordinateur source, SMB SMB 3.x prend en charge le mode Direct et de mise en réseau RDMA. RDMA déplace la plupart des coût UC de transfert à partir de la carte mère processeurs aux processeurs de carte réseau intégrées, ce qui réduit la latence et le serveur de l’utilisation du processeur. En outre, les réseaux RDMA comme ROCE et iWARP généralement la bande passante sensiblement plus élevée que TCP classique/ethernet, y compris les 25, 50 et vitesses de 100 Go par interface. À l’aide de SMB Direct généralement déplace la limite de vitesse de transfert à partir du réseau vers le stockage lui-même.   

- **Utilisez les 3 SMB multichannel.** Si le transfert d’un Windows Server 2012 ou d’un ordinateur de source plus loin, SMB 3.x prend en charge multicanal copie qui peut améliorer considérablement les fichiers des performances de copie. Cette fonctionnalité fonctionne automatiquement tant que la source et destination comportent :

   - Plusieurs cartes réseau
   - Une ou plusieurs cartes réseau qui prennent en charge la mise à l’échelle côté réception (RSS)
   - Une des plus de cartes réseau qui sont configurées en utilisant l’association de cartes réseau
   - Une ou plusieurs cartes réseau prenant en charge RDMA

- **Mettre à jour des pilotes.** Comme il convient, installez-y dernier stockage de fournisseur et du microprogramme du boîtier et pilotes derniers pilotes du fournisseur HBA, dernière version du microprogramme BIOS/UEFI de fournisseur, derniers pilotes réseau du fournisseur et derniers pilotes de chipsets de carte mère orchestrator, source et destination serveurs. Redémarrez les nœuds si besoin. Consultez la documentation de votre fournisseur de matériel pour la configuration du stockage partagé et du matériel réseau.

- **Permettre un traitement hautes performances.** Vérifiez que les paramètres BIOS/UEFI des serveurs permettent de hautes performances, par exemple avec la désactivation de C-State, la définition de la vitesse de QPI, l’activation de NUMA et la définition de la fréquence mémoire la plus élevée. Vérifiez la gestion de l’alimentation dans Windows Server est définie à hautes performances. Redémarrez si nécessaire. N’oubliez pas de les retourner aux États appropriés après avoir effectué la migration. 

- **Paramétrer le matériel** révision le [performances réglage des instructions pour Windows Server 2016](https://docs.microsoft.com/windows-server/administration/performance-tuning/) pour le réglage de l’orchestrator et les ordinateurs de destination exécutant Windows Server 2019 et de Windows Server 2016. Le [réglage des performances réseau sous-système](https://docs.microsoft.com/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics) section contient des informations particulièrement utiles.

- **Utilisation du stockage plus rapide.** S’il peut être difficile de mettre à niveau de la vitesse de stockage d’ordinateur source, vous devez vous assurer que le stockage de destination est au moins aussi rapide sur les performances d’e/s écriture, la source est au niveau de performances d’e/s lecture afin de vérifier qu'il n’existe aucun goulot d’étranglement inutile dans les transferts. Si la destination est une machine virtuelle, assurez-vous que, au moins dans le cadre de la migration, il s’exécute dans la couche de stockage le plus rapide de vos hôtes hyperviseur, comme sur la couche de flash ou avec les clusters de HCL directe des espaces de stockage utilisant la mise en miroir exclusivement flash ou espaces de hybride. Lors de la migration de SMS est terminée la machine virtuelle peut être migrées dynamiquement à un hôte ou un niveau plus lent.

- **Mettre à jour antivirus.** Vérifiez toujours votre source et destination exécutent la dernière version corrigée du logiciel antivirus pour garantir des performances minimal. Comme un test, vous pouvez *temporairement* exclure l’analyse des dossiers vous inventaire ou sur les serveurs source et de destination. Si vos performances de transfert a été amélioré, contactez votre fournisseur de logiciel antivirus pour obtenir des instructions ou pour une version mise à jour du logiciel antivirus ou une explication de la dégradation des performances attendues.

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
