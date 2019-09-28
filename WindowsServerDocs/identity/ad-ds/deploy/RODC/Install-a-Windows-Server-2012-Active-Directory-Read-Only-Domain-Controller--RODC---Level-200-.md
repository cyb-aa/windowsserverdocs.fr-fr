---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: Installer un contrôleur de domaine en lecture seule Windows Server 2012 Active Directory (niveau 200)
description: Cette rubrique explique comment créer un compte de contrôleur de domaine en lecture seule intermédiaire, puis associer un serveur à ce compte pendant l'installation d'un contrôleur de domaine en lecture seule. Cette rubrique explique également comment installer un contrôleur de domaine en lecture seule sans effectuer d'installation intermédiaire.
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 82b0035075c981d123ab3b90d56768940f65558e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391110"
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>Installer un contrôleur de domaine en lecture seule Windows Server 2012 Active Directory (niveau 200)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique explique comment créer un compte de contrôleur de domaine en lecture seule intermédiaire, puis associer un serveur à ce compte pendant l'installation d'un contrôleur de domaine en lecture seule. Cette rubrique explique également comment installer un contrôleur de domaine en lecture seule sans effectuer d'installation intermédiaire.  
  
## <a name="stage-rodc-workflow"></a>Workflow de création d'un contrôleur de domaine en lecture seule intermédiaire  
L'installation d'un contrôleur de domaine en lecture seule intermédiaire se déroule en deux phases discrètes :  
  
1.  Création intermédiaire d'un compte d'ordinateur inoccupé  
  
2.  Association d'un contrôleur de domaine en lecture seule à ce compte pendant la promotion  
  
Le diagramme suivant illustre le processus de création intermédiaire d'un contrôleur de domaine en lecture seule des services de domaine Active Directory, où vous créez un compte d'ordinateur de contrôleur de domaine en lecture seule vide dans le domaine à l'aide du Centre d'administration Active Directory (Dsac.exe).  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)  
  
## <a name="BKMK_StagePS"></a>Phase de RODC Windows PowerShell  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (les arguments en **gras** sont obligatoires. Les arguments en *italique* peuvent être spécifiés à l'aide de Windows PowerShell ou de l'Assistant Configuration des services de domaine Active Directory.)|  
|Add-addsreadonlydomaincontrolleraccount|-SkipPreChecks<br /><br />***-DomainControllerAccountName***<br /><br />***-DomainName***<br /><br />***-SiteName***<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />***-Credential***<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />*-NoGlobalCatalog*<br /><br />*-InstallDNS*<br /><br />-ReplicationSourceDC|  
  
> [!NOTE]  
> L'argument **-credential** est uniquement requis si vous n'êtes pas déjà connecté en tant que membre du groupe Admins du domaine.  
  
## <a name="attach-rodc-workflow"></a>Workflow d'association d'un contrôleur de domaine en lecture seule  
Le diagramme ci-dessous illustre le processus de configuration des services de domaine Active Directory, où vous avez déjà installé le rôle AD DS, créé le compte du contrôleur de domaine en lecture seule intermédiaire et démarré **Promouvoir ce serveur en contrôleur de domaine** à l'aide du Gestionnaire de serveur pour créer un contrôleur de domaine en lecture seule dans un domaine existant, en l'associant au compte d'ordinateur intermédiaire.  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)  
  
## <a name="BKMK_AttachPS"></a>Attacher Windows PowerShell RODC  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (les arguments en **gras** sont obligatoires. Les arguments en *italique* peuvent être spécifiés à l'aide de Windows PowerShell ou de l'Assistant Configuration des services de domaine Active Directory.)|  
|Install-AddsDomaincontroller|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />*-InstallationMediaPath*<br /><br />*-LogPath*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />***-UseExistingAccount***|  
  
> [!NOTE]  
> L'argument **-credential** est uniquement requis si vous n'êtes pas déjà connecté en tant que membre du groupe Admins du domaine.  
  
## <a name="staging"></a>Création intermédiaire  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)  
  
Vous procédez à la création intermédiaire d'un compte d'ordinateur de contrôleur de domaine en lecture seule en ouvrant le Centre d'administration Active Directory (**Dsac.exe**). Cliquez sur le nom du domaine dans le volet de navigation. Double-cliquez sur **Contrôleurs de domaine** dans la liste de gestion. Cliquez sur **Pré-créer un compte de contrôleur de domaine en lecture seule** dans le volet des tâches.  
  
