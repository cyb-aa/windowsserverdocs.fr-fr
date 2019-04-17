---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: "Installer un serveur Windows Server2012 ActiveDirectory Read-Only le contrôleur de domaine (RODC) (niveau 200)"
description: "Cette rubrique explique comment créer un compte RODC intermédiaire, puis associer un serveur à ce compte lors de l’installation RODC. Cette rubrique explique également comment installer un RODC sans effectuer d’installation intermédiaire."
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 78281ca845f79955aaa25aa45394284c59e639cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>Installer un serveur Windows Server2012 ActiveDirectory Read-Only le contrôleur de domaine (RODC) (niveau 200)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique explique comment créer un compte RODC intermédiaire, puis associer un serveur à ce compte lors de l’installation RODC. Cette rubrique explique également comment installer un RODC sans effectuer d’installation intermédiaire.  
  
## <a name="stage-rodc-workflow"></a>Flux de travail stage RODC  
Un intermédiaire lire uniquement contrôleur de domaine (RODC) installation fonctionne en deux phases discrètes:  
  
1.  Organisation d’un compte d’ordinateur inoccupé  
  
2.  Attachement d’un RODC à ce compte pendant la promotion  
  
Le diagramme suivant illustre les Services de domaine ActiveDirectory Read-Only le contrôleur de domaine intermédiaire des processus, où vous créez un compte d’ordinateur RODC vide dans le domaine à l’aide de la (Dsac.exe) Centre d’administration ActiveDirectory.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)  
  
## <a name="BKMK_StagePS"></a>Stage RODC Windows PowerShell  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (**gras** arguments sont requis. *En italique* arguments peuvent être spécifiés à l’aide de Windows PowerShell ou l’Assistant de Configuration ADDS.)|  
|Add-addsreadonlydomaincontrolleraccount|-SkipPreChecks<br /><br />***-DomainControllerAccountName***<br /><br />***-DomainName***<br /><br />***-SiteName***<br /><br />*AllowPasswordReplicationAccountName-*<br /><br />***-Credential***<br /><br />*DelegatedAdministratorAccountName-*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />*-NoGlobalCatalog*<br /><br />*-InstallDNS*<br /><br />ReplicationSourceDC-|  
  
> [!NOTE]  
> Le **-credential** argument est uniquement requis si vous ne sont pas déjà connecté en tant que membre du groupe Admins du domaine.  
  
## <a name="attach-rodc-workflow"></a>Joindre des flux de travail RODC  
Le diagramme ci-dessous illustre le processus de configuration des Services de domaine ActiveDirectory, où vous avez déjà installé le rôle ADDS, vous avez préparé le compte RODC et démarré **promouvoir ce serveur en contrôleur de domaine** à l’aide du Gestionnaire de serveur pour créer un nouveau RODC dans un domaine existant, en associant au compte d’ordinateur intermédiaire.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)  
  
## <a name="BKMK_AttachPS"></a>Attacher RODC Windows PowerShell  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (**gras** arguments sont requis. *En italique* arguments peuvent être spécifiés à l’aide de Windows PowerShell ou l’Assistant de Configuration ADDS.)|  
|Install-AddsDomaincontroller|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*ApplicationPartitionsToReplicate-*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />*-InstallationMediaPath*<br /><br />*-LogPath*<br /><br />-Norebootoncompletion<br /><br />*ReplicationSourceDC-*<br /><br />*SystemKey-*<br /><br />*-SYSVOLPath*<br /><br />***-UseExistingAccount***|  
  
> [!NOTE]  
> Le **-credential** argument est uniquement requis si vous ne sont pas déjà connecté en tant que membre du groupe Admins du domaine.  
  
## <a name="staging"></a>Mise en attente  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)  
  
Vous effectuez l’opération de transfert d’un compte ordinateur de contrôleur de domaine en lecture seule en ouvrant le centre d’administration ActiveDirectory (**Dsac.exe**). Cliquez sur le nom du domaine dans le volet de navigation. Double-cliquez sur **contrôleurs de domaine** dans la liste de gestion. Cliquez sur **créer au préalable un Read-only le compte de contrôleur de domaine** dans le volet des tâches.  
  
