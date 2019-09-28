---
title: Considérations matérielles dans le réglage des performances Active Directory
description: Considérations matérielles dans le réglage des performances Active Directory
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 8e9b121036d33bc36cabb92ca682407bc2382fca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355099"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>Considérations matérielles dans ajoute l’optimisation des performances 

>[!Important]
> Vous trouverez ci-dessous un résumé des recommandations et des considérations essentielles pour optimiser le matériel serveur pour les charges de travail de Active Directory couvertes plus en détail dans l’article [planification de la capacité pour Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) . Les lecteurs sont vivement encouragés à passer en revue la [planification de la capacité pour Active Directory Domain Services](https://go.microsoft.com/fwlink/?LinkId=324566) pour une meilleure compréhension technique et les implications de ces recommandations.

## <a name="avoid-going-to-disk"></a>Évitez d’accéder au disque

Active Directory met en cache la plus grande partie de la base de données que la mémoire l’autorise. L’extraction de pages à partir de la mémoire est plus rapide que le support physique, qu’il s’agisse d’un disque à base de disques ou SSD. Ajoutez de la mémoire pour réduire les e/s disque.

-   Active Directory meilleures pratiques recommandent de placer suffisamment de RAM pour charger l’ensemble du DIT dans la mémoire, ainsi que le système d’exploitation et d’autres applications installées, telles que les antivirus, les logiciels de sauvegarde, la surveillance, etc.

    -   Pour connaître les limitations des plateformes héritées, consultez [utilisation de la mémoire par le processus Lsass. exe sur les contrôleurs de domaine qui exécutent le serveur Windows server 2003 ou windows 2000](https://support.microsoft.com/kb/308356).

    -   Utilisez le compteur\\de performances durée de vie moyenne du cache en attente &gt; à long terme de la mémoire (s) 30 minutes.

-   Placez le système d’exploitation, les journaux et la base de données sur des volumes distincts. Si la totalité ou la majorité de la DIT peut être mise en cache, une fois le cache chauffé et dans un état stable, cela devient moins pertinent et offre un peu plus de flexibilité dans la disposition du stockage. Dans les scénarios où la DIT entière ne peut pas être mise en cache, l’importance du fractionnement du système d’exploitation, des journaux et de la base de données sur des volumes distincts devient plus importante.

-   Normalement, les ratios d’e/s du DIT sont d’environ 90% de lecture et de 10% d’écriture. Les scénarios où les volumes d’e/s d’écriture dépassent de manière significative 10%-20% sont considérés comme des écritures lourdes. Les scénarios à écriture intensive n’offrent pas beaucoup d’avantages du cache de Active Directory. Pour garantir la durabilité transactionnelle des données écrites dans le répertoire, Active Directory n’effectue pas de mise en cache d’écriture sur le disque. Au lieu de cela, elle valide toutes les opérations d’écriture sur le disque avant de renvoyer un état d’achèvement réussi pour une opération, sauf s’il existe une demande explicite qui ne le fait pas. Par conséquent, l’e/s disque rapide est importante pour les performances des opérations d’écriture sur Active Directory. Voici les recommandations matérielles qui peuvent améliorer les performances de ces scénarios :

    -   Contrôleurs RAID matériels

    -   Augmentez le nombre de disques à faible latence/RPM qui hébergent la DIT et les fichiers journaux.

    -   Cache d’écriture sur le contrôleur

-   Examinez les performances du sous-système de disque individuellement pour chaque volume. La plupart des Active Directory les scénarios sont principalement basés sur la lecture, donc les statistiques sur le volume qui héberge la DIT sont les plus importantes à inspecter. Toutefois, n’oubliez pas de surveiller le reste des lecteurs, y compris le système d’exploitation et les lecteurs de fichiers journaux. Pour déterminer si le contrôleur de domaine est correctement configuré pour éviter le goulot d’étranglement au niveau des performances, consultez la section sur les sous-systèmes de stockage pour obtenir des recommandations sur le stockage des normes. Dans de nombreux environnements, la philosophie consiste à s’assurer qu’il y a suffisamment d’espace pour supporter les pics ou les pics de charge. Ces seuils sont des seuils d’avertissement où la salle de la tête pour gérer les surtensions ou les pics de charge est limitée et les réactivités du client sont dégradées. En bref, le dépassement de ces seuils n’est pas défectueux à long terme (5 à 15 minutes, plusieurs fois par jour). Toutefois, un système qui s’exécute avec ces types de statistiques n’est pas entièrement Caching et peut être plus ou moins sollicité et doit être examiné.

    -   Base de données&gt; d’e/s base de\\données = = instances (LSASS/ntdsa) &lt; latence moyenne des lectures d’e/s 15ms

    -   Base de données&gt; = = instances (LSASS/\\ntdsa) lectures d’e/s &lt; de base de données/s 10

    -   Base de données&gt; = = instances (LSASS/\\ntdsa) latence moyenne des &lt; écritures de journal des e/s (10 ms)

    -   Base de données&gt; = = instances (LSASS/\\ntdsa) écritures du journal des e/s/s : informations uniquement.

        Pour maintenir la cohérence des données, toutes les modifications doivent être écrites dans le journal. Il n’y a pas de bon ou mauvais nombre ici, il s’agit uniquement d’une mesure de la quantité de stockage prise en charge.

-   Planifiez les charges d’e/s de disque non-noyau, telles que les analyses de sauvegarde et de protection antivirus, pour les périodes de chargement non-pic. Utilisez également des solutions de sauvegarde et de protection antivirus qui prennent en charge la fonctionnalité d’e/s basse priorité introduite dans Windows Server 2008 afin de réduire la concurrence avec les besoins d’e/s de Active Directory.

## <a name="dont-over-tax-the-processors"></a>Ne surchargez pas les processeurs

Les processeurs qui n’ont pas suffisamment de cycles libres peuvent entraîner des temps d’attente importants sur l’obtention des threads sur le processeur pour l’exécution. Dans de nombreux environnements, la philosophie consiste à s’assurer qu’il y a suffisamment d’espace pour gérer les pics ou les pics de charge afin de réduire l’impact sur la réactivité du client dans ces scénarios. En bref, le dépassement des seuils ci-dessous n’est pas un problème à long terme (5 à 15 minutes, plusieurs fois par jour). Toutefois, un système qui s’exécute avec ces types de statistiques ne fournit pas de salle de tête pour gérer les charges anormales et peut facilement être placé dans un sur des s imposés. cenario. Les dépenses de systèmes qui dépassent les seuils doivent être étudiées pour réduire les charges du processeur.

-   Pour plus d’informations sur la sélection d’un processeur, consultez [réglage des performances pour le matériel du serveur](../../hardware/index.md).

-   Ajouter du matériel, optimiser la charge, diriger les clients ailleurs ou supprimer la charge de l’environnement afin de réduire la charge de l’UC.

-   Utilisez les informations du processeur (\_Total) \\% utilisation du processeur &lt; 60% compteur de performances.

## <a name="avoid-overloading-the-network-adapter"></a>Éviter de surcharger la carte réseau

Tout comme avec les processeurs, une utilisation excessive de la carte réseau entraîne des temps d’attente importants pour que le trafic sortant soit sur le réseau. Active Directory tend à avoir des demandes entrantes de petite taille et des volumes de données beaucoup plus importants renvoyés aux systèmes clients. Les données envoyées dépassent beaucoup de données reçues. Dans de nombreux environnements, la philosophie consiste à s’assurer qu’il y a suffisamment d’espace pour supporter les pics ou les pics de charge. Ce seuil est un seuil d’avertissement dans lequel la salle de la tête pour gérer les pics ou les pics de charge est limitée et les réactivités du client sont dégradées. En bref, le dépassement de ces seuils n’est pas défectueux à long terme (5 à 15 minutes, plusieurs fois par jour), mais un système s’exécutant avec ces types de statistiques est trop sollicité et doit être examiné.

-   Pour plus d’informations sur la façon de régler le sous-système réseau, consultez [réglage des performances pour les sous-systèmes réseau](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md).

-   Utilisez le compteur de performances\*compare l’interface réseau ()\\octets envoyés/\*s\\avec l’interface réseau () Current bandwidth. Le ratio doit être inférieur à 60% utilisé.

## <a name="see-also"></a>Voir aussi
- [Réglage des performances Active Directory serveurs](index.md)
- [Considérations relatives au protocole LDAP](ldap-considerations.md)
- [Positionnement correct des contrôleurs de domaine et considérations relatives au site](site-definition-considerations.md)
- [Résoudre les problèmes de performances d’AD DS](troubleshoot.md) 
- [Planification de la capacité pour les services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)