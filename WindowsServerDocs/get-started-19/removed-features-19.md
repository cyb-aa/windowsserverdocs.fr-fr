---
title: Fonctionnalités supprimées ou vont dans Windows Server 2019
description: Découvrez les fonctionnalités supprimées ou vont en commençant par Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: jasgro
ms.date: 10/30/2018
ms.localizationpriority: medium
ms.openlocfilehash: e00185c2a55f12d211ada4639337e66e5d70aa40
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817370"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Fonctionnalités supprimées ou planifié pour le remplacement à partir de Windows Server 2019

>S'applique à : Windows Server 2019

Chaque version de Windows Server ajoute de nouvelles fonctions et fonctionnalités ; nous en supprimons aussi occasionnellement, généralement suite à l’ajout d’une meilleure option. Voici les détails sur les fonctionnalités que nous avons supprimé dans Windows Server 2019.   

> [!TIP]
> - Pour obtenir un accès en avant-première aux versions de Windows Server, rejoignez le [programme Windows Insider](https://insider.windows.com) : c'est un excellent moyen de tester les modifications apportées aux fonctionnalités.
> - Vous avez des questions sur les autres versions ? Consultez les informations pour [Windows Server, version 1803](../get-started/windows-server-1803-removed-features.md), [Windows Server, version 1709](../get-started/removed-features-1709.md), et [Windows Server 2016](../get-started/deprecated-features.md).

**La liste est susceptible d’être modifiée et peut ne pas inclure chaque fonctionnalité concernée ou la fonctionnalité.** 

## <a name="features-we-removed-in-this-release"></a>Fonctionnalités que nous avons supprimées de cette version

Nous allons supprimer les fonctionnalités suivantes à partir de l’image de produit installé dans Windows Server 2019. Les applications ou le code qui dépendent de ces fonctionnalités ne fonctionnent pas dans cette version, sauf si vous utilisez une autre méthode.   

|Fonctionnalité    |À la place, vous pouvez utiliser...|
|-----------|--------------------
|Business Scanning, également appelé Gestion de la numérisation distribuée (DSM)|Nous allons supprimer cette analyse sécurisée et la capacité de gestion de scanneur : il n’existe aucun appareil qui prennent en charge cette fonctionnalité.|
|Service iSNS (Internet Storage Name Service)|Le protocole iSNS est utilisé pour l’interaction entre les clients et les serveurs iSNS. [Server Message Block](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795\(v=ws.11\)) fournit essentiellement les mêmes fonctionnalités, ainsi que d’autres fonctionnalités.|
|Imprimer les composants - composant désormais facultatif pour les installations de Server Core|Dans les versions précédentes de Windows Server, les composants d’impression ont été *désactivé* par défaut dans l’option d’installation Server Core. Nous avons modifié que dans Windows Server 2016, ce qui leur permet par défaut. Dans Windows Server 2019, ces composants d’impression sont là aussi désactivés par défaut pour Server Core. Si vous devez activer les composants d’impression, vous pouvez le faire en exécutant la **Install-WindowsFeature-serveur d’impression** applet de commande.|
|[Service Broker pour les connexions Bureau à distance et Serveur hôte de virtualisation des services Bureau à distance](../remote/remote-desktop-services/desktop-hosting-service.md) sur une installation minimale|La plupart des déploiements des Services Bureau à distance ont ces rôles colocalisés avec le Serveur hôte de virtualisation des services Bureau à distance, ce qui nécessite un serveur avec Expérience utilisateur. Par souci de cohérence avec Serveur hôte de virtualisation des services Bureau à distance, nous changeons ces rôles pour qu'ils nécessitent également un serveur avec Expérience utilisateur. Ces rôles des services Bureau à distance ne sont plus disponibles pour une utilisation dans un [installation Server Core](../administration/server-core/what-is-server-core.md). Si vous avez besoin pour [déployer ces rôles dans le cadre de votre infrastructure de bureau à distance](../remote/remote-desktop-services/rds-deploy-infrastructure.md), vous pouvez [les installer sur Windows Server avec expérience](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Ces rôles sont également inclus dans l’option d’installation Expérience utilisateur de Windows Server 2019. |



## <a name="features-were-no-longer-developing"></a>Fonctionnalités que nous ne développons plus

Nous vous développez n’est plus activement ces fonctionnalités et peut les supprimer à partir d’une prochaine mise à jour. Certaines fonctionnalités ont été remplacées par d'autres fonctions ou fonctionnalités, tandis que d’autres sont désormais disponibles à partir d'autres sources. 

Si vous avez des commentaires à propos de la proposition de suppression de certaines de ces fonctionnalités, vous pouvez utiliser l'[application Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

|Fonctionnalité    |À la place, vous pouvez utiliser...|
|-----------|---------------------|
|Lecteur de stockage de clés dans Hyper-V|Nous travaillons n’est plus sur la fonctionnalité lecteur de stockage de clé dans Hyper-V. Si vous utilisez des machines virtuelles de génération 1, consultez [la sécurité de la virtualisation de génération de machine virtuelle 1](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) pour plus d’informations sur les options à l’avenir. Si vous créez des machines virtuelles utilisent machines virtuelles de génération 2 avec les périphériques de module de plateforme sécurisée pour une solution plus sécurisée. |
|Console de gestion de Module de plateforme approuvée|Les informations précédemment disponibles dans la console de gestion du module de plateforme sécurisée sont désormais disponibles sur le [ **sécurité des appareils** ](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) page dans le [Windows Defender Security Center](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center).|
|Mode d’attestation hôte Service Guardian Active Directory|Nous développons ne sont plus du mode d’attestation de répertoire Active du Service Guardian hôte : au lieu de cela, nous avons ajouté un nouveau mode d’attestation, [héberger l’attestation de clé](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), qui est beaucoup plus simple et tout aussi aussi compatible que Active Directory en attestation.  Ce nouveau mode fournit des fonctionnalités équivalentes avec une expérience d’installation, une gestion plus simple et moins dépendances d’infrastructure que l’attestation Active Directory. L’attestation de clé hôte n’a aucune autre configuration matérielle requise au-delà de quelles d’attestation d’Active Directory requis, donc tous les systèmes existants restent compatibles avec le nouveau mode. Consultez [déployer des hôtes service Guardian](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) pour plus d’informations sur les options d’attestation.|
|Service de OneSync|Le service OneSync synchronise les données pour les applications de messagerie, le calendrier et personnes. Nous avons ajouté un moteur de synchronisation à l’application Outlook qui fournit la synchronisation même.|
|Prise en charge des API de la Compression différentielle à distance|Prise en charge des API de la Compression différentielle à distance activé la synchronisation des données avec une source distante à l’aide de technologies de compression qui réduit la quantité de données envoyées sur le réseau. Cette prise en charge n’est pas actuellement utilisé par tous les produits Microsoft.|
|Extension de commutateur WFP filtre léger|L’extension de commutateur de filtre léger WFP permet aux développeurs de créer [extensions du commutateur virtuel Hyper-V de filtrage des paquets de réseau simple](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). Vous pouvez obtenir les mêmes fonctionnalités en créant une extension de filtrage complète. Par conséquent, nous allons supprimer cette extension à l’avenir.|

