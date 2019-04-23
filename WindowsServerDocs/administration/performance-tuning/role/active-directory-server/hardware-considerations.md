---
title: Considérations matérielles de réglage des performances AD
description: Considérations matérielles de réglage des performances AD
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0f1aa1e3c07c5cb9238a332156abfec248e74176
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866090"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>Considérations matérielles de réglage des performances d’AD DS 

>[!Important]
> Voici un résumé des recommandations essentielles et considérations pour optimiser le matériel de serveur pour les charges de travail Active Directory décrites plus en détail dans le [planification de la capacité pour les Services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566) article. Les lecteurs sont vivement encouragés à consulter [planification de la capacité pour les Services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566) pour une meilleure compréhension technique et les implications de ces recommandations.

## <a name="avoid-going-to-disk"></a>Éviter d’entrer sur le disque

Active Directory met en cache autant de caractères de la base de données comme permet de mémoire. Récupération des pages de la mémoire sont beaucoup plus rapidement qu’en allant sur un support physique, si le média est broche ou basé sur SSD. Ajoutez davantage de mémoire pour réduire les e/s disque.

-   Meilleures pratiques de Active Directory est recommandé de plaçant suffisamment de RAM pour charger toute la DIT dans la mémoire, ainsi que de prendre en charge le système d’exploitation et autres applications installées, tels que les logiciels antivirus, de sauvegarde et ainsi de suite.

    -   Pour connaître les limitations des plates-formes héritées, consultez [utilisation de la mémoire par le processus Lsass.exe sur les contrôleurs de domaine qui exécutent Windows Server 2003 ou Windows 2000 Server](https://support.microsoft.com/kb/308356).

    -   Utiliser la mémoire\\Long-Term Standby Cache durée de vie moyenne (s) &gt; compteur de performances de 30 minutes.

-   Placez le système d’exploitation, les journaux et la base de données sur des volumes distincts. Si tous les ou la majorité de la DIT peut être mis en cache, une fois le cache est initialisée et sous un état stable, cela devient moins pertinente et offre une plus grande flexibilité dans la disposition de stockage. Dans les scénarios où toute la DIT ne peut pas être mis en cache, l’importance de fractionner le système d’exploitation, les journaux et base de données sur des volumes distincts devient plus importante.

-   Normalement, le taux d’e/s de la DIT est environ 90 % en lecture et écriture de 10 %. Les scénarios où les volumes d’e/s écriture dépassent considérablement 10-20 % sont considérés comme des écritures. Scénarios d’écritures ne bénéficient pas considérablement à partir du cache d’Active Directory. Pour garantir la durabilité transactionnelle des données qui sont écrit dans le répertoire, Active Directory n’effectue pas de cache d’écriture disque. Au lieu de cela, elle valide toutes les opérations d’écriture sur le disque avant de retourner un état d’achèvement réussi d’une opération, sauf s’il existe une demande explicite ne pas pour ce faire. Par conséquent, les e/s de disque rapide est important des performances des opérations d’écriture dans Active Directory. Recommandations matérielles qui peuvent améliorer les performances pour ces scénarios sont les suivantes :

    -   Contrôleurs RAID matériels

    -   Augmenter le nombre de disques de faible latence/haute-tr/min qui hébergent les fichiers journaux et DIT

    -   Cache d’écriture sur le contrôleur

-   Passez en revue les performances du sous-système disque individuellement pour chaque volume. La plupart des scénarios d’Active Directory sont principalement en lecture, par conséquent, les statistiques sur le volume hébergeant le DIT sont les plus importants à inspecter. Toutefois, ne pas négliger le reste des lecteurs, y compris le système d’exploitation, de surveillance et les lecteurs de fichiers de journal. Pour déterminer si le contrôleur de domaine est correctement configuré pour éviter de stockage qui est le goulot d’étranglement de performances, faire référence à la section sur les sous-systèmes de stockage pour les recommandations de stockage de normes. Dans de nombreux environnements, la philosophie est pour vous assurer que principal l’espace est suffisant pour prendre en charge les pics d’activité ou des pics de charge. Ces seuils sont avertissement seuils où la marge pour prendre en charge les pics d’activité ou des pics de charge devient limitées et la réactivité du client se dégrade. En bref, n’est pas incorrect à court terme (5 à 15 minutes quelques fois par jour) de dépassement de ces seuils, toutefois un système en cours d’exécution prolongée ce genre de statistiques n’est pas entièrement mise en cache la base de données et peut être taxée et doit être examinés.

    -   Base de données ==&gt; Instances(lsass/NTDSA)\\d’e/s de base de données lit la latence moyenne &lt; 15 MS

    -   Database ==&gt; Instances(lsass/NTDSA)\\I/O Database Reads/sec &lt; 10

    -   Base de données ==&gt; Instances(lsass/NTDSA)\\d’e/s journal écrit la latence moyenne &lt; 10 MS

    -   Base de données ==&gt; Instances(lsass/NTDSA)\\d’e/s Journal écritures par seconde – d’information uniquement.

        Pour maintenir la cohérence des données, toutes les modifications doivent être écrites dans le journal. Nombre n’est pas bon ou mauvais ici, il est uniquement une mesure de la quantité de stockage le prend en charge.

-   Planifier des charges d’e/s de disque non principales, telles que des analyses antivirus et de sauvegarde, pour les périodes creuses. En outre, utilisez les solutions antivirues et de sauvegarde qui prennent en charge la fonctionnalité d’e/s de faible priorité introduite dans Windows Server 2008 afin de réduire la concurrence avec les besoins d’e/s d’Active Directory.

## <a name="dont-over-tax-the-processors"></a>Les processeurs de ne plus taxe

Processeurs qui n’ont pas suffisamment de cycles peuvent entraîner des temps d’attente long sur l’obtention des threads une session sur le processeur pour l’exécution. Dans de nombreux environnements, la philosophie consiste à s’assurer que principal l’espace est suffisant pour prendre en charge les pics d’activité ou les pics de charge afin de minimiser l’impact sur la réactivité du client dans ces scénarios. En bref, dépassant le dessous des seuils n’est pas incorrect à court terme (5 à 15 minutes quelques fois par jour), toutefois ne fournit pas un système en cours d’exécution prolongée ce genre de statistiques n’importe quel en-tête suffisamment de place pour prendre en charge les charges anormales et peut facilement être placés dans une taxer s scénario. Les systèmes des périodes prolongées au-dessus du seuil de dépense doivent être examinés comment réduire la charge du processeur.

-   Pour plus d’informations sur la sélection d’un processeur, consultez [réglage des performances pour le matériel de serveur](../../hardware/index.md).

-   Ajout de matériel, optimiser la charge, diriger les clients à un autre emplacement ou supprimer la charge de l’environnement pour réduire la charge de l’UC.

-   Utilisez les informations de processeur (\_Total)\\% utilisation du processeur &lt; compteur de performances de 60 %.

## <a name="avoid-overloading-the-network-adapter"></a>Éviter de surcharger la carte réseau

Tout comme avec les processeurs, utilisation de la carte réseau est excessive entraîne longues périodes d’attente pour le trafic sortant à accéder au réseau. Active Directory a tendance à avoir des petites demandes entrantes et relativement à considérablement plus grande quantité de données retournées pour les systèmes clients. Données envoyées dépassent largement les données reçues. Dans de nombreux environnements, la philosophie est pour vous assurer que principal l’espace est suffisant pour prendre en charge les pics d’activité ou des pics de charge. Ce seuil est un seuil d’avertissement où la marge pour prendre en charge les pics d’activité ou des pics de charge devienne limité et la réactivité du client se dégrade. En bref, n’est pas incorrect à court terme (5 à 15 minutes quelques fois par jour) de dépassement de ces seuils, toutefois un système en cours d’exécution prolongée ce genre de statistiques est via imposé et doit être examiné.

-   Pour plus d’informations sur la façon de régler le sous-système réseau, consultez [réglage des performances pour les sous-systèmes réseau](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md).

-   Utiliser l’interface réseau comparer (\*)\\octets envoyés/s avec une interface réseau (\*)\\compteur de performances de la bande passante actuelle. Le rapport doit être utilisé à moins de 60 %.

## <a name="see-also"></a>Voir aussi
- [Les serveurs Active Directory de réglage des performances](index.md)
- [Considérations relatives à LDAP](ldap-considerations.md)
- [Positionnement correct de contrôleurs de domaine et les considérations de site](site-definition-considerations.md)
- [Résolution des problèmes de performances d’AD DS](troubleshoot.md) 
- [Planification de capacité pour les Services de domaine Active Directory](https://go.microsoft.com/fwlink/?LinkId=324566)