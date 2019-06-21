---
title: Mettre à niveau des contrôleurs de domaine vers Windows Server 2016
description: Ce document décrit comment mettre à niveau à partir de Windows Server 2012 R2 vers Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 572f923c33739b854808372a826e9c9bbc6aaca3
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280852"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Mettre à niveau des contrôleurs de domaine vers Windows Server 2016

S'applique à : Windows Server 2016

Cette rubrique fournit des informations générales sur les Services de domaine Active Directory dans Windows Server 2016 et décrit le processus de mise à niveau des contrôleurs de domaine à partir de Windows Server 2012 ou Windows Server 2012 R2. 

## <a name="pre-requisites"></a>Conditions préalables
La méthode recommandée pour mettre à niveau un domaine consiste à promouvoir des contrôleurs de domaine qui exécutent des versions plus récentes de Windows Server et rétrograder les contrôleurs de domaine plus anciens en fonction des besoins. Cette méthode est préférable à la mise à niveau du système d’exploitation d’un contrôleur de domaine existant. Cette liste couvre les étapes générales à suivre avant de promouvoir un contrôleur de domaine qui exécute une version plus récente de Windows Server : 

1. Vérifiez que le serveur cible répond à la configuration requise. 
2. Vérifiez la compatibilité des applications. 
3. Passez en revue les recommandations pour le passage à Windows Server 2016 
4. Vérifiez les paramètres de sécurité Pour plus d’informations, consultez [fonctionnalités déconseillées et modifications de comportement associées aux services AD DS dans Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/deprecated-features). 
5. Vérifiez la connectivité au serveur cible à partir de l’ordinateur sur lequel vous envisagez d’exécuter l’installation. 
6. Vérifiez la disponibilité des rôles de maître d’opérations nécessaires : 
   - Pour installer le premier contrôleur de domaine qui exécute Windows Server 2016 dans une forêt et un domaine existant, l’ordinateur où vous exécutez l’installation nécessite une connectivité vers le **contrôleur de schéma** afin d’exécuter adprep /forestprep et au maître d’infrastructure Pour exécuter la commande adprep /domainprep. 
   - Pour installer le premier contrôleur de domaine dans un domaine où le schéma de la forêt est déjà étendu, seule une connectivité au maître d’infrastructure est requise. 
   - Pour installer ou supprimer un domaine dans une forêt existante, vous avez besoin de connectivité à la **maître d’attribution de noms de domaine**. 
   - Installation de n’importe quel contrôleur de domaine requiert également une connectivité le **maître RID.** 
   - Si vous installez le premier contrôleur de domaine en lecture seule dans une forêt existante, vous avez besoin d’une connectivité au maître d’infrastructure pour chaque partition d’annuaire d’applications, également désignée sous le nom de contexte de nommage (autre que celui d’un domaine). 

### <a name="installation-steps-and-required-administrative-levels"></a>Étapes d’installation et les niveaux d’administration requis
Le tableau suivant fournit un résumé de la procédure de mise à niveau et les spécifications d’autorisation d’effectuer ces étapes

|Action d’installation|Informations d’identification requises|
| ----- | ----- |
|Installer une nouvelle forêt|Administrateur local sur le serveur cible|
|Installer un nouveau domaine dans une forêt existante|Administrateurs de l’entreprise|
|Installer un autre contrôleur de domaine dans un domaine existant|Administrateurs du domaine|
|Exécuter adprep /forestprep|Administrateurs du schéma, Administrateurs de l’entreprise et Administrateurs du domaine|
|Exécuter adprep /domainprep|Administrateurs du domaine|
|Exécuter adprep /domainprep /gpprep|Administrateurs du domaine|
|Exécuter adprep /rodcprep|Administrateurs de l’entreprise|

Pour plus d’informations sur les nouvelles fonctionnalités dans Windows Server 2016, consultez [quelles sont les nouveautés dans Windows Server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).



## <a name="supported-in-place-upgrade-paths"></a>Chemins de mise à niveau sur place pris en charge
Les contrôleurs de domaine qui exécutent des versions 64 bits de Windows Server 2012 ou Windows Server 2012 R2 peuvent être mis à niveau vers Windows Server 2016. Uniquement les mises à niveau 64 bits sont pris en charge, car Windows Server 2016 est fourni uniquement dans une version 64 bits.

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

