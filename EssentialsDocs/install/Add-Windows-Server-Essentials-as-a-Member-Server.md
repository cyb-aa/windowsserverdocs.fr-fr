---
title: Ajouter Windows Server Essentials en tant que serveur membre
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d09dd82f-f7d2-47ce-862d-fd9869f2021c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4b87c066885ed2bf0ac6dfa29496317310b062d9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310175"
---
# <a name="add-windows-server-essentials-as-a-member-server"></a>Ajouter Windows Server Essentials en tant que serveur membre

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Cette rubrique s’applique à un serveur exécutant Windows Server 2012 R2 Standard, Windows Server 2012 R2 Datacenter ou Windows Server 2016 avec le rôle expérience Windows Server Essentials installé. Dans le reste de ce document, le rôle Expérience Windows Server Essentials sera appelé Windows Server Essentials.  
  
> [!NOTE]
>   Windows Server Essentials ne peut être déployé qu’en tant que contrôleur de domaine. Dans ce document, Windows Server Essentials n’inclut pas Windows Server Essentials.  
  
 Windows Server Essentials n'a pas besoin d'être un serveur principal dans un domaine Windows. Vous pouvez ajouter Windows Server Essentials en tant que serveur membre dans un environnement de domaine Active Directory existant et tirer parti des fonctionnalités de protection des données simple, de l'accès à distance sécurisé et d'intégration cloud offertes. De plus, Windows Server Essentials peut être déployé dans un environnement Active Directory existant sans être un contrôleur de domaine. Vous pouvez ainsi étendre le stockage ou utiliser une filiale pour l'administration et le stockage local.  
  
 Vous pouvez utiliser Windows Server Essentials dans les scénarios suivants :  
  
-   Ajouter Windows Server Essentials dans une succursale et le joindre au contrôleur de domaine situé au siège social, dans un emplacement différent, en utilisant les outils natifs. Vous pouvez activer les fonctionnalités BranchCache pour une utilisation optimale de la bande passante sur ce serveur membre.  
  
-   Ajoutez Windows Server Essentials en tant que serveur membre au sein d’un réseau Windows Server Essentials afin d’étendre le stockage sur votre réseau en ajoutant des dossiers serveur supplémentaires sur votre serveur membre.  
  
-   Ajoutez Windows Server Essentials en tant que serveur membre dans le bureau local si votre serveur principal exécutant Windows Server Essentials est hébergé dans Microsoft Azure ou hébergé par un hébergeur tiers. Avec Windows Server Essentials en tant que serveur membre de votre site local, vous pouvez optimiser l'utilisation de la bande passante.  
  
## <a name="adding-windows-server-essentials-as-a-member-server"></a>Ajout de Windows Server Essentials en tant que serveur membre  
 Pour ajouter Windows Server Essentials en tant que serveur membre à un serveur principal exécutant Windows Server 2012 R2 ou Windows Server Essentials dans un environnement de Active Directory existant, vous devez effectuer les étapes suivantes :  
  
1.  Joindre le serveur exécutant Windows Server Essentials à un groupe de travail.  
  
2.  Joindre le serveur exécutant Windows Server Essentials au domaine d’un serveur Windows Server Essentials principal.  
  
3.  Configurez l’expérience Windows Server Essentials à partir de Gestionnaire de serveur.  
  
#### <a name="to-join-windows-server-essentials-to-a-workgroup-or-domain"></a>Pour joindre Windows Server Essentials à un domaine ou groupe de travail  
  
1. Après avoir terminé l'installation de Windows Server Essentials sur votre second serveur, fermez l'Assistant Configuration de Windows Server Essentials.  
  
2. Dans la zone **Rechercher**, tapez **System Settings** et dans les résultats de la recherche, cliquez sur **Afficher les paramètres système avancés**.  
  
3. Dans **Propriétés système**, cliquez sur l'onglet **Nom de l'ordinateur**.  
  
4. Sous **Nom de l'ordinateur**, dans la section **Domaine**, cliquez sur **Modifier**.  
  
5. Dans **modification du nom ou du domaine**de l’ordinateur, dans la section **membre** , choisissez si vous voulez joindre le serveur exécutant Windows Server Essentials à un **groupe de travail** ou à un **domaine**.  
  
   -   Pour ajouter le serveur à un groupe de travail, tapez **workgroup**, puis cliquez sur **OK**.  
  
   -   Pour joindre ce serveur à un domaine Active Directory existant, tapez le nom du domaine, puis cliquez sur **OK**.  
  
6. Redémarrez le serveur pour appliquer les modifications.  
  
   Une fois que vous avez joint le serveur au domaine de votre serveur principal, vous pouvez continuer à configurer Windows Server Essentials en exécutant l’Assistant Configuration de Windows Server Essentials à partir de Gestionnaire de serveur.  
  
#### <a name="to-configure-windows-server-essentials-experience-on-a-member-server"></a>Pour configurer l'Expérience Windows Server Essentials sur un serveur membre  
  
1.  (Facultatif) Modifiez le nom du serveur si nécessaire.  
  
    > [!IMPORTANT]
    >  Vous ne pouvez pas modifier le nom du serveur après avoir configuré l’expérience Windows Server Essentials.  
  
2.  Connectez-vous au serveur à l'aide de votre compte d'administrateur de domaine.  
  
3.  Ouvrez Gestionnaire de serveur.  
  
4.  Dans la zone de notification d'indicateur dans **Gestionnaire de serveur**, cliquez sur l'indicateur, puis sur **Configurer Windows Server Essentials**.  
  
5.  Choisissez de configurer le serveur comme serveur membre, puis cliquez sur **Suivant**.  
  
6.  Cliquez sur **Configurer** pour commencer la configuration. Le processus de configuration prend environ 10 minutes.  
  
7.  Sur le Bureau, cliquez sur l'icône du tableau de bord pour démarrer le tableau de bord du serveur. Dans la page d'accueil, suivez les tâches de **prise en main** qui sont répertoriées sous l'onglet **Installation**.  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Installer Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [Installer Windows Server Essentials](../install/Install-Windows-Server-Essentials.md)

