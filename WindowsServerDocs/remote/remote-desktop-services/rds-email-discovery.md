---
title: Configurer la découverte électronique s’abonner à votre flux de RDS
description: Découvrez comment intégrer des Services de domaine Active Directory de Azure dans votre déploiement RDS.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1691668"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurer la découverte électronique s’abonner à votre flux de RDS

N’avez-vous jamais eu difficultés pour vos utilisateurs finaux connectés à leur RDS publié flux, soit en raison d’un seul caractère manquant dans le flux URL ou car ils auront perdu le courrier électronique avec l’URL? Presque toutes les applications de client de bureau à distance prend en charge la recherche de votre abonnement en entrant votre adresse de messagerie, rend plus facile que jamais pour obtenir vos utilisateurs connectés à leurs RemoteApps et les ordinateurs de bureau.

>[!IMPORTANT]
>L’application de bureau à distance de Microsoft dans Microsoft Store ne prend pas en charge abonnement d’adresse de messagerie à ce stade.

Avant de configurer la découverte électronique, procédez comme suit:

- Vérifiez que vous avez l’autorisation d’ajouter un enregistrement TXT au domaine associé à votre courrier électronique (par exemple, si vos utilisateurs ont @contoso.com , les adresses de messagerie vous devez disposer d’autorisations pour le domaine contoso.com)
- Créer une URL de flux du site Web services Bureau à distance (https://\ < rdweb-dns-name\ >.domain/RDWeb/Feed/webfeed.aspx, tel quehttps://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

Maintenant, suivez cette procédure pour configurer la découverte électronique:

1. Dans votre navigateur, connectez-vous au site Web d’inscriptions de nom de domaine où votre domaine est inscrit.
2. Accédez à la page appropriée pour votre domaine enregistré dans laquelle vous pouvez afficher, ajouter et modifier les enregistrements DNS.
3. Entrez un nouvel enregistrement DNS avec les propriétés suivantes:
   - **Hôte:** _msradc
   - **Texte:** \ < Web des services Bureau à distance flux URL\ >
   - **TTL:** 300

   Les noms des champs des enregistrements DNS varient par le serveur d’inscriptions de nom de domaine, mais ce processus entraînera un enregistrement TXT nommé _msradc; \ < nom_domaine > (par exemple, _msradc.contoso.com) qui a une valeur de flux RSS pour les services Bureau à distance Web complète.

Voilà! Lance l’application de bureau à distance sur votre appareil et s’abonner vous-même.