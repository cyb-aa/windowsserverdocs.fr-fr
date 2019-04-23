---
title: Intégration de réseau virtuel Azure
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6193d2e2a2c0b85e13d4e9474264d2581bea88fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879360"
---
#<a name="azure-virtual-network-integration"></a>Intégration de réseau virtuel Azure

>S'applique à : WindowsServer2016 Essentials

Comme les organisations font place à l’informatique en nuage, rarement seront ils déplacer toutes leurs ressources 100 % en même temps, mais plutôt prendre une approche où certaines ressources sont dans le cloud et certains sont toujours en local. Cette approche hybride facilite pour les entreprises non seulement déplacent des ressources informatiques vers le cloud, mais également leur permet d’augmenter leur infrastructure informatique sans avoir à acquérir de nouveau matériel.

Lorsque vous implémentez cette approche hybride à l’informatique, de manière transparente pour les ressources dans les deux emplacements pour communiquer entre eux est requis. Un réseau virtuel Azure est un service Azure qui permet aux organisations de créer un point à point (P2P) ou un site à site (S2S) réseau privé virtuel qui rend les ressources qui sont en cours d’exécution dans Azure (par exemple, les machines virtuelles et stockage) rechercher comme si elles étaient sur le réseau local pour un accès transparent application et ressource.

Configuration d’un réseau virtuel Azure peut être complexe. Avec Windows Server Essentials 2016, vous pouvez facilement configurer votre réseau virtuel Azure via un Assistant simple qui vous permet de choisir les valeurs par défaut plus appropriées pour votre environnement réseau. Comme indiqué dans la capture d’écran ci-dessous, une nouvelle tâche de l’intégration de réseau virtuel Azure a été ajoutée à la section Services de Cloud de Microsoft du tableau de bord Windows Essentials pour introduire un réseau virtuel Azure ainsi que de fournir un lien pour lancer l’intégration rapide .

![Une capture d’écran montrant l’onglet mise en route sur la page d’accueil du tableau de bord Windows Server Essentials. Sous l’onglet mise en route, la section Services a été sélectionnée, et le tableau de bord indique sous intégration de Services Cloud Microsoft que le réseau virtuel Azure est actuellement désactivée.](media/azure-virtual-network-1.PNG)

Lorsque vous cliquez sur le **intégrer maintenant** lier pour les réseaux virtuels Azure dans la capture d’écran ci-dessus, une boîte de dialogue s’affiche vous demandant de se connecter à votre compte Microsoft Azure. Si vous n’avez pas d’un compte Microsoft Azure, avoir l’option pour vous inscrire pour un sur cet écran, ce qui vous redirige vers le portail d’inscription de compte Azure :

![Une capture d’écran montrant la page de connexion à Microsoft Azure de l’Assistant réseau de l’intégration avec Azure Virtual.](media/azure-virtual-network-2.PNG)

Une fois que la signature dans Azure, s’affiche avec l’option à choisir quel abonnement ils souhaitent associer Azure virtuel mise en réseau de service :

![Une capture d’écran montrant la page de mon abonnement Microsoft Azure de l’intégration avec l’Assistant de réseau virtuel Azure.](media/azure-virtual-network-3.PNG)

Une fois que vous avez choisi de quel abonnement Azure que vous souhaitez utiliser pour un réseau virtuel Azure, s’affiche avec l’option permettant de créer un réseau virtuel Azure, ou si un a déjà été le programme d’installation sous cet abonnement, la zone de liste déroulante affichera qu'il est disponible. Choisissez également un nom pour le réseau Local que le réseau virtuel Azure utilisera pour identifier les ressources dans votre réseau local. Enfin, vous allez choisir quelle région Azure dans lequel vous souhaitez leur réseau virtuel Azure à héberger. Choix d’un emplacement qui est physiquement plus proche de votre réseau local est généralement préférable pour l’optimisation de la vitesse de la bande passante pour la communication avec les ressources que vous pouvez héberger dans leurs services Azure :

![Une capture d’écran montrant la page réseau défini de virtuelle Azure de l’Assistant réseau de l’intégration avec Azure Virtual.](media/azure-virtual-network-4.PNG)

