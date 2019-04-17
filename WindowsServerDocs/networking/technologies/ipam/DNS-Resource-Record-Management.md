---
title: Gestion d’enregistrement de ressource DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ed781ef37243b80ea9da8aad27a29046b8dc8c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="dns-resource-record-management"></a>Gestion d’enregistrement de ressource DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique fournit des informations sur la gestion des enregistrements de ressource DNS à l’aide d’IPAM.  
  
> [!NOTE]  
> Outre cette rubrique, les rubriques de gestion des enregistrements de ressource DNS suivantes sont disponibles dans cette section.  
>   
> -   [Ajouter un enregistrement de ressource DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Supprimer les enregistrements de ressource DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtrer l’affichage des enregistrements de ressource DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Afficher les enregistrements de ressource DNS pour une adresse IP spécifique](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Vue d’ensemble de gestion des enregistrements de ressource  
Lorsque vous déployez IPAM dans Windows Server2016, vous pouvez effectuer la découverte de serveur pour ajouter des serveurs DHCP et DNS à la console de gestion de serveur IPAM. Le serveur IPAM puis dynamique collecte des données DNS toutes les six heures à partir des serveurs DNS est configuré pour gérer. IPAM gère une base de données local où elle stocke ces données DNS. IPAM fournit des notifications du jour et le moment où les données du serveur a été collectées, ainsi que pour vous indiquer le jour suivant et le moment lorsque la collecte des données à partir de serveurs DNS se produit.  
  
La barre d’état jaune dans l’illustration suivante montre l’emplacement dans l’interface utilisateur de notifications IPAM.  
  
![Notifications IPAM](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
Les données DNS qui sont collectées incluent la zone et ressource informations d’enregistrement DNS. Vous pouvez configurer IPAM pour collecter des informations de zone à partir de votre serveur DNS préféré.  IPAM collecte basée sur les fichiers et les zones active.  
  
> [!NOTE]  
> IPAM collecte des données uniquement à partir des serveurs DNS joints au domaine. Des serveurs DNS tiers et serveurs joints au domaine ne sont pas pris en charge par IPAM.  
  
Voici une liste des types d’enregistrements de ressource DNS qui sont collectées par IPAM.  
  
-   Base de données AFS  
  
-   Adresse ATM  
  
-   CNAME  
  
-   DHCID  
  
-   DNAME  
  
-   Ordinateur hôte A ou AAAA  
  
-   Informations sur l’hôte  
  
-   RNIS  
  
-   MX  
  
-   Serveurs de noms  
  
-   Pointeur (PTR)  
  
-   Responsable  
  
-   Repositionner  
  
-   Emplacement du service  
  
-   SOA  
  
-   SRV  
  
-   Texte  
  
-   Services connus  
  
-   WINS  
  
-   WINS-R  
  
-   X.25  
  
Dans Windows Server2016, IPAM offre une intégration entre l’inventaire d’adresses IP, les Zones DNS et les enregistrements de ressource DNS:  
  
-   Vous pouvez utiliser IPAM pour générer automatiquement un inventaire d’adresses IP à partir d’enregistrements de ressource DNS.  
  
-   Vous pouvez créer manuellement un inventaire d’adresses IP à partir de DNS A et les enregistrements de ressources AAAA.  
  
-   Vous pouvez afficher les enregistrements de ressource DNS pour une zone DNS spécifique et filtrer les enregistrements en fonction du type, les adresse IP, les données d’enregistrement de ressource et les autres options de filtrage.  
  
-   IPAM crée automatiquement un mappage entre les plages d’adresses IP et des Zones de recherche inversée DNS.  
  
-   IPAM crée des adresses IP pour les enregistrements PTR qui sont présentes dans la zone de recherche inversée et qui sont inclus dans la plage d’adresses IP. Vous pouvez également modifier ce mappage si nécessaire.  
  
IPAM vous permet d’effectuer les opérations suivantes sur les enregistrements de ressource à partir de la console IPAM.  
  
-   Créer des enregistrements de ressource DNS  
  
-   Modifier les enregistrements de ressource DNS  
  
-   Supprimer les enregistrements de ressource DNS  
  
-   Créer des enregistrements de ressource  
  
IPAM enregistre automatiquement toutes les modifications de configuration DNS que vous apportez à l’aide de la console IPAM.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer IPAM](Manage-IPAM.md)  
  


