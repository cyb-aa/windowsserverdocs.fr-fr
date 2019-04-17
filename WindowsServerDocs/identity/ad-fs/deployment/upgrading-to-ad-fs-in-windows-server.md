---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: "La mise à niveau vers ADFS dans Windows Server2016"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ce07398a2d624a1e9b004cd35eb9228d59dc2b5b
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2018
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Mise à niveau vers ADFS dans Windows Server2016 à l’aide d’une base de données WID

>S’applique à: Windows Server2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Déplacement d’une batterie de serveurs AD FS de Windows Server 2012 R2 vers une batterie de serveurs AD FS de Windows Server 2016  
Le document suivant décrit comment mettre à niveau votre batterie de serveurs AD FS Windows Server 2012 R2 à AD FS dans Windows Server 2016, lorsque vous utilisez une base de données WID.  

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
>
> La mise à niveau le FBL lors de l’utilisation de SQL pour stocker la configuration ADFS crée une nouvelle base de données "AdfsConfigurationV3".

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2016-farm-behavior-level"></a>Pour mettre à niveau de votre batterie AD FS à niveau de comportement de la batterie de serveurs Windows Server 2016  

1.  À l’aide du Gestionnaire de serveur, installez le rôle Services de fédération Active Directory sur Windows Server 2016  

2.  À l’aide de l’Assistant Configuration des services ADFS, de joindre le nouveau serveur Windows Server 2016 à la batterie de serveurs AD FS existante.  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  Sur le serveur de fédération Windows Server 2016, ouvrez Gestion AD FS.    Notez que rien ne s’affiche que ce serveur de fédération n’est pas le serveur principal.  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  Une fois la jointure est terminée, sur le serveur Windows Server 2016, ouvrez PowerShell et exécutez l’applet de commande suivante: Set-AdfsSyncProperties-PrimaryComputer de rôle  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  Sur le serveur Windows Server 2012 R2 AD FS d’origine, ouvrez PowerShell et exécutez l’applet de commande suivante: Set-AdfsSyncProperties-rôle SecondaryComputer - PrimaryComputerName {nom de domaine complet}  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  Sur votre Proxy d’Application Web, ouvrez PowerShell et exécutez l’applet de commande followoing: Install-WebApplicationProxy - CertificateThumbprint {SSLCert} - fsname fsname - TrustCred $trustcred  

7.  Désormais sur le serveur de fédération Windows Server 2016 ouvrir Gestion AD FS.  Notez que tous les nœuds s’affichent dans la mesure où le rôle principal a été transféré sur ce serveur.  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

8.  Avec le support d’installation Windows Server 2016, ouvrez une invite de commandes et accédez au répertoire support\adprep.  Exécutez la commande suivante: adprep /forestprep.  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

9. Une fois que terminée exécuter adprep/domainprep  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

10. Désormais, sur le serveur Windows Server 2016, ouvrez PowerShell et exécutez l’applet de commande suivant: AdfsFarmBehaviorLevelRaise appeler  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

11. Lorsque vous y êtes invité, tapez Y. Commence alors augmenter le niveau.  Une fois cette opération terminée avec succès le FBL avoir élevé.  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

> [!NOTE]  
> Si vos serveurs ADFS utilisent SQL pour la configuration, une nouvelle base de données manamgement a maintenant été créé avec le nom "AdfsConfiguraionV3". 

12. Désormais, si vous accédez à gestion AD FS, vous verrez les nouveaux nœuds qui ont été ajoutés pour AD FS dans Windows Server 2016  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

13. De même, vous pouvez utiliser l’applet de commande PowerShell: Get-AdfsFarmInformation vous montrer le FBL actuel.  

    ![mise à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  
