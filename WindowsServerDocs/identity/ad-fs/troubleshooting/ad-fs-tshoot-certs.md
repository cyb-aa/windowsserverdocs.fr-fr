---
title: Résolution des problèmes d’AD FS - certificats
description: Ce document décrit les problèmes de certificat par défaut.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 42fdce936bb4a6f25b456c4a96c7b99f650e6033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860580"
---
# <a name="ad-fs-troubleshooting---certificates"></a>Résolution des problèmes d’AD FS - certificats
AD FS requiert les certificats suivants pour fonctionner correctement.  Si un de ces n’a pas été configuré ou configuré correctement les problèmes peuvent se produire.  

## <a name="required-certificates"></a>Certificats requis
AD FS requiert des certificats suivants :



- **Approbation de fédération** – cela, soit un certificat chaîné à une racine approuvée de Internet autorité de certification (AC) doit être présent dans le magasin racine approuvé du fournisseur de revendications et les serveurs de fédération, partie de confiance un conception de la certification croisée a été implémentée dans lequel chaque côté a échangé son autorité de certification racine avec son partenaire ou certificats auto-signés qui ont été importés sur chaque côté, le cas échéant.
- **Signature de jetons** – ordinateur de chaque Service de fédération requiert un certificat de signature de jetons.  Le certificat de signature de jetons de fournisseur de revendications doit être approuvé par le serveur de fédération de confiance. Le certificat de signature de jetons de partie de confiance tiers doit être approuvé par toutes les applications qui reçoivent des jetons à partir du serveur de fédération de partie de confiance.
- **Secure Sockets Layer (SSL)** – le certificat SSL pour le Service de fédération doit être présent dans un magasin approuvé sur l’ordinateur de proxy de serveur de fédération et a une chaîne valide vers un magasin d’autorité de certification (CA) approuvée.
- **Liste de révocation de certificats** – pour n’importe quel certificat qui a une liste de révocation publiée, la révocation de certificats doit être accessible à tous les clients et serveurs qui ont besoin d’accéder au certificat.

Si aucune de ces configurations ne sont pas configurés correctement, AD FS ne fonctionnera pas.

## <a name="common-things-to-check-with-certificates"></a>Points à vérifier avec des certificats
Voici une liste des éléments qui peuvent survenir et doivent être vérifiées lorsque vous tentez de résoudre un problème de certificat.

- Assurez-vous que le certificat est approuvé
    - Certificats SSL doivent être approuvé par les clients
    - Certificats de signatures de jeton doivent être approuvé par les parties de confiance
- Vérifiez la chaîne d’approbation - chaque certificat dans la chaîne doit être valide.
- Vérifier la date d’expiration de certificat
- Vérification de l’accessibilité de liste de révocation de certificats (CRL)
    - Assurez-vous que le champ CDP est renseigné
    - Naviguez manuellement jusqu’au CDP
- Assurez-vous que le certificat n’a pas été révoqué.

## <a name="common-certificate-errors"></a>Erreurs courantes de certificat
Le tableau suivant est une liste des erreurs courantes et les causes possibles.

