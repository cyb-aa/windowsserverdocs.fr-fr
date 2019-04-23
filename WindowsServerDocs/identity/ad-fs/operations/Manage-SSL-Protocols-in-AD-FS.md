---
title: La gestion des protocoles SSL/TLS et des Suites de chiffrement pour AD FS
description: Forum aux questions pour AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e834c50965c3af569dbe3756d677ec4cb2372542
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883920"
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>La gestion des protocoles SSL/TLS et des Suites de chiffrement pour AD FS
La documentation suivante fournit des informations sur comment désactiver et activer certains protocoles TLS/SSL et suites sont utilisés par AD FS de chiffrement

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL, SChannel et les Suites de chiffrement dans AD FS

La sécurité TLS (Transport Layer) et la couche de Sockets sécurisée (SSL) sont des protocoles qui fournissent pour les communications sécurisées.  Active Directory Federation Services utilise ces protocoles pour les communications.  Aujourd'hui, plusieurs versions de ces protocoles existent.

Schannel est un fournisseur de service de sécurité (SSP) qui implémente les protocoles d’authentification standard Internet SSL, TLS et DTLS. L’interface SSPI (Security Support Provider Interface) est une API utilisée par les systèmes Windows pour exécuter des fonctions relatives à la sécurité, dont notamment l’authentification. L’interface SSPI fonctionne comme une interface commune à plusieurs fournisseurs SSP, y compris le fournisseur SSP Schannel.

Une suite de chiffrement est un ensemble d’algorithmes de chiffrement. L’implémentation de fournisseur SSP schannel des protocoles TLS/SSL utilisent des algorithmes à partir d’une suite de chiffrement pour créer des clés et chiffrer les informations. Une suite de chiffrement spécifie un algorithme pour chacune des tâches suivantes :

- Échange de clés
- Chiffrement en bloc
- Authentification de message

AD FS utilise le fichier Schannel.dll pour effectuer ses interactions de communications sécurisées.  AD FS prend en charge tous les protocoles et les suites de chiffrement qui sont pris en charge par le fichier Schannel.dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Gestion des protocoles TLS/SSL et Suites de chiffrement
> [!IMPORTANT]
> Cette section contient des étapes qui vous indiquent comment modifier le Registre. Toutefois, les problèmes graves peuvent survenir si vous modifiez le Registre de façon incorrecte. Par conséquent, assurez-vous que vous suivez ces étapes attentivement. 
> 
> N’oubliez pas que la modification des paramètres de sécurité par défaut pour SCHANNEL pourrait arrêter ou empêcher les communications entre certains clients et serveurs.  Cela se produit si une communication sécurisée est nécessaire et ils n’ont pas un protocole de négociation de communications avec.
> 
> Si vous appliquez ces modifications, ils doivent être appliquées à tous vos serveurs AD FS dans votre batterie de serveurs.  Après avoir appliqué ces modifications, un redémarrage est nécessaire.

Dans la journée d’aujourd'hui et âge, renforcement de vos serveurs et la suppression plus anciennes ou suites de chiffrement faible devient une priorité majeure pour de nombreuses organisations.  Les suites logicielles sont disponibles qui hébergera vos serveurs de test et des informations détaillées sur ces suites et protocoles.  Pour rester conformes ou obtenir des évaluations sécurisées, suppression ou la désactivation des protocoles plus faibles ou les suites de chiffrement est devenu indispensable.  Le reste de ce document fournit des conseils sur la façon d’activer ou désactiver certains protocoles et suites de chiffrement.

Les clés de Registre ci-dessous se trouvent dans le même emplacement :  HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Utilisez regedit ou PowerShell pour activer ou désactiver ces protocoles et suites de chiffrement.

