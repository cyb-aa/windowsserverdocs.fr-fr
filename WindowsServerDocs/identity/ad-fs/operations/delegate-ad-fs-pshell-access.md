---
title: Déléguer l’accès d’applet de commande Powershell AD FS pour les utilisateurs Non administrateurs
description: Ce document décrit comment déléguer des autorisations aux utilisateurs non administrateurs pour les applets de commande PowerShell AD FS.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: zhvolosh
ms.date: 01/31/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 265d22b045011e34192e56bdb970955b601cda56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883560"
---
# <a name="delegate-ad-fs-powershell-commandlet-access-to-non-admin-users"></a>Déléguer l’accès d’applet de commande Powershell AD FS pour les utilisateurs Non administrateurs 
Par défaut, administration d’AD FS via PowerShell possible uniquement par les administrateurs AD FS. Pour nombreuses grandes organisations, cela peut-être pas un modèle opérationnel viable lorsque vous traitez des autres acteurs comme un personnel d’assistance.  

Avec Just Enough Administration (JEA), les clients peuvent déléguer maintenant des applets de commande spécifiques à des groupes de personnes différentes.  
Un bon exemple de ce cas d’usage est ce qui permet au support technique de requête AD FS état de verrouillage du compte et réinitialiser l’état de verrouillage de compte dans AD FS une fois qu’un utilisateur a été approuvée dans le programme. Dans ce cas, les applets de commande qui devraient être déléguées sont : 
- `Get-ADFSAccountActivity`
- `Set-ADFSAccountActivity` 
- `Reset-ADFSAccountLockout` 

Nous utilisons cet exemple dans le reste de ce document. Toutefois, vous pouvez personnaliser cette option pour autoriser également la délégation définir les propriétés des parties de confiance et de transmettre cette fonction aux propriétaires d’applications au sein de l’organisation.  


##  <a name="create-the-required-groups-necessary-to-grant-users-permissions"></a>Créer les groupes requis nécessaires pour accorder des autorisations d’utilisateurs 
1. Créer un [compte de Service administré de groupe](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview). Le compte de service administré de groupe est utilisé pour autoriser l’utilisateur JEA accéder aux ressources réseau en tant que les autres ordinateurs ou les services web. Il fournit une identité de domaine qui peut être utilisée pour s’authentifier auprès des ressources sur n’importe quel ordinateur dans le domaine. Le compte de service administré de groupe est accordé des droits d’administration nécessaires plus loin dans le programme d’installation. Pour cet exemple, nous appelons le compte **gMSAContoso**. 
2. Créer un annuaire Active Directory groupe peut être rempli avec les utilisateurs qui doivent bénéficier des droits pour les commandes déléguées. Dans cet exemple, personnel du support technique est accordé les autorisations pour lire, mettre à jour et réinitialiser l’état de verrouillage AD FS. Nous faisons référence à ce groupe dans l’ensemble de l’exemple en tant que **JEAContoso**. 

### <a name="install-the-gmsa-account-on-the-adfs-server"></a>Installer le compte de service administré de groupe sur le serveur ADFS : 
Créer un compte de service qui possède des droits administratifs sur les serveurs ADFS. Cela peut être effectuée sur le contrôleur de domaine ou à distance tant que le package RSAT d’Active Directory est installé.  Le compte de service doit être créé dans la même forêt que le serveur ADFS. Modifiez les valeurs d’exemple à la configuration de votre batterie de serveurs. 

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

Installer le compte de service administré de groupe sur le serveur ADFS.  Cette opération doit être exécuté sur chaque nœud AD FS dans la batterie de serveurs. 
 
```powershell
Install-ADServiceAccount gMSAcontoso 
```

### <a name="grant-the-gmsa-account-admin-rights"></a>Accorder les droits d’administrateur de compte de service administré de groupe 
Si la batterie de serveurs à l’aide de l’administration déléguée, accorder des droits d’administrateur de compte gMSA en l’ajoutant au groupe existant qui a délégué l’accès administrateur.  
 
Si la batterie de serveurs n’utilise pas l’administration déléguée, accorder des droits d’administrateur de compte gMSA en rendant le groupe d’administration locale sur tous les serveurs ADFS. 
 
 
### <a name="create-the-jea-role-file"></a>Créer le fichier de rôle JEA 
 
Créer le rôle JEA dans un fichier du bloc-notes. Instructions pour créer le rôle est fourni sur [les capacités de rôle JEA](https://docs.microsoft.com/powershell/jea/role-capabilities). 
 
Les applets de commande déléguées dans cet exemple sont `Reset-AdfsAccountLockout, Get-ADFSAccountActivity, and Set-ADFSAccountActivity`. 

Rôle JEA exemple délégation de l’accès des applets de commande « Réinitialisation ADFSAccountLockout », 'Get-ADFSAccountActivity' et 'Set-ADFSAccountActivity' :

```powershell
@{
GUID = 'b35d3985-9063-4de5-81f8-241be1f56b52'
ModulesToImport = 'adfs'
VisibleCmdlets = 'Reset-AdfsAccountLockout', 'Get-ADFSAccountActivity', 'Set-ADFSAccountActivity'
}
```


### <a name="create-the-jea-session-configuration-file"></a>Créer le fichier de Configuration de Session JEA 
Suivez les instructions pour créer le [configuration de session JEA](https://docs.microsoft.com/powershell/jea/session-configurations) fichier. Le fichier de configuration détermine qui peut utiliser le point de terminaison JEA et quelles fonctionnalités ils ont accès. 

Capacités de rôle sont référencées par le nom plat (nom de fichier sans l’extension) du fichier de capacités de rôle. Si plusieurs capacités de rôle sont disponibles sur le système portant le même nom plat, PowerShell utilise son ordre de recherche implicite pour sélectionner le fichier de capacités de rôle effectif. Il ne donne pas accès à tous les fichiers de capacités de rôle portant le même nom. 

Pour spécifier un fichier de capacités de rôle avec un chemin d’accès, utilisez le `RoleCapabilityFiles` argument. Pour un sous-dossier, JEA recherche les modules Powershell valides contenant un `RoleCapabilities` sous-dossier, où le `RoleCapabilityFiles` argument doit être modifié pour être `RoleCapabilities`. 

Fichier de configuration de session exemple : 

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
 
Il est fortement recommandé de [tester votre fichier de configuration de session](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Test-PSSessionConfigurationFile?view=powershell-5.1) si vous avez modifié le fichier pssc manuellement à l’aide d’un éditeur de texte pour vous assurer de la syntaxe est correcte. Si un fichier de configuration de session ne passe pas ce test, il n’est pas correctement inscrit sur le système.  
 
### <a name="install-the-jea-session-configuration-on-the-ad-fs-server"></a>Installer la configuration de session JEA sur le serveur AD FS 

Installer la configuration de session JEA sur le serveur AD FS 
 
```powershell
Register-PSSessionConfiguration -Path .\JEASessionConfig.pssc -name "AccountActivityAdministration" -force
``` 
## <a name="operational-instructions"></a>Instructions opérationnelles 
Une fois configurées, JEA journalisation et audit peut être utilisé pour déterminer si les utilisateurs corrects ont accès au point de terminaison JEA. 

Pour utiliser les commandes déléguées : 

```powershell
Enter-pssession -ComputerName server01.contoso.com -ConfigurationName "AccountActivityAdministration" -Credential <User Using JEA> 
Get-AdfsAccountActivity <User> 
```
