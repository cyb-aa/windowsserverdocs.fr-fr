---
title: Mettre à niveau des contrôleurs de domaine vers Windows Server 2016
description: Ce document explique comment effectuer une mise à niveau de Windows Server 2012 R2 vers Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3e144a09c99d9b72d623956e868b3f573962ed94
ms.sourcegitcommit: 67833e36b8b2c6194a1426a974c5ad9c859fa4c9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68329641"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Mettre à niveau des contrôleurs de domaine vers Windows Server 2016

S'applique à : Windows Server

Cette rubrique fournit des informations générales sur Active Directory Domain Services dans Windows Server 2016 et explique le processus de mise à niveau des contrôleurs de domaine à partir de Windows Server 2012 ou Windows Server 2012 R2.

## <a name="pre-requisites"></a>Prérequis

La méthode recommandée pour mettre à niveau un domaine consiste à promouvoir les contrôleurs de domaine qui exécutent des versions plus récentes de Windows Server et à rétrograder les contrôleurs de domaine plus anciens en fonction des besoins. Cette méthode est préférable à la mise à niveau du système d’exploitation d’un contrôleur de domaine existant. Cette liste décrit les étapes générales à suivre avant de promouvoir un contrôleur de domaine qui exécute une version plus récente de Windows Server:

1. Vérifiez que le serveur cible répond à la configuration requise.
1. Vérifiez la compatibilité des applications.
1. Passer en revue les recommandations relatives à la migration vers Windows Server 2016
1. Vérifiez les paramètres de sécurité Pour plus d’informations, consultez [fonctionnalités déconseillées et modifications de comportement associées aux AD DS dans Windows Server 2016](../../../get-started/deprecated-features.md).
1. Vérifiez la connectivité au serveur cible à partir de l’ordinateur sur lequel vous envisagez d’exécuter l’installation.
1. Vérifiez la disponibilité des rôles de maître d’opérations nécessaires :
   - Pour installer le premier contrôleur de domaine exécutant Windows Server 2016 dans une forêt et un domaine existants, l’ordinateur sur lequel vous exécutez l’installation a besoin d’une connectivité au **contrôleur de schéma** afin d’exécuter adprep/forestprep et le maître d’infrastructure afin d’exécuter adprep/ ForestPrep.
   - Pour installer le premier contrôleur de domaine dans un domaine où le schéma de forêt est déjà étendu, vous avez uniquement besoin d’une connectivité au **maître d’infrastructure**.
   - Pour installer ou supprimer un domaine dans une forêt existante, vous avez besoin d’une connectivité au **maître d’attribution de noms de domaine**.
   - Toute installation de contrôleur de domaine requiert également une connectivité au **maître RID.**
   - Si vous installez le premier contrôleur de domaine en lecture seule dans une forêt existante, vous devez disposer d’une connectivité au **maître d’infrastructure** pour chaque partition d’annuaire d’applications, également appelée contexte d’attribution de noms de domaine ou nommage (autre.

### <a name="installation-steps-and-required-administrative-levels"></a>Étapes d’installation et niveaux d’administration requis

Le tableau suivant fournit un résumé des étapes de mise à niveau et des autorisations requises pour effectuer ces étapes.

|Action d’installation|Informations d’identification requises|
| ----- | ----- |
|Installer une nouvelle forêt|Administrateur local sur le serveur cible|
|Installer un nouveau domaine dans une forêt existante|Administrateurs de l’entreprise|
|Installer un autre contrôleur de domaine dans un domaine existant|Admins du domaine|
|Exécuter adprep /forestprep|Administrateurs du schéma, Administrateurs de l’entreprise et Administrateurs du domaine|
|Exécuter adprep /domainprep|Admins du domaine|
|Exécuter adprep /domainprep /gpprep|Admins du domaine|
|Exécuter adprep /rodcprep|Administrateurs de l’entreprise|

Pour plus d’informations sur les nouvelles fonctionnalités de Windows Server 2016, voir [Nouveautés de Windows server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).

## <a name="supported-in-place-upgrade-paths"></a>Chemins de mise à niveau sur place pris en charge

Les contrôleurs de domaine qui exécutent des versions 64 bits de Windows Server 2012 ou Windows Server 2012 R2 peuvent être mis à niveau vers Windows Server 2016. Seules les mises à niveau de version 64 bits sont prises en charge, car Windows Server 2016 est uniquement disponible en version 64 bits.

|Si vous exécutez cette édition:|Vous pouvez effectuer une mise à niveau vers ces éditions :|
| ----- | ----- |
|Windows Server 2012 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server2012R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|WindowsServer2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|

Pour plus d’informations sur les chemins de mise à niveau pris en charge, consultez [chemins d’accès pris en charge](../../../get-started/supported-upgrade-paths.md) .

## <a name="adprep-and-domainprep"></a>Adprep et DomainPrep

Si vous effectuez une mise à niveau sur place d’un contrôleur de domaine existant vers le système d’exploitation Windows Server 2016, vous devez exécuter adprep/forestprep et adprep/domainprep manuellement.  Adprep/forestprep doit être exécuté une seule fois dans la forêt.  Adprep/domainprep doit être exécuté une seule fois dans chaque domaine dans lequel vous avez des contrôleurs de domaine que vous mettez à niveau vers Windows Server 2016.

Si vous promouvez un nouveau serveur Windows Server 2016, vous n’avez pas besoin de les exécuter manuellement.  Celles-ci sont intégrées dans PowerShell et les expériences de Gestionnaire de serveur.

Pour plus d’informations sur l’exécution d’adprep, consultez [exécution](https://technet.microsoft.com/library/dd464018.aspx) d’adprep

## <a name="functional-level-features-and-requirements"></a>Fonctionnalités et exigences de niveau fonctionnel

Windows Server 2016 requiert un niveau fonctionnel de forêt Windows Server 2003. Autrement dit, avant de pouvoir ajouter un contrôleur de domaine qui exécute Windows Server 2016 à une forêt Active Directory existante, le niveau fonctionnel de la forêt doit être Windows Server 2003 ou une version ultérieure. Si la forêt contient des contrôleurs de domaine exécutant Windows Server 2003 ou version supérieure, mais que le niveau fonctionnel de la forêt est toujours Windows 2000, l’installation est également bloquée.

Les contrôleurs de domaine Windows 2000 doivent être supprimés avant d’ajouter des contrôleurs de domaine Windows Server 2016 à votre forêt. Dans ce cas, examinez le flux de travail suivant :

1. Installez des contrôleurs de domaine qui exécutent Windows Server 2003 ou version supérieure. Ces contrôleurs de domaine peuvent être déployés sur une version d’évaluation de Windows Server. Cette étape nécessite également l’exécution d’Adprep. exe pour cette version du système d’exploitation comme condition préalable.
1. Supprimez les contrôleurs de domaine Windows 2000. Plus précisément, rétrogradez correctement ou supprimez de force les contrôleurs de domaine Windows Server 2000 dans le domaine et utilisez Utilisateurs et ordinateurs Active Directory pour supprimer les comptes de tous les contrôleurs de domaine supprimés.
1. Augmentez le niveau fonctionnel de la forêt à Windows Server 2003 ou version ultérieure.
1. Installez les contrôleurs de domaine qui exécutent Windows Server 2016.
1. Supprimez les contrôleurs de domaine qui exécutent des versions précédentes de Windows Server.

### <a name="rolling-back-functional-levels"></a>Restauration des niveaux fonctionnels

Une fois que vous avez défini le niveau fonctionnel de la forêt (FFL) sur une valeur donnée, vous ne pouvez pas restaurer ou réduire le niveau fonctionnel de la forêt, avec les exceptions suivantes:

- Si vous effectuez une mise à niveau à partir de Windows Server 2012 R2 FFL, vous pouvez la réduire à nouveau vers Windows Server 2012 R2.
- Si vous effectuez une mise à niveau à partir de Windows Server 2008 R2 FFL, vous pouvez la réduire à nouveau vers Windows Server 2008 R2.

Une fois que vous avez défini le niveau fonctionnel du domaine à une certaine valeur, vous ne pouvez pas restaurer ou réduire le niveau fonctionnel du domaine, avec les exceptions suivantes:

- Lorsque vous augmentez le niveau fonctionnel du domaine à Windows Server 2016 et si le niveau fonctionnel de la forêt est Windows Server 2012 ou une version antérieure, vous avez la possibilité de restaurer le niveau fonctionnel du domaine sur Windows Server 2012 ou Windows Server 2012 R2.

Pour plus d’informations sur les fonctionnalités qui sont disponibles à des niveaux fonctionnels inférieurs, voir [Présentation des niveaux fonctionnels des services de domaine Active Directory (AD DS)](../active-directory-functional-levels.md).

## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interopérabilité des services de domaine Active Directory avec d’autres rôles de serveur et systèmes d’exploitation Windows

Les services de domaine Active Directory ne sont pas pris en charge sur les systèmes d’exploitation Windows suivants :

- Windows MultiPoint Server
- WindowsServer2016 Essentials

Les services de domaine Active Directory ne peuvent pas être installés sur un serveur qui exécute également les rôles de serveur ou services de rôle suivants :

- Microsoft Hyper-V Server 2016
- Service Broker pour les connexions Bureau à distance

## <a name="administration-of-windows-server-2016-servers"></a>Administration des serveurs Windows Server 2016

Utilisez la Outils d’administration de serveur distant pour Windows 10 pour gérer les contrôleurs de domaine et d’autres serveurs qui exécutent Windows Server 2016. Vous pouvez exécuter Windows Server 2016 Outils d’administration de serveur distant sur un ordinateur qui exécute Windows 10.

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Procédure pas à pas pour la mise à niveau vers Windows Server 2016

Voici un exemple simple de mise à niveau de la forêt Contoso de Windows Server 2012 R2 vers Windows Server 2016.

![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1. Rejoignez le nouveau Windows Server 2016 à votre forêt. Redémarrez quand vous y êtes invité.

   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)

1. Connectez-vous au nouveau Windows Server 2016 avec un compte d’administrateur de domaine.
1. Dans **Gestionnaire de serveur**, sous **Ajouter des rôles et des fonctionnalités**, installez **Active Directory Domain Services** sur le nouveau Windows Server 2016. Adprep s’exécute automatiquement sur la forêt et le domaine 2012 R2.

   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png)

1. Dans **Gestionnaire de serveur**, cliquez sur le triangle jaune, puis dans la liste déroulante, cliquez sur **promouvoir le serveur en contrôleur de domaine**.

   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)

