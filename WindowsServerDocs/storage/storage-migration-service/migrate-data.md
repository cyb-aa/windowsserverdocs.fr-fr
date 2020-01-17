---
title: Migrer un serveur de fichiers à l’aide du service de migration de stockage
description: Brève description de la rubrique relative aux résultats du moteur de recherche
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 4da69087ab1df6200394b36c938cb05ec5185045
ms.sourcegitcommit: 3f54036c74c5a67799fbc06a8a18a078ccb327f9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124887"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>Utiliser Storage migration service pour migrer un serveur

Cette rubrique explique comment migrer un serveur, y compris ses fichiers et sa configuration, vers un autre serveur à l’aide du [service de migration de stockage](overview.md) et du centre d’administration Windows. La migration prend trois étapes une fois que vous avez installé le service et ouvert les ports de pare-feu nécessaires : inventoriez vos serveurs, transférez les données et coupez les serveurs.

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>Étape 0 : installer le service de migration de stockage et vérifier les ports du pare-feu

Avant de commencer, installez le service de migration de stockage et assurez-vous que les ports de pare-feu nécessaires sont ouverts.

1. Vérifiez les [exigences du service de migration de stockage](overview.md#requirements) et installez le [Centre d’administration Windows](../../manage/windows-admin-center/understand/windows-admin-center.md) sur votre ordinateur ou un serveur d’administration si vous ne l’avez pas déjà fait. Si vous migrez des ordinateurs sources joints à un domaine, vous devez installer et exécuter le service de migration de stockage sur un serveur joint au même domaine ou à la même forêt que les ordinateurs sources.
2. Dans le centre d’administration Windows, connectez-vous au serveur Orchestrator exécutant Windows Server 2019. <br>Il s’agit du serveur sur lequel vous allez installer le service de migration de stockage et utilisé pour gérer la migration. Si vous effectuez la migration d’un seul serveur, vous pouvez utiliser le serveur de destination tant qu’il exécute Windows Server 2019. Nous vous recommandons d’utiliser un serveur d’orchestration distinct pour les migrations de plusieurs serveurs.
1. Accédez à **Gestionnaire de serveur** (dans le centre d’administration Windows) > **service de migration de stockage** et sélectionnez **installer** pour installer le service de migration de stockage et ses composants requis (voir figure 1).
    ![capture d’écran de la page service de migration du stockage montrant le bouton installer](media/migrate/install.png) **figure 1 : installation de Storage migration service**
1. Installez le proxy service de migration de stockage sur tous les serveurs de destination exécutant Windows Server 2019. Cela double la vitesse de transfert lorsqu’elle est installée sur les serveurs de destination. <br>Pour ce faire, connectez-vous au serveur de destination dans le centre d’administration Windows, puis accédez à **Gestionnaire de serveur** (dans le centre d’administration windows) > **rôles et fonctionnalités**, sélectionnez **proxy de service de migration de stockage**, puis sélectionnez **installer**.
1. Sur tous les serveurs sources et sur les serveurs de destination exécutant Windows Server 2012 R2 ou Windows Server 2016, dans le centre d’administration Windows, connectez-vous à chaque serveur, accédez à **Gestionnaire de serveur** (dans le centre d’administration windows) > **pare-feu** > **Règles entrantes**, puis vérifiez que les règles suivantes sont activées :
    - Partage de fichiers et d’imprimantes (SMB-Entrée)
    - Service Netlogon (NP-in)
    - Windows Management Instrumentation (DCOM-In)
    - Windows Management Instrumentation (WMI-In)

   Si vous utilisez des pare-feu tiers, les plages de ports entrants à ouvrir sont TCP/445 (SMB), TCP/135 (mappeur de point de terminaison RPC/DCOM) et TCP 1025-65535 (ports éphémères RPC/DCOM). Les ports de service de migration du stockage sont TCP/28940 (Orchestrator) et TCP/28941 (proxy).

1. Si vous utilisez un serveur Orchestrator pour gérer la migration et que vous souhaitez télécharger des événements ou un journal des données que vous transférez, vérifiez que la règle de pare-feu partage de fichiers et d’imprimantes (SMB-in) est également activée sur ce serveur.

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>Étape 1 : créer un travail et inventorier vos serveurs pour déterminer les éléments à migrer

Au cours de cette étape, vous allez spécifier les serveurs à migrer, puis les analyser pour collecter des informations sur leurs fichiers et leurs configurations.

1. Sélectionnez **nouveau travail**, nommez le travail, puis indiquez si vous souhaitez migrer les serveurs Windows et les clusters ou les serveurs Linux qui utilisent samba. Sélectionnez **OK**.
2. Dans la page **entrer les informations d’identification** , tapez les informations d’identification d’administrateur qui fonctionnent sur les serveurs à partir desquels vous souhaitez effectuer la migration, puis sélectionnez **suivant**. <br>Si vous effectuez une migration à partir de serveurs Linux, vous devez entrer les informations d’identification dans les pages informations d’identification **samba** et **informations d’identification Linux** , y compris un mot de passe SSH ou une clé privée. 

3. Sélectionnez **Ajouter un appareil**, tapez un nom de serveur source ou le nom d’un serveur de fichiers en cluster, puis sélectionnez **OK**. <br>Répétez cette opération pour tous les autres serveurs que vous souhaitez inventorier.

4. Sélectionnez **Démarrer l’analyse**.<br>La page se met à jour pour afficher une fois l’analyse terminée.
    Capture d’écran ![montrant un serveur prêt à être analysé](media/migrate/inventory.png) **figure 2 : inventaire des serveurs**
5. Sélectionnez chaque serveur pour passer en revue les partages, la configuration, les cartes réseau et les volumes qui ont été inventoriés. <br><br>Le service de migration du stockage ne transfère pas les fichiers ou dossiers que nous connaissons pourraient interférer avec l’opération Windows. dans cette version, vous verrez des avertissements pour tous les partages situés dans le dossier système de Windows. Vous devrez ignorer ces partages pendant la phase de transfert. Pour plus d’informations, consultez [Quels fichiers et dossiers sont exclus des transferts](faq.md#what-files-and-folders-are-excluded-from-transfers).
6. Sélectionnez **suivant** pour passer au transfert de données.

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>Étape 2 : transférer les données de vos anciens serveurs vers les serveurs de destination

Au cours de cette étape, vous allez transférer des données après avoir spécifié où les placer sur les serveurs de destination.

1. Dans la page **transférer les données** > entrer les **informations d’identification** , tapez les informations d’identification d’administrateur qui fonctionnent sur les serveurs de destination vers lesquels vous souhaitez effectuer la migration, puis sélectionnez **suivant**.
2. Sur la page **Ajouter un appareil et des mappages de destination** , le premier serveur source est listé. Tapez le nom du serveur ou du serveur de fichiers en cluster vers lequel vous souhaitez effectuer la migration, puis sélectionnez **analyser l’appareil**. Si vous migrez à partir d’un ordinateur source joint à un domaine, le serveur de destination doit être joint au même domaine. Vous pouvez également cliquer sur « créer une nouvelle machine virtuelle Azure », puis utiliser l’Assistant pour déployer un nouveau serveur de destination dans Azure. Cela permet de dimensionner automatiquement votre machine virtuelle, d’approvisionner le stockage, de formater des disques, de joindre le domaine et d’ajouter le proxy de service de migration de stockage à une destination Windows Server 2019. Vous pouvez choisir parmi les machines virtuelles Windows Server 2019 (recommandé), Windows Server 2016 et Windows Server 2012 R2 de toute taille et utiliser des disques gérés.   

 > [!NOTE]
   > L’utilisation de la « création d’une machine virtuelle Azure » requiert les éléments suivants :
   > - Un abonnement Azure valide.
   > - Un groupe de ressources Azure Compute existant dans lequel vous disposez de droits de création.
   > - Un réseau virtuel et un sous-réseau Azure existants. 
   > - Une solution de routage ou VPN Azure Express liée au réseau virtuel et au sous-réseau qui permet la connectivité à partir de cette machine virtuelle Azure IaaS sur vos clients locaux, les contrôleurs de domaine, l’ordinateur d’Orchestrator du service de migration de stockage, l’ordinateur du centre d’administration Windows, et l’ordinateur source à migrer.

3. Mappez les volumes source aux volumes de destination, désactivez la case à cocher **inclure** pour tous les partages que vous ne souhaitez pas transférer (y compris les partages administratifs situés dans le dossier système Windows), puis sélectionnez **suivant**.
   ![capture d’écran montrant un serveur source et ses volumes et partages et l’emplacement de transfert vers le](media/migrate/transfer.png) de destination **figure 3 : un serveur source et l’endroit où son stockage sera transféré**
4. Ajoutez un serveur de destination et des mappages pour d’autres serveurs sources, puis sélectionnez **suivant**.
5. Sur la page **ajuster les paramètres de transfert** , indiquez si vous souhaitez migrer les utilisateurs et groupes locaux sur les serveurs sources, puis sélectionnez **suivant**. Cela vous permet de recréer tous les utilisateurs et groupes locaux sur les serveurs de destination afin que les autorisations de fichier ou de partage définies sur utilisateurs et groupes locaux ne soient pas perdues. Voici les options de migration des utilisateurs et groupes locaux :

    - **Renommer les comptes avec le même nom** est sélectionné par défaut et migre tous les utilisateurs et groupes locaux sur le serveur source. S’il trouve des utilisateurs ou des groupes locaux portant le même nom sur la source et la destination, il les renomme à la destination, sauf s’ils sont intégrés (par exemple, l’utilisateur administrateur et le groupe administrateurs). N’utilisez pas ce paramètre si votre serveur source ou de destination est un contrôleur de domaine.
    - **Réutiliser des comptes portant le même nom** mappe les utilisateurs et les groupes portant le même nom sur la source et la destination. N’utilisez pas ce paramètre si votre serveur source ou de destination est un contrôleur de domaine.
    - **Ne pas transférer les utilisateurs et les groupes** ignore la migration des utilisateurs et des groupes locaux, ce qui est nécessaire lorsque votre source ou destination est un contrôleur de domaine, ou lorsque les données sont amorcées pour réplication DFS (réplication DFS ne prend pas en charge les groupes et les utilisateurs locaux).

   > [!NOTE]
   > Les comptes d’utilisateurs migrés sont désactivés sur la destination et un mot de passe de 127 caractères qui est à la fois complexe et aléatoire, vous devez les activer et attribuer un nouveau mot de passe lorsque vous avez terminé de continuer à les utiliser. Cela permet de garantir que tous les anciens comptes avec des mots de passe oubliés et faibles sur la source ne continuent pas à être un problème de sécurité sur la destination. Vous pouvez également consulter la solution de [mot de passe administrateur local](https://www.microsoft.com/download/details.aspx?id=46899) pour gérer les mots de passe d’administrateur local.

6. Sélectionnez **valider** , puis sélectionnez **suivant**.
7. Sélectionnez **Démarrer le transfert** pour commencer à transférer des données.<br>La première fois que vous transférez, nous allons déplacer tous les fichiers existants dans une destination vers un dossier de sauvegarde. Lors des transferts suivants, par défaut, nous actualiserons la destination sans la sauvegarder en premier. <br>En outre, le service de migration du stockage est suffisamment intelligent pour traiter les partages qui se chevauchent : nous ne copions pas deux fois les mêmes dossiers dans le même travail.
8. Une fois le transfert terminé, vérifiez le serveur de destination pour vous assurer que tout est correctement transféré. Sélectionnez **Journal des erreurs uniquement** si vous souhaitez télécharger un journal de tous les fichiers qui n’ont pas été transférés.

   > [!NOTE]
   > Si vous souhaitez conserver une piste d’audit des transferts ou planifier l’exécution de plusieurs transferts dans un travail, cliquez sur **Journal de transfert** ou sur les autres options d’enregistrement de journal pour enregistrer une copie CSV. Chaque transfert suivant remplace les informations de base de données d’une exécution précédente. 

À ce stade, vous disposez de trois options :

- **Passez à l’étape suivante**, en coupant afin que les serveurs de destination adoptent les identités des serveurs sources.
- **Envisagez la migration terminée** sans prendre en charge les identités des serveurs sources.
- **Transférer à nouveau**, en copiant uniquement les fichiers qui ont été mis à jour depuis le dernier transfert.

Si votre objectif est de synchroniser les fichiers avec Azure, vous pouvez configurer les serveurs de destination avec Azure File Sync après le transfert des fichiers ou après avoir coupé les serveurs de destination (voir [planification d’un déploiement Azure file Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)).

## <a name="step-3-cut-over-to-the-new-servers"></a>Étape 3 : passer aux nouveaux serveurs

Au cours de cette étape, vous allez passer des serveurs sources aux serveurs de destination, en déplaçant les adresses IP et les noms d’ordinateur vers les serveurs de destination. Une fois cette étape terminée, les applications et les utilisateurs accèdent aux nouveaux serveurs via les noms et les adresses des serveurs à partir desquels vous avez migré.

1. Si vous avez quitté la tâche de migration, dans le centre d’administration Windows, accédez à **Gestionnaire de serveur** > **service de migration de stockage** , puis sélectionnez le travail que vous souhaitez effectuer.
2. Dans la page **couper les nouveaux serveurs** > **entrer les informations d’identification** , sélectionnez **suivant** pour utiliser les informations d’identification que vous avez tapées précédemment.

   Si votre destination est un serveur de fichiers en cluster, vous devrez peut-être fournir des informations d’identification disposant des autorisations nécessaires pour supprimer le cluster du domaine, puis le rajouter avec le nouveau nom.

3. Dans la page **configurer le basculement** , spécifiez la carte réseau de la destination qui doit prendre en charge les paramètres de chaque adaptateur sur la source. Cela déplace l’adresse IP de la source vers la destination dans le cadre du basculement, donnant au serveur source une nouvelle adresse IP DHCP ou statique. Vous avez la possibilité d’ignorer toutes les migrations réseau ou certaines interfaces. 
4. Spécifiez l’adresse IP à utiliser pour le serveur source après le déplacement de son adresse vers la destination. Vous pouvez utiliser DHCP ou une adresse statique. Si vous utilisez une adresse statique, le nouveau sous-réseau doit être le même que l’ancien sous-réseau ou le basculement échoue.
    ![capture d’écran montrant un serveur source, ses adresses IP et leurs noms d’ordinateur, ainsi que leur remplacement après le basculement](media/migrate/cutover.png) **figure 4 : un serveur source et la façon dont la configuration réseau est déplacée vers la destination**
5. Spécifiez Comment renommer le serveur source une fois que le serveur de destination a dépassé son nom. Vous pouvez utiliser un nom généré de manière aléatoire ou un type vous-même. Puis sélectionnez **Suivant**.
6. Sélectionnez **suivant** dans la page **ajuster les paramètres de basculement** .
7. Sélectionnez **valider** sur la page **valider l’appareil source et le périphérique de destination** , puis sélectionnez **suivant**.
8. Lorsque vous êtes prêt à effectuer le basculement, sélectionnez **Démarrer le basculement**. <br>Les utilisateurs et les applications peuvent rencontrer une interruption pendant que l’adresse et les noms sont déplacés et que les serveurs ont été redémarrés plusieurs fois, mais ils ne seront pas affectés par la migration. La durée du basculement dépend de la vitesse à laquelle les serveurs redémarrent, ainsi que des Active Directory et des temps de réplication DNS.

## <a name="see-also"></a>Articles associés

- [Vue d’ensemble de Storage migration service](overview.md)
- [Forum aux questions sur Storage migration services (FAQ)](faq.md)
- [Planification d’un déploiement de synchronisation de fichiers Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)
