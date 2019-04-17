---
ms.assetid: 1368bc83-9121-477a-af09-4ae73ac16789
title: Choix des disques pour les espaces de stockage direct
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
ms.localizationpriority: medium
ms.openlocfilehash: 227906685d77c31587c66d1c292f20ca94775058
ms.sourcegitcommit: bb626d8626ef47426b781925ea588755dbe2e403
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7871592"
---
# Choix des disques pour les espaces de stockage direct

>S’applique à: WindowsServer2016

Cette rubrique vous apporte des conseils pour le choix de disques adaptés pour les [espaces de stockage direct](storage-spaces-direct-overview.md), répondant par ailleurs à vos exigences en matière de performances et de capacité.

## Types de disques

Les espaces de stockage direct prennent actuellement en charge trois types de disques:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>NVMe</b> (Non-Volatile Memory Express) désigne des disques SSD situés directement sur le busPCIe. Les facteurs de forme courants sont les suivants: U.2 2,5pouces, PCIe Add-In-Card (AIC) etM.2. NVMe offre un plus haut débit d’E/S par seconde et d’E/S ainsi qu’une latence plus faible que tous les autres types de disques actuellement pris en charge.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px" >
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>SSD</b> désigne les disques SSD connectés via un SATA ou un SAS classique.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            <b>HDD</b> désigne les disques durs de rotation magnétiques qui offrent une grande capacité de stockage.
        </td>
    </tr>
</table>

## Cache intégré

Les espaces de stockage direct font usage d'un cache intégré côté serveur. Il s'agit d'un cache volumineux, persistant et en temps réel pour la lecture et l'écriture. Dans les déploiements impliquant plusieurs types de disques, il est configuré automatiquement pour utiliser tous les disques du type «le plus rapide». Les disques restants sont utilisés pour la capacité.

Pour plus d’informations, voir [Fonctionnement du cache dans les espaces de stockage direct](understand-the-cache.md).

## Option1: Optimisation des performances

