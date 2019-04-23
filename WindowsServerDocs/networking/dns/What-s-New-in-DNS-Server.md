---
title: Nouveautés du serveur DNS dans Windows Server
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités de serveur DNS dans Windows Server 2016 et versions ultérieures
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 665e411eda834a59c6dbe3581611b9b58bd006f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833570"
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Nouveautés du serveur DNS dans Windows Server

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les fonctionnalités de serveur de système DNS (Domain Name) qui sont nouvelles ou modifiées dans Windows Server 2016.  
  
Dans Windows Server 2016, le serveur DNS propose une prise en charge améliorée dans les domaines suivants.  
  
|Fonctionnalité|Nouveauté ou amélioration|Description|  
|-----------------|-------------------|---------------|  
|Stratégies DNS|Nouveau|Vous pouvez configurer des stratégies DNS pour spécifier comment un serveur DNS répond aux requêtes DNS. Les réponses DNS peuvent reposer sur l’adresse IP du client (emplacement), l’heure de la journée et plusieurs autres paramètres. Les stratégies DNS permettent la géolocalisation DNS, gestion du trafic, l’équilibrage de charge, DNS « split brain » et autres scénarios.|  
|Taux de réponse de limitation (RRL)|Nouveau|Vous pouvez activer la limitation du débit réponse sur vos serveurs DNS. Ce faisant, vous évitez la possibilité de systèmes malveillants à l’aide de vos serveurs DNS pour lancer une attaque par déni de service sur un client DNS.|  
|Authentification basée sur DNS d’entités nommées (DANE)|Nouveau|Vous pouvez utiliser des enregistrements TLSA (authentification de Transport Layer Security) pour fournir des informations aux clients DNS qui état quelle autorité de certification doivent-ils en attendre un certificat à partir de votre nom de domaine. Cela empêche les attaques man-in-the-middle où quelqu'un peut endommager le cache DNS pour pointer vers son propre site Web et fournir une attestation à partir d’une autre autorité de certification.|  
|Prise en charge de l’enregistrement inconnu|Nouveau|Vous pouvez ajouter des enregistrements qui ne sont pas explicitement prises en charge par le serveur DNS de Windows à l’aide de la fonctionnalité d’enregistrement inconnu.|  
|Indications de racine d’IPv6|Nouveau|Vous pouvez utiliser le protocole IPV6 natif prend en charge des indications de racine pour effectuer la résolution de noms internet à l’aide de serveurs racine IPV6.|  
|Prise en charge de Windows PowerShell|Amélioration|Nouvelles applets de commande Windows PowerShell sont disponibles pour le serveur DNS.|  
  
## <a name="dns-policies"></a>Stratégies DNS

Vous pouvez utiliser une stratégie DNS pour l’emplacement géographique en fonction de gestion du trafic, les réponses DNS intelligentes basées sur l’heure de la journée, pour gérer un seul serveur DNS configuré pour le fractionnement\-déploiement cerveau, application de filtres sur les requêtes DNS et bien plus encore. Les éléments suivants fournissent plus de détails sur ces fonctionnalités.

-   **Équilibrage de charge application.** Lorsque vous avez déployé plusieurs instances d’une application à différents emplacements, vous pouvez utiliser une stratégie DNS pour équilibrer la charge du trafic entre les différentes instances d’application, attribution dynamique de la charge du trafic de l’application.

-   **Geo\-emplacement en fonction de gestion du trafic.** Vous pouvez utiliser une stratégie DNS pour autoriser les serveurs DNS principaux et secondaires répondre aux requêtes du client DNS basés sur l’emplacement géographique du client et la ressource à laquelle le client tente de se connecter, en fournissant le client avec l’adresse IP de la plus proche ressource. 

-   **Fractionnement cerveau DNS.** Fractionnement\-du cerveau DNS, enregistrements DNS sont divisées en différentes zones étendues sur le même serveur DNS, et les clients DNS reçoivent une réponse selon que les clients sont des clients internes ou externes. Vous pouvez configurer le fractionnement\-du cerveau DNS pour les zones intégrées à Active Directory ou pour les zones sur des serveurs DNS autonomes.

-   **De filtrage.** Vous pouvez configurer une stratégie DNS pour créer des filtres de requête qui sont basées sur des critères que vous fournissez. Filtres de requête dans une stratégie DNS vous autorise à configurer le serveur DNS pour répondre de manière personnalisée en fonction de la requête DNS et le client DNS qui envoie la requête DNS. 
-   **Investigation.** Vous pouvez utiliser une stratégie DNS pour rediriger les clients DNS malveillants vers un non\-existant adresse IP au lieu de les diriger vers l’ordinateur, il essaie d’atteindre.

-   **Heure du jour en fonction de la redirection.** Vous pouvez utiliser une stratégie DNS pour distribuer le trafic d’application entre différentes instances distribuées d’une application à l’aide de stratégies DNS qui sont basés sur l’heure du jour. 
  
Vous pouvez également utiliser des stratégies DNS pour le DNS intégré à Active Directory zones.

