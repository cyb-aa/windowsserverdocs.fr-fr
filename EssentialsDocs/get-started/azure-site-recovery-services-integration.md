---
title: Intégration des services Azure Site Recovery
description: Décrit comment utiliser Windows Server Essentials
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
ms.openlocfilehash: e9dabc1b986fd86b5ee3efc6022023b331046250
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433952"
---
# <a name="azure-site-recovery-services-integration"></a>Intégration des services Azure Site Recovery

>S'applique à : WindowsServer2016 Essentials

[Azure Site Recovery Services](https://docs.microsoft.com/azure/site-recovery/) est un service proposé par Microsoft Azure, activez la réplication en temps réel de vos machines virtuelles (VM) dans un coffre de sauvegarde dans Azure. Dans le cas où votre serveur ou le site est arrêté en raison d’une défaillance matérielle ou autre, vous pouvez basculement vers Azure où l’image de machine virtuelle stockée dans votre coffre de sauvegarde sera déployé comme une machine virtuelle en cours d’exécution dans Azure. Combiné avec un réseau virtuel Azure, en cas de basculement vers Azure, le client PC précédemment connecté au serveur local en toute transparence se connectera au serveur en cours d’exécution dans Azure.

L’intégration d’Azure Site Recovery Services avec Windows Server Essentials démarre dans la même manière que la configuration de [mise en réseau virtuel Azure](azure-virtual-network-integration.md). À partir de la **intégration des Services Cloud Microsoft** dans le tableau de bord, cliquez sur **intégrer avec Azure Site Recovery Services** à droite du tableau de bord :

![Une capture d’écran montrant l’onglet mise en route sur la page d’accueil du tableau de bord Windows Server Essentials. Sous l’onglet mise en route, la section Services a été sélectionnée, et le tableau de bord indique sous intégration de Services Cloud Microsoft que Azure Recovery est actuellement désactivée.](media/azure-site-recovery-1.PNG)

Comme avec l’intégration du réseau virtuel Azure et l’intégration de sauvegarde Azure, vous devez vous connecter à Azure avec un compte Azure existant ou créer un nouveau compte :

![Une capture d’écran montrant la page de connexion à Microsoft Azure de l’Assistant Activer la réplication vers Azure. Le bouton se connecter s’affiche, car l’utilisateur n’a pas encore connecté à Microsoft Azure.](media/azure-site-recovery-2.PNG)

Après vous être connecté avec succès dans Azure, vous verrez un écran qui vous demande quel abonnement que vous souhaitez associer le Service Azure Site Recovery, ainsi que la région Azure où votre machine virtuelle est stockée et hébergé :

![Une capture d’écran montrant la page de connexion à Microsoft Azure de l’Assistant Activer la réplication vers Azure. Étant donné que l’utilisateur s’est connecté à Microsoft Azure, cette page fournit des options pour la sélection d’un abonnement, le compte de stockage et la région.](media/azure-site-recovery-3.PNG)

Après l’abonnement et la sélection de la région, un nouvel onglet s’affiche dans le **tableau de bord Windows Server Essentials** appelé **Azure Recovery**. Analyse de la mise en réseau est effectuée afin d’identifier et d’énumérer les serveurs hôtes pris en charge (exécutant Windows Server Hyper-V 2012 R2 et versions ultérieures), ainsi que les ordinateurs virtuels (invités) sous les ordinateurs hôtes individuels :

![Une capture d’écran montrant la page Azure Recovery du tableau de bord Windows Server Essentials. Deux hôtes Hyper-V sont affichés, ainsi que les machines virtuelles en cours d’exécution sur ces ordinateurs hôtes. Un ordinateur virtuel nommé ramh157v01 sur hôte RAM-H1-7 est sélectionné et la réplication vers Azure est actuellement désactivé pour cette machine virtuelle.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Activer les machines virtuelles invitées pour la protection

Fonction de sélection d’un ordinateur virtuel présent dans la fenêtre de récupération d’Azure, vous pouvez cliquer sur **activer la réplication vers Azure** sur le côté droit du tableau de bord pour préparer et copier la machine virtuelle™ image s dans Azure :

![Une capture d’écran montrant la boîte de dialogue Activer la réplication vers Azure. Une barre de progression s’affiche comme un hôte est ajouté.](media/azure-site-recovery-5.PNG)

Pendant ce processus, l’agent du Service Azure Site Recovery est installé sur le serveur hôte, un coffre de sauvegarde où l’image de l’invité de qu'ordinateur virtuel sera stocké est créée et la réplication de l’image à Azure commence. Selon la taille de la machine virtuelle en cours de réplication, l’achèvement du processus de réplication peut prendre des heures ou jours. Jusqu'à ce que l’image de machine virtuelle entière et dernière deltas sont répliqués sur Azure, les tâches de basculement ne sont pas disponibles et la machine virtuelle n’est pas protégée. Une fois la réplication terminée, la colonne d’état de Protection dans la fenêtre Azure Recovery pourra **réplication** à **activé**:

![Une capture d’écran montrant la page Azure Recovery du tableau de bord Windows Server Essentials. Deux hôtes Hyper-V sont affichés, ainsi que les machines virtuelles en cours d’exécution sur ces ordinateurs hôtes. Un ordinateur virtuel nommé ramh12v02 sur hôte RAM-H1-2 est sélectionnée et la réplication vers Azure est actuellement activée pour cette machine virtuelle.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Basculement d’un invité d’ordinateur virtuel vers Azure

![Capture d’écran montrant Failover VM à la page Azure.](media/azure-site-recovery-7.PNG)

Lorsqu’une machine virtuelle qui est protégé échoue, ou le serveur hôte qui les séries de machine virtuelle protégée sur échoue, le basculement vers Azure peuvent être effectuées pour maintenir la continuité des activités jusqu'à ce que le sur site virtual machine serveur ou hôte est réparé et disponible. Comme la figure ci-dessus, il existe trois types de basculement qui sont pris en charge avec Azure Site Recovery Services :

-   **Test de basculement** plan de récupération d’urgence satisfaisante ƒA incorpore la capacité à simuler un incident pour garantir des temps d’arrêt minimal en cas d’incident réel. Un Test de basculement prend l’image de machine virtuelle qui ont été répliquée vers votre coffre de récupération, il configure comme une machine virtuelle en cours d’exécution dans Azure et vous permet de se connecter au serveur pour tester des scénarios qui s’appliquent à l’entreprise. Pendant un test de basculement, la machine virtuelle locale continue de s’exécuter sans s’interrompre à ne perturbent pas l’activité pendant le test de récupération d’urgence. Une fois le basculement de Test est terminé, vous arrêtez le test via le portail Azure, ce qui annule la machine virtuelle, ainsi que le disque dur virtuel. Pendant le basculement de test complet, l’image de machine virtuelle dans votre coffre recovery continue répliquée à partir de la machine virtuelle sur site comme si rien ne s’est jamais produit.

-   **Basculement non planifié** ƒAn basculement non planifié se produit lors d’une erreur réelle avec le serveur hôte protégé ou de la machine virtuelle en cours d’exécution sur le serveur hôte. Le basculement est déclenché manuellement à partir d’un tableau de bord Windows Server Essentials, ou si le serveur en échec lui-même est le serveur que Windows Server Essentials est en cours d’exécution, peut être déclenché à partir du portail Azure directement. Dans ce cas, le basculement non planifié prend l’image de machine virtuelle qui ont été répliquée vers votre coffre de récupération, il configure comme une machine virtuelle en cours d’exécution dans Azure et vous permet de se connecter au serveur pour tester des scénarios qui s’appliquent à l’entreprise. Lorsque votre serveur est restauré en local, une restauration manuelle peut être déclenchée à partir du portail Azure que vous pouvez copier l’image de machine virtuelle décrémentielle vers le serveur local.

-   **Basculement planifié** ƒA planifié le basculement est une action qui peut être effectuée dans le cas où les activités planifiées, telles que la maintenance du matériel, doivent avoir lieu ce qui prendrait le serveur vers le bas. En cas d’un basculement planifié, le même processus a lieu en ce qui concerne l’approvisionnement de votre image de machine virtuelle répliquée dans Azure. Toutefois, avant cela, le serveur local est arrêté de façon ordonnée pour vous assurer que toutes les modifications sont répliquées vers Azure avant l’arrêt. Une fois la maintenance planifiée est terminée, vous pouvez déclencher manuellement une restauration automatique à partir du tableau de bord Windows Server Essentials, ou le portail Azure, ce qui aurait accès mettre le serveur local et puis retirer la machine virtuelle dans Azure et supprimer les. Fichier de disque dur virtuel. Réplication à partir de la machine virtuelle locale vers Azure puis continue à se produire plusieurs fois comme d’habitude.

Dans tous les trois cas ci-dessus, quand une machine virtuelle est basculée vers Azure, le tableau de bord Windows Server Essentials affichera la nouvelle machine virtuelle dans Azure en cours d’exécution, comme dans la figure ci-dessous.

![Une capture d’écran montrant la page Azure Recovery du tableau de bord Windows Server Essentials. Réplication vers Azure a été activée pour un ordinateur hôte nommé Essentials et un ordinateur virtuel nommé Essentials-Test en cours d’exécution dans Azure indique que l’hôte a basculé vers Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Voir aussi
--------
[Prise en main de Windows Server Essentials](get-started.md)
