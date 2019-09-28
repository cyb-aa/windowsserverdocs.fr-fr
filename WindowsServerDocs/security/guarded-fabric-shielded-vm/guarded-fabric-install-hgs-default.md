---
title: Installer SGH dans une nouvelle forêt
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 6dfbe24fb4d9011b48f366d7e5df92fdb80685d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386588"
---
# <a name="install-hgs-in-a-new-forest"></a>Installer SGH dans une nouvelle forêt 

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

## <a name="add-the-hgs-server-role"></a>Ajouter le rôle de serveur SGH

Exécutez les commandes suivantes dans une session PowerShell avec élévation de privilèges pour ajouter le rôle de serveur SGH et installer SGH.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>Installer SGH 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>Étapes suivantes

- Pour connaître les étapes à suivre pour configurer l’attestation basée sur le module de plateforme sécurisée, consultez [initialiser le cluster SGH à l’aide du mode TPM dans une nouvelle forêt dédiée (par défaut)](guarded-fabric-initialize-hgs-tpm-mode-default.md).
- Pour connaître les étapes suivantes pour configurer l’attestation de clé hôte, consultez [initialiser le cluster SGH à l’aide du mode clé dans une nouvelle forêt dédiée (par défaut)](guarded-fabric-initialize-hgs-key-mode-default.md).
- Pour connaître les étapes à suivre pour configurer l’attestation basée sur l’administration (déconseillée dans Windows Server 2019), consultez [initialiser le cluster SGH à l’aide du mode ad dans une nouvelle forêt dédiée (par défaut)](guarded-fabric-initialize-hgs-ad-mode-default.md).

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Initialiser SGH](guarded-fabric-initialize-hgs.md)


