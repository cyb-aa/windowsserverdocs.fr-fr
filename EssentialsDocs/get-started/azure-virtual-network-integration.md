---
title: Intégration du réseau virtuel Azure
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 92c8241d861e72d5f9f409a334e6edbeed5eae4c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310574"
---
# <a name="azure-virtual-network-integration"></a>Intégration du réseau virtuel Azure

>S’applique à : Windows Server 2016 Essentials

À mesure que les organisations font leur cloud computing, elles peuvent rarement déplacer toutes leurs ressources 100% à la fois, mais plutôt adopter une approche dans laquelle certaines ressources se trouvent dans le Cloud et d’autres encore en local. Grâce à cette approche hybride, les organisations peuvent non seulement déplacer des ressources informatiques dans le Cloud, mais aussi leur permettre de développer leur infrastructure informatique sans avoir à acquérir de nouveaux matériels.

Lors de l’implémentation de cette approche hybride du calcul, un moyen transparent pour les ressources des deux emplacements de communiquer entre eux est requis. La mise en réseau virtuel Azure est un service Azure qui permet aux organisations de créer un réseau privé virtuel (point à point) ou de site à site (S2S) qui rend les ressources qui s’exécutent dans Azure (telles que les ordinateurs virtuels et le stockage) comme s’ils étaient sur le réseau local pour un accès transparent aux applications et aux ressources.

La configuration d’un réseau virtuel Azure peut être complexe. Avec Windows Server Essentials 2016, vous pouvez facilement configurer votre réseau virtuel Azure à l’aide d’un assistant simple qui vous permet de choisir les valeurs par défaut les plus appropriées pour votre environnement réseau. Comme indiqué dans la capture d’écran ci-dessous, une nouvelle tâche d’intégration de réseau virtuel Azure a été ajoutée à la section services Microsoft Cloud du tableau de bord Windows Essentials pour présenter le réseau virtuel Azure et fournir un lien rapide pour lancer l’intégration .

![Capture d’écran montrant l’onglet prise en main sur la page d’accueil du tableau de bord Windows Server Essentials. Dans l’onglet prise en main, la section services a été sélectionnée et le tableau de bord indique sous Microsoft Cloud intégration des services que le réseau virtuel Azure est actuellement désactivé.](media/azure-virtual-network-1.PNG)

Lorsque vous cliquez sur le lien **intégrer maintenant** pour la mise en réseau virtuelle Azure dans la capture d’écran ci-dessus, une boîte de dialogue s’affiche pour vous demander de vous connecter à votre compte Microsoft Azure. Si vous n’avez pas de compte Microsoft Azure, vous avez la possibilité d’en créer un sur cet écran, ce qui vous redirige vers le portail d’inscription de compte Azure :

![Capture d’écran montrant la page se connecter à Microsoft Azure de l’Assistant intégration avec le réseau virtuel Azure.](media/azure-virtual-network-2.PNG)

Une fois connecté à Azure, vous pouvez choisir l’abonnement qu’il souhaite associer au service de mise en réseau virtuel Azure :

![Capture d’écran montrant la page My Microsoft Azure Subscription de l’Assistant intégration avec le réseau virtuel Azure.](media/azure-virtual-network-3.PNG)

Une fois que vous avez choisi l’abonnement Azure que vous souhaitez utiliser pour la mise en réseau virtuel Azure, vous avez la possibilité de créer un réseau virtuel Azure, ou s’il a déjà été configuré dans le cadre de cet abonnement, la zone de liste déroulante indique qu’il est disponible. Vous devez également choisir un nom pour le réseau local qui sera utilisé par le réseau virtuel Azure pour identifier les ressources de votre réseau local. Enfin, vous allez choisir la région Azure dans laquelle vous souhaitez que le réseau virtuel Azure soit hébergé. Le choix d’un emplacement physiquement le plus proche de votre réseau local est généralement le mieux adapté à l’optimisation de la vitesse de la bande passante pour communiquer avec les ressources que vous pouvez héberger dans ses services Azure :