Pour plus d’informations sur la Centre d’administration Active Directory, consultez [gestion avancée des AD DS &#40;à l'&#41; aide de centre d’administration Active Directory niveau 200](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md) et examen du centre d’administration de l’annuaire [Active : Prise en main @ no__t-0.  
  
Si vous avez déjà créé des contrôleurs de domaine en lecture seule, vous allez découvrir que l'Assistant Installation a la même interface graphique que celle affichée avec l'ancien composant logiciel enfichable Utilisateurs et ordinateurs Active Directory de Windows Server 2008 et utilise le même code, qui comprend l'exportation de la configuration au format de fichier d'installation sans assistance employé par le processus dcpromo obsolète.  
  
Windows Server 2012 introduit une nouvelle applet de commande ADDSDeployment pour créer des comptes d'ordinateurs de contrôleur de domaine en lecture seule intermédiaires, mais l'Assistant ne l'utilise pas pour son opération. Les sections suivantes affichent l'applet de commande et les arguments équivalents pour faciliter la compréhension des informations associées.  
  
Le lien **pré-créer un compte de contrôleur de domaine en lecture seule** dans le volet des tâches de centre d’administration Active Directory est équivalent à l’applet de commande Windows PowerShell ADDSDeployment :  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
### <a name="welcome"></a>Bienvenue  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)  
  
La boîte de dialogue **Assistant Installation des services de domaine Active Directory** contient une option nommée **Utiliser l'installation en mode avancé**. Cochez cette case et cliquez sur **Suivant** pour afficher des options de stratégie de réplication de mot de passe. Décochez cette case pour utiliser les valeurs par défaut pour les options de stratégie de réplication de mot de passe (cela est décrit plus en détail plus loin dans cette section).  
  
### <a name="network-credentials"></a>Informations d'identification réseau  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)  
  
L'option de nom de domaine dans la boîte de dialogue **Informations d'identification réseau** affiche le domaine ciblé par le Centre d'administration Active Directory par défaut. Vos informations d'identification actuelles sont utilisées par défaut. Si elles ne comprennent pas l'appartenance au groupe Admins du domaine, cliquez sur **Autres informations d'identification**, puis sur **Définir** pour fournir à l'Assistant un nom d'utilisateur et un mot de passe d'un membre du groupe Admins du domaine.  
  
L'argument Windows PowerShell ADDSDeployment équivalent est le suivant :  
  
```  
-credential <pscredential>  
```  
  
Gardez à l'esprit que le système intermédiaire est un port direct de Windows Server 2008 R2 et ne fournit pas la nouvelle fonctionnalité Adprep. Si vous envisagez de déployer des comptes de contrôleur de domaine en lecture seule intermédiaires, vous devez d'abord déployer un contrôleur de domaine en lecture seule non intermédiaire dans ce domaine pour que l'opération rodcprep automatique s'exécute, ou commencer par exécuter manuellement adprep.exe /rodcprep.  
  
Sinon, l'erreur « Vous ne pourrez pas installer un contrôleur de domaine en lecture seule dans ce domaine, car « adprep /rodcprep » n'a pas encore été exécuté. » s'affiche.  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)  
  
### <a name="specify-the-computer-name"></a>Spécifiez le nom de l'ordinateur  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)  
  
La boîte de dialogue **Spécifiez le nom de l'ordinateur** vous demande d'entrer le **Nom de l'ordinateur** en une partie d'un contrôleur de domaine qui n'existe pas. Le contrôleur de domaine que vous configurez et associez à ce compte par la suite doit avoir le même nom. À défaut, l'opération de promotion ne détecte pas le compte intermédiaire.  
  
L'argument Windows PowerShell ADDSDeployment équivalent est le suivant :  
  
```  
-domaincontrolleraccountname <string>  
```  
  
### <a name="select-a-site"></a>Sélectionner un site  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)  
  
La boîte de dialogue **Sélectionner un site** affiche une liste de sites Active Directory pour la forêt actuelle. L'opération du contrôleur de domaine en lecture seule intermédiaire vous demande de sélectionner un seul site dans la liste. Le contrôleur de domaine en lecture seule utilise ces informations pour créer son objet Paramètres NTDS dans la partition de configuration et se joint au site approprié quand il démarre pour la première fois après son déploiement.  
  
L'argument Windows PowerShell ADDSDeployment équivalent est le suivant :  
  
```  
-sitename <string>  
```  
  
### <a name="additional-domain-controller-options"></a>Options supplémentaires pour le contrôleur de domaine  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)  
  
La boîte de dialogue **Options supplémentaires du contrôleur de domaine** vous permet d'indiquer qu'un contrôleur de domaine inclut une exécution en tant que **Serveur DNS** et **Catalogue global**. Comme Microsoft recommande que les contrôleurs de domaine en lecture seule fournissent des services DNS et de catalogue global, les deux sont installés par défaut ; le rôle de contrôleur de domaine en lecture seule est également destiné aux filiales dans lesquelles le réseau étendu peut ne pas être disponible et, sans ces services DNS et de catalogue global, les ordinateurs de la filiale ne pourraient pas utiliser les fonctions et ressources des services de domaine Active Directory.  
  
L'option **Contrôleur de domaine en lecture seule (RODC)** est présélectionnée et ne peut pas être désactivée. Les arguments Windows PowerShell ADDSDeployment équivalents sont les suivants :  
  
```  
-installdns <string>  
-NoGlobalCatalog <{$true | $false}>  
  
```  
  
> [!NOTE]  
> Par défaut, la valeur **-NoGlobalCatalog** est $false, ce qui signifie que le contrôleur de domaine sera un serveur de catalogue global si l’argument n’est pas spécifié.  
  
### <a name="specify-the-password-replication-policy"></a>Spécifier la stratégie de réplication de mot de passe  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)  
  
La boîte de dialogue **Spécifier la stratégie de réplication de mot de passe** vous permet de modifier la liste par défaut de comptes qui sont autorisés à mettre en cache leurs mots de passe sur ce contrôleur de domaine en lecture seule. Les comptes de la liste configurés avec **Refuser** ou qui ne figurent pas dans la liste (implicites) ne mettent pas en cache leur mot de passe. Les comptes qui ne sont pas autorisés à mettre en cache leurs mots de passe sur le contrôleur de domaine en lecture seule et qui ne peuvent pas se connecter à un contrôleur de domaine accessible en écriture ni s'authentifier auprès de celui-ci ne sont pas en mesure d'accéder aux fonctions ni ressources fournies par Active Directory.  
  
> [!IMPORTANT]  
> L'Assistant affiche cette boîte de dialogue uniquement si vous cochez la case **Utiliser l'installation en mode avancé** sur l'écran d'accueil. Si vous décochez cette case, l'Assistant utilise les valeurs et les groupes par défaut suivants :  
>   
> -   Administrateurs - Refuser  
> -   Opérateurs de serveur - Refuser  
> -   Opérateurs de sauvegarde - Refuser  
> -   Opérateurs de compte - Refuser  
> -   Groupe de réplication dont le mot de passe RODC est refusé - Refuser  
> -   Groupe de réplication dont le mot de passe RODC est autorisé - Autoriser  
  
Les arguments Windows PowerShell ADDSDeployment équivalents sont les suivants :  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)  
  
