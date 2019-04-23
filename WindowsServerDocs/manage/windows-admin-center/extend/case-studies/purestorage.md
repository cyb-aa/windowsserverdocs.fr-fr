---
title: Étude de cas de kit de développement logiciel Windows Admin Center - stockage purs
description: Étude de cas de kit de développement logiciel Windows Admin Center - stockage purs
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849920"
---
# <a name="pure-storage-extension"></a>Extension de stockage purs

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>Une gestion de tableau de bout en bout pour Windows Admin Center 

[Stockage purs](https://www.purestorage.com/) fournit l’entreprise, les solutions de stockage flash de toutes les données qui offrent une architecture orientée données afin d’accélérer votre entreprise un avantage concurrentiel.  Pur est un partenaire Microsoft Gold, certifié pour Microsoft Windows Server et développe des intégrations techniques pour les solutions Microsoft clés tels que Azure, Hyper-V, SQL Server, System Center, Windows PowerShell et SMB de Windows. Pur a récemment annoncé un aperçu technique d’une extension prenant en charge de la dernière version de Windows Admin Center qui fournit un affichage simple dans les produits FlashArray Pure.  À partir de cette extension, les utilisateurs sont habilités à partir d’un outil pour effectuer des tâches de surveillance, afficher les mesures de performances en temps réel et gérer les initiateurs et les volumes de stockage.

Dès le départ lorsque Windows Admin Center était appelé « Projet Honolulu », pur vu la valeur de mettre à disposition des clients et partenaires la possibilité de gérer plusieurs Pure FlashArrays de stockage à partir du volet unique de verre Windows Admin Center fournit.

Lorsque pur commencé à rechercher le cas d’usage avec « Projet Honolulu » qu’ils comprenaient immédiatement le potentiel pour fournir une expérience de gestion unifiée entre Windows Admin Center et FlashArray. Pur étroitement collaboré avec l’équipe d’ingénierie Windows Admin Center, qui lui a permis de définir les détails d’implémentation pour les fonctionnalités. Pur est également parvenu à fournir des commentaires lors des premières phases de Windows Admin Center et de contribuer à l’équipe de Microsoft. 

![Extension de stockage purs](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>« Nous avons intégré un ensemble de fonctionnalités qui imite notre interface web de FlashArray pour activer la gestion directe depuis Windows Admin Center. Nos clients et partenaires bénéficient d’un volet unique plutôt que de devoir travailler avec deux outils d’administration différents. Outre le point de gestion unique avantages pour les clients seront en mesure de gérer en fonction du contexte des serveurs Windows qui sont connectés à la FlashArray. »</cite>
>
> --Barkz, directeur technique Microsoft Solutions & intégration, stockage purs

Les fonctionnalités qui sont incluses dans l’Extension de Solution de stockage Pure incluent :
- Connexion à plusieurs FlashArrays.
- Affichage des détails FlashArray, y compris les e/s, de bande passante, de latence, de réduction des données et de gestion de l’espace. Il s’agit des mêmes détails que vous obtenez à partir de l’interface utilisateur graphique de gestion FlashArray.
- Afficher les groupes hôtes configurés qui sont utilisés pour autoriser l’accès au volume partagé pour les hôtes Windows Server et les Volumes partagés en cluster (CSV).
- Hôtes de la vue, Toutes les informations de connectivité est disponible, y compris les noms d’hôte, iSCSI nom qualifié (IQN) et les noms WWN (WWN).
- Gérer les Volumes, Cela inclut la possibilité de créer et détruire des volumes. Une fois un volume est détruit. il est placé dans la plage d’éléments de destruction et vous devrez Eradicate à partir de l’interface utilisateur graphique de gestion principal FlashArray.
- Gestion des initiateurs — C’est une des fonctionnalités plus intéressantes de fournir un contexte aux serveurs individuels gérés par le déploiement de Windows Admin Center. Vous pouvez afficher les disques connectés (volumes) à des serveurs Windows individuels, vérifiez si les e/s de Multipath i/o (MPIO) est installé/configuré et création de montage de nouveaux volumes.

Un [vidéo de démonstration](https://youtu.be/IFAeCAd6V2g) a été créé qui affiche toutes les fonctionnalités fournies par l’Extension de Solution de stockage Pure. 

La capture d’écran ci-dessous illustre l’affichage les disques (volumes) sont connectés à un hôte Windows Server spécifique. Outre l’affichage des détails de la connectivité, nous vérifions si Multipath i/o-e/s est configuré.

![Extension de stockage purs](../../media/extend-case-study-purestorage/purestorage-2.png)

Outre l’affichage les disques, les nouveaux volumes peuvent être créés et montés immédiatement à l’hôte sans avoir à utiliser l’outil Gestion des disques Windows.

![Extension de stockage purs](../../media/extend-case-study-purestorage/purestorage-3.png)

Étant donné que la libération de notre Technical Preview, les commentaires des clients collectées jusqu'à présent ont été très positif et également nous a fourni les informations sur les différentes fonctionnalités à ajouter dans les futures versions. 

Ressources supplémentaires :
- [Billet de blog pure stockage extension annonce](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2) podcast