Pour plus d’informations sur le centre d’administration ActiveDirectory, voir [avancé ADDS gestion à l’aide de ActiveDirectory Administrative Center et #40; niveau 200 et #41; ](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)et passez en revue [centre d’administration ActiveDirectory: prise en main](https://technet.microsoft.com/library/dd560651(WS.10).aspx).  
  
Si vous avez déjà créé des contrôleurs de domaine en lecture seule, vous allez découvrir que l’Assistant installation a la même interface graphique que celle affichée à l’aide la plus anciens utilisateurs ActiveDirectory et le composant logiciel enfichable ordinateurs à partir de Windows Server2008 et utilise le même code, qui comprend l’exportation de la configuration dans le format de fichier d’installation sans assistance utilisé par le processus dcpromo obsolète.  
  
Windows Server2012 introduit une nouvelle applet de commande ADDSDeployment aux comptes d’ordinateurs stage RODC, mais l’Assistant n’utilise pas de l’applet de commande pour son opération. Les sections suivantes affichent l’applet de commande équivalente et les arguments afin de rendre les informations associées à chaque plus faciles à comprendre.  
  
Le **créer au préalable un Read-only le compte de contrôleur de domaine** lien volet du centre d’administration est équivalent à l’applet de commande WindowsPowerShellADDSDeployment:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
### <a name="welcome"></a>Bienvenue dans  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)  
  
Le **Bienvenue dans l’Assistant Installation des Services de domaine ActiveDirectory** boîte de dialogue comporte une option nommée **utilisez installation en mode avancé**. Sélectionnez cette option, cliquez sur **suivant** pour afficher les options de stratégie de réplication de mot de passe. Désactivez cette option pour utiliser les valeurs par défaut pour les options de stratégie de réplication de mot de passe (cela est décrit plus en détail plus loin dans cette section).  
  
### <a name="network-credentials"></a>Informations d’identification réseau  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)  
  
L’option de nom de domaine dans le **informations d’identification réseau** boîte de dialogue affiche le domaine ciblé par le centre d’administration ActiveDirectory par défaut. Vos informations d’identification actuelles sont utilisées par défaut. Si elles n’incluent pas d’appartenance dans le groupe Admins du domaine, cliquez sur **autres informations d’identification**, puis cliquez sur **définir** pour fournir un nom d’utilisateur et un mot de passe qui est membre du groupe Admins du domaine dans l’Assistant.  
  
L’argument WindowsPowerShellADDSDeployment équivalent est:  
  
```  
-credential <pscredential>  
```  
  
N’oubliez pas que le système intermédiaire est un port direct de Windows Server2008R2 et ne fournit pas de la nouvelle fonctionnalité Adprep. Si vous prévoyez de déployer les comptes de contrôleur nouvellement créés, vous devez tout d’abord déployer une seule non intermédiaire dans ce domaine pour que l’opération rodcprep automatique s’exécute, ou exécuter manuellement adprep.exe /rodcprep premier.  
  
Dans le cas contraire, vous recevez erreur «Vous ne serez pas être en mesure d’installer un contrôleur de domaine en lecture seule dans ce domaine, car «adprep /rodcprep» n’a pas été exécuté».  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)  
  
### <a name="specify-the-computer-name"></a>Spécifiez le nom d’ordinateur  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)  
  
Le **spécifier le nom d’ordinateur** boîte de dialogue vous demande d’entrer une partie **nom de l’ordinateur** d’un contrôleur de domaine qui n’existe pas. Le contrôleur de domaine, vous configurez et associez plus tard à ce compte doit avoir le même nom, ou l’opération de promotion ne détecte pas le compte intermédiaire.  
  
L’argument WindowsPowerShellADDSDeployment équivalent est:  
  
```  
-domaincontrolleraccountname <string>  
```  
  
### <a name="select-a-site"></a>Sélectionnez un Site  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)  
  
Le **sélectionner un Site** boîte de dialogue affiche une liste de sites ActiveDirectory pour la forêt actuelle. L’opération de contrôleur de domaine en lecture seule intermédiaire vous demande de sélectionner un seul site dans la liste. La seule utilise ces informations pour créer son objet Paramètres NTDS dans la partition de Configuration et se joint au site approprié quand il démarre pour la première fois après son déploiement.  
  
L’argument WindowsPowerShellADDSDeployment équivalent est:  
  
```  
-sitename <string>  
```  
  
### <a name="additional-domain-controller-options"></a>Options du contrôleur de domaine supplémentaire  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)  
  
Le **Options du contrôleur de domaine supplémentaire** boîte de dialogue permet de spécifier qu’un contrôleur de domaine inclut une exécution en tant qu’un **serveur DNS** et un **catalogue Global**. Microsoft recommande que les contrôleurs de domaine en lecture seule fournissent des services DNS et de catalogue global, afin que les deux sont installés par défaut; une intention du rôle RODC est les scénarios de succursale où le réseau étendu ne peut pas être disponible et sans ces DNS et les services de catalogue global, les ordinateurs de la filiale ne seront pas en mesure d’utiliser les fonctionnalités et ressources de domaine ActiveDirectory.  
  
Le **contrôleur de domaine en lecture seule (RODC)** option est sélectionnée au préalable et ne peut pas être désactivée. Les arguments WindowsPowerShellADDSDeployment équivalents sont:  
  
```  
-installdns <string>  
-NoGlobalCatalog <{$true | $false}>  
  
```  
  
> [!NOTE]  
> Par défaut, le **- NoGlobalCatalog** valeur est $false, ce qui signifie que le contrôleur de domaine sera un serveur de catalogue global si l’argument n’est pas spécifié.  
  
### <a name="specify-the-password-replication-policy"></a>Spécifier la stratégie de réplication de mot de passe  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)  
  
