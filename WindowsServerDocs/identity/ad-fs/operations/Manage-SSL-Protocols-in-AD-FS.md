---
title: La gestion des Suites de chiffrement et les protocoles SSL/TLS pour AD FS
description: Forum aux questions pour ADFS2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 23055a1b727e4f1a6b1ccafdea9a410c6021c432
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2018
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>La gestion des Suites de chiffrement et les protocoles SSL/TLS pour AD FS
La documentation suivante fournit des informations sur comment désactiver et activer certains protocoles TLS/SSL et qui sont utilisés par AD FS suites de chiffrement

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL, SChannel et Suites de chiffrement dans AD FS

La sécurité TLS (Transport Layer) et la couche SSL (Secure Sockets) sont des protocoles qui fournissent des communications sécurisées.  Active Directory Federation Services utilise ces protocoles pour les communications.  Aujourd'hui, plusieurs versions de ces protocoles existent.

Schannel est un fournisseur SSP (Security Support Provider) qui implémente les protocoles d’authentification standard SSL, TLS et DTLS Internet. Le fournisseur d’Interface SSPI (Security Support) est une API utilisée par les systèmes Windows pour exécuter des fonctions liées à la sécurité, y compris l’authentification. L’interface SSPI fonctionne comme une interface commune à plusieurs fournisseurs de prise en charge de sécurité (SSP), y compris le SSP. Schannel

Une suite de chiffrement est un ensemble d’algorithmes de chiffrement. L’implémentation de fournisseur SSP schannel des protocoles TLS/SSL utilisez des algorithmes à partir d’une suite de chiffrement pour créer des clés et chiffrer les informations. Une suite de chiffrement spécifie un algorithme pour chacune des tâches suivantes:

- Échange de clés
- Chiffrement en bloc
- Authentification des messages

AD FS utilise le fichier Schannel.dll pour effectuer ses interactions des communications sécurisées.  AD FS prend en charge tous les protocoles et les suites de chiffrement qui sont pris en charge par le fichier Schannel.dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Gestion des protocoles TLS/SSL et des Suites de chiffrement
> [!IMPORTANT]
> Cette section contient les étapes qui vous indiquent comment modifier le Registre. Toutefois, des problèmes graves peuvent se produire si vous modifiez le Registre de façon incorrecte. Par conséquent, assurez-vous que vous procédez avec précaution. 
> 
> N’oubliez pas que la modification des paramètres de sécurité par défaut de SCHANNEL peut interrompre ou empêcher les communications entre certains clients et serveurs.  Cela se produira si la communication sécurisée est requise, et ils n’ont pas d’un protocole de négociation de communications avec.
> 
> Si vous appliquez ces modifications, ils doivent être appliquées à tous les serveurs de votre batterie AD FS.  Après l’application de ces modifications, un redémarrage est nécessaire.

Dans jour aujourd'hui et âge, renforcement de vos serveurs et la suppression plus anciens ou les suites de chiffrement faible est devenu une priorité majeure pour de nombreuses organisations.  Les suites logicielles sont disponibles qui sera tester vos serveurs et fournissent des informations détaillées sur ces suites et protocoles.  Pour pouvoir restent conformes ou obtenir des évaluations sécurisées, suppression ou la désactivation des protocoles plus faibles ou les suites de chiffrement est devenu indispensable.  Le reste de ce document fournissent des instructions sur la façon d’activer ou désactiver certains protocoles et suites de chiffrement.

Les clés de Registre ci-dessous sont trouvent dans le même emplacement: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Utiliser regedit ou PowerShell pour activer ou désactiver ces protocoles et suites de chiffrement.

![Emplacement du Registre](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Activer et désactiver SSL 2.0
Pour activer et désactiver SSL 2.0, utilisez les clés de Registre suivantes et leurs valeurs.

### <a name="enable-ssl-20"></a>Activer le protocole SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] «DisabledByDefault» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] «DisabledByDefault» = DWORD: 00000000 

### <a name="disable-ssl-20"></a>Désactiver SSL 2.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SL 2.0\Server] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server] «DisabledByDefault» = DWORD: 00000001 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client] «DisabledByDefault» = DWORD: 00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Pour désactiver SSL 2.0 à l’aide de PowerShell

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>Activer et désactiver SSL 3.0
Pour activer et désactiver SSL 3.0, utilisez les clés de Registre suivantes et leurs valeurs.

### <a name="enable-ssl-30"></a>Activer SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] «DisabledByDefault» = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] «DisabledByDefault» = DWORD: 00000000 

### <a name="disable-ssl-30"></a>Désactiver SSL 3.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server] «DisabledByDefault» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client] «DisabledByDefault» = DWORD: 00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Pour désactiver SSL 3.0 à l’aide de PowerShell

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
> La désactivation de TLS 1.0 bloque le WAP pour la confiance AD FS.  Si vous désactivez TLS 1.0, vous devez activer une authentification forte pour vos applications.  Voir [activer l’authentification renforcée](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Activer le protocole TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] «DisabledByDefault» = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] «DisabledByDefault» = DWORD: 00000000 

