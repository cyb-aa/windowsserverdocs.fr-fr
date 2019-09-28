---
title: Configurations prises en charge pour les services Bureau à distance
description: Fournit des informations sur les configurations prises en charge pour les services Bureau à distance dans Windows Server 2016.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7363fe3eee33a5345a25c8df9e4216b9eda7e3b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403811"
---
# <a name="supported-configurations-for-remote-desktop-services-in-windows-server-2016"></a>Configurations prises en charge pour les services Bureau à distance dans Windows Server 2016

> S'applique à : Windows Server 2016

En ce qui concerne les configurations prises en charge pour les environnements des services Bureau à distance, l’interopérabilité des versions est le principal sujet de préoccupation. La plupart des environnements incluent plusieurs versions de Windows Server. Par exemple, vous disposez d’un déploiement des services Bureau à distance de Windows Server 2012 R2, mais vous souhaitez effectuer une mise à niveau vers Windows Server 2016 pour bénéficier des nouvelles fonctionnalités (par exemple la prise en charge d’OpenGL\OpenCL, l’affectation d’appareils en mode discret ou les espaces de stockage direct). La question devient celle-ci : quels sont les composants des services Bureau à distance qui peuvent fonctionner avec différentes versions et lesquels doivent être identiques ?

Dans cette optique, voici les instructions de base relatives aux configurations prises en charge par les services Bureau à distance dans Windows Server 2016.

> [!NOTE]
> Veillez à consulter la [configuration requise pour Windows Server 2016](../../get-started/system-requirements.md).

## <a name="best-practices"></a>Meilleures pratiques
- Utilisez Windows Server 2016 pour votre infrastructure de Bureau à distance : serveur d’accès web, serveur de passerelle, serveur du service Broker pour les connexions et serveur de licences. Windows Server 2016 présente une compatibilité descendante pour ces composants. Ainsi, un hôte de session Bureau à distance 2012 R2 peut se connecter à un service Broker pour les connexions Bureau à distance 2016, alors que l’inverse n’est pas vrai.

- Pour les hôtes de session Bureau à distance, tous les hôtes de session d’une collection doivent être au même niveau. Toutefois, vous pouvez avoir plusieurs collections. Vous pouvez avoir une collection avec des hôtes de session Windows Server 2012 R2 et une collection avec des hôtes de session Windows Server 2016.

- Si vous mettez à niveau votre hôte de session Bureau à distance vers Windows Server 2016, mettez également à niveau le serveur de licences. N’oubliez pas qu’un serveur de licences 2016 peut traiter les licences d’accès client de toutes les versions précédentes de Windows Server, jusqu’à Windows Server 2003.

