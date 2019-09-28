---
title: Déléguer l’accès à l’applet de AD FS PowerShell à des utilisateurs non administrateurs
description: Ce document descirbes Comment déléguer des autorisations à des non-administrateurs pour AD FS PowerShell applets.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 334bb96c77b0bc1e76a54ed1e0871f53753ded87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357754"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>Déléguer l’accès à l’applet de AD FS PowerShell à des utilisateurs non administrateurs 
Par défaut, l’administration de AD FS via PowerShell ne peut être effectuée que par les administrateurs de AD FS. Pour de nombreuses organisations de grande taille, cela peut ne pas être un modèle opérationnel viable lorsque vous traitez avec d’autres personnes, telles que le personnel du support technique.  

Avec juste assez d’administration (JEA), les clients peuvent maintenant déléguer des applets spécifiques à différents groupes de personnel.  
Un bon exemple de ce cas d’utilisation est d’autoriser le personnel du support technique à interroger AD FS état de verrouillage de compte et à réinitialiser l’état de verrouillage de compte dans AD FS une fois qu’un utilisateur a été approuvé. Dans ce cas, le applets qui doit être délégué est le suivant : 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

Nous utilisons cet exemple dans le reste de ce document. Toutefois, vous pouvez personnaliser cela pour permettre également à la délégation de définir les propriétés des parties de confiance et de les distribuer aux propriétaires d’applications au sein de l’organisation.  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>Créer les groupes requis nécessaires pour accorder des autorisations aux utilisateurs 
1. Créez un [compte de service administré de groupe](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). Le compte gMSA permet à l’utilisateur JEA d’accéder aux ressources réseau en tant qu’autres ordinateurs ou services Web. Il fournit une identité de domaine qui peut être utilisée pour l’authentification auprès de ressources sur n’importe quel ordinateur au sein du domaine. Le compte gMSA dispose des droits d’administration nécessaires ultérieurement dans le programme d’installation. Pour cet exemple, nous appelons le compte **gMSAContoso**. 
2. Créer un groupe de Active Directory peut être rempli avec les utilisateurs qui doivent disposer des droits sur les commandes déléguées. Dans cet exemple, le personnel du support technique dispose des autorisations de lecture, de mise à jour et de réinitialisation de l’état de verrouillage ADFS. Nous faisons référence à ce groupe dans l’exemple en tant que **JEAContoso**. 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>Installez le compte gMSA sur le serveur ADFS : 
Créez un compte de service qui dispose de droits d’administration sur les serveurs ADFS. Cela peut être effectué sur le contrôleur de domaine ou à distance tant que le package des outils d’installation à distance Active Directory est installé.  Le compte de service doit être créé dans la même forêt que le serveur ADFS. Modifiez les exemples de valeurs de la configuration de votre batterie de serveurs. 

```powershell
 # This command should only be run if this is the first time gMSA accounts are enabled in the forest 
Add-KdsRootKey -EffectiveTime ((get-date).addhours(-10))  
 
# Run this on every node that you want to have JEA configured on  
$adfsServer = Get-ADComputer server01.contoso.com  
 
# Run targeted at domain controller  
$serviceaccount = New-ADServiceAccount gMSAcontoso -DNSHostName <FQDN of the domain containing the KDS key> - PrincipalsAllowedToRetrieveManagedPassword $adfsServer –passthru 
 
# Run this on every node 
Add-ADComputerServiceAccount -Identity server01.contoso.com -ServiceAccount $ServiceAccount 
```

Installez le compte gMSA sur le serveur ADFS.  Cela doit être exécuté sur chaque nœud ADFS de la batterie de serveurs. 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>Accorder des droits d’administrateur de compte gMSA 
Si la batterie de serveurs utilise l’administration déléguée, accordez les droits d’administrateur de compte gMSA en l’ajoutant au groupe existant disposant d’un accès administrateur délégué.  
 
Si la batterie de serveurs n’utilise pas l’administration déléguée, accordez les droits d’administrateur de compte gMSA en faisant du groupe d’administration local sur tous les serveurs ADFS. 
 
 
### <a name="create-the-jea-role-file"></a>Créer le fichier de rôle JEA 
 
Sur le serveur AD FS, créez le rôle JEA dans un fichier bloc-notes. Les instructions pour créer le rôle sont fournies sur les [fonctionnalités de rôle Jea](https://docs.microsoft.com/powershell/jea/role-capabilities). 
 
Le applets délégué dans cet exemple est `Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`. 

Exemple de rôle JEA déléguer l’accès des applets « Reset-ADFSAccountLockout », « ADFSAccountActivity » et « Set-ADFSAccountActivity » :

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>Créer le fichier de configuration de session JEA 
Suivez les instructions pour créer le fichier de [configuration de session Jea](https://docs.microsoft.com/powershell/jea/session-configurations) . Le fichier de configuration détermine qui peut utiliser le point de terminaison JEA et les fonctionnalités auxquelles il a accès. 

Les capacités de rôle sont référencées par le nom plat (nom de fichier sans l’extension) du fichier de capacités de rôle. Si plusieurs capacités de rôle sont disponibles sur le système avec le même nom plat, PowerShell utilise son ordre de recherche implicite pour sélectionner le fichier de capacité de rôle effective. Elle n’autorise pas l’accès à tous les fichiers de capacités de rôle portant le même nom. 

Pour spécifier un fichier de capacité de rôle avec un chemin d' `RoleCapabilityFiles` accès, utilisez l’argument. Pour un sous-dossier, Jea recherche les modules PowerShell valides qui `RoleCapabilities` contiennent un sous-dossier `RoleCapabilityFiles` , où l’argument doit être `RoleCapabilities`modifié comme étant. 

Exemple de fichier de configuration de session : 

```powershell
@{
SchemaVersion = '2.0.0.0'
GUID = '54c8d41b-6425-46ac-a2eb-8c0184d9c6e6'
SessionType = 'RestrictedRemoteServer'
GroupManagedServiceAccount =  'CONTOSO\gMSAcontoso'
RoleDefinitions = @{ JEAcontoso = @{ RoleCapabilityFiles = 'C:\Program Files\WindowsPowershell\Modules\AccountActivityJEA\RoleCapabilities\JEAAccountActivityResetRole.psrc' } }
}
```

Enregistrez le fichier de configuration de session. 
 
Il est fortement recommandé de [tester votre fichier de configuration de session](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1) si vous avez modifié manuellement le fichier PSSC à l’aide d’un éditeur de texte pour vous assurer que la syntaxe est correcte. Si un fichier de configuration de session ne réussit pas ce test, il n’est pas correctement inscrit sur le système.  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>Installer la configuration de session JEA sur le serveur AD FS 

Installer la configuration de session JEA sur le serveur AD FS 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>Instructions opérationnelles 
Une fois configuré, la journalisation et l’audit JEA peuvent être utilisés pour déterminer si les utilisateurs appropriés ont accès au point de terminaison JEA. 

Pour utiliser les commandes déléguées : 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 


```
