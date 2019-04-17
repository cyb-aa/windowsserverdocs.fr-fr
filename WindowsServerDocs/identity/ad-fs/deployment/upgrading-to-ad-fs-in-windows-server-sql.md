---
title: "La mise à niveau vers ADFS dans Windows Server 2016 avec SQL Server"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 034d4f1f8d81cf105ba94bff34b180555702acde
ms.sourcegitcommit: 33c1f4965cd2eed7d384a096cfa5c883467b16a4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2017
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>La mise à niveau vers ADFS dans Windows Server 2016 avec SQL Server

>S’applique à: Windows Server2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Déplacement d’une batterie de serveurs AD FS de Windows Server 2012 R2 vers une batterie de serveurs AD FS de Windows Server 2016  
Le document suivant décrit comment mettre à niveau votre batterie de serveurs AD FS Windows Server 2012 R2 à AD FS dans Windows Server 2016, lorsque vous utilisez un serveur SQL Server pour la base de données AD FS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Mise à niveau des services AD FS vers Windows Server 2016 FBL  
Nouveau dans AD FS pour Windows Server 2016 est la fonctionnalité de niveau de comportement de la batterie de serveurs (FBL).   Cette fonctionnalité est l’échelle de la batterie et détermine les fonctionnalités qui permet de la batterie AD FS.   Par défaut, le FBL dans une batterie de serveurs AD FS de Windows Server 2012 R2 est à le FBL Windows Server 2012 R2.  

Un serveur Windows Server 2016 AD FS peut être ajouté à une batterie de serveurs Windows Server 2012 R2 et il fonctionnera à la même FBL comme un composant Windows Server 2012 R2.  Lorsque vous disposez d’un serveur AD FS de Windows Server 2016 de cette manière, votre batterie de serveurs est dit «mixte».  Toutefois, vous ne serez pas en mesure de tirer parti des nouvelles fonctionnalités de Windows Server 2016 jusqu'à ce que le FBL est déclenché pour Windows Server 2016.  Avec une batterie de serveurs mixte:  

-   Les administrateurs peuvent ajouter de nouveau, Windows Server 2016 les serveurs de fédération à une batterie de serveurs Windows Server 2012 R2 existant.  Par conséquent, la batterie est en «mode mixte» et fonctionne le niveau de comportement de la batterie de serveurs Windows Server 2012 R2.  Pour garantir un comportement cohérent entre la batterie de serveurs, les nouvelles fonctionnalités de Windows Server 2016 ne peut pas être configurées ou utilisées dans ce mode.  

-   Une fois tous les serveurs de fédération Windows Server 2012 R2 ont été supprimés de la batterie de serveurs en mode mixte, et dans le cas d’une batterie WID, un des nouveaux serveurs de fédération Windows Server 2016 a été promu au rôle de nœud principal, l’administrateur peut augmenter puis le FBL à partir de Windows Server 2012 R2 vers Windows Server 2016.  Par conséquent, toutes les nouvelles fonctionnalités de 2016 AD FS Windows Server peuvent être configurées et utilisées.  

-   Par conséquent, de la fonctionnalité de la batterie de serveurs mixte, AD FS Windows Server 2012 R2 organisations cherchant à mettre à niveau vers Windows Server 2016 ne seront pas nécessaire de déployer une batterie de serveurs entièrement nouveau, exporter et importer des données de configuration.  Au lieu de cela, ils peuvent ajouter des nœuds de Windows Server 2016 à une batterie existante alors qu’elle est en ligne et occasionner uniquement le temps d’arrêt relativement courte impliqués dans l’augmentation FBL.  

N’oubliez pas qu’en mode mixte de batterie de serveurs, la batterie AD FS n’est pas capable de toutes les nouvelles fonctionnalités ou une fonctionnalité introduite dans AD FS dans Windows Server 2016.  Cela signifie que les organisations qui souhaitent tester les nouvelles fonctionnalités ne sont pas jusqu'à ce que le FBL est déclenché.  Par conséquent, si votre organisation recherche pour tester les nouvelles fonctionnalités avant rasing le FBL, vous devez déployer une batterie de serveurs distincte pour ce faire.  

Le reste de l’est document fournit les étapes pour ajouter un serveur de fédération Windows Server 2016 dans un environnement Windows Server 2012 R2 et de puis déclencher le FBL à Windows Server 2016.  Ces étapes ont été effectuées dans un environnement de test indiqué par le schéma ci-dessous.  

