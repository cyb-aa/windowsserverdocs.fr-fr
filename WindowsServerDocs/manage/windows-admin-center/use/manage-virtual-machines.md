---
title: Gestion des ordinateurs virtuels avec Windows Admin Center
description: La gestion des hôtes Hyper-V et les machines virtuelles avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 84e1ce7864f04550ee25253bcf038afdd7b919fe
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811679"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>Gestion des ordinateurs virtuels avec Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

L’outil de Machines virtuelles est disponible dans [Server](manage-servers.md), [Cluster de basculement](manage-failover-clusters.md) ou [Hyper-Converged Cluster](manage-hyper-converged.md) connexions si le rôle Hyper-V est activé sur le serveur ou le cluster. Vous pouvez utiliser l’outil de Machines virtuelles pour gérer les hôtes Hyper-V exécutant Windows Server 2012 ou version ultérieure, soit installé avec la fonctionnalité expérience utilisateur ou en tant que Server Core. Hyper-V Server 2012, 2016 et 2019 sont également pris en charge.

## <a name="key-features"></a>Principales fonctionnalités

Points importants de l’outil de Machines virtuelles dans Windows Admin Center sont les suivantes :

- **Hyper-V hôte ressource Surveillance de niveau élevé.** Afficher globale utilisation du processeur et mémoire, les mesures de performances d’e/s, alertes d’intégrité de machine virtuelle et événements pour le serveur hôte Hyper-V ou l’ensemble du cluster dans un tableau de bord unique.
- **Expérience unifiée rassemblant les fonctionnalités de Gestionnaire Hyper-V et le Gestionnaire du Cluster de basculement.** Afficher toutes les machines virtuelles sur un cluster et d’examiner une seule machine virtuelle pour la gestion avancée et la résolution des problèmes.
- **Flux de travail simplifié et puissant pour la gestion de la machine virtuelle.** Nouvelles expériences de l’interface utilisateur adaptées aux scénarios d’administration informatique à créer, gérer et répliquer des machines virtuelles.

Voici quelques-unes des tâches Hyper-V, que vous pouvez faire dans Windows Admin Center :

