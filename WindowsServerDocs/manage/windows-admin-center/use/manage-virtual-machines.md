---
title: La gestion des Machines virtuelles avec Windows Admin Center
description: Gestion des hôtes Hyper-V et des machines virtuelles avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 374ee30dcbaf9af3caa60ee85ec59fd3c158206b
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296751"
---
# La gestion des Machines virtuelles avec Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

L’outil de Machines virtuelles est disponible dans les connexions de [serveur](manage-servers.md), [Cluster de basculement](manage-failover-clusters.md) ou un [Cluster hyperconvergé](manage-hyper-converged.md) si le rôle Hyper-V est activé sur le serveur ou cluster. Vous pouvez utiliser l’outil de Machines virtuelles pour gérer les hôtes Hyper-V exécutant Windows Server 2012 ou version ultérieure, soit installé avec expérience utilisateur ou en tant que Server Core. Hyper-V Server 2012, 2016 et 2019 sont également pris en charge.

## Fonctionnalités essentielles

Avantages de l’outil de Machines virtuelles dans Windows Admin Center:

- **Haut niveau Hyper-V hôte analyse de ressource.** Afficher globale utilisation du processeur et de mémoire, les métriques de performances d’e/s, les alertes d’intégrité de l’ordinateur virtuel et les événements pour le serveur hôte Hyper-V ou un cluster entier dans un tableau de bord.
- **Expérience unifiée rapprochant les fonctionnalités de Gestionnaire Hyper-V et le Gestionnaire du Cluster de basculement.** Afficher tous les ordinateurs virtuels sur un cluster et explorez en une seule machine virtuelle pour la gestion avancée et résolution des problèmes.
- **Flux de travail simplifiée et puissant pour la gestion de la machine virtuelle.** Nouvelles expériences d’interface utilisateur adaptée aux scénarios d’administration informatique à créer, gérer et répliquer les machines virtuelles.

Voici quelques-unes des tâches Hyper-V de Windows Admin Center:

