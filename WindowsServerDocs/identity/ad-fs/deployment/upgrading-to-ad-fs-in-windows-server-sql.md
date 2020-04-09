---
title: Mise à niveau vers AD FS dans Windows Server 2016 avec SQL Server
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e9488357eecb4a2093d6989e4ebfcc195ce68567
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854002"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>Mise à niveau vers AD FS dans Windows Server 2016 avec SQL Server


> [!NOTE]  
> Commencez une mise à niveau avec un intervalle de temps définitif planifié pour l’achèvement. Il n’est pas recommandé de conserver AD FS dans un État en mode mixte pendant une période prolongée, car laisser des AD FS en mode mixte peut entraîner des problèmes avec la batterie de serveurs.


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Passage d’une batterie de AD FS Windows Server 2012 R2 à une batterie de serveurs Windows Server 2016 AD FS  
Le document suivant décrit comment mettre à niveau votre AD FS batterie de serveurs Windows Server 2012 R2 vers AD FS dans Windows Server 2016 lorsque vous utilisez un SQL Server pour la base de données AD FS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Mise à niveau de AD FS vers Windows Server 2016 FBL  
Nouveauté de AD FS pour Windows Server 2016 est la fonctionnalité de niveau de comportement de la batterie de serveurs (FBL).   Cette fonctionnalité est étendue à la batterie de serveurs et détermine les fonctionnalités que la batterie de AD FS peut utiliser.   Par défaut, FBL dans une batterie de serveurs Windows Server 2012 R2 AD FS se trouve sur le FBL Windows Server 2012 R2.  

Un serveur Windows Server 2016 AD FS peut être ajouté à une batterie de serveurs Windows Server 2012 R2 et il fonctionnera sur le même FBL que Windows Server 2012 R2.  Si vous disposez d’un serveur Windows Server 2016 AD FS fonctionnant de cette manière, votre batterie de serveurs est dite « mixte ».  Toutefois, vous ne pourrez pas tirer parti des nouvelles fonctionnalités de Windows Server 2016 tant que FBL n’est pas élevé à Windows Server 2016.  Avec une batterie de serveurs mixte :  

-   Les administrateurs peuvent ajouter de nouveaux serveurs de Fédération Windows Server 2016 à une batterie de serveurs Windows Server 2012 R2 existante.  Par conséquent, la batterie de serveurs est en mode mixte et utilise le niveau de comportement de la batterie de serveurs Windows Server 2012 R2.  Pour garantir un comportement cohérent dans l’ensemble de la batterie de serveurs, les nouvelles fonctionnalités de Windows Server 2016 ne peuvent pas être configurées ou utilisées dans ce mode.  

-   Une fois que tous les serveurs de Fédération Windows Server 2012 R2 ont été supprimés de la batterie en mode mixte et, dans le cas d’une batterie de serveurs WID, l’un des nouveaux serveurs de Fédération Windows Servers 2016 a été promu au rôle de nœud principal, l’administrateur peut alors déclencher l’FBL à partir de Windows Server 2012 R2 vers Windows Server 2016.  Par conséquent, les nouvelles fonctionnalités de AD FS Windows Server 2016 peuvent ensuite être configurées et utilisées.  

-   En raison de la fonctionnalité de batterie de serveurs mixte, AD FS les organisations Windows Server 2012 R2 cherchant à effectuer une mise à niveau vers Windows Server 2016 n’auront pas besoin de déployer une batterie de serveurs entièrement nouvelle, d’exporter et d’importer des données de configuration.  Au lieu de cela, ils peuvent ajouter des nœuds Windows Server 2016 à une batterie de serveurs existante alors qu’il est en ligne et ne subir que les temps d’arrêt relativement brefs inhérents à l’augmentation de FBL.  

N’oubliez pas qu’en mode batterie mixte, la batterie de AD FS n’est pas compatible avec les nouvelles fonctionnalités ou fonctionnalités introduites dans AD FS dans Windows Server 2016.  Cela signifie que les organisations qui souhaitent essayer les nouvelles fonctionnalités ne peuvent pas effectuer cette opération tant que le FBL n’est pas généré.  Par conséquent, si votre organisation cherche à tester les nouvelles fonctionnalités avant de Rasing le FBL, vous devrez déployer une batterie de serveurs distincte pour effectuer cette opération.  

Le reste du document contient les étapes permettant d’ajouter un serveur de Fédération Windows Server 2016 à un environnement Windows Server 2012 R2, puis de déclencher l’FBL vers Windows Server 2016.  Ces étapes ont été effectuées dans un environnement de test décrit par le diagramme architectural ci-dessous.  

> [!NOTE]  
> Pour pouvoir passer à AD FS dans Windows Server 2016 FBL, vous devez supprimer tous les nœuds Windows 2012 R2.  Vous ne pouvez pas simplement mettre à niveau un système d’exploitation Windows Server 2012 R2 vers Windows Server 2016 et le faire devenir un nœud 2016.  Vous devez le supprimer et le remplacer par un nouveau nœud 2016.  

Le diagramme architectural suivant montre l’installation qui a été utilisée pour valider et enregistrer les étapes ci-dessous.

