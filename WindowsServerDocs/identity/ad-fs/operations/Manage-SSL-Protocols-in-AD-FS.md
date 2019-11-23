---
title: Gestion des protocoles SSL/TLS et des suites de chiffrement pour AD FS
description: Forum aux questions (FAQ) pour AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 44fb4c02421a431edb502daecaa38f00fb4dd2ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407538"
---
# <a name="managing-ssltls-protocols-and-cipher-suites-for-ad-fs"></a>Gestion des protocoles SSL/TLS et des suites de chiffrement pour AD FS
La documentation suivante fournit des informations sur la façon de désactiver et d’activer certains protocoles TLS/SSL et les suites de chiffrement utilisées par AD FS

## <a name="tlsssl-schannel-and-cipher-suites-in-ad-fs"></a>TLS/SSL, SChannel et les suites de chiffrement dans AD FS

Le protocole TLS (Transport Layer Security) et le protocole SSL (SSL) sont des protocoles qui fournissent des communications sécurisées.  Services ADFS utilise ces protocoles pour les communications.  Aujourd’hui, il existe plusieurs versions de ces protocoles.

Schannel est un fournisseur de service de sécurité (SSP) qui implémente les protocoles d’authentification standard Internet SSL, TLS et DTLS. L’interface SSPI (Security Support Provider Interface) est une API utilisée par les systèmes Windows pour exécuter des fonctions relatives à la sécurité, dont notamment l’authentification. L’interface SSPI fonctionne comme une interface commune à plusieurs fournisseurs SSP, y compris le fournisseur SSP Schannel.

Une suite de chiffrement est un ensemble d’algorithmes de chiffrement. L’implémentation du SSP Schannel des protocoles TLS/SSL utilise des algorithmes d’une suite de chiffrement pour créer des clés et chiffrer les informations. Une suite de chiffrement spécifie un algorithme pour chacune des tâches suivantes :

- Échange de clés
- Chiffrement en bloc
- Authentification de message

AD FS utilise Schannel. dll pour effectuer ses interactions de communications sécurisées.  Actuellement AD FS prend en charge tous les protocoles et suites de chiffrement pris en charge par Schannel. dll.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Gestion des protocoles TLS/SSL et des suites de chiffrement
> [!IMPORTANT]
> Cette section contient des étapes qui vous indiquent comment modifier le registre. Toutefois, des problèmes sérieux peuvent survenir si vous modifiez le registre de manière incorrecte. Par conséquent, veillez à suivre ces étapes avec soin. 
> 
> Sachez que la modification des paramètres de sécurité par défaut pour SCHANNEL peut rompre ou empêcher les communications entre certains clients et serveurs.  Cela se produit si la communication sécurisée est requise et qu’ils n’ont pas de protocole pour négocier les communications avec.
> 
> Si vous appliquez ces modifications, elles doivent être appliquées à tous vos serveurs AD FS de votre batterie de serveurs.  Après avoir appliqué ces modifications, un redémarrage est nécessaire.

Dans la journée et l’âge actuels, renforcer la sécurité de vos serveurs et supprimer les suites de chiffrement plus anciennes ou faibles constitue une priorité majeure pour de nombreuses organisations.  Des suites logicielles sont disponibles pour tester vos serveurs et fournir des informations détaillées sur ces protocoles et suites.  Pour rester conformes ou atteindre des évaluations sécurisées, la suppression ou la désactivation de protocoles ou de suites de chiffrement plus faibles est devenue un.  Le reste de ce document fournit des conseils sur la façon d’activer ou de désactiver certains protocoles et suites de chiffrement.

Les clés de Registre ci-dessous se trouvent au même emplacement : HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols.  Utilisez regedit ou PowerShell pour activer ou désactiver ces protocoles et suites de chiffrement.

![Emplacement du Registre](media/Managing-SSL-Protocols-in-AD-FS/registry.png)

## <a name="enable-and-disable-ssl-20"></a>Activer et désactiver SSL 2,0
Utilisez les clés de Registre suivantes et leurs valeurs pour activer et désactiver SSL 2,0.

### <a name="enable-ssl-20"></a>Activer SSL 2,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ serveur] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ serveur] "DisabledByDefault" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ client] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ client] "DisabledByDefault" = dword : 00000000 

### <a name="disable-ssl-20"></a>Désactiver SSL 2,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ serveur] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ serveur] "DisabledByDefault" = dword : 00000001 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ client] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0 \ client] "DisabledByDefault" = dword : 00000001

### <a name="using-powershell-to-disable-ssl-20"></a>Utilisation de PowerShell pour désactiver SSL 2,0

``` powershell
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -Force | Out-Null
    
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
            
New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
            
New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 2.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
Write-Host 'SSL 2.0 has been disabled.'
```

## <a name="enable-and-disable-ssl-30"></a>Activer et désactiver SSL 3,0
Utilisez les clés de Registre suivantes et leurs valeurs pour activer et désactiver SSL 3,0.

### <a name="enable-ssl-30"></a>Activer SSL 3.0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "DisabledByDefault" = dword : 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ client] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ client] "DisabledByDefault" = dword : 00000000 

### <a name="disable-ssl-30"></a>Désactiver SSL 3,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ Server] "DisabledByDefault" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ client] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0 \ client] "DisabledByDefault" = dword : 00000001 

