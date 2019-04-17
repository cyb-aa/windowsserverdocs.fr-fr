---
title: "Intégration d’Azure Site Recovery Services"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/01/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 85e681cfc19241941773dd94f05ba59759aaf62a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
#<a name="azure-site-recovery-services-integration"></a>Intégration d’Azure Site Recovery Services

>S’applique à: Windows Server2016Essentials

[Azure Site Recovery Services](https://azure.microsoft.com/en-us/documentation/services/site-recovery/) est un service proposé par activation de la réplication en temps réel de vos machines virtuelles (VM) de MicrosoftAzure pour un coffre de sauvegarde dans Azure. Dans le cas où votre serveur ou le site est arrêté en raison d’une panne matérielle ou autre, vous pouvez basculement vers Azure où l’image d’ordinateur virtuel stocké dans le coffre de sauvegarde est approvisionné en tant qu’un ordinateur virtuel en cours d’exécution dans Azure. Combiné avec un réseau virtuel Azure, en cas de basculement vers Azure, client PC précédemment connecté au serveur local en toute transparence se connectera au serveur en cours d’exécution dans Azure.

Intégration d’Azure Site Recovery Services avec WindowsServerEssentials démarre dans la même manière que la configuration [un réseau virtuel Azure](azure-virtual-network-integration.md). À partir de la **intégration à MicrosoftCloud Services** dans le tableau de bord, cliquez sur **intégrer à Azure Site Recovery Services** à droite du tableau de bord:

![Une capture d’écran montrant l’onglet mise en route sur la page d’accueil du tableau de bord WindowsServerEssentials. Sous l’onglet mise en route, la section Services a été sélectionnée, et le tableau de bord indique sous intégration à MicrosoftCloud Services que récupération Azure est actuellement désactivée.](media/azure-site-recovery-1.PNG)

Comme avec l’intégration de réseau virtuel Azure et intégration Azure Backup, vous devez connecter à Azure avec un compte Azure existant ou créer un nouveau compte:

![Une capture d’écran montrant le journal de page dans pour MicrosoftAzure de l’Assistant d’activer la réplication sur Azure. Le journal de bouton est affiché dans la mesure où l’utilisateur n’a pas encore connecté à MicrosoftAzure.](media/azure-site-recovery-2.PNG)

Une fois connecté dans Azure, vous verrez un écran qui vous demandera l’abonnement que vous souhaitez associer avec le Service de récupération de Site Azure, ainsi que la région Azure où votre ordinateur virtuel sera stocké et hébergé:

![Une capture d’écran montrant le journal de page dans pour MicrosoftAzure de l’Assistant d’activer la réplication sur Azure. Étant donné que l’utilisateur a ouvert une session dans MicrosoftAzure, cette page contient des options de sélection d’un abonnement, le compte de stockage et la région.](media/azure-site-recovery-3.PNG)

Une fois l’abonnement et la sélection de la région, un nouvel onglet s’affiche dans le **tableau de bord WindowsServerEssentials** appelée **Azure Recovery**. Analyse de la mise en réseau est effectuée pour identifier et énumérer les serveurs hôtes pris en charge (exécutant WindowsServerHyper-V2012R2 et versions ultérieures), ainsi que les ordinateurs virtuels (invités) sous les ordinateurs hôtes individuels:

![Une capture d’écran montrant la page récupération Azure du tableau de bord WindowsServerEssentials. Deux hôtes Hyper-V sont affichés, ainsi que les ordinateurs virtuels en cours d’exécution sur ces ordinateurs hôtes. Un ordinateur virtuel nommé ramh157v01 sur l’hôte H1-RAM-7 est sélectionné et la réplication vers Azure est actuellement désactivé pour cet ordinateur virtuel.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>L’activation des ordinateurs virtuels invités pour la protection

Lors de la sélection d’un ordinateur virtuel présent dans la fenêtre de récupération Azure, vous pouvez cliquer sur **activer la réplication vers Azure** sur le côté droit du tableau de bord pour préparer et copier la machine virtuelle™ image s vers Azure:

![Une capture d’écran montrant la boîte de dialogue Activer la réplication sur Azure. Une barre de progression s’affiche comme un ordinateur hôte est ajouté.](media/azure-site-recovery-5.PNG)

Pendant ce processus, l’agent de récupération de Site Azure Service est installé sur le serveur hôte, un coffre de sauvegarde où l’image de l’invité de qu'ordinateur virtuel sera stocké est créé, et le début de la réplication de l’image à Azure. Selon la taille de l’ordinateur virtuel en cours de réplication, l’achèvement du processus de réplication peut prendre des heures ou jours. Jusqu'à ce que la totalité de l’image VM et dernières deltas sont répliqués sur Azure, les tâches de basculement ne sont pas disponibles, et l’ordinateur virtuel n’est pas protégé. Une fois la réplication terminée, la colonne État de Protection dans la fenêtre Azure Recovery pourra **réplication** à **activé**:

![Une capture d’écran montrant la page récupération Azure du tableau de bord WindowsServerEssentials. Deux hôtes Hyper-V sont affichés, ainsi que les ordinateurs virtuels en cours d’exécution sur ces ordinateurs hôtes. Un ordinateur virtuel nommé ramh12v02 sur l’hôte H1-RAM-2 est sélectionné et la réplication vers Azure est actuellement activé pour cet ordinateur virtuel.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Basculement d’un invité d’ordinateur virtuel vers Azure

![Capture d’écran Failover VM à page Azure.](media/azure-site-recovery-7.PNG)

Lorsqu’un ordinateur virtuel qui est protégé échoue, ou le serveur hôte que l’exécution de la machine virtuelle protégée sur échoue, basculer vers Azure peut être effectuée pour assurer la continuité jusqu'à ce que le virtual machine ou un hôte serveur local est réparé et disponible. Comme l’illustration ci-dessus montre, il existe trois types de basculement qui sont pris en charge avec Azure Site Recovery Services:

-   **Test de basculement** plan de récupération d’urgence ƒA intègre la possibilité de simuler un incident pour vous assurer de temps d’arrêt minimal en cas d’incident réel. Un Test du basculement prend l’image d’ordinateur virtuel qui a été répliqué sur votre coffre de récupération, il configure comme un ordinateur virtuel en cours d’exécution dans Azure et vous permet de se connecter au serveur pour tester les scénarios qui s’appliquent à l’entreprise. Pendant un test de basculement, l’ordinateur virtuel local continue de s’exécuter avec aucun interruptions quant ne pas perturber business pendant le test de la récupération d’urgence. Une fois le Test du basculement terminée, vous arrêtez le test via le portail Azure, désélectionnez configure l’ordinateur virtuel et supprime le disque dur virtuel. Pendant le basculement test complet, l’image de machine virtuelle dans votre coffre de récupération continue de sont répliquées à partir de l’ordinateur virtuel sur site comme si rien ne s’est jamais produit.

-   **Basculement non planifié** ƒAn basculement non planifié se produit lorsqu’il existe une erreur réelle avec le serveur hôte protégé ou d’un ordinateur virtuel en cours d’exécution sur le serveur hôte. Le basculement est déclenché manuellement à partir d’un tableau de bord WindowsServerEssentials, ou si le serveur défaillant lui-même est le serveur que WindowsServerEssentials est en cours d’exécution, peut être déclenché à partir du portail Azure directement. Dans ce cas, le basculement non planifié prend l’image de machine virtuelle qui a été répliqué sur votre coffre de récupération, il configure comme un ordinateur virtuel en cours d’exécution dans Azure et vous permet de se connecter au serveur pour tester les scénarios qui s’appliquent à l’entreprise. Lorsque votre serveur est restauré sur local, un retour arrière manuels peut être déclenché à partir du portail Azure qui copie alors l’image VM retour vers le bas pour le serveur local.

-   **Basculement planifié** ƒA planifié basculement est une action qui peut être effectuée dans le cas où les activités planifiées, telle que la maintenance du matériel, doivent avoir lieu qui prendrait le serveur vers le bas. En cas d’un basculement planifié, le même processus a lieu en ce qui concerne l’approvisionnement de votre image d’ordinateur virtuel répliqué dans Azure. Toutefois, avant cela, le serveur local est arrêté de façon ordonnée pour vérifier que toutes les modifications sont répliquées vers Azure avant l’arrêt. Une fois que la maintenance planifiée est terminée, vous pouvez déclencher manuellement une restauration à partir du tableau de bord WindowsServerEssentials, ou le portail Azure, le mettre le serveur local et ensuite mettre hors service l’ordinateur virtuel dans Azure et supprimer. Fichier de disque dur virtuel. La réplication à partir de l’ordinateur virtuel local vers Azure puis continue de se produire à nouveau normalement.

Dans un des trois cas ci-dessus, lorsqu’un ordinateur virtuel est basculé sur Azure, le tableau de bord WindowsServerEssentials affiche la nouvelle machine virtuelle dans Azure en cours d’exécution comme dans la figure ci-dessous.

![Une capture d’écran montrant la page récupération Azure du tableau de bord WindowsServerEssentials. La réplication vers Azure a été activée pour un ordinateur hôte nommé Essentials et un ordinateur virtuel nommé Essentials-Test en cours d’exécution dans Azure indique que l’ordinateur hôte a basculé vers Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Voir aussi
--------
[Prise en main WindowsServerEssentials](get-started.md)