### <a name="delegation-of-rodc-installation-and-administration"></a>Délégation de l'installation et de l'administration du contrôleur de domaine en lecture seule (RODC)  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)  
  
La boîte de dialogue **Délégation de l'installation et de l'administration du contrôleur de domaine en lecture seule (RODC)** vous permet de configurer un utilisateur ou un groupe contenant des utilisateurs qui sont autorisés à associer le serveur au compte d'ordinateur de contrôleur de domaine en lecture seule. Cliquez sur **Définir** pour rechercher un utilisateur ou un groupe dans le domaine. L'utilisateur ou le groupe spécifié dans cette boîte de dialogue obtient des autorisations d'administrateur local sur le contrôleur de domaine en lecture seule. L’utilisateur spécifié ou les membres du groupe spécifié peuvent effectuer des opérations sur le contrôleur de domaine en lecture seule avec des privilèges équivalents au groupe administrateurs de l’ordinateur. Ils ne sont *pas* membres du groupe Admins du domaine ni du groupe Administrateurs intégré au domaine.  
  
Utilisez cette option pour déléguer l'administration de filiale sans accorder à l'administrateur de filiale l'appartenance au groupe Admins du domaine. La délégation de l’administration du contrôleur de domaine en lecture seule n'est pas requise.  
  
L'argument Windows PowerShell ADDSDeployment équivalent est le suivant :  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
### <a name="summary"></a>Récapitulatif  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)  
  
La boîte de dialogue **Résumé** vous permet de confirmer vos paramètres. Il s'agit de la dernière possibilité d'arrêter l'installation avant que l'Assistant ne crée le compte intermédiaire. Cliquez sur **Suivant** quand vous êtes prêt à créer le compte d'ordinateur de contrôleur de domaine en lecture seule intermédiaire.  Cliquez sur **Exporter les paramètres** pour enregistrer un fichier de réponses au format de fichier d'installation sans assistance du processus dcpromo obsolète.  
  
### <a name="creation"></a>Création  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)  
  
L'**Assistant Installation des services de domaine Active Directory** crée le contrôleur de domaine en lecture seule intermédiaire dans Active Directory. Vous ne pouvez pas annuler cette opération une fois qu'elle a démarré.  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)  
  
Utilisez l'applet de commande suivante pour créer un compte d'ordinateur de contrôleur de domaine en lecture seule intermédiaire à l'aide du module Windows PowerShell ADDSDeployment :  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
Pour connaître les arguments requis et facultatifs, voir [Créer un contrôleur de domaine en lecture seule intermédiaire dans Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS).  
  
Comme l'applet de commande **Add-addsreadonlydomaincontrolleraccount** contient uniquement une action avec deux phases (vérification de la configuration requise et installation), les captures d'écran suivantes illustrent la phase d'installation avec les arguments requis minimaux.  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)  
  
L'opération de création d'un contrôleur de domaine en lecture seule intermédiaire crée le compte d'ordinateur de contrôleur de domaine en lecture seule dans Active Directory. Le Centre d'administration Active Directory affiche le **Type de contrôleur de domaine** comme étant un **Compte de contrôleur de domaine inoccupé**. Ce type de contrôleur de domaine indique que le compte de contrôleur de domaine en lecture seule intermédiaire est prêt à être associé à un serveur comme contrôleur de domaine en lecture seule.  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)  
  
> [!IMPORTANT]  
> Le Centre d'administration Active Directory n'a plus à associer un serveur à un compte d'ordinateur de contrôleur de domaine en lecture seule. Utilisez le Gestionnaire de serveur et l'Assistant Configuration des services de domaine Active Directory ou l'applet de commande du module Windows PowerShell ADDSDeployment **Install-AddsDomainController** pour associer un nouveau contrôleur de domaine en lecture seule à son compte intermédiaire. Les étapes sont semblables à l'ajout d'un nouveau contrôleur de domaine accessible en écriture à un domaine existant, excepté que le compte d'ordinateur de contrôleur de domaine en lecture seule intermédiaire contient des options de configuration choisies au moment de la création du compte d'ordinateur de contrôleur de domaine en lecture seule intermédiaire.  
  
## <a name="attaching"></a>Association  
  
### <a name="deployment-configuration"></a>Configuration du déploiement  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Le Gestionnaire de serveur commence la promotion de chaque contrôleur de domaine par la page **Configuration de déploiement** . Dans cette page et les pages suivantes, les options disponibles et les champs requis varient selon l’opération de déploiement que vous sélectionnez.  
  
Pour ajouter un contrôleur de domaine en lecture seule à un domaine existant, sélectionnez **Ajouter un contrôleur de domaine à un domaine existant** et cliquez sur le bouton **Sélectionner** associé à **Spécifiez les informations de domaine pour cette opération**. Le Gestionnaire de serveur vous invite automatiquement à entrer des informations d'identification valides, ou vous pouvez cliquer sur **Modifier**.  
  
L'association d'un contrôleur de domaine en lecture seule requiert l'appartenance au groupe Admins du domaine dans Windows Server 2012. L’Assistant Configuration des services de domaine Active Directory vous avertit par la suite si vos informations d’identification actuelles ne comptent pas les autorisations ou les appartenances aux groupes appropriées.  
  
