---
ms.assetid: ''
title: Prise en charge de la délimitation pour une heure très précise
description: Cet article décrit la limite de support pour le service de temps Windows (W32Time) dans les environnements qui nécessitent une heure système extrêmement précise et stable.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 212b9c79bc2e43e966180b928c865a9053332c3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405268"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Prise en charge de la délimitation pour une heure très précise

>S’applique à : Windows Server 2016 et Windows 10 version 1607 ou ultérieure

Cet article décrit les limites de prise en charge du service de temps Windows (W32Time) dans les environnements qui nécessitent un temps système extrêmement précis et stable.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Prise en charge de la haute précision pour Windows 8.1 et 2012 R2 (ou version antérieure)

Les versions antérieures de Windows (antérieures à Windows 10 1607 ou Windows Server 2016 1607) ne peuvent pas garantir un temps très précis. Le service de temps Windows sur les systèmes suivants :

-   Fournir la précision de temps nécessaire pour répondre aux exigences d’authentification Kerberos version 5

-   Offre un temps faible et précis pour les clients et les serveurs Windows joints à une forêt Active Directory commune

Des exigences plus précises en termes de précision étaient en dehors des spécifications de conception du service de temps Windows sur ces systèmes d’exploitation et ne sont pas prises en charge.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 et Windows Server 2016

La précision du temps dans Windows 10 et Windows Server 2016 a été considérablement améliorée, tout en conservant une compatibilité NTP complète avec les anciennes versions de Windows. Dans les conditions de fonctionnement appropriées, les systèmes exécutant Windows 10 ou Windows Server 2016 et les versions plus récentes peuvent fournir une précision de 1 seconde, 50 ms (millisecondes) ou de 1 ms.

>[!IMPORTANT]
>**Sources de temps très précises**<br>
>L’exactitude du temps qui en résulte dans votre topologie dépend fortement de l’utilisation d’une source de temps exacte et stable (Stratum 1). Des fournisseurs tiers peuvent disposer d’une source de temps NTP, compatible avec Windows et non basée sur Windows. Contactez votre fournisseur pour vérifier l’exactitude de ses produits.

>[!IMPORTANT]
>**Précision du temps**<br>
>L’exactitude du temps implique la distribution de bout en bout du temps précis d’une source de temps de référence très précise à l’appareil final. Tout ce qui introduit l’asymétrie du réseau aura une incidence négative sur la précision, par exemple des périphériques réseau physiques ou une charge élevée du processeur sur le système cible.

## <a name="high-accuracy-requirements"></a>Exigences de haute précision

Le reste de ce document décrit les exigences environnementales qui doivent être satisfaites pour prendre en charge les cibles à haute précision respectives.

### <a name="target-accuracy-1-second-1s"></a>Précision de la cible : 1 seconde (1S)

Pour obtenir une précision de 1 pour un ordinateur cible spécifique par rapport à une source de temps très précise :

-   Le système cible doit exécuter Windows 10, Windows Server 2016.

-   Le système cible doit synchroniser l’heure à partir d’une hiérarchie NTP de serveurs de temps, ce qui se termine par une source de temps NTP compatible avec Windows très précise.

-   Tous les systèmes d’exploitation Windows dans la hiérarchie NTP mentionnée ci-dessus doivent être configurés comme indiqué dans la documentation sur la [Configuration des systèmes pour une haute précision](configuring-systems-for-high-accuracy.md) .

-   La latence réseau unidirectionnelle cumulée entre la cible et la source ne doit pas dépasser 100 ms. Le délai réseau cumulé est mesuré en ajoutant les retards unidirectionnels individuels entre les paires de nœuds client-serveur NTP dans la hiérarchie en commençant par la cible et en terminant à la source. Pour plus d’informations, consultez le document sur la synchronisation de l’heure de précision élevée.

### <a name="target-accuracy-50-milliseconds"></a>Précision de la cible : 50 millisecondes

Toutes les exigences décrites dans la section @no__t-précision 0Target : 1 seconde @ no__t-0 Apply, sauf lorsque des contrôles plus stricts sont décrits dans cette section.

Les exigences supplémentaires pour obtenir une précision 50 ms pour un système cible spécifique sont les suivantes :

-   L’ordinateur cible doit avoir une meilleure 5 ms de la latence du réseau entre sa source de temps.

-   Le système cible ne doit pas être supérieur à la couche 5 d’une source de temps très précise

    >[!Note]
    >Exécutez « w32tm/Query/Status » à partir de la ligne de commande pour afficher la couche.

-   Le système cible doit se trouver dans un minimum de 6 tronçons réseau à partir de la source de temps très précise.

-   L’utilisation moyenne du processeur par jour sur toutes les strates ne doit pas dépasser 90%

-   Pour les systèmes virtualisés, l’utilisation moyenne du processeur d’un jour de l’ordinateur hôte ne doit pas dépasser 90%

### <a name="target-accuracy-1-millisecond"></a>Précision de la cible : 1 milliseconde

Toutes les exigences décrites dans les sections @no__t-précision 0Target : 1 seconde @ no__t-0 et **Target précision : 50 millisecondes @ no__t-0 Apply, sauf lorsque des contrôles plus stricts sont décrits dans cette section.

Les exigences supplémentaires pour atteindre 1 ms de précision pour un système cible spécifique sont les suivantes :

-   L’ordinateur cible doit avoir plus de 0,1 ms de latence réseau entre sa source de temps

-   Le système cible ne doit pas être supérieur à la couche 5 d’une source de temps très précise

    >[!Note]
    >Exécutez’w32tm/Query/Status’à partir de la ligne de commande pour afficher la couche

-   Le système cible doit se trouver dans un maximum de 4 tronçons réseau à partir de la source de temps très précise.

-   L’utilisation moyenne du processeur sur une journée sur chaque strate ne doit pas dépasser 80%

-   Pour les systèmes virtualisés, l’utilisation moyenne du processeur d’un jour de l’ordinateur hôte ne doit pas dépasser 80%
