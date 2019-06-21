---
title: Gestion d’enregistrement de ressource DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2db802fa928a4051fbe409fc0ba60d774bb0428
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284051"
---
# <a name="dns-resource-record-management"></a>Gestion d’enregistrement de ressource DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit des informations sur la gestion des enregistrements de ressource DNS à l’aide d’IPAM.  
  
> [!NOTE]  
> Outre cette rubrique, les rubriques de gestion des enregistrements de ressource DNS suivantes sont disponibles dans cette section.  
>   
> -   [Ajouter un enregistrement de ressource DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Supprimer des enregistrements de ressource DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtrer l’affichage des enregistrements de ressource DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Afficher les enregistrements de ressource DNS pour une adresse IP spécifique](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Vue d’ensemble de gestion des enregistrements de ressources  
Lorsque vous déployez IPAM dans Windows Server 2016, vous pouvez effectuer la découverte de serveur pour ajouter des serveurs DHCP et DNS à la console de gestion de serveur IPAM. Le serveur IPAM puis dynamiquement collecte des données DNS toutes les six heures à partir des serveurs DNS est configuré pour gérer. IPAM gère une base de données local où il stocke ces données DNS. IPAM vous offre la notification de la journée et le moment où les données du serveur ont été collectées, ainsi que de vous indiquant le jour suivant et le moment lorsque la collecte de données à partir de serveurs DNS se produit.  
  
La barre d’état jaune dans l’illustration suivante montre l’emplacement dans l’interface utilisateur de notifications d’IPAM.  
  
![Notifications IPAM](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
Les données DNS qui sont collectées incluent des ressources et la zone informations d’enregistrement DNS. Vous pouvez configurer IPAM pour collecter des informations de zone à partir de votre serveur DNS préféré.  IPAM collecte pour les zones basé sur fichier et Active Directory.  
  
> [!NOTE]  
> IPAM collecte des données uniquement à partir des serveurs DNS joints au domaine. Des serveurs DNS tiers et les serveurs joints à un domaine non ne sont pas pris en charge par IPAM.  
  
Voici une liste de types d’enregistrements de ressource DNS qui sont collectées par IPAM.  
  
-   Base de données AFS  
  
-   Adresse ATM  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   Hôte A ou AAAA  
  
-   Informations sur l’hôte  
  
-   ISDN  
  
-   MX  
  
-   Serveurs de noms  
  
-   Pointeur (PTR)  
  
-   Personne responsable  
  
-   Acheminer via  
  
-   Emplacement du service  
  
-   SOA  
  
-   SRV  
  
-   Text  
  
-   Services connus  
  
-   WINS  
  
-   WINS-R  
  
-   X.25  
  
Dans Windows Server 2016, IPAM offre une intégration entre l’inventaire d’adresses IP, des Zones DNS et des enregistrements de ressource DNS :  
  
-   Vous pouvez utiliser IPAM pour générer automatiquement un inventaire d’adresses IP à partir des enregistrements de ressource DNS.  
  
-   Vous pouvez créer manuellement un inventaire d’adresses IP à partir des enregistrements de ressources AAAA et A DNS.  
  
-   Vous pouvez afficher les enregistrements de ressource DNS pour une zone DNS spécifique et filtrer les enregistrements en fonction de type, les adresse IP, les données d’enregistrement de ressource et les autres options de filtrage.  
  
-   IPAM crée automatiquement un mappage entre les plages d’adresses IP et les Zones de recherche inversée DNS.  
  
-   IPAM crée des adresses IP pour les enregistrements PTR qui ne figurent pas dans la zone de recherche inversée et qui sont inclus dans cette plage d’adresses IP. Vous pouvez modifier également manuellement ce mappage si nécessaire.  
  
IPAM vous permet d’effectuer les opérations suivantes sur les enregistrements de ressource à partir de la console IPAM.  
  
-   Créer des enregistrements de ressource DNS  
  
-   Modifier des enregistrements de ressource DNS  
  
-   Supprimer des enregistrements de ressource DNS  
  
-   Créer des enregistrements de ressource associée  
  
IPAM enregistre automatiquement toutes les modifications de configuration de DNS que vous apportez à l’aide de la console IPAM.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer IPAM](Manage-IPAM.md)  
  


