---
title: Gestion d’enregistrement de ressource DNS
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b66c09d-e401-4f70-9a2a-6047dd629bfa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fe318a8ac17c650d8dbf2339e72b561de529c4a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405677"
---
# <a name="dns-resource-record-management"></a>Gestion d’enregistrement de ressource DNS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique fournit des informations sur la gestion des enregistrements de ressources DNS à l’aide d’IPAM.  
  
> [!NOTE]  
> En plus de cette rubrique, les rubriques suivantes relatives à la gestion des enregistrements de ressources DNS sont disponibles dans cette section.  
>   
> -   [Ajouter un enregistrement de ressource DNS](../../technologies/ipam/Add-a-DNS-Resource-Record.md)  
> -   [Supprimer des enregistrements de ressources DNS](../../technologies/ipam/Delete-DNS-Resource-Records.md)  
> -   [Filtrer l’affichage des enregistrements de ressources DNS](../../technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)  
> -   [Afficher les enregistrements de ressources DNS pour une adresse IP spécifique](../../technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-Address.md)  
  
## <a name="resource-record-management-overview"></a>Présentation de la gestion des enregistrements de ressources  
Lorsque vous déployez IPAM dans Windows Server 2016, vous pouvez effectuer la découverte du serveur pour ajouter des serveurs DHCP et DNS à la console de gestion du serveur IPAM. Le serveur IPAM collecte ensuite dynamiquement des données DNS toutes les six heures à partir des serveurs DNS qu’il est configuré pour gérer. IPAM gère une base de données locale dans laquelle il stocke ces données DNS. IPAM vous fournit une notification du jour et de l’heure de collecte des données du serveur, ainsi que la date et l’heure de la collecte des données à partir des serveurs DNS.  
  
La barre d’état jaune de l’illustration suivante montre l’emplacement de l’interface utilisateur des notifications IPAM.  
  
![Notifications IPAM](../../media/DNS-Resource-Record-Management/ipam_DataCollection_01.jpg)  
  
Les données DNS collectées incluent les informations de zone DNS et d’enregistrement de ressource. Vous pouvez configurer IPAM pour collecter des informations de zone à partir de votre serveur DNS préféré.  IPAM collecte à la fois les zones de fichiers et les zones de Active Directory.  
  
> [!NOTE]  
> IPAM collecte des données uniquement à partir de serveurs DNS Microsoft joints à un domaine. Les serveurs DNS tiers et les serveurs non joints à un domaine ne sont pas pris en charge par IPAM.  
  
La liste suivante répertorie les types d’enregistrements de ressources DNS qui sont collectés par IPAM.  
  
-   Base de données AFS  
  
-   Adresse ATM  
  
-   CANONIQUE  
  
-   DHCID  
  
-   DNAME  
  
-   Hôte A ou AAAA  
  
-   Informations sur l’hôte  
  
-   COMPATIBILITÉ  
  
-   MX  
  
-   Serveurs de noms  
  
-   Pointeur (PTR)  
  
-   Personne responsable  
  
-   Itinéraire  
  
-   Emplacement du service  
  
-   START  
  
-   SRV  
  
-   Text  
  
-   Services bien connus  
  
-   WINS  
  
-   WINS-R  
  
-   X. 25  
  
Dans Windows Server 2016, IPAM assure l’intégration entre l’inventaire d’adresses IP, les Zones DNS et les enregistrements de ressources DNS :  
  
-   Vous pouvez utiliser IPAM pour créer automatiquement un inventaire d’adresses IP à partir d’enregistrements de ressources DNS.  
  
-   Vous pouvez créer manuellement un inventaire d’adresses IP à partir des enregistrements de ressources DNS A et AAAA.  
  
-   Vous pouvez afficher les enregistrements de ressources DNS pour une zone DNS spécifique et filtrer les enregistrements en fonction du type, de l’adresse IP, des données d’enregistrement de ressource et d’autres options de filtrage.  
  
-   IPAM crée automatiquement un mappage entre les plages d’adresses IP et les zones de recherche inversée DNS.  
  
-   IPAM crée des adresses IP pour les enregistrements PTR présents dans la zone de recherche inversée et qui sont inclus dans cette plage d’adresses IP. Vous pouvez également modifier manuellement ce mappage, si nécessaire.  
  
IPAM vous permet d’effectuer les opérations suivantes sur les enregistrements de ressource à partir de la console IPAM.  
  
-   Créer des enregistrements de ressources DNS  
  
-   Modifier les enregistrements de ressources DNS  
  
-   Supprimer des enregistrements de ressource DNS  
  
-   Créer des enregistrements de ressources associés  
  
IPAM journalise automatiquement toutes les modifications de configuration DNS que vous effectuez à l’aide de la console IPAM.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer IPAM](Manage-IPAM.md)  
  


