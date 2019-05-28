---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Mise à niveau vers ADFS dans Windows Server2016
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c8e72f1075b984506f9f992cd45cf853b50bddeb
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191921"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Mise à niveau vers AD FS dans Windows Server 2016 à l'aide d'une base de données WID



## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>La mise à niveau un serveur Windows Server 2012 R2 ou 2016 batterie de serveurs ADFS pour Windows Server 2019
Le document suivant décrit comment mettre à niveau votre batterie AD FS à AD FS dans Windows Server 2019 lorsque vous utilisez une base de données WID.  

### <a name="ad-fs-farm-behavior-levels-fbl"></a>Niveaux de comportement AD FS de batterie de serveurs (FBL)  
Dans AD FS pour Windows Server 2016, le niveau de comportement de la batterie (FBL) a été introduit. Il s’agit de batterie de serveurs à l’échelle du paramètre qui détermine que la batterie de serveurs fonctionnalités AD FS peut utiliser.

Le tableau suivant répertorie les valeurs FBL par version de Windows Server :
| Version de Windows Server  | FBL | AD FS Configuration Database Name |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]  
> La mise à niveau le FBL crée une nouvelle base de données de configuration AD FS.  Consultez le tableau ci-dessus pour les noms de la base de données de configuration pour chaque version de Windows Server AD FS et de la valeur FBL

### <a name="new-vs-upgraded-farms"></a>Nouveau vs mis à niveau les batteries de serveurs
Par défaut, le FBL dans une nouvelle batterie AD FS correspond à la valeur pour la version de Windows Server du premier nœud de batterie de serveurs.  

Un serveur AD FS d’une version ultérieure peut être jointe à une batterie de serveurs AD FS 2012 R2 ou 2016, et la batterie de serveurs fonctionnera à la même FBL en tant que les nœuds existants. Lorsque vous avez plusieurs versions de Windows Server dans la même batterie de serveurs à la valeur FBL de la version la plus faible, votre batterie de serveurs est dite « mixed ». Toutefois, vous ne serez pas en mesure de tirer parti des fonctionnalités des versions ultérieures jusqu'à ce que le FBL est déclenché. Avec une batterie de serveurs mixte :  

-   Les administrateurs peuvent ajouter de nouveaux serveurs de fédération Windows Server 2019 à un serveur Windows Server 2012 R2 existant ou de batterie de serveurs 2016. Par conséquent, la batterie de serveurs est en « mode mixte » et opère au même niveau de comportement de batterie de serveurs en tant que la batterie de serveurs d’origine. Pour garantir un comportement cohérent entre la batterie de serveurs, les fonctionnalités les plus récentes versions de Windows Server AD FS ne peut pas être configurées ou utilisées.  

- Avant du FBL peut être déclenché, les administrateurs doivent supprimer les nœuds AD FS de versions précédentes de Windows Server à partir de la batterie de serveurs.  Dans le cas d’une batterie de serveurs WID, notez que cela requiert une des tp de serveurs de fédération Windows Server 2019 nouveau être promu au rôle de nœud principal dans la batterie de serveurs.

-   Une fois que tous les serveurs de fédération dans la batterie de serveurs sont la même version de Windows Server, le FBL peut être déclenché.  Par conséquent, les nouvelles fonctionnalités de 2019 AD FS Windows Server peuvent ensuite être configurées et utilisées.

N’oubliez pas qu’en mode mixte de batterie de serveurs, la batterie de serveurs AD FS n’est pas capable d’aucune nouvelle fonctionnalité ou une fonctionnalité introduite dans AD FS dans Windows Server 2019. Cela signifie que les organisations qui souhaitent essayer de nouvelles fonctionnalités ne sont pas jusqu'à ce que le FBL est déclenché. Par conséquent, si votre organisation cherche à tester les nouvelles fonctionnalités avant rasing la FBL, vous devez déployer une batterie de serveurs distincte pour ce faire.  

Le reste de l’est le document décrit les étapes pour l’ajout d’un serveur de fédération Windows Server 2019 à un serveur Windows Server 2016 ou environnement 2012 R2 et puis d’augmenter la FBL à Windows Server 2019. Ces étapes ont été effectuées dans un environnement de test décrit par le diagramme architectural ci-dessous.  