Pour plus d’informations sur les chemins de mise à niveau pris en charge, consultez [pris en charge les chemins de mise à niveau](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep et Domainprep
Si vous effectuez une mise à niveau sur place d’un contrôleur de domaine existant pour le système d’exploitation Windows Server 2016, vous devrez exécuter adprep /forestprep et adprep /domainprep manuellement.  Adprep /forestprep doit être exécuté qu’une seule fois dans la forêt.  La commande adprep /domainprep doit être exécutée une fois dans chaque domaine dans lequel vous avez des contrôleurs de domaine que vous mettez à niveau vers Windows Server 2016.

Si vous promouvez un nouveau serveur Windows Server 2016, que vous n’avez pas besoin d’exécuter ces commandes manuellement.  Elles sont intégrées dans la commande PowerShell et expériences de gestionnaire de serveur.

Pour plus d’informations sur l’exécution d’adprep consultez [Adprep en cours d’exécution](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>Fonctionnalités et exigences de niveau fonctionnel
Windows Server 2016 nécessite un niveau fonctionnel de forêt Windows Server 2003. Autrement dit, avant de pouvoir ajouter un contrôleur de domaine qui exécute Windows Server 2016 à une forêt Active Directory existante, le niveau fonctionnel de forêt doit être Windows Server 2003 ou version ultérieure. Si la forêt contient des contrôleurs de domaine exécutant Windows Server 2003 ou version supérieure, mais que le niveau fonctionnel de la forêt est toujours Windows 2000, l’installation est également bloquée. 

Contrôleurs de domaine Windows 2000 doivent être supprimés avant d’ajouter des contrôleurs de domaine Windows Server 2016 à votre forêt. Dans ce cas, examinez le flux de travail suivant : 


1. Installez des contrôleurs de domaine qui exécutent Windows Server 2003 ou version supérieure. Ces contrôleurs de domaine peuvent être déployés sur une version d’évaluation de Windows Server. Cette étape nécessite également l’exécution d’adprep.exe pour cette version de système d’exploitation comme condition préalable. 
2.  Supprimez les contrôleurs de domaine Windows 2000. Plus précisément, rétrogradez correctement ou supprimez de force les contrôleurs de domaine Windows Server 2000 dans le domaine et utilisez Utilisateurs et ordinateurs Active Directory pour supprimer les comptes de tous les contrôleurs de domaine supprimés. 
3.  Augmentez le niveau fonctionnel de la forêt à Windows Server 2003 ou version ultérieure. 
4.  Installez des contrôleurs de domaine qui exécutent Windows Server 2016. 
5.  Supprimez les contrôleurs de domaine qui exécutent des versions précédentes de Windows Server. 

### <a name="rolling-back-functional-levels"></a>Annulation des niveaux fonctionnels

Après avoir défini le niveau fonctionnel de forêt (FFL) à une certaine valeur, vous ne pouvez pas restaurer ou diminuer le niveau fonctionnel de forêt, avec les exceptions suivantes : 

- Si vous mettez à niveau à partir de Windows Server 2012 R2 FFL, vous pouvez la réduire à Windows Server 2012 R2. 
- Si vous mettez à niveau à partir de Windows Server 2008 R2 FFL, vous pouvez la réduire à Windows Server 2008 R2.

Après avoir défini le niveau fonctionnel du domaine à une certaine valeur, vous ne pouvez pas restaurer ou diminuer le niveau fonctionnel de domaine, avec les exceptions suivantes : 

- Lorsque vous augmentez le niveau fonctionnel de domaine vers Windows Server 2016 et si le niveau fonctionnel de forêt est Windows Server 2012 ou version antérieure, vous avez la possibilité de restaurer le niveau fonctionnel du domaine revenir à Windows Server 2012 ou Windows Server 2012 R2 

Pour plus d’informations sur les fonctionnalités qui sont disponibles à des niveaux fonctionnels inférieurs, voir [Présentation des niveaux fonctionnels des services de domaine Active Directory (AD DS)](../active-directory-functional-levels.md). 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interopérabilité des services de domaine Active Directory avec d’autres rôles de serveur et systèmes d’exploitation Windows
Les services de domaine Active Directory ne sont pas pris en charge sur les systèmes d’exploitation Windows suivants : 


- Windows MultiPoint Server 
- WindowsServer2016 Essentials 

Les services de domaine Active Directory ne peuvent pas être installés sur un serveur qui exécute également les rôles de serveur ou services de rôle suivants : 

- Microsoft Hyper-V Server 2016
- Service Broker pour les connexions Bureau à distance 

## <a name="administration-of-windows-server-2016-servers"></a>Administration de serveurs Windows Server 2016
Utilisez les outils d’Administration à distance serveur pour Windows 10 pour gérer les contrôleurs de domaine et d’autres serveurs qui exécutent Windows Server 2016. Vous pouvez exécuter les outils Administration de serveur distant de Windows Server 2016 sur un ordinateur qui exécute Windows 10. 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Pas à pas pour la mise à niveau vers Windows Server 2016
Voici un exemple simple de mise à niveau de la forêt Contoso à partir de Windows Server 2012 R2 vers Windows Server 2016.

![Mettre à niveau/Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1. Joindre le nouveau Windows Server 2016 à votre forêt. Redémarrez lorsque vous y êtes invité. 
   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2. Connectez-vous à la nouvelle fonctionnalité Windows Server 2016 avec un compte d’administrateur de domaine.
3. Dans **le Gestionnaire de serveur**, sous **Ajout de rôles et fonctionnalités**, installer **Active Directory Domain Services** sur le nouveau Windows Server 2016. Adprep s’exécute automatiquement sur la forêt de R2 2012 et le domaine.
   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4. Dans **le Gestionnaire de serveur**et cliquez sur le triangle jaune dans la liste déroulante, cliquez sur **promouvoir le serveur sur un contrôleur de domaine**. 
   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5. Sur le **Configuration de déploiement** s’affiche, sélectionnez **ajouter un contrôleur de domaine à une forêt existante** et cliquez sur Suivant. 
   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6. Sur le **options du contrôleur de domaine** écran, entrez le **en Mode de restauration des Services annuaire (DSRM)** mot de passe et cliquez sur Suivant. 
7. Pour le reste des écrans, cliquez sur **suivant**. 
8. Sur le **vérification** , cliquez sur **installer**. Une fois le redémarrage terminé vous peut se reconnecter.
9. Sur le serveur Windows Server 2012 R2, dans **le Gestionnaire de serveur**, sous Outils, sélectionnez **Module Active Directory pour Windows PowerShell**. 
   ![Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. Dans windows PowerShell utilisez l’ADDirectoryServerOperationMasterRole de déplacement pour déplacer les rôles FSMO. Vous pouvez tapez le nom de chaque OperationMasterRole - ou utiliser des nombres pour spécifier les rôles. Pour plus d’informations, consultez [ADDirectoryServerOperationMasterRole de déplacement](https://technet.microsoft.com/library/hh852302.aspx)

    ``` powershell
    Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
    ```

    ![Mettre à niveau/Mise à niveau](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. Vérifiez les rôles ont été déplacés en accédant au serveur Windows Server 2016, en **le Gestionnaire de serveur**, sous **outils**, sélectionnez **Module Active Directory pour Windows PowerShell**. Utilisez le `Get-ADDomain` et `Get-ADForest` applets de commande pour afficher les détenteurs du rôle FSMO.
    ![Upgrade](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
    ![Upgrade](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. Rétrograder et supprimer le contrôleur de domaine Windows Server 2012 R2. Pour plus d’informations sur la rétrogradation d’un contrôleur de domaine, consultez [la rétrogradation des contrôleurs de domaine et de domaines](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. Une fois que le serveur est rétrogradé et supprimé, vous pouvez augmenter les niveaux fonctionnels de domaine vers Windows Server 2016 fonctionnel de forêt.


## <a name="next-steps"></a>Étapes suivantes
-   [Nouveautés relatives à l’installation et à la suppression d’Active Directory Domain Services](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [Installer les Services de domaine Active Directory &#40;niveau 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Niveaux fonctionnels de Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
