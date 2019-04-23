---
title: AD FS résolution des problèmes - Fiddler
description: Ce document décrit comment installer et configurer Fiddler pour résoudre les problèmes de revendications AD FS et Fiddler What ' s
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/02/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 822300d0e4b6ae462a3c942e22530bbed5f93e86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865630"
---
# <a name="ad-fs-troubleshooting---fiddler"></a>AD FS résolution des problèmes - Fiddler
Fiddler est un outil qui peut être utilisé pour capturer le trafic web HTTP/HTTPS.  Cet outil peut être utilisé pour aider à résoudre le processus d’émission de revendication.  En examinant le trafic, nous pouvons obtenir une meilleure compréhension de l’endroit où l’interaction est élimination.  Ce document décrit comment installer et configurer Fiddler pour capturer le trafic AD FS.  Pour une trace de fiddler d’exemple à l’aide de WS-Federation consultez [AD FS dépannage - Fiddler - WS-Federation](ad-fs-tshoot-fiddler-ws-fed.md)

## <a name="download-and-install-fiddler"></a>Téléchargez et installez Fiddler
Vous pouvez télécharger Fiddler [ici](https://www.telerik.com/download/fiddler).  Une fois que vous avez téléchargé, il continuez et installez-le.

## <a name="configure-fiddler-to-capture-ad-fs-traffic"></a>Configurez Fiddler pour capturer le trafic AD FS
Pour capturer le trafic AD FS, nous devons configurer Fiddler pour déchiffrer le trafic SSL. 

### <a name="configure-the-fiddler-ssl-certificate"></a>Configurer le certificat SSL de Fiddler
 Utilisez la procédure suivante pour configurer Fiddler pour déchiffrer le trafic SSL.

1.  Ouvrez Fiddler
2.  En haut, sous **outils**, sélectionnez **Fiddler Options**.
3.  Cliquez sur l’onglet HTTPS.
4.  Cochez la case **le trafic HTTPS déchiffrer** et sélectionnez **à partir de navigateurs uniquement** à partir de la liste déroulante.
5.  Cochez la case **ignorer les erreurs de certificat de serveur**.
6.  Cliquez sur **OK**.

![Fiddler](media/ad-fs-tshoot-fiddler/fiddler1.png)

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)