> [!NOTE]  
> Avant de pouvoir déplacer vers AD FS dans Windows Server 2019 FBL, vous devez supprimer tous les Windows Server 2016 ou 2012 R2 nœuds. Vous ne pouvez pas simplement mettre à niveau d’un serveur Windows Server 2016 ou le système d’exploitation de 2012 R2 vers Windows Server 2019 et le devenir un nœud 2019. Vous devez le supprimer et le remplacer par un nouveau nœud 2019.



##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>Pour mettre à niveau votre batterie AD FS pour le niveau de comportement de batterie de serveurs Windows Server 2019  

1.  À l’aide du Gestionnaire de serveur, installez le rôle Services de fédération Active Directory sur le 2019 de serveur Windows

2.  À l’aide de l’Assistant de Configuration AD FS, de joindre le nouveau serveur Windows Server 2019 pour la batterie de serveurs AD FS existante.  

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  Sur le serveur de fédération Windows Server 2019, ouvrez Gestion AD FS. Notez que les fonctionnalités de gestion ne sont pas disponibles, car ce serveur de fédération n’est pas le serveur principal.  

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  Sur le serveur Windows Server 2019, ouvrez une fenêtre de commande PowerShell avec élévation de privilèges et exécutez l’applet de commande suivante : `Set-AdfsSyncProperties -Role PrimaryComputer`

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  Sur le serveur AD FS qui a été configuré précédemment en tant que principal, ouvrez une fenêtre de commande PowerShell avec élévation de privilèges et exécutez l’applet de commande suivante : `Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN} `

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  Ouvrez Gestion AD FS maintenant sur le serveur de fédération Windows Server 2016. Notez que maintenant toutes les fonctionnalités d’administration apparaissent, car le rôle principal a été transféré à ce serveur.  

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

7.  Si vous mettez à niveau une batterie de serveurs AD FS 2012 R2 à 2016 ou 2019, la mise à niveau de la batterie de serveurs requiert le schéma Active Directory soit au moins le niveau 85.  Pour mettre à niveau le schéma, le support d’installation avec le Windows Server 2016, ouvrez une invite de commandes et accédez au répertoire de support\adprep. Exécutez la commande suivante :  `adprep /forestprep`

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

    Une fois que l’exécution se termine `adprep/domainprep`
    >[!NOTE]
    >Avant d’exécuter l’étape suivante, assurez-vous de que Windows Server est en cours en exécutant la mise à jour Windows à partir des paramètres. Continuez ce processus jusqu’à ce qu’il n’y ait plus de mises à jour nécessaires.
    >

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

8. Maintenant disponible sur le serveur Windows Server 2016, ouvrez PowerShell et exécutez l’applet de commande suivante :
    >[!NOTE]
    > Tous les serveurs de 2012 R2 doivent être supprimés de la batterie avant d’exécuter l’étape suivante.

    `Invoke-AdfsFarmBehaviorLevelRaise`  

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

9. Lorsque vous y êtes invité, tapez O. Ceci lancera augmentation du niveau. Une fois cette opération terminée vous avez correctement déclenché la FBL.  

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

10. Maintenant, si vous accédez à gestion AD FS, vous verrez que les nouvelles fonctionnalités ont été ajoutées pour la dernière version d’AD FS

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

11. De même, vous pouvez utiliser l’applet de commande PowerShell : `Get-AdfsFarmInformation` pour vous montrer le FBL actuel.  

    ![mettre à niveau](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  

12. Pour mettre à niveau les serveurs WAP au niveau plus récent, sur chaque Proxy d’Application Web, reconfigurez le proxy d’application Web en exécutant la commande PowerShell suivante dans une fenêtre avec élévation de privilèges :  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
    Supprimer les anciens serveurs du cluster et conserver uniquement les serveurs WAP exécutent la dernière version de serveur, qui ont été reconfigurés ci-dessus, en exécutant l’applet de commande Powershell suivante.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
    Vérifiez la configuration de proxy d’application Web en exécutant la commmandlet Get-WebApplicationProxyConfiguration. Le ConnectedServersName reflète le serveur à exécuter à partir de la commande précédente.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
    Pour mettre à niveau la ConfigurationVersion des serveurs WAP, exécutez la commande Powershell suivante.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
    Cela va terminer la mise à niveau des serveurs WAP.
