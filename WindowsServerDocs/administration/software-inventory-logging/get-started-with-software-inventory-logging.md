---
title: Prise en main de la journalisation de l’inventaire logiciel
description: Décrit comment installer et commencer à utiliser la journalisation de l’inventaire logiciel
ms.custom: na
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed51c13c-7cbf-4144-a675-011fd29379d4
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a38c984b2d81fc4db980a969ef0312109950b867
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383001"
---
# <a name="get-started-with-software-inventory-logging"></a>Prise en main de la journalisation de l’inventaire logiciel

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 La journalisation de l’inventaire logiciel collecte les données d’inventaire logiciel Microsoft sur une base par serveur. Avant d’utiliser la journalisation de l’inventaire logiciel avec Windows Server 2012 R2, assurez-vous que Windows Update [kb 3000850](https://support.microsoft.com/kb/3000850) et [KB 3060681](https://support.microsoft.com/kb/3060681) sont installés sur chaque système à inventorier. Aucune Windows Update n’est requise pour Windows Server 2016. En outre, si vous souhaitez utiliser la fonctionnalité de SIL pour transférer des données vers un serveur d’agrégation, assurez-vous que les certificats SSL sont valides pour votre réseau.

## <a name="BKMK_OVER"></a>Description de la fonctionnalité
La journalisation de l’inventaire logiciel dans Windows Server est une fonctionnalité comprenant des applets de commande PowerShell qui permettant aux administrateurs de serveurs de récupérer la liste des logiciels qui sont installés sur ces derniers. De plus, elle collecte et transmet ces données régulièrement à un serveur web cible via le réseau, à l'aide du protocole HTTPS, à des fins d'agrégation. La gestion de cette fonctionnalité, principalement pour la collecte et le transfert quotidiens, est également assurée par des commandes PowerShell.

> [!NOTE]
> Un serveur d’agrégation exécutant un service web peut être configuré séparément. En savoir plus sur [Agrégateur de journalisation de l’inventaire logiciel](software-inventory-logging-aggregator.md).

> [!IMPORTANT]
> Aucune des données collectées par la journalisation de l’inventaire logiciel n’est envoyée à Microsoft dans le cadre de la fonctionnalité.

## <a name="BKMK_APP"></a>Applications pratiques
La journalisation de l’inventaire logiciel vise à réduire les coûts d’exploitation liés à la récupération d’informations précises concernant les logiciels Microsoft déployés localement sur un serveur, mais plus particulièrement sur de nombreux serveurs d’un environnement informatique (en supposant qu’elle est déployée et activée dans l’environnement informatique). La possibilité de transférer ces données à un serveur d’agrégation (s’il est configuré séparément par un administrateur) permet une collecte centralisée des données par un processus uniforme et automatique. Même s’il est possible d’obtenir le même résultat en interrogeant directement les interfaces, la journalisation de l’inventaire logiciel, à l’aide d’une architecture de transmission (via le réseau) mise en œuvre par chaque serveur, permet de relever les défis liés à la découverte des ordinateurs, défis typiques de nombreux scénarios de gestion des actifs et des inventaires logiciels. SSL est utilisé pour sécuriser les données transmises via HTTPs au serveur d’agrégation d’un administrateur. La centralisation des données (sur un serveur) facilite l’analyse et la manipulation des données, ainsi que la création de rapports sur celles-ci. Il est important de noter qu’aucune de ces données n’est envoyée à Microsoft dans le cadre du mode de fonctionnement de la fonctionnalité. Les données et les fonctionnalités de journalisation de l’inventaire logiciel sont destinées à être utilisées uniquement par le propriétaire et les administrateurs titulaires d’une licence du logiciel serveur.

La journalisation de l’inventaire logiciel peut aider les administrateurs des serveurs à exécuter les tâches suivantes :

-   Récupération de l’inventaire logiciel et serveur de serveurs Windows à distance et à la demande.

-   Agrégation des informations d’inventaire logiciel et serveur pour un large éventail de scénarios de gestion des actifs logiciels en activant la fonctionnalité de journalisation de l’inventaire logiciel de chaque serveur et en choisissant un URI cible de serveur Web et une empreinte numérique de certificat pour l’authentification.

## <a name="see-also"></a>Voir aussi
[Agrégateur de journalisation de l’inventaire logiciel](https://technet.microsoft.com/library/mt572043.aspx)<br>
[Gérer la journalisation de l’inventaire logiciel](manage-software-inventory-logging.md)<br>
[Applets de commande de la journalisation de l’inventaire logiciel dans Windows PowerShell](https://technet.microsoft.com/library/dn283390.aspx)<br>
[Outil de gestion de l’activation en volume](http://blogs.technet.com/b/volume-licensing/) de [Microsoft Assessment and Planning Toolkit](https://www.microsoft.com/download/en/details.aspx?id=7826)