Le **spécifier la stratégie de réplication de mot de passe** boîte de dialogue permet de modifier la liste par défaut des comptes qui sont autorisés à mettre en cache leurs mots de passe sur ce contrôleur de domaine en lecture seule. Dans la liste configurés avec les comptes **refuser** ou qui ne figurent pas dans la liste (implicite) ne pas mettre en cache leur mot de passe. Comptes qui ne sont pas autorisés à des mots de passe du cache sur le RODC et ne peut pas se connecter et s’authentifier auprès d’un contrôleur de domaine accessible en écriture ne peut pas accéder aux ressources ou des fonctionnalités fournies par ActiveDirectory.  
  
> [!IMPORTANT]  
> L’Assistant affiche cette boîte de dialogue uniquement si vous sélectionnez le **utilisation avancée de l’Installation en Mode** case à cocher dans l’écran d’accueil. Si vous désactivez cette case à cocher, l’Assistant utilise les valeurs et les groupes par défaut:  
>   
> -   Administrateurs - refuser  
> -   Opérateurs de serveur - refuser  
> -   Opérateurs de sauvegarde - refuser  
> -   Opérateurs de compte - refuser  
> -   Refusé le groupe de réplication de mot de passe RODC - refuser  
> -   Autorisé le groupe de réplication de mot de passe RODC - autoriser  
  
Les arguments WindowsPowerShellADDSDeployment équivalents sont:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)  
  
### <a name="delegation-of-rodc-installation-and-administration"></a>Délégation de l’Administration et l’Installation RODC  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)  
  
Le **Installation de la délégation de contrôleur de domaine et l’Administration** boîte de dialogue permet de configurer un utilisateur ou un groupe contenant des utilisateurs qui sont autorisés à joindre le serveur au compte d’ordinateur RODC. Cliquez sur **définir** parcourir le domaine pour un utilisateur ou un groupe. L’utilisateur ou le groupe spécifié dans cette boîte de dialogue gains autorisations d’administrateur local sur le RODC. L’utilisateur spécifié ou les membres du groupe spécifié peuvent effectuer des opérations sur le RODC avec des privilèges équivalents au groupe Administrateurs de l’ordinateur. Ils sont *pas* les membres du groupe Administrateurs intégré au domaine ou Admins du domaine.  
  
Utilisez cette option pour déléguer l’administration de filiale sans accorder au groupe Admins du domaine de l’appartenance au groupe Administrateurs de branche. Délégation de l’administration RODC n’est pas nécessaire.  
  
L’argument WindowsPowerShellADDSDeployment équivalent est:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
### <a name="summary"></a>Résumé  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)  
  
Le **Résumé** boîte de dialogue vous permet de confirmer vos paramètres. Il s’agit de la dernière possibilité d’arrêter l’installation avant que l’Assistant crée le compte intermédiaire. Cliquez sur **suivant** lorsque vous êtes prêt à créer le compte d’ordinateur RODC intermédiaire.  Cliquez sur **exporter les paramètres** pour enregistrer un fichier de réponses dans le format de fichier d’installation sans assistance dcpromo obsolète.  
  
### <a name="creation"></a>Création  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)  
  
Le **Assistant Installation de Services de domaine ActiveDirectory** crée le contrôleur de domaine en lecture seule intermédiaire dans ActiveDirectory. Vous ne pouvez pas annuler cette opération après son démarrage.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)  
  
Utilisez l’applet de commande suivante à utiliser pour stocker un compte d’ordinateur du contrôleur domaine en lecture seule à l’aide du module WindowsPowerShellADDSDeployment:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
Voir [Stage RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS) pour connaître les arguments obligatoires et facultatifs.  
  
Étant donné que **Add-addsreadonlydomaincontrolleraccount** contient uniquement une action avec deux phases (configuration requise et installation), les captures d’écran suivantes illustrent la phase d’installation avec les arguments requis minimaux.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)  
  
L’opération RODC étape crée le compte d’ordinateur RODC dans ActiveDirectory. Le centre d’administration ActiveDirectory montre la **Type de contrôleur de domaine** comme un **compte de contrôleur de domaine inoccupé**. Ce type de contrôleur de domaine indique que le compte RODC intermédiaire est prêt pour un serveur comme un contrôleur de domaine en lecture seule.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)  
  
> [!IMPORTANT]  
> Le centre d’administration ActiveDirectory n’est plus nécessaire pour attacher un serveur à un compte ordinateur de contrôleur de domaine en lecture seule. Utilisez le Gestionnaire de serveur et l’Assistant Configuration des Services de domaine ActiveDirectory ou l’applet de commande du module WindowsPowerShellADDSDeployment **Install-AddsDomainController** pour attacher un RODC de nouveau à son intermédiaire compte. Les étapes sont similaires à l’ajout d’un nouveau contrôleur de domaine accessible en écriture à un domaine existant, à l’exception que le compte d’ordinateur RODC intermédiaire contient des options de configuration choisies au moment que vous avez organisé le compte d’ordinateur RODC.  
  
## <a name="attaching"></a>Attachement  
  
### <a name="deployment-configuration"></a>Configuration de déploiement  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Le Gestionnaire de serveur commence chaque promotion du contrôleur de domaine avec le **Configuration de déploiement** page. Les options disponibles et les champs obligatoires modifier sur cette page et les pages suivantes, en fonction de l’opération de déploiement que vous sélectionnez.  
  