La dernière étape du processus d’intégration consiste à configurer le périphérique VPN qui sera utilisé pour la connexion VPN de S2S. Dans la mesure où la plupart des petites entreprises ont des uniquement quelques serveurs dans leur environnement et ne disposent pas de l’équipe informatique pour configurer correctement un routeur VPN pour se connecter à Microsoft Azure, la sélection par défaut sera pour configurer le serveur Windows Server Essentials en tant que le serveur VPN que les ressources dans votre réseau local se connecte à puisse accéder aux ressources dans le réseau virtuel Azure. Toutefois, si vous préférez utiliser un autre serveur dans votre environnement en tant que le serveur VPN, ou si vous préférez utiliser un routeur VPN, vous pouvez sélectionner ces options.

En raison de la variation dans les modèles et les types de routeur, Windows Server Essentials n’essaie pas configurer automatiquement le routeur VPN. En sélectionnant le routeur VPN dans cet Assistant Intégration signale uniquement la mise en réseau virtuel Azure de type de périphérique pour les configurations de routage appropriés nécessaires dans Azure pour la connectivité.

À la fin de l’Assistant d’intégration, un nouvel onglet sera visible dans le tableau de bord Windows Server Essentials pour un réseau virtuel Azure :

![Une capture d’écran montrant la page réseau virtuel Azure du tableau de bord Windows Server Essentials. L’onglet réseau virtuel Azure est sélectionné et affiche l’état de configuration.](media/azure-virtual-network-5.PNG)

>! Notez que la fin de la configuration d’un réseau virtuel Azure dans le cloud peut prendre beaucoup de temps, vers le haut jusqu'à 30 minutes. Pendant ce temps, l’état de configuration sera présent dans la page d’état de réseau virtuel Azure du tableau de bord.

Une fois la configuration du réseau virtuel Azure est terminée, l’état modifie connecté et afficher les détails de votre réseau virtuel Azure comme données d’entrée/sortie, adresse IP de passerelle, local IP adresse et compte les informations :

![Une capture d’écran montrant la page réseau virtuel Azure du tableau de bord Windows Server Essentials. L’onglet réseau virtuel Azure est sélectionné et affiche l’état connecté et sous ces informations d’état sont affichés les détails du réseau virtuel.](media/azure-virtual-network-6.PNG)

Dans le volet de tâches sur le côté droit du tableau de bord sont les différentes tâches que vous pouvez prendre avec votre réseau virtuel Azure.

-   **Déconnexion d’Azure réseau virtuel** configuration d’un réseau virtuel Azure est gratuite, mais il existe un coût correspondant à la passerelle VPN qui se connecte en local et d’autres réseaux virtuels dans Azure. Déconnexion du réseau virtuel Azure arrête toute la facturation.

-   **Basculer sur un périphérique VPN** dans le cas où vous souhaitez modifier à partir d’un serveur VPN à un routeur VPN, cette tâche vous permettra de rendre le commutateur et d’avertir le réseau virtuel Azure.

-   **Configurer le réseau virtuel Azure** cette tâche vous permet de modifier les options de configuration avancée du réseau virtuel Azure en les redirigeant vers la page de configuration du portail Azure pour le réseau virtuel Azure.

-   **Actualiser l’état** actualise la page d’état, la mise à jour l’état de connexion du réseau virtuel Azure, y compris les données en entrée/sortie.

-   **Désactiver l’intégration de réseau virtuel Azure** se déconnecte du réseau virtuel Azure et supprime l’intégration à partir du tableau de bord Windows Server Essentials. Notez cette opération ne supprime pas le réseau virtuel Azure, de paramètres sont toujours conservés dans Azure si vous souhaitez réintégrer ultérieurement réseau virtuel Azure avec le tableau de bord.

-   **En savoir plus sur le réseau virtuel Azure** [ https://azure.microsoft.com/services/virtual-network/ ](https://azure.microsoft.com/services/virtual-network/).

<a name="see-also"></a>Voir aussi
--------
[Prise en main Windows Server Essentials](get-started.md)