1. Dans l’écran **configuration du déploiement** , sélectionnez **Ajouter un contrôleur de domaine à une forêt existante** , puis cliquez sur suivant.

   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)

1. Dans l’écran **Options du contrôleur de domaine** , entrez le mot de passe du mode de restauration des services d' **annuaire (DSRM)** , puis cliquez sur suivant.
1. Pour le reste des écrans, cliquez sur **suivant**.
1. Dans l’écran vérification de la **Configuration requise** , cliquez sur **installer**. Une fois le redémarrage terminé, vous pouvez vous reconnecter.
1. Sur le serveur Windows Server 2012 R2, dans **Gestionnaire de serveur**, sous outils, sélectionnez **Active Directory module pour Windows PowerShell**.

   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)

1. Dans les fenêtres PowerShell, utilisez Move-ADDirectoryServerOperationMasterRole pour déplacer les rôles FSMO. Vous pouvez taper le nom de chaque-OperationMasterRole ou utiliser des nombres pour spécifier les rôles. Pour plus d’informations [, consultez Move-ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

    ``` powershell
    Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
    ```

    ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)

1. Vérifiez que les rôles ont été déplacés en accédant au serveur Windows Server 2016, dans **Gestionnaire de serveur**, sous **outils**, sélectionnez **Active Directory module pour Windows PowerShell**. Utilisez les `Get-ADDomain` applets de commande et `Get-ADForest` pour afficher les détenteurs de rôle FSMO.

    ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)

    ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)

1. Rétrogradez et supprimez le contrôleur de domaine Windows Server 2012 R2. Pour plus d’informations sur la rétrogradation d’un contrôleur de domaine, consultez [rétrogradation de contrôleurs de domaine et de domaines](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
1. Une fois que le serveur est rétrogradé et supprimé, vous pouvez augmenter les niveaux fonctionnels fonctionnels et de domaine de la forêt à Windows Server 2016.

## <a name="next-steps"></a>Étapes suivantes

- [Nouveautés relatives à l’installation et à la suppression d’Active Directory Domain Services](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)
- [Installer le &#40;niveau de Active Directory Domain Services 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)
- [Niveaux fonctionnels de Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
