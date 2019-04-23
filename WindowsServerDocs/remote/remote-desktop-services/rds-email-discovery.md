---
title: Configurer la découverte de courrier électronique pour vous abonner à votre flux de RDS
description: Découvrez comment intégrer Azure AD Domain Services dans votre déploiement des services Bureau à distance.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878320"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurer la découverte de courrier électronique pour vous abonner à votre flux de RDS

N’avez-vous jamais eu des problèmes pour l’obtention de vos utilisateurs finaux connectés à leur RDS publiée de flux, soit en raison d’un seul caractère manquant dans le flux URL ou parce qu’ils ont perdu le courrier électronique avec l’URL ? Presque toutes les applications du client Bureau à distance prend en charge la recherche de votre abonnement en entrant votre adresse de messagerie, rendant ainsi plus facile que jamais pour obtenir les utilisateurs connectés à leurs RemoteApps et les postes de travail.

>[!IMPORTANT]
>L’application de bureau à distance Microsoft dans le Microsoft Store ne prend pas en charge l’abonnement à l’adresse e-mail pour l’instant.

Avant de configurer la découverte de courrier électronique, procédez comme suit :

- Assurez-vous que vous êtes autorisé à ajouter un enregistrement TXT pour le domaine associé à votre adresse de messagerie (par exemple, si vos utilisateurs disposent @contoso.com adresses e-mail, vous avez besoin d’autorisations pour le domaine contoso.com)
- Créer un site Web Bureau à distance URL du flux (https://\<nom-dns-rdweb\>.domain/RDWeb/Feed/webfeed.aspx, tel que https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

Maintenant, suivez ces étapes pour configurer la découverte de courrier électronique :

1. Dans votre navigateur, connectez-vous au site Web de l’enregistrement de nom de domaine dans lequel votre domaine est enregistré.
2. Accédez à la page appropriée pour votre domaine enregistré, où vous pouvez afficher, ajouter et modifier des enregistrements DNS.
3. Entrez un nouvel enregistrement DNS avec les propriétés suivantes :
   - **Hôte :** _msradc
   - **Texte :** \<URL du flux Web Bureau à distance\>
   - **DURÉE DE VIE :** 300

   Les noms des champs d’enregistrements DNS varient selon le bureau d’enregistrement de nom de domaine, mais ce processus entraîne un enregistrement TXT nommé _msradc. \<nom_domaine\> (par exemple, _msradc.contoso.com) qui a la valeur du flux Web Bureau à distance complet.

C’est tout. À présent, lancez l’application de bureau à distance sur votre appareil et vous abonner vous-même !