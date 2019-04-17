---
title: "Intégration de réseau virtuel Azure"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: 21057359967c73d8d9fc434694b8f203ad5c1f6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
#<a name="azure-virtual-network-integration"></a>Intégration de réseau virtuel Azure

>S’applique à: Windows Server2016Essentials

Lorsque les entreprises mettent leur moyen de cloud computing, rarement seront ils déplacer toutes leurs ressources 100% en même temps, mais plutôt adopter une approche où certaines ressources se trouvent dans le cloud et d’autres encore dans les locaux. Cette approche hybride facilite aux entreprises non seulement déplacement des ressources informatiques sur le cloud, mais leur permet également d’augmenter leur infrastructure informatique sans avoir à acquérir de nouveaux matériels.

Lorsque vous implémentez cette approche hybride pour l’informatique, un moyen transparent pour les ressources dans les emplacements de communiquer entre eux est nécessaire. Un réseau virtuel Azure est un service Azure qui permet aux organisations de créer un point à point (P2P) ou un site à site (S2S) réseau privé virtuel qui rend les ressources qui sont exécutent dans la vue Azure (par exemple, les ordinateurs virtuels et stockage) comme s’ils se trouvent sur le réseau local pour un accès transparent applications et des ressources.

Configuration d’un réseau virtuel Azure peut être complexe. Avec Windows Server2016Essentials, vous pouvez facilement configurer votre réseau virtuel Azure via un Assistant simple qui vous permet de choisir les paramètres par défaut la plus appropriées pour votre environnement réseau. Comme indiqué dans la capture d’écran ci-dessous, une nouvelle tâche de l’intégration de réseau virtuel Azure a été ajoutée à la section MicrosoftCloud Services du tableau de bord Windows Essentials pour introduire un réseau virtuel Azure ainsi que fournir un lien rapide pour lancer l’intégration.

![Une capture d’écran montrant l’onglet mise en route sur la page d’accueil du tableau de bord WindowsServerEssentials. Sous l’onglet mise en route, la section Services a été sélectionnée, et le tableau de bord indique sous intégration à MicrosoftCloud Services que le réseau virtuel Azure est actuellement désactivé.](media/azure-virtual-network-1.PNG)

Lorsque vous cliquez sur le **intégrer maintenant** lier la mise en réseau virtuel Azure dans la capture d’écran ci-dessus, une boîte de dialogue s’affiche vous demandant de se connecter à votre compte MicrosoftAzure. Si vous ne disposez pas d’un compte MicrosoftAzure, vous aurez la possibilité pour vous inscrire pour un sur cet écran, ce qui vous redirige vers le portail d’inscription de compte Azure:

![Une capture d’écran montrant le journal de page dans pour MicrosoftAzure de l’Assistant Intégration avec Azure virtuel du réseau.](media/azure-virtual-network-2.PNG)

Une fois que la signature dans Azure, s’affiche avec l’option choisir abonnement qu’ils souhaitent à associer à l’ordinateur virtuel Azure service de mise en réseau:

![Une capture d’écran montrant la page de mon abonnement Azure de Microsoft de l’intégration avec l’Assistant réseau virtuel Azure.](media/azure-virtual-network-3.PNG)

Une fois, vous avez choisi l’abonnement Azure que vous souhaitez utiliser pour un réseau virtuel Azure, s’affiche avec l’option pour créer un nouveau réseau virtuel Azure, ou si un a déjà été configuré sous cet abonnement, affiche la zone de liste déroulante qu'il est disponible. Vous serez également choisir un nom pour le réseau Local par le réseau virtuel Azure pour identifier les ressources de votre réseau local. Enfin, vous allez choisir quelle région Azure dans laquelle vous souhaitez que leur réseau virtuel Azure pour être hébergée. Choix d’un emplacement qui est physiquement plus proche de chez votre réseau local est généralement préférable d’optimisation de la vitesse de la bande passante pour communiquer avec les ressources que vous pouvez héberger dans leurs services Azure:

![Une capture d’écran montrant la page réseau défini d’Azure virtuel de l’Assistant Intégration avec Azure virtuel du réseau.](media/azure-virtual-network-4.PNG)

