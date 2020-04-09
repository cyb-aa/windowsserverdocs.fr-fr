---
title: Installer SGH dans une nouvelle forêt
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8f896b0cea49f9dd26a828a2580b59a78348763a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856602"
---
# <a name="install-hgs-in-a-new-forest"></a>Installer SGH dans une nouvelle forêt 

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

## <a name="add-the-hgs-server-role"></a>Ajouter le rôle de serveur SGH

Exécutez les commandes suivantes dans une session PowerShell avec élévation de privilèges pour ajouter le rôle de serveur SGH et installer SGH.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>Installer SGH 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>Étapes suivantes :

- Pour connaître les étapes à suivre pour configurer l’attestation basée sur le module de plateforme sécurisée, consultez [initialiser le cluster SGH à l’aide du mode TPM dans une nouvelle forêt dédiée (par défaut)](guarded-fabric-initialize-hgs-tpm-mode-default.md).
- Pour connaître les étapes suivantes pour configurer l’attestation de clé hôte, consultez [initialiser le cluster SGH à l’aide du mode clé dans une nouvelle forêt dédiée (par défaut)](guarded-fabric-initialize-hgs-key-mode-default.md).
- Pour connaître les étapes à suivre pour configurer l’attestation basée sur l’administration (déconseillée dans Windows Server 2019), consultez [initialiser le cluster SGH à l’aide du mode ad dans une nouvelle forêt dédiée (par défaut)](guarded-fabric-initialize-hgs-ad-mode-default.md).

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Initialiser SGH](guarded-fabric-initialize-hgs.md)