L'applet de commande Windows PowerShell ADDSDeployment **Configuration de déploiement** et les arguments sont les suivants :  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)  
  
La page **Options du contrôleur de domaine** affiche les options du contrôleur de domaine pour le nouveau contrôleur de domaine. Quand cette page est chargée, l'Assistant Configuration des services de domaine Active Directory envoie une requête LDAP à un contrôleur de domaine existant pour rechercher les comptes inoccupés. Si la requête trouve un compte d’ordinateur de contrôleur de domaine inoccupé qui partage le même nom que l’ordinateur actuel, l’Assistant affiche un message d’information en haut de la page qui indique «**A compte RODC précréé qui correspond au nom de la le serveur cible existe dans le répertoire. Choisissez d’utiliser ce compte RODC existant ou de réinstaller ce contrôleur de domaine @ no__t-0.» L'Assistant utilise l'option **Utiliser le compte RODC existant** comme configuration par défaut.  
  
> [!IMPORTANT]  
> Vous pouvez utiliser l'option **Réinstaller ce contrôleur de domaine** quand un contrôleur de domaine a rencontré un problème physique et ne peut plus fonctionner. Vous gagnez du temps quand vous configurez le contrôleur de domaine de remplacement en laissant le compte d'ordinateur de contrôleur de domaine et les métadonnées d'objet dans Active Directory. Installez le nouvel ordinateur avec le *même nom*et promouvez-le comme contrôleur du domaine. L’option **réinstaller ce contrôleur de domaine** n’est pas disponible si vous avez supprimé les métadonnées de l’objet contrôleur de domaine Active Directory (nettoyage des métadonnées).  
  
Vous ne pouvez pas configurer des options de contrôleur de domaine quand vous associez un serveur à un compte d'ordinateur de contrôleur de domaine en lecture seule. Vous configurez des options de contrôleur de domaine quand vous créez le compte d'ordinateur de contrôleur de domaine en lecture seule intermédiaire.  
  
Le **Mot de passe du mode de restauration des services d'annuaire** spécifié doit respecter la stratégie de mot de passe appliquée au serveur. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète.  
  
Les arguments Windows PowerShell ADDSDeployment **Options du contrôleur de domaine** sont les suivants :  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Le nom du site doit déjà exister s’il est fourni en tant qu’argument à **-sitename**. L'applet de commande **install-AddsDomainController** ne crée pas de nom de site. Vous pouvez utiliser l'applet de commande **new-adreplicationsite** pour créer des sites.  
  
Les arguments **Install-ADDSDomainController** suivent les mêmes paramètres par défaut que le Gestionnaire de serveur s'ils ne sont pas spécifiés.  
  
Le fonctionnement de l'argument **SafeModeAdministratorPassword** est spécial :  
  
-   S'ils ne sont *pas spécifiés* en tant qu'arguments, l'applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer un contrôleur de domaine en lecture seule dans le domaine corp.contoso.com et être invité à entrer et à confirmer un mot de passe masqué :  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
> Il n'est pas recommandé de fournir ni de stocker un mot de passe en texte clair ou obfusqué. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe DSRM de ce contrôleur de domaine.  Toute personne ayant accès au fichier peut annuler ce mot de passe obfusqué. Munie de cette information, elle peut ouvrir une session sur un contrôleur de domaine en mode DSRM et finir par emprunter l'identité du contrôleur de domaine lui-même, en élevant ses privilèges au niveau le plus élevé d'une forêt Active Directory. Une autre procédure utilisant **System.Security.Cryptography** pour chiffrer les données du fichier texte est conseillée, mais n'est pas traitée ici. Le mieux est d'éviter totalement tout stockage de mot de passe.  
  
### <a name="additional-options"></a>Autres options  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)  
  
La page **Options supplémentaires** offre des options de configuration permettant de nommer un contrôleur de domaine en tant que source de réplication ; vous pouvez aussi utiliser n'importe quel contrôleur de domaine comme source de réplication.  
  
Vous pouvez également choisir d’installer le contrôleur de domaine à partir d’un support sauvegardé à l’aide de l’option Installation à partir du support (IFM). La case **Installation à partir du support** fournit une option de navigation quand elle est cochée et vous devez cliquer sur **Vérifier** pour garantir que le chemin d'accès indiqué est un support valide.

Les instructions relatives à la source IFM : • support utilisé par l’option IFM sont créées avec Sauvegarde Windows Server ou Ntdsutil. exe à partir d’un autre contrôleur de domaine Windows Server existant avec la même version du système d’exploitation uniquement. Par exemple, vous ne pouvez pas utiliser un système d’exploitation Windows Server 2008 R2 ou antérieur pour créer un média pour un contrôleur de domaine Windows Server 2012.
• Les données sources de l’IFM doivent provenir d’un contrôleur de domaine accessible en écriture. Alors qu’une source à partir de RODC travaillera techniquement pour créer un nouveau RODC, il existe des avertissements de réplication positifs erronés que le RODC source IFM ne réplique pas.

Pour plus d'informations sur les modifications apportées à l'option Installation à partir du support, voir [Modifications apportées à l'option Installation à partir du support avec Ntdsutil.exe](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). En cas d'utilisation du support protégé avec SYSKEY, le Gestionnaire de serveur vous invite à entrer le mot de passe de l'image pendant la vérification. 
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)  
  
Les arguments de l'applet de commande ADDSDeployment **Options supplémentaires** sont les suivants :  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Chemins d’accès  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)  
  
