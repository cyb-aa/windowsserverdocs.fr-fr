---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: Installer les Services de domaine ActiveDirectory (niveau 100)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f76aa1e5200a9fc2f47a559c4a318aa619d31557
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-active-directory-domain-services-level-100"></a>Installer les Services de domaine ActiveDirectory (niveau 100)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique explique comment installer les services ADDS dans Windows Server2012 en utilisant l’une des méthodes suivantes:  
  
-   [Informations d’identification requises pour exécuter Adprep.exe et installer les Services de domaine ActiveDirectory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)  
  
-   [L’installation des services ADDS à l’aide de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)  
  
-   [L’installation des services ADDS à l’aide du Gestionnaire de serveur](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)  
  
-   [Exécution d’une Installation RODC intermédiaire à l’aide de l’Interface graphique utilisateur](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)  
  
## <a name="BKMK_Creds"></a>Informations d’identification requises pour exécuter Adprep.exe et installer les Services de domaine ActiveDirectory  
Les informations d’identification suivantes sont requis pour exécuter Adprep.exe et installer les services ADDS.  
  
-   Pour installer une nouvelle forêt, vous devez être connecté en tant qu’administrateur local de l’ordinateur.  
  
-   Pour installer un nouveau domaine enfant ou une nouvelle arborescence de domaine, vous devez être connecté en tant que membre du groupe Administrateurs de l’entreprise.  
  
-   Pour installer un contrôleur de domaine supplémentaire dans un domaine existant, vous devez être membre du groupe Admins du domaine.  
  
    > [!NOTE]  
    > Si vous n’exécutez pas de commande adprep.exe séparément et que vous installez le premier contrôleur de domaine qui exécute Windows Server2012dans une forêt ou un domaine existant, vous devrez fournir les informations d’identification pour exécuter des commandes Adprep. Les informations d’identification requises sont les suivantes:  
    >   
    > -   Pour introduire le premier contrôleur de domaine Windows Server2012dans la forêt, vous devez fournir les informations d’identification d’un membre du groupe Administrateurs de l’entreprise, le groupe Administrateurs du schéma, et groupe Admins du domaine dans le domaine qui héberge le contrôleur de schéma.  
    > -   Pour introduire le premier contrôleur de domaine Windows Server2012dans un domaine, vous devez fournir les informations d’identification d’un membre du groupe Admins du domaine.  
    > -   Pour introduire le premier contrôleur de domaine en lecture seule (RODC) dans la forêt, vous devez fournir les informations d’identification d’un membre du groupe Administrateurs de l’entreprise.  
    >   
    >     > [!NOTE]  
    >     > Si vous avez déjà exécuté adprep /rodcprep dans Windows Server2008 ou Windows Server2008R2, vous n’avez pas besoin de s’exécuter à nouveau pour Windows Server2012.  
  
