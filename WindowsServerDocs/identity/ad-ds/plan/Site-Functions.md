---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Fonctions de site
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f40122d84185c69fa19f5158c2b1c370ec1bfe82
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="site-functions"></a>Fonctions de site

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

 Windows Server2008 utilise les informations de site dans des buts, y compris le routage de la réplication, l’affinité du client, réplication du système de volume (SYSVOL), espaces de noms de système de fichiers distribués (DFSN) et l’emplacement du service.  
  
## <a name="routing-replication"></a>Réplication de routage  
Services de domaine ActiveDirectory (ADDS) utilise une méthode de réplication multimaître, stockage et de transfert. Un contrôleur de domaine communique les modifications d’annuaire à un deuxième contrôleur de domaine, puis communique avec un tiers et ainsi de suite jusqu'à ce que tous les contrôleurs de domaine ont reçu la modification. Pour obtenir le meilleur équilibre entre ce qui réduit la latence de réplication et en réduisant le trafic, topologie de site contrôle la réplication ActiveDirectory en faire la distinction entre la réplication se produit au sein d’un site et qui se produit entre les sites.  
  
Dans les sites, la réplication est optimisée pour déclencher la réplication speeddata mises à jour, et les données sont envoyées sans subir la surcharge requise par la compression des données. À l’inverse, la réplication entre sites est compressée pour réduire le coût de transmission sur des liaisons réseau (étendu WAN). Lorsque la réplication a lieu entre les sites, un seul contrôleur de domaine par domaine sur chaque site collecte et stocke les modifications d’annuaire et les transmet à une heure planifiée à un contrôleur de domaine dans un autre site.  
  
## <a name="client-affinity"></a>Affinité du client  
Contrôleurs de domaine utilisent les informations de site pour informer les clients ActiveDirectory sur les contrôleurs de domaine présents dans le site le plus proche en tant que le client. Par exemple, imaginons un client dans le site de Seattle qui ne sait pas son affiliation de site et contacte un contrôleur de domaine à partir du site Atlanta. En fonction de l’adresse IP du client, le contrôleur de domaine à Atlanta détermine quel site, le client est en réalité à partir d’et envoie les informations de site au client. Le contrôleur de domaine indique également le client si le contrôleur de domaine choisi est la plus proche à elle. Le client met en cache les informations de site fournies par le contrôleur de domaine à Atlanta, des requêtes pour l’enregistrement de ressource spécifique au site de service (SRV) (un enregistrement de ressource système DNS (Domain Name) utilisé pour localiser les contrôleurs de domaine pour les services ADDS) et recherche ainsi un contrôleur de domaine au sein du même site.  
  
En recherchant un contrôleur de domaine dans le même site, le client évite des communications sur les liaisons WAN. Si aucun contrôleur de domaine n’est situés sur le site client, un contrôleur de domaine qui a les connexions de coût le plus faible par rapport aux autres sites connectés publie lui-même (inscrit un enregistrement de ressource spécifique au site de service (SRV) dans le DNS) dans le site qui ne dispose pas d’un contrôleur de domaine. Les contrôleurs de domaine qui sont publiés dans DNS sont ceux à partir du site le plus proche, tel que défini par la topologie de site. Ce processus permet de s’assurer que chaque site dispose d’un contrôleur de domaine par défaut pour l’authentification.  
  
Pour plus d’informations sur le processus de localisation d’un contrôleur de domaine, voir collecte ActiveDirectory ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)).  
  
## <a name="sysvol-replication"></a>Réplication SYSVOL  
SYSVOL est un ensemble de dossiers dans le système de fichiers qui existe sur chaque contrôleur de domaine dans un domaine. Les dossiers SYSVOL fournissent un emplacement d’ActiveDirectory par défaut pour les fichiers qui doivent être répliquées dans tout un domaine, y compris les objets de stratégie de groupe (GPO), les scripts de démarrage et d’arrêt et les scripts d’ouverture de session et la fermeture de session.  Windows Server2008 peut utiliser le Service de réplication de fichiers (FRS) ou la réplication de système de fichiers distribués (DFSR) pour répliquer les modifications apportées aux dossiers SYSVOL d’un contrôleur de domaine vers d’autres contrôleurs de domaine. FRS et DFSR répliquent ces modifications conformément au calendrier que vous créez lors de votre conception de topologie de site.  
  
## <a name="dfsn"></a>DFSN  
DFSN utilise les informations de site pour diriger un client vers le serveur qui héberge les données demandées au sein du site. Si DFSN ne trouve pas une copie des données au sein du même site que le client, DFSN utilise les informations de site dans ADDS pour déterminer quel serveur de fichiers qui a DFSN partagée des données sont plus proches du client.  
  
## <a name="service-location"></a>Emplacement du service  
En publiant des services tels que des fichiers et des services d’impression dans ADDS, vous autorisez les clients ActiveDirectory localiser le service demandé dans le même ou le plus proche du site. Services d’impression utilisent l’attribut location stockée dans ADDS pour permettre aux utilisateurs de rechercher un emplacement par défaut sans connaître leur emplacement précis. Pour plus d’informations sur la conception et déploiement de serveurs d’impression, voir conception et déploiement de serveurs d’impression ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)).  
  


