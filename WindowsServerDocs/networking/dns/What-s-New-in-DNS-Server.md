---
title: Nouveautés du serveur DNS dans Windows Server
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités du serveur DNS dans Windows Server 2016 et versions ultérieures.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: de502d7be023d12e3350063e467a60356b2472c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406233"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Nouveautés du serveur DNS dans Windows Server

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique décrit les nouveautés et les modifications apportées à la fonctionnalité de serveur DNS (Domain Name System) dans Windows Server 2016.  
  
Dans Windows Server 2016, le serveur DNS offre une prise en charge améliorée dans les domaines suivants.  
  
|Fonctionnalité|Nouveauté ou amélioration|Description|  
|-----------------|-------------------|---------------|  
|Stratégies DNS|Nouveau|Vous pouvez configurer des stratégies DNS pour spécifier la façon dont un serveur DNS répond aux requêtes DNS. Les réponses DNS peuvent être basées sur l’adresse IP du client (emplacement), l’heure de la journée et plusieurs autres paramètres. Les stratégies DNS activent les DNS sensibles à l’emplacement, la gestion du trafic, l’équilibrage de charge, le DNS de fractionnement et d’autres scénarios.|  
|Limitation du taux de réponse (RRL)|Nouveau|Vous pouvez activer la limitation du taux de réponse sur vos serveurs DNS. En procédant ainsi, vous évitez la possibilité que des systèmes malveillants utilisent vos serveurs DNS pour lancer une attaque par déni de service sur un client DNS.|  
|Authentification DNS des entités nommées (n)|Nouveau|Vous pouvez utiliser des enregistrements TLSA (Transport Layer Security Authentication) pour fournir des informations aux clients DNS qui indiquent l’autorité de certification à partir de laquelle ils doivent s’attendre à recevoir un certificat pour votre nom de domaine. Cela empêche les attaques de l’intercepteur, où un utilisateur peut corrompre le cache DNS pour pointer vers son propre site Web et fournir un certificat qu’il a émis à partir d’une autre autorité de certification.|  
|Prise en charge des enregistrements inconnus|Nouveau|Vous pouvez ajouter des enregistrements qui ne sont pas explicitement pris en charge par le serveur DNS Windows à l’aide de la fonctionnalité d’enregistrement inconnu.|  
|Indications de racine IPv6|Nouveau|Vous pouvez utiliser la prise en charge des indications de racine IPV6 natives pour effectuer une résolution de noms Internet à l’aide des serveurs racine IPV6.|  
|Prise en charge de Windows PowerShell|Amélioration|De nouvelles applets de commande Windows PowerShell sont disponibles pour le serveur DNS.|  
  
## <a name="dns-policies"></a>Stratégies DNS

Vous pouvez utiliser la stratégie DNS pour la gestion du trafic basée sur la géolocalisation, les réponses DNS intelligentes en fonction de l’heure de la journée, pour gérer un serveur DNS unique configuré pour le déploiement Split @ no__t-0brain, l’application de filtres sur les requêtes DNS, etc. Les éléments suivants fournissent plus de détails sur ces fonctionnalités.

-   **Équilibrage de charge de l’application.** Lorsque vous avez déployé plusieurs instances d’une application à différents emplacements, vous pouvez utiliser la stratégie DNS pour équilibrer la charge du trafic entre les différentes instances d’application, en allouant de manière dynamique la charge du trafic pour l’application.

-   **La gestion du trafic basée sur la géo-no__t-1Location.** Vous pouvez utiliser une stratégie DNS pour permettre aux serveurs DNS principaux et secondaires de répondre aux requêtes du client DNS en fonction de l’emplacement géographique du client et de la ressource à laquelle le client tente de se connecter, en fournissant au client l’adresse IP la plus proche. ressource. 

-   **Fractionnement du DNS Brain.** Avec le DNS split @ no__t-0brain, les enregistrements DNS sont répartis en différentes étendues de zones sur le même serveur DNS, et les clients DNS reçoivent une réponse selon que les clients sont des clients internes ou externes. Vous pouvez configurer le service DNS split @ no__t-0brain pour Active Directory zones intégrées ou pour les zones sur des serveurs DNS autonomes.

-   **Filtration.** Vous pouvez configurer une stratégie DNS pour créer des filtres de requête basés sur des critères que vous fournissez. Les filtres de requête dans la stratégie DNS vous permettent de configurer le serveur DNS pour qu’il réponde de manière personnalisée en fonction de la requête DNS et du client DNS qui envoie la requête DNS. 
-   **Investigation.** Vous pouvez utiliser une stratégie DNS pour rediriger les clients DNS malveillants vers une adresse IP non-no__t-0existent au lieu de les rediriger vers l’ordinateur auquel ils essaient d’accéder.

-   **Redirection basée sur l’heure de la journée.** Vous pouvez utiliser une stratégie DNS pour distribuer le trafic d’application sur différentes instances géographiquement distribuées d’une application à l’aide de stratégies DNS basées sur l’heure de la journée. 
  