### <a name="using-powershell-to-disable-ssl-30"></a>Utilisation de PowerShell pour désactiver SSL 3,0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\SSL 3.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'SSL 3.0 has been disabled.'
```

## <a name="enable-and-disable-tls-10"></a>Activer et désactiver TLS 1,0
Utilisez les clés de Registre suivantes et leurs valeurs pour activer et désactiver TLS 1,0.

> [!IMPORTANT]
> La désactivation de TLS 1,0 rompt le WAP pour AD FS approbation.  Si vous désactivez TLS 1,0, vous devez activer l’authentification forte pour vos applications.  Voir [activer l’authentification forte](#enabling-strong-authentication-for-net-applications) 



### <a name="enable-tls-10"></a>Activer TLS 1,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "DisabledByDefault" = dword : 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ client] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ client] "DisabledByDefault" = dword : 00000000 

### <a name="disable-tls-10"></a>Désactiver TLS 1,0
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ Server] "DisabledByDefault" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ client] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0 \ client] "DisabledByDefault" = dword : 00000001 

### <a name="using-powershell-to-disable-tls-10"></a>Utilisation de PowerShell pour désactiver TLS 1,0

```powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.0 has been disabled.'
```


## <a name="enable-and-disable-tls-11"></a>Activer et désactiver TLS 1,1
Utilisez les clés de Registre suivantes et leurs valeurs pour activer et désactiver TLS 1,1.

### <a name="enable-tls-11"></a>Activer TLS 1,1
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "DisabledByDefault" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ client] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ client] "DisabledByDefault" = dword : 00000000

### <a name="disable-tls-11"></a>Désactiver TLS 1,1
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ Server] "DisabledByDefault" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ client] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1 \ client] "DisabledByDefault" = dword : 00000001 

### <a name="using-powershell-to-disable-tls-11"></a>Utilisation de PowerShell pour désactiver TLS 1,1

``` powershell
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.1 has been disabled.'
```

## <a name="enable-and-disable-tls-12"></a>Activer et désactiver TLS 1,2

Utilisez les clés de Registre suivantes et leurs valeurs pour activer et désactiver TLS 1,2.

### <a name="enable-tls-12"></a>Activer TLS 1,2
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "DisabledByDefault" = dword : 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ client] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ client] "DisabledByDefault" = dword : 00000000

### <a name="disable-tls-12"></a>Désactiver TLS 1,2
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server] "DisabledByDefault" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ client] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ client] "DisabledByDefault" = dword : 00000001

### <a name="using-powershell-to-disable-tls-12"></a>Utilisation de PowerShell pour désactiver TLS 1,2

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

Utilisez les clés de Registre suivantes et leurs valeurs pour activer et désactiver RC4.  Les clés de registre de cette suite de chiffrement se trouvent ici :

- HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\

![Emplacement du Registre](media/Managing-SSL-Protocols-in-AD-FS/cipher.png)



### <a name="enable-rc4"></a>Activer RC4

- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = dword : 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = dword : 00000001 

### <a name="disable-rc4"></a>Désactiver RC4

- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128] "Enabled" = dword : 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128] "Enabled" = dword : 00000000 

### <a name="using-powershell"></a>À l'aide de PowerShell

```powershell
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 128/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 40/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    ([Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey([Microsoft.Win32.RegistryHive]::LocalMachine,$env:COMPUTERNAME)).CreateSubKey('SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128') 
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Ciphers\RC4 56/128' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="enabling-or-disabling-additional-cipher-suites"></a>Activation ou désactivation de suites de chiffrement supplémentaires

Vous pouvez désactiver certains chiffrements spécifiques en les supprimant de HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\Cryptography\Configuration\Local\SSL\00010002 

![Emplacement du Registre](media/Managing-SSL-Protocols-in-AD-FS/suites.png)

Pour activer une suite de chiffrement, ajoutez sa valeur de chaîne à la clé de valeur de chaînes multiples functions.  Par exemple, si nous voulons activer TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P521, nous l’ajouterons à la chaîne.

Pour obtenir la liste complète des suites de chiffrement prises en charge, voir [suites de chiffrement dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).  Ce document fournit un tableau des suites qui sont activées par défaut et celles qui sont prises en charge, mais qui ne sont pas activées par défaut.  Pour hiérarchiser les suites de chiffrement, consultez [hiérarchisation des suites de chiffrement Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx).

## <a name="enabling-strong-authentication-for-net-applications"></a>Activation de l’authentification forte pour les applications .NET
Les applications .NET Framework 3.5/4.0/4.5. x peuvent basculer le protocole par défaut vers TLS 1,2 en activant la clé de Registre SchUseStrongCrypto.  Cette clé de Registre forcera les applications .NET à utiliser TLS 1,2.

> [!IMPORTANT]
> Pour AD FS sur Windows Server 2016 et Windows Server 2012 R2, vous devez utiliser la clé .NET Framework 4.0/4.5. x : HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\\. NETFramework\v4.0.30319


Pour la .NET Framework 3,5, utilisez la clé de Registre suivante :

[HKEY_LOCAL_MACHINE \SOFTWARE\Wow6432Node\Microsoft\\. NETFramework\v2.0.50727] "SchUseStrongCrypto" = dword : 00000001

Pour le .NET Framework 4.0/4.5. x, utilisez la clé de Registre suivante : HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\\. NETFramework\v4.0.30319 "SchUseStrongCrypto" = dword : 00000001

![Authentification renforcée](media/Managing-SSL-Protocols-in-AD-FS/strongauth.png)

```powershell
    
    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NetFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null
```

## <a name="additional-information"></a>Informations supplémentaires

- [Suites de chiffrement dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx)
- [Suites de chiffrement TLS dans Windows 8.1](https://msdn.microsoft.com/library/windows/desktop/mt767781.aspx)
- [Hiérarchisation des suites de chiffrement Schannel](https://msdn.microsoft.com/library/windows/desktop/bb870930.aspx)
- [En parlant dans les chiffrements et autres langues Enigmatic](https://blogs.technet.microsoft.com/askds/2015/12/08/speaking-in-ciphers-and-other-enigmatic-tonguesupdate/)
