---
title: Résolution des problèmes de AD FS-Fiddler
description: Ce document décrit ce qu’est Fiddler et comment installer et configurer Fiddler pour résoudre les problèmes de AD FS les revendications
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f2abf1a0b844e8e8799458f5237d7a80059880ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366152"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>Résolution des problèmes de AD FS-Fiddler
Fiddler est un outil qui peut être utilisé pour capturer le trafic Web HTTP/HTTPs.  Cet outil peut être utilisé pour aider à résoudre les problèmes liés au processus d’émission des revendications.  En examinant le trafic, nous pouvons mieux comprendre où l’interaction est interrompue.  Ce document décrit comment installer et configurer Fiddler pour capturer le trafic AD FS.  Pour obtenir un exemple de trace Fiddler à l’aide de WS-Federation, consultez [AD FS Troubleshooting-Fiddler-WS-Federation](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>Télécharger et installer Fiddler
Vous pouvez télécharger Fiddler [ici](https://www.telerik.com/download/fiddler).  Une fois que vous l’avez téléchargé, installez-le.

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>Configurer Fiddler pour capturer le trafic AD FS
Afin de capturer AD FS trafic, nous devons configurer Fiddler pour déchiffrer le trafic SSL. 

### <a name="configure-the-fiddler-ssl-certificate"></a>Configurer le certificat SSL Fiddler
 Utilisez la procédure suivante pour configurer Fiddler afin de déchiffrer le trafic SSL.

1.  Ouvrir Fiddler
2.  En haut, sous **Outils**, sélectionnez **options Fiddler**.
3.  Cliquez sur l’onglet HTTPs.
4.  Activez la case à cocher **déchiffrer le trafic HTTPS** et sélectionnez **parmi les navigateurs uniquement** dans la liste déroulante.
5.  Activez la case à cocher **Ignorer les erreurs de certificat de serveur**.
6.  Cliquez sur **OK**.

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)