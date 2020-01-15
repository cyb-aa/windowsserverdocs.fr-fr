---
title: Forum aux questions (FAQ) sur le service de migration de stockage
description: Forum aux questions sur le service de migration de stockage, comme les fichiers exclus des transferts lors de la migration d’un serveur vers un autre.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 08/19/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: b7a6dd37cfc054ead153d274ffa7f0d13844305e
ms.sourcegitcommit: 10331ff4f74bac50e208ba8ec8a63d10cfa768cc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75953031"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>Forum aux questions (FAQ) sur le service de migration de stockage

Cette rubrique contient des réponses aux questions fréquemment posées (FAQ) sur l’utilisation de [Storage migration service](overview.md) pour migrer des serveurs.

## <a name="what-files-and-folders-are-excluded-from-transfers"></a>Quels fichiers et dossiers sont exclus des transferts ?

Le service de migration du stockage ne transfère pas les fichiers ou dossiers que nous savons être en mesure d’interférer avec le fonctionnement de Windows. Plus précisément, voici ce que nous n’allons pas transférer ou déplacer dans le dossier PreExistingData de la destination :

- Windows, fichiers programme, Program Files (x86), données de programme, utilisateurs
- $Recycle. bin, recycler, recyclé, informations sur le volume système, $UpgDrv $, $SysReset, $Windows. ~ BT, $Windows. ~ LS, Windows. old, démarrage, récupération, documents et paramètres
- pagefile. sys, Hiberfil. sys, swap. sys, winpepge. sys, config. sys, Bootsect. bak, bootmgr, bootnxt
- Tous les fichiers ou dossiers sur le serveur source sont en conflit avec les dossiers exclus sur la destination. <br>Par exemple, s’il existe un dossier N:\Windows sur la source et qu’il est mappé à C:\ volume sur la destination, il n’est pas transféré, indépendamment de ce qu’il contient, car il interfère avec le dossier système C:\Windows sur la destination.

## <a name="are-domain-migrations-supported"></a>Les migrations de domaine sont-elles prises en charge ?

Le service de migration du stockage n’autorise pas la migration entre des domaines de Active Directory. Les migrations entre les serveurs vont toujours joindre le serveur de destination au même domaine. Vous pouvez utiliser les informations d’identification de migration à partir de différents domaines de la forêt Active Directory. Le service de migration de stockage prend en charge la migration entre les groupes de travail.  

## <a name="are-clusters-supported-as-sources-or-destinations"></a>Les clusters sont-ils pris en charge en tant que sources ou destinations ?

