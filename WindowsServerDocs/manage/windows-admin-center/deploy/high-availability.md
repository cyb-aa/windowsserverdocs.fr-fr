---
title: Déployer Windows Admin Center à haute disponibilité
description: Déployer Windows Admin Center à haute disponibilité (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 802a7bd537cab22893abb7e6657c4be90346ef88
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/23/2019
ms.locfileid: "9025031"
---
# Déployer Windows Admin Center à haute disponibilité

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Vous pouvez déployer Windows Admin Center dans un cluster de basculement pour fournir une haute disponibilité de votre service de passerelle Windows Admin Center. La solution fournie est une solution actif / passif, où une seule instance de Windows Admin Center est active. Si un des nœuds du cluster échoue, Windows Admin Center normalement bascule vers un autre nœud, ce qui vous permet de continuer à gérer les serveurs dans votre environnement en toute transparence. 

[Découvrez les autres options de déploiement de Windows Admin Center.](../plan/installation-options.md)

## Conditions préalables

- Un cluster de basculement de 2 ou plusieurs nœuds sur Windows Server 2016 ou 2019. [En savoir plus sur le déploiement d’un Cluster de basculement](../../../failover-clustering/failover-clustering-overview.md).
- Un cluster shared volumes (CSV) pour Windows Admin Center stocker des données persistantes sont accessibles par tous les nœuds du cluster. 10 Go sera suffisant pour votre volume partagé de cluster.
- Script de déploiement à haute disponibilité à partir du [fichier zip de haute disponibilité Script Windows Admin Center](https://aka.ms/WACHAScript). Téléchargez le fichier .zip contenant le script sur votre ordinateur local, puis copiez le script en fonction des besoins selon les instructions ci-dessous.
- Recommandée, mais facultative: un mot de passe de certificat .pfx &. Vous n’êtes pas obligé du certificat déjà installé sur les nœuds de cluster: le script s’en chargera pour vous. Si vous ne fournissez pas une, le script d’installation génère un certificat auto-signé, ce qui arrive à expiration après 60 jours.

## Installer Windows Admin Center sur un cluster de basculement

1. Copie le ```Install-WindowsAdminCenterHA.ps1``` script à un nœud dans votre cluster. Télécharger ou copier le fichier .msi Windows Admin Center sur le même nœud.
2. Connectez-vous au nœud via RDP et exécuter la ```Install-WindowsAdminCenterHA.ps1``` script à partir de ce nœud avec les paramètres suivants:
    - `-clusterStorage`: le chemin d’accès local du Volume partagé de Cluster pour stocker les données de Windows Admin Center.
    - `-clientAccessPoint`: choisissez un nom que vous allez utiliser pour accéder au centre d’administration de Windows. Par exemple, si vous exécutez le script avec le paramètre `-clientAccessPoint contosoWindowsAdminCenter`, vous accède au service Windows Admin Center en consultant `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Facultatif. Une ou plusieurs adresses statiques du service de cluster générique. 
    - `-msiPath`: Le chemin d’accès du fichier .msi de Windows Admin Center.
    - `-certPath`: Facultatif. Le chemin d’accès d’un fichier .pfx de certificat.
    - `-certPassword`: Facultatif. Le certificat au format .pfx fournies dans un mot de passe SecureString `-certPath`
    - `-generateSslCert`: Facultatif. Si vous ne souhaitez pas fournir un certificat signé, inclure cet indicateur de paramètre pour générer un certificat auto-signé. Notez que le certificat auto-signé va expirer dans les 60 jours.
    - `-portNumber`: Facultatif. Si vous ne spécifiez un port, le service de passerelle est déployé sur le port 443 (HTTPS). Pour utiliser un autre port spécifiez dans ce paramètre. Notez que si vous utilisez un port personnalisé (rien outre 443), vous devez accéder le centre d’administration Windows en accédant à https://\<clientAccessPoint\>:\<port\>.

> [!NOTE]
> Le ```Install-WindowsAdminCenterHA.ps1``` prend en charge de script ```-WhatIf ``` et ```-Verbose``` paramètres

### Exemples

#### Installez avec un certificat signé:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### Installez avec un certificat auto-signé:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## Mise à jour une installation existante de la haute disponibilité

Utiliser le même ```Install-WindowsAdminCenterHA.ps1``` script pour mettre à jour de votre déploiement de la haute disponibilité, sans perte de vos données de connexion.

### Mettre à jour vers une nouvelle version de Windows Admin Center

Quand une nouvelle version de Windows Admin Center est libérée, exécutez simplement la ```Install-WindowsAdminCenterHA.ps1``` script à nouveau avec uniquement le ```msiPath``` paramètre:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### Mettre à jour le certificat utilisé par Windows Admin Center

Vous pouvez mettre à jour le certificat utilisé par un déploiement de la haute disponibilité de Windows Admin Center à tout moment en fournissant .pfx fichier du nouveau certificat et et mot de passe.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

Vous pouvez également mettre à jour le certificat en même temps que vous mettre à jour la plateforme Windows Admin Center avec un nouveau fichier .msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## Désinstaller

Pour désinstaller le déploiement de la haute disponibilité de Windows Admin Center à partir de votre cluster de basculement, transmettez le ```-Uninstall``` paramètre à le ```Install-WindowsAdminCenterHA.ps1``` script.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## Résolution des problèmes

Journaux sont enregistrés dans le dossier temporaire du volume partagé de cluster (par exemple, C:\ClusterStorage\Volume1\temp).