Pour ajouter un contrôleur de domaine en lecture seule à un domaine existant, sélectionnez **ajouter un contrôleur de domaine à un domaine existant** et cliquez sur le **sélectionnez** bouton à **spécifier les informations de domaine pour ce domaine**. Le Gestionnaire de serveur vous invite automatiquement pour des informations d’identification valides, ou vous pouvez cliquer sur **modification**.  
  
Attachement d’un RODC nécessite l’appartenance au groupe Admins du domaine dans Windows Server2012. L’Assistant Configuration des Services de domaine ActiveDirectory vous invite ultérieurement si vos informations d’identification actuelles ne disposent pas des autorisations appropriées ou les appartenances au groupe.  
  
Le **Configuration de déploiement** applet de commande WindowsPowerShellADDSDeployment et les arguments sont:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)  
  
Le **Options du contrôleur de domaine** page affiche le domaine options du contrôleur pour le nouveau contrôleur de domaine. Lorsque cette page est chargée, l’Assistant Configuration des Services de domaine ActiveDirectory envoie une requête LDAP à un contrôleur de domaine existant pour rechercher les comptes inoccupés. Si la requête trouve un contrôleur de domaine inoccupé qui partage le même nom que l’ordinateur actuel compte d’ordinateur, puis l’Assistant affiche un message d’information en haut de la page qui indique «**un compte RODC précréé correspondant au nom du serveur cible existe dans le répertoire. Indiquez si vous souhaitez utiliser ce compte RODC existant ou de réinstaller ce contrôleur de domaine**.» L’Assistant utilise le **utiliser le compte RODC existant** en tant que la configuration par défaut.  
  
> [!IMPORTANT]  
> Vous pouvez utiliser le **réinstaller ce contrôleur de domaine** option lorsqu’un contrôleur de domaine a subi un problème physique et ne peut pas retourner à la fonctionnalité. Vous gagnez du temps lors de la configuration du contrôleur de domaine de remplacement en laissant le contrôleur de domaine de compte d’ordinateur et métadonnées d’objet dans ActiveDirectory. Installez le nouvel ordinateur avec le *même nom*et promouvez-le comme contrôleur de domaine dans le domaine. Le **réinstaller ce contrôleur de domaine** option n’est pas disponible si vous avez supprimé les métadonnées de l’objet de contrôleur de domaine à partir d’ActiveDirectory (nettoyage des métadonnées).  
  
Vous ne pouvez pas configurer les options du contrôleur de domaine lorsque vous associez un serveur à un compte d’ordinateur RODC. Vous configurez les options du contrôleur de domaine lorsque vous créez le compte d’ordinateur RODC intermédiaire.  
  
Spécifié **passe en Mode restauration des Services Directory** doit respecter la stratégie de mot de passe appliquée au serveur. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète.  
  
Le **Options du contrôleur de domaine** sont des arguments WindowsPowerShellADDSDeployment:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Le nom du site doit déjà exister quand fourni en tant qu’argument à **- sitename**. Le **install-AddsDomainController** applet de commande ne crée pas de noms de sites. Vous pouvez utiliser la cmdlet **nouveau-adreplicationsite** pour créer des sites.  
  
Le **Install-ADDSDomainController** arguments suivent les mêmes paramètres par défaut en tant que gestionnaire de serveur, le cas contraire.  
  
Le **SafeModeAdministratorPassword** de l’argument est spécial:  
  
-   Si *ne pas spécifié* en tant qu’argument, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer un nouveau RODC dans le corp.contoso.com et être invité à entrer et à confirmer un mot de passe masqué:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
  
### <a name="additional-options"></a>Options supplémentaires  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)  
  
Le **Options supplémentaires** page contient des options de configuration pour nommer un contrôleur de domaine comme source de réplication, ou vous pouvez utiliser n’importe quel contrôleur de domaine comme source de réplication.  
  
Vous pouvez également choisir d’installer le contrôleur de domaine à l’aide de support à l’aide de l’installation à partir de l’option de support (IFM) sauvegardé. Le **installer à partir du média** case à cocher fournit une option de parcourir une fois sélectionnée et vous devez cliquer sur **vérifier** pour garantir le chemin d’accès fourni est un support valide. Support utilisé par l’option IFM est créé avec sauvegarde Windows Server ou Ntdsutil.exe à partir d’un autre Windows Server2012 ordinateur existant uniquement; pour créer un média pour un contrôleur de domaine Windows Server2012, vous ne pouvez pas utiliser un Windows Server2008R2 ou un système d’exploitation précédent. Pour plus d’informations sur les modifications apportées dans IFM, voir [Ntdsutil.exe Install from Media Changes](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Si vous utilisez le média protégé avec SYSKEY, le Gestionnaire de serveur demande un mot de passe de l’image pendant la vérification.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)  
  
Le **Options supplémentaires** sont des arguments de l’applet de commande ADDSDeployment:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Chemins d’accès  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)  
  
