---
title: Ajouter WindowsServerEssentials en tant que serveur membre
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d09dd82f-f7d2-47ce-862d-fd9869f2021c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8fb73f8186d3984c9e93f7a6e39cb72a54db1e58
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>Ajouter WindowsServerEssentials en tant que serveur membre

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Cette rubrique s’applique à un serveur exécutant Windows Server2012R2 Standard, Windows Server2012R2 Datacenter ou Windows Server2016 avec le rôle expérience WindowsServerEssentials installé. Dans le reste de ce document, le rôle expérience WindowsServerEssentials sera appelé WindowsServerEssentials.  
  
> [!NOTE]
>   WindowsServerEssentials peut uniquement être déployé en tant que contrôleur de domaine. Dans ce document, WindowsServerEssentials n’inclut pas de WindowsServerEssentials.  
  
 WindowsServerEssentials n’a pas besoin être un serveur principal dans un domaine Windows. Vous pouvez ajouter WindowsServerEssentials en tant que serveur membre dans un environnement de domaine ActiveDirectory existant et tirer parti de la protection des données simple, sécuriser l’accès à distance et des fonctionnalités d’intégration cloud offertes. En outre, WindowsServerEssentials peut être déployé dans un environnement ActiveDirectory existant sans avoir à être un contrôleur de domaine. Cela vous permet d’étendre le stockage ou d’utiliser une filiale pour l’administration et de stockage local.  
  
 Vous pouvez ajouter WindowsServerEssentials dans les scénarios suivants:  
  
-   Ajouter WindowsServerEssentials dans une succursale et le joindre au contrôleur de domaine qui se trouve dans le siège social, dans un emplacement distinct en utilisant les outils natifs. Vous pouvez activer les fonctionnalités BranchCache pour l’utilisation de la bande passante optimale sur ce serveur membre.  
  
-   Ajouter WindowsServerEssentials en tant que serveur membre au sein d’un réseau WindowsServerEssentials afin d’étendre le stockage sur votre réseau en ajoutant des dossiers du serveur supplémentaires sur votre serveur membre.  
  
-   Ajouter WindowsServerEssentials en tant que serveur membre dans la filiale si votre serveur principal exécutant WindowsServerEssentials est hébergé dans MicrosoftAzure ou hébergé par un hébergeur tiers. Avec WindowsServerEssentials en tant que serveur membre à votre permet de site local office d’optimiser l’utilisation de la bande passante.  
  
## <a name="adding-windows-server-essentials-as-a-member-server"></a>Ajout de WindowsServerEssentials en tant que serveur membre  
 Pour ajouter WindowsServerEssentials en tant que serveur membre à un serveur principal exécutant Windows Server2012R2 ou WindowsServerEssentials dans un environnement ActiveDirectory existant, vous devez effectuer les étapes suivantes:  
  
1.  Joignez le serveur exécutant WindowsServerEssentials à un groupe de travail.  
  
2.  Joignez le serveur exécutant WindowsServerEssentials au domaine d’un serveur principal de WindowsServerEssentials.  
  
3.  Configurer l’expérience WindowsServerEssentials à partir du Gestionnaire de serveur.  
  
#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>Pour joindre WindowsServerEssentials à un groupe de travail ou un domaine  
  
1.  Après avoir terminé l’installation de WindowsServerEssentials sur votre second serveur, fermez l’Assistant Configurer WindowsServerEssentials.  
  
2.  Dans le **recherche**, tapez **paramètres système**et dans les résultats de recherche, cliquez sur **afficher les paramètres système avancés**.  
  
3.  Dans **propriétés système**, cliquez sur le **nom de l’ordinateur** onglet.  
  
4.  Dans **nom de l’ordinateur**, dans le **domaine**, cliquez sur **modification**.  
  
5.  Dans **modification du nom ou du domaine ordinateur**, dans le **membre**, choisissez si vous voulez joindre le serveur exécutant WindowsServerEssentials pour un **groupe de travail** ou à un **domaine**.  
  
    -   Pour ajouter le serveur à un groupe de travail, tapez **groupe de travail**, puis cliquez sur **OK**.  
  
    -   Pour joindre ce serveur à un domaine ActiveDirectory existant, tapez le nom du domaine, puis cliquez sur **OK**.  
  
6.  Redémarrez le serveur pour appliquer les modifications.  
  
 Une fois que vous avez joint le serveur à votre domaine de s du serveur principal, vous pouvez continuer à configurer WindowsServerEssentials en exécutant l’Assistant Configurer WindowsServerEssentials à partir du Gestionnaire de serveur.  
  
#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>Pour configurer l’expérience WindowsServerEssentials sur un serveur membre  
  
1.  (Facultatif) Modifier le nom du serveur, si nécessaire.  
  
    > [!IMPORTANT]
    >  Vous ne pouvez pas modifier le nom du serveur après avoir configuré l’expérience WindowsServerEssentials.  
  
2.  Connectez-vous au serveur à l’aide de votre compte d’administrateur de domaine.  
  
3.  Ouvrez le Gestionnaire de serveur.  
  
4.  Dans la zone de notification d’indicateur dans **le Gestionnaire de serveur**, cliquez sur l’indicateur, puis cliquez sur **configurer WindowsServerEssentials**.  
  
5.  Choisissez de configurer le serveur comme serveur membre, puis cliquez sur **suivant**.  
  
6.  Cliquez sur **configurer** pour commencer la configuration. Le processus de configuration prend environ 10minutes.  
  
7.  Sur le bureau, cliquez sur l’icône du tableau de bord pour démarrer le serveur du tableau de bord. Sur la page d’accueil, effectuez les **prise en main** les tâches qui sont répertoriés sur le **le programme d’installation** onglet.  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Installer WindowsServerEssentials](Install-Windows-Server-Essentials.md)

-   [Installer WindowsServerEssentials](../install/Install-Windows-Server-Essentials.md)

