---
title: Installer la fonctionnalité BranchCache et configurer le serveur de Cache hébergé par Point de connexion de Service
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server2016 et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 854ff9f80a2221a857fab4e6ea7f7c8e6d51f1ef
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Installer la fonctionnalité BranchCache et configurer le serveur de Cache hébergé par Point de connexion de Service

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser cette procédure pour installer la fonctionnalité BranchCache sur votre serveur de cache hébergé, HCS1 et pour configurer le serveur pour enregistrer un Point de connexion de Service \(SCP\) dans les Services de domaine ActiveDirectory \(ADDS\).

Lorsque vous inscrivez des serveurs de cache hébergé avec un CSP dans ADDS, le SCP permet aux ordinateurs clients qui sont configurées correctement pour découvrir automatiquement les serveurs de cache hébergé en interrogeant les services ADDS pour le SCP. Instructions sur la façon de configurer les ordinateurs clients pour effectuer cette action sont fournies plus loin dans ce guide.

>[!IMPORTANT]
>Avant d’effectuer cette procédure, vous devez joindre l’ordinateur au domaine et configurez l’ordinateur avec une adresse IP statique.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Pour installer la fonctionnalité BranchCache et configurer le serveur de cache hébergé  

1. Sur l’ordinateur du serveur, exécutez Windows PowerShell en tant qu’administrateur. Tapez la commande suivante et appuyez sur ENTRÉE.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Pour configurer l’ordinateur comme serveur de cache hébergé, après avoir installé la fonctionnalité BranchCache, pour enregistrer un Point de connexion de Service dans ADDS, tapez la commande suivante dans Windows PowerShell et appuyez sur ENTRÉE.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Pour vérifier la configuration de serveur de cache hébergé, tapez la commande suivante et appuyez sur ENTRÉE.

    ```  
    Get-BCStatus  
    ```  
  
    Les résultats de la commande affichent l’état de tous les aspects de votre installation de BranchCache. Voici quelques-uns des paramètres BranchCache et la valeur correcte pour chaque élément:  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. Pour préparer l’étape de la copie de vos packages de données à partir de vos serveurs de contenu sur vos serveurs de cache hébergé, identifier un partage existant sur le serveur de cache hébergé ou créer un nouveau dossier et partager le dossier afin qu’il soit accessible à partir de vos serveurs de contenu. Une fois que vous créez vos packages de données sur vos serveurs de contenu, vous allez copier les packages de données vers ce dossier partagé sur le serveur de cache hébergé.
  
5. Si vous déployez plusieurs serveurs de cache hébergé, répétez cette procédure sur chaque serveur.

Pour continuer avec ce guide, consultez [déplacer et redimensionner le Cache hébergé et #40; facultatif et #41;](6-Bc-Move-Resize-Cache.md).
