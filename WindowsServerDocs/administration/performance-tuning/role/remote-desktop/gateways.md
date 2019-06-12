---
title: Les passerelles de bureau à distance de réglage des performances
description: Recommandations pour les passerelles des services Bureau à distance de réglage des performances
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f3ac020b3137621f6b2535c973ab7759443e1535
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811426"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>Les passerelles de bureau à distance de réglage des performances

> [!NOTE]
> Dans Windows 8 et Windows Server 2012 R2 +, la passerelle des services Bureau à distance (passerelle RD) prend en charge TCP, UDP et les transports RPC hérités. La plupart des données suivantes est ce qui concerne le transport RPC hérité. Si le transport RPC hérité n’est pas utilisé, cette section n’est pas applicable.

Cette rubrique décrit les paramètres liés aux performances d’améliorer les performances d’un déploiement de client et les réglages qui s’appuient sur des modèles d’utilisation réseau du client.

Fondamentalement, passerelle Bureau à distance effectue le paquet de nombreuses opérations entre les instances de la connexion Bureau à distance et les instances de serveur hôte de Session Bureau à distance dans le réseau du client de transfert.

> [!NOTE]
> Les paramètres suivants s’appliquent au transport RPC uniquement.

Internet Information Services (IIS) et passerelle Bureau à distance exportent les paramètres de Registre suivants pour aider à améliorer les performances du système dans la passerelle Bureau à distance.

**Réglages de thread**

-   **MaxIoThreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    Ce pool de threads de spécifiques de l’application spécifie le nombre de threads qui crée de passerelle Bureau à distance pour gérer les demandes entrantes. Si ce paramètre de Registre est présent, il prend effet. Le nombre de threads est égal au nombre de processus logiques. Si le nombre de processeurs logiques est inférieur à 5, la valeur par défaut est 5 threads.

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    Ce paramètre spécifie le nombre de threads de pool IIS à créer par processeur logique. Le pool de threads IIS regardez les demandes réseau et traite toutes les demandes entrantes. Le **MaxPoolThreads** nombre n’inclut pas les threads qui consomme de la passerelle Bureau à distance. La valeur par défaut est 4.

**Réglages d’appel de procédure distante pour la passerelle Bureau à distance**

Les paramètres suivants peuvent vous aider à régler les appels de procédure distante (RPC) qui sont reçus par connexion Bureau à distance et passerelle Bureau à distance des ordinateurs. Modification de la windows permet de limiter la quantité de données est transmis via chaque connexion et peut améliorer les performances pour RPC sur HTTP v2 scenarios.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    La valeur par défaut est 64 Ko. Cette valeur spécifie la fenêtre que le serveur utilise pour les données reçues à partir du proxy RPC. La valeur minimale est définie à 8 Ko, et la valeur maximale est fixée à 1 Go. Si une valeur n’est pas présente, la valeur par défaut est utilisée. Lorsque des modifications sont apportées à cette valeur, IIS doit être redémarré pour que la modification prenne effet.

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    La valeur par défaut est 64 Ko. Cette valeur spécifie la fenêtre que le client utilise pour les données reçues à partir du proxy RPC. La valeur minimale est de 8 Ko, et la valeur maximale est de 1 Go. Si une valeur n’est pas présente, la valeur par défaut est utilisée.

## <a name="monitoring-and-data-collection"></a>Collecte de données et de surveillance

La liste suivante des compteurs de performance est considéré comme un ensemble de compteurs de base lorsque vous surveillez l’utilisation des ressources sur la passerelle Bureau à distance :

-   \\Passerelle de Service Terminal Server\\\*

-   \\Proxy RPC/HTTP\\\*

-   \\Proxy RPC/HTTP par serveur\\\*

-   \\Service Web\\\*

-   \\W3SVC\_W3WP\\\*

-   \\IPv4\\\*

-   \\Mémoire\\\*

-   \\Interface réseau (\*)\\\*

-   \\Processus (\*)\\\*

-   \\Informations de processeur (\*)\\\*

-   \\Synchronization(\*)\\\*

-   \\Système\\\*

-   \\TCPv4\\\*

Les compteurs de performances suivants sont appliquent uniquement aux ancien transport RPC :

-   \\Proxy RPC/HTTP\\ \* RPC

-   \\Proxy RPC/HTTP par serveur\\ \* RPC

-   \\Service Web\\ \* RPC

-   \\W3SVC\_W3WP\\\* RPC

> [!NOTE]
> Le cas échéant, ajoutez le \\IPv6\\ \* et \\TCPv6\\ \* objets. ReplaceThisText

