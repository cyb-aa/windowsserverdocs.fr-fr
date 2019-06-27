---
title: Étude de cas de kit de développement logiciel Windows Admin Center - Thomas-Krenn
description: Étude de cas de kit de développement logiciel Windows Admin Center - Thomas-Krenn
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/24/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93b8a450aa86a454ec6febd349fcaa35df590266
ms.sourcegitcommit: 3be280c8638214857dc355b201eb56a04499a5e5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67396759"
---
# <a name="thomas-krennag-extension"></a>Extension de Thomas-Krenn.AG

## <a name="intuitive-server-and-storage-health-management"></a>Gestion intuitive d’intégrité de serveur et de stockage

L’extension de Thomas Krenn.AG Windows Admin Center est conçue spécifiquement pour le nœud de 2 hautement disponible, [S2D Micro-Cluster](https://www.thomas-krenn.com/en/products/application/software-defined-storage/s2d-micro-cluster.html) appliance. L’interface web conviviale, graphique visualise l’état d’intégrité de Micro-d’un Cluster via un tableau de bord simple et permet de parcourir des dispositifs de stockage, les interfaces réseau ou l’ensemble du cluster pour afficher plus de détails.

L’extension fournit un accès intuitif aux informations généralement nécessaires pour les appels de service et le support premier niveau, tels que les numéros de série, versions des logiciels, l’utilisation du stockage et bien plus encore. Il est conçu pour être utile aux administrateurs qui n’ont aucune expérience préalable avec l’infrastructure Hyper-convergée de Windows Server.

Parmi les informations disponibles est :
- Informations générales sur les nœuds de Micro et le Micro-Cluster
- Système d’exploitation / de l’état du périphérique de démarrage
- La capacité de disque dur et de la mise en cache d’état des disques SSD
- Événements de cluster
- État du réseau et des informations

Utilisez le tableau de bord pour déterminer l’état d’intégrité et les informations système importantes telles que les numéros de série, modèle, version du système d’exploitation et l’utilisation du cluster. En outre, le ventilateur, d’une carte réseau et d’intégrité de matériel de nœud globale sont affichés sur le tableau de bord.

![Extension de Thomas-Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-1.png)

Vous pouvez explorer les dispositifs de stockage pour afficher les numéros de série, l’état d’à puce et utilisation de la capacité. Périphériques de démarrage indiquent également l’usure des indicateurs, réaffectées de secteurs et la puissance à temps, qui fournissent la meilleure indication d’intégrité de disque SSD.

![Extension de Thomas-Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-2.png)

L’icône d’état de cluster se développe pour afficher un résumé des détails opérationnels du cluster.

![Extension de Thomas-Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-3.png)

Une fois que le témoin de cloud Azure de ce Cluster Micro n’était pas disponible pour une nuit entière, un coup de œil suffit pour identifier le problème. Répertorie les événements pertinents pour rapide mise à jour en cliquant sur « Notifications » immédiatement. Événements de cluster sont localisés et déterminés par la langue du système d’exploitation de base. L’extension elle-même prend en charge l’anglais et allemand.

![Extension de Thomas-Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-4.png)

Informations sur le réseau sont également facilement disponibles.

![Extension de Thomas-Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-5.png)

En fonction des commentaires des clients, nous avons également implémenté « Mode sombre », disponible dans Windows Admin Center v1904. Il s’agit d’APAISEMENT dans des centres de données foncé et armoires mal surlignés et armoires. En outre, Windows Admin Center plus accessibles en réduisant les reflets pour les administrateurs avec certains troubles visuels.

![Extension de Thomas-Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-6.png)

Thomas-Krenn réalisées immédiatement que facilité d’utilisation et l’accessibilité pour les administrateurs non formés serait clé une excellente expérience client pour l’infrastructure Hyper-convergée sur le marché des petites et moyennes entreprises. Extension de Micro-Cluster de Thomas-Krenn complète idéalement les fonctions de gestion de HCL de Windows Admin Center en incluant des informations de matériel propriétaire du tableau de bord et nouveau regroupement d’informations d’intégrité cluster important dans une nouvelle interface conviviale.

Pendant le processus de développement, il a été décidé de déployer Windows Admin Center 1904 dans une configuration de haute disponibilité sur le cluster lui-même, en garantissant la facilité de gestion même après des défaillances de nœud. L’extension est préinstallée, tout comme le système d’exploitation complet.

L’extension a été générée en parallèle avec Windows Admin Center 1904 est en cours de développement chez Microsoft. Une coopération étroite et problèmes de commentaires en continu exposées sur les deux côtés ont été résolues conjointement avant le produit a été lancé en avril 2019. Thomas-Krenn est extrêmement fier d’être parmi les premiers entièrement prendre en charge et implémenter des fonctionnalités nouvelles de 1904 Windows Admin Center.