- Suivez l’ordre de mise à niveau recommandé dans [Mise à niveau de votre environnement des services Bureau à distance](upgrade-to-rds.md#flow-for-deployment-upgrades). 

- Si vous créez un environnement hautement disponible, tous vos serveurs du service Broker pour les connexions doivent être au même niveau d’OS (système d’exploitation).

## <a name="rd-connection-brokers"></a>Serveurs du service Broker pour les connexions Bureau à distance

Windows Server 2016 supprime la restriction relative au nombre de serveurs du service Broker pour les connexions disponibles dans un déploiement quand vous utilisez des hôtes de session Bureau à distance et des serveurs hôtes de virtualisation des services Bureau à distance qui exécutent également Windows Server 2016. Le tableau suivant indique les versions des composants des services Bureau à distance compatibles avec les versions 2016 et 2012 R2 du service Broker pour les connexions dans un déploiement hautement disponible avec au moins trois serveurs du service Broker pour les connexions.

| Plus de 3 serveurs du service Broker pour les connexions en haute disponibilité              | Hôte de session Bureau à distance 2016 | Serveur hôte de virtualisation des services Bureau à distance 2016 | Hôte de session Bureau à distance 2012 R2  | Serveur hôte de virtualisation des services Bureau à distance 2012 R2  |
|------------------------------------------|-----------|-----------|---------------|---------------|
| Service Broker pour les connexions Windows Server 2016    | Prise en charge | Prise en charge | Prise en charge     | Prise en charge     |
| Service Broker pour les connexions Windows Server 2012 R2 | N/A       | N/A       | Prise en charge     | Prise en charge     |

## <a name="support-for-gpu-acceleration-with-hyper-v"></a>Prise en charge de l’accélération GPU avec Hyper-V
Le tableau suivant détaille la prise en charge de l’accélération GPU sur les machines virtuelles. Consultez l’article [Quelle technologie de virtualisation graphique vous convient ?](rds-graphics-virtualization.md) pour déterminer ce dont vous avez besoin. Pour plus d’informations sur l’affectation d’appareils en mode discret, consultez [Planification du déploiement de l’affectation d’appareils en mode discret](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

|OS invité de machine virtuelle  |Windows Server 2012 R2 ou Windows Server 2016<br> Hyper-V avec RemoteFX vGPU (machine virtuelle de génération 1) |  Windows Server 2016 Hyper-V avec RemoteFX vGPU (machine virtuelle de génération 2) |  Windows Server 2016 Hyper-V avec affectation d’appareils en mode discret (machine virtuelle de génération 2) |
|-----------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
| Windows 7 SP1               | Oui                                                        | Non                                                     | Non                                                                  |
| Windows 8.1                 | Oui                                                        | Non                                                     | Non                                                                  |
| Windows 10, mise à jour 1511      | Oui                                                        | Oui                                                    | Oui                                                                 |
| Windows Server 2012 R2      | Oui                                                        | Non                                                     | Oui (nécessite la mise à jour KB 3133690)                                           |
| Windows Server 2016         | Oui                                                        | Oui                                                    | Oui                                                                 |
| Hôte de session Bureau à distance Windows Server 2012 R2 | Non                                                         | Non                                                     | Oui (nécessite la mise à jour KB 3133690)                                           |
| Hôte de session Bureau à distance Windows Server 2016    | Non                                                         | Non                                                     | Oui                                                                 |
## <a name="vdi-deployment--supported-guest-oss"></a>Déploiement VDI - OS invités pris en charge 
Les serveurs hôtes de virtualisation des services Bureau à distance de Windows Server 2016 prennent en charge les OS (systèmes d’exploitation) invités suivants :

- Windows 10 Entreprise
- Windows 8.1 Entreprise 
- Windows 8 Entreprise 
- Windows 7 SP1 Entreprise 

Le tableau ci-dessous liste les combinaisons prises en charge pour les systèmes d’exploitation hôtes de virtualisation des services Bureau à distance et les systèmes d’exploitation invités :

| Version de l’OS du serveur hôte de virtualisation des services Bureau à distance        | Version de l’OS invité           |
| ------------- |-------------|
| Windows Server 2016      | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012 R2   | Windows 7 SP1, Windows 8, Windows 8.1, Windows 10 |
| Windows Server 2012      | Windows 7 SP1, Windows 8, Windows 8.1 |

> [!NOTE]  
> - Les services Bureau à distance de Windows Server 2016 ne prennent pas en charge les collections hétérogènes. Toutes les machines virtuelles d’une collection doivent avoir la même version d’OS. 
> - Vous pouvez avoir des collections homogènes distinctes avec différentes versions d’OS invité sur le même hôte. 
> - Vous devez créer les modèles de machine virtuelle sur un hôte Windows Server 2016 Hyper-V pour pouvoir les utiliser en tant qu’OS invités sur un hôte Windows Server 2016 Hyper-V.

## <a name="single-sign-on-sso"></a>SSO (authentification unique)
Les services Bureau à distance de Windows Server 2016 prennent en charge deux expériences SSO principales :

 - Dans l’application (application Bureau à distance sur Windows, iOS, Android et Mac)
 - SSO de web
 
À l’aide de l’application Bureau à distance, vous pouvez stocker les informations d’identification dans le cadre des informations de connexion ([Mac](clients/remote-desktop-mac.md)) ou dans le cadre des comptes managés ([iOS](clients/remote-desktop-ios.md#manage-your-user-accounts), [Android](clients/remote-desktop-android.md#manage-your-user-accounts), Windows) de manière sécurisée grâce aux mécanismes propres à chaque OS.

Pour vous connecter à des bureaux et des programmes RemoteApp à l’aide de SSO via le client Connexion Bureau à distance intégré à Windows, vous devez vous connecter à la page web du Bureau à distance via Internet Explorer. Les options de configuration suivantes sont nécessaires côté serveur. Aucune autre configuration n’est prise en charge pour le SSO web :

 - Connexion Bureau à distance par le web avec l’option d’authentification basée sur les formulaires (par défaut)
 - Passerelle des services Bureau à distance avec l’option d’authentification par mot de passe (par défaut)
 - Déploiement des services Bureau à distance avec l’option « Utiliser les informations d’identification de la passerelle des services Bureau à distance pour les ordinateurs distants » (par défaut) dans les propriétés de la passerelle des services Bureau à distance

> [!NOTE]
> En raison des options de configuration nécessaires, le SSO web n’est pas pris en charge avec les cartes à puce. Les utilisateurs qui se connectent via des cartes à puce risquent d’être confrontés à plusieurs invites de connexion.

Pour plus d’informations sur la création d’un déploiement VDI des services Bureau à distance, consultez [Configurations de sécurité Windows 10 prises en charge pour l’infrastructure VDI des services Bureau à distance](rds-vdi-supported-config.md).

## <a name="using-remote-desktop-services-with-application-proxy-services"></a>Utilisation des services Bureau à distance avec les services proxy d’application

Vous pouvez utiliser les services Bureau à distance, à l’exception du client web, avec le [proxy d’application Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-remote-desktop). Les services Bureau à distance ne prennent pas en charge l’utilisation du [proxy d’application web](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server), inclus dans Windows Server 2016 et les versions antérieures.
