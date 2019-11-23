---
title: Résolution des problèmes de AD FS-résolution DNS
description: Ce document explique comment dépanner les aspects DNS des AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7ffda6916bd91f1195ac0c289959becafff1d2c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407202"
---
# <a name="ad-fs-troubleshooting---dns"></a>Résolution des problèmes de AD FS-DNS 
L’une des premières choses à vérifier, si AD FS ne fonctionne pas ou ne répond pas, est la résolution de noms DNS.  Il s’agit de tests de base pour déterminer si les serveurs AD FS ou les serveurs WAP sont détectés sur votre réseau.  Pour les utilisateurs internes, ces tests doivent être résolus en serveurs AD FS (STS).    Pour les utilisateurs externes, ces tests doivent être résolus en serveurs WAP.

Le reste de ce document explique comment effectuer des vérifications de résolution de noms rapides à l’aide d’outils en ligne de commande.

## <a name="ping-test"></a>Test ping
Vérifie la connectivité de niveau IP à un autre ordinateur TCP/IP en envoyant des messages de demande d’écho ICMP (Internet Control Message Protocol). La réception des messages de réponse à écho correspondants est affichée, ainsi que les durées d’aller-retour.  Pour plus d’informations, consultez la [commande ping](https://technet.microsoft.com/library/ff961503.aspx).


>[!NOTE]
>Sachez que certaines organisations bloquent ce port sur leurs serveurs et que vous pouvez obtenir une réponse à expiration du délai d’attente de la **demande** .

### <a name="to-use-a-ping-test"></a>Pour utiliser un test PING
1.  Ouvrir une invite de commandes
2. Entrez PING <name of adfs server> a. Exemple : PING sts.contoso.com
3. Vous devez voir une réponse du serveur

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
Affiche des informations que vous pouvez utiliser pour diagnostiquer l’infrastructure DNS (Domain Name System).  Pour plus d’informations, consultez [nslookup](https://technet.microsoft.com/library/cc725991.aspx).

### <a name="to-use-a-nslookup"></a>Pour utiliser une NSLookup
1.  Ouvrir une invite de commandes
2. Entrez PING <name of adfs server> a. Exemple : nslookup sts.contoso.com
3. Vous devez voir les informations DNS pour le serveur ![NSLookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
Détermine le chemin d’accès à une destination en envoyant la demande d’écho ICMP (Internet Control Message Protocol) ou des messages ICMPv6 à la destination avec des valeurs de champ TTL (Time to Live) de manière incrémentielle.   Pour plus d’informations, consultez [tracert](https://technet.microsoft.com/library/ff961507.aspx).


### <a name="to-use-tracert"></a>Pour utiliser tracert
1.  Ouvrir une invite de commandes
2. Entrez tracert <name of adfs server> a. Exemple : tracert sts.contoso.com
3. Vous devez voir le chemin d’accès de destination utilisé pour atteindre le serveur ![tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)