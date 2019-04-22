---
title: Résolution des problèmes d’AD FS - résolution DNS
description: Ce document explique comment résoudre les aspects DNS des services AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e0065feac4241b617b8b13c6867d5dc36634bd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815900"
---
# <a name="ad-fs-troubleshooting---dns"></a>Résolution des problèmes d’AD FS - DNS 
Une des premières choses à vérifier, si AD FS n’est pas fonctionne ou ne répond, est la résolution de noms DNS.  Il s’agit des tests de base pour déterminer si les serveurs AD FS ou serveurs WAP sont détectés sur votre réseau.  Pour les utilisateurs internes, ces tests doivent résoudre pour les serveurs AD FS (STS).    Pour les utilisateurs externes, ces tests doivent résoudre pour les serveurs WAP.

Le reste de ce document explique comment effectuer certaines vérifications de résolution de nom rapide à l’aide des outils de ligne de commande.

## <a name="ping-test"></a>Test ping
Vérifie la connectivité de niveau IP vers un autre ordinateur TCP/IP en envoyant des messages de demande d’écho ICMP Internet Control Message Protocol (). La réception de messages de réponse à écho correspondants s’affichent, ainsi que les durées de boucles.  Pour plus d’informations, consultez [Ping](https://technet.microsoft.com/library/ff961503.aspx).


>[!NOTE]
>N’oubliez pas que certaines organisations bloquent ce port sur leurs serveurs et vous risquez d’obtenir un **demande dépassé** réponse.

### <a name="to-use-a-ping-test"></a>Pour utiliser un test PING
1.  Ouvrez une invite de commandes
2. Entrez PING <name of adfs server> un. Exemple :  PING sts.contoso.com
3. Vous devez voir une réponse du serveur

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
Affiche des informations que vous pouvez utiliser pour diagnostiquer l’infrastructure du système DNS (Domain Name).  Pour plus d’informations, consultez [NSLookup](https://technet.microsoft.com/library/cc725991.aspx).

### <a name="to-use-a-nslookup"></a>Pour utiliser un NSLookup
1.  Ouvrez une invite de commandes
2. Entrez PING <name of adfs server> un. Exemple : nslookup sts.contoso.com
3. Vous devez voir les informations dns pour le serveur ![NSLookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
Détermine le chemin emprunté vers une destination par envoi contrôle Message ICMP (Internet Protocol) Echo ou ICMPv6 vers la destination de façon incrémentielle plus en plus de temps aux valeurs de champ de vie (TTL).   Pour plus d’informations, consultez [Tracert](https://technet.microsoft.com/library/ff961507.aspx).


### <a name="to-use-tracert"></a>Utilisation de Tracert
1.  Ouvrez une invite de commandes
2. Entrez tracert <name of adfs server> un. Exemple : tracert sts.contoso.com
3. Vous devez voir le chemin d’accès de destination utilisé pour atteindre le serveur ![Tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)