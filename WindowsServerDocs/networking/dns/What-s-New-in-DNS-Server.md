---
title: Nouveautés du serveur DNS dans Windows Server
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités de serveur DNS dans Windows Server2016 et versions ultérieures
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: c9cecb94-3cd5-4da7-9a3e-084148b8226b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ddb8920c045f231dcf5286283d9895ef6ffff47
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-server-in-windows-server"></a>Nouveautés du serveur DNS dans Windows Server

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique décrit les fonctionnalités de serveur de système DNS (Domain Name) qui sont nouvelles ou modifiées dans Windows Server 2016.  
  
Dans Windows Server 2016, le serveur DNS propose une prise en charge améliorée dans les domaines suivants.  
  
|Fonctionnalités|Nouvelles ou améliorées|Description|  
|-----------------|-------------------|---------------|  
|Stratégies DNS|Nouveau|Vous pouvez configurer des stratégies DNS pour spécifier comment un serveur DNS répond aux requêtes DNS. Les réponses DNS peuvent être basés sur l’adresse IP du client (emplacement), heure de la journée et plusieurs autres paramètres. Stratégies DNS activer DNS prenant en charge emplacement, la gestion du trafic, l’équilibrage de charge, DNS «split brain» et autres scénarios.|  
|Taux de limitation (RRL)|Nouveau|Vous pouvez activer la limitation de vitesse de réponse sur vos serveurs DNS. Ce faisant, vous évitez la possibilité de systèmes malveillants à l’aide de vos serveurs DNS pour lancer une attaque par déni de service sur un client DNS.|  
|Authentification basée sur DNS d’entités nommées (DANE)|Nouveau|Vous pouvez utiliser des enregistrements TLSA (Transport Layer Security authentification) pour fournir des informations sur les clients DNS qui indiquent quelle autorité de certification qu’ils doivent s’attendre un certificat à partir de votre nom de domaine. Cela empêche les attaques de man-in-the-middle où quelqu'un peut endommager le cache DNS pour pointer vers son propre site Web et fournir une attestation à partir d’une autre autorité de certification.|  
|Prise en charge de l’enregistrement inconnu|Nouveau|Vous pouvez ajouter des enregistrements qui ne sont pas explicitement prises en charge par le serveur DNS Windows à l’aide de la fonctionnalité d’enregistrement inconnu.|  
|Indications de racine IPv6|Nouveau|Vous pouvez utiliser le protocole IPV6 natif prend en charge des indications de racine pour effectuer la résolution de noms internet à l’aide de serveurs racine IPV6.|  
|Prise en charge de Windows PowerShell|Améliorée|Nouvelles applets de commande Windows PowerShell sont disponibles pour le serveur DNS.|  
  
## <a name="dns-policies"></a>Stratégies DNS

Vous pouvez utiliser une stratégie DNS pour la gestion du trafic basée géolocalisation, des réponses DNS intelligentes basées sur l’heure du jour, pour gérer un seul serveur DNS configuré pour le déploiement split\-brain, application de filtres sur les requêtes DNS et bien plus encore. Les éléments suivants fournissent plus de détails sur ces fonctionnalités.

-   **Application l’équilibrage de charge.** Lorsque vous avez déployé plusieurs instances d’une application à différents emplacements, vous pouvez utiliser une stratégie DNS pour équilibrer la charge du trafic entre les instances d’application différents, attribution dynamique de la charge du trafic de l’application.

-   **Emplacement de Geo\ en fonction de gestion du trafic.** Vous pouvez utiliser une stratégie DNS pour permettre à des serveurs DNS principaux et secondaires répondre aux requêtes de client DNS basées sur l’emplacement géographique du client et la ressource à laquelle le client tente de se connecter, en fournissant le client avec l’adresse IP de la ressource la plus proche. 

-   **Fractionnement cerveau DNS.** Avec split\-brain DNS, les enregistrements DNS sont réparties en différentes étendues de Zone sur le même serveur DNS, et les clients DNS recevoir une réponse si les clients sont des clients internes ou externes. Vous pouvez configurer split\-brain DNS pour les zones intégrées à Active Directory ou pour les zones sur des serveurs DNS autonomes.

-   **Le filtrage.** Vous pouvez configurer une stratégie DNS pour créer des filtres de requête qui sont basées sur des critères que vous fournissez. Filtres de requête dans une stratégie DNS vous autorise à configurer le serveur DNS pour répondre de manière personnalisée en fonction de la requête DNS et client DNS qui envoie la requête DNS. 
-   **Légales.** Vous pouvez utiliser une stratégie DNS pour rediriger les clients DNS malveillants à une adresse IP d’inexistantes autre que celle au lieu de les dirigeant vers l’ordinateur qu’il tente d’atteindre.

-   **Heure de la journée en fonction de redirection.** Vous pouvez utiliser une stratégie DNS pour distribuer le trafic des applications sur différentes instances dispersés géographiquement d’une application à l’aide de stratégies DNS qui sont basées sur l’heure du jour. 
  
Vous pouvez également utiliser des stratégies DNS pour DNS intégré à Active Directory zones.

