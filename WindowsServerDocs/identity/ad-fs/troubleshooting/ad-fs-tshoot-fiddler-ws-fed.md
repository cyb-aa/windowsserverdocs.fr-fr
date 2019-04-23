---
title: AD FS - Fiddler - WS-Federation de résolution des problèmes
description: Ce document montre une trace détaillée d’un échange WS-Federation avec AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be1c9f466ec13272d10f0fb9ca31cf326a1ec29a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846900"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>AD FS - Fiddler - WS-Federation de résolution des problèmes
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>Étape 1 et 2
Il s’agit de début de notre trace.  Dans ce cadre, nous voyons ce qui suit : ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

La demande :

- HTTP GET pour notre (tiers de confiance http://sql1.contoso.com/SampApp)

Réponse :

- La réponse est un HTTP 302 (redirection).  Les données de Transport dans l’en-tête de réponse indiquent où rediriger vers)https://sts.contoso.com/adfs/ls)
- L’URL de redirection contient wa = wsignin 1.0 qui nous indique que notre application de partie de confiance a créé une demande de connexion WS-Federation pour nous et cela envoyé à ADFS/ls/point de terminaison de l’AD FS.  Il s’agit comme la redirection de liaison.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>Étape 3 et 4

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

La demande :

- HTTP GET pour notre server(sts.contoso.com) AD FS

Réponse :

- La réponse est une invite pour les informations d’identification.  Cela indique que nous utilisons authnetication de formulaires
- En cliquant sur l’affichage Web de la réponse, vous pouvez voir les informations d’identification invite.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>Étape 5 et 6

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

La demande :

- HTTP POST avec notre nom d’utilisateur et le mot de passe.  
- Nous vous présentons nos informations d’identification.  En examinant les données brutes dans la demande, nous pouvons voir les informations d’identification

Réponse :

- La réponse est trouvé et le MSIAuth cookie chiffré est créé et retourné.  Cela permet de valider l’assertion SAML produite par le client.  Cela est également appelé « cookie d’authentification » et ne sont présente lorsque AD FS est le fournisseur d’identité.


## <a name="step-7-and-8"></a>Étape 7 et 8
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

La demande :

- Maintenant que nous avons authentifiés nous un autre que HTTP GET au serveur AD FS et présenter notre jeton d’authentification

Réponse :

- La réponse est un OK HTTP, ce qui signifie que les services AD FS a authentifié l’utilisateur basé sur les informations d’identification fournies
- En outre, nous paramétrons des 3 cookies vers le client
    - MSISAuthenticated contient une valeur d’horodatage codé en base64 pour lorsque le client a été authentifié.
    - MSISLoopDetectionCookie est utilisé par le mécanisme de détection de boucle infinie AD FS aux clients d’arrêt qui se sont retrouvés dans une boucle infinie de redirection pour le serveur de fédération. Les données de cookie sont un horodatage qui est codé en base64.
    - MSISSignout est utilisé pour suivre le fournisseur d’identité, et tous les fournisseurs de ressources mis en œuvre pour la session de l’authentification unique. Ce cookie est utilisé lorsqu’une déconnexion WS-Federation est appelé. Vous pouvez voir le contenu de ce cookie à l’aide d’un décodeur base64.
    
## <a name="step-9-and-10"></a>Étape 9 et 10
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png) La demande :

- HTTP POST

Réponse :

- La réponse est une détection

## <a name="step-11-and-12"></a>Étape 11 et 12
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png) La demande :

- HTTP GET

Réponse :

- La réponse est OK

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)