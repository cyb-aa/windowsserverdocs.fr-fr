---
title: Limite de prise en charge pour un temps haute précision
description: Cet article décrit la limite de prise en charge pour le service de temps Windows (W32Time) dans les environnements qui nécessitent une heure système extrêmement précise et stable.
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: f6df2a07fa7af2b5912c368393bdab39ccc5ced3
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861632"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Limite de prise en charge pour un temps haute précision

>S'applique à : Windows Server 2016 et Windows 10, version 1607 ou ultérieure

Cet article décrit les limites de prise en charge pour le service de temps Windows (W32Time) dans les environnements qui nécessitent une heure système extrêmement précise et stable.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Prise en charge haute précision pour Windows 8.1 et 2012 R2 (ou version antérieure)

Les versions antérieures de Windows (antérieures à Windows 10 1607 ou Windows Server 2016 1607) ne peuvent pas garantir un temps haute précision. Sur ces systèmes, le service de temps Windows :

-   Fournissait la précision de temps nécessaire pour répondre aux exigences d’authentification Kerberos version 5

-   Fournissait un temps moyennement précis pour les clients et les serveurs Windows joints à une forêt Active Directory commune

Des exigences plus strictes en termes de précision n’étaient pas prévues dans les spécifications conceptuelles du service de temps Windows sur ces systèmes d’exploitation et ne sont pas prises en charge.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 et Windows Server 2016

La précision du temps dans Windows 10 et Windows Server 2016 a été considérablement améliorée, tout en conservant une compatibilité NTP complète avec les anciennes versions de Windows. Dans les conditions de fonctionnement appropriées, les systèmes exécutant Windows 10 ou Windows Server 2016 et les versions plus récentes peuvent fournir une précision de 1 seconde, 50 ms (millisecondes) ou 1 ms.

>[!IMPORTANT]
>**Sources de temps très précises**<br>
>La précision du temps obtenue dans votre topologie dépend fortement de l’utilisation d’une source de temps exacte et stable (couche 1). Les fournisseurs tiers peuvent vendre du matériel de source de temps NTP compatible avec Windows haute précision basé ou non sur Windows. Contactez votre fournisseur pour vérifier la précision de ses produits.

>[!IMPORTANT]
>**Précision du temps**<br>
>La précision du temps implique la distribution de bout en bout d’un temps précis depuis une source de temps de référence très précise vers l’appareil final. Tout ce qui introduit une asymétrie réseau a une incidence négative sur la précision, par exemple des appareils réseau physiques ou une charge élevée du processeur sur le système cible.

## <a name="high-accuracy-requirements"></a>Exigences de haute précision

Le reste de ce document décrit les exigences environnementales qui doivent être satisfaites pour prendre en charge les cibles à haute précision respectives.

### <a name="target-accuracy-1-second-1s"></a>Précision de la cible : 1 seconde (1s)

Pour obtenir une précision de 1s pour un ordinateur cible spécifique par rapport à une source de temps très précise :

-   Le système cible doit exécuter Windows 10, Windows Server 2016.

-   Le système cible doit synchroniser le temps à partir d’une hiérarchie NTP de serveurs de temps, aboutissant à une source de temps NTP compatible avec Windows très précise.

-   Tous les systèmes d’exploitation Windows dans la hiérarchie NTP mentionnée ci-dessus doivent être configurés comme indiqué dans la documentation [Configuration de systèmes de haute précision](configuring-systems-for-high-accuracy.md).

-   La latence réseau unidirectionnelle cumulée entre la cible et la source ne doit pas dépasser 100 ms. Le délai réseau cumulé est mesuré en ajoutant les délais unidirectionnels individuels entre les paires de nœuds client-serveur NTP dans la hiérarchie en commençant par la cible et en terminant à la source. Pour plus d’informations, consultez le document sur la synchronisation du temps haut précision.

### <a name="target-accuracy-50-milliseconds"></a>Précision de la cible : 50 millisecondes

Toutes les exigences décrites dans la section **Précision de la cible : 1 seconde** s’appliquent, sauf quand des contrôles plus stricts sont décrits dans cette section.

Les exigences supplémentaires pour obtenir une précision de 50 ms dans le cas d’un système cible spécifique sont les suivantes :

-   L’ordinateur cible doit avoir une latence réseau meilleure que 5 ms par rapport à sa source de temps.

-   Le système cible ne doit pas être situé au-delà de la couche 5 par rapport à une source de temps très précise.

    >[!Note]
    >Exécutez « w32tm /query /status » à partir de la ligne de commande pour voir la couche.

-   Le système cible doit se trouver au plus à 6 tronçons réseau de la source de temps très précise.

-   L’utilisation moyenne quotidienne du processeur sur toutes les couches ne doit pas dépasser 90 %.

-   Pour les systèmes virtualisés, l’utilisation moyenne quotidienne du processeur de l’hôte ne doit pas dépasser 90 %.

### <a name="target-accuracy-1-millisecond"></a>Précision de la cible : 1 milliseconde

Toutes les exigences décrites dans les section **Précision de la cible : 1 seconde** et **Précision de la cible : 50 millisecondes** s’appliquent, sauf quand des contrôles plus stricts sont décrits dans cette section.

Les exigences supplémentaires pour obtenir une précision de 1 ms dans le cas d’un système cible spécifique sont les suivantes :

-   L’ordinateur cible doit avoir une latence réseau meilleure que 0,1 ms par rapport à sa source de temps.

-   Le système cible ne doit pas être situé au-delà de la couche 5 par rapport à une source de temps très précise.

    >[!Note]
    >Exécutez « w32tm /query /status » à partir de la ligne de commande pour voir la couche.

-   Le système cible doit se trouver au plus à 4 tronçons réseau de la source de temps très précise.

-   L’utilisation moyenne quotidienne du processeur sur chaque couche ne doit pas dépasser 80 %.

-   Pour les systèmes virtualisés, l’utilisation moyenne quotidienne du processeur de l’hôte ne doit pas dépasser 80 %.