Le **chemins d’accès** page vous permet de remplacer les emplacements de dossier par défaut de la base de données ADDS, les journaux des transactions de base de données, et du partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de % systemroot%. Le **chemins d’accès** sont des arguments de l’applet de commande ADDSDeployment:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Examiner les Options et afficher le Script  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)  
  
Le **examiner les Options** page vous permet de valider vos paramètres et assurez-vous qu’elles répondent à vos exigences avant de commencer l’installation. Cela n’est pas la possibilité d’arrêter l’installation à l’aide du Gestionnaire de serveur. Cette page vous permet simplement de vérifier et confirmer vos paramètres avant de poursuivre la configuration. Le **examiner les Options** page dans le Gestionnaire de serveur offre également une option **afficher le Script** bouton pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme de script Windows PowerShell unique. Cela vous permet d’utiliser l’interface graphique du Gestionnaire de serveur comme un studio de déploiement de Windows PowerShell. Utilisez l’Assistant Configuration des Services de domaine ActiveDirectory pour configurer les options, exportez la configuration et annulez ensuite l’Assistant. Ce processus crée un exemple valide et la syntaxe correct pour modification supplémentaire ou une utilisation directe. Par exemple:  
  
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
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s’appuie pas sur les valeurs par défaut (car elles peuvent changer entre les versions futures de Windows ou des service packs). La seule exception est le **- safemodeadministratorpassword** argument. Pour forcer une demande de confirmation omettez la valeur en cas d’exécution interactive de l’applet de commande  
  
Utilisez l’option **Whatif** argument avec la **Install-ADDSDomainController** applet de commande pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d’une applet de commande.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)  
  
### <a name="prerequisites-check"></a>Vérification des conditions préalables  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)  
  
Le **vérification des conditions requises** est une nouvelle fonctionnalité de configuration du domaine ADDS. Cette nouvelle phase valide que la configuration du serveur est capable de prendre en charge une nouvelle forêt ADDS.  
  
Lorsque vous installez un domaine racine de forêt, l’Assistant Gestionnaire de serveur ActiveDirectory domaine Services Configuration appelle une série de tests modulaires sérialisés. Ces tests vous alerte avec les options de réparation suggérés. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus installation de contrôleur de domaine ne peut pas continuer tant que la configuration requise de tous les tests de passer.  
  
Le **vérification des conditions requises** exposent également des informations pertinentes, telles que les modifications de sécurité qui affectent les anciens systèmes d’exploitation. Pour plus d’informations sur les vérifications de la configuration requise, voir [Prerequisite Checking](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Vous ne pouvez pas ignorer le **vérification** lorsque à l’aide du Gestionnaire de serveur, mais vous pouvez ignorer le processus lors de l’utilisation de l’applet de commande de déploiement des services ADDS à l’aide de l’argument suivant:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft déconseille d’ignorer la vérification en tant qu’il peut entraîner une promotion du contrôleur de domaine partielle ou endommagée forêt ADDS.  
  
Cliquez sur **installer** pour commencer le processus de promotion de contrôleur de domaine. Il s’agit de dernière possibilité d’annuler l’installation. Vous ne pouvez pas annuler le processus de promotion une fois qu’il commence. L’ordinateur redémarre automatiquement à la fin de la promotion, quel que soit le résultat.  
  
### <a name="installation"></a>Installation  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)  
  
Lorsque la page d’Installation s’affiche, la configuration de contrôleur de domaine commence et ne peut pas être interrompue ou annulée. Les opérations détaillées s’affichent sur cette page et sont écrites dans les journaux:  
  
-   %Systemroot%\Debug\Dcpromo.log  
  
-   %SystemRoot%\debug\dcpromoui.log  
  
Pour installer une nouvelle forêt ActiveDirectory à l’aide du module ADDSDeployment, utilisez l’applet de commande suivante:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Voir [Attach RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS) pour connaître les arguments obligatoires et facultatifs.  
  
Le **Install-addsdomaincontroller** applet de commande comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d’installation avec les arguments requis minimaux **- domainname**, **- useexistingaccount**, et **-credential**. Notez comment, comme le Gestionnaire de serveur, **Install-ADDSDomainController** vous rappelle que la promotion redémarre automatiquement le serveur:  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)  
  
Pour accepter automatiquement l’invite de redémarrage, utilisez la **-force** ou **-confirmer: $false** arguments avec une applet de commande WindowsPowerShellADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez le **- norebootoncompletion** argument.  
  
> [!WARNING]  
> Il est déconseillé de remplacer le redémarrage. Le contrôleur de domaine doit redémarrer pour fonctionner correctement.  
  
### <a name="results"></a>Résultats  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
Le **résultats** page indique la réussite ou l’échec de la promotion et toute information d’administration importante. Le contrôleur de domaine redémarre automatiquement après 10secondes.  
  
