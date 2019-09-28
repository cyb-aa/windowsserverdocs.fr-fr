---
title: Résolution des problèmes de AD FS-émission de revendications
description: Ce document décrit comment résoudre les problèmes d’émission de jetons avec AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ea0e6112f00f9cace6a0c580661a5319b5adaea5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366239"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>Résolution des problèmes de AD FS-émission de revendications
Une revendication est une déclaration qu’un sujet fait à propos de lui-même ou d’un autre objet.  Les revendications sont émises par une partie de confiance, et elles reçoivent une ou plusieurs valeurs, puis empaquetées dans des jetons de sécurité émis par le serveur AD FS.  Étant donné qu’il y a plusieurs parties mobiles dans ce processus, l’émission de revendications peut être divisée en ces éléments clés.

>[!NOTE]  
>Vous pouvez utiliser [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) sur le site d' [aide ADFS](https://adfshelp.microsoft.com) pour vous aider à résoudre les problèmes liés aux revendications.   

## <a name="token-request"></a>Demande de jeton
Lorsque vous accédez à une partie de confiance, vous êtes redirigé vers AD FS avec une demande de jeton.  Des problèmes peuvent survenir avec la demande.  Notamment :

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>Mise en forme de la demande avec des tiers (en particulier SAML)

### <a name="pre-formated-urls-that-have-typos"></a>URL préformatées avec des fautes de frappe
Lors de l’émission d’un jeton à partir de parties de confiance WS-Federaion, la demande de jeton intervient avec les paramètres de chaîne de requête d’URL.  Si la partie de confiance ne spécifie pas les paramètres corrects dans cette URL lorsqu’elle effectue la redirection vers AD FS, cela peut entraîner un problème avec la demande.


Pour vérifier le format de jeton, vous pouvez utiliser un outil de débogage Web.


## <a name="token-response"></a>Réponse du jeton

## <a name="authentication"></a>Authentication

## <a name="claim-rule-processing"></a>Traitement de la règle de revendication