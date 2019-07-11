---
title: Configuration des autorisations et contrôle d’accès utilisateur
description: Découvrez comment configurer le contrôle d’accès utilisateur et les autorisations à l’aide d’Active Directory ou Azure AD (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ef87a3bcc5bd0b924a938f055307a0a87cb60d0b
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792320"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurer les autorisations et contrôle d’accès utilisateur

> S’applique à : Windows Admin Center, Windows Admin Center Preview

Si vous n’avez pas déjà, vous familiariser avec la [options de contrôle d’accès utilisateur dans Windows Admin Center](../plan/user-access-options.md)

> [!NOTE]
> L’accès en fonction de groupe dans Windows Admin Center n’est pas pris en charge dans les environnements de groupe de travail ou entre des domaines non approuvés.

## <a name="gateway-access-role-definitions"></a>Définitions de rôle de passerelle accès

Il existe deux rôles pour l’accès au service de passerelle Windows Admin Center :

**Utilisateurs de la passerelle** peut se connecter au service de passerelle Windows Admin Center pour gérer les serveurs via cette passerelle, mais ils ne peuvent pas modifier les autorisations d’accès ni le mécanisme d’authentification utilisé pour authentifier à la passerelle.

**Administrateurs de passerelles** configurables qui obtient l’accès, ainsi que la manière dont les utilisateurs s’authentifient à la passerelle. Seuls les administrateurs de passerelle peuvent afficher et configurer les paramètres d’accès dans Windows Admin Center. Administrateurs locaux sur l’ordinateur de passerelle sont toujours des administrateurs du service de passerelle Windows Admin Center.

> [!NOTE]
> Accès à la passerelle n’implique pas accéder aux serveurs gérés visibles par la passerelle. Pour gérer un serveur cible, l’utilisateur connecté doit utiliser les informations d’identification (via ses transmis via Windows informations d’identification ou via les informations d’identification fournies dans la session Windows Admin Center à l’aide du **gérer en tant que** action) qui ont accès administratif à ce serveur cible.

## <a name="active-directory-or-local-machine-groups"></a>Active Directory ou des groupes d’ordinateurs locaux

Par défaut, les groupes d’ordinateurs locaux ou Active Directory sont utilisés pour contrôler l’accès à la passerelle. Si vous avez un domaine Active Directory, vous pouvez gérer la passerelle utilisateur et administrateur accéder à partir de l’interface Windows Admin Center.

Sur le **utilisateurs** onglet, vous pouvez contrôler qui peut accéder à Windows Admin Center en tant qu’un utilisateur de la passerelle. Par défaut, et si vous ne spécifiez pas un groupe de sécurité, tout utilisateur qui accède à l’URL de la passerelle a accès. Une fois que vous ajoutez un ou plusieurs groupes de sécurité à la liste des utilisateurs, l’accès est limité aux membres de ces groupes.

Si vous n’utilisez pas un domaine Active Directory dans votre environnement, l’accès est contrôlé par le `Users` et `Administrators` des groupes locaux sur l’ordinateur de passerelle Windows Admin Center.

### <a name="smartcard-authentication"></a>Authentification par carte à puce

Vous pouvez appliquer **l’authentification par carte à puce** en spécifiant un autre _requis_ groupe pour les groupes de sécurité basée sur une carte à puce. Une fois que vous avez ajouté un groupe de sécurité basée sur une carte à puce, un utilisateur peut uniquement accéder au Windows Admin Center service si ils sont membres d’un groupe de sécurité et un groupe de carte à puce est inclus dans la liste des utilisateurs.

Sur le **administrateurs** onglet, vous pouvez contrôler qui peut accéder à Windows Admin Center en tant qu’un administrateur de passerelle. Le groupe Administrateurs local sur l’ordinateur aura toujours un accès administrateur complet et ne peut pas être supprimé de la liste. En ajoutant des groupes de sécurité, vous accorder à des membres de ces privilèges de groupes pour modifier les paramètres de la passerelle Windows Admin Center. La liste d’administrateurs prend en charge l’authentification par carte à puce dans la même façon que la liste des utilisateurs : avec la condition AND pour un groupe de sécurité et un groupe de carte à puce.

## <a name="azure-active-directory"></a>Azure Active Directory

Si votre organisation utilise Azure Active Directory (Azure AD), vous pouvez choisir d’ajouter un **supplémentaires** couche de sécurité pour Windows Admin Center en exigeant l’authentification Azure AD pour accéder à la passerelle. Pour accéder à Windows Admin Center, l’utilisateur **compte de Windows** doit également avoir accès au serveur de passerelle (même si l’authentification Azure AD est utilisée). Lorsque vous utilisez Azure AD, vous gérerez des autorisations d’accès utilisateur et administrateur Windows Admin Center à partir du portail Azure, plutôt que depuis l’interface utilisateur de Windows Admin Center.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>L’accès à Windows Admin Center lorsque l’authentification Azure AD est activée.

Selon le navigateur utilisé, certains utilisateurs l’accès à Windows Admin Center avec l’authentification Azure AD configurée reçoit une invite supplémentaire **à partir du navigateur** où ils doivent fournir leurs informations d’identification du compte Windows pour l’ordinateur sur lequel Windows Admin Center est installé. Après avoir entré ces informations, les utilisateurs reçoivent l’invite d’authentification Azure Active Directory supplémentaire, ce qui nécessite les informations d’identification d’un compte Azure qui a été accordé l’accès dans l’application Azure AD dans Azure.

> [!NOTE]
> Les utilisateurs de Windows a compte **droits d’administrateur** sur la passerelle machine n’est pas invitée pour l’authentification Azure AD.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configuration de l’authentification Azure Active Directory pour Windows Admin Center Preview

Accédez à Windows Admin Center **paramètres** > **accès** et utilisez le bouton bascule pour activer « utiliser Azure Active Directory pour ajouter une couche de sécurité à la passerelle ». Si vous n’êtes pas inscrit la passerelle vers Azure, vous serez guidé pour ce faire à ce stade.

Par défaut, tous les membres du locataire Azure AD ont l’accès utilisateur au service de passerelle Windows Admin Center. Seuls les administrateurs locaux sur l’ordinateur de passerelle ont un accès administrateur à la passerelle Windows Admin Center. Notez que les droits des administrateurs locaux sur l’ordinateur de passerelle ne peut pas être limités, administrateurs locaux peuvent faire tout ce qu’Azure AD est utilisé pour l’authentification.

Si vous souhaitez Azure AD spécifique de donner aux utilisateurs ou d’utilisateur de passerelle de groupes ou d’accès d’administrateur de passerelle pour le service Windows Admin Center, vous devez procédez comme suit :

1.  Accédez à votre application Windows Admin Center Azure AD dans le portail Azure en utilisant le lien hypertexte fourni dans les paramètres d’accès. Notez que ce lien hypertexte est disponible uniquement lorsque l’authentification Azure Active Directory est activée. 
    -   Vous pouvez également trouver votre application dans le portail Azure en accédant à **Azure Active Directory** > **applications d’entreprise** > **detouteslesapplications** et la recherche **WindowsAdminCenter** (l’application Azure AD sera nommée WindowsAdminCenter -<gateway name>). Si vous n’obtenez aucun résultat, vérifiez **afficher** a la valeur **toutes les applications**, **état de l’application** a la valeur **n’importe quel** et cliquez sur Appliquer, Recommencez votre recherche. Une fois que vous avez trouvé l’application, accédez à **utilisateurs et groupes**
2.  Sous l’onglet Propriétés, définissez **affectation utilisateur requise** sur Oui.
    Une fois que vous avez fait, seuls les membres répertoriés dans le **utilisateurs et groupes** onglet sera en mesure d’accéder à la passerelle Windows Admin Center.
3.  Dans l’onglet utilisateurs et groupes, sélectionnez **ajouter un utilisateur**. Vous devez attribuer un utilisateur de la passerelle ou le rôle d’administrateur de passerelle pour chaque utilisateur ou groupe ajouté.

Une fois que vous activez l’authentification Azure AD, le service de passerelle redémarre et que vous devez actualiser votre navigateur. Vous pouvez mettre à jour l’accès utilisateur pour l’application de PME Azure AD dans le portail Azure à tout moment.

Les utilisateurs seront invités à se connecter à l’aide de leur identité Azure Active Directory lorsqu’ils tentent d’accéder à l’URL de la passerelle Windows Admin Center. N’oubliez pas que les utilisateurs doivent également être membre des utilisateurs locaux sur le serveur de passerelle pour accéder à Windows Admin Center.

Les utilisateurs et les administrateurs peuvent afficher leur compte actuellement connecté et ainsi de la déconnexion de ce compte Azure AD à partir de la **compte** onglet des paramètres de Windows Admin Center.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configuration de l’authentification Azure Active Directory pour Windows Admin Center

[Pour configurer l’authentification Azure AD, vous devez tout d’abord inscrire votre passerelle avec Azure](azure-integration.md) (mais il vous suffit d’effectuer cette opération une fois pour votre passerelle Windows Admin Center). Cette étape crée une application Azure AD à partir de laquelle vous pouvez gérer l’utilisateur de la passerelle et l’accès d’administrateur de passerelle.

Si vous souhaitez Azure AD spécifique de donner aux utilisateurs ou d’utilisateur de passerelle de groupes ou d’accès d’administrateur de passerelle pour le service Windows Admin Center, vous devez procédez comme suit :

1.  Accédez à votre application SME Azure AD dans le portail Azure. 
    -   Lorsque vous cliquez sur **contrôle d’accès de modification** , puis sélectionnez **Azure Active Directory** à partir des paramètres Windows Admin Center accès, vous pouvez utiliser le lien hypertexte fourni dans l’interface utilisateur pour accéder à votre Azure AD application dans le portail Azure. Ce lien hypertexte est également disponible dans les paramètres d’accès une fois que vous cliquez sur Enregistrer et que vous avez sélectionné Azure AD comme fournisseur d’identité de contrôle l’accès.
    -   Vous pouvez également trouver votre application dans le portail Azure en accédant à **Azure Active Directory** > **applications d’entreprise** > **detouteslesapplications** et la recherche **SME** (l’application Azure AD sera nommée SME -<gateway>). Si vous n’obtenez aucun résultat, vérifiez **afficher** a la valeur **toutes les applications**, **état de l’application** a la valeur **n’importe quel** et cliquez sur Appliquer, Recommencez votre recherche. Une fois que vous avez trouvé l’application, accédez à **utilisateurs et groupes**
2.  Sous l’onglet Propriétés, définissez **affectation utilisateur requise** sur Oui.
    Une fois que vous avez fait, seuls les membres répertoriés dans le **utilisateurs et groupes** onglet sera en mesure d’accéder à la passerelle Windows Admin Center.
3.  Dans l’onglet utilisateurs et groupes, sélectionnez **ajouter un utilisateur**. Vous devez attribuer un utilisateur de la passerelle ou le rôle d’administrateur de passerelle pour chaque utilisateur ou groupe ajouté.

Une fois que vous enregistrez l’annonce Azure contrôle d’accès dans le **contrôle d’accès de modification** volet, le service de passerelle redémarre et vous devez actualiser votre navigateur. Vous pouvez mettre à jour l’accès utilisateur pour l’application Windows Admin Center Azure AD dans le portail Azure à tout moment. 

Les utilisateurs seront invités à se connecter à l’aide de leur identité Azure Active Directory lorsqu’ils tentent d’accéder à l’URL de la passerelle Windows Admin Center. N’oubliez pas que les utilisateurs doivent également être membre des utilisateurs locaux sur le serveur de passerelle pour accéder à Windows Admin Center. 

À l’aide de la **Azure** onglet de paramètres généraux de Windows Admin Center, les utilisateurs et les administrateurs permettre afficher leur compte actuellement connecté et ainsi de la déconnexion de ce compte Azure AD.

### <a name="conditional-access-and-multi-factor-authentication"></a>Accès conditionnel et l’authentification multifacteur

Un des avantages de l’utilisation d’Azure AD comme une couche supplémentaire de sécurité pour contrôler l’accès à la passerelle Windows Admin Center est que vous pouvez tirer parti puissantes fonctionnalités de sécurité d’Azure AD telles que l’accès conditionnel et l’authentification multifacteur. 

[En savoir plus sur la configuration de l’accès conditionnel à Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurer l’authentification unique

**L’authentification unique lors du déploiement en tant que Service sur Windows Server**

Lorsque vous installez Windows Admin Center sur Windows 10, il est prêt à utiliser l’authentification unique. Toutefois, si vous prévoyez d’utiliser Windows Admin Center sur Windows Server, vous devez configurer une forme de délégation Kerberos contrainte dans votre environnement avant de pouvoir utiliser l’authentification unique. La délégation configure l’ordinateur de passerelle comme approuvés pour déléguer au nœud cible. 

Pour configurer [délégation contrainte basée sur les ressources](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) dans votre environnement, exécutez les applets de commande PowerShell suivante. (En être conscient que cela nécessite un contrôleur de domaine exécutant Windows Server 2012 ou version ultérieure).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

Dans cet exemple, la passerelle Windows Admin Center est installée sur le serveur **WindowsAdminCenterGW**, et le nom du nœud cible est **ManagedNode**.

Pour supprimer cette relation, exécutez l’applet de commande suivante :

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Contrôle d’accès en fonction du rôle

Contrôle d’accès en fonction du rôle vous permet de fournir aux utilisateurs un accès limité à l’ordinateur au lieu de faire les administrateurs locales complet.
[En savoir plus sur le contrôle d’accès en fonction du rôle et les rôles disponibles.](../plan/user-access-options.md#role-based-access-control)

Configuration de RBAC comporte 2 étapes : l’activation de la prise en charge sur les ordinateurs cible et l’affectation d’utilisateurs aux rôles appropriés.

> [!TIP]
> Vérifiez que vous disposez des privilèges d’administrateur local sur les ordinateurs où vous configurez la prise en charge pour le contrôle d’accès en fonction du rôle.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Appliquer le contrôle d’accès en fonction du rôle à un seul ordinateur

Le modèle de déploiement de machine unique est idéal pour les environnements simples avec peu d’ordinateurs à gérer.
Configuration d’un ordinateur prenant en charge le contrôle d’accès en fonction du rôle entraîne les modifications suivantes :

-   Les modules PowerShell avec les fonctions requises par Windows Admin Center seront installés sur votre lecteur système, sous `C:\Program Files\WindowsPowerShell\Modules`. Tous les modules démarrera avec **Microsoft.Sme**
-   Desired State Configuration exécutera une configuration à usage unique pour configurer un point de terminaison Just Enough Administration sur l’ordinateur nommé **Microsoft.Sme.PowerShell**. Ce point de terminaison définit les 3 rôles utilisés par Windows Admin Center et s’exécute en tant qu’un administrateur local temporaire lorsqu’un utilisateur se connecte à ce dernier.
-   3 nouveaux groupes locaux seront créés pour contrôler quels utilisateurs sont affectés accès à quels rôles :
    -   Administrateurs Windows Admin Center
    -   Administrateurs Hyper-V de Windows Admin Center
    -   Lecteurs Windows Admin Center

Pour activer la prise en charge pour le contrôle d’accès en fonction du rôle sur un seul ordinateur, procédez comme suit :

1.  Ouvrez Windows Admin Center et connectez-vous à l’ordinateur que vous souhaitez configurer avec le contrôle d’accès en fonction du rôle à l’aide d’un compte avec des privilèges d’administrateur local sur l’ordinateur cible.
2.  Sur le **vue d’ensemble** d’outils, cliquez sur **paramètres** > **contrôle d’accès en fonction du rôle**.
3.  Cliquez sur **appliquer** en bas de la page pour activer la prise en charge pour le contrôle d’accès en fonction du rôle sur l’ordinateur cible. Le processus d’application implique la copie des scripts PowerShell et appel d’une configuration (à l’aide de PowerShell Desired State Configuration) sur l’ordinateur cible. Il peut prendre jusqu'à 10 minutes pour terminer et provoquera le redémarrage de WinRM. Cela se déconnecte temporairement les utilisateurs Windows Admin Center, PowerShell et WMI.
4.  Actualisez la page pour vérifier l’état du contrôle d’accès en fonction du rôle. Lorsqu’il est prêt à être utilisé, l’état passe à **appliqué**.

Une fois que la configuration est appliquée, vous pouvez affecter des utilisateurs aux rôles :

1.  Ouvrez le **utilisateurs et groupes locaux** outil et accédez à la **groupes** onglet.
2.  Sélectionnez le **Windows Admin Center lecteurs** groupe.
3.  Dans le *détails* volet en bas, cliquez sur **ajouter un utilisateur** et entrez le nom d’un utilisateur ou groupe de sécurité qui doit avoir un accès en lecture seule au serveur par le biais de Windows Admin Center. Les utilisateurs et groupes peuvent provenir de l’ordinateur local ou de votre domaine Active Directory.
4.  Répétez les étapes 2 et 3 pour le **administrateurs Hyper-V de Windows Admin Center** et **Windows Admin Center administrateurs** groupes.

Vous pouvez également remplir ces groupes de manière cohérente dans votre domaine en configurant un objet de stratégie de groupe avec le [paramètre de stratégie Groupes restreints](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Appliquer le contrôle d’accès en fonction du rôle sur plusieurs ordinateurs

Dans un déploiement de grande entreprise, vous pouvez utiliser vos outils d’automatisation existante pour envoyer la fonctionnalité de contrôle d’accès en fonction du rôle à vos ordinateurs en téléchargeant le package de configuration à partir de la passerelle Windows Admin Center.
Le package de configuration est conçu pour être utilisé avec PowerShell Desired State Configuration, mais vous pouvez adapter qu’il fonctionne avec votre solution d’automatisation préféré.

#### <a name="download-the-role-based-access-control-configuration"></a>Télécharger la configuration de contrôle d’accès en fonction du rôle

Pour télécharger le package de configuration de contrôle de l’accès en fonction du rôle, vous devez avoir accès à Windows Admin Center et une invite PowerShell.

Si vous utilisez la passerelle Windows Admin Center en mode de service sur Windows Server, utilisez la commande suivante pour télécharger le package de configuration.
Veillez à mettre à jour l’adresse de passerelle avec celui qui convient pour votre environnement.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Si vous utilisez la passerelle Windows Admin Center sur votre ordinateur Windows 10, exécutez la commande suivante à la place :

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Lorsque vous développez l’archive zip, vous verrez la structure de dossiers suivante :

- InstallJeaFeatures.ps1
- JustEnoughAdministration (répertoire)
- Modules (répertoire)
    - Microsoft.SME. \* (répertoires)
    - WindowsAdminCenter.Jea (directory)

Pour configurer la prise en charge pour le contrôle d’accès en fonction du rôle sur un nœud, vous devez effectuer les actions suivantes :

1.  Copiez la JustEnoughAdministration, Microsoft.SME. \*et les modules WindowsAdminCenter.Jea dans le répertoire de module PowerShell sur l’ordinateur cible. En règle générale, il se trouve dans `C:\Program Files\WindowsPowerShell\Modules`.
2.  Mise à jour **InstallJeaFeature.ps1** fichier pour correspondre à la configuration souhaitée pour le point de terminaison RBAC.
3.  Exécutez InstallJeaFeature.ps1 pour compiler la ressource DSC.
4.  Déployer votre configuration DSC sur tous vos ordinateurs pour appliquer la configuration.

La section suivante explique comment effectuer cette opération à l’aide de la communication à distance PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Déployer sur plusieurs ordinateurs

Pour déployer la configuration que vous avez téléchargé sur plusieurs ordinateurs, vous devrez mettre à jour le **InstallJeaFeatures.ps1** script pour inclure des groupes de sécurité appropriés pour votre environnement, copiez les fichiers sur chacun de vos ordinateurs, et appeler les scripts de configuration.
Vous pouvez utiliser vos outils préférés automation pour y parvenir, mais cet article met l’accent sur une approche basée sur PowerShell pure.

Par défaut, le script de configuration crée des groupes de sécurité locaux sur l’ordinateur pour contrôler l’accès à chacun des rôles.
C’est approprié pour le groupe de travail et les ordinateurs joints à un domaine, mais si vous effectuez un déploiement dans un environnement de domaine uniquement que vous pouvez souhaiter directement associer un groupe de sécurité de domaine à chaque rôle.
Pour mettre à jour la configuration pour utiliser des groupes de sécurité de domaine, ouvrez **InstallJeaFeatures.ps1** et apportez les modifications suivantes :

1.  Supprimer les 3 **groupe** ressources à partir du fichier :
    1.  « Groupe MS-lecteurs-Group »
    2.  « Groupe MS-Hyper-V-administrateurs-Group »
    3.  « Groupe MS-administrateurs-Group »
2.  Supprimez les ressources de groupe 3 de la JeaEndpoint **DependsOn** propriété
    1.  « [Group] MS-lecteurs-Group »
    2.  « [Group] MS-Hyper-V-administrateurs-Group »
    3.  « [Group] MS-administrateurs-Group »
3.  Modifier les noms de groupe dans le JeaEndpoint **RoleDefinitions** propriété à vos groupes de sécurité souhaité. Par exemple, si vous avez un groupe de sécurité *CONTOSO\MyTrustedAdmins* qui doit avoir accès au rôle d’administrateurs de Windows Admin Center, modification `'$env:COMPUTERNAME\Windows Admin Center Administrators'` à `'CONTOSO\MyTrustedAdmins'`. Les trois chaînes que vous devez mettre à jour sont :
    1.  « $env : COMPUTERNAME\Windows Admin Center Administrators
    2.  ' $env : administrateurs Hyper-V COMPUTERNAME\Windows Admin Center
    3.  ' $env : lecteurs COMPUTERNAME\Windows Admin Center

> [!NOTE]
> Veillez à utiliser des groupes de sécurité unique pour chaque rôle. Configuration échoue si le même groupe de sécurité est affecté à plusieurs rôles.

Ensuite, à la fin de la **InstallJeaFeatures.ps1** , ajoutez les lignes suivantes de PowerShell vers le bas du script :

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

Enfin, vous pouvez copier le dossier contenant les modules, la ressource de DSC et la configuration à chaque nœud cible et exécutez le **InstallJeaFeature.ps1** script.
Pour ce faire à distance à partir de votre station de travail administration, vous pouvez exécuter les commandes suivantes :

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
