---
title: Déployer Windows Admin Center avec une haute disponibilité
description: Déployer Windows Admin Center avec une haute disponibilité (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ae7bd9ed7aee5835ac1f53b9e10879ad8824f52
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71406944"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Déployer Windows Admin Center avec une haute disponibilité

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Vous pouvez déployer Windows Admin Center dans un cluster de basculement pour fournir une haute disponibilité à votre service de passerelle Windows Admin Center. La solution fournie est une solution active-passive, où une seule instance de Windows Admin Center est active. Si l’un des nœuds du cluster échoue, Windows Admin Center bascule normalement vers un autre nœud, ce qui vous permet de continuer à gérer les serveurs de votre environnement sans aucune interruption. 

[Apprenez-en davantage sur les autres options de déploiement de Windows Admin Center.](../plan/installation-options.md)

## <a name="prerequisites"></a>Prérequis

- Un cluster de basculement de deux nœuds ou plus sur Windows Server 2016 ou 2019. [Apprenez-en davantage sur le déploiement d’un cluster de basculement](../../../failover-clustering/failover-clustering-overview.md).
- Un volume partagé de cluster pour Windows Admin Center afin de stocker les données persistantes accessibles par tous les nœuds du cluster. 10 Go suffiront pour votre volume partagé de cluster.
- Script de déploiement de haute disponibilité obtenu à partir du [fichier zip du script de haute disponibilité Windows Admin Center](https://aka.ms/WACHAScript). Téléchargez le fichier .zip contenant le script sur votre ordinateur local, puis copiez le script en fonction de vos besoins, en suivant les instructions ci-dessous.
- Recommandé, mais facultatif : un certificat signé .pfx et un mot de passe. Vous n’avez pas besoin d’avoir déjà installé le certificat sur les nœuds du cluster : le script le fera pour vous. Si vous n’en fournissez pas, le script d’installation génère un certificat auto-signé qui expire au bout de 60 jours.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Installer Windows Admin Center sur un cluster de basculement

1. Copiez le script ```Install-WindowsAdminCenterHA.ps1``` sur un nœud de votre cluster. Téléchargez ou copiez le fichier .msi de Windows Admin Center sur le même nœud.
2. Connectez-vous au nœud par le biais du protocole RDP et exécutez le script ```Install-WindowsAdminCenterHA.ps1``` à partir de ce nœud avec les paramètres suivants :
    - `-clusterStorage` : chemin local du volume partagé de cluster pour stocker les données Windows Admin Center.
    - `-clientAccessPoint` : choisissez un nom que vous utiliserez pour accéder à Windows Admin Center. Par exemple, si vous exécutez le script avec le paramètre `-clientAccessPoint contosoWindowsAdminCenter`, vous accéderez au service Windows Admin Center en visitant `https://contosoWindowsAdminCenter.<domain>.com`.
    - `-staticAddress` : Facultatif. Une ou plusieurs adresses statiques pour le service générique de cluster. 
    - `-msiPath` : Chemin du fichier .msi de Windows Admin Center.
    - `-certPath` : Facultatif. Chemin d’un fichier de certificat .pfx.
    - `-certPassword` : Facultatif. Mot de passe SecureString pour le fichier de certificat .pfx fourni dans `-certPath`.
    - `-generateSslCert` : Facultatif. Si vous ne souhaitez pas fournir de certificat signé, incluez cet indicateur de paramètre pour générer un certificat auto-signé. Notez que le certificat auto-signé expire après 60 jours.
    - `-portNumber` : Facultatif. Si vous ne spécifiez pas de port, le service de passerelle est déployé sur le port 443 (HTTPS). Pour utiliser un port différent, spécifiez-le dans ce paramètre. Notez que si vous utilisez un port personnalisé (autre que 443), vous accéderez à Windows Admin Center en accédant à https://\<point_d’accès_client\>:\<port\>.

> [!NOTE]
> Le script ```Install-WindowsAdminCenterHA.ps1``` prend en charge les paramètres ```-WhatIf ``` et ```-Verbose```.

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

## <a name="update-an-existing-high-availability-installation"></a>Mettre à jour une installation de haute disponibilité existante

Utilisez le même script ```Install-WindowsAdminCenterHA.ps1``` pour mettre à jour votre déploiement à haute disponibilité, sans perdre vos données de connexion.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Mettre à jour vers une nouvelle version de Windows Admin Center

Quand une nouvelle version de Windows Admin Center est publiée, il vous suffit de réexécuter le script ```Install-WindowsAdminCenterHA.ps1``` avec uniquement le paramètre ```msiPath``` :

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Mettre à jour le certificat utilisé par Windows Admin Center

Vous pouvez mettre à jour le certificat utilisé par un déploiement à haute disponibilité de Windows Admin Center à tout moment en fournissant le fichier .pfx et le mot de passe du nouveau certificat.

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

Pour désinstaller le déploiement à haute disponibilité de Windows Admin Center à partir de votre cluster de basculement, transmettez le paramètre ```-Uninstall``` au script ```Install-WindowsAdminCenterHA.ps1```.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Résolution des problèmes

Les journaux sont enregistrés dans le dossier temp du volume partagé de cluster (par exemple, C:\ClusterStorage\Volume1\temp).