Pour plus d’informations, consultez le [Guide de scénario de stratégie DNS](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>Limitation des taux de réponse

Vous pouvez configurer les paramètres de RRL pour contrôler la façon de répondre aux demandes d’un client DNS lorsque votre serveur reçoit plusieurs demandes ciblant le même client. Ce faisant, vous pouvez empêcher un utilisateur d’envoyer une attaque par déni de Service (Dos) à l’aide de vos serveurs DNS. Par exemple, un réseau de robots peut envoyer des demandes à votre serveur DNS à l’aide de l’adresse IP d’un troisième ordinateur en tant que le demandeur. Sans RRL, vos serveurs DNS peuvent répondre à toutes les demandes, inonder le troisième ordinateur. Lorsque vous utilisez RRL, vous pouvez configurer les paramètres suivants :  
  
-   **Réponses par seconde**. Il s’agit du nombre maximal de fois qu'aura la même réponse à un client au sein d’une seconde.  
  
-   **Erreurs par seconde**. Il s’agit du nombre maximal de fois où qu'une réponse d’erreur sera envoyée au client même au sein d’une seconde.  
  
-   **Fenêtre**. Ceci est le nombre de secondes pour lequel les réponses à un client seront suspendus si trop de demandes est effectuées.  
  
-   **Taux de fuite**. Il s’agit de la fréquence à laquelle le serveur DNS répond à une requête pendant le temps de réponses sont suspendus. Par exemple, si le serveur interrompt les réponses à un client pendant 10 secondes, et le taux de fuite est 5, le serveur répond toujours à une seule requête pour chaque 5 requêtes envoyées. Ainsi, les clients légitimes obtenir des réponses même lorsque le serveur DNS applique le taux de réponse de limitation de leur sous-réseau ou le nom de domaine complet.  
  
-   **Taux de TC**. Cela est utilisé pour indiquer au client de tester la connexion avec TCP lorsque les réponses au client sont suspendus. Par exemple, si le taux de TC est 3, et le serveur interrompt les réponses à un client donné, le serveur émet une demande de connexion TCP pour chaque 3 requêtes reçues. Assurez-vous que la valeur de taux de TC est inférieure au taux de fuite pour donner au client l’option pour vous connecter via TCP avant une fuite de réponses.  
  
-   **Réponses maximales**. Il s’agit du nombre maximal de réponses que le serveur émet à un client tandis que les réponses sont suspendus.  
  
-   **Domaines de la liste verte**. Il s’agit d’une liste de domaines à exclure de paramètres de RRL.  
  
-   **Liste blanche sous-réseaux**. Il s’agit d’une liste de sous-réseaux à exclure de paramètres de RRL.  
  
-   **Interfaces de serveur de liste blanche**. Il s’agit d’une liste d’interfaces de serveur DNS doit être exclu des paramètres de RRL.  
  
## <a name="dane-support"></a>Prise en charge DANE

Vous pouvez utiliser la prise en charge DANE \(RFC 6394 et 6698\) pour indiquer à vos clients DNS quelle autorité de certification doivent-ils en attendre certificats émises à partir des noms de domaines hébergés dans votre serveur DNS. Cela empêche une forme d’attaque man-in-the-middle où une personne est en mesure d’endommager un cache DNS et un nom DNS du point à leur propre adresse IP.  
  
Par exemple, imaginez que vous hébergez un site Web sécurisé qui utilise le protocole SSL à l’aide d’un certificat auprès d’une autorité bien connu nommé CA1 à www.contoso.com. Une personne peut toujours être en mesure d’obtenir un certificat pour www.contoso.com, à partir d’un différent, non donc-connue, certificat d’autorité nommée 2. Puis, l’entité qui héberge le site Web www.contoso.com faux peut être en mesure d’endommager le cache DNS d’un client ou un serveur pour pointer www.contoto.com vers leur site factice. L’utilisateur final s’afficheront un certificat à partir de 2 et peut simplement accuser réception et connectez-vous au faux site. Avec DANE, le client serait effectuer une demande au serveur DNS pour demander l’enregistrement TLSA de contoso.com et découvrez que le certificat pour www.contoso.com a des problèmes signalés par l’autorité de certification 1. Si un certificat à partir d’une autre autorité de certification, la connexion est abandonnée.  
  
## <a name="unknown-record-support"></a>Prise en charge de l’enregistrement inconnu

Un « enregistrement inconnu » est un enregistrement de ressource dont le format RDATA ne connaît pas le serveur DNS. La prise en charge ajoutée pour les types d’enregistrement inconnu (RFC 3597) signifie que vous pouvez ajouter les types d’enregistrements non pris en charge dans les zones de serveur DNS de Windows dans le format de câble binaire. Les fenêtres de programme de résolution de mise en cache possède déjà la possibilité de traiter des types d’enregistrement inconnu. Serveur Windows DNS n’effectue aucun traitement enregistrement spécifique pour les enregistrements inconnus, mais envoyer dans les réponses si la requête n’est reçue pour celui-ci.  
  
## <a name="ipv6-root-hints"></a>Indications de racine d’IPv6

Les indications de racine IPV6, publié par l’IANA, ont été ajoutées au serveur DNS windows. Les requêtes de noms internet peuvent maintenant utiliser les serveurs racine IPv6 pour effectuer des résolutions de noms.

## <a name="windows-powershell-support"></a>Prise en charge de Windows PowerShell

Le suivant nouvelles applets de commande Windows PowerShell et les paramètres sont introduits dans Windows Server 2016.
  
-   **Add-DnsServerRecursionScope**. Cette applet de commande crée une nouvelle portée de la récursivité sur le serveur DNS. Étendues de récursivité sont utilisés par les stratégies DNS pour spécifier une liste de redirecteurs à utiliser dans une requête DNS.  
  
-   **Remove-DnsServerRecursionScope**. Cette applet de commande supprime les étendues de récursivité existantes.  
  
-   **Set-DnsServerRecursionScope**. Cette applet de commande modifie les paramètres d’une étendue existante de la récursivité.  
  
-   **Get-DnsServerRecursionScope**. Cette applet de commande récupère des informations sur les étendues de récursivité existantes.  
  
-   **Add-DnsServerClientSubnet**. Cette applet de commande crée un nouveau sous-réseau du client DNS. Sous-réseaux sont utilisés par les stratégies DNS pour identifier où se trouve un client DNS.  
  
-   **Remove-DnsServerClientSubnet**. Cette applet de commande supprime les sous-réseaux de client DNS existants.  
  
-   **Set-DnsServerClientSubnet**. Cette applet de commande modifie les paramètres d’un sous-réseau de client DNS existant.  
  
-   **Get-DnsServerClientSubnet**. Cette applet de commande récupère des informations sur les sous-réseaux de client DNS existants.  
  
-   **Add-DnsServerQueryResolutionPolicy**. Cette applet de commande crée une nouvelle stratégie de résolution de requête DNS. Stratégies de résolution des requêtes DNS sont utilisés pour spécifier comment, ou si une requête reçoit une réponse, selon différents critères.  
  
-   **Remove-DnsServerQueryResolutionPolicy**. Cette applet de commande supprime les stratégies existantes du DNS.  
  
-   **Set-DnsServerQueryResolutionPolicy**. Cette applet de commande modifie les paramètres d’une stratégie DNS existante.  
  
-   **Get-DnsServerQueryResolutionPolicy**. Cette applet de commande récupère des informations sur les stratégies existantes du DNS.  
  
-   **Enable-DnsServerPolicy**. Cette applet de commande permet de stratégies DNS existantes.  
  
-   **Disable-DnsServerPolicy**. Cette applet de commande désactive les stratégies existantes du DNS.  
  
-   **Add-DnsServerZoneTransferPolicy**. Cette applet de commande crée une nouvelle stratégie de transfert de zone de serveur DNS. Stratégies de transfert de zone DNS spécifient s’il faut refuser ou ignorer un transfert de zone selon différents critères.  
  
-   **Remove-DnsServerZoneTransferPolicy**. Cette applet de commande supprime les stratégies du transfert de zone du serveur DNS existants.  
  
-   **Set-DnsServerZoneTransferPolicy**. Cette applet de commande modifie les paramètres d’une stratégie de transfert de zone de serveur DNS existante.  
  
-   **Get-DnsServerResponseRateLimiting**. Cette applet de commande récupère les paramètres de RRL.  
  
-   **Set-DnsServerResponseRateLimiting**. Cette applet de commande modifie les paramètres RRL.  
  
-   **Add-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande crée une liste d’exceptions RRL sur le serveur DNS.  
  
-   **Get-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande récupère les listes d’excception RRL.  
  
-   **Remove-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande supprime une liste d’exceptions RRL existante.  
  
-   **Set-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande modifie les listes d’exception RRL.  
  
-   **Add-DnsServerResourceRecord**. Cette applet de commande a été mis à jour pour prendre en charge le type d’enregistrement inconnu.  
  
-   **Get-DnsServerResourceRecord**. Cette applet de commande a été mis à jour pour prendre en charge le type d’enregistrement inconnu.  
  
-   **Remove-DnsServerResourceRecord**. Cette applet de commande a été mis à jour pour prendre en charge le type d’enregistrement inconnu.  
  
-   **Set-DnsServerResourceRecord**. Cette applet de commande a été mis à jour pour prendre en charge le type d’enregistrement inconnu

Pour plus d’informations, consultez les rubriques de référence de commande Windows Server 2016 Windows PowerShell suivantes.

- [Module de DnsServer](https://docs.microsoft.com/powershell/module/dnsserver/?view=win10-ps)
- [Module de DnsClient](https://docs.microsoft.com/powershell/module/dnsclient/?view=win10-ps)

## <a name="see-also"></a>Voir aussi  
  
-   [Quelles sont les nouveautés dans le Client DNS](What-s-New-in-DNS-Client.md)  
  

  

