---
title: Équilibrage de charge de serveur de Proxy de serveur NPS
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les fonctionnalités de Windows Server 2016 et VPN Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 04f466f7646fc5109af7379cab1240d8cd6829ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874400"
---
# <a name="nps-proxy-server-load-balancing"></a>Équilibrage de charge de serveur de Proxy de serveur NPS

S'applique à : Windows Server 2016

Clients distants d’authentification Dial-In Service RADIUS (User), qui sont des serveurs d’accès réseau comme les serveurs de réseau privé virtuel (VPN) et des points d’accès sans fil, créer des demandes de connexion et les envoient aux serveurs RADIUS tels que NPS. Dans certains cas, un serveur NPS peut recevoir trop de demandes de connexion en même temps, ce qui entraîne une dégradation des performances ou une surcharge. Lorsqu’un serveur NPS est surchargé, il est conseillé d’ajouter plus NPSs à votre réseau et de configurer l’équilibrage de charge. Lorsque vous distribuez uniformément les demandes de connexion entrantes parmi plusieurs NPSs afin d’éviter la surcharge d’un ou plusieurs NPSs, elle est appelée à l’équilibrage de charge.

L’équilibrage de charge est particulièrement utile pour :

- Les organisations qui utilisent Extensible Authentication Protocol-Transport Layer Security \(EAP-TLS\) ou Protected Extensible Authentication Protocol \(PEAP\)- TLS pour l’authentification. Étant donné que ces méthodes d’authentification utilisent des certificats pour l’authentification serveur et pour l’utilisateur ou l’authentification d’ordinateur client, la charge sur les serveurs et les proxys RADIUS est plus importante que lorsque les méthodes d’authentification par mot de passe sont utilisés.
- Organisations qui doivent faire face aux disponibilité continue des services.
- Fournisseurs de services Internet \(ISP\) qui sous-traiter l’accès VPN à d’autres organisations. Les services VPN externalisés peuvent générer un gros volume de trafic d’authentification.

Il existe deux méthodes que vous pouvez utiliser pour équilibrer la charge des demandes de connexion envoyées à votre NPSs :

- Configurer vos serveurs d’accès réseau pour envoyer des demandes de connexion à plusieurs serveurs RADIUS. Par exemple, si vous avez 20 points d’accès sans fil et deux serveurs RADIUS, configurez chaque point d’accès pour envoyer des demandes de connexion pour les deux serveurs RADIUS. Vous pouvez équilibrer la charge et fournir un basculement à chaque serveur d’accès réseau en configurant le serveur d’accès pour envoyer des demandes de connexion à plusieurs serveurs RADIUS dans un ordre de priorité spécifié. Cette méthode d’équilibrage de charge est généralement préférable pour les petites organisations qui ne déploient pas un grand nombre de clients RADIUS.
- Utilisez NPS configuré en tant que proxy RADIUS pour équilibrer les demandes de connexion entre plusieurs NPSs ou d’autres serveurs RADIUS. Par exemple, si vous avez 100 points d’accès sans fil, un proxy de serveur NPS et trois serveurs RADIUS, vous pouvez configurer les points d’accès pour envoyer tout le trafic vers le proxy de serveur NPS. Sur le proxy NPS, configurez l’équilibrage de charge afin que le proxy distribue uniformément les demandes de connexion entre les trois serveurs RADIUS. Cette méthode d’équilibrage de charge est idéal pour les moyennes et grandes entreprises qui ont de nombreux clients et serveurs RADIUS.

Dans de nombreux cas, la meilleure approche pour l’équilibrage de charge consiste à configurer des clients RADIUS pour envoyer des demandes de connexion à deux serveurs proxy de serveur NPS, puis configurez les proxys de serveur NPS afin d’équilibrer entre les serveurs RADIUS. Cette approche fournit le basculement et équilibrage de charge pour les proxys de serveur NPS et les serveurs RADIUS.

## <a name="radius-server-priority-and-weight"></a>Priorité du serveur RADIUS et la pondération

Pendant le processus de configuration de proxy de serveur NPS, vous pouvez créer des groupes de serveurs RADIUS distants et puis ajoutez les serveurs RADIUS à chaque groupe. Pour configurer l’équilibrage de charge, vous devez disposer de plusieurs serveurs RADIUS par groupe de serveurs RADIUS distants. Lors de l’ajout de membres du groupe, ou après la création d’un serveur RADIUS en tant qu’un membre du groupe, vous pouvez accéder de la boîte de dialogue Ajouter un RADIUS server pour configurer les éléments suivants sur l’onglet de l’équilibrage de charge :

