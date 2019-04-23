---
title: AD FS résolution des problèmes - points de terminaison AD FS
description: Ce document explique comment résoudre les points de terminaison AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13b830c0317341280bd87499e3abd8dcd1a33fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857560"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>AD FS résolution des problèmes - points de terminaison de métadonnées ADFS
Points de terminaison fournissent l’accès à la fonctionnalité de serveur de fédération d’AD FS, telles que de la publication des métadonnées de fédération.  Pour vérifier que le serveur AD FS répond aux requêtes web, nous pouvons vérifier les points de terminaison différents.


## <a name="federation-metadata-test"></a>Test des métadonnées de fédération
Fédération passive fait référence aux scénarios où votre navigateur est redirigé vers la page de connexion AD FS.  En testant le point de terminaison de métadonnées, nous pouvons déterminer si le serveur AD FS répond aux requêtes web dans ces scénarios passifs.  Utilisez la procédure suivante pour tester le point de terminaison.

1.  À l’aide d’un navigateur web, accédez à votre point de terminaison de métadonnées de fédération AD FS.  Par exemple :  https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. Le fichier xml doit télécharger localement sur votre ordinateur.
3. Ouvrez-le et vérifiez qu’il contient des informations similaires pour les informations ci-dessous : ![Passive](media/ad-fs-tshoot-endpoints/meta2.png)

## <a name="ws-mex-test-active-test"></a>WS-MEX (test de Active)
WS-MetaDataExchange est un protocole de services web et fait partie de la feuille de route de WS-Federation.  Elle utilise un message SOAP pour demander les métadonnées.  En testant le point de terminaison, nous pouvons déterminer si le serveur AD FS répond aux requêtes web pour WS-MetaDataExchange.  Utilisez la procédure suivante pour tester le point de terminaison.
1.  À l’aide d’un navigateur web, accédez à votre point de terminaison de métadonnées de fédération AD FS.  Par exemple :  https://sts.contoso.com/adfs/services/trust/mex
2. Le fichier xml doit être affiché automatiquement dans le navigateur.  Il doit ressembler à l’image ci-dessous :

![Actif](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)