## <a name="rodc-without-staging-workflow"></a>RODC sans intermédiaire Workflow  
Le diagramme suivant illustre le processus de configuration des Services de domaine ActiveDirectory, lorsque vous avez installé précédemment le rôle ADDS et que vous avez démarré l’Assistant ActiveDirectory domaine Services de Configuration à l’aide du Gestionnaire de serveur pour créer un nouveau contrôleur de domaine en lecture seule non intermédiaire dans un domaine Windows Server2012 existant.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)  
  
## <a name="rodc-without-staging-windows-powershell"></a>SEULE sans création intermédiaire dans Windows PowerShell  
  
|||  
|-|-|  
|**Applet de commande ADDSDeployment**|Arguments (**gras** arguments sont requis. *En italique* arguments peuvent être spécifiés à l’aide de Windows PowerShell ou l’Assistant de Configuration ADDS.)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />***-SiteName***<br /><br />*ApplicationPartitionsToReplicate-*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />*-CriticalReplicationOnly*<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-DNSOnNetwork<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*ReplicationSourceDC-*<br /><br />-SkipAutoConfigureDNS<br /><br />*SystemKey-*<br /><br />*-SYSVOLPath*<br /><br />*AllowPasswordReplicationAccountName-*<br /><br />*DelegatedAdministratorAccountName-*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />***-ReadOnlyReplica***|  
  
> [!NOTE]  
> Le **-credential** argument est uniquement requis si vous ne sont pas déjà connecté en tant que membre du groupe Admins du domaine.  
  
## <a name="rodc-without-staging-deployment"></a>RODC sans déploiement intermédiaire  
  
### <a name="deployment-configuration"></a>Configuration de déploiement  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Le Gestionnaire de serveur commence chaque promotion du contrôleur de domaine avec le **Configuration de déploiement** page. Les options disponibles et les champs obligatoires modifier sur cette page et les pages suivantes, en fonction de l’opération de déploiement que vous sélectionnez.  
  
Pour ajouter un contrôleur de domaine en lecture seule non intermédiaire à un domaine Windows Server2012 existant, sélectionnez **ajouter un contrôleur de domaine à un domaine existant** et cliquez sur le **sélectionnez** bouton à **spécifier les informations de domaine pour ce domaine**. Le Gestionnaire de serveur vous invite automatiquement pour des informations d’identification valides, ou vous pouvez cliquer sur **modification**.  
  
Attachement d’un RODC nécessite l’appartenance au groupe Admins du domaine dans Windows Server2012. L’Assistant Configuration des Services de domaine ActiveDirectory vous invite ultérieurement si vos informations d’identification actuelles ne disposent pas des autorisations appropriées ou les appartenances au groupe.  
  
Le **Configuration de déploiement** applet de commande WindowsPowerShellADDSDeployment et les arguments sont:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)  
  
Le **Options du contrôleur de domaine** page spécifie les fonctions du contrôleur de domaine pour le nouveau contrôleur de domaine. Les fonctions du contrôleur de domaine configurables sont **serveur DNS**, **catalogue Global**, et **contrôleur de domaine en lecture seule**. Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et de catalogue global pour la haute disponibilité dans les environnements distribués. Dans le catalogue global est toujours sélectionné par défaut et serveur DNS est sélectionné par défaut si le domaine actuel héberge déjà DNS sur ses contrôleurs de domaine basé sur une requête de démarrage de l’autorité.  
  
Le **Options du contrôleur de domaine** page vous permet également de choisir la logique ActiveDirectory approprié **nom du site** à partir de la configuration de la forêt. Par défaut, il sélectionne le site avec le sous-réseau le mieux adapté. S’il existe un seul site, il sélectionne automatiquement ce site.  
  
> [!IMPORTANT]  
> Si le serveur n’appartient pas à un sous-réseau ActiveDirectory, et il existe plusieurs sites ActiveDirectory, rien n’est sélectionné et le **suivant** bouton n’est pas disponible jusqu'à ce que vous choisissez un site dans la liste.  
  
Spécifié **passe en Mode restauration des Services Directory** doit respecter la stratégie de mot de passe appliquée au serveur. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète passphrase.The **Options du contrôleur de domaine** sont des arguments WindowsPowerShellADDSDeployment:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> Le nom du site doit déjà exister quand fourni en tant qu’argument à **- sitename**. Le **install-AddsDomainController** applet de commande ne crée pas de noms de sites. Vous pouvez utiliser la cmdlet **nouveau-adreplicationsite** pour créer des sites.  
  
Le **Install-ADDSDomainController** arguments suivent les mêmes paramètres par défaut en tant que gestionnaire de serveur, le cas contraire.  
  
Le **SafeModeAdministratorPassword** de l’argument est spécial:  
  
-   Si *ne pas spécifié* en tant qu’argument, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer un nouveau RODC dans le corp.contoso.com et être invité à entrer et à confirmer un mot de passe masqué:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
  
### <a name="rodc-options"></a>Options RODC  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)  
  
Le **Options RODC** page vous permet de modifier les paramètres:  
  
-   Compte d’administrateur délégué  
  
-   Comptes qui sont autorisés à répliquer les mots de passe pour RODC  
  
-   Comptes qui sont autorisés à répliquer les mots de passe pour RODC  
  