- [Surveiller les performances et les ressources de l’hôte Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Afficher l’inventaire machine virtuelle](#view-virtual-machine-inventory)
- [Créer une machine virtuelle](#create-a-new-virtual-machine)
- [Modifier les paramètres de machine virtuelle](#change-virtual-machine-settings)
- [Migrer dynamiquement un ordinateur virtuel vers un autre nœud de cluster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Gestion avancée et la résolution des problèmes pour une seule machine virtuelle](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Gérer une machine virtuelle via l’hôte Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Modifier les paramètres de l’hôte Hyper-V](#change-hyper-v-host-settings)
- [Afficher les journaux des événements Hyper-V](#view-hyper-v-event-logs)
- [Protéger les machines virtuelles avec Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>Surveiller les performances et les ressources de l’hôte Hyper-V

![Écran de résumé de Machines virtuelles](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Cliquez sur le **Machines virtuelles** outil depuis le volet de navigation de gauche.
2. Il existe deux onglets en haut de la **Machines virtuelles** outil, le **Résumé** onglet et le **inventaire** onglet. Le **Résumé** onglet fournit une vue holistique des ressources de l’hôte Hyper-V et de performances pour le serveur actuel ou l’ensemble du cluster, y compris les éléments suivants :
    - Le nombre de machines virtuelles regroupées par état - en cours d’exécution, off, mise en pause et enregistré
    - Alertes d’intégrité récentes ou les événements de journal des événements Hyper-V (alertes sont uniquement disponibles pour les clusters hyperconvergés exécutant Windows Server 2016 ou version ultérieure)
    - Utilisation du processeur et mémoire avec répartition d’invité hôte vs
    - Machines virtuelles principales consommant le plus de ressources processeur et mémoire
    - Graphiques pour le débit d’e/s et d’e/s de ligne de données actives et historiques (graphiques de ligne de performances de stockage sont uniquement disponibles pour les clusters hyperconvergés exécutant Windows Server 2016 ou version ultérieure. Les données d’historique sont uniquement disponibles pour les clusters hyperconvergés exécutant Windows Server 2019)

## <a name="view-virtual-machine-inventory"></a>Afficher l’inventaire machine virtuelle

![Écran de l’inventaire des ordinateurs virtuels](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Cliquez sur le **Machines virtuelles** outil depuis le volet de navigation de gauche.
2. Il existe deux onglets en haut de la **Machines virtuelles** outil, le **Résumé** onglet et le **inventaire** onglet. Le **inventaire** onglet répertorie les ordinateurs virtuels disponibles sur le serveur actuel ou l’ensemble du cluster et fournit des commandes pour gérer des machines virtuelles individuelles. Vous pouvez :
    - Afficher la liste des ordinateurs virtuels en cours d’exécution sur le serveur actuel ou le cluster.
    - Afficher le serveur hôte et l’état de la machine virtuelle si vous affichez des machines virtuelles pour un cluster. Afficher également l’utilisation du processeur et de la mémoire à partir de la perspective de l’hôte, y compris la sollicitation de la mémoire, la demande de mémoire et mémoire affectée et la machine virtuelle temps d’activité, état de pulsation et état de protection à l’aide d’Azure Site Recovery.
    - [Créer une machine virtuelle](#create-a-new-virtual-machine).
    - Supprimer, Démarrer, désactiver, arrêter, suspendre, reprendre, réinitialiser ou renommer un ordinateur virtuel. Également enregistrer la machine virtuelle, supprimer un état enregistré ou créer un point de contrôle.
    - [Modifier les paramètres pour une machine virtuelle](#change-virtual-machine-settings).
    - Se connecter à une console de machine virtuelle à l’aide de VMConnect par le biais de l’hôte Hyper-V.
    - [Répliquer une machine virtuelle à l’aide d’Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - Pour les opérations qui peuvent être exécutées sur plusieurs machines virtuelles, notamment Démarrer, arrêter, enregistrer, Pause, supprimer, réinitialiser, vous pouvez sélectionner plusieurs machines virtuelles et exécuter l’opération à la fois.

REMARQUE : Si vous êtes connecté à un cluster, l’outil de Machine virtuelle affiche uniquement les ordinateurs virtuels en cluster. Nous prévoyons également afficher non cluster des machines virtuelles à l’avenir.

## <a name="create-a-new-virtual-machine"></a>Créer une machine virtuelle

![Créer un écran de machine virtuelle](../media/manage-virtual-machines/new-vm.png)

1. Cliquez sur le **Machines virtuelles** outil depuis le volet de navigation de gauche.
2. En haut de l’outil de Machines virtuelles, choisissez le **inventaire** onglet, puis cliquez sur **New** pour créer une machine virtuelle.
3. Entrez le nom de la machine virtuelle et le choix entre les ordinateurs virtuels de génération 1 et 2.
4. Si vous créez une machine virtuelle sur un cluster, vous pouvez choisir quel hôte pour créer initialement la machine virtuelle sur. Si vous exécutez Windows Server 2016 ou version ultérieure, l’outil fournit une recommandation de l’hôte pour vous.
5. Choisissez un chemin d’accès pour les fichiers de machine virtuelle. Choisissez un volume à partir de la liste déroulante ou cliquez sur **Parcourir** pour choisir un dossier à l’aide du sélecteur de dossier. Les fichiers de configuration de machine virtuelle et le fichier de disque dur virtuel seront enregistrés dans un seul dossier sous le `\Hyper-V\\[virtual machine name]` chemin du volume sélectionné ou du chemin d’accès.

   >[!Tip]
   > Dans le sélecteur de dossier, vous pouvez accéder à n’importe quel partage SMB disponible sur le réseau en entrant le chemin d’accès dans le **nom du dossier** champ en tant que ```\\server\share```. À l’aide d’un partage réseau pour le stockage de machine virtuelle nécessiteront [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Choisissez le nombre de processeurs virtuels, si vous souhaitez que la virtualisation imbriquée est activée, configurez les paramètres de mémoire, les cartes réseau, les disques durs virtuels et choisissez si vous souhaitez installer un système d’exploitation à partir d’un fichier image .iso ou à partir du réseau.
7. Cliquez sur **Créer** pour créer l’ordinateur virtuel.
8. Une fois que la machine virtuelle est créée et apparaît dans la liste des machines virtuelles, vous pouvez démarrer l’ordinateur virtuel.
9. Une fois la machine virtuelle est démarrée, vous pouvez vous connecter à la console de la machine virtuelle par le biais de VMConnect pour installer le système d’exploitation. Sélectionnez la machine virtuelle à partir de la liste, cliquez sur **plus** > **Connect** pour télécharger le fichier .rdp. Dans l’application Connexion Bureau à distance, ouvrez le fichier .rdp. Dans la mesure où il se connecte à la console de la machine virtuelle, vous devez entrer les informations d’identification administrateur de l’hôte Hyper-V.

## <a name="change-virtual-machine-settings"></a>Modifier les paramètres de machine virtuelle

![Écran des paramètres de machine virtuelle](../media/manage-virtual-machines/vm-settings.png)

1. Cliquez sur le **Machines virtuelles** outil depuis le volet de navigation de gauche.
2. En haut de l’outil de Machines virtuelles, choisissez le **inventaire** onglet. Choisissez une machine virtuelle à partir de la liste et cliquez sur **plus** > **paramètres**.
3. Basculer entre le **général**, **sécurité**, **mémoire**, **processeurs**, **disques**, **Réseaux**, **ordre de démarrage** et **points de contrôle** onglet, configurez les paramètres nécessaires, puis cliquez sur **enregistrer** pour enregistrer l’onglet actuel Paramètres. Les paramètres disponibles varient en fonction de la génération de la machine virtuelle. En outre, certains paramètres ne peut pas être modifiés pour l’exécution de machines virtuelles et vous devez arrêter la machine virtuelle tout d’abord.

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>Migrer dynamiquement un ordinateur virtuel vers un autre nœud de cluster

Si vous êtes connecté à un cluster, vous pouvez en direct migrer un ordinateur virtuel vers un autre nœud de cluster.

1. À partir d’un Cluster de basculement ou la connexion au cluster hyperconvergé, cliquez sur le **Machines virtuelles** outil depuis le volet de navigation de gauche.
2. En haut de l’outil de Machines virtuelles, choisissez le **inventaire** onglet. Choisissez une machine virtuelle à partir de la liste et cliquez sur **plus** > **déplacer**.
3. Choisissez un serveur dans la liste des nœuds de cluster disponibles et cliquez sur **déplacer**.
4. Notifications pour la progression de déplacement seront affichera dans le coin supérieur droit de Windows Admin Center. Si le déplacement a réussi, vous verrez le nom du serveur hôte modifié dans la liste des machines virtuelles.

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>Gestion avancée et la résolution des problèmes pour une seule machine virtuelle

![Écran de détails de machine virtuelle unique](../media/manage-virtual-machines/vm-details.png)

Vous pouvez afficher des informations détaillées et des graphiques de performances pour une seule machine virtuelle à partir de la page de la machine virtuelle unique.

1. Cliquez sur le **Machines virtuelles** outil depuis le volet de navigation de gauche.
2. En haut de l’outil de Machines virtuelles, choisissez le **inventaire** onglet. Cliquez sur le nom d’une machine virtuelle à partir de la liste des machines virtuelles.
3. Dans la page de la machine virtuelle unique, vous pouvez :
    - Afficher des informations détaillées pour la machine virtuelle.
    - Afficher en direct et des graphiques de ligne de données d’historique pour le processeur, mémoire, réseau, débit d’e/s et d’e/s (données d’historique sont disponibles uniquement pour les clusters hyperconvergés exécutant Windows Server 2019)
    - Afficher, créer, appliquer, renommer et supprimer des points de contrôle.
    - Afficher les détails des fichiers de disque dur virtuel (.vhd), cartes réseau et de serveur hôte de la machine virtuelle.
    - Supprimer, Démarrer, désactiver, arrêter, suspendre, reprendre, réinitialiser ou renommez l’ordinateur virtuel. Également enregistrer la machine virtuelle, supprimer un état enregistré ou créer un point de contrôle.
    - [Modifier les paramètres de la machine virtuelle](#change-virtual-machine-settings).
    - Connectez-vous à la console de machine virtuelle avec VMConnect par le biais de l’hôte Hyper-V.
    - [Répliquez la machine virtuelle à l’aide d’Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## <a name="manage-a-virtual-machine-through-the-hyper-v-host-vmconnect"></a>Gérer une machine virtuelle via l’hôte Hyper-V (VMConnect)

![Machine virtuelle se connecter via votre navigateur web](../media/manage-virtual-machines/vm-connect.png)

1. Cliquez sur le **Machines virtuelles** outil depuis le volet de navigation de gauche.
2. En haut de l’outil de Machines virtuelles, choisissez le **inventaire** onglet. Choisissez une machine virtuelle à partir de la liste et cliquez sur **plus** > **Connect** ou **fichier RDP télécharger**. **Se connecter** vous permettra d’interagir avec la machine virtuelle invitée via la console web de bureau à distance, intégrée à Windows Admin Center. **Télécharger le fichier RDP** télécharge un fichier .rdp que vous pouvez ouvrir avec l’application Connexion Bureau à distance (mstsc.exe). Les deux options utilisent VMConnect pour vous connecter à la machine virtuelle invitée via l’hôte Hyper-V et vous demandera d’entrer des informations d’identification d’administrateur pour le serveur hôte Hyper-V.

## <a name="change-hyper-v-host-settings"></a>Modifier les paramètres de l’hôte Hyper-V

![Écran de paramètres d’hôte Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. Sur une connexion serveur, Cluster hyperconvergé ou Cluster de basculement, cliquez sur le **paramètres** menu en bas du volet de navigation gauche.
2. Sur un serveur hôte Hyper-V ou un cluster, vous verrez un **paramètres de l’hôte Hyper-V** groupe avec les sections suivantes :
    - Général : Modification des disques durs et les machines virtuelles fichier chemin d’accès virtuel et l’hyperviseur type de planification (si pris en charge)
    - Mode de Session étendu
    - Fractionnement NUMA
    - Migration dynamique
    - Migration du stockage
3. Si vous apportez n’importe quel hôte Hyper-V changements de paramètre dans une connexion de Cluster hyperconvergé ou Cluster de basculement, la modification sera appliquée à tous les nœuds de cluster.

## <a name="view-hyper-v-event-logs"></a>Afficher les journaux des événements Hyper-V

Vous pouvez afficher les journaux des événements directement à partir de l’outil de Machines virtuelles Hyper-V.

1. Cliquez sur le **Machines virtuelles** outil depuis le volet de navigation de gauche.
2. En haut de l’outil de Machines virtuelles, choisissez le **Résumé** onglet. Dans la section événements supérieure droite, cliquez sur **afficher tous les événements**.
3. L’outil Observateur d’événements affiche les canaux d’événement Hyper-V dans le volet gauche. Choisissez un canal pour afficher les événements dans le volet droit. Si vous gérez un cluster de basculement ou d’un cluster hyperconvergé, les journaux des événements affiche les événements pour tous les nœuds de cluster, afficher le serveur hôte dans la colonne de la Machine.

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>Protéger les machines virtuelles avec Azure Site Recovery

Vous pouvez utiliser Windows Admin Center pour configurer Azure Site Recovery et répliquer vos machines virtuelles de local vers Azure. [En savoir plus](../azure/azure-site-recovery.md)

## <a name="more-coming"></a>Bientôt plus

Gestion d’ordinateurs virtuels dans Windows Admin Center est activement en cours de développement et de nouvelles fonctionnalités seront ajoutées dans un avenir proche. Vous pouvez afficher l’état et voter pour les fonctionnalités dans UserVoice :

- [Importation/exportation des machines virtuelles](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)
- [Machines virtuelles de tri dans des dossiers](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)
- [Prend en charge les paramètres de machine virtuelle supplémentaire](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)
- [Prise en charge de la réplication Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)
- [Déléguer l’appartenance à machine virtuelle](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)
- [Cloner l’ordinateur virtuel](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)
- [Créer un modèle à partir d’une machine virtuelle existante](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)
- [Afficher les machines virtuelles entre les hôtes Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)
- [Configurer un réseau local virtuel dans le volet de la Machine virtuelle](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)

[Afficher tout ou proposer de nouvelles fonctionnalités](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D).
