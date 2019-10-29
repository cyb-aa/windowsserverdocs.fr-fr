---
title: Fonctionnalités supprimées ou dont la suppression est prévue dans Windows Server 2019
description: Découvrez les fonctions et fonctionnalités supprimées ou dont la suppression est prévue à compter de Windows Server 2019.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: jasongerend
ms.author: jgerend
manager: jasgro
ms.date: 10/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: a2d3a871165812ac3a27e65b5f52cc56a05c9efe
ms.sourcegitcommit: 3262c5c7cece9f2adf2b56f06b7ead38754a451c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812294"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Fonctionnalités supprimées ou dont le remplacement est prévu à compter de Windows Server 2019

>S’applique à : Windows Server 2019

Chaque version de Windows Server ajoute de nouvelles fonctions et fonctionnalités ; nous en supprimons aussi de temps en temps, généralement suite à l’ajout d’une meilleure option. Voici les détails des fonctions et fonctionnalités que nous avons supprimées de Windows Server 2019.

> [!TIP]
> - Pour obtenir un accès en avant-première aux builds Windows Server, rejoignez le [programme Windows Insider](https://insider.windows.com) : c’est un excellent moyen de tester les changements apportés aux fonctionnalités.
> - Vous avez des questions sur d’autres versions ? Consultez [Fonctionnalités supprimées ou dont la suppression est prévue dans Windows Server](removed-features.md).

**Cette liste est susceptible d’être modifiée et risque de ne pas inclure toutes les fonctions ou fonctionnalités concernées.** 

## <a name="features-we-removed-in-this-release"></a>Fonctionnalités que nous avons supprimées de cette version

Nous supprimons les fonctions et fonctionnalités suivantes de l’image de produit installée dans Windows Server 2019. Les applications ou le code qui dépendent de ces fonctionnalités ne fonctionnent pas dans cette version, sauf si vous utilisez une autre méthode.

| Fonctionnalité   | À la place, vous pouvez utiliser... |
| --------- | -------------------- |
| Business Scanning, également appelée Gestion de la numérisation distribuée (DSM)|Nous supprimons cette fonctionnalité d’analyse sécurisée et de gestion de scanneur : aucun appareil ne prend en charge cette fonctionnalité. |
| Composants d’impression : composant devenu facultatif pour les installations Server Core|Dans les versions précédentes de Windows Server, les composants d’impression étaient *désactivés* par défaut dans l’option d’installation Server Core. Nous avons changé cela dans Windows Server 2016, en les activant par défaut. Dans Windows Server 2019, ces composants d’impression sont à nouveau désactivés par défaut pour Server Core. Si vous devez les activer, vous pouvez le faire en exécutant l’applet de commande **Install-WindowsFeature Print-Server**. |
| [Service Broker pour les connexions Bureau à distance et Serveur hôte de virtualisation des services Bureau à distance](../remote/remote-desktop-services/desktop-hosting-service.md) sur une installation Server Core|La plupart des déploiements des Services Bureau à distance ont ces rôles colocalisés avec l’Hôte de session Bureau à distance, ce qui nécessite un serveur avec Expérience utilisateur. Par souci de cohérence avec la fonctionnalité Serveur hôte de virtualisation des services Bureau à distance, nous changeons ces rôles pour qu’ils exigent également un serveur avec Expérience utilisateur. Ces rôles des services Bureau à distance ne peuvent plus être utilisés dans une [installation Server Core](../administration/server-core/what-is-server-core.md). Si vous avez besoin de [déployer ces rôles dans le cadre de votre infrastructure de Bureau à distance](../remote/remote-desktop-services/rds-deploy-infrastructure.md), vous pouvez [les installer sur Windows Server avec Expérience utilisateur](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Ces rôles sont également inclus dans l’option d’installation Expérience utilisateur de Windows Server 2019. |
| [Carte vidéo 3D RemoteFX (vGPU)](../remote/remote-desktop-services/rds-remotefx-vgpu.md)|Nous mettons au point de nouvelles options d’accélération graphique pour les environnements virtualisés. Vous pouvez également utiliser [Discrete Device Assignment (DDA)](../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md) en guise d’alternative. |

## <a name="features-were-no-longer-developing"></a>Fonctionnalités que nous ne développons plus

Nous ne développons plus activement les fonctionnalités suivantes et il est possible que nous les supprimions dans une mise à jour ultérieure. Certaines fonctionnalités ont été remplacées par d’autres fonctions ou fonctionnalités, tandis que d’autres sont désormais disponibles à partir d’autres sources. 

Si vous avez des commentaires à propos de la proposition de suppression de certaines de ces fonctionnalités, vous pouvez utiliser l’[application Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

| Fonctionnalité     | À la place, vous pouvez utiliser... |
| ----------- | --------------------- |
| Lecteur de stockage de clé dans Hyper-V|Nous ne travaillons plus sur la fonctionnalité de lecteur de stockage de clé dans Hyper-V. Si vous utilisez des machines virtuelles de génération 1, consultez [Sécurité de la virtualisation des machines virtuelles de génération 1](../virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v.md) pour plus d’informations sur les options disponibles à l’avenir. Si vous créez des machines virtuelles, utilisez des machines virtuelles de génération 2 avec les appareils TPM pour une solution plus sécurisée. |
| Console de gestion de module de plateforme sécurisée (TPM)|Les informations qui étaient disponibles dans la console de gestion du module TPM sont maintenant disponibles dans la page [**Sécurité des appareils**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) du [Centre de sécurité Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center). |
| Mode d’attestation Active Directory du Service Guardian hôte|Nous ne développons plus le mode d’attestation Active Directory du Service Guardian hôte. À la place, nous avons ajouté un nouveau mode d’attestation, l’[attestation de clé d’hôte](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), qui est beaucoup plus simple et tout aussi compatible que l’attestation basée sur Active Directory.  Ce nouveau mode fournit des fonctionnalités équivalentes avec une expérience de configuration, une gestion simplifiée et moins de dépendances d’infrastructure que l’attestation Active Directory. L’attestation de clé d’hôte n’a aucune exigence matérielle supplémentaire au-delà de ce qu’exigeait Active Directory. Tous les systèmes existants restent donc compatibles avec le nouveau mode. Pour plus d’informations sur vos options d’attestation, consultez [Déployer des hôtes protégés](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md). |
| Service OneSync | Le service OneSync synchronise les données pour les applications Courrier, Calendrier et Contacts. Nous avons ajouté à l’application Outlook un moteur de synchronisation qui fournit la même synchronisation. |
| Prise en charge de l’API RDC (Remote Differential Compression) | La prise en charge de l’API RDC (Remote Differential Compression) permettait de synchroniser des données avec une source distante à l’aide de technologies de compression, ce qui réduisait la quantité de données envoyées sur le réseau. |
| Extension de commutateur de filtre léger WFP | L’extension de commutateur de filtre léger WFP permet aux développeurs de générer des [extensions de filtrage de paquets réseau simples pour le commutateur virtuel Hyper-V](https://docs.microsoft.com/windows-hardware/drivers/network/using-virtual-switch-filtering). Vous pouvez obtenir les mêmes fonctionnalités en créant une extension de filtrage complète. Par conséquent, nous allons supprimer cette extension. |
