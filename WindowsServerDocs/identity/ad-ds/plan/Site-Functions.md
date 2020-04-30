---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Fonctions de site
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 41fe13a5083a855a597a693f8cd707e3e211966a
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81623887"
---
# <a name="site-functions"></a>Fonctions de site

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 utilise des informations de site à de nombreuses fins, notamment la réplication de routage, l’affinité du client, la réplication du volume système (SYSVOL), les espaces de noms système de fichiers DFS (DFSN) et l’emplacement du service.

## <a name="routing-replication"></a>Réplication du routage
Active Directory Domain Services (AD DS) utilise une méthode multimaître, de stockage et de transfert de réplication. Un contrôleur de domaine communique les modifications d’annuaire à un deuxième contrôleur de domaine, qui communique ensuite avec un troisième, et ainsi de suite, jusqu’à ce que tous les contrôleurs de domaine aient reçu la modification. Pour obtenir le meilleur équilibre entre la réduction de la latence de réplication et la réduction du trafic, les contrôles de topologie de site Active Directory la réplication en distinguant la réplication qui se produit au sein d’un site et la réplication qui se produit entre les sites.

Dans les sites, la réplication est optimisée pour la vitesse, les mises à jour de données déclenchent la réplication et les données sont envoyées sans la surcharge requise par la compression des données. À l’inverse, la réplication entre les sites est compressée pour réduire le coût des transmissions sur des liaisons de réseau étendu (WAN). Lorsque la réplication a lieu entre des sites, un seul contrôleur de domaine par domaine sur chaque site collecte et stocke les modifications d’annuaire et les communique à une heure planifiée à un contrôleur de domaine d’un autre site.

## <a name="client-affinity"></a>Affinité du client
Les contrôleurs de domaine utilisent les informations de site pour informer Active Directory clients des contrôleurs de domaine présents dans le site le plus proche en tant que client. Par exemple, imaginez un client sur le site de Seattle qui ne connaît pas son affiliation à un site et qui contacte un contrôleur de domaine à partir du site Atlanta. En fonction de l’adresse IP du client, le contrôleur de domaine à Atlanta détermine le site à partir duquel le client provient et renvoie les informations du site au client. Le contrôleur de domaine informe également le client si le contrôleur de domaine choisi est le contrôleur de domaine le plus proche. Le client met en cache les informations de site fournies par le contrôleur de domaine à Atlanta, interroge l’enregistrement de ressource de service spécifique au site (SRV) (un enregistrement de ressource DNS (Domain Name System) utilisé pour localiser les contrôleurs de domaine pour AD DS) et trouve ainsi un contrôleur de domaine au sein du même site.

En recherchant un contrôleur de domaine dans le même site, le client évite les communications sur les liaisons de réseau étendu. Si aucun contrôleur de domaine n’est situé sur le site du client, un contrôleur de domaine qui dispose des connexions les moins coûteuses par rapport à d’autres sites connectés se publie lui-même (inscrit un enregistrement de ressource de service spécifique au site (SRV) dans DNS) sur le site qui n’a pas de contrôleur de domaine. Les contrôleurs de domaine qui sont publiés dans DNS sont ceux du site le plus proche, tel que défini par la topologie de site. Ce processus garantit que chaque site dispose d’un contrôleur de domaine préféré pour l’authentification.

Pour plus d’informations sur le processus de localisation d’un contrôleur de domaine, consultez [Active Directory Collection](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc780036(v=ws.10)).

## <a name="sysvol-replication"></a>Réplication SYSVOL
SYSVOL est un ensemble de dossiers dans le système de fichiers qui existe sur chaque contrôleur de domaine dans un domaine. Les dossiers SYSVOL fournissent un emplacement par défaut Active Directory pour les fichiers qui doivent être répliqués dans l’ensemble d’un domaine, y compris les objets stratégie de groupe (GPO), les scripts de démarrage et d’arrêt, ainsi que les scripts d’ouverture et de fermeture de session.  Windows Server 2008 peut utiliser le service de réplication de fichiers (FRS) ou la réplication de système de fichiers DFS (DFSR) pour répliquer les modifications apportées aux dossiers SYSVOL d’un contrôleur de domaine à d’autres contrôleurs de domaine. FRS et DFSR répliquent ces modifications en fonction de la planification que vous créez lors de la conception de la topologie de votre site.

## <a name="dfsn"></a>DFSN
DFSN utilise les informations de site pour diriger un client vers le serveur qui héberge les données demandées dans le site. Si DFSN ne trouve pas de copie des données dans le même site que le client, DFSN utilise les informations du site dans AD DS pour déterminer le serveur de fichiers qui contient les données partagées DFSN le plus proche du client.

## <a name="service-location"></a>Emplacement du service
En publiant des services tels que les services de fichiers et d’impression dans AD DS, vous permettez aux clients Active Directory de localiser le service demandé dans le même site ou le site le plus proche. Les services d’impression utilisent l’attribut emplacement stocké dans AD DS pour permettre aux utilisateurs de rechercher des imprimantes par emplacement sans connaître leur emplacement précis. Pour plus d’informations sur la conception et le déploiement de serveurs d’impression, consultez [conception et déploiement de serveurs d’impression](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc785842(v=ws.10)).
