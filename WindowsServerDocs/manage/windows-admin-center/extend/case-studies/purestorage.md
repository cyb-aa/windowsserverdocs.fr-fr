---
title: Étude de cas du kit de développement logiciel (SDK) du centre d’administration Windows-stockage pur
description: Étude de cas du kit de développement logiciel (SDK) du centre d’administration Windows-stockage pur
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c6ab75a2375368d7c87d9dffd6175eb611508dd5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406953"
---
# <a name="pure-storage-extension"></a>Extension de stockage pur

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>Fourniture de la gestion de groupe de bout en bout pour le centre d’administration Windows 

Le [stockage pur](https://www.purestorage.com/) fournit des solutions de stockage de données d’entreprise et toutes les données Flash qui fournissent une architecture orientée données pour accélérer votre activité et bénéficier d’un avantage concurrentiel.  Pure est un partenaire Microsoft Gold, certifié pour Microsoft Windows Server, et développe des intégrations techniques pour des solutions Microsoft clés comme Azure, Hyper-V, SQL Server, System Center, Windows PowerShell et Windows SMB. Les versions pures ont récemment annoncé la version d’évaluation technique d’une extension qui prend en charge la dernière version du centre d’administration Windows, qui fournit une vue à un seul volet des produits FlashArray purs.  À partir de cette extension, les utilisateurs sont en mesure d’effectuer des tâches de surveillance, d’afficher les mesures de performances en temps réel et de gérer les volumes de stockage et les initiateurs.

Au début, lorsque le centre d’administration Windows était connu sous le nom de « Project Honolulu », la possibilité de fournir aux clients et aux partenaires la possibilité de gérer plusieurs FlashArrays de stockage pur à partir du seul volet de transparence fourni par le centre d’administration Windows.

Lors du début de la recherche du cas d’usage avec « Project Honolulu », il a immédiatement réalisé la possibilité de fournir une expérience de gestion unifiée entre le centre d’administration Windows et FlashArray. En étroite collaboration avec l’équipe d’ingénierie du centre d’administration Windows, qui a aidé à définir les détails de l’implémentation des fonctionnalités. Pure était également en mesure de fournir des commentaires aux premières étapes du centre d’administration Windows et de faire des contributions à l’équipe Microsoft. 

![Extension de stockage pur](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>», nous avons intégré un ensemble de fonctionnalités qui imite notre interface Web FlashArray pour activer la gestion directe à partir du centre d’administration Windows. Nos clients et partenaires bénéficient d’un seul volet de transparence et doivent travailler avec deux outils de gestion différents. Outre le point de gestion unique, les clients peuvent gérer de façon contextuelle les serveurs Windows connectés au FlashArray.» </cite>
>
> --BARKZ, directeur technique Microsoft Solutions & Integration, stockage pur

Les fonctionnalités incluses dans l’extension de solution de stockage pur sont les suivantes :
- Connexion à plusieurs FlashArrays.
- Affichage des détails de FlashArray, notamment les e/s par seconde, la bande passante, la latence, la réduction des données et la gestion de l’espace. Il s’agit des mêmes détails que ceux de l’interface utilisateur graphique de gestion FlashArray.
- Affichez les groupes hôtes configurés qui permettent d’activer l’accès aux volumes partagés pour les hôtes Windows Server et les volumes partagés en cluster (CSV).
- Afficher les ordinateurs hôtes : toutes les informations de connectivité sont disponibles, y compris les noms d’hôte, le nom qualifié iSCSI (IQN) et les noms WWN (WWN).
- Gérer les volumes : cela comprend la possibilité de créer et de détruire des volumes. Une fois qu’un volume est détruit, il est placé dans le compartiment éléments détruits et vous devez l’éradiquer de l’interface graphique principale de gestion FlashArray.
- Gérer les initiateurs : il s’agit de l’une des fonctionnalités les plus intéressantes qui fournissent le contexte aux serveurs individuels gérés par le déploiement du centre d’administration Windows. Vous pouvez afficher les disques connectés (volumes) sur des serveurs Windows individuels, vérifier si MPIO (Multipath-IO) est installé/configuré et créer/monter de nouveaux volumes.

Une [vidéo de démonstration](https://youtu.be/IFAeCAd6V2g) a été créée, qui présente toutes les fonctionnalités fournies par l’extension de solution de stockage pur. 

La capture d’écran ci-dessous illustre l’affichage des disques (volumes) qui sont connectés à un hôte Windows Server spécifique. En plus d’afficher les détails de connectivité, nous vérifions si l’option Multipath-IO est configurée.

![Extension de stockage pur](../../media/extend-case-study-purestorage/purestorage-2.png)

Outre l’affichage des disques, de nouveaux volumes peuvent être créés et immédiatement montés sur l’ordinateur hôte sans avoir à utiliser l’outil Gestion des disques de Windows.

![Extension de stockage pur](../../media/extend-case-study-purestorage/purestorage-3.png)

Depuis la publication de notre version d’évaluation technique, les commentaires des clients collectés jusqu’à présent ont été très positifs et nous ont également fourni des informations sur les différentes fonctionnalités à ajouter dans les versions ultérieures. 

Ressources supplémentaires :
- [Billet de blog sur l’annonce de l’extension de stockage pur](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- Podcast [PureReport](https://itunes.apple.com/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)
