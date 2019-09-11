---
title: Optimisation des performances Bureau à distance les passerelles
description: Recommandations en matière de réglage des performances pour les passerelles Bureau à distance
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ad314fbf6701da3f96ddc68a598bf3024eaafe16
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866468"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>Optimisation des performances Bureau à distance les passerelles

> [!NOTE]
> Dans Windows 8 et Windows Server 2012 R2 +, la passerelle Bureau à distance (passerelle des services Bureau à distance) prend en charge TCP, UDP et les transports RPC hérités. La plupart des données suivantes concernent le transport RPC hérité. Si le transport RPC hérité n’est pas utilisé, cette section n’est pas applicable.

Cette rubrique décrit les paramètres liés aux performances qui permettent d’améliorer les performances d’un déploiement client et les réglages qui reposent sur les modèles d’utilisation du réseau du client.

À son cœur, la passerelle des services Bureau à distance effectue de nombreuses opérations de transfert de paquets entre Connexion Bureau à distance instances et les instances de serveur hôte de session Bureau à distance au sein du réseau du client.

> [!NOTE]
> Les paramètres suivants s’appliquent uniquement au transport RPC.

Internet Information Services (IIS) et la passerelle des services Bureau à distance exportent les paramètres de registre suivants afin d’améliorer les performances du système dans la passerelle des services Bureau à distance.

**Paramétrages de thread**

-   **MaxIOThreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    Ce pool de threads spécifique à l’application spécifie le nombre de threads que la passerelle des services Bureau à distance crée pour gérer les demandes entrantes. Si ce paramètre de Registre est présent, il prend effet. Le nombre de threads est égal au nombre de processus logiques. Si le nombre de processeurs logiques est inférieur à 5, la valeur par défaut est 5 threads.

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    Ce paramètre spécifie le nombre de threads du pool IIS à créer par processeur logique. Les threads du pool IIS observent le réseau pour les demandes et traitent toutes les demandes entrantes. Le **nombre** de tentatives n’inclut pas les threads consommés par la passerelle des services Bureau à distance. La valeur par défaut est 4.

**Paramétrage des appels de procédure distante pour la passerelle des services Bureau à distance**

Les paramètres suivants permettent de paramétrer les appels de procédure distante (RPC) reçus par les Connexion Bureau à distance et les ordinateurs de passerelle Bureau à distance. La modification de Windows permet de limiter la quantité de données transitant par chaque connexion et peut améliorer les performances pour les scénarios RPC sur HTTP v2.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    La valeur par défaut est 64 Ko. Cette valeur spécifie la fenêtre utilisée par le serveur pour les données reçues du proxy RPC. La valeur minimale est définie à 8 Ko, et la valeur maximale est définie à 1 Go. Si aucune valeur n’est présente, la valeur par défaut est utilisée. Lorsque des modifications sont apportées à cette valeur, IIS doit être redémarré pour que la modification prenne effet.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    La valeur par défaut est 64 Ko. Cette valeur spécifie la fenêtre utilisée par le client pour les données reçues du proxy RPC. La valeur minimale est de 8 Ko, tandis que la valeur maximale est de 1 Go. Si aucune valeur n’est présente, la valeur par défaut est utilisée.

## <a name="monitoring-and-data-collection"></a>Surveillance et collecte des données

La liste suivante de compteurs de performances est considérée comme un ensemble de compteurs de base lorsque vous surveillez l’utilisation des ressources sur la passerelle des services Bureau à distance :

-   \\Passerelle des services Terminal Server\\\*

-   \\Proxy RPC/HTTP\\\*

-   \\Proxy RPC/HTTP par serveur\\\*

-   \\Service Web\\\*

-   \\W3SVC\_W3WP\\\*

-   \\IPv4/IPv6\\\*

-   \\Capacité\\\*

-   \\Interface réseau (\*)\\\*

-   \\Processus (\*)\\\*

-   \\Informations sur le\*processeur ()\\\*

-   \\Synchronisation (\*)\\\*

-   \\Requise\\\*

-   \\TCPv4\\\*

Les compteurs de performances suivants s’appliquent uniquement au transport RPC hérité :

-   \\RPC proxy\\ \* RPC/http

-   \\RPC/proxy http par serveur\\RPC \*

-   \\RPC du\\ service\* Web

-   \\W3SVC\_RPC\\ W3WP\*

> [!NOTE]
> Le cas échéant, ajoutez \\les\\ objets \\IPv6\* et TCPv6\\ \* . ReplaceThisText

