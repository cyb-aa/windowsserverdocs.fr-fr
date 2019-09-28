---
title: Résolution des problèmes de AD FS-AD FS des points de terminaison
description: Ce document explique comment dépanner des points de terminaison de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 807b5c5de14bf6a43419d0b9d2d3a4e6953d0075
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366223"
---
# <a name="ad-fs-troubleshooting---ad-fs-metadata-endpoints"></a>Résolution des problèmes de AD FS AD FS les points de terminaison de métadonnées
Les points de terminaison fournissent un accès aux fonctionnalités du serveur de Fédération de AD FS, telles que la publication de métadonnées de Fédération.  Pour vérifier que le serveur de AD FS répond aux requêtes Web, nous pouvons vérifier les différents points de terminaison.


## <a name="federation-metadata-test"></a>Test des métadonnées de Fédération
La Fédération passive fait référence à des scénarios dans lesquels votre navigateur est redirigé vers la page de connexion AD FS.  En testant le point de terminaison de métadonnées, nous pouvons déterminer si le serveur de AD FS répond aux requêtes Web dans ces scénarios passifs.  Utilisez la procédure suivante pour tester le point de terminaison.

1.  À l’aide d’un navigateur Web, accédez à votre point de terminaison de métadonnées de fédération AD FS.  Par exemple : https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml
2. Le fichier XML doit être téléchargé localement sur votre ordinateur.
3. Ouvrez-le et vérifiez qu’il contient des informations similaires à celles du sert ci-dessous : ![Passive @ no__t-1

## <a name="ws-mex-test-active-test"></a>Test WS-MEX (test actif)
WS-MetaDataExchange est un protocole de services Web qui fait partie de la feuille de route WS-Federation.  Elle utilise un message SOAP pour demander des métadonnées.  En testant le point de terminaison, nous pouvons déterminer si le serveur de AD FS répond aux demandes Web pour WS-MetaDataExchange.  Utilisez la procédure suivante pour tester le point de terminaison.
1.  À l’aide d’un navigateur Web, accédez à votre point de terminaison de métadonnées de fédération AD FS.  Par exemple : https://sts.contoso.com/adfs/services/trust/mex
2. Le fichier XML doit être affiché automatiquement dans le navigateur.  Elle doit ressembler à l’image ci-dessous :

![Actif](media/ad-fs-tshoot-endpoints/meta3.png)


## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)