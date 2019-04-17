---
title: Quelles sont les nouveautés dans IPAM
description: Cette rubrique décrit les fonctionnalités de gestion des adresses IP (IPAM) qui sont nouvelles ou modifiées dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b90cd1ab223e38cbf5933b58a594b32d5e3d4858
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ipam"></a>Quelles sont les nouveautés dans IPAM

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique décrit les fonctionnalités de gestion des adresses IP (IPAM) qui sont nouvelles ou modifiées dans Windows Server 2016.  
  
IPAM fournit des fonctionnalités d’administration et d’analyse hautement personnalisables pour l’adresse IP et l’infrastructure DNS sur un réseau d’entreprise ou le fournisseur de services Cloud (CSP). Vous pouvez analyser, auditer et gérer des serveurs exécutant DHCP Dynamic Host Configuration Protocol () et le système DNS (Domain Name) à l’aide d’IPAM.  
  
## <a name="BKMK_IPAM2012R2"></a>Mises à jour dans le serveur IPAM  
Voici les fonctionnalités nouvelles et améliorées pour IPAM dans Windows Server 2016.  
  
|Caractéristiques et fonctionnalités|Nouvelles ou améliorées|Description|  
|--------------------------|-------------------|---------------|  
|[Gestion améliorée des adresses IP](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Améliorée|Fonctionnalités d’IPAM sont améliorées pour les scénarios tels que la gestion des sous-réseaux /32 IPv4 et IPv6 /128 et recherche libres sous-réseaux d’adresses IP et plages dans un bloc d’adresses IP.|  
|[Gestion améliorée des services DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Nouveau|IPAM prend en charge l’enregistrement de ressource DNS redirecteur conditionnel et la gestion des zones DNS pour les serveurs DNS de l’intégré à Active Directory et reposant sur le fichier joint au domaine.|  
|[Intégrée DNS, DHCP et l’adresse IP gestion DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Améliorée|Plusieurs nouvelles expériences automatiser et gestion du cycle de vie intégrée opérations sont activées, telles que la visualisation de tous les enregistrements de ressource DNS qui se rapportent à une adresse IP, inventaire d’adresses IP basée sur les enregistrements de ressource DNS et la gestion du cycle de vie des adresses IP pour les opérations de DNS et DHCP.|  
|[Prise en charge de plusieurs forêts Active Directory](#bkmk_ad)|Nouveau|Vous pouvez utiliser IPAM pour gérer les serveurs DNS et DHCP de plusieurs forêts Active Directory lorsqu’il existe une relation d’approbation bidirectionnelle entre la forêt où IPAM est installé et chacune des forêts à distance.|  
|[Vider les données d’utilisation](#bkmk_purge)|Nouveau|Vous pouvez désormais réduire la taille de la base de données IPAM en purger les données d’utilisation de l’adresse IP qui sont antérieures à une date que vous spécifiez.|  
|[Prise en charge de Windows PowerShell pour Role Based Access Control](#bkmk_ps)|Nouveau|Vous pouvez utiliser Windows PowerShell pour définir des étendues d’accès sur des objets IPAM.|  
  
### <a name="EIP"></a>Gestion améliorée des adresses IP  
Les fonctionnalités suivantes améliorent les fonctionnalités de gestion des adresses IPAM.  
>[!NOTE]
>Pour la référence de commande IPAM Windows PowerShell, voir [applets de commande de serveur de gestion des adresses IP (IPAM) dans Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Prise en charge de /31 /32 et /128 sous-réseaux  
IPAM dans Windows Server 2016 maintenant prend en charge /31, /32 et /128 sous-réseaux. Par exemple, un sous-réseau de deux adresse (/ 31 IPv4) peuvent être nécessaires pour un lien de point à point entre les commutateurs. En outre, certains commutateurs peuvent nécessiter des adresses de bouclage unique (/ 32 pour IPv4, / 128 pour IPv6).  
  
#### **<a name="find-free-subnets-with-find-ipamfreesubnet"></a>Rechercher les sous-réseaux libres avec IpamFreeSubnet de recherche**  
  
Cette commande retourne les sous-réseaux qui sont disponibles pour une allocation, un bloc IP, la longueur du préfixe et un nombre de sous-réseaux demandées.   
  
Si le nombre de sous-réseaux disponibles est inférieur au nombre de sous-réseaux demandés, les sous-réseaux disponibles sont renvoyés avec un avertissement indiquant que le nombre disponible est inférieur au nombre demandé.  
  
>[!NOTE]
>Cette fonction n’attribue pas réellement les sous-réseaux, il ne signale que leur disponibilité. Toutefois, la sortie de l’applet de commande permettre être dirigée vers le **Add-IpamSubnet** commande pour créer le sous-réseau.  
  
Pour plus d’informations, voir [Find-IpamFreeSubnet](https://technet.microsoft.com/library/mt712782.aspx).  
  
#### **<a name="find-free-address-ranges-with-find-ipamfreerange"></a>Rechercher des plages d’adresses libres avec IpamFreeRange de recherche**  
  
Cette nouvelle commande renvoie disponible plages d’adresses IP donnée d’un sous-réseau IP, le nombre d’adresses qui sont requis dans la plage et le nombre de plages demandé.   
  
La commande recherche une série continue d’adresses IP non alloués qui correspond au nombre d’adresses demandés. Le processus est répété jusqu'à ce que le nombre de plages demandé n’est trouvé, ou jusqu'à ce qu’il existe plus aucune adresse de plages d’adresses disponible.  
  
> [!NOTE]
> Cette fonction n’attribue pas réellement les plages, il ne signale que leur disponibilité. Toutefois, la sortie de l’applet de commande permettre être dirigée vers le **Add-IpamRange** commande pour créer une plage.  
  
Pour plus d’informations, voir [Find-IpamFreeRange](https://technet.microsoft.com/library/mt712772.aspx).  
  
### <a name="EDNS"></a>Gestion améliorée des services DNS  
IPAM dans Windows Server 2016 prend désormais en charge la découverte des serveurs DNS basée sur les fichiers joints au domaine dans une forêt Active Directory dans lequel IPAM est en cours d’exécution.  
  
En outre, les fonctions DNS suivantes ont été ajoutées:  
  
-   Zones DNS et ressources enregistre la collection (autres que celles relatives à DNSSEC) à partir des serveurs DNS exécutant Windows Server 2008 ou version ultérieure.  
  
-   Configurer (créer, modifier et supprimer) propriétés et les opérations sur tous les types d’enregistrements de ressource (autres que celles relatives à DNSSEC).  
  
-   Configurer (créer, modifier, supprimer) propriétés et les opérations sur tous les types de zones DNS notamment secondaire principal et les zones de Stub).  
  
-   Déclenchée quel tâches sur l’ordinateur secondaire et les zones de stub, s’ils sont pour le transfert ou zones de recherche inversée. Par exemple, des tâches telles que **transférer à partir du maître** ou **transférer la nouvelle copie de zone à partir du maître**.  
  
-   Contrôle d’accès basé sur les rôles pour la configuration DNS pris en charge (enregistrements DNS et les zones DNS).  
  
-   Configuration et collecte de redirecteurs conditionnels (créer, supprimer, modifier).  
  
### <a name="DDI"></a>Intégrée DNS, DHCP et l’adresse IP gestion DDI)  
Lorsque vous affichez une adresse IP dans l’inventaire d’adresses IP, vous avez la possibilité dans l’affichage de détails pour voir tous les enregistrements de ressource DNS associés à l’adresse IP.  
  
Comme la collection d’enregistrements ressource partie DNS, IPAM collecte les enregistrements PTR pour les zones de recherche inversée DNS. Pour toutes les zones de recherche inversée qui sont mappées à une plage d’adresse IP, IPAM crée les enregistrements d’adresses IP pour tous les enregistrements PTR appartenant à cette zone dans la plage d’adresses IP mappée correspondante. Si l’adresse IP existe déjà, l’enregistrement PTR est simplement associé à cette adresse IP. Les adresses IP ne sont pas créés automatiquement si la zone de recherche inversée n’est pas mappée à une plage d’adresse IP.  
  
Un enregistrement PTR est créé dans une zone de recherche inversée par IPAM, l’inventaire d’adresses IP est mis à jour de la même façon comme décrit ci-dessus. Au cours de la collecte ultérieure, dans la mesure où l’adresse IP existe déjà dans le système, l’enregistrement PTR sera simplement mappé avec cette adresse IP.  
  
### <a name="bkmk_ad"></a>Prise en charge de plusieurs forêts Active Directory  
Dans Windows Server 2012 R2, IPAM a été en mesure de détecter et gérer les serveurs DNS et DHCP appartenant à la même forêt Active Directory que le serveur IPAM. Vous pouvez désormais gérer les serveurs DNS et DHCP appartenant à une autre forêt Active Directory lorsqu’il a une relation d’approbation bidirectionnelle avec la forêt où est installé le serveur IPAM. Vous pouvez accéder à la **configurer la découverte de serveur** boîte de dialogue zone et ajouter des domaines à partir de l’autre approuvés forêts que vous souhaitez gérer. Une fois les serveurs sont découverts, l’expérience de gestion est le même que pour les serveurs qui appartiennent à la même forêt où IPAM est installé.  
  
Pour plus d’informations, voir [gérer les ressources dans plusieurs forêts Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="bkmk_purge"></a>Vider les données d’utilisation  
Données d’utilisation de purge vous permet d’afin de réduire la taille de la base de données IPAM en supprimant les anciennes données de l’utilisation des adresses IP. Pour effectuer la suppression des données, vous spécifiez une date et IPAM supprime toutes les entrées de base de données qui sont plus anciennes qu’ou égale à la date que vous fournissez.   
  
Pour plus d’informations, voir [purger les données d’utilisation](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="bkmk_ps"></a>Prise en charge de Windows PowerShell pour Role Based Access Control  
Vous pouvez désormais utiliser Windows PowerShell pour configurer Role Based Access Control. Vous pouvez utiliser les commandes Windows PowerShell pour récupérer des objets de DNS et DHCP dans IPAM et modifier leurs étendues d’accès. Pour cette raison, vous pouvez écrire des scripts Windows PowerShell pour affecter des étendues d’accès pour les objets suivants.  
  
-   Espace d’adressage IP  
  
-   Bloc d’adresses IP  
  
-   Sous-réseaux d’adresses IP  
  
-   Plages d’adresses IP  
  
-   Serveurs DNS  
  
-   Zones DNS  
  
-   Redirecteurs DNS conditionnels  
  
-   Enregistrements de ressource DNS  
  
-   Serveurs DHCP  
  
-   Étendues globales DHCP  
  
-   Étendues DHCP  
  
Pour plus d’informations, voir [gérer Role Based Access Control avec Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) et [applets de commande de serveur de gestion des adresses IP (IPAM) dans Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  

