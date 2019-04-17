---
title: Équilibrage de la charge du serveur NPS Proxy
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les fonctionnalités de Windows Server2016 et VPN Windows10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f00eb6575284a69358c58b36d08672cdd69dc18
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="nps-proxy-server-load-balancing"></a>Équilibrage de la charge du serveur NPS Proxy

S’applique à: Windows Server2016

Clients distants Authentication Dial-In RADIUS (User Service), qui sont des serveurs d’accès réseau tels que les serveurs de réseau privé virtuel (VPN) et les points d’accès sans fil, créent des demandes de connexion et les envoient à des serveurs RADIUS tels que NPS. Dans certains cas, un serveur NPS peut recevoir un trop grand nombre de demandes de connexion à la fois, ce qui entraîne une dégradation des performances ou une surcharge. Lorsqu’un serveur NPS est surchargé, il est judicieux d’ajouter des serveurs NPS pour votre réseau et pour configurer l’équilibrage de charge. Lorsque vous distribuez uniformément des demandes de connexion entrantes entre plusieurs serveurs NPS afin d’éviter la surcharge d’un ou plusieurs serveurs NPS, elle est appelée à l’équilibrage de charge.

L’équilibrage de charge est particulièrement utile pour:

- Les organisations qui utilisent \(EAP-TLS\) Extensible Authentication Protocol-Transport Layer Security ou Protected Extensible Authentication Protocol \ (PEAP\)-TLS pour l’authentification. Étant donné que ces méthodes d’authentification utilisent des certificats pour l’authentification du serveur et pour l’utilisateur ou l’authentification de l’ordinateur client, la charge sur les serveurs et des proxys RADIUS est plus importante que lorsque les méthodes d’authentification par mot de passe sont utilisés.
- Organisations qui doivent maintenir la disponibilité continue des services.
- Service Internet \(ISPs\) fournisseurs externes de l’accès VPN pour d’autres organisations. Les services VPN externalisés peuvent générer un important volume de trafic d’authentification.

Il existe deux méthodes que vous pouvez utiliser pour équilibrer la charge des demandes de connexion envoyées à vos serveurs NPS:

- Configurer vos serveurs d’accès réseau pour envoyer des demandes de connexion à plusieurs serveurs RADIUS. Par exemple, si vous disposez de 20points d’accès sans fil et deux serveurs RADIUS, configurez chaque point d’accès pour envoyer des demandes de connexion pour les deux serveurs RADIUS. Vous pouvez équilibrer la charge et fournir un basculement sur chaque serveur d’accès réseau en configurant le serveur d’accès pour envoyer des demandes de connexion à plusieurs serveurs RADIUS dans un ordre de priorité spécifié. Cette méthode d’équilibrage de charge est généralement préférable pour les petites entreprises qui ne déploient pas un grand nombre de clients RADIUS.
- Utilisez NPS configuré en tant que proxy RADIUS pour charger équilibrer les demandes de connexion entre plusieurs serveurs NPS ou d’autres serveurs RADIUS. Par exemple, si vous disposez de trois serveurs RADIUS, un proxy NPS et 100points d’accès sans fil, vous pouvez configurer les points d’accès pour envoyer tout le trafic au proxy NPS. Sur le proxy NPS, configurer l’équilibrage de charge afin que le serveur proxy distribue uniformément les demandes de connexion entre les trois serveurs RADIUS. Cette méthode d’équilibrage de charge est recommandée pour les moyennes ou grandes organisations qui ont de nombreux clients et serveurs RADIUS.

Dans de nombreux cas, la meilleure approche pour l’équilibrage de charge consiste à configurer des clients RADIUS pour envoyer des demandes de connexion à deux serveurs proxy de serveur NPS, puis configurez les serveurs proxy NPS pour charger le solde entre les serveurs RADIUS. Cette approche offre de basculement et équilibrage de charge pour les serveurs proxy NPS et les serveurs RADIUS.

## <a name="radius-server-priority-and-weight"></a>Poids et la priorité du serveur RADIUS

Pendant le processus de configuration du proxy NPS, vous pouvez créer des groupes de serveurs RADIUS distants et puis ajoutez les serveurs RADIUS à chaque groupe. Pour configurer l’équilibrage de charge, vous devez disposer de plusieurs serveurs RADIUS par groupe de serveurs RADIUS distants. Lors de l’ajout de membres du groupe, ou après la création d’un serveur RADIUS en tant que membre du groupe, vous pouvez accéder la boîte de dialogue du serveur RADIUS Ajouter pour configurer les éléments suivants sous l’onglet équilibrage de charge:

- **Priorité**. Priorité spécifie l’ordre d’importance du serveur RADIUS pour le serveur proxy NPS. Niveau de priorité doit être affecté à une valeur qui est un entier, par exemple, 1, 2 ou 3. Plus le nombre, la priorité de proxy NPS donne sur le serveur RADIUS. Par exemple, si le serveur RADIUS est attribué à la priorité la plus élevée de 1, le proxy NPS envoie les demandes de connexion sur le serveur RADIUS Si les serveurs avec une priorité 1 ne sont pas disponibles, NPS envoie alors les demandes de connexion pour les serveurs RADIUS avec une priorité 2 et ainsi de suite. Vous pouvez attribuer la même priorité à plusieurs serveurs RADIUS et ensuite utiliser le paramètre de poids pour charger équilibre entre eux.

- **Poids**. Utilise NPS ce paramètre de poids pour déterminer le nombre de connexion demande à envoyer à chaque membre de groupe lorsque les membres du groupe ont le même niveau de priorité. Paramètre de poids doit être affecté à une valeur comprise entre 1 et 100, et la valeur représente un pourcentage de 100%. Par exemple, si le groupe de serveurs RADIUS distants contient deux membres ont un niveau de priorité de 1 et une évaluation des poids de 50, le proxy de serveur NPS transfère 50 pour cent de demandes de connexion à chaque serveur RADIUS.

- **Paramètres avancés**. Ces paramètres de basculement offrent un moyen d’un serveur NPS déterminer si le serveur RADIUS à distance n’est pas disponible. Si NPS détermine qu’un serveur RADIUS n’est pas disponible, il peut commencer à envoyer des demandes de connexion à d’autres membres du groupe. Avec ces paramètres, vous pouvez configurer le nombre de secondes pendant lesquelles le proxy NPS attend une réponse à partir du serveur RADIUS avant qu’il considère que la demande est supprimée; le nombre maximal de demandes ignorées avant que le proxy NPS identifie le serveur RADIUS comme non disponibles; et le nombre de secondes qui peuvent s’écouler entre les demandes avant le proxy NPS identifie le serveur RADIUS comme non disponible.

## <a name="configure-nps-proxy-load-balancing"></a>Configurer l’équilibrage de charge proxy NPS

Avant de configurer l’équilibrage de charge, créez un plan de déploiement qui inclut le nombre à distance groupes de serveurs RADIUS vous avez besoin, les serveurs qui sont membres de chaque groupe particulier et le paramètre de priorité et de pondération pour chaque serveur.

>[!NOTE]
>Les étapes qui suivent supposent que vous avez déjà déployé et configuré des serveurs RADIUS.

Pour configurer NPS à agir comme un serveur proxy et les demandes de connexion directe à partir de clients RADIUS à des serveurs RADIUS distants, vous devez effectuer les actions suivantes:

1. Déploiement de vos clients RADIUS \ (serveurs VPN, serveurs d’accès à distance, les serveurs de passerelle des Services Terminal Server, commutateurs d’authentification 802. 1 X et 802. 1 X sans fil points\ accès) et les configurer pour envoyer des demandes de connexion à vos serveurs proxy de serveur NPS.

2. Sur le proxy NPS, configurez les serveurs d’accès réseau en tant que clients RADIUS. Pour plus d’informations, voir [configurer des Clients RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. Sur le serveur proxy NPS, créez un ou plusieurs groupes de serveurs distants. Pendant ce processus, ajouter des serveurs RADIUS pour les groupes de serveurs RADIUS distants. Pour plus d’informations, voir [configurer de groupes de serveurs RADIUS distants](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. Sur le proxy NPS, pour chaque serveur RADIUS que vous ajoutez à un groupe de serveurs RADIUS distants, cliquez sur le serveur RADIUS **l’équilibrage de charge** onglet, puis configurez **priorité**, **poids**, et **paramètres avancés**.

5. Sur le proxy NPS, configurer des stratégies de demande de connexion pour transférer les demandes d’authentification et gestion des comptes à des groupes de serveurs RADIUS distants. Vous devez créer une stratégie de demande de connexion par groupe de serveurs RADIUS distants. Pour plus d’informations, voir [configurer les stratégies de demande de connexion](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