|Événement|Cause|Résolution
|-----|-----|-----|
|Événement 249 - un certificat est introuvable dans le magasin de certificats. Dans les scénarios de substitution de certificat, cela peut entraîner un échec lorsque le Service de fédération est signature ou le déchiffrement à l’aide de ce certificat.|Le certificat en question n’est pas présent dans le magasin de certificats local, ou le compte de service ne dispose pas d’autorisation de clé privée du certificat.|Vérifiez que le certificat est installé dans le magasin de LocalMAchine\My sur le serveur AD FS. Assurez-vous que le compte de service AD FS dispose d’un accès en lecture à la clé privée du certificat.|
|Événement 315 - une erreur s’est produite lors d’une tentative pour générer la chaîne de certificat pour l’approbation de fournisseur de revendications certificat de signature.|Le certificat a été révoqué.</br></br>La chaîne de certificats ne peut pas être vérifiée.</br></br>Le certificat a expiré ou n’est pas valide.|Assurez-vous que le certificat est valide et n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation est accessible.|
|Événement 316 - une erreur s’est produite lors d’une tentative pour générer la chaîne de certificat pour la confiance certificat de signature.|Le certificat a été révoqué.</br></br>La chaîne de certificats ne peut pas être vérifiée.</br></br>Le certificat a expiré ou n’est pas valide.|Assurez-vous que le certificat est valide et n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation est accessible.|
|Événement 317 - une erreur s’est produite lors d’une tentative pour générer la chaîne de certificat pour le certificat de chiffrement de niveau de confiance tiers partie de confiance.|Le certificat a été révoqué.</br></br>La chaîne de certificats ne peut pas être vérifiée.</br></br>Le certificat a expiré ou n’est pas valide.|Assurez-vous que le certificat est valide et n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation est accessible.|
|Événement 319 - une erreur s’est produite pendant la création de la chaîne de certificats pour le certificat client.|Le certificat a été révoqué.</br></br>La chaîne de certificats ne peut pas être vérifiée.</br></br>Le certificat a expiré ou n’est pas valide.|Assurez-vous que le certificat est valide et n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation est accessible.|
|Événement 360 - une demande a été faite pour un point de terminaison de transport de certificat, mais la requête ne comprenait pas d’un certificat client.|La racine Autorité de certification qui a émis le certificat client n’est pas approuvée.</br></br>Le certificat client a expiré.</br></br>Le certificat client est auto-signé et n’est pas approuvé.|Vérifiez que la racine Autorité de certification qui a émis le certificat client est présente dans le magasin racine approuvé.</br></br>Assurez-vous que le certificat client n’a pas expiré.</br></br>Si le certificat client est auto-signé, assurez-vous qu’il a été ajouté à la liste de certificats de confiance ou remplacer le certificat auto-signé avec un certificat approuvé.|
|Événement 374 - une erreur s’est produite lors de la création de la chaîne de certificats pour le fournisseur de revendications de certificat de chiffrement de niveau de confiance.|Le certificat a été révoqué.</br></br>La chaîne de certificats ne peut pas être vérifiée.</br></br>Le certificat a expiré ou n’est pas valide.|Assurez-vous que le certificat est valide et n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation est accessible.|
|Événement 381 - une erreur s’est produite lors d’une tentative pour générer la chaîne de certificat pour le certificat de configuration.|Le certificat configuré pour une utilisation sur le serveur AD FS est arrivé à expiration ou a été révoqué.|Assurez-vous que tous les certificats configurés n’ont pas été révoqués et n’ont pas expiré.|
|Événement 385 - AD FS a détecté qu’un ou plusieurs certificats dans la base de données de configuration AD FS doit être mis à jour manuellement.|Le certificat configuré pour une utilisation sur le serveur AD FS a expiré ou est proche de sa date d’expiration.|Mettre à jour le certificat a expiré ou à-point d’expirer avec un remplacement. (Si vous utilisez des certificats auto-signés et substitution de certificat automatique est activée, cette erreur peut être ignorée car il sera résolu automatiquement.)|
|Événement 387 - AD FS a détecté qu’un ou plusieurs certificats qui sont spécifiés dans le Service de fédération n’étaient pas accessibles au compte de service utilisé par le Service AD FS Windows.|Le compte de service AD FS ne dispose pas des autorisations de lecture à la clé privée d’un ou plusieurs certificats configurés.|Assurez-vous que le compte de service AD FS a accès en lecture à la clé privée de tous les certificats configurés.|
|Événement 389 - AD FS a détecté qu’un ou plusieurs de vos approbations nécessitent leurs certificats à mettre à jour manuellement, car elles ont expiré ou va bientôt expirer.|Un des certificats de votre partenaire configuré a expiré ou est sur le point d’expirer. Cela peut concerner une approbation de fournisseur de revendications ou une partie de confiance.|Vous avez créé manuellement cette approbation, mettre à jour la configuration des certificats manuellement. Si vous avez utilisé des métadonnées de fédération pour créer l’approbation, le certificat met à jour automatiquement dès que le partenaire met à jour le certificat.|




## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)
 