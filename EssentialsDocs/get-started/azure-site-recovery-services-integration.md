---
title: Intégration des services Azure Site Recovery
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/01/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b947ca49a82c18fd7a6c1da71b1e4b43ea741b41
ms.sourcegitcommit: f247065941508b913c31828944978d3e721e2110
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82876414"
---
# <a name="azure-site-recovery-services-integration"></a>Intégration des services Azure Site Recovery 

>S’applique à : Windows Server 2016 Essentials

[Azure Site Recovery Services](https://docs.microsoft.com/azure/site-recovery/) est un service proposé par Microsoft Azure l’activation de la réplication en temps réel de vos machines virtuelles vers un coffre de sauvegarde dans Azure. Si votre serveur ou site est en panne en raison d’une défaillance matérielle ou autre, vous pouvez basculer vers Azure, où l’image de machine virtuelle stockée dans votre coffre de sauvegarde sera approvisionnée en tant que machine virtuelle en cours d’exécution dans Azure. Combiné avec un réseau virtuel Azure, en cas de basculement vers Azure, les PC clients qui étaient précédemment connectés au serveur local se connectent de manière transparente au serveur s’exécutant dans Azure.

L’intégration de Azure Site Recovery Services avec Windows Server Essentials démarre de la même manière que la configuration de la [mise en réseau virtuel Azure](azure-virtual-network-integration.md). Dans la page d' **intégration des services Microsoft Cloud** du tableau de bord, cliquez sur **intégrer avec les services Azure Site Recovery** à droite du tableau de bord :

![Capture d’écran montrant l’onglet prise en main sur la page d’accueil du tableau de bord Windows Server Essentials. Dans l’onglet prise en main, la section services a été sélectionnée et le tableau de bord indique sous Microsoft Cloud intégration des services qu’Azure Recovery est actuellement désactivée.](media/azure-site-recovery-1.PNG)

Comme avec l’intégration du réseau virtuel Azure et l’intégration de la sauvegarde Azure, vous devez vous connecter à Azure avec votre compte Azure existant ou créer un nouveau compte :

![Capture d’écran montrant la page se connecter à Microsoft Azure de l’Assistant Activation de la réplication vers Azure. Le bouton de connexion s’affiche car l’utilisateur ne s’est pas encore connecté à Microsoft Azure.](media/azure-site-recovery-2.PNG)

Après vous être connecté à Azure, un écran s’affiche pour vous demander l’abonnement que vous souhaitez associer au service Azure Site Recovery, ainsi que la région Azure où votre machine virtuelle sera stockée et hébergée :

![Capture d’écran montrant la page se connecter à Microsoft Azure de l’Assistant Activation de la réplication vers Azure. Étant donné que l’utilisateur s’est connecté à Microsoft Azure, cette page fournit des options pour sélectionner un abonnement, un compte de stockage et une région.](media/azure-site-recovery-3.PNG)

Une fois l’abonnement et la sélection de la région sélectionnés, un nouvel onglet apparaît dans le **tableau de bord Windows Server Essentials** nommé **récupération Azure**. L’analyse réseau est effectuée pour identifier et énumérer les serveurs hôtes pris en charge (exécutant Windows Server Hyper-V 2012 R2 et versions ultérieures), ainsi que les machines virtuelles (invités) sous les ordinateurs hôtes individuels :

![Capture d’écran montrant la page de récupération Azure du tableau de bord Windows Server Essentials. Deux hôtes Hyper-V sont affichés avec les ordinateurs virtuels en cours d’exécution sur ces ordinateurs hôtes. Une machine virtuelle nommée ramh157v01 sur l’ordinateur hôte RAM-H1-7 est sélectionnée, et la réplication vers Azure est actuellement désactivée pour cette machine virtuelle.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Activation des machines virtuelles invitées pour la protection

Lors de la sélection d’une machine virtuelle présente dans la fenêtre de récupération Azure, vous pouvez cliquer sur **activer la réplication vers Azure** sur le côté droit du tableau de bord &trade;pour préparer et copier l’image de l’ordinateur virtuel dans Azure :

![Capture d’écran montrant la boîte de dialogue Activer la réplication vers Azure. Une barre de progression s’affiche lorsqu’un ordinateur hôte est en cours d’ajout.](media/azure-site-recovery-5.PNG)

Au cours de ce processus, l’agent de service Azure Site Recovery est installé sur le serveur hôte, un coffre de sauvegarde dans lequel l’image de la machine virtuelle invitée sera stockée est créé, et la réplication de l’image vers Azure démarre. Selon la taille de la machine virtuelle répliquée, l’achèvement du processus de réplication peut prendre plusieurs heures ou plusieurs jours. Tant que la totalité de l’image de machine virtuelle et des deltas les plus récents ne sont pas répliquées vers Azure, les tâches de basculement ne sont pas disponibles et la machine virtuelle n’est pas protégée. Une fois la réplication terminée, la colonne État de la protection de la fenêtre récupération Azure passe de **réplication** à **activé**:

![Capture d’écran montrant la page de récupération Azure du tableau de bord Windows Server Essentials. Deux hôtes Hyper-V sont affichés avec les ordinateurs virtuels en cours d’exécution sur ces ordinateurs hôtes. Un ordinateur virtuel nommé ramh12v02 sur l’ordinateur hôte RAM-H1-2 est sélectionné, et la réplication vers Azure est actuellement activée pour cette machine virtuelle.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Basculement d’une machine virtuelle invitée vers Azure

![Capture d’écran montrant la machine virtuelle de basculement vers la page Azure.](media/azure-site-recovery-7.PNG)

Lorsqu’un ordinateur virtuel protégé échoue ou que le serveur hôte sur lequel s’exécute l’ordinateur virtuel protégé échoue, le basculement vers Azure peut être effectué pour assurer la continuité de l’activité jusqu’à ce que la machine virtuelle ou le serveur hôte local soit réparé et disponible. Comme le montre la figure ci-dessus, il existe trois types de basculement pris en charge avec les services Azure Site Recovery :

-   **Test de basculement** ƒA un bon plan de récupération d’urgence intègre la possibilité de simuler un sinistre pour garantir un temps d’arrêt minimal en cas de catastrophe. Un test de basculement prend l’image de machine virtuelle qui a été répliquée dans votre coffre de récupération, la configure en tant que machine virtuelle en cours d’exécution dans Azure et vous permet de vous connecter au serveur pour tester les scénarios qui s’appliquent à l’entreprise. Pendant un test de basculement, la machine virtuelle locale continue à s’exécuter sans interruptions, car elle ne perturbe pas l’activité pendant le test de récupération d’urgence. Une fois le test de basculement terminé, vous arrêtez le test via le portail Azure, qui annule la disposition de l’ordinateur virtuel et supprime le disque dur virtuel. Pendant l’intégralité du test de basculement, l’image de machine virtuelle dans votre coffre de récupération continue d’être répliquée à partir de la machine virtuelle sur site comme si rien ne s’est produit.

-   **Basculement non planifié** ƒAn un basculement non planifié se produit lorsqu’il existe une erreur réelle avec le serveur hôte protégé ou une machine virtuelle en cours d’exécution sur le serveur hôte. Le basculement est déclenché manuellement à partir du tableau de bord Windows Server Essentials, ou si le serveur défaillant lui-même est le serveur sur lequel Windows Server Essentials est exécuté, peut être déclenché directement à partir du portail Azure. Dans ce cas, le basculement non planifié prend l’image de machine virtuelle qui a été répliquée dans votre coffre de récupération, la configure en tant que machine virtuelle en cours d’exécution dans Azure et vous permet de vous connecter au serveur pour tester les scénarios qui s’appliquent à l’entreprise. Lorsque votre serveur est restauré localement, une restauration manuelle peut être déclenchée à partir du portail Azure, ce qui permet de copier l’image de machine virtuelle vers le serveur local.

-   Basculement **planifié** ƒA un basculement planifié est une action qui peut être effectuée au cas où des activités planifiées, telles que la maintenance matérielle, doivent avoir lieu, ce qui ralentit le serveur. En cas de basculement planifié, le même processus a lieu en ce qui concerne l’approvisionnement de votre image de machine virtuelle répliquée dans Azure. Toutefois, avant cela, le serveur local est arrêté de manière ordonnée pour garantir que toutes les modifications sont répliquées vers Azure avant l’arrêt. Une fois la maintenance planifiée terminée, vous pouvez déclencher manuellement une restauration automatique à partir du tableau de bord Windows Server Essentials ou de la Portail Azure, ce qui entraînerait l’installation du serveur local, puis l’annulation de l’approvisionnement de la machine virtuelle dans Azure et la suppression de la. Fichier VHD. La réplication à partir de la machine virtuelle locale vers Azure continuera à se reproduira normalement.

Dans l’un des trois cas ci-dessus, quand une machine virtuelle est basculée vers Azure, le tableau de bord Windows Server Essentials affiche la nouvelle machine virtuelle dans Azure qui s’exécute comme dans la figure ci-dessous.

![Capture d’écran montrant la page de récupération Azure du tableau de bord Windows Server Essentials. La réplication vers Azure a été activée pour un ordinateur hôte nommé Essentials, et une machine virtuelle nommée Essentials-test en cours d’exécution dans Azure indique que l’ordinateur hôte a basculé vers Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Voir aussi
--------
[Prise en main de Windows Server Essentials](get-started.md)
