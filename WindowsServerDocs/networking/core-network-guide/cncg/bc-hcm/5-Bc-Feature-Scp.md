---
title: Installer la fonctionnalité BranchCache et configurer le serveur de cache hébergé par le point de connexion de service
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 987b46b1748d5a889aa69823d3492a707948a100
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318474"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Installer la fonctionnalité BranchCache et configurer le serveur de cache hébergé par le point de connexion de service

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser cette procédure pour installer la fonctionnalité BranchCache sur votre serveur de cache hébergé, HCS1 et configurer le serveur pour inscrire un point de connexion de service \(SCP\) dans Active Directory Domain Services AD DS \(\).

Lorsque vous inscrivez des serveurs de cache hébergé avec un SCP dans AD DS, le SCP permet aux ordinateurs clients configurés correctement de découvrir automatiquement les serveurs de cache hébergé en interrogeant AD DS pour le SCP. Des instructions sur la façon de configurer des ordinateurs clients pour effectuer cette action sont fournies plus loin dans ce guide.

>[!IMPORTANT]
>Avant d’effectuer cette procédure, vous devez joindre l’ordinateur au domaine et configurer l’ordinateur avec une adresse IP statique.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Pour installer la fonctionnalité BranchCache et configurer le serveur de cache hébergé  

1. Sur l’ordinateur serveur, exécutez Windows PowerShell en tant qu’administrateur. Tapez la commande suivante et appuyez sur ENTRÉE.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Pour configurer l’ordinateur en tant que serveur de cache hébergé après l’installation de la fonctionnalité BranchCache et pour inscrire un point de connexion de service dans AD DS, tapez la commande suivante dans Windows PowerShell, puis appuyez sur entrée.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Pour vérifier la configuration du serveur de cache hébergé, tapez la commande suivante et appuyez sur entrée.

    ```  
    Get-BCStatus  
    ```  
  
    Les résultats de la commande affichent l’état de tous les aspects de votre installation BranchCache. Voici quelques-uns des paramètres BranchCache et la valeur correcte pour chaque élément :  
  
    -   BranchCacheIsEnabled : true

    -   HostedCacheServerIsEnabled : true

    -   HostedCacheScpRegistrationEnabled : true

4. Pour préparer l’étape de copie de vos packages de données à partir de vos serveurs de contenu vers vos serveurs de cache hébergé, identifiez un partage existant sur le serveur de cache hébergé ou créez un nouveau dossier et partagez le dossier afin qu’il soit accessible à partir de vos serveurs de contenu. Après avoir créé vos packages de données sur vos serveurs de contenu, vous allez copier les packages de données dans ce dossier partagé sur le serveur de cache hébergé.
  
5. Si vous déployez plusieurs serveurs de cache hébergé, répétez cette procédure sur chaque serveur.

Pour continuer ce guide, consultez [déplacer et redimensionner le &#40;cache hébergé facultatif&#41;](6-Bc-Move-Resize-Cache.md).
