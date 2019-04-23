---
title: Migrer un serveur de fichiers à l’aide du Service de Migration de stockage
description: Brève description de la rubrique pour les résultats de recherche
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 966f25eb0bd43513b3c544fb3dc97115ed668b28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872750"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>Service de Migration de stockage permet de migrer un serveur

Cette rubrique explique comment migrer un serveur, y compris ses fichiers et la configuration, vers un autre serveur à l’aide de [Service de Migration de stockage](overview.md) et Windows Admin Center. La migration s’effectue en trois étapes une fois que vous avez installé le service et ouvrir des ports de pare-feu nécessaires : l’inventaire de vos serveurs, de transférer des données et de basculer vers les nouveaux serveurs.

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>Étape 0 : Installez un Service de Migration de stockage et les ports de pare-feu

Avant de commencer, installez le Service de Migration de stockage et vous assurer que les ports de pare-feu nécessaires sont ouverts.

1. Vérifier le [exigences du Service de Migration de stockage](overview.md#requirements) et installer [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) sur votre PC ou un serveur d’administration si vous n’avez pas déjà.
2. Dans Windows Admin Center, se connecter au serveur orchestrator exécutant Windows Server 2019. <br>C’est le serveur que vous allez installer le Service de Migration de stockage sur et utiliser pour gérer la migration. Si vous effectuez une migration qu’un seul serveur, vous pouvez utiliser le serveur de destination tant qu’il s’exécute Windows Server 2019. Nous vous recommandons de qu'utiliser un serveur d’orchestration distincte pour les migrations de serveurs multiples.
1. Accédez à **le Gestionnaire de serveur** (dans Windows Admin Center) > **Service de Migration de stockage** et sélectionnez **installer** pour installer le Service de Migration de stockage et ses composants requis (voir Figure 1).
    ![Capture d’écran de la page de Service de Migration de stockage montrant le bouton Installer](media/migrate/install.png) **Figure 1 : Installation du Service de Migration de stockage**
1. Installer le proxy du Service de Migration de stockage sur tous les serveurs de destination exécutant Windows Server 2019. Cela double la vitesse de transfert lors de l’installation sur les serveurs de destination. <br>Pour ce faire, connectez-vous au serveur de destination dans Windows Admin Center et passez à **le Gestionnaire de serveur** (dans Windows Admin Center) > **des rôles et fonctionnalités**, sélectionnez **Service de Migration de stockage Proxy**, puis sélectionnez **installer**.
1. Sur tous les serveurs sources et sur les serveurs de destination exécutant Windows Server 2012 R2 ou Windows Server 2016, dans Windows Admin Center, se connecter à chaque serveur, accédez à **le Gestionnaire de serveur** (dans Windows Admin Center) > **pare-feu**   >  **Les règles entrantes**, puis vérifiez que les règles suivantes sont activées :
    - Partage de fichiers et d’imprimantes (SMB-Entrée)
    - Service accès réseau (NP-Entrée)
    - Windows Management Instrumentation (DCOM-In)
    - Windows Management Instrumentation (WMI-In)

   > [!NOTE]
   > Si vous utilisez des pare-feu tiers, les plages de port d’entrée pour ouvrir sont TCP/445 (SMB), TCP/135 (mappeur de point de terminaison RPC/DCOM) et TCP 1025-65535 (ports éphémères DCOM/RPC).

1. Si vous utilisez un serveur orchestrator pour gérer la migration et que vous souhaitez télécharger les événements ou un journal de quelles données vous transférer, vérifiez que la règle de pare-feu (SMB-In) partage de fichiers et imprimantes est activée sur ce serveur.

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>Étape 1 : Créer un travail et l’inventaire de vos serveurs pour déterminer les éléments à migrer

Dans cette étape, vous spécifiez les serveurs à migrer et puis les analyser pour collecter des informations sur leurs fichiers et les configurations.

1. Sélectionnez **nouveau travail**, nommez la tâche, puis sélectionnez **OK**.
1. Sur le **Entrez les informations d’identification** page, tapez les informations d’identification d’administrateur qui fonctionnent sur les serveurs que vous souhaitez migrer à partir de, puis sélectionnez **suivant**.
1. Sélectionnez **ajouter un appareil**, tapez un nom de serveur source, puis sélectionnez **OK**. <br>Répétez cette procédure pour tous les autres serveurs à inventorier.
1. Sélectionnez **démarrer l’analyse**.<br>Les page mises à jour s’affiche une fois l’analyse est terminée.
    ![Capture d’écran montrant un serveur prêt à être analysés](media/migrate/inventory.png) **Figure 2 : Inventaire des serveurs**
1. Sélectionnez chacun des serveurs pour passer en revue les partages, la configuration, les cartes réseau et les volumes qui ont été inventoriés. <br><br>Service de Migration de stockage ne sont pas transférer des fichiers ou dossiers que nous savons peut interférer avec l’opération de Windows, dans cette version, vous verrez des avertissements pour tous les partages situés dans le dossier de système de Windows. Vous devez ignorer ces partages pendant la phase de transfert. Pour plus d’informations, consultez [quels fichiers et dossiers sont exclus de transferts](faq.md#excluded-files).
1. Sélectionnez **suivant** pour passer à transférer des données.

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>Étape 2 : Transférer des données à partir de vos anciens serveurs vers les serveurs de destination

Dans cette étape vous transférer des données après la spécification de son emplacement sur le serveur de destination.

 1. Sur le **transférer des données** > **Entrez les informations d’identification** page, tapez les informations d’identification d’administrateur qui fonctionnent sur les serveurs de destination que vous souhaitez migrer vers, puis sélectionnez **suivant**.
 1. Sur le **ajouter un périphérique de destination et les mappages** page, le premier serveur source est répertorié. Tapez le nom du serveur auquel vous souhaitez migrer, puis sélectionnez **rechercher sur l’appareil**.
 1. Mapper les volumes source aux volumes de destination, désactivez la **Include** case à cocher pour tous les partages vous souhaitez transférer (y compris les partages administratifs situés dans le dossier du système Windows), puis sélectionnez **suivant** .
    ![Capture d’écran montrant un serveur source et ses volumes et les partages et où ils amèneront transférés sur la destination](media/migrate/transfer.png) **Figure 3 : Un serveur source et où son stockage est transféré vers**
 1. Ajouter des mappages pour des serveurs source et un serveur de destination, puis sélectionnez **suivant**.
 1. Si vous le souhaitez ajuster les paramètres de transfert, puis sélectionnez **suivant**.
 1. Sélectionnez **Validate** , puis sélectionnez **suivant**.
 1. Sélectionnez **démarrer Transfert** pour démarrer le transfert de données.<br>La première fois que vous transférez, nous passerons tous les fichiers existants dans une destination à un dossier de sauvegarde. Sur les transferts suivants, par défaut, nous allons actualiser la destination sans sauvegardant en premier. <br>En outre, Service de Migration de stockage est suffisamment intelligent pour gérer avec chevauchement des partages, nous ne copier les mêmes dossiers que deux fois dans la même tâche.
 1. Une fois le transfert terminé, consultez le serveur de destination pour vous assurer que tous les éléments transférés correctement. Sélectionnez **journal des erreurs** si vous souhaitez télécharger un journal de tous les fichiers qui n’a pas de transfert.

  > [!NOTE]
  > Si vous souhaitez conserver une piste d’audit des transferts d’ou que vous comptez exécuter plus d’un transfert d’un travail, cliquez sur **journal de transfert** pour enregistrer une copie de volume partagé de cluster. Chaque transfert ultérieur remplace les informations de base de données d’une exécution précédente. 

À ce stade, vous avez trois options :

- **Accédez à l’étape suivante**, coupez sur afin que les serveurs de destination adoptent les identités des serveurs source.
- **Envisagez la migration complète** sans prise en charge les identités des serveurs de la source.
- **Transférer de nouveau**, copie uniquement les fichiers qui ont été mis à jour depuis le dernier transfert.

Si votre objectif est de synchroniser les fichiers avec Azure, vous pouvez configurer les serveurs de destination avec Azure File Sync après le transfert de fichiers, ou après la découpe sur vers les serveurs de destination (consultez [planification d’un déploiement d’Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)).

## <a name="step-3-optionally-cut-over-to-the-new-servers"></a>Étape 3 : Si vous le souhaitez basculer vers les nouveaux serveurs

Dans cette étape vous basculer à partir des serveurs source pour les serveurs de destination, déplacer les adresses IP et les noms d’ordinateurs pour les serveurs de destination. Une fois cette étape terminée, applications et les utilisateurs accéder les nouveaux serveurs via les noms et adresses des serveurs que vous avez migré à partir de.

 1. Si vous avez accédé en dehors de la tâche de migration dans Windows Admin Center, accédez à **le Gestionnaire de serveur** > **Service de Migration de stockage** , puis sélectionnez le travail que vous souhaitez effectuer. 
 1. Sur le **basculer vers les nouveaux serveurs** > **Entrez les informations d’identification** page, sélectionnez **suivant** à utiliser les informations d’identification que vous avez tapé précédemment.
 1. Sur le **configurer le basculement** page, spécifiez les cartes réseau pour contrôler les paramètres de l’appareil de chaque source. Cette opération déplace l’adresse IP de la source vers la destination dans le cadre du basculement.
 1. Spécifiez quelle adresse IP pour l’utiliser pour le serveur source après le basculement déplace son adresse à la destination. Vous pouvez utiliser DHCP ou une adresse statique. Si vous utilisez une adresse statique, le nouveau sous-réseau doit être qu'identique à l’ancien sous-réseau ou le basculement échouera.
    ![Capture d’écran montrant un serveur source et ses adresses IP et noms d’ordinateur et qu’ils amèneront remplacés par après le basculement](media/migrate/cutover.png)
    **Figure 4 : Un serveur source et la manière dont sa configuration réseau déplace vers la destination**
 1. Spécifiez comment renommer le serveur source une fois que le serveur de destination est sur son nom. Vous pouvez utiliser un nom généré de manière aléatoire ou tapez un vous-même. Puis sélectionnez **suivant**.
 1. Sélectionnez **suivant** sur le **ajuster les paramètres de basculement** page.
 1. Sélectionnez **Validate** sur le **appareil source et destination de valider** page, puis sélectionnez **suivant**.
 1. Lorsque vous êtes prêt à effectuer le basculement, sélectionnez **démarrer le basculement**. <br>Utilisateurs et des applications peuvent produire une interruption pendant que l’adresse et les noms sont déplacés et les serveurs redémarré plusieurs fois chacune, mais sinon seront pas affectées par la migration. La durée pendant laquelle le basculement dépend de la vitesse à laquelle les serveurs de redémarrent, ainsi que les temps de réplication Active Directory et DNS.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble du Service de Migration de stockage](overview.md)
- [Services de Migration de stockage Forum aux questions (FAQ)](faq.md)
- [Planification d’un déploiement d’Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)