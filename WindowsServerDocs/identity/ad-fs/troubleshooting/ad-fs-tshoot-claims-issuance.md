---
title: Résolution des problèmes d’AD FS - émission de revendications
description: Ce document décrit comment résoudre les problèmes d’émission de jeton avec AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdf8851fe9b35f82191458ba3313fda2dc3ee4cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839660"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>Résolution des problèmes d’AD FS - émission de revendications
Une revendication est une instruction qu’un sujet fait à propos de lui-même ou un autre objet.  Revendications sont émises par une partie de confiance, ils sont et un ou plusieurs des valeurs empaquetées dans des jetons de sécurité émis par le serveur AD FS.  Comme il existe plusieurs éléments dans ce processus, émission de revendications peut être divisée en ces éléments clés.

>[!NOTE]  
>Vous pouvez utiliser [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) sur le [ADFS aide](https://adfshelp.microsoft.com) site pour aider à résoudre les problèmes de revendications.   

## <a name="token-request"></a>Demande de jeton
Lorsque vous accédez à une partie de confiance qu’il vous redirige vers AD FS avec une demande de jeton.  Des problèmes peuvent survenir avec la demande.  Plus particulièrement :

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>La demande de mise en forme avec 3e parties particulièrement SAML)

### <a name="pre-formated-urls-that-have-typos"></a>URL au format antérieur qui ont des fautes de frappe
Lors de l’émission d’un jeton à partir de parties de confiance WS-Federaion cette demande de jeton rencontre avec des paramètres de chaîne de requête URL.  Si la partie de confiance ne spécifie pas les paramètres corrects dans cette URL lorsqu’il effectue la redirection à AD FS cela peut entraîner un problème avec la demande.


Dans l’ordre pour vérifiez le format de jeton, un outil de débogage web utilisable


## <a name="token-response"></a>Réponse de jeton

## <a name="authentication"></a>Authentification

## <a name="claim-rule-processing"></a>Traitement des règles de revendication