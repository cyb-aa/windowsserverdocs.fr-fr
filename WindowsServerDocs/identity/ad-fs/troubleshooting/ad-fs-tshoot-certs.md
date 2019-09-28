---
title: Résolution des problèmes de AD FS-certificats
description: Ce document décrit les problèmes courants liés aux certificats.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fdadbefc138562246c72f7707b303d966bff0989
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407193"
---
# <a name="ad-fs-troubleshooting---certificates"></a>Résolution des problèmes de AD FS-certificats
AD FS requiert les certificats suivants pour fonctionner correctement.  Si l’un de ces éléments n’a pas été configuré ou configuré correctement, des problèmes peuvent survenir.  

## <a name="required-certificates"></a>Certificats requis
AD FS requiert les certificats suivants :



- **Approbation de Fédération** : cela nécessite qu’un certificat chaîné à une autorité de certification racine Internet (ca) approuvée mutuellement soit présent dans le magasin racine approuvé du fournisseur de revendications et des serveurs de Fédération de la partie de confiance. la conception de certification croisée a été implémentée dans laquelle chaque côté a échangé son autorité de certification racine avec son partenaire, ou les certificats auto-signés qui ont été importés de chaque côté, le cas échéant.
- **Signature de jetons** : chaque service FS (Federation Service) ordinateur requiert un certificat de signature de jetons.  Le certificat de signature de jetons du fournisseur de revendications doit être approuvé par le serveur de Fédération de la partie de confiance. Le certificat de signature de jetons de la partie de confiance doit être approuvé par toutes les applications qui reçoivent des jetons du serveur de Fédération RP.
- **Protocole SSL (SSL)** : le certificat SSL pour le service FS (Federation Service) doit être présent dans un magasin approuvé sur l’ordinateur proxy du serveur de Fédération et disposer d’une chaîne valide à un magasin d’autorités de certification approuvées.
- **Liste de révocation de certificats** : pour tout certificat ayant une liste de révocation de certificats publiée, la liste de révocation des certificats doit être accessible à tous les clients et serveurs qui doivent accéder au certificat.

Si l’un des éléments ci-dessus n’est pas configuré correctement, AD FS ne fonctionnera pas.

## <a name="common-things-to-check-with-certificates"></a>Éléments courants à vérifier avec les certificats
Voici une liste des éléments qui peuvent se présenter et doivent être vérifiés lors de la tentative de résolution d’un problème de certificat.

- Assurez-vous que le certificat est approuvé
    - Les certificats SSL doivent être approuvés par les clients
    - Les certificats de signature de jeton doivent être approuvés par les parties de confiance
- Vérifier la chaîne d’approbation : chaque certificat de la chaîne doit être valide.
- Vérifier la date d’expiration du certificat
- Vérifier l’accessibilité de la liste de révocation de certificats
    - Assurez-vous que le champ CDP est rempli
    - Accéder manuellement au CDP
- Assurez-vous que le certificat n’a pas été révoqué

## <a name="common-certificate-errors"></a>Erreurs de certificat courantes
Le tableau suivant répertorie les erreurs courantes et les causes possibles.

