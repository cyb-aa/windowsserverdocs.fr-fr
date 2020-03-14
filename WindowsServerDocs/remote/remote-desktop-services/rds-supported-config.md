---
title: Configurations prises en charge pour les services Bureau à distance
description: Fournit des informations sur les configurations prises en charge pour les services Bureau à distance dans Windows Server 2016 et Windows Server 2019.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/22/2019
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c925c7eb-6880-411f-8e59-bd0f57cc5fc3
author: lizap
manager: dongill
ms.openlocfilehash: e501d550e5371c668f7e243f00106a0b79f694dc
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323351"
---
# <a name="supported-configurations-for-remote-desktop-services"></a>Configurations prises en charge pour les services Bureau à distance

> S'applique à : Windows Server 2016, Windows Server 2019

En ce qui concerne les configurations prises en charge pour les environnements des services Bureau à distance, l’interopérabilité des versions est le principal sujet de préoccupation. La plupart des environnements incluent plusieurs versions de Windows Server. Par exemple, vous disposez d’un déploiement des services Bureau à distance de Windows Server 2012 R2, mais vous souhaitez effectuer une mise à niveau vers Windows Server 2016 pour bénéficier des nouvelles fonctionnalités (par exemple la prise en charge d’OpenGL\OpenCL, l’affectation d’appareils en mode discret ou les espaces de stockage direct). La question devient celle-ci : quels sont les composants des services Bureau à distance qui peuvent fonctionner avec différentes versions et lesquels doivent être identiques ?

Dans cette optique, voici les instructions de base relatives aux configurations prises en charge par les services Bureau à distance dans Windows Server.

> [!NOTE]
> Veillez à consulter la [configuration système requise pour Windows Server 2016](../../get-started/system-requirements.md) et la [configuration système requise pour Windows Server 2019](../../get-started-19/sys-reqs-19.md).

## <a name="best-practices"></a>Bonnes pratiques

- Utilisez Windows Server 2019 pour votre infrastructure de Bureau à distance (serveur d’accès web, serveur de passerelle, serveur du service Broker pour les connexions et serveur de licences). Windows Server 2019 offre une compatibilité descendante avec ces composants, ce qui signifie qu’un hôte de session Bureau à distance Windows Server 2016 ou Windows Server 2012 R2 peut se connecter à un service Broker pour les connexions Bureau à distance 2019, mais pas l’inverse.

- Pour les hôtes de session Bureau à distance, tous les hôtes de session d’une collection doivent être au même niveau. Toutefois, vous pouvez avoir plusieurs collections. Vous pouvez avoir une collection avec des hôtes de session Windows Server 2016 et une collection avec des hôtes de session Windows Server 2019.

- Si vous mettez à niveau votre hôte de session Bureau à distance vers Windows Server 2019, mettez également à niveau le serveur de licences. N’oubliez pas qu’un serveur de licences 2019 peut traiter les licences d’accès client de toutes les versions précédentes de Windows Server, jusqu’à Windows Server 2003.

