---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: Installer Active Directory Domain Services (niveau 100)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fcb6d90832ed032302ceb0b3c4ec6a0eaff7d807
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886870"
---
# <a name="install-active-directory-domain-services-level-100"></a>Installer Active Directory Domain Services (niveau 100)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique explique comment installer les services AD DS dans Windows Server 2012 en utilisant l’une des méthodes suivantes :  
  
-   [Informations d’identification requises pour exécuter Adprep.exe et installer les Services de domaine Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)  
  
-   [Installation des services AD DS à l’aide de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)  
  
-   [L’installation des services AD DS à l’aide du Gestionnaire de serveur](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)  
  
-   [Effectuez une Installation RODC intermédiaire à l’aide de l’Interface utilisateur graphique](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)  
  
## <a name="BKMK_Creds"></a>Informations d’identification requises pour exécuter Adprep.exe et installer les Services de domaine Active Directory  
Les informations d’identification suivantes sont requises pour exécuter Adprep.exe et installer les services AD DS.  
  
-   Pour installer une nouvelle forêt, vous devez avoir ouvert une session en tant qu’administrateur local de l’ordinateur.  
  
-   Pour installer un nouveau domaine enfant ou une nouvelle arborescence de domaine, vous devez avoir ouvert une session en tant que membre du groupe Administrateurs de l’entreprise.  
  
-   Pour installer un autre contrôleur de domaine dans un domaine existant, vous devez être membre du groupe Admins du domaine.  
  
    > [!NOTE]  
    > Si vous n’exécutez pas de commande adprep.exe séparément et que vous installez le premier contrôleur de domaine qui exécute Windows Server 2012 dans un domaine existant ou d’une forêt, vous devrez fournir les informations d’identification pour exécuter les commandes Adprep. Les informations d’identification requises sont les suivantes :  
    >   
    > -   Pour introduire le premier contrôleur de domaine de Windows Server 2012 dans la forêt, vous devez fournir les informations d’identification d’un membre du groupe Administrateurs de l’entreprise, le groupe Administrateurs du schéma, et les administrateurs de domaine groupe du domaine qui héberge le contrôleur de schéma.  
    > -   Pour introduire le premier contrôleur de domaine de Windows Server 2012 dans un domaine, vous devez fournir des informations d’identification d’un membre du groupe Admins du domaine.  
    > -   Pour introduire le premier contrôleur de domaine en lecture seule (RODC) dans la forêt, vous devez fournir les informations d’identification d’un membre du groupe Administrateurs de l’entreprise.  
    >   
    >     > [!NOTE]  
    >     > Si vous avez déjà exécuté adprep /rodcprep dans Windows Server 2008 ou Windows Server 2008 R2, il est inutile pour l’exécuter à nouveau pour Windows Server 2012.  
  
