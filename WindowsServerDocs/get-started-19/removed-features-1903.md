---
title: Fonctionnalités supprimées ou planifié pour le remplacement à compter de Windows Server, version 1903
description: Voici une liste des fonctionnalités et des fonctionnalités dans Windows Server, version 1903 qui ont été supprimées du produit dans cette version ou commencent à être pris en compte pour un remplacement potentiel dans les versions ultérieures. Ce produit s’adresse aux professionnels de l’informatique qui mettent à jour des systèmes d’exploitation dans un environnement commercial.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.date: 05/21/2019
author: jasongerend
ms.author: jgerend
manager: daveba
ms.openlocfilehash: e2f51af55ba7005cb20d8a1c22f6ba9edc20c704
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983419"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1903"></a>Fonctionnalités supprimées ou planifié pour le remplacement à compter de Windows Server, version 1903

>S'applique à : Windows Server, version 1903

Voici une liste des fonctionnalités et des fonctionnalités dans Windows Server, version 1903 qui ont été supprimées du produit dans cette version ou commencent à être pris en compte pour un remplacement potentiel dans les versions ultérieures. Ce produit s’adresse aux professionnels de l’informatique qui mettent à jour des systèmes d’exploitation dans un environnement commercial. **Cette liste est susceptible de changer dans les versions ultérieures et peut ne pas inclure chaque fonctionnalité concernée ou la fonctionnalité.**

Consultez également [fonctionnalités supprimées ou planifié pour le remplacement à partir de Windows Server 2019](removed-features-19.md).

## <a name="features-were-no-longer-developing"></a>Fonctionnalités que nous ne développons plus

Nous vous développez n’est plus activement ces fonctionnalités et peut les supprimer à partir d’une prochaine mise à jour. Certaines fonctionnalités ont été remplacées par d'autres fonctions ou fonctionnalités, tandis que d’autres sont désormais disponibles à partir d'autres sources. 

Si vous avez des commentaires à propos de la proposition de suppression de certaines de ces fonctionnalités, vous pouvez utiliser l'[application Hub de commentaires](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

| Fonctionnalité | Au lieu de cela, vous pouvez utiliser |
|-----------|---------------------|
|Wi-Fi WEP et TKIP (**nouveau**)| Les réseaux Wi-Fi à l’aide des anciens chiffrements WEP et TKIP ne sont pas aussi sécurisés que ceux à l’aide d’AES, telles que WPA2 et WPA3. Sur Windows 10, version 1903, connexion à un réseau WEP ou TKIP affiche un message d’avertissement que le réseau n’est pas sécurisé, mais aucun message n’est affiché sur Windows Server, version 1903. Dans une version ultérieure, toute connexion à un réseau Wi-Fi à l’aide de ces anciens chiffrements est rejetée. Pour plus d’informations sur les risques de sécurité de WEP et TKIP, consultez ce [billet de blog](https://go.microsoft.com/fwlink/p/?linkid=2008426).|
|Pilote d’affichage à distance XDDM (**nouveau**)|À partir de cette version, les Services Bureau à distance utilise un Windows WDDM Display Driver Model () en fonction pilote d’affichage Indirect (IDD), une seule session Bureau à distance. La prise en charge pour les pilotes d’affichage à distance de Windows 2000 affichage modèle de pilote (XDDM) en fonction sera supprimée dans une version ultérieure. Les éditeurs de logiciels indépendants qui utilisent le pilote d’affichage à distance XDDM doit planifier une migration vers le modèle de pilote WDDM. Pour plus d’informations sur l’implémentation d’affichage à distance pilote d’affichage indirect éditeurs de logiciels indépendants permettre contacter [ rdsdev@microsoft.com ](mailto:rdsdev@microsoft.com).|
|Outil de collecte de journaux UCS (**nouveau**)|L’outil de collecte du journal UCS, bien que pas explicitement conçue pour une utilisation avec Windows Server, est néanmoins remplacé par le hub de commentaires sur Windows 10.|
|Lecteur de stockage de clés dans Hyper-V|Nous travaillons n’est plus sur la fonctionnalité lecteur de stockage de clé dans Hyper-V. Si vous utilisez des machines virtuelles de génération 1, consultez [la sécurité de la virtualisation de génération de machine virtuelle 1](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) pour plus d’informations sur les options à l’avenir. Si vous créez des machines virtuelles utilisent machines virtuelles de génération 2 avec les périphériques de module de plateforme sécurisée pour une solution plus sécurisée. |
|Console de gestion de Module de plateforme approuvée|Les informations précédemment disponibles dans la console de gestion du module de plateforme sécurisée sont désormais disponibles sur le [ **sécurité des appareils** ](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) page dans le [Windows Defender Security Center](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center).|
|Mode d’attestation hôte Service Guardian Active Directory|Nous développons ne sont plus du mode d’attestation de répertoire Active du Service Guardian hôte : au lieu de cela, nous avons ajouté un nouveau mode d’attestation, [héberger l’attestation de clé](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), qui est beaucoup plus simple et tout aussi aussi compatible que Active Directory en attestation.  Ce nouveau mode fournit des fonctionnalités équivalentes avec une expérience d’installation, une gestion plus simple et moins dépendances d’infrastructure que l’attestation Active Directory. L’attestation de clé hôte n’a aucune autre configuration matérielle requise au-delà de quelles d’attestation d’Active Directory requis, donc tous les systèmes existants restent compatibles avec le nouveau mode. Consultez [déployer des hôtes service Guardian](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) pour plus d’informations sur les options d’attestation.|
|Service de OneSync|Le service OneSync synchronise les données pour les applications de messagerie, le calendrier et personnes. Nous avons ajouté un moteur de synchronisation à l’application Outlook qui fournit la synchronisation même.|
|Prise en charge des API de la Compression différentielle à distance|Prise en charge des API de la Compression différentielle à distance activé la synchronisation des données avec une source distante à l’aide de technologies de compression qui réduit la quantité de données envoyées sur le réseau. Cette prise en charge n’est pas actuellement utilisé par tous les produits Microsoft.|
|Extension de commutateur WFP filtre léger|L’extension de commutateur de filtre léger WFP permet aux développeurs de créer [extensions du commutateur virtuel Hyper-V de filtrage des paquets de réseau simple](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). Vous pouvez obtenir les mêmes fonctionnalités en créant une extension de filtrage complète. Par conséquent, nous allons supprimer cette extension à l’avenir.|
