---
title: Configuration du contrôle d’accès utilisateur et des autorisations
description: Découvrez comment configurer le contrôle d’accès utilisateur et les autorisations à l’aide de Active Directory ou Azure AD (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 20b311e9330880c2b26e2494aabe27bb04891868
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407039"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurer les Access Control et les autorisations des utilisateurs

> S’applique à : Windows Admin Center, Windows Admin Center Preview

Si vous ne l’avez pas déjà fait, familiarisez-vous avec les [options de contrôle d’accès utilisateur dans le centre d’administration Windows](../plan/user-access-options.md) .

> [!NOTE]
> L’accès en fonction du groupe dans le centre d’administration Windows n’est pas pris en charge dans les environnements de groupe de travail ou les domaines non approuvés.

## <a name="gateway-access-role-definitions"></a>Définitions de rôles d’accès à la passerelle

Il existe deux rôles pour l’accès au service de passerelle du centre d’administration Windows :

**Les utilisateurs** de la passerelle peuvent se connecter au service de passerelle du centre d’administration Windows pour gérer les serveurs par le biais de cette passerelle, mais ils ne peuvent pas modifier les autorisations d’accès ni le mécanisme d’authentification utilisé pour s’authentifier auprès de la passerelle.

**Les administrateurs de passerelle** peuvent configurer les utilisateurs qui accèdent à la passerelle, ainsi que la façon dont ils s’authentifient. Seuls les administrateurs de passerelle peuvent afficher et configurer les paramètres d’accès dans le centre d’administration Windows. Les administrateurs locaux sur l’ordinateur de passerelle sont toujours les administrateurs du service de passerelle du centre d’administration Windows.

> [!NOTE]
> L’accès à la passerelle n’implique pas l’accès aux serveurs gérés visibles par la passerelle. Pour gérer un serveur cible, l’utilisateur qui se connecte doit utiliser les informations d’identification (par le biais de leurs informations d’identification Windows transmises ou par le biais des informations d’identification fournies dans la session du centre d’administration Windows à l’aide de l’action **gérer en tant que** ) qui disposent d’un accès administratif. sur ce serveur cible.

## <a name="active-directory-or-local-machine-groups"></a>Active Directory ou groupes d’ordinateurs locaux

Par défaut, les Active Directory ou les groupes d’ordinateurs locaux sont utilisés pour contrôler l’accès à la passerelle. Si vous disposez d’un domaine Active Directory, vous pouvez gérer l’accès de l’utilisateur et de l’administrateur de la passerelle à partir de l’interface du centre d’administration Windows.

Sous l’onglet **utilisateurs** , vous pouvez contrôler qui peut accéder au centre d’administration Windows en tant qu’utilisateur de la passerelle. Par défaut, et si vous ne spécifiez pas de groupe de sécurité, tout utilisateur qui accède à l’URL de la passerelle a accès. Une fois que vous avez ajouté un ou plusieurs groupes de sécurité à la liste des utilisateurs, l’accès est limité aux membres de ces groupes.

Si vous n’utilisez pas un domaine Active Directory dans votre environnement, l’accès est contrôlé par les groupes locaux `Users` et `Administrators` sur l’ordinateur passerelle du centre d’administration Windows.

### <a name="smartcard-authentication"></a>Authentification par carte à puce

Vous pouvez appliquer **l’authentification par carte à puce** en spécifiant un groupe supplémentaire _requis_ pour les groupes de sécurité basés sur une carte à puce. Une fois que vous avez ajouté un groupe de sécurité basé sur une carte à puce, un utilisateur peut accéder au service du centre d’administration Windows uniquement s’il est membre d’un groupe de sécurité et d’un groupe de cartes à puce inclus dans la liste des utilisateurs.

Sous l’onglet **administrateurs** , vous pouvez contrôler qui peut accéder au centre d’administration Windows en tant qu’administrateur de passerelle. Le groupe Administrateurs local sur l’ordinateur aura toujours un accès administrateur complet et ne pourra pas être supprimé de la liste. En ajoutant des groupes de sécurité, vous accordez aux membres de ces groupes des privilèges leur permettant de modifier les paramètres de la passerelle du centre d’administration Windows. La liste administrateurs prend en charge l’authentification par carte à puce de la même façon que la liste des utilisateurs : avec la condition AND pour un groupe de sécurité et un groupe de cartes à puce.

## <a name="azure-active-directory"></a>Azure Active Directory

Si votre organisation utilise Azure Active Directory (Azure AD), vous pouvez choisir d’ajouter une couche de sécurité **supplémentaire** au centre d’administration Windows en exigeant Azure ad l’authentification pour accéder à la passerelle. Pour accéder au centre d’administration Windows, le **compte Windows** de l’utilisateur doit également avoir accès au serveur de passerelle (même si Azure ad authentification est utilisée). Lorsque vous utilisez Azure AD, vous gérez les autorisations d’accès administrateur et utilisateur du centre d’administration Windows à partir du portail Azure, plutôt qu’à partir de l’interface utilisateur du centre d’administration Windows.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Accès au centre d’administration Windows lors de l’activation de l’authentification Azure AD

Selon le navigateur utilisé, certains utilisateurs qui accèdent au centre d’administration Windows avec Azure AD l’authentification configurée recevront une invite supplémentaire **du navigateur** où ils doivent fournir leurs informations d’identification de compte Windows pour l’ordinateur sur lequel Le centre d’administration Windows est installé. Après avoir entré ces informations, les utilisateurs obtiendront l’invite d’authentification supplémentaire Azure Active Directory, qui requiert les informations d’identification d’un compte Azure disposant de l’accès dans l’application Azure AD dans Azure.

> [!NOTE]
> Les utilisateurs qui ont un compte Windows disposant de **droits d’administrateur** sur l’ordinateur de passerelle ne seront pas invités à fournir l’authentification Azure ad.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configuration de l’authentification Azure Active Directory pour la version préliminaire du centre d’administration Windows

Accédez à **paramètres**du centre d’administration Windows  > **accès** et utilisez le commutateur bascule pour activer « utiliser Azure Active Directory pour ajouter une couche de sécurité à la passerelle ». Si vous n’avez pas inscrit la passerelle auprès d’Azure, vous serez guidé à le faire à ce stade.

Par défaut, tous les membres du locataire Azure AD disposent d’un accès utilisateur au service de passerelle du centre d’administration Windows. Seuls les administrateurs locaux sur l’ordinateur de passerelle disposent d’un accès administrateur à la passerelle du centre d’administration Windows. Notez que les droits des administrateurs locaux sur l’ordinateur de passerelle ne peuvent pas être restreints-les administrateurs locaux peuvent faire quoi que ce soit Azure AD est utilisé pour l’authentification.

Si vous souhaitez accorder à des utilisateurs ou des groupes spécifiques Azure AD ou à un administrateur de passerelle un accès au service Centre d’administration Windows, vous devez effectuer les opérations suivantes :

1.  Accédez à votre centre d’administration Windows Azure AD application dans le Portail Azure à l’aide du lien hypertexte fourni dans paramètres d’accès. Notez que ce lien hypertexte est disponible uniquement lorsque l’option Azure Active Directory l’authentification est activée. 
    -   Vous pouvez également trouver votre application dans le Portail Azure en accédant à **Azure Active Directory**applications d'**entreprise** >   > **toutes les applications** et en recherchant **WindowsAdminCenter** (l’application Azure ad sera nommée WindowsAdminCenter-<gateway name>). Si vous n’avez pas de résultats de recherche, vérifiez que l' **affichage** est défini sur **toutes les applications**, que État de l' **application** est défini sur **tout** , cliquez sur appliquer, puis essayez votre recherche. Une fois que vous avez trouvé l’application, accédez à **utilisateurs et groupes** .
2.  Dans l’onglet Propriétés, affectez à l’affectation de l' **utilisateur** la valeur Oui.
    Une fois que vous avez effectué cette opération, seuls les membres listés dans l’onglet **utilisateurs et groupes** pourront accéder à la passerelle du centre d’administration Windows.
3.  Sous l’onglet utilisateurs et groupes, sélectionnez **Ajouter un utilisateur**. Vous devez attribuer un rôle d’administrateur de passerelle ou d’utilisateur de passerelle pour chaque utilisateur/groupe ajouté.

Une fois que vous activez l’authentification Azure AD, le service de passerelle redémarre et vous devez actualiser votre navigateur. Vous pouvez mettre à jour l’accès utilisateur pour l’application SME Azure AD dans le Portail Azure à tout moment.

Les utilisateurs sont invités à se connecter à l’aide de leur identité Azure Active Directory lorsqu’ils tentent d’accéder à l’URL de la passerelle du centre d’administration Windows. N’oubliez pas que les utilisateurs doivent également être membres des utilisateurs locaux sur le serveur de passerelle pour accéder au centre d’administration Windows.

Les utilisateurs et les administrateurs peuvent afficher leur compte actuellement connecté et se déconnecter de ce compte Azure AD à partir de l’onglet **compte** des paramètres du centre d’administration Windows.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configuration de l’authentification Azure Active Directory pour le centre d’administration Windows

[Pour configurer l’authentification Azure ad, vous devez d’abord inscrire votre passerelle auprès d’Azure](azure-integration.md) (vous ne devez effectuer cette opération qu’une seule fois pour votre passerelle du centre d’administration Windows). Cette étape crée une application Azure AD à partir de laquelle vous pouvez gérer l’accès de l’administrateur de la passerelle et de l’utilisateur de la passerelle.

Si vous souhaitez accorder à des utilisateurs ou des groupes spécifiques Azure AD ou à un administrateur de passerelle un accès au service Centre d’administration Windows, vous devez effectuer les opérations suivantes :

1.  Accédez à votre application SME Azure AD dans le Portail Azure. 
    -   Lorsque vous cliquez sur **modifier le contrôle d’accès** , puis sélectionnez **Azure Active Directory** dans les paramètres d’accès du centre d’administration Windows, vous pouvez utiliser le lien hypertexte fourni dans l’interface utilisateur pour accéder à votre application Azure ad dans le portail Azure. Ce lien hypertexte est également disponible dans les paramètres d’accès une fois que vous avez cliqué sur enregistrer et que vous avez sélectionné Azure AD comme fournisseur d’identité de contrôle d’accès.
    -   Vous pouvez également trouver votre application dans le Portail Azure en accédant à **Azure Active Directory**applications d'**entreprise** >   > **toutes les applications** et en recherchant **SME** (l’application Azure ad s’intitule SME-<gateway>). Si vous n’avez pas de résultats de recherche, vérifiez que l' **affichage** est défini sur **toutes les applications**, que État de l' **application** est défini sur **tout** , cliquez sur appliquer, puis essayez votre recherche. Une fois que vous avez trouvé l’application, accédez à **utilisateurs et groupes** .
2.  Dans l’onglet Propriétés, affectez à l’affectation de l' **utilisateur** la valeur Oui.
    Une fois que vous avez effectué cette opération, seuls les membres listés dans l’onglet **utilisateurs et groupes** pourront accéder à la passerelle du centre d’administration Windows.
3.  Sous l’onglet utilisateurs et groupes, sélectionnez **Ajouter un utilisateur**. Vous devez attribuer un rôle d’administrateur de passerelle ou d’utilisateur de passerelle pour chaque utilisateur/groupe ajouté.

Une fois que vous avez enregistré le contrôle d’accès Azure AD dans le volet **modifier le contrôle d’accès** , le service de passerelle redémarre et vous devez actualiser votre navigateur. Vous pouvez mettre à jour l’accès utilisateur pour le centre d’administration Windows Azure AD application dans le Portail Azure à tout moment. 

Les utilisateurs sont invités à se connecter à l’aide de leur identité Azure Active Directory lorsqu’ils tentent d’accéder à l’URL de la passerelle du centre d’administration Windows. N’oubliez pas que les utilisateurs doivent également être membres des utilisateurs locaux sur le serveur de passerelle pour accéder au centre d’administration Windows. 

À l’aide de l’onglet **Azure** des paramètres généraux du centre d’administration Windows, les utilisateurs et les administrateurs peuvent afficher leur compte actuellement connecté et déconnecter ce compte Azure ad.

### <a name="conditional-access-and-multi-factor-authentication"></a>Accès conditionnel et multi-Factor Authentication

L’un des avantages de l’utilisation de Azure AD comme couche supplémentaire de sécurité pour contrôler l’accès à la passerelle du centre d’administration Windows est que vous pouvez tirer parti des puissantes fonctionnalités de sécurité de Azure AD telles que l’accès conditionnel et l’authentification multifacteur. 

[En savoir plus sur la configuration de l’accès conditionnel avec Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurer l'authentification unique

**Authentification unique lorsqu’elle est déployée en tant que service sur Windows Server**

Lorsque vous installez le centre d’administration Windows sur Windows 10, il est prêt à utiliser l’authentification unique. Toutefois, si vous envisagez d’utiliser le centre d’administration Windows sur Windows Server, vous devez configurer une certaine forme de délégation Kerberos dans votre environnement avant de pouvoir utiliser l’authentification unique. La délégation configure l’ordinateur de la passerelle comme étant approuvé pour déléguer au nœud cible. 

Pour configurer la [délégation avec restriction basée sur les ressources](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) dans votre environnement, exécutez les applets de commande PowerShell suivantes. (N’oubliez pas que cela nécessite un contrôleur de domaine exécutant Windows Server 2012 ou version ultérieure).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

Dans cet exemple, la passerelle du centre d’administration Windows est installée sur le serveur **WindowsAdminCenterGW**, et le nom du nœud cible est **ManagedNode**.

Pour supprimer cette relation, exécutez l’applet de commande suivante :

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Contrôle d'accès basé sur les rôles

Le contrôle d’accès en fonction du rôle vous permet de fournir aux utilisateurs un accès limité à l’ordinateur au lieu d’en faire des administrateurs locaux complets.
[En savoir plus sur le contrôle d’accès en fonction du rôle et les rôles disponibles.](../plan/user-access-options.md#role-based-access-control)

La configuration de RBAC est constituée de deux étapes : l’activation de la prise en charge sur le ou les ordinateurs cibles et l’attribution d’utilisateurs aux rôles appropriés.

> [!TIP]
> Vérifiez que vous disposez de privilèges d’administrateur local sur les ordinateurs sur lesquels vous configurez la prise en charge du contrôle d’accès en fonction du rôle.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Appliquer le contrôle d’accès en fonction du rôle à un seul ordinateur

Le modèle de déploiement à un seul ordinateur est idéal pour les environnements simples avec seulement quelques ordinateurs à gérer.
La configuration d’un ordinateur avec prise en charge du contrôle d’accès en fonction du rôle entraîne les modifications suivantes :

-   Les modules PowerShell avec les fonctions requises par le centre d’administration Windows seront installés sur votre lecteur système, sous `C:\Program Files\WindowsPowerShell\Modules`. Tous les modules vont commencer par **Microsoft. SME**
-   La configuration d’état souhaité exécutera une configuration unique pour configurer un point de terminaison d’administration juste suffisant sur l’ordinateur, nommé **Microsoft. SME. PowerShell**. Ce point de terminaison définit les 3 rôles utilisés par le centre d’administration Windows et s’exécute en tant qu’administrateur local temporaire lorsqu’un utilisateur s’y connecte.
-   3 de nouveaux groupes locaux seront créés pour contrôler les utilisateurs qui ont accès aux rôles :
    -   Administrateurs du centre d’administration Windows
    -   Administrateurs Hyper-V du centre d’administration Windows
    -   Lecteurs du centre d’administration Windows

Pour activer la prise en charge du contrôle d’accès en fonction du rôle sur un seul ordinateur, procédez comme suit :

1.  Ouvrez le centre d’administration Windows et connectez-vous à l’ordinateur que vous souhaitez configurer avec le contrôle d’accès en fonction du rôle à l’aide d’un compte disposant de privilèges d’administrateur local sur l’ordinateur cible.
2.  Dans l’outil **vue d’ensemble** , cliquez sur **paramètres** > **contrôle d’accès en fonction du rôle**.
3.  Cliquez sur **appliquer** au bas de la page pour activer la prise en charge du contrôle d’accès en fonction du rôle sur l’ordinateur cible. Le processus d’application implique la copie de scripts PowerShell et l’appel d’une configuration (à l’aide de la configuration d’état souhaité PowerShell) sur l’ordinateur cible. Cette opération peut prendre jusqu’à 10 minutes et entraîner le redémarrage de WinRM. Cette opération déconnectera temporairement les utilisateurs du centre d’administration Windows, de PowerShell et de WMI.
4.  Actualisez la page pour vérifier l’état du contrôle d’accès en fonction du rôle. Lorsqu’il est prêt à être utilisé, l’état passe à **appliqué**.

Une fois la configuration appliquée, vous pouvez affecter des utilisateurs aux rôles :

1.  Ouvrez l’outil **utilisateurs et groupes locaux** et accédez à l’onglet **groupes** .
2.  Sélectionnez le groupe de **lecteurs du centre d’administration Windows** .
3.  Dans le volet d' *informations* en bas, cliquez sur **Ajouter un utilisateur** , puis entrez le nom d’un utilisateur ou d’un groupe de sécurité qui doit avoir un accès en lecture seule au serveur via le centre d’administration Windows. Les utilisateurs et les groupes peuvent provenir de l’ordinateur local ou du domaine de votre Active Directory.
4.  Répétez les étapes 2-3 pour les groupes Administrateurs **Hyper-V du centre d’administration Windows** et **administrateurs du centre d’administration Windows** .

Vous pouvez également remplir ces groupes de manière cohérente dans votre domaine en configurant un objet stratégie de groupe avec le [paramètre de stratégie groupes restreints](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Appliquer le contrôle d’accès en fonction du rôle à plusieurs ordinateurs

Dans un déploiement d’entreprise de grande taille, vous pouvez utiliser vos outils d’automatisation existants pour pousser la fonctionnalité de contrôle d’accès en fonction du rôle sur vos ordinateurs en téléchargeant le package de configuration à partir de la passerelle du centre d’administration Windows.
Le package de configuration est conçu pour être utilisé avec la configuration d’état souhaité PowerShell, mais vous pouvez l’adapter pour qu’il fonctionne avec votre solution d’automatisation par défaut.

#### <a name="download-the-role-based-access-control-configuration"></a>Télécharger la configuration de contrôle d’accès en fonction du rôle

Pour télécharger le package de configuration de contrôle d’accès en fonction du rôle, vous devez avoir accès au centre d’administration Windows et à une invite de commandes PowerShell.

Si vous exécutez la passerelle du centre d’administration Windows en mode service sur Windows Server, utilisez la commande suivante pour télécharger le package de configuration.
Veillez à mettre à jour l’adresse de la passerelle avec celle qui convient à votre environnement.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Si vous exécutez la passerelle du centre d’administration Windows sur votre ordinateur Windows 10, exécutez la commande suivante à la place :

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Lorsque vous développez l’archive zip, vous voyez s’afficher la structure de dossiers suivante :

- InstallJeaFeatures. ps1
- JustEnoughAdministration (répertoire)
- Modules (répertoire)
    - Microsoft. SME. \* (répertoires)
    - WindowsAdminCenter. Jea (répertoire)

Pour configurer la prise en charge du contrôle d’accès en fonction du rôle sur un nœud, vous devez effectuer les actions suivantes :

1.  Copiez les modules JustEnoughAdministration, Microsoft. SME. \* et WindowsAdminCenter. Jea dans le répertoire du module PowerShell sur l’ordinateur cible. En général, il se trouve à l’emplacement `C:\Program Files\WindowsPowerShell\Modules`.
2.  Mettez à jour le fichier **InstallJeaFeature. ps1** pour qu’il corresponde à la configuration souhaitée pour le point de terminaison RBAC.
3.  Exécutez InstallJeaFeature. ps1 pour compiler la ressource DSC.
4.  Déployez votre configuration DSC sur toutes vos machines pour appliquer la configuration.

La section suivante explique comment effectuer cette opération à l’aide de la communication à distance PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Déployer sur plusieurs ordinateurs

Pour déployer la configuration que vous avez téléchargée sur plusieurs ordinateurs, vous devez mettre à jour le script **InstallJeaFeatures. ps1** pour inclure les groupes de sécurité appropriés pour votre environnement, copier les fichiers sur chacun de vos ordinateurs et appeler la scripts de configuration.
Pour ce faire, vous pouvez utiliser vos outils d’automatisation préférés. Toutefois, cet article se concentre sur une approche basée sur PowerShell.

Par défaut, le script de configuration crée des groupes de sécurité locaux sur l’ordinateur pour contrôler l’accès à chacun des rôles.
Cela convient pour les ordinateurs de groupe de travail et les ordinateurs joints à un domaine, mais si vous effectuez un déploiement dans un environnement de domaine uniquement, vous souhaiterez peut-être associer directement un groupe de sécurité de domaine à chaque rôle.
Pour mettre à jour la configuration afin d’utiliser des groupes de sécurité de domaine, ouvrez **InstallJeaFeatures. ps1** et apportez les modifications suivantes :

1.  Supprimez les 3 ressources de **groupe** du fichier :
    1.  « Groupe MS-Readers-Group »
    2.  « Groupe MS-Hyper-V-administrateurs-groupe »
    3.  « Groupe MS-administrateurs-groupe »
2.  Supprimer les 3 ressources de groupe de la propriété **DependsOn** JeaEndpoint
    1.  « [Group] MS-Readers-Group »
    2.  « [Group] MS-Hyper-V-Administrators-Group »
    3.  « [Group] MS-Administrators-Group »
3.  Modifiez les noms de groupe dans la propriété JeaEndpoint **RoleDefinitions** par les groupes de sécurité souhaités. Par exemple, si vous avez un groupe de sécurité *CONTOSO\MyTrustedAdmins* auquel un accès au rôle Administrateurs du centre d’administration Windows doit être affecté, remplacez `'$env:COMPUTERNAME\Windows Admin Center Administrators'` par `'CONTOSO\MyTrustedAdmins'`. Les trois chaînes que vous devez mettre à jour sont les suivantes :
    1.  « $env : administrateurs du centre d’administration COMPUTERNAME\Windows »
    2.  « $env : administrateurs Hyper-V du centre d’administration COMPUTERNAME\Windows »
    3.  « $env : COMPUTERNAME\Windows Admin Center Readers »

> [!NOTE]
> Veillez à utiliser des groupes de sécurité uniques pour chaque rôle. La configuration échoue si le même groupe de sécurité est attribué à plusieurs rôles.

Ensuite, à la fin du fichier **InstallJeaFeatures. ps1** , ajoutez les lignes suivantes de PowerShell au bas du script :

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

Enfin, vous pouvez copier le dossier contenant les modules, la ressource DSC et la configuration sur chaque nœud cible et exécuter le script **InstallJeaFeature. ps1** .
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
