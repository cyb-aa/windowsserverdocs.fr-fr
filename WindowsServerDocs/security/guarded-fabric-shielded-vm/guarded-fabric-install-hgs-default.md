---
title: Installer le service SGH dans une nouvelle forêt
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7f04d9964f1a19e79335e50a1882f0326f15ddc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860030"
---
# <a name="install-hgs-in-a-new-forest"></a>Installer le service SGH dans une nouvelle forêt 

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

## <a name="add-the-hgs-server-role"></a>Ajouter le rôle de serveur SGH

Exécutez les commandes suivantes dans une session PowerShell avec élévation de privilèges pour ajouter le rôle de serveur SGH et installer le service SGH.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>Installer SGH 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>Étapes suivantes

- Pour les étapes suivantes configurer l’attestation basée sur le module de plateforme sécurisée, consultez [initialiser le cluster SGH à l’aide du mode de module de plateforme sécurisée dans une forêt dédiée (par défaut)](guarded-fabric-initialize-hgs-tpm-mode-default.md).
- Pour les étapes suivantes configurer l’attestation de clé hôte, consultez [initialiser le cluster SGH dans une forêt dédiée (par défaut) à l’aide de clés en mode](guarded-fabric-initialize-hgs-key-mode-default.md).
- Pour la prochaine procédure de configuration de l’administrateur d’une attestation (déconseillée dans Windows Server 2019), consultez [initialiser le cluster SGH dans une forêt dédiée (par défaut) à l’aide de mode AD](guarded-fabric-initialize-hgs-ad-mode-default.md).

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Initialiser SGH](guarded-fabric-initialize-hgs.md)