Pour plus d’informations, voir la [Guide de scénario de stratégie DNS](deploy/DNS-Policies-Overview.md).

## <a name="response-rate-limiting"></a>Réponse de débit maximal

Vous pouvez configurer les paramètres de RRL pour contrôler comment répondre aux demandes pour un client DNS lorsque votre serveur reçoit plusieurs requêtes de cibler le même client. Ce faisant, vous pouvez empêcher une personne d’envoyer une attaque par déni de Service (Dos) à l’aide de vos serveurs DNS. Par exemple, un réseau de robots peut envoyer des demandes à votre serveur DNS à l’aide de l’adresse IP d’un troisième ordinateur en tant que le demandeur. Sans RRL, vos serveurs DNS peuvent répondre à toutes les demandes, inonder le troisième ordinateur. Lorsque vous utilisez RRL, vous pouvez configurer les paramètres suivants:  
  
-   **Réponses par seconde**. Il s’agit du nombre maximal de que la même réponse sera attribuée à un client au sein d’une seconde.  
  
-   **Erreurs par seconde**. Il s’agit du nombre maximal de fois où qu'une réponse d’erreur sera être envoyée sur le même client au sein d’une seconde.  
  
-   **Fenêtre**. Il s’agit du nombre de secondes pour lesquels des réponses à un client seront suspendues si trop de demandes sont effectuées.  
  
-   **Taux de fuite**. Il s’agit de la fréquence à laquelle le serveur DNS répond à une requête pendant la durée des réponses sont suspendus. Par exemple, si le serveur interrompt les réponses à un client pour 10 secondes, et le taux de fuite est 5, le serveur répond toujours à une requête pour toutes les 5 requêtes envoyées. Ainsi, les clients légitimes obtenir des réponses même lorsque le serveur DNS applique le taux de limitation sur leur sous-réseau ou le nom de domaine complet.  
  
-   **Taux de TC**. Cela est utilisé pour indiquer au client pour essayer de vous connecter avec TCP lorsque les réponses au client sont suspendus. Par exemple, si le taux de TC est de 3 et que le serveur interrompt les réponses à un client donné, le serveur émet une demande de connexion TCP pour chaque 3 requêtes reçues. Assurez-vous que la valeur de vitesse TC est inférieure à celle du taux de fuite, à donner au client la possibilité de se connecter via TCP avant une fuite de réponses.  
  
-   **Réponses maximales**. Il s’agit du nombre maximal de réponses que émet le serveur à un client tandis que les réponses sont suspendus.  
  
-   **Domaines de la liste blanche**. Il s’agit d’une liste de domaines à exclure de paramètres RRL.  
  
-   **Liste blanche sous-réseaux**. Il s’agit d’une liste des sous-réseaux à exclure de paramètres RRL.  
  
-   **Interfaces de serveur de liste blanche**. Il s’agit d’une liste des interfaces de serveur DNS doit être exclu des paramètres RRL.  
  
## <a name="dane-support"></a>Prise en charge DANE

Vous pouvez utiliser la prise en charge DANE \ (RFC 6394 et 6698\) pour indiquer à vos clients DNS qu’ils doivent s’attendre à être émise à partir des noms de domaines des certificats autorité de certification hébergés dans votre serveur DNS. Cela empêche une forme d’attaque man-in-the-middle où une personne est en mesure d’endommager un cache DNS et un nom DNS du point à leur propre adresse IP.  
  
Par exemple, imaginez que vous hébergez un site Web sécurisé qui utilise le protocole SSL à l’aide d’un certificat auprès d’une autorité bien connu nommé CA1 à www.contoso.com. Un utilisateur peut toujours être en mesure d’obtenir un certificat pour www.contoso.com à partir d’un autre, non afin-connu, certificat autorité nommé 2. Ensuite, l’entité qui héberge le site Web www.contoso.com faux peut être en mesure d’endommager le cache DNS d’un client ou un serveur pour pointer www.contoto.com vers leur site factice. L’utilisateur final est présenté un certificat à partir de 2 et peut simplement accusé de réception et connectez-vous au site factice. Le client serait DANE, effectuer une demande au serveur DNS pour contoso.com demandant pour l’enregistrement TLSA et savoir que le certificat pour www.contoso.com a des problèmes signalés par l’autorité de certification 1. Si un certificat à partir d’une autre autorité de certification, la connexion est abandonnée.  
  
## <a name="unknown-record-support"></a>Prise en charge de l’enregistrement inconnu

Un «enregistrement inconnu» est un enregistrement de ressource dont le format RDATA n’est pas connu pour le serveur DNS. La prise en charge nouvellement ajouté pour les types d’enregistrement inconnu (RFC 3597) signifie que vous pouvez ajouter les types d’enregistrement non pris en charge dans les zones serveur DNS Windows au format binaire simultanée. Les fenêtres de résolution du cache possède déjà la capacité de traiter les types d’enregistrement inconnu. Serveur DNS Windows ne suffit pas tout traitement enregistrement spécifique pour les enregistrements inconnus, mais envoie dans les réponses si les requêtes sont reçus pour lui.  
  