- **priorité**. Priorité spécifie l’ordre d’importance du serveur RADIUS sur le serveur de proxy NPS. Niveau de priorité doit être attribué à une valeur qui est un entier, comme 1, 2 ou 3. Faible le nombre, la priorité plus élevée proxy NPS donne au serveur RADIUS. Par exemple, si le serveur RADIUS est affecté à la priorité la plus élevée de 1, le proxy de serveur NPS envoie les demandes de connexion au serveur RADIUS tout d’abord ; Si les serveurs de priorité 1 ne sont pas disponibles, NPS envoie ensuite des demandes de connexion pour les serveurs RADIUS de priorité 2 et ainsi de suite. Vous pouvez affecter la même priorité à plusieurs serveurs RADIUS et puis utiliser le paramètre de poids pour équilibrer la charge entre eux.

- **Poids**. Utilise NPS ce paramètre de poids pour déterminer combien de connexion demande à envoyer à chaque membre de groupe lorsque les membres du groupe ont le même niveau de priorité. Paramètre de poids doit être attribué à une valeur comprise entre 1 et 100, et la valeur représente un pourcentage de 100 pour cent. Par exemple, si le groupe de serveurs RADIUS distants contient deux membres ont un niveau de priorité 1 et une évaluation des poids de 50, le proxy NPS transfère 50 pour cent des demandes de connexion à chaque serveur RADIUS.

- **Paramètres avancés**. Ces paramètres de basculement fournissent un moyen d’un serveur NPS déterminer si le serveur RADIUS à distance n’est pas disponible. Si NPS détermine qu’un serveur RADIUS n’est pas disponible, il peut commencer à envoyer des demandes de connexion aux autres membres du groupe. Avec ces paramètres, vous pouvez configurer le nombre de secondes pendant lesquelles le proxy NPS attend une réponse à partir du serveur RADIUS avant qu’elle considère la demande est supprimée ; le nombre maximal de demandes ignorées avant que le proxy NPS identifie le serveur RADIUS comme non disponibles ; et le nombre de secondes pouvant s’écouler entre les demandes avant le proxy NPS identifie le serveur RADIUS comme non disponibles.

## <a name="configure-nps-proxy-load-balancing"></a>Configurer l’équilibrage de charge proxy NPS

Avant de configurer l’équilibrage de charge, créez un plan de déploiement qui inclut le comment de nombreux groupes de serveurs distants vous avez besoin, quels serveurs sont membres de chaque groupe et le paramètre de priorité et le poids pour chaque serveur.

>[!NOTE]
>Les étapes qui suivent supposent que vous avez déjà déployé et configuré les serveurs RADIUS.

Pour configurer NPS pour agir comme un serveur proxy et les demandes de connexion directe à partir de clients RADIUS pour les serveurs RADIUS distants, vous devez effectuer les actions suivantes :

1. Déploiement de vos clients RADIUS \(serveurs VPN, les serveurs d’accès à distance, les serveurs de passerelle des Services Terminal Server, commutateurs de l’authentification 802. 1 X et 802. 1 X de points d’accès sans fil\) et les configurer pour envoyer des demandes de connexion à votre serveur proxy de serveur NPS serveurs.

2. Sur le proxy NPS, configurez les serveurs d’accès réseau en tant que clients RADIUS. Pour plus d’informations, consultez [configurer des Clients RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. Sur le proxy NPS, créez un ou plusieurs groupes de serveurs distants. Pendant ce processus, ajouter des serveurs RADIUS pour les groupes de serveurs RADIUS distants. Pour plus d’informations, consultez [configurer de groupes de serveurs RADIUS distants](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. Sur le proxy de serveur NPS, pour chaque serveur RADIUS que vous ajoutez à un groupe de serveurs RADIUS distants, cliquez sur le serveur RADIUS **l’équilibrage de charge** onglet, puis configurez **priorité**, **poids** , et **paramètres avancés**.

5. Sur le proxy NPS, configurer des stratégies de demande de connexion pour transférer les demandes d’authentification et gestion des comptes à des groupes de serveurs RADIUS distants. Vous devez créer une stratégie de demande de connexion par groupe de serveurs RADIUS distants. Pour plus d’informations, consultez [configurer les stratégies de demande de connexion](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