## <a name="BKMK_PS"></a>L’installation des services ADDS à l’aide de Windows PowerShell  
À compter de Windows Server2012, vous pouvez installer les services ADDS à l’aide de Windows PowerShell. Dcpromo.exe est déconseillée à compter de Windows Server2012, mais vous pouvez toujours exécuter dcpromo.exe à l’aide d’un fichier de réponses (dcpromo//unattend:<answerfile> ou dcpromo//answer:<answerfile>). La possibilité de poursuivre l’exécution de dcpromo.exe avec un fichier de réponses fournit aux organisations qui utilisent des ressources investis dans le temps d’automatisation existantes pour convertir l’automatisation dcpromo.exe Windows PowerShell. Pour plus d’informations sur l’exécution de dcpromo.exe avec un fichier de réponses, voir [https://support.microsoft.com/kb/947034](https://support.microsoft.com/kb/947034).  
  
Pour plus d’informations sur la suppression des services ADDS à l’aide de Windows PowerShell, voir [supprimer les services ADDS à l’aide de Windows PowerShell](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS).  
  
Commencez par ajouter le rôle à l’aide de Windows PowerShell. Cette commande installe le rôle de serveur ADDS et installe les services ADDS et ADLDS server outils d’administration, y compris les outils d’interface graphique utilisateur tels que les utilisateurs ActiveDirectory et les ordinateurs et les outils de ligne de commande tels que dcdia.exe. Outils d’administration de serveur ne sont pas installés par défaut lorsque vous utilisez Windows PowerShell. Vous devez spécifier **«IncludeManagementTools** pour gérer le serveur local ou installer [Remote Server Administration Tools](https://www.microsoft.com/download/details.aspx?id=28972) pour gérer un serveur distant.  
  
```  
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools  
<<Windows PowerShell cmdlet and arguments>>  
```  
  
Il n’existe aucun redémarrage requis avant l’installation des services ADDS est terminée.  
  
Vous pouvez ensuite exécuter cette commande pour voir les applets de commande disponibles dans le module ADDSDeployment.  
  
```  
Get-Command -Module ADDSDeployment
```  
  
Pour afficher la liste des arguments qui peuvent être spécifiées pour une syntaxe et les applets de commande:  
  
```  
Get-Help <cmdlet name>  
```  
  
Par exemple, pour connaître les arguments pour la création d’un compte de contrôleur (RODC) inoccupé domaine en lecture seule, tapez:  
  
```  
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```  
  
Les arguments facultatifs apparaissent entre crochets.  
  
Vous pouvez également télécharger les dernières aide exemples et concepts pour les applets de commande Windows PowerShell. Pour plus d’informations, voir [about_Updatable_Help](https://technet.microsoft.com/library/hh847735.aspx).  
  
Vous pouvez exécuter les applets de commande Windows PowerShell sur des serveurs distants:  
  
-   Dans Windows PowerShell, utilisez Invoke-Command avec l’applet de commande ADDSDeployment. Par exemple, pour installer les services ADDS sur un serveur distant nommé ConDC3 dans le domaine contoso.com, tapez:  
  
    ```  
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3  
    ```  
  
- ou -  
  
-   Dans le Gestionnaire de serveur, créez un groupe de serveurs qui inclut le serveur distant. Cliquez sur le nom du serveur distant, puis cliquez sur **Windows PowerShell**.  
  
Les sections suivantes expliquent comment exécuter les applets de commande module ADDSDeployment pour installer les services ADDS.  
  
-   [Arguments de l’applet de commande ADDSDeployment](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)  
  
-   [Spécification des informations d’identification de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)  
  
-   [À l’aide des applets de commande de test](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)  
  
-   [L’installation d’un domaine racine de forêt à l’aide de Windows PowerShell](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)  
  
-   [Installer un nouveau domaine enfant ou d’arborescence à l’aide de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)  
  
-   [Installation d’un contrôleur de domaine supplémentaire (réplica) à l’aide de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)  
  
### <a name="BKMK_Params"></a>Arguments de l’applet de commande ADDSDeployment  
Le tableau suivant répertorie les arguments pour les applets de commande ADDSDeployment dans Windows PowerShell. Les arguments en gras sont requis. Les arguments équivalents pour dcpromo.exe sont répertoriés entre parenthèses s’ils sont nommés différemment dans Windows PowerShell.  
  
Commutateurs Windows PowerShell acceptent les arguments $TRUE ou $FALSE. Les arguments qui sont $TRUE par défaut n’avez pas besoin d’être spécifié.  
  
Pour remplacer les valeurs par défaut, vous pouvez spécifier l’argument avec la valeur $False. Par exemple, dans la mesure où **- installdns** est exécuté automatiquement pour une installation nouvelle forêt s’il n’est pas spécifié, la seule façon *empêcher* installation DNS lorsque vous installez une nouvelle forêt consiste à utiliser:  
  
```  
-InstallDNS:$false  
```  
  
De même, étant donné que **«installdns** a la valeur par défaut $False si vous installez un contrôleur de domaine dans un environnement qui n’héberge pas de serveur Windows DNS server, vous devez spécifier l’argument suivant pour installer le serveur DNS:  
  
```  
-InstallDNS:$true  
```  
  
|Argument|Description|  
|------------|---------------|  
|**ADPrepCredential <PS Credential> ** **Remarque:** requis si vous installez le premier contrôleur de domaine Windows Server2012 dans un domaine ou forêt et les informations d’identification de l’utilisateur actuel sont insuffisantes pour effectuer l’opération.|Spécifie le compte au groupe Administrateurs de l’entreprise et administrateurs du schéma qui peut préparer la forêt, selon les règles de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) et un objet PSCredential.<br /><br />Si aucune valeur n’est spécifiée, la valeur de la **«informations d’identification** argument est utilisé.|  
|AllowDomainControllerReinstall|Spécifie si vous souhaitez continuer l’installation de ce contrôleur de domaine accessible en écriture, malgré le fait qu’un autre compte de contrôleur de domaine accessible en écriture portant le même nom a été détecté.<br /><br />Utilisez **$True** uniquement si vous êtes sûr que le compte n’est pas actuellement utilisé par un autre contrôleur de domaine accessible en écriture.<br /><br />La valeur par défaut est **$False**.<br /><br />Cet argument n’est pas valide pour un RODC.|  
|AllowDomainReinstall|Spécifie si un domaine existant est recréé.<br /><br />La valeur par défaut est **$False**.|  
|AllowPasswordReplicationAccountName < chaîne [] >|Spécifie les noms des comptes d’utilisateur, comptes de groupes et comptes d’ordinateur dont les mots de passe peuvent être répliqués sur ce RODC. Utilisez une chaîne vide «» si vous souhaitez conserver la valeur vide. Par défaut, uniquement autorisé RODC mot de passe de groupe de réplication est autorisé, et il est vide à l’origine.<br /><br />Fournir des valeurs sous la forme d’un tableau de chaînes. Par exemple:<br /><br />AllowPasswordReplicationAccountName-«JSmith», «JSmithPC», «Branche utilisateurs» de code|  
|ApplicationPartitionsToReplicate < chaîne [] > **Remarque:** il n’existe aucune option équivalente dans l’interface utilisateur. Si vous installez à l’aide de l’interface utilisateur, ou à l’aide d’IFM, toutes les partitions d’application seront répliquées.|Spécifie les partitions d’annuaire application à répliquer. Cet argument est uniquement appliqué lorsque vous spécifiez la **- InstallationMediaPath** argument à installer à partir du support (IFM). Par défaut, toutes les applications de partitions répliquera basé sur leur propre étendue.<br /><br />Fournir des valeurs sous la forme d’un tableau de chaînes. Par exemple:<br /><br />Code:<br /><br />-Les ApplicationPartitionsToReplicate «partition1», «partition2», «partition3»|  
|Confirmer|Vous demande confirmation avant d’exécuter l’applet de commande.|  
|CreateDnsDelegation **Remarque:** vous ne pouvez pas spécifier cet argument lorsque vous exécutez l’applet de commande Add-ADDSReadOnlyDomainController.|Indique s’il faut créer une délégation DNS qui référence le nouveau serveur DNS que vous installez ainsi que le contrôleur de domaine. Valide pour «intégrées à ActiveDirectory DNS uniquement. Les enregistrements de délégation peuvent être créés uniquement sur les serveurs DNS Microsoft qui sont en ligne et accessible. Les enregistrements de délégation ne peuvent pas être créés pour les domaines qui sont immédiatement subordonnés à des domaines de premier niveau tels que .com, .gov, .biz, .edu ou domaines de code de pays à deux lettres tels que .nz et .au.<br /><br />La valeur par défaut est calculée automatiquement en fonction de l’environnement.|  
|**Informations d’identification <PS Credential> ** **Remarque:** requis uniquement si les informations d’identification de l’utilisateur actuel sont insuffisantes pour effectuer l’opération.|Spécifie le compte de domaine qui peut se connecter au domaine, selon les règles de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) et un objet PSCredential.<br /><br />Si aucune valeur n’est spécifiée, les informations d’identification de l’utilisateur actuel sont utilisées.|  
|CriticalReplicationOnly|Spécifie si l’opération d’installation ADDS effectue uniquement la réplication critique avant le redémarrage, puis continue. La réplication non critique se produit après l’installation est terminée et l’ordinateur redémarre.<br /><br />À l’aide de cet argument n’est pas recommandée.<br /><br />Il n’existe aucun équivalent pour cette option dans l’interface utilisateur (UI).|  
|DatabasePath <string>|Spécifie le complet, non «chemin d’accès Universal Naming Convention (UNC) vers un répertoire sur un disque fixe de l’ordinateur local qui contient la base de données du domaine, par exemple, **C:\Windows\NTDS.**<br /><br />La valeur par défaut est **%SYSTEMROOT%\NTDS**. **Important:** pendant que vous pouvez stocker les fichiers de base de données et journaux ADDS sur un volume formaté avec le système de fichiers résilient (ReFS), il n’y a aucune avantages spécifiques pour l’hébergement des services ADDS sur ReFS, autres que les avantages classiques de résilience vous bénéficiez en hébergeant des données sur ReFS.|  
|DelegatedAdministratorAccountName <string>|Spécifie le nom de l’utilisateur ou le groupe que vous pouvez installer et administrer le RODC.<br /><br />Par défaut, seuls les membres du groupe Admins du domaine peuvent administrer un RODC.|  
|DenyPasswordReplicationAccountName < chaîne [] >|Spécifie les noms des comptes d’utilisateur, comptes de groupes et comptes d’ordinateur dont les mots de passe ne doivent ne pas être répliqués sur ce RODC. Utilisez une chaîne vide «» si vous ne souhaitez pas refuser la réplication des informations d’identification des utilisateurs ou ordinateurs. Par défaut, les administrateurs, opérateurs de serveur, opérateurs de sauvegarde, opérateurs de compte et le groupe de réplication de mot de passe RODC refusée sont refusé. Par défaut, le groupe de réplication de mot de passe RODC refusée contient les éditeurs de certificats, Admins du domaine, administrateurs de l’entreprise, contrôleurs de domaine d’entreprise, entreprise Read-Only contrôleurs de domaine, propriétaires créateurs de la stratégie de groupe, le compte krbtgt et administrateurs du schéma.<br /><br />Fournir des valeurs sous la forme d’un tableau de chaînes. Par exemple:<br /><br />Code:<br /><br />-DenyPasswordReplicationAccountName «RegionalAdmins», «AdminPCs»|  
|DnsDelegationCredential <PS Credential> **Remarque:** vous ne pouvez pas spécifier cet argument lorsque vous exécutez l’applet de commande Add-ADDSReadOnlyDomainController.|Spécifie le nom d’utilisateur et mot de passe pour créer la délégation DNS selon les règles de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) et un objet PSCredential.|  
|DomainMode <DomainMode> {Win2003 & #124; Win2008 & #124; Win2008R2 & #124; Win2012 & #124; Win2012R2}<br /><br />Ou<br /><br />DomainMode <DomainMode> {2 & #124; 3 & #124; 4 & #124; 5 & #124; 6}|Spécifie le niveau fonctionnel du domaine lors de la création d’un nouveau domaine.<br /><br />Le niveau fonctionnel du domaine ne peut pas être inférieur au niveau fonctionnel de forêt, mais il peut être supérieur.<br /><br />La valeur par défaut est automatiquement calculée et définir le niveau fonctionnel de forêt existante ou la valeur est définie pour **- ForestMode**.|  
|**Nom de domaine**<br /><br />Requis pour les applets de commande Install-ADDSForest et Install-ADDSDomainController.|Spécifie le nom de domaine complet du domaine dans lequel vous souhaitez installer un contrôleur de domaine supplémentaire.|  
|**DomainNetbiosName <string>**<br /><br />Requis pour Install-ADDSForest si le nom de préfixe de nom de domaine complet est plu de 15 caractères.|Utilisez avec Install-ADDSForest. Attribue un nom NetBIOS pour le domaine racine de forêt.|  
|DomainType <DomainType> {ChildDomain & #124; TreeDomain} ou {enfant & #124; arborescence}|Indique le type de domaine que vous souhaitez créer: une nouvelle arborescence de domaine dans un existant de la forêt, un enfant d’un domaine existant, ou une nouvelle forêt.<br /><br />Type de domaine par défaut est ChildDomain.|  
|Force|Lorsque ce paramètre est spécifié tous les avertissements susceptibles d’apparaître normalement lors de l’installation et l’ajout du contrôleur de domaine sont supprimés pour permettre à l’applet de commande de terminer son exécution. Ce paramètre peut être utile d’inclure les scripts d’installation.|  
|ForestMode <ForestMode> {Win2003 et #124; win2008 et #124; win2008R2 et #124; win2012 et #124; win2012R2}<br /><br />Ou<br /><br />ForestMode <ForestMode> {2 et #124; 3 et #124; 4 et #124; 5 et #124; 6}|Spécifie le niveau fonctionnel de forêt lorsque vous créez une nouvelle forêt.<br /><br />La valeur par défaut est Win2012.|  
|InstallationMediaPath|Indique l’emplacement du support d’installation qui servira à installer un nouveau contrôleur de domaine.|  
|InstallDns|Spécifie si le service serveur DNS doit être installé et configuré sur le contrôleur de domaine.<br /><br />Pour une nouvelle forêt, la valeur par défaut est **$True** et serveur DNS est installé.<br /><br />Pour un nouveau domaine enfant ou une arborescence de domaine, si le domaine parent (ou le domaine racine de forêt pour une arborescence de domaine) héberge et stocke déjà les noms DNS pour le domaine, la valeur par défaut pour ce paramètre est $True.<br /><br />Pour l’installation d’un contrôleur de domaine dans un domaine existant, si ce paramètre est omis et le domaine actuel héberge et stocke les noms DNS pour le domaine, puis la valeur par défaut pour ce paramètre est déjà **$True**. Dans le cas contraire, si les noms de domaine DNS sont hébergés en dehors d’ActiveDirectory, la valeur par défaut est **$False** et aucun serveur DNS est installé.|  
|LogPath <string>|Spécifie le chemin d’accès non UNC complet, un répertoire sur un disque fixe de l’ordinateur local qui contient les fichiers journaux du domaine, par exemple, **C:\Windows\Logs**.<br /><br />La valeur par défaut est **%SYSTEMROOT%\NTDS**. **Important:** ne stockez pas les fichiers journaux ActiveDirectory sur un volume de données formaté avec le système de fichiers résilient (ReFS).|  
|MoveInfrastructureOperationMasterRoleIfNecessary|Spécifie si vous souhaitez transférer le rôle de maître d’opérations maître infrastructure (également appelées opérations à maître unique flottant ou FSMO) sur le contrôleur de domaine que vous créez «dans le cas il est actuellement hébergé sur un serveur de catalogue global», et vous ne souhaitez pas utiliser le contrôleur de domaine que vous créez un serveur de catalogue global. Spécifiez ce paramètre pour transférer le rôle de maître d’infrastructure au contrôleur de domaine que vous créez dans le cas où le transfert est nécessaire; dans ce cas, spécifiez la **NoGlobalCatalog** option si vous souhaitez que le rôle de maître d’infrastructure de rester où il est actuellement.|  
|**NewDomainName <string>****Remarque:** obligatoire uniquement pour Install-ADDSDomain.|Spécifie le nom de domaine unique pour le nouveau domaine.<br /><br />Par exemple, si vous souhaitez créer un nouveau domaine enfant nommé **emea.corp.fabrikam.com**, vous devez spécifier **emea** en tant que la valeur de cet argument.|  
|**NewDomainNetbiosName <string>**<br /><br />Requis pour Install-ADDSDomain si le nom de préfixe de nom de domaine complet est plu de 15caractères.|Utilisez avec Install-ADDSDomain. Attribue un nom NetBIOS au nouveau domaine. La valeur par défaut est dérivée de la valeur de **«NewDomainName**.|  
|NoDnsOnNetwork|Spécifie que le service DNS n’est pas disponible sur le réseau. Ce paramètre est utilisé uniquement lorsque le paramètre IP de la carte réseau de cet ordinateur n’est pas configuré avec le nom d’un serveur DNS pour la résolution de noms. Il indique qu’un serveur DNS sera installé sur cet ordinateur pour la résolution de noms. Dans le cas contraire, les paramètres IP de la carte réseau doivent d’abord être configurés avec l’adresse d’un serveur DNS.<br /><br />Si vous omettez ce paramètre (la valeur par défaut) indique que les paramètres de client TCP/IP de la carte réseau sur cet ordinateur serveur seront utilisés pour contacter un serveur DNS. Par conséquent, si vous ne spécifiez pas ce paramètre, vérifiez que les paramètres de client TCP/IP sont tout d’abord configurés avec une adresse de serveur DNS préféré.|  
|NoGlobalCatalog|Spécifie que vous ne souhaitez pas que le contrôleur de domaine soit un serveur de catalogue global.<br /><br />Par défaut, les contrôleurs de domaine qui exécutent Windows Server2012 sont installés avec le catalogue global. En d’autres termes, il s’exécute automatiquement sans calcul, sauf si vous spécifiez:<br /><br />Code:<br /><br />-NoGlobalCatalog|  
|NoRebootOnCompletion|Spécifie s’il faut redémarrer l’ordinateur à l’issue de la commande, qu’elle ait abouti. Par défaut, l’ordinateur redémarre. Pour empêcher le serveur de redémarrer, spécifiez:<br /><br />Code:<br /><br />-NoRebootOnCompletion: $True<br /><br />Il n’existe aucun équivalent pour cette option dans l’interface utilisateur (UI).|  
|**ParentDomainName <string>****Remarque:** requis pour l’applet de commande Install-ADDSDomain|Spécifie le nom de domaine complet d’un domaine parent existant. Vous utilisez cet argument lorsque vous installez un domaine enfant ou une nouvelle arborescence de domaine.<br /><br />Par exemple, si vous souhaitez créer un nouveau domaine enfant nommé **emea.corp.fabrikam.com**, vous devez spécifier **corp.fabrikam.com** en tant que la valeur de cet argument.|  
|ReadOnlyReplica|Spécifie s’il faut installer un contrôleur de domaine en lecture seule (RODC).|  
|ReplicationSourceDC <string>|Indique le nom de domaine complet du contrôleur de domaine partenaire à partir duquel vous répliquez les informations de domaine. La valeur par défaut est automatiquement calculée.|  
|**SafeModeAdministratorPassword <securestring>**|Fournit le mot de passe pour le compte d’administrateur lorsque l’ordinateur est démarré en Mode sans échec ou dans une variante du Mode sans échec, par exemple, le Mode de restauration des Services d’annuaire.<br /><br />La valeur par défaut est un mot de passe vide. Vous devez fournir un mot de passe. Le mot de passe doit être fourni dans un format System.Security.SecureString, comme celle fournie par la lecture-host - assecurestring ou ConvertTo-SecureString.<br /><br />L’argument de SafeModeAdministratorPassword est spécial: ne pas spécifié en tant qu’argument, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré cas d’exécution interactive de l’applet de commande interactively.If est spécifié sans valeur et qu’aucun autre argument n’est spécifié pour l’applet de commande, l’applet de commande vous invite à entrer un mot de passe masqué sans confirmation. Cela n’est pas le mode d’utilisation préféré cas d’exécution interactive de l’applet de commande interactively.If spécifié avec une valeur, la valeur doit être une chaîne sécurisée. Cela n’est pas le mode d’utilisation préféré cas d’exécution interactive de l’applet de commande interactively.For, vous pouvez manuellement inviter un mot de passe à l’aide de l’applet de commande Read-Host pour inviter l’utilisateur d’une chaîne sécurisée:-safemodeadministratorpassword (en lecture-host - invite «mot de passe:» - assecurestring) vous pouvez également fournir une chaîne sécurisée en tant qu’une variable en texte clair convertie, bien que ceci soit fortement déconseillé. -safemodeadministratorpassword (convertto-securestring «Password1» - asplaintext-force)|  
|**Nom du site <string>**<br /><br />Requis pour l’applet de commande Add-addsreadonlydomaincontrolleraccount|Spécifie le site où le contrôleur de domaine sera installé. Il existe aucun **«sitename** argument lorsque vous exécutez **Install-ADDSForest**, car le premier site créé est Default-First-Site-Name.<br /><br />Le nom du site doit déjà exister quand fourni en tant qu’argument à **- sitename**. L’applet de commande ne crée pas le site.|  
|SkipAutoConfigureDNS|Ignore la configuration automatique des paramètres de client DNS, des redirecteurs et des indications de racine. Cet argument est uniquement en vigueur si le service serveur DNS est déjà installé ou automatiquement installé avec **- InstallDNS**.|  
|SystemKey <string>|Spécifie la clé système pour le média à partir duquel vous répliquez les données.<br /><br />La valeur par défaut est **aucun**.<br /><br />Les données doivent être au format fourni par en lecture-host - assecurestring ou ConvertTo-SecureString.|  
|SysvolPath <string>|Spécifie le chemin d’accès non UNC complet, un répertoire sur un disque fixe de l’ordinateur local, par exemple, **C:\Windows\SYSVOL**.<br /><br />La valeur par défaut est **%SYSTEMROOT%\SYSVOL**. **Important:** SYSVOL ne peut pas être stocké sur un volume de données formaté avec le système de fichiers résilient (ReFS).|  
|SkipPreChecks|N’exécute pas les vérifications de la configuration requise avant de commencer l’installation. Il n’est pas conseillé d’utiliser ce paramètre.|  
|WhatIf|Montre ce qui se passerait si l’applet de commande s’exécute. L’applet de commande n’est pas exécutée.|  
  
### <a name="BKMK_PSCreds"></a>Spécification des informations d’identification de Windows PowerShell  
Vous pouvez spécifier des informations d’identification sans les divulguer en texte brut à l’écran à l’aide de [Get-credential](https://technet.microsoft.com/library/dd315327.aspx).  
  
L’opération de - SafeModeAdministratorPassword et LocalAdministratorPassword arguments est spéciale:  
  
-   Si non spécifié en tant qu’argument, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré cas d’exécution interactive de l’applet de commande.  
  
-   Si spécifié avec une valeur, la valeur doit être une chaîne sécurisée. Cela n’est pas le mode d’utilisation préféré cas d’exécution interactive de l’applet de commande.  
  
Par exemple, vous pouvez demander manuellement un mot de passe à l’aide de la **Read-Host** applet de commande pour inviter l’utilisateur d’une chaîne sécurisée  
  
```  
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```  
  
> [!WARNING]  
> Comme l’option précédente ne confirme pas le mot de passe, faites preuve de prudence: le mot de passe n’est pas visible.  
  
Vous pouvez également fournir une chaîne sécurisée en tant qu’une variable en texte clair convertie, bien que ceci soit fortement déconseillé:  
  
```  
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```  
  
> [!WARNING]  
> Fournir ou de stocker un mot de passe de texte en clair n’est pas recommandée. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe DSRM de ce contrôleur de domaine. Avec cette information, ils peuvent emprunter l’identité du contrôleur de domaine et élever son privilège au niveau plus élevé dans une forêt ActiveDirectory.  
  
### <a name="BKMK_TestCmdlets"></a>À l’aide des applets de commande de test  
Chaque applet de commande ADDSDeployment est associée à une applet de commande de test. L’applets de commande de test s’exécute uniquement les vérifications de la configuration requise pour l’opération d’installation; paramètres d’installation sont configurés. Les arguments pour chaque applet de commande test sont les mêmes que pour l’applet de commande installation correspondante, mais **«SkipPreChecks** n’est pas disponible pour les applets de commande de test.  
  
|Applet de commande test|Description|  
|---------------|---------------|  
|Test-ADDSForestInstallation|Exécute la configuration requise pour l’installation d’une nouvelle forêt ActiveDirectory.|  
|Test-ADDSDomainInstallation|Exécute la configuration requise pour installer un nouveau domaine dans ActiveDirectory.|  
|Test-ADDSDomainControllerInstallation|Exécute la configuration requise pour installer un contrôleur de domaine dans ActiveDirectory.|  
|Test-ADDSReadOnlyDomainControllerAccountCreation|Exécute la configuration requise pour l’ajout d’un contrôleur de domaine en lecture seule compte (RODC).|  
  
### <a name="BKMK_PSForest"></a>L’installation d’un domaine racine de forêt à l’aide de Windows PowerShell  
La syntaxe de commande pour installer une nouvelle forêt est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> L’argument - DomainNetBIOSName est requis si vous souhaitez modifier le nom de 15caractères qui est généré automatiquement basé sur le préfixe de nom de domaine DNS ou si le nom dépasse 15caractères.  
  
Par exemple, pour installer une nouvelle forêt nommée corp.contoso.com et en toute sécurité invité à fournir le mot de passe DSRM, tapez:  
  
```  
Install-ADDSForest -DomainName "corp.contoso.com"   
```  
  
> [!NOTE]  
> Serveur DNS est installé par défaut lorsque vous exécutez Install-ADDSForest.  
  
Pour installer une nouvelle forêt nommée corp.contoso.com créer une délégation DNS dans le domaine contoso.com, définir le niveau fonctionnel du domaine vers Windows Server2008R2 et définir le niveau fonctionnel de forêt vers Windows Server2008, installez la base de données ActiveDirectory et SYSVOL sur le lecteur D:\, installer les fichiers journaux sur le lecteur E:\ et être invité à fournir le mot de passe de Mode de restauration des Services d’annuaire et le type:  
  
```  
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="BKMK_PSDomain"></a>Installer un nouveau domaine enfant ou d’arborescence à l’aide de Windows PowerShell  
La syntaxe de commande pour installer un nouveau domaine est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> Le **-credential** argument est uniquement requis lorsque vous n'êtes pas actuellement connecté en tant que membre du groupe Administrateurs de l’entreprise.  
>   
> Le **- NewDomainNetBIOSName** argument est requis si vous souhaitez modifier le nom de 15caractères automatiquement généré basé sur le préfixe de nom de domaine DNS ou si le nom dépasse 15caractères.  
  
Par exemple, pour utiliser les informations d’identification de corp\EnterpriseAdmin1 pour créer un nouveau domaine enfant nommé child.corp.contoso.com, installer le serveur DNS, créer une délégation DNS dans le domaine corp.contoso.com, définissez le niveau fonctionnel du domaine vers Windows Server2003, vérifiez le contrôleur de domaine à un serveur de catalogue global dans un site nommé Houston, utilisez DC1.corp.contoso.com en tant que contrôleur de domaine source la réplication, installez la base de données ActiveDirectory et SYSVOL sur le lecteur D:\, installer les fichiers journaux sur le lecteur E:\ et être invité à fournir le mot de passe du Mode de restauration des Services d’annuaire, mais ne pas invité à confirmer la commande, tapez:  
  
```  
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False  
```  
  
### <a name="BKMK_PSReplica"></a>Installation d’un contrôleur de domaine supplémentaire (réplica) à l’aide de Windows PowerShell  
La syntaxe de commande pour installer un contrôleur de domaine supplémentaire est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Pour installer un contrôleur de domaine et le serveur DNS dans le domaine corp.contoso.com et être invité à fournir les informations d’identification administrateur de domaine et le mot de passe DSRM, tapez:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```  
  
Si l’ordinateur est déjà joint au domaine et que vous êtes membre du groupe Admins du domaine, vous pouvez utiliser:  
  
```  
Install-ADDSDomainController -DomainName "corp.contoso.com"  
```  
  
Pour être invité à entrer le nom de domaine, tapez:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```  
  
La commande suivante utilise les informations d’identification de Contoso\EnterpriseAdmin1 pour installer un contrôleur de domaine accessible en écriture et un serveur de catalogue global dans un site nommé Boston, installer le serveur DNS, créer une délégation DNS dans le domaine contoso.com, installer à partir du média qui est stocké dans le dossier c:\ADDS IFM, installer la base de données ActiveDirectory et SYSVOL sur le lecteur D:\, installer les fichiers journaux sur le lecteur E:\, ont le serveur redémarre automatiquement après l’installation des services ADDS terminée et être invitée à fournir le mot de passe du Mode de restauration des Services d’annuaire:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>Effectuer une installation RODC intermédiaire à l’aide de Windows PowerShell  
La syntaxe de commande pour créer un compte RODC est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]  
```  
  
La syntaxe de commande pour attacher un serveur à un compte RODC est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Par exemple, pour créer un compte RODC nommé RODC1:  
  
```  
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA  
```  
  
Puis exécutez les commandes suivantes sur le serveur que vous voulez attacher au compte RODC1. Le serveur ne peut pas être joint au domaine. Tout d’abord, installez les outils de gestion et des rôles de serveur ADDS:  
  
```  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```  
  
L’exécution de la commande suivante pour créer le RODC:  
  
```  
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```  
  
Appuyez sur **Y** pour confirmer ou incluez le **«confirmer** argument afin d’éviter l’invite de confirmation.  
  
## <a name="BKMK_GUI"></a>L’installation des services ADDS à l’aide du Gestionnaire de serveur  
Les services ADDS peuvent être installés dans Windows Server2012 à l’aide de l’Assistant Ajout de rôles dans le Gestionnaire de serveur, suivie de l’Assistant de Configuration ActiveDirectory domaine Services, qui est nouvelle possibilité à compter de Windows Server2012. L’Assistant Installation de Services de domaine ActiveDirectory (dcpromo.exe) est déconseillée à compter de Windows Server2012.  
  
Les sections suivantes expliquent comment créer des pools de serveurs pour installer et gérer les services ADDS sur plusieurs serveurs et comment utiliser les Assistants pour installer les services ADDS.  
  
### <a name="BKMK_ServerPools"></a>Création de pools de serveurs  
Le Gestionnaire de serveur peut mettre en pool autres serveurs sur le réseau tant qu’ils sont accessibles à partir de l’ordinateur exécutant le Gestionnaire de serveur. Une fois mis en pool, vous choisissez de ces serveurs pour une installation à distance des services ADDS ou toute autre option de configuration possible dans le Gestionnaire de serveur. L’ordinateur exécutant le Gestionnaire de serveur automatiquement se met en pool. Pour plus d’informations sur les pools de serveurs, voir [ajouter des serveurs au Gestionnaire de serveur](https://technet.microsoft.com/library/hh831453.aspx).  
  
> [!NOTE]  
> Pour gérer un ordinateur joint au domaine à l’aide du Gestionnaire de serveur sur un serveur de groupe de travail, ou inversement, les étapes de configuration supplémentaires sont nécessaires. Pour plus d’informations, voir «Ajouter et gérer des serveurs dans des groupes de travail» dans [ajouter des serveurs au Gestionnaire de serveur](https://technet.microsoft.com/library/hh831453.aspx).  
  
### <a name="BKMK_installADDSGUI"></a>Installation des services ADDS  
**Informations d’identification d’administration**  
  
Les informations d’identification requises pour installer les services ADDS varient en fonction de la configuration de déploiement que vous choisissez. Pour plus d’informations, voir [des informations d’identification requises pour exécuter Adprep.exe et installer les Services de domaine ActiveDirectory](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
Utilisez les procédures suivantes pour installer les services ADDS à l’aide de l’interface graphique. Les étapes peuvent être effectuées localement ou à distance. Pour obtenir une explication plus détaillée de ces étapes, consultez les rubriques suivantes:  
  
-   [Déploiement d’une forêt avec le Gestionnaire de serveur](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Installer un contrôleur de domaine Windows Server2012 répliqué dans un domaine existant et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  
-   [Installer le nouveau Windows Server2012 ActiveDirectory enfant ou domaine d’arborescence et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
  
-   [Installer un serveur Windows Server2012 ActiveDirectory Read-Only le contrôleur de domaine et #40; rODC et #41; et #40; niveau 200 et #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)  
  
##### <a name="to-install-ad-ds-by-using-server-manager"></a>Pour installer les services ADDS à l’aide du Gestionnaire de serveur  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **gérer** et cliquez sur **Ajout de rôles et fonctionnalités** pour démarrer l’Assistant Ajout de rôles.  
  
2.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
3.  Sur le **sélectionner le type d’installation**, cliquez sur **installation basée sur un rôle ou une fonctionnalité** puis cliquez sur **suivant**.  
  
4.  Sur le **serveur de destination sélectionnez**, cliquez sur **sélectionner un serveur du pool de serveurs**, cliquez sur le nom du serveur sur lequel vous souhaitez installer les services ADDS, puis cliquez sur **suivant**.  
  
    Pour sélectionner des serveurs distants, tout d’abord créer un pool de serveurs et ajouter les serveurs distants à celui-ci. Pour plus d’informations sur la création de pools de serveurs, voir [ajouter des serveurs au Gestionnaire de serveur](https://technet.microsoft.com/library/hh831453.aspx).  
  
5.  Sur le **sélectionner des rôles de serveur**, cliquez sur **les Services de domaine ActiveDirectory**, puis sur le **Assistant Ajouter des rôles et fonctionnalités** boîte de dialogue, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des fonctionnalités**, sélectionnez les fonctionnalités supplémentaires que vous souhaitez installer, cliquez sur **suivant**.  
  
7.  Sur le **les Services de domaine ActiveDirectory** page, passez en revue les informations, puis sur **suivant**.  
  
8.  Sur le **confirmer les sélections d’installation** , cliquez sur **installer**.  
  
9. Sur le **résultats**, vérifiez que l’installation a réussi, puis cliquez sur **promouvoir ce serveur en contrôleur de domaine** pour démarrer l’Assistant Configuration des Services de domaine ActiveDirectory.  
  
    ![Installer les services ADDS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)  
  
    > [!IMPORTANT]  
    > Si vous fermez l’Assistant Ajout de rôles à ce stade sans démarrer l’Assistant Configuration des Services de domaine ActiveDirectory, vous pouvez le redémarrer en cliquant sur tâches dans le Gestionnaire de serveur.  
  
    ![Installer les services ADDS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
10. Sur le **Configuration de déploiement** page, choisissez l’une des options suivantes:  
  
    -   Si vous installez un contrôleur de domaine supplémentaire dans un domaine existant, cliquez sur **ajouter un contrôleur de domaine à un domaine existant**et tapez le nom du domaine (par exemple, emea.corp.contoso.com) ou cliquez sur **sélectionnez... **pour choisir un domaine et les informations d’identification (par exemple, spécifiez un compte qui est membre du groupe Admins du domaine), puis **suivant**.  
  
        > [!NOTE]  
        > Le nom du domaine et les informations d’identification utilisateur actuel sont fournis par défaut uniquement si l’ordinateur est joint au domaine et si vous effectuez une installation locale. Si vous installez les services ADDS sur un serveur distant, vous devez spécifier les informations d’identification, par conception. Si les informations d’identification utilisateur actuel ne sont pas suffisantes pour effectuer l’installation, cliquez sur **modifier... **afin d’indiquer les informations d’identification différentes.  
  
        Pour plus d’informations, voir [installer un contrôleur de domaine réplica Windows Server2012dans un domaine existant et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
    -   Si vous installez un nouveau domaine enfant, cliquez sur **ajouter un nouveau domaine à une forêt existante**, pour **sélectionner le type de domaine**, sélectionnez **domaine enfant**, tapez ou recherchez le nom du nom DNS de domaine parent (par exemple, corp.contoso.com), tapez le nom relatif du nouveau domaine enfant (par exemple, emea), tapez les informations d’identification à utiliser pour créer le nouveau domaine, puis cliquez sur **suivant**.  
  
        Pour plus d’informations, voir [installer le nouveau Windows Server2012 ActiveDirectory enfant ou domaine d’arborescence et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Si vous installez une nouvelle arborescence de domaine, cliquez sur **ajouter un nouveau domaine à une forêt existante**, pour **sélectionner le type de domaine**, choisissez **domaine d’arborescence**, tapez le nom du domaine racine (par exemple, corp.contoso.com), tapez le nom DNS du nouveau domaine (par exemple, fabrikam.com), tapez les informations d’identification à utiliser pour créer le nouveau domaine, puis cliquez sur **suivant**.  
  
        Pour plus d’informations, voir [installer le nouveau Windows Server2012 ActiveDirectory enfant ou domaine d’arborescence et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Si vous installez une nouvelle forêt, cliquez sur **ajouter une nouvelle forêt** puis tapez le nom du domaine racine (par exemple, corp.contoso.com).  
  
        Pour plus d’informations, voir [installer une nouvelle forêt Windows Server2012 ActiveDirectory et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
11. Sur le **Options du contrôleur de domaine** page, choisissez l’une des options suivantes:  
  
    -   Si vous créez une forêt ou un domaine, sélectionnez les niveaux fonctionnels de domaine et de forêt, cliquez sur **serveur du système DNS (Domain Name)**, spécifiez le mot de passe DSRM, puis cliquez sur **suivant**.  
  
    -   Si vous ajoutez un contrôleur de domaine à un domaine existant, cliquez sur **serveur du système DNS (Domain Name)**, **catalogue Global (GC)**, ou **lecture uniquement contrôleur de domaine (RODC)** en fonction des besoins, choisissez le nom du site et tapez le mot de passe DSRM, puis sur **suivant**.  
  
    Pour plus d’informations sur les options de cette page sont disponibles ou non dans des conditions différentes, voir [Options du contrôleur de domaine](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage).  
  
12. Sur le **Options DNS** (qui apparaît uniquement si vous installez un serveur DNS), cliquez sur **délégation DNS de mise à jour** en fonction des besoins. Si vous le faites, fournir des informations d’identification qui ont l’autorisation de création d’enregistrements de délégation DNS dans la zone DNS parente.  
  
    Si un serveur DNS qui héberge la zone parente ne peut pas être contacté, le **mise à jour la délégation DNS** option n’est pas disponible.  
  
    Pour plus d’informations sur la nécessité de mettre à jour la délégation DNS, voir [présentation la délégation de Zone](https://technet.microsoft.com/library/cc771640.aspx). Si vous tentez de mettre à jour la délégation DNS et une erreur se produit, voir [Options DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
13. Sur le **Options RODC** page (qui apparaît uniquement si vous installez un RODC), spécifiez le nom d’un groupe ou d’utilisateur qui sera gérer le RODC, ajoutez des comptes ou supprimer des comptes de groupes de réplication de mot de passe autorisé ou refusé, puis cliquez sur **suivant**.  
  
    Pour plus d’informations, voir [stratégie de réplication de mot de passe](https://technet.microsoft.com/library/cc730883(v=ws.10)).  
  
14. Sur le **Options supplémentaires** page, choisissez l’une des options suivantes:  
  
    -   Si vous créez un nouveau domaine, tapez un nouveau nom NetBIOS ou vérifiez le nom NetBIOS par défaut du domaine, puis cliquez sur **suivant**.  
  
    -   Si vous ajoutez un contrôleur de domaine à un domaine existant, sélectionnez le contrôleur de domaine que vous souhaitez répliquer les données d’installation d’ADDS (ou autorisez l’Assistant à sélectionner n’importe quel contrôleur de domaine). Si vous installez à partir du support, cliquez sur **installer à partir du chemin d’accès du support** et vérifiez le chemin d’accès aux fichiers source d’installation, puis tapez **suivant**.  
  
        Vous ne pouvez pas utiliser installer à partir du support (IFM) pour installer le premier contrôleur de domaine dans un domaine. IFM ne fonctionne pas entre les versions de système d’exploitation différent. En d’autres termes, pour pouvoir installer un contrôleur de domaine supplémentaire qui exécute Windows Server2012 à l’aide d’IFM, vous devez créer le support de sauvegarde sur un contrôleur de domaine Windows Server2012. Pour plus d’informations sur IFM, voir [l’installation d’un contrôleur de domaine supplémentaire à l’aide d’IFM](https://technet.microsoft.com/library/cc816722(WS.10).aspx).  
  
15. Sur le **chemins d’accès** page, tapez les emplacements de la base de données ActiveDirectory, le fichiers journaux et le dossier SYSVOL (ou acceptez les emplacements par défaut), puis cliquez sur **suivant**.  
  
    > [!IMPORTANT]  
    > Ne stockez pas la base de données ActiveDirectory, les fichiers journaux, ou le dossier SYSVOL sur un volume de données formatés avec le système de fichiers résilient (ReFS).  
  
16. Sur le **Options de préparation** page, les informations d’identification de type sont suffisantes pour exécuter adprep. Pour plus d’informations, voir [des informations d’identification requises pour exécuter Adprep.exe et installer les Services de domaine ActiveDirectory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
17. Sur le **examiner les Options**, confirmez vos sélections, cliquez sur **afficher le script** si vous souhaitez exporter les paramètres vers un script Windows PowerShell, puis cliquez sur **suivant**.  
  
18. Sur le **vérification des conditions requises** page, confirmez que la validation terminée, puis sur **installer**.  
  
19. Sur le **résultats**, vérifiez que le serveur a été correctement configuré comme un contrôleur de domaine. Le serveur redémarrera automatiquement pour terminer l’installation des services ADDS.  
  
## <a name="BKMK_UIStaged"></a>Exécution d’une Installation RODC intermédiaire à l’aide de l’Interface graphique utilisateur  
Une installation RODC intermédiaire vous permet de créer un RODC en deux étapes. Dans la première phase, un membre du groupe Admins du domaine crée un compte RODC. Dans la deuxième phase, un serveur est associé au compte RODC. La deuxième phase peut être effectuée par un membre du groupe Admins du domaine ou un groupe ou d’utilisateur de domaine délégué.  
  
#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>Pour créer un compte RODC à l’aide des outils d’administration ActiveDirectory  
  
1.  Vous pouvez créer le compte RODC à l’aide du centre d’administration ActiveDirectory ou des utilisateurs ActiveDirectory et des ordinateurs.  
  
    1.  Cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **centre d’administration ActiveDirectory**.  
  
    2.  Dans le volet de navigation (volet gauche), cliquez sur le nom du domaine.  
  
    3.  Dans la liste de gestion (volet central), cliquez sur le **contrôleurs de domaine** unité d’organisation.  
  
    4.  Dans le volet des tâches (volet droit), cliquez sur **créer au préalable un compte de contrôleur de domaine en lecture seule**.  
  
    - Ou -  
  
    1.  Cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **ActiveDirectory Users and Computers**.  
  
    2.  Soit avec le bouton droit le **contrôleurs de domaine** unité d’organisation (UO) ou cliquez sur le **contrôleurs de domaine** unité d’organisation, puis cliquez sur **Action**.  
  
    3.  Cliquez sur **créer au préalable Read-only le compte de contrôleur de domaine**.  
  
2.  Sur le **Bienvenue dans l’Assistant Installation des Services de domaine ActiveDirectory** page, si vous souhaitez modifier la valeur par défaut de la réplication de stratégie de mot de passe, sélectionnez **utilisez installation en mode avancé**, puis cliquez sur **suivant**.  
  
3.  Sur le **informations d’identification réseau** sous **spécifier les informations d’identification de compte à utiliser pour effectuer l’installation**, cliquez sur **informations d’identification enregistrées mon** ou cliquez sur **autres informations d’identification**, puis cliquez sur **définir**. Dans le **sécurité Windows** boîte de dialogue Entrez le nom d’utilisateur et mot de passe d’un compte qui peut installer le contrôleur de domaine supplémentaire. Pour installer un contrôleur de domaine supplémentaire, vous devez être membre du groupe Administrateurs de l’entreprise ou Admins du domaine. Lorsque vous avez terminé de fournir des informations d’identification, cliquez sur **suivant**.  
  
4.  Sur le **spécifier le nom d’ordinateur**, tapez le nom d’ordinateur du serveur qui sera le RODC.  
  
5.  Sur le **sélectionner un Site** page, sélectionnez un site dans la liste ou sélectionnez l’option pour installer le contrôleur de domaine dans le site qui correspond à l’adresse IP de l’ordinateur sur lequel vous exécutez l’Assistant, puis cliquez sur **suivant**.  
  
6.  Sur le **Options du contrôleur de domaine supplémentaire** page, effectuez les sélections suivantes, puis cliquez sur **suivant**:  
  
    -   **Serveur DNS**: cette option est sélectionnée par défaut pour que votre contrôleur de domaine puisse fonctionner comme un serveur de système DNS (Domain Name). Si vous ne souhaitez pas que le contrôleur de domaine soit un serveur DNS, désactivez cette option. Toutefois, si vous n’installez pas le rôle de serveur DNS sur le RODC et que ce dernier est le seul contrôleur de domaine dans la filiale, les utilisateurs de la filiale ne sera pas en mesure d’effectuer la résolution de noms lorsque le réseau étendu (WAN) au site hub est hors connexion.  
  
    -   **Catalogue global**: cette option est sélectionnée par défaut. Il ajoute le catalogue global, les partitions d’annuaire en lecture seule pour le contrôleur de domaine, et elle active la fonctionnalité de recherche de catalogue global. Si vous ne souhaitez pas que le contrôleur de domaine soit un serveur de catalogue global, désactivez cette option. Toutefois, si vous n’installez un serveur de catalogue global dans la filiale ou activer la mise en cache de l’appartenance au groupe universel pour le site qui inclut le RODC, les utilisateurs de la filiale ne sera pas en mesure de se connecter au domaine lorsque le réseau étendu au site hub est hors connexion.  
  
    -   **Contrôleur de domaine en lecture seule**. Lorsque vous créez un compte RODC, cette option est sélectionnée par défaut et vous ne pouvez pas le désactiver.  
  
7.  Si vous avez sélectionné le **utilisez installation en mode avancé** case à cocher sur le **Bienvenue** page, le **spécifier la stratégie de réplication de mot de passe** page s’affiche. Par défaut, aucun mot de passe de compte n’est répliqués dans le RODC, et comptes sensibles à la sécurité (par exemple, les membres du groupe Admins du domaine) sont explicitement refusés jamais être répliqués sur le RODC leurs mots de passe.  
  
    Pour ajouter d’autres comptes à la stratégie, cliquez sur **ajouter**, puis cliquez sur **autoriser les mots de passe pour le compte à répliquer vers ce RODC** ou cliquez sur **refuser des mots de passe pour le compte de réplication vers ce RODC**, puis sélectionnez les comptes.  
  
    Une fois terminée (ou acceptez le paramètre par défaut), cliquez sur **suivant**.  
  
8.  Sur le **Installation de la délégation de contrôleur de domaine et l’Administration** page, tapez le nom de l’utilisateur ou le groupe qui joindra le serveur au compte RODC que vous créez. Vous pouvez taper le nom de principal de sécurité qu’un seul.  
  
    Recherchez le répertoire pour un utilisateur ou groupe spécifique, cliquez sur **définir**. Dans **sélectionner utilisateur ou groupe**, tapez le nom de l’utilisateur ou le groupe. Nous vous recommandons de déléguer installation contrôleur de domaine et l’administration à un groupe.  
  
    Cet utilisateur ou groupe sera également disposer de droits d’administration locales sur le RODC après l’installation. Si vous ne spécifiez pas un utilisateur ou un groupe, seuls les membres du groupe Admins du domaine ou du groupe Administrateurs de l’entreprise seront en mesure de joindre le serveur au compte.  
  
    Lorsque vous avez terminé, cliquez sur **suivant**.  
  
9. Sur le **Résumé** page, passez en revue vos sélections. Cliquez sur **précédent** pour modifier les sélections, si nécessaire.  
  
    Pour enregistrer les paramètres que vous avez sélectionné dans un fichier de réponses que vous pouvez utiliser pour automatiser les prochaines opérations de domaine ActiveDirectory, cliquez sur **exporter les paramètres**. Tapez un nom pour votre fichier de réponses, puis cliquez sur **enregistrer**.  
  
    Lorsque vous êtes sûr que vos sélections sont correctes, cliquez sur **suivant** pour créer le compte RODC.  
  
10. Sur le **fin de l’Assistant Installation des Services de domaine ActiveDirectory**, cliquez sur **Terminer**.  
  
Une fois un compte RODC est créé, vous pouvez attacher un serveur au compte pour terminer l’installation du contrôleur de domaine. Cette deuxième phase peut être effectuée dans la filiale où se trouvera le RODC. Le serveur sur lequel vous effectuez cette procédure ne doit pas être joint au domaine. À compter de Windows Server2012, vous utilisez l’Assistant Ajout de rôles dans le Gestionnaire de serveur pour attacher un serveur à un compte RODC.  
  
#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>Pour attacher un serveur à un compte RODC à l’aide du Gestionnaire de serveur  
  
1.  Ouvrez une session en tant qu’administrateur local.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
3.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
4.  Sur le **sélectionner le type d’installation**, cliquez sur **installation basée sur un rôle ou une fonctionnalité** puis cliquez sur **suivant**.  
  
5.  Sur le **serveur de destination sélectionnez**, cliquez sur **sélectionner un serveur du pool de serveurs**, cliquez sur le nom du serveur sur lequel vous souhaitez installer les services ADDS, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des rôles de serveur**, cliquez sur **les Services de domaine ActiveDirectory**, cliquez sur **ajouter des fonctionnalités** puis cliquez sur **suivant**.  
  
7.  Sur le **sélectionner des fonctionnalités**, sélectionnez les fonctionnalités supplémentaires que vous souhaitez installer, cliquez sur **suivant**.  
  
8.  Sur le **les Services de domaine ActiveDirectory** page, passez en revue les informations, puis sur **suivant**.  
  
9. Sur le **confirmer les sélections d’installation** , cliquez sur **installer**.  
  
10. Sur le **résultats**, vérifiez **Installation a réussi**, puis cliquez sur **promouvoir ce serveur en contrôleur de domaine** pour démarrer l’Assistant Configuration des Services de domaine ActiveDirectory.  
  
    > [!IMPORTANT]  
    > Si vous fermez l’Assistant Ajout de rôles à ce stade sans démarrer l’Assistant Configuration des Services de domaine ActiveDirectory, vous pouvez le redémarrer en cliquant sur tâches dans le Gestionnaire de serveur.  
  
    (media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
11. Sur le **Configuration de déploiement**, cliquez sur **ajouter un contrôleur de domaine à un domaine existant**, tapez le nom du domaine (par exemple, emea.contoso.com) et les informations d’identification (par exemple, spécifiez un compte qui est délégué pour gérer et installer le RODC), puis cliquez sur **suivant**.  
  
12. Sur le **Options du contrôleur de domaine**, cliquez sur **utiliser le compte RODC existant**, tapez et confirmez le mot de passe du Mode de restauration des Services d’annuaire, puis cliquez sur **suivant**.  
  
13. Sur le **Options supplémentaires** page, si vous installez à partir du support, cliquez sur **installer à partir du chemin d’accès du support** tapez et vérifiez le chemin d’accès aux fichiers source d’installation, sélectionnez le contrôleur de domaine que vous souhaitez répliquer les données d’installation d’ADDS (ou autorisez l’Assistant à sélectionner n’importe quel contrôleur de domaine), puis **suivant**.  
  
14. Sur le **chemins d’accès** page, tapez les emplacements de la base de données ActiveDirectory, les fichiers journaux et dossier SYSVOL, ou acceptez les emplacements par défaut, puis cliquez sur **suivant**.  
  
15. Sur le **examiner les Options**, confirmez vos sélections, cliquez sur **afficher le Script** pour exporter les paramètres vers un script Windows PowerShell, puis cliquez sur **suivant**.  
  
16. Sur le **vérification des conditions requises** page, confirmez que la validation terminée, puis sur **installer**.  
  
    Pour terminer l’installation des services ADDS, le serveur redémarre automatiquement.  
  
## <a name="see-also"></a>Voir aussi  
[Résolution des problèmes de déploiement de contrôleur de domaine](Troubleshooting-Domain-Controller-Deployment.md)  
[Installer une nouvelle forêt d’ActiveDirectory Windows Server2012 et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)  
[Installer le nouveau Windows Server2012 ActiveDirectory enfant ou domaine d’arborescence et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
[Installer un contrôleur de domaine Windows Server2012 répliqué dans un domaine existant et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  



