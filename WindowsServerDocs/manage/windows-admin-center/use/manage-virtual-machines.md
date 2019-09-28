---
title: Gestion des machines virtuelles avec le centre d’administration Windows
description: Gestion des ordinateurs hôtes Hyper-V et des machines virtuelles avec le centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 02a33383b466e8bade2db0bbddaff66f0196954c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356856"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>Gestion des machines virtuelles avec le centre d’administration Windows

>S'applique à : Windows Admin Center, Windows Admin Center Preview

L’outil machines virtuelles est disponible dans [serveur](manage-servers.md), [cluster de basculement](manage-failover-clusters.md) ou connexions de [cluster hyper-convergé](manage-hyper-converged.md) si le rôle Hyper-V est activé sur le serveur ou le cluster. Vous pouvez utiliser l’outil machines virtuelles pour gérer les ordinateurs hôtes Hyper-V exécutant Windows Server 2012 ou une version ultérieure, soit installé avec expérience utilisateur, soit en tant que serveur principal. Hyper-V Server 2012, 2016 et 2019 sont également pris en charge.

## <a name="key-features"></a>Principales fonctionnalités

Les principales caractéristiques de l’outil machines virtuelles dans le centre d’administration Windows sont les suivantes :

- **Analyse des ressources de l’hôte Hyper-V de haut niveau.** Affichez l’utilisation globale du processeur et de la mémoire, les métriques de performances d’e/s, les alertes d’intégrité de machine virtuelle et les événements pour le serveur hôte Hyper-V ou le cluster entier dans un tableau de bord unique.
- **Expérience unifiée qui consiste à réunir les fonctionnalités du Gestionnaire Hyper-V et de Gestionnaire du cluster de basculement.** Affichez toutes les machines virtuelles dans un cluster et explorez une seule machine virtuelle pour une gestion et un dépannage avancés.
- **Des flux de travail simplifiés et puissants pour la gestion des ordinateurs virtuels.** Nouvelles expériences de l’interface utilisateur adaptées aux scénarios d’administration informatique pour créer, gérer et répliquer des machines virtuelles.

Voici quelques-unes des tâches Hyper-V que vous pouvez effectuer dans le centre d’administration Windows :

