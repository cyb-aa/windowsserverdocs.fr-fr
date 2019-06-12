---
title: Configurations prises en charge pour les Services Bureau à distance
description: Fournit des informations sur les configurations prises en charge pour les services Bureau à distance dans Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: 8571c2220f804a27e4e1a6b744e8e15e38bd53a3
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453086"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Configurations prises en charge pour les Services Bureau à distance dans Windows Server 2016

> S'applique à : Windows Server 2016

Lorsqu’il s’agit des configurations prises en charge pour les environnements de Services Bureau à distance, la plus grande préoccupation a tendance à être l’interopérabilité de version. La plupart des environnements incluent plusieurs versions de Windows Server : par exemple, vous avez peut-être un déploiement services Bureau à distance de Windows Server 2012 R2 existant mais souhaitez mettre à niveau vers Windows Server 2016 pour tirer parti des nouvelles fonctionnalités (telles que la prise en charge de OpenGL\OpenCL, discrète Affectation d’appareils, ou les espaces de stockage Direct). Puis la question devient, les composants des services Bureau à distance peuvent travailler avec différentes versions et qui doivent être les mêmes ?

Par conséquent, dans cette optique, vous trouverez ici les instructions de base pour les configurations prises en charge des Services Bureau à distance dans Windows Server 2016.

> [!NOTE]
> Passez en revue la [configuration système requise pour Windows Server 2016](../../get-started/system-requirements.md).

## <a name="best-practices"></a>Meilleures pratiques
- Utilisez Windows Server 2016 pour votre infrastructure de bureau à distance - l’accès Web, passerelle, service Broker pour les connexions et serveur de licences. Windows Server 2016 présente une compatibilité descendante pour ces composants : un hôte de Session Bureau à distance de 2012 R2 peut se connecter à un agent de connexion Bureau à distance 2016, mais l’inverse n’est pas vrai.

- Pour les hôtes de Session Bureau à distance - tous les hôtes de Session dans une collection doivent être au même niveau, mais vous pouvez avoir plusieurs collections. Vous pouvez avoir une collection avec les hôtes de Session de Windows Server 2012 R2 et l’autre avec les hôtes de Session de Windows Server 2016.

- Si vous mettez à niveau votre hôte de Session Bureau à distance vers Windows Server 2016, également mettre à niveau le serveur de licences. N’oubliez pas qu’un serveur de licences de 2016 peut traiter les licences d’accès client à partir de toutes les versions précédentes de Windows Server, jusqu'à Windows Server 2003.