## <a name="BKMK_PS"></a>Installation des services AD DS à l’aide de Windows PowerShell  
À compter de Windows Server 2012, vous pouvez installer les services AD DS à l’aide de Windows PowerShell. Dcpromo.exe est déconseillée à compter de Windows Server 2012, mais vous pouvez toujours exécuter dcpromo.exe à l’aide d’un fichier de réponses (dcpromo / unattend :<answerfile> ou dcpromo/answer :<answerfile>). La possibilité d’exécuter dcpromo.exe avec un fichier de réponses donne aux organisations ayant investi des ressources dans la création d’une automatisation avec dcpromo.exe le temps de convertir l’automatisation existante vers Windows PowerShell. Pour plus d’informations sur l’exécution de dcpromo.exe avec un fichier de réponses, consultez [ https://support.microsoft.com/kb/947034 ](https://support.microsoft.com/kb/947034).  
  
Pour plus d’informations sur la suppression des services AD DS à l’aide de Windows PowerShell, voir [Supprimer les services AD DS à l’aide de Windows PowerShell](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS).  
  
Commencez par ajouter le rôle à l’aide de Windows PowerShell. Cette commande installe le rôle serveur AD DS ainsi que les outils d’administration de serveur AD DS et AD LDS, y compris des outils basés sur une interface utilisateur graphique tels qu’Utilisateurs et ordinateurs Active Directory et des outils de ligne de commande tels que dcdia.exe. Les outils d’administration de serveur ne sont pas installés par défaut lorsque vous utilisez Windows PowerShell. Vous devez spécifier **» IncludeManagementTools** pour gérer le serveur local ou installer [outils d’Administration de serveur distant](https://www.microsoft.com/download/details.aspx?id=28972) pour gérer un serveur distant.  
  
```  
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools  
<<Windows PowerShell cmdlet and arguments>>  
```  
  
Aucun redémarrage n’est requis avant la fin de l’installation des services AD DS.  
  
Vous pouvez alors exécuter cette commande pour voir les applets de commande disponibles dans le module ADDSDeployment.  
  
```  
Get-Command -Module ADDSDeployment
```  
  
Pour afficher la liste des arguments que vous pouvez spécifier pour les applets de commande et la syntaxe à utiliser :  
  
```  
Get-Help <cmdlet name>  
```  
  
Par exemple, pour afficher les arguments à utiliser pour créer un compte de contrôleur de domaine en lecture seule (RODC) inoccupé, tapez :  
  
```  
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```  
  
Les arguments facultatifs apparaissent entre crochets.  
  
Vous pouvez également télécharger les derniers exemples et concepts d’aide pour les applets de commande Windows PowerShell. Pour plus d’informations, voir [about_Updatable_Help](https://technet.microsoft.com/library/hh847735.aspx).  
  
Vous pouvez exécuter les applets de commande Windows PowerShell sur des serveurs distants :  
  
-   Dans Windows PowerShell, utilisez Invoke-Command avec l’applet de commande ADDSDeployment. Par exemple, pour installer les services AD DS sur un serveur distant nommé ConDC3 dans le domaine contoso.com, tapez :  
  
    ```  
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3  
    ```  
  
- ou -  
  
-   Dans le Gestionnaire de serveur, créez un groupe de serveurs qui inclut le serveur distant. Cliquez avec le bouton droit sur le nom du serveur distant, puis cliquez sur **Windows PowerShell**.  
  
Les sections suivantes expliquent comment exécuter les applets de commande du module ADDSDeployment pour installer les services AD DS.  
  
-   [Arguments de l’applet de commande ADDSDeployment](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)  
  
-   [Spécification des informations d’identification de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)  
  
-   [À l’aide des applets de commande de test](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)  
  
-   [L’installation d’un nouveau domaine racine à l’aide de Windows PowerShell](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)  
  
-   [Installer un nouveau domaine enfant ou une arborescence à l’aide de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)  
  
-   [Installation d’un contrôleur de domaine supplémentaire (réplica) à l’aide de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)  
  
### <a name="BKMK_Params"></a>Arguments de l’applet de commande ADDSDeployment  
Le tableau suivant répertorie les arguments de l’applet de commande ADDSDeployment dans Windows PowerShell. Les arguments en gras sont requis. Les arguments équivalents pour dcpromo.exe sont répertoriés entre parenthèses s’ils sont nommés différemment dans Windows PowerShell.  
  
Les commutateurs Windows PowerShell acceptent les arguments $TRUE ou $FALSE. Les arguments qui sont $TRUE par défaut n’avez pas besoin de la définir.  
  
Pour remplacer des valeurs par défaut, vous pouvez spécifier l’argument avec une valeur $False. Par exemple, l’argument **-installdns** étant exécuté automatiquement lors de l’installation d’une nouvelle forêt même s’il n’est pas spécifié, la seule façon d’*empêcher* l’installation DNS lorsque vous installez une nouvelle forêt est d’utiliser ce qui suit :  
  
```  
-InstallDNS:$false  
```  
  
De même, étant donné que **» installdns** a la valeur par défaut $False si vous installez un contrôleur de domaine dans un environnement qui n’héberge pas de serveur Windows DNS server, vous devez spécifier l’argument suivant pour installer le serveur DNS :  
  
```  
-InstallDNS:$true  
```  
  
|Argument|Description|  
|------------|---------------|  
|**ADPrepCredential <PS Credential>**  **Remarque :** Requis si vous installez le premier contrôleur de domaine Windows Server 2012 dans un domaine ou forêt et les informations d’identification de l’utilisateur actuel sont insuffisantes pour effectuer l’opération.|Spécifie le compte appartenant au groupe Administrateurs de l’entreprise et Administrateurs du schéma qui peut préparer la forêt selon les règles de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) et un objet PSCredential.<br /><br />Si aucune valeur n’est spécifiée, la valeur de la **« informations d’identification** argument est utilisé.|  
|AllowDomainControllerReinstall|Indique si l’installation de ce contrôleur de domaine accessible en écriture doit se poursuivre bien qu’un autre compte de contrôleur de domaine accessible en écriture du même nom ait été détecté.<br /><br />Utilisez **$True** uniquement si vous êtes sûr que le compte n’est pas actuellement utilisé par un autre contrôleur de domaine accessible en écriture.<br /><br />La valeur par défaut est **$False**.<br /><br />Cet argument n’est pas valide pour un contrôleur de domaine en lecture seule.|  
|AllowDomainReinstall|Indique si un domaine existant est recréé.<br /><br />La valeur par défaut est **$False**.|  
|AllowPasswordReplicationAccountName <chaîne_[]>|Spécifie les noms des comptes d’utilisateur, des comptes de groupe et des comptes d’ordinateur dont les mots de passe peuvent être répliqués sur ce contrôleur de domaine en lecture seule. Utilisez une chaîne vide "" si vous voulez que la valeur reste vide. Par défaut, seul le « Groupe de réplication dont le mot de passe RODC est autorisé » est autorisé et il est vide à l’origine.<br /><br />Spécifiez les valeurs sous forme d’un tableau de chaînes. Exemple :<br /><br />Code -AllowPasswordReplicationAccountName "JSmith","JSmithPC","Branch Users"|  
|ApplicationPartitionsToReplicate < string [] > **Remarque :** Aucune option équivalente n’est disponible dans l’interface utilisateur. Si vous procédez à l’installation à l’aide de l’interface utilisateur ou de l’option Installation à partir du support, toutes les partitions d’application sont alors répliquées.|Spécifie les partitions de l’annuaire d’applications à répliquer. Cet argument est uniquement appliqué lorsque vous spécifiez l’argument **-InstallationMediaPath** pour effectuer l’installation à partir du support. Par défaut, toutes les partitions d’application sont répliquées selon leur propre étendue.<br /><br />Spécifiez les valeurs sous forme d’un tableau de chaînes. Exemple :<br /><br />Code -<br /><br />-ApplicationPartitionsToReplicate "partition1","partition2","partition3"|  
|Confirm|Votre confirmation sera requise avant l’exécution de l’applet de commande.|  
|CreateDnsDelegation **Remarque :** Vous ne pouvez pas spécifier cet argument lorsque vous exécutez l’applet de commande Add-ADDSReadOnlyDomainController.|Indique si une délégation DNS qui référence le nouveau serveur DNS que vous installez avec le contrôleur de domaine doit être créée. Valide pour « intégré à Active Directory DNS uniquement. Les enregistrements de délégation ne peuvent être créés que sur des serveurs DNS Microsoft qui sont en ligne et accessibles. Il est impossible de créer des enregistrements de délégation pour des domaines qui sont immédiatement subordonnés à des domaines de premier niveau tels que .com, .gov, .biz, .edu ou à des domaines dont l’indicatif de pays comporte deux lettres, tels que .nz et .au.<br /><br />La valeur par défaut est calculée automatiquement en fonction de l’environnement.|  
|**Informations d’identification <PS Credential>**  **Remarque :** Requis uniquement si les informations d’identification de l’utilisateur actuel sont insuffisantes pour effectuer l’opération.|Spécifie le compte de domaine qui peut ouvrir une session au domaine selon les règles de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) et un objet PSCredential.<br /><br />Si aucune valeur n’est spécifiée, les informations d’identification de l’utilisateur actuel sont utilisées.|  
|CriticalReplicationOnly|Spécifie si l’opération d’installation AD DS effectue uniquement la réplication critique avant le redémarrage, puis continue. La réplication non critique a lieu au terme de l’installation et après le redémarrage de l’ordinateur.<br /><br />L’utilisation de cet argument n’est pas recommandée.<br /><br />Aucune option équivalente n’est disponible dans l’interface utilisateur.|  
|DatabasePath <string>|Spécifie le qualifié complet, non « chemin d’accès UNC Universal Naming Convention () dans un répertoire sur un disque fixe de l’ordinateur local qui contient la base de données de domaine, par exemple, **C:\Windows\NTDS.**<br /><br />La valeur par défaut est **%SYSTEMROOT%\NTDS**. **Important :** Alors que vous pouvez stocker la base de données et les fichiers journaux AD DS sur un volume formaté avec le système ReFS (Resilient File System), l’hébergement des services AD DS sur ReFS ne présente pas d’avantage particulier autre que les avantages classiques de la résilience dont vous bénéficiez en hébergeant n'importe quelle donnée sur ReFS.|  
|DelegatedAdministratorAccountName <string>|Spécifie le nom de l’utilisateur ou du groupe pouvant installer et gérer le contrôleur de domaine en lecture seule.<br /><br />Par défaut, seuls les membres du groupe Admins du domaine peuvent gérer un contrôleur de domaine en lecture seule.|  
|DenyPasswordReplicationAccountName <chaîne_[]>|Spécifie les noms des comptes d’utilisateur, des comptes de groupe et des comptes d’ordinateur dont les mots de passe ne doivent pas être répliqués sur ce contrôleur de domaine en lecture seule. Utilisez une chaîne vide "" si vous ne voulez refuser la réplication des informations d’identification d’aucun utilisateur ou ordinateur. Par défaut, Administrateurs, Opérateurs de serveur, Opérateurs de sauvegarde, Opérateurs de compte et Groupe de réplication dont le mot de passe RODC est refusé sont refusés. Par défaut, le Groupe de réplication dont le mot de passe RODC est refusé inclut Éditeurs de certificats, Admins du domaine, Administrateurs de l’entreprise, Contrôleurs de domaine d’entreprise, Contrôleurs de domaine d’entreprise en lecture seule, Propriétaires créateurs de la stratégie de groupe, le compte krbtgt et Administrateurs du schéma.<br /><br />Spécifiez les valeurs sous forme d’un tableau de chaînes. Exemple :<br /><br />Code -<br /><br />-DenyPasswordReplicationAccountName « RegionalAdmins », « AdminPCs »|  
|DnsDelegationCredential <PS Credential> **Remarque :** Vous ne pouvez pas spécifier cet argument lorsque vous exécutez l’applet de commande Add-ADDSReadOnlyDomainController.|Spécifie le nom d’utilisateur et le mot de passe pour créer la délégation DNS selon les règles de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) et un objet PSCredential.|  
|DomainMode <DomainMode> {Win2003 &#124; Win2008 &#124; Win2008R2 &#124; Win2012 &#124; Win2012R2}<br /><br />Ou<br /><br />DomainMode <DomainMode> {2 &#124; 3 &#124; 4 &#124; 5 &#124; 6}|Spécifie le niveau fonctionnel du domaine au cours de la création d’un domaine.<br /><br />Le niveau fonctionnel du domaine ne peut pas être inférieur à celui de la forêt, mais il peut être supérieur.<br /><br />La valeur par défaut est automatiquement calculée et correspond au niveau fonctionnel existant de la forêt ou à la valeur définir pour **-ForestMode**.|  
|**DomainName**<br /><br />Requis pour les applets de commande Install-ADDSForest et Install-ADDSDomainController.|Spécifie le nom de domaine complet du domaine dans lequel vous voulez installer un contrôleur de domaine supplémentaire.|  
|**DomainNetbiosName <string>**<br /><br />Requis pour Install-ADDSForest si le nom du préfixe du nom de domaine complet comporte plus de 15 caractères.|À utiliser avec Install-ADDSForest. Attribue un nom NetBIOS au nouveau domaine racine de forêt.|  
|DomainType <DomainType> {ChildDomain &#124; TreeDomain} ou {enfant &#124; arborescence}|Indique le type de domaine que vous voulez créer : une nouvelle arborescence de domaine dans une forêt existante, un enfant d’un domaine existant ou une nouvelle forêt.<br /><br />La valeur par défaut est ChildDomain.|  
|Force|Lorsque ce paramètre est spécifié, tous les avertissements susceptibles d’apparaître normalement lors de l’installation et de l’ajout du contrôleur de domaine sont supprimés pour permettre à l’applet de commande de terminer son exécution. Il peut être utile d’inclure ce paramètre dans le cadre d’une installation avec un script.|  
|ForestMode <ForestMode> {Win2003 &#124; Win2008 &#124; Win2008R2 &#124; Win2012 &#124; Win2012R2}<br /><br />Ou<br /><br />ForestMode <ForestMode> {2 &#124; 3 &#124; 4 &#124; 5 &#124; 6}|Spécifie le niveau fonctionnel de la forêt lorsque vous créez une forêt.<br /><br />La valeur par défaut est Win2012.|  
|InstallationMediaPath|Indique l’emplacement du support d’installation qui sera utilisé pour installer un nouveau contrôleur de domaine.|  
|InstallDNS|Spécifie si le service Serveur DNS doit être installé et configuré sur le contrôleur de domaine.<br /><br />Pour une nouvelle forêt, la valeur par défaut est **$True** et le service Serveur DNS est installé.<br /><br />Pour un nouveau domaine enfant ou une nouvelle arborescence de domaine, si le domaine parent (ou le domaine racine de forêt pour une arborescence de domaine) héberge et stocke déjà les noms DNS pour le domaine, la valeur par défaut pour ce paramètre est $True.<br /><br />Dans le cadre de l’installation d’un contrôleur de domaine dans un domaine existant, si ce paramètre n’est pas spécifié et que le domaine actuel héberge et stocke déjà les noms DNS pour le domaine, la valeur par défaut pour ce paramètre est alors **$True**. Dans le cas contraire, si les noms de domaine DNS sont hébergés en dehors d’Active Directory, la valeur par défaut est **$False** et aucun serveur DNS n’est installé.|  
|LogPath <string>|Spécifie le chemin d’accès complet non UNC à un répertoire sur un disque fixe de l’ordinateur local contenant les fichiers journaux du domaine. Par exemple, **C:\Windows\Logs**.<br /><br />La valeur par défaut est **%SYSTEMROOT%\NTDS**. **Important :** Ne stockez pas les fichiers journaux Active Directory sur un volume de données au format ReFS (Resilient File System).|  
|MoveInfrastructureOperationMasterRoleIfNecessary|Spécifie s’il faut transférer le rôle de maître d’opérations maître infrastructure (également appelés opérations à maître unique ou FSMO) au contrôleur de domaine que vous créez « dans les cas il est actuellement hébergé sur un serveur de catalogue global » et que vous ne souhaitez pas Vérifiez le contrôleur de domaine que vous créez un serveur de catalogue global. Spécifiez ce paramètre pour transférer le rôle de maître d’infrastructure sur le contrôleur de domaine que vous créez si le transfert est nécessaire ; dans ce cas, spécifiez l’option **NoGlobalCatalog** si vous voulez que le rôle de maître d’infrastructure demeure à son emplacement actuel.|  
|**NewDomainName <string>**  **Remarque :** Requis uniquement pour Install-ADDSDomain.|Spécifie le nom de domaine unique pour le nouveau domaine.<br /><br />Par exemple, si vous voulez créer un nouveau domaine enfant nommé **emea.corp.fabrikam.com**, spécifiez **emea** comme valeur de cet argument.|  
|**NewDomainNetbiosName <string>**<br /><br />Requis pour Install-ADDSDomain si le nom du préfixe du nom de domaine complet comporte plus de 15 caractères.|À utiliser avec Install-ADDSDomain. Attribue un nom NetBIOS au nouveau domaine. La valeur par défaut est dérivée de la valeur de **» NewDomainName**.|  
|NoDnsOnNetwork|Spécifie que le service DNS n’est pas disponible sur le réseau. Ce paramètre est uniquement utilisé lorsque le paramètre IP de la carte réseau de cet ordinateur n’est pas configuré avec le nom d’un serveur DNS à des fins de résolution de noms. Il indique que le serveur DNS sera installé sur cet ordinateur à des fins de résolution de noms. Sinon, les paramètres IP de la carte réseau doivent être au préalable configurés avec l’adresse d’un serveur DNS.<br /><br />Si ce paramètre est omis (par défaut), cela signifie que les paramètres du client TCP/IP de la carte réseau sur cet ordinateur serveur seront utilisés pour contacter un serveur DNS. Par conséquent, si vous ne spécifiez pas ce paramètre, vérifiez que les paramètres du client TCP/IP sont au préalable configurés avec l’adresse d’un serveur DNS préféré.|  
|NoGlobalCatalog|Indique que vous ne voulez pas que le contrôleur de domaine soit un serveur de catalogue global.<br /><br />Les contrôleurs de domaine qui exécutent Windows Server 2012 sont installés avec le catalogue global par défaut. En d’autres termes, l’exécution a lieu automatiquement sans calcul, sauf si vous spécifiez :<br /><br />Code -<br /><br />-NoGlobalCatalog|  
|NoRebootOnCompletion|Spécifie si l’ordinateur doit être redémarré à la fin d’une commande, qu’elle ait abouti ou non. Par défaut, l’ordinateur redémarre. Pour empêcher le serveur de redémarrer, spécifiez :<br /><br />Code -<br /><br />-NoRebootOnCompletion:$True<br /><br />Aucune option équivalente n’est disponible dans l’interface utilisateur.|  
|**ParentDomainName <string>**  **Remarque :** Requis pour l’applet de commande Install-ADDSDomain.|Spécifie le nom de domaine complet d’un domaine parent existant. Vous utilisez cet argument lorsque vous installez un domaine d’enfant ou une nouvelle arborescence de domaine.<br /><br />Par exemple, si vous voulez créer un nouveau domaine enfant nommé **emea.corp.fabrikam.com**, spécifiez **corp.fabrikam.com** comme valeur de cet argument.|  
|ReadOnlyReplica|Spécifie si un contrôleur de domaine en lecture seule doit être installé.|  
|ReplicationSourceDC <string>|Indique le nom de domaine complet du contrôleur de domaine partenaire à partir duquel vous répliquez les informations du domaine. La valeur par défaut est automatiquement calculée.|  
|**SafeModeAdministratorPassword <securestring>**|Fournit le mot de passe du compte d’administrateur lorsque l’ordinateur est démarré en mode sans échec ou dans une variante du mode sans échec, par exemple le mode Restauration des services d’annuaire.<br /><br />La valeur par défaut est un mot de passe vide. Vous devez fournir un mot de passe. Le mot de passe doit être spécifié au format System.Security.SecureString, comme celui fourni par read-host -assecurestring ou ConvertTo-SecureString.<br /><br />Le fonctionnement de l’argument SafeModeAdministratorPassword est spécial : s’il n’est pas spécifié en tant qu’argument, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande. S’il est spécifié sans valeur et qu’aucun autre argument n’est spécifié pour l’applet de commande, cette dernière vous invite à entrer un mot de passe masqué sans confirmation. Il ne s’agit pas du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande. S’il est spécifié avec une valeur, la valeur doit être une chaîne sécurisée. Il ne s’agit pas du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande. Par exemple, vous pouvez manuellement inviter l’utilisateur à entrer un mot de passe sous forme d’une chaîne sécurisée à l’aide de l’applet de commande Read-Host : -safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring). Vous pouvez également fournir une chaîne sécurisée sous forme d’une variable en texte clair convertie, bien que ceci soit fortement déconseillé. -safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)|  
|**SiteName <string>**<br /><br />Requis pour l’applet de commande Add-addsreadonlydomaincontrolleraccount|Spécifie le site dans lequel le contrôleur de domaine sera installé. Il existe aucune **« sitename** argument lorsque vous exécutez **Install-ADDSForest** , car le premier site créé est Default-First-Site-Name.<br /><br />Le nom du site doit déjà exister s’il est fourni en tant qu’argument à **-sitename**. L’applet de commande ne crée pas le site.|  
|SkipAutoConfigureDNS|Ignore la configuration automatique des paramètres des clients DNS, des redirecteurs et des indications de racine. Cet argument est uniquement en vigueur si le service Serveur DNS est déjà installé ou automatiquement installé avec **-InstallDNS**.|  
|SystemKey <string>|Spécifie la clé système du support à partir duquel vous répliquez les données.<br /><br />La valeur par défaut est **none**.<br /><br />Les données doivent être au format fourni par read-host -assecurestring ou ConvertTo-SecureString.|  
|SysvolPath <string>|Spécifie le chemin d’accès complet non UNC à un répertoire sur un disque fixe de l’ordinateur local. Par exemple, **C:\Windows\SYSVOL**.<br /><br />La valeur par défaut est **%SYSTEMROOT%\SYSVOL**. **Important :** SYSVOL ne peut pas être stocké sur un volume de données au format ReFS (Resilient File System).|  
|SkipPreChecks|N’exécute pas les vérifications de la configuration requise avant le début de l’installation. Il n’est pas conseillé d’utiliser ce paramètre.|  
|Whatif|Présente les conséquences éventuelles de l’exécution de l’applet de commande. L’applet de commande n’est pas exécutée.|  
  
### <a name="BKMK_PSCreds"></a>Spécification des informations d’identification de Windows PowerShell  
Vous pouvez spécifier des informations d’identification sans les divulguer en texte brut à l’écran à l’aide de [Get-credential](https://technet.microsoft.com/library/dd315327.aspx).  
  
Le fonctionnement des arguments -SafeModeAdministratorPassword et LocalAdministratorPassword est spécial :  
  
-   S’ils ne sont pas spécifiés en tant qu’arguments, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.  
  
-   S’ils sont spécifiés avec une valeur, la valeur doit être une chaîne sécurisée. Il ne s’agit pas du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.  
  
Par exemple, vous pouvez manuellement inviter l’utilisateur à entrer un mot de passe sous forme d’une chaîne sécurisée à l’aide de l’applet de commande **Read-Host** .  
  
```  
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```  
  
> [!WARNING]  
> Étant donné que l’option précédente ne confirme pas le mot de passe, faites preuve de prudence, car le mot de passe n’est pas visible.  
  
Vous pouvez également fournir une chaîne sécurisée sous forme d’une variable en texte clair convertie, bien que ceci soit fortement déconseillé.  
  
```  
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```  
  
> [!WARNING]  
> Il n’est pas recommandé de fournir ou de stocker un mot de passe en texte clair. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe DSRM de ce contrôleur de domaine. Munie de cette information, elle peut alors emprunter l’identité du contrôleur de domaine elle-même et élever son privilège au niveau le plus élevé d’une forêt Active Directory.  
  
### <a name="BKMK_TestCmdlets"></a>À l’aide des applets de commande de test  
Chaque applet de commande ADDSDeployment est associée à une applet de commande de test. Les applets de commande de test exécutent uniquement les vérifications de la configuration requise pour l’opération d’installation ; aucun paramètre d’installation n’est configuré. Les arguments pour chaque applet de commande de test sont les mêmes que pour l’applet de commande installation correspondante, mais **» SkipPreChecks** n’est pas disponible pour les applets de commande de test.  
  
|Applet de commande de test|Description|  
|---------------|---------------|  
|Test-ADDSForestInstallation|Exécute les vérifications de la configuration requise pour installer une nouvelle forêt Active Directory.|  
|Test-ADDSDomainInstallation|Exécute les vérifications de la configuration requise pour installer un nouveau domaine dans Active Directory.|  
|Test-ADDSDomainControllerInstallation|Exécute les vérifications de la configuration requise pour installer un contrôleur de domaine dans Active Directory.|  
|Test-ADDSReadOnlyDomainControllerAccountCreation|Exécute les vérifications de la configuration requise pour ajouter un compte de contrôleur de domaine en lecture seule.|  
  
### <a name="BKMK_PSForest"></a>L’installation d’un nouveau domaine racine à l’aide de Windows PowerShell  
La syntaxe de commande pour installer une nouvelle forêt est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> L’argument -DomainNetBIOSName est requis si vous voulez modifier le nom de 15 caractères qui est automatiquement généré en fonction du préfixe du nom de domaine DNS ou si le nom fait plus de 15 caractères.  
  
Par exemple, pour installer une nouvelle forêt nommée corp.contoso.com et inviter l’utilisateur à entrer de manière sécurisée le mot de passe DSRM, tapez :  
  
```  
Install-ADDSForest -DomainName "corp.contoso.com"   
```  
  
> [!NOTE]  
> Le serveur DNS est installé par défaut lorsque vous exécutez Install-ADDSForest.  
  
Pour installer une nouvelle forêt nommée corp.contoso.com, créer une délégation DNS dans le domaine contoso.com, définir Windows Server 2008 R2 comme niveau fonctionnel du domaine et définir Windows Server 2008 comme niveau fonctionnel de la forêt, installer la base de données Active Directory et SYSVOL sur le lecteur D:\, installer les fichiers journaux sur le lecteur E:\ et inviter l’utilisateur à fournir le mot de passe du mode de restauration des services d’annuaire (DSRM), tapez :  
  
```  
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="BKMK_PSDomain"></a>Installer un nouveau domaine enfant ou une arborescence à l’aide de Windows PowerShell  
La syntaxe de commande pour installer un nouveau domaine est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> L’argument **-credential** est uniquement requis lorsque vous n’êtes pas actuellement connecté en tant que membre du groupe Administrateurs de l’entreprise.  
>   
> L’argument **-NewDomainNetBIOSName** est requis si vous voulez modifier le nom de 15 caractères automatiquement généré en fonction du préfixe du nom de domaine DNS ou si le nom fait plus de 15 caractères.  
  
Par exemple, pour utiliser les informations d’identification de corp\EnterpriseAdmin1 pour créer un domaine enfant nommé child.corp.contoso.com, installer Serveur DNS, créer une délégation DNS dans le domaine corp.contoso.com, définir Windows Server 2003 comme niveau fonctionnel du domaine, configurer le contrôleur de domaine en tant que serveur de catalogue global dans un site nommé Houston, utiliser DC1.corp.contoso.com comme contrôleur de domaine source de réplication, installer la base de données Active Directory et SYSVOL sur le lecteur D:\, installer les fichiers journaux sur le lecteur E:\ et inviter l’utilisateur à fournir le mot de passe du mode de restauration des services d’annuaire (DSRM) sans lui demander de confirmer la commande, tapez :  
  
```  
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False  
```  
  
### <a name="BKMK_PSReplica"></a>Installation d’un contrôleur de domaine supplémentaire (réplica) à l’aide de Windows PowerShell  
La syntaxe de commande pour installer un contrôleur de domaine supplémentaire est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Pour installer un contrôleur de domaine et un serveur DNS dans le domaine corp.contoso.com et inviter l’utilisateur à entrer les informations d’identification de l’administrateur du domaine et le mot de passe DSRM, tapez :  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```  
  
Si l’ordinateur est déjà joint à un domaine et que vous êtes membre du groupe Admins du domaine, vous pouvez utiliser :  
  
```  
Install-ADDSDomainController -DomainName "corp.contoso.com"  
```  
  
Pour inviter l’utilisateur à entrer le nom du domaine, tapez :  
  
```  
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```  
  
La commande suivante utilise les informations d’identification de Contoso\EnterpriseAdmin1 pour installer un contrôleur de domaine accessible en écriture et un serveur de catalogue global dans un site nommé Boston, installer le serveur DNS, créer une délégation DNS dans le domaine contoso.com, effectuer l’installation à partir du support stocké dans le dossier c:\ADDS IFM, installer la base de données Active Directory et SYSVOL sur le lecteur D:\, installer les fichiers journaux sur le lecteur E:\, redémarrer automatiquement le serveur une fois l’installation des services AD DS terminée et inviter l’utilisateur à fournir le mot de passe du mode de restauration des services d’annuaire (DSRM).  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>Installation intermédiaire d’un contrôleur de domaine à l’aide de Windows PowerShell  
La syntaxe de commande pour créer un compte RODC est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]  
```  
  
La syntaxe de commande pour attacher un serveur à compte RODC est la suivante. Les arguments facultatifs apparaissent entre crochets.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Par exemple, pour créer un compte RODC nommé RODC1 :  
  
```  
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA  
```  
  
Exécutez ensuite les commandes suivantes sur le serveur que vous voulez attacher au compte RODC1. Le serveur ne peut pas être joint au domaine. Pour commencer, installez le rôle serveur AD DS et les outils de gestion :  
  
```  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```  
  
Ensuite, exécutez la commande suivante pour créer le contrôleur de domaine en lecture seule :  
  
```  
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```  
  
Appuyez sur **Y** pour confirmer ou incluez le **« confirmer** argument afin d’éviter l’invite de confirmation.  
  
## <a name="BKMK_GUI"></a>L’installation des services AD DS à l’aide du Gestionnaire de serveur  
Les services AD DS peut être installés dans Windows Server 2012 à l’aide de l’Assistant Ajout de rôles dans le Gestionnaire de serveur, suivie de l’Assistant de Configuration Active Directory domaine Services, qui est la nouvelle possibilité à compter de Windows Server 2012. L’Active Directory domaine Assistant Installation des Services (dcpromo.exe) est déconseillé à compter de Windows Server 2012.  
  
Les sections suivantes expliquent comment créer des pools de serveurs pour installer et gérer les services AD DS sur plusieurs serveurs et comment utiliser les Assistants pour installer les services AD DS.  
  
### <a name="BKMK_ServerPools"></a>Création de pools de serveurs  
Le Gestionnaire de serveur peut mettre en pool d’autres serveurs sur le réseau tant qu’ils sont accessibles à partir de l’ordinateur exécutant le Gestionnaire de serveur. Une fois la mise en pool terminée, vous pouvez sélectionner ces serveurs en vue d’une installation à distance des services AD DS ou toute autre option de configuration possible dans le Gestionnaire de serveur. L’ordinateur exécutant le Gestionnaire de serveur se met en pool automatiquement. Pour plus d’informations sur les pools de serveurs, voir [Ajouter des serveurs au Gestionnaire de serveur](https://technet.microsoft.com/library/hh831453.aspx).  
  
> [!NOTE]  
> Pour gérer un ordinateur appartenant à un domaine à l’aide du Gestionnaire de serveur sur un serveur de groupe de travail, ou inversement, des étapes de configuration supplémentaires sont nécessaires. Pour plus d’informations, consultez « Ajouter et gérer des serveurs dans des groupes de travail » dans [ajouter des serveurs au Gestionnaire de serveur](https://technet.microsoft.com/library/hh831453.aspx).  
  
### <a name="BKMK_installADDSGUI"></a>Installation des services AD DS  
**Informations d’identification administratives**  
  
Les informations d’identification requises pour installer les services AD DS varient en fonction de la configuration de déploiement que vous choisissez. Pour plus d’informations, voir [Informations d’identification requises pour exécuter Adprep.exe et installer les services de domaine Active Directory](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
Utilisez les procédures suivantes pour installer les services AD DS à l’aide de l’interface graphique utilisateur. Les étapes peuvent être effectuées localement ou à distance. Pour obtenir une explication plus détaillée de ces étapes, voir les rubriques suivantes :  
  
-   [Déploiement d’une forêt avec le Gestionnaire de serveur](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Installer un contrôleur de domaine Windows Server 2012 dans un domaine existant &#40;niveau 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  
-   [Installer un nouvel enfant de Active Directory Windows Server 2012 ou d’un domaine d’arborescence &#40;niveau 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
  
-   [Installer un contrôleur de domaine Server 2012 Active Directory en lecture seule Windows &#40;RODC&#41; &#40;niveau 200&#41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)  
  
##### <a name="to-install-ad-ds-by-using-server-manager"></a>Pour installer les services AD DS à l’aide du Gestionnaire de serveur  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajout de rôles et de fonctionnalités** pour démarrer l’Assistant Ajout de rôles.  
  
2.  Dans la page **Avant de commencer** , cliquez sur **Suivant**.  
  
3.  Dans la page **Sélectionner le type d’installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.  
  
4.  Dans la page **Sélectionner le serveur de destination**, cliquez sur **Sélectionner un serveur du pool de serveurs**, cliquez sur le nom du serveur sur lequel vous voulez installer les services AD DS, puis cliquez sur **Suivant**.  
  
    Pour sélectionner des serveurs distants, commencez par créer un pool de serveurs et ajoutez-y les serveurs distants. Pour plus d’informations sur la création de pools de serveurs, voir [Ajouter des serveurs au Gestionnaire de serveur](https://technet.microsoft.com/library/hh831453.aspx).  
  
5.  Dans la page **Sélectionner des rôles de serveurs**, cliquez sur **Services de domaine Active Directory**. Ensuite, dans la boîte de dialogue **Assistant Ajout de rôles et de fonctionnalités**, cliquez **Ajouter des fonctionnalités**, puis sur **Suivant**.  
  
6.  Dans la page **Sélectionner des fonctionnalités**, sélectionnez les fonctionnalités supplémentaires à installer, puis cliquez sur **Suivant**.  
  
7.  Dans **Services de domaine Active Directory**, examinez les informations, puis cliquez sur **Suivant**.  
  
8.  Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
9. Dans la page **Résultats**, vérifiez que l’installation s’est correctement déroulée, puis cliquez sur **Promouvoir ce serveur en contrôleur de domaine** pour démarrer l’Assistant Configuration des services de domaine Active Directory.  
  
    ![Installer AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)  
  
    > [!IMPORTANT]  
    > Si vous fermez l’Assistant Ajout de rôles à ce stade sans démarrer l’Assistant Configuration des services de domaine Active Directory, vous pouvez le redémarrer en cliquant sur Tâches dans le Gestionnaire de serveur.  
  
    ![Installer AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
10. Dans la page **Configuration de déploiement**, choisissez l’une des options suivantes :  
  
    -   Si vous installez un contrôleur de domaine supplémentaire dans un domaine existant, cliquez sur **ajouter un contrôleur de domaine à un domaine existant**et tapez le nom du domaine (par exemple, emea.corp.contoso.com) ou cliquez sur **sélectionnez ...**  pour choisir un domaine et les informations d’identification (par exemple, spécifiez un compte qui est membre du groupe Admins du domaine), puis **suivant**.  
  
        > [!NOTE]  
        > Le nom du domaine et les informations d’identification de l’utilisateur actuel sont fournis par défaut uniquement si l’ordinateur est joint à un domaine et que vous effectuez une installation locale. Si vous installez les services AD DS sur un serveur distant, vous devez, par conception, spécifier les informations d’identification. Si les informations d’identification utilisateur actuelles ne sont pas suffisantes pour effectuer l’installation, cliquez sur **modification...**  afin de spécifier les informations d’identification différentes.  
  
        Pour plus d’informations, consultez [installer un contrôleur de domaine réplica Windows Server 2012 dans un domaine existant &#40;niveau 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
    -   Si vous installez un nouveau domaine enfant, cliquez sur **Ajouter un nouveau domaine à une forêt existante**, pour **Sélectionnez le type du domaine**, sélectionnez **Domaine enfant**, tapez le nom DNS du domaine parent (par exemple, corp.contoso.com) ou accédez à celui-ci, tapez le nom relatif du nouveau domaine enfant (par exemple, emea), tapez les informations d’identification à utiliser pour créer le domaine, puis cliquez sur **Suivant**.  
  
        Pour plus d’informations, consultez [installer un nouveau Windows Server 2012 Active Directory domaine enfant ou arborescence &#40;niveau 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Si vous installez une nouvelle arborescence de domaine, cliquez sur **Ajouter un nouveau domaine à une forêt existante**, pour **Sélectionnez le type du domaine**, sélectionnez **Domaine de l’arborescence**, tapez le nom du domaine racine (par exemple, corp.contoso.com), tapez le nom DNS du nouveau domaine (par exemple, fabrikam.com), tapez les informations d’identification à utiliser pour créer le domaine, puis cliquez sur **Suivant**.  
  
        Pour plus d’informations, consultez [installer un nouveau Windows Server 2012 Active Directory domaine enfant ou arborescence &#40;niveau 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Si vous installez une nouvelle forêt, cliquez sur **Ajouter une nouvelle forêt**, puis tapez le nom du domaine racine (par exemple, corp.contoso.com).  
  
        Pour plus d’informations, consultez [installer une nouvelle forêt Windows Server 2012 Active Directory &#40;niveau 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
11. Dans la page **Options du contrôleur de domaine**, choisissez l’une des options suivantes :  
  
    -   Si vous créez une forêt ou un domaine, sélectionnez les niveaux fonctionnels du domaine et de la forêt, cliquez sur **Serveur du Système de Noms de Domaine (DNS)**, spécifiez le mot de passe DSRM, puis cliquez sur **Suivant**.  
  
    -   Si vous ajoutez un contrôleur de domaine à un domaine existant, cliquez sur **Serveur du Système de Noms de Domaine (DNS)**, **Catalogue global (GC)** ou **Contrôleur de domaine en lecture seule (RODC)** selon les besoins, puis choisissez le nom du site. Ensuite, tapez le mot de passe DSRM, puis cliquez sur **Suivant**.  
  
    Pour plus d’informations sur les options disponibles ou non dans cette page dans des conditions différentes, voir [Options du contrôleur de domaine](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage).  
  
12. Dans la page **Options DNS** (qui apparaît uniquement si vous installez un serveur DNS), cliquez sur **Mettre à jour la délégation DNS** selon les besoins. Dans ce cas, entrez les informations d’identification qui autorisent la création d’enregistrements de délégation DNS dans la zone DNS parente.  
  
    Si un serveur DNS qui héberge la zone parente ne peut pas être contacté, l’option **Mettre à jour la délégation DNS** n’est pas disponible.  
  
    Pour plus d’informations sur la nécessité ou non de mettre à jour la délégation DNS, consultez [Présentation de la délégation de zone](https://technet.microsoft.com/library/cc771640.aspx). Si vous tentez de mettre à jour la délégation DNS et qu’une erreur se produit, voir [Options DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
13. Dans la page **Options RODC** (qui apparaît uniquement si vous installez un contrôleur de domaine en lecture seule), spécifiez le nom d’un groupe ou d’un utilisateur qui gèrera le contrôleur de domaine en lecture seule, ajoutez des comptes aux groupes de réplication de mot de passe Autorisé ou Refusé ou supprimez des comptes de ceux-ci, puis cliquez sur **Suivant**.  
  
    Pour plus d’informations, consultez [Stratégie de réplication de mot de passe](https://technet.microsoft.com/library/cc730883(v=ws.10)).  
  
14. Dans la page **Options supplémentaires**, choisissez l’une des options suivantes :  
  
    -   Si vous créez un nouveau domaine, tapez un nouveau nom NetBIOS ou vérifiez le nom NetBIOS par défaut du domaine, puis cliquez sur **Suivant**.  
  
    -   Si vous ajoutez un contrôleur de domaine à un domaine existant, sélectionnez le contrôleur de domaine à partir duquel vous voulez répliquer les données d’installation AD DS (ou autorisez l’Assistant à sélectionner n’importe quel contrôleur de domaine). Si vous effectuez l’installation à partir d’un support, cliquez sur **Installation à partir du chemin d’accès au support** et vérifiez le chemin d’accès aux fichiers de la source d’installation, puis cliquez sur **Suivant**.  
  
        Vous ne pouvez pas recourir à l’installation à partir du support (IFM) pour installer le premier contrôleur de domaine dans un domaine. IFM ne fonctionne pas entre différentes versions de système d’exploitation. En d’autres termes, pour installer un contrôleur de domaine supplémentaire qui exécute Windows Server 2012 à l’aide d’IFM, vous devez créer le support de sauvegarde sur un contrôleur de domaine Windows Server 2012. Pour plus d’informations sur IFM, voir [Installation d’un contrôleur de domaine supplémentaire à l’aide d’IFM](https://technet.microsoft.com/library/cc816722(WS.10).aspx).  
  
15. Dans la page **Chemins d’accès**, tapez les emplacements de la base de données Active Directory, des fichiers journaux et du dossier SYSVOL (ou acceptez les emplacements par défaut), puis cliquez sur **Suivant**.  
  
    > [!IMPORTANT]  
    > Ne stockez pas la base de données Active Directory, les fichiers journaux ou le dossier SYSVOL sur un volume de données au format ReFS.  
  
16. Dans page **Options de préparation**, tapez des informations d’identification suffisantes pour exécuter adprep. Pour plus d’informations, voir [Informations d’identification requises pour exécuter Adprep.exe et installer les services de domaine Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
17. Dans la page **Examiner les options**, confirmez vos sélections, cliquez sur **Afficher le script** si vous voulez exporter les paramètres vers un script Windows PowerShell, puis cliquez sur **Suivant**.  
  
18. Dans la page **Vérification de la configuration requise**, confirmez que la validation de la configuration requise est terminée, puis cliquez sur **Installer**.  
  
19. Dans la page **Résultats**, vérifiez que le serveur a été correctement configuré en tant que contrôleur de domaine. Le serveur est redémarré automatiquement pour terminer l’installation des services AD DS.  
  
## <a name="BKMK_UIStaged"></a>Effectuez une Installation RODC intermédiaire à l’aide de l’Interface utilisateur graphique  
Une installation RODC intermédiaire vous permet de créer un contrôleur de domaine en lecture seule en deux étapes. Lors de la première phase, un membre du groupe Admins du domaine crée un compte RODC. Lors de la deuxième phase, un serveur est attaché au compte RODC. La deuxième phase peut être effectuée par un membre du groupe Admins du domaine ou un utilisateur ou un groupe d’un domaine délégué.  
  
#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>Pour créer un compte RODC à l’aide des outils de gestion Active Directory  
  
1.  Vous pouvez créer le compte RODC dans le Centre d’administration Active Directory ou dans Utilisateurs et ordinateurs Active Directory.  
  
    1.  Cliquez sur **Démarrer**, **Outils d’administration**, puis sur **Centre d’administration Active Directory**.  
  
    2.  Dans le volet de navigation (volet gauche), cliquez sur le nom du domaine.  
  
    3.  Dans la liste Gestion (volet central), cliquez sur l’unité d’organisation **Domain Controllers**.  
  
    4.  Dans le volet Tâches (volet droit), cliquez sur **Pré-créer un compte de contrôleur de domaine en lecture seule**.  
  
    - Ou -  
  
    1.  Cliquez sur **Démarrer**, **Outils d'administration**, puis sur **Utilisateurs et ordinateurs Active Directory**.  
  
    2.  Cliquez avec le bouton droit sur l’unité d’organisation **Domain Controllers** ou cliquez sur l’unité d’organisation **Domain Controllers**, puis cliquez sur **Action**.  
  
    3.  Cliquez sur **Créer au préalable un compte de contrôleur de domaine en lecture seule**.  
  
2.  Dans la page **Assistant Installation des services de domaine Active Directory**, si vous souhaitez modifier la stratégie de réplication de mot de passe par défaut, sélectionnez **Utiliser l’installation en mode avancé**, puis cliquez sur **Suivant**.  
  
3.  Dans la page **Informations d’identification réseau**, sous **Spécifiez les informations d’identification de compte à utiliser pour effectuer l’installation**, cliquez sur **Mes informations d’identification de connexion actuelles** ou sur **Autres informations d’identification**, puis cliquez sur **Définir**. Dans la boîte de dialogue **Sécurité de Windows**, indiquez le nom d’utilisateur et le mot de passe d’un compte pouvant installer le contrôleur de domaine supplémentaire. Pour installer un contrôleur de domaine supplémentaire, vous devez être membre du groupe Administrateurs de l’entreprise ou du groupe Admins du domaine. Une fois les informations d’identification fournies, cliquez sur **Suivant**.  
  
4.  Dans la page **Spécifiez le nom de l’ordinateur**, tapez le nom d’ordinateur du serveur devant jouer le rôle de contrôleur de domaine en lecture seule.  
  
5.  Dans la page **Sélectionnez un site**, choisissez un site dans la liste ou sélectionnez l’option permettant d’installer le contrôleur de domaine dans le site qui correspond à l’adresse IP de l’ordinateur sur lequel vous exécutez l’Assistant, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Options supplémentaires pour le contrôleur de domaine**, effectuez les sélections suivantes, puis cliquez sur **Suivant** :  
  
    -   **Serveur DNS**: cette case à cocher est activée par défaut pour que votre contrôleur de domaine puisse fonctionner en tant que serveur DNS (Domain Name System). Si vous ne souhaitez pas que le contrôleur de domaine soit un serveur DNS, désactivez cette case à cocher. Toutefois, si vous n’installez pas le rôle Serveur DNS sur le contrôleur de domaine en lecture seule et que ce dernier est le seul contrôleur de domaine dans la filiale, les utilisateurs dans la filiale ne peuvent pas effectuer la résolution de noms lorsque le réseau étendu (WAN) relié au site hub est hors connexion.  
  
    -   **Catalogue global**: Cette option est sélectionnée par défaut. Elle ajoute les partitions d’annuaire en lecture seule de catalogue global au contrôleur de domaine, et elle active la fonctionnalité de recherche dans le catalogue global. Si vous ne souhaitez pas que le contrôleur de domaine soit un serveur de catalogue global, désactivez cette case à cocher. Cependant, si vous n’installez aucun serveur de catalogue global dans la succursale ou activez la mise en cache de l’appartenance au groupe universel pour le site comprenant le contrôleur de domaine en lecture seule, les utilisateurs de la succursale ne pourront pas se connecter au domaine lorsque le réseau étendu (WAN) relié au site hub est hors connexion.  
  
    -   **Contrôleur de domaine en lecture seule** : cette option est sélectionnée par défaut et ne peut être désactivée lorsque vous créez un compte de contrôleur de domaine en lecture seule.  
  
7.  Si vous avez activé la case à cocher **Utiliser l’installation en mode avancé** dans la page **Bienvenue**, la page **Spécifier la stratégie de réplication de mot de passe** apparaît. Par défaut, aucun mot de passe de compte n’est répliqué dans le contrôleur de domaine en lecture seule et les mots de passe des comptes dont la sécurité est primordiale (tels que les membres du groupe Admins du domaine) ne peuvent en aucun cas être répliqués sur le contrôleur de domaine en lecture seule.  
  
    Pour ajouter d’autres comptes à la stratégie, cliquez sur **Ajouter**, puis sur **Autoriser la réplication des mots de passe du compte sur ce contrôleur de domaine en lecture seule (RODC)** ou cliquez sur **Refuser la réplication des mots de passe du compte sur le contrôleur de domaine en lecture seule (RODC)**, puis sélectionnez les comptes.  
  
    Une fois terminé (ou pour accepter le paramètre par défaut), cliquez sur **Suivant**.  
  
8.  Sur la page **Délégation de l’installation et de l’administration du contrôleur de domaine en lecture seule (RODC)**, tapez le nom de l’utilisateur ou du groupe qui joindra le serveur au compte de contrôleur de domaine en lecture que vous créez. Vous pouvez taper le nom d’un seul principal de sécurité.  
  
    Pour rechercher un utilisateur ou un groupe spécifique dans l’annuaire, cliquez sur **Définir**. Dans **Sélectionner Utilisateur ou groupe**, tapez le nom de l’utilisateur ou du groupe. Nous vous recommandons de déléguer l’installation et l’administration d’un contrôleur de domaine en lecture seule à un groupe.  
  
    Cet utilisateur ou ce groupe disposera de droits d’administrateur local sur le contrôleur de domaine en lecture seule après l’installation. Si vous ne spécifiez aucun utilisateur ou groupe, seuls les membres du groupe Admins du domaine ou du groupe Administrateurs de l’entreprise seront en mesure de joindre le serveur au compte.  
  
    Lorsque vous avez terminé, cliquez sur **Suivant**.  
  
9. Dans la page **Résumé**, vérifiez vos sélections. Cliquez sur **Précédent** pour modifier les sélections, si nécessaire.  
  
    Pour enregistrer les paramètres que vous avez sélectionné dans un fichier de réponses que vous pouvez utiliser pour automatiser les opérations suivantes de AD DS, cliquez sur **exporter les paramètres**. Tapez le nom de votre fichier de réponses, puis cliquez sur **Enregistrer**.  
  
    Lorsque vous êtes certain que vos sélections sont correctes, cliquez sur **Suivant** pour créer le compte RODC.  
  
10. Dans la page **Fin de l’Assistant Installation des services de domaine Active Directory**, cliquez sur **Terminer**.  
  
Une fois qu’un compte RODC est créé, vous pouvez attacher un serveur au compte pour terminer l’installation du contrôleur de domaine en lecture seule. Cette deuxième phase peut être effectuée dans la filiale où le contrôleur de domaine en lecture seule sera situé. Le serveur sur lequel vous effectuez cette procédure ne doit pas être joint au domaine. À compter de Windows Server 2012, vous utilisez l’Assistant Ajout de rôles dans le Gestionnaire de serveur pour attacher un serveur à un compte RODC.  
  
#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>Pour attacher un serveur à un compte RODC à l’aide du Gestionnaire de serveur  
  
1.  Ouvrez une session en tant qu’administrateur local.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **Ajouter des rôles et fonctionnalités**.  
  
3.  Dans la page **Avant de commencer** , cliquez sur **Suivant**.  
  
4.  Dans la page **Sélectionner le type d’installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.  
  
5.  Dans la page **Sélectionner le serveur de destination**, cliquez sur **Sélectionner un serveur du pool de serveurs**, cliquez sur le nom du serveur sur lequel vous voulez installer les services AD DS, puis cliquez sur **Suivant**.  
  
6.  Dans la page **Sélectionner des rôles de serveurs**, cliquez sur **Services de domaine Active Directory**, sur **Ajouter des fonctionnalités**, puis sur **Suivant**.  
  
7.  Dans la page **Sélectionner des fonctionnalités**, sélectionnez les fonctionnalités supplémentaires à installer, puis cliquez sur **Suivant**.  
  
8.  Dans **Services de domaine Active Directory**, examinez les informations, puis cliquez sur **Suivant**.  
  
9. Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
10. Dans la page **Résultats**, vérifiez la présence du message **Réussite de l’installation**, puis cliquez sur **Promouvoir ce serveur en contrôleur de domaine** pour démarrer l’Assistant Configuration des services de domaine Active Directory.  
  
    > [!IMPORTANT]  
    > Si vous fermez l’Assistant Ajout de rôles à ce stade sans démarrer l’Assistant Configuration des services de domaine Active Directory, vous pouvez le redémarrer en cliquant sur Tâches dans le Gestionnaire de serveur.  
  
    (media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
11. Dans la page **Configuration de déploiement**, cliquez sur **Ajouter un contrôleur de domaine à un domaine existant**, tapez le nom du domaine (par exemple, emea.contoso.com) et les informations d’identification (par exemple, spécifiez un compte qui est délégué pour gérer et installer le contrôleur de domaine en lecture seule), puis cliquez sur **Suivant**.  
  
12. Dans la page **Options du contrôleur de domaine**, cliquez sur **Utiliser le compte RODC existant**, tapez et confirmez le mot de passe du mode de restauration des services d’annuaire, puis cliquez sur **Suivant**.  
  
13. Dans la page **Options supplémentaires**, si vous effectuez l’installation à partir d’un support, cliquez sur **Installation à partir du chemin d’accès au support** et vérifiez le chemin d’accès aux fichiers de la source d’installation, sélectionnez le contrôleur de domaine à partir duquel vous voulez répliquer les données d’installation AD DS (ou autorisez l’Assistant à sélectionner n’importe quel contrôleur de domaine), puis cliquez sur **Suivant**.  
  
14. Dans la page **Chemins d’accès**, tapez les emplacements de la base de données Active Directory, des fichiers journaux et du dossier SYSVOL (ou acceptez les emplacements par défaut), puis cliquez sur **Suivant**.  
  
15. Dans la page **Examiner les options**, confirmez vos sélections, cliquez sur **Afficher le script** pour exporter les paramètres vers un script Windows PowerShell, puis cliquez sur **Suivant**.  
  
16. Dans la page **Vérification de la configuration requise**, confirmez que la validation de la configuration requise est terminée, puis cliquez sur **Installer**.  
  
    Pour terminer l’installation des services AD DS, le serveur redémarre automatiquement.  
  
## <a name="see-also"></a>Voir aussi  
[Résolution des problèmes de déploiement du contrôleur de domaine](Troubleshooting-Domain-Controller-Deployment.md)  
[Installer une nouvelle forêt de Active Directory Windows Server 2012 &#40;niveau 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)  
[Installer un nouvel enfant de Active Directory Windows Server 2012 ou d’un domaine d’arborescence &#40;niveau 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
[Installer un contrôleur de domaine Windows Server 2012 dans un domaine existant &#40;niveau 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  