La page **Chemins d’accès** vous permet de remplacer les emplacements de dossier par défaut de la base de données AD DS, des journaux de transaction de base de données et du partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de %systemroot%. Les arguments de l'applet de commande ADDSDeployment **Chemins d'accès** sont les suivants :  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Examiner les options et Afficher le script  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)  
  
La page **Examiner les options** vous permet de valider vos paramètres et de vérifier qu’ils répondent à vos exigences avant le démarrage de l’installation. Notez que vous avez encore la possibilité d’arrêter l’installation à l’aide du Gestionnaire de serveur. Cette page vous permet simplement d’examiner et de confirmer vos paramètres avant de poursuivre la configuration. La page **Examiner les options** du Gestionnaire de serveur offre également un bouton **Afficher le script** facultatif pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme d’un script Windows PowerShell unique. Vous pouvez ainsi utiliser l’interface graphique Gestionnaire de serveur sous forme d’un studio de déploiement Windows PowerShell. Utilisez l’Assistant Configuration des services de domaine Active Directory pour configurer les options, exportez la configuration, puis annulez l’Assistant. Ce processus crée un exemple valide et correct du point de vue syntaxique pour permettre des modifications ultérieures ou une utilisation directe. Exemple :  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "corp.contoso.com" `  
-LogPath "C:\Windows\NTDS" `  
-SYSVOLPath "C:\Windows\SYSVOL" `  
-UseExistingAccount:$true `  
-Norebootoncompletion:$false  
-Force:$true  
  
```  
  
> [!NOTE]  
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s'appuie pas sur les valeurs par défaut (car elles peuvent changer entre les futures versions de Windows ou les Service Packs). La seule exception concerne l'argument **-safemodeadministratorpassword** . Pour forcer une demande de confirmation, omettez la valeur en cas d'exécution interactive de l'applet de commande.  
  
Utilisez l'argument **Whatif** facultatif avec l'applet de commande **Install-ADDSDomainController** pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d'une applet de commande.  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)  
  
### <a name="prerequisites-check"></a>Vérification de la configuration requise  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)  
  
La fonctionnalité **Vérification de la configuration requise** est nouvelle dans la configuration de domaine AD DS. Cette nouvelle phase valide que la configuration du serveur est capable de prendre en charge une nouvelle forêt AD DS.  
  
Pendant l'installation d'un nouveau domaine racine de forêt, l'Assistant Configuration des services de domaine Active Directory du Gestionnaire de serveur appelle une série de tests modulaires sérialisés. Ces tests vous alertent avec des suggestions d'opérations de réparation. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus d'installation du contrôleur de domaine ne peut pas continuer tant que tous les tests de configuration requise ne sont pas réussis.  
  
La fonctionnalité **Vérification de la configuration requise** met également en évidence des informations pertinentes, telles que les modifications de sécurité qui affectent les systèmes d'exploitation plus anciens. Pour plus d'informations sur les vérifications de la configuration requise, voir [Vérification de la configuration requise](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Vous ne pouvez pas ignorer la fonctionnalité **Vérification de la configuration requise** quand vous utilisez le Gestionnaire de serveur, mais vous pouvez ignorer le processus quand vous utilisez l'applet de commande de déploiement des services AD DS avec l'argument suivant :  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft déconseille d'ignorer la vérification de la configuration requise, car cela peut aboutir à une promotion partielle du contrôleur de domaine ou à l'endommagement de la forêt AD DS.  
  
Cliquez sur **Installer** pour commencer le processus de promotion du contrôleur de domaine. Il s'agit de la dernière possibilité d'annuler l'installation. Vous ne pouvez pas annuler le processus de promotion une fois qu'il a commencé. L'ordinateur redémarre automatiquement à la fin de la promotion, quel qu'en soit le résultat.  
  
### <a name="installation"></a>Installation  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)  
  
Quand la page Installation s'affiche, la configuration du contrôleur de domaine commence et ne peut pas être arrêtée ni annulée. Les opérations détaillées s'affichent dans cette page et sont écrites dans les journaux :  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Pour installer une nouvelle forêt Active Directory à l'aide du module ADDSDeployment, utilisez l'applet de commande suivante :  
  
```  
Install-addsdomaincontroller  
  