La dernière étape du processus d’intégration consiste à configurer l’appareil VPN qui sera utilisé pour la connexion VPN S2S. Dans la mesure où la plupart des petites entreprises ont uniquement quelques serveurs dans leur environnement et ne possèdent pas le personnel informatique pour configurer correctement un routeur VPN pour se connecter à MicrosoftAzure, la sélection par défaut sera pour configurer le serveur WindowsServerEssentials en tant que serveur VPN en ressources de votre réseau local se connectera afin d’accéder aux ressources dans le réseau virtuel Azure. Toutefois, si vous préférez utiliser un autre serveur dans votre environnement en tant que serveur VPN, ou si vous préférez utiliser un routeur VPN, vous pouvez sélectionner ces options.

En raison de la variante dans les modèles et types de routeurs, WindowsServerEssentials n’essaie pas automatiquement configurer le routeur de VPN. En sélectionnant le routeur VPN dans cet Assistant Intégration signale uniquement la mise en réseau virtuel Azure du type de périphérique pour les configurations de routage appropriés nécessaires dans Azure pour la connectivité.

À la fin de l’Assistant d’intégration, un nouvel onglet sera visible dans le tableau de bord WindowsServerEssentials pour un réseau virtuel Azure:

![Une capture d’écran montrant la page de réseau virtuel Azure du tableau de bord WindowsServerEssentials. L’onglet de réseau virtuel Azure est sélectionné et affiche l’état de configuration.](media/azure-virtual-network-5.PNG)

>! Remarque terminé la configuration d’un réseau virtuel Azure dans le cloud peut prendre un certain temps, vers le haut à 30minutes. Pendant ce temps, l’état de configuration sera présent dans la page État du réseau virtuel Azure du tableau de bord.

Une fois la configuration du réseau virtuel Azure est terminée, l’état est placez-vous connecté et afficher les détails de votre réseau virtuel Azure comme données d’entrée/sortie, adresse IP de passerelle, locales détails de compte et d’adresse IP:

![Une capture d’écran montrant la page de réseau virtuel Azure du tableau de bord WindowsServerEssentials. L’onglet de réseau virtuel Azure est sélectionné et affiche l’état connecté, et sous les informations d’état sont affichés les détails du réseau virtuel.](media/azure-virtual-network-6.PNG)

Dans le volet des tâches sur le côté droit du tableau de bord sont les différentes tâches que vous pouvez prendre avec votre réseau virtuel Azure.

-   **Se déconnecter d’Azure réseau virtuel** la configuration d’un réseau virtuel Azure est disponible, mais il existe une charge de la passerelle VPN qui se connecte à locaux et d’autres VNETs dans Azure. Déconnecter le réseau virtuel Azure arrête tous les facturation.

-   **Basculer sur un périphérique VPN** dans le cas où vous souhaitez modifier à partir d’un serveur VPN à un routeur VPN, cette tâche pour vous permettre de rendre le commutateur et d’avertir le réseau virtuel Azure.

-   **Configurer le réseau virtuel Azure** cette tâche permet de modifier les options de configuration avancée du réseau virtuel Azure en redirigeant les à la page configuration du portail Azure pour le réseau virtuel Azure.

-   **Actualiser l’état** des actualisations de la page d’état, l’état de connexion du réseau virtuel Azure, y compris les données d’entrée/sortie de mise à jour.

-   **Désactiver l’intégration de réseau virtuel Azure** se déconnecte du réseau virtuel Azure et supprime l’intégration à partir du tableau de bord WindowsServerEssentials. Remarque Cela ne supprime pas le réseau virtuel Azure, les paramètres sont toujours conservés dans Azure si vous souhaitez intégrer nouveau réseau virtuel Azure avec le tableau de bord.

-   **En savoir plus sur Azure réseau virtuel**[https://azure.microsoft.com/en-us/services/virtual-network/](https://azure.microsoft.com/en-us/services/virtual-network/).

<a name="see-also"></a>Voir aussi
--------
[Prise en main WindowsServerEssentials](get-started.md)
