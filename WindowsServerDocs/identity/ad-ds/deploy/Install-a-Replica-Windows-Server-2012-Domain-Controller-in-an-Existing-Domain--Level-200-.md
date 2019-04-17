---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: "Installer un contrôleur de domaine Windows Server2012 répliqué dans un domaine existant (niveau 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e3151a8beee2870ecc747a64241df9d562ad4cc2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>Installer un contrôleur de domaine Windows Server2012 répliqué dans un domaine existant (niveau 200)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique couvre les étapes nécessaires pour mettre à niveau d’une forêt existante ou un domaine vers Windows Server2012, à l’aide du Gestionnaire de serveur ou Windows PowerShell. Elle explique comment ajouter des contrôleurs de domaine qui exécutent Windows Server2012 à un domaine existant.  
  
-   [Mise à niveau et flux de travail de réplication](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Mise à niveau et réplication dans Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [Déploiement](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="BKMK_Workflow"></a>Mise à niveau et flux de travail de réplication  
Le diagramme suivant illustre le processus de configuration des Services de domaine ActiveDirectory lorsque vous avez installé précédemment le rôle ADDS et que vous avez démarré l’Assistant ActiveDirectory domaine Services de Configuration à l’aide du Gestionnaire de serveur pour créer un nouveau contrôleur de domaine dans un domaine existant.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="BKMK_PS"></a>Mise à niveau et réplication dans Windows PowerShell  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (**gras** arguments sont requis. *En italique* arguments peuvent être spécifiés à l’aide de Windows PowerShell ou l’Assistant de Configuration ADDS.)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-SiteName*<br /><br />*-ADPrepCredential*<br /><br />ApplicationPartitionsToReplicate-<br /><br />*-AllowDomainControllerReinstall*<br /><br />-Confirmer<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-Force<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />-NoDnsOnNetwork<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*ReplicationSourceDC-*<br /><br />-SkipAutoConfigureDNS<br /><br />-SiteName<br /><br />*SystemKey-*<br /><br />*-SYSVOLPath*<br /><br />*-UseExistingAccount*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> Le **-credential** argument est uniquement requis si vous ne sont pas déjà connecté en tant que membre du groupe Admins du domaine ou les groupes Administrateurs de l’entreprise et administrateurs du schéma (si vous mettez à niveau la forêt) (si vous ajoutez un nouveau contrôleur de domaine à un domaine existant).  
  
## <a name="BKMK_Dep"></a>Déploiement  
  
### <a name="deployment-configuration"></a>Configuration de déploiement  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
Le Gestionnaire de serveur commence chaque promotion du contrôleur de domaine avec le **Configuration de déploiement** page. Les options disponibles et les champs obligatoires modifier sur cette page et les pages suivantes, en fonction de l’opération de déploiement que vous sélectionnez.  
  
Pour mettre à niveau d’une forêt existante ou ajouter un contrôleur de domaine accessible en écriture à un domaine existant, cliquez sur **ajouter un contrôleur de domaine à un domaine existant** et cliquez sur **sélectionnez** à **spécifier les informations de domaine pour ce domaine**. Le Gestionnaire de serveur vous invite aux informations d’identification valides si nécessaire.  
  
La mise à niveau de la forêt nécessite des informations d’identification qui incluent des appartenances aux groupes dans les groupes à la fois les administrateurs de l’entreprise et administrateurs du schéma dans Windows Server2012. L’Assistant Configuration des Services de domaine ActiveDirectory vous invite ultérieurement si vos informations d’identification actuelles ne disposent pas des autorisations appropriées ou les appartenances au groupe.  
  
Le processus Adprep automatique est la seule différence opérationnelle entre l’ajout d’un contrôleur de domaine à un domaine Windows Server2012 existant et un domaine où les contrôleurs de domaine exécutent une version antérieure de Windows Server.  
  
L’applet de commande ADDSDeployment de Configuration de déploiement et les arguments sont:  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
Certains tests sont effectués sur chaque page, certains d'entre eux répétez ultérieurement des vérifications de la configuration requise comme discrètes. Par exemple, si le domaine sélectionné ne répond pas aux niveaux fonctionnels minimaux, il est inutile accéder à toute la promotion à la vérification des prérequis pour savoir:  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
Le **Options du contrôleur de domaine** page spécifie les fonctions du contrôleur de domaine pour le nouveau contrôleur de domaine. Les fonctions du contrôleur de domaine configurables sont **serveur DNS**, **catalogue Global**, et **contrôleur de domaine en lecture seule**. Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et de catalogue global pour la haute disponibilité dans les environnements distribués. Dans le catalogue global est toujours sélectionné par défaut et serveur DNS est sélectionné par défaut si le domaine actuel héberge déjà DNS sur ses contrôleurs de domaine basé sur une requête de démarrage de l’autorité. Le **Options du contrôleur de domaine** page vous permet également de choisir la logique ActiveDirectory approprié **nom du site** à partir de la configuration de la forêt. Par défaut, il sélectionne le site avec le sous-réseau le mieux adapté. S’il existe un seul site, il sélectionne automatiquement.  
  
> [!NOTE]  
> Si le serveur n’appartient pas à un sous-réseau ActiveDirectory, et il existe plusieurs sites ActiveDirectory, rien n’est sélectionné et le **suivant** bouton n’est pas disponible jusqu'à ce que vous choisissez un site dans la liste.  
  
Spécifié **passe en Mode restauration des Services Directory** doit respecter la stratégie de mot de passe appliquée au serveur. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète.  
  
Le **Options du contrôleur de domaine** arguments ADDSDeployment sont:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Le nom du site doit déjà exister quand fourni en tant qu’argument à **- sitename**. Le **install-AddsDomainController** applet de commande ne crée pas de sites. Vous pouvez utiliser la cmdlet **nouveau-adreplicationsite** pour créer des sites.  
  
Le **SafeModeAdministratorPassword** de l’argument est spécial:  
  
-   Si *ne pas spécifié* en tant qu’argument, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer un contrôleur de domaine supplémentaire dans le domaine treyresearch.net et être invité à entrer et à confirmer un mot de passe masqué:  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
    ```  
  
-   Si spécifié *avec une valeur*, la valeur doit être une chaîne sécurisée. Cela n’est pas le mode d’utilisation préféré cas d’exécution interactive de l’applet de commande.  
  
Par exemple, vous pouvez demander manuellement un mot de passe à l’aide de la **Read-Host** applet de commande pour inviter l’utilisateur d’une chaîne sécurisée:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Comme l’option précédente ne confirme pas le mot de passe, faites preuve de prudence: le mot de passe n’est pas visible.  
  
Vous pouvez également fournir une chaîne sécurisée en tant qu’une variable en texte clair convertie, bien que ceci soit fortement déconseillé.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
Enfin, vous pouvez stocker le mot de passe obscurci dans un fichier et réutiliser plus tard, sans le mot de passe ne s’affiche. Par exemple:  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Fournir ou de stocker un mot de passe de texte clair ou obfusqué n’est pas recommandée. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe DSRM de ce contrôleur de domaine.  Toute personne ayant accès au fichier peut annuler ce mot de passe obfusqué. Avec cette information, ils peuvent se connecter à un contrôleur de domaine démarré en mode DSRM et finir par emprunter l’identité du contrôleur de domaine lui-même, en élevant ses privilèges au niveau plus élevé d’une forêt ActiveDirectory. Un ensemble d’étapes à l’aide supplémentaire **System.Security.Cryptography** pour chiffrer le fichier texte données sont conseillée, mais hors de portée. La meilleure pratique consiste à éviter totalement tout stockage de mot de passe.  
  
L’applet de commande ADDSDeployment offre une autre option pour ignorer la configuration automatique des paramètres de client DNS, des redirecteurs et des indications de racine. Vous ne pouvez pas ignorer cette option de configuration lorsque vous utilisez le Gestionnaire de serveur. Cet argument n’est important que si vous avez installé le rôle serveur DNS avant de configurer le contrôleur de domaine:  
  
```  
-SkipAutoConfigureDNS  
```  
  
Le **Options du contrôleur de domaine** page vous avertit que vous ne pouvez pas créer de lecture uniquement les contrôleurs de domaine si vos contrôleurs de domaine existants exécutent Windows Server2003. Ce scénario est prévu, et vous pouvez fermer l’avertissement.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Options DNS et les informations d’identification de délégation DNS  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
Le **Options DNS** page permet de configurer la délégation DNS si vous avez sélectionné le **serveur DNS** option sur le *Options du contrôleur de domaine* page et si vous pointez vers une zone où les délégations DNS sont autorisées. Vous devrez peut-être fournir d’autres informations d’identification d’un utilisateur qui est membre de la **administrateurs DNS** groupe.  
  
Le **Options DNS** sont des arguments de l’applet de commande ADDSDeployment:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
Pour plus d’informations sur la nécessité de créer une délégation DNS, voir [présentation la délégation de Zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Options supplémentaires  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
Le **Options supplémentaires** page contient l’option de configuration pour nommer un contrôleur de domaine comme source de réplication, ou vous pouvez utiliser n’importe quel contrôleur de domaine comme source de réplication.  
  
Vous pouvez également choisir d’installer le contrôleur de domaine à l’aide de support à l’aide de l’installation à partir de l’option de support (IFM) sauvegardé. Le **installer à partir du média** case à cocher fournit une option de parcourir une fois sélectionnée et vous devez cliquer sur **vérifier** pour garantir le chemin d’accès fourni est un support valide. Support utilisé par l’option IFM est créé avec sauvegarde Windows Server ou Ntdsutil.exe à partir d’un autre Windows Server2012 ordinateur existant uniquement; pour créer un média pour un contrôleur de domaine Windows Server2012, vous ne pouvez pas utiliser un Windows Server2008R2 ou un système d’exploitation précédent. Pour plus d’informations sur les modifications apportées dans IFM, voir [Simplified Administration Appendix](../../ad-ds/deploy/Simplified-Administration-Appendix.md). Si vous utilisez le média protégé avec SYSKEY, le Gestionnaire de serveur demande un mot de passe de l’image pendant la vérification.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
Le **Options supplémentaires** sont des arguments de l’applet de commande ADDSDeployment:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>Chemins d’accès  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
Le **chemins d’accès** page vous permet de remplacer les emplacements de dossier par défaut de la base de données ADDS, les journaux des transactions de base de données, et du partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de % systemroot%.  
  
Les arguments d’applet de commande ADDSDeployment de chemins d’accès ActiveDirectory sont:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Options de préparation  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
Le **Options de préparation** page vous avertit que la configuration des services ADDS inclut l’extension du schéma (forestprep) et la mise à jour le domaine (domainprep).  Cette page s’affiche uniquement lorsque la forêt et le domaine n'ont pas été préparés par une précédente installation de contrôleur de domaine Windows Server2012 ou d’exécuter manuellement Adprep.exe. Par exemple, l’Assistant Configuration des Services de domaine ActiveDirectory supprime cette page si vous ajoutez un nouveau contrôleur de domaine à un domaine racine de forêt Windows Server2012 existant.  
  
L’extension du schéma et le domaine de mise à jour ne se produisent pas lorsque vous cliquez sur **suivant**. Ces événements se produisent uniquement pendant la phase d’installation. Cette page vous informe simplement des événements qui se produira plus loin dans l’installation.  
  
Cette page valide également que les informations d’identification utilisateur actuel sont membres des groupes Administrateurs du schéma et administrateurs de l’entreprise, que vous avez besoin de l’appartenance à ces groupes pour étendre le schéma ou préparer un domaine. Cliquez sur **modification** pour fournir les informations d’identification utilisateur appropriées si la page vous informe que les informations d’identification actuelles ne fournissent pas d’autorisations suffisantes.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
L’argument d’applet de commande ADDSDeployment Options supplémentaires est:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Comme avec les versions précédentes de Windows Server, la préparation du domaine automatisée pour les contrôleurs de domaine qui exécutent Windows Server2012 n’exécute pas GPPREP. Exécutez **adprep.exe /gpprep** manuellement pour tous les domaines qui n’étaient pas auparavant préparés pour Windows Server2003, Windows Server2008 ou Windows Server2008R2. Vous devez exécuter GPPrep qu’une seule fois dans l’historique d’un domaine et non avec chaque mise à niveau. Adprep.exe n’exécute pas /gpprep automatiquement, car l’opération peut provoquer de tous les fichiers et dossiers dans le dossier SYSVOL pour la réplication de nouveau sur tous les contrôleurs de domaine.  
>   
> La commande RODCPrep automatique s’exécute quand vous promouvez le premier RODC non intermédiaire dans un domaine. Il ne se produit pas quand vous promouvez le premier contrôleur de domaine Windows Server2012 accessible en écriture. Vous pouvez aussi exécuter manuellement **adprep.exe /rodcprep** si vous prévoyez de déployer des contrôleurs de domaine en lecture seule.  
  
### <a name="review-options-and-view-script"></a>Examiner les Options et afficher le Script  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
Le **examiner les Options** page vous permet de valider vos paramètres et assurez-vous qu’elles répondent à vos exigences avant de commencer l’installation. Cela n’est pas la possibilité d’arrêter l’installation à l’aide du Gestionnaire de serveur. Cette page vous permet simplement de vérifier et confirmer vos paramètres avant de poursuivre la configuration.  
  
Le **examiner les Options** page dans le Gestionnaire de serveur offre également une option **afficher le Script** bouton pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme de script Windows PowerShell unique. Cela vous permet d’utiliser l’interface graphique du Gestionnaire de serveur comme un studio de déploiement de Windows PowerShell. Utilisez l’Assistant Configuration des Services de domaine ActiveDirectory pour configurer les options, exportez la configuration et annulez ensuite l’Assistant.  Ce processus crée un exemple valide et la syntaxe correct pour modification supplémentaire ou une utilisation directe.  
  
Par exemple:  
  
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
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s’appuie pas sur les valeurs par défaut (car elles peuvent changer entre les versions futures de Windows ou des service packs). La seule exception est le **- safemodeadministratorpassword** argument. Pour forcer une demande de confirmation omettez la valeur en cas d’exécution interactive de l’applet de commande  
>   
> Utilisez l’option **Whatif** argument avec la **Install-ADDSDomainController** applet de commande pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d’une applet de commande.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>Vérification des conditions préalables  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
Le **vérification des conditions requises** est une nouvelle fonctionnalité de configuration du domaine ADDS. Cette nouvelle phase valide que le domaine et la forêt sont capables de prendre en charge un nouveau contrôleur de domaine Windows Server2012.  
  
Lorsque vous installez un nouveau contrôleur de domaine, l’Assistant Gestionnaire de serveur ActiveDirectory domaine Services Configuration appelle une série de tests modulaires sérialisés. Ces tests vous alerte avec les options de réparation suggérés. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus de contrôleur de domaine ne peut pas continuer tant que la configuration requise de tous les tests passer.  
  
Le **vérification des conditions requises** exposent également des informations pertinentes, telles que les modifications de sécurité qui affectent les anciens systèmes d’exploitation.  
  
Pour plus d’informations sur les vérifications spécifiques de la configuration requise, voir [Prerequisite Checking](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Vous ne pouvez pas ignorer le **vérification** lorsque à l’aide du Gestionnaire de serveur, mais vous pouvez ignorer le processus lors de l’utilisation de l’applet de commande de déploiement des services ADDS à l’aide de l’argument suivant:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft déconseille d’ignorer la vérification en tant qu’il peut entraîner une promotion du contrôleur de domaine partielle ou endommagée forêt ADDS.  
  
Cliquez sur **installer** pour commencer le processus de promotion de contrôleur de domaine. Il s’agit de dernière possibilité d’annuler l’installation. Vous ne pouvez pas annuler le processus de promotion une fois qu’il commence. L’ordinateur redémarre automatiquement à la fin de la promotion, quel que soit la promotion results.The **vérification des conditions requises** page affiche tous les problèmes rencontrés pendant le processus et des conseils sur la résolution du problème.  
  
### <a name="installation"></a>Installation  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
Lorsque le **Installation** page s’affiche, la configuration de contrôleur de domaine commence et ne peut pas être interrompue ou annulée. Les opérations détaillées s’affichent sur cette page et sont écrites dans les journaux:  
  
-   %Systemroot%\Debug\Dcpromo.log  
  
-   %SystemRoot%\debug\dcpromoui.log  
  
-   %SystemRoot%\debug\adprep\logs  
  
-   %SystemRoot%\debug\netsetup.log (si le serveur est un groupe de travail)  
  
Pour installer une nouvelle forêt ActiveDirectory à l’aide du module ADDSDeployment, utilisez l’applet de commande suivante:  
  
```  
Install-addsdomaincontroller  
```  
  
Voir [mise à niveau et réplication Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS) pour connaître les arguments obligatoires et facultatifs.  
  
Le **Install-AddsDomainController** applet de commande comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d’installation avec les arguments requis minimaux **- domainname** et **-credential**. Notez comment l’opération Adprep se produit automatiquement dans le cadre de l’ajout du premier contrôleur de domaine Windows Server2012 à une forêt Windows Server2003 existante:  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
Notez comment, comme le Gestionnaire de serveur, **Install-ADDSDomainController** vous rappelle que la promotion redémarre automatiquement le serveur. Pour accepter automatiquement l’invite de redémarrage, utilisez la **-force** ou **-confirmer: $false** arguments avec une applet de commande WindowsPowerShellADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez le **- norebootoncompletion** argument.  
  
> [!WARNING]  
> Il est déconseillé de remplacer le redémarrage. Le contrôleur de domaine doit redémarrer pour fonctionner correctement.  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
Pour configurer un contrôleur de domaine à distance à l’aide de Windows PowerShell, encapsuler les **install-adddomaincontroller** applet de commande *à l’intérieur de* de la **invoke-command** applet de commande. Cela nécessite l’utilisation des accolades.  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
Par exemple:  
  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> Pour plus d’informations sur Adprep et installation le fonctionnement du processus, voir la [Troubleshooting Domain Controller Deployment](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
### <a name="results"></a>Résultats  
![Installer un réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
Le **résultats** page indique la réussite ou l’échec de la promotion et toute information d’administration importante. En cas de succès, le contrôleur de domaine redémarre automatiquement après 10secondes.  
  
Comme avec les versions précédentes de Windows Server, la préparation du domaine automatisée pour les contrôleurs de domaine qui exécutent Windows server 2012 n’exécute pas GPPREP. Exécutez **adprep.exe /gpprep** manuellement pour tous les domaines qui n’étaient pas auparavant préparés pour Windows Server2003, Windows Server2008 ou Windows Server2008R2. Vous devez exécuter GPPrep qu’une seule fois dans l’historique d’un domaine et non avec chaque mise à niveau. Adprep.exe n’exécute pas /gpprep automatiquement, car l’opération peut provoquer de tous les fichiers et dossiers dans le dossier SYSVOL pour la réplication de nouveau sur tous les contrôleurs de domaine.  
  

