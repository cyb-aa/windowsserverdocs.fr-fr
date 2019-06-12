---
title: Résolution des problèmes liés aux services ADFS (Active Directory Federation Services)
description: Ce document explique comment résoudre les divers aspects des services AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6410d510085d1772ca6d8ced47226e00239a1a02
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443901"
---
# <a name="troubleshooting-ad-fs"></a>Résolution des problèmes liés aux services ADFS (Active Directory Federation Services)
AD FS possède un grand nombre d’éléments mobiles, touche plusieurs choses et comporte de nombreuses dépendances différents.  Bien entendu, cela peut donner lieu à différents problèmes.  Ce document est conçu pour vous aider à démarrer sur la résolution de ces problèmes.  Ce document vous présente les zones classiques que vous devez vous concentrer sur l’activation de fonctionnalités pour des informations supplémentaires et des outils différents qui peuvent être utilisées pour dépister les problèmes.  

>[!NOTE]
>Pour plus d’informations, consultez [ADFS aide](http://adfshelp.microsoft.com) qui fournit des outils efficaces dans un placent qui facilite pour les utilisateurs et les administrateurs à résoudre les problèmes d’authentification à un rythme plus rapide. 


## <a name="what-to-check-first"></a>Les éléments à vérifier le premier
Avant de vous plongez dans la résolution des problèmes approfondie, il existe quelques points que vous devez vérifier tout d’abord.  Celles-ci sont les suivantes :
- **Configuration DNS** -vous pouvez résoudre le nom du service de fédération ?  Cela devrait résoudre à l’adresse IP de soit l’équilibreur de charge ou l’adresse IP d’un des serveurs AD FS dans votre batterie de serveurs.  Pour plus d’informations, consultez [AD FS résolution des problèmes - DNS](ad-fs-tshoot-dns.md).
- **Points de terminaison AD FS** -vous pouvez parcourir aux points de terminaison AD FS ?  En accédant à cela, vous pouvez déterminer si votre serveur de web AD FS répond aux demandes.  Si vous pouvez accéder à ce fichier, vous savez que AD FS traite les demandes sur le port 443 parfaitement.  Pour plus d’informations, consultez [AD FS résolution des problèmes - points de terminaison](ad-fs-tshoot-endpoints.md).
- **Authentification unique initiée par le fournisseur d’identité On** -se connecter et s’authentifier via la page authentification Idp-Initiated ?  Vous devez vous assurer que cette page a été activée parce qu’elle est désactivée par défaut.  Utilisez `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` pour activer la page.  Si vous pouvez vous connecter et s’authentifier vous savez que les services ADFS fonctionnent dans cette zone.  Pour plus d’informations, consultez [AD FS résolution des problèmes - SignOn](ad-fs-tshoot-initiatedsignon.md).
  ##  <a name="common-troubleshooting-areas"></a>Zones de résolution des problèmes courants

|Nom|Description|
|-----|-----|
|[Journalisation des événements et audit](ad-fs-tshoot-logging.md)|Utilisez les journaux des événements Windows pour afficher les informations au niveau basse et haute via les journaux Admin et Trace.  Il peut également être utilisé pour afficher l’audit de sécurité.|
|[Connectivité SQL](ad-fs-tshoot-sql.md)|Pour plus d’informations sur le test de la connectivité entre vos serveurs AD FS et les bases de données principales SQL|
|[Émission de revendications](ad-fs-tshoot-claims-issuance.md)|Informations sur la détermination de si AD FS est correctement émission des revendications.|
|[Détection des boucles](ad-fs-tshoot-loop.md)|Informations sur la détermination et empêcher les utilisateurs de retournés dans les deux sens entre le fournisseur d’identité et une partie de confiance.|
|[Certificats](ad-fs-tshoot-certs.md)|Problèmes de certificat dans le cas qui peuvent se produire|
|[Fiddler](ad-fs-tshoot-fiddler.md)|Pour plus d’informations sur la procédure d’installation et à l’aide de Fiddler|
|[WS-Federation avec Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|Suivi Fiddler détaillées d’une interaction de WS-Federation|
|[Règles de revendication](ad-fs-tshoot-claims-rules.md)|Pour plus d’informations sur la résolution des règles de revendication et leur syntaxe|
|[Authentification Windows intégrée](ad-fs-tshoot-iwa.md)|Informations sur la résolution de l’authentification intégrée.|
|[Azure AD](ad-fs-tshoot-azure.md)|Informations sur la résolution des problèmes d’interaction d’AD FS avec Azure AD.|
|[AD FS Diagnostics Analyzer](ad-fs-diagnostics-analyzer.md)|Analyseur de Diagnostics aident à AD FS peut aider à effectuer des vérifications de base AD FS à l’aide du module PowerShell de diagnostics. 