![Architecture](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Joindre le serveur Windows 2016 AD FS à la batterie de serveurs AD FS

1.  Utilisation de Gestionnaire de serveur installer le rôle Services ADFS sur Windows Server 2016  

2.  À l’aide de l’Assistant Configuration de AD FS, joignez le nouveau serveur Windows Server 2016 à la batterie de AD FS existante.  Dans l’écran d' **Accueil** , cliquez sur **suivant**.
 ](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png) de la batterie de serveurs ![Join  
3.  Dans l’écran **se connecter à Active Directory Domain Services** ,**pécifier un compte administrateur** disposant des autorisations nécessaires pour effectuer la configuration des services de Fédération, puis cliquez sur **suivant**.
4.  Dans l’écran **spécifier une batterie de serveurs** , entrez le nom de l’instance et du serveur SQL Server, puis cliquez sur **suivant**.
](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png) de la batterie de serveurs ![Join
5.  Dans l’écran **spécifier le certificat SSL** , spécifiez le certificat, puis cliquez sur **suivant**.
](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png) de la batterie de serveurs ![Join
6.  Dans l’écran **spécifier le compte de service** , spécifiez le compte de service, puis cliquez sur **suivant**.
7.  Dans l’écran **examiner les options** , passez en revue les options, puis cliquez sur **suivant**.
8.  Dans l’écran **vérifications des conditions** préalables, assurez-vous que toutes les vérifications des conditions préalables ont été satisfaites, puis cliquez sur **configurer**.
9.  Dans l’écran **résultats** , vérifiez que le serveur a été correctement configuré, puis cliquez sur **Fermer**.


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Supprimer le serveur de AD FS Windows Server 2012 R2

>[!NOTE]
>Vous n’avez pas besoin de définir le serveur de AD FS principal à l’aide de Set-AdfsSyncProperties-Role lors de l’utilisation de SQL comme base de données.  Cela est dû au fait que tous les nœuds sont considérés comme des nœuds principaux dans cette configuration.

1.  Sur le serveur Windows Server 2012 R2 AD FS dans Gestionnaire de serveur Utilisez **supprimer des rôles et des fonctionnalités** sous **gérer**.
![supprimer le serveur](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  Dans l'écran **Avant de commencer**, cliquez sur **Suivant**.
3.  Dans l’écran **sélection du serveur** , cliquez sur **suivant**.
4.  Dans l’écran **rôles de serveurs** , désactivez la case à cocher en regard de **services ADFS** , puis cliquez sur **suivant**.
![supprimer le serveur](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  Dans l’écran **fonctionnalités** , cliquez sur **suivant**.
6.  Dans l’écran **confirmation** , cliquez sur **supprimer**.
7.  Une fois cette opération terminée, redémarrez le serveur.

#### <a name="raise-the-farm-behavior-level-fbl"></a>Élever le niveau de comportement de la batterie de serveurs (FBL)
Avant cette étape, vous devez vous assurer que ForestPrep et DomainPrep ont été exécutés sur votre environnement Active Directory et que Active Directory dispose du schéma Windows Server 2016.  Ce document a démarré avec un contrôleur de domaine Windows 2016 et n’a pas besoin de l’exécuter, car ils ont été exécutés lors de l’installation d’AD.

>[!NOTE]
>Avant de commencer le processus ci-dessous, assurez-vous que Windows Server 2016 est en cours d’exécution en exécutant Windows Update à partir de paramètres.  Continuez ce processus jusqu’à ce qu’il n’y ait plus de mises à jour nécessaires.

1. Maintenant, sur le serveur Windows Server 2016, ouvrez PowerShell et exécutez la commande suivante : **$cred = acquérir-Credential** et appuyez sur entrée.
2. Entrez les informations d’identification qui possèdent des privilèges d’administrateur sur la SQL Server.
3. Maintenant, dans PowerShell, entrez ce qui suit : **Invoke-AdfsFarmBehaviorLevelRaise-Credential $cred**
2. Lorsque vous y êtes invité, tapez **o**.  Cela commencera à augmenter le niveau.  Une fois cette opération terminée, vous avez correctement déclenché le FBL.  
![terminer la mise à jour](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. Maintenant, si vous accédez à AD FS Management, vous verrez les nouveaux nœuds qui ont été ajoutés pour AD FS dans Windows Server 2016  
4. De même, vous pouvez utiliser PowerShell applet : obten-AdfsFarmInformation pour afficher le FBL actuel.  
![Terminer la mise à jour](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>Mettre à niveau la version de configuration des serveurs WAP existants
1. Sur chaque proxy d’application Web, reconfigurez le WAP en exécutant la commande PowerShell suivante dans une fenêtre avec élévation de privilèges :  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. Supprimez les anciens serveurs du cluster et conservez uniquement les serveurs WAP qui exécutent la dernière version du serveur, qui a été reconfigurée ci-dessus, en exécutant l’applet de commande PowerShell suivante.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. Vérifiez la configuration du WAP en exécutant la commmandlet WebApplicationProxyConfiguration. Le ConnectedServersName reflète le serveur exécuté à partir de la commande précédente.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. Pour mettre à niveau les ConfigurationVersion des serveurs WAP, exécutez la commande PowerShell suivante.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. Vérifiez que ConfigurationVersion a été mis à niveau avec la commande PowerShell WebApplicationProxyConfiguration.
