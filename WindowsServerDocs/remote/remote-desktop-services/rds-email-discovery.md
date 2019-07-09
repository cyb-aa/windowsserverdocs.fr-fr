---
title: Configurer la découverte de courrier électronique pour vous abonner à votre flux RDS
description: Découvrez comment intégrer Azure AD Domain Services avec votre déploiement des services Bureau à distance.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712657"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>Configurer la découverte de courrier électronique pour vous abonner à votre flux RDS

Vous avez déjà rencontré des problèmes pour que vos utilisateurs se connectent à leur flux de services Bureau à distance publié, soit parce qu’il manquait un seul caractère l’URL du flux, soit ou parce que les utilisateurs avaient perdu le message contenant l’URL ? Presque toutes les applications du client Bureau à distance prennent en charge la recherche de votre abonnement en entrant votre adresse de messagerie, ce qui simplifie la connexion des utilisateurs à leurs applications distantes et leurs postes de travail.

>[!IMPORTANT]
>L’application Bureau à distance Microsoft dans le Microsoft Store ne prend pas actuellement en charge l’abonnement par adresse de messagerie.

Avant de configurer la découverte de courrier électronique, effectuez les opérations suivantes :

- Assurez-vous que vous êtes autorisé à ajouter un enregistrement TXT pour le domaine associé à votre adresse de messagerie (par exemple, si vos utilisateurs disposent d’adresses e-mail @contoso.com, vous avez besoin d’autorisations pour le domaine contoso.com)
- Créez une URL de flux web Bureau à distance (https://\<nom-dns-rdweb\>.domain/RDWeb/Feed/webfeed.aspx), par exemple https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

Maintenant, suivez ces étapes pour configurer la découverte d’adresse de messagerie :

1. Dans votre navigateur, connectez-vous au site web via lequel votre domaine est enregistré.
2. Accédez à la page appropriée de votre domaine, où vous pouvez afficher, ajouter et modifier des enregistrements DNS.
3. Entrez un nouvel enregistrement DNS avec les propriétés suivantes :
   - **Host:** _msradc
   - **Text:** \<RD Web Feed URL\>
   - **TTL:** 300

   Les noms des champs d’enregistrements DNS varient en fonction de la société d’enregistrement de nom de domaine, mais ce processus produit un enregistrement TXT appelé _msradc. \<nom_domaine\> (par exemple, _msradc.contoso.com) qui a la valeur d’un flux web Bureau à distance complet.

C’est tout. À présent, lancez l’application Bureau à distance sur votre appareil et abonnez-vous !