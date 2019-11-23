---
title: Déployer le centre d’administration Windows avec une haute disponibilité
description: Déployer le centre d’administration Windows avec une haute disponibilité (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ae7bd9ed7aee5835ac1f53b9e10879ad8824f52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406944"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Déployer le centre d’administration Windows avec une haute disponibilité

>S’applique à : Windows Admin Center, Windows Admin Center Preview

Vous pouvez déployer le centre d’administration Windows dans un cluster de basculement pour fournir une haute disponibilité à votre service de passerelle du centre d’administration Windows. La solution fournie est une solution active-passive, où une seule instance du centre d’administration Windows est active. Si l’un des nœuds du cluster échoue, le centre d’administration Windows bascule normalement vers un autre nœud, ce qui vous permet de continuer à gérer les serveurs de votre environnement en toute transparence. 

[En savoir plus sur les autres options de déploiement du centre d’administration Windows.](../plan/installation-options.md)

## <a name="prerequisites"></a>Conditions préalables

- Un cluster de basculement de 2 nœuds ou plus sur Windows Server 2016 ou 2019. [En savoir plus sur le déploiement d’un cluster de basculement](../../../failover-clustering/failover-clustering-overview.md).
- Un volume partagé de cluster (CSV) pour le centre d’administration Windows pour stocker les données persistantes accessibles par tous les nœuds du cluster. 10 Go suffira pour votre CSV.
- Script de déploiement de haute disponibilité à partir du fichier zip du script de haute disponibilité du [Centre d’administration Windows](https://aka.ms/WACHAScript). Téléchargez le fichier. zip contenant le script sur votre ordinateur local, puis copiez le script selon vos besoins, en suivant les instructions ci-dessous.
- Recommandé, mais facultatif : un certificat signé. pfx & un mot de passe. Vous n’avez pas besoin d’avoir déjà installé le certificat sur les nœuds du cluster : le script le fera pour vous. Si vous n’en fournissez pas, le script d’installation génère un certificat auto-signé, qui expire au bout de 60 jours.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Installer le centre d’administration Windows sur un cluster de basculement

1. Copiez le script ```Install-WindowsAdminCenterHA.ps1``` sur un nœud de votre cluster. Téléchargez ou copiez le fichier. msi du centre d’administration Windows sur le même nœud.
2. Connectez-vous au nœud via RDP et exécutez le script ```Install-WindowsAdminCenterHA.ps1``` à partir de ce nœud avec les paramètres suivants :
    - `-clusterStorage`: chemin d’accès local du Volume partagé de cluster pour stocker les données du centre d’administration Windows.
    - `-clientAccessPoint`: choisissez un nom que vous utiliserez pour accéder au centre d’administration Windows. Par exemple, si vous exécutez le script avec le paramètre `-clientAccessPoint contosoWindowsAdminCenter`, vous accédez au service Centre d’administration Windows en visitant `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: facultatif. Une ou plusieurs adresses statiques pour le service générique de cluster. 
    - `-msiPath`: chemin d’accès du fichier. msi du centre d’administration Windows.
    - `-certPath`: facultatif. Chemin d’accès d’un fichier de certificat. pfx.
    - `-certPassword`: facultatif. Un mot de passe SecureString pour le fichier Certificate. pfx fourni dans `-certPath`
    - `-generateSslCert`: facultatif. Si vous ne souhaitez pas fournir de certificat signé, incluez cet indicateur de paramètre pour générer un certificat auto-signé. Notez que le certificat auto-signé expire dans 60 jours.
    - `-portNumber`: facultatif. Si vous ne spécifiez pas de port, le service de passerelle est déployé sur le port 443 (HTTPs). Pour utiliser un port différent, spécifiez dans ce paramètre. Notez que si vous utilisez un port personnalisé (autre que 443), vous accédez au centre d’administration Windows en accédant à https://\<clientAccessPoint\>:\<port\>.

> [!NOTE]
> Le script ```Install-WindowsAdminCenterHA.ps1``` prend en charge les paramètres ```-WhatIf ``` et ```-Verbose```

### <a name="examples"></a>Exemples

#### <a name="install-with-a-signed-certificate"></a>Installez avec un certificat signé :

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Installez avec un certificat auto-signé :

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Mettre à jour une installation de haute disponibilité existante

Utilisez le même ```Install-WindowsAdminCenterHA.ps1``` script pour mettre à jour votre déploiement à haute disponibilité, sans perdre vos données de connexion.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Mettre à jour vers une nouvelle version du centre d’administration Windows

Quand une nouvelle version du centre d’administration Windows est publiée, il vous suffit d’exécuter à nouveau le script ```Install-WindowsAdminCenterHA.ps1``` avec uniquement le paramètre ```msiPath``` :

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Mettre à jour le certificat utilisé par le centre d’administration Windows

Vous pouvez mettre à jour le certificat utilisé par un déploiement à haute disponibilité du centre d’administration Windows à tout moment en fournissant le fichier. pfx du nouveau certificat et le mot de passe.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

Vous pouvez également mettre à jour le certificat en même temps que vous mettez à jour la plateforme du centre d’administration Windows avec un nouveau fichier. msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Désinstaller

Pour désinstaller le déploiement à haute disponibilité du centre d’administration Windows à partir de votre cluster de basculement, transmettez le paramètre ```-Uninstall``` au script ```Install-WindowsAdminCenterHA.ps1```.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Résolution des problèmes

Les journaux sont enregistrés dans le dossier Temp du volume partagé de cluster (par exemple, C:\ClusterStorage\Volume1\temp).