Vous devez opter pour une solution «100% flash» si vous souhaitez garantir une latence homogène et prévisible inférieure à une milliseconde pour toutes les opérations aléatoires de lecture et d’écriture de données, ou si vous voulez permettre un très grand nombre d’E/S (nous en avons réalisé [plus de six millions](https://www.youtube.com/watch?v=0LviCzsudGY&t=28m)!) ou d’opérations d’E/S par seconde (nous en avons réalisé [plus d’1To/s](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=16m50s)!).

Vous disposez actuellement de troismoyens pour y parvenir:

![All-Flash-Deployment-Possibilities](media/choosing-drives-and-resiliency-types/All-Flash-Deployment-Possibilities.png)

1. **Uniquement des disques NVMe.** L’utilisation de disques NVMe uniquement offre des performances sans égal, en plus d’une faible latence hautement prévisible. Si tous vos disques sont du même modèle, il n’y a pas de cache. Vous pouvez également combiner des modèles de disques NVMe plus ou moins endurants, en configurant les modèles à haute endurance pour mettre en cache les écritures sur les modèles à plus faible endurance ([nécessite une configuration](understand-the-cache.md#manual)).

2. **Disques NVMe+ SSD.** Si vous utilisez un disque NVMe avec des disques SSD, le NVMe met automatiquement en cache les écritures sur les SSD. De cette manière, les écritures sont regroupées dans le cache et déstockées uniquement si nécessaire, afin de réduire la charge sur les SSD. Les caractéristiques d’écriture sont similaires au NVMe, mais les lectures sont effectuées directement à partir des SSD rapides.

3. **Disques SSD uniquement.** Comme pour l’option avec uniquement des NVMe, il n’y a pas de cache si tous vos disques sont du même modèle. Si vous combinez des modèles de disques plus ou moins endurants, vous pouvez configurer les modèles à haute endurance pour mettre en cache les écritures sur les modèles à plus faible endurance ([nécessite une configuration](understand-the-cache.md#manual)).

   >[!NOTE]
   > L’un des avantages d’utiliser uniquement des disques NVMe ou SSD sans cache est que vous exploitez toute la capacité de stockage de chaque disque. Aucune partie de la capacité n’est utilisée pour la mise en cache, ce qui peut être intéressant pour les déploiements à petite échelle.

## Option2: Équilibre entre performances et capacité

Pour les environnements présentant une diversité d’applications et de charges de travail, dont certaines sont assorties d'exigences strictes en termes de performances tandis que d’autres nécessitent une d'importantes capacités de stockage, vous devez opter pour une solution «hybride» avec des NVMe ou des SSD assurant la mise en cache pour les disques HDD plus gros.

![Possibilités de déploiement hybride](media/choosing-drives-and-resiliency-types/Hybrid-Deployment-Possibilities.png)

1. **NVMe + HDD**. Les disques NVMe accélèrent les opérations de lecture et d’écriture en les mettant toutes en cache. La mise en cache des lectures permet aux disques HDD de traiter en priorité les opérations d’écriture. La mise en cache des écritures absorbe les pics d’opérations et permet de regrouper les écritures et de les déstocker uniquement si nécessaire, d’une manière sérialisée artificielle qui optimise le débit d’E/S par seconde et d’E/S des disquesHDD. Les caractéristiques d’écriture sont similaires au NVMe. Pour les données lues récemment ou fréquemment, les caractéristiques de lecture sont également similaires au NVMe.

2. **SSD+ HDD**. Comme pour les disques précédents, les disques SSD accélèrent les opérations de lecture et d’écriture en les mettant toutes en cache. Les caractéristiques d’écriture sont similaires au disque SSD. Pour les données lues récemment ou fréquemment, les caractéristiques de lecture sont également similaires au disque SSD.

    Il y a une autre option, plus marginale, qui consiste à utiliser des disques des *trois* types.

3. **Disques NVMe +SSD +HDD.** Si vous utilisez des disques des trois types, les disques NVMe assurent la mise en cache pour les disques SSD et HDD. L’avantage est que cela vous permet de créer des volumes sur les disques SSD et sur les disques HDD, côte-à-côte dans le même cluster et optimisés par le disque NVMe. Les premiers fonctionnent exactement comme dans un déploiement «100% flash» et les seconds fonctionnent exactement comme dans un déploiement «hybride», comme décrit plus haut. D’un point de vue conceptuel, cela revient à avoir deux pools, avec une gestion indépendante de la capacité, des cycles d’échec et de réparation, etc.

   >[!IMPORTANT]
   > Nous vous recommandons d’utiliser le niveau SSD pour placer vos charges de travail les plus sensibles aux performances sur du 100% Flash.

## Option3: Optimisation de la capacité

Pour les charges de travail qui requièrent une grande capacité de stockage et qui réalisent peu d'écritures (archives, cibles de sauvegarde, entrepôts de données ou stockage "froid", par exemple), il convient combiner quelques disques SSD pour la mise en cache avec un grand nombre de disques HDD de grande capacité.

![Options de déploiement pour optimiser la capacité](media/choosing-drives-and-resiliency-types/maximizing-capacity.png)

1. **SSD+ HDD**. Les disques SSD assurent la mise en cache des lectures et des écritures, pour absorber les pics d’opérations et offrir les performances d’écriture des SSD, et les déstockent seulement quand nécessaire sur les disques HDD pour optimiser la capacité.

>[!IMPORTANT]
>Configuration avec des disques durs uniquement n'est pas pris en charge. Disques SSD une endurance élevée à faible endurance disques SSD de mise en cache n’est pas recommandé.

## Considérations relatives à la taille

### Cache

Chaque serveur doit disposer au minimum de deux disques de cache pour assurer la redondance. Nous vous recommandons de choisir un nombre de disques de capacité qui soit un multiple du nombre de disques de cache. Par exemple, si vous avez quatre disques de cache, vous obtiendrez des performances plus constantes avec huit disques de capacité plutôt qu'avec sept ou neuf.

La taille du cache doit être pour prendre en charge la plage de travail de vos applications et charges de travail, autrement dit, toutes les données qu’ils sont lisent et écrivent à un moment donné. Il n’y a pas d’autre exigence pour la taille du cache. Pour les déploiements avec des disques durs, de prévoir au départ est 10 % de la capacité: par exemple, si chaque serveur a 4 x 4 To HDD = 16 To de capacité, puis 2 x SSD de 800 Go = 1,6 To de cache par serveur. Pour les déploiements de tout-flash, en particulier avec une très [grande endurance](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/) disques SSD, elle peut être juste pour démarrer le plus proche de 5 % de la capacité: par exemple, si chaque serveur possède 24 x 1.2 SSD to = 28,8 To de capacité, puis 2 x 750 Go NVMe = 1,5 To de cache par serveur. Vous pouvez ajuster la taille à tout moment en ajoutant ou en supprimant des disques de cache.

### Général

Nous recommandons de limiter la capacité de stockage totale par serveur à environ 100téraoctets (To). Plus la capacité de stockage par serveur est élevée, plus il faut de temps pour resynchroniser les données après un arrêt ou un redémarrage (par exemple, pour appliquer des mises à jour logicielles).

Actuellement, la taille maximale par pool de stockage est de 1pétaoctet (Po), soit 1000téraoctets (To).

## Voir également

- [Présentation des espaces de stockage direct](storage-spaces-direct-overview.md)
- [Fonctionnement du cache dans les espaces de stockage direct](understand-the-cache.md)
- [Configuration matérielle requise pour les espaces de stockage direct](storage-spaces-direct-hardware-requirements.md)
- [Planification des volumes dans les espaces de stockage direct](plan-volumes.md)
- [Tolérance de panne et efficacité du stockage](storage-spaces-fault-tolerance.md)
