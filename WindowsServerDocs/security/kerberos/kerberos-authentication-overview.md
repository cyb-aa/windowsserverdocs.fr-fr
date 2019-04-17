---
title: "Vue d’ensemble de l’authentification Kerberos"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 646c6309-e865-4be2-b415-44dd125af5c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 9f8878fb959c34663b1e1f3858fa7e0b6e7b6105
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-authentication-overview"></a>Vue d’ensemble de l’authentification Kerberos

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Kerberos est un protocole d’authentification qui est utilisé pour vérifier l’identité d’un utilisateur ou un ordinateur hôte. Cette rubrique contient des informations sur l’authentification Kerberos dans Windows Server2012 et Windows8.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
Les systèmes d’exploitation Windows Server implémentent le protocole d’authentification Kerberos version5 et des extensions pour l’authentification de clé publique, les données d’autorisation et la délégation de transport. Le client de l’authentification Kerberos est implémenté en tant qu’un fournisseur de support de sécurité \(SSP\) et il est accessible par le biais de l’Interface SSPI \(SSPI\). L’authentification utilisateur initiale est intégrée à l’architecture unique Winlogon sur connexion.

Le \(KDC\) centre de Distribution de clés Kerberos est intégré à d’autres services de sécurité de Windows Server qui s’exécutent sur le contrôleur de domaine. Le KDC utilise la base de données des Services de domaine ActiveDirectory du domaine en tant que sa base de données du compte de sécurité. Les Services de domaine ActiveDirectory est requis pour les implémentations Kerberos par défaut au sein du domaine ou de la forêt.

## <a name="kerb_tr_Kerb_Benefits"></a>Applications pratiques
Les avantages liés à l’aide de Kerberos pour l’authentification basée sur les domain\ sont:

-   **Authentification déléguée.**

    Services qui s’exécutent sur des systèmes d’exploitation Windows peuvent emprunter l’identité d’un ordinateur client lors de l’accès aux ressources sur le client. Dans de nombreux cas, un service peut effectuer son travail pour le client en accédant aux ressources sur l’ordinateur local. Lorsqu’un ordinateur client s’authentifie au service, protocoles NTLM et Kerberos fournissent les informations d’autorisation qu’un service a besoin pour emprunter l’identité de l’ordinateur client localement. Toutefois, certaines applications distribuées sont conçues afin qu’un service de bout en bout front\ doit utiliser l’identité de l’ordinateur client lorsqu’il se connecte aux services de bout en bout back\ sur d’autres ordinateurs. L’authentification Kerberos prend en charge un mécanisme de délégation qui permet à un service d’agir au nom de son client lors de la connexion à d’autres services.

-   **Authentification unique.**

    À l’aide de Kerberos l’authentification au sein d’un domaine ou dans une forêt permet à l’utilisateur ou l’accès aux ressources autorisées par les administrateurs en évitant plusieurs demandes d’informations d’identification. Après l’authentification initiale de domaine via Winlogon, Kerberos gère les informations d’identification tout au long de la forêt à chaque fois que les tentatives d’accès aux ressources.

-   **Interopérabilité.**

    L’implémentation du protocole Kerberos V5 par Microsoft est basée sur standards\-spécifications sont recommandées pour la Internet Engineering Task Force \(IETF\). Par conséquent, dans les systèmes d’exploitation Windows, le protocole Kerberos établit une base pour l’interopérabilité avec d’autres réseaux dans lesquels le protocole Kerberos est utilisé pour l’authentification. En outre, Microsoft publie la documentation relative aux protocoles Windows pour l’implémentation du protocole Kerberos. La documentation contient les spécifications techniques, les limitations, les dépendances et les comportement du protocole Windows spécifique pour l’implémentation Microsoft du protocole Kerberos.

-   **Authentification plus efficace auprès des serveurs.**

    Avant Kerberos, l’authentification NTLM pourrait être utilisée, ce qui nécessite un serveur d’applications pour se connecter à un contrôleur de domaine pour authentifier chaque ordinateur client ou un service. Avec le protocole Kerberos, session renouvelables remplacent pass\-par le biais de l’authentification. Le serveur n’est pas requis pour accéder à un contrôleur de domaine \ (sauf si elle doit valider une \(PAC\)\) privilège attribut certificat. Au lieu de cela, le serveur peut authentifier l’ordinateur client en examinant les informations d’identification présentées par le client. Les ordinateurs clients peuvent obtenir des informations d’identification pour un serveur en particulier une seule fois et puis réutiliser ces informations d’identification tout au long d’une ouverture de session réseau.

-   **Authentification mutuelle.**

    À l’aide du protocole Kerberos, une partie à chaque extrémité d’une connexion réseau peut vérifier que la partie à l’autre extrémité est l’entité qu’il prétend être. NTLM ne permet pas de clients vérifier l’identité d’un serveur ou un serveur vérifier l’identité d’un autre. L’authentification NTLM a été conçue pour un environnement réseau dans lequel les serveurs sont considérés comme authentiques. Le protocole Kerberos ne rend aucun ce genre d’hypothèse.

## <a name="see-also"></a>Voir aussi
[Vue d’ensemble de l’authentification Windows](../windows-authentication/windows-authentication-overview.md)