```  
  
Voir [Attach RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS) pour les arguments obligatoires et facultatifs.  
  
L'applet de commande **Install-addsdomaincontroller** comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d'installation avec les arguments requis minimaux **-domainname**, **-useexistingaccount**et **-credential**. Notez comment, de la même façon que le Gestionnaire de serveur, **Install-ADDSDomainController** vous rappelle que la promotion redémarre automatiquement le serveur :  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)  
  
Pour accepter automatiquement l'invite de redémarrage, utilisez les arguments **-force** ou **-confirm:$false** avec n'importe quelle applet de commande Windows PowerShell ADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez l'argument **-norebootoncompletion**.  
  
> [!WARNING]  
> Il n'est pas conseillé de remplacer le redémarrage. Le contrôleur de domaine doit redémarrer pour fonctionner correctement.  
  
### <a name="results"></a>Résultats  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La page **Résultats** indique la réussite ou l'échec de la promotion et toute information d'administration importante. Le contrôleur de domaine redémarre automatiquement après 10 secondes.  
  
## <a name="rodc-without-staging-workflow"></a>Workflow de contrôleur de domaine en lecture seule sans création intermédiaire  
Le diagramme suivant illustre le processus de configuration des services de domaine Active Directory quand vous avez auparavant installé le rôle AD DS et démarré l'Assistant Configuration des services de domaine Active Directory à l'aide du Gestionnaire de serveur pour créer un contrôleur de domaine en lecture seule non intermédiaire dans un domaine Windows Server 2012 existant.  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)  
  
## <a name="rodc-without-staging-windows-powershell"></a>Contrôleur de domaine en lecture seule sans création intermédiaire dans Windows PowerShell  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (les arguments en **gras** sont obligatoires. Les arguments en *italique* peuvent être spécifiés à l'aide de Windows PowerShell ou de l'Assistant Configuration des services de domaine Active Directory.)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />***-SiteName***<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />*-CriticalReplicationOnly*<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-DNSOnNetwork<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />***-ReadOnlyReplica***|  
  
> [!NOTE]  
> L'argument **-credential** est uniquement requis si vous n'êtes pas déjà connecté en tant que membre du groupe Admins du domaine.  
  
## <a name="rodc-without-staging-deployment"></a>Déploiement de contrôleur de domaine en lecture seule sans création intermédiaire  
  
### <a name="deployment-configuration"></a>Configuration du déploiement  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Le Gestionnaire de serveur commence la promotion de chaque contrôleur de domaine par la page **Configuration de déploiement** . Dans cette page et les pages suivantes, les options disponibles et les champs requis varient selon l’opération de déploiement que vous sélectionnez.  
  
Pour ajouter un contrôleur de domaine en lecture seule non intermédiaire à un domaine Windows Server 2012 existant, sélectionnez **Ajouter un contrôleur de domaine à un domaine existant** et cliquez sur le bouton **Sélectionner** associé à **Spécifiez les informations de domaine pour cette opération**. Le Gestionnaire de serveur vous invite automatiquement à entrer des informations d'identification valides, ou vous pouvez cliquer sur **Modifier**.  
  
L'association d'un contrôleur de domaine en lecture seule requiert l'appartenance au groupe Admins du domaine dans Windows Server 2012. L’Assistant Configuration des services de domaine Active Directory vous avertit par la suite si vos informations d’identification actuelles ne comptent pas les autorisations ou les appartenances aux groupes appropriées.  
  
L'applet de commande Windows PowerShell ADDSDeployment **Configuration de déploiement** et les arguments sont les suivants :  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)  
  
La page **Options du contrôleur de domaine** spécifie les fonctions du contrôleur de domaine pour le nouveau contrôleur de domaine. Les fonctions du contrôleur de domaine configurables sont **Serveur DNS**, **Catalogue global**et **Contrôleur de domaine en lecture seule**. Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et de catalogue global à des fins de haute disponibilité dans les environnements distribués. Le catalogue global est toujours sélectionné par défaut et le serveur DNS est sélectionné par défaut si le domaine actuel héberge déjà DNS sur ses contrôleurs de domaine, selon une requête SOA (Start-of-Authority).  
  
La page **Options du contrôleur de domaine** vous permet également de choisir le **nom de site** logique Active Directory approprié à partir de la configuration de la forêt. Par défaut, le sous-réseau le mieux adapté est sélectionné. S’il n’y a qu’un site, celui-ci est automatiquement sélectionné.  
  
> [!IMPORTANT]  
> Si le serveur n'appartient pas à un sous-réseau Active Directory et qu'il existe plusieurs sites Active Directory, rien n'est sélectionné et le bouton **Suivant** n'apparaît que quand vous avez choisi un site dans la liste.  
  
Le **Mot de passe du mode de restauration des services d'annuaire** spécifié doit respecter la stratégie de mot de passe appliquée au serveur. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète. Les arguments Windows PowerShell ADDSDeployment **Options du contrôleur de domaine** sont les suivants :  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Le nom du site doit déjà exister s’il est fourni en tant qu’argument à **-sitename**. L'applet de commande **install-AddsDomainController** ne crée pas de nom de site. Vous pouvez utiliser l'applet de commande **new-adreplicationsite** pour créer des sites.  
  
Les arguments **Install-ADDSDomainController** suivent les mêmes paramètres par défaut que le Gestionnaire de serveur s'ils ne sont pas spécifiés.  
  
Le fonctionnement de l'argument **SafeModeAdministratorPassword** est spécial :  
  
-   S'ils ne sont *pas spécifiés* en tant qu'arguments, l'applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer un contrôleur de domaine en lecture seule dans le domaine corp.contoso.com et être invité à entrer et à confirmer un mot de passe masqué :  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
> Il n'est pas recommandé de fournir ni de stocker un mot de passe en texte clair ou obfusqué. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe DSRM de ce contrôleur de domaine.  Toute personne ayant accès au fichier peut annuler ce mot de passe obfusqué. Munie de cette information, elle peut ouvrir une session sur un contrôleur de domaine en mode DSRM et finir par emprunter l'identité du contrôleur de domaine lui-même, en élevant ses privilèges au niveau le plus élevé d'une forêt Active Directory. Une autre procédure utilisant **System.Security.Cryptography** pour chiffrer les données du fichier texte est conseillée, mais n'est pas traitée ici. Le mieux est d'éviter totalement tout stockage de mot de passe.  
  
### <a name="rodc-options"></a>Options RODC  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)  
  
La page **Options RODC** vous permet de modifier les paramètres :  
  
-   Compte d'administrateur délégué  
  
-   Comptes autorisés à répliquer les mots de passe pour RODC  
  
-   Comptes non autorisés à répliquer les mots de passe pour RODC  
  
Les comptes d’administrateurs délégués se voient octroyer des autorisations administratives locales au contrôleur de domaine en lecture seule. Ces utilisateurs peuvent utiliser des privilèges équivalents au groupe administrateurs de l’ordinateur local.  Ils ne sont membres ni du groupe Admins du domaine ni du groupe Administrateurs intégré au domaine. Cette option est utile pour déléguer l’administration de filiales sans octroyer d’autorisations administratives au domaine. La configuration de la délégation de l’administration n’est pas requise.  
  
L'argument Windows PowerShell ADDSDeployment équivalent est le suivant :  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
Les comptes qui ne sont pas autorisés à mettre en cache leurs mots de passe sur le contrôleur de domaine en lecture seule et qui ne peuvent pas se connecter à un contrôleur de domaine accessible en écriture ni s'authentifier auprès de celui-ci ne sont pas en mesure d'accéder aux fonctions ni ressources fournies par Active Directory.  
  
> [!IMPORTANT]  
> En l'absence de modification, les paramètres et groupes par défaut sont utilisés :  
>   
> -   Administrateurs - Refuser  
> -   Opérateurs de serveur - Refuser  
> -   Opérateurs de sauvegarde - Refuser  
> -   Opérateurs de compte - Refuser  
> -   Groupe de réplication dont le mot de passe RODC est refusé - Refuser  
> -   Groupe de réplication dont le mot de passe RODC est autorisé - Autoriser  
  
Les arguments Windows PowerShell ADDSDeployment équivalents sont les suivants :  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)  
  
### <a name="additional-options"></a>Autres options  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)  
  
La page **Options supplémentaires** offre des options de configuration permettant de nommer un contrôleur de domaine en tant que source de réplication ; vous pouvez aussi utiliser n'importe quel contrôleur de domaine comme source de réplication.  
  
Vous pouvez également choisir d’installer le contrôleur de domaine à partir d’un support sauvegardé à l’aide de l’option Installation à partir du support (IFM). La case **Installation à partir du support** fournit une option de navigation quand elle est cochée et vous devez cliquer sur **Vérifier** pour garantir que le chemin d'accès indiqué est un support valide.

Les instructions relatives à la source IFM : • support utilisé par l’option IFM sont créées avec Sauvegarde Windows Server ou Ntdsutil. exe à partir d’un autre contrôleur de domaine Windows Server existant avec la même version du système d’exploitation uniquement. Par exemple, vous ne pouvez pas utiliser un système d’exploitation Windows Server 2008 R2 ou antérieur pour créer un média pour un contrôleur de domaine Windows Server 2012.
• Les données sources de l’IFM doivent provenir d’un contrôleur de domaine accessible en écriture. Alors qu’une source à partir de RODC travaillera techniquement pour créer un nouveau RODC, il existe des avertissements de réplication positifs erronés que le RODC source IFM ne réplique pas.

Pour plus d'informations sur les modifications apportées à l'option Installation à partir du support, voir [Modifications apportées à l'option Installation à partir du support avec Ntdsutil.exe](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). En cas d'utilisation du support protégé avec SYSKEY, le Gestionnaire de serveur vous invite à entrer le mot de passe de l'image pendant la vérification.
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)  
  
Les arguments de l’applet de commande ADDSDeployment Options supplémentaires sont les suivants :  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Chemins d’accès  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)  
  
La page **Chemins d’accès** vous permet de remplacer les emplacements de dossier par défaut de la base de données AD DS, des journaux de transaction de base de données et du partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de %systemroot%. Les arguments de l'applet de commande ADDSDeployment **Chemins d'accès** sont les suivants :  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Options de préparation  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)  
  
La page **Options de préparation** vous informe que la configuration des services de domaine Active Directory comprend l'extension du schéma (forestprep) et la mise à jour du domaine (domainprep). Cette page ne s'affiche que quand la forêt ou le domaine n'a pas été préparé par une précédente installation de contrôleur de domaine Windows Server 2012 ni par une exécution manuelle d'Adprep.exe. Par exemple, l'Assistant Configuration des services de domaine Active Directory supprime cette page si vous ajoutez un nouveau contrôleur de domaine répliqué à un domaine racine de forêt Windows Server 2012 existant.  
  
L'extension du schéma et la mise à jour du domaine ne se produisent pas quand vous cliquez sur **Suivant**. Ces événements se produisent uniquement pendant la phase d'installation. Cette page vous informe simplement des événements qui vont se produire plus tard pendant l'installation.  
  
Cette page valide également que les informations d'identification de l'utilisateur actuel sont membres des groupes Administrateurs de l'entreprise et Administrateurs du schéma, car l'appartenance à ces groupes est nécessaire pour étendre le schéma ou préparer un domaine. Cliquez sur **Modifier** pour fournir des informations d'identification d'utilisateur appropriées si la page vous indique que les informations d'identification actuelles n'offrent pas des autorisations suffisantes.  
  
L’argument de l’applet de commande ADDSDeployment Options supplémentaires est le suivant :  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Comme avec les versions précédentes de Windows Server, la préparation du domaine automatisée de Windows Server 2012 n'exécute pas GPPREP. Exécutez **adprep.exe /gpprep** manuellement pour tous les domaines qui n'étaient pas auparavant préparés pour Windows Server 2003, Windows Server 2008 ni Windows Server 2008 R2. Vous ne devez exécuter GPPrep qu'une seule fois dans l'historique d'un domaine et non avec chaque mise à niveau. Adprep.exe n'exécute pas /gpprep automatiquement, car l'opération peut provoquer une nouvelle réplication de tous les fichiers et dossiers dans le dossier SYSVOL sur tous les contrôleurs de domaine.  
>   
> La commande RODCPrep automatique est exécutée quand vous promouvez le premier contrôleur de domaine en lecture seule non intermédiaire dans un domaine. Elle n'est pas exécutée quand vous promouvez le premier contrôleur de domaine Windows Server 2012 accessible en écriture. Vous pouvez aussi exécuter manuellement **adprep.exe /rodcprep** si vous envisagez de déployer des contrôleurs de domaine en lecture seule.  
  
### <a name="review-options-and-view-script"></a>Examiner les options et Afficher le script  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)  
  
La page **Examiner les options** vous permet de valider vos paramètres et de vérifier qu’ils répondent à vos exigences avant le démarrage de l’installation. Notez que vous avez encore la possibilité d’arrêter l’installation à l’aide du Gestionnaire de serveur. Cette page vous permet simplement d’examiner et de confirmer vos paramètres avant de poursuivre la configuration.  
  
La page **Examiner les options** du Gestionnaire de serveur offre également un bouton **Afficher le script** facultatif pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme d’un script Windows PowerShell unique. Vous pouvez ainsi utiliser l’interface graphique Gestionnaire de serveur sous forme d’un studio de déploiement Windows PowerShell. Utilisez l’Assistant Configuration des services de domaine Active Directory pour configurer les options, exportez la configuration, puis annulez l’Assistant. Ce processus crée un exemple valide et correct du point de vue syntaxique pour permettre des modifications ultérieures ou une utilisation directe. Exemple :  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-AllowPasswordReplicationAccountName @("CORP\Allowed RODC Password Replication Group", "CORP\Chicago RODC Admins", "CORP\Chicago RODC Users and Computers") `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DelegatedAdministratorAccountName "CORP\Chicago RODC Admins" `  
-DenyPasswordReplicationAccountName @("BUILTIN\Administrators", "BUILTIN\Server Operators", "BUILTIN\Backup Operators", "BUILTIN\Account Operators", "CORP\Denied RODC Password Replication Group") `  
-DomainName "corp.contoso.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-ReadOnlyReplica:$true `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s'appuie pas sur les valeurs par défaut (car elles peuvent changer entre les futures versions de Windows ou les Service Packs). La seule exception concerne l'argument **-safemodeadministratorpassword** . Pour forcer une demande de confirmation, omettez la valeur en cas d'exécution interactive de l'applet de commande.  
  
