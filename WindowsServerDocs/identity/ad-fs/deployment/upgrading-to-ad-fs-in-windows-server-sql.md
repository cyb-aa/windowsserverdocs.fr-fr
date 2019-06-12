---
title: La mise à niveau vers AD FS dans Windows Server 2016 avec SQL Server
description: ''
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0a3db2a095d1a31f55bd1c8bfc5bf3c9f6bb65b8
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687396"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>La mise à niveau vers AD FS dans Windows Server 2016 avec SQL Server


> [!NOTE]  
> Commencer une mise à niveau avec un intervalle de temps définitif complété. Il n’est pas recommandé de conserver les AD FS dans un état en mode mixte pendant une période prolongée, laissant AD FS dans un état en mode mixte peut entraîner des problèmes avec la batterie de serveurs.


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Déplacement d’une batterie de serveurs Windows Server 2012 R2 AD FS vers une batterie de serveurs Windows Server 2016 AD FS  
Le document suivant décrit comment mettre à niveau votre batterie de serveurs AD FS Windows Server 2012 R2 vers AD FS dans Windows Server 2016, lorsque vous utilisez un serveur SQL Server pour la base de données AD FS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Mise à niveau d’AD FS vers Windows Server 2016 FBL  
Nouveautés dans AD FS pour Windows Server 2016 est la fonctionnalité de niveau de comportement de batterie de serveurs (FBL).   Cette fonctionnalité est l’échelle de la batterie et détermine les fonctionnalités qui permet de la batterie de serveurs AD FS.   Par défaut, le FBL dans une batterie de serveurs Windows Server 2012 R2 AD FS est à le FBL Windows Server 2012 R2.  

Un serveur Windows Server 2016 AD FS peut être ajouté à une batterie de serveurs Windows Server 2012 R2 et il fonctionnera à la même FBL comme un serveur Windows Server 2012 R2.  Lorsque vous avez un serveur Windows Server 2016 AD FS fonctionne de cette manière, votre batterie de serveurs est dite « mixed ».  Toutefois, vous ne serez pas en mesure de tirer parti des nouvelles fonctionnalités de Windows Server 2016 jusqu'à ce que le FBL est déclenché pour Windows Server 2016.  Avec une batterie de serveurs mixte :  

-   Les administrateurs peuvent ajouter de nouveaux serveurs de fédération Windows Server 2016 à une batterie existante de Windows Server 2012 R2.  Par conséquent, la batterie de serveurs est en « mode mixte » et opère le niveau de comportement de batterie de serveurs Windows Server 2012 R2.  Pour garantir un comportement cohérent entre la batterie de serveurs, les nouvelles fonctionnalités de Windows Server 2016 ne peut pas être configurées ou utilisées dans ce mode.  

-   Une fois que tous les serveurs de fédération Windows Server 2012 R2 ont été supprimés de la batterie de serveurs en mode mixte, et dans le cas d’une batterie de serveurs WID, un des nouveaux serveurs de fédération Windows Server 2016 a été promu au rôle de nœud principal, l’administrateur peut déclencher ensuite le FBL à partir de Win avec Windows Server 2012 R2 vers Windows Server 2016.  Par conséquent, les nouvelles fonctionnalités de 2016 AD FS Windows Server peuvent ensuite être configurées et utilisées.  

-   Par conséquent, de la fonctionnalité de batterie de serveurs mixte, AD FS Windows Server 2012 R2, les organisations qui souhaitent mettre à niveau vers Windows Server 2016 ne devront pas déployer une batterie de serveurs entièrement nouvelle, exporter et importer des données de configuration.  Au lieu de cela, ils peuvent ajouter des nœuds de Windows Server 2016 à une batterie existante alors qu’il est en ligne et le temps d’arrêt relativement brève impliqués dans raise FBL n’occasionnent que des.  

N’oubliez pas qu’en mode mixte de batterie de serveurs, la batterie de serveurs AD FS n’est pas capable d’aucune nouvelle fonctionnalité ou une fonctionnalité introduite dans AD FS dans Windows Server 2016.  Cela signifie que les organisations qui souhaitent essayer de nouvelles fonctionnalités ne sont pas jusqu'à ce que le FBL est déclenché.  Par conséquent, si votre organisation cherche à tester les nouvelles fonctionnalités avant rasing la FBL, vous devez déployer une batterie de serveurs distincte pour ce faire.  

Le reste de l’est le document décrit les étapes pour ajouter un serveur de fédération Windows Server 2016 dans un environnement Windows Server 2012 R2 et puis d’augmenter la FBL vers Windows Server 2016.  Ces étapes ont été effectuées dans un environnement de test décrit par le diagramme architectural ci-dessous.  

> [!NOTE]  
> Avant de pouvoir déplacer vers AD FS dans Windows Server 2016 FBL, vous devez supprimer tous les nœuds Windows 2012 R2.  Vous ne pouvez pas simplement mettre à niveau un système d’exploitation de Windows Server 2012 R2 vers Windows Server 2016 et le devenir un nœud de 2016.  Vous devez le supprimer et le remplacer par un nouveau nœud de 2016.  

Diagramme architectural suivant explique le programme d’installation a été utilisé pour valider et enregistrer les étapes ci-dessous.