### <a name="disable-tls-10"></a>Désactiver le protocole TLS 1.0
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server] «DisabledByDefault» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client] «DisabledByDefault» = DWORD: 00000001 

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
Pour activer et désactiver TLS 1.1, utilisez les clés de Registre suivantes et leurs valeurs.

### <a name="enable-tls-11"></a>Activer le protocole TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] «DisabledByDefault» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] «DisabledByDefault» = DWORD: 00000000

### <a name="disable-tls-11"></a>Désactiver TLS 1.1
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server] «DisabledByDefault» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client] «DisabledByDefault» = DWORD: 00000001 

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

Pour activer et désactiver TLS 1.2, utilisez les clés de Registre suivantes et leurs valeurs.

### <a name="enable-tls-12"></a>Activer le protocole TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] «DisabledByDefault» = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] «DisabledByDefault» = DWORD: 00000000

### <a name="disable-tls-12"></a>Désactiver TLS 1.2
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] «DisabledByDefault» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] «DisabledByDefault» = DWORD: 00000001

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

Pour activer et désactiver RC4, utilisez les clés de Registre suivantes et leurs valeurs.  Clés de Registre de cette suite de chiffrement sont trouvent ici:

- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Emplacement du Registre](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Activer RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] «Activé» = DWORD: 00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] «Activé» = DWORD: 00000001 

### <a name="disable-rc4"></a>Désactiver RC4

- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] «Activé» = DWORD: 00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] «Activé» = DWORD: 00000000 

### <a name="using-powershell"></a>À l’aide de PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Activation ou désactivation des suites de chiffrement supplémentaires

Vous pouvez désactiver certaines chiffrements spécifiques en les supprimant de HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\0010002 

![Emplacement du Registre](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Pour activer une suite de chiffrement, à sa valeur de chaîne à la clé de la valeur de chaînes multiples fonctions.  Par exemple, si nous voulons permettre TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521 puis nous serait l’ajouter à la chaîne.

Pour obtenir la liste complète de chiffrement prise en charge les suites de voir [Suites de chiffrement de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx).  Ce document fournit une table des suites sont activés par défaut et ceux qui sont pris en charge mais pas activé par défaut.  Pour classer par priorité les suites de chiffrement voir [hiérarchisation des Suites de chiffrement Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>L’activation de l’authentification stricte pour les applications .NET
Les applications 3.5/4.0/4.5.x de .NET Framework peuvent basculer le protocole par défaut pour TLS 1.2 par l’activation de la clé de Registre SchUseStrongCrypto.  Cette clé de Registre force les applications .NET à utiliser le protocole TLS 1.2.

> [!IMPORTANT]
> Pour AD FS dans Windows Server 2016 et Windows Server 2012 R2, vous devez utiliser la clé de 4.0/4.5.x .NET Framework: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\. NETFramework\v4.0.30319


Pour le .NET Framework 3.5, utilisez la clé de Registre suivante:

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\. NETFramework\v2.0.50727] «SchUseStrongCrypto» = DWORD: 00000001

Pour le .NET Framework 4.0/4.5.x utiliser la clé de Registre suivante: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\. NETFramework\v4.0.30319 «SchUseStrongCrypto» = DWORD: 00000001

![Authentification forte](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Informations supplémentaires

- [Suites de chiffrement de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa374757.aspx)
- [Suites de chiffrement TLS dans Windows 8.1](https://msdn.microsoft.com/en-us/library/windows/desktop/mt767781.aspx)
- [Définition des priorités des Suites de chiffrement Schannel](https://msdn.microsoft.com/en-us/library/windows/desktop/bb870930.aspx)
- [Parler dans les chiffrements et les autres langues Enigmatic](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