|Événement|Cause|Résolution :
|-----|-----|-----|
|Événement 249-un certificat est introuvable dans le magasin de certificats. Dans les scénarios de substitution de certificat, cela peut provoquer un échec lorsque le service FS (Federation Service) est la signature ou le déchiffrement à l’aide de ce certificat.|Le certificat en question n’est pas présent dans le magasin de certificats local, ou le compte de service n’a pas l’autorisation d’accès à la clé privée du certificat.|Assurez-vous que le certificat est installé dans le magasin LocalMAchine\My sur le serveur AD FS. Assurez-vous que le compte de service AD FS dispose d’un accès en lecture à la clé privée du certificat.|
|Événement 315 : une erreur s’est produite lors d’une tentative de création de la chaîne de certificats pour le certificat de signature d’approbation du fournisseur de revendications.|Le certificat a été révoqué.</br></br>Impossible de vérifier la chaîne de certificats.</br></br>Le certificat a expiré ou n’est pas encore valide.|Assurez-vous que le certificat est valide et qu’il n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation de certificats est accessible.|
|Événement 316 : une erreur s’est produite lors de la tentative de création de la chaîne de certificats pour le certificat de signature d’approbation de la partie de confiance.|Le certificat a été révoqué.</br></br>Impossible de vérifier la chaîne de certificats.</br></br>Le certificat a expiré ou n’est pas encore valide.|Assurez-vous que le certificat est valide et qu’il n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation de certificats est accessible.|
|Événement 317 : une erreur s’est produite lors de la tentative de création de la chaîne de certificats pour le certificat de chiffrement de l’approbation de la partie de confiance.|Le certificat a été révoqué.</br></br>Impossible de vérifier la chaîne de certificats.</br></br>Le certificat a expiré ou n’est pas encore valide.|Assurez-vous que le certificat est valide et qu’il n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation de certificats est accessible.|
|Événement 319 : une erreur s’est produite lors de la génération de la chaîne de certificats pour le certificat client.|Le certificat a été révoqué.</br></br>Impossible de vérifier la chaîne de certificats.</br></br>Le certificat a expiré ou n’est pas encore valide.|Assurez-vous que le certificat est valide et qu’il n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation de certificats est accessible.|
|Événement 360 : une demande a été effectuée sur un point de terminaison de transport de certificat, mais la demande n’a pas inclus de certificat client.|L’autorité de certification racine qui a émis le certificat client n’est pas approuvée.</br></br>Le certificat client a expiré.</br></br>Le certificat client est auto-signé et n’est pas approuvé.|Assurez-vous que l’autorité de certification racine qui a émis le certificat client est présente dans le magasin racine approuvé.</br></br>Assurez-vous que le certificat client n’est pas arrivé à expiration.</br></br>Si le certificat client est auto-signé, assurez-vous qu’il a été ajouté à la liste des certificats approuvés ou remplacez le certificat auto-signé par un certificat approuvé.|
|Événement 374 : une erreur s’est produite lors de la génération de la chaîne de certificats pour le certificat de chiffrement d’approbation du fournisseur de revendications.|Le certificat a été révoqué.</br></br>Impossible de vérifier la chaîne de certificats.</br></br>Le certificat a expiré ou n’est pas encore valide.|Assurez-vous que le certificat est valide et qu’il n’a pas été révoqué.</br></br>Vérifiez que la liste de révocation de certificats est accessible.|
|Événement 381 : une erreur s’est produite lors de la tentative de création de la chaîne de certificats pour le certificat de configuration.|L’un des certificats configurés pour une utilisation sur le serveur AD FS a expiré ou a été révoqué.|Assurez-vous que tous les certificats configurés n’ont pas été révoqués et n’ont pas expiré.|
|Événement 385-AD FS détecté qu’un ou plusieurs certificats de la base de données de configuration AD FS doivent être mis à jour manuellement.|L’un des certificats configurés pour une utilisation sur le serveur AD FS a expiré ou est proche de sa date d’expiration.|Mettez à jour le certificat expiré ou arrivant à expiration en remplaçant. (Si vous utilisez des certificats auto-signés et que la substitution de certificat automatique est activée, cette erreur peut être ignorée, car elle se résout automatiquement.)|
|Événement 387-AD FS a détecté qu’un ou plusieurs certificats spécifiés dans le service FS (Federation Service) n’étaient pas accessibles au compte de service utilisé par le service Windows AD FS.|Le compte de service AD FS ne dispose pas d’autorisations de lecture sur la clé privée d’un ou de plusieurs certificats configurés.|Assurez-vous que le compte de service AD FS dispose d’une autorisation de lecture sur la clé privée de tous les certificats configurés.|
|Événement 389-AD FS a détecté qu’une ou plusieurs de vos approbations nécessitent la mise à jour manuelle de leurs certificats, car ils ont expiré ou expirent bientôt.|L’un des certificats de votre partenaire configuré a expiré ou arrive à expiration. Cela peut s’appliquer à une approbation de fournisseur de revendications ou à une approbation de partie de confiance.|Si vous avez créé cette approbation manuellement, mettez à jour la configuration du certificat manuellement. Si vous avez utilisé les métadonnées de Fédération pour créer l’approbation, le certificat est automatiquement mis à jour dès que le partenaire met à jour le certificat.|




## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)
 