![Architecture](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Joindre le serveur Windows 2016 AD FS à la batterie de serveurs AD FS

1.  À l’aide du Gestionnaire de serveur, installez le rôle Services de fédération Active Directory sur Windows Server 2016  

2.  À l’aide de l’Assistant de Configuration AD FS, de joindre le nouveau serveur Windows Server 2016 à la batterie de serveurs AD FS existante.  Sur le **Bienvenue** écran, cliquez sur **suivant**.
 ![Rejoindre la batterie de serveurs](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  Sur le **se connecter aux Services de domaine Active Directory** écran, s**pécifiez un compte d’administrateur** avec des autorisations pour effectuer la configuration de services de fédération, puis cliquez sur **suivant**.
4.  Sur le **spécifier la batterie de serveurs** écran, entrez le nom de SQL server et de l’instance, puis sur **suivant**.
![Rejoindre la batterie de serveurs](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  Sur le **spécifier le certificat SSL** écran, spécifiez le certificat, puis cliquez sur **suivant**.
![Rejoindre la batterie de serveurs](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  Sur le **spécifier un compte de Service** écran, spécifiez le compte de service, puis cliquez sur **suivant**.
7.  Sur le **passez en revue les Options** écran, passez en revue les options, puis cliquez sur **suivant**.
8.  Sur le **vérifie des conditions préalables** , assurez-vous que toutes les vérifications des conditions préalables ont réussi et que vous cliquez sur **configurer**.
9.  Sur le **résultats** écran, vérifiez ce serveur a été correctement configuré, puis cliquez sur **fermer**.


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Supprimer le serveur Windows Server 2012 R2 AD FS

>[!NOTE]
>Vous n’avez pas besoin de définir le serveur AD FS principal à l’aide de Set-AdfsSyncProperties-rôle lors de l’utilisation de SQL en tant que la base de données.  Il s’agit, car tous les nœuds sont considérés comme principales dans cette configuration.

1.  Sur le serveur Windows Server 2012 R2 AD FS en cours d’utilisation du Gestionnaire de serveur **supprimer des rôles et fonctionnalités** sous **gérer**.
![Supprimer le serveur](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  Dans l'écran **Avant de commencer** , cliquez sur **Suivant**.
3.  Sur le **sélection du serveur** écran, cliquez sur **suivant**.
4.  Sur le **rôles serveur** écran, décochez la case à côté **Active Directory Federation Services** et cliquez sur **suivant**.
![Supprimer le serveur](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  Sur le **fonctionnalités** écran, cliquez sur **suivant**.
6.  Sur le **Confirmation** écran, cliquez sur **supprimer**.
7.  Une fois cette opération terminée, redémarrez le serveur.

#### <a name="raise-the-farm-behavior-level-fbl"></a>Augmenter le niveau de comportement de la batterie de serveurs (FBL)
Avant cette étape, vous devez vous assurer que forestprep et domainprep ont été exécutés sur votre environnement Active Directory et qu’Active Directory a le schéma Windows Server 2016.  Ce document démarrer avec un contrôleur de domaine Windows 2016 et ne nécessitait pas en cours d’exécution ces, car ils ont été exécutés lors de l’installation AD.

>[!NOTE]
>Avant de commencer le processus ci-dessous, assurez-vous de que Windows Server 2016 est en cours en exécutant la mise à jour Windows à partir des paramètres.  Continuez ce processus jusqu’à ce qu’il n’y ait plus de mises à jour nécessaires.

1. Maintenant disponible sur le serveur Windows Server 2016, ouvrez PowerShell et exécutez la commande suivante : **$cred = Get-Credential** puis appuyez sur ENTRÉE.
2. Entrez les informations d’identification disposant des privilèges d’administrateur sur le serveur SQL Server.
3. Maintenant dans PowerShell, entrez les informations suivantes : **AdfsFarmBehaviorLevelRaise appeler-$cred des informations d’identification**
2. Lorsque vous y êtes invité, tapez **Y**.  Ceci lancera augmentation du niveau.  Une fois cette opération terminée vous avez correctement déclenché la FBL.  
![Terminer la mise à jour](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. Maintenant, si vous accédez à gestion AD FS, vous verrez les nouveaux nœuds qui ont été ajoutés pour AD FS dans Windows Server 2016  
4. De même, vous pouvez utiliser l’applet de commande PowerShell :  Get-AdfsFarmInformation pour vous montrer le FBL actuel.  
![Terminer la mise à jour](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>Mise à niveau la Version de Configuration des serveurs WAP existants
1. Sur chaque Proxy d’Application Web, reconfigurez le proxy d’application Web en exécutant la commande PowerShell suivante dans une fenêtre avec élévation de privilèges :  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. Supprimer les anciens serveurs du cluster et conserver uniquement les serveurs WAP exécutent la dernière version de serveur, qui ont été reconfigurés ci-dessus, en exécutant l’applet de commande Powershell suivante.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. Vérifiez la configuration de proxy d’application Web en exécutant la commmandlet Get-WebApplicationProxyConfiguration. Le ConnectedServersName reflète le serveur à exécuter à partir de la commande précédente.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. Pour mettre à niveau la ConfigurationVersion des serveurs WAP, exécutez la commande Powershell suivante.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. Vérifiez que la ConfigurationVersion a été mis à niveau avec la commande Powershell de Get-WebApplicationProxyConfiguration.