- Suivez l’ordre de mise à niveau recommandée dans [mise à niveau votre environnement de Services Bureau à distance](upgrade-to-rds.md#flow-for-deployment-upgrades). 

- Si vous créez un environnement hautement disponible, tous les agents de votre connexion doivent être au même niveau de système d’exploitation.

## <a name="rd-connection-brokers"></a>Agents de connexion Bureau à distance

Windows Server 2016 supprime la restriction pour le nombre d’agents de connexion que vous pouvez avoir dans un déploiement lorsque vous utilisez des hôtes de Session de bureau à distance (RDSH) et à distance les hôtes de virtualisation Bureau (RDVH) qui exécutent également Windows Server 2016. Le tableau suivant montre quelles versions de travail de composants de services Bureau à distance avec les versions R2 2016 et 2012 du service Broker de connexion dans un déploiement à haute disponibilité avec trois ou plusieurs agents de connexion.

| 3 + agents de connexion dans la haute disponibilité              | RDSH 2016 | RDVH 2016 | RDSH 2012 R2  | RDVH 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Service Broker de connexion Windows Server 2016    | Prise en charge | Prise en charge | Prise en charge     | Prise en charge     |
| Agent de connexion de Windows Server 2012 R2 | N/A       | N/A       | Prise en charge     | Prise en charge     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>Prise en charge pour l’accélération GPU avec Hyper-V
Le tableau suivant décrit en détail la prise en charge pour l’accélération GPU sur des machines virtuelles. Consultez [quelle technologie de virtualisation de graphiques vous convient ?](rds-graphics-virtualization.md) pour essayer de comprendre ce dont vous avez besoin de l’aide. Pour obtenir des informations spécifiques sur DDA, consultez [planifier le déploiement d’attribution discrète d’appareil](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

|Système d’exploitation invité de machine virtuelle  |Windows Server 2012 R2 ou Windows Server 2016<br> Hyper-V RemoteFX vGPU (Gen 1 machine virtuelle) |  Windows Server 2016 Hyper-V RemoteFX vGPU (Gen 2 VM) |  Attribution de discrète d’appareils Windows Server 2016 Hyper-V (machine virtuelle 2 de la génération) |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | Oui                                                        | Non                                                     | Non                                                                  |
| Windows 8.1                 | Oui                                                        | Non                                                     | Non                                                                  |
| Mise à jour de Windows 10 1511      | Oui                                                        | Oui                                                    | Oui                                                                 |
| Windows Server 2012 R2      | Oui                                                        | Non                                                     | Oui (exige la base de connaissances 3133690)                                           |
| Windows Server 2016         | Oui                                                        | Oui                                                    | Oui                                                                 |
| Windows Server 2012 R2 RDSH | Non                                                         | Non                                                     | Oui (exige la base de connaissances 3133690)                                           |
| Windows Server 2016 RDSH    | Non                                                         | Non                                                     | Oui                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>Déploiement VDI – des systèmes d’exploitation invités pris en charge 
Serveurs hôtes de virtualisation des services Bureau à distance Windows Server 2016 prennent en charge les systèmes d’exploitation invités suivants :

- Windows 10 Entreprise
- Windows 8.1 Entreprise 
- Windows 8 Entreprise 
- Windows 7 SP1 Enterprise 

Le tableau ci-dessous répertorie les systèmes d’exploitation hôtes de virtualisation des services Bureau à distance et les combinaisons de système d’exploitation invité :

| Version du système d’exploitation RDVH        | Version du système d’exploitation invité           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012      | Windows 7 SP1, Windows 8, Windows 8.1 |

> [!NOTE]  
> - Services Bureau à distance de Windows Server 2016 ne prend pas en charge des collections hétérogènes. Toutes les machines virtuelles dans une collection doivent être la même version du système d’exploitation. 
> - Vous pouvez avoir des collections distinctes homogènes avec les versions de système d’exploitation invités différents sur le même hôte. 
> - Modèles d’ordinateur virtuel doivent être créés sur un ordinateur hôte Windows Server 2016 Hyper-V utilisé comme système d’exploitation invité sur un ordinateur hôte Windows Server 2016 Hyper-V.

## <a name="single-sign-on-sso"></a>Single Sign-On (SSO)
Services Bureau à distance de Windows Server 2016 prend en charge deux expériences de l’authentification unique principales :

 - Dans l’application (application de bureau à distance sur Windows, iOS, Android et Mac)
 - SSO de web
 
À l’aide de l’application de bureau à distance, vous pouvez stocker des informations d’identification dans le cadre des informations de connexion ([Mac](clients/remote-desktop-mac.md)) ou dans le cadre des comptes gérés ([iOS](clients/remote-desktop-ios.md#manage-your-user-accounts), [Android](clients/remote-desktop-android.md#manage-your-user-accounts), Windows) en toute sécurité par le biais de mécanismes propres à chaque système d’exploitation.

Pour vous connecter à des bureaux et RemoteApps avec l’authentification unique via le client Connexion Bureau à distance de boîte de réception sur Windows, vous devez vous connecter à la page Web du Bureau à distance via Internet Explorer. Les options de configuration suivantes sont requises sur le côté serveur. Aucune autre configuration n’est pris en charge pour l’authentification unique Web :

 - Web Bureau à distance est définie sur l’authentification par formulaire (par défaut)
 - Passerelle Bureau à distance à l’authentification de mot de passe (par défaut)
 - Déploiement des services Bureau à distance est définie sur « Informations d’identification de passerelle Bureau à distance de l’utilisation pour les ordinateurs distants » (par défaut) dans les propriétés de passerelle Bureau à distance

> [!NOTE]
> Les options de configuration requise, en raison d’une SSO de Web n’est pas pris en charge avec les cartes à puce. Utilisateurs qui se connecte via les cartes à puce pouvez rencontrer plusieurs invites de connexion.

Pour plus d’informations sur la création de déploiement VDI des Services Bureau à distance, consultez [les configurations de sécurité pris en charge Windows 10 pour l’infrastructure VDI des Services Bureau à distance](rds-vdi-supported-config.md).

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>À l’aide des Services Bureau à distance avec les services de proxy d’application

Vous pouvez utiliser des Services Bureau à distance, à l’exception du client web, avec [Proxy d’Application Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop). Services Bureau à distance ne prend pas en charge à l’aide de [Proxy d’Application Web](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server), qui est inclus dans Windows Server 2016 et versions antérieures.
