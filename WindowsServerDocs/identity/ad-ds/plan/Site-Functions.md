---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Fonctions de site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a330a9dae8ab8d3b9de5d1fff0e52060908f520
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815810"
---
# <a name="site-functions"></a>Fonctions de site

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 utilise les informations de site à de nombreuses fins, notamment le routage de la réplication, l’affinité du client, réplication de système de volume (SYSVOL), espaces de noms de système de fichiers distribués (DFSN) et emplacement du service.  
  
## <a name="routing-replication"></a>Routage de la réplication  
Services de domaine Active Directory (AD DS) utilise une méthode de réplication multimaître, stockage et transfert. Un contrôleur de domaine communique les modifications d’annuaire à un second contrôleur de domaine, qui communique ensuite avec un tiers et ainsi de suite, jusqu'à ce que tous les contrôleurs de domaine ont reçu la modification. Pour optimiser l’équilibre entre la réduction de la latence de réplication et en réduisant le trafic, la topologie de site contrôle la réplication Active Directory en faire la distinction entre la réplication qui se produit au sein d’un site et de réplication qui se produit entre les sites.  
  
Dans les sites, la réplication est optimisée pour la vitesse, la réplication de déclencheur de mises à jour de données et les données sont envoyées sans la surcharge requise par la compression des données. À l’inverse, la réplication entre sites est compressée pour réduire le coût de transmission sur des liaisons réseau (étendu WAN). En cas de réplication entre sites, un seul contrôleur de domaine par domaine sur chaque site collecte et stocke les modifications d’annuaire et les transmet à une heure planifiée à un contrôleur de domaine dans un autre site.  
  
## <a name="client-affinity"></a>Affinité du client  
Contrôleurs de domaine utilisent les informations de site pour informer les clients Active Directory sur les contrôleurs de domaine présents dans le site le plus proche en tant que le client. Par exemple, considérez un client dans le site de Seattle qui ne connaît pas son affiliation de site et contacte un contrôleur de domaine à partir du site Atlanta. Selon l’adresse IP du client, le contrôleur de domaine à Atlanta détermine quel site, le client est en fait à partir d’et envoie les informations de site au client. Le contrôleur de domaine indique également au client si le contrôleur de domaine choisie est le plus proche à celui-ci. Le client met en cache les informations de site fournies par le contrôleur de domaine à Atlanta, les requêtes pour l’enregistrement de ressource spécifique au site de service (SRV) (un enregistrement de ressource système DNS (Domain Name) utilisé pour localiser les contrôleurs de domaine pour les services AD DS) et ce qui localise un domaine contrôleur au sein du même site.  
  
En recherchant un contrôleur de domaine dans le même site, le client évite des communications sur les liaisons WAN. Si aucun contrôleur de domaine n’est situés sur le site client, un contrôleur de domaine qui a les connexions de coût le plus bas par rapport à d’autres sites connectés publie lui-même (enregistre un enregistrement de ressource spécifique au site de service (SRV) dans DNS) dans le site qui n’a pas un contrôleur de domaine. Les contrôleurs de domaine qui sont publiés dans DNS sont ceux à partir du site le plus proche, tel que défini par la topologie de site. Ce processus garantit que chaque site dispose d’un contrôleur de domaine préféré pour l’authentification.  
  
Pour plus d’informations sur le processus de localisation d’un contrôleur de domaine, consultez collecte Active Directory ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)).  
  
## <a name="sysvol-replication"></a>Réplication SYSVOL  
SYSVOL est une collection de dossiers du système de fichiers qui existe sur chaque contrôleur de domaine dans un domaine. Les dossiers SYSVOL fournissent un emplacement d’Active Directory par défaut pour les fichiers qui doit être répliqué dans un domaine, y compris les objets de stratégie de groupe (GPO), les scripts de démarrage et arrêt et les scripts d’ouverture et fermeture de session.  Windows Server 2008 peuvent utiliser le Service de réplication de fichiers (FRS) ou la réplication de système de fichiers distribués (DFSR) pour répliquer les modifications apportées aux dossiers SYSVOL à partir d’un contrôleur de domaine vers d’autres contrôleurs de domaine. FRS et DFSR répliquent ces modifications en fonction de la planification que vous créez lors de votre conception de topologie de site.  
  
## <a name="dfsn"></a>DFSN  
DFSN utilise les informations de site pour diriger un client vers le serveur qui héberge les données demandées au sein du site. Si DFSN ne trouve pas une copie des données dans le même site que le client, DFSN utilise les informations de site dans AD DS pour déterminer quel serveur de fichiers qui a DFSN les données partagées sont plus proches du client.  
  
## <a name="service-location"></a>Emplacement du service  
En publiant des services tels que des fichiers et des services d’impression dans AD DS, vous autorisez les clients Active Directory localiser le service demandé dans le même ou le plus proche du site. Services d’impression utilisent l’attribut d’emplacement stockée dans AD DS pour permettre aux utilisateurs de rechercher un emplacement par défaut sans connaître leur emplacement précis. Pour plus d’informations sur la conception et le déploiement des serveurs d’impression, consultez Conception et déploiement de serveurs d’impression ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)).  
  


