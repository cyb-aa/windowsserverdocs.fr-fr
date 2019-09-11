---
title: Étude de cas du kit de développement logiciel (SDK) Windows Admin Center-DataON
description: Étude de cas du kit de développement logiciel (SDK) Windows Admin Center-DataON
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 01/11/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 12de939aa451ba75b4bafed85cd57bdd8280bc81
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70864936"
---
# <a name="dataon-must-extension"></a>DataON doit être une extension

## <a name="integrated-monitoring-and-management-for-microsoft-hyper-converged-infrastructure"></a>Surveillance et gestion intégrées pour l’infrastructure hyper-convergée Microsoft

[DataON](http://www.dataonstorage.com/) est le fournisseur leader du secteur de l’infrastructure hyper-convergée et des systèmes de stockage optimisés pour les environnements Microsoft Windows Server. Conçu exclusivement pour la diffusion d’applications Microsoft, la virtualisation, la protection des données et les services Cloud hybrides, il compte plus de 650 déploiements d’entreprise et plus de 120PB de déploiements de espaces de stockage direct.

L’extension de [DataON](http://www.dataonstorage.com/must) pour le centre d’administration Windows est un excellent exemple de la valeur que l’intégration de deux produits complémentaires peut offrir aux clients, ce qui permet d’obtenir des informations de surveillance et de gestion, ainsi que des informations de bout en bout sur le matériel et les logiciels. un cluster entier dans une expérience unifiée.

> <cite>«Nous avons suivi notre outil de visibilité, de surveillance et de gestion pour pouvoir travailler dans le centre d’administration Windows. Les clients bénéficieront des capacités étendues que doit fournir, et la combinaison de doit et du centre d’administration Windows à partir d’une console unique offrira une expérience de gestion optimale pour l’infrastructure basée sur Windows Server.»</cite>
>
> --Howard Lo, vice-président Sales and Marketing, DataON

L’extension doit étendre les fonctionnalités du centre d’administration Windows en fournissant des fonctionnalités telles que :
- **Rapport historique des données** : fournit des tableaux de bord en temps réel et mensuels de vos données de performances système, notamment les e/s par seconde, la latence, le débit sur votre cluster, le pool de stockage, le volume et les nœuds.
- **Mappage de disque** : doit afficher les types d’appareils et les composants dans chacun des nœuds, ce qui fournit une carte de disque claire de l’ensemble du nœud. Elle indique le nombre de disques, le type de disque, l’emplacement et l’emplacement de chaque lecteur, ainsi que l’état d’intégrité du disque.
- **Alertes système** : exploite les erreurs de Windows service de contrôle d’intégrité pour identifier les défaillances matérielles, les problèmes de configuration et la saturation des ressources. Il fournit également une évaluation à plusieurs niveaux d’emplacements spécifiques, de descriptions d’erreurs et d’actions de récupération. Vous pouvez également utiliser des interruptions de surveillance SNMP tierces pour vous avertir lorsque vous avez besoin de remplacements de disque ou de matériel.
- **Service d’appel à distance d’un réseau San** – demandé par les alertes système, les administrateurs peuvent envoyer des alertes par courrier électronique automatiques aux contacts clés.

![](../../media/extend-case-study-dataon/dataon-1.png)
*Le mappage de disque d’extension DataON dans le DataON doit être extension pour le centre d’administration Windows*

> <cite>«Il est parfait que le centre d’administration Windows autorise les extensions telles que DataON. je peux donc utiliser les deux outils dans la même console et j’aime l’intégration transparente. Le centre d’administration Windows et DataON doivent, ensemble, nous permettre d’être plus efficaces et de gagner beaucoup de temps à notre équipe. Cela nous permet d’obtenir des tâches d’administrateur beaucoup plus rapidement que nous l’avons fait précédemment.</cite>
>
> --Matt Roper, facilitateur des services de support technique, Cherokee comté (GA) School District

![](../../media/extend-case-study-dataon/dataon-2.png)
*Les services d’alerte de l’extension DataON dans le DataON doivent être extensions pour le centre d’administration Windows*

> <cite>«DOIT être très utile et était un grand point de vente. Pour nous, il a démontré un engagement de DataON pour prendre en charge l’infrastructure hyper-convergée Microsoft. L’inclusion de doit être effectuée avec son appliance S2D, ce qui termine la solution avec espaces de stockage direct comme un remplacement SAN viable.»</cite>
>
> --Benjamin Clements, Président, Strategic Online Systems, Inc.