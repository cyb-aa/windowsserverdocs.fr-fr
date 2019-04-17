---
title: Étude de cas SDK Windows Admin Center - stockage pur
description: Étude de cas SDK Windows Admin Center - stockage pur
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: ebeec824f802f020d0ece17524ba43b1baeba893
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "8995321"
---
# Extension de stockage pur

## En proposant une gestion tableau de bout en bout pour Windows Admin Center 

[Stockage pur](https://www.purestorage.com/) fournit une entreprise, les solutions de stockage flash-toutes les données qui permettent d’architecture centrées sur les données afin d’accélérer vos activités pour un avantage compétitif.  Pure est un Microsoft Gold Partner, certifiés pour Microsoft Windows Server et développe des intégrations techniques pour les solutions Microsoft clées comme Azure, Hyper-V, SQL Server, System Center, Windows PowerShell et SMB de Windows. Pure récemment annoncé un tech preview de l’extension prise en charge de la dernière version de Windows Admin Center qui fournit une vue du volet unique dans les produits FlashArray pur.  À partir de cette extension, les utilisateurs sont habilitées à partir d’un seul outil pour effectuer des tâches de surveillance, afficher les mesures de performances en temps réel et gérer des initiateurs et les volumes de stockage.

Dès le départ lorsque Windows Admin Center a été appelé «Projet Honolulu», Pure vu la valeur d’être en mesure de fournir des clients et partenaires la possibilité de gérer plusieurs FlashArrays stockage pur à partir du volet unique qui fournit Windows Admin Center.

Démarrage de recherche le cas d’utilisation avec «Projet Honolulu» Pure ils réalisés immédiatement le risque de fournir une expérience de gestion unifiée entre Windows Admin Center et FlashArray. Pure étroitement collaboré avec l’équipe d’ingénieurs de Windows Admin Center, qui a permis à définir les détails d’implémentation pour les fonctionnalités. Pure a été également en mesure de fournir des commentaires sur les premières phases de Windows Admin Center et apportez les contributions à l’équipe Microsoft. 

![Extension de stockage pur](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>«Nous avons intégré un ensemble de fonctionnalités qui imite notre interface web de FlashArray pour activer la gestion directe à partir de Windows Admin Center. Nos clients et partenaires bénéficieront d’un volet unique par rapport à la nécessité de fonctionner avec deux outils de gestion différents. Outre le point unique de la gestion des avantages pour les clients sera en mesure de gérer des serveurs Windows qui sont connectés à le FlashArray opportun.»</cite>
>
> --Barkz, directeur technique Microsoft Solutions et intégration de stockage pur

Les fonctionnalités qui sont incluses dans l’Extension de Solution de stockage pur sont les suivantes:
- Connexion à plusieurs FlashArrays.
- Affichage des détails FlashArray, y compris/s par seconde, la bande passante, latence, réduction des données et gestion de l’espace. Il s’agit de tous les détails mêmes que vous obtenez de l’interface utilisateur graphique de gestion FlashArray.
- Afficher les groupes hôte configuré qui sont utilisés pour activer l’accès de volume partagé pour les hôtes de Windows Server et les Volumes partagés en cluster (volumes partagés de cluster).
- Vue hôtes: Toutes les informations de connectivité est disponible, y compris les noms d’hôtes, iSCSI nom qualifié (IQN) et World Wide Names (WWN).
- Gérer les Volumes, Cela inclut la possibilité de créer et supprimer les volumes. Une fois qu’un volume est détruit il va être placé dans la plage d’éléments détruit et vous devez Eradicate à partir de l’interface utilisateur graphique de gestion FlashArray principal.
- Gérer les initiateurs, C’est une des fonctionnalités plus intéressantes fournissant le contexte pour les serveurs gérés par le déploiement de Windows Admin Center. Vous pouvez afficher les disques connectés (volumes) aux serveurs Windows individuels, vérifiez si les e/s-MPIO (MPIO) est installé/configuré et création/montage de nouveaux volumes.

Une [vidéo de démonstration](https://youtu.be/IFAeCAd6V2g) qui montre toutes les fonctionnalités qui fournit l’Extension de Solution de stockage pur a été créé. 

La ci-dessous capture d’écran illustre l’affichage les disques (volumes) sont connectés à un hôte Windows Server spécifique. En plus de consulter les détails de connectivité, nous vérifions si Multipath-e/s est configuré.

![Extension de stockage pur](../../media/extend-case-study-purestorage/purestorage-2.png)

En plus de l’affichage des disques, nouveaux volumes peuvent être créés et montés immédiatement à l’hôte sans avoir à utiliser l’outil Gestion des disques de Windows.

![Extension de stockage pur](../../media/extend-case-study-purestorage/purestorage-3.png)

Étant donné que notre Technical Preview, les commentaires des clients collectées jusqu'à présent, en libérant a été très positive et également nous a fourni des informations sur les différentes fonctionnalités à ajouter dans les futures versions. 

Ressources supplémentaires:
- [Billet de blog pure stockage extension annonce](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- Podcast [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)