- [Surveiller les performances et les ressources de l’hôte Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Afficher l’inventaire des machines virtuelles](#view-virtual-machine-inventory)
- [Créer un nouvel ordinateur virtuel](#create-a-new-virtual-machine)
- [Modifier les paramètres de l’ordinateur virtuel](#change-virtual-machine-settings)
- [Migrer dynamiquement un ordinateur virtuel vers un autre nœud de cluster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Gestion avancée et résolution des problèmes pour une seule machine virtuelle](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Gérer un ordinateur virtuel par le biais de l’hôte Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Modifier les paramètres de l’hôte Hyper-V](#change-hyper-v-host-settings)
- [Afficher les journaux des événements Hyper-V](#view-hyper-v-event-logs)
- [Protéger les machines virtuelles avec Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>Surveiller les performances et les ressources de l’hôte Hyper-V

![Écran Résumé des machines virtuelles](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Cliquez sur l’outil **machines virtuelles** dans le volet de navigation de gauche.
2. Il existe deux onglets en haut de l’outil **machines virtuelles** : l’onglet **Résumé** et l’onglet **inventaire** . L’onglet **Résumé** fournit une vue holistique des ressources et des performances des ordinateurs hôtes Hyper-V pour le serveur actuel ou le cluster entier, y compris les éléments suivants :
    - Nombre de machines virtuelles regroupées par État : en cours d’exécution, désactivé, suspendu et enregistré
    - Alertes d’intégrité récentes ou événements du journal des événements Hyper-V (les alertes sont uniquement disponibles pour les clusters hyper-convergent exécutant Windows Server 2016 ou version ultérieure)
    - Utilisation de l’UC et de la mémoire avec décomposition de l’hôte et de l’invité
    - Principales machines virtuelles consommant le plus de ressources processeur et mémoire
    - Graphiques en ligne de données historiques et en temps réel pour les e/s par seconde et le débit d’e/s (les graphiques en courbes de performances de stockage sont uniquement disponibles pour les clusters hyper-convergents exécutant Windows Server 2016 ou une version ultérieure. Les données d’historique sont uniquement disponibles pour les clusters hyper-convergent exécutant Windows Server 2019)

## <a name="view-virtual-machine-inventory"></a>Afficher l’inventaire des machines virtuelles

![Écran d’inventaire des machines virtuelles](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Cliquez sur l’outil **machines virtuelles** dans le volet de navigation de gauche.
2. Il existe deux onglets en haut de l’outil **machines virtuelles** : l’onglet **Résumé** et l’onglet **inventaire** . L’onglet **inventaire** répertorie les machines virtuelles disponibles sur le serveur actuel ou le cluster entier, et fournit des commandes permettant de gérer des machines virtuelles individuelles. Vous pouvez :
    - Affichez la liste des machines virtuelles en cours d’exécution sur le serveur ou le cluster actif.
    - Affichez l’état de la machine virtuelle et le serveur hôte si vous affichez des machines virtuelles pour un cluster. Affichez également l’utilisation du processeur et de la mémoire du point de vue de l’hôte, y compris la sollicitation de la mémoire, la demande de mémoire et la mémoire allouée, ainsi que la durée de fonctionnement de la machine virtuelle, l’état de la pulsation Azure Site Recovery et
    - [Créez un nouvel ordinateur virtuel](#create-a-new-virtual-machine).
    - Supprimer, démarrer, désactiver, arrêter, suspendre, reprendre, réinitialiser ou renommer un ordinateur virtuel. Enregistrez également l’ordinateur virtuel, supprimez un état enregistré ou créez un point de contrôle.
    - [Modifiez les paramètres d’un ordinateur virtuel](#change-virtual-machine-settings).
    - Connectez-vous à une console de machine virtuelle à l’aide de VMConnect via l’hôte Hyper-V.
    - [Répliquer une machine virtuelle à l’aide de Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - Pour les opérations qui peuvent être exécutées sur plusieurs machines virtuelles, telles que Démarrer, arrêter, enregistrer, suspendre, supprimer, réinitialiser, vous pouvez sélectionner plusieurs machines virtuelles et exécuter l’opération à la fois.

REMARQUE : Si vous êtes connecté à un cluster, l’outil ordinateur virtuel affiche uniquement les ordinateurs virtuels en cluster. Nous prévoyons également d’afficher des machines virtuelles non cluster à l’avenir.

## <a name="create-a-new-virtual-machine"></a>Créer une machine virtuelle

![Écran créer un nouvel ordinateur virtuel](../media/manage-virtual-machines/new-vm.png)

1. Cliquez sur l’outil **machines virtuelles** dans le volet de navigation de gauche.
2. En haut de l’outil machines virtuelles, choisissez l’onglet **inventaire** , puis cliquez sur **nouveau** pour créer un nouvel ordinateur virtuel.
3. Entrez le nom de l’ordinateur virtuel et choisissez entre les ordinateurs virtuels de génération 1 et 2.
4. Si vous créez une machine virtuelle sur un cluster, vous pouvez choisir l’ordinateur hôte sur lequel la machine virtuelle doit être créée pour la première fois. Si vous exécutez Windows Server 2016 ou une version ultérieure, l’outil vous fournira une recommandation pour votre ordinateur hôte.
5. Choisissez un chemin d’accès pour les fichiers de l’ordinateur virtuel. Choisissez un volume dans la liste déroulante ou cliquez sur **Parcourir** pour choisir un dossier à l’aide du sélecteur de dossiers. Les fichiers de configuration d’ordinateur virtuel et le fichier de disque dur virtuel sont enregistrés dans un dossier `\Hyper-V\\[virtual machine name]` unique sous le chemin d’accès du volume ou du chemin d’accès sélectionné.

   >[!Tip]
   > Dans le sélecteur de dossiers, vous pouvez accéder à n’importe quel partage SMB disponible sur le réseau en entrant le chemin d’accès dans le champ **nom du dossier** en tant que ```\\server\share```. L’utilisation d’un partage réseau pour le stockage de machines virtuelles nécessite [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Choisissez le nombre de processeurs virtuels, si vous souhaitez activer la virtualisation imbriquée, configurez les paramètres de mémoire, les cartes réseau, les disques durs virtuels et indiquez si vous souhaitez installer un système d’exploitation à partir d’un fichier image. ISO ou du réseau.
7. Cliquez sur **Créer** pour créer l’ordinateur virtuel.
8. Une fois que l’ordinateur virtuel est créé et apparaît dans la liste des machines virtuelles, vous pouvez démarrer l’ordinateur virtuel.
9. Une fois la machine virtuelle démarrée, vous pouvez vous connecter à la console de la machine virtuelle via VMConnect pour installer le système d’exploitation. Sélectionnez la machine virtuelle dans la liste **, puis cliquez** > sur**se connecter** pour télécharger le fichier. RDP. Ouvrez le fichier. RDP dans l’application Connexion Bureau à distance. Dans la mesure où il se connecte à la console de la machine virtuelle, vous devrez entrer les informations d’identification d’administrateur de l’hôte Hyper-V.

## <a name="change-virtual-machine-settings"></a>Modifier les paramètres de l’ordinateur virtuel

![Écran des paramètres de machine virtuelle](../media/manage-virtual-machines/vm-settings.png)

1. Cliquez sur l’outil **machines virtuelles** dans le volet de navigation de gauche.
2. En haut de l’outil machines virtuelles, choisissez l’onglet **inventaire** . Choisissez une machine virtuelle dans la liste et cliquez sur **plus** > **paramètres**.
3. Basculez entre les onglets **général**, **sécurité**, **mémoire**, **processeurs**, **disques**, **réseaux**, **ordre de démarrage** et **points de contrôle** , configurez les paramètres nécessaires, puis cliquez sur **Enregistrer** pour enregistrer. paramètres de l’onglet actuel. Les paramètres disponibles varient en fonction de la génération de la machine virtuelle. En outre, certains paramètres ne peuvent pas être modifiés pour les ordinateurs virtuels en cours d’exécution et vous devrez d’abord arrêter l’ordinateur virtuel.

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>Migrer dynamiquement un ordinateur virtuel vers un autre nœud de cluster

Si vous êtes connecté à un cluster, vous pouvez effectuer la migration dynamique d’un ordinateur virtuel vers un autre nœud de cluster.

1. À partir d’un cluster de basculement ou d’une connexion de cluster hyper-convergé, cliquez sur l’outil **machines virtuelles** dans le volet de navigation de gauche.
2. En haut de l’outil machines virtuelles, choisissez l’onglet **inventaire** . Choisissez une machine virtuelle dans la liste et cliquez sur **plus** > **déplacer**.
3. Choisissez un serveur dans la liste des nœuds de cluster disponibles, puis cliquez sur **déplacer**.
4. Les notifications relatives à la progression du déplacement s’affichent dans le coin supérieur droit du centre d’administration Windows. Si le déplacement réussit, le nom du serveur hôte est modifié dans la liste des machines virtuelles.

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>Gestion avancée et résolution des problèmes pour une seule machine virtuelle

![Écran de détails d’une machine virtuelle unique](../media/manage-virtual-machines/vm-details.png)

Vous pouvez afficher des informations détaillées et des graphiques de performances pour une seule machine virtuelle à partir de la page de l’ordinateur virtuel unique.

1. Cliquez sur l’outil **machines virtuelles** dans le volet de navigation de gauche.
2. En haut de l’outil machines virtuelles, choisissez l’onglet **inventaire** . Cliquez sur le nom d’un ordinateur virtuel dans la liste des machines virtuelles.
3. À partir de la page de l’ordinateur virtuel unique, vous pouvez :
    - Affichez des informations détaillées sur la machine virtuelle.
    - Affichez les graphiques en ligne de données historiques et en temps réel pour l’UC, la mémoire, le réseau, les e/s par seconde et le débit d’e/s (les données historiques sont uniquement disponibles pour les clusters hyper-convergent exécutant Windows Server 2019)
    - Afficher, créer, appliquer, renommer et supprimer des points de contrôle.
    - Affichez les détails des fichiers de disque dur virtuel (. vhd) de l’ordinateur virtuel, des cartes réseau et du serveur hôte.
    - Supprimer, démarrer, arrêter, arrêter, suspendre, reprendre, réinitialiser ou renommer l’ordinateur virtuel. Enregistrez également l’ordinateur virtuel, supprimez un état enregistré ou créez un point de contrôle.
    - [Modifiez les paramètres de la machine virtuelle](#change-virtual-machine-settings).
    - Connectez-vous à la console de la machine virtuelle à l’aide de VMConnect via l’hôte Hyper-V.
    - [Répliquez la machine virtuelle à l’aide de Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## <a name="manage-a-virtual-machine-through-the-hyper-v-host-vmconnect"></a>Gérer un ordinateur virtuel par le biais de l’hôte Hyper-V (VMConnect)

![Connexion de machine virtuelle via votre navigateur Web](../media/manage-virtual-machines/vm-connect.png)

1. Cliquez sur l’outil **machines virtuelles** dans le volet de navigation de gauche.
2. En haut de l’outil machines virtuelles, choisissez l’onglet **inventaire** . Choisissez une machine virtuelle dans la liste et cliquez sur **plus** > **se connecter** ou **Télécharger un fichier RDP**. La **connexion** vous permettra d’interagir avec la machine virtuelle invitée via la console Web Bureau à distance, intégrée au centre d’administration Windows. **Télécharger le fichier RDP** permet de télécharger un fichier. RDP que vous pouvez ouvrir avec l’application Connexion Bureau à distance (Mstsc. exe). Les deux options utilisent VMConnect pour se connecter à la machine virtuelle invitée par le biais de l’hôte Hyper-V. vous devrez donc entrer les informations d’identification d’administrateur pour le serveur hôte Hyper-V.

## <a name="change-hyper-v-host-settings"></a>Modifier les paramètres de l’hôte Hyper-V

![Écran des paramètres de l’hôte Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. Sur un serveur, un cluster hyper-convergé ou une connexion de cluster de basculement, cliquez sur le menu **paramètres** en bas du volet de navigation gauche.
2. Sur un serveur hôte ou un cluster Hyper-V, vous verrez un groupe de paramètres de l' **ordinateur hôte Hyper-v** avec les sections suivantes :
    - Général Changer le chemin de fichier des disques durs virtuels et des machines virtuelles, et le type de planification de l’hyperviseur (si pris en charge)
    - Mode de session étendu
    - Fractionnement NUMA
    - Migration dynamique
    - Migration du stockage
3. Si vous modifiez les paramètres de l’ordinateur hôte Hyper-V dans un cluster hyper-convergé ou une connexion de cluster de basculement, la modification sera appliquée à tous les nœuds du cluster.

## <a name="view-hyper-v-event-logs"></a>Afficher les journaux des événements Hyper-V

Vous pouvez afficher les journaux des événements Hyper-V directement à partir de l’outil machines virtuelles.

1. Cliquez sur l’outil **machines virtuelles** dans le volet de navigation de gauche.
2. En haut de l’outil machines virtuelles, choisissez l’onglet **Résumé** . Dans la section événements en haut à droite, cliquez sur **Afficher tous les événements**.
3. L’outil observateur d’événements affiche les canaux d’événements Hyper-V dans le volet gauche. Choisissez un canal pour afficher les événements dans le volet droit. Si vous gérez un cluster de basculement ou un cluster hyper-convergé, les journaux des événements affichent les événements de tous les nœuds de cluster, en affichant le serveur hôte dans la colonne ordinateur.

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>Protéger les machines virtuelles avec Azure Site Recovery

Vous pouvez utiliser le centre d’administration Windows pour configurer Azure Site Recovery et répliquer vos machines virtuelles locales sur Azure. [En savoir plus](../azure/azure-site-recovery.md)

## <a name="more-coming"></a>Plus à venir

La gestion des machines virtuelles dans le centre d’administration Windows est activement en cours de développement et de nouvelles fonctionnalités seront ajoutées dans un avenir proche. Vous pouvez afficher l’État et voter pour les fonctionnalités de UserVoice :

- [Importer/exporter des machines virtuelles](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)
- [Trier des machines virtuelles dans des dossiers](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)
- [Prendre en charge des paramètres de machine virtuelle supplémentaires](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)
- [Prise en charge des réplicas Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)
- [Déléguer la propriété de l’ordinateur virtuel](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)
- [Cloner l’ordinateur virtuel](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)
- [Créer un modèle à partir d’une machine virtuelle existante](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)
- [Afficher des machines virtuelles sur des hôtes Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)
- [Configurer le réseau local virtuel dans le nouveau volet de la machine virtuelle](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)

[Voir tout ou proposer de nouvelles fonctionnalités](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D).