> [!NOTE]  
> Avant de pouvoir déplacer à AD FS dans Windows Server 2016 FBL, vous devez supprimer tous les nœuds Windows 2012 R2.  Vous ne pouvez pas simplement mettre à niveau d’un système d’exploitation de Windows Server 2012 R2 vers Windows Server 2016 et devenir un nœud 2016.  Vous devez le supprimer et le remplacer par un nouveau nœud 2016.  

Diagramme architectural suivant indique le programme d’installation qui a été utilisé pour valider et enregistrer les étapes ci-dessous.

![Architecture](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png) 


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Joindre le serveur Windows 2016 AD FS à la batterie AD FS

1.  À l’aide du Gestionnaire de serveur, installez le rôle Services de fédération Active Directory sur Windows Server 2016  

2.  À l’aide de l’Assistant Configuration des services ADFS, de joindre le nouveau serveur Windows Server 2016 à la batterie de serveurs AD FS existante.  Sur le **Bienvenue** écran, cliquez sur **suivant**.
 ![Rejoindre la batterie de serveurs](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  Sur le **se connecter aux Services de domaine Active Directory** écran, s**pécifier un compte d’administrateur** avec des autorisations pour effectuer la configuration de services de fédération, puis cliquez sur **suivant**.
4.  Sur le **spécifier la batterie de serveurs** de l’écran, entrez le nom du serveur SQL et instance, puis sur **suivant**.
![Rejoindre la batterie de serveurs](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  Sur le **spécifier le certificat SSL** écran, spécifiez le certificat, cliquez sur **suivant**.
![Rejoindre la batterie de serveurs](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  Sur le **spécifier un compte de Service** écran, spécifiez le compte de service, cliquez sur **suivant**. 
7.  Sur le **passer en revue les Options** de l’écran, passez en revue les options, cliquez sur **suivant**. 
8.  Sur le **vérifie les composants requis** écran, assurez-vous que toutes les vérifications préalables ont été satisfaites, cliquez sur **configurer**.
9.  Sur le **résultats** écran, assurez-vous que le serveur a été correctement configuré, cliquez sur **fermer**.
 
   
#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Supprimer le serveur AD FS de Windows Server 2012 R2

>[!NOTE]
>Vous n’avez pas besoin de définir le principal serveur AD FS à l’aide de Set-AdfsSyncProperties-rôle lors de l’utilisation de SQL en tant que la base de données.  Il s’agit, car tous les nœuds sont considérées comme principales dans cette configuration.

1.  Sur le serveur AD FS de Windows Server 2012 R2 en cours d’utilisation du Gestionnaire de serveur **suppression de rôles et fonctionnalités** sous **gérer**. 
![Supprimer le serveur](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  Sur le **avant de commencer** , cliquez sur **suivant**.
3.  Sur le **sélection du serveur** écran, cliquez sur **suivant**.
4.  Sur le **des rôles de serveurs** de l’écran, décochez la case à côté **Active Directory Federation Services** et cliquez sur **suivant**.
![Supprimer le serveur](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  Sur le **fonctionnalités** écran, cliquez sur **suivant**.
6.  Sur le **Confirmation** écran, cliquez sur **supprimer**.
7.  Une fois cette opération terminée, redémarrez le serveur.
     
#### <a name="raise-the-farm-behavior-level-fbl"></a>Augmenter le niveau de comportement de la batterie de serveurs (FBL)
Avant cette étape, vous devez vous assurer que les commandes forestprep et domainprep ont été exécutés sur votre environnement Active Directory et qu’Active Directory a le schéma Windows Server 2016.  Ce document a démarré avec un contrôleur de domaine Windows 2016 et n’exigeait pas en cours d’exécution ces car ils ont été exécutées lors de l’installation d’Active Directory.

1. Sur le serveur Windows Server2016, ouvrez PowerShell et exécutez la commande suivante: **$cred = Get-Credential** et appuyez sur entrer.
2. Entrez les informations d’identification qui ont des privilèges d’administrateur sur le serveur SQL Server.
3. Désormais, dans PowerShell, entrez les informations suivantes: **Invoke-AdfsFarmBehaviorLevelRaise-$cred des informations d’identification**
2. Lorsque vous y êtes invité, tapez **Y**. Commence alors augmenter le niveau.  Une fois cette opération terminée avec succès le FBL avoir élevé.  
![Terminer la mise à jour](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. Désormais, si vous accédez à gestion AD FS, vous verrez les nouveaux nœuds qui ont été ajoutés pour AD FS dans Windows Server 2016  
4. De même, vous pouvez utiliser l’applet de commande PowerShell: Get-AdfsFarmInformation vous montrer le FBL actuel.  
![Terminer la mise à jour](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)