Utilisez l’argument Whatif facultatif avec l’applet de commande Install-ADDSDomainController pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d'une applet de commande.  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)  
  
### <a name="prerequisites-check"></a>Vérification de la configuration requise  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)  
  
La fonctionnalité **Vérification de la configuration requise** est nouvelle dans la configuration de domaine AD DS. Cette nouvelle phase valide que la configuration du serveur est capable de prendre en charge une nouvelle forêt AD DS.  
  
Pendant l'installation d'un nouveau domaine racine de forêt, l'Assistant Configuration des services de domaine Active Directory du Gestionnaire de serveur appelle une série de tests modulaires sérialisés. Ces tests vous alertent avec des suggestions d'opérations de réparation. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus du contrôleur de domaine ne peut pas continuer tant que tous les tests de configuration requise ne sont pas réussis.  
  
La fonctionnalité **Vérification de la configuration requise** met également en évidence des informations pertinentes, telles que les modifications de sécurité qui affectent les systèmes d'exploitation plus anciens.  
  
Vous ne pouvez pas ignorer la fonctionnalité **Vérification de la configuration requise** quand vous utilisez le Gestionnaire de serveur, mais vous pouvez ignorer le processus quand vous utilisez l'applet de commande de déploiement des services AD DS avec l'argument suivant :  
  
