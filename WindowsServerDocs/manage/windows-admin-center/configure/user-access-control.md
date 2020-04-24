---
title: Configuration des autorisations et du contrôle d’accès utilisateur
description: Découvrez comment configurer les autorisations et le contrôle d’accès utilisateur à l’aide d’Active Directory ou d’Azure AD (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 39af45506ff7023cebe437992e90f6d4ec051333
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "79323591"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurer les autorisations et le contrôle d’accès utilisateur

> S'applique à : Windows Admin Center, Windows Admin Center Preview

Si vous ne l’avez pas encore fait, familiarisez-vous avec les [options de contrôle d’accès utilisateur dans Windows Admin Center](../plan/user-access-options.md).

> [!NOTE]
> L’accès basé sur les groupes dans Windows Admin Center n’est pas pris en charge dans les environnements de groupe de travail ni dans les domaines non approuvés.

## <a name="gateway-access-role-definitions"></a>Définitions des rôles d’accès à la passerelle

Deux rôles permettent d’accéder au service de passerelle Windows Admin Center :

Les **utilisateurs de passerelle** peuvent se connecter au service de passerelle Windows Admin Center pour gérer les serveurs via cette passerelle, mais ils ne peuvent pas modifier les autorisations d’accès ni le mécanisme d’authentification utilisé pour l’authentification auprès de la passerelle.

Les **administrateurs de passerelle** peuvent configurer qui obtient l’accès à la passerelle, ainsi que la façon dont les utilisateurs s’authentifient auprès d’elle. Seuls les administrateurs de passerelle peuvent afficher et configurer les paramètres d’accès dans Windows Admin Center. Les administrateurs locaux sur l’ordinateur passerelle sont toujours des administrateurs du service de passerelle Windows Admin Center.

> [!NOTE]
> L’accès à la passerelle n’implique pas l’accès aux serveurs gérés visibles par la passerelle. Pour gérer un serveur cible, l’utilisateur qui se connecte doit utiliser des informations d’identification (les informations d’identification Windows qui lui ont été transmises ou les informations d’identification fournies dans la session Windows Admin Center à l’aide de l’action **Gérer en tant que**) qui disposent d’un accès administratif à ce serveur cible.

## <a name="active-directory-or-local-machine-groups"></a>Groupes d’ordinateurs Active Directory ou locaux

Par défaut, des groupes d’ordinateurs Active Directory ou locaux sont utilisés pour contrôler l’accès à la passerelle. Si vous avez un domaine Active Directory, vous pouvez gérer l’accès utilisateur et administrateur de la passerelle à partir de l’interface Windows Admin Center.

Sous l’onglet **Utilisateurs**, vous pouvez contrôler qui peut accéder à Windows Admin Center en tant qu’utilisateur de passerelle. Par défaut, si vous ne spécifiez pas de groupe de sécurité, tout utilisateur qui accède à l’URL de la passerelle a accès à cette dernière. Une fois que vous avez ajouté un ou plusieurs groupes de sécurité à la liste des utilisateurs, l’accès est limité aux membres de ces groupes.

Si vous n’utilisez pas un domaine Active Directory dans votre environnement, l’accès est contrôlé par les groupes locaux `Users` et `Administrators` sur l’ordinateur passerelle Windows Admin Center.

### <a name="smartcard-authentication"></a>Authentification par carte à puce

Vous pouvez appliquer une **authentification par carte à puce** en spécifiant un groupe _requis_ supplémentaire pour les groupes de sécurité basés sur une carte à puce. Une fois que vous avez ajouté un groupe de sécurité basé sur carte à puce, un utilisateur peut accéder au service Windows Admin Center uniquement s’il est membre d’un groupe de sécurité ET d’un groupe de cartes à puce inclus dans la liste des utilisateurs.

Sous l’onglet **Administrateurs**, vous pouvez contrôler qui peut accéder à Windows Admin Center en tant qu’administrateur de passerelle. Le groupe des administrateurs locaux sur l’ordinateur a toujours un accès administrateur complet et ne peut pas être supprimé de la liste. En ajoutant des groupes de sécurité, vous accordez aux membres de ces groupes des privilèges leur permettant de modifier les paramètres de la passerelle Windows Admin Center. La liste des administrateurs prend en charge l’authentification par carte à puce de la même façon que la liste des utilisateurs : à condition d’être membre d’un groupe de sécurité ET d’un groupe de cartes à puce.

## <a name="azure-active-directory"></a>Azure Active Directory

Si votre organisation utilise Azure Active Directory (Azure AD), vous pouvez choisir d’ajouter une couche de sécurité **supplémentaire** à Windows Admin Center en exigeant une authentification Azure AD pour accéder à la passerelle. Pour accéder à Windows Admin Center, le **compte Windows** de l’utilisateur doit également avoir accès au serveur de passerelle (même si l’authentification Azure AD est utilisée). Lorsque vous utilisez Azure AD, vous gérez les autorisations d’accès utilisateur et administrateur à Windows Admin Center à partir du Portail Azure plutôt qu’à partir de l’interface utilisateur Windows Admin Center.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Accès à Windows Admin Center quand l’authentification Azure AD est activée

Selon le navigateur, certains utilisateurs qui accèdent à Windows Admin Center avec l’authentification Azure AD configurée reçoivent une invite supplémentaire **du navigateur**, leur demandant de fournir les informations d’identification de leur compte Windows pour l’ordinateur sur lequel Windows Admin Center est installé. Après avoir entré ces informations, les utilisateurs obtiennent l’invite d’authentification Azure Active Directory supplémentaire, qui requiert les informations d’identification d’un compte Azure disposant de l’accès à l’application Azure AD dans Azure.

> [!NOTE]
> Les utilisateurs dont le compte Windows dispose de **droits d’administrateur** sur l’ordinateur passerelle ne sont pas invités à fournir une authentification Azure AD.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configuration de l’authentification Azure Active Directory pour la préversion de Windows Admin Center

Dans Windows Admin Center, accédez à **Paramètres** > **Accès** et utilisez le bouton bascule pour activer « Utiliser Azure Active Directory pour ajouter une couche de sécurité à la passerelle ». Si vous n’avez pas inscrit la passerelle auprès d’Azure, vous serez guidé pour le faire à ce stade.

Par défaut, tous les membres du locataire Azure AD disposent d’un accès utilisateur au service de passerelle Windows Admin Center. Seuls les administrateurs locaux sur l’ordinateur passerelle disposent d’un accès administrateur à la passerelle Windows Admin Center. Notez que les droits des administrateurs locaux sur l’ordinateur passerelle ne peuvent pas être restreints : les administrateurs locaux peuvent faire tout ce qu’il est possible de faire, qu’Azure AD soit utilisé ou non pour l’authentification.

Si vous souhaitez accorder à des utilisateurs ou à des groupes Azure AD spécifiques un accès d’utilisateur de passerelle ou d’administrateur de passerelle au service Windows Admin Center, vous devez effectuer les opérations suivantes :

1.  Accédez à votre application Azure AD Windows Admin Center dans le Portail Azure en utilisant le lien hypertexte fourni dans Paramètres d’accès. Notez que ce lien hypertexte est disponible uniquement lorsque l’authentification Azure Active Directory est activée. 
    -   Vous trouverez également votre application dans le Portail Azure en accédant à **Azure Active Directory** > **Applications d’entreprise** > **Toutes les applications** et en recherchant **WindowsAdminCenter** (l’application Azure AD sera nommée WindowsAdminCenter-<gateway name>). Si vous n’obtenez pas de résultats de recherche, veillez à ce que l’option **Afficher** ait la valeur **toutes les applications** et que l’option **État de l’application** ait la valeur **n’importe lequel**, et cliquez sur Appliquer, puis essayez d’effectuer votre recherche. Une fois que vous avez trouvé l’application, accédez à **Utilisateurs et groupes**.
2.  Sous l’onglet Propriétés, définissez **Affectation de l’utilisateur obligatoire** sur Oui.
    Après cela, seuls les membres listés sous l’onglet **Utilisateurs et groupes** peuvent accéder à la passerelle Windows Admin Center.
3.  Sous l’onglet Utilisateurs et groupes, sélectionnez **Ajouter un utilisateur**. Vous devez attribuer un rôle d’utilisateur de passerelle ou d’administrateur de passerelle pour chaque utilisateur/groupe ajouté.

Une fois que vous avez activé l’authentification Azure AD, le service de passerelle redémarre et vous devez actualiser votre navigateur. Vous pouvez mettre à jour l’accès utilisateur pour l’application SME Azure AD dans le Portail Azure à tout moment.

Les utilisateurs sont invités à se connecter à l’aide de leur identité Azure Active Directory lorsqu’ils tentent d’accéder à l’URL de la passerelle Windows Admin Center. N’oubliez pas que les utilisateurs doivent également être membres des utilisateurs locaux sur le serveur de passerelle pour accéder à Windows Admin Center.

Les utilisateurs et les administrateurs peuvent afficher leur compte actuellement connecté et déconnecter ce compte Azure AD à partir de l’onglet **Compte** dans les paramètres Windows Admin Center.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configuration de l’authentification Azure Active Directory pour Windows Admin Center

[Pour configurer l’authentification Azure AD, vous devez d’abord inscrire votre passerelle auprès d’Azure](azure-integration.md) (vous ne devez effectuer cette opération qu’une seule fois pour votre passerelle Windows Admin Center). Cette étape crée une application Azure AD à partir de laquelle vous pouvez gérer l’accès d’utilisateur de passerelle et d’administrateur de passerelle.

Si vous souhaitez accorder à des utilisateurs ou à des groupes Azure AD spécifiques un accès d’utilisateur de passerelle ou d’administrateur de passerelle au service Windows Admin Center, vous devez effectuer les opérations suivantes :

1.  Accédez à votre application SME Azure AD dans le Portail Azure. 
    -   Lorsque vous cliquez sur **Change access control** (Modifier le contrôle d’accès), puis sélectionnez **Azure Active Directory** dans les paramètres d’accès Windows Admin Center, vous pouvez utiliser le lien hypertexte fourni dans l’interface utilisateur pour accéder à votre application Azure AD dans le Portail Azure. Ce lien hypertexte est également disponible dans les paramètres d’accès une fois que vous avez cliqué sur Enregistrer et que vous avez sélectionné Azure AD comme fournisseur d’identité de contrôle d’accès.
    -   Vous trouverez également votre application dans le Portail Azure en accédant à **Azure Active Directory** > **Applications d’entreprise** > **Toutes les applications** et en recherchant **SME** (l’application Azure AD sera nommée SME-<gateway>). Si vous n’obtenez pas de résultats de recherche, veillez à ce que l’option **Afficher** ait la valeur **toutes les applications** et que l’option **État de l’application** ait la valeur **n’importe lequel**, et cliquez sur Appliquer, puis essayez d’effectuer votre recherche. Une fois que vous avez trouvé l’application, accédez à **Utilisateurs et groupes**.
2.  Sous l’onglet Propriétés, définissez **Affectation de l’utilisateur obligatoire** sur Oui.
    Après cela, seuls les membres listés sous l’onglet **Utilisateurs et groupes** peuvent accéder à la passerelle Windows Admin Center.
3.  Sous l’onglet Utilisateurs et groupes, sélectionnez **Ajouter un utilisateur**. Vous devez attribuer un rôle d’utilisateur de passerelle ou d’administrateur de passerelle pour chaque utilisateur/groupe ajouté.

Une fois que vous avez enregistré le contrôle d’accès Azure AD dans le volet **Change access control** (Modifier le contrôle d’accès), le service de passerelle redémarre et vous devez actualiser votre navigateur. Vous pouvez mettre à jour l’accès utilisateur pour l’application Azure AD Windows Admin Center dans le Portail Azure à tout moment. 

Les utilisateurs sont invités à se connecter à l’aide de leur identité Azure Active Directory lorsqu’ils tentent d’accéder à l’URL de la passerelle Windows Admin Center. N’oubliez pas que les utilisateurs doivent également être membres des utilisateurs locaux sur le serveur de passerelle pour accéder à Windows Admin Center. 

Grâce à l’onglet **Azure** des paramètres généraux Windows Admin Center, les utilisateurs et les administrateurs peuvent afficher leur compte actuellement connecté et déconnecter ce compte Azure AD.

### <a name="conditional-access-and-multi-factor-authentication"></a>Accès conditionnel et authentification multifacteur

L’un des avantages de l’utilisation d’Azure AD comme couche de sécurité supplémentaire pour contrôler l’accès à la passerelle Windows Admin Center est que vous pouvez tirer parti des puissantes fonctionnalités de sécurité d’Azure AD telles que l’accès conditionnel et l’authentification multifacteur. 

[En savoir plus sur la configuration de l’accès conditionnel avec Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurer l'authentification unique

**Authentification unique lors d’un déploiement en tant que service sur Windows Server**

Lorsque vous installez Windows Admin Center sur Windows 10, il est prêt à utiliser l’authentification unique. Toutefois, si vous envisagez d’utiliser Windows Admin Center sur Windows Server, vous devez configurer une certaine forme de délégation Kerberos dans votre environnement avant de pouvoir utiliser l’authentification unique. La délégation configure l’ordinateur passerelle comme approuvé pour déléguer au nœud cible. 

Pour configurer la [délégation contrainte basée sur les ressources](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) dans votre environnement, utilisez l’exemple PowerShell suivant. Cet exemple montre comment configurer un serveur Windows Server [node01.contoso.com] pour accepter la délégation à partir de votre passerelle Windows Admin Center [wac.contoso.com] dans le domaine contoso.com.

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount (Get-ADComputer wac)
```

Pour supprimer cette relation, exécutez l’applet de commande suivante :

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Contrôle d'accès basé sur les rôles

Le contrôle d’accès en fonction du rôle vous permet de fournir aux utilisateurs un accès limité à l’ordinateur au lieu d’en faire des administrateurs locaux complets.
[En savoir plus sur les rôles disponibles et le contrôle d’accès en fonction du rôle.](../plan/user-access-options.md#role-based-access-control)

La configuration de RBAC est constituée de 2 étapes : l’activation de la prise en charge sur le ou les ordinateurs cibles et l’attribution d’utilisateurs aux rôles appropriés.

> [!TIP]
> Vérifiez que vous disposez de privilèges d’administrateur local pour les ordinateurs sur lesquels vous configurez la prise en charge du contrôle d’accès en fonction du rôle.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Appliquer le contrôle d’accès en fonction du rôle à un ordinateur individuel

Le modèle de déploiement d’ordinateur individuel est idéal pour les environnements simples avec quelques ordinateurs seulement à gérer.
La configuration d’un ordinateur avec la prise en charge du contrôle d’accès en fonction du rôle entraîne les modifications suivantes :

-   Les modules PowerShell avec les fonctions requises par Windows Admin Center sont installés sur votre lecteur système, sous `C:\Program Files\WindowsPowerShell\Modules`. Tous les modules commencent par **Microsoft.Sme**
-   La configuration d’état souhaité exécute une configuration unique pour configurer un point de terminaison d’administration suffisante sur l’ordinateur, nommé **Microsoft.Sme.PowerShell**. Ce point de terminaison définit les 3 rôles utilisés par Windows Admin Center et s’exécute en tant qu’administrateur local temporaire lorsqu’un utilisateur s’y connecte.
-   3 nouveaux groupes locaux sont créés pour contrôler quels utilisateurs se voient accorder l’accès à quels rôles :
    -   Administrateurs Windows Admin Center
    -   Administrateurs Hyper-V Windows Admin Center
    -   Lecteurs Windows Admin Center

Pour activer la prise en charge du contrôle d’accès en fonction du rôle sur un ordinateur individuel, effectuez les opérations suivantes :

1.  Ouvrez Windows Admin Center et connectez-vous à l’ordinateur que vous souhaitez configurer avec le contrôle d’accès en fonction du rôle à l’aide d’un compte disposant de privilèges d’administrateur local sur l’ordinateur cible.
2.  Dans l’outil **Vue d’ensemble**, cliquez sur **Paramètres** > **Contrôle d’accès en fonction du rôle**.
3.  Cliquez sur **Appliquer** en bas de la page pour activer la prise en charge du contrôle d’accès en fonction du rôle sur l’ordinateur cible. Le processus d’application implique la copie de scripts PowerShell et l’appel d’une configuration (à l’aide de la configuration d’état souhaité PowerShell) sur l’ordinateur cible. Cette opération peut prendre jusqu’à 10 minutes et entraîner le redémarrage de WinRM. Cette opération déconnecte temporairement les utilisateurs Windows Admin Center, PowerShell et WMI.
4.  Actualisez la page pour vérifier l’état du contrôle d’accès en fonction du rôle. Lorsqu’il est prêt à être utilisé, l’état passe à **Appliqué**.

Une fois la configuration appliquée, vous pouvez affecter des utilisateurs aux rôles :

1.  Ouvrez l’outil **Utilisateurs et groupes locaux** et accédez à l’onglet **Groupes**.
2.  Sélectionnez le groupe **Lecteurs Windows Admin Center**.
3.  Dans le volet *Détails* en bas de la page, cliquez sur **Ajouter un utilisateur** et entrez le nom d’un utilisateur ou d’un groupe de sécurité qui doit avoir un accès en lecture seule au serveur via Windows Admin Center. Les utilisateurs et les groupes peuvent provenir de l’ordinateur local ou de votre domaine Active Directory.
4.  Répétez les étapes 2 et 3 pour les groupes **Administrateurs Hyper-V Windows Admin Center** et **Administrateurs Windows Admin Center**.

Vous pouvez également remplir ces groupes de manière cohérente dans votre domaine en configurant un objet de stratégie de groupe avec le [paramètre de stratégie Groupes restreints](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Appliquer le contrôle d’accès en fonction du rôle à plusieurs ordinateurs

Dans un déploiement d’entreprise de grande taille, vous pouvez utiliser vos outils d’automatisation existants pour étendre la fonctionnalité de contrôle d’accès en fonction du rôle à vos ordinateurs en téléchargeant le package de configuration à partir de la passerelle Windows Admin Center.
Le package de configuration a été conçu pour être utilisé avec la configuration d’état souhaité PowerShell, mais vous pouvez l’adapter pour qu’il fonctionne avec votre solution d’automatisation par défaut.

#### <a name="download-the-role-based-access-control-configuration"></a>Télécharger la configuration du contrôle d’accès en fonction du rôle

Pour télécharger le package de configuration du contrôle d’accès en fonction du rôle, vous devez avoir accès à Windows Admin Center et à une invite PowerShell.

Si vous exécutez la passerelle Windows Admin Center en mode service sur Windows Server, utilisez la commande suivante pour télécharger le package de configuration.
Veillez à mettre à jour l’adresse de la passerelle avec celle qui convient pour votre environnement.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Si vous exécutez la passerelle Windows Admin Center sur votre ordinateur Windows 10, exécutez la commande suivante à la place :

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Lorsque vous développez l’archive zip, vous voyez s’afficher la structure de dossiers suivante :

- InstallJeaFeatures.ps1
- JustEnoughAdministration (répertoire)
- Modules (répertoire)
    - Microsoft.SME.\* (répertoires)
    - WindowsAdminCenter.Jea (répertoire)

Pour configurer la prise en charge du contrôle d’accès en fonction du rôle sur un nœud, vous devez effectuer les actions suivantes :

1.  Copiez les modules JustEnoughAdministration, Microsoft.SME.\* et WindowsAdminCenter.Jea dans le répertoire de modules PowerShell sur l’ordinateur cible. En général, il s’agit du répertoire `C:\Program Files\WindowsPowerShell\Modules`.
2.  Mettez à jour le fichier **InstallJeaFeature.ps1** pour qu’il corresponde à la configuration souhaitée du point de terminaison RBAC.
3.  Exécutez InstallJeaFeature.ps1 pour compiler la ressource DSC.
4.  Déployez votre configuration DSC sur toutes vos machines pour appliquer la configuration.

La section suivante explique comment effectuer cette opération à l’aide de la communication à distance PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Déployer sur plusieurs ordinateurs

Pour déployer la configuration que vous avez téléchargée sur plusieurs ordinateurs, vous devez mettre à jour le script **InstallJeaFeatures.ps1** afin d’inclure les groupes de sécurité appropriés pour votre environnement, copier les fichiers sur chacun de vos ordinateurs et appeler les scripts de configuration.
Pour ce faire, vous pouvez utiliser vos outils d’automatisation par défaut. Toutefois, cet article porte sur une approche basée purement sur PowerShell.

Par défaut, le script de configuration crée des groupes de sécurité locaux sur l’ordinateur pour contrôler l’accès à chacun des rôles.
Cela convient pour les ordinateurs d’un groupe de travail et les ordinateurs joints à un domaine, mais si vous effectuez un déploiement dans un environnement de domaine uniquement, vous souhaiterez peut-être associer directement un groupe de sécurité de domaine à chaque rôle.
Pour mettre à jour la configuration afin d’utiliser des groupes de sécurité de domaine, ouvrez **InstallJeaFeatures.ps1** et apportez les modifications suivantes :

1.  Supprimez les 3 ressources **Group** du fichier :
    1.  « Group MS-Readers-Group »
    2.  « Group MS-Hyper-V-Administrators-Group »
    3.  « Group MS-Administrators-Group »
2.  Supprimez les 3 ressources Group de la propriété JeaEndpoint **DependsOn**.
    1.  « [Group]MS-Readers-Group »
    2.  « [Group]MS-Hyper-V-Administrators-Group »
    3.  « [Group]MS-Administrators-Group »
3.  Remplacez les noms de groupe dans la propriété JeaEndpoint **RoleDefinitions** par les groupes de sécurité que vous souhaitez. Par exemple, si vous avez un groupe de sécurité *CONTOSO\MyTrustedAdmins* auquel doit être affecté l’accès au rôle Administrateurs Windows Admin Center, remplacez `'$env:COMPUTERNAME\Windows Admin Center Administrators'` par `'CONTOSO\MyTrustedAdmins'`. Les trois chaînes que vous devez mettre à jour sont les suivantes :
    1.  '$env:COMPUTERNAME\Windows Admin Center Administrators'
    2.  '$env:COMPUTERNAME\Windows Admin Center Hyper-V Administrators'
    3.  '$env:COMPUTERNAME\Windows Admin Center Readers'

> [!NOTE]
> Veillez à utiliser des groupes de sécurité uniques pour chaque rôle. La configuration échoue si le même groupe de sécurité est affecté à plusieurs rôles.

Ensuite, à la fin du fichier **InstallJeaFeatures.ps1**, ajoutez les lignes suivantes de PowerShell en bas du script :

```powershell
Copy-Item "$PSScriptRoot\JustEnoughAdministration" "$env:ProgramFiles\WindowsPowerShell\Modules" -Recurse -Force
$ConfigData = @{
    AllNodes = @()
    ModuleBasePath = @{
        Source = "$PSScriptRoot\Modules"
        Destination = "$env:ProgramFiles\WindowsPowerShell\Modules"
    }
}
InstallJeaFeature -ConfigurationData $ConfigData | Out-Null
Start-DscConfiguration -Path "$PSScriptRoot\InstallJeaFeature" -JobName "Installing JEA for Windows Admin Center" -Force
```

Enfin, vous pouvez copier le dossier contenant les modules, la ressource DSC et la configuration sur chaque nœud cible, puis exécuter le script **InstallJeaFeature.ps1**.
Pour effectuer cette opération à distance à partir de votre station de travail d’administration, vous pouvez exécuter les commandes suivantes :

```powershell
$ComputersToConfigure = 'MyServer01', 'MyServer02'

$ComputersToConfigure | ForEach-Object {
    $session = New-PSSession -ComputerName $_ -ErrorAction Stop
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC\JustEnoughAdministration\" -Destination "$env:ProgramFiles\WindowsPowerShell\Modules\" -ToSession $session -Recurse -Force
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC" -Destination "$env:TEMP\WindowsAdminCenter_RBAC" -ToSession $session -Recurse -Force
    Invoke-Command -Session $session -ScriptBlock { Import-Module JustEnoughAdministration; & "$env:TEMP\WindowsAdminCenter_RBAC\InstallJeaFeature.ps1" } -AsJob
    Disconnect-PSSession $session
}
```
