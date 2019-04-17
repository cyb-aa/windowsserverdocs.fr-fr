---
title: Fonctionnalités supprimées ou dont la suppression dans Windows Server 2019
description: Apprenez-en plus sur les fonctions et fonctionnalités supprimées ou dont la suppression à partir de Windows Server 2019.
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
ms.date: 03/29/2019
ms.localizationpriority: medium
ms.openlocfilehash: 470857616a9b36d238de031b4ccf80a68eff1e61
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279130"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Fonctionnalités supprimées ou dont le remplacement est prévu à partir de Windows Server 2019

>S'applique à: WindowsServer2019

Chaque version de WindowsServer ajoute de nouvelles fonctions et fonctionnalités; nous en supprimons aussi occasionnellement, généralement suite à l’ajout d’une meilleure option. Voici les détails sur les fonctions et fonctionnalités que nous avons supprimées de Windows Server 2019.   

> [!TIP]
> - Pour obtenir un accès en avant-première aux versions de Windows Server, rejoignez le [programme Windows Insider](https://insider.windows.com): c'est un excellent moyen de tester les modifications apportées aux fonctionnalités.
> - Vous avez des questions sur les autres versions? Consultez les informations relatives à [Windows Server, version 1803](../get-started/windows-server-1803-removed-features.md), [Windows Server, version 1709](../get-started/removed-features-1709.md)et [Windows Server 2016](../get-started/deprecated-features.md).

**Cette liste peut faire l’objet de modifications et peut ne pas inclure toutes les fonctions ou fonctionnalités concernées.** 

## <a name="features-we-removed-in-this-release"></a>Fonctionnalités que nous avons supprimées de cette version

Nous mettons supprimer les fonctions et fonctionnalités suivantes de l’image de produit installée dans Windows Server 2019. Les applications ou le code qui dépendent de ces fonctionnalités ne fonctionnent pas dans cette version, sauf si vous utilisez une autre méthode.   

|Fonctionnalité    |À la place, vous pouvez utiliser...|
|-----------|--------------------
|Business Scanning, également appelé Gestion de la numérisation distribuée (DSM)|Nous allons supprimer cette sécuriser la numérisation et la fonctionnalité de gestion de scanneur: aucun appareil prenant en charge cette fonctionnalité.|
|Imprimer les composants - composant désormais facultatif pour les installations Server Core|Dans les versions précédentes de Windows Server, les composants d’impression ont été *désactivé* par défaut dans l’option d’installation Server Core. Nous avons modifié que dans Windows Server 2016, ce qui leur permet par défaut. Dans Windows Server 2019, ces composants d’impression sont là encore désactivées par défaut à Server Core. Si vous devez activer les composants d’impression, vous pouvez le faire en exécutant l’applet de commande **Install-WindowsFeature-serveur d’impression** .|
|[Service Broker pour les connexions Bureau à distance et Serveur hôte de virtualisation des services Bureau à distance](../remote/remote-desktop-services/desktop-hosting-service.md) sur une installation minimale|La plupart des déploiements des Services Bureau à distance ont ces rôles colocalisés avec le Serveur hôte de virtualisation des services Bureau à distance, ce qui nécessite un serveur avec Expérience utilisateur. Par souci de cohérence avec Serveur hôte de virtualisation des services Bureau à distance, nous changeons ces rôles pour qu'ils nécessitent également un serveur avec Expérience utilisateur. Ces rôles de services Bureau à distance ne sont plus disponibles pour une utilisation dans une [installation minimale](../administration/server-core/what-is-server-core.md). Si vous avez besoin pour [déployer ces rôles dans le cadre de votre infrastructure de bureau à distance](../remote/remote-desktop-services/rds-deploy-infrastructure.md), vous pouvez [les installer sur Windows Server avec expérience utilisateur](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Ces rôles sont également inclus dans l’option d’installation Expérience utilisateur de Windows Server2019. |



## <a name="features-were-no-longer-developing"></a>Fonctionnalités que nous ne développons plus

Nous développez n’est plus activement ces fonctionnalités et peuvent les supprimions dans une prochaine mise à jour. Certaines fonctionnalités ont été remplacées par d'autres fonctions ou fonctionnalités, tandis que d’autres sont désormais disponibles à partir d'autres sources. 

Si vous avez des commentaires à propos de la proposition de suppression de certaines de ces fonctionnalités, vous pouvez utiliser l'[application Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

|Fonctionnalité    |À la place, vous pouvez utiliser...|
|-----------|---------------------|
|Disque de stockage de clés dans Hyper-V|Nous travaillons n’est plus sur la fonctionnalité de disque de stockage de clé dans Hyper-V. Si vous utilisez des machines virtuelles de génération 1, consultez [La sécurité de machine virtuelle de la virtualisation de génération 1](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) pour plus d’informations sur les options à l’avenir. Si vous créez nouveaux ordinateurs virtuels utilisent des machines virtuelles de génération 2 avec les périphériques de module de plateforme sécurisée pour une solution plus sécurisée. |
|Console de gestion approuvé Platform Module (TPM)|Les informations précédemment disponibles dans la console de gestion du module de plateforme sécurisée sont désormais disponibles sur la page de [**sécurité de l’appareil**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) dans le [Centre de sécurité Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center).|
|Mode d’attestation Host Guardian Service Active Directory|Nous ne développons plus le mode d’attestation Host Guardian Service Active Directory: au lieu de cela, nous avons ajouté un nouveau mode d’attestation, l' [attestation de clé d’hôte](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), qui est beaucoup plus simple et tout aussi compatible Active Directory en fonction d’attestation.  Ce nouveau mode fournit des fonctionnalités équivalentes avec une expérience d’installation, une gestion simplifiée et les dépendances d’infrastructure moins que l’attestation Active Directory. Attestation de clé d’hôte n’a aucune configuration matérielle supplémentaire requise au-delà de quel attestation Active Directory nécessaire, afin que tous les systèmes existants restera compatibles avec le nouveau mode. Pour plus d’informations sur les options de l’attestation, voir [déployer surveillés hôtes](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) .|
|Service OneSync|Le service OneSync synchronise les données pour les applications de messagerie, calendrier et contacts. Nous avons ajouté un moteur de synchronisation à l’application Outlook qui fournit la même synchronisation.|
|Prise en charge des API de Compression différentielle à distance|Prise en charge des API de Compression différentielle à distance activé synchroniser des données avec une source distante à l’aide de technologies de compression, ce qui réduit la quantité de données envoyées sur le réseau. Cette prise en charge n’est pas actuellement utilisé par tous les produits Microsoft.|
|Extension de commutateur Protection filtre léger|L’extension de commutateur de filtre léger protection permet aux développeurs de créer des [extensions de filtrage de paquet réseau simple pour le virtuel Hyper-V basculer](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). Vous pouvez obtenir les mêmes fonctionnalités en créant une extension de filtrage complet. Par conséquent, nous supprimerons cette extension à l’avenir.|

