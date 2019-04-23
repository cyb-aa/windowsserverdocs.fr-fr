---
title: Déployer Windows Admin Center avec une haute disponibilité
description: Déployer Windows Admin Center avec une haute disponibilité (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861060"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Déployer Windows Admin Center avec une haute disponibilité

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Vous pouvez déployer Windows Admin Center dans un cluster de basculement pour fournir une haute disponibilité pour votre service de passerelle Windows Admin Center. La solution fournie est une solution actif / passif, où une seule instance de Windows Admin Center est active. Si un des nœuds du cluster échoue, Windows Admin Center normalement bascule vers un autre nœud, ce qui vous permet de continuer à gérer les serveurs dans votre environnement en toute transparence. 

[En savoir plus sur les autres options de déploiement Windows Admin Center.](../plan/installation-options.md)

## <a name="prerequisites"></a>Prérequis

- Un cluster de basculement de nœuds 2 ou plus sur Windows Server 2016 ou 2019. [En savoir plus sur le déploiement d’un Cluster de basculement](../../../failover-clustering/failover-clustering-overview.md).
- Un cluster de volume partagé (CSV pour Windows Admin Center stocker les données persistantes qui sont accessible par tous les nœuds du cluster). 10 Go sera suffisant pour votre fichier CSV.
- Script de déploiement à haute disponibilité à partir de [fichier zip de haute disponibilité Script Windows Admin Center](https://aka.ms/WACHAScript). Téléchargez le fichier .zip contenant le script sur votre ordinateur local, puis copiez le script en fonction des besoins selon les instructions ci-dessous.
- Recommandé mais facultatif : un certificat auto-signé .pfx & un mot de passe. Vous n’avez pas besoin de l’avez déjà installé le certificat sur les nœuds de cluster, le script effectue cette opération pour vous. Si vous ne fournissez pas une, le script d’installation génère un certificat auto-signé, qui expire au bout de 60 jours.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Installer Windows Admin Center sur un cluster de basculement

1. Copie le ```Install-WindowsAdminCenterHA.ps1``` script à un nœud dans votre cluster. Téléchargez ou copiez le fichier .msi Windows Admin Center dans le même nœud.
2. Connectez-vous au nœud via RDP et exécutez le ```Install-WindowsAdminCenterHA.ps1``` script à partir de ce nœud avec les paramètres suivants :
    - `-clusterStorage`: le chemin d’accès local du Volume partagé de Cluster pour stocker les données de Windows Admin Center.
    - `-clientAccessPoint`: choisissez un nom que vous utiliserez pour accéder à Windows Admin Center. Par exemple, si vous exécutez le script avec le paramètre `-clientAccessPoint contosoWindowsAdminCenter`, vous accéderez le service Windows Admin Center en vous rendant sur `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Facultatif. Un ou plusieurs des adresses statiques pour le service de cluster générique. 
    - `-msiPath`: Le chemin d’accès pour le fichier .msi Windows Admin Center.
    - `-certPath`: Facultatif. Le chemin d’accès pour un fichier de certificat .pfx.
    - `-certPassword`: Facultatif. Un mot de passe de SecureString pour le fichier .pfx de certificat fourni dans `-certPath`
    - `-generateSslCert`: Facultatif. Si vous ne souhaitez pas fournir un certificat signé, inclure cet indicateur de paramètre pour générer un certificat auto-signé. Notez que le certificat auto-signé expire dans 60 jours.
    - `-portNumber`: Facultatif. Si vous ne spécifiez pas un port, le service de passerelle est déployé sur le port 443 (HTTPS). Pour utiliser un autre port spécifiez dans ce paramètre. Notez que si vous utilisez un port personnalisé (quoi que ce soit outre le port 443), vous allez accéder à la Windows Admin Center en accédant à https://\<clientAccessPoint\>:\<port\>.

> [!NOTE]
> Le ```Install-WindowsAdminCenterHA.ps1``` prend en charge de script ```-WhatIf ``` et ```-Verbose``` paramètres

### <a name="examples"></a>Exemples

#### <a name="install-with-a-signed-certificate"></a>Installer avec un certificat signé :

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Installer avec un certificat auto-signé :

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Mise à jour une installation existante de la haute disponibilité

Utiliser le même ```Install-WindowsAdminCenterHA.ps1``` script pour mettre à jour votre déploiement de haute disponibilité, sans perdre vos données de connexion.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Mettre à jour vers une nouvelle version de Windows Admin Center

Quand une nouvelle version de Windows Admin Center est publiée, exécutez simplement le ```Install-WindowsAdminCenterHA.ps1``` nouveau script avec uniquement le ```msiPath``` paramètre :

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Mettre à jour le certificat utilisé par Windows Admin Center

Vous pouvez mettre à jour le certificat utilisé par un déploiement de haute disponibilité de Windows Admin Center à tout moment en fournissant du nouveau fichier du certificat .pfx et et le mot de passe.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

Vous pouvez également mettre à jour le certificat en même temps que vous mettez à jour la plateforme Windows Admin Center avec un nouveau fichier .msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Désinstaller

Pour désinstaller le déploiement de haute disponibilité de Windows Admin Center à partir de votre cluster de basculement, passez le ```-Uninstall``` paramètre à la ```Install-WindowsAdminCenterHA.ps1``` script.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Résolution des problèmes

Journaux sont enregistrés dans le dossier temporaire du fichier CSV (par exemple, C:\ClusterStorage\Volume1\temp).