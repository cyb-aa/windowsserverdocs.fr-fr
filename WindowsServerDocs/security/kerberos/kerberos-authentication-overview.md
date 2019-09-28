---
title: Kerberos Authentication Overview
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 33712dc8502035bd9e47e1d2bdd4583eb8347dec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386306"
---
# <a name="kerberos-authentication-overview"></a>Kerberos Authentication Overview

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Kerberos est un protocole d’authentification utilisé pour vérifier l’identité d’un utilisateur ou d’un hôte. Cette rubrique contient des informations sur l’authentification Kerberos dans Windows Server 2012 et Windows 8.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
Les systèmes d’exploitation Windows Server implémentent le protocole d’authentification Kerberos version 5 et des extensions pour l’authentification des clés publiques, les données d’autorisation de transport et la délégation. Le client d’authentification Kerberos est implémenté en tant que fournisseur de support de sécurité @no__t 0SSP @ no__t-1, et il est accessible via l’interface du fournisseur de support de sécurité \(SSPI @ no__t-3. L’authentification initiale de l’utilisateur est intégrée à l’architecture Single Sign @ no__t-0ON de Winlogon.

Le centre de distribution de clés Kerberos \(KDC @ no__t-1 est intégré à d’autres services de sécurité Windows Server qui s’exécutent sur le contrôleur de domaine. Le KDC utilise la base de données de Active Directory Domain Services du domaine comme base de données du compte de sécurité. Les services de domaine Active Directory (AD DS) sont requis pour les implémentations Kerberos par défaut au sein du domaine ou de la forêt.

## <a name="kerb_tr_Kerb_Benefits"></a>Applications pratiques
Les avantages obtenus à l’aide de Kerberos pour l’authentification de domaine @ no__t-0based sont les suivants :

-   **Authentification déléguée.**

    Les services qui s’exécutent sur les systèmes d’exploitation Windows peuvent emprunter l’identité d’un ordinateur client lors de l’accès aux ressources au nom du client. Dans de nombreux cas, un service peut effectuer son travail pour le client en accédant aux ressources de l’ordinateur local. Lorsqu’un ordinateur client s’authentifie auprès du service, les protocoles NTLM et Kerberos fournissent les informations d’autorisation dont un service a besoin pour emprunter l’identité de l’ordinateur client localement. Toutefois, certaines applications distribuées sont conçues de façon à ce qu’un service frontal @ no__t-0end doive utiliser l’identité de l’ordinateur client lorsqu’il se connecte aux services @ no__t-1fin sur d’autres ordinateurs. L’authentification Kerberos prend en charge un mécanisme de délégation qui permet à un service d’agir au nom de son client lors de la connexion à d’autres services.

-   **Authentification unique.**

    L’utilisation de l’authentification Kerberos dans un domaine ou dans une forêt permet à l’utilisateur ou au service d’accéder aux ressources autorisées par les administrateurs en évitant plusieurs demandes d’informations d’identification. Après l’authentification initiale de domaine via Winlogon, Kerberos gère les informations d’identification dans la forêt lors de chaque tentative d’accès aux ressources.

-   **Interopérabilité.**

    L’implémentation du protocole Kerberos V5 par Microsoft est basée sur les spécifications standard @ no__t-0track, qui sont recommandées pour l’IETF (Internet Engineering Task Force) \(IETF @ no__t-2. Par conséquent, dans les systèmes d’exploitation Windows, le protocole Kerberos pose les bases d’une interopérabilité avec les autres réseaux où il est utilisé à des fins d’authentification. En outre, Microsoft publie la documentation relative aux protocoles Windows pour l’implémentation du protocole Kerberos. La documentation contient les spécifications techniques, les limitations, les dépendances et le comportement du protocole Windows @ no__t-0specific pour l’implémentation par Microsoft du protocole Kerberos.

-   **Authentification plus efficace auprès des serveurs.**

    Avant Kerberos, il était possible d’utiliser l’authentification NTLM, qui requiert un serveur d’applications pour se connecter à un contrôleur de domaine afin d’authentifier chaque ordinateur ou service client. Avec le protocole Kerberos, les tickets de session renouvelables remplacent l’authentification Pass @ no__t-0through. Le serveur n’a pas besoin d’accéder à un contrôleur de domaine \(unless il doit valider un certificat d’attribut de privilège \(PAC @ no__t-2 @ no__t-3. Au lieu de cela, le serveur peut authentifier l’ordinateur client en examinant les informations d’identification présentées par le client. Les ordinateurs clients peuvent obtenir des informations d’identification pour un serveur en particulier et les réutiliser au cours d’une session de connexion réseau.

-   **Authentification mutuelle.**

    À l’aide du protocole Kerberos, chacune des parties situées à chaque extrémité d’une connexion réseau peut vérifier que la partie distante est bien l’entité qu’elle prétend être. NTLM ne permet pas aux clients de vérifier l’identité d’un serveur ou de permettre à un serveur de vérifier l’identité d’un autre serveur. L’authentification NTLM a été conçue pour un environnement réseau dans lequel les serveurs sont considérés comme authentiques. Le protocole Kerberos n’utilise pas ce genre d’hypothèse.

## <a name="see-also"></a>Voir aussi
[Vue d’ensemble de l’authentification Windows](../windows-authentication/windows-authentication-overview.md)