Le service de migration de stockage prend en charge la migration depuis et vers les clusters après l’installation de mises à jour cumulatives [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ou de mises à jour ultérieures. Cela comprend la migration d’un cluster source vers un cluster de destination, ainsi que la migration d’un serveur source autonome vers un cluster de destination à des fins de consolidation des appareils. 

## <a name="do-local-groups-and-local-users-migrate"></a>Les groupes locaux et les utilisateurs locaux migrent-ils ?

Le service de migration de stockage prend en charge la migration des utilisateurs et groupes locaux après l’installation de mises à jour cumulatives [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ou de mises à jour ultérieures. 

## <a name="is-domain-controller-migration-supported"></a>La migration du contrôleur de domaine est-elle prise en charge ?

Le service de migration de stockage ne migre pas actuellement les contrôleurs de domaine dans Windows Server 2019. En guise de solution de contournement, à condition que vous disposiez de plusieurs contrôleurs de domaine dans le domaine Active Directory, rétrogradez le contrôleur de domaine avant de le migrer, puis promouvez la destination une fois la coupure terminée. Si vous choisissez de migrer une source ou une destination de contrôleur de domaine, vous ne pourrez pas le découper. Vous ne devez jamais migrer des utilisateurs et des groupes lors de la migration depuis ou vers un contrôleur de domaine.

## <a name="what-attributes-are-migrated-by-the-storage-migration-service"></a>Quels attributs sont migrés par le service de migration de stockage ?

Storage migration service migre tous les indicateurs, paramètres et sécurité des partages SMB. Cette liste d’indicateurs migrés par le service de migration de stockage comprend les éléments suivants :

    - État du partage
    - Type de disponibilité
    - Type de partage
    - Mode d’énumération *des dossiers (également appelé énumération basée sur l’accès ou Abe)*
    - Mode de mise en cache
    - Mode de Leasing
    - Instance SMB
    - Délai d’expiration de l’AC
    - Limite d’utilisateurs simultanés
    - Disponible en continu
    - Description           
    - Chiffrer les données
    - Communication à distance des identités
    - Infrastructure
    - Nom
    - Chemin d’accès
    - Délimité
    - Nom de l'étendue
    - Descripteur de sécurité
    - Cliché instantané
    - Spéciale
    - Stockage

## <a name="can-i-consolidate-multiple-servers-into-one-server"></a>Puis-je consolider plusieurs serveurs en un seul serveur ?

La version du service de migration du stockage fournie dans Windows Server 2019 ne prend pas en charge la consolidation de plusieurs serveurs en un seul serveur. Un exemple de consolidation consiste à migrer trois serveurs sources distincts, qui peuvent avoir les mêmes noms de partage et chemins d’accès de fichiers locaux, sur un nouveau serveur qui a virtualisé ces chemins d’accès et partages pour empêcher tout chevauchement ou collision, puis de répondre aux trois noms et adresses IP des serveurs précédents. Toutefois, vous pouvez migrer des serveurs autonomes sur plusieurs ressources de serveur de fichiers sur un seul cluster. 

## <a name="can-i-migrate-from-sources-other-than-windows-server"></a>Puis-je effectuer une migration à partir de sources autres que Windows Server ?

Le service de migration de stockage prend en charge la migration à partir de serveurs samba Linux après l’installation de mises à jour cumulatives [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) ou de mises à jour ultérieures. Consultez la configuration requise pour obtenir la liste des versions de samba prises en charge et des distributions Linux.

## <a name="can-i-migrate-previous-file-versions"></a>Puis-je migrer des versions de fichier précédentes ?

La version du service de migration du stockage fournie dans Windows Server 2019 ne prend pas en charge la migration des versions précédentes (effectuées avec le service VSS) de fichiers. Seule la version actuelle sera migrée. 

## <a name="optimizing-inventory-and-transfer-performance"></a>Optimisation des performances d’inventaire et de transfert

Le service de migration de stockage contient un moteur de lecture et de copie multithread appelé service de proxy de migration de stockage, que nous avons conçu pour être à la fois rapide et apporter une fidélité des données inadaptée à de nombreux outils de copie de fichiers. Si la configuration par défaut est optimale pour de nombreux clients, il existe des moyens d’améliorer les performances de SMS pendant l’inventaire et le transfert.

- **Utilisez Windows Server 2019 pour le système d’exploitation de destination.** Windows Server 2019 contient le service de proxy de service de migration de stockage. Lorsque vous installez cette fonctionnalité et migrez vers des destinations Windows Server 2019, tous les transferts fonctionnent comme une ligne de vue directe entre la source et la destination. Ce service s’exécute sur l’orchestrateur pendant le transfert si les ordinateurs de destination sont Windows Server 2012 R2 ou Windows Server 2016, ce qui signifie que les transferts double-saut et sont beaucoup plus lents. Si plusieurs tâches s’exécutent avec des destinations Windows Server 2012 R2 ou Windows Server 2016, l’orchestrateur devient un goulot d’étranglement. 

- **Modifiez les threads de transfert par défaut.** Le service de proxy de migration de stockage copie 8 fichiers simultanément dans un travail donné. Vous pouvez augmenter le nombre de threads de copie simultanés en ajustant le Registre suivant REG_DWORD nom de la valeur en décimal sur chaque nœud exécutant le proxy de service de migration de stockage :

    HKEY_Local_Machine \Software\Microsoft\SMSProxy
    
    FileTransferThreadCount

   La plage valide est comprise entre 1 et 128 dans Windows Server 2019. Après modification, vous devez redémarrer le service de proxy du service de migration de stockage sur tous les ordinateurs participant à une migration. Soyez vigilant avec ce paramètre. la définition d’une valeur supérieure peut nécessiter des cœurs supplémentaires, des performances de stockage et une bande passante réseau. La définition d’une valeur trop élevée peut entraîner une réduction des performances par rapport aux paramètres par défaut.

- **Ajoutez des cœurs et de la mémoire.**  Nous recommandons vivement que les ordinateurs source, Orchestrator et de destination disposent d’au moins deux cœurs de processeur ou de deux processeurs virtuels. en plus, ils peuvent améliorer de manière significative les performances d’inventaire et de transfert, en particulier lorsqu’ils sont combinés avec FileTransferThreadCount (ci-dessus). Lors du transfert de fichiers plus volumineux que les formats Office habituels (gigaoctets ou plus), les performances de transfert tirent plus de mémoire que le minimum par défaut de 2 Go.

- **Créer plusieurs travaux.** Lors de la création d’un travail avec plusieurs sources de serveur, chaque serveur est contacté en série pour l’inventaire, le transfert et le basculement. Cela signifie que chaque serveur doit terminer sa phase avant le démarrage d’un autre serveur. Pour exécuter plusieurs serveurs en parallèle, il vous suffit de créer plusieurs travaux, chaque travail contenant un seul serveur. SMS prend en charge jusqu’à 100 travaux en cours d’exécution simultanément, ce qui signifie qu’un seul Orchestrator peut paralléliser plusieurs ordinateurs de destination Windows Server 2019. Nous vous déconseillons d’exécuter plusieurs tâches parallèles si vos ordinateurs de destination sont Windows Server 2016 ou Windows Server 2012 R2 comme sans le service proxy SMS s’exécutant sur la destination, l’orchestrateur doit effectuer tous les transferts et peut devenir un situ. La possibilité pour les serveurs de s’exécuter en parallèle à l’intérieur d’une seule tâche est une fonctionnalité que nous prévoyons d’ajouter dans une version ultérieure de SMS.

- **Utilisez SMB 3 avec des réseaux RDMA.** Si vous transférez à partir d’un ordinateur source Windows Server 2012 ou version ultérieure, SMB 3. x prend en charge le mode SMB direct et la mise en réseau RDMA. RDMA déplace la plus grande partie du coût de transfert des processeurs de la carte mère vers les processeurs de carte réseau intégrés, ce qui réduit la latence et l’utilisation du processeur du serveur. En outre, les réseaux RDMA tels que ROCE et iWARP ont généralement beaucoup plus de bande passante que le TCP/Ethernet classique, y compris 25, 50 et 100 Go de vitesses par interface. L’utilisation de SMB direct déplace généralement la limite de vitesse de transfert du réseau vers le stockage lui-même.   

- **Utilisez SMB 3 Multichannel.** Si vous transférez à partir d’un ordinateur source Windows Server 2012 ou version ultérieure, SMB 3. x prend en charge les copies multicanaux qui peuvent améliorer les performances de copie de fichiers. Cette fonctionnalité fonctionne automatiquement tant que la source et la destination possèdent toutes les deux :

   - Plusieurs cartes réseau
   - Une ou plusieurs cartes réseau prenant en charge la mise à l’échelle côté réception (RSS)
   - Une ou plusieurs cartes réseau configurées à l’aide de l’Association de cartes réseau
   - Une ou plusieurs cartes réseau prenant en charge RDMA

- **Mettre à jour les pilotes.** Le cas échéant, installez le dernier microprogramme de stockage et de boîtier du fournisseur, les derniers pilotes HBA des fournisseurs, le dernier microprogramme du fournisseur BIOS/UEFI, les derniers pilotes réseau du fournisseur et les derniers pilotes de carte mère sur les serveurs source, de destination et Orchestrator. Redémarrez les nœuds si nécessaire. Consultez la documentation de votre fournisseur de matériel pour la configuration du stockage partagé et du matériel réseau.

- **Activez le traitement hautes performances.** Vérifiez que les paramètres BIOS/UEFI des serveurs permettent de hautes performances, par exemple avec la désactivation de C-State, la définition de la vitesse de QPI, l’activation de NUMA et la définition de la fréquence mémoire la plus élevée. Assurez-vous que la gestion de l’alimentation dans Windows Server est définie sur hautes performances. Redémarrez si nécessaire. N’oubliez pas de les retourner aux États appropriés une fois la migration terminée. 

- **Réglage du matériel** Passez en revue les [instructions de réglage des performances pour Windows server 2016](https://docs.microsoft.com/windows-server/administration/performance-tuning/) afin de paramétrer les ordinateurs Orchestrator et de destination exécutant windows server 2019 et windows server 2016. La section sur le [réglage des performances du sous-système réseau](https://docs.microsoft.com/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics) contient des informations particulièrement précieuses.

- **Utilisez un stockage plus rapide.** Bien qu’il soit difficile de mettre à niveau la vitesse de stockage de l’ordinateur source, vous devez vous assurer que le stockage de destination est au moins aussi rapide que les performances d’e/s en écriture, car la source est aux performances d’e/s de lecture afin de garantir qu’il n’y a pas de goulot d’étranglement inutile dans les transferts. Si la destination est une machine virtuelle, assurez-vous que, au moins dans le cadre de la migration, elle s’exécute dans la couche de stockage la plus rapide de vos hôtes hyperviseur, par exemple sur le niveau Flash ou avec des clusters HCI espaces de stockage direct à l’aide d’espaces en miroir tout-Flash ou hybrides. Lorsque la migration SMS est terminée, la machine virtuelle peut être migrée dynamiquement vers un niveau ou un hôte plus lent.

- **Mettre à jour l’antivirus.** Assurez-vous toujours que votre source et votre destination exécutent la dernière version corrigée du logiciel antivirus pour garantir une surcharge minimale des performances. En guise de test, vous pouvez exclure *temporairement* l’analyse des dossiers que vous stockez ou migrez sur les serveurs source et de destination. Si vos performances de transfert sont améliorées, contactez le fournisseur de votre logiciel antivirus pour obtenir des instructions ou pour obtenir une version mise à jour du logiciel antivirus ou une explication de la dégradation des performances attendue.

## <a name="can-i-migrate-from-ntfs-to-refs"></a>Puis-je migrer de NTFS vers REFS ?

La version du service de migration du stockage fournie dans Windows Server 2019 ne prend pas en charge la migration des systèmes de fichiers NTFS vers REFS. Vous pouvez migrer de NTFS vers NTFS et REFS vers ReFS. Cela est lié à la conception, en raison des nombreuses différences de fonctionnalités, de métadonnées et d’autres aspects que ReFS ne duplique pas à partir de NTFS. ReFS est conçu comme un système de fichiers de charge de travail d’application, pas un système de fichiers général. Pour plus d’informations, consultez [vue d’ensemble du système de fichiers résilient (ReFS)](../refs/refs-overview.md) . 

## <a name="can-i-move-the-storage-migration-service-database"></a>Puis-je déplacer la base de données du service de migration de stockage ?

Le service de migration de stockage utilise une base de données ESE (Extensible Storage Engine) qui est installée par défaut dans le dossier c:\programdata\microsoft\storagemigrationservice masqué. Cette base de données croît au fur et à mesure que des tâches sont ajoutées et que les transferts sont effectués, et peut consommer un espace disque important après la migration de millions de fichiers si vous ne supprimez pas les travaux. Si la base de données doit être déplacée, procédez comme suit :

1. Arrêtez le service de migration de stockage sur l’ordinateur Orchestrator.
2. Appropriation du dossier `%programdata%/Microsoft/StorageMigrationService`
3. Ajoutez votre compte d’utilisateur pour avoir un contrôle total sur ce partage et tous ses fichiers et sous-dossiers.
4. Déplacez le dossier vers un autre lecteur sur l’ordinateur Orchestrator.
5. Définissez la valeur de REG_SZ de Registre suivante :

    HKEY_Local_Machine \Software\Microsoft\SMS DatabasePath = *chemin d’accès au nouveau dossier de base de données sur un autre volume* . 
6. Vérifier que le système dispose d’un contrôle total sur tous les fichiers et sous-dossiers de ce dossier
7. Supprimez vos propres autorisations de compte.
8. Démarrez le service de migration de stockage.

## <a name="give-feedback"></a>Quelles sont les options permettant d’envoyer des commentaires, de signaler des bogues ou d’obtenir un support ?

Pour envoyer des commentaires sur le service de migration de stockage :

- Utilisez l’outil Hub de commentaires inclus dans Windows 10, en cliquant sur « suggérer une fonctionnalité » et en spécifiant la catégorie « Windows Server » et la sous-catégorie « migration du stockage ».
- Utiliser le site [Windows Server UserVoice](https://windowsserver.uservoice.com)
- L’adresse e-mail smsfeed@microsoft.com

Pour signaler les bogues :

- Utilisez l’outil Hub de commentaires inclus dans Windows 10, en cliquant sur « signaler un problème » et en spécifiant la catégorie « Windows Server » et la sous-catégorie « migration du stockage ».
- Ouvrir un dossier de support via [support Microsoft](https://support.microsoft.com)

Pour bénéficier du support technique :

 - Publiez une question sur la [communauté des technologies Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - Publication sur le [Forum TechNet de Windows Server 2019](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - Ouvrir un dossier de support via [support Microsoft](https://support.microsoft.com)

## <a name="see-also"></a>Articles associés

- [Vue d’ensemble de Storage migration service](overview.md)