Vous pouvez également utiliser des stratégies DNS pour Active Directory zones DNS intégrées.

Pour plus d’informations, consultez le [Guide de scénarios de stratégie DNS](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>Limitation du taux de réponses

Vous pouvez configurer les paramètres RRL pour contrôler la façon de répondre aux demandes adressées à un client DNS lorsque votre serveur reçoit plusieurs demandes ciblant le même client. En procédant ainsi, vous pouvez empêcher une personne d’envoyer une attaque par déni de service (dos) à l’aide de vos serveurs DNS. Par exemple, un bot net peut envoyer des requêtes à votre serveur DNS à l’aide de l’adresse IP d’un troisième ordinateur en tant que demandeur. Sans RRL, vos serveurs DNS peuvent répondre à toutes les demandes en saturant le troisième ordinateur. Quand vous utilisez RRL, vous pouvez configurer les paramètres suivants :  
  
-   **Réponses par seconde**. Il s’agit du nombre maximal de fois que la réponse est donnée à un client dans un délai d’une seconde.  
  
-   **Erreurs par seconde**. Il s’agit du nombre maximal de fois qu’une réponse d’erreur sera envoyée au même client en une seconde.  
  
-   **Fenêtre**. Il s’agit du nombre de secondes pendant lesquelles les réponses à un client seront interrompues si le nombre de requêtes est trop important.  
  
-   **Taux de fuite**. Il s’agit de la fréquence à laquelle le serveur DNS répond à une requête pendant l’interruption des réponses. Par exemple, si le serveur interrompt les réponses à un client pendant 10 secondes et que le taux de fuite est de 5, le serveur continue de répondre à une requête pour toutes les 5 requêtes envoyées. Cela permet aux clients légitimes d’accéder aux réponses même lorsque le serveur DNS applique la limitation du taux de réponse sur leur sous-réseau ou nom de domaine complet.  
  
-   **Taux TC**. Cette valeur est utilisée pour indiquer au client d’essayer de se connecter avec TCP lorsque les réponses au client sont suspendues. Par exemple, si le taux TC est 3 et que le serveur interrompt les réponses à un client donné, le serveur émet une demande de connexion TCP pour toutes les 3 requêtes reçues. Assurez-vous que la valeur du taux TC est inférieure au taux de fuite, pour donner au client la possibilité de se connecter via TCP avant de divulguer des réponses.  
  
-   **Nombre maximal de réponses**. Il s’agit du nombre maximal de réponses que le serveur émettra à un client lorsque les réponses sont suspendues.  
  
-   **Domaines de liste blanche**. Il s’agit de la liste des domaines à exclure des paramètres RRL.  
  
-   **Répertorier les sous-réseaux blancs**. Il s’agit de la liste des sous-réseaux à exclure des paramètres RRL.  
  
-   **Interfaces de serveur de liste blanche**. Il s’agit de la liste des interfaces de serveur DNS à exclure des paramètres RRL.  
  
## <a name="dane-support"></a>Prise en charge de l’e

Vous pouvez utiliser la prise en @no__t charge de la fonction 0RFC-6394 et 6698 @ no__t-1 pour spécifier à vos clients DNS l’autorité de certification à partir de laquelle les certificats doivent être émis pour les noms de domaine hébergés sur votre serveur DNS. Cela empêche une forme d’attaque de l’intercepteur dans laquelle un utilisateur peut corrompre un cache DNS et faire pointer un nom DNS sur sa propre adresse IP.  
  
Par exemple, imaginez que vous hébergez un site Web sécurisé qui utilise SSL sur www.contoso.com à l’aide d’un certificat d’une autorité connue nommée CA1. Une personne peut toujours être en mesure d’obtenir un certificat pour www.contoso.com à partir d’une autorité de certification différente et non connue, nommée CA2. Ensuite, l’entité hébergeant le site Web factice www.contoso.com peut corrompre le cache DNS d’un client ou d’un serveur pour pointer www.contoto.com vers son site factice. L’utilisateur final reçoit un certificat à partir de CA2 et peut simplement le reconnaître et se connecter au site factice. Avec la valeur de l’enregistrement, le client envoie une demande au serveur DNS pour que contoso.com demande l’enregistrement TLSA et s’assure que le certificat pour www.contoso.com était un problème par CA1. S’il est présenté avec un certificat d’une autre autorité de certification, la connexion est abandonnée.  
  
## <a name="unknown-record-support"></a>Prise en charge des enregistrements inconnus

Un « enregistrement inconnu » est un RR dont le format RDATA n’est pas connu du serveur DNS. La prise en charge récemment ajoutée pour les types d’enregistrements inconnus (RFC 3597) signifie que vous pouvez ajouter les types d’enregistrements non pris en charge dans les zones de serveur DNS Windows au format binaire. Le programme de résolution de Windows Caching a déjà la possibilité de traiter les types d’enregistrements inconnus. Le serveur DNS Windows n’effectue aucun traitement spécifique des enregistrements pour les enregistrements inconnus, mais le renvoie aux réponses si des requêtes y sont reçues.  
  
## <a name="ipv6-root-hints"></a>Indications de racine IPv6

Les indications de racine IPV6, publiées par l’IANA, ont été ajoutées au serveur DNS Windows. Les requêtes de noms Internet peuvent désormais utiliser des serveurs racine IPv6 pour effectuer des résolutions de noms.

## <a name="windows-powershell-support"></a>Prise en charge de Windows PowerShell

Les nouvelles applets de commande et paramètres Windows PowerShell suivants sont introduits dans Windows Server 2016.
  
-   **Add-DnsServerRecursionScope**. Cette applet de commande crée une nouvelle étendue de récurrence sur le serveur DNS. Les étendues de récurrence sont utilisées par les stratégies DNS pour spécifier une liste de redirecteurs à utiliser dans une requête DNS.  
  
-   **Supprimez-DnsServerRecursionScope**. Cette applet de commande supprime les étendues de récurrence existantes.  
  
-   **Set-DnsServerRecursionScope**. Cette applet de commande modifie les paramètres d’une étendue de récursivité existante.  
  
-   **Accédez à DnsServerRecursionScope**. Cette applet de commande récupère des informations sur les étendues de récurrence existantes.  
  
-   **Add-DnsServerClientSubnet**. Cette applet de commande crée un sous-réseau client DNS. Les sous-réseaux sont utilisés par les stratégies DNS pour identifier l’emplacement d’un client DNS.  
  
-   **Supprimez-DnsServerClientSubnet**. Cette applet de commande supprime les sous-réseaux du client DNS existants.  
  
-   **Set-DnsServerClientSubnet**. Cette applet de commande modifie les paramètres d’un sous-réseau client DNS existant.  
  
-   **Accédez à DnsServerClientSubnet**. Cette applet de commande récupère des informations sur les sous-réseaux du client DNS existants.  
  
-   **Add-DnsServerQueryResolutionPolicy**. Cette applet de commande crée une stratégie de résolution de requêtes DNS. Les stratégies de résolution de requêtes DNS sont utilisées pour spécifier la manière dont une requête répond à, en fonction de différents critères.  
  
-   **Supprimez-DnsServerQueryResolutionPolicy**. Cette applet de commande supprime les stratégies DNS existantes.  
  
-   **Set-DnsServerQueryResolutionPolicy**. Cette applet de commande modifie les paramètres d’une stratégie DNS existante.  
  
-   **Accédez à DnsServerQueryResolutionPolicy**. Cette applet de commande récupère des informations sur les stratégies DNS existantes.  
  
-   **Enable-DnsServerPolicy**. Cette applet de commande active les stratégies DNS existantes.  
  
-   **Disable-DnsServerPolicy**. Cette applet de commande désactive les stratégies DNS existantes.  
  
-   **Add-DnsServerZoneTransferPolicy**. Cette applet de commande crée une stratégie de transfert de zone de serveur DNS. Les stratégies de transfert de zone DNS spécifient s’il faut refuser ou ignorer un transfert de zone selon différents critères.  
  
-   **Supprimez-DnsServerZoneTransferPolicy**. Cette applet de commande supprime les stratégies de transfert de zone de serveur DNS existantes.  
  
-   **Set-DnsServerZoneTransferPolicy**. Cette applet de commande modifie les paramètres d’une stratégie de transfert de zone de serveur DNS existante.  
  
-   **Accédez à DnsServerResponseRateLimiting**. Cette applet de commande récupère les paramètres RRL.  
  
-   **Set-DnsServerResponseRateLimiting**. Cette applet de commande modifie RRL paramètres.  
  
-   **Add-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande crée une liste d’exceptions RRL sur le serveur DNS.  
  
-   **Accédez à DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande récupère les listes de excception RRL.  
  
-   **Supprimez-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande supprime une liste d’exceptions RRL existante.  
  
-   **Set-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande modifie les listes d’exceptions RRL.  
  
-   **Add-DnsServerResourceRecord**. Cette applet de commande a été mise à jour pour prendre en charge un type d’enregistrement inconnu.  
  
-   **Accédez à DnsServerResourceRecord**. Cette applet de commande a été mise à jour pour prendre en charge un type d’enregistrement inconnu.  
  
-   **Supprimez-DnsServerResourceRecord**. Cette applet de commande a été mise à jour pour prendre en charge un type d’enregistrement inconnu.  
  
-   **Set-DnsServerResourceRecord**. Cette applet de commande a été mise à jour pour prendre en charge un type d’enregistrement inconnu

Pour plus d’informations, consultez les rubriques de référence sur les commandes Windows PowerShell suivantes de Windows Server 2016.

- [Module DnsServer](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [Module dnsclient Show State](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>Voir aussi  
  
-   [Nouveautés du client DNS](What-s-New-in-DNS-Client.md)  
  

  

