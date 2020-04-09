---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: Installer un nouveau domaine enfant ou domaine d’arborescence Active Directory Windows Server 2012 (niveau 200)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f7244b76364c8e2ce7249af8e76825a08b2a75c8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825332"
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>Installer un nouveau domaine enfant ou domaine d’arborescence Active Directory Windows Server 2012 (niveau 200)

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique explique comment ajouter des domaines enfants et d'arborescence à une forêt Windows Server 2012 existante à l'aide du Gestionnaire de serveur ou de Windows PowerShell.  
  
-   [Flux de travail de domaine enfant et d’arborescence](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Domaine enfant et arborescence Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [Déploiement](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="child-and-tree-domain-workflow"></a><a name="BKMK_Workflow"></a>Flux de travail de domaine enfant et d’arborescence  
Le diagramme suivant illustre le processus de configuration des services de domaine Active Directory quand vous avez auparavant installé le rôle AD DS et démarré l'Assistant Configuration des services de domaine Active Directory à l'aide du Gestionnaire de serveur pour créer un domaine dans une forêt existante.  
  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="child-and-tree-domain-windows-powershell"></a><a name="BKMK_PS"></a>Domaine enfant et arborescence Windows PowerShell  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (les arguments en **gras** sont obligatoires. Les arguments en *italique* peuvent être spécifiés à l'aide de Windows PowerShell ou de l'Assistant Configuration des services de domaine Active Directory.)|  
|**Install-AddsDomain**|-SkipPreChecks<p>***-NewDomainName***<p>***-ParentDomainName***<p>***-SafeModeAdministratorPassword***<p>*-ADPrepCredential*<p>-AllowDomainReinstall<p>-Confirm<p>*-CreateDNSDelegation*<p>***-Credential***<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>-NoDNSOnNetwork<p>*-DomainMode*<p>***-DomainType***<p>-Force<p>*-InstallDNS*<p>*-LogPath*<p>*-NewDomainNetBIOSName*<p>*-NoGlobalCatalog*<p>-NoNorebootoncompletion<p>*-ReplicationSourceDC*<p>*-SiteName*<p>-SkipAutoConfigureDNS<p>*-SYSVOLPath*<p>*-WhatIf*|  
  
> [!NOTE]  
> L'argument **-credential** est uniquement requis quand vous n'êtes actuellement pas connecté en tant que membre du groupe Administrateurs de l'entreprise. L'argument **-NewDomainNetBIOSName** est requis si vous voulez modifier le nom de 15 caractères automatiquement généré en fonction du préfixe du nom de domaine DNS ou si le nom compte plus de 15 caractères.  
  
## <a name="deployment"></a><a name="BKMK_Deployment"></a>Déploiement  
  
### <a name="deployment-configuration"></a>Configuration du déploiement  
La capture d'écran suivante présente les options permettant d'ajouter un domaine enfant :  
  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
La capture d'écran suivante présente les options permettant d'ajouter un domaine d'arborescence :  
  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
Le Gestionnaire de serveur commence la promotion de chaque contrôleur de domaine par la page **Configuration de déploiement** . Dans cette page et les pages suivantes, les options disponibles et les champs requis varient selon l’opération de déploiement que vous sélectionnez.  
  
Cette rubrique combine deux opérations discrètes : la promotion d'un domaine enfant et la promotion d'un domaine d'arborescence. La seule différence entre les deux opérations concerne le type de domaine que vous choisissez de créer. Toutes les autres étapes sont identiques entre les deux opérations.  
  
-   Pour créer un domaine enfant, cliquez sur **Ajouter un nouveau domaine à une forêt existante** et choisissez **Domaine enfant**. Pour **Nom du domaine parent**, tapez ou sélectionnez le nom du domaine parent. Tapez ensuite le nom du nouveau domaine dans la zone **Nouveau nom de domaine**. Entrez un nom de domaine enfant en une partie valide ; le nom doit satisfaire aux conditions d'attribution des noms de domaine DNS.  
  
-   Pour créer un domaine d'arborescence dans une forêt existante, cliquez sur **Ajouter un nouveau domaine à une forêt existante** et choisissez **Domaine de l'arborescence**. Tapez le nom du domaine racine de forêt, puis celui du nouveau domaine. Entrez un nom de domaine racine complet valide ; le nom ne doit pas être en une partie et doit satisfaire aux conditions d'attribution des noms de domaine DNS.  
  
Pour plus d'informations sur les noms DNS, consultez la page [Conventions d'affectation de noms dans Active Directory pour les ordinateurs, domaines, sites et unités d'organisation](https://support.microsoft.com/kb/909264).  
  
L'Assistant Configuration des services de domaine Active Directory du Gestionnaire de serveur vous invite à entrer les informations d'identification du domaine si vos informations d'identification actuelles ne proviennent pas du domaine. Cliquez sur **Modifier** pour fournir des informations d'identification du domaine pour l'opération de promotion.  
  
L'applet de commande ADDSDeployment de configuration du déploiement et les arguments sont les suivants :  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
La page **Options du contrôleur de domaine** spécifie les options du contrôleur de domaine pour le nouveau contrôleur de domaine. Parmi les options du contrôleur de domaine configurables figurent **Serveur DNS** et **Catalogue global**. Notez que vous ne pouvez pas configurer un contrôleur de domaine en lecture seule comme premier contrôleur de domaine dans un nouveau domaine.  
  
Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et de catalogue global à des fins de haute disponibilité dans les environnements distribués. Le catalogue global est toujours sélectionné par défaut et DNS est sélectionné par défaut si le domaine actuel héberge déjà DNS sur ses contrôleurs de domaine, selon une requête SOA (Start-of-Authority). Vous devez également spécifier un **Niveau fonctionnel du domaine**. Le niveau fonctionnel par défaut est Windows Server 2012 et vous pouvez choisir toute autre valeur supérieure ou égale au niveau fonctionnel actuel de la forêt.  
  
La page **Options du contrôleur de domaine** vous permet également de choisir le **nom de site** logique Active Directory approprié à partir de la configuration de la forêt. Par défaut, le site avec le sous-réseau le mieux adapté est sélectionné. S'il n'existe qu'un site, celui-ci est automatiquement sélectionné.  
  
> [!IMPORTANT]  
> Si le serveur n'appartient pas à un sous-réseau Active Directory et qu'il existe plusieurs sites Active Directory, rien n'est sélectionné et le bouton **Suivant** n'apparaît que quand vous avez choisi un site dans la liste.  
  
Le **Mot de passe du mode de restauration des services d'annuaire** spécifié doit respecter la stratégie de mot de passe appliquée au serveur. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète.  
  
Les arguments de l'applet de commande ADDSDeployment **Options du contrôleur de domaine** sont les suivants :  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> Le nom du site doit déjà exister quand il est fourni en tant que valeur à l'argument **sitename** . L'applet de commande **install-AddsDomainController** ne crée pas de nom de site. Vous pouvez utiliser l'applet de commande **new-adreplicationsite** pour créer des sites.  
  
Les arguments de l'applet de commande **Install-ADDSDomainController** suivent les mêmes paramètres par défaut que le Gestionnaire de serveur s'ils ne sont pas spécifiés.  
  
Le fonctionnement de l'argument **SafeModeAdministratorPassword** est spécial :  
  
-   S'ils ne sont *pas spécifiés* en tant qu'arguments, l'applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer un domaine enfant nommé NorthAmerica dans la forêt Contoso.com et être invité à entrer et à confirmer un mot de passe masqué :  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
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
  
Enfin, vous pouvez stocker le mot de passe obscurci dans un fichier, puis le réutiliser plus tard, sans que le mot de passe en texte clair ne s'affiche. Par exemple :  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Il n'est pas recommandé de fournir ni de stocker un mot de passe en texte clair ou obfusqué. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe DSRM de ce contrôleur de domaine.  Toute personne ayant accès au fichier peut annuler ce mot de passe obfusqué. Munie de cette information, elle peut ouvrir une session sur un contrôleur de domaine en mode DSRM et finir par emprunter l'identité du contrôleur de domaine lui-même, en élevant ses privilèges au niveau le plus élevé d'une forêt Active Directory. Une autre procédure utilisant **System.Security.Cryptography** pour chiffrer les données du fichier texte est conseillée, mais n'est pas traitée ici. Le mieux est d'éviter totalement tout stockage de mot de passe.  
  
Le module ADDSDeployment offre une autre option permettant d'ignorer la configuration automatique des paramètres des clients DNS, des redirecteurs et des indications de racine. Cette option n'est pas configurable en utilisant le Gestionnaire de serveur. Cet argument n'est important que si vous avez déjà installé le service Serveur DNS avant de configurer le contrôleur de domaine :  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Options DNS et informations d'identification de délégation DNS  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
La page **Options DNS** vous permet d'indiquer d'autres informations d'identification d'administration DNS pour la délégation.  
  
Pendant l'installation d'un nouveau domaine dans une forêt existante, où vous avez sélectionné l'installation du service DNS dans la page **Options du contrôleur de domaine** , vous ne pouvez configurer aucune option ; la délégation se produit de façon automatique et irrévocable. Vous avez la possibilité de fournir d'autres informations d'identification d'administration DNS avec des droits de mise à jour de cette structure.  
  
Les arguments Windows PowerShell ADDSDeployment **Options DNS** sont les suivants :  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
Pour plus d'informations sur la délégation DNS, consultez la page [Présentation de la délégation de zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Options supplémentaires  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
La page **Options supplémentaires** indique le nom NetBIOS du domaine et vous permet de le remplacer. Par défaut, le nom de domaine NetBIOS correspond à la partie la plus à gauche du nom de domaine complet fourni dans la page **Configuration de déploiement** . Par exemple, si vous avez indiqué le nom de domaine complet corp.contoso.com, le nom de domaine NetBIOS par défaut est CORP.  
  
Si le nom contient au maximum 15 caractères et n'est en conflit avec aucun autre nom NetBIOS, il n'est pas modifié. S'il est en conflit avec un autre nom NetBIOS, un numéro est ajouté au nom. Si le nom contient plus de 15 caractères, l'Assistant propose une suggestion tronquée unique. Dans les deux cas, l'Assistant valide d'abord que le nom n'est pas déjà utilisé via une recherche WINS et une diffusion NetBIOS.  
  
Pour plus d'informations sur les noms DNS, consultez la page [Conventions d'affectation de noms dans Active Directory pour les ordinateurs, domaines, sites et unités d'organisation](https://support.microsoft.com/kb/909264).  
  
Les arguments **Install-AddsDomain** suivent les mêmes paramètres par défaut que le Gestionnaire de serveur s'ils ne sont pas spécifiés. Le fonctionnement de **DomainNetBIOSName** est spécial :  
  
1.  Si l'argument **NewDomainNetBIOSName** n'est pas spécifié avec un nom de domaine NetBIOS et que le nom de domaine avec préfixe en une partie dans l'argument **DomainName** contient au maximum 15 caractères, la promotion continue avec un nom généré automatiquement.  
  
2.  Si l'argument **NewDomainNetBIOSName** n'est pas spécifié avec un nom de domaine NetBIOS et que le nom de domaine avec préfixe en une partie dans l'argument **DomainName** contient au minimum 16 caractères, la promotion échoue.  
  
3.  Si l'argument **NewDomainNetBIOSName** est spécifié avec un nom de domaine NetBIOS de 15 caractères au maximum, la promotion continue avec ce nom spécifié.  
  
4.  Si l'argument **NewDomainNetBIOSName** est spécifié avec un nom de domaine NetBIOS de 16 caractères au minimum, la promotion échoue.  
  
L'argument de l'applet de commande ADDSDeployment **Options supplémentaires** est le suivant :  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>Chemins d’accès  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
La page **Chemins d'accès** vous permet de remplacer les emplacements de dossier par défaut de la base de données AD DS, les journaux des transactions de base de données et le partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de %systemroot%.  
  
Les arguments de l'applet de commande ADDSDeployment **Chemins d'accès** sont les suivants :  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Examiner les options et Afficher le script  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
La page **Examiner les options** vous permet de valider vos paramètres et de vérifier qu'ils répondent à vos exigences avant le démarrage de l'installation. Notez que vous avez encore la possibilité d'arrêter l'installation quand vous utilisez le Gestionnaire de serveur. Il s'agit simplement d'une option permettant de confirmer vos paramètres avant de poursuivre la configuration  
  
La page **Examiner les options** du Gestionnaire de serveur offre également un bouton **Afficher le script** facultatif pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme d’un script Windows PowerShell unique. Vous pouvez ainsi utiliser l’interface graphique Gestionnaire de serveur sous forme d’un studio de déploiement Windows PowerShell. Utilisez l’Assistant Configuration des services de domaine Active Directory pour configurer les options, exportez la configuration, puis annulez l’Assistant.  Ce processus crée un exemple valide et correct du point de vue syntaxique pour permettre des modifications ultérieures ou une utilisation directe. Par exemple :  
  
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
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s'appuie pas sur les valeurs par défaut (car elles peuvent changer entre les futures versions de Windows ou les Service Packs). La seule exception concerne l'argument **-safemodeadministratorpassword** (qui est délibérément omis du script). Pour forcer une demande de confirmation, omettez la valeur en cas d'exécution interactive de l'applet de commande.  
  
Utilisez l'argument **Whatif** facultatif avec l'applet de commande **Install-ADDSForest** pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d'une applet de commande.  
  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>Vérification des conditions préalables  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
La fonctionnalité **Vérification de la configuration requise** est nouvelle dans la configuration de domaine AD DS. Cette nouvelle phase valide que la configuration du serveur est capable de prendre en charge un nouveau domaine AD DS.  
  
Pendant l'installation d'un nouveau domaine racine de forêt, l'Assistant Configuration des services de domaine Active Directory du Gestionnaire de serveur appelle une série de tests modulaires sérialisés. Ces tests vous alertent avec des suggestions d'opérations de réparation. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus du contrôleur de domaine ne peut pas continuer tant que tous les tests de configuration requise ne sont pas réussis.  
  
La fonctionnalité **Vérification de la configuration requise** met également en évidence des informations pertinentes, telles que les modifications de sécurité qui affectent les systèmes d'exploitation plus anciens.  
  
Pour plus d'informations sur les vérifications spécifiques de la configuration requise, voir [Vérification de la configuration requise](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Vous ne pouvez pas ignorer la fonctionnalité **Vérification de la configuration requise** quand vous utilisez le Gestionnaire de serveur, mais vous pouvez ignorer le processus quand vous utilisez l'applet de commande de déploiement des services AD DS avec l'argument suivant :  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft déconseille d'ignorer la vérification de la configuration requise, car cela peut aboutir à une promotion partielle du contrôleur de domaine ou à l'endommagement de la forêt AD DS.  
  
Cliquez sur **Installer** pour commencer le processus de promotion du contrôleur de domaine. Il s'agit de la dernière possibilité d'annuler l'installation. Vous ne pouvez pas annuler le processus de promotion une fois qu'il a commencé. L'ordinateur redémarre automatiquement à la fin de la promotion, quel qu'en soit le résultat.  
  
### <a name="installation"></a>Installation  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
Lorsque la page **Installation** s'affiche, la configuration du contrôleur de domaine commence et ne peut pas être arrêtée ni annulée. Les opérations détaillées s'affichent dans cette page et sont écrites dans les journaux :  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Pour installer un nouveau domaine Active Directory à l'aide du module ADDSDeployment, utilisez l'applet de commande suivante :  
  
```  
Install-addsdomain  
```  
  
Pour connaître les arguments requis et facultatifs, voir [Domaine enfant et d'arborescence dans Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS). L'applet de commande **Install-addsdomain** comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d'installation avec les arguments requis minimaux **-domaintype**, **-newdomainname**, **-parentdomainname** et **-credential**. Notez comment, de la même façon que le Gestionnaire de serveur, **Install-ADDSDomain** vous rappelle que la promotion redémarre automatiquement le serveur.  
  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
Pour accepter automatiquement l'invite de redémarrage, utilisez les arguments **-force** ou **-confirm:$false** avec n'importe quelle applet de commande Windows PowerShell ADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez l'argument **-norebootoncompletion**.  
  
> [!WARNING]  
> Il n'est pas recommandé de remplacer le redémarrage. Le contrôleur de domaine doit redémarrer pour fonctionner correctement.  
  
### <a name="results"></a>Résultats  
![Installer un nouvel enfant AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La page **Résultats** indique la réussite ou l'échec de la promotion et toute information d'administration importante. Le contrôleur de domaine redémarre automatiquement après 10 secondes.  
  