Comptes d’administrateur délégué obtenir des autorisations d’administrateur local sur le RODC. Ces utilisateurs peuvent fonctionner avec des privilèges équivalents au groupe Administrateurs de l’ordinateur local.  Ils ne sont pas membres du groupe Admins du domaine ou le groupe Administrateurs intégré au domaine. Cette option est utile pour déléguer l’administration de filiales sans octroyer d’autorisations d’administration de domaine. Configuration de la délégation de l’administration n’est pas nécessaire.  
  
L’argument WindowsPowerShellADDSDeployment équivalent est:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
Comptes qui ne sont pas autorisés à des mots de passe du cache sur le RODC et ne peut pas se connecter et s’authentifier auprès d’un contrôleur de domaine accessible en écriture ne peut pas accéder aux ressources ou des fonctionnalités fournies par ActiveDirectory.  
  
> [!IMPORTANT]  
> Si ne pas modifié, les groupes par défaut et les paramètres sont utilisés:  
>   
> -   Administrateurs - refuser  
> -   Opérateurs de serveur - refuser  
> -   Opérateurs de sauvegarde - refuser  
> -   Opérateurs de compte - refuser  
> -   Refusé le groupe de réplication de mot de passe RODC - refuser  
> -   Autorisé le groupe de réplication de mot de passe RODC - autoriser  
  
Les arguments WindowsPowerShellADDSDeployment équivalents sont:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)  
  
### <a name="additional-options"></a>Options supplémentaires  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)  
  
Le **Options supplémentaires** page contient des options de configuration pour nommer un contrôleur de domaine comme source de réplication, ou vous pouvez utiliser n’importe quel contrôleur de domaine comme source de réplication.  
  
Vous pouvez également choisir d’installer le contrôleur de domaine à l’aide de support à l’aide de l’installation à partir de l’option de support (IFM) sauvegardé. Le **installer à partir du média** case à cocher fournit une option de parcourir une fois sélectionnée et vous devez cliquer sur **vérifier** pour garantir le chemin d’accès fourni est un support valide. Support utilisé par l’option IFM est créé avec sauvegarde Windows Server ou Ntdsutil.exe à partir d’un autre Windows Server2012 ordinateur existant uniquement; pour créer un média pour un contrôleur de domaine Windows Server2012, vous ne pouvez pas utiliser un Windows Server2008R2 ou un système d’exploitation précédent.  Les annexes fournissent plus d’informations sur les changements d’IFM. Si vous utilisez le média protégé avec SYSKEY, le Gestionnaire de serveur demande un mot de passe de l’image pendant la vérification.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)  
  
Les arguments d’applet de commande ADDSDeployment Options supplémentaires sont:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Chemins d’accès  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)  
  
Le **chemins d’accès** page vous permet de remplacer les emplacements de dossier par défaut de la base de données ADDS, les journaux des transactions de base de données, et du partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de % systemroot%. Le **chemins d’accès** sont des arguments de l’applet de commande ADDSDeployment:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Options de préparation  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)  
  
Le **Options de préparation** page vous avertit que la configuration des services ADDS inclut l’extension du schéma (forestprep) et la mise à jour le domaine (domainprep). Cette page s’affiche uniquement lorsque la forêt ou le domaine n’a pas été préparé par une précédente installation de contrôleur de domaine Windows Server2012 ou d’exécuter manuellement Adprep.exe. Par exemple, l’Assistant Configuration des Services de domaine ActiveDirectory supprime cette page si vous ajoutez un nouveau contrôleur de domaine réplica à un domaine racine de forêt Windows Server2012 existant.  
  
L’extension du schéma et le domaine de mise à jour ne se produisent pas lorsque vous cliquez sur **suivant**. Ces événements se produisent uniquement pendant la phase d’installation. Cette page vous informe simplement des événements qui se produira plus loin dans l’installation.  
  
Cette page valide également que les informations d’identification utilisateur actuel sont membres des groupes Administrateurs du schéma et administrateurs de l’entreprise, que vous avez besoin de l’appartenance à ces groupes pour étendre le schéma ou préparer un domaine. Cliquez sur **modification** pour fournir les informations d’identification utilisateur appropriées si la page vous informe que les informations d’identification actuelles ne fournissent pas d’autorisations suffisantes.  
  
L’argument d’applet de commande ADDSDeployment Options supplémentaires est:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Comme avec les versions précédentes de Windows Server, la préparation du domaine automatisée de Windows Server2012 n’exécute pas GPPREP. Exécutez **adprep.exe /gpprep** manuellement pour tous les domaines qui n’étaient pas auparavant préparés pour Windows Server2003, Windows Server2008 ou Windows Server2008R2. Vous devez exécuter GPPrep qu’une seule fois dans l’historique d’un domaine et non avec chaque mise à niveau. Adprep.exe n’exécute pas /gpprep automatiquement, car l’opération peut provoquer de tous les fichiers et dossiers dans le dossier SYSVOL pour la réplication de nouveau sur tous les contrôleurs de domaine.  
>   
> La commande RODCPrep automatique s’exécute quand vous promouvez le premier RODC non intermédiaire dans un domaine. Il ne se produit pas quand vous promouvez le premier contrôleur de domaine Windows Server2012 accessible en écriture. Vous pouvez également toujours exécuter manuellement **adprep.exe /rodcprep** si vous prévoyez de déployer des contrôleurs de domaine en lecture seule.  
  
