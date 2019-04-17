---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: "Installer un nouveau Windows Server2012 ActiveDirectory enfant ou un domaine d’arborescence (niveau 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc0eecc44bbc5f7459f22aceb5ebe41cd61948b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>Installer un nouveau Windows Server2012 ActiveDirectory enfant ou un domaine d’arborescence (niveau 200)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique explique comment ajouter des domaines enfants et d’arborescence à une forêt existante de Windows Server2012, à l’aide du Gestionnaire de serveur ou Windows PowerShell.  
  
-   [Enfants et Workflow de domaine d’arborescence](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Enfants et Tree Domain Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [Déploiement](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="BKMK_Workflow"></a>Enfants et Workflow de domaine d’arborescence  
Le diagramme suivant illustre le processus de configuration des Services de domaine ActiveDirectory lorsque vous avez installé précédemment le rôle ADDS et que vous avez démarré l’Assistant ActiveDirectory domaine Services de Configuration à l’aide du Gestionnaire de serveur pour créer un nouveau domaine dans une forêt existante.  
  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="BKMK_PS"></a>Enfants et Tree Domain Windows PowerShell  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (**gras** arguments sont requis. *En italique* arguments peuvent être spécifiés à l’aide de Windows PowerShell ou l’Assistant de Configuration ADDS.)|  
|**Install-AddsDomain**|-SkipPreChecks<br /><br />***-NewDomainName***<br /><br />***-ParentDomainName***<br /><br />***-SafeModeAdministratorPassword***<br /><br />*-ADPrepCredential*<br /><br />-AllowDomainReinstall<br /><br />-Confirmer<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-NoDNSOnNetwork<br /><br />*DomainMode-*<br /><br />***-DomainType***<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />*-NewDomainNetBIOSName*<br /><br />*-NoGlobalCatalog*<br /><br />-NoNorebootoncompletion<br /><br />*ReplicationSourceDC-*<br /><br />*-SiteName*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> Le **-credential** argument est uniquement requis lorsque vous n'êtes pas actuellement connecté en tant que membre des administrateurs de l’entreprise group.The **- NewDomainNetBIOSName** argument est requis si vous souhaitez modifier le nom de 15caractères automatiquement généré basé sur le préfixe de nom de domaine DNS ou si le nom dépasse 15caractères.  
  
## <a name="BKMK_Deployment"></a>Déploiement  
  
### <a name="deployment-configuration"></a>Configuration de déploiement  
La capture d’écran suivante présente les options pour l’ajout d’un domaine enfant:  
  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
La capture d’écran suivante présente les options pour l’ajout d’un domaine d’arborescence:  
  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
Le Gestionnaire de serveur commence chaque promotion du contrôleur de domaine avec le **Configuration de déploiement** page. Les options disponibles et les champs obligatoires modifier sur cette page et les pages suivantes, en fonction de l’opération de déploiement que vous sélectionnez.  
  
Cette rubrique combine deux opérations discrètes: la promotion du domaine enfant et promotion de domaine d’arborescence. La seule différence entre les deux opérations est le type de domaine que vous choisissez de créer. Toutes les autres étapes sont identiques entre les deux opérations.  
  
-   Pour créer un nouveau domaine enfant, cliquez sur **ajouter un domaine à une forêt existante** et choisissez **domaine enfant**. Pour **nom du domaine Parent**, tapez ou sélectionnez le nom du domaine parent. Puis tapez le nom du nouveau domaine dans le **nouveau nom de domaine** zone. Fournir un enfant valide, une partie de nom de domaine; le nom doit satisfaire les exigences de nom de domaine DNS.  
  
-   Pour créer un domaine d’arborescence dans une forêt existante, cliquez sur **ajouter un domaine à une forêt existante** et choisissez **domaine d’arborescence**. Tapez le nom du domaine racine de forêt, puis tapez le nom du nouveau domaine. Entrez un nom de domaine racine complet valide; le nom ne peut pas être une partie et doit utiliser des exigences de nom de domaine DNS.  
  
Pour plus d’informations sur les noms DNS, consultez [conventions de dénomination dans ActiveDirectory pour les ordinateurs, domaines, sites et unités d’organisation](https://support.microsoft.com/kb/909264).  
  
L’Assistant Gestionnaire de serveur ActiveDirectory domaine Services Configuration vous demande les informations d’identification de domaine si vos informations d’identification actuelles ne proviennent pas du domaine. Cliquez sur **modification** pour fournir des informations d’identification de domaine pour l’opération de promotion.  
  
L’applet de commande ADDSDeployment de Configuration de déploiement et les arguments sont:  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
Le **Options du contrôleur de domaine** page spécifie les options du contrôleur de domaine pour le nouveau contrôleur de domaine. Les options du contrôleur de domaine configurables incluent **serveur DNS** et **catalogue Global**; Vous ne pouvez pas configurer le contrôleur de domaine en lecture seule comme premier contrôleur de domaine dans un nouveau domaine.  
  
Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et de catalogue global pour la haute disponibilité dans les environnements distribués. Dans le catalogue global est toujours sélectionné par défaut et DNS est activée par défaut si le domaine actuel héberge déjà DNS sur ses contrôleurs de domaine, basés sur une requête Start-of-Authority. Vous devez également spécifier un **niveau fonctionnel du domaine**. Le niveau fonctionnel par défaut est Windows Server2012, et vous pouvez choisir une autre valeur est égale ou supérieure au niveau fonctionnel de forêt actuel.  
  
Le **Options du contrôleur de domaine** page vous permet également de choisir la logique ActiveDirectory approprié **nom du site** à partir de la configuration de la forêt. Par défaut, le site avec le sous-réseau le mieux adapté est sélectionné. S’il existe un seul site, il est automatiquement sélectionné.  
  
> [!IMPORTANT]  
> Si le serveur n’appartient pas à un sous-réseau ActiveDirectory, et il existe plusieurs sites ActiveDirectory, rien n’est sélectionné et le **suivant** bouton n’est pas disponible jusqu'à ce que vous choisissez un site dans la liste.  
  
Spécifié **passe en Mode restauration des Services Directory** doit respecter la stratégie de mot de passe appliquée au serveur. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète.  
  
Le **Options du contrôleur de domaine** sont des arguments de l’applet de commande ADDSDeployment:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> Le nom du site doit déjà exister quand fournie comme valeur pour le **sitename** argument. Le **install-AddsDomainController** applet de commande ne crée pas de noms de sites. Vous pouvez utiliser le **nouveau-adreplicationsite** applet de commande pour créer des sites.  
  
Le **Install-ADDSDomainController** arguments de l’applet de commande suivent les mêmes paramètres par défaut en tant que gestionnaire de serveur si non spécifié.  
  
Le **SafeModeAdministratorPassword** de l’argument est spécial:  
  
-   Si *ne pas spécifié* en tant qu’argument, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer un nouvel enfant domaine nommé NorthAmerica dans la forêt Contoso.com et être invité à entrer et à confirmer un mot de passe masqué:  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
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
  
Le module ADDSDeployment offre une autre option pour ignorer la configuration automatique des paramètres de client DNS, des redirecteurs et des indications de racine. Cela n’est pas configurable lorsque vous utilisez le Gestionnaire de serveur. Cet argument n’est important que si vous déjà installé le service serveur DNS avant de configurer le contrôleur de domaine:  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Options DNS et les informations d’identification de délégation DNS  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
Le **Options DNS** page vous permet de fournir d’autres informations d’identification d’administrateur DNS pour la délégation.  
  
Lors de l’installation d’un nouveau domaine dans une forêt existante, où vous avez sélectionné installation du service DNS sur le **Options du contrôleur de domaine** page - vous ne pouvez pas configurer les options; la délégation se produit automatiquement et irrévocable. Vous avez la possibilité de fournir d’autres informations d’identification d’administration DNS avec des droits pour mettre à jour de cette structure.  
  
Le **Options DNS** sont des arguments WindowsPowerShellADDSDeployment:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
Pour plus d’informations sur la délégation DNS, voir [présentation la délégation de Zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Options supplémentaires  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
Le **Options supplémentaires** page affiche le nom NetBIOS du domaine et vous permet de remplacer. Par défaut, le nom de domaine NetBIOS correspond à l’étiquette la plus à gauche du nom de domaine complet fourni sur le **Configuration de déploiement** page. Par exemple, si vous avez indiqué le nom de domaine complet de corp.contoso.com, le nom de domaine NetBIOS par défaut est CORP.  
  
Si le nom est de 15caractères ou moins et n’est pas en conflit avec un autre nom NetBIOS, il est modifié. Si elle n’est en conflit avec un autre nom NetBIOS, un numéro est ajouté au nom. Si le nom est plus de 15caractères, l’Assistant fournit une suggestion tronquée unique. Dans les deux cas, l’Assistant vérifie tout d’abord le nom n’est pas déjà en cours d’utilisation via une recherche WINS et une diffusion NetBIOS.  
  
Pour plus d’informations sur les noms DNS, consultez [conventions de dénomination dans ActiveDirectory pour les ordinateurs, domaines, sites et unités d’organisation](https://support.microsoft.com/kb/909264).  
  
Le **Install-AddsDomain** arguments suivent les mêmes paramètres par défaut en tant que gestionnaire de serveur, le cas contraire. Le **DomainNetBIOSName** opération est spéciale:  
  
1.  Si le **NewDomainNetBIOSName** argument n’est pas spécifié avec un nom de domaine NetBIOS et le nom de domaine de préfixe en une partie dans le **DomainName** argument est de 15caractères ou moins, puis la promotion continue avec un nom généré automatiquement.  
  
2.  Si le **NewDomainNetBIOSName** argument n’est pas spécifié avec un nom de domaine NetBIOS et le nom de domaine de préfixe en une partie dans le **DomainName** argument est de 16caractères ou plus, puis la promotion échoue.  
  
3.  Si le **NewDomainNetBIOSName** argument est spécifié avec un nom de domaine NetBIOS de 15caractères au maximum, puis la promotion continue avec ce nom spécifié.  
  
4.  Si le **NewDomainNetBIOSName** argument est spécifié avec un nom de domaine NetBIOS de 16caractères ou plus, puis la promotion échoue.  
  
Le **Options supplémentaires** argument de l’applet de commande ADDSDeployment est:  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>Chemins d’accès  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
Le **chemins d’accès** page vous permet de remplacer les emplacements de dossier par défaut de la base de données ADDS, les journaux de transactions de base de données et le partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de % systemroot%.  
  
Le **chemins d’accès** sont des arguments de l’applet de commande ADDSDeployment:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Examiner les Options et afficher le Script  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
Le **examiner les Options** page vous permet de valider vos paramètres et assurez-vous qu’ils répondent à vos besoins avant de commencer l’installation. Cela n’est pas la possibilité d’arrêter l’installation lors de l’utilisation du Gestionnaire de serveur. Il s’agit simplement d’une option pour confirmer vos paramètres avant de poursuivre la configuration  
  
Le **examiner les Options** page dans le Gestionnaire de serveur offre également une option **afficher le Script** bouton pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme de script Windows PowerShell unique. Cela vous permet d’utiliser l’interface graphique du Gestionnaire de serveur comme un studio de déploiement de Windows PowerShell. Utilisez l’Assistant Configuration des Services de domaine ActiveDirectory pour configurer les options, exportez la configuration et annulez ensuite l’Assistant.  Ce processus crée un exemple valide et la syntaxe correct pour modification supplémentaire ou une utilisation directe. Par exemple:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomain `  
-NoGlobalCatalog:$false `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainType "ChildDomain" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NewDomainName "research" `  
-NewDomainNetBIOSName "RESEARCH" `  
-ParentDomainName "corp.contoso.com" `  
-Norebootoncompletion:$false `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s’appuie pas sur les valeurs par défaut (car elles peuvent changer entre les versions futures de Windows ou des service packs). La seule exception est le **- safemodeadministratorpassword** argument (qui est délibérément omis du script). Pour forcer une demande de confirmation, omettez la valeur en cas d’exécution interactive de l’applet de commande.  
  
Utilisez l’option **Whatif** argument avec la **Install-ADDSForest** applet de commande pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d’une applet de commande.  
  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>Vérification des conditions préalables  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
Le **vérification des conditions requises** est une nouvelle fonctionnalité de configuration du domaine ADDS. Cette nouvelle phase valide que la configuration du serveur est capable de prendre en charge un nouveau domaine ADDS.  
  
Lorsque vous installez un domaine racine de forêt, l’Assistant Gestionnaire de serveur ActiveDirectory domaine Services Configuration appelle une série de tests modulaires sérialisés. Ces tests vous alerte avec les options de réparation suggérés. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus de contrôleur de domaine ne peut pas continuer tant que la configuration requise de tous les tests passer.  
  
Le **vérification des conditions requises** exposent également des informations pertinentes, telles que les modifications de sécurité qui affectent les anciens systèmes d’exploitation.  
  
Pour plus d’informations sur les vérifications spécifiques de la configuration requise, voir [Prerequisite Checking](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Vous ne pouvez pas ignorer le **vérification** lorsque à l’aide du Gestionnaire de serveur, mais vous pouvez ignorer le processus lors de l’utilisation de l’applet de commande de déploiement des services ADDS à l’aide de l’argument suivant:  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft déconseille d’ignorer la vérification en tant qu’il peut entraîner une promotion du contrôleur de domaine partielle ou endommagée forêt ADDS.  
  
Cliquez sur **installer** pour commencer le processus de promotion de contrôleur de domaine. Il s’agit de dernière possibilité d’annuler l’installation. Vous ne pouvez pas annuler le processus de promotion une fois qu’il commence. L’ordinateur redémarre automatiquement à la fin de la promotion, quel que soit le résultat.  
  
### <a name="installation"></a>Installation  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
Lorsque le **Installation** page s’affiche, la configuration de contrôleur de domaine commence et ne peut pas être interrompue ou annulée. Les opérations détaillées s’affichent sur cette page et sont écrites dans les journaux:  
  
-   %Systemroot%\Debug\Dcpromo.log  
  
-   %SystemRoot%\debug\dcpromoui.log  
  
Pour installer un nouveau domaine ActiveDirectory à l’aide du module ADDSDeployment, utilisez l’applet de commande suivante:  
  
```  
Install-addsdomain  
```  
  
Voir [enfant et arborescence de domaine Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS) pour connaître les arguments obligatoires et facultatifs arguments.The **Install-addsdomain** applet de commande comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d’installation avec les arguments requis minimaux **- domaintype**, **- newdomainname**, **- parentdomainname**, et **-credential**. Notez comment, comme le Gestionnaire de serveur, **Install-ADDSDomain** vous rappelle que la promotion redémarre automatiquement le serveur.  
  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
Pour accepter automatiquement l’invite de redémarrage, utilisez la **-force** ou **-confirmer: $false** arguments avec une applet de commande WindowsPowerShellADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez le **- norebootoncompletion** argument.  
  
> [!WARNING]  
> Remplacer le redémarrage n’est pas recommandée. Le contrôleur de domaine doit redémarrer pour fonctionner correctement  
  
### <a name="results"></a>Résultats  
![Installer un nouveau enfant ActiveDirectory.](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
Le **résultats** page indique la réussite ou l’échec de la promotion et toute information d’administration importante. Le contrôleur de domaine redémarre automatiquement après 10secondes.  
  

