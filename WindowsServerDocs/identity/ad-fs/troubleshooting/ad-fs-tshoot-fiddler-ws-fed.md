---
title: AD FS résolution des problèmes-Fiddler-WS-Federation
description: Ce document présente une trace détaillée d’un échange WS-Federation avec AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d263f48aadff7c77cba44a2328d472ebbe5dfbbf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407218"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>AD FS résolution des problèmes-Fiddler-WS-Federation
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>Étape 1 et 2
Il s’agit du début de notre suivi.  Dans ce frame, nous voyons ce qui suit : ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

Demande

- HTTP sur la partie de confiance (http://sql1.contoso.com/SampApp)

Lutte

- La réponse est un HTTP 302 (redirection).  Les données de transport dans l’en-tête de réponse indiquent où rediriger vers (https://sts.contoso.com/adfs/ls)
- L’URL de redirection contient wa = wsignin 1,0 qui nous indique que notre application de RP a créé une demande de connexion WS-Federation pour nous et l’a envoyée au point de terminaison/adfs/ls/de AD FS.  C’est ce que l’on appelle la liaison de redirection.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>Étape 3 et 4

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

Demande

- HTTP accédez à notre serveur AD FS (STS. contoso. com)

Lutte

- La réponse est une invite pour les informations d’identification.  Cela indique que nous utilisons Forms méthodes
- En cliquant sur la WebView de la réponse, vous pouvez voir l’invite des informations d’identification.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>Étape 5 et 6

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

Demande

- HTTP HTTP avec votre nom d’utilisateur et votre mot de passe.  
- Nous présentons nos informations d’identification.  En examinant les données brutes dans la requête, nous pouvons voir les informations d’identification

Lutte

- La réponse est trouvée et le cookie chiffré MSIAuth est créé et retourné.  Cette valeur est utilisée pour valider l’assertion SAML produite par notre client.  Cette opération est également appelée « cookie d’authentification » et sera uniquement présente quand AD FS est le IDP.


## <a name="step-7-and-8"></a>Étape 7 et 8
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

Demande

- Maintenant que nous sommes authentifiés, nous faisons un autre accès HTTP au serveur AD FS et présentons notre jeton d’authentification

Lutte

- La réponse est un HTTP OK, ce qui signifie que AD FS a authentifié l’utilisateur en fonction des informations d’identification fournies
- En outre, nous avons défini 3 cookies sur le client
    - MSISAuthenticated contient une valeur d’horodatage encodée en base64 pour le moment où le client a été authentifié.
    - MSISLoopDetectionCookie est utilisé par le AD FS mécanisme de détection de boucle infinie pour arrêter les clients qui se sont terminés dans une boucle de redirection infinie sur le serveur de Fédération. Les données de cookie sont un horodatage encodé en base64.
    - MSISSignout est utilisé pour effectuer le suivi du IdP et de tous les RPs visités pour la session SSO. Ce cookie est utilisé lors de l’appel d’une déconnexion WS-Federation. Vous pouvez voir le contenu de ce cookie à l’aide d’un décodeur base64.
    
## <a name="step-9-and-10"></a>Étape 9 et 10
Demande ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png) :

- HTTP APRÈS

Lutte

- La réponse est un trouvé

## <a name="step-11-and-12"></a>Étape 11 et 12
Demande ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png) :

- HTTP-RÉCUPÉRATION

Lutte

- La réponse est OK

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)