---
title: Nouveautés dans IPAM
description: Cette rubrique décrit la fonctionnalité de gestion des adresses IP (IPAM) qui est nouvelle ou modifiée dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: get-started-article
ms.assetid: f2f2f1a5-ac2f-41b7-a495-98ad0e2a9b20
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5bf0dc2e55b1ff7d04045a860aa9816d47ee8417
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854792"
---
# <a name="whats-new-in-ipam"></a>Nouveautés dans IPAM

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit la fonctionnalité de gestion des adresses IP (IPAM) qui est nouvelle ou modifiée dans Windows Server 2016.  
  
IPAM offre des fonctionnalités d’administration et d’analyse hautement personnalisables pour l’adresse IP et l’infrastructure DNS sur un réseau d’entreprise ou de fournisseur de services Cloud (CSP). Vous pouvez surveiller, auditer et gérer les serveurs exécutant le protocole DHCP (Dynamic Host Configuration Protocol) et le système DNS (Domain Name System) à l’aide d’IPAM.  
  
## <a name="updates-in-ipam-server"></a><a name="BKMK_IPAM2012R2"></a>Mises à jour dans le serveur IPAM  
Voici les fonctionnalités nouvelles et améliorées d’IPAM dans Windows Server 2016.  
  
|Fonctionnalité/fonction|Nouveauté ou amélioration|Description|  
|--------------------------|-------------------|---------------|  
|[Gestion améliorée des adresses IP](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EIP)|Amélioration|Les fonctionnalités IPAM sont améliorées pour des scénarios tels que la gestion des sous-réseaux IPv4/32 et IPv6/128, ainsi que la recherche de sous-réseaux et de plages d’adresses IP libres dans un bloc d’adresses IP.|  
|[Gestion améliorée des services DNS](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#EDNS)|Nouveau|IPAM prend en charge les enregistrements de ressources DNS, les redirecteurs conditionnels et la gestion des zones DNS pour les serveurs DNS intégrés au domaine Active Directory et sauvegardés dans un fichier.|  
|[DNS intégré, DHCP et gestion des adresses IP (DDI)](../../technologies/ipam/../../technologies/ipam/../../technologies/ipam/What-s-New-in-IPAM.md#DDI)|Amélioration|Plusieurs nouvelles expériences et opérations de gestion du cycle de vie intégrées sont activées, telles que la visualisation de tous les enregistrements de ressources DNS qui se rapportent à une adresse IP, l’inventaire automatisé des adresses IP basées sur les enregistrements de ressources DNS et la gestion du cycle de vie des adresses IP. pour les opérations DNS et DHCP.|  
|[Prise en charge de plusieurs forêts Active Directory](#bkmk_ad)|Nouveau|Vous pouvez utiliser IPAM pour gérer les serveurs DNS et DHCP de plusieurs forêts Active Directory lorsqu’il existe une relation d’approbation bidirectionnelle entre la forêt où IPAM est installé et chacune des forêts distantes.|  
|[Purger les données d’utilisation](#bkmk_purge)|Nouveau|Vous pouvez maintenant réduire la taille de la base de données IPAM en purgeant les données d’utilisation des adresses IP antérieures à une date que vous spécifiez.|  
|[Prise en charge de Windows PowerShell pour les Access Control basés sur les rôles](#bkmk_ps)|Nouveau|Vous pouvez utiliser Windows PowerShell pour définir des étendues d’accès sur des objets IPAM.|  
  
### <a name="enhanced-ip-address-management"></a><a name="EIP"></a>Gestion améliorée des adresses IP  
Les fonctionnalités suivantes améliorent les fonctionnalités de gestion des adresses IPAM.  
>[!NOTE]
>Pour obtenir des informations de référence sur les commandes IPAM Windows PowerShell, consultez [applets de commande de serveur de gestion des adresses IP (IPAM) dans Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/).  
  
#### <a name="support-for-31-32-and-128-subnets"></a>Prise en charge des sous-réseaux/31,/32 et/128  
IPAM dans Windows Server 2016 prend désormais en charge les sous-réseaux/31,/32 et/128. Par exemple, un sous-réseau à deux adresses (/31 IPv4) peut être nécessaire pour une liaison point à point entre les commutateurs. En outre, certains commutateurs peuvent nécessiter des adresses de bouclage uniques (/32 pour IPv4,/128 pour IPv6).  
  
#### <a name="find-free-subnets-with-find-ipamfreesubnet"></a>**Rechercher des sous-réseaux gratuits avec Find-IpamFreeSubnet**  
  
Cette commande retourne les sous-réseaux qui peuvent être alloués, en fonction d’un bloc IP, d’une longueur de préfixe et d’un nombre de sous-réseaux demandés.   
  
Si le nombre de sous-réseaux disponibles est inférieur au nombre de sous-réseaux demandés, les sous-réseaux disponibles sont retournés avec un avertissement indiquant que le nombre disponible est inférieur au nombre demandé.  
  
>[!NOTE]
>Cette fonction n’alloue pas réellement les sous-réseaux, elle signale uniquement leur disponibilité. Toutefois, la sortie de l’applet de commande peut être dirigée vers la commande **Add-IpamSubnet** pour créer le sous-réseau.  
  
Pour plus d’informations, consultez [Find-IpamFreeSubnet](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeSubnet).  
  
#### <a name="find-free-address-ranges-with-find-ipamfreerange"></a>**Rechercher des plages d’adresses gratuites avec Find-IpamFreeRange**  
  
Cette nouvelle commande retourne les plages d’adresses IP disponibles en fonction d’un sous-réseau IP, du nombre d’adresses nécessaires dans la plage et du nombre de plages demandées.   
  
La commande recherche une série continue d’adresses IP non allouées qui correspondent au nombre d’adresses demandées. Le processus se répète jusqu’à ce que le nombre demandé de plages soit trouvé, ou jusqu’à ce qu’il n’y ait plus de plages d’adresses disponibles.  
  
> [!NOTE]
> Cette fonction n’alloue pas réellement les plages, elle signale uniquement leur disponibilité. Toutefois, la sortie de l’applet de commande peut être dirigée vers la commande **Add-IpamRange** pour créer la plage.  
  
Pour plus d’informations, consultez [Find-IpamFreeRange](https://docs.microsoft.com/powershell/module/ipamserver/Find-IpamFreeRange).  
  
### <a name="enhanced-dns-service-management"></a><a name="EDNS"></a>Gestion améliorée des services DNS  
IPAM dans Windows Server 2016 prend désormais en charge la détection de serveurs DNS basés sur des fichiers et joints à un domaine dans une forêt Active Directory dans laquelle IPAM s’exécute.  
  
En outre, les fonctions DNS suivantes ont été ajoutées :  
  
-   Les zones DNS et la collection d’enregistrements de ressources (autres que celles appartenant à DNSSEC) à partir de serveurs DNS exécutant Windows Server 2008 ou une version ultérieure.  
  
-   Configurez (créez, modifiez et supprimez) les propriétés et les opérations sur tous les types d’enregistrements de ressource (autres que ceux appartenant à DNSSEC).  
  
-   Configurer (créer, modifier, supprimer) les propriétés et les opérations sur tous les types de zones DNS, y compris les zones secondaires principales et les zones de stub).  
  
-   Tâches déclenchées sur les zones secondaires et les zones de stub, qu’il s’agit de zones de recherche directe ou inversée. Par exemple, des tâches telles que le **transfert à partir de Master** ou le **transfert d’une nouvelle copie de zone à partir de Master**.  
  
-   Contrôle d’accès en fonction du rôle pour la configuration DNS prise en charge (enregistrements DNS et zones DNS).  
  
-   La collection et la configuration des redirecteurs conditionnels (créer, supprimer, modifier).  
  
### <a name="integrated-dns-dhcp-and-ip-address-ddi-management"></a><a name="DDI"></a>DNS intégré, DHCP et gestion des adresses IP (DDI)  
Lorsque vous affichez une adresse IP dans l’inventaire d’adresses IP, vous avez l’option dans le mode Détails pour afficher tous les enregistrements de ressources DNS associés à l’adresse IP.  
  
Dans le cadre de la collecte d’enregistrements de ressources DNS, IPAM collecte les enregistrements PTR pour les zones de recherche inversée DNS. Pour toutes les zones de recherche inversée qui sont mappées à une plage d’adresses IP, IPAM crée les enregistrements d’adresse IP pour tous les enregistrements PTR appartenant à cette zone dans la plage d’adresses IP mappée correspondante. Si l’adresse IP existe déjà, l’enregistrement PTR est simplement associé à cette adresse IP. Les adresses IP ne sont pas créées automatiquement si la zone de recherche inversée n’est mappée à aucune plage d’adresses IP.  
  
Lorsqu’un enregistrement PTR est créé dans une zone de recherche inversée via IPAM, l’inventaire d’adresses IP est mis à jour de la même façon que décrit ci-dessus. Pendant la collecte suivante, étant donné que l’adresse IP existe déjà dans le système, l’enregistrement PTR sera simplement mappé avec cette adresse IP.  
  
### <a name="multiple-active-directory-forest-support"></a><a name="bkmk_ad"></a>Prise en charge de plusieurs forêts Active Directory  
Dans Windows Server 2012 R2, IPAM a pu détecter et gérer les serveurs DNS et DHCP appartenant à la même forêt de Active Directory que le serveur IPAM. Vous pouvez désormais gérer les serveurs DNS et DHCP appartenant à une forêt AD différente lorsqu’il a une relation d’approbation bidirectionnelle avec la forêt dans laquelle le serveur IPAM est installé. Vous pouvez accéder à la boîte de dialogue **configurer la découverte de serveurs** et ajouter des domaines à partir des autres forêts approuvées que vous souhaitez gérer. Une fois les serveurs découverts, l’expérience de gestion est la même que pour les serveurs qui appartiennent à la même forêt où IPAM est installé.  
  
Pour plus d’informations, consultez [gérer les ressources dans plusieurs forêts Active Directory](../../technologies/ipam/Manage-Resources-in-Multiple-Active-Directory-Forests.md)  
  
### <a name="purge-utilization-data"></a><a name="bkmk_purge"></a>Purger les données d’utilisation  
La purge des données d’utilisation vous permet de réduire la taille de la base de données IPAM en supprimant les anciennes données d’utilisation des adresses IP. Pour effectuer la suppression des données, vous spécifiez une date, et IPAM supprime toutes les entrées de base de données qui sont antérieures ou égales à la date que vous fournissez.   
  
Pour plus d’informations, consultez [purger les données d’utilisation](../../technologies/ipam/Purge-Utilization-Data.md).  
  
### <a name="windows-powershell-support-for-role-based-access-control"></a><a name="bkmk_ps"></a>Prise en charge de Windows PowerShell pour les Access Control basés sur les rôles  
Vous pouvez maintenant utiliser Windows PowerShell pour configurer des Access Control basées sur les rôles. Vous pouvez utiliser les commandes Windows PowerShell pour récupérer des objets DNS et DHCP dans IPAM et modifier leurs étendues d’accès. Pour cette raison, vous pouvez écrire des scripts Windows PowerShell pour affecter des étendues d’accès aux objets suivants.  
  
-   Espace d’adressage IP  
  
-   Bloc d’adresses IP  
  
-   Sous-réseaux d’adresses IP  
  
-   Plages d'adresses IP  
  
-   Serveurs DNS  
  
-   Zones DNS  
  
-   Redirecteurs conditionnels DNS  
  
-   Enregistrements de ressources DNS  
  
-   Serveurs DHCP  
  
-   Les étendues globales DHCP  
  
-   Étendues DHCP  
  
Pour plus d’informations, consultez [gérer les Access Control basés sur des rôles avec les](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) [applets de commande serveur de gestion des adresses IP (IPAM) et Windows PowerShell dans Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/).  

