---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: Installer un contrôleur de domaine Windows Server 2012 répliqué dans un domaine existant (niveau 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a7fec85301e2b70fb64f35f0b6e345adde29eed0
ms.sourcegitcommit: 67833e36b8b2c6194a1426a974c5ad9c859fa4c9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68329649"
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>Installer un contrôleur de domaine Windows Server 2012 répliqué dans un domaine existant (niveau 200)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les étapes nécessaires à la mise à niveau d'une forêt ou d'un domaine existant vers Windows Server 2012 à l'aide du Gestionnaire de serveur ou de Windows PowerShell. Elle explique comment ajouter des contrôleurs de domaine qui exécutent Windows Server 2012 à un domaine existant.  
  
-   [Workflow de mise à niveau et de réplica](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Mettre à niveau et répliquer Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [Déploiement](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="BKMK_Workflow"></a>Workflow de mise à niveau et de réplica  
Le diagramme suivant illustre le processus de configuration des services de domaine Active Directory quand vous avez auparavant installé le rôle AD DS et démarré l'Assistant Configuration des services de domaine Active Directory à l'aide du Gestionnaire de serveur pour créer un contrôleur de domaine dans un domaine existant.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="BKMK_PS"></a>Mettre à niveau et répliquer Windows PowerShell  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (les arguments en **gras** sont obligatoires. Les arguments en *italique* peuvent être spécifiés à l'aide de Windows PowerShell ou de l'Assistant Configuration des services de domaine Active Directory.)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-SiteName*<br /><br />*-ADPrepCredential*<br /><br />-ApplicationPartitionsToReplicate<br /><br />*-AllowDomainControllerReinstall*<br /><br />-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-Force<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />-NoDnsOnNetwork<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />-SiteName<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-UseExistingAccount*<br /><br />*-WhatIf*|  
  
> [!NOTE]  
> L'argument **-credential** est uniquement requis si vous n'êtes pas déjà connecté en tant que membre des groupes Administrateurs de l'entreprise et Administrateurs du schéma (si vous mettez à niveau la forêt) ou du groupe Admins du domaine (si vous ajoutez un nouveau contrôleur de domaine à un domaine existant).  
  
## <a name="BKMK_Dep"></a>Déploiement  
  
### <a name="deployment-configuration"></a>Configuration du déploiement  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
Le Gestionnaire de serveur commence la promotion de chaque contrôleur de domaine par la page **Configuration de déploiement** . Dans cette page et les pages suivantes, les options disponibles et les champs requis varient selon l’opération de déploiement que vous sélectionnez.  
  
Pour mettre à niveau une forêt existante ou ajouter un contrôleur de domaine accessible en écriture à un domaine existant, cliquez sur **Ajouter un contrôleur de domaine à un domaine existant** et sur le bouton **Sélectionner** associé à **Spécifiez les informations de domaine pour cette opération**. Le cas échéant, le Gestionnaire de serveur vous invite à entrer des informations d’identification valides.  
  
La mise à niveau de la forêt nécessite des informations d'identification qui incluent des appartenances aux groupes Administrateurs de l'entreprise et Administrateurs du schéma dans Windows Server 2012. L’Assistant Configuration des services de domaine Active Directory vous avertit par la suite si vos informations d’identification actuelles ne comptent pas les autorisations ou les appartenances aux groupes appropriées.  
  
Le processus Adprep automatique est la seule différence opérationnelle entre l'ajout d'un contrôleur de domaine à un domaine Windows Server 2012 existant et un domaine dans lequel les contrôleurs exécutent une version antérieure de Windows Server.  
  
L'applet de commande ADDSDeployment de configuration du déploiement et les arguments sont les suivants :  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
Certains tests sont effectués sur chaque page et une partie d'entre eux sont répétés plus tard en tant que vérifications discrètes de la configuration requise. Par exemple, si le domaine sélectionné ne répond pas aux niveaux fonctionnels minimaux, vous n'avez pas besoin de procéder à toute la promotion jusqu'à la vérification de la configuration requise pour découvrir ce qui suit :  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
La page **Options du contrôleur de domaine** spécifie les fonctions du contrôleur de domaine pour le nouveau contrôleur de domaine. Les fonctions du contrôleur de domaine configurables sont **Serveur DNS**, **Catalogue global**et **Contrôleur de domaine en lecture seule**. Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et de catalogue global à des fins de haute disponibilité dans les environnements distribués. Le catalogue global est toujours sélectionné par défaut et le serveur DNS est sélectionné par défaut si le domaine actuel héberge déjà DNS sur ses contrôleurs de domaine, selon une requête SOA (Start-of-Authority). La page **Options du contrôleur de domaine** vous permet également de choisir le **nom de site** logique Active Directory approprié à partir de la configuration de la forêt. Par défaut, le sous-réseau le mieux adapté est sélectionné. S'il n'existe qu'un site, celui-ci est automatiquement sélectionné.  
  
> [!NOTE]  
> Si le serveur n'appartient pas à un sous-réseau Active Directory et qu'il existe plusieurs sites Active Directory, rien n'est sélectionné et le bouton **Suivant** n'apparaît que quand vous avez choisi un site dans la liste.  
  
Le **Mot de passe du mode de restauration des services d'annuaire** spécifié doit respecter la stratégie de mot de passe appliquée au serveur. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète.  
  
Les arguments ADDSDeployment **Options du contrôleur de domaine** sont les suivants :  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Le nom du site doit déjà exister s’il est fourni en tant qu’argument à **-sitename**. L'applet de commande **install-AddsDomainController** ne crée pas de site. Vous pouvez utiliser l'applet de commande **new-adreplicationsite** pour créer des sites.  
  
Le fonctionnement de l'argument **SafeModeAdministratorPassword** est spécial :  
  
-   S'ils ne sont *pas spécifiés* en tant qu'arguments, l'applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer un autre contrôleur de domaine dans le domaine treyresearch.net et être invité à entrer et à confirmer un mot de passe masqué :  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
    ```  
  
-   S'ils sont spécifiés *avec une valeur*, la valeur doit être une chaîne sécurisée. Il ne s’agit pas du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.  
  
Par exemple, vous pouvez manuellement inviter l'utilisateur à entrer un mot de passe sous forme d'une chaîne sécurisée à l'aide de l'applet de commande **Read-Host**.  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Étant donné que l’option précédente ne confirme pas le mot de passe, faites preuve de prudence, car le mot de passe n’est pas visible.  
  
Vous pouvez également fournir une chaîne sécurisée sous forme d'une variable en texte clair convertie, bien que ceci soit fortement déconseillé.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
Enfin, vous pouvez stocker le mot de passe obscurci dans un fichier, puis le réutiliser plus tard, sans que le mot de passe en texte clair ne s'affiche. Exemple :  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Il n'est pas recommandé de fournir ni de stocker un mot de passe en texte clair ou obfusqué. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe DSRM de ce contrôleur de domaine.  Toute personne ayant accès au fichier peut annuler ce mot de passe obfusqué. Munie de cette information, elle peut ouvrir une session sur un contrôleur de domaine en mode DSRM et finir par emprunter l'identité du contrôleur de domaine lui-même, en élevant ses privilèges au niveau le plus élevé d'une forêt Active Directory. Une autre procédure utilisant **System.Security.Cryptography** pour chiffrer les données du fichier texte est conseillée, mais n'est pas traitée ici. Le mieux est d'éviter totalement tout stockage de mot de passe.  
  
L'applet de commande ADDSDeployment offre une autre option permettant d'ignorer la configuration automatique des paramètres des clients DNS, des redirecteurs et des indications de racine. Vous ne pouvez pas ignorer cette option de configuration quand vous utilisez le Gestionnaire de serveur. Cet argument n'est important que si vous avez installé le rôle Serveur DNS avant de configurer le contrôleur de domaine :  
  
```  
-SkipAutoConfigureDNS  
```  
  
La page **Options du contrôleur de domaine** vous avertit que vous ne pouvez pas créer de contrôleurs de domaine en lecture seule si vos contrôleurs de domaine existants exécutent Windows Server 2003. Ce scénario est prévu et vous pouvez fermer l'avertissement.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Options DNS et informations d'identification de délégation DNS  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
La page **Options DNS** vous permet de configurer la délégation DNS si vous avez sélectionné l'option **Serveur DNS** dans la page *Options du contrôleur de domaine* et si vous pointez vers une zone où les délégations DNS sont autorisées. Il est possible que vous deviez fournir d'autres informations d'identification d'un utilisateur qui est membre du groupe **Administrateurs DNS** .  
  
Les arguments de l'applet de commande ADDSDeployment **Options DNS** sont les suivants :  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
Pour plus d’informations sur la nécessité de créer une délégation DNS, voir [Présentation de la délégation de zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Autres options  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
La page **Options supplémentaires** offre l'option de configuration permettant de nommer un contrôleur de domaine en tant que source de réplication ; vous pouvez aussi utiliser n'importe quel contrôleur de domaine comme source de réplication.  
  
Vous pouvez également choisir d’installer le contrôleur de domaine à partir d’un support sauvegardé à l’aide de l’option Installation à partir du support (IFM). La case **Installation à partir du support** fournit une option de navigation quand elle est cochée et vous devez cliquer sur **Vérifier** pour garantir que le chemin d'accès indiqué est un support valide. Le support utilisé par l'option Installation à partir du support est créé avec Sauvegarde Windows Server ou Ntdsutil.exe à partir d'un autre ordinateur Windows Server 2012 existant uniquement ; vous ne pouvez pas utiliser Windows Server 2008 R2 ni un système d'exploitation antérieur pour créer des supports pour un contrôleur de domaine Windows Server 2012. Pour plus d'informations sur les modifications apportées à l'option Installation à partir du support, voir [Annexe de l'administration simplifiée](../../ad-ds/deploy/Simplified-Administration-Appendix.md). En cas d'utilisation du support protégé avec SYSKEY, le Gestionnaire de serveur vous invite à entrer le mot de passe de l'image pendant la vérification.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
Les arguments de l'applet de commande ADDSDeployment **Options supplémentaires** sont les suivants :  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>Chemins d'accès  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
La page **Chemins d’accès** vous permet de remplacer les emplacements de dossier par défaut de la base de données AD DS, des journaux de transaction de base de données et du partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de %systemroot%.  
  
Les arguments de l'applet de commande ADDSDeployment Chemins d'accès Active Directory sont les suivants :  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Options de préparation  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
La page **Options de préparation** vous informe que la configuration des services de domaine Active Directory comprend l'extension du schéma (forestprep) et la mise à jour du domaine (domainprep).  Cette page ne s'affiche que quand la forêt et le domaine n'ont pas été préparés par une précédente installation de contrôleur de domaine Windows Server 2012 ni par une exécution manuelle d'Adprep.exe. Par exemple, l'Assistant Configuration des services de domaine Active Directory supprime cette page si vous ajoutez un nouveau contrôleur de domaine à un domaine racine de forêt Windows Server 2012 existant.  
  
L'extension du schéma et la mise à jour du domaine ne se produisent pas quand vous cliquez sur **Suivant**. Ces événements se produisent uniquement pendant la phase d'installation. Cette page vous informe simplement des événements qui vont se produire plus tard pendant l'installation.  
  
Cette page valide également que les informations d'identification de l'utilisateur actuel sont membres des groupes Administrateurs de l'entreprise et Administrateurs du schéma, car l'appartenance à ces groupes est nécessaire pour étendre le schéma ou préparer un domaine. Cliquez sur **Modifier** pour fournir des informations d'identification d'utilisateur appropriées si la page vous indique que les informations d'identification actuelles n'offrent pas des autorisations suffisantes.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
L’argument de l’applet de commande ADDSDeployment Options supplémentaires est le suivant :  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Comme avec les versions précédentes de Windows Server, la préparation du domaine automatisée pour les contrôleurs de domaine qui exécutent Windows Server 2012 n'exécute pas GPPREP. Exécutez **adprep.exe /gpprep** manuellement pour tous les domaines qui n'étaient pas auparavant préparés pour Windows Server 2003, Windows Server 2008 ni Windows Server 2008 R2. Vous ne devez exécuter GPPrep qu'une seule fois dans l'historique d'un domaine et non avec chaque mise à niveau. Adprep.exe n'exécute pas /gpprep automatiquement, car l'opération peut provoquer une nouvelle réplication de tous les fichiers et dossiers dans le dossier SYSVOL sur tous les contrôleurs de domaine.  
>   
> La commande RODCPrep automatique est exécutée quand vous promouvez le premier contrôleur de domaine en lecture seule non intermédiaire dans un domaine. Elle n'est pas exécutée quand vous promouvez le premier contrôleur de domaine Windows Server 2012 accessible en écriture. Vous pouvez aussi exécuter manuellement **adprep.exe /rodcprep** si vous envisagez de déployer des contrôleurs de domaine en lecture seule.  
  
### <a name="review-options-and-view-script"></a>Examiner les options et Afficher le script  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
La page **Examiner les options** vous permet de valider vos paramètres et de vérifier qu’ils répondent à vos exigences avant le démarrage de l’installation. Notez que vous avez encore la possibilité d’arrêter l’installation à l’aide du Gestionnaire de serveur. Cette page vous permet simplement d’examiner et de confirmer vos paramètres avant de poursuivre la configuration.  
  
La page **Examiner les options** du Gestionnaire de serveur offre également un bouton **Afficher le script** facultatif pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme d’un script Windows PowerShell unique. Vous pouvez ainsi utiliser l’interface graphique Gestionnaire de serveur sous forme d’un studio de déploiement Windows PowerShell. Utilisez l’Assistant Configuration des services de domaine Active Directory pour configurer les options, exportez la configuration, puis annulez l’Assistant.  Ce processus crée un exemple valide et correct du point de vue syntaxique pour permettre des modifications ultérieures ou une utilisation directe.  
  
Exemple :  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "root.fabrikam.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s'appuie pas sur les valeurs par défaut (car elles peuvent changer entre les futures versions de Windows ou les Service Packs). La seule exception concerne l'argument **-safemodeadministratorpassword** . Pour forcer une demande de confirmation, omettez la valeur en cas d'exécution interactive de l'applet de commande.  
>   
> Utilisez l'argument **Whatif** facultatif avec l'applet de commande **Install-ADDSDomainController** pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d'une applet de commande.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>Vérification de la configuration requise  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
La fonctionnalité **Vérification de la configuration requise** est nouvelle dans la configuration de domaine AD DS. Cette nouvelle phase valide que le domaine et la forêt sont capables de prendre en charge un nouveau contrôleur de domaine Windows Server 2012.  
  
Pendant l'installation d'un nouveau contrôleur de domaine, l'Assistant Configuration des services de domaine Active Directory du Gestionnaire de serveur appelle une série de tests modulaires sérialisés. Ces tests vous alertent avec des suggestions d'opérations de réparation. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus du contrôleur de domaine ne peut pas continuer tant que tous les tests de configuration requise ne sont pas réussis.  
  
La fonctionnalité **Vérification de la configuration requise** met également en évidence des informations pertinentes, telles que les modifications de sécurité qui affectent les systèmes d'exploitation plus anciens.  
  
Pour plus d’informations sur les vérifications spécifiques de la configuration requise, consultez la page [Prerequisite Checking](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Vous ne pouvez pas ignorer la fonctionnalité **Vérification de la configuration requise** quand vous utilisez le Gestionnaire de serveur, mais vous pouvez ignorer le processus quand vous utilisez l'applet de commande de déploiement des services AD DS avec l'argument suivant :  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft déconseille d'ignorer la vérification de la configuration requise, car cela peut aboutir à une promotion partielle du contrôleur de domaine ou à l'endommagement de la forêt AD DS.  
  
Cliquez sur **Installer** pour commencer le processus de promotion du contrôleur de domaine. Il s'agit de la dernière possibilité d'annuler l'installation. Vous ne pouvez pas annuler le processus de promotion une fois qu'il a commencé. L'ordinateur redémarre automatiquement à la fin de la promotion, quel qu'en soit le résultat. La page **Vérification de la configuration requise** affiche tous les problèmes rencontrés pendant le processus et des indications pour les résoudre.  
  
### <a name="installation"></a>Installation  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
Lorsque la page **Installation** s'affiche, la configuration du contrôleur de domaine commence et ne peut pas être arrêtée ni annulée. Les opérations détaillées s'affichent dans cette page et sont écrites dans les journaux :  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
-   %systemroot%\debug\adprep\logs  
  
-   %systemroot%\debug\netsetup.log (si le serveur est dans un groupe de travail)  
  
Pour installer une nouvelle forêt Active Directory à l'aide du module ADDSDeployment, utilisez l'applet de commande suivante :  
  
```  
Install-addsdomaincontroller  
```  
  
Consultez [Upgrade and Replica Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS) pour connaître les arguments requis et facultatifs.  
  
L'applet de commande **Install-AddsDomainController** comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d'installation avec les arguments requis minimaux **-domainname** et **-credential**. Notez comment l'opération Adprep a lieu automatiquement dans le cadre de l'ajout du premier contrôleur de domaine Windows Server 2012 à une forêt Windows Server 2003 existante :  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
Notez comment, de la même façon que le Gestionnaire de serveur, **Install-ADDSDomainController** vous rappelle que la promotion redémarre automatiquement le serveur. Pour accepter automatiquement l'invite de redémarrage, utilisez les arguments **-force** ou **-confirm:$false** avec n'importe quelle applet de commande Windows PowerShell ADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez l'argument **-norebootoncompletion**.  
  
> [!WARNING]  
> Il n'est pas conseillé de remplacer le redémarrage. Le contrôleur de domaine doit redémarrer pour fonctionner correctement.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
Pour configurer un contrôleur de domaine à distance à l’aide de Windows PowerShell, encapsulez l’applet de commande **install-addsdomaincontroller** *à l’intérieur* de l’applet de **commande Invoke-Command** . Cette opération requiert l'utilisation des accolades.  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
Exemple :  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> Pour plus d’informations sur le fonctionnement du processus Adprep et d’installation, voir [Troubleshooting Domain Controller Deployment](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
### <a name="results"></a>Résultats  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La page **Résultats** indique la réussite ou l'échec de la promotion et toute information d'administration importante. En cas de réussite, le contrôleur de domaine redémarre automatiquement après 10 secondes.  
  
Comme avec les versions précédentes de Windows Server, la préparation du domaine automatisée pour les contrôleurs de domaine qui exécutent Windows Server 2012 n'exécute pas GPPREP. Exécutez **adprep.exe /gpprep** manuellement pour tous les domaines qui n'étaient pas auparavant préparés pour Windows Server 2003, Windows Server 2008 ni Windows Server 2008 R2. Vous ne devez exécuter GPPrep qu'une seule fois dans l'historique d'un domaine et non avec chaque mise à niveau. Adprep.exe n'exécute pas /gpprep automatiquement, car l'opération peut provoquer une nouvelle réplication de tous les fichiers et dossiers dans le dossier SYSVOL sur tous les contrôleurs de domaine.  
  