![Capture d’écran montrant la page Configurer le réseau virtuel Azure de l’Assistant intégration avec le réseau virtuel Azure.](media/azure-virtual-network-4.PNG)

La dernière étape du processus d’intégration consiste à configurer le périphérique VPN qui sera utilisé pour la connexion VPN S2S. Étant donné que la plupart des petites entreprises n’ont que quelques serveurs dans leur environnement et ne disposent pas du personnel informatique pour configurer correctement un routeur VPN pour se connecter à Microsoft Azure, la sélection par défaut consiste à configurer le serveur Windows Server Essentials en tant que serveur VPN auquel les ressources dans votre réseau local, connectez-vous à pour accéder aux ressources du réseau virtuel Azure. Toutefois, si vous préférez utiliser un autre serveur de votre environnement en tant que serveur VPN ou si vous préférez utiliser un routeur VPN, vous pouvez sélectionner ces options.

En raison de la variation des modèles et des types de routeur, Windows Server Essentials ne tente pas de configurer automatiquement le routeur VPN. La sélection du routeur VPN dans cet Assistant d’intégration ne notifie que la mise en réseau virtuelle Azure du type d’appareil pour les configurations de routage appropriées nécessaires dans Azure pour la connectivité.

À la fin de l’Assistant intégration, un nouvel onglet est visible dans le tableau de bord Windows Server Essentials pour la mise en réseau virtuel Azure :

![Capture d’écran montrant la page réseau virtuel Azure du tableau de bord Windows Server Essentials. L’onglet réseau virtuel Azure est sélectionné et affiche l’état de configuration.](media/azure-virtual-network-5.PNG)

>! Remarque : l’exécution de la configuration d’un réseau virtuel Azure dans le Cloud peut prendre beaucoup de temps, jusqu’à 30 minutes. Pendant ce temps, l’état de la configuration de sera présent dans la page État du réseau virtuel Azure du tableau de bord.

Une fois la configuration du réseau virtuel Azure terminée, l’état passe à connecté et affiche les détails de votre réseau virtuel Azure, tels que les données en provenance/sortie, l’adresse IP de la passerelle, l’adresse IP locale et les détails du compte :

![Capture d’écran montrant la page réseau virtuel Azure du tableau de bord Windows Server Essentials. L’onglet réseau virtuel Azure est sélectionné et affiche l’état connecté, et sous ces informations d’État, les détails du réseau virtuel s’affichent.](media/azure-virtual-network-6.PNG)

Dans le volet tâches situé à droite du tableau de bord se trouvent les différentes tâches que vous pouvez effectuer avec votre réseau virtuel Azure.

-   Se **déconnecter d’Azure VNET** La configuration d’un réseau virtuel Azure est gratuite, mais des frais sont facturés pour la passerelle VPN qui se connecte à local et à d’autres réseaux virtuels dans Azure. La déconnexion du réseau virtuel Azure arrête toutes les facturations.

-   **Basculer le périphérique VPN** Dans le cas où vous souhaitez passer d’un serveur VPN à un routeur VPN, cette tâche vous permet d’effectuer le basculement et d’informer le réseau virtuel Azure.

-   **Configurer** un réseau virtuel Azure Cette tâche vous permet de modifier les options de configuration avancées du réseau virtuel Azure en les redirigeant vers la page de configuration Portail Azure pour Azure VNET.

-   **État** de l’actualisation Actualise la page d’État, en mettant à jour l’état de la connexion du réseau virtuel Azure, y compris les données sortantes/sortantes.

-   **Désactiver Azure intégration au réseau virtuel** Déconnecte le réseau virtuel Azure et supprime l’intégration du tableau de bord Windows Server Essentials. Remarque : cela ne supprime pas le réseau virtuel Azure, les paramètres sont toujours conservés dans Azure si vous souhaitez réintégrer ultérieurement Azure VNET avec le tableau de bord.

-   **En savoir plus sur Azure VNET** [https://azure.microsoft.com/services/virtual-network/](https://azure.microsoft.com/services/virtual-network/).

<a name="see-also"></a>Voir aussi
--------
[Prise en main de Windows Server Essentials](get-started.md)