## <a name="ipv6-root-hints"></a>Indications de racine IPv6

Les indications de racine IPV6, publiées par IANA, ont été ajoutées au serveur DNS windows. Les requêtes de noms internet peuvent désormais utiliser les serveurs racine IPv6 pour effectuer la résolution de noms.

## <a name="windows-powershell-support"></a>Prise en charge de Windows PowerShell

Les paramètres suivants nouvelles applets de commande Windows PowerShell sont introduits dans Windows Server 2016.
  
-   **Ajouter-DnsServerRecursionScope**. Cette applet de commande crée une nouvelle étendue de la récursivité sur le serveur DNS. Les étendues de la récursivité sont utilisés par les stratégies DNS pour spécifier une liste de redirecteurs pour être utilisé dans une requête DNS.  
  
-   **Remove-DnsServerRecursionScope**. Cette applet de commande supprime les étendues de la récursivité existantes.  
  
-   **Set-DnsServerRecursionScope**. Cette applet de commande modifie les paramètres d’une étendue de la récursivité existante.  
  
-   **Get-DnsServerRecursionScope**. Cette applet de commande récupère les informations sur les étendues de la récursivité existant.  
  
-   **Ajouter-DnsServerClientSubnet**. Cette applet de commande crée un nouveau sous-réseau de client DNS. Sous-réseaux sont utilisés par les stratégies DNS pour identifier où se trouve un client DNS.  
  
-   **Remove-DnsServerClientSubnet**. Cette applet de commande supprime le sous-réseau de client DNS existant.  
  
-   **Set-DnsServerClientSubnet**. Cette applet de commande modifie les paramètres d’un sous-réseau de client DNS existant.  
  
-   **Get-DnsServerClientSubnet**. Cette applet de commande récupère des informations sur les sous-réseaux du client DNS existants.  
  
-   **Ajouter-DnsServerQueryResolutionPolicy**. Cette applet de commande crée une nouvelle stratégie de résolution de requête DNS. Stratégies de résolution de requête DNS sont utilisées pour spécifier comment, ou, si une requête est réponse, en fonction des critères différents.  
  
-   **Remove-DnsServerQueryResolutionPolicy**. Cette applet de commande supprime les stratégies de DNS.  
  
-   **Set-DnsServerQueryResolutionPolicy**. Cette applet de commande modifie les paramètres d’une stratégie DNS existante.  
  
-   **Get-DnsServerQueryResolutionPolicy**. Cette applet de commande récupère des informations sur les stratégies de DNS.  
  
-   **Enable-DnsServerPolicy**. Cette applet de commande permet de stratégies DNS existantes.  
  
-   **Disable-DnsServerPolicy**. Cette applet de commande désactive les stratégies de DNS.  
  
-   **Ajouter-DnsServerZoneTransferPolicy**. Cette applet de commande crée une nouvelle stratégie de transfert de zone de serveur DNS. Stratégies de transfert de zone DNS Indiquez si vous souhaitez refuser ou ignorer un transfert de zone en fonction de critères différents.  
  
-   **Remove-DnsServerZoneTransferPolicy**. Cette applet de commande supprime les stratégies du transfert de zone du serveur DNS existants.  
  
-   **Set-DnsServerZoneTransferPolicy**. Cette applet de commande modifie les paramètres d’une stratégie de transfert de zone de serveur DNS existante.  
  
-   **Get-DnsServerResponseRateLimiting**. Cette applet de commande récupère les paramètres de RRL.  
  
-   **Set-DnsServerResponseRateLimiting**. Cette applet de commande modifie RRL settigns.  
  
-   **Ajouter-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande crée une liste d’exceptions RRL sur le serveur DNS.  
  
-   **Get-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande récupère RRL excception listes.  
  
-   **Remove-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande supprime une liste d’exceptions RRL existante.  
  
-   **Set-DnsServerResponseRateLimitingExceptionlist**. Cette applet de commande modifie les listes d’exception RRL.  
  
-   **Ajouter-DnsServerResourceRecord**. Cette applet de commande a été mis à jour pour prendre en charge le type d’enregistrement inconnu.  
  
-   **Get-DnsServerResourceRecord**. Cette applet de commande a été mis à jour pour prendre en charge le type d’enregistrement inconnu.  
  
-   **Remove-DnsServerResourceRecord**. Cette applet de commande a été mis à jour pour prendre en charge le type d’enregistrement inconnu.  
  
-   **Set-DnsServerResourceRecord**. Cette applet de commande a été mis à jour pour prendre en charge le type d’enregistrement inconnu

Pour plus d’informations, consultez les rubriques de référence de commande Windows Server 2016, Windows PowerShell suivantes.

- [Module serveurDNS](https://technet.microsoft.com/itpro/powershell/windows/dns-server/index)
- [Module DnsClient](https://technet.microsoft.com/itpro/powershell/windows/dns-client/index)

## <a name="see-also"></a>Voir aussi  
  
-   [Quelles sont les nouveautés dans le Client DNS](What-s-New-in-DNS-Client.md)  
  

  