![Emplacement du Registre](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Activer et désactiver le protocole SSL 2.0
Pour activer et désactiver SSL 2.0, utilisez les clés de Registre suivantes et leurs valeurs.

### <a name="enable-ssl-20"></a>Activer le protocole SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] « Activé » = DWORD : 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault"=dword:00000000 

### <a name="disable-ssl-20"></a>Désactiver le protocole SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SL 2.0\Server] « Activé » = DWORD : 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] "DisabledByDefault"=dword:00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] « Activé » = DWORD : 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] "DisabledByDefault"=dword:00000001

### <a name="using-powershell-to-disable-ssl-20"></a>À l’aide de PowerShell pour désactiver SSL 2.0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>Activer et désactiver le protocole SSL 3.0
Utiliser des clés de Registre suivantes et leurs valeurs pour activer et désactiver SSL 3.0.

### <a name="enable-ssl-30"></a>Activer SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault"=dword:00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] « Activé » = DWORD : 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault"=dword:00000000 

### <a name="disable-ssl-30"></a>Désactiver le protocole SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] « Activé » = DWORD : 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] "DisabledByDefault"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] « Activé » = DWORD : 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] "DisabledByDefault"=dword:00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>À l’aide de PowerShell pour désactiver SSL 3.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>Activer et désactiver TLS 1.0
Pour activer et désactiver le protocole TLS 1.0, utilisez les clés de Registre suivantes et leurs valeurs.

> [!IMPORTANT]
> La désactivation de TLS 1.0, le proxy d’application Web s’arrête au niveau de confiance AD FS.  Si vous désactivez le protocole TLS 1.0, vous devez activer une authentification forte pour vos applications.  Consultez [activer l’authentification renforcée](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Activer TLS 1.0
- 1. [0\Server HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « Activé » = DWORD : 00000001
- 1. [0\Server HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « DisabledByDefault » = DWORD : 00000000 
- [1. 0\Client HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « Activé » = DWORD : 00000001
- [1. 0\Client HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « DisabledByDefault » = DWORD : 00000000 

### <a name="disable-tls-10"></a>Désactivation de TLS 1.0
- 1. [0\Server HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « Activé » = DWORD : 00000000
- 1. [0\Server HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « DisabledByDefault » = DWORD : 00000001
- [1. 0\Client HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « Activé » = DWORD : 00000000
- [1. 0\Client HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « DisabledByDefault » = DWORD : 00000001 

### <a name="using-powershell-to-disable-tls-10"></a>À l’aide de PowerShell pour désactiver TLS 1.0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>Activer et désactiver TLS 1.1
Utiliser des clés de Registre suivantes et leurs valeurs pour activer et désactiver TLS 1.1.

### <a name="enable-tls-11"></a>Activer TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] « Activé » = DWORD : 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] « DisabledByDefault » = DWORD : 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] « Activé » = DWORD : 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] « DisabledByDefault » = DWORD : 00000000

### <a name="disable-tls-11"></a>Désactivation de TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] « Activé » = DWORD : 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] « DisabledByDefault » = DWORD : 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] « Activé » = DWORD : 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] « DisabledByDefault » = DWORD : 00000001 

### <a name="using-powershell-to-disable-tls-11"></a>À l’aide de PowerShell pour désactiver TLS 1.1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>Activer et désactiver TLS 1.2

Utiliser des clés de Registre suivantes et leurs valeurs pour activer et désactiver TLS 1.2.

### <a name="enable-tls-12"></a>Activer TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS currentcontrolset\control\securityproviders\schannel\protocols\tls 1. 2\server] « Activé » = DWORD : 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS currentcontrolset\control\securityproviders\schannel\protocols\tls 1. 2\server] « DisabledByDefault » = DWORD : 00000000 
- [1. 2\client HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « Activé » = DWORD : 00000001
- [1. 2\client HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « DisabledByDefault » = DWORD : 00000000

### <a name="disable-tls-12"></a>Désactivation de TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS currentcontrolset\control\securityproviders\schannel\protocols\tls 1. 2\server] « Activé » = DWORD : 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS currentcontrolset\control\securityproviders\schannel\protocols\tls 1. 2\server] « DisabledByDefault » = DWORD : 00000001
- [1. 2\client HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « Activé » = DWORD : 00000000
- [1. 2\client HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS] « DisabledByDefault » = DWORD : 00000001

### <a name="using-powershell-to-disable-tls-12"></a>À l’aide de PowerShell pour désactiver TLS 1.2

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="enable-and-disable-rc4"></a>Activer et désactiver RC4 

Pour activer et désactiver RC4, utilisez les clés de Registre suivantes et leurs valeurs.  Les clés de Registre de cette suite de chiffrement se trouvent ici :

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Emplacement du Registre](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Activer RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled"=dword:00000001 

### <a name="disable-rc4"></a>Désactiver RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled"=dword:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled"=dword:00000000 

### <a name="using-powershell"></a>À l'aide de PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Activation ou désactivation des suites de chiffrement supplémentaires

Vous pouvez désactiver certains chiffrements spécifiques en les supprimant de HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\00010002 

![Emplacement du Registre](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Pour activer une suite de chiffrement, ajoutez sa valeur de chaîne à la clé de la valeur de chaînes multiples fonctions.  Par exemple, si nous voulons permettre TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521 nous serait ajoutez-la à la chaîne.

Pour obtenir une liste complète de chiffrement prises en charge les suites consultez [Suites de chiffrement dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx).  Ce document fournit un tableau des suites sont activées par défaut et celles qui sont pris en charge mais n’est pas activée par défaut.  Pour classer par priorité les suites de chiffrement consultez [hiérarchisation des Suites de chiffrement Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>L’activation d’une authentification forte pour les applications .NET
Les .NET Framework 3.5/4.0/4.5.x applications peuvent passer le protocole par défaut TLS 1.2 l’activation de la clé de Registre SchUseStrongCrypto.  Cette clé de Registre forcera les applications .NET pour utiliser TLS 1.2.

> [!IMPORTANT]
> Pour AD FS sur Windows Server 2016 et Windows Server 2012 R2, vous devez utiliser la clé/4.5.x 4.0 de .NET Framework :  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319


Pour le .NET Framework 3.5, utilisez la clé de Registre suivante :

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\\.NETFramework\v2.0.50727] "SchUseStrongCrypto"=dword:00000001

Pour le .NET Framework 4.0/4.5.x utiliser la clé de Registre suivante : HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework\v4.0.30319 "SchUseStrongCrypto"=dword:00000001

![Authentification renforcée](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Informations supplémentaires

- [Suites de chiffrement dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)
- [Suites de chiffrement TLS dans Windows 8.1](https://msdn.microsoft.com/en-us/library/windows/desktop/mt767781.aspx)
- [Hiérarchisation des Suites de chiffrement de Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)
- [En parlant de chiffrements et autres fonctions énigmatiques pour les langues](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