```  
-skipprechecks  
  
```  
  
Cliquez sur **Installer** pour commencer le processus de promotion du contrôleur de domaine. Il s'agit de la dernière possibilité d'annuler l'installation. Vous ne pouvez pas annuler le processus de promotion une fois qu'il a commencé. L'ordinateur redémarre automatiquement à la fin de la promotion, quel qu'en soit le résultat.  
  
### <a name="installation"></a>Installation  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)  
  
Lorsque la page **Installation** s'affiche, la configuration du contrôleur de domaine commence et ne peut pas être arrêtée ni annulée. Les opérations détaillées s'affichent dans cette page et sont écrites dans les journaux :  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Pour installer une nouvelle forêt Active Directory à l'aide du module ADDSDeployment, utilisez l'applet de commande suivante :  
  
```  
Install-addsdomaincontroller  
  
```  
  
Pour connaître les arguments requis et facultatifs, voir le tableau **Applet de commande ADDSDeployment** au début de cette section.  
  
L'applet de commande **Install-addsdomaincontroller** comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d'installation avec les arguments requis minimaux **-domainname**, **-readonlyreplica**, **-sitename** et **-credential**. Notez comment, de la même façon que le Gestionnaire de serveur, **Install-ADDSDomainController** vous rappelle que la promotion redémarre automatiquement le serveur :  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)  
  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)  
  
Pour accepter automatiquement l'invite de redémarrage, utilisez les arguments **-force** ou **-confirm:$false** avec n'importe quelle applet de commande Windows PowerShell ADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez l'argument **-norebootoncompletion**.  
  
> [!WARNING]  
> Il n'est pas recommandé de remplacer le redémarrage. Le contrôleur de domaine doit redémarrer pour fonctionner correctement. Si vous fermez la session du contrôleur de domaine, vous ne pouvez pas la rouvrir de façon interactive tant que vous ne l'avez pas redémarré.  
  
### <a name="results"></a>Résultats  
![Installer le contrôleur de domaine en lecture seule](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)  
  
La page **Résultats** indique la réussite ou l'échec de la promotion et toute information d'administration importante. Le contrôleur de domaine redémarre automatiquement après 10 secondes.  
  