- Suivez l’ordre de mise à niveau recommandé dans [Mise à niveau de votre environnement des services Bureau à distance](upgrade-to-rds.md#flow-for-deployment-upgrades).

- Si vous créez un environnement hautement disponible, tous vos serveurs du service Broker pour les connexions doivent être au même niveau d’OS (système d’exploitation).

## <a name="rd-connection-brokers"></a>Serveurs du service Broker pour les connexions Bureau à distance

Windows Server 2016 supprime la restriction relative au nombre de serveurs du service Broker pour les connexions disponibles dans un déploiement quand vous utilisez des hôtes de session Bureau à distance et des serveurs hôtes de virtualisation des services Bureau à distance qui exécutent également Windows Server 2016. Le tableau suivant indique les versions des composants des services Bureau à distance compatibles avec les versions 2016 et 2012 R2 du service Broker pour les connexions dans un déploiement hautement disponible avec au moins trois serveurs du service Broker pour les connexions.

|Plus de 3 serveurs du service Broker pour les connexions en haute disponibilité|hôtes de session Bureau à distance et serveurs hôtes de virtualisation des services Bureau à distance 2019|hôtes de session Bureau à distance et serveurs hôtes de virtualisation des services Bureau à distance 2016|hôtes de session Bureau à distance et serveurs hôtes de virtualisation des services Bureau à distance 2012 R2|
|---|---|---|---|
 |Service Broker pour les connexions Windows Server 2019|Pris en charge|Pris en charge|Pris en charge|
 |Service Broker pour les connexions Windows Server 2016|NON APPLICABLE|Pris en charge|Pris en charge|
 |Service Broker pour les connexions Windows Server 2012 R2|NON APPLICABLE|NON APPLICABLE|Non pris en charge|

## <a name="support-for-graphics-processing-unit-gpu-acceleration"></a>Prise en charge de l’accélération de l’unité de traitement graphique (GPU)

Les services Bureau à distance prennent en charge les systèmes équipés de GPU. Les applications nécessitant un GPU peuvent être utilisées via la connexion à distance. En outre, l’encodage et le rendu accélérés par GPU peuvent être activés pour améliorer les performances et la scabilité des applications.

Les hôtes de session des services Bureau à distance et les systèmes d’exploitation clients monosession peuvent tirer parti du GPU physique ou virtuel présenté au système d’exploitation de nombreuses façons, notamment au travers des [tailles d’ordinateur virtuel à GPU optimisé Azure](/en-us/azure/virtual-machines/windows/sizes-gpu), des GPU disponibles pour le serveur RDSH physique, des vGPU RemoteFX (uniquement sur Windows Server 2016) et des GPU présentés aux ordinateurs virtuel par des hyperviseurs pris en charge.

Consultez l’article [Quelle technologie de virtualisation graphique vous convient ?](rds-graphics-virtualization.md) pour déterminer ce dont vous avez besoin. Pour plus d’informations sur l’affectation d’appareils en mode discret, consultez [Planification du déploiement de l’affectation d’appareils en mode discret](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

Les fournisseurs de GPU peuvant avoir un schéma de licence distinct pour les scénarios d’hôte de session Bureau à distance ou limiter l’utilisation des GPU sur le système d’exploitation serveur, vérifiez les exigences auprès de votre fournisseur favori.

Les GPU présentés par une plateforme hyperviseur non-Microsoft ou cloud doivent avoir des pilotes signés numériquement par WHQL et fournis par le fournisseur de GPU.

### <a name="remote-desktop-session-host-support-for-gpus"></a>Prise en charge de l’hôte de session Bureau à distance pour les GPU

Le tableau suivant présente les scénarios pris en charge par les différentes versions des hôtes de session Bureau à distance.

|Fonctionnalité|Windows Server 2008 R2|Windows Server 2012 R2|Windows Server 2016|Windows Server 2019|
|---|---|---|---|---|
|Utilisation du GPU matériel pour toutes les sessions RDP|Non|Oui|Oui|Oui|
|Encodage matériel H.264/AVC (si pris en charge par le GPU)|Non|Non|Oui|Oui|
|Équilibrage de charge entre plusieurs GPU présentés au système d’exploitation|Non|Non|Non|Oui|
|Optimisations de l’encodage H.264/AVC pour réduire l’utilisation de la bande passante|Non|Non|Non|Oui|
|Prise en charge de H.264/AVC pour la résolution 4K|Non|Non|Non|Oui|

### <a name="vdi-support-for-gpus"></a>Prise en charge de VDI pour les GPU

Le tableau suivant indique la prise en charge des scénarios de GPU dans le système d’exploitation client.

|Fonctionnalité|Windows 7 SP1|Windows 8.1|Windows 10|
|---|---|---|---|
|Utilisation du GPU matériel pour toutes les sessions RDP|Non|Oui|Oui|
|Encodage matériel H.264/AVC (si pris en charge par le GPU)|Non|Non|Windows 10 1703 et versions ultérieures|
|Équilibrage de charge entre plusieurs GPU présentés au système d’exploitation|Non|Non|Windows 10 1803 et versions ultérieures|
|Optimisations de l’encodage H.264/AVC pour réduire l’utilisation de la bande passante|Non|Non|Windows 10 1803 et versions ultérieures|
|Prise en charge de H.264/AVC pour la résolution 4K|Non|Non|Windows 10 1803 et versions ultérieures|

### <a name="remotefx-3d-video-adapter-vgpu-support"></a>Prise en charge de la carte vidéo 3D RemoteFX (vGPU)

Les services Bureau à distance prennent en charge les vGPU RemoteFX quand la machine virtuelle s’exécute en tant qu’invité Hyper-V sur Windows Server 2012 R2 ou Windows Server 2016. Les systèmes d’exploitation invités suivants prennent en charge les vGPU RemoteFX :

- Windows 7 SP1
- Windows 8.1
- Windows 10 1703 ou version ultérieure
- Windows Server 2016 dans un déploiement mono-session uniquement
- Windows Server 2019 dans un déploiement mono-session uniquement

### <a name="discrete-device-assignment-support"></a>Prise en charge de l’attribution d’appareils en mode discret

Les services Bureau à distance prennent en charge les GPU physiques présentés avec l’attribution d’appareils en mode discret à partir d’hôtes Hyper-V Windows Server 2016 ou Windows Server 2019. Pour plus d’informations, consultez [Planifier le déploiement de l’attribution d’appareils en mode discret](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).


## <a name="vdi-deployment--supported-guest-oses"></a>Déploiement VDI - OS invités pris en charge

Les serveurs hôtes de virtualisation des services Bureau à distance de Windows Server 2016 et Windows Server 2019 prennent en charge les OS (systèmes d’exploitation) invités suivants :

- Windows 10 Entreprise
- Windows 8.1 Enterprise
- Windows 7 SP1 Entreprise

> [!NOTE]
> - Les services Bureau à distance ne prennent pas en charge les collections de sessions hétérogènes. Les systèmes d’exploitation de toutes les machines virtuelles d’une collection doivent avoir la même version.
> - Vous pouvez avoir des collections homogènes distinctes avec différentes versions d’OS invité sur le même hôte.
> - L’hôte Hyper-V utilisé pour exécuter des machines virtuelles doit avoir la même version que l’hôte Hyper-V utilisé pour créer les modèles de machine virtuelle d’origine.

## <a name="single-sign-on"></a>Authentification unique

Les services Bureau à distance de Windows Server 2016 et Windows Server 2019 prennent en charge deux expériences SSO principales :

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
