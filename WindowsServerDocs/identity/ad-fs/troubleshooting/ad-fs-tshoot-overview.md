---
title: Résolution des problèmes liés aux services ADFS (Active Directory Federation Services)
description: Ce document décrit comment résoudre les différents aspects de AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5fc5c2384a6d8d13f807a1ce99c5db78bee5b108
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950129"
---
# <a name="troubleshooting-ad-fs"></a>Résolution des problèmes liés aux services ADFS (Active Directory Federation Services)
AD FS a un grand nombre de pièces mobiles, touche de nombreuses choses et a de nombreuses dépendances.  À l’évidence, cet état de fait peut donner lieu à des problèmes divers.  Ce document est conçu pour vous aider à résoudre ces problèmes.  Ce document vous présente les zones courantes sur lesquelles vous devez vous concentrer, comment activer des fonctionnalités pour obtenir des informations supplémentaires et différents outils qui peuvent être utilisés pour détecter les problèmes.  

>[!NOTE]
>Pour plus d’informations, consultez [l’aide d’ADFS](https://adfshelp.microsoft.com) qui fournit des outils efficaces à un seul emplacement, ce qui permet aux utilisateurs et aux administrateurs de résoudre plus facilement les problèmes d’authentification à un rythme plus rapide. 


## <a name="what-to-check-first"></a>Éléments à vérifier en premier
Avant de vous plonger dans un dépannage approfondi, vous devez d’abord vérifier quelques éléments.  Ils sont les suivants :
- **Configuration DNS** : pouvez-vous résoudre le nom du service de Fédération ?  Cela doit correspondre à l’adresse IP de l’équilibreur de charge ou à l’adresse IP de l’un des serveurs AD FS de votre batterie de serveurs.  Pour plus d’informations [, consultez AD FS Troubleshooting-DNS](ad-fs-tshoot-dns.md).
- **Points de terminaison de AD FS** : pouvez-vous accéder aux points de terminaison de AD FS ?  En accédant à cela, vous pouvez déterminer si votre serveur Web AD FS répond aux demandes.  Si vous pouvez accéder à ce fichier, vous savez que AD FS traite les demandes en plus de 443.  Pour plus d’informations [, consultez AD FS dépannage-points de terminaison](ad-fs-tshoot-endpoints.md).
- **Connexion initiée par IDP** : pouvez-vous vous connecter et vous authentifier à l’aide de la page de connexion initiée par IDP ?  Vous devez vous assurer que cette page a été activée, car elle est désactivée par défaut.  Utilisez `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` pour activer la page.  Si vous pouvez vous connecter et vous authentifier, vous savez que AD FS fonctionne dans ce domaine.  Pour plus d’informations [, consultez AD FS Troubleshooting-](ad-fs-tshoot-initiatedsignon.md)Signing.
  ##  <a name="common-troubleshooting-areas"></a>Zones de dépannage courantes

|Nom|Description|
|-----|-----|
|[Journalisation et audit des événements](ad-fs-tshoot-logging.md)|Utilisez les journaux des événements Windows pour afficher des informations de haut niveau et de bas niveau à l’aide des journaux d’administration et de suivi.  Elle peut également être utilisée pour afficher l’audit de sécurité.|
|[Connectivité SQL](ad-fs-tshoot-sql.md)|Informations sur le test de la connectivité entre vos serveurs AD FS et les bases de données SQL principales|
|[Émission de revendications](ad-fs-tshoot-claims-issuance.md)|Informations pour déterminer si AD FS émet correctement des revendications.|
|[Détection de boucle](ad-fs-tshoot-loop.md)|Informations sur la détermination et l’empêchement des utilisateurs d’être retournés entre les IDP et les RP.|
|[Certificats](ad-fs-tshoot-certs.md)|Typcial les problèmes de certificat qui peuvent survenir|
|[Fiddler](ad-fs-tshoot-fiddler.md)|Informations sur l’installation et l’utilisation de Fiddler|
|[WS-Federation avec Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|Trace Fiddler détaillée d’une interaction WS-Federation|
|[Règles de revendication](ad-fs-tshoot-claims-rules.md)|Informations sur la résolution des problèmes de règles de revendication et leur syntaxe|
|[Authentification Windows intégrée](ad-fs-tshoot-iwa.md)|Informations sur la résolution des problèmes d’authentification intégrée.|
|[Azure AD](ad-fs-tshoot-azure.md)|Informations sur la résolution des problèmes d’interaction AD FS avec Azure AD.|
|[Analyseur de diagnostics de AD FS](ad-fs-diagnostics-analyzer.md)|AD FS aide de l’analyseur de diagnostics peut aider à effectuer des vérifications de AD FS de base à l’aide du module PowerShell de diagnostic. 