### <a name="review-options-and-view-script"></a>Examiner les Options et afficher le Script  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)  
  
Le **examiner les Options** page vous permet de valider vos paramètres et assurez-vous qu’elles répondent à vos exigences avant de commencer l’installation. Cela n’est pas la possibilité d’arrêter l’installation à l’aide du Gestionnaire de serveur. Cette page vous permet simplement de vérifier et confirmer vos paramètres avant de poursuivre la configuration.  
  
Le **examiner les Options** page dans le Gestionnaire de serveur offre également une option **afficher le Script** bouton pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme de script Windows PowerShell unique. Cela vous permet d’utiliser l’interface graphique du Gestionnaire de serveur comme un studio de déploiement de Windows PowerShell. Utilisez l’Assistant Configuration des Services de domaine ActiveDirectory pour configurer les options, exportez la configuration et annulez ensuite l’Assistant. Ce processus crée un exemple valide et la syntaxe correct pour modification supplémentaire ou une utilisation directe. Par exemple:  
  
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
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s’appuie pas sur les valeurs par défaut (car elles peuvent changer entre les versions futures de Windows ou des service packs). La seule exception est le **- safemodeadministratorpassword** argument. Pour forcer une demande de confirmation, omettez la valeur en cas d’exécution interactive de l’applet de commande.  
  
Utilisez l’argument Whatif facultatif avec l’applet de commande Install-ADDSDomainController pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d’une applet de commande.  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)  
  
### <a name="prerequisites-check"></a>Vérification des conditions préalables  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)  
  
Le **vérification des conditions requises** est une nouvelle fonctionnalité de configuration du domaine ADDS. Cette nouvelle phase valide que la configuration du serveur est capable de prendre en charge une nouvelle forêt ADDS.  
  
Lorsque vous installez un domaine racine de forêt, l’Assistant Gestionnaire de serveur ActiveDirectory domaine Services Configuration appelle une série de tests modulaires sérialisés. Ces tests vous alerte avec les options de réparation suggérés. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus de contrôleur de domaine ne peut pas continuer tant que la configuration requise de tous les tests passer.  
  
Le **vérification des conditions requises** exposent également des informations pertinentes, telles que les modifications de sécurité qui affectent les anciens systèmes d’exploitation.  
  
Vous ne pouvez pas ignorer le **vérification** lorsque à l’aide du Gestionnaire de serveur, mais vous pouvez ignorer le processus lors de l’utilisation de l’applet de commande de déploiement des services ADDS à l’aide de l’argument suivant:  
  
```  
-skipprechecks  
  
```  
  
Cliquez sur **installer** pour commencer le processus de promotion de contrôleur de domaine. Il s’agit de dernière possibilité d’annuler l’installation. Vous ne pouvez pas annuler le processus de promotion une fois qu’il commence. L’ordinateur redémarre automatiquement à la fin de la promotion, quel que soit le résultat.  
  
### <a name="installation"></a>Installation  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)  
  
Lorsque le **Installation** page s’affiche, la configuration de contrôleur de domaine commence et ne peut pas être interrompue ou annulée. Les opérations détaillées s’affichent sur cette page et sont écrites dans les journaux:  
  
-   %Systemroot%\Debug\Dcpromo.log  
  
-   %SystemRoot%\debug\dcpromoui.log  
  
Pour installer une nouvelle forêt ActiveDirectory à l’aide du module ADDSDeployment, utilisez l’applet de commande suivante:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Consultez le **ADDSDeployment Cmdlet** table au début de cette section pour connaître les arguments obligatoires et facultatifs.  
  
Le **Install-addsdomaincontroller** applet de commande comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d’installation avec les arguments requis minimaux **- domainname**, **- readonlyreplica**, **- sitename**, et **-credential**. Notez comment, comme le Gestionnaire de serveur, **Install-ADDSDomainController** vous rappelle que la promotion redémarre automatiquement le serveur:  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)  
  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)  
  
Pour accepter automatiquement l’invite de redémarrage, utilisez la **-force** ou **-confirmer: $false** arguments avec une applet de commande WindowsPowerShellADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez le **- norebootoncompletion** argument.  
  
> [!WARNING]  
> Remplacer le redémarrage n’est pas recommandée. Le contrôleur de domaine doit redémarrer pour fonctionner correctement. Si vous vous déconnectez le contrôleur de domaine, vous ne pouvez pas rouvrir de façon interactive jusqu'à ce que vous le redémarrez.  
  
### <a name="results"></a>Résultats  
![Installer RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)  
  
Le **résultats** page indique la réussite ou l’échec de la promotion et toute information d’administration importante. Le contrôleur de domaine redémarre automatiquement après 10secondes.  
  

