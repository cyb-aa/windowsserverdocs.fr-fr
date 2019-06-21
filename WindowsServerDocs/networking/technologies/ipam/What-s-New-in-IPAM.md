---
title: Nouveautés dans IPAM
description: Cette rubrique décrit les fonctionnalités de gestion des adresses IP (IPAM) qui sont nouvelles ou modifiées dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 64de9327dedadbe421e4cceb71496de3609be398
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283822"
---
# <a name="whats-new-in-ipam"></a>Nouveautés dans IPAM

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les fonctionnalités de gestion des adresses IP (IPAM) qui sont nouvelles ou modifiées dans Windows Server 2016.  
  
IPAM fournit des capacités d’administration et d’analyse hautement personnalisables pour l’adresse IP et une infrastructure DNS sur un réseau d’entreprise ou fournisseur de services Cloud (CSP). Vous pouvez surveiller, auditer et gérer les serveurs exécutant DHCP Dynamic Host Configuration Protocol () et le système DNS (Domain Name) à l’aide d’IPAM.  
  
## <a name="BKMK_IPAM2012R2"></a>Mises à jour dans le serveur IPAM  
Voici les fonctionnalités nouvelles et améliorées pour IPAM dans Windows Server 2016.  
  
|Fonctionnalité/fonction|Nouveauté ou amélioration|Description|  
|--------------------------|-------------------|---------------|  
|[Gestion améliorée des adresses IP](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Amélioration|Fonctionnalités d’IPAM sont améliorées pour les scénarios tels que la gestion des sous-réseaux /32 d’IPv4 et IPv6 /128 et recherche gratuits sous-réseaux d’adresses IP et plages dans un bloc d’adresses IP.|  
|[Gestion améliorée des services DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Nouveau|IPAM prend en charge l’enregistrement de ressource DNS redirecteur conditionnel et la gestion des zones DNS pour les serveurs DNS de l’intégré à Active Directory et la sauvegarde de fichiers joints au domaine.|  
|[Intégré DNS, DHCP et des adresses IP gestion de (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Amélioration|Plusieurs nouvelles expériences et les opérations de gestion du cycle de vie intégrée sont activées, telles que la visualisation des ressources DNS tous les enregistrements qui se rapportent à une adresse IP d’adresses, un inventaire automatisé des adresses IP en fonction des enregistrements de ressource DNS et la gestion du cycle de vie des adresses IP pour les opérations DNS et DHCP.|  
|[Prise en charge de plusieurs forêts Active Directory](#bkmk_ad)|Nouveau|Vous pouvez utiliser IPAM pour gérer les serveurs DNS et DHCP de plusieurs forêts Active Directory lorsqu’il existe une relation d’approbation bidirectionnelle entre la forêt où IPAM est installé et que chacune des forêts distantes.|  
|[Purger les données d’utilisation](#bkmk_purge)|Nouveau|Vous pouvez désormais purger les données d’utilisation de l’adresse IP qui sont antérieures à une date que vous spécifiez pour réduire la taille de la base de données IPAM.|  
|[Prise en charge de Windows PowerShell pour Role Based Access Control](#bkmk_ps)|Nouveau|Vous pouvez utiliser Windows PowerShell pour définir des étendues d’accès sur des objets IPAM.|  
  
### <a name="EIP"></a>Gestion améliorée des adresses IP  
Les fonctionnalités suivantes améliorent les fonctionnalités de gestion d’adresse IPAM.  
>[!NOTE]
>Pour la référence de commande IPAM Windows PowerShell, consultez [l’applets de commande du serveur de gestion des adresses IP (IPAM) dans Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Prise en charge de /31, /32 et /128 sous-réseaux  
IPAM dans Windows Server 2016 maintenant prend en charge /31, /32 et /128 sous-réseaux. Par exemple, un sous-réseau d’adresses de deux (/ 31 IPv4) peuvent être nécessaires pour une liaison de point à point entre les commutateurs. En outre, certains commutateurs peuvent nécessiter des adresses de bouclage unique (/ 32 pour IPv4, / 128 pour IPv6).  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**Rechercher les sous-réseaux gratuits avec IpamFreeSubnet de recherche**  
  
Cette commande renvoie les sous-réseaux qui sont disponibles pour l’allocation, étant donné un bloc IP, la longueur du préfixe et nombre de sous-réseaux demandées.   
  
Si le nombre de sous-réseaux disponibles est inférieur au nombre de sous-réseaux demandées, les sous-réseaux disponibles sont retournées avec un avertissement indiquant que le nombre disponible est inférieur au nombre demandé.  
  
>[!NOTE]
>Cette fonction n’alloue pas réellement les sous-réseaux, il signale uniquement leur disponibilité. Toutefois, la sortie de l’applet de commande peut être dirigée vers le **Add-IpamSubnet** commande pour créer le sous-réseau.  
  
Pour plus d’informations, consultez [Find-IpamFreeSubnet](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeSubnet).  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**Rechercher des plages d’adresses libres avec IpamFreeRange de recherche**  
  
Cette nouvelle commande retourne disponible plages d’adresses IP étant donné un sous-réseau IP, le nombre d’adresses qui sont nécessaires dans la plage et le nombre de plages demandé.   
  
La commande recherche une série continue d’adresses IP non alloués qui correspond au nombre d’adresses demandés. Le processus est répété jusqu'à ce que le nombre demandé de plages est trouvé, ou jusqu'à ce qu’il existe plus aucune adresse de plages d’adresses disponible.  
  
> [!NOTE]
> Cette fonction n’alloue pas réellement les plages d’adresses, il signale uniquement leur disponibilité. Toutefois, la sortie de l’applet de commande peut être dirigée vers le **Add-IpamRange** commande pour créer une plage.  
  
Pour plus d’informations, consultez [Find-IpamFreeRange](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeRange).  
  
### <a name="EDNS"></a>Gestion améliorée des services DNS  
IPAM dans Windows Server 2016 prend désormais en charge la découverte des serveurs DNS basé sur fichier joint au domaine dans une forêt Active Directory dans lequel IPAM est en cours d’exécution.  
  
En outre, les fonctions DNS suivantes ont été ajoutées :  
  
-   Zones DNS et des ressources des enregistrements collection (autres que celles se rapportant à DNSSEC) à partir de serveurs DNS exécutant Windows Server 2008 ou version ultérieure.  
  
-   Configurer (créer, modifier et supprimer) propriétés et opérations sur tous les types d’enregistrements de ressources (autres que celles se rapportant à DNSSEC).  
  
-   Configurer (créer, modifier, supprimer) les opérations sur tous les types de zones DNS, y compris le principal secondaire et les zones de Stub et les propriétés).  
  
-   Déclenche les tâches sur la base de données secondaire et les zones de stub, quel que soit si leur progression ou de zones de recherche inversée. Par exemple, les tâches telles que **transfert à partir de Master** ou **nouvelle copie de la zone de transfert à partir de Master**.  
  
-   Contrôle d’accès basé sur les rôles pour la configuration DNS pris en charge (enregistrements DNS et zones DNS).  
  
-   Configuration et collection de redirecteurs conditionnels (créer, supprimer, modifier).  
  
### <a name="DDI"></a>Intégré DNS, DHCP et des adresses IP gestion de (DDI)  
Lorsque vous affichez une adresse IP dans l’inventaire d’adresses IP, vous avez la possibilité de la vue de détails pour afficher tous les enregistrements de ressource DNS associés à l’adresse IP.  
  
Dans le cadre de la collection d’enregistrements de ressource DNS, IPAM collecte les enregistrements PTR pour les zones de recherche inversée DNS. Pour toutes les zones de recherche inversée qui sont mappés à toute plage d’adresses IP, IPAM crée les enregistrements d’adresses IP pour tous les enregistrements PTR appartenant à cette zone dans la plage d’adresses IP mappée correspondante. Si l’adresse IP existe déjà, l’enregistrement PTR est simplement associé à cette adresse IP. Les adresses IP ne sont pas créés automatiquement si la zone de recherche inversée n’est pas mappée à toute plage d’adresses IP.  
  
Création d’un enregistrement PTR dans une zone de recherche inversée par IPAM, l’inventaire d’adresses IP est mise à jour de la même façon que ci-dessus. Au cours de la collecte ultérieure, étant donné que l’adresse IP existe déjà dans le système, l’enregistrement PTR sera simplement mappé avec cette adresse IP.  
  
### <a name="bkmk_ad"></a>Prise en charge de plusieurs forêts Active Directory  
Dans Windows Server 2012 R2, IPAM a été en mesure de détecter et gérer des serveurs DNS et DHCP qui appartiennent à la même forêt Active Directory que le serveur IPAM. Vous pouvez désormais gérer les serveurs DNS et DHCP appartenant à une autre forêt AD lorsqu’il a une relation d’approbation bidirectionnelle avec la forêt où est installé le serveur IPAM. Vous pouvez accéder à la **configurer la découverte de serveur** boîte de dialogue zone et ajouter des domaines à partir de l’autre approuvé des forêts que vous souhaitez gérer. Une fois les serveurs sont découverts, l’expérience de gestion est la même que pour les serveurs qui appartiennent à la même forêt où IPAM est installé.  
  
Pour plus d’informations, consultez [gérer les ressources dans plusieurs forêts Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>Purger les données d’utilisation  
Purger les données de l’utilisation du vous permet de réduire la taille de la base de données IPAM en supprimant les anciennes données de l’utilisation d’adresses IP. Pour effectuer une suppression de données, vous spécifiez une date et IPAM supprime toutes les entrées de base de données qui sont antérieures à ou égale à la date que vous fournissez.   
  
Pour plus d’informations, consultez [purger les données d’utilisation](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="bkmk_ps"></a>Prise en charge de Windows PowerShell pour Role Based Access Control  
Vous pouvez maintenant utiliser Windows PowerShell pour configurer Role Based Access Control. Vous pouvez utiliser les commandes Windows PowerShell pour récupérer des objets DNS et DHCP dans IPAM et de modifier leurs étendues d’accès. Pour cette raison, vous pouvez écrire des scripts Windows PowerShell pour affecter des étendues d’accès aux objets suivants.  
  
-   Espace d’adressage IP  
  
-   Bloc d’adresses IP  
  
-   Sous-réseaux d’adresses IP  
  
-   Plages d'adresses IP  
  
-   Serveurs DNS  
  
-   Zones DNS  
  
-   Redirecteurs DNS conditionnels  
  
-   Enregistrements de ressource DNS  
  
-   Serveurs DHCP  
  
-   Les étendues globales DHCP  
  
-   Étendues DHCP  
  
Pour plus d’informations, consultez [gérer Role Based Access Control avec Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) et [l’applets de commande du serveur de gestion des adresses IP (IPAM) dans Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/).  

