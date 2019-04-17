---
title: Configuration des autorisations et contrôle d’accès utilisateur
description: Découvrez comment configurer le contrôle d’accès utilisateur et les autorisations à l’aide d’Active Directory ou Azure AD (projet Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2019
ms.locfileid: "9152044"
---
# Configurer les autorisations et contrôle d’accès utilisateur

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Si vous n’avez pas déjà, vous familiariser avec les [options de contrôle d’accès utilisateur dans Windows Admin Center](../plan/user-access-options.md)

>[!NOTE]
> L’accès en fonction de groupe dans Windows Admin Center n’est pas pris en charge dans les environnements de groupe de travail ou entre des domaines non approuvés.

## Définitions de rôle de l’accès de passerelle

Il existe deux rôles pour l’accès au service de passerelle Windows Admin Center:

**Les utilisateurs de passerelle** peut se connecter au service de passerelle Windows Admin Center pour gérer les serveurs par le biais de cette passerelle, mais ils ne peuvent pas modifier les autorisations d’accès, ni le mécanisme d’authentification utilisée pour s’authentifier auprès de la passerelle.

**Les administrateurs de passerelle** peuvent configurer qui obtient l’accès, ainsi que la manière dont les utilisateurs s’authentifient à la passerelle. Seuls les administrateurs de passerelle peuvent afficher et configurer les paramètres d’accès dans Windows Admin Center. Les administrateurs locaux sur l’ordinateur passerelle sont toujours des administrateurs du service de passerelle Windows Admin Center.

> [!NOTE]
> Accéder à la passerelle n’implique pas accès aux serveurs gérés visible par la passerelle. Pour gérer un serveur cible, l’utilisateur qui se connecte doit utiliser les informations d’identification (soit par le biais de leurs informations d’identification pass-through Windows ou par le biais des informations d’identification fournies dans la session Windows Admin Center à l’aide de l’action de **gérer en tant que** ) qui ont un accès administratif à ce serveur cible.

## Active Directory ou des groupes d’ordinateurs locaux

Par défaut, Active Directory ou des groupes d’ordinateurs locaux sont utilisés pour contrôler l’accès de passerelle. Si vous disposez d’un domaine Active Directory, vous pouvez gérer les utilisateurs de passerelle et les administrateurs accéder à partir de l’interface Windows Admin Center.

Sous l’onglet **utilisateurs** , vous pouvez contrôler qui peut accéder à Windows Admin Center sous la forme d’un utilisateur de passerelle. Par défaut, et si vous ne spécifiez un groupe de sécurité, tout utilisateur qui accède à l’URL de la passerelle a accès. Une fois que vous ajoutez un ou plusieurs groupes de sécurité à la liste des utilisateurs, l’accès est limité aux membres de ces groupes.

Si vous n’utilisez pas un domaine Active Directory dans votre environnement, l’accès est contrôlé par le ```Users``` et ```Administrators``` groupes locaux sur l’ordinateur passerelle Windows Admin Center.

### Authentification par carte à puce

Vous pouvez appliquer **l’authentification par carte à puce** en spécifiant un groupe supplémentaires _requis_ pour les groupes de sécurité basée sur une carte à puce. Une fois que vous avez ajouté un groupe de sécurité basée sur une carte à puce, un utilisateur peut accéder uniquement le service Windows Admin Center s’ils sont un membre d’un groupe de sécurité et un groupe de carte à puce inclus dans la liste des utilisateurs.

Sous l’onglet **administrateurs** , vous pouvez contrôler qui peut accéder à Windows Admin Center en tant qu’un administrateur de passerelle. Groupe Administrateurs local sur l’ordinateur a toujours un accès administrateur complet et ne peut pas être supprimé de la liste. En ajoutant des groupes de sécurité, vous attribuez à des membres de ces privilèges de groupes pour modifier les paramètres de passerelle Windows Admin Center. La liste des administrateurs prend en charge l’authentification par carte à puce dans la même manière que la liste des utilisateurs: avec la condition AND pour un groupe de sécurité et un groupe de carte à puce.

## Azure Active Directory

Si votre organisation utilise Azure Active Directory (AD Azure), vous pouvez choisir d’ajouter une couche **supplémentaire** de sécurité pour Windows Admin Center en exigeant l’authentification Azure AD pour accéder à la passerelle. Afin d’accéder à Windows Admin Center, de l’utilisateur **compte Windows** doit également avoir accès au serveur de passerelle (même si l’authentification Azure AD est utilisée). Lorsque vous utilisez Azure AD, vous les gérez autorisations d’accès utilisateur et l’administrateur de Windows Admin Center à partir du portail Azure, plutôt qu’à partir de l’interface utilisateur de Windows Admin Center.

### L’accès au centre d’administration Windows lors de l’authentification Azure AD est activée

Selon le navigateur utilisé, certains utilisateurs l’accès à Windows Admin Center avec configuré l’authentification Azure AD bénéficient d’une supplémentaires invite **à partir du navigateur** dans lesquels ils doivent fournir leur Windows compte des informations d’identification pour l’ordinateur sur lequel Windows Admin Center est installé. Après avoir saisi ces informations, les utilisateurs reçoivent l’invite d’authentification Azure Active Directory supplémentaires, ce qui nécessite les informations d’identification d’un compte Azure qui a accès dans l’application Azure AD dans Azure.

> [!NOTE]
> Utilisateurs compte Windows disposant de **droits d’administrateur** sur l’ordinateur passerelle n’aura ne pas invité pour l’authentification Azure AD.

### Configuration de l’authentification Azure Active Directory pour Windows Admin Center Preview

Accédez à Windows Admin Center **paramètres** > **accès** et utilisez le bouton bascule pour activer «utiliser Azure Active Directory pour ajouter une couche de sécurité à la passerelle». Si vous n’avez pas inscrit la passerelle vers Azure, vous serez guidé pour ce faire à ce stade.

Par défaut, tous les membres du client Azure AD ont accès utilisateur au service de passerelle Windows Admin Center. Seuls les administrateurs locaux sur l’ordinateur passerelle ont un accès administrateur à la passerelle Windows Admin Center. Notez que les droits des administrateurs locaux sur l’ordinateur passerelle ne peut pas être limités, les administrateurs locaux peuvent faire quoi que ce soit, quel que soit le que Azure AD est utilisé pour l’authentification.

Si vous souhaitez que les utilisateurs donnent Azure AD spécifique ou utilisateur de la passerelle groupes ou un accès administrateur passerelle au service Windows Admin Center, vous devez effectuer les éléments suivants:

1.  Accédez à votre application Windows Admin Center Azure AD dans le portail Azure en utilisant le lien hypertexte fourni dans les paramètres d’accès. Notez que ce lien hypertexte est disponible uniquement lorsque l’authentification Azure Active Directory est activée. 
    -   Vous pouvez également trouver votre application dans le portail Azure en accédant à **Azure Active Directory** > **des applications d’entreprise** > **toutes les applications** et recherche **WindowsAdminCenter** (l’application Azure AD sera nommée WindowsAdminCenter-<gateway name>). Si vous n’obtenez aucun résultat de recherche, assurez-vous **Afficher** est défini sur **toutes les applications**, **état de l’application** est définie sur **n’importe quel** et cliquez sur Appliquer, puis relancez votre recherche. Une fois que vous avez trouvé l’application, accédez à **utilisateurs et groupes**
2.  Dans l’onglet Propriétés, définir **l’attribution utilisateur requis** sur Oui.
    Une fois que vous avez terminé, seuls les membres répertoriés dans l’onglet **utilisateurs et groupes** sera en mesure d’accéder à la passerelle Windows Admin Center.
3.  Dans les utilisateurs et groupes, sélectionnez **Ajouter un utilisateur**. Vous devez affecter un utilisateur de la passerelle ou le rôle d’administrateur de passerelle pour chaque utilisateur/groupe ajouté.

Une fois que vous activez l’authentification Azure Active Directory, le redémarrage de service de passerelle et que vous devez actualiser votre navigateur. Vous pouvez mettre à jour l’accès utilisateur pour l’application SME Azure AD dans le portail Azure à tout moment.

Les utilisateurs devront se connecter à l’aide de leur identité Azure Active Directory lorsqu’ils essaient d’accéder à l’URL de la passerelle Windows Admin Center. N’oubliez pas que les utilisateurs doivent également être membre des utilisateurs locaux sur le serveur de passerelle pour accéder au centre d’administration de Windows.

Les utilisateurs et les administrateurs peuvent afficher son compte connecté et également comme déconnexion de cette publicité Azure paramètres du compte à partir de l’onglet **compte** de Windows Admin Center.

### Configuration de l’authentification Azure Active Directory pour Windows Admin Center

[Pour configurer l’authentification Azure AD, vous devez d’abord inscrire votre passerelle avec Azure](azure-integration.md) (vous souhaitez de le faire qu’une seule fois pour votre passerelle Windows Admin Center). Cette étape crée une application Azure AD à partir duquel vous pouvez gérer les utilisateurs de passerelle et un accès administrateur passerelle.

Si vous souhaitez que les utilisateurs donnent Azure AD spécifique ou utilisateur de la passerelle groupes ou un accès administrateur passerelle au service Windows Admin Center, vous devez effectuer les éléments suivants:

1.  Accédez à votre application SME Azure AD dans le portail Azure. 
    -   Lorsque vous cliquez sur le **contrôle d’accès de modification** et sélectionnez ensuite **Azure Active Directory** dans les paramètres Windows l’accès au centre d’administration, vous pouvez utiliser le lien hypertexte figurant dans l’interface utilisateur pour accéder à votre application Azure AD dans le portail Azure. Ce lien hypertexte est également disponible dans les paramètres d’accès une fois que vous cliquez sur Enregistrer et que vous avez sélectionné Azure AD en tant que votre fournisseur d’identité contrôle l’accès.
    -   Vous pouvez également trouver votre application dans le portail Azure en accédant à **Azure Active Directory** > **des applications d’entreprise** > **toutes les applications** et recherche **SME** (l’application Azure AD sera nommée PME -<gateway>). Si vous n’obtenez aucun résultat de recherche, assurez-vous **Afficher** est défini sur **toutes les applications**, **état de l’application** est définie sur **n’importe quel** et cliquez sur Appliquer, puis relancez votre recherche. Une fois que vous avez trouvé l’application, accédez à **utilisateurs et groupes**
2.  Dans l’onglet Propriétés, définir **l’attribution utilisateur requis** sur Oui.
    Une fois que vous avez terminé, seuls les membres répertoriés dans l’onglet **utilisateurs et groupes** sera en mesure d’accéder à la passerelle Windows Admin Center.
3.  Dans les utilisateurs et groupes, sélectionnez **Ajouter un utilisateur**. Vous devez affecter un utilisateur de la passerelle ou le rôle d’administrateur de passerelle pour chaque utilisateur/groupe ajouté.

Une fois que vous enregistrez le contrôle d’accès Azure AD dans le volet de **contrôle d’accès de modification** , le redémarrage de service de passerelle et vous devez actualiser votre navigateur. Vous pouvez mettre à jour l’accès utilisateur pour l’application Windows Admin Center Azure AD dans le portail Azure à tout moment. 

Les utilisateurs devront se connecter à l’aide de leur identité Azure Active Directory lorsqu’ils essaient d’accéder à l’URL de la passerelle Windows Admin Center. N’oubliez pas que les utilisateurs doivent également être membre des utilisateurs locaux sur le serveur de passerelle pour accéder au centre d’administration de Windows. 

À l’aide de l’onglet **Azure** de paramètres généraux de Windows Admin Center, les utilisateurs et les administrateurs peuvent afficher leur compte actuellement connecté, ainsi que puis déconnectez-vous de ce compte Azure AD.

### Accès conditionnel et l’authentification multifacteur

Un des avantages de l’utilisation d’Azure AD comme une couche supplémentaire de sécurité pour contrôler l’accès à la passerelle Windows Admin Center est que vous pouvez exploiter puissantes fonctions de sécurité de Azure AD telles que l’accès conditionnel et l’authentification multifacteur. 

[En savoir plus sur la configuration de l’accès conditionnel à Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## Configurer l’authentification unique

**Authentification unique sur lorsqu’elles sont déployées en tant que Service sur Windows Server**

Lorsque vous installez Windows Admin Center sur Windows 10, il est prêt à être utilisé de l’authentification unique. Toutefois, si vous avez l’intention d’utiliser Windows Admin Center sur Windows Server, vous devez configurer une forme de délégation Kerberos dans votre environnement avant de pouvoir utiliser de l’authentification unique. La délégation configure l’ordinateur passerelle comme approuvé pour déléguer au nœud cible. 

Pour configurer [basée sur les ressources de la délégation contrainte](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1) dans votre environnement, exécutez les applets de commande PowerShell suivante. (Être conscient que cela nécessite un contrôleur de domaine exécutant Windows Server 2012 ou version ultérieure).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

Dans cet exemple, la passerelle Windows Admin Center est installée sur le serveur **WindowsAdminCenterGW**, et le nom du nœud cible est **ManagedNode**.

Pour supprimer cette relation, exécutez l’applet de commande suivante:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## Contrôle d'accès basé sur les rôles

Contrôle d’accès en fonction du rôle vous permet de fournir aux utilisateurs un accès limité à l’ordinateur au lieu de rendre les administrateurs locales complet.
[En savoir plus sur le contrôle d’accès en fonction du rôle et les rôles disponibles.](../plan/user-access-options.md#role-based-access-control)

Configuration de RBAC se compose de 2 étapes: l’activation de la prise en charge sur les ordinateurs cible et affectation d’utilisateurs aux rôles pertinentes.

> [!TIP]
> Vérifiez que vous disposez des privilèges d’administrateur local sur les ordinateurs où vous configurez la prise en charge pour le contrôle d’accès en fonction du rôle.

### Appliquer un contrôle d’accès basé sur un rôle à un ordinateur unique

Le modèle de déploiement ordinateur unique est idéal pour les environnements simples avec seulement quelques ordinateurs pour gérer.
Configuration d’un ordinateur avec prise en charge pour le contrôle d’accès en fonction du rôle se traduit par les modifications suivantes:
-   Les modules PowerShell avec des fonctions requises par Windows Admin Center seront installées sur votre lecteur système, sous `C:\Program Files\WindowsPowerShell\Modules`. Tous les modules démarre avec **Microsoft.Sme**
-   Configuration d’état souhaité s’exécute une configuration pour configurer un point de terminaison Just Enough Administration sur l’ordinateur, nommé **Microsoft.Sme.PowerShell**à usage unique. Ce point de terminaison définit les 3 rôles utilisés par Windows Admin Center et s’exécutera en tant qu’un administrateur local temporaire lorsque l’utilisateur se connecte à celui-ci.
-   3 nouveaux groupes locaux seront créés pour contrôler les utilisateurs qui sont un accès affectés pour les rôles:
    -   Administrateurs de Windows Admin Center
    -   Administrateurs Hyper-V de Windows Admin Center
    -   Lecteurs de Windows Admin Center

Pour activer la prise en charge pour le contrôle d’accès en fonction du rôle sur un ordinateur unique, procédez comme suit:

1.  Ouvrez Windows Admin Center et connectez-vous à l’ordinateur que vous souhaitez configurer avec le contrôle d’accès basé sur un rôle à l’aide d’un compte avec des privilèges d’administrateur local sur l’ordinateur cible.
2.  Dans l’outil de la **vue d’ensemble** , cliquez sur **paramètres** > **le contrôle d’accès en fonction du rôle**.
3.  Cliquez sur **Appliquer** en bas de la page pour activer la prise en charge pour le contrôle d’accès en fonction du rôle sur l’ordinateur cible. Le processus de candidature implique la copie des scripts PowerShell et l’appel d’une configuration (à l’aide de la Configuration d’état souhaité PowerShell) sur l’ordinateur cible. Il peut prendre jusqu'à 10 minutes pour s’exécuter et entraîne le redémarrage de WinRM. Ceci déconnectera temporairement les utilisateurs de Windows Admin Center, PowerShell et WMI.
4.  Actualisez la page pour vérifier l’état du contrôle d’accès en fonction du rôle. Lorsqu’elle est prête à être utilisée, le statut deviendra **appliqué**.

Une fois que la configuration est appliquée, vous pouvez affecter des utilisateurs aux rôles:

1.  Ouvrez l’outil **utilisateurs et groupes locaux** et accédez à l’onglet **groupes** .
2.  Sélectionnez le groupe de **Lecteurs de Windows Admin Center** .
3.  Dans le volet *d’informations* dans la partie inférieure, cliquez sur **Ajouter un utilisateur** et entrez le nom d’un utilisateur ou groupe de sécurité qui doit avoir un accès en lecture seule au serveur par le biais de Windows Admin Center. Les utilisateurs et groupes peuvent provenir de l’ordinateur local ou votre domaine Active Directory.
4.  Répétez les étapes 2 à 3 pour les groupes **Administrateurs Hyper-V de Windows Admin Center** et **Les administrateurs de Windows Admin Center** .

Vous pouvez également remplir ces groupes de façon cohérente sur votre domaine en configurant un objet de stratégie de groupe avec le [Paramètre de stratégie de groupes restreint](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### Appliquer un contrôle d’accès en fonction du rôle sur plusieurs ordinateurs

Dans un déploiement de grandes entreprises, vous pouvez utiliser vos outils d’automation existants pour transmettre information la fonctionnalité de contrôle d’accès basé sur un rôle à vos ordinateurs en téléchargeant le package de configuration à partir de la passerelle Windows Admin Center.
Le package de configuration est conçu pour être utilisé avec la Configuration d’état souhaité PowerShell, mais vous pouvez adapter pour fonctionner avec votre solution d’automatisation par défaut.

#### Télécharger la configuration de contrôle d’accès basé sur un rôle

Pour télécharger le package de configuration d’accès en fonction du rôle de contrôle, vous devez avoir accès à Windows Admin Center et une invite de commandes PowerShell.

Si vous utilisez la passerelle Windows Admin Center en mode service sur Windows Server, utilisez la commande suivante pour télécharger le package de configuration.
Veillez à mettre à jour l’adresse de passerelle avec celle qui convient pour votre environnement.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Si vous utilisez la passerelle Windows Admin Center sur votre ordinateur Windows 10, exécutez la commande suivante à la place:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Lorsque vous développez l’archive zip, vous verrez la structure de dossiers suivante:
- InstallJeaFeatures.ps1
- JustEnoughAdministration (répertoire)
- Modules (répertoire)
    - Microsoft.SME.\* (répertoires)
    - WindowsAdminCenter.Jea (répertoire)

Pour configurer la prise en charge pour le contrôle d’accès en fonction du rôle sur un nœud, vous devez effectuer les actions suivantes:
1.  Copiez les modules JustEnoughAdministration, Microsoft.SME.\* et WindowsAdminCenter.Jea dans le répertoire du module PowerShell sur l’ordinateur cible. En règle générale, il se trouve dans `C:\Program Files\WindowsPowerShell\Modules`.
2.  Fichier **InstallJeaFeature.ps1** de mise à jour pour correspondre à la configuration de votre choix pour le point de terminaison RBAC.
3.  Exécutez InstallJeaFeature.ps1 pour compiler la ressource DSC.
4.  Déployer votre configuration de DSC sur tous vos ordinateurs pour appliquer la configuration.

La section suivante explique comment effectuer cette opération à l’aide de la communication à distance PowerShell.

#### Déploiement sur plusieurs ordinateurs

Pour déployer la configuration que vous avez téléchargé sur plusieurs ordinateurs, vous aurez besoin pour mettre à jour le script **InstallJeaFeatures.ps1** afin d’inclure des groupes de sécurité appropriés pour votre environnement, copiez les fichiers dans chacun de vos ordinateurs et appeler le scripts de configuration.
Vous pouvez utiliser vos outils automation préféré pour effectuer cette opération, mais cet article allons nous concentrer sur une approche pure basé sur PowerShell.

Par défaut, le script de configuration crée des groupes de sécurité locale sur l’ordinateur pour contrôler l’accès à chacun des rôles.
Cela est approprié pour le groupe de travail et les machines jointes à domaine, mais si vous effectuez le déploiement dans un environnement de domaine que vous souhaiterez peut-être directement associer un groupe de sécurité du domaine à chaque rôle.
Pour mettre à jour la configuration pour utiliser des groupes de sécurité de domaine, ouvrez **InstallJeaFeatures.ps1** et apportez les modifications suivantes:

1.  Supprimer les ressources de **groupe** 3 à partir du fichier:
    1.  «Groupe MS-lecteurs-Group»
    2.  «Groupe MS-Hyper-V-administrateurs-Group»
    3.  «Groupe MS-administrateurs-Group»
2.  Supprimer les ressources de groupe 3 à partir de la propriété JeaEndpoint **DependsOn**
    1.  «[Groupe de travail] MS-lecteurs-Group»
    2.  «[Groupe de travail] MS-Hyper-V-administrateurs-Group»
    3.  «[Groupe de travail] MS-administrateurs-Group»
3.  Modifier les noms de groupe dans la propriété JeaEndpoint **RoleDefinitions** à vos groupes de sécurité de votre choix. Par exemple, si vous disposez d’un groupe de sécurité *CONTOSO\MyTrustedAdmins* qui doit être un accès affecté au rôle d’administrateurs de Windows Admin Center, modifiez `'$env:COMPUTERNAME\Windows Admin Center Administrators'` à `'CONTOSO\MyTrustedAdmins'`. Les trois chaînes que vous devez mettre à jour sont:
    1.  «$env: administrateurs COMPUTERNAME\Windows Admin Center
    2.  «$env: administrateurs COMPUTERNAME\Windows Admin Center Hyper-V
    3.  «$env: lecteurs COMPUTERNAME\Windows Admin Center

> [!NOTE]
> Veillez à utiliser des groupes de sécurité uniques pour chaque rôle. Configuration échouera si le même groupe de sécurité est affecté à plusieurs rôles.

Ensuite, à la fin du fichier **InstallJeaFeatures.ps1** , ajoutez les lignes suivantes de PowerShell vers le bas du script:

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

Enfin, vous pouvez copier le dossier contenant les modules, la ressource DSC et la configuration à chaque nœud cible et exécutez le script **InstallJeaFeature.ps1** .
Pour ce faire à distance à partir de votre station de travail administration, vous pouvez exécuter les commandes suivantes:

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
