---
ms.assetid: ''
title: Prise en charge de la délimitation pour une heure très précise
description: Cet article décrit la limite prise en charge pour le service de temps Windows (W32Time) dans les environnements nécessitant un temps système très précis et plus stable.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 991bf4502546771dae9f092c6d5732f96b1278ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866270"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Prise en charge de la délimitation pour une heure très précise

>S’applique à : Windows Server 2016 et Windows 10 version 1607 ou ultérieure

Cet article décrit les limites de prise en charge pour le service de temps de Windows (W32Time) dans les environnements nécessitant un temps système très précis et plus stable.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Prise en charge de haute précision pour Windows 8.1 et 2012 R2 (ou précédent)

Les versions antérieures de Windows (avant Windows 10 1607 ou Windows Server 2016 1607) ne peut pas garantir des temps extrêmement précis. Le service de temps de Windows sur ces systèmes :

-   Fourni la précision du temps nécessaire pour satisfaire les exigences d’authentification Kerberos version 5

-   Fourni heure faiblement précise pour les clients Windows et les serveurs joints à une forêt Active Directory courantes

Spécifications de précision plus strictes étaient en dehors de la spécification de conception du Service de temps Windows sur ces systèmes d’exploitation et n’est pas pris en charge.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 et Windows Server 2016

Précision du temps dans Windows 10 et Windows Server 2016 a été considérablement améliorée, tout en conservant une compatibilité NTP avec les versions antérieures de Windows. Sous la droite les conditions d’exploitation, les systèmes exécutant Windows 10 ou Windows Server 2016 et versions ultérieures peuvent fournir 1 seconde, aller-retour de 50 millisecondes (en millisecondes), ou 1 MS précision.

>[!IMPORTANT]
>**Sources de temps très précis**<br>
>La précision de temps résultant dans votre topologie est dépendent largement à l’aide d’une racine précise et stable (niveau 1) source de temps. Il sont Windows en fonction et non Windows en très précis, Windows compatible, temps NTP source matérielle vendus par les fournisseurs de 3 rd-party. Veuillez vérifier avec votre fournisseur de l’exactitude de leurs produits.

>[!IMPORTANT]
>**Précision du temps**<br>
>Précision du temps implique la distribution de bout en bout de temps précis à partir d’une source de temps faisant autorité extrêmement précis sur l’appareil de fin. Tout ce qui présente une asymétrie réseau influenceront négativement précision, périphériques réseau physiques par exemple ou une charge élevée du processeur sur le système cible.

## <a name="high-accuracy-requirements"></a>Exigences de haute précision

Le reste de ce document décrit les conditions d’environnement qui doivent être satisfaites pour prendre en charge les objectifs de haute précision respectifs.

### <a name="target-accuracy-1-second-1s"></a>Précision de la cible : 1 seconde (1)

Pour atteindre 1 s précision pour une cible spécifique d’ordinateur par rapport à une source de temps très précis :

-   Le système cible doit exécuter Windows 10, Windows Server 2016.

-   Le système cible doit synchroniser l’heure à partir d’une hiérarchie NTP de serveurs de temps, sanctionné dans une très précis, Windows compatible source de temps NTP.

-   Tous les systèmes d’exploitation de Windows dans la hiérarchie NTP mentionné ci-dessus doit être configurés comme décrit dans la [configuration des systèmes de haute précision](configuring-systems-for-high-accuracy.md) documentation.

-   La latence réseau unidirectionnel cumulative entre la cible et source ne doit pas dépasser 100 ms. Le délai réseau cumulé est mesuré en ajoutant l’individu unidirectionnel qui sépare les paires de nœuds de client-serveur NTP dans la hiérarchie, en commençant par la cible et en terminant à la source. Pour plus d’informations, consultez le document de synchronisation de temps de haute précision.

### <a name="target-accuracy-50-milliseconds"></a>Précision de la cible : 50 millisecondes

Toutes les conditions requises décrites dans la section **précision de la cible : 1 seconde** s’appliquent, sauf lorsque des contrôles plus stricts sont décrites dans cette section.

Les exigences supplémentaires pour atteindre la précision de 50 ms pour un système cible spécifique sont :

-   L’ordinateur cible doit avoir de meilleures performances que 5 ms de latence du réseau entre sa source de temps.

-   Le système cible doit être pas plus loin que 5 à partir d’une source de temps très précis de niveau

    >[!Note]
    >Exécutez « w32tm /query/Status » à partir de la ligne de commande pour afficher la couche.

-   Le système cible doit se trouver dans 6 moins de tronçons réseau à partir de la source de temps très précis

-   L’une journée utilisation moyenne du processeur sur tous les stratums ne doit pas dépasser 90 %

-   Pour les systèmes virtualisés, l’une journée utilisation moyenne du processeur de l’hôte ne doit pas dépasser 90 %

### <a name="target-accuracy-1-millisecond"></a>Précision de la cible : 1 milliseconde

Toutes les exigences décrites dans les sections **précision de la cible : 1 seconde** et **cible d’analyse de précision : 50 millisecondes** s’appliquent, sauf lorsque des contrôles plus stricts sont décrites dans cette section.

Les exigences supplémentaires pour atteindre 1 ms précision pour un système cible spécifique sont :

-   L’ordinateur cible doit avoir les meilleures performances que 0,1 ms de latence du réseau entre sa source de temps

-   Le système cible doit être pas plus loin que 5 à partir d’une source de temps très précis de niveau

    >[!Note]
    >Exécutez « w32tm /query /status » à partir de la ligne de commande pour afficher la couche

-   Le système cible doit se trouver dans les tronçons réseau 4 inférieure ou égale à partir de la source de temps très précis

-   L’une journée utilisation moyenne du processeur sur chaque couche ne doit pas dépasser 80 %

-   Pour les systèmes virtualisés, l’une journée utilisation moyenne du processeur de l’hôte ne doit pas dépasser 80 %
