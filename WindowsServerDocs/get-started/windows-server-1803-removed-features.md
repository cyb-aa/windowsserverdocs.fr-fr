---
title: Windows Server, version 1803 - Fonctionnalités qui ont été supprimées
description: Découvrez les fonctionnalités qui seront supprimées ou dépréciées dans Windows Server, version 1803 ou une version ultérieure
ms.prod: windows-server-threshold
ms.mktglfcycl: plan
ms.localizationpriority: medium
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.date: 05/10/2018
ms.openlocfilehash: c80738fe7ceda43a1a73adb0a8b1061bbb24319f
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63684523"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1803"></a>Fonctionnalités supprimées ou dont le remplacement est prévu à compter de Windows Server, version 1803

> S’applique à : Windows Server, version 1803

Chaque version de Windows Server ajoute de nouvelles fonctions et fonctionnalités ; nous en supprimons aussi de temps en temps, généralement suite à l’ajout d’une meilleure option. Voici les détails des fonctions et fonctionnalités que nous avons supprimées de Windows Server, version 1803.   

> [!TIP]
> - Pour obtenir un accès en avant-première aux builds Windows Server, rejoignez le [programme Windows Insider](https://insider.windows.com) : c’est un excellent moyen de tester les changements apportés aux fonctionnalités.
> - Vous avez des questions sur d’autres versions ? Consultez les informations sur [Windows Server 2016](deprecated-features.md) et [Windows Server, version 1709](removed-features-1709.md).

**Cette liste est susceptible d’être modifiée et risque de ne pas inclure toutes les fonctions ou fonctionnalités concernées.** 

## <a name="features-we-removed-in-this-release"></a>Fonctionnalités que nous avons supprimées de cette version

Nous avons supprimé les fonctions et fonctionnalités suivantes de l’image de produit installée dans Windows Server, version 1803. Les applications ou le code qui dépendent de ces fonctionnalités ne fonctionnent pas dans cette version, sauf si vous utilisez une autre méthode.   

|Fonctionnalité    |À la place, vous pouvez utiliser...|
|-----------|--------------------|
|[Service de réplication de fichiers](https://support.microsoft.com/en-us/help/4025991/windows-server-version-1709-no-longer-supports-frs)|Les Services de réplication de fichiers (FRS), introduits dans Windows Server 2003 R2, ont été remplacés par la réplication DFS. Vous devez [faire migrer tous les contrôleurs de domaine qui utilisent FRS vers la réplication DFS à l’aide de SYSVOL](https://blogs.technet.microsoft.com/filecab/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol/).|
|Virtualisation de réseau Hyper-V|La [Virtualisation de réseau](../networking/sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md) est désormais incluse dans Windows Server dans le cadre de la solution [SDN (Software Defined Networking)](../networking/sdn/software-defined-networking.md), qui inclut également le Contrôleur de réseau, l’équilibrage de charge logiciel, le routage défini par l’utilisateur et les listes de contrôle d’accès.|

## <a name="features-were-no-longer-developing"></a>Fonctionnalités que nous ne développons plus

Nous ne développons plus activement les fonctionnalités suivantes et il est possible que nous les supprimions dans une mise à jour ultérieure. Certaines fonctionnalités ont été remplacées par d’autres fonctions ou fonctionnalités, tandis que d’autres sont désormais disponibles à partir d’autres sources. 

>[!NOTE]
> Veuillez noter que certaines des fonctionnalités et des rôles décrits ci-dessous ne sont pas inclus dans l’option d’installation Server Core, qui est fournie dans Windows Server, version 1803. Ils *sont* présents dans le serveur avec l’option d’installation Expérience utilisateur, qui a été publiée pour la dernière fois avec Windows Server 2016 et sera publiée à nouveau dans Windows Server 2019.

Si vous avez des commentaires à propos de la proposition de suppression de certaines de ces fonctionnalités, vous pouvez utiliser l’[application Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

|Fonctionnalité ou rôle    |À la place, vous pouvez utiliser...|
|-----------|---------------------|
|Business Scanning, également appelée Gestion de la numérisation distribuée (DSM)|La [fonctionnalité Gestion de la numérisation](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759124\(v%3dws.11\)) a été introduite dans Windows Server 2008 R2. Elle permettait de sécuriser la numérisation et de gérer les scanneurs dans une entreprise. Nous arrêtons les investissements dans cette fonctionnalité, et plus aucun appareil disponible ne la prend en charge.|
|Technologies de transition IPv4/6 (6to4, ISATAP et Direct Tunnels)|6to4 est désactivé par défaut depuis Windows 10, version 1607 (mise à jour anniversaire), ISATAP est désactivé par défaut depuis Windows 10, version 1703 (Creators Update), et Direct Tunnels a toujours été désactivé par défaut. Utilisez à la place la prise en charge IPv6 native.|
|[MultiPoint Services](../remote/multipoint-services/multipoint-services.md)|Nous ne développons plus le rôle MultiPoint Services dans le cadre de Windows Server. Des services de connecteur MultiPoint sont disponibles par le biais de [Fonctionnalité à la demande](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) pour Windows Server et Windows 10. Vous pouvez utiliser les [Services Bureau à distance](../remote/remote-desktop-services/welcome-to-rds.md), en particulier l’hôte de session des services Bureau à distance, pour fournir une connectivité RDP. |
|[Packages de symboles hors ligne](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugger-download-symbols) (MSI de symboles de débogage)|Nous ne mettons plus à disposition de packages de symboles sous forme de MSI téléchargeables. À la place, le [serveur de symboles Microsoft évolue vers un magasin de symboles basé sur Azure](https://blogs.msdn.microsoft.com/windbg/2017/10/18/update-on-microsofts-symbol-server/). Si vous avez besoin de symboles Windows, connectez-vous au serveur de symboles Microsoft pour mettre en cache vos symboles localement ou utilisez un fichier de manifeste avec SymChk.exe sur un ordinateur ayant accès à Internet.|
|[Service Broker pour les connexions Bureau à distance et Serveur hôte de virtualisation des services Bureau à distance](../remote/remote-desktop-services/desktop-hosting-service.md) sur une installation Server Core|La plupart des déploiements des Services Bureau à distance ont ces rôles colocalisés avec l’Hôte de session Bureau à distance, ce qui nécessite un serveur avec Expérience utilisateur. Par souci de cohérence avec la fonctionnalité Serveur hôte de virtualisation des services Bureau à distance, nous changeons ces rôles pour qu’ils exigent également un serveur avec Expérience utilisateur. Nous ne développons plus ces rôles de Services Bureau à distance pour l’utilisation dans une [installation Server Core](../administration/server-core/what-is-server-core.md). Si vous avez besoin de [déployer ces rôles dans le cadre de votre infrastructure de Bureau à distance](../remote/remote-desktop-services/rds-deploy-infrastructure.md), vous pouvez [les installer sur Windows Server 2016 avec l’Expérience utilisateur](getting-started-with-server-with-desktop-experience.md). <br/><br/>Ces rôles sont également inclus dans l’option d’installation Expérience utilisateur de Windows Server 2019. Vous pouvez les tester dans la [build Windows Insider de Windows Server 2019](https://docs.microsoft.com/windows-insider/at-work/). Veillez simplement à choisir l’image du Canal de maintenance à long terme (LTSC). |
|[RemoteFX vGPU](../remote/remote-desktop-services/rds-remotefx-vgpu.md)|Nous mettons au point de nouvelles options d’accélération graphique pour les environnements virtualisés. Vous pouvez également utiliser [Discrete Device Assignment (DDA)](../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md) en guise d’alternative.|
|[Stratégies de restriction logicielle](../identity/software-restriction-policies/software-restriction-policies.md) dans la stratégie de groupe|Au lieu d’utiliser des stratégies de restriction logicielle par le biais de la stratégie de groupe, vous pouvez utiliser [AppLocker](https://docs.microsoft.com/windows/security/threat-protection/applocker/applocker-overview) ou le [Contrôle d’application Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control) pour contrôler les applications auxquelles les utilisateurs peuvent accéder et quel code peut s’exécuter dans le noyau.|
|Espaces de stockage dans une configuration partagée à l’aide d’une infrastructure de signature d’accès partagé|Déployez les [espaces de stockage direct](../storage/storage-spaces/storage-spaces-direct-overview.md) à la place. Les espaces de stockage direct prennent en charge l’utilisation de pièces jointes SAS HLK certifiées, mais dans une configuration non partagée, comme décrit dans [Configuration matérielle requise pour les espaces de stockage direct](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).|
|Expérience Windows Server Essentials|Nous ne développons plus le rôle Expérience Essentials pour les références Windows Server Standard ou Windows Server Datacenter. Si vous avez besoin d’une solution de serveur simple à utiliser pour de petites et moyennes entreprises, consultez notre nouvelle solution [Microsoft 365 Business](https://www.microsoft.com/microsoft-365/business) ou utilisez [Windows Server 2016 Essentials](https://docs.microsoft.com/windows-server-essentials/get-started/get-started).|