- [Analyser les performances et les ressources de l’hôte Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Afficher le stock de machine virtuelle](#view-virtual-machine-inventory)
- [Créer un ordinateur virtuel](#create-a-new-virtual-machine)
- [Modifier les paramètres de la machine virtuelle](#change-virtual-machine-settings)
- [Migration dynamique d’une machine virtuelle à un autre nœud de cluster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Gestion avancée et résolution des problèmes pour une seule machine virtuelle](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Gérer une machine virtuelle par le biais de l’hôte Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Modifier les paramètres de l’hôte Hyper-V](#change-hyper-v-host-settings)
- [Afficher les journaux d’événements Hyper-V](#view-hyper-v-event-logs)
- [Protéger les machines virtuelles avec Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## Analyser les performances et les ressources de l’hôte Hyper-V

![Écran récapitulatif des Machines virtuelles](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Cliquez sur l’outil de **Machines virtuelles** à partir du volet de navigation gauche.
2. Il existe deux onglets en haut de l’outil de **Machines virtuelles** , les onglets **Résumé** et **l’inventaire** . L’onglet **Résumé** offre une vue holistique de ressources de l’hôte Hyper-V et de performance pour le serveur actuel ou l’ensemble du cluster, y compris les éléments suivants:
    - Le nombre d’ordinateurs virtuels regroupés par état - en cours d’exécution, désactivé, en pause et enregistré
    - Les alertes récentes intégrité ou des événements de journal des événements de Hyper-V (alertes sont uniquement disponibles pour les clusters hyperconvergés exécutant Windows Server 2016 ou version ultérieure)
    - Utilisation du processeur et de mémoire avec répartition d’invité hôte Visual Studio
    - Machines virtuelles supérieur consomme le plus de ressources du processeur et de la mémoire
    - Les données historiques et en temps réel des courbes/s par seconde et e/s de débit (graphiques de ligne de performances de stockage sont uniquement disponibles pour les clusters hyperconvergés exécutant Windows Server 2016 ou version ultérieure. Les données historiques sont uniquement disponibles pour les clusters hyperconvergés exécutant Windows Server 2019)

## Afficher le stock de machine virtuelle

![Écran de l’inventaire des Machines virtuelles](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Cliquez sur l’outil de **Machines virtuelles** à partir du volet de navigation gauche.
2. Il existe deux onglets en haut de l’outil de **Machines virtuelles** , les onglets **Résumé** et **l’inventaire** . L’onglet **inventaire** répertorie les ordinateurs virtuels disponibles sur le serveur actuel ou l’ensemble du cluster et fournit des commandes pour gérer les machines virtuelles. Vous pouvez:
    - Afficher une liste des machines virtuelles en cours d’exécution sur le serveur actuel ou un cluster.
    - Permet d’afficher de la machine virtuelle état et le serveur hôte si vous affichez les machines virtuelles pour un cluster. Permet également d’afficher l’utilisation du processeur et de la mémoire à partir du point de vue hôte, y compris la sollicitation de la mémoire, à la demande de mémoire et la mémoire affectée et de la machine virtuelle disponibilité, état de pulsation et état de la protection à l’aide d’Azure Site Recovery.
    - [Créer une machine virtuelle](#create-a-new-virtual-machine).
    - Supprimer, Démarrer, désactiver, arrêter, suspendre, reprendre, réinitialiser ou renommer une machine virtuelle. Également enregistrer la machine virtuelle, supprimer un état enregistré ou créer un point de contrôle.
    - [Modifier les paramètres d’une machine virtuelle](#change-virtual-machine-settings).
    - Se connecter à une console de machine virtuelle avec VMConnect via l’hôte Hyper-V.
    - [Répliquer une machine virtuelle à l’aide d’Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - Pour les opérations pouvant être exécutées sur plusieurs ordinateurs virtuels, par exemple, Démarrer, arrêter, enregistrer, Pause, supprimer, réinitialiser, vous pouvez sélectionner plusieurs ordinateurs virtuels et exécution de l’opération à la fois.

Remarque: Si vous êtes connecté à un cluster, l’outil de la Machine virtuelle affiche uniquement les machines virtuelles en cluster. Nous prévoyons afin d’afficher également non cluster des machines virtuelles à l’avenir.

## Créer un ordinateur virtuel

![Créer le nouveau filtre de machine virtuelle](../media/manage-virtual-machines/new-vm.png)

1. Cliquez sur l’outil de **Machines virtuelles** à partir du volet de navigation gauche.
2. En haut de l’outil de Machines virtuelles, choisissez l’onglet de **l’inventaire** , puis cliquez sur **Nouveau** pour créer une machine virtuelle.
3. Entrez le nom de la machine virtuelle et choisir entre les machines virtuelles de génération 1 et 2.
4. Si vous créez une machine virtuelle sur un cluster, vous pouvez choisir quel hôte initialement créer la machine virtuelle sur. Si vous exécutez Windows Server 2016 ou version ultérieure, l’outil fournit une recommandation hôte pour vous.
5. Choisissez un chemin d’accès pour les fichiers de la machine virtuelle. Choisissez un volume à partir de la liste déroulante ou cliquez sur **Parcourir** pour choisir un dossier à l’aide du sélecteur de dossiers. Les fichiers de configuration de la machine virtuelle et le fichier de disque dur virtuel seront enregistrées dans un dossier unique sous la `\Hyper-V\\[virtual machine name]` chemin d’accès du volume sélectionné ou du chemin d’accès.

>[!Tip]
> Dans le sélecteur de dossiers, vous pouvez parcourir à n’importe quel partage SMB disponible sur le réseau en entrant le chemin d’accès dans le champ du **nom du dossier** en tant que ```\\server\share```. À l’aide d’un partage réseau pour le stockage de l’ordinateur virtuel nécessite [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Choisissez le nombre de processeurs virtuels, si vous souhaitez que la virtualisation imbriquée activée, configurez les paramètres de la mémoire, les cartes réseau, les disques durs virtuels et choisissez si vous souhaitez installer un système d’exploitation à partir d’un fichier image .iso ou à partir du réseau.
7. Cliquez sur **créer** pour créer la machine virtuelle.
8. Une fois que la machine virtuelle est créée et s’affiche dans la liste des machines virtuelles, vous pouvez démarrer la machine virtuelle.
9. Une fois que la machine virtuelle est démarrée, vous pouvez vous connecter à la console de la machine virtuelle via VMConnect pour installer le système d’exploitation. Sélectionnez la machine virtuelle dans la liste, cliquez sur **plusieurs** > **Connect** pour télécharger le fichier .rdp. Ouvrez le fichier .rdp dans l’application Connexion Bureau à distance. Dans la mesure où il se connecte à la console de la machine virtuelle, vous devez entrer les informations d’identification d’administrateur de l’hôte Hyper-V.

## Modifier les paramètres de la machine virtuelle

![Écran des paramètres de machine virtuelle](../media/manage-virtual-machines/vm-settings.png)

1. Cliquez sur l’outil de **Machines virtuelles** à partir du volet de navigation gauche.
2. En haut de l’outil de Machines virtuelles, choisissez l’onglet **d’inventaire** dans la liste, choisissez une machine virtuelle et cliquez sur **plusieurs** > **paramètres**.
3. Basculer entre l’onglet **Général**, **sécurité**, **mémoire**, **processeurs**, **disques**, **réseaux**, **ordre de démarrage** et **les points de contrôle** , configurez les paramètres nécessaires, puis cliquez sur **Enregistrer** pour enregistrer paramètres de l’onglet actif. Les paramètres disponibles varie en fonction de la génération de la machine virtuelle. En outre, certains paramètres ne peuvent pas être modifiés pour les machines virtuelles en cours d’exécution et vous devez arrêter tout d’abord de la machine virtuelle.

## Migration dynamique d’une machine virtuelle à un autre nœud de cluster

Si vous êtes connecté à un cluster, vous pouvez migrer direct une machine virtuelle à un autre nœud de cluster.

1. À partir d’une connexion de cluster hyperconvergé ou un Cluster de basculement, cliquez sur l’outil de **Machines virtuelles** à partir du volet de navigation gauche.
2. En haut de l’outil de Machines virtuelles, choisissez l’onglet **d’inventaire** dans la liste, choisissez une machine virtuelle et cliquez sur **plusieurs** > **déplacer**.
3. Choisissez un serveur à partir de la liste des nœuds de cluster disponibles, puis cliquez sur **déplacer**.
4. Notifications pour la progression de déplacement seront affichera dans le coin supérieur droit de Windows Admin Center. Si le déplacement a réussi, vous verrez le nom du serveur hôte a changé dans la liste des machines virtuelles.

## Gestion avancée et résolution des problèmes pour une seule machine virtuelle

![Écran des détails de machine virtuelle unique](../media/manage-virtual-machines/vm-details.png)

Vous pouvez afficher des informations détaillées et des performances graphiques pour une seule machine virtuelle à partir de la page de machine virtuelle unique.

1. Cliquez sur l’outil de **Machines virtuelles** à partir du volet de navigation gauche.
2. En haut de l’outil de Machines virtuelles, choisissez l’onglet **inventaire** sur le nom d’une machine virtuelle à partir de la liste des machines virtuelles.
3. À partir de la page de la machine virtuelle unique, vous pouvez:
    - Permet d’afficher des informations détaillées sur la machine virtuelle.
    - Afficher Live et des graphiques de ligne de données d’historique de processeur, mémoire, réseau, débit/s par seconde et d’e/s (les données historiques sont uniquement disponibles pour les clusters hyperconvergés exécutant Windows Server 2019)
    - Afficher, créer, appliquer, renommer et supprimer des points de contrôle.
    - Afficher les détails des fichiers de disque dur virtuel (.vhd), les cartes réseau et les serveur hôte de la machine virtuelle.
    - Supprimer, Démarrer, désactiver, arrêter, suspendre, reprendre, réinitialiser ou renommer la machine virtuelle. Également enregistrer la machine virtuelle, supprimer un état enregistré ou créer un point de contrôle.
    - [Modifier les paramètres de la machine virtuelle](#change-virtual-machine-settings).
    - Se connecter à la console de machine virtuelle avec VMConnect via l’hôte Hyper-V.
    - [Répliquer la machine virtuelle à l’aide d’Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## Gérer une machine virtuelle par le biais de l’hôte Hyper-V (VMConnect)

![Ordinateur virtuel se connecter via votre navigateur web](../media/manage-virtual-machines/vm-connect.png)

1. Cliquez sur l’outil de **Machines virtuelles** à partir du volet de navigation gauche.
2. En haut de l’outil de Machines virtuelles, choisissez l’onglet **d’inventaire** dans la liste, choisissez une machine virtuelle et cliquez sur **plusieurs** > **Connect** ou **fichier RDP télécharger**. **Se connecter** vous permettra d’interagir avec l’ordinateur virtuel invité par le biais de la console web services Bureau à distance, intégrée à Windows Admin Center. **Fichier RDP télécharger** télécharge un fichier .rdp que vous pouvez ouvrir avec l’application de connexion Bureau à distance (mstsc.exe). Les deux options utiliseront VMConnect pour se connecter à l’ordinateur virtuel invité par le biais de l’hôte Hyper-V et nécessitent que vous entrez des informations d’identification d’administrateur pour le serveur hôte Hyper-V.

## Modifier les paramètres de l’hôte Hyper-V

![Écran des paramètres de l’hôte Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. Dans une connexion serveur, Cluster hyperconvergé ou un Cluster de basculement, cliquez sur le menu **paramètres** en bas du volet de navigation gauche.
2. Sur un serveur hôte Hyper-V ou un cluster, vous verrez un groupe de **Paramètres de l’hôte Hyper-V** avec les sections suivantes:
    - Général: Modification des disques durs et les machines virtuelles fichier chemin d’accès virtuel et hyperviseur planifier type (si pris en charge)
    - Mode de Session étendu
    - NUMA couvrant
    - La Migration dynamique
    - Migration du stockage
3. Si vous effectuez n’importe quel hôte Hyper-V aux paramètres de connexion à un Cluster hyperconvergé ou de Cluster de basculement, la modification s’appliqueront à tous les nœuds de cluster.

## Afficher les journaux d’événements Hyper-V

Vous pouvez afficher les journaux des événements directement à partir de l’outil de Machines virtuelles Hyper-V.

1. Cliquez sur l’outil de **Machines virtuelles** à partir du volet de navigation gauche.
2. En haut de l’outil de Machines virtuelles, choisissez l’onglet **Résumé** . Dans la section événements supérieure droite, cliquez sur **Tous les événements d’affichage**.
3. L’outil de l’Observateur d’événements affiche les canaux d’événement Hyper-V dans le volet gauche. Choisissez un canal pour afficher les événements dans le volet droit. Si vous gérez un cluster de basculement ou un cluster hyperconvergé, les journaux des événements affiche les événements pour tous les nœuds de cluster, affichant le serveur hôte dans la colonne de l’ordinateur.

## Protéger les machines virtuelles avec Azure Site Recovery

Vous pouvez utiliser Windows Admin Center pour configurer Azure Site Recovery et répliquer vos machines virtuelles de local à Azure. [En savoir plus](../azure/azure-site-recovery.md)

## Entrée plus

Gestion des machines virtuelles dans Windows Admin Center est activement en cours de développement et de nouvelles fonctionnalités sont ajoutées à l’avenir proche. Vous pouvez afficher l’état et les votes pour les fonctionnalités dans UserVoice:

|Demande de fonctionnalité|
|-------|
|[Importation/exportation des machines virtuelles](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[Tri des ordinateurs virtuels dans les dossiers](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[Prise en charge des paramètres de machine virtuelle supplémentaires](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[Prise en charge réplica Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[Déléguer la propriété de la machine virtuelle](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[Clonez la machine virtuelle](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[Créer un modèle à partir d’une machine virtuelle existante](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[Machines virtuelles de vue sur les hôtes Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[Configurer un réseau local virtuel dans le volet de Machine virtuelle](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**Voir tous les ou proposer la nouvelle fonctionnalité**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
